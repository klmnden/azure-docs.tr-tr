---
title: "Apache Cordova uygulaması Azure Mobile Apps için anında iletme bildirimleri ekleme | Microsoft Docs"
description: "Apache Cordova uygulamanıza anında iletme bildirimleri göndermek için Azure Mobile Apps kullanmayı öğrenin."
services: app-service\mobile
documentationcenter: javascript
manager: crdun
editor: 
author: conceptdev
ms.assetid: 92c596a9-875c-4840-b0e1-69198817576f
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: crdun
ms.openlocfilehash: 05fa692f9331cf6b5178c3e9dca60ad2598dc609
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="add-push-notifications-to-your-apache-cordova-app"></a>Apache Cordova uygulamanıza anında iletme bildirimleri ekleme
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Genel Bakış
Böylece bir kayda eklenen her zaman bir anında iletme bildirimi cihaza gönderilir Bu öğreticide, anında iletme bildirimleri [Apache Cordova Hızlı Başlangıç] projeye ekleyin.

İndirilen hızlı başlangıç sunucu projesi kullanmazsanız, anında iletme bildirimi uzantısı paketi gerekir. Daha fazla bilgi için bkz: [.NET arka uç sunucusu SDK ile Azure Mobile Apps için iş][1].

## <a name="prerequisites"></a>Önkoşullar
Bu öğretici, Google Android öykünücüsü, bir Android cihazında, bir Windows aygıtı ve bir iOS aygıtı üzerinde çalışan Visual Studio 2015 ile geliştirilen bir Apache Cordova uygulaması kapsar.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Bir bilgisayarla [Visual Studio Community 2015] [ 2] veya sonraki sürümler.
* [Apache Cordova için Visual Studio Araçları][4].
* Bir [etkin Azure hesabı][3].
* Bir tamamlanmış [Apache Cordova Hızlı Başlangıç] [ 5] projesi.
* (Android) A [Google hesabı] [ 6] doğrulanmış e-posta adresine sahip.
* (iOS) Bir [Apple Developer Program üyeliği] [ 7] ve bir iOS cihazı (iOS simülatörü itme desteklemez).
* (Windows) A [Windows mağazası Geliştirici hesabınızla] [ 8] ve Windows 10 cihaz.

## <a name="configure-hub"></a>Bildirim hub'ı yapılandırma
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

[Bu bölümdeki adımları gösteren bir videoyu izleyin][9]

## <a name="update-the-server-project"></a>Güncelleştirme sunucusu projesi
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="add-push-to-app"></a>Cordova uygulamanızı değiştirme
Apache Cordova uygulaması projenize Cordova itme eklentisi artı platforma özgü anında hizmetlerin yükleyerek anında iletme bildirimleri işlemeye hazır olduğundan emin olun.

#### <a name="update-the-cordova-version-in-your-project"></a>Projenizdeki Cordova sürümünü güncelleştirin.
Projenizi Apache Cordova v6.1.1'den önceki bir sürümünü kullanıyorsa, istemci projesi güncelleştirin. Projeyi güncelleştirmek için:

* Sağ `config.xml` yapılandırma Tasarımcısı'nı açın.
* Platformlar sekmesini seçin.
* İçinde 6.1.1 seçin **Cordova CLI** metin kutusu.
* Seçin **yapı**, ardından **yapı çözümü** projeyi güncelleştirmek için.

#### <a name="install-the-push-plugin"></a>Anında iletme eklentisini yükleme
Apache Cordova uygulamaları aygıt ya da ağ yetenekleri yerel işleyemez.  Bu özellikler tarafından sağlanan olan eklentileri üzerinde ya da yayımlanan [npm] [ 10] veya github'da.  `phonegap-plugin-push` Eklentisi ağ anında iletme bildirimleri işlemek için kullanılır.

Anında iletme eklentisi şu yollardan biriyle yükleyebilirsiniz:

**Komut isteminden:**

Aşağıdaki komutu yürütün:

    cordova plugin add phonegap-plugin-push

**Gelen Visual Studio içinde:**

1. Çözüm Gezgini'nde açık `config.xml` dosyası **eklentileri** > **özel**seçin **Git** yükleme kaynağı olarak enter `https://github.com/phonegap/phonegap-plugin-push` kaynağı olarak.

   ![][img1]

