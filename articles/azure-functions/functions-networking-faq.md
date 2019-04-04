---
title: Azure işlevleri de ağ oluşturmayla ilgili sık sorulan sorular
description: Bazı yaygın sorular ve Azure işlevleri ile ağ iletişimi senaryoları yanıtlar.
services: functions
author: alexkarcher-msft
manager: jehollan
ms.service: azure-functions
ms.topic: troubleshooting
ms.date: 2/26/2019
ms.author: alkarche
ms.openlocfilehash: 7946b7f45ff3df9225a27b70ccfbdb895bfd03c4
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58896489"
---
# <a name="frequently-asked-questions-about-networking-in-azure-functions"></a>Azure işlevleri de ağ oluşturmayla ilgili sık sorulan sorular

Ağ sık sorulan soruların bir listesi aşağıdadır. Daha kapsamlı bir bakış için okuma [ağ seçenekleri belge işlevleri](functions-networking-options.md)

## <a name="how-do-i-set-a-static-ip-in-functions"></a>Statik IP işlevleri nasıl ayarlayabilirim?

Bir işlev bir App Service ortamı (ASE) dağıtımı şu anda işleviniz için bir statik gelen ve giden IP tek yoludur. Bir ASE kullanma hakkında daha fazla ayrıntı için buraya makale ile başlayın: [Oluşturma ve ILB ASE kullanma](../app-service/environment/create-ilb-ase.md).

## <a name="how-do-i-restrict-internet-access-to-my-function"></a>My işlevi için Internet erişimi nasıl kısıtlarım?

Aşağıda listelenen şekilde çeşitli internet erişimini kısıtlayabilirsiniz.

* [IP kısıtlamaları](../app-service/app-service-ip-restrictions.md): gelen trafik işlev uygulamanıza IP aralığına göre kısıtlama.
* Tüm HTTP Tetikleyicileri kaldırın. Bazı uygulamalar için yalnızca HTTP Tetikleyicileri önlemek ve işlevinizi tetikleyecek şekilde başka bir olay kaynağı kullanmak yeterli olur.

Bunu yaparken en önemli konu, Azure portal Düzenleyicisi'ni kullanmak için çalışan işlevinizi doğrudan erişim gerektiren aklınızda bulundurun sağlamaktır. Herhangi bir kod değişikliği Azure portalı üzerinden kendi IP beyaz listeye sağlamak için portalın göz atmak için kullanmakta olduğunuz cihaz gerektirir. Ancak platform Özellikleri sekmesi altındaki her şeyi yerinde ağ kısıtlamaları ile kullanmaya devam edebilirsiniz.

## <a name="how-do-i-restrict-my-function-app-to-a-vnet"></a>Bir sanal ağa işlevi uygulamamı nasıl kısıtlarım?

Bir işlev bir sanal ağ tüm trafik akışları gibi tamamen kısıtlamak için tek yolu kullanmaktır bir dahili olarak yük dengeli (ILB) App Service ortamı (ASE). Bu seçenek, bir sanal ağ içinde ayrılmış altyapı sitenizde dağıtır ve tüm tetikleyiciler ve sanal ağ üzerinden geçen trafik gönderir. Bir ASE kullanma hakkında daha fazla ayrıntı için buraya makale ile başlayın: [Oluşturma ve ILB ASE kullanma](../app-service/environment/create-ilb-ase.md).

## <a name="how-can-i-access-resources-in-a-vnet-from-a-function-app"></a>Bir vnet'teki kaynakların bir işlev uygulamasından nasıl erişebilirim?

VNET tümleştirmesi kullanarak çalışan bir işlevden bir sanal ağ içindeki kaynaklara erişebilir. Daha fazla bilgi için [VNET tümleştirmesi](functions-networking-options.md#vnet-integration)

## <a name="how-do-i-access-resources-protected-by-service-endpoints"></a>Hizmet uç noktaları tarafından korunan kaynaklara nasıl erişim sağlanır?

(Şu anda Önizleme aşamasında) kullanarak yeni sanal ağ tümleştirmesi hizmeti güvenli hale getirilmiş kaynaklara çalışan bir işlevden uç noktası erişebilir. Daha fazla bilgi için [sanal ağ tümleştirmesi Önizleme](functions-networking-options.md#preview-vnet-integration).

## <a name="how-can-i-trigger-a-function-from-a-resource-in-a-vnet"></a>Bir kaynağı bir sanal ağ içindeki bir işlevden nasıl tetiklenebilir mi?

Bir vnet'teki bir kaynaktan bir işlev, işlev uygulamanızı bir App Service ortamı için dağıtarak yalnızca tetikleyebilirsiniz. Bir ASE kullanma hakkında daha fazla bilgi için bkz [oluşturma ve ILB ASE kullanır](../app-service/environment/create-ilb-ase.md).


## <a name="how-can-i-deploy-my-function-app-in-a-vnet"></a>İşlev uygulamamı bir vnet'teki nasıl dağıtabilir miyim?

App Service ortamı için dağıtımı ILB ASE kullanma hakkında bir VNET için ayrıntıları içinde tamamen olan bir işlev uygulaması oluşturmak için burada makalesiyle başlayın tek yoludur: [Oluşturma ve ILB ASE kullanma](https://docs.microsoft.com/azure/app-service/environment/create-ilb-ase).

Yalnızca gereken ağ kaynakları için veya kapsamlı ağ yalıtımı daha az tek yönlü erişimi senaryoları için bkz: [ağ bağlantısına genel bakış işlevleri](functions-networking-options.md).
