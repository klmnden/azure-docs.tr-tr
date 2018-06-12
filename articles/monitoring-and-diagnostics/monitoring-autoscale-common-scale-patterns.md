---
title: Sık kullanılan otomatik ölçeklendirme desenlerini genel bakış
description: Bazı ölçeği otomatik olarak ortak desenler kaynağınız Azure öğrenin.
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/07/2017
ms.author: ancav
ms.component: autoscale
ms.openlocfilehash: 84727ec3694f64d40ad002a248a255df9074d7f4
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35263272"
---
# <a name="overview-of-common-autoscale-patterns"></a>Sık kullanılan otomatik ölçeklendirme desenlerini genel bakış
Bu makalede Azure kaynağınız ölçeklendirmek için ortak desenler bazıları açıklanmaktadır.

Azure İzleyici otomatik ölçek yalnızca sanal makine ölçek kümeleri (VMSS), bulut Hizmetleri, uygulama hizmeti planları ve app service ortamları için geçerlidir. 

# <a name="lets-get-started"></a>Sağlar kullanmaya başlama

Bu makalede, otomatik ölçekli bilgi sahibi olduğunuzu varsayar. Yapabilecekleriniz [kaynağınız ölçeklendirmek için buradan başlayın][1]. Ortak ölçek desenler bazıları şunlardır:

## <a name="scale-based-on-cpu"></a>CPU üzerinde göre ölçeklendirin

Bir web uygulaması (/ VMSS/bulut hizmeti rolü) sahip ve 

- İstediğiniz ölçek genişletme/ölçek dayalı olarak CPU.
- Ayrıca, en az bir örnek sayısı yoktur sağlamak istiyorsunuz. 
- Ayrıca, için ölçeklendirebilirsiniz örnek sayısı maksimum bir sınır koymak sağlamak istiyorsunuz.

![CPU üzerinde göre ölçeklendirin][2]

## <a name="scale-differently-on-weekdays-vs-weekends"></a>Haftanın günü vs hafta sonu farklı ölçeklendirme

Bir web uygulaması (/ VMSS/bulut hizmeti rolü) sahip ve

- Varsayılan olarak (günlerinde) 3 örneklerinin istediğiniz
- Hafta sonu trafiği beklemeyen ve hafta sonu aşağıya doğru 1 örneği ölçeklendirmek istiyorsanız bu nedenle.

![Haftanın günü vs hafta sonu farklı ölçeklendirme][3]

## <a name="scale-differently-during-holidays"></a>Farklı tatilleri ölçeklendirme

Bir web uygulaması (/ VMSS/bulut hizmeti rolü) sahip ve 

- Varsayılan olarak CPU kullanımı dikkate alarak yukarı/aşağı ölçeklendirmek istediğiniz
- Ancak, tatilde (veya işletmeniz için önemli olan belirli gün) sırasında varsayılanlarını geçersiz kıl ve daha fazla kapasite elinizin ister.

![Ölçek farklı üzerinde tatiller][4]

## <a name="scale-based-on-custom-metric"></a>Özel bir ölçü göre ölçeklendirin

Bir web ön uç ve arka uç ile iletişim kuran bir API katmanı vardır. 

- Özel olaylar ön uçtaki temel API katmanı istediğiniz (örnek: alışveriş sepeti öğelerinin sayısına dayalı olarak kullanıma alma işleminizi ölçeklendirmek istediğiniz)

![Özel bir ölçü göre ölçeklendirin][5]

<!--Reference-->
[1]: ./monitoring-autoscale-get-started.md
[2]: ./media/monitoring-autoscale-common-scale-patterns/scale-based-on-cpu.png
[3]: ./media/monitoring-autoscale-common-scale-patterns/weekday-weekend-scale.png
[4]: ./media/monitoring-autoscale-common-scale-patterns/holidays-scale.png
[5]: ./media/monitoring-autoscale-common-scale-patterns/custom-metric-scale.png