2. Yükleme kaynağını yanındaki oka tıklayın.
3. İçinde **SENDER_ID**, Google Developer konsol projesi için bir sayısal proje kimliği zaten varsa, bunu burada ekleyebilirsiniz. Aksi takdirde, 777777 gibi bir yer tutucu değerini girin.  Android hedefliyorsanız, bu değeri daha sonra config.xml güncelleştirebilirsiniz.
     SENDER_ID kaldırıldığı sürüm 2.0.0 itibariyle zaman ve google services.json yükleme unutmayın, projenin kök klasöründe yüklü olması gerekir.  Daha fazla ayrıntı [burada.](https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/INSTALLATION.md)
4. **Ekle**'ye tıklayın.

Anında iletme eklentisi artık yüklüdür.

#### <a name="install-the-device-plugin"></a>Cihaz eklentisini yükleme
Anında iletme eklentisini yüklemek için kullandığınız yordamın aynısını izleyin.  Çekirdek eklentiler listesinden aygıt eklenti ekleme (tıklatın **eklentileri** > **çekirdek** bulmak için). Platform adı almak için bu eklentinin gerekir.

#### <a name="register-your-device-on-application-start-up"></a>Uygulama başlatma aygıtınızda kaydetme
Başlangıçta, biz bazı çok az kod Android için içerir. Daha sonra iOS veya Windows 10 çalıştırmak için uygulamayı değiştirin.

1. Bir çağrı ekleyin **registerForPushNotifications** oturum açma işlemi için ya da alt kısmındaki geri çağırma sırasında **onDeviceReady** yöntemi:

        // Login to the service.
        client.login('google')
            .then(function () {
                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                    // Added to register for push notifications.
                registerForPushNotifications();

            }, handleError);

    Bu örnek, arama gösterir **registerForPushNotifications** kimlik doğrulaması başarılı olduktan sonra.  Çağırabilirsiniz `registerForPushNotifications()` gereklidir sıklıkta.

2. Yeni Ekle **registerForPushNotifications** yöntemini aşağıdaki şekilde:

        // Register for Push Notifications. Requires that phonegap-plugin-push be installed.
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
3. (Android) Önceki kodla `Your_Project_ID` uygulamanızdan için kimliği ile sayısal proje [Google Developer konsolunda][18].

## <a name="optional-configure-and-run-the-app-on-android"></a>(İsteğe bağlı) Yapılandırma ve uygulamayı Android çalıştırma
Android için anında iletme bildirimleri etkinleştirmek için bu bölümü tamamlayın.

#### <a name="enable-gcm"></a>Firebase etkinleştirmek bulut Mesajlaşma
Google Android platform başlangıçta desteği bu yana Firebase Cloud Messaging etkinleştirmeniz gerekir.

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

#### <a name="configure-backend"></a>Mobil uygulama arka ucu FCM kullanarak anında iletme istekleri göndermek için yapılandırma
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

#### <a name="configure-your-cordova-app-for-android"></a>Android için Cordova uygulamanızı yapılandırma
Cordova uygulamanızı config.xml açın ve değiştirmek `Your_Project_ID` uygulamanızdan için kimliği ile sayısal proje [Google Developer konsolunda][18].

        <plugin name="phonegap-plugin-push" version="1.7.1" src="https://github.com/phonegap/phonegap-plugin-push.git">
            <variable name="SENDER_ID" value="Your_Project_ID" />
        </plugin>

İndex.js açın ve sayısal proje kimliğinizi kullanmak için kodu güncelleştirme

        pushRegistration = PushNotification.init({
            android: { senderID: 'Your_Project_ID' },
            ios: { alert: 'true', badge: 'true', sound: 'true' },
            wns: {}
        });

#### <a name="configure-device"></a>USB hata ayıklama için Android Cihazınızı yapılandırın
Android Cihazınızı uygulamanıza dağıtmadan önce USB hata ayıklamayı etkinleştirmek gerekir.  Android telefonunuzda aşağıdaki adımları gerçekleştirin:

1. Git **ayarları** > **telefon hakkında**, ardından dokunun **yapı numarası** geliştirici modunu (yaklaşık yedi defa) etkinleştirilene kadar.
2. Geri **ayarları** > **Geliştirici seçenekleri** etkinleştirmek **USB hata ayıklama**, USB Kablosuyla geliştirme Android telefonunuz bağlanmak.

