---
title: Apache Cordova eklentisi için Azure Mobile Apps kullanma
description: Apache Cordova eklentisi için Azure Mobile Apps kullanma
services: app-service\mobile
documentationcenter: javascript
author: conceptdev
manager: crdun
editor: ''
ms.assetid: a56a1ce4-de0c-4f3c-8763-66252c52aa59
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: crdun
ms.openlocfilehash: 3c22aab20a9260bfd21869f0b327211e2f3d8894
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62109585"
---
# <a name="how-to-use-apache-cordova-client-library-for-azure-mobile-apps"></a>Azure Mobile Apps için Apache Cordova istemci kitaplığını kullanma
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Bu kılavuzu kullanarak en yaygın senaryoları gerçekleştirmek için öğretir [Azure mobil uygulamalar için Apache Cordova eklentisi]. Azure Mobile Apps için yeni başladıysanız, ilk olarak işlemini [Azure Mobile Apps Hızlı Başlangıç] bir arka ucu oluşturmak için tablo oluşturma ve önceden oluşturulmuş bir Apache Cordova projenizi indirin. Bu kılavuzda, biz üzerinde istemci tarafı Apache Cordova eklentisi odaklanın.

## <a name="supported-platforms"></a>Desteklenen platformlar
Apache Cordova v6.0.0 ve daha sonra iOS, Android ve Windows bu SDK'yı destekleyen cihazlar.  Platform desteği aşağıdaki gibidir:

* Android API 19-24 (KitKat Nougat aracılığıyla).
* iOS 8.0 ve üzeri sürümler.
* Windows Phone 8.1.
* Evrensel Windows platformu.

## <a name="Setup"></a>Kurulum ve Önkoşullar
Bu kılavuz, bir arka ucu içeren bir tablo oluşturdunuz varsayılır. Bu kılavuz, tablo bu öğreticilerde tabloların aynı şemaya sahip olduğunu varsayar. Bu kılavuz ayrıca kodunuza Apache Cordova eklentisi eklediğinizi varsayar.  Bunu yapmadıysanız, Apache Cordova eklentisi komut satırında projenize ekleyebilirsiniz:

```
cordova plugin add cordova-plugin-ms-azure-mobile-apps
```

Oluşturma hakkında daha fazla bilgi için [ilk Apache Cordova uygulamanızı], kendi belgelerine bakın.

## <a name="ionic"></a>Ionic v2 uygulama ayarlama

Ionic v2 proje düzgün bir şekilde yapılandırmak için ilk temel bir uygulama oluşturun ve Cordova eklentisini ekleyin:

```
ionic start projectName --v2
cd projectName
ionic plugin add cordova-plugin-ms-azure-mobile-apps
```

Aşağıdaki satırları ekleyin `app.component.ts` istemci nesnesini oluşturmak için:

```typescript
declare var WindowsAzure: any;
var client = new WindowsAzure.MobileServiceClient("https://yoursite.azurewebsites.net");
```

Artık, derleme ve proje tarayıcıda çalıştırın:

```
ionic platform add browser
ionic run browser
```

Azure Mobile Apps Cordova eklentisini hem Ionic v1 ve v2 uygulamalarını destekler.  Ionic v2 uygulamalar için ek bildirim gerektiriyor. yalnızca `WindowsAzure` nesne.

[!INCLUDE [app-service-mobile-html-js-library.md](../../includes/app-service-mobile-html-js-library.md)]

## <a name="auth"></a>Nasıl Yapılır: Kullanıcıların kimliklerini doğrulama
Azure App Service kimlik doğrulama ve yetkilendirme çeşitli dış kimlik sağlayıcısı kullanarak uygulama kullanıcılarının destekler: Facebook, Google, Microsoft hesabı ve Twitter gibi. Tablolarda yalnızca kimliği doğrulanmış kullanıcılar için belirli işlemler için erişimi sınırlandırmak için izinleri ayarlayabilirsiniz. Sunucu betiklerini yetkilendirme kurallarını uygulamak için kimliği doğrulanmış kullanıcıların kimliğini de kullanabilirsiniz. Daha fazla bilgi için [kimlik doğrulamayı kullanmaya başlama] öğretici.

Bir Apache Cordova uygulamasında kimlik doğrulaması kullanırken, aşağıdaki Cordova eklentileri kullanılabilir olmalıdır:

