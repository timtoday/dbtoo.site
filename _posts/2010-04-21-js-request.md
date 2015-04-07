---
title:js版request
layout: post
tags:
- js
---
<div> var Request=function(name)<br/>{<br/>    new RegExp("(^|&amp;)"+name+"=([^&amp;]*)").exec(window.location.search.substr(1));<br/>    return RegExp.$2<br/>} </div>