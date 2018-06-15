---
title: Ortam işleme genel bakış ölçeklendirme | Microsoft Docs
description: Bu konu Azure Media Services ile ölçeklendirme medyayı işleme bir genel bakıştır.
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
ms.date: 07/04/2017
ms.author: juliako
ms.openlocfilehash: c80bddfe1896b0b99319ef007c25718b5a754005
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
ms.locfileid: "29803578"
---
# <a name="scaling-media-processing-overview"></a>Ölçeklendirme medyayı işleme genel bakış
Bu sayfa hakkında genel bakış ve medya işleme ölçeklendirmek neden sağlar. 

## <a name="overview"></a>Genel Bakış
Media Services hesabı bir Ayrılmış Birim Türüyle ilişkilendirilir ve bu da medya işleme görevlerinizin ne hızda işleneceğini belirler. Şu ayrılmış birim türlerinden birini seçebilirsiniz: **S1**, **S2** veya **S3**. Örneğin, aynı kodlama işi **S2** ayrılmış birim türünü kullandığınızda **S1** türüne göre daha hızlı çalışır. Daha fazla bilgi için bkz: [ayrılmış birim türleri](https://azure.microsoft.com/blog/high-speed-encoding-with-azure-media-services/).

Ayrılmış birim türü belirtmeye ek olarak, hesabınız ile ayrılan birimler sağlamak için belirtebilirsiniz. Sağlanan ayrılmış birim sayısı, verili bir hesapta eşzamanlı olarak işlenebilecek medya görevlerinin sayısını belirler. Örneğin, beş medya görevler eşzamanlı olarak kadar uzun çalışıyor olacak beş ayrılmış birimler, hesabınız varsa, olarak işlenecek görevler vardır. Kalan görevler sıraya bekler ve sıralı olarak çalışan bir görev tamamlandığında işlemek için toplanmış. Bir hesap sağlanan tüm ayrılan birimler yoksa, ardından görevleri sırayla çekilir. Bu durumda, sonraki bir başlangıç ve bitiş görev arasındaki bekleme süresine sistemde kaynakları kullanılabilirliğine bağlı olacaktır.

## <a name="choosing-between-different-reserved-unit-types"></a>Farklı ayrılmış birim türleri arasında seçme
Aşağıdaki tabloda farklı kodlama hızları arasında seçerken karar vermenize yardımcı olur. Ayrıca, bazı Kıyaslama durumlar sağlar ve kendi testlerinizi gerçekleştirmek videolar indirmek için kullanabileceğiniz SAS URL'leri sağlar:

| Senaryolar | **S1** | **S2** | **S3** |
| --- | --- | --- | --- |
| Hedeflenen kullanım örneği |Tek bit hızlı kodlama. <br/>Dosyaları SD veya çözümler altında hassas, düşük maliyetli değildir zaman. |Tek bit hızlı ve Çoklu bit hızı kodlaması.<br/>SD ve HD kodlama için normal kullanım. |Tek bit hızlı ve Çoklu bit hızı kodlaması.<br/>Tam HD ve 4K çözümleme videolar. Hassas ve hızlı bir döngü kodlama zaman. |
| Kıyaslama |[Giriş dosyası: 5 dakika uzun 29.97 adresindeki 640x360p Çerçeve/saniye](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_360p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D).<br/><br/>Tek bit hızlı MP4 dosyası, aynı çözünürlükte kodlamasını yaklaşık 11 dakika sürer. |[Giriş dosyası: 5 dakika uzun 29.97 adresindeki 1280x720p Çerçeve/saniye](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_720p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D)<br/><br/>"H264 Çoklu bit hızı ile 720 p" kodlama yaklaşık 5 dakika sürer hazır.<br/><br/>İle kodlama "H264 Çoklu bit hızı 720p" hazır yaklaşık 11.5 dakika sürer. |[Giriş dosyası: 5 dakika uzun 29.97 adresindeki 1920x1080p Çerçeve/saniye](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_1080p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D). <br/><br/>"H264 Çoklu bit hızı ile 1080 p" kodlama yaklaşık 2.7 dakika sürer hazır.<br/><br/>İle kodlama "H264 Çoklu bit hızı 1080p" hazır yaklaşık 5.7 dakika sürer. |

## <a name="considerations"></a>Dikkat edilmesi gerekenler
> [!IMPORTANT]
> Bu bölümde açıklanan konuları gözden geçirin.  
> 
> 

* Azure Media Indexer kullanarak işleri dizin oluşturma dahil olmak üzere tüm medyayı işleme parallelizing için ayrılan birimler çalışır.  Bununla birlikte kodlamadan farklı olarak, dizin oluşturma işleri daha hızlı ayrılmış birimlerde daha hızlı işlenmez.
* Paylaşılan havuzu kullanıyorsanız, diğer bir deyişle, tüm ayrılan birimler sonra kodla görevlerinizi aynı performans S1 RUs olarak sahip. Ancak, görevlerinizi sıraya alınmış durumda harcayabilir saat için üst sınır yoktur ve belirli bir zamanda, tek en fazla görev çalıştırırsınız.

## <a name="billing"></a>Faturalandırma

Medya Ayrılmış Birimlerinin fiili olarak kullanıldığı dakika sayısı üzerinden ücretlendirilirsiniz. Ayrıntılı bir açıklama için SSS bölümüne bakın [Media Services fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/) sayfası.   

## <a name="quotas-and-limitations"></a>Kotalar ve sınırlamalar
Kotalar ve sınırlamalar ve Destek bileti açmak nasıl hakkında daha fazla bilgi için bkz: [kotaları ve kısıtlamaları](media-services-quotas-and-limitations.md).

## <a name="next-step"></a>Sonraki adım
Bu teknolojiler biriyle ölçeklendirme medya işleme görevi elde: 

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
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

