---
title: integrate-cdn-resource-with-vbulletin
displayName: vBulletin
published: true
order: 200
toc:
---
Before you take any steps please back up your files and database. The plugin works only with the default CMS pattern. If you manually changed CMS patterns, the plugin might not help you.

1\. Login to your vBulletin control panel.

2\. Go to Styles, then to Replacement Variable Manager.

3\. Choose Add New Replacement Variable.

<img src="https://support.gcore.com/hc/article_attachments/360010892858/vbulletin.png" alt="vbulletin.png">

Using your CNAME from the [control panel](https://control.gcdn.co/) (cdn.site.com) create a new replacement variable for each item in the following table. Ensure that your CNAME record has been [configured](https://support.gcore.com/hc/en-us/articles/213969769-%D0%A1NAME) in a proper way before using it for integration.<img src="https://support.gcore.com/hc/article_attachments/360010892898/bulletin_______.png" alt="bulletin_______.png">



| Find in the text                                                       | Replace with                                                                                                   |
|----------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------|
|   src=”customavatars/                                                  | src=”http://cdn.site.com/customavatars/                                                                        |
|   src=”customprofilepics/                                              |   src=”http://cdn.site.com/customprofilepics/                                                                  |
|   src=”images/                                                         |   src=”http://cdn.site.com/images/                                                                             |
|   url(“clientscript                                                    |   url(“cdn.site.com                                                                                            |
|   src=”clientscript/                                                   |   src=”http://cdn.site.com/                                                                                    |
|   src=”{vb:raw vboptions.bburl}/clientscript/                          |   src=”http://cdn.site.com/                                                                                    |
|   href=”clientscript/                                                  |   href=”http://cdn.site.com/                                                                                   |
|   url(./images/                                                        |   url(http://cdn.site.com/images/                                                                              |
|   url(images/                                                          |   url(http://cdn.site.com/images/                                                                              |
|   var IMGDIR_MISC=“images/misc”;var IMGDIR_BUTTON=“images/buttons”;  |   var IMGDIR_MISC=“http://cdn.site.com/images/misc”;var IMGDIR_BUTTON=“http://cdn.site.com/images/buttons”;  |

4\. If you have more directories for your images repeat the process for each of them.

5\. Integration has been completed! We highly recommend you to check the HTML code of your web page to ensure that URLs have been rewritten properly from your original ones to CNAME from the control panel.

To do that press F12 or open Developers Tools in your browser, choose the Network tab and refresh the page. All static files should have your CNAME in URLs.