---
title: Azure geçişi Media Services v3 v2 | Microsoft Docs
description: Bu makalede, Azure Media Services v3 sürümünde yapılan değişiklikleri açıklar ve iki sürümü arasındaki farklar gösterilmektedir. Makalede, Media Services v2'den v3 taşımak için geçiş kılavuzunu de sağlanır.
services: media-services
documentationcenter: na
author: Juliako
manager: femila
editor: ''
tags: ''
keywords: azure media services, akış, yayın, canlı, çevrimdışı
ms.service: media-services
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 05/01/2019
ms.author: juliako
ms.openlocfilehash: da8d1000ebe2695dbc95d475027722962a3305d9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65555888"
---
# <a name="migration-guidance-for-moving-from-media-services-v2-to-v3"></a>Media Services v2'den v3 taşımak için Geçiş Kılavuzu

Bu makalede, Azure Media Services v3 sürümünde yapılan değişiklikleri açıklar, iki sürümü arasındaki farklar gösterilmektedir ve Geçiş Kılavuzu sağlar.

Bugün üzerine geliştirilen bir video hizmeti varsa [eski Media Services v2 API'leri](../previous/media-services-overview.md), aşağıdaki yönergeleri ve v3 API'ler için geçirmeden önce konuları gözden geçirmeniz gerekir. Çok sayıda avantaj ve geliştirici deneyimi ve Media Services'ın özellikleri geliştiren yeni özellikler v3 API vardır. Ancak, olarak adlandırılan aşımı [bilinen sorunlar](#known-issues) bölümünde bu makalede, korunmasından da API sürümleri arasındaki değişiklikleri nedeniyle. Bu sayfa, medya Hizmetleri ekibi v3 API'ler için devam eden bir iyileşme ve sürümler arasındaki boşlukları adresleri korunacaktır. 

> [!NOTE]
> Şu anda, v3 kaynaklarını yönetmek için Azure portalını kullanamıyorsunuz. [REST API](https://aka.ms/ams-v3-rest-ref), [CLI](https://aka.ms/ams-v3-cli-ref) veya desteklenen [SDK'lardan](media-services-apis-overview.md#sdks) birini kullanın.

## <a name="benefits-of-media-services-v3"></a>Avantajları medya Hizmetleri v3
  
### <a name="api-is-more-approachable"></a>API daha ulaşılabilir

*  v3, Azure Resource Manager'da yerleşik olan yönetim ve işlem işlevselliğini kullanıma sunan, birleşik bir API yüzeyini temel alır. Azure Resource Manager şablonları oluşturmak ve dönüştürmeler, akış uç noktalarını, Canlı olayları ve daha fazla dağıtmak için kullanılabilir.
* [Openapı belirtimini (Swagger eski adıyla)](https://aka.ms/ams-v3-rest-sdk) belge.
    Dosya tabanlı kodlama dahil olmak üzere tüm hizmet bileşenleri için şema kullanıma sunar.
* SDK'ları için kullanılabilir [.NET](https://aka.ms/ams-v3-dotnet-ref), .NET Core [Node.js](https://aka.ms/ams-v3-nodejs-ref), [Python](https://aka.ms/ams-v3-python-ref), [Java](https://aka.ms/ams-v3-java-ref), [Git](https://aka.ms/ams-v3-go-ref)ve Ruby.
* [Azure CLI](https://aka.ms/ams-v3-cli-ref) tümleştirme basit betik oluşturma desteği.

### <a name="new-features"></a>Yeni Özellikler

* Dosya tabanlı iş işlenmesi için girdi olarak bir HTTP (S) URL kullanabilirsiniz.<br/>Zaten Azure'da depolanan içeriğe sahip gerekmez veya varlıkları oluşturmak ihtiyacınız.
* Kavramını sunar [dönüştüren](transforms-jobs-concept.md) dosya tabanlı iş işleme için. Dönüşüm, Azure Resource Manager şablonları oluşturmak ve işleme ayarlarını birden çok müşteriler veya kiracılar arasında yalıtmak için yeniden kullanılabilir yapılandırmaları oluşturmak için kullanılabilir.
* Bir varlık birden çok olabilir [akış bulucuları](streaming-locators-concept.md) her biri farklı [dinamik paketleme](dynamic-packaging-overview.md) ve dinamik şifreleme ayarları.
* [İçerik koruma](content-key-policy-concept.md) birden çok anahtar özelliklerini destekler.
* Canlı 24 tekli bit hızı katkı için medya hizmetlerine kullanarak Çoklu bit hızlarında bir çıkış akışına zaman akışı saate kadar uzun olan olay akışını yapabilirsiniz.
* Canlı olayları yeni düşük gecikme süresi canlı akış desteği. Daha fazla bilgi için [gecikme](live-event-latency.md).
* Canlı olay Önizleme destekler [dinamik paketleme](dynamic-packaging-overview.md) ve dinamik şifreleme. Bu önizleme yanı sıra DASH ve HLS paketleme içerik koruması sağlar.
* Canlı çıkış v2 API programı varlıkta daha basittir. 
* Geliştirilmiş RTMP desteği (daha fazla kararlılık ve daha fazla kaynak Kodlayıcı desteği).
* Güvenli RTMPS alın.<br/>Canlı bir olay oluşturduğunuzda, 4 alma URL'lerini alabilirsiniz. 4 alma URL'leri neredeyse aynıdır, yalnızca bir bağlantı noktası numarası bölümü farklı aynı akış belirteci (AppID) sahip. URL'lerin ikisinin, birincil ve yedek RTMPS için.   
* Varlıklarınızı bir rol tabanlı erişim denetimi (RBAC) sahip. 

## <a name="changes-from-v2"></a>V2 değişiklikleri

* Media Services v3 ile varlıkları oluşturulan için yalnızca destekler [Azure depolama sunucu tarafı depolama şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).
    * V3 API'ler olan v2 API'leri ile oluşturulan varlıklar ile kullanabileceğiniz [depolama şifrelemesi](../previous/media-services-rest-storage-encryption.md) (AES 256), Media Services tarafından sağlanan.
    * Eski AES 256 ile yeni varlıklar oluşturulamıyor [depolama şifrelemesi](../previous/media-services-rest-storage-encryption.md) v3 API'lerini kullanarak.
* V3 SDK'ları, depolama kullanmak istiyorsanız ve sürüm oluşturma sorunları önler SDK sürümü üzerinde daha fazla denetim verir depolama SDK artık birbirinden ayrılmıştır. 
* V3 API'leri, tüm kodlama hızları bit / saniye cinsindendir. Bu, Media Encoder Standard hazır ayarları v2 farklıdır. Örneğin, v2'de hızı (kbps) 128 belirtilebilir, ancak v3 sürümünde bu 128000 (bit/saniye) olacaktır. 
* Varlıkları AssetFiles AccessPolicies ve IngestManifests v3 sürümünde yok.
* IAsset.ParentAssets özelliği v3 sürümünde mevcut değil.
* ContentKeys artık bir varlık, akış Bulucu özelliği sunulmuştur.
* Event Grid destek NotificationEndpoints değiştirir.
* Aşağıdaki varlıkların yeniden adlandırıldı
    * İş çıktısı, görev değiştirir ve artık bir iş parçasıdır.
    * Akış Bulucusu Bulucu değiştirir.
    * Canlı olay kanal değiştirir.<br/>Canlı etkinlikler üzerinde Canlı kanal oranları üzerinden faturalandırılır. Daha fazla bilgi için [fatura](live-event-states-billing.md) ve [fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/).
    * Canlı çıktı Program değiştirir.
* Oluşturma çıkışları başlangıç canlı ve silindiğinde sona erer. V2 API'leri farklı şekilde çalışan programlar, bunlar oluşturulduktan sonra başlatılmış olması gerekiyordu.
*  Bir iş hakkında bilgi almak için işin oluşturulduğu dönüştürme adını bilmeniz gerekir. 

## <a name="feature-gaps-with-respect-to-v2-apis"></a>Özellik boşluklarına v2 API'leri göre

V3 API v2 API'si ile ilgili aşağıdaki özellik boşluklarına sahiptir. Boşlukları Kapatma aşamasındadır.

* [Premium Kodlayıcı](../previous/media-services-premium-workflow-encoder-formats.md) ve eski [medya analizi işlemcileri](../previous/media-services-analytics-overview.md) (Azure medya Hizmetleri Dizin Oluşturucu 2 Önizleme, Face Redactor, vb.) v3 erişilebilir değildir.<br/>Media Indexer 1 veya 2 Önizleme geçirmek isteyen müşteriler, hemen v3 API'SİNDE önceden AudioAnalyzer kullanabilirsiniz.  Bu yeni hazır eski Media Indexer 1'den daha fazla özellik ya da 2 içerir. 
* Çoğu [özelliklerinin Media Encoder Standard v2'de Gelişmiş](../previous/media-services-advanced-encoding-with-mes.md) API'ler şu anda v3 sürümünde kullanılabilir değildir gibi:
    * (İsteğe bağlı ve canlı senaryoları için) kırpma
    * Varlıkları birleştirme
    * Yer paylaşımları
    * Kırpma
    * Küçük resim zıplamasını sağlayın
    * Ses yok sahip olduğunda giriş sessiz bir ses kaydı ekleme
    * Video sahip olduğunda giriş video parça ekleme
* API çağrısı aracılığıyla maskeleme görüntüsü ekleme Orta stream ve reklam işareti ekleme ile etkinliklerin Canlı kodlama dönüştürme şu anda desteklemez. 

> [!NOTE]
> Lütfen bu makalede yer işareti ve güncelleştirmeleri denetleme tutun.
 
## <a name="code-differences"></a>Kod farkları

Aşağıdaki tabloda, v2 ve v3 sık karşılaşılan senaryolara yönelik kod farkları gösterilmektedir.

|Senaryo|V2 API'Sİ|V3 API|
|---|---|---|
|Bir varlık oluşturun ve bir dosyayı karşıya yükleyin |[v2 .NET örneği](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-aes/blob/master/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs#L113)|[V3 .NET örneği](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#L169)|
|Bir iş gönderdiniz|[v2 .NET örneği](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-aes/blob/master/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs#L146)|[V3 .NET örneği](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#L298)<br/><br/>İlk dönüşüm işi oluşturma ve ardından bir gönderme işlemi gösterilmektedir.|
|Bir varlık AES şifreleme ile yayımlama |1. ContentKeyAuthorizationPolicyOption oluşturma<br/>2. ContentKeyAuthorizationPolicy oluşturma<br/>3. Create AssetDeliveryPolicy<br/>4. Varlık oluşturma ve içerik işi veya Gönder yüklemek ve çıktı varlığına kullanın<br/>5. AssetDeliveryPolicy varlıkla ilişkilendirme<br/>6. Create ContentKey<br/>7. Varlık için ContentKey ekleme<br/>8. AccessPolicy oluşturma<br/>9. Bulucu<br/><br/>[v2 .NET örneği](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-aes/blob/master/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs#L64)|1. İçerik anahtarı ilkesi oluşturma<br/>2. Varlık oluşturma<br/>3. İçerik yüklemek veya varlık JobOutput kullanın<br/>4. Akış Bulucusu oluşturma<br/><br/>[V3 .NET örneği](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES/Program.cs#L105)|
|İş ayrıntılarını almak ve işleri Yönet |[V2 ile işleri Yönet](../previous/media-services-dotnet-manage-entities.md#get-a-job-reference) |[V3 ile işleri Yönet](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#L546)|

## <a name="known-issues"></a>Bilinen sorunlar

* Şu anda, v3 kaynaklarını yönetmek için Azure portalını kullanamıyorsunuz. Kullanım [REST API](https://aka.ms/ams-v3-rest-sdk), CLI, desteklenen Sdk'lardan birini veya.
* Eşzamanlılık ve işlerinizin performansını, özellikle olanları Video veya ses analizi içeren denetlemek için hesabınızdaki medya ayrılmış birimi (MRU) sağlamak gerekir. Daha fazla bilgi için bkz. [Medya İşlemeyi Ölçeklendirme](../previous/media-services-scale-media-processing-overview.md). MRU kullanarak yönetebileceğiniz [CLI 2.0 için Media Services v3](media-reserved-units-cli-how-to.md)kullanarak [Azure portalında](../previous/media-services-portal-scale-media-processing.md), veya bu adı kullanıyor [v2 API'leri](../previous/media-services-dotnet-encoding-units.md). Media Services v2 veya v3 kullanıp kullanmadığınızı MRU, sağlama için gereken API.
* Media Services, API v2 API'si tarafından yönetilemez v3 ile oluşturduğunuz varlıkları.  
* V3 API'ler aracılığıyla v2 API ile oluşturulan varlıkları yönetmek için önerilmez. Varlıkları iki sürümde uyumsuz hale farklar örnekleri aşağıda verilmiştir:   
    * Dönüşüm ile ilişkili olmayan olarak işleri ve görevleri v2'de oluşturulan v3 sürümünde gösterilmez. V3 dönüşümler ve işler için geçmeniz önerilir. İzleyici Hareket halindeki v2 için geçiş sırasında işleri ihtiyaç duyan, bir çok daha kısa zaman dilimi olur.
    * Kanallar ve (Canlı olayları ve canlı çıkışlara v3 sürümünde eşlenir) v2 ile oluşturulan programları v3 ile yönetilen devam edemiyor. V3 Canlı olayları ve canlı çıkışları için uygun kanal Durma geçmeniz önerilir.<br/>Şu anda, sürekli olarak çalışan kanalların geçiremezsiniz.  

> [!NOTE]
> Bu sayfa, medya Hizmetleri ekibi v3 API'ler için devam eden bir iyileşme ve sürümler arasındaki boşlukları adresleri korunacaktır.

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, görüşlerinizi, güncelleştirmeleri alın

Kullanıma [Azure Media Services topluluğu](media-services-community.md) soru sorun, görüşlerinizi ve medya hizmetleri hakkında güncelleştirmeler almak farklı yollarını görmek için makaleyi.

## <a name="next-steps"></a>Sonraki adımlar

Video dosyalarını kodlamaya ve akışa almaya başlamanın ne kadar kolay olduğunu görmek için [Dosyaları akışa alma](stream-files-dotnet-quickstart.md) bölümüne göz atın. 

