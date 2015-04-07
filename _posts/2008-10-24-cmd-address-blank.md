---
title:浏览器地址栏实用命令
layout: post
tags:
- js
- script
---
<div> 表单解锁<p>                        以下为引用的内容：<br/>            javascript:var input=document.getElementsByTagName("input");for(var i=0;i&lt;input.length;i++){input[i].disabled=false;}alert("解锁完毕");             <br/>表单锁定</p><p>                        以下为引用的内容：<br/>            javascript:var input=document.getElementsByTagName("input");for(var i=0;i&lt;input.length;i++){input[i].disabled=true;}alert("锁定完毕");             <br/>链接统计</p><p>                        以下为引用的内容：<br/>            javascript:alert("链接数: "+document.links.length+"\n表单数: "+document.forms.length+"\n框架数: "+document.frames.length+"\n图片数: "+document.images.length);             <br/>终极输入                        以下为引用的内容：<br/> <p>javascript:var input=document.getElementsByTagName("input");for(var i=0;i&lt;input.length;i++){var newvar=prompt(input[i].name+"="+input[i].value,input[i].value);if(newvar != null){input[i].value=newvar;}}alert("终极输入完毕!");</p> </p> </div>