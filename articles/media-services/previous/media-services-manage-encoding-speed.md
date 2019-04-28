---
title: Hız ve Azure Media Services ile kodlama eşzamanlılık yönetme | Microsoft Docs
description: Bu makalede, hız ve kodlama işleri/görevlerinize Azure Media Services ile eşzamanlılığı nasıl yönetebileceğinizi kısa genel bakış sağlar.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 676313f8-a158-4e3a-a99b-2c29a341ecc9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 6bcaadc8dd61899aff860ad246e30170c99ec0f6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61463761"
---
#  <a name="manage-speed-and-concurrency-of-your-encoding"></a>Kodlamanızın hızını ve eşzamanlılığını yönetme  

Bu makalede, hız ve kodlama işleri/görevleri eşzamanlılığı nasıl yönetebileceğinizi kısa genel bakış sağlar.

## <a name="overview"></a>Genel Bakış

Medya Hizmetleri'nde bir **ayrılmış birim türünü** ile medya işleme görevlerinizin işlenme hızını belirler. Şu ayrılmış birim türlerinden seçebilirsiniz: **S1**, **S2**, veya **S3**. Örneğin, aynı kodlama işi **S2** ayrılmış birim türünü kullandığınızda **S1** türüne göre daha hızlı çalışır. [Kodlama birimleri ölçeklendirme](media-services-scale-media-processing-overview.md) konu farklı kodlama hızlarını arasında seçim yaparken kararı vermenize yardımcı olan bir tablo gösterir.

Ayrılmış birim türünü belirtmenin yanı sıra, hesabınızla sağlamak için belirtebileceğiniz **ayrılmış birim**. Sağlanan ayrılmış birim sayısı, verili bir hesapta eşzamanlı olarak işlenebilecek medya görevlerinin sayısını belirler. Örneğin, beş medya görevi aynı anda uzun çalışacağı beş ayrılmış birim, hesabınız varsa, olarak işlenmek üzere görevleri vardır. Kalan görevlerin kuyrukta bekler ve sıralı olarak çalışan bir görev tamamlandığında işlemek için toplanmış. Sağlanan herhangi bir ayrılmış birim hesabınız yoksa, ardından görevleri sıralı olarak seçilir. Bu durumda, bir görev tamamlama ve ileri bir başlangıç arasındaki bekleme süresini sistemde kaynaklarının kullanılabilirliğine bağlıdır.

Ayrıntılı bilgi ve kodlama birimleri ölçeklendirme gösteren örnekler için bkz: [bu](media-services-scale-media-processing-overview.md) konu.

## <a name="next-step"></a>Sonraki adım

[Kodlama birimleri ölçeklendirme](media-services-scale-media-processing-overview.md)

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

