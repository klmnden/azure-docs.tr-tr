---
title: Anında iletme bildirimleri için Azure Notification hubs'ı kullanan Swift iOS uygulamaları | Microsoft Docs
description: Anında iletme bildirimleri için Azure Notification hubs'ı kullanan Swift iOS uygulamaları hakkında bilgi edinin.
services: notification-hubs
documentationcenter: ios
author: mikeparker104
manager: patniko
editor: spelluru
ms.assetid: 4e3772cf-20db-4b9f-bb74-886adfaaa65d
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 05/21/2019
ms.author: miparker
ms.openlocfilehash: a4773ddd8114659118e89cfee57e73ddb39ff6b6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67116647"
---
# <a name="tutorial-push-notifications-to-swift-ios-apps-that-use-the-notification-hubs-rest-api"></a>Öğretici: Notification Hubs REST API'si kullanan Swift iOS uygulamaları için anında iletme bildirimleri

> [!div class="op_single_selector"]
> * [Objective-C](notification-hubs-ios-apple-push-notification-apns-get-started.md)
> * [Swift](notification-hubs-ios-push-notifications-swift-apps-get-started.md)

Bu öğreticide, Azure Notification hubs'ı bir Swift tabanlı iOS uygulamanıza anında iletme bildirimleri kullanarak [REST API](/rest/api/notificationhubs/). Ayrıca kullanarak anında iletme bildirimleri alan boş bir iOS uygulaması oluşturma [Apple anında iletilen bildirim servisi (APNs)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1).

Bu öğreticide aşağıdaki adımları gösterilir:

> [!div class="checklist"]
> * Sertifika imzalama istek dosyası oluşturur.
> * Anında iletme bildirimleri için uygulamanızı isteyin.
> * Uygulama için bir sağlama profili oluşturun.
> * Bildirim hub’ı oluşturma.
> * Bildirim hub'ı APNs bilgilerle yapılandırın.
> * İOS uygulamanızı bildirim hub'ına bağlama.
> * Çözüm test edin.

## <a name="prerequisites"></a>Önkoşullar

Örneği takip etmek için ihtiyacınız vardır:

