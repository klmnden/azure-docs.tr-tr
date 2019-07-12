---
title: VMware çözümü CloudSimple hizmet tarafından
description: Genel Bakış CloudSimple hizmeti ve kavramlarını açıklar.
author: sharaths-cs
ms.author: b-shsury
ms.date: 04/24/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 0bebc68129ee2ff79241bcefec1ce0c3ca0b472d
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67595308"
---
# <a name="cloudsimple-service-overview"></a>CloudSimple hizmetine genel bakış

CloudSimple hizmetiyle CloudSimple tarafından Azure VMware çözümü kullanabilirsiniz. Hizmeti oluşturduktan sonra düğümleri sağlama, düğümler ayırmak ve özel Bulutlar oluştur. CloudSimple hizmet CloudSimple hizmet kullanılabilir olduğu her Azure bölgesinde oluşturun. Hizmet, Azure VMware CloudSimple çözümüyle uç ağı tanımlar. Edge ağını, VPN, Azure ExpressRoute ve internet bağlantısı için özel bulutlarınızın Hizmetleri destekler.

## <a name="gateway-subnet"></a>Ağ geçidi alt ağı

Bir ağ geçidi alt ağı CloudSimple hizmeti başına gereklidir ve da oluşturulduğu bölgesiyle benzersizdir. Ağ geçidi alt ağı/28 gerektirir ve edge ağını oluşturduğunuzda kullanılan CIDR bloğu. Ağ geçidi alt ağ adres alanı benzersiz olmalıdır. CloudSimple ortamıyla iletişim kurduğu herhangi bir ağ ile örtüşmemelidir. Şirket içi ağlar ve Azure sanal ağları CloudSimple ile iletişim kuran ağlar içerir. Oluşturulduktan sonra ağ geçidi alt ağı silinemiyor. Ağ geçidi alt ağı hizmet silindikten kaldırılır.

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [Azure'da CloudSimple hizmet oluşturma](quickstart-create-cloudsimple-service.md).
