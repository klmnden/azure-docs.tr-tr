---
title: Azure Stack fiziksel cihaz denetimi
description: Fiziksel cihaz Azure Stack'te erişim denetim tümleştirmeyi öğrenin
services: azure-stack
author: PatAltimore
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 08/01/2018
ms.author: patricka
ms.reviewer: fiseraci
keywords: ''
ms.openlocfilehash: 459cdf4e1a70ee02d818dd6abe101e4fc3475b68
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2018
ms.locfileid: "40034801"
---
# <a name="azure-stack-datacenter-integration---physical-device-auditing"></a>Azure Stack veri merkezi tümleştirmesi - fiziksel cihazı denetleme

Azure stack'teki temel kart yönetim denetleyicileri (Bmc'ler) ve ağ anahtarları gibi tüm fiziksel cihazlar denetim günlüklerini yayma ve genel denetim çözümünüze tümleşik. Cihazların farklı Azure Stack OEM donanım satıcıları arasında değişir olduğundan, tümleştirme denetim belgeleri için satıcınıza başvurun. Aşağıdaki bölümlerde, Azure yığını ' fiziksel cihaz denetimi için bazı genel bilgileri sağlayın.  

## <a name="physical-device-access-auditing"></a>Fiziksel cihaz erişimini denetleme

Azure stack'teki tüm fiziksel cihazlar TACACS veya RADIUS kullanımını destekler. Bu, ağ anahtarlarını ve temel kart yönetim denetleyicisine (BMC) erişim içerir.

Azure Stack çözümleri, RADIUS veya TACACS yerleşik olarak bulunmaz. Ancak, çözümler piyasadaki mevcut RADIUS veya TACACS çözüm kullanımını desteklemek üzere doğrulandı.

RADIUS için yalnızca MSCHAPv2 doğrulandı. Bu, RADIUS kullanan en güvenli uygulamasını temsil eder.
Azure Stack çözümünüzle birlikte dahil edilen cihazlar TACAS veya RADIUS etkinleştirmek için OEM donanım satıcınıza başvurun.

## <a name="syslog-forwarding-for-network-devices"></a>Ağ aygıtları için Syslog iletmeyi

Azure stack'teki tüm fiziksel ağ cihazları, syslog iletileri destekler. Syslog sunucusu ile Azure Stack çözümleri bulunmaz. Ancak, cihazlar piyasadaki mevcut syslog çözümleri iletileri gönderme desteklemek üzere doğrulandı.

İsteğe bağlı parametresi dağıtım için toplanan syslog hedef adresidir ancak dağıtım sonrası da eklenebilir. Ağ cihazlarınızda iletme syslog'u yapılandırmak için OEM donanım satıcınıza başvurun.

## <a name="next-steps"></a>Sonraki adımlar

[Hizmet İlkesi](azure-stack-servicing-policy.md)
