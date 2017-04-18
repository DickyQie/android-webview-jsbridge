# Android之利用JSBridge库实现Html，JavaScript与Android的所有交互 
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
<p>1：默认方式（两种（1：<span style="color:#000000">DefaultHandler默认的方式</span>）；2：自定类实现）</p> 
<pre><code class="language-java">//展示第一种
bridgeWebView.setDefaultHandler(new DefaultHandler());</code></pre> 
<pre><code class="language-java">//data是JavaScript返回的数据
private void setHandler(){

        bridgeWebView.setDefaultHandler(new BridgeHandler() {
            @Override
            public void handler(String data, CallBackFunction function) {
                Toast.makeText(MainActivity.this,"DefaultHandler默认："+data,Toast.LENGTH_LONG).show();
            }
        });
   }</code></pre> 
<p>JS</p> 
<pre><code class="language-javascript">connectWebViewJavascriptBridge(function(bridge) {
            bridge.init(function(message, responseCallback) {
                console.log('JS got a message', message);
                var data = {
                    'json': 'JS返回任意数据!'
                };
                console.log('JS responding with', data);/*打印信息*/
                 document.getElementById("init").innerHTML = "data = " + message;
                responseCallback(data);
            });</code></pre> 
<p>2：Html点击事件利用JS function方法调Android端并相互传值。</p> 
<pre><code class="language-javascript">function testClick() {
            var str1 = document.getElementById("text1").value;
            var str2 = document.getElementById("text2").value;

            window.WebViewJavascriptBridge.callHandler(
                'submitFromWeb'
                , {'Data': 'json数据传给Android端'}  //该类型是任意类型
                , function(responseData) {
                    document.getElementById("show").innerHTML = "得到Java传过来的数据 data = " + responseData
                }
            );
        }</code></pre> 
<p>Android.Java</p> 
<pre><code class="language-java">

 //注册submitFromWeb方法
        bridgeWebView.registerHandler("submitFromWeb", new BridgeHandler() {
            @Override
            public void handler(String data, CallBackFunction function) {
                Log.i(TAG,"得到JS传过来的数据 data ="+data);
                show(data);
                function.onCallBack("传递数据给JS");
            }
        });</code></pre> 
<p>3：Android点击事件调用JS方法并相互传值。</p> 
<pre><code class="language-java">@Override
    public void onClick(View v) {
        //Java 调JS的functionJs方法并得到返回值
        bridgeWebView.callHandler("functionJs", "Android", new CallBackFunction() {
            @Override
            public void onCallBack(String data) {
                // TODO Auto-generated method stub
               show(data);
            }

        });
    }</code></pre> 
<p>JS.js</p> 
<pre><code class="language-java"> bridge.registerHandler("functionJs", function(data, responseCallback) {
                document.getElementById("show").innerHTML = ("Android端: = " + data);
                var responseData = "Javascript 数据";
                responseCallback(responseData);//回调返回给Android端
            });</code></pre> 
<p>send方式（包含又返回值和无返回值两种）</p> 
<p>无返回值：</p> 
<pre><code class="language-java">bridgeWebView.send("无返回值");</code></pre> 
<pre><code class="language-javascript">function testClick() {
            var str1 = document.getElementById("text1").value;
            var str2 = document.getElementById("text2").value;
            //将Android端得到的数据在网页上显示，并其他数据传给Android端，  可用于初始化和点击操作
            var data = {id: 1, content: "我是内容哦"};
            window.WebViewJavascriptBridge.send(
                data
                , function(responseData) {
                    document.getElementById("show").innerHTML = "data = " + responseData
                }
            );
       }</code></pre> 
<p>其他方式，如 文件；</p> 
<p>&nbsp;</p> 
<p>代码稍微有点多，就不一 一展示了，直接下载即可</p> 
<p>&nbsp;</p> 
