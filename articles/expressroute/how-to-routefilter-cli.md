---
title: "Azure ExpressRoute Microsoft eşliği için rota filtrelerini yapılandırın: CLI | Microsoft Docs"
description: "Bu makalede, Microsoft Azure CLI kullanarak Peering için yol filtrelerini yapılandırmak açıklar"
documentationcenter: na
services: expressroute
author: anzaman
manager: ganesr
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: anzaman
ms.openlocfilehash: a85a68393f3dc946db651791de9efff0694f9989
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="configure-route-filters-for-microsoft-peering-azure-cli"></a>Microsoft eşliği için rota filtrelerini yapılandırın: Azure CLI

> [!div class="op_single_selector"]
> * [Azure Portal](how-to-routefilter-portal.md)
> * [Azure PowerShell](how-to-routefilter-powershell.md)
> * [Azure CLI](how-to-routefilter-cli.md)
> 

Yol filtreleri Microsoft eşleme yoluyla desteklenen hizmetleri kümesini kullanmak için bir yoldur. Bu makaledeki adımları yapılandırmak ve ExpressRoute bağlantı hatları için yol filtreleri yönetmenize yardımcı olması.

Dynamics 365 Hizmetleri ve Exchange Online, SharePoint Online ve Skype gibi Office 365 Hizmetleri iş için Microsoft eşleme aracılığıyla erişilebilir. Microsoft eşlemesi bir expressroute bağlantı hattı yapılandırıldığında, bu hizmetleri ile ilgili tüm ön eklerin oluşturulmuş BGP oturumları bildirilir. BGP topluluk değeri öneki sunulan Hizmeti'ni tanımlamak için her ön eki eklenir. BGP topluluk değerlerini ve eşlemek için Hizmetler listesi için bkz: [BGP toplulukları](expressroute-routing.md#bgp).

Tüm hizmetleri için bağlantı gerektiriyorsa, çok sayıda önekleri BGP bildirilir. Bu, ağınızdaki yönlendiricileri tarafından tutulan yol tablolarını boyutunu önemli ölçüde artırır. Yalnızca bir alt kümesini Microsoft eşliği ile sunulan hizmetlere kullanmayı planlıyorsanız, yol tablolarınız iki yolla boyutunu azaltabilirsiniz. Şunları yapabilirsiniz:

* İstenmeyen önekleri BGP toplulukları yol filtreleri uygulayarak filtreleyin. Bu standart bir ağ uygulaması ve birçok ağ içinde yaygın olarak kullanılır.

* Rota filtrelerini tanımlayın ve bunları, expressroute bağlantı hattı uygulayabilirsiniz. Bir rota filtre Microsoft eşliği ile kullanmak için plan Hizmetleri listesini seçmenize izin veren yeni bir kaynaktır. ExpressRoute yönlendiriciler yalnızca rota filtrede tanımlanan hizmetlere ait önekleri listesini gönderir.

### <a name="about"></a>Rota filtreler hakkında

Microsoft eşlemesi, expressroute bağlantı hattı üzerinde yapılandırıldığında, sınır yönlendiricileri (sizin ya da bağlantı sağlayıcınızın) BGP oturumları çifti Microsoft sınır yönlendiricileri kurun. Yol yok, ağınıza bildirilir. Yol tanıtımlarını ağınıza etkinleştirmek için bir rota filtre ilişkilendirmeniz gerekir.

Bir rota filtre expressroute bağlantı hattı 's Microsoft eşliği ile kullanmak istediğiniz hizmetleri tanımlamanıza olanak sağlar. Tüm BGP topluluk değerlerini temelde beyaz listesi var. Bir rota filtre kaynak tanımlanmış ve bir expressroute bağlantı hattı bağlı sonra BGP topluluk değerlerine eşlemek tüm ön eklerin ağınıza bildirilir.

Yol filtreleri bunlardaki Office 365 Hizmetleri ile eklemek için Office 365 hizmetlerini ExpressRoute aracılığıyla kullanma yetkisi olması gerekir. ExpressRoute aracılığıyla Office 365 hizmetlerini kullanma yetkisi olmayan yol filtreleri ekleme işlemi başarısız olur. Yetkilendirme işlemi hakkında daha fazla bilgi için bkz: [Office 365 için Azure ExpressRoute](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd). Dynamics 365 hizmetlerine bağlantıyı önceki tüm yetkilendirme gerektirmez.

