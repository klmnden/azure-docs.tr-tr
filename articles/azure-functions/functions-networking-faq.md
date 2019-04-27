---
title: Azure işlevleri'nde ağ hakkında sık sorulan sorular
description: Bazı yaygın sorular ve Azure işlevleri ile ağ iletişimi senaryoları yanıtlar.
services: functions
author: alexkarcher-msft
manager: jeconnoc
ms.service: azure-functions
ms.topic: troubleshooting
ms.date: 4/11/2019
ms.author: alkarche, glenga
ms.openlocfilehash: 3cf6a0d080e2d8cafcab8e69a614b59a470c7aba
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60637055"
---
# <a name="frequently-asked-questions-about-networking-in-azure-functions"></a>Azure işlevleri'nde ağ hakkında sık sorulan sorular

Bu makalede, Azure işlevleri'nde ağ hakkında sık sorulan sorular listelenmektedir. Daha kapsamlı bir bakış için bkz: [ağ seçeneklerini işlevleri](functions-networking-options.md).

## <a name="how-do-i-set-a-static-ip-in-functions"></a>Statik IP işlevleri nasıl ayarlayabilirim?

Bir App Service ortamında bir işlev dağıtımı şu anda işleviniz için bir statik gelen ve giden IP tek yoludur. Bir App Service ortamını kullanma hakkında daha fazla ayrıntı için makale ile başlayın [oluşturma ve kullanma bir App Service ortamı ile iç yük dengeleyici](../app-service/environment/create-ilb-ase.md).

## <a name="how-do-i-restrict-internet-access-to-my-function"></a>My işlevi için internet erişimi nasıl kısıtlarım?

Çeşitli şekillerde Internet erişimi kısıtlayabilirsiniz:

* [IP kısıtlamaları](../app-service/app-service-ip-restrictions.md): Gelen trafik için işlev uygulamanızın IP aralığına göre kısıtlayın.
* Tüm HTTP Tetikleyicileri kaldırılması. Bazı uygulamalar için yalnızca HTTP Tetikleyicileri önlemek ve işlevinizi tetikleyecek şekilde başka bir olay kaynağı kullanmak için yeterlidir.

Azure portal Düzenleyicisi çalışan işlevinize doğrudan erişimi gerektiğini aklınızda bulundurun. Herhangi bir kod değişikliği Azure portalı üzerinden kendi IP beyaz listeye sağlamak için portalın göz atmak için kullanmakta olduğunuz cihaz gerektirir. Ancak, platform Özellikleri sekmesi altındaki her şeyi yerinde ağ kısıtlamaları ile kullanmaya devam edebilirsiniz.

## <a name="how-do-i-restrict-my-function-app-to-a-virtual-network"></a>Bir sanal ağa işlevi uygulamamı nasıl kısıtlarım?

Bir işlev aracılığıyla bir sanal ağ tüm trafik akışları gibi tamamen kısıtlamak için tek yolu, yük dengeli bir App Service ortamı kullanmaktır. Bu seçenek, bir sanal ağ içindeki özel bir altyapı sitenizde dağıtır ve tüm tetikleyiciler ve trafiği sanal ağ üzerinden gönderir. 

Bir App Service ortamını kullanma hakkında daha fazla ayrıntı için makale ile başlayın [oluşturma ve kullanma bir App Service ortamı ile iç yük dengeleyici](../app-service/environment/create-ilb-ase.md).

## <a name="how-can-i-access-resources-in-a-virtual-network-from-a-function-app"></a>Bir sanal ağ içindeki kaynaklarla bir işlev uygulamasından nasıl erişebilirim?

Sanal ağ Tümleştirmesi'ni kullanarak bir sanal ağ içindeki kaynaklarla çalışan bir işlevden erişebilirsiniz. Daha fazla bilgi için [sanal ağ tümleştirmesi](functions-networking-options.md#virtual-network-integration).

## <a name="how-do-i-access-resources-protected-by-service-endpoints"></a>Hizmet uç noktaları tarafından korunan kaynaklara nasıl erişim sağlanır?

(Şu anda Önizleme aşamasında) kullanarak sanal ağ tümleştirmesi, çalışan bir işlevden hizmet uç noktası-güvenlikli kaynaklara erişebilir. Daha fazla bilgi için [sanal ağ tümleştirmesi Önizleme](functions-networking-options.md#preview-version-of-virtual-network-integration).

## <a name="how-can-i-trigger-a-function-from-a-resource-in-a-virtual-network"></a>Bir kaynağı bir sanal ağdaki bir işlevden nasıl tetiklenebilir mi?

Bir sanal ağdaki bir kaynaktan bir işlev, işlev uygulamanızı bir App Service ortamı için yalnızca dağıtarak tetikleyebilirsiniz. Bir App Service ortamını kullanma hakkında daha fazla bilgi için bkz [oluşturma ve kullanma bir App Service ortamı ile iç yük dengeleyici](../app-service/environment/create-ilb-ase.md).


## <a name="how-can-i-deploy-my-function-app-in-a-virtual-network"></a>İşlev uygulamamı bir sanal ağdaki nasıl dağıtabilir miyim?

App Service ortamı için dağıtma, sanal ağ içinde tamamen olan bir işlev uygulaması oluşturmak için tek bir yoludur. Bir App Service ortamı ile iç yük dengeleyici kullanma hakkında daha fazla ayrıntı için makale ile başlayın [oluşturma ve kullanma bir App Service ortamı ile iç yük dengeleyici](https://docs.microsoft.com/azure/app-service/environment/create-ilb-ase).

Yalnızca tek yönlü erişim için sanal ağ kaynaklarına veya kapsamlı ağ yalıtımı daha az duyduğunuz senaryolara senaryoları için bkz: [ağ bağlantısına genel bakış işlevleri](functions-networking-options.md).

## <a name="next-steps"></a>Sonraki adımlar

Ağ ve işlevleri hakkında daha fazla bilgi edinmek için: 

* [Sanal ağ Tümleştirmesi ile çalışmaya başlama hakkında bir öğretici uygulayın](./functions-create-vnet.md)
* [Azure işlevleri içindeki ağ seçenekleri hakkında daha fazla bilgi edinin](./functions-networking-options.md)
* [App Service ve işlevler ile sanal ağ tümleştirmesi hakkında daha fazla bilgi edinin](../app-service/web-sites-integrate-with-vnet.md)
* [Azure'daki sanal ağlar hakkında daha fazla bilgi edinin](../virtual-network/virtual-networks-overview.md)
* [Daha fazla ağ özellikleri ve App Service ortamları ile denetimi etkinleştir](../app-service/environment/intro.md)
