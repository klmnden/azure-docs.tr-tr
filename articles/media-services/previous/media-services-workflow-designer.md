---
title: İş Akışı Tasarımcısı ile gelişmiş kodlama iş akışları oluşturma | Microsoft Docs
description: İş Akışı Tasarımcısı ile gelişmiş kodlama iş akışları oluşturma hakkında bilgi edinin.
services: media-services
documentationcenter: ''
author: anilmur
manager: femila
editor: ''
ms.assetid: 004815f2-0761-4706-87a1-675ba36e0322
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako;johndeu;anilmur
ms.openlocfilehash: 0ade52d3ae9714f2b370308253e455bcde7ac7a5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60825141"
---
# <a name="create-advanced-encoding-workflows-with-workflow-designer"></a>İş Akışı Tasarımcısı ile Gelişmiş Kodlama İş Akışları Oluşturma  
## <a name="overview"></a>Genel Bakış
**İş akışı Tasarımcısı** tasarım ve kodlama ile için özel iş akışlarını oluşturmak için kullanılan bir Windows Masaüstü aracı olan **Media Encoder Premium iş akışı**.
İş Akışı Tasarımcısı araç gücünü kullanarak, tasarım ve çalışacak karmaşık iş akışları oluşturmak **Medya Kodlayıcısı Premium**.  

Giriş kaynak dosyanın özelliklerine bağlı dallanma ve iş akışları müşteri kararı mantığına içerebilir. Geçersiz kılınabilir özellikleri ve hatta en karmaşık kodlama görevleri yineleyin ve bulutta özelleştirmek kolay hale getirmek için dinamik değerler ile iş akışları oluşturabilirsiniz.

Örnek iş akışları oluşturabileceğiniz şunlardır:

* Çözüm için kaynak içeriği inceleyin ve istenen çıkış parçaları kodlama iş akışları karar temel.  Kaynak içeriği yanlışlıkla upscaling tarafından oluşturulacak harcanan parçaları ortadan kaldırarak bu yararlı olur.
* Birden fazla giriş dosyası, açıklamalı alt yazılar, yer paylaşımları ve birleştirme birlikte içeriği desteklemek için kullanılabilir. 

Bu araç herhangi birini değiştirmek için de kullanılabilir bizim [iş akışları yayımlanan](media-services-workflow-designer.md#existing_workflows). 

> [!NOTE]
> İş Akışı Tasarımcısı araç kopyasını almak için lütfen başvurun mepd@microsoft.com.

Bir iş akışı dosyası oluşturulduktan sonra bir varlık yüklenebilir ve ardından medya dosyalarını kodlama için kullanılabilir. İle kodlama hakkında bilgi için **Media Encoder Premium iş akışı** kullanarak **.NET**, bkz: [Gelişmiş Media Encoder Premium iş akışı ile kodlama](media-services-encode-with-premium-workflow.md).

## <a id="existing_workflows"></a>Mevcut iş akışlarını değiştirin
Varsayılan [iş akışları yayımlanan](media-services-workflow-designer.md#existing_workflows) tasarımcı araç kullanılarak değiştirilebilir. Varsayılan iş akışı dosyalarını alabilirsiniz [burada](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows). Klasör, bu dosyaların açıklaması da içerir.

Aşağıdaki videolarda tasarımcısının nasıl kullanılacağını göstermektedir.

### <a name="day-1--getting-started"></a>Günlük 1 – kullanmaya başlama
1. gün video kapsar:

* Tasarımcı genel bakış
* Temel iş akışları – "Hello World"
* Çıkış MP4 dosyalarını Azure Media Services akış ile kullanım için birden çok oluşturma

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Premium-Encoder-Workflow-Designer-Training-Videos-Day-1/player]
> 
> 

### <a name="day-2"></a>2. gün
2. gün video kapsar:

* Kaynak dosya senaryoları değişen – ses işleme
* Gelişmiş mantık ile iş akışları
* Graf aşamaları

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Premium-Encoder-Workflow-Designer-Training-Videos-Day-2/player]
> 
> 

### <a name="day-3"></a>3. günde
3. günde video kapsar:

* İş akışları/şemaları içinde betik oluşturma
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

Destek ihtiyaç veya özel iş akışlarını iş akışı Tasarımcısı aracında oluşturma hakkında sorularınız varsa lütfen e-posta gönderin mepd@microsoft.com.

## <a name="see-also"></a>Ayrıca Bkz.
[Azure Premium Kodlayıcı iş akışı Tasarımcısı eğitim videoları](http://johndeutscher.com/2015/07/06/azure-premium-encoder-workflow-designer-training-videos/)