> [!IMPORTANT]
> 1 Ağustos 2017 önce yapılandırılmış olan ExpressRoute bağlantı hatları, Microsoft eşlemesi yol filtreleri tanımlanmamış olsa bile eşleme, Microsoft tarafından tanıtılan tüm hizmet ön eklerin sahip olur. Ve 1 Ağustos 2017 sonrasında yapılandırılmış ExpressRoute bağlantı hatları, Microsoft eşlemesi sahip olmaz tüm ön eklerin rota filtre devresine bağlanana kadar tanıtılan.
> 
> 

### <a name="workflow"></a>İş akışı

Microsoft eşlemesini üzerinden hizmetlere başarıyla bağlanabilmek için için aşağıdaki yapılandırma adımlarını tamamlamanız gerekir:

* Microsoft sağlanan eşlemesini sahip etkin bir expressroute olması gerekir. Bu görevleri gerçekleştirmek için aşağıdaki yönergeleri kullanın:
  * [Bir expressroute bağlantı hattı oluşturma](howto-circuit-cli.md) ve devam etmeden önce bağlantı sağlayıcınız tarafından etkinleştirilen hattı sahip. Expressroute bağlantı hattının sağlanmış ve etkin durumda olması gerekir.
  * [Microsoft eşlemesi oluşturmak](howto-routing-cli.md) BGP oturumu doğrudan yönetiyorsanız. Veya, bağlantı sağlayıcınız hattınız için eşlemesini Microsoft sağlamak.

* Oluşturma ve bir rota filtre yapılandırmanız gerekir.
  * Hizmetleri tanımlamak sizinle Microsoft eşliği ile kullanmak için
  * BGP topluluk değerlerini servisleri ile ilişkili olan listesini belirlemesini
  * BGP topluluk değerlerini eşleşen Önek listesi izin verecek bir kural oluşturun

* Expressroute bağlantı hattı için rota filtre eklemeniz gerekir.

## <a name="before-you-begin"></a>Başlamadan önce

Başlamadan önce, CLI komutlarının en son sürümünü (2.0 veya üzeri) yükleyin. CLI komutlarını yükleme hakkında bilgi için bkz. [Azure CLI 2.0’ı yükleme](/cli/azure/install-azure-cli) ve [Azure CLI 2.0’ı Kullanmaya Başlama](/cli/azure/get-started-with-azure-cli).

* Gözden geçirme [Önkoşullar](expressroute-prerequisites.md) ve [iş akışları](expressroute-workflows.md) yapılandırmaya başlamadan önce.

* Etkin bir ExpressRoute bağlantı hattınızın olması gerekir. Devam etmeden önce [ExpressRoute bağlantı hattı oluşturma](howto-circuit-cli.md) yönergelerini izleyin ve bağlantı sağlayıcınızın bağlantı hattını etkinleştirmesini isteyin. Expressroute bağlantı hattının sağlanmış ve etkin durumda olması gerekir.

* Etkin bir Microsoft eşlemesi olması gerekir. Bölümündeki yönergeleri izleyin [oluşturma ve eşleme yapılandırmasını değiştirme](howto-routing-cli.md)

### <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>Azure hesabınızda oturum açın ve aboneliğinizi seçin

Yapılandırmanızı başlamak için Azure hesabınızda oturum açın. Aşağıdaki örnekler bağlanmanıza yardımcı olması için kullanın:

```azurecli
az login
```

Hesapla ilişkili abonelikleri kontrol edin.

```azurecli
az account list
```

Bir expressroute bağlantı hattı oluşturmak istediğiniz aboneliği seçin.

```azurecli
az account set --subscription "<subscription ID>"
```

## <a name="prefixes"></a>1. adım: ön ekleri ve BGP topluluk değerlerini listesini al

### <a name="1-get-a-list-of-bgp-community-values"></a>1. BGP topluluk değerlerini listesini al

