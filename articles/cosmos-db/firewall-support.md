---
title: Azure Cosmos hesapları için IP Güvenlik Duvarı
description: Güvenlik Duvarı desteği için IP erişim denetim ilkeleri kullanarak Azure Cosmos DB verileri güvenli hale getirmeyi öğrenin.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: govindk
ms.openlocfilehash: 9398eb4038afcd17788e750fcb5c27c76e9f3f44
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66241073"
---
# <a name="ip-firewall-in-azure-cosmos-db"></a>Azure Cosmos DB'de IP Güvenlik Duvarı

Hesabınızda depolanan verilerin güvenliğini sağlamak için Azure Cosmos DB kullanan bir tanımlayıcı karma tabanlı ileti kimlik doğrulama kodu (HMAC) gizli tabanlı yetkilendirme modeli destekler. Ayrıca, Azure Cosmos DB için gelen güvenlik duvarı desteği IP tabanlı erişim denetimlerini destekler. Bu model, geleneksel veritabanı sistemi güvenlik duvarı kurallarına benzer ve ek bir güvenlik hesabınıza düzeyi sağlar. Güvenlik duvarları ile erişilebilir yalnızca onaylanmış bir makine kümesinden ve/veya Bulut Hizmetleri için Azure Cosmos hesabınıza yapılandırabilirsiniz. Bu onaylı kümelerinden makineleri ve Hizmetleri Azure Cosmos veritabanında saklanan verilere erişim hala geçerli bir yetkilendirme belirteciyle sunmak çağıranın gerektirir.

## <a id="ip-access-control-overview"></a>IP erişim denetimine genel bakış

Varsayılan olarak, Azure Cosmos hesabınız isteği bir geçerli bir yetkilendirme belirteciyle birlikte sunulduğu sürece, internet'ten erişilebilir. IP ilke tabanlı erişim denetimini yapılandırmak için kullanıcının IP adresi veya IP adresi aralığı CIDR (sınıfsız etki alanları arası yönlendirme) formunda, istemci IP'leri, belirli bir Azure Cosmos hesabına erişmesi için izin verilen listesi olarak dahil edilecek dizi sağlamanız gerekir. Bu yapılandırma uygulandıktan sonra bu izin verilen liste dışındaki makinelerden gelen tüm istekler 403 (Yasak) yanıt alırsınız. IP Güvenlik Duvarı'nı kullanırken, Azure portalı hesabınıza erişmesine izin vermeniz önerilir. Erişim, hesabınız için Azure portalında görünmesi ölçümleri almak için de Veri Gezgini kullanımına olanak tanımak için gereklidir. İzin vererek, hesabınıza erişmek için Azure portalına ek olarak, Veri Gezgini'ni kullanarak, ayrıca güvenlik duvarı kuralları geçerli IP adresinizi eklemek için Güvenlik Duvarı ayarlarını güncelleştirmeniz gerekir. Güvenlik Duvarı değişikliklerinin yayılması 15 dakika sürebileceğini unutmayın. 

IP tabanlı güvenlik duvarı, alt ağ ve VNET erişim denetimiyle birleştirebilirsiniz. Bunları birleştirerek genel bir IP herhangi bir kaynağı ve/veya sanal ağ içindeki belirli bir alt ağdan erişimi sınırlayabilirsiniz. Alt ağ ve VNET tabanlı erişim denetimi kullanma hakkında daha fazla bilgi için [erişim Azure Cosmos DB kaynaklarını sanal ağlardan](vnet-service-endpoint.md).

Özetlemek gerekirse, yetkilendirme belirteci her zaman bir Azure Cosmos hesaba erişmek için gereklidir. IP Güvenlik Duvarı ve sanal ağ erişim denetimi listesi (ACL) ayarlanmadınız, Azure Cosmos hesabı yetkilendirme belirteci ile erişim sağlanabilir. IP Güvenlik Duvarı'nı veya sanal ağ ACL'leri ya da hem Azure Cosmos hesapta ayarlandıktan sonra belirttiğiniz kaynaklardan (ve yetkilendirme belirtecini) kaynaklanan yalnızca istekleri geçerli yanıtlar alın. 

## <a name="next-steps"></a>Sonraki adımlar

Ardından aşağıdaki belgeleri kullanarak hesabınız için IP Güvenlik Duvarı'nı veya sanal ağ hizmet uç noktası yapılandırabilirsiniz:

* [Azure Cosmos hesabınız için IP Güvenlik Duvarı yapılandırma](how-to-configure-firewall.md)
* [Erişim Azure Cosmos DB kaynaklarını sanal ağlardan](vnet-service-endpoint.md)
* [Sanal ağ hizmet uç noktaları Azure Cosmos hesabınız için yapılandırma](how-to-configure-vnet-service-endpoint.md)




