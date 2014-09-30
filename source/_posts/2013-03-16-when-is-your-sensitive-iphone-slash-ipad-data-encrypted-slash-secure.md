title: When is your sensitive iPhone/iPad data encrypted/secure?
alias: /blog/2013/03/when-is-your-sensitive-iphone-slash-ipad-data-encrypted-slash-secure/index.html
date: 2013-03-16 15:10:12 +0200
categories:
- Mobile
tags:
- mobile
- apple
- security
- iphone
- ipad
---

I have rolled out many enterprise BYOD scenario's for a wide variety of mobile devices and the one question I always get asked  is **"will our sensitive data on the iPhones and iPads be encrypted?"**. The answer, unfortunately, is not black and white.

Unlike other manufacturers like Blackberry (who encrypt the whole disk) Apple has chosen a different approach and uses 2 different methods to provide encryption:

1. **Basic encryption** using the pincode (which is totally circumvented and therefore useless after jailbreaking a device)
2. **Data Protection** which is an extra layer that app-builders can use to make sure their files are encrypted on a per-file basis (and unreadable even after jailbreaking)

Good news one might think but:

- unfortunately not all apps make use of Data Protection
- because Apple stores files "inside" the app this makes the app the weakest link in the chain responsible for securing your sensitive enterprise data.

The flow below should make things very clear and might be a good starting point to convince your management to invest in a decent Mobile Device Management (MDM) solution with much needed enterprise-features like application blacklisting/whitelisting etc.

{% img /images/apple/apple-file-encryption-determination-flow.png 'Apple file encryption determination flow' 'Image: Apple file encryption determination flow' %}

One more thing: Apple's email and email-attachments use Data Protection by default and nowadays more and more popular apps (like iAnnotate) have implemented it as well. But however comforting this might sound one should not be tricked; a badly-written 3rd party mail/pdf reader downloaded by one of your users might not support Data Protection and in that case your sensitive data can easily end up on the streets (in the worst case along with your company's name in next morning's papers) ;)
