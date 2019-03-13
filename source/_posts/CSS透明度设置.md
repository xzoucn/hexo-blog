---
title: CSS透明度设置
date: 2019-03-13 17:31:30
tags: CSS
categories: CSS
---
>.transparent_class {  
&nbsp; &nbsp;&nbsp; &nbsp;filter:alpha(opacity=50);  
&nbsp; &nbsp;&nbsp; &nbsp;-moz-opacity:0.5;  
&nbsp; &nbsp;&nbsp; &nbsp;-khtml-opacity: 0.5;  
&nbsp; &nbsp;&nbsp; &nbsp;opacity: 0.5;  
}  

opacity: 0.5; 当前的标准属性 . 大部分版本的Firefox, Safari 和 Opera都可以使用。
filter:alpha(opacity=50); 适用于IE。
-moz-opacity:0.5; 适用于老版本的Mozilla 浏览器如Netscape Navigator。
-khtml-opacity: 0.5; 适用于老版本的 Safari (1.x)，它采用KTHML而不是WebKit。
