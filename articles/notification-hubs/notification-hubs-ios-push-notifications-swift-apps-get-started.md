---
title: Anında iletme bildirimleri için Azure Notification hubs'ı kullanarak Swift iOS uygulamaları | Microsoft Docs
description: Anında iletme bildirimleri için Azure Notification hubs'ı kullanarak iOS Swift uygulamaları hakkında bilgi edinin.
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
ms.openlocfilehash: 64a05c6cbd412953ae10b31efc1d141c9cefd062
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65994774"
---
# <a name="tutorial-push-notifications-to-swift-ios-apps-using-the-notification-hubs-rest-api"></a>Öğretici: Notification Hubs REST API kullanarak Swift iOS uygulamaları için anında iletme bildirimleri

> [!div class="op_single_selector"]
> * [Objective-C](notification-hubs-ios-apple-push-notification-apns-get-started.md)
> * [Swift](notification-hubs-ios-push-notifications-swift-apps-get-started.md)

Bu öğreticide, Azure Notification hubs'ı kullanarak iOS Swift tabanlı uygulama için anında iletme bildirimleri için kullandığınız [REST API](/rest/api/notificationhubs/). [Apple Anında İletilen Bildirim servisini (APNs)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1) kullanarak anında iletme bildirimleri alan boş bir iOS uygulaması oluşturursunuz.

Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Sertifika imzalama isteği dosyası oluşturma
> * Anında iletme bildirimleri için uygulamanızı isteme
> * Uygulama için bir sağlama profili oluşturun
> * Bildirim hub’ı oluşturma
> * Bildirim hub'ı APNS bilgilerle yapılandırın.
> * iOS uygulamanızı bildirim hub’larına bağlama
> * Çözüm test

## <a name="prerequisites"></a>Önkoşullar
Örneği takip etmek için ihtiyacınız vardır:

