# 🌐 Website to Android Application

Turn any website into a lightweight native Android app — no Android Studio, no boilerplate. Just drop in a URL and compile.

Built with a minimal, dependency-free `WebView` wrapper and includes built-in ad-blocking by restricting requests to third-party domains.

---

## ✨ Features

| Feature | Description |
|---|---|
| 🚫 **Ad Blocking** | Restricts network requests to third-party URLs, keeping the experience clean. |
| 🔄 **Pull-to-Refresh** | Single-finger touch listener reloads the page when swiping down from the top of the screen. |
| 🎬 **Fullscreen Video Playback** | Automatically handles screen orientation and system UI visibility for embedded web videos, with null-safe callback handling. |
| 🔍 **Zoom Lock** | Injects a custom meta viewport tag via JavaScript to disable user scaling, keeping a native UI feel. |
| ⬅️ **History Navigation** | Intercepts the back key event to navigate web history before closing the app. |
| 🖼️ **Lazy-Image Loader** | Forces hidden/lazy-loaded images (`data-src`, `data-original`, etc.) to render immediately, with a `MutationObserver` to catch dynamically injected content. |
| 📐 **Responsive Grid Injection** | Injects custom CSS to reflow post/card layouts into a clean, responsive grid (2–4 columns based on screen width). |
| 🕵️ **Custom User-Agent** | Spoofs a mobile Chrome user-agent string for consistent site rendering. |
| 🍪 **Full Cookie Support** | Accepts both first-party and third-party cookies for sites that require persistent sessions. |
| 🔎 **Pinch-to-Zoom Controls** | Built-in zoom controls enabled (on-screen buttons hidden) alongside the zoom-lock injection. |
| ⚡ **Popup & Media Handling** | Allows JS-triggered windows and autoplay media without requiring a user gesture. |

---

## 🛠️ Implementation

