# Website to Android Application
## Features
Ad-blocking functionality is implemented by restricting network requests to third-party URLs.

* **Pull-to-Refresh:** Custom touch listener that reloads the page when swiping down from the top of the screen.
* **Fullscreen Video Playback:** Automatically handles screen orientation and UI visibility for embedded web videos.
* **Zoom Lock:** Injects a custom meta viewport tag via JavaScript to disable user scaling, maintaining a native UI feel.
* **Link Restriction:** Restricts web navigation exclusively to the defined target URL.
* **History Navigation:** Intercepts the hardware back button to navigate the web history before closing the application.

## Implementation
( [Sketchware Pro](https://github.com/Sketchware-Pro/Sketchware-Pro/releases) / Java )

### 1. `onCreate` Setup

```java
final String URL="https://cin.wiki";final android.webkit.WebView webview1=new android.webkit.WebView(this);setContentView(webview1);webview1.getSettings().setJavaScriptEnabled(true);webview1.getSettings().setDomStorageEnabled(true);android.webkit.CookieManager.getInstance().setAcceptCookie(true);webview1.setOnTouchListener(new android.view.View.OnTouchListener(){private float startY;@Override public boolean onTouch(android.view.View v,android.view.MotionEvent event){switch(event.getAction()){case android.view.MotionEvent.ACTION_DOWN:startY=event.getY();break;case android.view.MotionEvent.ACTION_UP:float endY=event.getY();if((endY-startY)>500&&webview1.getScrollY()==0){webview1.reload();}break;}return false;}});webview1.setWebViewClient(new android.webkit.WebViewClient(){@Override public boolean shouldOverrideUrlLoading(android.webkit.WebView view,String url){return!url.startsWith(URL);}@Override public void onPageStarted(android.webkit.WebView view,String url,android.graphics.Bitmap favicon){super.onPageStarted(view,url,favicon);injectDisableZoom(view);}@Override public void onPageFinished(android.webkit.WebView view,String url){super.onPageFinished(view,url);injectDisableZoom(view);}private void injectDisableZoom(android.webkit.WebView view){view.loadUrl("javascript:(function(){var head=document.getElementsByTagName('head')[0];var meta=document.createElement('meta');meta.name='viewport';meta.content='width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no';head.appendChild(meta);})()");}});webview1.setWebChromeClient(new android.webkit.WebChromeClient(){private android.view.View v;private CustomViewCallback cb;@Override public void onShowCustomView(android.view.View view,CustomViewCallback callback){v=view;cb=callback;getWindow().getDecorView().setSystemUiVisibility(5894);setRequestedOrientation(android.content.pm.ActivityInfo.SCREEN_ORIENTATION_SENSOR_LANDSCAPE);((android.view.ViewGroup)getWindow().getDecorView()).addView(v);}@Override public void onHideCustomView(){((android.view.ViewGroup)getWindow().getDecorView()).removeView(v);getWindow().getDecorView().setSystemUiVisibility(0);setRequestedOrientation(android.content.pm.ActivityInfo.SCREEN_ORIENTATION_UNSPECIFIED);if(cb!=null)cb.onCustomViewHidden();}});webview1.loadUrl(URL);
```

### 2. `onBackPressed` Setup

```java
android.webkit.WebView webview1=(android.webkit.WebView)getWindow().getDecorView().getRootView().findFocus();if(webview1!=null&&webview1.canGoBack()){webview1.goBack();}else{finish();}
```
