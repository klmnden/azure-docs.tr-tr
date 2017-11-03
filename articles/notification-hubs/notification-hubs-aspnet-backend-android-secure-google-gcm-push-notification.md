---
title: "Azure Notification Hubs ile güvenli anında iletme bildirimleri gönderme"
description: "Azure'dan bir Android uygulamasına güvenli anında iletme bildirimleri göndermek öğrenin. Java ve C# içinde yazılan kod örnekleri."
documentationcenter: android
keywords: "anında iletme bildirimi, anında iletme bildirimleri, anında iletileri, android anında iletme bildirimleri"
author: ysxu
manager: erikre
editor: 
services: notification-hubs
ms.assetid: daf3de1c-f6a9-43c4-8165-a76bfaa70893
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: android
ms.devlang: java
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 29f8c516e611c13fb73c7edc15e7c52708c75bb0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="sending-secure-push-notifications-with-azure-notification-hubs"></a>Azure Notification Hubs ile güvenli anında iletme bildirimleri gönderme
> [!div class="op_single_selector"]
> * [Windows Evrensel](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)
> 
> 

## <a name="overview"></a>Genel Bakış
> [!IMPORTANT]
> Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).
> 
> 

Microsoft Azure anında iletme bildirimi desteği, mobil platformlar için tüketici ve kurumsal uygulama için anında iletme bildirimleri uyarlamasını büyük ölçüde basitleştirir kullanımı kolay, çok platformlu, ölçeği anında iletme iletisi altyapı erişmenize olanak tanır.

Yasal nedeniyle veya güvenlik kısıtlamaları, bazen bir uygulama bir şey standart anında iletme bildirimi altyapısı iletilen bildirimi dahil olmak isteyebilirsiniz. Bu öğretici, hassas bilgileri istemci Android cihaz ve uygulama arka ucu arasında güvenli, kimliği doğrulanmış bir bağlantı aracılığıyla göndererek aynı deneyimi elde etmek açıklar.

Yüksek bir düzeyde akışı aşağıdaki gibidir:

1. Uygulama arka ucu:
   * Arka uç veritabanı güvenli yükünde depolar.
   * Bu bildirim Kimliğini (güvenli hiçbir bilgi gönderilmez) Android cihaza gönderir.
2. Bildirim alırken cihaza uygulamanın:
   * Android cihaz güvenli yükü isteyen arka uç bağlantı kurar.
   * Uygulama yükü cihaz bildirim olarak gösterebilir.

Önceki akış (ve Bu öğreticide), kullanıcı oturum açtığında sonra cihaz kimlik doğrulama belirtecini yerel depoda sakladığı varsayıyoruz olduğunu dikkate almak önemlidir. Cihaz Bu belirteci kullanarak bildirim 's güvenli yükü alabilir gibi tamamen sorunsuz bir deneyim daha güvence altına alır. Uygulamanızın kimlik doğrulama belirteçleri cihazda depolamaz veya bu belirteçleri süresi, anında iletme bildirimi aldığında gerçekleştireceği cihaz uygulaması uygulamayı başlatmak için kullanıcıdan genel bir bildirim görüntülemelidir. Uygulama kullanıcının kimliğini doğrular ve bildirim yükü gösterir.

Bu öğretici güvenli anında iletme bildirimleri göndermeyi gösterir. Derlemeler [kullanıcılara bildirme](notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md) henüz yapmadıysanız, bu öğreticide ilk adımları şekilde öğretici.

> [!NOTE]
> Bu öğreticide oluşturduğunuz ve bildirim hub'ınızı açıklandığı şekilde yapılandırılmış varsayar [bildirim hub'ları (Android) ile çalışmaya başlama](notification-hubs-android-push-notification-google-gcm-get-started.md).
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-android-project"></a>Android projesi değiştirme
Uygulama göndermek için uç değiştiren göre yalnızca *kimliği* bir anında iletme bildirimi Android uygulamanızı bu bildirim işlemek ve görüntülenecek güvenli ileti almak için arka uç geri arama için değiştirmeniz gerekir.
Bu hedefe ulaşmak için Android uygulamanızı anında iletme bildirimleri aldığında kendisi ile arka uç kimlik doğrulaması yapmayı bilir emin olmak zorunda.

