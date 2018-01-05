---
title: "Apache Cordova eklentisi için Azure Mobile Apps kullanma"
description: "Apache Cordova eklentisi için Azure Mobile Apps kullanma"
services: app-service\mobile
documentationcenter: javascript
author: conceptdev
manager: crdun
editor: 
ms.assetid: a56a1ce4-de0c-4f3c-8763-66252c52aa59
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: crdun
ms.openlocfilehash: f166d2e533dc49ca7779b45f3dec57a53c22fc40
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2018
---
# <a name="how-to-use-apache-cordova-client-library-for-azure-mobile-apps"></a>Azure Mobile Apps için Apache Cordova istemci kitaplığını kullanma
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

En son kullanılarak yaygın senaryolar gerçekleştirmek için bu kılavuzu öğretilmektedir [Azure Mobile Apps için Apache Cordova eklentisi]. Azure Mobile Apps yeniyseniz, ilk tamamlamak [Azure Mobile Apps Hızlı Başlangıç] bir arka uç oluşturmak için bir tablo oluşturun ve önceden oluşturulmuş bir Apache Cordova projenizi indirin. Bu kılavuzda, istemci-tarafı Apache Cordova eklentisi üzerinde odaklanın.

## <a name="supported-platforms"></a>Desteklenen platformlar
Apache Cordova v6.0.0 ve daha sonra iOS, Android ve Windows bu SDK destekleyen cihazlar.  Platform desteği aşağıdaki gibidir:

* Android API 19-24 (KitKat Nougat aracılığıyla).
* iOS 8.0 ve sonraki sürümleri.
* Windows Phone 8.1.
* Evrensel Windows platformu.

## <a name="Setup"></a>Kurulum ve Önkoşullar
Bu kılavuz, bir tablo ile bir arka uç oluşturduğunuzu varsayar. Bu kılavuz tablo bu öğreticiler aynı şemaya tabloları sahip olduğunu varsayar. Bu kılavuz ayrıca kodunuz için Apache Cordova eklentisi eklediğiniz varsayar.  Bunu yapmadıysanız komut satırında projenize Apache Cordova eklentisi ekleyebilirsiniz:

```
cordova plugin add cordova-plugin-ms-azure-mobile-apps
```

Oluşturma hakkında daha fazla bilgi için [ilk Apache Cordova uygulamanızı], kendi belgelerine bakın.

## <a name="ionic"></a>Ionic v2 uygulama ayarlama

Ionic v2 projesinde düzgün bir şekilde yapılandırmak için önce temel bir uygulama oluşturun ve Cordova eklentisi ekleyin:

```
ionic start projectName --v2
cd projectName
ionic plugin add cordova-plugin-ms-azure-mobile-apps
```

Aşağıdaki satırları ekleyin `app.component.ts` istemci nesnesi oluşturmak için:

```
declare var WindowsAzure: any;
var client = new WindowsAzure.MobileServiceClient("https://yoursite.azurewebsites.net");
```

Şimdi oluşturun ve projeyi tarayıcıda çalıştırın:

```
ionic platform add browser
ionic run browser
```

Azure Mobile Apps Cordova eklentisi her iki Ionic v1 ve v2 uygulamaları destekler.  Ionic v2 uygulamalar için ek bildirim gerektiriyor yalnızca `WindowsAzure` nesnesi.

[!INCLUDE [app-service-mobile-html-js-library.md](../../includes/app-service-mobile-html-js-library.md)]

## <a name="auth"></a>Nasıl yapılır: kullanıcıların kimlik doğrulaması
Azure uygulama hizmeti, kimlik doğrulaması ve çeşitli dış kimlik sağlayıcılarını kullanarak uygulama kullanıcıları yetkilendirmek destekler: Facebook, Google, Microsoft Account ve Twitter. Yalnızca kimliği doğrulanmış kullanıcılar için belirli işlemler için erişimi kısıtlamak için tablolarda izinlerini ayarlayabilirsiniz. Kimliği doğrulanmış kullanıcıların kimliğini, sunucu komut dosyalarında yetkilendirme kuralları uygulamak için de kullanabilirsiniz. Daha fazla bilgi için bkz: [kimlik doğrulamayı kullanmaya başlama] Öğreticisi.

Kimlik doğrulaması bir Apache Cordova uygulaması kullanırken, aşağıdaki Cordova eklenti kullanılabilir olması gerekir:

* [cordova-plugin-device]
* [cordova eklentisi inappbrowser]

İki kimlik doğrulama akışı desteklenir: sunucu akışı ve bir istemci akışı.  Sağlayıcının web kimlik doğrulaması arabirimde alacağından sunucu akış Basit kimlik doğrulama deneyimi sağlar. Gibi istemci akışı aygıta özgü özellikleri ile daha derin tümleştirme sağlar çoklu oturum açma sağlayıcıya özgü aygıta özgü SDK'ları üzerinde alacağından.

[!INCLUDE [app-service-mobile-html-js-auth-library.md](../../includes/app-service-mobile-html-js-auth-library.md)]

### <a name="configure-external-redirect-urls"></a>Nasıl yapılır: Mobil uygulama hizmetiniz için dış yönlendirme URL'lerini yapılandırın.
Apache Cordova uygulamaları çeşitli OAuth UI akışları işlemek için bir geri döngü özelliği kullanır.  Kimlik doğrulama hizmeti varsayılan olarak, hizmeti kullanmaya nasıl yalnızca bilir beri OAuth UI akışları localhost üzerinde sorunlara neden olur.  Sorunlu OAuth UI akışları örnekleri şunlardır:

