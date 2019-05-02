---
title: VMware çözümüyle CloudSimple - Azure ağ bağlantıları
description: Azure sanal ağınız CloudSimple bölge ağınıza bağlama hakkında bilgi edinin
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 8ea98d6493b824bfa232ef8193388e93b97c506b
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2019
ms.locfileid: "64577008"
---
# <a name="azure-network-connection-overview"></a>Azure ağ bağlantısına genel bakış

Bir bölgede bir CloudSimple hizmet oluşturduğunuzda bunu:

* Azure ExpressRoute bağlantı hattı oluşturur ve bu bölgede hizmeti ekler
* Azure sanal ağınız veya şirket içi ağınızı Azure ExpressRoute kullanarak CloudSimple bölge ağınıza bağlanır.
* Azure aboneliğiniz ya da şirket içi ağınızdan, özel bulut ortamınızda çalışan erişim hizmetleri sağlar

Düşük gecikme süresi ile yüksek bant genişliği bağlantısıdır.

## <a name="benefits"></a>Avantajlar

Azure ağ bağlantısı sağlar:

* Azure sanal makineleri, özel bulut için bir yedekleme hedefi olarak kullanın.
* KMS sunucuları, özel bulut vSAN veri deposunu şifrelemek için Azure aboneliğinize dağıtın.
* Burada, uygulamanın web katmanı genel bulutta sırasında uygulama çalışır ve veritabanı katmanları, özel bulutta çalışan karma uygulamalar'ı kullanın.

## <a name="expressroute-connection-to-on-premises-network"></a>Şirket içi bir ağı ExpressRoute bağlantısı

Var olan Azure ExpressRoute devreniz CloudSimple bölgenizi bağlanabilirsiniz. ExpressRoute Global erişim özelliği, iki bağlantı hatlarının birbirleri ile bağlanmak için kullanılır.  CloudSimple ExpressRoute bağlantı hatları ve şirket içi arasında bağlantı kurulur.

Bu yöntem olduğundan iki ortam arasında bağlantı kurar:

* Güvenlik
* Özel
* Yüksek bant genişliği
* Düşük gecikme süresi

Bir şirket içi ağı bir ExpressRoute bağlantısı oluşturmak için [desteğe](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Sonraki adımlar

* [Sanal ağ bağlantısı kurma](https://docs.azure.cloudsimple.com/virtual-network-connection)