* [cordova-plugin-device]
* [cordova-plugin-ınappbrowser'a]

İki kimlik doğrulama akışı desteklenir: sunucu akış ve istemci akışı.  Sağlayıcının web kimlik doğrulaması arabirimde olmasına olduğundan sunucu akışı Basit kimlik doğrulaması deneyimi sağlar. Gibi istemci akışı için cihaza özgü özellikleri ile daha derin tümleştirme sağlayan çoklu oturum açma olarak, sağlayıcıya özgü cihaza özgü dillerde kullanır.

[!INCLUDE [app-service-mobile-html-js-auth-library.md](../../includes/app-service-mobile-html-js-auth-library.md)]

### <a name="configure-external-redirect-urls"></a>Nasıl Yapılır: Mobil uygulama hizmetiniz için dış yönlendirme URL'leri yapılandırın.
Birden fazla Apache Cordova uygulamaları, OAuth UI akışları işlemek için bir geri döngü olanağını kullanın.  Kimlik doğrulama hizmeti yalnızca hizmetiniz varsayılan olarak nasıl bilir beri localhost OAuth UI akışlarda sorunlara neden.  Sorunlu OAuth UI akışlar örnekleri şunlardır:

* Ripple öykünücüsüne.
* Canlı yeniden yükleme Ionic ile.
* Mobil arka uç yerel olarak çalıştırma
* Mobil arka uç sağlayan bir kimlik doğrulaması değerinden farklı bir Azure App Service'te çalışıyor.

Yerel ayarlarınızı yapılandırmaya eklemek için aşağıdaki yönergeleri izleyin:

1. [Azure portal]’nda oturum açın
2. Seçin **tüm kaynakları** veya **uygulama hizmetleri** ardından mobil uygulamanızın adına tıklayın.
3. Tıklayın **araçları**
4. Tıklayın **kaynak Gezgini** GÖZLEMLE menüde, ardından **Git**.  Yeni penceresi veya sekmesinde açar.
5. Genişletin **config**, **authsettings** sitenizin sol taraftaki gezinti düğümleri.
6. Tıklayın **Düzenle**
7. "AllowedExternalRedirectUrls" öğesini arayın.  Null ya da bir dizi değere ayarlanabilir.  Değeri şu değere değiştirin:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    URL'leri hizmetinizin URL ile değiştirin.  Örnekler `http://localhost:3000` (Node.js örnek hizmeti için) veya `http://localhost:4400` (Ripple hizmeti için).  Örnekleri - örneklerde sözü Hizmetleri dahil olmak üzere durumunuz farklı olabilir ancak, bu URL'ler uygulanır.
8. Tıklayın **okuma/yazma** ekranının sağ üst köşesindeki düğme.
9. Yeşil tıklayın **PUT** düğmesi.

Bu noktada ayarlar kaydedildi.  Ayarları bitinceye kadar tarayıcı penceresini kapatmayın kaydediliyor.
App Service CORS ayarlarını da bu döngü URL'leri ekleyin:

1. [Azure portal]’nda oturum açın
2. Seçin **tüm kaynakları** veya **uygulama hizmetleri** ardından mobil uygulamanızın adına tıklayın.
3. Ayarlar dikey penceresi otomatik olarak açılır.  Tıklayarak içermiyorsa **tüm ayarlar**.
4. Tıklayın **CORS** API menüsü altında.
5. Enter kutuya eklemek istediğiniz URL'yi sağlanan ve Enter tuşuna basın.
6. Gerektiğinde ek URL'leri girin.
7. Ayarları kaydetmek için **Kaydet**’e tıklayın.

Bu işlem yaklaşık 10-15 etkili olması için saniye yeni ayarların götürür.

## <a name="register-for-push"></a>Nasıl Yapılır: Anında iletme bildirimleri için kaydolun
Yükleme [modul phonegap plugin push] anında iletme bildirimleri işlemek için.  Bu eklentiyi kullanarak kolayca eklenebilir `cordova plugin add` komut satırında veya Visual Studio içinde Git eklentisi yükleyici aracılığıyla komutu.  Aşağıdaki kodda, Apache Cordova uygulamanızı anında iletme bildirimleri için Cihazınızı kaydeder:

```javascript
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

Sunucudan anında iletme bildirimleri göndermek için Notification Hubs SDK'sı kullanın.  Hiçbir zaman doğrudan istemcilerden anında iletme bildirimleri gönderin. Bunun yapılması bir bildirim hub'ları veya PNS karşı hizmet saldırısı reddi tetiklemek için kullanılabilir.  PNS trafiğinizi bu tür saldırıları sonucunda yasaklamak.

## <a name="more-information"></a>Daha fazla bilgi

Ayrıntılı API ayrıntıları bulabilirsiniz bizim [API belgeleri](https://azure.github.io/azure-mobile-apps-js-client/).

<!-- URLs. -->
[Azure portal]: https://portal.azure.com
[Azure Mobile Apps hızlı başlangıç]: app-service-mobile-cordova-get-started.md
[Kimlik doğrulamayı kullanmaya başlama]: app-service-mobile-cordova-get-started-users.md
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

[Azure mobil uygulamalar için Apache Cordova eklentisi]: https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-apps
[ilk Apache Cordova uygulamanızı]: https://cordova.apache.org/#getstarted
[phonegap-facebook-plugin]: https://github.com/wizcorp/phonegap-facebook-plugin
[modul phonegap plugin push]: https://www.npmjs.com/package/phonegap-plugin-push
[cordova-plugin-device]: https://www.npmjs.com/package/cordova-plugin-device
[cordova-plugin-ınappbrowser'a]: https://www.npmjs.com/package/cordova-plugin-inappbrowser
[Query object documentation]: https://msdn.microsoft.com/library/azure/jj613353.aspx
