---
title: İş Akışı Tasarımcısı ile gelişmiş kodlama iş akışları oluşturmak | Microsoft Docs
description: İş Akışı Tasarımcısı ile gelişmiş kodlama iş akışları oluşturma hakkında bilgi edinin.
services: media-services
documentationcenter: ''
author: anilmur
manager: cfowler
editor: ''
ms.assetid: 004815f2-0761-4706-87a1-675ba36e0322
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: juliako;johndeu;anilmur
ms.openlocfilehash: d75a3b58934b3da05a15700ecaf82226b549fffa
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33790303"
---
# <a name="create-advanced-encoding-workflows-with-workflow-designer"></a>İş Akışı Tasarımcısı ile Gelişmiş Kodlama İş Akışları Oluşturma
## <a name="overview"></a>Genel Bakış
**İş akışı Tasarımcısı** tasarlamak ve ile kodlamak için özel iş akışları oluşturmak için kullanılan bir Windows Masaüstü aracı **Medya Kodlayıcısı Premium iş akışı**.
İş Akışı Tasarımcısı aracını gücünü kullanarak, tasarım ve çalışacak karmaşık iş akışları oluşturmak **Medya Kodlayıcısı Premium**.  

İş akışları müşteri karar mantık içerebilir ve dallara ayırma giriş kaynak dosyanın özelliklerine bağlı. Geçersiz kılınabilir özellikleri ve yineleyin ve bulutta özelleştirmek bile en karmaşık kodlama görevleri kolaylaştırmak için dinamik değerler ile iş akışları oluşturabilirsiniz.

Oluşturabileceğiniz örnek iş akışları içerir:

* Karar çözümlemesi için kaynak içerik inceleyebilir ve yalnızca istenen çıkış parçaları kodlamak iş akışları temel.  Kaynak içerik inadvertantly upscaling tarafından oluşturulacak harcanan parçaları ortadan kaldırarak helfpul budur.
* Birden çok giriş dosyaları başlıklar, yer paylaşımları ve Dikiş birlikte içeriği desteklemek için kullanılabilir. 

Bu araç herhangi birini değiştirmek için de kullanılabilir bizim [iş akışları yayımlanan](media-services-workflow-designer.md#existing_workflows). 

> [!NOTE]
> İş Akışı Tasarımcısı aracını kopyanızı almak için lütfen başvurun mepd@microsoft.com.
> 
> 

Bir iş akışı dosyası oluşturulduktan sonra bir varlık yüklenebilir ve daha sonra medya dosyalarını kodlama için kullanılıyor. İle kodlamak hakkında bilgi için **Medya Kodlayıcısı Premium iş akışı** kullanarak **.NET**, bkz: [Medya Kodlayıcısı Premium akışıyla kodlama Gelişmiş](media-services-encode-with-premium-workflow.md).

## <a id="existing_workflows"></a>Var olan iş akışlarını değiştirin
Varsayılan [iş akışları yayımlanan](media-services-workflow-designer.md#existing_workflows) Tasarımcı aracı kullanılarak değiştirilebilir. Varsayılan iş akışı dosyalarını almak [burada](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows). Klasör, bu dosyaların açıklaması da içerir.

Aşağıdaki videoları tasarımcının nasıl kullanılacağı gösterilmektedir.

### <a name="day-1--getting-started"></a>Günde 1 – Başlarken
Günde 1 video kapsar:

* Tasarımcı genel bakış
* Temel iş akışları – "Hello World"
* Çıkış MP4 dosyaları Azure Media Services akış ile kullanım için birden çok oluşturma

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Premium-Encoder-Workflow-Designer-Training-Videos-Day-1/player]
> 
> 

### <a name="day-2"></a>Gün 2
Gün 2 video kapsar:

* Kaynak dosya senaryoları değişen – ses işleme
* Gelişmiş mantığı ile iş akışları
* Grafik aşamaları

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Premium-Encoder-Workflow-Designer-Training-Videos-Day-2/player]
> 
> 

### <a name="day-3"></a>Gün 3
Gün 3 video kapsar:

* İş akışları/planlar içinde komut dosyası oluşturma
* Geçerli Kodlayıcı ile kısıtlamaları
* SORU- CEVAP

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Premium-Encoder-Workflow-Designer-Training-Videos-Day-3/player]
> 
> 

## <a name="next-step"></a>Sonraki adım
Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

Destek veya özel iş akışları iş akışı Tasarımcısı aracında oluşturma hakkında sorularınız varsa lütfen e-posta gönderin mepd@microsoft.com.

## <a name="see-also"></a>Ayrıca Bkz.
[Azure Premium Kodlayıcı iş akışı Tasarımcısı eğitim videoları](http://johndeutscher.com/2015/07/06/azure-premium-encoder-workflow-designer-training-videos/)

