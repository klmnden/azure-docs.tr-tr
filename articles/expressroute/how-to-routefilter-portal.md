---
title: "Azure ExpressRoute Microsoft eşliği için rota filtrelerini yapılandırın: portalı | Microsoft Docs"
description: "Bu makalede, Microsoft Azure portalını kullanarak Peering için yol filtrelerini yapılandırmak açıklar"
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
ms.openlocfilehash: 0129a48e43e90001785a5977d4b0d1fd9fa9fd7d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configure-route-filters-for-microsoft-peering-azure-portal"></a>Microsoft eşliği için rota filtrelerini yapılandırın: Azure portal
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
  - [Bir expressroute bağlantı hattı oluşturma](expressroute-howto-circuit-portal-resource-manager.md) ve devam etmeden önce bağlantı sağlayıcınız tarafından etkinleştirilen hattı sahip. Expressroute bağlantı hattının sağlanmış ve etkin durumda olması gerekir.
  - [Microsoft eşlemesi oluşturmak](expressroute-howto-routing-portal-resource-manager.md) BGP oturumu doğrudan yönetiyorsanız. Veya, bağlantı sağlayıcınız hattınız için eşlemesini Microsoft sağlamak.

-  Oluşturma ve bir rota filtre yapılandırmanız gerekir.
    - Hizmetleri tanımlamak sizinle Microsoft eşliği ile kullanmak için
    - BGP topluluk değerlerini servisleri ile ilişkili olan listesini belirlemesini
    - BGP topluluk değerlerini eşleşen Önek listesi izin verecek bir kural oluşturun

-  Expressroute bağlantı hattı için rota filtre eklemeniz gerekir.

## <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmaya başlamadan önce aşağıdaki ölçütlere uyan emin olun:

 - Gözden geçirme [Önkoşullar](expressroute-prerequisites.md) ve [iş akışları](expressroute-workflows.md) yapılandırmaya başlamadan önce.

 - Etkin bir ExpressRoute bağlantı hattınızın olması gerekir. Devam etmeden önce [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-portal-resource-manager.md) yönergelerini izleyin ve bağlantı sağlayıcınızın bağlantı hattını etkinleştirmesini isteyin. Expressroute bağlantı hattının sağlanmış ve etkin durumda olması gerekir.

 - Etkin bir Microsoft eşlemesi olması gerekir. Bölümündeki yönergeleri izleyin [oluşturma ve eşleme yapılandırmasını değiştirme](expressroute-howto-routing-portal-resource-manager.md)


## <a name="prefixes"></a>1. adım: ön ekleri ve BGP topluluk değerlerini listesini al

### <a name="1-get-a-list-of-bgp-community-values"></a>1. BGP topluluk değerlerini listesini al

Microsoft eşleme aracılığıyla erişilebilir hizmetleriyle ilişkili BGP topluluk değerlerini sağlanmıştır [ExpressRoute yönlendirme gereksinimleri](expressroute-routing.md) sayfası.

### <a name="2-make-a-list-of-the-values-that-you-want-to-use"></a>2. Kullanmak istediğiniz değerleri listesi olun

Rota filtrede kullanmak istediğiniz BGP topluluk değerlerini listesini hazırlayın. Örnek olarak, Dynamics 365 Hizmetleri için BGP topluluk değeri 12076:5040 ' dir.

## <a name="filter"></a>2. adım: bir rota filtre ve bir filtre kuralı oluşturma

Bir rota filtre yalnızca bir kuralınız olabilir ve kural 'İzin ver' türünde olmalıdır. Bu kural, kendisiyle ilişkilendirilmiş BGP topluluk değerlerini listesini olabilir.

### <a name="1-create-a-route-filter"></a>1. Bir rota filtresi oluşturma
Yeni bir kaynak oluşturma seçeneğini seçerek bir rota filtre oluşturabilirsiniz. Tıklatın **yeni** > **ağ** > **RouteFilter**aşağıdaki görüntüde gösterildiği gibi:

![Bir rota filtresi oluşturma](.\media\how-to-routefilter-portal\CreateRouteFilter1.png)

Bir kaynak grubunda rota filtre yerleştirmeniz gerekir. 

![Bir rota filtresi oluşturma](.\media\how-to-routefilter-portal\CreateRouteFilter.png)

### <a name="2-create-a-filter-rule"></a>2. Bir filtre kuralı oluşturma

Ekleme ve rota filtre Yönet kural sekmesini seçerek kuralları güncelleştirin.

![Bir rota filtresi oluşturma](.\media\how-to-routefilter-portal\ManageRouteFilter.png)


Açılır listeden bağlanmak ve bittiğinde kuralını kaydetmek istediğiniz hizmetleri seçebilirsiniz.

![Bir rota filtresi oluşturma](.\media\how-to-routefilter-portal\AddRouteFilterRule.png)


## <a name="attach"></a>3. adım: bir expressroute bağlantı hattı için rota filtre ekleme

"Hattı Ekle" düğmesini seçerek ve expressroute bağlantı hattı açılır listeden seçerek bağlantı hattı için rota filtresi ekleyebilirsiniz.

![Bir rota filtresi oluşturma](.\media\how-to-routefilter-portal\AddCktToRouteFilter.png)

Bağlantı sağlayıcı "Hattı Ekle" düğmesini seçmeden önce ExpressRoute bağlantı hattı dikey penceresinden bağlantı hattı için ExpressRoute bağlantı hattı yenileme eşlemesini yapılandırırsa.

![Bir rota filtresi oluşturma](.\media\how-to-routefilter-portal\RefreshExpressRouteCircuit.png)

## <a name="tasks"></a>Genel görevler

### <a name="getproperties"></a>Bir rota filtre özelliklerini almak için

Portalda kaynak açtığınızda, bir rota filtre özelliklerini görüntüleyebilirsiniz.

![Bir rota filtresi oluşturma](.\media\how-to-routefilter-portal\ViewRouteFilter.png)


### <a name="updateproperties"></a>Bir rota filtre özelliklerini güncelleştirmek için

BGP topluluk değerlerini "Yönet kuralı" düğmesini seçerek bir devresine bağlı listesini güncelleştirebilirsiniz.


![Bir rota filtresi oluşturma](.\media\how-to-routefilter-portal\ManageRouteFilter.png)

![Bir rota filtresi oluşturma](.\media\how-to-routefilter-portal\AddRouteFilterRule.png) 


### <a name="detach"></a>Bir expressroute bağlantı hattı rota filtresinden ayırmak için

Rota filtre hattından ayırmak için bağlantı hattındaki sağ tıklatın ve "ilişkisini üzerinde"'i tıklatın.

![Bir rota filtresi oluşturma](.\media\how-to-routefilter-portal\DetachRouteFilter.png) 


### <a name="delete"></a>Bir rota filtreyi silmek için

Bir rota filtre Sil düğmesini seçerek silebilirsiniz. 

![Bir rota filtresi oluşturma](.\media\how-to-routefilter-portal\DeleteRouteFilter.png) 

## <a name="next-steps"></a>Sonraki Adımlar

ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).