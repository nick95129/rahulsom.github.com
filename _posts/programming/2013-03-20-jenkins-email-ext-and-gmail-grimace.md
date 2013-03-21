---
layout: post
title: "Jenkins, Email Ext and Gmail Grimace"
description: ""
category: programming
tags: []
---
{% include JB/setup %}

If you're working on any project that involves more than one developer and 
weeks or possibly months of work, you should be using some sort of Continuous 
Integration. The most common system to use is 
[Jenkins](http://jenkins-ci.org/).

If you start using Jenkins and start committing code often, you'll realize you
need email notifications on build status on each commit. The default emails 
from Jenkins are to put it mildly, *a bit lacking*.

The solution is, the 
[Email-ext](https://wiki.jenkins-ci.org/display/JENKINS/Email-ext+plugin)
plugin. This lets you customize your emails. They have some decent templates 
in their code. However you'll want to customize your emails further. I took 
the approach of taking their template and making it prettier with 
[Zurb's responsive email](http://www.zurb.com/playground/responsive-email-templates)
templates. I use Mail.app on the Mac and the Mail app on my iPhone. Things 
were going great until I committed something to github with my gmail id.

That's when I learnt about 
[GMail Grimace](http://www.flickr.com/groups/project-gmail-grimace/).

Rewriting all my templates to not use style tags was not an option. Changing 
the appearance of an html element using css classes that are variables in your 
template is super easy. Doing that using if conditions in your template, is a 
pain in the ass.

Then I ran into this [SO](http://stackoverflow.com/questions/4521557/automatically-convert-style-sheets-to-inline-style).
So I decided to use this information and create a `pre-send` script for the 
Email-ext plugin. However I learnt that [there were some problems](https://groups.google.com/forum/?fromgroups=#!topic/jenkinsci-users/Avme-hTCeDs).

So I decided to [hack the plugin](https://github.com/jenkinsci/email-ext-plugin/pull/60) to do what I want. It's now part of v2.28.

All you need to do is create a `style` tag in your email with an additional
attribute `data-inline` and value `true`.

{% highlight html %}
<style type="text/css" data-inline="true">
  div.good {
  	background-color: blue;
  }
  div.bad {
  	background-color: red;
  }
</style>
<style type="text/css">
  ... more styles ...
</style>
{% endhighlight %}

The plugin now will take this css and apply it inline to all elements. This
makes GMail happy.

Here's my [Jelly Template](https://gist.github.com/rahulsom/5125421).
<script src="https://gist.github.com/rahulsom/5125421.js">Gist comes here</script>
