title: Fighting spam in Drupal 7 - disable the homepage field on comments
date: 2013-06-02 18:58:00
tags:
- drupal
- programming
---
Spam is really annoying, particularly for anybody maintaining a website with anonymous contribution - in my case via comments. Mollum filters the bulk of the spammers out. Unfortunately if you want to capture anonymous commenter's contact information in Drupal 7 this also exposes and displays the homepage field. This is also displayed to all visitors to your website that can access this content - incentive for link spammers :(
<!-- more -->
If you don't care about following up with your commenters, or you only allow commenting by registered users - the email and homepage field on anonymous comments can be disabled here:

Administration -> Structure -> Content Types -> [choose relevant content type] -> Comment settings -> Anonymous commenting

This however doesn't work for me - I like to be able to reach out to legitmate commenters via email and require it (but don't display it). I don't care for the homepage field. The easiest and quickest way to disable the field is to add the functionality yourself via a module. Adding this in a simple module  should do the trick (I stuck it in my "jeff_hacks" module, you need to update the prefix to your module name):

```
function jeff_hacks_form_alter(&$form){
    $form['author']['homepage']['#access'] = FALSE;
}
```
