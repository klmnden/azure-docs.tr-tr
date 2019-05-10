---
title: Azure uygulama yapılandırma en iyi yöntemler | Microsoft Docs
description: En iyi Azure uygulama yapılandırmasını kullanma hakkında bilgi edinin
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: d1275a48de5cad9321186ba20860d853b8ce55ad
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65413614"
---
# <a name="azure-app-configuration-best-practices"></a>Azure uygulama yapılandırma en iyi uygulamaları

Bu makalede, Azure uygulama yapılandırması kullanılırken ortak desenler ve uygulamalar açıklanmaktadır.

## <a name="key-groupings"></a>Anahtar gruplandırmaları

Uygulama yapılandırma anahtarları düzenlemek için iki seçenek sunar: anahtar ön ekleri veya etiketler. Biri veya her ikisini de kullanabilirsiniz.

Anahtar önekleri anahtarları başına bölümlerdir. Aynı öneke adlarını kullanarak, bir anahtar kümesini mantıksal olarak gruplayabilirsiniz. Ön ekleri bir sınırlayıcı gibi birbirlerine bağlı birden çok bileşen içerebilir `/`benzer bir ad alanı oluşturmak için bir URL yolu. Bu tür hiyerarşileri anahtarları birçok uygulama, Bileşen Hizmetleri ve ortamları için bir uygulama yapılandırma deposunda depolamak yararlıdır. Akılda tutulması gereken önemli bir şey anahtarları uygulama kodunuza karşılık gelen ayarlarının değerleri almak için başvuruyor olmasıdır. Bir anahtar değil değiştirmeniz gerekir; aksi takdirde, gerçekleşen her zaman kodları değiştirmeniz gerekir.

Etiketleri olan bir öznitelik anahtarlar. Bunlar, bir anahtar türevleri oluşturmak için kullanılır. Örneğin, bir anahtarın birden çok sürümünü etiketler atayabilirsiniz. Bir sürüm, bir yineleme, ortamı veya diğer bir bağlam bilgileri olabilir. Uygulamanızı tamamen farklı bir anahtar-değer kümesini başka bir etiketi belirterek isteyebilir. Tüm anahtar başvuruları değişmeden kalabilir.

## <a name="key-value-compositions"></a>Anahtar-değer birleşimleri

Uygulama yapılandırması ile bağımsız varlıklar olarak depolanan tüm anahtarları değerlendirir. Anahtarlar arasındaki herhangi bir ilişki Infer veya anahtar değerlerini kendi hiyerarşi temelinde devral denemez. Birden fazla anahtarları toplayabilirsiniz, ancak, etiketleri kullanarak uygulama kodunuzda yığınlama uygun yapılandırma ile birlikte.

Bir örneğe göz atalım. Bir ayara sahip **Asset1** değeri "Geliştirme" ortamına değişebilir. Boş bir etiket ve "Geliştirme" adlı bir etiketi "Asset1" adlı bir anahtar oluşturabilirsiniz. İçin varsayılan değeri **Asset1** eski ve herhangi belirli bir değer için "Geliştirme" ikinci. Kodunuzda, önce anahtar değerlerinin herhangi bir etiket ve ardından önceki tüm değerleri aynı anahtarların üzerine yazmak için "Geliştirme" etiket olanlar olmadan alın. .NET Core gibi modern bir programlama çerçevesi kullanıyorsanız, uygulama yapılandırması ile erişmek için yerel yapılandırma sağlayıcısı kullanıyorsanız bu yığın özellik ücretsiz alabilirsiniz. Aşağıdaki kod parçacığı, bir .NET Core uygulamasında yığın nasıl uygulayacağınıza dair gösterir.

```csharp
// Augment the ConfigurationBuilder with Azure App Configuration
// Pull the connection string from an environment variable
configBuilder.AddAzureAppConfiguration(options => {
    options.Connect(configuration["connection_string"])
           .Use(KeyFilter.Any, LabelFilter.Null)
           .Use(KeyFilter.Any, "Development");
});
```

## <a name="app-configuration-bootstrap"></a>Uygulama yapılandırma önyükleme

Bir uygulama yapılandırma deposuna erişmek için Azure portalında kullanılabilir, bağlantı dizesini kullanabilirsiniz. Bağlantı dizeleri, kimlik bilgilerini içeren ve gizli dizi olarak değerlendirilir. Bir anahtar Kasası'nda depolanması gerekir. Daha iyi bir seçenek, Azure'ı kullanmak için yönetilen Kimliği ' dir. Bu yöntemle, yalnızca uygulama yapılandırma uç nokta URL'si, yapılandırma deposuna erişimi önyüklemenizi gerekir. URL uygulama kodunuza katıştırabilirsiniz (örneğin, *appsettings.json* dosyası). Bkz: [Azure ile tümleştirme yönetilen kimlikleri](howto-integrate-azure-managed-service-identity.md) daha fazla ayrıntı için.

## <a name="web-app-or-function-access-to-app-configuration"></a>Web uygulaması ya da işlev erişimi uygulama yapılandırması

Azure portalı üzerinden App Service uygulama ayarları, uygulama yapılandırma deposuna yönelik bağlantı dizesini girin. Ayrıca, anahtar Kasası'nda depolayabilir ve [App Service'ten başvuru](https://docs.microsoft.com/azure/app-service/app-service-key-vault-references). Azure ayrıca kullanabileceğiniz yönetilen yapılandırma deposuna erişim için kimliği. Bkz: [Azure ile tümleştirme yönetilen kimlikleri](howto-integrate-azure-managed-service-identity.md) daha fazla ayrıntı için.

Alternatif olarak, App Service uygulama yapılandırmasından yapılandırma gönderebilirsiniz. Uygulama yapılandırması doğrudan App Service'e veri gönderen bir dışarı aktarma işlevi (Azure portalı ve CLI) sağlar. Bu yöntem ile hiç uygulama kodunu değiştirmeniz gerekmez.

## <a name="next-steps"></a>Sonraki adımlar

* [Anahtarları ve değerleri](./concept-key-value.md)
