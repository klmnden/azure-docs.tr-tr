---
title: Azure App Service Mobile Apps özelliğini bir Apache Cordova uygulaması için anında iletme bildirimleri ekleme | Microsoft Docs
description: Mobile Apps Apache Cordova uygulamanıza anında iletme bildirimleri göndermek için nasıl kullanılacağını öğrenin.
services: app-service\mobile
documentationcenter: javascript
manager: crdun
editor: ''
author: conceptdev
ms.assetid: 92c596a9-875c-4840-b0e1-69198817576f
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: crdun
ms.openlocfilehash: 13c1a53cfa3f998c9e3fa3ee1ee2dcec37357095
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34598003"
---
# <a name="add-push-notifications-to-your-apache-cordova-app"></a>Apache Cordova uygulamanıza anında iletme bildirimleri ekleme
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Genel Bakış
Bu öğreticide, anında iletme bildirimleri ekleme [Apache Cordova quickstart] [ 5] proje böylece bir kayda eklenen her zaman bir anında iletme bildirimi cihaza gönderilir.

İndirilen hızlı başlangıç sunucu projesi kullanmazsanız, anında iletme bildirimi uzantısı paketi gerekir. Daha fazla bilgi için bkz: [mobil uygulamalar için .NET arka uç sunucusu SDK çalışma][1].

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticide, Visual Studio 2015 ile geliştirilmiş bir Apache Cordova uygulaması olduğunu varsayar. Bu cihaz, Google Android öykünücüsü, bir Android cihazında, bir Windows aygıtı veya bir iOS aygıtı çalıştırmanız gerekir.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Bir bilgisayarla [Visual Studio Community 2015] [ 2] veya daha yenisi
* [Apache Cordova için Visual Studio Araçları][4]
* Bir [etkin Azure hesabı][3]
* Bir tamamlanmış [Apache Cordova quickstart] [ 5] proje
* (Android) A [Google hesabı] [ 6] doğrulanmış e-posta adresi
* (iOS) Bir [Apple Developer Program üyeliği] [ 7] ve bir iOS cihazı (Simulator desteklemiyor iOS anında iletme bildirimleri)
* (Windows) A [Microsoft mağazası Geliştirici hesabınızla] [ 8] ve Windows 10 cihaz

## <a name="configure-hub"></a>Bildirim hub'ı yapılandırma
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

[Bu bölümdeki adımları gösteren bir video izlemek][9].

## <a name="update-the-server-project"></a>Güncelleştirme sunucusu projesi
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="add-push-to-app"></a>Cordova uygulamanızı değiştirme
Apache Cordova uygulaması projenize anında iletme bildirimleri işlemeye hazır olduğundan emin olmak için Cordova itme eklentisi artı platforma özgü anında hizmetlerin yükleyin.

#### <a name="update-the-cordova-version-in-your-project"></a>Projenizdeki Cordova sürümünü güncelleştirin.
Projenizi 6.1.1 sürümden daha eski bir Apache Cordova sürümü kullanılıyorsa, istemci projesi güncelleştirin. Projeyi güncelleştirmek için aşağıdaki adımları uygulayın: 

* Yapılandırma Tasarımcısı'nı açmak için sağ `config.xml`.
* Seçin **platformları** sekmesi.
* İçinde **Cordova CLI** metin kutusunda **6.1.1**. 
* Projeyi güncelleştirmek için seçin **yapı**ve ardından **yapı çözümü**.

#### <a name="install-the-push-plugin"></a>Anında iletme eklentisini yükleme
Apache Cordova uygulamaları aygıt ya da ağ yetenekleri yerel işleyemez.  Bu özellikler tarafından sağlanan olan eklentileri üzerinde ya da yayımlanan [npm] [ 10] veya github'da. `phonegap-plugin-push` Eklentisi ağ anında iletme bildirimleri işler.

Anında iletme eklentisi aşağıdaki yöntemlerle yükleyebilirsiniz:

**Komut isteminden:**

Şu komutu çalıştırın:

    cordova plugin add phonegap-plugin-push

**Gelen Visual Studio içinde:**

1. Çözüm Gezgini'nde Aç `config.xml` dosya. Ardından, **eklentileri** > **özel**. Ardından **Git** yükleme kaynağı olarak. 
    
2. Girin `https://github.com/phonegap/phonegap-plugin-push` kaynağı olarak.

    ![Çözüm Gezgini'nde config.xml dosyasını açın][img1]

3. Yükleme kaynağını yanındaki oka seçin.

4. İçinde **SENDER_ID**, Google Developer konsol projesi için bir sayısal proje kimliği zaten varsa, bunu burada ekleyebilirsiniz. Aksi takdirde, 777777 gibi bir yer tutucu değerini girin. Android hedefliyorsanız, bu değer dosyasında daha sonra güncelleştirebilirsiniz.

    >[!NOTE]
    >Sürüm 2.0.0 itibariyle google services.json Gönderen Kimliği yapılandırmak için projenin kök klasöründe yüklü olması gerekir Daha fazla bilgi için bkz: [yükleme belgeleri.](https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/INSTALLATION.md)
5. **Add (Ekle)** seçeneğini belirleyin.

Anında iletme eklentisi artık yüklüdür.

#### <a name="install-the-device-plugin"></a>Cihaz eklentisini yükleme
Anında iletme eklentisini yüklemek için kullandığınız yordamın aynısını izleyin. Cihaz eklentisi çekirdek eklentiler listesinden ekleyin. (Bunu bulmak için seçin **eklentileri** > **çekirdek**.) Platform adı almak için bu eklentinin gerekir.

#### <a name="register-your-device-when-the-application-starts"></a>Uygulama başlatıldığında Cihazınızı kaydedin 
Başlangıçta, biz bazı çok az kod Android için içerir. Daha sonra iOS veya Windows 10 çalıştırmak için uygulamayı değiştirebilirsiniz.

1. Bir çağrı ekleyin **registerForPushNotifications** oturum açma işlemi için geri çağırma sırasında. Alternatif olarak, alt kısmındaki ekleyebilirsiniz **onDeviceReady** yöntemi:

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

    Bu örnek, arama gösterir **registerForPushNotifications** kimlik doğrulaması başarılı olduktan sonra. Çağırabilirsiniz `registerForPushNotifications()` gereklidir sıklıkta.

2. Yeni Ekle **registerForPushNotifications** yöntemini aşağıdaki şekilde:

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
3. (Android) Önceki kodla `Your_Project_ID` uygulamanızdan için kimliği ile sayısal proje [Google Developer konsolunda][18].

## <a name="optional-configure-and-run-the-app-on-android"></a>(İsteğe bağlı) Yapılandırma ve uygulamayı Android çalıştırma
Android için anında iletme bildirimleri etkinleştirmek için bu bölümü tamamlayın.

#### <a name="enable-gcm"></a>Firebase etkinleştirmek bulut Mesajlaşma
Google Android platformu başlangıçta hedeflediğiniz beri Firebase Cloud Messaging etkinleştirmeniz gerekir.

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

#### <a name="configure-backend"></a>Mobil uygulama arka ucu FCM kullanarak anında iletme istekleri göndermek için yapılandırma
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

#### <a name="configure-your-cordova-app-for-android"></a>Android için Cordova uygulamanızı yapılandırma
Cordova uygulamanızı config.xml açın. Yerine `Your_Project_ID` uygulamanızdan için kimliği ile sayısal proje [Google Developer konsolunda][18].

        <plugin name="phonegap-plugin-push" version="1.7.1" src="https://github.com/phonegap/phonegap-plugin-push.git">
            <variable name="SENDER_ID" value="Your_Project_ID" />
        </plugin>

İndex.js açın. Sayısal proje kimliğinizi kullanmak için kodu güncelleştirme

        pushRegistration = PushNotification.init({
            android: { senderID: 'Your_Project_ID' },
            ios: { alert: 'true', badge: 'true', sound: 'true' },
            wns: {}
        });

#### <a name="configure-device"></a>USB hata ayıklama için Android Cihazınızı yapılandırın
Android Cihazınızı uygulamanıza dağıtmadan önce USB hata ayıklamayı etkinleştirmek gerekir. Android telefonunuzda aşağıdaki adımları uygulayın:

1. Git **ayarları** > **telefon hakkında**. ' A dokunun **yapı numarası** geliştirici modunu (yaklaşık yedi defa) etkinleştirilene kadar.
2. Geri **ayarları** > **Geliştirici seçenekleri**, etkinleştirme **USB hata ayıklama**. Ardından Android telefonunuz bir USB kablosu ile geliştirme bilgisayarınıza bağlayın.

Bu Android 6.0 (Marshmallow) çalıştıran bir Google Nexus 5 X aygıtı kullanarak test. Ancak, tüm modern Android sürüm arasında ortak tekniklerdir.

#### <a name="install-google-play-services"></a>Google Play hizmetlerini yükleyin
Anında iletme eklentisi Android Google Play Hizmetleri'nin anında iletme bildirimleri için kullanır.

1. Visual Studio'da seçin **Araçları** > **Android** > **Android SDK Manager**. Ardından **ek özellikler** klasör. Her biri aşağıdaki SDK'ları yüklendiğinden emin olmak için uygun onay kutularını işaretleyin:

   * Android 2.3 veya daha yüksek
   * Google depo düzeltme 27 veya daha yüksek
   * Google Play hizmetlerini 9.0.2 veya üzeri

2. Seçin **yükleme paketleri**. Daha sonra yüklemesinin tamamlanması için bekleyin.

Geçerli gerekli kitaplıklar listelenen [phonegap eklentiyi itme yükleme belgelerini][19].

#### <a name="test-push-notifications-in-the-app-on-android"></a>Android uygulamasına anında iletme bildirimlerini test
Uygulamayı çalıştırarak ve öğeleri Todoıtem tabloda ekleme şimdi test anında iletme bildirimleri olabilir. Aynı arka uç kullandığınız sürece, aynı aygıttan veya ikinci bir cihaz test edebilirsiniz. Android platformunda Cordova uygulamanızı aşağıdaki yollardan biriyle test edin:

* *Bir fiziksel cihaz üzerindeki:* geliştirme bilgisayarınıza bir USB kablosu ile Android Cihazınızı ekleyin.  Yerine **Google Android öykünücüsü**seçin **aygıt**. Visual Studio aygıta uygulama dağıtır ve uygulamayı çalıştırır. Cihazda uygulama ile etkileşim kurabilirsiniz.

  Ekran paylaşımı uygulamalar gibi [Mobizen] [ 20] Android uygulamaları geliştirmeye yardımcı olabilir. Mobizen bilgisayarınıza bir web tarayıcısı Android ekrana projeleri.

* *Android öykünücüsünde üzerinde:* bir öykünücü kullanırken gerekli olan ek yapılandırma adımları vardır.

    Google API'leri hedef olarak ayarlanmış Android sanal cihazı (AVD) Yöneticisi'nde gösterildiği gibi sanal cihaza dağıttığınız emin olun.

    ![Android sanal cihaz Yöneticisi](./media/app-service-mobile-cordova-get-started-push/google-apis-avd-settings.png)

    Daha hızlı x86 kullanmak istiyorsanız öykünücüsü, [HAXM sürücüyü yüklemek][11]ve ardından öykünücü kullanmak için yapılandırın.

    Bir Google hesabı Android aygıtına seçerek eklemek **uygulamaları** > **ayarları** > **hesabı eklemek**. Ardından yönergeleri izleyin.

    ![Android cihazına bir Google hesabı Ekle](./media/app-service-mobile-cordova-get-started-push/add-google-account.png)

    Önce Yapılacaklar listesi uygulaması gibi çalıştırabilir ve yeni bir Yapılacaklar öğesi ekleyin. Bu süre, bildirim alanında bir bildirim simgesi görüntülenir. Tam metin bildirimi görüntülemek için bildirim çekmecesini açabilirsiniz.

    ![Görünüm bildirim](./media/app-service-mobile-cordova-get-started-push/android-notifications.png)

## <a name="optional-configure-and-run-on-ios"></a>(İsteğe bağlı) Yapılandırma ve İos'ta çalıştırma
Bu bölümde, iOS cihazlarda Cordova projesi çalıştırmaya yöneliktir. İOS aygıtları ile çalışıyorsanız değil, bu bölümü atlayabilirsiniz.

#### <a name="install-and-run-the-ios-remote-build-agent-on-a-mac-or-cloud-service"></a>Yükleme ve bir Mac veya Bulut hizmetinde iOS uzak derleme Aracısı'nı çalıştırın
Visual Studio kullanarak iOS Cordova uygulaması çalıştırmadan önce içinde adımlarını [iOS Kurulum Kılavuzu] [ 12] yükleyip uzak derleme Aracısı'nı çalıştırın.

İOS için uygulama oluşturduğunuzdan emin olun. Kurulum Kılavuzu'ndaki adımları, Visual Studio'dan iOS için uygulama oluşturmak için gereklidir. Mac yoksa, iOS için Uzak derleme aracısı MacInCloud gibi bir hizmet kullanarak oluşturabilirsiniz. Daha fazla bilgi için bkz: [iOS uygulamanızı bulutta çalışan][21].

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
Apple Geliştirici hesabınızda zaten oluşturulmuş uygulama kimliği dosyasında pencere öğesinin kimliği eşleşiyorsa, bu adımı atlayabilirsiniz. Kimlikler eşleşmezse, ancak aşağıdaki adımları uygulayın:

1. Platformlar klasörü projenizden silin.
2. Eklenti klasörü projenizden silin.
3. Node_modules klasörü projenizden silin.
4. Dosyasında Apple Geliştirici hesabınızda oluşturduğunuz uygulama Kimliğini kullanmak için pencere öğesinin kimliği özniteliği güncelleştirin.
5. Projenizi yeniden derleyin.

##### <a name="test-push-notifications-in-your-ios-app"></a>İOS uygulamanızı test anında iletme bildirimleri
1. Visual Studio'da olduğundan emin olun **iOS** dağıtım hedefi olarak seçilir. Ardından **aygıt** anında iletme bildirimleri, bağlı iOS Cihazınızda çalıştırmak için.

    İTunes PC'NİZLE bağlı bir iOS cihazına anında iletme bildirimleri çalıştırabilirsiniz. İOS simülatörü'nü anında iletme bildirimlerini desteklemiyor.

2. Seçin **çalıştırmak** düğmesini veya **F5** projeyi oluşturun ve uygulamayı bir iOS aygıtı başlatmak için Visual Studio'da. Ardından **Tamam** anında iletme bildirimleri kabul etmek için.

   > [!NOTE]
   > Uygulamayı anında iletme bildirimleri için ilk çalıştırma sırasında onay ister.

3. Uygulamasında bir görev yazın ve ardından artı seçin **(+)** simgesi.
4. Bir bildirim alındı doğrulayın. Ardından **Tamam** bildirim kapatılamadı.

## <a name="optional-configure-and-run-on-windows"></a>(İsteğe bağlı) Yapılandırma ve Windows'ta çalıştırma
Bu bölümde, Apache Cordova uygulaması projesi (PhoneGap itme eklentisi üzerinde Windows 10 desteklenir) Windows 10 cihazlarda çalışan açıklar. Windows cihazlarıyla çalışmıyorsanız, bu bölümü atlayabilirsiniz.

#### <a name="register-your-windows-app-for-push-notifications-with-wns"></a>Windows uygulamanızı anında iletme bildirimleri için WNS ile kaydetme
Visual Studio'da depolama seçenekleri kullanmak için bir Windows hedef çözüm platformları gibi listesinden **Windows x64** veya **Windows x86**. (Kaçının **Windows AnyCPU** anında iletme bildirimleri için.)

[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

[Benzer adımları gösteren bir videoyu izleyin][13]

#### <a name="configure-the-notification-hub-for-wns"></a>WNS için bildirim hub'ı yapılandırma
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="configure-your-cordova-app-to-support-windows-push-notifications"></a>Windows anında iletme bildirimlerini desteklemek için Cordova uygulamanızı yapılandırma
Yapılandırma Tasarımcısı'nı sağ tıklayarak açın **config.xml**. Ardından **Görünüm Tasarımcısı**. Ardından, **Windows** sekmesini tıklatın ve ardından **Windows 10** altında **Windows hedef sürüm**.

Varsayılan (hata ayıklama) derlemeleriniz anında iletme bildirimlerini desteklemek için build.json dosyasını açın. Ardından, hata ayıklama yapılandırmasını "yayın" yapılandırma kopyalayın.

        "windows": {
            "release": {
                "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
                "publisherId": "CN=yourpublisherID"
            }
        }

Güncelleştirme tamamlandıktan sonra aşağıdaki kodu build.json dosyası içermelidir:

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

Uygulamayı derleyin ve hata sahip olduğunuzu doğrulayın. İstemci uygulamanızın artık Mobile Apps arka ucundan bildirim kaydetmelisiniz. Bu bölümde, çözümünüz içinde her Windows projesi için tekrarlayın.

#### <a name="test-push-notifications-in-your-windows-app"></a>Windows uygulamanızı test anında iletme bildirimleri
Visual Studio'da Windows platformu aşağıdaki gibi dağıtım hedefteki seçildiğinden emin olun **Windows x64** veya **Windows x86**. Windows 10 Visual Studio barındıran Bilgisayara uygulamayı çalıştırmak için tercih **yerel makine**.

1. Seçin **çalıştırmak** düğmesine projeyi oluşturun ve uygulamayı başlatın.

2. Uygulamasındaki yeni todoıtem için bir ad yazın ve ardından artı seçin **(+)** eklemek için simge.

Öğe eklendiğinde, bir bildirim alındığında doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [bildirim hub'ları] [ 17] anında iletme bildirimleri hakkında bilgi edinmek için.
* Zaten yapmadıysanız, Öğreticisi tarafından devam [kimlik doğrulaması ekleme] [ 14] Apache Cordova uygulamanıza.

Aşağıdaki SDK'ları kullanmayı öğrenin:

* [Apache Cordova SDK'sı][15]
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