Bu Android 6.0 (Marshmallow) çalıştıran bir Google Nexus 5 X aygıtı kullanarak test.  Ancak, tüm modern Android sürüm arasında ortak tekniklerdir.

#### <a name="install-google-play-services"></a>Google Play hizmetlerini yükleyin
Anında iletme eklentisi Android Google Play Hizmetleri'nin anında iletme bildirimleri için kullanır.

1. Visual Studio'da sırasıyla **Araçları** > **Android** > **Android SDK Manager**, genişletin **ek özellikler** klasörü ve onay kutusu her aşağıdaki SDK yüklü olduğundan emin olun.

   * Android 2.3 veya daha yüksek
   * Google depo düzeltme 27 veya daha yüksek
   * Google Play hizmetlerini 9.0.2 veya üzeri

2. Tıklatın **yükleme paketleri** ve yüklemenin tamamlanması için bekleyin.

Geçerli gerekli kitaplıklar listelenen [phonegap eklentiyi itme yükleme belgelerini][19].

#### <a name="test-push-notifications-in-the-app-on-android"></a>Android uygulamasına anında iletme bildirimlerini test
Uygulamayı çalıştırarak ve öğeleri Todoıtem tabloda ekleme şimdi test anında iletme bildirimleri olabilir. Aynı arka uç kullandığınız sürece, aynı aygıttan veya ikinci bir cihaz test edebilirsiniz. Android platformunda Cordova uygulamanızı aşağıdaki yollardan biriyle test edin:

* **Bir fiziksel cihaz üzerindeki:** geliştirme bilgisayarınıza bir USB kablosu ile Android Cihazınızı ekleyin.  Yerine **Google Android öykünücüsü**seçin **aygıt**. Visual Studio aygıta uygulama dağıtır ve uygulamayı çalıştırır.  Cihazda uygulama ile etkileşim kurabilirsiniz.

  Geliştirme deneyiminizi iyileştirebilir.  Ekran uygulamaları gibi paylaşımı [Mobizen] [ 20] Android uygulama geliştirmede yardımcı olabilir.  Mobizen bilgisayarınıza bir web tarayıcısı Android ekrana projeleri.

* **Android öykünücüsünde üzerinde:** bir öykünücü üzerinde çalışırken gereken ek yapılandırma adımları vardır.

    Google API'leri hedef olarak ayarlanmış Android sanal cihazı (AVD) Yöneticisi'nde gösterildiği gibi sanal cihaza dağıttığınız emin olun.

    ![](./media/app-service-mobile-cordova-get-started-push/google-apis-avd-settings.png)

    Daha hızlı x86 kullanmak istiyorsanız öykünücüsü, [HAXM sürücüyü yüklemek] [ 11] ve kullanmak için öykünücü yapılandırın.

    Tıklayarak Android aygıtına bir Google hesabı ekleyin **uygulamaları** > **ayarları** > **hesabı eklemek**, ardından yönergeleri izleyin.

    ![](./media/app-service-mobile-cordova-get-started-push/add-google-account.png)

    Önce Yapılacaklar listesi uygulaması gibi çalıştırabilir ve yeni bir Yapılacaklar öğesi ekleyin. Bu süre, bildirim alanında bir bildirim simgesi görüntülenir. Tam metin bildirimi görüntülemek için bildirim çekmecesini açabilirsiniz.

    ![](./media/app-service-mobile-cordova-get-started-push/android-notifications.png)

## <a name="optional-configure-and-run-on-ios"></a>(İsteğe bağlı) Yapılandırma ve İos'ta çalıştırma
Bu bölümde, iOS cihazlarda Cordova projesi çalıştırmaya yöneliktir. İOS cihazlarıyla çalışmıyorsanız, bu bölümü atlayabilirsiniz.

#### <a name="install-and-run-the-ios-remote-build-agent-on-a-mac-or-cloud-service"></a>Yükleme ve bir Mac veya Bulut hizmetinde iOS uzak derleme Aracısı'nı çalıştırın
Visual Studio kullanarak iOS Cordova uygulaması çalıştırmadan önce içinde adımlarını [iOS kurulum kılavuzunu] [ 12] yükleyip uzak derleme Aracısı'nı çalıştırın.

