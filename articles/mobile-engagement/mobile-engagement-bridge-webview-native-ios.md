---
title: Yerel Mobile Engagement iOS SDK ile iOS WebView köprüsü
description: Javascript ve yerel Mobile Engagement iOS SDK'sı çalıştıran Web görünümü arasında bir köprü oluşturmayı açıklar
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: ''
ms.assetid: e1d6ff6f-cd67-4131-96eb-c3d6318de752
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 07b2b8be80c090ce533f31cc28910f3ce7b91f73
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="bridge-ios-webview-with-native-mobile-engagement-ios-sdk"></a>Yerel Mobile Engagement iOS SDK ile iOS WebView köprüsü
> [!IMPORTANT]
> Azure Mobile Engagement 31/3/2018 üzerinde denemeler. Bu sayfa, kısa süre sonra silinir.
> 

> [!div class="op_single_selector"]
> * [Android Bridge](mobile-engagement-bridge-webview-native-android.md)
> * [iOS Bridge](mobile-engagement-bridge-webview-native-ios.md)
> 
> 

Bazı mobil uygulamalar, uygulamanın kendi yerel iOS Objective-C geliştirme kullanılarak geliştirilen ancak bile tümünü veya bazılarını ekranlar WebView iOS içinde işlenir bir karma uygulama olarak tasarlanmıştır. Bu tür uygulamaların içindeki Mobile Engagement iOS SDK'sı hala tüketebileceği ve Bu öğreticide bunu yapma hakkında Git açıklar. 

Her ikisi de belgelenmemiş ancak bunun için iki yaklaşım vardır:

