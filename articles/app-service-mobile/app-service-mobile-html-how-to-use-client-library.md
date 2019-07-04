---
title: Azure mobil uygulamaları için JavaScript SDK'sını kullanma
description: Nasıl Azure Mobile Apps v kullanılacak
services: app-service\mobile
documentationcenter: javascript
author: elamalani
manager: crdun
editor: ''
ms.assetid: 53b78965-caa3-4b22-bb67-5bd5c19d03c4
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: html
ms.devlang: javascript
ms.topic: article
ms.date: 06/25/2019
ms.author: emalani
ms.openlocfilehash: d5aa2e326739a97ff3d518ec383f4cf14311ca74
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446344"
---
# <a name="how-to-use-the-javascript-client-library-for-azure-mobile-apps"></a>Azure Mobile Apps için JavaScript istemci kitaplığını kullanma
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

> [!NOTE]
> Visual Studio App Center, mobil uygulama geliştirme merkezi hizmetlerinde yeni ve tümleşik yatırım yapıyor. Geliştiriciler **derleme**, **Test** ve **Dağıt** hizmetlerinin sürekli tümleştirme ve teslim işlem hattı ayarlayın. Uygulama dağıtıldığında, geliştiriciler kendi uygulamasını kullanarak kullanımı ve durumu izleyebilirsiniz **Analytics** ve **tanılama** kullanarak kullanıcılarla etkileşim kurun ve hizmetlerini **anında iletme** hizmeti. Geliştiriciler de yararlanabilir **Auth** , kullanıcıların kimliğini doğrulamak ve **veri** kalıcı hale getirmek ve uygulama verilerini bulutta eşitleme hizmeti. Kullanıma [App Center](https://appcenter.ms/?utm_source=zumo&utm_campaign=app-service-mobile-html-how-to-use-client-library) bugün.
>

## <a name="overview"></a>Genel Bakış
Bu kılavuzu kullanarak en yaygın senaryoları gerçekleştirmek için öğretir [Azure mobil uygulamaları için JavaScript SDK'sı]. Azure Mobile Apps için yeni başladıysanız, ilk olarak işlemini [Azure Mobile Apps Hızlı Başlangıç] bir arka uç oluşturma ve bir tablo oluşturun. Bu kılavuzda, HTML/JavaScript Web uygulamaları kullanarak mobil arka uç odaklanın.

## <a name="supported-platforms"></a>Desteklenen platformlar
Bilinen tarayıcılar geçerli ve son sürümleri için tarayıcı desteği sınırlandırırız:  Google Chrome, Microsoft Edge, Internet Explorer ve Mozilla Firefox.  SDK'sı göreceli olarak modern bir tarayıcı ile işlevine bekliyoruz.

Paket, globals, AMD, destekler ve CommonJS biçimleri Evrensel JavaScript modül olarak dağıtılır.

## <a name="Setup"></a>Kurulum ve Önkoşullar
Bu kılavuz, bir arka ucu içeren bir tablo oluşturdunuz varsayılır. Bu kılavuz, tablo bu öğreticilerde tabloların aynı şemaya sahip olduğunu varsayar.

Aracılığıyla Azure Mobile Apps JavaScript SDK'sını yükleme yapılabilir `npm` komutu:

```
npm install azure-mobile-apps-client --save
```

Kitaplık Browserify ve Web gibi ve AMD kitaplığı olarak CommonJS ortamlar içinde bir ES2015 modülü olarak da kullanılabilir.  Örneğin:

```javascript
// For ECMAScript 5.1 CommonJS
var WindowsAzure = require('azure-mobile-apps-client');
// For ES2015 modules
import * as WindowsAzure from 'azure-mobile-apps-client';
```

Bizim CDN'den doğrudan indirerek, önceden oluşturulmuş bir SDK sürümünü de kullanabilirsiniz:

```html
<script src="https://zumo.blob.core.windows.net/sdk/azure-mobile-apps-client.min.js"></script>
```

[!INCLUDE [app-service-mobile-html-js-library](../../includes/app-service-mobile-html-js-library.md)]

## <a name="auth"></a>Nasıl Yapılır: Kullanıcıların kimliklerini doğrulama
Azure App Service kimlik doğrulama ve yetkilendirme çeşitli dış kimlik sağlayıcısı kullanarak uygulama kullanıcılarının destekler: Facebook, Google, Microsoft hesabı ve Twitter gibi. Tablolarda yalnızca kimliği doğrulanmış kullanıcılar için belirli işlemler için erişimi sınırlandırmak için izinleri ayarlayabilirsiniz. Sunucu betiklerini yetkilendirme kurallarını uygulamak için kimliği doğrulanmış kullanıcıların kimliğini de kullanabilirsiniz. Daha fazla bilgi için [kimlik doğrulamayı kullanmaya başlama] öğretici.

İki kimlik doğrulama akışı desteklenir: sunucu akış ve istemci akışı.  Sağlayıcının web kimlik doğrulaması arabirimde olmasına olduğundan sunucu akışı Basit kimlik doğrulaması deneyimi sağlar. Gibi istemci akışı için cihaza özgü özellikleri ile daha derin tümleştirme sağlayan çoklu oturum açma olarak, sağlayıcıya özgü SDK'ları üzerinde kullanır.

[!INCLUDE [app-service-mobile-html-js-auth-library](../../includes/app-service-mobile-html-js-auth-library.md)]

### <a name="configure-external-redirect-urls"></a>Nasıl Yapılır: Mobil uygulama hizmetiniz için dış yönlendirme URL'leri yapılandırın.
Çeşitli JavaScript uygulamaları, OAuth UI akışları işlemek için bir geri döngü olanağını kullanın.  Bu özellikler şunları içerir:

* Hizmetinizi yerel olarak çalıştırma
* Canlı yeniden yükleme Ionic Framework ile kullanma
* App Service kimlik doğrulaması için yönlendirme.

Varsayılan olarak, App Service kimlik doğrulaması yalnızca Mobile App arka ucunuzu erişime izin verecek şekilde yapılandırıldığından, yerel olarak çalışan sorunlara neden olabilir. Sunucu yerel olarak çalıştırırken kimlik doğrulamasını etkinleştirmek için App Service ayarlarını değiştirmek için aşağıdaki adımları kullanın:

1. [Azure portal]’nda oturum açın
2. Mobil uygulama arka ucunuza gidin.
3. Seçin **kaynak Gezgini** içinde **geliştirme araçları** menüsü.
4. Tıklayın **Git** bir yeni bir sekme veya pencerede Mobile App arka ucunuzu için kaynak gezginini açın.
5. Genişletin **config** > **authsettings** uygulamanız için düğüm.
6. Tıklayın **Düzenle** kaynağın düzenlemeyi etkinleştirmek için düğme.
7. Bulma **allowedExternalRedirectUrls** öğesi null olmalıdır. URL'nizde bir dizi içinde ekleyin:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    Olan bu örnekte hizmetinizi URL'lerle dizi URL'lerinde Değiştir `http://localhost:3000` yerel Node.js örnek hizmeti. Ayrıca kullanabileceğinizi `http://localhost:4400` Ripple hizmet veya uygulamanızı nasıl yapılandırıldığına bağlı olarak bazı diğer URL'si.
8. Sayfanın üst kısmında tıklayın **okuma/yazma**, ardından **PUT** güncelleştirmelerinizi kaydedin.

Ayrıca aynı geri döngü URL'leri CORS beyaz liste ayarlarına eklemeniz gerekir:

1. Geri gidin [Azure portal].
2. Mobil uygulama arka ucunuza gidin.
3. Tıklayın **CORS** içinde **API** menüsü.
4. Her URL boş girin **izin verilen çıkış noktaları** metin kutusu.  Yeni bir metin kutusu oluşturulur.
5. Tıklayın **Kaydet**

Arka uç güncelleştirildikten sonra uygulamanızda yeni geri döngü URL'lerini kullanmanız mümkün olacaktır.

<!-- URLs. -->
[Azure Mobile Apps hızlı başlangıç]: app-service-mobile-cordova-get-started.md
[Kimlik doğrulamayı kullanmaya başlama]: app-service-mobile-cordova-get-started-users.md
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

[Azure portal]: https://portal.azure.com/
[Azure mobil uygulamaları için JavaScript SDK'sı]: https://www.npmjs.com/package/azure-mobile-apps-client
[Query object documentation]: https://msdn.microsoft.com/library/azure/jj613353.aspx
