# Website to Android Application
Ad-blocking implemented by restricting network requests to third-party URLs.

## Features

* **Pull-to-Refresh:** Custom touch listener that reloads the page when swiping down from the top of the screen.
* **Fullscreen Video Playback:** Automatically handles screen orientation and UI visibility for embedded web videos.
* **Zoom Lock:** Injects a custom meta viewport tag via JavaScript to disable user scaling, maintaining a native UI feel.
* **History Navigation:** Intercepts the back key event to navigate the web history before closing the application.

## Implementation
Modify the URL then compile it using [Sketchware Pro](https://github.com/Sketchware-Pro/Sketchware-Pro/releases) or another Java-based application builder.

### `onCreate`

```java
final String URL="https://s13.nontonanimeid.boats";final android.webkit.WebView w=new android.webkit.WebView(this);setContentView(w);w.getSettings().setJavaScriptEnabled(true);w.getSettings().setDomStorageEnabled(true);w.getSettings().setSupportZoom(true);w.getSettings().setBuiltInZoomControls(true);android.webkit.CookieManager.getInstance().setAcceptCookie(true);w.setOnTouchListener(new android.view.View.OnTouchListener(){float y;public boolean onTouch(android.view.View v,android.view.MotionEvent e){if(e.getAction()==0)y=e.getY();if(e.getAction()==1&&e.getY()-y>500&&w.getScrollY()==0)w.reload();return false;}});w.setWebViewClient(new android.webkit.WebViewClient(){public boolean shouldOverrideUrlLoading(android.webkit.WebView v,String u){return!u.startsWith(URL);}});w.setWebChromeClient(new android.webkit.WebChromeClient(){android.view.View v;CustomViewCallback c;public void onShowCustomView(android.view.View i,CustomViewCallback k){v=i;c=k;getWindow().getDecorView().setSystemUiVisibility(5894);setRequestedOrientation(4);((android.view.ViewGroup)getWindow().getDecorView()).addView(v);}public void onHideCustomView(){((android.view.ViewGroup)getWindow().getDecorView()).removeView(v);getWindow().getDecorView().setSystemUiVisibility(0);setRequestedOrientation(2);if(c!=null)c.onCustomViewHidden();}});w.loadUrl(URL);
```

### `onBackPressed`

```java
android.webkit.WebView webview1=(android.webkit.WebView)getWindow().getDecorView().getRootView().findFocus();if(webview1!=null&&webview1.canGoBack()){webview1.goBack();}else{finish();}
```

## Support Us
By joining our [Telegram Channel](https://t.me/S_O_S_P)