- Üzerinden geçmek üzere [Azure Notification Hubs'a genel bakış](notification-hubs-push-notification-overview.md) hizmetiyle ilgili bilgi sahibi değilseniz.
- Hakkında bilgi edinmek için [kayıtları ve yükleme](notification-hubs-push-notification-registration-management.md).
- Etkin bir [Apple Geliştirici hesabı](https://developer.apple.com).
- Xcode Anahtarlığınıza yüklü bir geçerli bir geliştirici sertifikası ile birlikte çalıştıran bir Mac.
- Bir fiziksel iPhone cihaz çalıştırın ve simülatör ile anında iletme bildirimleri test edilemiyor, hata ayıklama.
- Kayıtlı fiziksel iPhone Cihazınızı [Apple portalı](https://developer.apple.com) ve sertifikanızla ilişkili.
- Bir [Azure aboneliği](https://portal.azure.com) oluşturduğunuz ve kaynaklarını yönetme.

Önceden deneyiminiz olmasa iOS geliştirme olsa bile bu ilkeleri ilk örnek oluşturma adımları boyunca izleyin olmalıdır. Ancak, aşağıdaki kavramları bilgisi şunlardan yararlanabilirsiniz:

- Xcode ve Swift ile iOS uygulamaları oluşturmak.
- Yapılandırma bir [Azure bildirim hub'ı](notification-hubs-ios-apple-push-notification-apns-get-started.md) iOS için.
- [Apple Developer Portal'a](https://developer.apple.com) ve [Azure portalında](https://portal.azure.com).

> [!NOTE]
> Bildirim hub'ı kullanmak için yapılandırılmış **korumalı alan** yalnızca kimlik doğrulama modu. Üretim iş yükleri için bu kimlik doğrulama modu kullanmamalıdır.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="connect-your-ios-app-to-a-notification-hub"></a>İOS uygulamanızı bildirim hub'ına bağlama

Bu bölümde, bildirim hub'ına bağlanan bir iOS uygulaması oluşturacaksınız.  

### <a name="create-an-ios-project"></a>Bir iOS projesi oluşturun

1. Xcode'da yeni bir iOS projesi oluşturun ve **Single View Application** (Tek Görünüm Uygulaması) şablonunu seçin.

1. Yeni proje için seçenekleri ayarlarken:

   1. Belirtin **ürün adı** (PushDemo) ve **kuruluş tanımlayıcısı** (`com.<organization>`) ayarlarken kullandığınız **paket grubu tanımlayıcısı** Apple Geliştirici Portalı.

   1. Seçin **takım** , **uygulama kimliği** için ayarlanmış.

   1. Ayarlama **dil** için **Swift**.

   1. **İleri**’yi seçin.

1. Adlı yeni bir klasör oluşturun **SupportingFiles**.

1. Adlı yeni bir p-listesi dosyası oluşturma **devsettings.plist** içinde **SupportingFiles** klasör. Bu klasöre eklediğinizden emin olun, **da gitignore** bir git deposu ile çalışırken taahhüt aksi şekilde dosya. Bir üretim uygulamasında, büyük olasılıkla koşullu olarak bu gizli dizileri bir otomatik yapı işleminin bir parçası ayarlamanız. Bu ayarlar, bu izlenecek yolda kapsamında olmayan.

1. Güncelleştirme **devsettings.plist** sağladığınız bildirim hub'ından kendi değerlerini kullanarak aşağıdaki yapılandırma girdileri eklemek için:

   | Anahtar                            | Tür                     | Değer                     |
   |--------------------------------| -------------------------| --------------------------|
   | notificationHubKey             | String                   | <hubKey>                  |
   | notificationHubKeyName         | String                   | <hubKeyName>              |
   | notificationHubName            | String                   | <hubName>                 |
   | notificationHubNamespace       | String                   | <hubNamespace>            |

   Gereken değerleri, Azure Portalı'ndaki bildirim hub'ı kaynağına giderek bulabilirsiniz. Özellikle, **notificationHubName** ve **notificationHubNamespace** değerler sağ üst köşesinde **Essentials** içindeÖzet**Genel bakış** sayfası.

   ![Bildirim hub'ları Essentials özeti](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-essentials.png)

   Ayrıca bulabilirsiniz **notificationHubKeyName** ve **notificationHubKey** giderek değerleri **erişim ilkeleri** ve ilgili seçerek  **Erişim İlkesi**, gibi `DefaultFullSharedAccessSignature`. Bundan sonra kopyalama kaynağı **PRIMARY CONNECTION Strıng'i** değeri ön eki `SharedAccessKeyName=` için `notificationHubKeyName` ve değerin önüne `SharedAccessKey=` için `notificationHubKey`.

   Bağlantı dizesi şu biçimde olmalıdır:

   ```xml
   Endpoint=sb://<namespace>.servicebus.windows.net/;SharedAccessKeyName=<notificationHubKeyName>;SharedAccessKey=<notificationHubKey>
   ```

   Basit tutmak için belirtin `DefaultFullSharedAccessSignature` bildirimleri göndermek için belirteci kullanabilirsiniz. Uygulamada `DefaultListenSharedAccessSignature` durumlarda, yalnızca istediğiniz bildirim almak için daha iyi bir seçenek olacaktır.

1. Altında **Proje Gezgini**seçin **proje adı** seçip **genel** sekmesi.

1. Bulma **kimlik** ve ardından **paket grubu tanımlayıcısı** böylece eşleşecek değer `com.<organization>.PushDemo`, değer için kullanıldığı **uygulama kimliği** önceki adımdaki.

1. Bulma **imzalama**ve ardından uygun **takım** için **Apple Geliştirici hesabı**. **Takım** altında oluşturduğunuz sertifikalar ve profillerinin bir değer eşleşmelidir.

1. Xcode otomatik olarak çekmesini uygun **sağlama profili** değerini temel alarak **paket grubu tanımlayıcısı**. Yeni görmüyorsanız **sağlama profili** değeri, için profilleri yenilemeyi deneyin **imzalama kimliği** seçerek **Xcode**  >  **Tercihleri** > **hesabı** > **ayrıntıları görüntüle**. Seçin **imzalama kimliği**ve ardından **Yenile** düğmesine sağ alt profilleri indirilemedi.

1. Seçin **özellikleri** emin olun ve sekme **anında iletme bildirimleri** etkinleştirilir.

1. Açık, **AppDelegate.swift** uygulamak için dosya **UNUserNotificationCenterDelegate** protokolünü ve sınıfının en üstüne aşağıdaki kodu ekleyin:

    ```swift
    @UIApplicationMain
    class AppDelegate: UIResponder, UIApplicationDelegate, UNUserNotificationCenterDelegate {

        ...

        var configValues: NSDictionary?
        var notificationHubNamespace : String?
        var notificationHubName : String?
        var notificationHubKeyName : String?
        var notificationHubKey : String?
        let tags = ["12345"]
        let genericTemplate = PushTemplate(withBody: "{\"aps\":{\"alert\":\"$(message)\"}}")

        ...
    }
    ```

    Bu üyeleri daha sonra kullanacaksınız. Özellikle, kullanacağınız **etiketleri** ve **genericTemplate** üyeleri kaydın bir parçası olarak. Etiketler hakkında daha fazla bilgi için bkz. [kayıtlar için etiketler](notification-hubs-tags-segment-push-message.md) ve [şablon kayıtları](notification-hubs-templates-cross-platform-push-messages.md).

1. Aynı dosyada, aşağıdaki kodu ekleyin **didFinishLaunchingWithOptions** işlevi:

    ```swift
    if let path = Bundle.main.path(forResource: "devsettings", ofType: "plist") {
        if let configValues = NSDictionary(contentsOfFile: path) {
            self.notificationHubNamespace = configValues["notificationHubNamespace"] as? String
            self.notificationHubName = configValues["notificationHubName"] as? String
            self.notificationHubKeyName = configValues["notificationHubKeyName"] as? String
            self.notificationHubKey = configValues["notificationHubKey"] as? String
        }
    }

    if #available(iOS 10.0, *){
        UNUserNotificationCenter.current().delegate = self
        UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .sound, .badge]) {
            (granted, error) in

            if (granted)
            {
                DispatchQueue.main.async {
                    application.registerForRemoteNotifications()
                }
            }
        }
    }

    return true
    ```

    Bu kod, ayarı değerleri alır. **devsettings.plist**, ayarlar **AppDelegate** olarak sınıf **UNUserNotificationCenter** temsilcisi, istekleri yetkilendirme anında iletme bildirimleri ve aramalar için **registerForRemoteNotifications**.

    Kodu basit tutmak için destekleyen *iOS 10 ve üzeri*. Normalde yaptığınız gibi koşullu olarak ilgili API'leri ve yaklaşımları kullanarak önceki işletim sistemi sürümleri için destek ekleyebilirsiniz.

1. Aynı dosyada, aşağıdaki işlevleri ekleyin:

    ```swift
    func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
        let installationId = (UIDevice.current.identifierForVendor?.description)!
        let pushChannel = deviceToken.reduce("", {$0 + String(format: "%02X", $1)})
    }

    func showAlert(withText text : String) {
        let alertController = UIAlertController(title: "PushDemo", message: text, preferredStyle: UIAlertControllerStyle.alert)
        alertController.addAction(UIAlertAction(title: "OK", style: UIAlertActionStyle.default,handler: nil))
        self.window?.rootViewController?.present(alertController, animated: true, completion: nil)
    }
    ```

    Kod **InstallationID** ve **pushChannel** bildirim hub'ınızla kaydolmak için değerler. Bu durumda, kullanmakta olduğunuz **UIDevice.current.identifierForVendor** cihaz ve ardından biçimlendirmesini tanımlamak için benzersiz bir değer sağlamak için **deviceToken** istenen sağlamak  **pushChannel** değeri. **ShowAlert** exists işlevi yalnızca tanıtım amacıyla bazı ileti metni görüntülemek için.

1. Hala **AppDelegate.swift** ekleyin **willPresent** ve **didReceive** yaramaz **UNUserNotificationCenterDelegate**. Bu işlevler bir uygulama sırasıyla ön plan veya arka planda çalıştığı bildirilir bir uyarı görüntülenir.

    ```swift
    @available(iOS 10.0, *)
    func userNotificationCenter(_ center: UNUserNotificationCenter, 
        willPresent notification: UNNotification, 
        withCompletionHandler completionHandler: @escaping (UNNotificationPresentationOptions) -> Void) {
        showAlert(withText: notification.request.content.body)
    }

    @available(iOS 10.0, *)
    func userNotificationCenter(_ center: UNUserNotificationCenter, 
        didReceive response: UNNotificationResponse, 
        withCompletionHandler completionHandler: @escaping () -> Void) {
        showAlert(withText: response.notification.request.content.body)
    }
    ```

1. Yazdırma deyimleri bölmenizin altına eklemek **didRegisterForRemoteNotificationsWithDeviceToken** doğrulamak için işlevi **InstallationID** ve **pushChannel** yükleniyor atanan değerler.

1. Oluşturma **modelleri**, **Hizmetleri**, ve **yardımcı programları** vurgulanan temel bileşenlere klasörleri eklediğiniz projeye daha sonra.

1. Proje oluşturulur ve fiziksel bir cihazda çalışan kontrol edin. Simülatör'ü kullanarak anında iletme bildirimleri test edilemiyor.

### <a name="create-models"></a>Model oluşturma

Bu adımda, bir dizi temsil etmek için modelleri oluşturacaksınız [Notification Hubs REST API'si](/rest/api/notificationhubs/) yükler ve mağazaya gerekli paylaşılan erişim imzası (SAS) belirteci veri.

1. Adlı yeni bir Swift dosya ekleme **PushTemplate.swift** için **modelleri** klasör. Bu modeli temsil eden yapısı sağlar **GÖVDESİ** bireysel şablonunun bir parçası olarak **DeviceInstallation** yükü.

    ```swift
    import Foundation

    struct PushTemplate : Codable {
        let body : String

        init(withBody body : String) {
            self.body = body
        }
    }
    ```

1. Adlı yeni bir Swift dosya ekleme **DeviceInstallation.swift** için **modelleri** klasör. Bu dosya, oluşturmak veya güncelleştirmek için yükü temsil eden bir yapı tanımlar. bir **cihaz yüklemesi**. Dosyaya aşağıdaki kodu ekleyin:

    ```swift
    import Foundation

    struct DeviceInstallation : Codable {
        let installationId : String
        let pushChannel : String
        let platform : String = "apns"
        var tags : [String]
        var templates : Dictionary<String, PushTemplate>

        init(withInstallationId installationId : String, andPushChannel pushChannel : String) {
            self.installationId = installationId
            self.pushChannel = pushChannel
            self.tags = [String]()
            self.templates = Dictionary<String, PushTemplate>()
        }
    }
    ```

1. Adlı yeni bir Swift dosya ekleme **TokenData.swift** için **modelleri** klasör. Bu model, bir SAS belirteci, sona erme birlikte depolamak için kullanılır.

    ```swift
    import Foundation

    struct TokenData {

        let token : String
        let expiration : Int

        init(withToken token : String, andTokenExpiration expiration : Int) {
            self.token = token
            self.expiration = expiration
        }
    }
    ```

### <a name="generate-a-sas-token"></a>Bir SAS belirteci oluştur

Notification hubs'ı, Azure Service Bus aynı güvenlik altyapısını kullanır. REST API'sini çağırmak için yapmanız gerekir [program aracılığıyla SAS belirteci oluşturmak](/rest/api/eventhub/generate-sas-token) içinde kullanılabilir **yetkilendirme** isteği üstbilgisi.  

Elde edilen belirteç şu biçimde olacaktır:

```xml
SharedAccessSignature sig=<UrlEncodedSignature>&se=<ExpiryEpoch>&skn=<KeyName>&sr=<UrlEncodedResourceUri>
```

İşlemin kendisi altı aynı temel adımları kapsar:  

1. Süre sonu içinde bilgi işlem [UNIX dönem zamanı](https://en.wikipedia.org/wiki/Unix_time) geçen gece yarısından itibaren Eşgüdümlü Evrensel Saat, 1 Ocak 1970 biçiminde saniye sayısını anlamına gelir.
1. Biçimlendirme **ResourceUrl** yüzde olarak kodlanmış ve küçük olacak şekilde erişmeye çalıştığınız kaynak temsil eder. **ResourceUrl** formundadır `'https://<namespace>.servicebus.windows.net/<hubName>'`.
1. Hazırlama **StringToSign**, olarak biçimlendirilmiş `'<UrlEncodedResourceUrl>\n<ExpiryEpoch>'`.
1. Bilgi işlem ve Base64 kodlaması **imza** HMAC SHA256 karmasını kullanarak **StringToSign** değeri. Karma değeri ile kullanılan **anahtarı** parçası **bağlantı dizesi** ilgili için **yetkilendirme kuralı**.
1. Base64 kodlu biçimlendirme **imza** , bu nedenle yüzde olarak kodlanmış.
1. Beklenen biçim belirteci kullanarak oluşturmak **UrlEncodedSignature**, **ExpiryEpoch**, **KeyName**, ve **UrlEncodedResourceUrl** değerleri.

Bkz: [Azure Service Bus belgeleri](../service-bus-messaging/service-bus-sas.md) paylaşılan erişim imzası ve bunu nasıl Azure Service Bus ve Notification hubs'ın kullandığı ilişkin daha kapsamlı bir genel bakış.

Swift bu örneğin amacı doğrultusunda, Apple'nın açık kaynak olacak **CommonCrypto** karma imzası ile yardımcı olmak için kitaplığı. Bir C Kitaplığı olduğu gibi kullanıma hazır Swift erişilebilir değil. Bir köprü oluşturma üst bilgisini kullanarak kitaplık kullanılabilir hale getirebilirsiniz.

Ekleme ve köprü oluşturma üst bilgi yapılandırmak için:

1. Xcode içindeki seçin **dosya** > **yeni** > **dosya** > **üstbilgi dosyası**. Üst bilgi dosyası adını **BridgingHeader.h**.

1. İçeri aktarma dosyasını düzenleyerek **CommonHMAC.h**:

    ```swift
    #import <CommonCrypto/CommonHMAC.h>

    #ifndef BridgingHeader_h
    #define BridgingHeader_h


    #endif /* BridgingHeader_h */
    ```

1. Hedefin güncelleştirme **Build Settings** köprü oluşturma üst bilgisi başvurmak için:

   1. Açık **yapı ayarları** sekmesini ve ekranı aşağı kaydırarak **Swift derleyicisi** bölümü.

   1. Emin **yükleme Objective-C uyumluluğu üstbilgi** seçeneği **Evet**.

   1. Dosya yolunu girin `'<ProjectName>/BridgingHeader.h'` içine **Objective-C köprü oluşturma üst bilgisi** seçeneği. Bizim köprü oluşturma üst bilgi dosyası yolu budur.

   Bu seçenekler bulamazsanız, sahip olduğunuzdan emin olun **tüm** seçilen görünümü yerine **temel** veya **özelleştirilmiş**.

   Kullanarak yapabileceğiniz birçok üçüncü taraf açık kaynak kapsayıcı kitaplıkları kullanılabilir vardır **CommonCrypto** biraz daha kolay. Ancak, bu tür kitaplıklarının bu makalenin kapsamı dışındadır tartışmadır.

1. Adlı yeni bir Swift dosya ekleme **TokenUtility.swift** içinde **yardımcı programları** klasörü ve aşağıdaki kodu ekleyin:

   ```swift
   import Foundation

   struct TokenUtility {    
        typealias Context = UnsafeMutablePointer<CCHmacContext>

        static func getSasToken(forResourceUrl resourceUrl : String, withKeyName keyName : String, andKey key : String, andExpiryInSeconds expiryInSeconds : Int = 3600) -> TokenData {
            let expiry = (Int(NSDate().timeIntervalSince1970) + expiryInSeconds).description
            let encodedUrl = urlEncodedString(withString: resourceUrl)
            let stringToSign = "\(encodedUrl)\n\(expiry)"
            let hashValue = sha256HMac(withData: stringToSign.data(using: .utf8)!, andKey: key.data(using: .utf8)!)
            let signature = hashValue.base64EncodedString(options: .init(rawValue: 0))
            let encodedSignature = urlEncodedString(withString: signature)
            let sasToken = "SharedAccessSignature sr=\(encodedUrl)&sig=\(encodedSignature)&se=\(expiry)&skn=\(keyName)"
            let tokenData = TokenData(withToken: sasToken, andTokenExpiration: expiryInSeconds)

            return tokenData
        }

        private static func sha256HMac(withData data : Data, andKey key : Data) -> Data {
            let context = Context.allocate(capacity: 1)
            CCHmacInit(context, CCHmacAlgorithm(kCCHmacAlgSHA256), (key as NSData).bytes, size_t((key as NSData).length))
            CCHmacUpdate(context, (data as NSData).bytes, (data as NSData).length)
            var hmac = Array<UInt8>(repeating: 0, count: Int(CC_SHA256_DIGEST_LENGTH))
            CCHmacFinal(context, &hmac)

            let result = NSData(bytes: hmac, length: hmac.count)
            context.deallocate()

            return result as Data
        }

        private static func urlEncodedString(withString stringToConvert : String) -> String {
            var encodedString = ""
            let sourceUtf8 = (stringToConvert as NSString).utf8String
            let length = strlen(sourceUtf8)

            let charArray: [Character] = [ ".", "-", "_", "~", "a", "z", "A", "Z", "0", "9"]
            let asUInt8Array = String(charArray).utf8.map{ Int8($0) }

            for i in 0..<length {
                let currentChar = sourceUtf8![i]

                if (currentChar == asUInt8Array[0] || currentChar == asUInt8Array[1] || currentChar == asUInt8Array[2] || currentChar == asUInt8Array[3] ||
                    (currentChar >= asUInt8Array[4] && currentChar <= asUInt8Array[5]) ||
                    (currentChar >= asUInt8Array[6] && currentChar <= asUInt8Array[7]) ||
                    (currentChar >= asUInt8Array[8] && currentChar <= asUInt8Array[9])) {
                    encodedString += String(format:"%c", currentChar)
                }
                else {
                    encodedString += String(format:"%%%02x", currentChar)
                }
            }

            return encodedString
        }
    }
   ```

   Bu yardımcı programı, SAS belirteci oluşturan mantıksal kapsüller.

   Daha önce belirtildiği gibi **getSasToken** işlevi belirteç hazırlanmak için gereken üst düzey adımlar düzenler. İşlevi, bu öğreticinin ilerleyen bölümlerinde Yükleme hizmeti tarafından çağrılır.

   Diğer iki işlev çağıran **getSasToken** işlevi: **sha256HMac** imza bilgi işlem ve **urlEncodedString** ilişkili URL kodlaması için dize. **UrlEncodedString** işlevi, yerleşik kullanarak gerekli çıkış elde etmek mümkün değil olarak gereklidir **addingPercentEncoding** işlevi.

   [Azure depolama iOS SDK'sı](https://github.com/Azure/azure-storage-ios/blob/master/Lib/Azure%20Storage%20Client%20Library/Azure%20Storage%20Client%20Library/AZSUtil.m) Objective-c içinde bu işlemler yaklaşımını nasıl mükemmel bir örneğidir Azure Service Bus SAS belirteçleri hakkında daha fazla bilgi bulunabilir [Azure Service Bus belgeleri](../service-bus-messaging/service-bus-sas.md).

### <a name="verify-the-sas-token"></a>SAS belirteci doğrulayın

İstemci yüklemesi hizmeti uygulamadan önce uygulamamızı doğru SAS belirteci seçtiğiniz, HTTP yardımcı programını kullanarak oluşturduğunu denetleyin. Bu öğreticinin amaçları doğrultusunda, tercih ettiğiniz aracımızı olacaktır **Postman**.

Not için bir uygun şekilde yerleştirilen yazdırma deyimi veya kesme noktası kullanın **InstallationID** ve **belirteci** uygulama tarafından oluşturulan değerleri.

Çağırmak için bu adımları **yüklemeleri** API:

1. İçinde **Postman**, yeni bir sekme açın.

1. İstek kümesine **alma** ve aşağıdaki adresi belirtin:

    ```xml
    https://<namespace>.servicebus.windows.net/<hubName>/installations/<installationId>?api-version=2015-01
    ```

1. İstek üst bilgilerini aşağıdaki gibi yapılandırın:

   | Anahtar           | Değer            |
   | ------------- | ---------------- |
   | İçerik türü  | uygulama/json |
   | Yetkilendirme | <sasToken>       |
   | x-ms-version  | 2015-01          |

1. Seçin **kod** sağ üst altında görünen düğme **Kaydet** düğmesi. İstek aşağıdaki örneğe benzer olmalıdır:

    ```html
    GET /<hubName>/installations/<installationId>?api-version=2015-01 HTTP/1.1
    Host: <namespace>.servicebus.windows.net
    Content-Type: application/json
    Authorization: <sasToken>
    x-ms-version: 2015-01
    Cache-Control: no-cache
    Postman-Token: <postmanToken>
    ```

1. Seçin **Gönder** düğmesi.

Kayıt için belirtilen mevcut **InstallationID** bu noktada. Doğrulama "401 Yetkisiz" yanıt yerine bir "404 bulunamadı" yanıtı neden olur. Bu sonuç, SAS belirteci kabul edildiğini onaylamalıdır.

### <a name="implement-the-installation-service-class"></a>Uygulama yükleme hizmet sınıfı

Bizim temel funcınner çevresindeki sonraki uygulayacaksınız [yüklemeleri REST API](/rest/api/notificationhubs/create-overwrite-installation).  

Adlı yeni bir Swift dosya ekleme **NotificationRegistrationService.swift** altında **Hizmetleri** klasörünü ve ardından bu dosyaya aşağıdaki kodu ekleyin:

```swift
import Foundation

class NotificationRegistrationService {
    private let tokenizedBaseAddress: String = "https://%@.servicebus.windows.net/%@"
    private let tokenizedCreateOrUpdateInstallationRequest = "/installations/%@?api-version=%@"
    private let session = URLSession(configuration: URLSessionConfiguration.default)
    private let apiVersion = "2015-01"
    private let jsonEncoder = JSONEncoder()
    private let defaultHeaders: [String : String]
    private let installationId : String
    private let pushChannel : String
    private let hubNamespace : String
    private let hubName : String
    private let keyName : String
    private let key : String
    private var tokenData : TokenData? = nil

    init(withInstallationId installationId : String,
            andPushChannel pushChannel : String,
            andHubNamespace hubNamespace : String,
            andHubName hubName : String,
            andKeyName keyName : String,
            andKey key: String) {
        self.installationId = installationId
        self.pushChannel = pushChannel
        self.hubNamespace = hubNamespace
        self.hubName = hubName
        self.keyName = keyName
        self.key = key
        self.defaultHeaders = ["Content-Type": "application/json", "x-ms-version": apiVersion]
    }

    func register(
        withTags tags : [String]? = nil,
        andTemplates templates : Dictionary<String, PushTemplate>? = nil,
        completeWith completion: ((_ result: Bool) -> ())? = nil) {

        var deviceInstallation = DeviceInstallation(withInstallationId: installationId, andPushChannel: pushChannel)

        if let tags = tags {
            deviceInstallation.tags = tags
        }

        if let templates = templates {
            deviceInstallation.templates = templates
        }

        if let deviceInstallationJson = encodeToJson(deviceInstallation) {
            let sasToken = getSasToken()
            let requestUrl = String.init(format: tokenizedCreateOrUpdateInstallationRequest, installationId, apiVersion)
            let apiEndpoint = "\(getBaseAddress())\(requestUrl)"

            var request = URLRequest(url: URL(string: apiEndpoint)!)
            request.httpMethod = "PUT"

            for (key,value) in self.defaultHeaders {
                request.addValue(value, forHTTPHeaderField: key)
            }

            request.addValue(sasToken, forHTTPHeaderField: "Authorization")
            request.httpBody = Data(deviceInstallationJson.utf8)

            (self.session.dataTask(with: request) { dat, res, err in
                if let completion = completion {
                        completion(err == nil && (res as! HTTPURLResponse).statusCode == 200)
                }
            }).resume()
        }
    }

    private func getBaseAddress() -> String {
        return String.init(format: tokenizedBaseAddress, hubNamespace, hubName)
    }

    private func getSasToken() -> String {
        if (tokenData == nil ||
            Date(timeIntervalSince1970: Double((tokenData?.expiration)!)) < Date(timeIntervalSinceNow: -(5 * 60))) {
            self.tokenData = TokenUtility.getSasToken(forResourceUrl: getBaseAddress(), withKeyName: self.keyName, andKey: self.key)
        }

        return (tokenData?.token)!
    }

    private func encodeToJson<T : Encodable>(_ object: T) -> String? {
        do {
            let jsonData = try jsonEncoder.encode(object)
            if let jsonString = String(data: jsonData, encoding: .utf8) {
                return jsonString
            } else {
                return nil
            }
        }
        catch {
            return nil
        }
    }
}
```

Önkoşul ayrıntıları başlatma bir parçası olarak sağlanır. Etiketler ve şablonlar isteğe bağlı olarak geçirildi **kaydetme** bir parçasını oluşturan işleve **cihaz yüklemesi** JSON yükü.  

**Kaydetme** istek hazırlamak için diğer özel işlevler işlevini çağırır. Bir yanıt alındıktan sonra tamamlama çağrılır ve kaydın başarılı olup olmadığını gösterir.  

İstek uç noktası tarafından oluşturulan **getBaseAddress** işlevi. Yapı bildirim hub'ı parametrelerini kullanır *ad alanı* ve *adı* başlatma sırasında sağlandı.  

**GetSasToken** işlevi şu anda saklı belirtecin geçerli olup olmadığını denetler. Belirteç geçerli değil, işlevini çağırır. **TokenUtility** yeni bir belirteç oluşturmak için bir değer döndürmeden önce depolar.

Son olarak, **encodeToJson** istek gövdesi bir parçası olarak kullanmak için ilgili model nesnelerini JSON'a dönüştürür.

### <a name="invoke-the-notification-hubs-rest-api"></a>Notification hubs'ı REST API çağırma

Son adım güncelleştirme **AppDelegate** kullanılacak **NotificationRegistrationService** kaydetmek için sunduğumuz **NotificationHub**.

1. Açık **AppDelegate.swift** ve bir başvuru depolamak için bir sınıf düzeyi değişkenleri ekleyin **NotificationRegistrationService**:

    ```swift
    var registrationService : NotificationRegistrationService?
    ```

1. Aynı dosyada, güncelleştirme **didRegisterForRemoteNotificationsWithDeviceToken** işlevi **NotificationRegistrationService** gerekli parametreler ve ardından bir çağrı ile **kaydetme** işlevi.

    ```swift
    func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
        let installationId = (UIDevice.current.identifierForVendor?.description)!
        let pushChannel = deviceToken.reduce("", {$0 + String(format: "%02X", $1)})

        // Initialize the Notification Registration Service
        self.registrationService = NotificationRegistrationService(
            withInstallationId: installationId,
            andPushChannel: pushChannel,
            andHubNamespace: notificationHubNamespace!,
            andHubName: notificationHubName!,
            andKeyName: notificationHubKeyName!,
            andKey: notificationHubKey!)

        // Call register, passing in the tags and template parameters
        self.registrationService!.register(withTags: tags, andTemplates: ["genericTemplate" : self.genericTemplate]) { (result) in
            if !result {
                print("Registration issue")
            } else {
                print("Registered")
            }
        }
    }
    ```

    Basit tutmak için çıkış penceresine sonucuyla güncelleştirilecek birkaç yazdırma deyimleri kullanılacak gideceğinizi **kaydetme** işlemi.

1. Şimdi oluşturun ve uygulamayı fiziksel bir cihazda çalıştırın. Çıkış penceresinde "kaydedildi" görmeniz gerekir.

## <a name="test-the-solution"></a>Çözüm test

Bu aşamada uygulamamızı kayıtlı **NotificationHub** ve anında iletme bildirimleri alabilirsiniz. Xcode'da, hata ayıklayıcıyı durdurun ve şu anda çalışmakta olan uygulamayı kapatın. Ardından, bu maddeyi **cihaz yüklemesi** anında iletme bildirimleri ayrıntıları beklendiği gibi ve uygulamamızı artık alabilir.  

### <a name="verify-the-device-installation"></a>Cihaz yüklemesini doğrulama

Kullanarak daha önce yaptığınız gibi artık aynı istekte bulunabilirsiniz **Postman** için [SAS belirteci doğrulama](#verify-the-sas-token). SAS belirteci dolmadığından varsayarak yanıt artık etiketler ve şablonlar gibi sağlanan yükleme ayrıntıları içermelidir.

```json
{
    "installationId": "<installationId>",
    "pushChannel": "<pushChannel>",
    "pushChannelExpired": false,
    "platform": "apns",
    "expirationTime": "9999-12-31T23:59:59.9999999Z",
    "tags": [
        "12345"
    ],
    "templates": {
        "genericTemplate": {
            "body": "{\"aps\":{\"alert\":\"$(message)\"}}",
            "tags": [
                "genericTemplate"
            ]
        }
    }
}
```

### <a name="send-a-test-notification-azure-portal"></a>(Azure portalı) test bildirimi gönder

Bildirimleri almak, test etmek için en hızlı yolu, Azure Portalı'nda bildirim hub'ına göz atmaktır:

1. Azure portalında göz **genel bakış** bildirim hub'ınıza sekmesinde.

1. Seçin **Test gönderimi**, üzerinde **Essentials** sol üst ve portal penceresinin özeti:

    ![Bildirim hub'ları Essentials Özet sınama Gönder düğmesi](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-essentials-test-send.png)

1. Seçin **özel şablonu** gelen **platformları** listesi.

1. Girin **12345** için **etiket ifadesine Gönder**. Bu etiket bizim yüklemesinde önceden belirttiğiniz.

1. İsteğe bağlı olarak Düzenle **ileti** JSON yükündeki:

    ![Bildirim hub'ları Test gönderimi](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-test-send.png)

1. **Gönder**’i seçin. Portalda, bildirim cihaza başarıyla gönderilip gönderilmediğini belirtmeniz gerekir:

    ![Bildirim hub'ları Test sonuçları gönder](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-test-send-result.png)

    Uygulama ön planda çalışmayan varsayılarak, ayrıca bir bildirim görmeniz gerekir **bildirim Merkezi** Cihazınızda. Bildirime dokunulduğunda uygulama açmalı ve uyarıyı gösterir.

    ![Örnek bildirim aldı](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/test-send-notification-received.png)

### <a name="send-a-test-notification-mail-carrier"></a>(Posta taşıyıcı) test bildirimi gönder

Aracılığıyla bildirim gönderebilen [REST API](/rest/api/notificationhubs/) kullanarak **Postman**, test etmek için daha kullanışlı bir yol olabilir.

1. Yeni bir sekmede açmak **Postman**.

1. İstek kümesine **POST**, aşağıdaki adresi girin:

    ```xml
    https://<namespace>.servicebus.windows.net/<hubName>/messages/?api-version=2015-01
    ```

1. İstek üst bilgilerini aşağıdaki gibi yapılandırın:

   | Anahtar                            | Değer                          |
   | ------------------------------ | ------------------------------ |
   | İçerik türü                   | Uygulama/json; charset = utf-8 |
   | Yetkilendirme                  | <sasToken>                     |
   | ServiceBusNotification-Format  | şablon                       |
   | Tags                           | "12345"                        |

1. Yapılandırma isteğini **GÖVDESİ** kullanılacak **RAW - JSON (application.json)** aşağıdaki JSON yükü ile:

    ```json
    {
       "message" : "Hello from Postman!"
    }
    ```

1. Seçin **kod** altında olan düğme **Kaydet** pencerenin sağ üst düğmesi. İstek aşağıdaki örneğe benzer olmalıdır:

    ```html
    POST /<hubName>/messages/?api-version=2015-01 HTTP/1.1
    Host: <namespace>.servicebus.windows.net
    Content-Type: application/json;charset=utf-8.
    ServiceBusNotification-Format: template
    Tags: "12345"
    Authorization: <sasToken>
    Cache-Control: no-cache
    Postman-Token: <postmanToken>

    {
        "message" : "Hello from Postman!"
    }
    ```

1. Seçin **Gönder** düğmesi.

Başarılı durum kodu alın ve istemci cihazda bildirim almak gerekir.

## <a name="next-steps"></a>Sonraki adımlar
Bildirim hub'ı bağlı bir temel iOS Swift uygulaması artık sahip [REST API](/rest/api/notificationhubs/) ve gönderebilir ve bildirimleri alabilirsiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure Notification Hubs'a genel bakış](notification-hubs-push-notification-overview.md)
- [Notification hubs'ı REST API'leri](/rest/api/notificationhubs/)
- [Arka uç işlemleri için Notification Hubs SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)
- [Notification hubs'ın GitHub üzerinde SDK](https://github.com/Azure/azure-notificationhubs)
- [Uygulama arka ucu ile kaydetme](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)
- [Kayıt Yönetimi](notification-hubs-push-notification-registration-management.md)
- [Etiketleri ile çalışma](notification-hubs-tags-segment-push-message.md) 
- [Özel şablonları ile çalışma](notification-hubs-templates-cross-platform-push-messages.md)
- [Paylaşılan erişim imzaları ile Service Bus erişim denetimi](../service-bus-messaging/service-bus-sas.md)
- [Program aracılığıyla SAS belirteçleri oluşturmak](/rest/api/eventhub/generate-sas-token)
- [Apple güvenlik: ortak şifreleme](https://developer.apple.com/security/)
- [UNIX dönem zamanı](https://en.wikipedia.org/wiki/Unix_time)
- [HMAC](https://en.wikipedia.org/wiki/HMAC)
