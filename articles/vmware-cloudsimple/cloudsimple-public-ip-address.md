---
title: VMware çözümüyle CloudSimple - Azure genel IP adresi
description: Genel IP adresleri ve VMware çözümü CloudSimple tarafından avantajlarına hakkında bilgi edinin
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: f57b7397f4a2d288cd2b8b55cf23b2d635aa5f8c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65209558"
---
# <a name="cloudsimple-public-ip-address-overview"></a>CloudSimple genel IP adresi genel bakış

Genel bir IP adresi iletişim kurmak internet kaynakları özel bulut kaynaklarına özel bir IP adresinden gelen sağlar. Bir sanal makine veya bir yazılım yük dengeleyicisinin özel IP adresi var. Özel IP adresi, özel bulut vCenter ' dir. Genel IP adresini internet'e özel bulutunuzda çalışan hizmetleri kullanıma sunmanıza olanak sağlar.

Bunu atamasını kadar genel IP adresi için özel IP adresi ayrılır. Genel bir IP adresi, yalnızca bir özel IP adresine atanabilir.

Her zaman bir genel IP adresiyle ilişkili bir kaynak Internet erişimi için genel IP adresini kullanır. Varsayılan olarak, yalnızca giden internet erişimi genel bir IP adresi izin verilir.  Genel IP adresine gelen trafik reddedilir.  Gelen trafiğe izin veren belirli bağlantı noktası genel IP adresi için bir güvenlik duvarı kuralı oluşturun.

## <a name="benefits"></a>Avantajlar

Gelen iletişim kurmak için bir genel IP adresi kullanılması sağlar:

* Hizmet (DDoS) saldırısını önlemeyi dağıtılmış engelleme. Bu koruma için genel IP adresini otomatik olarak etkinleştirilir.
* Her zaman açık trafik izleme ve gerçek zamanlı azaltma ortak ağ düzeyinde saldırı. Bu savunma Microsoft online services tarafından kullanılan aynı savunmaları ' dir.
* Azure genel ağ tüm ölçek. Ağ, dağıtmak ve bölgeler arasında saldırı trafiği azaltmak için kullanılabilir.  

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [genel bir IP adresi ayırmayı](https://docs.azure.cloudsimple.com/public-ips/)
