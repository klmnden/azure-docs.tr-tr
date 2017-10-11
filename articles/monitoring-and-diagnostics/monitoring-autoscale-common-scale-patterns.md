---
title: "Sık kullanılan otomatik ölçeklendirme desenlerini genel bakış | Microsoft Docs"
description: "Bazı ölçeği otomatik olarak ortak desenler kaynağınız Azure öğrenin."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d37d3fda-8ef1-477c-a360-a855b418de84
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: ancav
ms.openlocfilehash: fce51546e041c8989d813c3935e058c52b38ba77
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
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