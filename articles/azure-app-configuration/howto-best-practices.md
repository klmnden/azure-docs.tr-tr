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
ms.openlocfilehash: 3d9a597e7ced631627a121f3f0757e472f9a4bae
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393584"
---
# <a name="azure-app-configuration-best-practices"></a>Azure uygulama yapılandırma en iyi uygulamaları

Bu makalede, Azure uygulama yapılandırması kullanılırken ortak desenler ve en iyi uygulamalar açıklanmaktadır.

## <a name="key-groupings"></a>Anahtar gruplandırmaları

Uygulama yapılandırma anahtarları düzenlemek için iki seçenek sunar:

* Anahtar ön ekleri
* Etiketler

Anahtarlarınızı grubuna bir veya iki seçeneklerini kullanabilirsiniz.

*Anahtar önekleri* anahtarları başına bölümlerdir. Aynı öneke adlarını kullanarak, bir anahtar kümesini mantıksal olarak gruplayabilirsiniz. Ön ekleri gibi bir sınırlandırıcı ile bağlı birden çok bileşen içerebilir `/`benzer bir ad alanı oluşturmak için bir URL yolu. Bir uygulama yapılandırma deposunda birçok uygulama, Bileşen Hizmetleri ve ortamları için anahtarları depolamak için bu tür hiyerarşileri yararlıdır.

Akılda tutulması gereken önemli bir şey anahtarları uygulama kodunuza karşılık gelen ayarlarının değerleri almak için başvuruyor olmasıdır. Anahtarları değiştirmeniz gerekmez, aksi takdirde gerçekleşen her zaman kodunuzu değiştirmeniz gerekir.

*Etiketleri* anahtarlar bir özniteliği olan. Bunların türevleri bir anahtar oluşturmak için kullanılır. Örneğin, bir anahtarın birden çok sürümünü etiketler atayabilirsiniz. Bir sürümü, bir yineleme, bir ortam veya diğer bir bağlam bilgileri olabilir. Uygulamanızı tamamen farklı bir anahtar değerler kümesini başka bir etiketi belirterek isteyebilir. Sonuç olarak, tüm anahtar başvurularını, kodunuzda değişmeden kalır.

## <a name="key-value-compositions"></a>Anahtar-değer birleşimleri

Uygulama yapılandırması ile bağımsız varlıklar olarak depolanan tüm anahtarları değerlendirir. Uygulama yapılandırması, anahtarlar arasındaki herhangi bir ilişki çıkarsanacak veya anahtar değerlerini kendi hiyerarşi temelinde devralmak için çalışmaz. Birden fazla anahtarları, ancak etiketleri uygun yapılandırma, uygulama kodunda yığın ile birlikte kullanarak toplayabilirsiniz.

Bir örneğe göz atalım. Adlı bir ayar olduğunu varsayalım **Asset1**, geliştirme ortamına bağlı değeri değişiklik gösterebilir. Boş bir etiket ve "Geliştirme" adlı bir etiketi "Asset1" adlı bir anahtar oluşturun. İlk etiketi için varsayılan değer yerleştirdiğiniz **Asset1**, belirli bir değeri "Geliştirme" için ikinci yerleştirin.

Kodunuzda önce anahtar değerlerinin tüm etiketleri olmadan almak ve sonra "Geliştirme" etiketi ile ikinci kez aynı anahtar değerleri kümesini alır. İkinci kez değerleri aldığınızda, önceki değerleri anahtarların üzerine yazılır. .NET Core yapılandırma sistemi "birden çok kümesi yapılandırma verilerini birbirinin yığın" sağlar. Birden fazla kümesindeki bir anahtar varsa içerdiği belirlenen son kullanılır. Uygulama yapılandırması ile erişmek için yerel yapılandırma sağlayıcısı kullanıyorsanız gibi .NET Core, modern bir programlama çerçevesi ile bu yığın özellik ücretsiz sahip olursunuz. Aşağıdaki kod parçacığı, bir .NET Core uygulamasında yığın nasıl uygulayacağınıza dair gösterir:

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

Bir uygulama yapılandırma deposuna erişmek için Azure portalında kullanılabilir, bağlantı dizesini kullanabilirsiniz. Bağlantı dizeleri, kimlik bilgilerini içerdiğinden, gizli dizileri kabul edilmeleri. Bu gizli dizileri Azure Key Vault'ta depolanması gereken ve kodunuzu bunları almak için anahtar Kasası'na kimliğini doğrulaması gerekir.

Azure Active Directory'de yönetilen kimlikleri özelliğiyle daha iyi bir seçenektir. Yönetilen kimliklerle uygulama yapılandırma deponuz için yalnızca uygulama yapılandırma uç nokta URL'sini önyükleme erişimi gerekir. URL uygulama kodunuza katıştırabilirsiniz (örneğin, *appsettings.json* dosyası). Bkz: [Azure ile tümleştirme yönetilen kimlikleri](howto-integrate-azure-managed-service-identity.md) Ayrıntılar için.

## <a name="app-or-function-access-to-app-configuration"></a>Uygulama yapılandırma uygulama ya da işlev erişimi

Erişim web apps veya işlevleri için uygulama yapılandırma için aşağıdaki yöntemlerden birini kullanarak sağlayabilirsiniz:

* Azure portalı üzerinden, App Service uygulama ayarları, uygulama yapılandırma deposuna bağlantı dizesini girin.
* Anahtar Kasası'nda bağlantı dizesini uygulama yapılandırma deponuza Store ve [App Service'ten başvuru](https://docs.microsoft.com/azure/app-service/app-service-key-vault-references).
* Azure'daki uygulama yapılandırması mağazaya erişmek için kimliklerini yönetilen. Daha fazla bilgi için [Azure ile tümleştirme yönetilen kimlikleri](howto-integrate-azure-managed-service-identity.md).
* App Service gönderim yapılandırmasını uygulama yapılandırmasından. Uygulama yapılandırması doğrudan App Service'e veri gönderen bir dışarı aktarma işlevi (Azure portalı ve Azure CLI) sağlar. Bu yöntem ile hiç uygulama kodunu değiştirmeniz gerekmez.

## <a name="next-steps"></a>Sonraki adımlar

* [Anahtarları ve değerleri](./concept-key-value.md)
