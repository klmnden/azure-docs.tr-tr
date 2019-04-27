---
title: Azure Notification Hubs ile güvenli anında iletme bildirimleri gönderme
description: Azure'dan bir Android uygulamasına güvenli anında iletme bildirimleri göndermeyi öğrenin. Java ve C# içinde yazılan kod örneklerini.
documentationcenter: android
keywords: anında iletme bildirimi, anında iletme bildirimleri, anında iletme iletileri, android anında iletme bildirimleri
author: jwargo
manager: patniko
editor: spelluru
services: notification-hubs
ms.assetid: daf3de1c-f6a9-43c4-8165-a76bfaa70893
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: android
ms.devlang: java
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: 27536b0a3d7e0858a5660b4c7b33cb6679b5fbf1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60874571"
---
# <a name="sending-secure-push-notifications-with-azure-notification-hubs"></a>Azure Notification Hubs ile güvenli anında iletme bildirimleri gönderme

> [!div class="op_single_selector"]
> * [Windows Evrensel](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)

## <a name="overview"></a>Genel Bakış

> [!IMPORTANT]
> Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).

Microsoft azure'da anında iletme bildirimi desteği, bir kullanımı kolay, çok platformlu, ölçeği genişletilen anında ileti altyapısı, tüketici hem kurumsal uygulamalar için anında iletme bildirimleri yürütmesinin büyük ölçüde basitleştiren erişmenize olanak tanır. Mobil platformlar.

Yasal nedeniyle veya güvenlik kısıtlamaları, bazen bir uygulama bir sorun standart bir anında iletme bildirimi altyapısı aracılığıyla aktarılan bildirim dahil olmak isteyebilirsiniz. Bu öğreticide, istemci Android cihaz ve uygulama arka ucu arasında güvenli, kimliği doğrulanmış bir bağlantı üzerinden hassas bilgiler göndererek aynı deneyimi elde etmek açıklar.

Yüksek bir düzeyde akışı aşağıdaki gibidir:

1. Uygulama arka ucu:
   * Arka uç veritabanı güvenli yükteki depolar.
   * Bu bildirim kimliği (hiçbir güvenli bilgiler gönderilir) Android cihaza gönderir.
2. Cihazın, bildirim alındığında uygulama:
   * Android cihaz güvenli yükü isteme ve arka uç bağlantı kurar.
   * Uygulamanın cihaz bildirim olarak yük gösterebilirsiniz.

Önceki akış (ve Bu öğreticide), bu kullanıcının oturum açması sonra cihaz kimlik doğrulama belirteci yerel depolama alanında depolar, kabul edilir dikkat edin önemlidir. Bu yaklaşım, cihaz bildirimin güvenli yükü bu belirteci kullanarak alabilirsiniz gibi sorunsuz bir deneyim garanti eder. Uygulamanız kimlik doğrulama belirteçlerinizi cihazda depolamaz veya bu belirteçlerin süresi, uygulamayı başlatmak için kullanıcıdan genel bir bildirim alır almaz anında iletme bildirimi cihaz uygulaması görüntülemelidir. Uygulama kullanıcının kimliğini doğrular ve bildirim yükü gösterir.

Bu öğreticide, güvenli bir anında iletme bildirimleri göndermek gösterilir. Yapılar [kullanıcılara bildirme](notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md) henüz yapmadıysanız adımları Bu öğreticinin ilk önce tamamlamanız gereken şekilde öğretici.

> [!NOTE]
> Bu öğreticide oluşturduğunuz ve bildirim hub'ınıza açıklandığı gibi yapılandırılmış varsayılır [bildirim hub'ları (Android) ile çalışmaya başlama](notification-hubs-android-push-notification-google-gcm-get-started.md).

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-android-project"></a>Android projesine değiştirme