- Git aracılığıyla [Azure Notification Hubs'a genel bakış](notification-hubs-push-notification-overview.md) hizmetiyle ilgili bilgi sahibi değilseniz. 
- Kayıtları ve yükleme hakkında bilgi edinin: [Kayıt Yönetimi](notification-hubs-push-notification-registration-management.md)
- Etkin bir [Apple Geliştirici hesabı](https://developer.apple.com) 
- Xcode birlikte Anahtarlığınıza yüklü bir geçerli bir geliştirici sertifikası ile bir Mac
- Bir fiziksel iPhone cihaz çalıştırın ve ile hata ayıklama (simülatör ile anında iletme bildirimlerini test etmek mümkün değildir)
- Kayıtlı fiziksel iPhone Cihazınızı [Apple portalı](https://developer.apple.com) ve sertifikanızla ilişkili
- Bir [Azure abonelik](https://portal.azure.com) oluşturduğunuz ve kaynaklarını yönetme

Bu ilkeleri ilk örnek konusunda deneyim olmadan oluşturmak için takip etmek mümkündür. Ancak, aşağıdaki kavramları bilgisi yararlı olacaktır:

- Xcode ve Swift ile iOS uygulamaları oluşturma
- Yapılandırma bir [Azure bildirim hub'ı](notification-hubs-ios-apple-push-notification-apns-get-started.md) iOS için
- Konusunda [Apple Developer Portal'a](https://developer.apple.com) ve [Azure portalı](https://portal.azure.com)

> [!NOTE]
> Bildirim hub'ı kullanmak için yapılandırılmış *korumalı alan* yalnızca kimlik doğrulama modu. Üretim iş yükleri için bu kimlik doğrulama modu kullanmamalıdır.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="connect-your-ios-app-to-notification-hubs"></a>iOS uygulamanızı Notification Hubs'a bağlama
Bu bölümde, bildirim hub'ına bağlanan bir iOS uygulaması oluşturacaksınız.  

### <a name="create-an-ios-project"></a>Bir iOS projesi oluşturun
1. İçinde **Xcode**, yeni bir iOS projesi oluşturun ve seçin **tek görünüm uygulaması** şablonu.
2. Yeni proje için seçenekleri ayarlarken, şu adımları izleyin:
    1. Aynı **ürün adı** (diğer bir deyişle, **PushDemo**) ve **kuruluş tanımlayıcısı** (diğer bir deyişle, `com.<organization>`) eklerken kullandığınız **paket Tanımlayıcı** belirlenip **Apple Developer Portal'a**. 
    2. Seçin **takım** , **uygulama kimliği** için ayarlanmış.
    3. Ayarlama **dil** için **Swift**.
    4. **İleri**'ye tıklayın.
3. Adlı yeni bir klasör oluşturun **SupportingFiles**.
4. Yeni bir **plist** adlı dosya **devsettings.plist** altında **SupportingFiles** klasör. Bu klasöre eklediğinizden emin olun, **da gitignore** ile çalışırken taahhüt aksi şekilde dosya bir **git deposu**. Bir üretim uygulamasında, büyük olasılıkla koşullu olarak bu gizli dizileri bir otomatik yapı işleminin bir parçası ayarlamanız. Ancak, bu kılavuzun bir parçası olarak kapsanmıyor.
5. Güncelleştirme **devsettings.plist** aşağıdaki yapılandırma girdileri eklemek için (kendi değerleri kullanarak **bildirim hub'ı** , sağlanan):

   | Anahtar                            | Tür                     | Değer                     |               
   |--------------------------------| -------------------------| --------------------------|
   | notificationHubKey             | String                   | \<hubKey\>                |
   | notificationHubKeyName         | String                   | \<hubKeyName\>            |
   | notificationHubName            | String                   | \<HubName\>               |
   | notificationHubNamespace       | String                   | \<hubNamespace\>          |
    
   Gerekli değerler giderek bulabilirsiniz **bildirim hub'ı** kaynakta **Azure portalında**. Bulabilirsiniz **notificationHubName** ve **notificationHubNamespace** değerleri sağ üst köşesindeki **Essentials** içinde Özet  **Genel Bakış** sayfası.

   ![Bildirim hub'ları Essentials özeti](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-essentials.png)

   Bulabilirsiniz **notificationHubKeyName** ve **notificationHubKey** giderek değerleri **erişim ilkeleri**, öğesine tıklayarak ilgili **erişim İlke**. Örneğin, `DefaultFullSharedAccessSignature`. Ardından kopyalama **PRIMARY CONNECTION Strıng'i** değeri ön eki `SharedAccessKeyName=` için `notificationHubKeyName` ve değerin önüne `SharedAccessKey=` için `notificationHubKey`. Bağlantı dizesi şu biçimde olmalıdır:
    
   ```xml
   Endpoint=sb://<namespace>.servicebus.windows.net/;SharedAccessKeyName=<notificationHubKeyName>;SharedAccessKey=<notificationHubKey>
   ```

   Basit tutmak için kullanacağınız *DefaultFullSharedAccessSignature* de bildirimleri göndermek için belirteci kullanabilirsiniz. Ancak, uygulamada `DefaultListenSharedAccessSignature` durumlarda, yalnızca istediğiniz bildirim almak için daha iyi bir seçenek olacaktır.       
6. Altında **Proje Gezgini**, tıklayın **proje adı**, ardından **genel** sekmesi
7. Bulma **kimlik**, ardından **paket grubu tanımlayıcısı** için kullanılanla eşleşen için değer **uygulama kimliği** (önceki adımdan) diğer bir deyişle, `'com.<organization>.PushDemo'`
8. Bulma **imzalama**, ardından uygun seçtiğinizden emin olun **takım** için **Apple Geliştirici hesabı** (altında oluşturduğunuz sertifika ve profilleri bir daha önce).  **Xcode** otomatik olarak uygun çekmelidir **sağlama profili** göre **paket grubu tanımlayıcısı**. Yeni görmüyorsanız **sağlama profili**, için profilleri yenilemeyi deneyin **imzalama kimliği** (*Xcode > Tercihler > Hesap > ayrıntıları görüntüle*). Tıklayarak **imzalama kimliği**, ardından **Yenile** sağ alt düğmesini profilleri indirme.
9. Seçin **özellikleri** emin olun ve sekme **anında iletme bildirimleri** etkinleştirilir.
10. Açık, **AppDelegate.swift** uygulamak için dosya *UNUserNotificationCenterDelegate* protokolünü ve sınıfının en üstüne aşağıdaki kodu ekleyin:
    
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

    Bu üyeleri daha sonra kullanacaksınız. *Etiketleri* ve *genericTemplate* kaydın bir parçası kullanılır. Etiketler hakkında daha fazla bilgi için bkz. [kayıtlar için etiketler](notification-hubs-tags-segment-push-message.md) ve [şablon kayıtları](notification-hubs-templates-cross-platform-push-messages.md).
 
11. Aynı dosyada, aşağıdaki kodu ekleyin *didFinishLaunchingWithOptions* işlevi:

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

    Bu kod, ayarı değerleri alır. **devsettings.plist**, ayarlar **AppDelegate** olarak sınıf *UNUserNotificationCenter* temsilcisi, istekleri yetkilendirme anında iletme bildirimleri ve aramalar için *registerForRemoteNotifications*.
    
    Kodu basit tutmak için destekleyen **iOS 10 ve yalnızca yukarıda**. Normalde yaptığınız gibi koşullu olarak ilgili API'leri ve yaklaşımları kullanarak eski işletim sistemi sürümleri için destek ekleyebilirsiniz.

12. Aynı dosyada, aşağıdaki işlevleri ekleyin:

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

    Kod *InstallationID* ve *pushChannel* kaydetmek için değerleri **bildirim hub'ı**. Bu durumda, kullanmakta olduğunuz *UIDevice.current.identifierForVendor* bize tarafından cihazı belirlemek ve sonra biçimlendirmek için benzersiz bir değer sağlamak için *deviceToken* bize istenen ilesağlamakiçin*pushChannel* değeri. *ShowAlert* işlev, yalnızca tanıtım amacıyla bazı ileti metni görüntülemek için.

13. Hala **AppDelegate.swift**, ekleme *willPresent* ve *didReceive* **UNUserNotificationCenterDelegate** yaramaz Uygulama ön plan ve arka plan sırasıyla çalışırken bildirim gelen dolduğunda bir uyarı görüntüle
    
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

14. Yazdırma deyimleri bölmenizin altına eklemek *didRegisterForRemoteNotificationsWithDeviceToken* doğrulamak için işlevi *InstallationID* ve *pushChannel* yükleniyor atanan değerler
15. Klasör oluşturma (**modelleri**, **Hizmetleri**, ve **yardımcı programları**) eklediğiniz projeye daha sonra temel bileşenleri
16. Projeyi oluşturan ve çalıştıran fiziksel bir cihazda (anında iletme bildirimleri, simülatör'ü kullanarak sınanamıyor) denetleyin

### <a name="create-models"></a>Model oluşturma
Bu adımda, bir dizi temsil etmek için modelleri oluşturacaksınız [Notification Hubs REST API'si](/rest/api/notificationhubs/) yükler ve gerekli depolamak için **SAS belirteci** veri.


1.  Yeni bir swift dosyasına eklemek **modelleri** adlı **PushTemplate.swift**. Bu modeli temsil eden yapısı sağlar **GÖVDESİ** bireysel şablonunun bir parçası olarak **DeviceInstallation** yükü:
    
    ```swift
    import Foundation

    struct PushTemplate : Codable {
        let body : String
    
        init(withBody body : String) {
            self.body = body
        }
    }
    ```

2. Yeni bir swift dosyasına eklemek **modelleri** adlı klasöre **DeviceInstallation.swift**. Bu dosya, oluşturmak veya güncelleştirmek için yükü temsil eden bir yapı tanımlar. bir **cihaz yüklemesi**. Dosyaya aşağıdaki kodu ekleyin:
    
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

3.  Altında yeni bir swift dosya ekleme **modelleri** adlı **TokenData.swift**. Bu model depolamak için kullanılan bir **SAS belirteci** ermesinden yanı sıra

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
**Bildirim hub'ları** aynı güvenlik altyapısı olarak kullanma **Azure Service Bus**. REST API'sini çağırmak için yapmanız gerekir [program aracılığıyla SAS belirteci oluşturmak](/rest/api/eventhub/generate-sas-token) içinde kullanılabilir **yetkilendirme** isteği üstbilgisi.  

Elde edilen belirteç şu biçimde olacaktır: 

```xml
SharedAccessSignature sig=<UrlEncodedSignature>&se=<ExpiryEpoch>&skn=<KeyName>&sr=<UrlEncodedResourceUri>
```

İşlemin kendisi altı aynı temel adımları kapsar:  

1.  Süre sonu içinde bilgi işlem [UNIX dönem zamanı](https://en.wikipedia.org/wiki/Unix_time) biçimi (00:00:00 UTC 1 Ocak 1970'ten beri geçen saniye)
2.  Biçimlendirme **ResourceUrl** (çalıştığınız erişmek için diğer bir deyişle, kaynağını temsil eden `'https://\<namespace\>.servicebus.windows.net/\<hubName\>'`) yüzde olarak kodlanmış ve küçük olacak şekilde
3.  Hazırlama **StringToSign**, hangi, oluşur `'\<**UrlEncodedResourceUrl**\>\n\<**ExpiryEpoch**\>'`
4.  Bilgi işlem (ve taban 64 kodlaması) **imza** kullanarak **HMAC SHA256** , **StringToSign** değerini **anahtar** parçası **Bağlantı dizesi** (ilgili için **yetkilendirme kuralı**)
5.  Base 64 kodlamalı biçimlendirme **imza** , bu nedenle yüzde olarak kodlanmış
6.  Oluşturma **belirteci** beklenen biçimi kullanarak **UrlEncodedSignature**, **ExpiryEpoch**, **KeyName**ve **UrlEncodedResourceUrl** değerleri

Bkz: [Azure Service Bus belgeleri](../service-bus-messaging/service-bus-sas.md) ilişkin daha kapsamlı bir genel bakış **paylaşılan erişim imzası** ve tarafından nasıl kullanıldıkları **Azure Service Bus** ve  **Bildirim hub'ları**.

Swift bu örneğin amacı doğrultusunda, Apple'nın açık kaynak olacak **CommonCrypto** karma imzası ile yardımcı olmak için kitaplığı. Bir C Kitaplığı olduğu gibi Swift,-hazır erişilebilir değil. Ancak, bu bir köprü oluşturma üst bilgisi kullanarak kullanılabilir hale getirebilirsiniz. Ekleme ve köprü oluşturma üst bilgi yapılandırmak için:

1. İçinde **Xcode**Git **dosya**, ardından **yeni**, ardından **dosya** seçip **üstbilgi dosyası** adlandırma *'BridgingHeader.h'*
2. İçeri aktarma dosyasını düzenleyerek **CommonHMAC**

    ```swift
    #import <CommonCrypto/CommonHMAC.h>

    #ifndef BridgingHeader_h
    #define BridgingHeader_h


    #endif /* BridgingHeader_h */
    ```

3. Hedefin güncelleştirme **Build Settings** köprü oluşturma üst bilgisi başvurmak için. Açık **yapı ayarları** sekmesini ve ekranı aşağı kaydırarak **Swift derleyicisi** bölümü. Emin **yükleme Objective-C uyumluluğu üstbilgi** seçeneğini ayarlamak **Evet** ve bizim köprü oluşturma üst bilgisi dosya yolunu girin **Objective-C köprü oluşturma üst bilgisi**   diğer bir deyişle seçeneği `'\<ProjectName\>/BridgingHeader.h'`. Bu seçenekler bulamazsanız, olduğundan emin olun **tüm** seçili görünüm (yerine **temel** veya **özelleştirilmiş**).
    
   Hangi kullanarak yapabileceğiniz birçok üçüncü taraf açık kaynak kapsayıcı kitaplık kullanılabilir **CommonCrypto** biraz daha kolay. Bu makalenin kapsamı dışındadır var.

4. Altında yeni bir Swift dosya ekleme **yardımcı programları** adlı bir klasör **TokenUtility.swift** ve aşağıdaki kodu ekleyin:

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
    
    Bu yardımcı program üreten mantığı kapsülleyen **SAS belirteci**. *GetSasToken* işlevi daha önce belirtildiği gibi belirteç hazırlanmak için gereken üst düzey adımlar düzenler ve daha sonra Bu öğreticide adımları Yükleme hizmeti tarafından çağrılır. Diğer iki işlev çağıran *getSasToken işlevi*; *sha256HMac* imza bilgi işlem ve *urlEncodedString* ilgili Url dizesini kodlamak için. *UrlEncodedString* yerleşik kullanarak gerekli çıkış elde etmek mümkün değildi gibi işlev gerekli *addingPercentEncoding* işlevi. [Azure depolama iOS SDK'sı](https://github.com/Azure/azure-storage-ios/blob/master/Lib/Azure%20Storage%20Client%20Library/Azure%20Storage%20Client%20Library/AZSUtil.m) Objective-c ile birlikte, bu işlemler yaklaşımını nasıl mükemmel bir örnek olarak sunulur Daha fazla bilgi üzerinde **Azure Service Bus SAS belirteçlerini** bulunabilir [Azure Service Bus belgeleri](../service-bus-messaging/service-bus-sas.md). 

### <a name="verify-the-sas-token"></a>SAS belirteci doğrulayın
İstemci yüklemesi hizmeti uygulamadan önce uygulamamızı doğru oluşturduğunu denetleyebilirsiniz **SAS belirteci** seçtiğiniz, http yardımcı programını kullanarak. Bu gönderinin amacı doğrultusunda, tercih ettiğiniz aracımızı olacaktır **Postman**.

Bir Not *InstallationID* ve *belirteci* uygun şekilde yerleştirilen kullanarak uygulama tarafından oluşturulan değerleri yazdırma deyim ya da kesme noktası. 

Çağırmak için bu adımları *yüklemeleri* API:

1. İçinde **Postman**, yeni bir sekme açın
2. İstek kümesine **alma** ve aşağıdaki adresi:

    ```xml
    https://<namespace>.servicebus.windows.net/<hubName>/installations/<installationId>?api-version=2015-01
    ```

3. İstek üst bilgilerini aşağıdaki gibi yapılandırın:
    
   | Anahtar           | Değer            |
   | ------------- | ---------------- |
   | Content-Type  | uygulama/json |
   | Yetkilendirme | \<sasToken\>     |
   | x-ms-version  | 2015-01          |

4. Tıklayın **kod** düğmesine (sağ üst altında **Kaydet** düğmesi). İstek aşağıdaki örneğe benzer olmalıdır:

    ```html
    GET /<hubName>/installations/<installationId>?api-version=2015-01 HTTP/1.1
    Host: <namespace>.servicebus.windows.net
    Content-Type: application/json
    Authorization: <sasToken>
    x-ms-version: 2015-01
    Cache-Control: no-cache
    Postman-Token: <postmanToken>
    ```

5. Tıklayın **Gönder** düğmesi

Kayıt için belirtilen mevcut *InstallationID* bu noktada, ancak neden bir **404 Bulunamadı** yanıt yerine **401 Yetkisiz**. Bu sonuç, onaylamalıdır **SAS belirteci** kabul edildi.

### <a name="implement-the-installation-service-class"></a>Uygulama yükleme hizmet sınıfı
Bizim temel funcınner çevresindeki sonraki uygulayacaksınız [yüklemeleri REST API](/rest/api/notificationhubs/create-overwrite-installation).  

Altında yeni bir Swift dosya ekleme **Hizmetleri** adlı bir klasör **NotificationRegistrationService.swift**, ardından bu dosyaya aşağıdaki kodu ekleyin:

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
 
Önkoşul ayrıntıları başlatma bir parçası olarak sağlanır. Etiketler ve şablonlar isteğe bağlı olarak geçirildi *kaydetme* bir parçasını oluşturan işleve **cihaz yüklemesi** JSON yükü.  

*Kaydetme* işlevi istek hazırlamak için diğer özel işlevler için çağırır. Bir yanıt alındığında, kaydın başarılı olup olmadığını tamamlandığını gösteren çağrılır.  

İstek uç noktası tarafından oluşturulan *getBaseAddress* kullanarak bu işlevi **bildirim hub'ı** parametreleri **ad alanı** ve **adı**başlatma sırasında sağlanan.  

*GetSasToken* işlevi, şu anda saklı belirtecin geçerli olup olmadığını, aksi takdirde, duyurmak için denetleyecek **TokenUtility** yeni bir tane oluşturmanız ve bir değer döndürmeden önce depolamak için. 

Son olarak *encodeToJson* ilgili model nesnelerini JSON'a için istek gövdesi bir parçası olarak dönüştürür.

### <a name="invoke-the-notification-hubs-rest-api"></a>Notification hubs'ı REST API çağırma
Güncelleştirilecek son adımdır **AppDelegate** kullanılacak **NotifiationRegistrationService** kaydetmek için sunduğumuz **NotificationHub**. 

1. Açık **AppDelegate.swift** ve bir başvuru depolamak için bir sınıf düzeyi değişkenleri ekleyin **NoficiationRegistrationService**:

    ```swift
    var registrationService : NotificationRegistrationService?
    ```

2. Aynı dosyada, güncelleştirme *didRegisterForRemoteNotificationsWithDeviceToken* işlevi **NotificationRegistrationService** önkoşul parametrelerle ardındançağırın.*kaydetme* işlevi.

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
    
        // Call register passing in the tags and template parameters
        self.registrationService!.register(withTags: tags, andTemplates: ["genericTemplate" : self.genericTemplate]) { (result) in
            if !result {
                print("Registration issue")
            } else {
                print("Registered")
            }
        }
    }
    ```

    Basit tutmak için çıkış penceresine sonucuyla güncelleştirilecek birkaç yazdırma deyimleri kullanılacak gideceğinizi *kaydetme* işlemi. 

3. Şimdi oluşturun ve uygulamayı (bir fiziksel cihaz üzerinde) çalıştırın. Görmelisiniz **"Registered"** çıktı penceresinde.

## <a name="test-the-solution"></a>Çözüm test
Bu aşamada, uygulamamız ile kayıtlı **NotificationHub** ve anında iletme bildirimleri alabilirsiniz. İçinde **Xcode**, hata ayıklayıcıyı durdurun ve (şu anda çalışmıyorsa) uygulamayı kapatın. Ardından, kontrol **cihaz yüklemesi** anında iletme bildirimleri ayrıntıları beklendiği gibi ve uygulamamız gerçekten de artık alabilir.  

### <a name="verify-the-device-installation"></a>Cihaz yüklemesini doğrulama
Daha önce kullanarak yaptığınız gibi artık aynı istekte bulunabilirsiniz **Postman** için [SAS belirteci doğrulama](#verify-the-sas-token). Varsayarak **SAS belirteci** henüz süresi dolduğunda, yanıt artık etiketler ve şablonlar gibi sağlanan yükleme ayrıntıları içermelidir.  

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
Gitmek için bildirimler almak, test etmek için en hızlı yolu olan **bildirim hub'ı** içinde **Azure portalında**.

1. İçinde **Azure portalında**, gitmek **genel bakış** sekmesinde, **bildirim hub'ı**
2. Tıklayın **Test gönderimi** (sol üst) yukarıda **Essentials** özeti

    ![Bildirim hub'ları Essentials Özet sınama Gönder düğmesi](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-essentials-test-send.png)
3. Seçin **özel şablonu** listesinden **platformları**
4. Girin **12345** için **etiket ifadesine Gönder** (Bu etiket bizim yüklemesinde belirttiğiniz)
5. İsteğe bağlı olarak, Düzen **ileti** JSON yükündeki
    
    ![Bildirim hub'ları Test gönderimi](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-test-send.png)
6. Tıklayın **Gönder** ve portal bildirimi cihaza başarıyla gönderilip gönderilmediğini belirtmelidir

    ![Bildirim hub'ları Test sonuçları gönder](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-test-send-result.png)

    Ayrıca, bir bildirim görmeniz gerekir **bildirim Merkezi** Cihazınızda (uygulama ön planda çalışmayan varsayılarak). Bildirim dokunarak uygulamayı açıp uyarıyı gösterir.

    ![Örnek bildirim aldı](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/test-send-notification-received.png)

### <a name="send-a-test-notification-postman"></a>(Postman) test bildirimi gönder
İlgili'aracılığıyla bildirim gönderebilen [REST API](/rest/api/notificationhubs/) aracılığıyla **Postman** gibi iyi ve test etmek için daha kullanışlı bir yol olabilir. 

1. İçinde **Postman**, yeni bir sekme açın
2. İstek kümesine **POST** ve aşağıdaki adresi girin:

    ```xml
    https://<namespace>.servicebus.windows.net/<hubName>/messages/?api-version=2015-01
    ```

3. İstek üst bilgilerini aşağıdaki gibi yapılandırın:
    
   | Anahtar                            | Değer                          |
   | ------------------------------ | ------------------------------ |
   | Content-Type                   | Uygulama/json; charset = utf-8 |
   | Yetkilendirme                  | \<sasToken\>                   |
   | ServiceBusNotification-Format  | şablon                       |
   | Tags                           | "12345"                        |

4. Yapılandırma isteğini **GÖVDESİ** kullanılacak **RAW - JSON (application.json)** aşağıdaki JSON yükü ile:

    ```json
    {
       "message" : "Hello from Postman!"
    }
    ```

5. Tıklayın **kod** düğmesine (sağ üst altında **Kaydet** düğmesi). İstek aşağıdaki örneğe benzer olmalıdır:

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

5. Tıklayın **Gönder** düğmesi

Başarılı durum kodu alın ve istemci cihazda bildirim almak gerekir.

## <a name="next-steps"></a>Sonraki adımlar
Bağlı bir temel iOS Swift uygulaması artık sahip bir **bildirim hub'ı** aracılığıyla [REST API](/rest/api/notificationhubs/) ve gönderebilir ve bildirimleri alabilirsiniz. Daha fazla bilgi için aşağıdaki makalelere bakın: 

- [Azure Notification Hubs'a genel bakış](notification-hubs-push-notification-overview.md)
- [Notification hubs'ı REST API'leri](/rest/api/notificationhubs/)
- [Arka uç işlemleri için Notification Hubs SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)
- [Notification hubs'ın GitHub üzerinde SDK](https://github.com/Azure/azure-notificationhubs)
- [Uygulama arka ucuna kaydetme](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)
- [Kayıt Yönetimi](notification-hubs-push-notification-registration-management.md)
- [Etiketleri ile çalışma](notification-hubs-tags-segment-push-message.md) 
- [Özel şablonları ile çalışma](notification-hubs-templates-cross-platform-push-messages.md)
- [Paylaşılan erişim imzaları ile Service Bus erişim denetimi](../service-bus-messaging/service-bus-sas.md)
- [Program aracılığıyla SAS belirteçleri oluşturmak](/rest/api/eventhub/generate-sas-token)
- [Apple güvenlik: ortak şifreleme](https://developer.apple.com/security/)
- [UNIX dönem zamanı](https://en.wikipedia.org/wiki/Unix_time)
- [HMAC](https://en.wikipedia.org/wiki/HMAC)