Biz şimdi değiştirecek *oturum açma* kimlik doğrulaması üstbilgi değeri, uygulamanızın paylaşılan tercihlerinde kaydetmek için akış. Benzer mekanizmaları uygulama kullanıcı kimlik bilgilerini gerek kalmadan kullanmak zorunda kalırsınız tüm kimlik doğrulama belirteci (örneğin OAuth belirteçlerini) depolamak için kullanılabilir.

1. Android uygulaması projenize en üstünde olan aşağıdaki sabitleri ekleyin **MainActivity** sınıfı:
   
        public static final String NOTIFY_USERS_PROPERTIES = "NotifyUsersProperties";
        public static final String AUTHORIZATION_HEADER_PROPERTY = "AuthorizationHeader";
2. Hala **MainActivity** sınıfı, güncelleştirme `getAuthorizationHeader()` yöntemi aşağıdaki kod içerir:
   
        private String getAuthorizationHeader() throws UnsupportedEncodingException {
            EditText username = (EditText) findViewById(R.id.usernameText);
            EditText password = (EditText) findViewById(R.id.passwordText);
            String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
            basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);
   
            SharedPreferences sp = getSharedPreferences(NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            sp.edit().putString(AUTHORIZATION_HEADER_PROPERTY, basicAuthHeader).commit();
   
            return basicAuthHeader;
        }
3. Aşağıdakileri ekleyin `import` deyimleri en üstündeki **MainActivity** dosyası:
   
        import android.content.SharedPreferences;

Şimdi biz bildirim alındığında çağrılan işleyici değiştireceksiniz.

1. İçinde **MyHandler** sınıf değişiklik `OnReceive()` yöntemi içerir:
   
        public void onReceive(Context context, Bundle bundle) {
            ctx = context;
            String secureMessageId = bundle.getString("secureId");
            retrieveNotification(secureMessageId);
        }
2. Ardından ekleyin `retrieveNotification()` yer tutucu değiştirme yöntemi `{back-end endpoint}` , arka uç dağıtırken elde arka uç uç noktası ile:
   
        private void retrieveNotification(final String secureMessageId) {
            SharedPreferences sp = ctx.getSharedPreferences(MainActivity.NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            final String authorizationHeader = sp.getString(MainActivity.AUTHORIZATION_HEADER_PROPERTY, null);
   
            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
                        HttpUriRequest request = new HttpGet("{back-end endpoint}/api/notifications/"+secureMessageId);
                        request.addHeader("Authorization", "Basic "+authorizationHeader);
                        HttpResponse response = new DefaultHttpClient().execute(request);
                        if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                            Log.e("MainActivity", "Error retrieving secure notification" + response.getStatusLine().getStatusCode());
                            throw new RuntimeException("Error retrieving secure notification");
                        }
                        String secureNotificationJSON = EntityUtils.toString(response.getEntity());
                        JSONObject secureNotification = new JSONObject(secureNotificationJSON);
                        sendNotification(secureNotification.getString("Payload"));
                    } catch (Exception e) {
                        Log.e("MainActivity", "Failed to retrieve secure notification - " + e.getMessage());
                        return e;
                    }
                    return null;
                }
            }.execute(null, null, null);
        }

Bu yöntem, uygulama paylaşılan tercihlerinde saklanan kimlik bilgilerini kullanarak bildirim içerik almak için uç çağırır ve normal bildirim olarak görüntüler. Diğer bir anında iletme bildirimi gibi tam olarak uygulama kullanıcıya bildirim arar.

Arka uç tarafından durumlarında eksik kimlik doğrulama üstbilgisi özelliği ya da reddetme tercih olduğuna dikkat edin. Bu durumlarda belirli işleme çoğunlukla, hedef kullanıcı deneyimi bağlıdır. Gerçek bildirim almak için kullanıcının kimliğini doğrulamak bildirim genel istemiyle görüntüle bir seçenektir.

## <a name="run-the-application"></a>Uygulamayı çalıştırın
Uygulamayı çalıştırmak için aşağıdakileri yapın:

1. Emin olun **AppBackend** Azure'da dağıtılır. Visual Studio kullanarak çalıştırırsanız **AppBackend** Web API uygulaması. Bir ASP.NET web sayfası görüntülenir.
2. Eclipse'te, fiziksel bir Android cihaz veya öykünücü uygulamayı çalıştırın.
3. Android uygulama kullanıcı Arabirimi, bir kullanıcı adı ve parola girin. Bunlar herhangi bir dize olabilir, ancak aynı değeri olması gerekir.
4. Android uygulamada UI'ı tıklatın **oturum**. Ardından **Gönder itme**.

