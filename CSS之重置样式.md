# 第四章 CSS之重置样式

windows的IE8下图片出现蓝色边框，可能是初始化样式中未设置img{border:none;}所导致的。

在有的浏览器下，a标签hover状态时，虽然对a本身已经设置过a{text-decoration:none;}其文字会出现下划线，主要原因可能是初始化样式中未设置a:hover{text-decoration:none;} ，即对hover状态也要进行去除下划线的初始化。
