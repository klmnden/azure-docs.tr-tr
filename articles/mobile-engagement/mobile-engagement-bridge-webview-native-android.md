---
title: "Yerel Mobile Engagement Android SDK'sı Android WebView köprüsü"
description: "Javascript ve yerel Mobile Engagement Android SDK'sı çalıştıran Web görünümü arasında bir köprü oluşturmayı açıklar"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: cf272f3f-2b09-41b1-b190-944cdca8bba2
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: f4fc7b3c81747ec80974a99084eeb1acc311f11f
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="bridge-android-webview-with-native-mobile-engagement-android-sdk"></a>Yerel Mobile Engagement Android SDK'sı Android WebView köprüsü
> [!div class="op_single_selector"]
> * [Android köprüsü](mobile-engagement-bridge-webview-native-android.md)
> * [iOS köprüsü](mobile-engagement-bridge-webview-native-ios.md)
> 
> 

Bazı mobil uygulamalar, uygulamanın kendi yerel Android geliştirme kullanılarak geliştirilen ancak ekranlar bile tümünün veya bir Android WebView içinde işlenir bir karma uygulama olarak tasarlanmıştır. Mobile Engagement Android SDK içinde bu tür uygulamalar hala tüketebileceği ve Bu öğreticide bunu yapma hakkında Git açıklar. Aşağıdaki örnek kod tabanlı üzerinde Android belgelerini [burada](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript). Bunu nasıl belgelenen bu yaklaşım karma uygulama görünümünden de olayları izlemesine, işler, hatalar, app-info isteklerine bizim Android SDK cmdlet'ine sırasında başlatabilirsiniz, Mobile Engagement Android SDK'ın yaygın olarak kullanılan yöntemleri için aynı uygulamak için kullanılabilecek açıklar. 

1. Öncelikle, çalıştınız emin olmak gereken bizim [Başlarken Öğreticisi](mobile-engagement-android-get-started.md) Mobile Engagement Android SDK karma uygulamanıza tümleştirmek için. Bunu yaptığınızda, `OnCreate` yöntemi, aşağıdaki gibi görünür.  
   
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
   
            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("<Mobile Engagement Conn String>");
            EngagementAgent.getInstance(this).init(engagementConfiguration);
        }
2. Şimdi karma uygulamanızı bir Web görünümü ekranla olduğundan emin olun. Burada size yerel bir HTML dosyası yüklenen kodunu aşağıdakine benzer olacaktır **örnek.HTML** Webview içinde `onCreate` ekranınızın yöntemi. 
   
        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
        }
   
        protected void onCreate(Bundle savedInstanceState) {
            ...
            ...
            SetWebView();
        }
3. Şimdi adlı bir köprü dosyası oluşturun **WebAppInterface** Mobile Engagement Android SDK yöntemlerini kullanarak bir sarmalayıcı bazı yaygın olarak oluşturan kullanılan `@JavascriptInterface` yaklaşım açıklanan [Android belgelerine](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript):
   
        import android.content.Context;
        import android.os.Bundle;
        import android.util.Log;
        import android.webkit.JavascriptInterface;
   
        import com.microsoft.azure.engagement.EngagementAgent;
   
        import org.json.JSONArray;
        import org.json.JSONObject;
   
        public class WebAppInterface {
            Context mContext;
   
            /** Instantiate the interface and set the context */
            WebAppInterface(Context c) {
                mContext = c;
            }
   
            @JavascriptInterface
            public void sendEngagementEvent(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendEvent(name, ParseExtras(extras));
            }
   
            @JavascriptInterface
            public void startEngagementJob(String name, String extras ){
                EngagementAgent.getInstance(mContext).startJob(name, ParseExtras(extras));
            }
   
            @JavascriptInterface
            public void endEngagementJob(String name){
                EngagementAgent.getInstance(mContext).endJob(name);
            }
   
            @JavascriptInterface
            public void sendEngagementError(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendError(name, ParseExtras(extras));
            }
   
            @JavascriptInterface
            public void sendEngagementAppInfo(String appInfo){
                EngagementAgent.getInstance(mContext).sendAppInfo(ParseExtras(appInfo));
            }
   
            public Bundle ParseExtras(String input) {
                Bundle extras = new Bundle();
   
                try {
                    JSONObject jObject = new JSONObject(input);
                    extras.putString(jObject.names().getString(0),
                            jObject.get(jObject.names().getString(0)).toString());
                } catch (Exception e) {
                    e.printStackTrace();
                }
                return extras;
            }
        }  