İOS için uygulama oluşturduğunuzdan emin olun. Kurulum Kılavuzu'ndaki adımları, iOS için Visual Studio'dan oluşturmak için gereklidir. Mac yoksa uzak derleme aracısı MacInCloud gibi bir hizmet kullanarak iOS için oluşturabilirsiniz. Daha fazla bilgi için bkz: [iOS uygulamanızı bulutta çalışan][21].

> [!NOTE]
> XCode 7 veya daha büyük İos'ta anında iletme eklentisi kullanmak için gereklidir.

#### <a name="find-the-id-to-use-as-your-app-id"></a>Uygulama Kimliğiniz kullanmak üzere bir kimlik bulunamıyor
Anında iletme bildirimleri, Cordova uygulamanızı açık Config.XML'de için uygulamanızı kaydetme önce Bul `id` öznitelik değeri pencere öğesinde ve daha sonra kullanmak üzere kopyalayın. Aşağıdaki XML dosyasında kimliğidir `io.cordova.myapp7777777`.

        <widget defaultlocale="en-US" id="io.cordova.myapp7777777"
          version="1.0.0" windows-packageVersion="1.1.0.0" xmlns="http://www.w3.org/ns/widgets"
            xmlns:cdv="http://cordova.apache.org/ns/1.0" xmlns:vs="http://schemas.microsoft.com/appx/2014/htmlapps">

Apple Geliştirici Portalı üzerinde bir uygulama kimliği oluşturduğunuzda, daha sonra bu tanımlayıcı kullanın. Farklı bir uygulama kimliği Geliştirici portalında oluşturursanız, daha sonra Bu öğreticide birkaç ek adımlar atmanız gerekir. Pencere öğesi kimliği Geliştirici Portalı Uygulama Kimliğinin eşleşmesi gerekir.

#### <a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>Apple Geliştirici portalındaki anında iletme bildirimleri için uygulamanızı kaydetmeniz
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

[Benzer adımları gösteren bir video izleyin](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-5-Set-up-apns-for-push)

#### <a name="configure-azure-to-send-push-notifications"></a>Anında iletme bildirimleri göndermek için Azure yapılandırma
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

#### <a name="verify-that-your-app-id-matches-your-cordova-app"></a>Uygulama Kimliğiniz Cordova uygulamanızı eşleştiğini doğrulayın
Apple Geliştirici hesabınızda zaten oluşturulmuş uygulama kimliği config.xml pencere öğesinde Kimliğini eşleşiyorsa, bu adımı atlayabilirsiniz. Kimlikler eşleşmezse, ancak aşağıdaki adımları uygulayın:

1. Platformlar klasörü projenizden silin.
2. Eklenti klasörü projenizden silin.
3. Node_modules klasörü projenizden silin.
4. Apple Geliştirici hesabınızda oluşturduğunuz uygulama Kimliğini kullanan config.xml pencere öğesinde ID özniteliği güncelleştirin.
5. Projenizi yeniden derleyin.

##### <a name="test-push-notifications-in-your-ios-app"></a>İOS uygulamanızı test anında iletme bildirimleri
1. Visual Studio'da olduğundan emin olun **iOS** dağıtım hedefi olarak seçilir ve ardından **aygıt** bağlı iOS Cihazınızda çalıştırmak için.

    Bilgisayarınıza bağlı bir iOS cihazında, iTunes kullanarak çalıştırabilirsiniz. İOS simülatörü'nü anında iletme bildirimlerini desteklemiyor.

2. Tuşuna **çalıştırmak** düğmesini veya **F5** projeyi oluşturun ve uygulamayı bir iOS aygıtı başlatmak için Visual Studio'yu içinde ardından **Tamam** anında iletme bildirimleri kabul etmek için.

   > [!NOTE]
   > Uygulamayı anında iletme bildirimleri için ilk çalıştırma sırasında onay ister.

3. Uygulamasında bir görevi yazın ve ardından artı (+) simgesi.
4. Doğrulayın. bir bildirim alındıktan sonra bildirim kapatmak için Tamam'ı tıklatın.