* Ripple öykünücüsü.
* Yeniden Ionic ile canlı.
* Mobil arka uç yerel olarak çalıştırma
* Mobil arka uç bir sağlayan kimlik doğrulama'den farklı bir Azure uygulama hizmeti çalışıyor.

Yerel ayarlarınızı yapılandırmaya eklemek için bu yönergeleri izleyin:

1. [Azure portalı]’nda oturum açın
2. Seçin **tüm kaynakları** veya **uygulama hizmetleri** mobil uygulamanızın adına tıklayın.
3. Tıklatın **araçları**
4. Tıklatın **kaynak Gezgini** GÖZLEMLE menüde ardından **Git**.  Yeni bir pencere veya sekmesinde açar.
5. Genişletme **config**, **authsettings** düğümleri için sol gezinti sitenizdeki.
6. Tıklatın **Düzenle**
7. "AllowedExternalRedirectUrls" öğesini arayın.  Null ya da bir dizi değere ayarlanabilir.  Değer şu değere değiştirin:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    URL'leri hizmetinizi URL'ler ile değiştirin.  (İçin Node.js örnek hizmeti) "http://localhost: 3000" veya "http://localhost:4400" (Ripple hizmeti için) örnek olarak verilebilir.  Ancak, bu URL'ler örnekler - örneklerde, belirtilen Hizmetleri dahil olmak üzere durumunuz farklı değildir.
8. Tıklatın **okuma/yazma** ekranın sağ üst köşesindeki düğmesi.
9. Yeşil tıklatın **PUT** düğmesi.

Bu noktada kaydedilmiş ayarları.  Ayarları bitinceye kadar tarayıcı penceresini kapatmayın kaydediliyor.
Ayrıca bu geri döngü URL'leri uygulama hizmetiniz için CORS ayarları ekleyin:

1. [Azure portalı]’nda oturum açın
2. Seçin **tüm kaynakları** veya **uygulama hizmetleri** mobil uygulamanızın adına tıklayın.
3. Ayarlar dikey penceresi otomatik olarak açılır.  ' I içermiyorsa tıklatırsanız **tüm ayarları**.
4. Tıklatın **CORS** API menüsünün altında.
5. Enter kutusunda eklemek istediğiniz URL sağlanan ve Enter tuşuna basın.
6. Gerektiğinde ek URL'leri girin.
7. Tıklatın **kaydetmek** ayarları kaydetmek için.

Yaklaşık olarak 10-15 etkili saniye yeni ayarları alır.

## <a name="register-for-push"></a>Nasıl yapılır: anında iletme bildirimleri için
Yükleme [phonegap eklentiyi itme] anında iletme bildirimleri işlemek için.  Bu eklenti kullanarak kolayca eklenebilen `cordova plugin add` komut satırında veya Visual Studio içinde Git eklentisi yükleyici aracılığıyla komutu.  Apache Cordova uygulamanızı aşağıdaki kodda Cihazınızı anında iletme bildirimleri için kaydeder:

```
var pushOptions = {
    android: {
        senderId: '<from-gcm-console>'
    },
    ios: {
        alert: true,
        badge: true,
        sound: true
    },
    windows: {
    }
};
pushHandler = PushNotification.init(pushOptions);

pushHandler.on('registration', function (data) {
    registrationId = data.registrationId;
    // For cross-platform, you can use the device plugin to determine the device
    // Best is to use device.platform
    var name = 'gcm'; // For android - default
    if (device.platform.toLowerCase() === 'ios')
        name = 'apns';
    if (device.platform.toLowerCase().substring(0, 3) === 'win')
        name = 'wns';
    client.push.register(name, registrationId);
});

pushHandler.on('notification', function (data) {
    // data is an object and is whatever is sent by the PNS - check the format
    // for your particular PNS
});

pushHandler.on('error', function (error) {
    // Handle errors
});
```

Sunucudan anında iletme bildirimleri göndermek için Notification Hubs SDK'sı kullanın.  Hiçbir zaman doğrudan istemcilerden anında iletme bildirimleri gönderme. Bunun yapılması bir hizmet reddi saldırısına karşı bildirim hub'ları veya PNS tetiklemek için kullanılabilir.  PNS trafiğinizi bu tür saldırıları sonucunda bSunucu.

## <a name="more-information"></a>Daha fazla bilgi

Ayrıntılı API ayrıntılarda bulabilirsiniz bizim [API belgelerine](http://azure.github.io/azure-mobile-apps-js-client/).

<!-- URLs. -->
[Azure portalı]: https://portal.azure.com
[Azure Mobile Apps Hızlı Başlangıç]: app-service-mobile-cordova-get-started.md
[kimlik doğrulamayı kullanmaya başlama]: app-service-mobile-cordova-get-started-users.md
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

[Azure Mobile Apps için Apache Cordova eklentisi]: https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-apps
[ilk Apache Cordova uygulamanızı]: http://cordova.apache.org/#getstarted
[phonegap-facebook-plugin]: https://github.com/wizcorp/phonegap-facebook-plugin
[phonegap eklentiyi itme]: https://www.npmjs.com/package/phonegap-plugin-push
[cordova-plugin-device]: https://www.npmjs.com/package/cordova-plugin-device
[cordova eklentisi inappbrowser]: https://www.npmjs.com/package/cordova-plugin-inappbrowser
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx
