title: How to set up a multi-language FIM 2010 R2 Self Service Password Reset portal
alias: /blog/2013/04/how-to-set-up-a-multi-language-fim-2010-r2-self-service-password-reset-portal/index.html
date: 2013-04-25 15:30:34 +0200
categories:
- Microsoft
tags:
- microsoft
- fim
---

The 2010 R2 release for Microsoft Forefront Identity Manager (FIM) contains a huge overhaul of the password-registration and password-reset web (SSPR) portals.

Besides the fact that they now function properly (duh!) much attention has been put into adding multi-lingual support which is great if you want to display different customized languages  based on your clients browser preferences.

Make sure to read this (almost) perfect [Technet Article](http://technet.microsoft.com/en-us/library/jj134312%28v=ws.10%29.aspx "FIM 2010 R2 Portal Customization") that describes most customizations in details.

**Important:**

- Do not use *Strings.language-culture.resources* when naming your language file (e.g. Strings.nl-nl.resources)
- Use *Strings.culture.resources* instead (e.g. Strings.nl.resources)

##An example multi-language setup:

Without going into all of the technical details of the file-contents (read the Technet article for that) create the following directory structure for a fully customized and multi-lingual portal for both Dutch and English language. Remember that you must have the language pack for your desired language installed or all will fail miserably!

{% asset_img fim-2010-r2-portal-customizations.png 'FIM 2010 R2 Portal Customizations - Directory Structure' %}

###Images:

Create 3 banners using your (company) logo

- my-fimlogo.png: horizontal image used for normal portal access with transparent background
- my-fimlogo-ie6.png: horizontal image used for IE6 portal access with non-transparent background
- my-fimlogo-portrait.png: vertical image used for mobile portal access

###Custom stylesheet:

FIM always loads it's own core.css stylesheet but will overload them with values found in Styles.css. This is ofcourse very useful because we can keep your customizations stylesheet very small. The following example leaves all markup found in core.css intact except for replacing the banner images.

	.title-block {
	  background: url("../Customizations/my-fimlogo.png") no-repeat scroll 0 0 transparent;
	}

	.ie6 .title-block {
	  background-image: url(../Customizations/my-fimlogo-ie6.png);
	}
	@media only screen and (max-width: 480px) {
	  .title-block {
	    background: url("../Customizations/my-fimlogo-portrait.png") no-repeat scroll 0 0 transparent;
	  }
	}

###Language files

As with the stylesheet FIM always loads all translations found in the language pack first before overloading them with any custom translation found in your resources file. Good news again because we can keep our translation files very small as well.

- **Strings.resources:** custom strings placed in this file will always be used regardless of the browser language-setting. Good use for this file would be the "ExampleReset1" and "ExampleReset2" keys which represent the language-agnostic values "contoso\mmeyers" and "mmeyers@contoso.com"
- **Strings.nl.resources:** specify key/value pairs you want to show to users with browser language preference set to "nl-nl"
- **Strings.en.resources:** specify key/value pairs you want to show to users with browser language preference set to "en-us" or "en-uk"

All in all a pretty good job by Microsoft with some good though put into the customization scheme.
