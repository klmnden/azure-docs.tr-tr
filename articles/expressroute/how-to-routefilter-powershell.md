---
title: "Azure ExpressRoute Microsoft eşliği için rota filtrelerini yapılandırın: PowerShell | Microsoft Docs"
description: "Bu makalede, Microsoft PowerShell kullanarak Peering için yol filtrelerini yapılandırmak açıklar"
documentationcenter: na
services: expressroute
author: ganesr
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/26/2017
ms.author: ganesr
ms.openlocfilehash: c940d2eab4d8e977b67b3553ab2e3d9110710956
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="configure-route-filters-for-microsoft-peering-powershell"></a>Microsoft eşliği için rota filtrelerini yapılandırın: PowerShell
> [!div class="op_single_selector"]
> * [Azure Portal](how-to-routefilter-portal.md)
> * [Azure PowerShell](how-to-routefilter-powershell.md)
> * [Azure CLI](how-to-routefilter-cli.md)
> 

Yol filtreleri Microsoft eşleme yoluyla desteklenen hizmetleri kümesini kullanmak için bir yoldur. Bu makaledeki adımları yapılandırmak ve ExpressRoute bağlantı hatları için yol filtreleri yönetmenize yardımcı olması.

Dynamics 365 Hizmetleri ve iş için Exchange Online, SharePoint Online ve Skype gibi Office 365 Hizmetleri ve depolama ve SQL DB gibi Azure Hizmetleri Microsoft eşleme aracılığıyla erişilebilir. Microsoft eşlemesi bir expressroute bağlantı hattı yapılandırıldığında, bu hizmetleri ile ilgili tüm ön eklerin oluşturulmuş BGP oturumları bildirilir. BGP topluluk değeri öneki sunulan Hizmeti'ni tanımlamak için her ön eki eklenir. BGP topluluk değerlerini ve eşlemek için Hizmetler listesi için bkz: [BGP toplulukları](expressroute-routing.md#bgp).

Tüm hizmetleri için bağlantı gerektiriyorsa, çok sayıda önekleri BGP bildirilir. Bu, ağınızdaki yönlendiricileri tarafından tutulan yol tablolarını boyutunu önemli ölçüde artırır. Yalnızca bir alt kümesini Microsoft eşliği ile sunulan hizmetlere kullanmayı planlıyorsanız, yol tablolarınız iki yolla boyutunu azaltabilirsiniz. Şunları yapabilirsiniz:

- İstenmeyen önekleri BGP toplulukları yol filtreleri uygulayarak filtreleyin. Bu standart bir ağ uygulaması ve birçok ağ içinde yaygın olarak kullanılır.

- Rota filtrelerini tanımlayın ve bunları, expressroute bağlantı hattı uygulayabilirsiniz. Bir rota filtre Microsoft eşliği ile kullanmak için plan Hizmetleri listesini seçmenize izin veren yeni bir kaynaktır. ExpressRoute yönlendiriciler yalnızca rota filtrede tanımlanan hizmetlere ait önekleri listesini gönderir.

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

- Microsoft sağlanan eşlemesini sahip etkin bir expressroute olması gerekir. Bu görevleri gerçekleştirmek için aşağıdaki yönergeleri kullanın:
  - [Bir expressroute bağlantı hattı oluşturma](expressroute-howto-circuit-arm.md) ve devam etmeden önce bağlantı sağlayıcınız tarafından etkinleştirilen hattı sahip. Expressroute bağlantı hattının sağlanmış ve etkin durumda olması gerekir.
  - [Microsoft eşlemesi oluşturmak](expressroute-circuit-peerings.md) BGP oturumu doğrudan yönetiyorsanız. Veya, bağlantı sağlayıcınız hattınız için eşlemesini Microsoft sağlamak.

-  Oluşturma ve bir rota filtre yapılandırmanız gerekir.
    - Hizmetleri tanımlamak sizinle Microsoft eşliği ile kullanmak için
    - BGP topluluk değerlerini servisleri ile ilişkili olan listesini belirlemesini
    - BGP topluluk değerlerini eşleşen Önek listesi izin verecek bir kural oluşturun

-  Expressroute bağlantı hattı için rota filtre eklemeniz gerekir.

## <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmaya başlamadan önce aşağıdaki ölçütlere uyan emin olun:

 - Azure Resource Manager PowerShell cmdlet'lerinin en son sürümünü yükleyin. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/install-azurerm-ps).

  > [!NOTE]
  > PowerShell Galerisi yerine Yükleyicisi'ni kullanarak en son sürümü yükleyin. Yükleyici gerekli cmdlet'lerini şu anda desteklemiyor.
  > 

 - Gözden geçirme [Önkoşullar](expressroute-prerequisites.md) ve [iş akışları](expressroute-workflows.md) yapılandırmaya başlamadan önce.

 - Etkin bir ExpressRoute bağlantı hattınızın olması gerekir. Devam etmeden önce [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-arm.md) yönergelerini izleyin ve bağlantı sağlayıcınızın bağlantı hattını etkinleştirmesini isteyin. Expressroute bağlantı hattının sağlanmış ve etkin durumda olması gerekir.

 - Etkin bir Microsoft eşlemesi olması gerekir. Bölümündeki yönergeleri izleyin [oluşturma ve eşleme yapılandırmasını değiştirme](expressroute-circuit-peerings.md)

### <a name="log-in-to-your-azure-account"></a>Azure hesabınızda oturum açın

Bu yapılandırmaya başlamadan önce Azure hesabınızda oturum açmalısınız. Bu cmdlet, Azure hesabınıza ilişkin oturum açma kimlik bilgilerinizi girmenizi ister. Oturum açtıktan sonra, Azure PowerShell'de kullanabilmeniz için hesap ayarlarınızı indirir.

