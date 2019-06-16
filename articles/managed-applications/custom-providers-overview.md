---
title: Azure özel sağlayıcılar Önizlemesi'ne genel bakış
description: Azure Resource Manager ile özel kaynak sağlayıcısı oluşturmak için kavramları açıklar
author: MSEvanhi
ms.service: managed-applications
ms.topic: conceptual
ms.date: 05/01/2019
ms.author: evanhi
ms.openlocfilehash: bbfb10f612690af0f4fd3683e0f58986a21048d8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65159863"
---
# <a name="azure-custom-providers-preview-overview"></a>Azure özel sağlayıcıları Önizleme genel bakış

Azure özel sağlayıcıları ile hizmetiniz ile çalışmak için Azure genişletebilirsiniz. Size özelleştirilmiş kaynak türleri ve Eylemler dahil olmak üzere, kendi kaynak sağlayıcısı oluşturun. Özel bir sağlayıcı, Azure Resource Manager ile tümleşiktir. Hizmetinizin güvenliğini sağlama ve dağıtma için Resource Manager'ın özellikleri, şablon dağıtımları ve rol tabanlı access control gibi kullanabilirsiniz.

Bu makalede, özel sağlayıcılar ve özelliklerine genel bakış sağlar. Aşağıdaki görüntüde kaynaklara ve işlemlere tanımlanan özel bir sağlayıcı için iş akışı gösterilmektedir.

![Özel sağlayıcı genel bakış](./media/custom-providers-overview/overview.png)

> [!IMPORTANT]
> Özel sağlayıcıları şu an genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="define-your-custom-provider"></a>Özel sağlayıcınızın tanımlayın

Azure Resource Manager'ın özel sağlayıcınızın hakkında bilmeniz vererek başlattığınız. Azure'da kaynak türünü kullanan bir özel sağlayıcı kaynak dağıtma **Microsoft.CustomProviders/resourceProviders**. Bu kaynakta hizmetiniz için kaynakları ve eylemleri tanımlayın.

Örneğin, hizmetiniz adlı bir kaynak türü gerekiyorsa **kullanıcılar**, bu kaynak türü özel sağlayıcı Tanımınızda içerir. Her kaynak türü için bu kaynak türü için REST işlemlerini (PUT, alma, silme) sunan bir uç nokta sağlar. Uç nokta, herhangi bir ortam üzerinde barındırılabilir ve hizmetinizi kaynak türü üzerinde işlemler nasıl işlediğini için mantığı içerir.

Kaynak sağlayıcınız için özel eylemler de tanımlayabilirsiniz. Eylemler sonrası işlemleri temsil eder. Eylemler, Başlat, Durdur veya yeniden başlatma gibi işlemler için kullanın. İsteği işleyen bir uç nokta sağlar.

Aşağıdaki örnek, bir eylem ve bir kaynak türü ile özel bir sağlayıcı tanımlamak gösterilmektedir.

```json
{
  "apiVersion": "2018-09-01-preview",
  "type": "Microsoft.CustomProviders/resourceProviders",
  "name": "[parameters('funcName')]",
  "location": "[parameters('location')]",
  "properties": {
    "actions": [
      {
        "name": "ping",
        "routingType": "Proxy",
        "endpoint": "[concat('https://', parameters('funcName'), '.azurewebsites.net/api/{requestPath}')]"
      }
    ],
    "resourceTypes": [
      {
        "name": "users",
        "routingType": "Proxy,Cache",
        "endpoint": "[concat('https://', parameters('funcName'), '.azurewebsites.net/api/{requestPath}')]"
      }
    ]
  }
},
```

İçin **routingType**, kabul edilen değerler `Proxy` ve `Cache`. Proxy kaynak türü için istekleri anlamına gelir veya eylem bitiş noktası tarafından işlenir. Önbellek ayarını, yalnızca eylemleri değil kaynak türleri için desteklenir. Önbellek belirtmek için proxy belirtmeniz gerekir. Önbellek yanıtları uç noktasından okuma işlemleri en iyi duruma getirme depolanan anlamına gelir. Önbellek ayarını kullanarak diğer Resource Manager hizmetleriyle tutarlı ve uyumlu olan API uygulama kolaylaştırır.

## <a name="deploy-your-resource-types"></a>Kaynak türlerinizi dağıtma

Özel sağlayıcınızın tanımladıktan sonra özelleştirilmiş kaynak türlerini dağıtabilirsiniz. Aşağıdaki örnek, kaynak türü, özel sağlayıcınızın dağıtmak için şablonunuzdaki dahil JSON'u göstermektedir. Bu kaynak türü, diğer Azure kaynaklarıyla aynı şablonu içinde dağıtılabilir.

```json
{
    "apiVersion": "2018-09-01-preview",
    "type": "Microsoft.CustomProviders/resourceProviders/users",
    "name": "[concat(parameters('rpname'), '/santa')]",
    "location": "[parameters('location')]",
    "properties": {
        "FullName": "Santa Claus",
        "Location": "NorthPole"
    }
}
```

## <a name="manage-access"></a>Erişimi yönetme

Azure kullanan [rol tabanlı erişim denetimi](../role-based-access-control/overview.md) kaynak sağlayıcısına erişimi yönetmek için. Atayabileceğiniz [yerleşik roller](../role-based-access-control/built-in-roles.md) sahibi, katkıda bulunan veya okuyucu kullanıcılara gibi. Veya tanımlayabilirsiniz [özel roller](../role-based-access-control/custom-roles.md) özgü, kaynak sağlayıcısı işlemleri.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, özel sağlayıcıları hakkında bilgi edindiniz. Özel bir sağlayıcı oluşturmak için sonraki makaleye gidin.

> [!div class="nextstepaction"]
> [Öğretici: Özel bir sağlayıcı oluşturmak ve özel kaynakları dağıtma](create-custom-provider.md)
