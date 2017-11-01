title: Redirect an old hostname to new hostname using mod_rewrite and Drupal
date: 2013-08-10 16:27:00
tags:
- drupal
---
I was browsing through the referer logs in my website and noticed Drupal was serving content to my old hostname, http://oolportal.com . SEO gurus also suggest it's a [bad idea to serve the same content from multiple domains](http://searchenginewatch.com/article/2219759/Multiple-Domain-Choice-SEO-Considerations-of-Single-vs.-Multiple-Domains), and that you can transfer link value using a redirect. Over the long run I want the references to all end up going to http://poweredbyjeff.com so it's time to take corrective action.
<!-- more -->
The simplest way I have found to achieve this is to have Apache's mod_rewrite simply send the browser a 301 "permanent redirect" whenever my old hostname is requested. I find .htaccess to be some voodoo... the [documentation](http://httpd.apache.org/docs/2.0/misc/rewriteguide.html) is intense reading and best done after lots of sleep and at least 2 cappucinos. Also make sure you have brushed up on [regular expressions](http://www.regular-expressions.info/reference.html) if you want to understand the fine detail here. Having said that, here is my solution:
```
RewriteCond %{HTTP_HOST} ^(.*)oolportal.com$ [NC]
RewriteRule ^(.*)$ http://poweredbyjeff.com/$1 [R=301,L]
```
These 2 lines are added to my Drupal installation's .htaccess file just below the sections for adding/removing the www. prefix automagically. Works a charm, you can try it by replacing "poweredbyjeff" with "oolportal" in your browser and see the redirection magic!
