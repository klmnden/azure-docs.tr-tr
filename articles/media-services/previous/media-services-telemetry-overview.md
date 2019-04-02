---
title: Azure Media Services'ı Telemetri | Microsoft Docs
description: Bu makalede, Azure Media Services telemetri genel bir bakış sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 95c20ec4-c782-4063-8042-b79f95741d28
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/01/2019
ms.author: juliako
ms.openlocfilehash: 8e8b493881662483e66dd835d1cc68a471b18454
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58803318"
---
# <a name="azure-media-services-telemetry"></a>Azure Media Services telemetri  


> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürüm olan [Media Services v3](https://docs.microsoft.com/azure/media-services/latest/)’ü inceleyin. Ayrıca bkz [geçiş kılavuzuna v2'den v3](../latest/migrate-from-v2-to-v3.md)

Azure Media Services (AMS), telemetri/ölçüm verilerini hizmetlerinin erişmenize olanak tanır. AMS'ın geçerli sürümü Canlı telemetri verilerini toplamanıza olanak tanır **kanal**, **StreamingEndpoint**ve canlı **arşiv** varlıklar. 

Telemetri, belirlediğiniz bir Azure depolama hesabındaki bir depolama tablosuna yazılır (genellikle, AMS hesabınızla ilişkili depolama hesabını kullanırsınız). 

Telemetri sisteminin veri saklama yönetmez. Depolama tabloları silerek eski telemetri verilerini kaldırabilirsiniz.

Bu konu başlığı altında yapılandırmak ve AMS telemetri kullanma anlatılmaktadır.

## <a name="configuring-telemetry"></a>Telemetri yapılandırma

Telemetri üzerinde bir bileşen düzeyinde ayrıntı düzeyi yapılandırabilirsiniz. İki ayrıntı düzeyi "Normal" ve "Ayrıntılı" vardır. Şu anda, hem düzeyleri aynı bilgileri döndürün. "Normal. kullanmak için önerilir 

Aşağıdaki konular, telemetri nasıl etkinleştirilir gösterir:

[.NET ile telemetri etkinleştirme](media-services-dotnet-telemetry.md) 

[REST ile telemetri etkinleştirme](media-services-rest-telemetry.md)

## <a name="consuming-telemetry-information"></a>Telemetri bilgilerini kullanma

Telemetri, Media Services hesabı için telemetri yapılandırıldığında belirttiğiniz depolama hesabı bir Azure depolama tablosuna yazılır. Bu bölümde, depolama tabloları ölçümler açıklanmıştır.

Telemetri verileri aşağıdaki yollardan birini kullanabilir:

- Verileri doğrudan Azure tablo (örneğin depolama SDK'sını kullanarak) depolama'yı okuyun. Telemetri depolama tabloları açıklaması için bkz: **telemetri bilgilerini kullanan** içinde [bu](https://msdn.microsoft.com/library/mt742089.aspx) konu.

Veya

- Media Services .NET SDK depolama veri okumak için açıklandığı gibi kullanmanızı [bu](media-services-dotnet-telemetry.md) konu. 


Aşağıda açıklanan telemetri şema, Azure tablo depolama sınırları dahilinde iyi bir performans kazancı sunmak üzere tasarlanmıştır:

- Veri bağımsız olarak Sorgulanacak kimliği ve hizmet kimliği her bir hizmetten alınan telemetri izin vermek için hesaba göre bölümlenir.
- Bölümler bölüm boyutu makul bir üst sınır vermek tarihini içerir.
- Belirli bir hizmet için Sorgulanacak en son telemetri öğeye izin vermek için geriye doğru zaman sırayla satır anahtarı var.

Bu, birçok ortak sorguların verimli olması için izin vermelidir:

- Paralel, bağımsız ayrı hizmetler için veri yükleniyor.
- Belirli bir hizmetin bir tarih aralığındaki tüm verileri alınıyor.
- Bir hizmet için en son verileri alınıyor.

### <a name="telemetry-table-storage-output-schema"></a>Telemetri tablo depolama çıkış şeması

Telemetri verilerini toplama bir tablodaki "nerede"20160321"oluşturulmuş bir tabloyu tarihidir TelemetryMetrics20160321" depolanır. Telemetri sisteminin, 00:00 UTC'de dayalı yeni her gün için ayrı bir tablo oluşturur. Tablo yinelenen değerleri depolamak için kullanılan zaman, gönderilen bayt sayısı, vb. belirli bir pencere içinde hızı gibi alma. 

Özellik|Değer|Örnekler/notları
---|---|---
PartitionKey|{hesabı kimliği} _ {varlık kimliği}|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66<br/<br/>Hesap Kimliği iş akışları birden çok Media Services hesapları için aynı depolama hesabını burada yazıyorsanız basitleştirmek için bölüm anahtarını dahil edilir.
RowKey|{seconds yarısından} _ {rastgele değer}|01688_00199<br/><br/>Satır anahtarı bölüm üst n stili sorgulara izin vermek için gece saniye sayısı ile başlar. Daha fazla bilgi için [bu makaleye](../../cosmos-db/table-storage-design-guide.md#log-tail-pattern) bakın. 
Zaman damgası|Tarih/Saat|Azure tablo 2016 itibaren zaman damgası auto-09-09T22:43:42.241Z
Type|Telemetri verilerini sağlayan varlık türü|StreamingEndpoint/kanal/arşivleme<br/><br/>Olay türü yalnızca bir dize değeridir.
Ad|Telemetri olayı adı|ChannelHeartbeat/StreamingEndpointRequestLog
ObservedTime|(UTC) telemetrisi olayın gerçekleştiği zaman|2016-09-09T22:42:36.924Z<br/><br/>Gözlemlenen saat (örneğin bir kanal) telemetri gönderdiği varlık tarafından sağlanır. Bu değer, bu nedenle bileşenler arasındaki eşitleme sorunlarını yaklaşık bir saat olabilir
ServiceId|{service} kimliği|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
Varlığa özgü özellikleri|Olay tarafından tanımlandığı şekilde|StreamName: stream1, bit hızı 10123...<br/><br/>Diğer özellikler, belirli bir olay türü için tanımlanır. Azure tablo içeriğini anahtar-değer çiftlerinin ' dir.  (diğer bir deyişle, tablo içindeki farklı satırlar farklı özellik kümelerine sahip).

### <a name="entity-specific-schema"></a>Varlığa özgü şeması

Üç tür varlık özel telemetri verileri girişleri aşağıdaki sıklığı gönderilen vardır:

- Akış uç noktaları: Her 30 saniyede
- Canlı kanallar: Her dakika
- Canlı arşiv: Her dakika

**Akış uç noktası**

Özellik|Değer|Örnekler
---|---|---
PartitionKey|PartitionKey|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66
RowKey|RowKey|01688_00199
Zaman damgası|Zaman damgası|Azure tablo 2016 itibaren zaman damgası auto-09-09T22:43:42.241Z
Type|Type|StreamingEndpoint
Ad|Ad|StreamingEndpointRequestLog
ObservedTime|ObservedTime|2016-09-09T22:42:36.924Z
ServiceId|Hizmet Kimliği|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
ana bilgisayar adı|Uç nokta konak adı|builddemoserver.origin.mediaservices.Windows.NET
statusCode|Kayıtları HTTP durumu|200
ResultCode|Sonuç kodu ayrıntısı|S_OK
RequestCount|Toplamadaki toplam istek|3
BytesSent|Gönderilen toplam bayt|2987358
ServerLatency|Ortalama sunucu gecikme süresi (depolama dahil)|129
E2ELatency|Ortalama uçtan uca gecikme süresi|250

**Canlı kanal**

Özellik|Değer|Örnekler/notları
---|---|---
PartitionKey|PartitionKey|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66
RowKey|RowKey|01688_00199
Zaman damgası|Zaman damgası|Azure tablo 2016 itibaren zaman damgası auto-09-09T22:43:42.241Z
Type|Type|Kanal
Ad|Ad|ChannelHeartbeat
ObservedTime|ObservedTime|2016-09-09T22:42:36.924Z
ServiceId|Hizmet Kimliği|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
TrackType|İzleme görüntü/ses/metin türü|Görüntü/ses
TrackName|İzleme adı|Video/audio_1
Bit hızı|İzleme hızı|785000
CustomAttributes||   
IncomingBitrate|Gelen gerçek hızı|784548
OverlapCount|Çakışma içinde alma|0
DiscontinuityCount|Süreksizlik izlemek için|0
LastTimestamp|Son alınan veri zaman damgası|1800488800
NonincreasingCount|Parçaların sayısı atılan nedeniyle olmayan artan zaman damgası|2
UnalignedKeyFrames|Çerçeve hizalı değil (kalite düzeylerine) fragment(s) aldık olup olmadığını burada anahtar |True
UnalignedPresentationTime|Aldık mı (kalite düzeylerine/parçaları arasında) burada sunu zamanını hizalanmamış fragment(s).|True
UnexpectedBitrate|Ses/video için hesaplanan/gerçek hızı > 40.000 bps ve IncomingBitrate izlerseniz true, == 0 veya IncomingBitrate ve actualBitrate % 50'ye farklılık gösterir. |True
Sorunsuz|Gerekirse true, <br/>overlapCount, <br/>DiscontinuityCount, <br/>NonIncreasingCount, <br/>UnalignedKeyFrames, <br/>UnalignedPresentationTime, <br/>UnexpectedBitrate<br/> Tüm 0|True<br/><br/>Aşağıdaki koşullardan herhangi biri tuttuğunuzda false döndüren bir bileşik işlev iyi durumda:<br/><br/>-OverlapCount > 0<br/>- DiscontinuityCount > 0<br/>-NonincreasingCount > 0<br/>-UnalignedKeyFrames == True<br/>-UnalignedPresentationTime == True<br/>-UnexpectedBitrate == True

**Canlı arşiv**

Özellik|Değer|Örnekler/notları
---|---|---
PartitionKey|PartitionKey|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66
RowKey|RowKey|01688_00199
Zaman damgası|Zaman damgası|Azure tablo 2016 itibaren zaman damgası auto-09-09T22:43:42.241Z
Type|Type|Arşiv
Ad|Ad|ArchiveHeartbeat
ObservedTime|ObservedTime|2016-09-09T22:42:36.924Z
ServiceId|Hizmet Kimliği|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
manifestName|Program URL'si|asset-eb149703-ed0a-483c-91c4-e4066e72cce3/a0a5cfbf-71ec-4bd2-8c01-a92a2b38c9ba.ism
TrackName|İzleme adı|audio_1
TrackType|İzleme türü|Ses/video
CustomAttribute|Aynı ada sahip farklı izlemek ve bit hızı (çoklu kamera açısını) arasında ayırır dize onaltılık|
Bit hızı|İzleme hızı|785000
Sorunsuz|Gerekirse true, FragmentDiscardedCount == 0 & & ArchiveAcquisitionError == False|TRUE (Bu iki değerler ölçümü mevcut değildir, ancak kaynak olayı yok)<br/><br/>Aşağıdaki koşullardan herhangi biri tuttuğunuzda false döndüren bir bileşik işlev iyi durumda:<br/><br/>- FragmentDiscardedCount > 0<br/>-ArchiveAcquisitionError == True

## <a name="general-qa"></a>Genel soru- cevap

### <a name="how-to-consume-metrics-data"></a>Ölçüm verilerini kullanmak nasıl?

Ölçüm verileri bir dizi Azure tabloları müşterinin depolama hesabında depolanır. Bu veriler, aşağıdaki araçları kullanarak kullanılabilir:

- AMS SDK'SI
- Microsoft Azure Depolama Gezgini (virgülle ayrılmış değer biçimindedir ve işlenen, Excel dışarı aktarma destekler)
- REST API

### <a name="how-to-find-average-bandwidth-consumption"></a>Ortalama bant genişliği tüketiminizi nasıl?

Ortalama bant genişliği tüketimini BytesSent bir zaman aralığı boyunca ortalamasıdır.

### <a name="how-to-define-streaming-unit-count"></a>Akış birimi sayısı tanımlamak nasıl?

Akış birimi sayısı en yüksek aktarım hızı bir akış uç noktası en yüksek aktarım hızı tarafından ayrılmış hizmetin akış uç noktalarından olarak tanımlanabilir. Bir akış uç noktası en yüksek kullanılabilir verimini 160 MB / sn'dir.
Örneğin, bir müşterinin hizmetten en yüksek aktarım hızı 40 MB/sn (en yüksek değer BytesSent bir zaman aralığı üzerinde) olduğunu varsayın. Ardından, akış birimi sayısı (40 MB/sn ile) eşittir * (8 bit/bayt) /(160 Mbps) = 2 akış birimi.

### <a name="how-to-find-average-requestssecond"></a>Ortalama İstek/saniye bulmak nasıl?

Ortalama İstek/saniye bulmak için bir zaman aralığı (RequestCount) istekleri ortalama sayısı işlem.

### <a name="how-to-define-channel-health"></a>Kanal durumu tanımlamak nasıl?

Aşağıdaki koşullardan herhangi biri tuttuğunuzda yanlış şekilde kanal sistem durumu bir bileşik Boole işlevi tanımlanabilir:

- OverlapCount > 0
- DiscontinuityCount > 0
- NonincreasingCount > 0
- UnalignedKeyFrames == True 
- UnalignedPresentationTime == True 
- UnexpectedBitrate == True


### <a name="how-to-detect-discontinuities"></a>Discontinuities nasıl?

Tüm kanal veri girişleri discontinuities algılamak için bulma burada DiscontinuityCount > 0. Karşılık gelen ObservedTime zaman damgası discontinuities oluştuğu süreleri gösterir.

### <a name="how-to-detect-timestamp-overlaps"></a>Zaman damgası nasıl çakışıyor?

Zaman damgası çakışıyor algılamak için tüm kanal veri girişi bulunamadı. burada OverlapCount > 0. Karşılık gelen ObservedTime zaman damgası, zaman damgası çakışıyor kez oluştu gösterir.

### <a name="how-to-find-streaming-request-failures-and-reasons"></a>Akış istek hataları ve nedenlerini bulmak nasıl?

Akış istek hataları ve nedenlerini bulmak için ResultCode S_OK için eşit olduğu tüm akış uç noktası veri girişi bulun. Karşılık gelen StatusCode alan isteği hatanın nedenini gösterir.

### <a name="how-to-consume-data-with-external-tools"></a>Dış araçları ile veri tüketmek nasıl?

Telemetri veriler işlenir ve aşağıdaki araçlarla görselleştirilmiş:

- PowerBI
- Application Insights
- Azure İzleyici (eski adıyla ayakkabı)
- AMS Canlı Panosu
- Azure portalı (bekleyen sürüm)

### <a name="how-to-manage-data-retention"></a>Veri saklama yönetmek nasıl?

Telemetri sisteminin veri saklama Yönetimi veya otomatik silme eski kayıtları sağlamaz. Bu nedenle, yönetmek ve eski kayıt depolama tablosundan el ile silmeniz gerekir. Depolama SDK'sı için bunu nasıl başvurabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma

[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