1. Set your target `URL` in the snippet below.
2. Compile it using [Sketchware Pro](https://github.com/Sketchware-Pro/Sketchware-Pro/releases) or another Java-based Android app builder.

### `onCreate`

```java
final String URL="https://";final android.webkit.WebView webview1=new android.webkit.WebView(this);setContentView(webview1);final android.webkit.WebSettings settings=webview1.getSettings();settings.setJavaScriptEnabled(true);settings.setDomStorageEnabled(true);settings.setDatabaseEnabled(true);settings.setLoadsImagesAutomatically(true);settings.setBlockNetworkImage(false);settings.setUseWideViewPort(true);settings.setLoadWithOverviewMode(false);webview1.setInitialScale(0);settings.setSupportZoom(true);settings.setBuiltInZoomControls(true);settings.setDisplayZoomControls(false);settings.setCacheMode(android.webkit.WebSettings.LOAD_DEFAULT);settings.setJavaScriptCanOpenWindowsAutomatically(true);settings.setMediaPlaybackRequiresUserGesture(false);android.webkit.CookieManager cookieManager=android.webkit.CookieManager.getInstance();cookieManager.setAcceptCookie(true);cookieManager.setAcceptThirdPartyCookies(webview1,true);settings.setUserAgentString("Mozilla/5.0 (Linux; Android 10; K) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/151.0.0.0 Mobile Safari/537.36");webview1.setFocusable(true);webview1.setFocusableInTouchMode(true);webview1.setHorizontalScrollBarEnabled(true);webview1.setVerticalScrollBarEnabled(true);webview1.setOverScrollMode(android.view.View.OVER_SCROLL_ALWAYS);webview1.setOnTouchListener(new android.view.View.OnTouchListener(){private float startY;@Override public boolean onTouch(android.view.View v,android.view.MotionEvent event){switch(event.getAction()){case android.view.MotionEvent.ACTION_DOWN:if(event.getPointerCount()==1){startY=event.getY();}break;case android.view.MotionEvent.ACTION_UP:if(event.getPointerCount()==1){float endY=event.getY();if((endY-startY)>500&&webview1.getScrollY()==0){webview1.reload();}}break;}return false;}});webview1.setWebViewClient(new android.webkit.WebViewClient(){@Override public boolean shouldOverrideUrlLoading(android.webkit.WebView view,String url){return!url.startsWith(URL);}@Override public void onPageStarted(android.webkit.WebView view,String url,android.graphics.Bitmap favicon){super.onPageStarted(view,url,favicon);injectDisableZoom(view);}@Override public void onPageFinished(android.webkit.WebView view,String url){super.onPageFinished(view,url);String js="(function(){try{var head=document.head;if(head){var meta=document.querySelector('meta[name=\"viewport\"]');if(!meta){meta=document.createElement('meta');meta.name='viewport';head.appendChild(meta);}meta.setAttribute('content','width=device-width,initial-scale=1.0');}var st=document.createElement('style');st.textContent='.misha_posts_wrap{display:grid!important;grid-template-columns:repeat(2,1fr)!important;gap:12px!important;padding:10px!important;}.animeseries{display:flex!important;flex-direction:column!important;width:auto!important;float:none!important;margin:0!important;background:#fff!important;border-radius:10px!important;overflow:hidden!important;box-shadow:0 2px 8px rgba(0,0,0,0.1)!important;}.animeseries .sera{display:block!important;margin:0!important;width:100%!important;position:relative!important;background:#e0e0e0!important;aspect-ratio:3/4!important;overflow:hidden!important;}.sera .limit{display:block!important;padding:0!important;height:100%!important;position:relative!important;}.animeseries img,.sera .limit img{position:absolute!important;top:0!important;left:0!important;width:100%!important;height:100%!important;object-fit:cover!important;}.animeseries .judul,.animeseries .episode,.animeseries .rating,.animeseries .date,.animeseries .title,.animeseries .meta,.animeseries .info{display:block!important;padding:4px 8px!important;font-size:12px!important;line-height:1.5!important;word-break:break-word!important;overflow:hidden!important;text-overflow:ellipsis!important;max-height:3em!important;}.animeseries .judul{font-weight:bold!important;font-size:13px!important;color:#333!important;margin-top:4px!important;}.animeseries .episode{color:#666!important;font-size:11px!important;}.animeseries .rating{color:#f59e0b!important;font-size:11px!important;}@media(min-width:380px){.misha_posts_wrap{grid-template-columns:repeat(3,1fr)!important;}}@media(min-width:600px){.misha_posts_wrap{grid-template-columns:repeat(4,1fr)!important;}}';if(head){head.appendChild(st);}function loadImages(){var imgs=document.getElementsByTagName('img');for(var i=0;i<imgs.length;i++){var img=imgs[i];var src=null;var names=['data-src','data-original','data-lazy-src','data-original-src','data-image','data-url','data-lazy-srcset'];for(var j=0;j<names.length;j++){var x=img.getAttribute(names[j]);if(x&&x.length>5){src=x;break;}}var ss=img.getAttribute('data-srcset');if(ss){img.setAttribute('srcset',ss);}if(src){img.setAttribute('src',src);}try{img.loading='eager';img.decoding='async';}catch(e){}var c=img.className;if(typeof c==='string'){img.className=c.replace(/lazy|lazyload|lazy-loaded/gi,'');}try{img.style.visibility='visible';img.style.opacity='1';}catch(e){}}}loadImages();try{window.dispatchEvent(new Event('scroll'));window.dispatchEvent(new Event('resize'));}catch(e){}window.scrollBy(0,1);window.scrollBy(0,-1);setTimeout(function(){loadImages();window.dispatchEvent(new Event('scroll'));},300);setTimeout(function(){loadImages();window.dispatchEvent(new Event('scroll'));},1000);setTimeout(function(){loadImages();window.dispatchEvent(new Event('scroll'));},2000);setTimeout(function(){loadImages();window.dispatchEvent(new Event('scroll'));},4000);if(window.MutationObserver){new MutationObserver(function(){loadImages();}).observe(document.body,{childList:true,subtree:true});}}catch(e){}})()";view.evaluateJavascript(js,null);injectDisableZoom(view);}private void injectDisableZoom(android.webkit.WebView view){view.loadUrl("javascript:(function(){var head=document.getElementsByTagName('head')[0];var meta=document.createElement('meta');meta.name='viewport';meta.content='width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no';head.appendChild(meta);})()");}});webview1.setWebChromeClient(new android.webkit.WebChromeClient(){private android.view.View v;private CustomViewCallback cb;@Override public void onShowCustomView(android.view.View view,CustomViewCallback callback){v=view;cb=callback;getWindow().getDecorView().setSystemUiVisibility(5894);setRequestedOrientation(android.content.pm.ActivityInfo.SCREEN_ORIENTATION_SENSOR_LANDSCAPE);((android.view.ViewGroup)getWindow().getDecorView()).addView(v);}@Override public void onHideCustomView(){if(v!=null){((android.view.ViewGroup)getWindow().getDecorView()).removeView(v);v=null;}getWindow().getDecorView().setSystemUiVisibility(0);setRequestedOrientation(android.content.pm.ActivityInfo.SCREEN_ORIENTATION_UNSPECIFIED);if(cb!=null){cb.onCustomViewHidden();cb=null;}}});webview1.loadUrl(URL);
```

> **Note:** the injected CSS block that reflows the site's post/card layout into a responsive grid is site-specific (targets particular class names). Swap it out for selectors matching your target site's markup.

### `onBackPressed`

```java
android.webkit.WebView webview1=(android.webkit.WebView)getWindow().getDecorView().getRootView().findFocus();if(webview1!=null&&webview1.canGoBack()){webview1.goBack();}else{finish();}
```

---

## 💬 Support Us

Join our [Telegram Channel](https://t.me/S_O_S_P) for updates and support.
