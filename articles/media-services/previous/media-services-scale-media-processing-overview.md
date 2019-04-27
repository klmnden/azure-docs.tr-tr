---
title: Genel Bakış medya işlemeyi ölçeklendirme | Microsoft Docs
description: Bu konuda, ölçeklendirme medya işleme Azure Media Services ile bir genel bakıştır.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: 51d168474fd593dd537a25c0434e240a426c2cbf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60817342"
---
# <a name="scaling-media-processing-overview"></a>Genel Bakış medya işlemeyi ölçeklendirme 
Bu sayfa hakkında genel bakış ve neden medya işlemeyi ölçeklendirme sağlar. 

## <a name="overview"></a>Genel Bakış
Media Services hesabı bir Ayrılmış Birim Türüyle ilişkilendirilir ve bu da medya işleme görevlerinizin ne hızda işleneceğini belirler. Şu ayrılmış birim türlerinden seçebilirsiniz: **S1**, **S2**, veya **S3**. Örneğin, aynı kodlama işi **S2** ayrılmış birim türünü kullandığınızda **S1** türüne göre daha hızlı çalışır. Daha fazla bilgi için [ayrılmış birim türlerinden](https://azure.microsoft.com/blog/high-speed-encoding-with-azure-media-services/).

Ayrılmış birim türünü belirtmenin yanı sıra, ayrılmış birim ile hesabınızı sağlamak için belirtebilirsiniz. Sağlanan ayrılmış birim sayısı, verili bir hesapta eşzamanlı olarak işlenebilecek medya görevlerinin sayısını belirler. Örneğin, beş medya görevi aynı anda uzun çalışacağı beş ayrılmış birim, hesabınız varsa, olarak işlenmek üzere görevleri vardır. Kalan görevlerin kuyrukta bekler ve sıralı olarak çalışan bir görev tamamlandığında işlemek için toplanmış. Sağlanan herhangi bir ayrılmış birim hesabınız yoksa, ardından görevleri sıralı olarak seçilir. Bu durumda, bir görev tamamlama ve ileri bir başlangıç arasındaki bekleme süresini sistemde kaynaklarının kullanılabilirliğine bağlıdır.

## <a name="choosing-between-different-reserved-unit-types"></a>Farklı ayrılmış birim türlerinden seçme
Aşağıdaki tabloda farklı kodlama hızlarını arasında seçim yaparken bir karar vermenize yardımcı olur. Şirket birkaç Kıyaslama durum ayrıca sağlar [indirebileceğiniz bir video](https://nimbuspmteam.blob.core.windows.net/asset-46f1f723-5d76-477e-a153-3fd0f9f90f73/SeattlePikePlaceMarket_7min.ts?sv=2015-07-08&sr=c&si=013ab6a6-5ebf-431e-8243-9983a6b5b01c&sig=YCgEB8DxYKK%2B8W9LnBykzm1ZRUTwQAAH9QFUGw%2BIWuc%3D&se=2118-09-21T19%3A28%3A57Z) kendi testleri gerçekleştirmek için:

|RU türü|Senaryo|Örnek sonuçlarını [video 7 dk 1080 p](https://nimbuspmteam.blob.core.windows.net/asset-46f1f723-5d76-477e-a153-3fd0f9f90f73/SeattlePikePlaceMarket_7min.ts?sv=2015-07-08&sr=c&si=013ab6a6-5ebf-431e-8243-9983a6b5b01c&sig=YCgEB8DxYKK%2B8W9LnBykzm1ZRUTwQAAH9QFUGw%2BIWuc%3D&se=2118-09-21T19%3A28%3A57Z)|
|---|---|---|
| **S1**|Tek bit hızlı kodlama. <br/>SD veya çözümleri altındaki dosyaları, duyarlı, düşük maliyetli değildir zaman.|Çoklu bit hızı SD çözümleme MP4 dosyasını kullanarak "H264 Çoklu bit hızı SD 16 x 9" için kodlama, 10 dakika sürer.|
| **S2**|Tekli bit hızı ve Çoklu bit hızlı kodlama.<br/>SD hem HD kodlaması için normal kullanım.|"H264 tekli bit hızı ile 720 p" kodlama yaklaşık 8 dakika sürer hazır.<br/><br/>Kodlama ile "H264 Çoklu bit hızı 720p" önayarını yaklaşık 16,8 birkaç dakika sürer.|
| **S3**|Tekli bit hızı ve Çoklu bit hızlı kodlama.<br/>Tam HD ve 4K çözünürlüklü videolar. Kodlama duyarlı, daha hızlı bir döngü süresi.|"H264 tekli bit hızı ile 1080 p" kodlama yaklaşık 4 dakika sürer hazır.<br/><br/>Kodlama ile "H264 Çoklu bit hızı 1080p" önayarını yaklaşık 8 dakika sürer.|

## <a name="considerations"></a>Dikkat edilmesi gerekenler
> [!IMPORTANT]
> Bu bölümde açıklanan konuları gözden geçirin.  
> 
> 

* Media Services v3 veya Video Indexer tarafından tetiklenen ses analizi ve Video analizi işleri, S3 birimi türü önemle tavsiye edilir.
* Paylaşılan havuzu kullanıyorsanız, diğer bir deyişle, tüm ayrılmış birim daha sonra kodla görevlerinizi aynı performans adet S1 RU olarak sahip. Ancak, kuyruğa alınmış durumda görevlerinizi ayırmasına zamana üst sınır yoktur ve herhangi bir zamanda yalnızca en fazla bir görev çalışacağı.

## <a name="billing"></a>Faturalandırma

Medya ayrılmış birimi sağlanan dakika sayısına, hesabınızdaki üzerinden ücretlendirilirsiniz. Bu bağımsız olarak mı ortaya çıkar hesabınızda çalışan herhangi bir işi vardır. Ayrıntılı bir açıklaması için SSS bölümüne bakın. [Media Services fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/) sayfası.   

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