* Bir bu ilk açıklanan [bağlantı](http://stackoverflow.com/questions/9826792/how-to-invoke-objective-c-method-from-javascript-and-send-back-data-to-javascrip) kaydetme içerir bir `UIWebViewDelegate` web görünümü ve catch-ve-hemen-iptal JavaScript'te yapılan bir konum Değiştir. 
* Bir bu ikinci dayanır [WWDC 2013 oturum](https://developer.apple.com/videos/play/wwdc2013/615), ilk temizleyici olduğu ve biz bu kılavuzun izleyecek bir yaklaşımdır. Bu yaklaşım yalnızca iOS7 ve üzeri sürümlerde çalıştığını unutmayın. 

Örnek iOS köprülemek için aşağıdaki adımları izleyin:

1. Öncelikle, çalıştınız emin olmak gereken bizim [Başlarken Öğreticisi](mobile-engagement-ios-get-started.md) karma uygulamanızı Mobile Engagement iOS SDK'sını tümleştirmek için. İsteğe bağlı olarak, biz görünümünden tetiklemek gibi SDK yöntemleri görebilmeniz için test gibi günlüğe kaydetmeyi etkinleştirebilirsiniz. 
   
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
           ....
             [EngagementAgent setTestLogEnabled:YES];
           ....
        }
2. Şimdi karma uygulamanızı bir Web görünümü ekranla olduğundan emin olun. Ona ekleyebilirsiniz `Main.storyboard` uygulamanın. 
3. Bu Web görünümü ile ilişkilendirmek, **ViewController** tıklatıp Görünüm denetleyicisini Sahne gelen webview sürükleyerek `ViewController.h` yerleştirerek ekran Düzenle yalnızca aşağıda `@interface` satır. 
4. Bunu yaptıktan sonra bir iletişim kutusu için bir ad isteyen açılır. Ad olarak **webView**. `ViewController.h` Dosya, aşağıdaki gibi görünmelidir:
   
        #import <UIKit/UIKit.h>
        #import "EngagementViewController.h"
   
        @interface ViewController : EngagementViewController
        @property (strong, nonatomic) IBOutlet UIWebView *webView;
   
        @end
5. Güncelleştireceğiz `ViewController.m` daha sonra dosya ancak yaygın olarak kullanılan bazı Mobile Engagement iOS SDK yöntemleri bir sarmalayıcı oluşturur köprüsü dosyasının ilk oluşturacağız. Adlı yeni bir üst bilgi dosyası oluştur **EngagementJsExports.h** kullanan `JSExport` daha önce bahsedilen içinde açıklanan mekanizması [oturum](https://developer.apple.com/videos/play/wwdc2013/615) yerel iOS yöntemleri göstermek için. 
   
        #import <Foundation/Foundation.h>
        #import <JavaScriptCore/JavascriptCore.h>
   
        @protocol EngagementJsExports <JSExport>
   
        + (void) sendEngagementEvent:(NSString*) name :(NSString*)extras;
        + (void) startEngagementJob:(NSString*) name :(NSString*)extras;
        + (void) endEngagementJob:(NSString*) name;
        + (void) sendEngagementError:(NSString*) name :(NSString*)extras;
        + (void) sendEngagementAppInfo:(NSString*) appInfo;
   
        @end
   
        @interface EngagementJs : NSObject <EngagementJsExports>
   
        @end
6. Sonraki ikinci bölümü köprüsü dosyasının oluşturacağız. Adlı bir dosya oluşturun **EngagementJsExports.m** Mobile Engagement iOS SDK yöntemlerini çağırarak gerçek sarmalayıcıları oluşturma uygulama içerecek. Ayrıca biz ayrıştırma Not `extras` webview JavaScript'ten geçirilen ve, içine koyma bir `NSMutableDictionary` ile Engagement SDK'sı yöntem çağrılarını geçirilecek nesnesi.  
   
        #import <UIKit/UIKit.h>
        #import "EngagementAgent.h"
        #import "EngagementJsExports.h"
   
        @implementation EngagementJs
   
        +(void) sendEngagementEvent:(NSString*)name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] sendEvent:name extras:extrasInput];
        }
   
        + (void) startEngagementJob:(NSString*) name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] startJob:name extras:extrasInput];
        }
   
        + (void) endEngagementJob:(NSString*) name {
           [[EngagementAgent shared] endJob:name];
        }
   
        + (void) sendEngagementError:(NSString*) name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] sendError:name extras:extrasInput];
        }
   
        + (void) sendEngagementAppInfo:(NSString*) appInfo {
           NSMutableDictionary* appInfoInput = [self ParseExtras:appInfo];
           [[EngagementAgent shared] sendAppInfo:appInfoInput];
        }
   
        + (NSMutableDictionary*) ParseExtras:(NSString*) input {
           NSData *data = [input dataUsingEncoding:NSUTF8StringEncoding];
           NSError* error = nil;
           NSMutableDictionary* extras = [NSJSONSerialization JSONObjectWithData:data options:0 error:&error];
   
           return extras;
        }
   
        @end
7. Biz geri gelmeniz artık **ViewController.m** ve aşağıdaki kod ile güncelleştirin: 
   
        #import <JavaScriptCore/JavaScriptCore.h>
        #import "ViewController.h"
        #import "EngagementJsExports.h"
   
        @interface ViewController ()
   
        @end
   
        @implementation ViewController
   
        - (void)viewDidLoad
        {
           self.webView.delegate = self;
           [super viewDidLoad];
           [self loadWebView];
        }
   
        - (void)loadWebView {
           NSBundle* mainBundle = [NSBundle mainBundle];
           NSURL* htmlPage = [mainBundle URLForResource:@"LocalPage" withExtension:@"html"];
   
           NSURLRequest* urlReq = [NSURLRequest requestWithURL:htmlPage];
           [self.webView loadRequest:urlReq];
        }
   
        - (void)webViewDidFinishLoad:(UIWebView*)wv
        {
           JSContext* context = [wv valueForKeyPath:@"documentView.webView.mainFrame.javaScriptContext"];
   
           context[@"EngagementJs"] = [EngagementJs class];
        }
   
        - (void)webView:(UIWebView*)wv didFailLoadWithError:(NSError*)error
        {
           NSLog(@"Error for WEBVIEW: %@", [error description]);
        }
   
        - (void)didReceiveMemoryWarning {
           [super didReceiveMemoryWarning];
           // Dispose of any resources that can be recreated.
        }
   
        @end