Uygulama göndermek için arka değiştirdiğiniz göre yalnızca *kimliği* bir anında iletme bildirimi, Android uygulamanızı bu bildirim işlemek ve geri görüntülenecek güvenli ileti almak için arka uç çağırmak için değiştirmeniz gerekir.
Bu hedefe ulaşmak için Android uygulamanızı anında iletme bildirimleri alan zaman kendisi ile arka uç kimlik doğrulaması yapmayı bilir emin olmanız gerekir.

Şimdi değiştirmek *oturum açma* paylaşılan tercihlerini uygulamanızın kimlik doğrulaması üstbilgi değeri kaydetmek için akış. Benzer mekanizmaları uygulamanın kullanıcı kimlik bilgilerini ihtiyaç duymadan kullanabilmesine olduğu tüm kimlik doğrulama belirteci (örneğin, OAuth belirteçlerini) depolamak için kullanılabilir.

1. Android uygulaması projenizde, aşağıdaki sabitler üstüne ekleyin. `MainActivity` sınıfı:

    ```java
    public static final String NOTIFY_USERS_PROPERTIES = "NotifyUsersProperties";
    public static final String AUTHORIZATION_HEADER_PROPERTY = "AuthorizationHeader";
    ```
2. Hala `MainActivity` sınıfı, güncelleştirme `getAuthorizationHeader()` yöntemini aşağıdaki kod içerir:

    ```java
    private String getAuthorizationHeader() throws UnsupportedEncodingException {
        EditText username = (EditText) findViewById(R.id.usernameText);
        EditText password = (EditText) findViewById(R.id.passwordText);
        String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
        basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);

        SharedPreferences sp = getSharedPreferences(NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
        sp.edit().putString(AUTHORIZATION_HEADER_PROPERTY, basicAuthHeader).commit();

        return basicAuthHeader;
    }
    ```
3. Aşağıdaki `import` deyimleri en üstündeki `MainActivity` dosyası:

    ```java
    import android.content.SharedPreferences;
    ```

Şimdi, bildirim alındığında çağrılan işleyici değiştirin.

1. İçinde `MyHandler` sınıfı değişiklik `OnReceive()` içerecek şekilde yöntemi:

    ```java
    public void onReceive(Context context, Bundle bundle) {
        ctx = context;
        String secureMessageId = bundle.getString("secureId");
        retrieveNotification(secureMessageId);
    }
    ```
2. Ardından Ekle `retrieveNotification()` yer tutucusunu değiştirerek yöntemi `{back-end endpoint}` arka ucunuz dağıtırken elde arka uç noktası ile:

    ```java
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
    ```

Bu yöntem, uygulama paylaşılan tercihlerini depolanan kimlik bilgilerini kullanarak bildirim içeriği almak için arka çağırır ve normal bir bildirim olarak görüntüler. Bildirim, uygulama kullanıcının herhangi bir anında iletme bildirimi gibi tam olarak arar.

Tarafından arka uç kimlik doğrulaması üstbilgi özelliği eksik ya da ret örneklerini işlemek için daha iyidir. Bu gibi durumlarda belirli işlenmesini çoğunlukla hedef kullanıcı deneyiminizi bağlıdır. Gerçek bildirim almak için kullanıcının kimliğini doğrulamak bildirim genel bir istemle görüntüle bir seçenektir.

## <a name="run-the-application"></a>Uygulamayı çalıştırın

Uygulamayı çalıştırmak için aşağıdaki eylemleri gerçekleştirin:

1. Emin **AppBackend** Azure'da dağıtılır. Visual Studio kullanıyorsanız, çalıştırma **AppBackend** Web API uygulaması. Bir ASP.NET web sayfası görüntülenir.
2. Eclipse'te, uygulamayı fiziksel bir Android cihaz veya öykünücü üzerinde çalıştırın.
3. Android uygulamasında kullanıcı Arabirimi, bir kullanıcı adı ve parola girin. Bunlar herhangi bir dize olabilir, ancak aynı değere sahip olmalıdır.
4. Android uygulama kullanıcı Arabirimindeki, tıklayın **oturum**. Ardından **gönderin, anında iletme**.
