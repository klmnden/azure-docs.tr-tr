---
title: Azure App Service Mobile Apps özelliğini bir Apache Cordova uygulamasında anında iletme bildirimleri ekleme | Microsoft Docs
description: Apache Cordova uygulamanıza anında iletme bildirimleri göndermek için Mobile Apps'ı kullanmayı öğrenin.
services: app-service\mobile
documentationcenter: javascript
manager: crdun
editor: ''
author: elamalani
ms.assetid: 92c596a9-875c-4840-b0e1-69198817576f
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 06/25/2019
ms.author: emalani
ms.openlocfilehash: e6755c3fb1fca342d94fdaa96c0dce614d762172
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443564"
---
# <a name="add-push-notifications-to-your-apache-cordova-app"></a>Apache Cordova uygulamanıza anında iletme bildirimleri ekleme

[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

> [!NOTE]
> Visual Studio App Center, mobil uygulama geliştirme merkezi hizmetlerinde yeni ve tümleşik yatırım yapıyor. Geliştiriciler **derleme**, **Test** ve **Dağıt** hizmetlerinin sürekli tümleştirme ve teslim işlem hattı ayarlayın. Uygulama dağıtıldığında, geliştiriciler kendi uygulamasını kullanarak kullanımı ve durumu izleyebilirsiniz **Analytics** ve **tanılama** kullanarak kullanıcılarla etkileşim kurun ve hizmetlerini **anında iletme** hizmeti. Geliştiriciler de yararlanabilir **Auth** , kullanıcıların kimliğini doğrulamak ve **veri** kalıcı hale getirmek ve uygulama verilerini bulutta eşitleme hizmeti. Kullanıma [App Center](https://appcenter.ms/?utm_source=zumo&utm_campaign=app-service-mobile-cordova-get-started-push) bugün.
>

## <a name="overview"></a>Genel Bakış

Bu öğreticide, anında iletme bildirimleri ekleme [Apache Cordova hızlı][5] anında iletme bildirimi kayıt eklenen her zaman cihaza gönderilir, böylece proje.

İndirilen hızlı başlangıç sunucu projesi kullanmazsanız, anında iletme bildirimi uzantı paketi gerekir. Daha fazla bilgi için [Mobile Apps için .NET arka uç sunucu SDK'sı ile çalışma][1].

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide, Visual Studio 2015 ile geliştirilmiş bir Apache Cordova uygulaması sahibi olduğunuzu varsayar. Bu cihaz, Google Android öykünücüsü, bir Android cihazı, bir Windows cihazı veya bir iOS cihazı çalıştırmanız gerekir.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Bir bilgisayarla [Visual Studio Community 2015][2] veya üzeri
* [Apache Cordova için Visual Studio Araçları][4]
* [Etkin bir Azure hesabı][3]
* Tamamlanmış bir [Apache Cordova hızlı][5] proje
* (Android) A [Google hesabı][6] ile doğrulanmış bir e-posta adresi
* (iOS) Bir [Apple Developer Program üyeliği][7] ve bir iOS cihazı (iOS Simulator desteklemiyor anında iletme bildirimleri)
* (Windows) A [Microsoft Store Geliştirici hesabı][8] ve Windows 10 cihaz

## <a name="configure-hub"></a>Bildirim hub'ı yapılandırma

[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

[Bu bölümdeki adımları gösteren bir video izleyin][9].

## <a name="update-the-server-project"></a>Sunucu projesini güncelleştir

[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="add-push-to-app"></a>Cordova uygulamanızı değiştirme

Apache Cordova uygulaması projenizi anında iletme bildirimleri işlemeye hazır olmasını sağlamak için Cordova anında iletme eklentisi artı herhangi bir platforma özgü anında iletme hizmetlerini yükleyin.

#### <a name="update-the-cordova-version-in-your-project"></a>Projenizi Cordova sürümünü güncelleştirin.

Projeniz Apache Cordova 6.1.1 sürümünden önceki bir sürümünü kullanıyorsa, istemci projesinin güncelleştirin. Projeyi güncelleştirmek için aşağıdaki adımları uygulayın:

* Yapılandırma tasarımcısını açmak için sağ `config.xml`.
* Seçin **platformları** sekmesi.
* İçinde **Cordova CLI** metin kutusunda **6.1.1**. 
* Proje güncelleştirmeye seçin **derleme**ve ardından **Çözümü Derle**.

#### <a name="install-the-push-plugin"></a>Anında iletme eklentisini yükleme

Apache Cordova uygulamalarını yerel olarak cihazı veya ağ özellikleri işleyemez.  Bu özellikler tarafından sağlanan olan eklentiler üzerinde ya da yayımlanan [npm][10] veya GitHub üzerinde. `phonegap-plugin-push` Eklentisi ağ anında iletme bildirimleri işler.

Anında iletme eklentisi aşağıdaki yöntemlerle yükleyebilirsiniz:

**Komut isteminden:**

Şu komutu çalıştırın:

    cordova plugin add phonegap-plugin-push

**Gelen Visual Studio içinde:**

1. Çözüm Gezgini'nde açın `config.xml` dosya. Ardından, **eklentileri** > **özel**. Ardından **Git** yükleme kaynağı olarak.

2. Girin `https://github.com/phonegap/phonegap-plugin-push` kaynağı olarak.

    ![Çözüm Gezgini'nde Config.cml dosyasını açın][img1]

3. Yükleme kaynağı yanındaki oku seçin.

4. İçinde **SENDER_ID**, Google Geliştirici Konsolu proje için bir sayısal proje kimliği zaten varsa, burada ekleyebilirsiniz. Aksi takdirde, 777777 gibi bir yer tutucu değerini girin. Android hedefliyorsanız, bu değer daha sonra config.xml dosyasında güncelleştirebilirsiniz.

    >[!NOTE]
    >2\.0.0 sürümü itibarıyla, google-services.json Gönderen Kimliği yapılandırmak için projenizin kök klasöründe yüklü olması gerekir Daha fazla bilgi için [yükleme belgelerine.](https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/INSTALLATION.md)

5. **Add (Ekle)** seçeneğini belirleyin.

Anında iletme eklentisi artık yüklüdür.

#### <a name="install-the-device-plugin"></a>Cihaz eklentisini yükleme

Anında iletme eklentisini yüklemek için kullandığınız yordamın aynısını izleyin. Çekirdek eklentiler listesinden cihaz eklentisini ekleyin. (Bulmak için seçin **eklentileri** > **çekirdek**.) Platform adı almak için bu eklentiyi ihtiyacınız vardır.

#### <a name="register-your-device-when-the-application-starts"></a>Uygulama başlatıldığında Cihazınızı kaydedin 

Başlangıçta, bazı çok az kod için Android ekliyoruz. Daha sonra uygulamayı iOS veya Windows 10 çalışacak şekilde değiştirebilirsiniz.

1. Bir çağrı ekleyin **registerForPushNotifications** sırasında oturum açma işlemi için geri çağırma. Alternatif olarak, alt kısmında ekleyebilirsiniz **onDeviceReady** yöntemi:

    ```javascript
    // Log in to the service.
    client.login('google')
        .then(function () {
            // Create a table reference.
            todoItemTable = client.getTable('todoitem');

            // Refresh the todoItems.
            refreshDisplay();

            // Wire up the UI Event Handler for the Add Item.
            $('#add-item').submit(addItemHandler);
            $('#refresh').on('click', refreshDisplay);

                // Added to register for push notifications.
            registerForPushNotifications();

        }, handleError);
    ```

    Bu örnek, arama gösterir **registerForPushNotifications** kimlik doğrulaması başarılı olduktan sonra. Çağırabilirsiniz `registerForPushNotifications()` gereklidir sıklıkta.

2. Yeni Ekle **registerForPushNotifications** yöntemini aşağıdaki şekilde:

    ```javascript
    // Register for push notifications. Requires that phonegap-plugin-push be installed.
    var pushRegistration = null;
    function registerForPushNotifications() {
        pushRegistration = PushNotification.init({
            android: { senderID: 'Your_Project_ID' },
            ios: { alert: 'true', badge: 'true', sound: 'true' },
            wns: {}
        });

    // Handle the registration event.
    pushRegistration.on('registration', function (data) {
        // Get the native platform of the device.
        var platform = device.platform;
        // Get the handle returned during registration.
        var handle = data.registrationId;
        // Set the device-specific message template.
        if (platform == 'android' || platform == 'Android') {
            // Register for GCM notifications.
            client.push.register('gcm', handle, {
                mytemplate: { body: { data: { message: "{$(messageParam)}" } } }
            });
        } else if (device.platform === 'iOS') {
            // Register for notifications.
            client.push.register('apns', handle, {
                mytemplate: { body: { aps: { alert: "{$(messageParam)}" } } }
            });
        } else if (device.platform === 'windows') {
            // Register for WNS notifications.
            client.push.register('wns', handle, {
                myTemplate: {
                    body: '<toast><visual><binding template="ToastText01"><text id="1">$(messageParam)</text></binding></visual></toast>',
                    headers: { 'X-WNS-Type': 'wns/toast' } }
            });
        }
    });

    pushRegistration.on('notification', function (data, d2) {
        alert('Push Received: ' + data.message);
    });

    pushRegistration.on('error', handleError);
    }
    ```
3. (Android) Önceki kod içinde `Your_Project_ID` uygulamanızdan için kimliği ile sayısal proje [Google Developer Console][18].

## <a name="optional-configure-and-run-the-app-on-android"></a>(İsteğe bağlı) Yapılandırma ve uygulama Android'de çalıştırma

Android için anında iletme bildirimlerini etkinleştirmek için bu bölümü tamamlayın.

#### <a name="enable-gcm"></a>Etkinleştirme Firebase Cloud Messaging

Google Android platformu başlangıçta hedeflediğiniz olduğundan, Firebase Cloud Messaging etkinleştirmeniz gerekir.

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

#### <a name="configure-backend"></a>FCM kullanarak anında iletme istekleri göndermek için mobil uygulama arka ucu yapılandırın

[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

#### <a name="configure-your-cordova-app-for-android"></a>Android için Cordova uygulamanızı yapılandırın

Cordova uygulamanızı açın **config.xml**. Ardından değiştirin `Your_Project_ID` uygulamanızdan için kimliği ile sayısal proje [Google Developer Console][18].

```xml
<plugin name="phonegap-plugin-push" version="1.7.1" src="https://github.com/phonegap/phonegap-plugin-push.git">
    <variable name="SENDER_ID" value="Your_Project_ID" />
</plugin>
```

Açık **index.js**. Ardından, sayısal proje kimliği kullanmak için kodu güncelleştirin

```javascript
pushRegistration = PushNotification.init({
    android: { senderID: 'Your_Project_ID' },
    ios: { alert: 'true', badge: 'true', sound: 'true' },
    wns: {}
});
```

#### <a name="configure-device"></a>USB hata ayıklama için Android Cihazınızı yapılandırma

Android cihazınıza uygulamanızı dağıtabilmeniz için önce USB hata ayıklamayı etkinleştirmek gerekir. Android telefonunuzda aşağıdaki adımları uygulayın:

1. Git **ayarları** > **telefon hakkında**. Ardından dokunun **yapı numarası** Geliştirici modu (yaklaşık yedi kat) etkinleştirilene kadar.
2. Geri **ayarları** > **Geliştirici seçenekleri**, etkinleştirme **USB hata ayıklamanın**. Ardından Android telefonunuz geliştirme PC bir USB kablosu ile bağlayın.

Biz Android 6.0 (Marshmallow) çalıştıran bir Google Nexus 5 X cihaz kullanarak test. Ancak, herhangi bir modern Android yayın arasında teknikleri yaygındır.

#### <a name="install-google-play-services"></a>Google Play hizmetlerini yükleyin

Android Google Play Hizmetleri için anında iletme bildirimleri gönderme eklenti kullanır.

1. Visual Studio'da **Araçları** > **Android** > **Android SDK Yöneticisi**. Ardından **ek özellikler** klasör. Her biri aşağıdaki Sdk'lardan yüklendiğinden emin olmak için uygun kutuları işaretleyin:

   * Android 2.3 veya üstü
   * Google deposu düzeltme 27 veya daha yüksek
   * Google Play Services 9.0.2 veya üstü

2. Seçin **yükleme paketleri**. Ardından yüklemenin tamamlanmasını bekleyin.

Geçerli gerekli kitaplıkları listelenen [modul phonegap plugin push yükleme belgelerine][19].

#### <a name="test-push-notifications-in-the-app-on-android"></a>Android uygulamasında test anında iletme bildirimleri

Şimdi test anında iletme bildirimleri, uygulamayı çalıştırarak ve öğeleri Todoıtem tablosu ekleme yapabilirsiniz. Aynı arka uç kullanmakta olduğunuz sürece, aynı cihaza ikinci bir CİHAZDAN veya test edebilirsiniz. Cordova uygulamanızı Android platformunda aşağıdaki yollardan biriyle test edin:

* *Fiziksel bir cihaz üzerinde:* Android Cihazınızı geliştirme bilgisayarınıza bir USB kablosuyla iliştirin.  Yerine **Google Android öykünücüsü**seçin **cihaz**. Visual Studio, cihaza uygulama dağıtır ve uygulamayı çalıştırır. Cihazdaki uygulama ile etkileşim kurabilir.

  Ekran paylaşımı gibi uygulamalar [Mobizen][20] Android uygulamaları geliştirmenize yardımcı olabilir. Mobizen Bilgisayarınızda bir web tarayıcısı Android ekrana yansıtıyor.

* *Bir Android emulator'da:* Bir öykünücü kullanılırken gerekli olan ek yapılandırma adımları vardır.

    Android sanal cihazı (AVD) Yöneticisi'nde gösterildiği Google API'leri hedef olarak ayarlanmış olan bir sanal cihaza dağıttığınız emin olun.

    ![Android sanal cihaz Yöneticisi](./media/app-service-mobile-cordova-get-started-push/google-apis-avd-settings.png)

    Daha hızlı x86 kullanmak istiyorsanız öykünücüsü [HAXM sürücüsü yüklemek][11]ve ardından öykünücü kullanmak için yapılandırın.

    Bir Google hesabı seçerek Android cihaza ekleyin **uygulamaları** > **ayarları** > **Hesap Ekle**. Ardından yönergeleri izleyin.

    ![Android cihaza bir Google hesabı ekleme](./media/app-service-mobile-cordova-get-started-push/add-google-account.png)

    Önce Yapılacaklar listesi uygulaması olarak çalıştırmak ve yeni bir todo öğesini ekleyin. Bu kez, bildirim alanında bir bildirim simgesi görüntülenir. Bildirimin tam metin görüntülemek için bildirim çekmecesini açabilirsiniz.

    ![Görünüm bildirimi](./media/app-service-mobile-cordova-get-started-push/android-notifications.png)

## <a name="optional-configure-and-run-on-ios"></a>(İsteğe bağlı) Yapılandırma ve İos'ta çalıştırma

Bu bölümde, iOS cihazlarında Cordova projesi çalıştırmaya yöneliktir. İOS cihazları ile çalışmayan, bu bölümü atlayabilirsiniz.

#### <a name="install-and-run-the-ios-remote-build-agent-on-a-mac-or-cloud-service"></a>Yükleme ve bir Mac veya Bulut hizmeti iOS uzak derleme Aracısı'nı çalıştırın

Visual Studio kullanarak iOS üzerinde bir Cordova uygulaması çalıştırmadan önce adımları inceleyin [iOS Kurulum Kılavuzu][12] yükleme ve uzak derleme Aracısı'nı çalıştırın.

İOS için uygulama oluşturabileceğinizi emin olun. Kurulum Kılavuzu'ndaki adımları Visual Studio'dan iOS için uygulama oluşturmak için gereklidir. Bir Mac yoksa, iOS için Macıncloud gibi bir hizmet üzerindeki uzak derleme aracısını kullanarak oluşturabilirsiniz. Daha fazla bilgi için [iOS uygulamanızı bulutta çalıştırma][21].

> [!NOTE]
> Xcode 7 veya üst sürümü, İos'ta anında iletme eklentisini kullanmak için gereklidir.

#### <a name="find-the-id-to-use-as-your-app-id"></a>Uygulama Kimliğinize kullanmak üzere bir kimlik bulunamadı

Uygulamanız için anında iletme bildirimleri, Cordova uygulamanızı açık Config.XML'de kaydetmeden önce Bul `id` öznitelik değeri pencere öğesinde bulunan ve daha sonra kullanmak üzere kopyalayın. Aşağıdaki XML'de Kimliğin şöyle olduğunu `io.cordova.myapp7777777`.

```xml
<widget defaultlocale="en-US" id="io.cordova.myapp7777777"
    version="1.0.0" windows-packageVersion="1.1.0.0" xmlns="https://www.w3.org/ns/widgets"
    xmlns:cdv="http://cordova.apache.org/ns/1.0" xmlns:vs="http://schemas.microsoft.com/appx/2014/htmlapps">
```

Daha sonra Apple'nın Geliştirici Portalı'nda bir uygulama kimliği oluşturduğunuzda bu tanımlayıcıyı kullanın. Farklı bir uygulama kimliği Geliştirici portalında oluşturursanız, bu öğreticinin ilerleyen bölümlerinde birkaç ek adımlar uygulaması gerekir. Pencere öğesi kimliği, geliştirici portalında uygulama Kimliğinin eşleşmesi gerekir.

#### <a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>Apple'nın Geliştirici portalında anında iletme bildirimleri için uygulamayı kaydetme

[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

[Benzer adımları gösteren bir video izleyin](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-5-Set-up-apns-for-push)

#### <a name="configure-azure-to-send-push-notifications"></a>Anında iletme bildirimleri göndermek için Azure'ı yapılandırma

[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

#### <a name="verify-that-your-app-id-matches-your-cordova-app"></a>Uygulama Kimliğinize Cordova uygulamanızı eşleştiğini doğrulayın

Apple Geliştirici hesabınızda önceden oluşturulmuş bir uygulama kimliği config.xml dosyasında pencere öğesinin kimliği eşleşiyorsa, bu adımı atlayabilirsiniz. Kimlikler eşleşmezse, ancak aşağıdaki adımları uygulayın:

1. Platforms klasörünün projenizden silin.
2. Eklenti klasörü projenizden silin.
3. Node_modules klasörü projenizden silin.
4. ID özniteliği pencere öğesi öğenin Apple Geliştirici hesabınızı oluşturduğunuz uygulama Kimliğini kullanmanız için Config.cml dosyasını güncelleştirin.
5. Projenizi yeniden derleyin.

##### <a name="test-push-notifications-in-your-ios-app"></a>Test iOS uygulamanıza anında iletme bildirimleri

1. Visual Studio'da emin **iOS** dağıtım hedefi olarak seçilir. Ardından **cihaz** bağlı iOS Cihazınızda anında iletme bildirimleri çalıştırılacak.

    Anında iletme bildirimleri, iOS cihazında iTunes ile bilgisayarınıza bağlı çalıştırabilirsiniz. İOS simülatörü anında iletme bildirimlerini desteklemiyor.

2. Seçin **çalıştırma** düğmesini veya **F5** Visual Studio projeyi oluşturun ve uygulamayı bir iOS cihazının başlatın. Ardından **Tamam** anında iletme bildirimleri kabul etmek için.

   > [!NOTE]
   > Uygulamayı ilk çalıştırma sırasında anında iletme bildirimleri için onay ister.

3. Uygulamasında, bir görev yazın ve ardından artı seçin **(+)** simgesi.
4. Bir bildirim alındı doğrulayın. Ardından **Tamam** bildirimi kapatmak için.

## <a name="optional-configure-and-run-on-windows"></a>(İsteğe bağlı) Yapılandırma ve Windows üzerinde çalıştırma

Bu bölümde, Apache Cordova uygulaması projesi (PhoneGap anında iletme eklentisi, Windows 10'da desteklenir), Windows 10 cihazlarda çalıştırmak açıklar. Windows cihazlarıyla çalışmıyorsanız, bu bölümü atlayabilirsiniz.

#### <a name="register-your-windows-app-for-push-notifications-with-wns"></a>WNS ile anında iletme bildirimleri için Windows uygulamanızı kaydetme

Visual Studio'da Store seçenekleri kullanmak için bir Windows hedef çözüm platformları gibi listesinden **Windows x64** veya **Windows x86**. (Önlemek **Windows AnyCPU** anında iletme bildirimleri için.)

[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

[Benzer adımları gösteren bir video izleyin][13]

#### <a name="configure-the-notification-hub-for-wns"></a>WNS için bildirim hub'ı yapılandırma

[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="configure-your-cordova-app-to-support-windows-push-notifications"></a>Windows anında iletme bildirimlerini desteklemek için Cordova uygulamanızı yapılandırın

Yapılandırma tasarımcısını açın, sağ tıklayarak **config.xml**. Ardından **Görünüm Tasarımcısı**. Ardından, **Windows** sekmesine tıklayın ve ardından **Windows 10** altında **Windows hedef sürümü**.

Varsayılan (hata ayıklama) yapılarınızı anında iletme bildirimlerini desteklemek için açın **Build.JSON dosyası** dosya. Ardından, hata ayıklama yapılandırması için "Sürüm" yapılandırmasını kopyalayın.

```json
"windows": {
    "release": {
        "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
        "publisherId": "CN=yourpublisherID"
    }
}
```

Güncelleştirmeden sonra **Build.JSON dosyası** dosya, aşağıdaki kodu içermelidir:

```json
"windows": {
    "release": {
        "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
        "publisherId": "CN=yourpublisherID"
        },
    "debug": {
        "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
        "publisherId": "CN=yourpublisherID"
        }
    }
```

Uygulama derleme ve hata olup olmadığını doğrulayın. İstemci uygulamanızın artık Mobile Apps arka ucu gelen bildirimlere kaydolması. Bu bölümde, çözümünüzdeki her Windows projesi için tekrarlayın.

#### <a name="test-push-notifications-in-your-windows-app"></a>Windows uygulamanızı test anında iletme bildirimleri

Visual Studio'da bir Windows platformu dağıtım hedefi gibi seçili olduğundan emin olun **Windows x64** veya **Windows x86**. Visual Studio barındırma bir Windows 10 bilgisayarı üzerinde uygulamayı çalıştırmak için tercih **yerel makine**.

1. Seçin **çalıştırma** düğmesi projeyi oluşturun ve uygulamayı başlatın.

2. Uygulamada, yeni bir todoıtem için'bir ad yazın ve ardından artı seçin **(+)** simgesi ekleyin.

Bir öğe eklendiğinde bir bildiriminin alındığını doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [Notification hubs'ı][17] anında iletme bildirimleri hakkında bilgi edinmek için.
* Zaten yapmadıysanız, öğreticiyle devam [kimlik doğrulaması ekleme][14] Apache Cordova uygulamanıza.

Aşağıdaki Sdk'lardan kullanmayı öğrenin:

* [Apache Cordova SDK][15]
* [ASP.NET Sunucusu SDK][1]
* [Node.js Sunucusu SDK][16]

<!-- Images -->
[img1]: ./media/app-service-mobile-cordova-get-started-push/add-push-plugin.png

<!-- URLs -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: https://www.visualstudio.com/
[3]: https://azure.microsoft.com/pricing/free-trial/
[4]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[5]: app-service-mobile-cordova-get-started.md
[6]: https://go.microsoft.com/fwlink/p/?LinkId=268302
[7]: https://developer.apple.com/programs/
[8]: https://developer.microsoft.com/en-us/store/register
[9]: https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-3-Create-azure-notification-hub
[10]: https://www.npmjs.com/
[11]: https://taco.visualstudio.com/en-us/docs/run-app-apache/#HAXM
[12]: https://taco.visualstudio.com/en-us/docs/ios-guide/
[13]: https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-6-Set-up-wns-for-push
[14]: app-service-mobile-cordova-get-started-users.md
[15]: app-service-mobile-cordova-how-to-use-client-library.md
[16]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[17]: ../notification-hubs/notification-hubs-push-notification-overview.md
[18]: https://console.developers.google.com/home/dashboard
[19]: https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/INSTALLATION.md
[20]: https://www.mobizen.com/
[21]: https://taco.visualstudio.com/en-us/docs/build_ios_cloud/