PowerShell konsolunuzu yükseltilmiş ayrıcalıklarla açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

```powershell
Login-AzureRmAccount
```

Birden çok Azure aboneliğiniz varsa, hesabın aboneliklerini denetleyin.

```powershell
Get-AzureRmSubscription
```

Kullanmak istediğiniz aboneliği belirtin.

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
```

## <a name="prefixes"></a>1. adım: ön ekleri ve BGP topluluk değerlerini listesini al

### <a name="1-get-a-list-of-bgp-community-values"></a>1. BGP topluluk değerlerini listesini al

Microsoft eşlemesini erişilebilir hizmetleriyle ilişkili BGP topluluk değerlerini listesi ve bunlarla ilişkilendirilmiş önekleri listesini almak için aşağıdaki cmdlet'i kullanın:

```powershell
Get-AzureRmBgpServiceCommunity
```
### <a name="2-make-a-list-of-the-values-that-you-want-to-use"></a>2. Kullanmak istediğiniz değerleri listesi olun

Rota filtrede kullanmak istediğiniz BGP topluluk değerlerini listesini hazırlayın. Örnek olarak, Dynamics 365 Hizmetleri için BGP topluluk değeri 12076:5040 ' dir.

## <a name="filter"></a>2. adım: bir rota filtre ve bir filtre kuralı oluşturma

Bir rota filtre yalnızca bir kuralınız olabilir ve kural 'İzin ver' türünde olmalıdır. Bu kural, kendisiyle ilişkilendirilmiş BGP topluluk değerlerini listesini olabilir.

### <a name="1-create-a-route-filter"></a>1. Bir rota filtresi oluşturma

İlk olarak, rota filtre oluşturun. Komut 'New-AzureRmRouteFilter' yalnızca bir rota filtre kaynak oluşturur. Kaynak oluşturduktan sonra ardından bir kural oluşturmak ve rota filtre nesnesine ekleme gerekir. Bir rota filtre kaynak oluşturmak için aşağıdaki komutu çalıştırın:

```powershell
New-AzureRmRouteFilter -Name "MyRouteFilter" -ResourceGroupName "MyResourceGroup" -Location "West US"
```

### <a name="2-create-a-filter-rule"></a>2. Bir filtre kuralı oluşturma

Örnekte gösterildiği gibi BGP toplulukları bir dizi virgülle ayrılmış bir liste belirtebilirsiniz. Yeni bir kural oluşturmak için aşağıdaki komutu çalıştırın:
 
```powershell
$rule = New-AzureRmRouteFilterRuleConfig -Name "Allow-EXO-D365" -Access Allow -RouteFilterRuleType Community -CommunityList "12076:5010,12076:5040"
```

### <a name="3-add-the-rule-to-the-route-filter"></a>3. Rota filtre kuralı Ekle

Filtre kuralı için rota filtre eklemek için aşağıdaki komutu çalıştırın:
 
```powershell
$routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
$routefilter.Rules.Add($rule)
Set-AzureRmRouteFilter -RouteFilter $routefilter
```

## <a name="attach"></a>3. adım: bir expressroute bağlantı hattı için rota filtre ekleme

Yalnızca Microsoft eşlemesi sahip olduğu varsayılarak expressroute bağlantı hattı için rota filtre eklemek için aşağıdaki komutu çalıştırın:

```powershell
$ckt.Peerings[0].RouteFilter = $routefilter 
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="tasks"></a>Genel görevler

### <a name="getproperties"></a>Bir rota filtre özelliklerini almak için

Bir rota filtre özelliklerini almak için aşağıdaki adımları kullanın:

1. Rota filtre kaynak almak için aşağıdaki komutu çalıştırın:

  ```powershell
  $routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
  ```
2. Yol, aşağıdaki komutu çalıştırarak, rota filtre kaynak için filtre kuralları alın:

  ```powershell
  $routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
  $rule = $routefilter.Rules[0]
  ```

### <a name="updateproperties"></a>Bir rota filtre özelliklerini güncelleştirmek için

Rota filtre zaten bir hattına bağlıysa, BGP topluluk listesine güncelleştirmeleri otomatik olarak uygun önek tanıtım değişiklikleri kurulan BGP oturumları aracılığıyla yayılır. Aşağıdaki komutu kullanarak, rota filtre BGP topluluk listesini güncelleştirebilirsiniz:

```powershell
$routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
$routefilter.rules[0].Communities = "12076:5030", "12076:5040"
Set-AzureRmRouteFilter -RouteFilter $routefilter
```

### <a name="detach"></a>Bir expressroute bağlantı hattı rota filtresinden ayırmak için

Bir rota filtre expressroute bağlantı hattı ayrılmış sonra hiçbir önekleri BGP oturumu aracılığıyla bildirilir. Aşağıdaki komutu kullanarak bir expressroute bağlantı hattı rota filtresinden ayırabilirsiniz:
  
```powershell
$ckt.Peerings[0].RouteFilter = $null
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="delete"></a>Bir rota filtreyi silmek için

Tüm devresine bağlı olmayan bir rota filtre yalnızca silebilirsiniz. Rota filtre herhangi devresine silmeye çalışmadan önce bağlı olmadığından emin olun. Aşağıdaki komutu kullanarak bir rota filtre silebilirsiniz:

```powershell
Remove-AzureRmRouteFilter -Name "MyRouteFilter" -ResourceGroupName "MyResourceGroup"
```

## <a name="next-steps"></a>Sonraki Adımlar

ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
