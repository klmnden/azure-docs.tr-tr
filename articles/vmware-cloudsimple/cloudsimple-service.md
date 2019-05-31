---
title: VMware çözümler CloudSimple - hizmet tarafından
description: Genel Bakış CloudSimple hizmeti ve kavramlarını açıklar.
author: sharaths-cs
ms.author: b-shsury
ms.date: 4/24/19
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: f7b4be0ff3997e27dd5b5321dd44b5006ae52102
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66358116"
---
# <a name="cloudsimple-service-overview"></a>CloudSimple hizmetine genel bakış

CloudSimple service, Azure VMware CloudSimple çözümüyle kullanmasını sağlar.  Hizmet oluşturma, düğümleri satın, düğümler ayırmak ve özel Bulutlar oluşturmak olanak tanır.  CloudSimple hizmet CloudSimple hizmet kullanılabilir olduğu her Azure bölgesinde oluşturun.  Hizmet, Azure VMware CloudSimple çözümüyle uç ağı tanımlar.  Edge ağını, VPN ve ExpressRoute özel Bulutlarınızın internet bağlantısı hizmetlerini destekler.

## <a name="gateway-subnet"></a>Ağ geçidi alt ağı

Bir ağ geçidi alt ağı CloudSimple hizmeti başına gereklidir ve da oluşturulduğu bölgesiyle benzersizdir. Ağ geçidi alt ağı/28 gerektirir ve edge ağ oluştururken kullanılır CIDR bloğu.  Ağ geçidi alt ağ adres alanı benzersiz olmalıdır. CloudSimple ortamıyla iletişim kurduğu herhangi bir ağ ile örtüşmemelidir. CloudSimple ile iletişim kuran ağlar, şirket içi ağlar ve Azure sanal ağı içerir.  Oluşturulduktan sonra ağ geçidi alt ağı silinemiyor.  Ağ geçidi alt ağı hizmet silindikten kaldırılır.

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [Azure'da CloudSimple hizmeti oluşturma](quickstart-create-cloudsimple-service.md)