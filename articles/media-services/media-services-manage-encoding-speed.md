---
title: "Hız ve Azure Media Services ile kodlama eşzamanlılık yönetme | Microsoft Docs"
description: "Bu makalede hızı ve Azure Media Services ile kodlama işleri/görevleri eşzamanlılığı nasıl yönetebilmeniz için kısa bir genel bakış sunulmaktadır."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 676313f8-a158-4e3a-a99b-2c29a341ecc9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2017
ms.author: juliako
ms.openlocfilehash: 0463904fd9bf1138587d0d214e572ddd38cc2184
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
#  <a name="manage-speed-and-concurrency-of-your-encoding"></a>Hız ve, kodlama eşzamanlılık yönetme

Bu makalede hızı ve kodlama işleri/görevlerinizi eşzamanlılığı nasıl yönetebilmeniz için kısa bir genel bakış sunulmaktadır.

## <a name="overview"></a>Genel Bakış

Media Services, bir **ayrılmış birim türü** ile görevleri işleme medyanızı işlenir hızını belirler. Şu ayrılmış birim türlerinden birini seçebilirsiniz: **S1**, **S2** veya **S3**. Örneğin, aynı kodlama işi **S2** ayrılmış birim türünü kullandığınızda **S1** türüne göre daha hızlı çalışır. [Kodlama birimleri ölçeklendirme](media-services-scale-media-processing-overview.md) konu arasında farklı kodlama hızları seçerken karar vermenize yardımcı olan bir tablo gösterir.

Ayrılmış birim türü belirtmeye ek olarak, hesabınızla sağlayacak belirtebilirsiniz **ayrılan birimler**. Sağlanan ayrılmış birim sayısı, verili bir hesapta eşzamanlı olarak işlenebilecek medya görevlerinin sayısını belirler. Örneğin, beş medya görevler eşzamanlı olarak kadar uzun çalışıyor olacak beş ayrılmış birimler, hesabınız varsa, olarak işlenecek görevler vardır. Kalan görevler sıraya bekler ve sıralı olarak çalışan bir görev tamamlandığında işlemek için toplanmış. Bir hesap sağlanan tüm ayrılan birimler yoksa, ardından görevleri sırayla çekilir. Bu durumda, sonraki bir başlangıç ve bitiş görev arasındaki bekleme süresine sistemde kaynakları kullanılabilirliğine bağlı olacaktır.

Ayrıntılı bilgi ve kodlama birimleri ölçeklendirme gösteren örnekler için bkz: [bu](media-services-scale-media-processing-overview.md) konu.

## <a name="next-step"></a>Sonraki adım

[Kodlama ölçek birimleri](media-services-scale-media-processing-overview.md)

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