8. İlgili aşağıdaki noktalara dikkat edin **ViewController.m** dosyası:
   
   * İçinde `loadWebView` yöntemi, biz yüklenirken adlı yerel bir HTML dosyası **LocalPage.html** biz gözden sonraki kodu. 
   * İçinde `webViewDidFinishLoad` yöntemi, biz ele geçirme `JsContext` ve bizim sarmalayıcı sınıfı ile ilişkilendirme. Bu bizim sarmalayıcı işleci kullanılarak SDK yöntemleri çağırma sağlayacak **EngagementJs** webView gelen. 
9. Adlı bir dosya oluşturun **LocalPage.html** aşağıdaki kod ile:
   
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
                   alert('window.onerror: ' + err);
               }
   
               function send(inputId)
               {
                   var input = document.getElementById(inputId);
                   if(input)
                   {
                       var value = input.value;
                       // Example of how extras info can be passed with the Engagement logs
                       var extras = '{"CustomerId":"MS290011"}';
                   }
   
                   if(value && value.length > 0)
                   {
                       switch(inputId)
                       {
                           case "event":
                           EngagementJs.sendEngagementEvent(value, extras);
                           break;
   
                           case "job":
                           EngagementJs.startEngagementJob(value, extras);
                           window.setTimeout(
                           function(){
                               EngagementJs.endEngagementJob(value);
                           }, 10000 );
                           break;
   
                           case "error":
                           EngagementJs.sendEngagementError(value, extras);
                           break;
   
                           case "appInfo":
                           var appInfo = '{"customer_name":"' + value + '"}';
                           EngagementJs.sendEngagementAppInfo(appInfo);
                           break;
                       }
                   }
               }
               </script>
   
           </head>
           <body>
               <h1>Bridge Tester</h1>
   
               <div id='engagement'>
   
                   <br/>
                   <h2>Event</h2>
                   <input type="text" id="event" size="35">
                   <button onclick="send('event')">Send</button>
   
                   <br/>
                   <h2>Job</h2>
                   <input type="text" id="job" size="35">
                   <button onclick="send('job')">Send</button>
   
                   <br/>
                   <h2>Error</h2>
                   <input type="text" id="error" size="35">
                   <button onclick="send('error')">Send</button
   
                   <br/>
                   <h2>AppInfo</h2>
                   <input type="text" id="appInfo" size="35">
                   <button onclick="send('appInfo')">Send</button>
   
               </div>
           </body>
        </html>
10. Yukarıdaki HTML dosya ile ilgili aşağıdaki noktalara dikkat edin:
    
    * Olay, iş, hata, appInfo adları olarak kullanılacak verileri nerede sağlayabilir giriş kutularının kümesi içerir. Yanında düğmesine tıkladığınızda sonuçta Mobile Engagement iOS SDK'sı bu çağrıyı geçirmek için köprü dosyasından yöntemlerini çağıran Javascript için bir çağrı yapılır. 
    * Olaylar, işler ve bu nasıl yapılabilir göstermek için bile hatalar için statik bazı ek bilgiler üzerinde etiketleme. Bakarsanız JSON dize olarak, bu ek bilgileri gönderilir `EngagementJsExports.m` dosya, ayrıştırılır ve olaylar, işleri, hataları gönderme birlikte geçirildi. 
    * Mobile Engagement işi için 10 saniye çalıştırın ve bilgisayarı kapat giriş kutusunda belirttiğiniz adla koparılan. 
    * Mobile Engagement appInfo veya etiketi statik anahtarı ve değeri olarak giriş etiketi için girilen değer olarak 'customer_name' ile geçirilir. 
11. Uygulamayı çalıştırın ve aşağıdaki görürsünüz. Şimdi aşağıdaki gibi bir test olayı için bazı ad ve tıklatın **Gönder** yanında. 
    
     ![][1]
12. Giderseniz şimdi **İzleyici** 'ın altına bakın ve uygulama sekmesinde **olayları -> ayrıntıları**, statik uygulama biz Gönderen bilgisi birlikte görünmesini bu olay görürsünüz. 
    
    ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-ios/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-ios/event-output.png
