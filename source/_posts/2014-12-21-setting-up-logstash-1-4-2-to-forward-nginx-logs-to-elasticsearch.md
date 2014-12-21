title: Setting up Logstash 1.4.2 to forward Nginx logs to Elasticsearch
date: 2014-12-21 14:13:25
categories:
- Sysop
tags:
- logstash
- elasticsearch
- nginx
---
Step-by-step instructions on how to configure Logstash v1.4.2 to forward your Nginx access logs to Elasticsearch v1.4.2.

## Create a custom Nginx log format

Add the following custom log_format to the <em>/etc/nginx/conf.d</em> file so it becomes available to all (virtual) Nginx hosts: 

    # Custom logstash format
    log_format logstash '$http_host '
                        '$remote_addr [$time_local] '
                        '"$request" $status $body_bytes_sent '
                        '"$http_referer" "$http_user_agent" '
                        '$request_time '
                        '$upstream_response_time';

> Make sure to add this **above** the "# virtual hosting" directive to prevent nginx failing with "unkown log format" errors.

## Enable the custom Nginx log format

Update your nginx site configuration file(s) to start logging with the new custom log_format:

    #access_log /var/log/nginx/access.log;
    access_log /var/log/nginx/access.log logstash;

Restart nginx. 

Check your access log(s) and confirm that entries no longer look like the first line below (default nginx format) but look similar to second one (using our custom logstash format):

	10.33.10.1 - - [21/Dec/2014:14:36:25 +0000] "GET /test_126 HTTP/1.1" 404 6642 "-" "Mozilla/5.0 (Windows NT 6.3; WOW64; rv:34.0) Gecko/20100101 Firefox/34.0"
	my.app 10.33.10.1 [21/Dec/2014:13:58:48 +0000] "GET /test_126 HTTP/1.1" 404 6641 "-" "Mozilla/5.0 (Windows NT 6.3; WOW64; rv:34.0) Gecko/20100101 Firefox/34.0" 2.465 2.465

## Create a Logstash nginx pattern

Create new file <em>/opt/logstash/server/patterns/nginx</em> with:
	
    NGUSERNAME [a-zA-Z\.\@\-\+_%]+
    NGUSER %{NGUSERNAME}
    NGINXACCESS %{IPORHOST:http_host} %{IPORHOST:clientip} \[%{HTTPDATE:timestamp}\] \"(?:%{WORD:verb} %{NOTSPACE:request}(?: HTTP/%{NUMBER:httpversion})?|%{DATA:rawrequest})\" %{NUMBER:response} (?:%{NUMBER:bytes}|-) %{QS:referrer} %{QS:agent} %{NUMBER:request_time:float} %{NUMBER:upstream_time:float}
    NGINXACCES %{IPORHOST:http_host} %{IPORHOST:clientip} \[%{HTTPDATE:timestamp}\] \"(?:%{WORD:verb} %{NOTSPACE:request}(?: HTTP/%{NUMBER:httpversion})?|%{DATA:rawrequest})\" %{NUMBER:response} (?:%{NUMBER:bytes}|-) %{QS:referrer} %{QS:agent} %{NUMBER:request_time:float}    

> We create a pattern so we can keep our our logstash filters clean (simply referencing the pattern).

## Create a Logstash configuration file

Create new file <em>/opt/logstash/server/etc/conf.d/nginx.conf</em> with:

	input {
	  file {
		path => "/var/log/nginx/*access*"
	  }
	}

	filter {
	  mutate { replace => { "type" => "nginx_access" } }
	  grok {
		match => { "message" => "%{NGINXACCESS}" }
	  }
	  date {
		match => [ "timestamp" , "dd/MMM/YYYY:HH:mm:ss Z" ]
	  }
	  geoip {
		source => "clientip"
	  }
	}

	output {
	  elasticsearch {
		host => "localhost"
		protocol => "http"
	  }
	  stdout { codec => rubydebug }
	}

**Notes:**

- we change the timestamp to use the time the request arrived at the nginx server (instead of using the default which is the time the event was written to log, which is after the request has been processed)
- all logentries will be indexed using Elasticsearch type "nginx_access"
- we do NOT use the "index" directive in our output, all logentries will use Elasticsearch index "logstash-%{+YYYY.MM.dd}"

> We MUST use the http protocol. By default Logstash will use the "node" protocol which joins Logstash as a participating node to the Elasticsearch cluster using it's embedded ES library. Since that embedded version (1.1.1) is currently incompatible with version 1.4.2 it will cause issues all over the place like breaking Kibana due to incompatible nodes etc.


## Start logging to Elasticsearch

Manually test your new Logstash configuration by running:

    /opt/logstash/server/bin/logstash -f /opt/logstash/server/etc/conf.d/nginx.conf

If your logs are appearing in Elasticsearch restart your logstash service. 
	
## Credits

Hat-tip to [FTBpro developers](http://tech.ftbpro.com/post/92330141601/getting-the-best-out-of-logstash-for-nginx) for the inspiration and the Logstash pattern
