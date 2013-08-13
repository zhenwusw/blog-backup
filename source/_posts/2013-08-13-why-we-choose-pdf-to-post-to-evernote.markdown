---
layout: post
title: "why we choose PDF to post to Evernote"
date: 2013-08-13 07:34
comments: true
categories: 
---


# Problem

  Evernote integration is one of the outside service connections in our
system. There are two problems when we decide to create a note by using
Evernote API.

  As we know, Evernote use a specific markup language called ENML to
represent the content of notes. But problem is that ENML is designed to
represent the static content of documents, 'active' elements of XHTML
such as forms and scripts are not allowed within it. (WHAT!) We have
some route maps which need to execute Javascript and render by google
Maps API v3. 

  Another problem is in ENML document all styles must be specified
in-line using the 'style' attributes. References to external or embedded
stylesheets and the use of the 'class' attribute are not allowed.
(WHAT!) That means we need to duplicate and transform our complicated
HTML documents to ENML format with verbose styling?

  That's terrible things. With Evernote API, we send documents through
background jobs, that means we have nowhere, at least for now, to
generate the maps before putting documents to Evernote server. And we
don't want to maintain duplicated HTML documents as well.
  

# Solutions   
   
  Ok, maybe there are some internal technologies inside Evernote to
support rendering Google maps in a note, it was just not documented, or
they have some ways to allow me not to make HTML to inline styling ENML.
Unfortunately answers to both questions are no, but one of the
developers suggested that we can upload google maps as attached images
and use Evernote Web Clipper, a Chrome extension, to help the
transforming of the existing HTML documents. 

* Use static Google map version
  
  After research, this approach was also voted down since there is a
1024 bytes limitation for the Google static map url but that's not
suitable for most of the route maps and there's no way to sidestep.


* Use PhantomJS and CasperJS to generate map images

  Actually rendering route maps can be done without browsers, we can
take advantage of headless Webkit scriptable environments such as
[PhantomJS](!http://phantomjs.org/). With the help of CasperJS(an
utility for PhantomJS), I successfully create nice looking images of
google maps. So the process becomes: I need to take it into two parts to
create a note, one is to create ENML compatible template as note content
and one is to make an image as note attachment. But there are also some
other unconsidered problems: what about the web font of the content,
what if the template needs to modify, how to handle images after
creating notes. Other way is we don't need to maintain these ENML
templates, just make the whole document as a big image and attach it to
Evernote, but it's unsearchable, believe me, that's important! Also
PhantomJS is rather a heavy tool to do this job as well.
  

* Make the whole document as PDF

  No maintaining duplicated documents, no big images, and no PhantomJS.
Last way we can think about is to make a PDF of the whole page,
including route maps and other information, although PDF is kind of
larger of the size than plain ENML with images. Finally, I decided to
use [wkhtmltopdf](!https://code.google.com/p/wkhtmltopdf/), a simple
shell utility to convert html to pdf using the webkit rendering engine,
and it works good.
   
{% img center /images/plan.png 800 300 Place Kitten #2 %}

# Learn

  It took me nearly one week to investigate and make tests, actually
this is one of some prerequisites before I started to implement. It's
known as boundary conditions when we're doing integration work and I
shouldn't have presumed that there **is** a way to do this job without
doing any investigation.
