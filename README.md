### Android之利用JSBridge库实现Html，JavaScript与Android的所有交互 
  <p>java 和 js互通框架 WebViewJavascriptBridge是移动UIView和Html交互通信的桥梁，用作者的话来说就是实现java和js的互相调用的桥梁。</p> 
<p>替代了WebView的自带的JavascriptInterface的接口，使得我们的开发更加灵活和安全。</p> 
<p>本博客把<strong><span style="color:#800000">JSBridge库近所有Android与（HTML+JS）的交互的方式全部实现，代码详细，注释清除</span></strong>，希望对各位有所帮助。</p> 
<p>效果如下图：</p> 
<p>　　　　　　<img alt="" src="https://static.oschina.net/uploads/img/201704/18140419_fHTv.gif"></p> 
<p>开发前的准备：<strong><span style="color:#800000">（两种方式选择，选一种即可）</span></strong></p> 
<p>　　　方式1：直接导入JSBridge的library包即可,&nbsp; AndroidStudio导library包请看博客：<a href="https://my.oschina.net/zhangqie/blog/881725" rel="nofollow">AndroidStudio怎样导入library项目开源库</a></p> 
<p>　　　　　　<a href="http://download.csdn.net/detail/dickyqie/9817471" rel="nofollow">&nbsp; library包点击下载</a></p> 
<p>&nbsp;</p> 
<p>　　&nbsp; 方式2：引入库，在bulid.gradle中添加如下代码</p> 
<pre><code class="language-html">repositories {
    maven { url "https://jitpack.io" }
}</code></pre> 
<p>&nbsp;</p> 
<pre><code>dependencies {
    compile 'com.github.lzyzsd:jsbridge:1.0.4'
}</code></pre> 
<p>&nbsp;</p> 