Microsoft eşlemesini erişilebilir hizmetleriyle ilişkili BGP topluluk değerlerini listesi ve bunlarla ilişkilendirilmiş önekleri listesini almak için aşağıdaki cmdlet'i kullanın:

```azurecli
az network route-filter rule list-service-communities
```
### <a name="2-make-a-list-of-the-values-that-you-want-to-use"></a>2. Kullanmak istediğiniz değerleri listesi olun

Rota filtrede kullanmak istediğiniz BGP topluluk değerlerini listesini hazırlayın. Örnek olarak, Dynamics 365 Hizmetleri için BGP topluluk değeri 12076:5040 ' dir.

## <a name="filter"></a>2. adım: bir rota filtre ve bir filtre kuralı oluşturma

Bir rota filtre yalnızca bir kuralınız olabilir ve kural 'İzin ver' türünde olmalıdır. Bu kural, kendisiyle ilişkilendirilmiş BGP topluluk değerlerini listesini olabilir.

### <a name="1-create-a-route-filter"></a>1. Bir rota filtresi oluşturma

İlk olarak, rota filtre oluşturun. 'Ağ rota-filtresi oluşturma az' komutu, yalnızca bir rota filtre kaynak oluşturur. Kaynak oluşturduktan sonra ardından bir kural oluşturmak ve rota filtre nesnesine ekleme gerekir. Bir rota filtre kaynak oluşturmak için aşağıdaki komutu çalıştırın:

```azurecli
az network route-filter create -n MyRouteFilter -g MyResourceGroup
```

### <a name="2-create-a-filter-rule"></a>2. Bir filtre kuralı oluşturma

Yeni bir kural oluşturmak için aşağıdaki komutu çalıştırın:
 
```azurecli
az network route-filter rule create --filter-name MyRouteFilter -n CRM --communities 12076:5040 --access Allow -g MyResourceGroup
```

## <a name="attach"></a>3. adım: bir expressroute bağlantı hattı için rota filtre ekleme

Expressroute bağlantı hattı için rota filtre eklemek için aşağıdaki komutu çalıştırın:

```azurecli
az network express-route peering update --circuit-name MyCircuit -g ExpressRouteResourceGroupName --name MicrosoftPeering --route-filter MyRouteFilter
```

## <a name="tasks"></a>Genel görevler

### <a name="getproperties"></a>Bir rota filtre özelliklerini almak için

Bir rota filtre özelliklerini almak için aşağıdaki komutu kullanın:

```azurecli
az network route-filter show -g ExpressRouteResourceGroupName --name MyRouteFilter 
```

### <a name="updateproperties"></a>Bir rota filtre özelliklerini güncelleştirmek için

Rota filtre zaten bir hattına bağlıysa, BGP topluluk listesine güncelleştirmeleri otomatik olarak uygun önek tanıtım değişiklikleri kurulan BGP oturumları aracılığıyla yayılır. Aşağıdaki komutu kullanarak, rota filtre BGP topluluk listesini güncelleştirebilirsiniz:

```azurecli
az network route-filter rule update --filter-name MyRouteFilter -n CRM -g ExpressRouteResourceGroupName --add communities '12076:5040' --add communities '12076:5010'
```

### <a name="detach"></a>Bir expressroute bağlantı hattı rota filtresinden ayırmak için

Bir rota filtre expressroute bağlantı hattı ayrılmış sonra hiçbir önekleri BGP oturumu aracılığıyla bildirilir. Aşağıdaki komutu kullanarak bir expressroute bağlantı hattı rota filtresinden ayırabilirsiniz:

```azurecli
az network express-route peering update --circuit-name MyCircuit -g ExpressRouteResourceGroupName --name MicrosoftPeering --remove routeFilter
```

### <a name="delete"></a>Bir rota filtreyi silmek için

Tüm devresine bağlı olmayan bir rota filtre yalnızca silebilirsiniz. Rota filtre herhangi devresine silmeye çalışmadan önce bağlı olmadığından emin olun. Aşağıdaki komutu kullanarak bir rota filtre silebilirsiniz:

```azurecli
az network route-filter delete -n MyRouteFilter -g MyResourceGroup
```

## <a name="next-steps"></a>Sonraki Adımlar

ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