4. Biz yukarıdaki köprüsü dosyasını oluşturduktan sonra Web görünümü ile ilişkili olduğundan emin gerekir. Bu gerçekleşecek şekilde düzenlemeniz gerekir, `SetWebview` yöntemi şekilde aşağıdaki gibi görünür:
   
        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
            WebSettings webSettings = myWebView.getSettings();
            webSettings.setJavaScriptEnabled(true);
            myWebView.addJavascriptInterface(new WebAppInterface(this), "EngagementJs");
        }
5. Yukarıdaki kod parçacığında, biz adlı `addJavascriptInterface` bizim köprüsü sınıfı bizim Webview ile ilişkilendirmek için ve ayrıca adlı bir tanıtıcı oluşturulan **EngagementJs** köprüsü dosyasından yöntemleri çağırmak için. 
6. Şimdi adlı aşağıdaki dosya oluşturmak **örnek.HTML** projenize bir klasör olarak adlandırılan **varlıklar** Webview yüklenir ve burada çağrılacak yöntem köprüsü dosyasından.
   
        <!doctype html>
        <html>
            <head>
                <style type='text/css'>
                    html { font-family:Helvetica; color:#222; }
                    h1 { color:steelblue; font-size:22px; margin-top:16px; }
                    h2 { color:grey; font-size:14px; margin-top:18px; margin-bottom:0px; }
                </style>
   
                <script type="text/javascript">
   
                    window.onerror = function(err)
                    {
                      log('window.onerror: ' + err);
                    }
   
                    send = function(inputId)
                    {
                        var input = document.getElementById(inputId);
                        if(input)
                        {
                            var value = input.value;
                            // Example of how extras info can be passed with the Engagement logs
                            var extras = '{"CustomerId":"MS290011"}';
   
                            if(value && value.length > 0)
                            {
                                switch(inputId)
                                {
                                    case "event":
                                    EngagementJs.sendEngagementEvent(value, extras);
                                    break;
   
                                    case "job":
                                    EngagementJs.startEngagementJob(value, extras);
                                    window.setTimeout( function()
                                    {
                                      EngagementJs.endEngagementJob(value);
                                    }, 10000 );
                                    break;
   
                                    case "error":
                                    EngagementJs.sendEngagementError(value, extras);
                                    break;
   
                                    case "appInfo":
                                    EngagementJs.sendEngagementAppInfo({"customer_name":value});
                                    break;
                                }
                            }
                        }
                    }
                </script>
            </head>
            <body>
                <h1>Bridge Tester</h1>
                <div id='engagement'>
                    <h2>Event</h2>
                    <input type="text" id="event" size="35">
                    <button onclick="send('event')">Send</button>
   
                    <h2>Job</h2>
                    <input type="text" id="job" size="35">
                    <button onclick="send('job')">Send</button>
   
                    <h2>Error</h2>
                    <input type="text" id="error" size="35">
                    <button onclick="send('error')">Send</button>
   
                    <h2>AppInfo</h2>
                    <input type="text" id="appInfo" size="35">
                    <button onclick="send('appInfo')">Send</button>
                </div>
            </body>
        </html>
7. Yukarıdaki HTML dosya ile ilgili aşağıdaki noktalara dikkat edin:
   
   * Olay, iş, hata, appInfo adları olarak kullanılacak verileri nerede sağlayabilir giriş kutularının kümesi içerir. Yanında düğmesine tıkladığınızda sonuçta Mobile Engagement Android SDK'sı bu çağrıyı geçirmek için köprü dosyasından yöntemlerini çağıran Javascript için bir çağrı yapılır. 
   * Olaylar, işler ve bu nasıl yapılabilir göstermek için bile hatalar için statik bazı ek bilgiler üzerinde etiketleme. Bakarsanız JSON dize olarak, bu ek bilgileri gönderilir `WebAppInterface` dosya, ayrıştırılır ve bir Android put `Bundle` ve olaylar, işleri, hataları gönderme birlikte geçirilir. 
   * Mobile Engagement işi için 10 saniye çalıştırın ve bilgisayarı kapat giriş kutusunda belirttiğiniz adla koparılan. 
   * Mobile Engagement appInfo veya etiketi statik anahtarı ve değeri olarak giriş etiketi için girilen değer olarak 'customer_name' ile geçirilir. 
8. Uygulamayı çalıştırın ve aşağıdaki görürsünüz. Şimdi aşağıdaki gibi bir test olayı için bazı ad ve tıklatın **Gönder** altındaki. 
   
    ![][1]
9. Giderseniz şimdi **İzleyici** 'ın altına bakın ve uygulama sekmesinde **olayları -> ayrıntıları**, statik uygulama biz Gönderen bilgisi birlikte görünmesini bu olay görürsünüz. 
   
   ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-android/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-android/event-output.png
