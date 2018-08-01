---
title: Genel Bakış medya işlemeyi ölçeklendirme | Microsoft Docs
description: Bu konuda, ölçeklendirme medya işleme Azure Media Services ile bir genel bakıştır.
services: media-services
documentationcenter: ''
author: juliako
manager: cfowler
editor: ''
ms.assetid: 780ef5c2-3bd6-4261-8540-6dee77041387
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2018
ms.author: juliako
ms.openlocfilehash: 81fab8903c0101d0e4aae8a392f05129651cd762
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39369145"
---
# <a name="scaling-media-processing-overview"></a>Genel Bakış medya işlemeyi ölçeklendirme
Bu sayfa hakkında genel bakış ve neden medya işlemeyi ölçeklendirme sağlar. 

## <a name="overview"></a>Genel Bakış
Media Services hesabı bir Ayrılmış Birim Türüyle ilişkilendirilir ve bu da medya işleme görevlerinizin ne hızda işleneceğini belirler. Şu ayrılmış birim türlerinden birini seçebilirsiniz: **S1**, **S2** veya **S3**. Örneğin, aynı kodlama işi **S2** ayrılmış birim türünü kullandığınızda **S1** türüne göre daha hızlı çalışır. Daha fazla bilgi için [ayrılmış birim türlerinden](https://azure.microsoft.com/blog/high-speed-encoding-with-azure-media-services/).

Ayrılmış birim türünü belirtmenin yanı sıra, ayrılmış birim ile hesabınızı sağlamak için belirtebilirsiniz. Sağlanan ayrılmış birim sayısı, verili bir hesapta eşzamanlı olarak işlenebilecek medya görevlerinin sayısını belirler. Örneğin, beş medya görevi aynı anda uzun çalışacağı beş ayrılmış birim, hesabınız varsa, olarak işlenmek üzere görevleri vardır. Kalan görevlerin kuyrukta bekler ve sıralı olarak çalışan bir görev tamamlandığında işlemek için toplanmış. Sağlanan herhangi bir ayrılmış birim hesabınız yoksa, ardından görevleri sıralı olarak seçilir. Bu durumda, bir görev tamamlama ve ileri bir başlangıç arasındaki bekleme süresini sistemde kaynaklarının kullanılabilirliğine bağlıdır.

## <a name="choosing-between-different-reserved-unit-types"></a>Farklı ayrılmış birim türlerinden seçme
Aşağıdaki tabloda farklı kodlama hızlarını arasında seçim yaparken kararı vermenize yardımcı olur. Ayrıca, birkaç Kıyaslama durum sağlar ve kendi testlerinizi gerçekleştirin videoları indirmek için kullanabileceğiniz tüm SAS URL'lerini sağlar:

| Senaryolar | **S1** | **S2** | **S3** |
| --- | --- | --- | --- |
| Hedeflenen kullanım örneğini |Tek bit hızlı kodlama. <br/>SD veya çözümleri altındaki dosyaları, duyarlı, düşük maliyetli değildir zaman. |Tekli bit hızı ve Çoklu bit hızlı kodlama.<br/>SD hem HD kodlaması için normal kullanım. |Tekli bit hızı ve Çoklu bit hızlı kodlama.<br/>Tam HD ve 4K çözünürlüklü videolar. Kodlama duyarlı, daha hızlı bir döngü süresi. |
| Kıyaslama |Tek bit hızlı MP4 dosyası, aynı çözünürlükte, kodlamayı yaklaşık 11 dakika sürer. |"H264 tekli bit hızı ile 720 p" kodlama yaklaşık 5 dakika sürer hazır.<br/><br/>Kodlama ile "H264 Çoklu bit hızı 720p" önayarını yaklaşık 11.5 birkaç dakika sürer. |"H264 tekli bit hızı ile 1080 p" kodlama yaklaşık 2.7 dakika sürer hazır.<br/><br/>Kodlama ile "H264 Çoklu bit hızı 1080p" önayarını yaklaşık 5.7 birkaç dakika sürer. |


## <a name="considerations"></a>Dikkat edilmesi gerekenler
> [!IMPORTANT]
> Bu bölümde açıklanan konuları gözden geçirin.  
> 
> 

* Media Services v3 veya Video Indexer tarafından tetiklenen ses analizi ve Video analizi işleri, S3 birimi türü önemle tavsiye edilir.
* Paylaşılan havuzu kullanıyorsanız, diğer bir deyişle, tüm ayrılmış birim daha sonra kodla görevlerinizi aynı performans adet S1 RU olarak sahip. Ancak, kuyruğa alınmış durumda görevlerinizi ayırmasına zamana üst sınır yoktur ve herhangi bir zamanda yalnızca en fazla bir görev çalışacağı.

## <a name="billing"></a>Faturalandırma

Medya Ayrılmış Birimlerinin fiili olarak kullanıldığı dakika sayısı üzerinden ücretlendirilirsiniz. Ayrıntılı bir açıklaması için SSS bölümüne bakın. [Media Services fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/) sayfası.   

## <a name="quotas-and-limitations"></a>Kotalar ve sınırlamalar
Kotalar ve sınırlamalar ve Destek bileti açmak nasıl hakkında daha fazla bilgi için bkz. [kotaları ve sınırlamaları](media-services-quotas-and-limitations.md).

## <a name="next-step"></a>Sonraki adım
Bu teknolojilerden biri ile ölçekleme medya işleme görevi ulaşın: 

> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-encoding-units.md)
> * [Portal](media-services-portal-scale-media-processing.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 

> [!NOTE]
> Java SDK'ın en son sürümünü almak ve Java ile geliştirmeye başlamak için bkz. [Media Services için Java istemcisi SDK’sı ile çalışmaya başlama](https://docs.microsoft.com/azure/media-services/media-services-java-how-to-use). <br/>
> Media Services için en yeni PHP SDK'sını indirmek üzere, [Packagist deposunda](https://packagist.org/packages/microsoft/windowsazure#v0.5.7) Microsoft/WindowAzure paketinin 0.5.7 sürümünü arayın.  

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

