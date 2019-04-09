---
title: Azure Media Services v3 sürüm notları | Microsoft Docs
description: İle en son gelişmeleri güncel kalmak için bu makalede, Azure Media Services v3 en yeni güncelleştirmeleri sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: na
ms.topic: article
ms.date: 04/04/2019
ms.author: juliako
ms.openlocfilehash: de5432c4e04fb0cfaf0517426fe9ee9da2a57b37
ms.sourcegitcommit: b4ad15a9ffcfd07351836ffedf9692a3b5d0ac86
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59058093"
---
# <a name="azure-media-services-v3-release-notes"></a>Azure Media Services v3 sürüm notları

İle en son gelişmeleri güncel kalmak için bu makalede, ile hakkında bilgi sağlar:

* En son sürümleri
* Bilinen sorunlar
* Hata düzeltmeleri
* Kullanım dışı işlev

## <a name="known-issues"></a>Bilinen sorunlar

> [!NOTE]
> Şu anda, v3 kaynaklarını yönetmek için Azure portalını kullanamıyorsunuz. Kullanım [REST API](https://aka.ms/ams-v3-rest-sdk), CLI, desteklenen Sdk'lardan birini veya.

Daha fazla bilgi için [Media Services v2'den v3 taşımak için geçiş kılavuzunu](migrate-from-v2-to-v3.md#known-issues).

## <a name="march-2019"></a>Mart 2019

Dinamik paketleme artık Dolby Atmos. destekler Daha fazla bilgi için [dinamik paketleme tarafından desteklenen ses codec bileşenleri](dynamic-packaging-overview.md#audio-codecs-supported-by-dynamic-packaging).

Artık, akış Bulucu için uygulamak varlık veya hesap filtrelerin listesini belirtebilirsiniz. Daha fazla bilgi için [filtreleri ile akış Bulucu ilişkilendirmek](filters-concept.md#associate-filters-with-streaming-locator).

## <a name="february-2019"></a>Şubat 2019

Media Services v3 Ulusal Azure bulutlarında artık desteklenmektedir. Tüm özellikler tüm bulutlara henüz kullanılabilir. Ayrıntılar için bkz [Bulutlar ve hangi Azure Media Services v3 mevcut bölgeleri](azure-clouds-regions.md).

[Microsoft.Media.JobOutputProgress](media-services-event-schemas.md#monitoring-job-output-progress) olay, medya Hizmetleri için Azure Event Grid şemaları için eklendi.

## <a name="january-2019"></a>Ocak 2019

### <a name="media-encoder-standard-and-mpi-files"></a>Media Encoder Standard ve MPI dosyaları 

Medya Kodlayıcısı MP4 dosyaları üretmek için standart ile kodlarken .mpi yeni bir dosya oluşturulur ve çıktıyı eklenen varlık. Bu MPI dosya performansını artırmak için kullanılmaya [dinamik paketleme](dynamic-packaging-overview.md) ve senaryoları akış.

Değiştirme veya MPI dosyayı kaldırmak veya gerekir (veya etkinleştirmezsiniz) varlığını hizmetinizdeki herhangi bir bağımlılık olması, böyle bir dosya.

## <a name="december-2018"></a>Aralık 2018

GA sürümündeki V3 API'yi güncelleştirmeleri içerir:
       
* **PresentationTimeRange** özellikleri gereklidir artık ' için ' **varlık filtreleri** ve **hesap filtreleri**. 
* $Top ve $skip sorgu seçeneklerini **işleri** ve **dönüştüren** kaldırıldı ve $orderby eklendi. Yeni sipariş işlevselliği ekleme bir parçası olarak uygulanan değil olsa da $top ve $skip seçeneklerini yanlışlıkla daha önce açığa bulundu.
* Numaralandırma genişletilebilirliği tekrar etkinleştirilecektir. Bu özellik SDK preview sürümleri etkinleştirildikten ve yanlışlıkla GA sürümünde devre dışı.
* Önceden tanımlanmış iki akış ilke yeniden adlandırıldı. **SecureStreaming** artık **MultiDrmCencStreaming**. **SecureStreamingWithFairPlay** artık **Predefined_MultiDrmStreaming**.

## <a name="november-2018"></a>Kasım 2018

CLI 2.0 modülü için kullanıma sunuldu [Azure Media Services v3 GA](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest) – v 2.0.50.

### <a name="new-commands"></a>Yeni komutlar

- [ams hesabı az](https://docs.microsoft.com/cli/azure/ams/account?view=azure-cli-latest)
- [az ams hesabına filtreleme](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest)
- [az ams varlığı](https://docs.microsoft.com/cli/azure/ams/asset?view=azure-cli-latest)
- [az ams varlığı filtreleme](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest)
- [az ams içeriğini-key-policy](https://docs.microsoft.com/cli/azure/ams/content-key-policy?view=azure-cli-latest)
- [az ams işi](https://docs.microsoft.com/cli/azure/ams/job?view=azure-cli-latest)
- [az ams canlı olay](https://docs.microsoft.com/cli/azure/ams/live-event?view=azure-cli-latest)
- [az ams Canlı çıkış](https://docs.microsoft.com/cli/azure/ams/live-output?view=azure-cli-latest)
- [az ams akış uç noktası](https://docs.microsoft.com/cli/azure/ams/streaming-endpoint?view=azure-cli-latest)
- [Akış az ams-Bulucu](https://docs.microsoft.com/cli/azure/ams/streaming-locator?view=azure-cli-latest)
- [az ams hesabınızı mru](https://docs.microsoft.com/cli/azure/ams/account/mru?view=azure-cli-latest) -medya ayrılmış birimi yönetmenizi sağlar. Daha fazla bilgi için [ölçek medya ayrılmış birimi](media-reserved-units-cli-how-to.md).

### <a name="new-features-and-breaking-changes"></a>Yeni özellikler ve bozucu değişiklikler

#### <a name="asset-commands"></a>Varlık komutları

- ```--storage-account``` ve ```--container``` bağımsız değişkenleri eklendi.
- Varsayılan değerleri (şimdi + 23 h) süre sonu ve izinleri (okuma) için ```az ams asset get-sas-url``` eklenen komutu.

#### <a name="job-commands"></a>İş komutları

- ```--correlation-data``` ve ```--label``` bağımsız değişkenleri eklendi
- ```--output-asset-names``` olarak yeniden adlandırıldı ```--output-assets```. Varlıklar, boşlukla ayrılmış listesini kabul artık ' assetName label =' biçimi. Bir varlık etiketi olmadan şöyle gönderilebilir: ' assetName ='.

#### <a name="streaming-locator-commands"></a>Akış Bulucusu komutları

- ```az ams streaming locator``` temel komutu yerine ```az ams streaming-locator```.
- ```--streaming-locator-id``` ve ```--alternative-media-id support``` bağımsız değişkenleri eklendi.
- ```--content-keys argument``` bağımsız değişken güncelleştirildi.
- ```--content-policy-name``` olarak yeniden adlandırıldı ```--content-key-policy-name```.

#### <a name="streaming-policy-commands"></a>Akış ilke komutları

- ```az ams streaming policy``` temel komutu yerine ```az ams streaming-policy```.
- Şifreleme parametreleri, destek ```az ams streaming-policy create``` eklendi.

#### <a name="transform-commands"></a>Komutları dönüştürme

- ```--preset-names``` bağımsız değişken yerine ```--preset```. Artık yalnızca 1 çıkış/ön ayarı teker teker ayarlayabilirsiniz (daha fazla çalıştırmak için sahip olduğunuz eklemek için ```az ams transform output add```). Ayrıca, özel JSON yolunu geçirerek özel StandardEncoderPreset ayarlayabilirsiniz.
- ```az ams transform output remove``` kaldırmak için çıkış dizinini geçirerek gerçekleştirilebilir.
- ```--relative-priority, --on-error, --audio-language and --insights-to-extract``` eklenen bağımsız değişkenleri ```az ams transform create``` ve ```az ams transform output add``` komutları.

## <a name="october-2018---ga"></a>Ekim 2018 - GA

Bu bölümde, Azure Media Services (AMS) Ekim güncelleştirmeleri açıklanmaktadır.

### <a name="rest-v3-ga-release"></a>REST v3 GA sürümü

[REST v3 GA sürümü](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01) Canlı için daha fazla API'ları içeren hesabı/varlık düzeyi bildirim filtresi ve DRM desteği.

#### <a name="azure-resource-management"></a>Azure Kaynak Yönetimi 

Azure kaynak yönetimi için destek, birleşik yönetim ve işlemler (artık her şeyi tek bir yerde) API sağlar.

Bu sürüm ile başlayarak, Canlı olaylar oluşturmak için Resource Manager şablonlarını kullanabilirsiniz.

#### <a name="improvement-of-asset-operations"></a>Varlık işlemleri iyileştirme 

Aşağıdaki geliştirmeleri kullanıma sunulan:

- Http (s) URL'leri ya da Azure Blob Depolama SAS URL'lerini alın.
- Varlıklar için kendi kapsayıcı adları belirt 
- Azure işlevleri ile özel iş akışları oluşturmak için daha kolay çıkış desteği.

#### <a name="new-transform-object"></a>Yeni dönüştürme nesnesi

Yeni **dönüştürme** kodlama model nesnesi basitleştirir. Yeni nesne oluşturma ve kodlama Resource Manager şablonları ve hazır paylaşma daha kolay hale getirir. 

#### <a name="azure-active-directory-authentication-and-rbac"></a>Azure Active Directory kimlik doğrulaması ve RBAC

Azure AD kimlik doğrulaması ve rol tabanlı Access Control (RBAC) güvenli dönüşümleri, LiveEvents, içerik anahtarı ilkeleri veya rol veya kullanıcılar tarafından varlıklar Azure AD'de etkinleştirin.

#### <a name="client-sdks"></a>İstemci SDK'ları  

Media Services v3 sürümünde desteklenen diller: .NET Core, Java, Node.js, Ruby, Typescript, Python, Go.

#### <a name="live-encoding-updates"></a>Canlı kodlama güncelleştirmeler

Aşağıdaki Canlı kodlama güncelleştirmeler yapılmıştır:

- Yeni düşük gecikme süresi modu için Canlı (10 saniye için uçtan uca).
- Geliştirilmiş RTMP desteği (daha fazla kararlılık ve daha fazla kaynak Kodlayıcı desteği).
- Güvenli RTMPS alın.

    Canlı bir olay oluşturduğunuzda, artık 4 alma URL'leri alma. 4 alma URL'leri neredeyse aynıdır, yalnızca bir bağlantı noktası numarası bölümü farklı aynı akış belirteci (AppID) sahip. URL'lerin ikisinin, birincil ve yedek RTMPS için. 
- 24 saatlik biçim dönüştürme desteği. 
- RTMP SCTE35 aracılığıyla ad sinyal desteği geliştirildi.

#### <a name="improved-event-grid-support"></a>Event Grid için geliştirilmiş destek

İyileştirmeleri desteği aşağıdaki Event Grid görebilirsiniz:

- Logic Apps ve Azure işlevleri ile daha kolay geliştirme için Azure Event Grid tümleştirmesi. 
- Olayları kodlama, Canlı Kanallar ve daha fazlası için abone olun.

### <a name="cmaf-support"></a>CMAF desteği

(İOS 11 +) Apple HLS ve MPEG-DASH için CMAF ve 'cbcs' şifreleme desteği CMAF destekleyen oynatıcılar.

### <a name="video-indexer"></a>Video Indexer

Video Indexer GA sürümü Ağustos'ta Duyuruldu. Yeni şu anda desteklenen özellikler hakkında bilgi için [Video Indexer nedir](../../cognitive-services/video-indexer/video-indexer-overview.md?toc=/azure/media-services/video-indexer/toc.json&bc=/azure/media-services/video-indexer/breadcrumb/toc.json). 

### <a name="plans-for-changes"></a>Değişiklikleri planları

#### <a name="azure-cli-20"></a>Azure CLI 2.0
 
İşlemler (Live, içerik anahtar ilkeleri, hesabı/varlık filtreleri, akış ilkeleri dahil) tüm özellikler dahildir Azure CLI 2.0 modülü yakında kullanıma sunulacaktır. 

### <a name="known-issues"></a>Bilinen sorunlar

API Önizleme varlık veya AccountFilters için kullanılan müşteriler aşağıdaki sorunundan etkilendiğinizi.

Varlıklar veya hesap filtreleri 09/28 ile 10 arasında oluşturduğunuz, / 12 ile Media Services v3 CLI veya API'leri, gereken tüm varlık ve AccountFilters kaldırın ve bunları sürüm çakışması nedeniyle yeniden oluşturun. 

## <a name="may-2018---preview"></a>Mayıs 2018 - Önizleme

### <a name="net-sdk"></a>.NET SDK

.NET SDK'yı aşağıdaki özellikler mevcuttur:

* **Dönüşümler** ve **işleri** kodlayın veya medya içeriği çözümlemek için. Örnekler için bkz [Stream dosyaları](stream-files-tutorial-with-api.md) ve [Çözümle](analyze-videos-tutorial-with-api.md).
* **Akış bulucuları** yayımlama ve son kullanıcı cihazlarına içerik akışı yapmak için
* **İlkeleri akış** ve **içeriği anahtar ilkeleri** içerik sunarken anahtar teslim ve content protection (DRM) yapılandırmak için.
* **Canlı olayları** ve **Canlı çıkışları** alma ve canlı akış içeriğini arşivleme yapılandırmak için.
* **Varlıklar** depolamak ve Azure Depolama'da medya içeriği yayımlamak için. 
* **Akış uç noktaları** yapılandırmak ve dinamik paketleme, şifreleme ve canlı ve isteğe bağlı medya içeriği akışı ölçeklendirin.

### <a name="known-issues"></a>Bilinen sorunlar

* Bir iş gönderirken HTTPS URL'leri, SAS URL'lerini veya yolları kullanarak dosyaları Azure Blob Depolama alanında bulunan kaynak videonuzu içe almak için belirtebilirsiniz. AMS v3 şu anda HTTPS URL'leri üzerinden yığın halinde aktarım kodlamasını desteklememektedir.

## <a name="next-steps"></a>Sonraki adımlar

[Genel Bakış](media-services-overview.md)