## <a name="optional-configure-and-run-on-windows"></a>(İsteğe bağlı) Yapılandırma ve Windows'ta çalıştırma
Bu bölüm, Windows 10 cihazlarda (PhoneGap itme eklentisi üzerinde Windows 10 desteklenir) Apache Cordova uygulaması projesi çalıştırmaya yöneliktir. Windows cihazlarıyla çalışmıyorsanız, bu bölümü atlayabilirsiniz.

#### <a name="register-your-windows-app-for-push-notifications-with-wns"></a>Windows uygulamanızı anında iletme bildirimleri için WNS ile kaydetme
Visual Studio'da depolama seçenekleri kullanmak için bir Windows hedef çözüm platformları gibi listesinden **Windows x64** veya **Windows x86** (kaçının **Windows AnyCPU** anında iletme bildirimleri için).

[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

[Benzer adımları gösteren bir videoyu izleyin][13]

#### <a name="configure-the-notification-hub-for-wns"></a>WNS için bildirim hub'ı yapılandırma
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="configure-your-cordova-app-to-support-windows-push-notifications"></a>Windows anında iletme bildirimlerini desteklemek için Cordova uygulamanızı yapılandırma
Yapılandırma Tasarımcısı'nı açın (sağ tıklatın ve config.xml **Görünüm Tasarımcısı**), select **Windows** sekmesini tıklatın ve seçin **Windows 10** altında **Windows hedef sürüm**.

Anında iletme desteklemek için varsayılan (hata ayıklama) bildirimleri oluşturur, açık build.json dosya. "Yayın" yapılandırmasını hata ayıklama yapılandırmanızı kopyalayın.

        "windows": {
            "release": {
                "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
                "publisherId": "CN=yourpublisherID"
            }
        }

Güncelleştirme tamamlandıktan sonra aşağıdaki kodu build.json içermelidir:

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

Uygulamayı derleyin ve hata sahip olduğunuzu doğrulayın. İstemci uygulamanızı şimdi mobil uygulama arka ucu bildirimler için kaydetmeniz. Bu bölümde, çözümünüz içinde her Windows projesi için tekrarlayın.

#### <a name="test-push-notifications-in-your-windows-app"></a>Windows uygulamanızı test anında iletme bildirimleri
Visual Studio'da Windows platformu aşağıdaki gibi dağıtım hedefteki seçildiğinden emin olun **Windows x64** veya **Windows x86**. Windows 10 Visual Studio barındıran bir Bilgisayara uygulamayı çalıştırmak için tercih **yerel makine**.

Projeyi oluşturmak ve uygulamayı başlatmak için Çalıştır düğmesine basın.

Uygulamasındaki yeni todoıtem için bir ad yazın ve ardından artı (+) eklemek için simge.

Öğe eklendiğinde, bir bildirim alındığında doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [bildirim hub'ları] [ 17] anında iletme bildirimleri hakkında bilgi edinmek için.
* Zaten yapmadıysanız, Öğreticisi tarafından devam [Authentication ekleme] [ 14] Apache Cordova uygulamanıza.

SDK'ları kullanmayı öğrenin.

* [Apache Cordova SDK][15]
* [ASP.NET sunucusu SDK][1]
* [Node.js sunucusu SDK][16]

<!-- Images -->
[img1]: ./media/app-service-mobile-cordova-get-started-push/add-push-plugin.png

<!-- URLs -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: http://www.visualstudio.com/
[3]: https://azure.microsoft.com/pricing/free-trial/
[4]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[5]: app-service-mobile-cordova-get-started.md
[6]: http://go.microsoft.com/fwlink/p/?LinkId=268302
[7]: https://developer.apple.com/programs/
[8]: https://developer.microsoft.com/en-us/store/register
[9]: https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-3-Create-azure-notification-hub
[10]: https://www.npmjs.com/
[11]: https://taco.visualstudio.com/en-us/docs/run-app-apache/#HAXM
[12]: http://taco.visualstudio.com/en-us/docs/ios-guide/
[13]: https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-6-Set-up-wns-for-push
[14]: app-service-mobile-cordova-get-started-users.md
[15]: app-service-mobile-cordova-how-to-use-client-library.md
[16]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[17]: ../notification-hubs/notification-hubs-push-notification-overview.md
[18]: https://console.developers.google.com/home/dashboard
[19]: https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/INSTALLATION.md
[20]: https://www.mobizen.com/
[21]: http://taco.visualstudio.com/en-us/docs/build_ios_cloud/
