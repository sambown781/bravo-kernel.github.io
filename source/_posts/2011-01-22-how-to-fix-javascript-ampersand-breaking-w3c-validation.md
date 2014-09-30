title: How to fix Javascript ampersand breaking W3C validation
alias: /blog/2011/01/how-to-fix-javascript-ampersand-breaking-w3c-validation/index.html
date: 2011-01-22 18:05:13 +0200
categories:
- Web
tags:
- javascript
- validator
---

If your page doesn't pass [W3C Validation](http://validator.w3.org/ "Markup Validation Service") because of Javascript containing:

- ampersands (&)
- greater/lesser than (&lt; &gt;)

Encapsulate your script inside a CDATA tag and the W3C parser will ignore it.

	<script type="text/javascript">
		//<![CDATA[
			script here
		//]]>
	</script>
