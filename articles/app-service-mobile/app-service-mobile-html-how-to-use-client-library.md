---
title: "JavaScript SDK'sı için Azure Mobile Apps kullanma"
description: "Azure Mobile Apps v kullanılmak üzere nasıl"
services: app-service\mobile
documentationcenter: javascript
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 53b78965-caa3-4b22-bb67-5bd5c19d03c4
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: glenga
ms.openlocfilehash: 0c4b4de560d70592f5bbdee28b56a7686b5689f4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-the-javascript-client-library-for-azure-mobile-apps"></a>Azure Mobile Apps için JavaScript istemci kitaplığını kullanma
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

En son kullanılarak yaygın senaryolar gerçekleştirmek için bu kılavuzu öğretilmektedir [JavaScript SDK'sı Azure Mobile Apps için]. Azure Mobile Apps yeniyseniz, ilk tamamlamak [Azure Mobile Apps Hızlı Başlangıç] için bir arka uç ve bir tablo oluşturun. Bu kılavuzda, HTML/JavaScript Web uygulamaları kullanarak mobil arka uç odaklanın.

## <a name="supported-platforms"></a>Desteklenen platformlar
Hesabınıza başlıca tarayıcılar geçerli ve son sürümleri için tarayıcı desteği sınır: Google Chrome, Microsoft Edge, Microsoft Internet Explorer ve Mozilla Firefox.  Görece modern tarayıcılar ile çalışması için SDK'sı bekliyoruz.

Paket bir evrensel JavaScript modülü olarak CommonJS biçimleri ve genel öğeleri, AMD, destekler şekilde dağıtılır.

## <a name="Setup"></a>Kurulum ve Önkoşullar
Bu kılavuz, bir tablo ile bir arka uç oluşturduğunuzu varsayar. Bu kılavuz tablo bu öğreticiler aynı şemaya tabloları sahip olduğunu varsayar.

Aracılığıyla Azure Mobile Apps JavaScript SDK'sını yükleme yapılabilir `npm` komutu:

```
npm install azure-mobile-apps-client --save
```

Kitaplık CommonJS ortamlarda AMD kitaplığı olarak ve Browserify ve Webpack gibi bir ES2015 modül olarak da kullanılabilir.  Örneğin:

```
# For ECMAScript 5.1 CommonJS
var WindowsAzure = require('azure-mobile-apps-client');
# For ES2015 modules
import * as WindowsAzure from 'azure-mobile-apps-client';
```

Önceden oluşturulmuş bir SDK sürümü doğrudan sunduğumuz CDN yükleyerek de kullanabilirsiniz:

```html
<script src="https://zumo.blob.core.windows.net/sdk/azure-mobile-apps-client.min.js"></script>
```

[!INCLUDE [app-service-mobile-html-js-library](../../includes/app-service-mobile-html-js-library.md)]

## <a name="auth"></a>Nasıl yapılır: kullanıcıların kimlik doğrulaması
Azure uygulama hizmeti, kimlik doğrulaması ve çeşitli dış kimlik sağlayıcılarını kullanarak uygulama kullanıcıları yetkilendirmek destekler: Facebook, Google, Microsoft Account ve Twitter. Yalnızca kimliği doğrulanmış kullanıcılar için belirli işlemler için erişimi kısıtlamak için tablolarda izinlerini ayarlayabilirsiniz. Kimliği doğrulanmış kullanıcıların kimliğini, sunucu komut dosyalarında yetkilendirme kuralları uygulamak için de kullanabilirsiniz. Daha fazla bilgi için bkz: [kimlik doğrulamayı kullanmaya başlama] Öğreticisi.

İki kimlik doğrulama akışı desteklenir: sunucu akışı ve bir istemci akışı.  Sağlayıcının web kimlik doğrulaması arabirimde alacağından sunucu akış Basit kimlik doğrulama deneyimi sağlar. Gibi istemci akışı aygıta özgü özellikleri ile daha derin tümleştirme sağlar çoklu oturum açma sağlayıcıya özgü SDK'ları üzerinde alacağından.

[!INCLUDE [app-service-mobile-html-js-auth-library](../../includes/app-service-mobile-html-js-auth-library.md)]

### <a name="configure-external-redirect-urls"></a>Nasıl yapılır: Mobil uygulama hizmetiniz için dış yönlendirme URL'lerini yapılandırın.
Çeşitli JavaScript uygulamaları, OAuth UI akışları işlemek için bir geri döngü özelliği kullanır.  Bu özellikler şunları içerir:

* Hizmetinizi yerel olarak çalıştırma
* Dinamik yeniden Ionic Framework ile kullanma
* Uygulama hizmeti için kimlik doğrulaması için yönlendirme.

Varsayılan olarak, App Service kimlik doğrulaması yalnızca mobil uygulama arka erişime izin verecek şekilde yapılandırıldığından, yerel olarak çalışan sorunlara neden olabilir. Sunucu yerel olarak çalıştırırken kimlik doğrulamasını etkinleştirmek için uygulama hizmeti ayarlarını değiştirmek için aşağıdaki adımları kullanın:

1. [Azure portalı]’nda oturum açın
2. Mobil uygulama arka ucunuza gidin.
3. Seçin **kaynak Gezgini** içinde **geliştirme araçları** menüsü.
4. Tıklatın **Git** yeni sekmesinde veya penceresinde, mobil uygulama arka ucu için kaynak gezginini açın.
5. Genişletme **config** > **authsettings** düğümü, uygulamanız için.
6. Tıklatın **Düzenle** kaynak düzenleme etkinleştirmek için düğmeyi.
7. Bul **allowedExternalRedirectUrls** öğesi null olmalıdır. URL'nizde bir dizide ekleyin:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    Dizideki URL'leri olan bu örnekte hizmetinizi URL'ler ile Değiştir `http://localhost:3000` yerel Node.js örnek hizmeti. De kullanabilirsiniz `http://localhost:4400` Ripple hizmet veya uygulamanızı nasıl yapılandırıldığına bağlı olarak bazı diğer URL.
8. Sayfanın üstündeki **okuma/yazma**, ardından **PUT** güncelleştirmelerinizi kaydetmek için.

Ayrıca aynı geri döngü URL'leri CORS beyaz liste ayarlarına eklemeniz gerekir:

1. Geri gidin [Azure portalı].
2. Mobil uygulama arka ucunuza gidin.
3. Tıklatın **CORS** içinde **API** menüsü.
4. Her URL boş girin **izin verilen çıkış noktası** metin kutusu.  Yeni bir metin kutusu oluşturulur.
5. Tıklatın **Kaydet**

Arka uç güncelleştirildikten sonra uygulamanızda yeni geri döngü URL'ler kullanmanız mümkün olacaktır.

<!-- URLs. -->
[Azure Mobile Apps Hızlı Başlangıç]: app-service-mobile-cordova-get-started.md
[kimlik doğrulamayı kullanmaya başlama]: app-service-mobile-cordova-get-started-users.md
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

[Azure portalı]: https://portal.azure.com/
[JavaScript SDK'sı Azure Mobile Apps için]: https://www.npmjs.com/package/azure-mobile-apps-client
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx
