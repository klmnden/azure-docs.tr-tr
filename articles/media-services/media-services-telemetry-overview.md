---
title: Azure Media Services Telemetri | Microsoft Docs
description: "Bu makalede Azure Media Services telemetri genel bir bakış sağlar."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 95c20ec4-c782-4063-8042-b79f95741d28
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 1b26d7925fe5bd39905d9f51d22433b1eea43af6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="azure-media-services-telemetry"></a>Telemetri Azure medya Hizmetleri

Azure Media Services (AMS) hizmetlerinin telemetri/ölçüm verilerini erişmenizi sağlar. AMS geçerli sürümü Canlı telemetri verilerini toplamanıza olanak tanır **kanal**, **StreamingEndpoint**ve canlı **arşiv** varlıklar. 

Telemetri, belirttiğiniz bir Azure depolama hesabı depolama tablosuna yazılır (genellikle, AMS hesabınızla ilişkili depolama hesabı kullanırsınız). 

Telemetri sistem veri saklama yönetmez. Depolama tabloları silerek eski telemetri verilerini kaldırabilirsiniz.

Bu konuda nasıl yapılandırılacağı ve AMS telemetri tüketen anlatılmaktadır.

## <a name="configuring-telemetry"></a>Telemetri yapılandırma

Bir bileşen düzeyinde ayrıntı telemetri yapılandırabilirsiniz. İki ayrıntı düzeyi "Normal" ve "Ayrıntılı" vardır. Şu anda, her iki düzeyleri aynı bilgileri döndürün. "Normal. kullanmak için önerilir. 

Aşağıdaki konular telemetri etkinleştirme gösterir:

[.NET ile telemetri etkinleştirme](media-services-dotnet-telemetry.md) 

[REST telemetriyle etkinleştirme](media-services-rest-telemetry.md)

## <a name="consuming-telemetry-information"></a>Telemetri bilgileri kullanma

Telemetri, bir Azure depolama tabloya telemetri Media Services hesabı için yapılandırıldığında, belirttiğiniz depolama hesabındaki yazılır. Bu bölümde, ölçümler için depolama tabloları açıklanmaktadır.

Telemetri verileri aşağıdaki yollardan birini kullanabilir:

- Verileri doğrudan Azure Table (örn. depolama SDK'yı kullanarak) depolama alanından okuyun. Telemetri depolama tabloları açıklaması için bkz: **telemetri bilgileri tüketen** içinde [bu](https://msdn.microsoft.com/library/mt742089.aspx) konu.

Veya

- Media Services .NET SDK'ın depolama verileri okumak için açıklandığı gibi kullanmanızı [bu](media-services-dotnet-telemetry.md) konu. 


Aşağıda açıklanan telemetri şema Azure Table Storage sınırları içinde iyi bir performans sağlamak üzere tasarlanmıştır:

- Veri Hizmeti Kimliğinde ve her hizmetinden telemetri izin vermek için bağımsız olarak Sorgulanacak hesabı tarafından bölümlenmiş.
- Bölümler bölüm boyutu makul bir üst sınır vermek tarihini içerir.
- Belirli bir hizmeti için Sorgulanacak en son telemetri öğeleri izin vermek için ters zaman sırayla satır anahtarları şunlardır.

Bu çok verimli olmasını genel sorgular izin vermesi gerekir:

- Paralel, bağımsız veri ayrı Hizmetleri Yükleniyor.
- Bir tarih aralığındaki belirli bir hizmeti için tüm veriler alınıyor.
- Bir hizmet için en son veriler alınıyor.

### <a name="telemetry-table-storage-output-schema"></a>Telemetri tablo depolama çıkış şeması

Telemetri verileri bir tabloda "" 20160321"oluşturulmuş bir tabloyu tarihi olduğu TelemetryMetrics20160321" aggregate depolanır. Telemetri sistem yeni her gün 00:00 UTC temel için ayrı bir tablo oluşturur. Tablo yinelenen değerleri depolamak için kullanılan bit hızı içinde belirli bir pencere süresi, gönderilen bayt sayısı, vb. gibi alma. 

Özellik|Değer|Örnekler/notları
---|---|---
PartitionKey|{Hesap Kimliği} _ {varlık kimliği}|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66 < br /<br/>Hesap Kimliği iş akışları birden çok Media Services hesabı için aynı depolama hesabındaki burada yazıyorsanız basitleştirmek için bölüm anahtarı dahil edilir.
RowKey|{gece saniyeye} _ {rastgele bir değeri}|01688_00199<br/><br/>Satır anahtarı bölüm içinde üst n stili sorguları izin vermek için gece yarısına saniye sayısını başlar. Daha fazla bilgi için bkz: [bu](../cosmos-db/table-storage-design-guide.md#log-tail-pattern) makalesi. 
zaman damgası|Tarih/saat|Zaman damgası 2016 Azure tablosundan otomatik-09-09T22:43:42.241Z
Tür|Telemetri verileri sağlayan varlık türü|StreamingEndpoint/kanal/arşiv<br/><br/>Olay türü yalnızca bir dize değeridir.
Ad|Telemetri olayın adı|ChannelHeartbeat/StreamingEndpointRequestLog
ObservedTime|(UTC) telemetri olayın gerçekleştiği zaman|2016-09-09T22:42:36.924Z<br/><br/>Gözlemlenen süre (örneğin bir kanal) telemetri göndermesini varlık tarafından sağlanır. Bu değer için bileşenler arasındaki eşitleme sorunlarını yaklaşık bir saat olabilir
ServiceId|{Hizmet kimliği}|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
Varlık özgü özellikleri|Olay için tanımlanan|StreamName: stream1, bit hızı 10123...<br/><br/>Kalan özellikleri belirtilen olay türü için tanımlanır. Azure tablo anahtar değer çifti içeriktir.  (diğer bir deyişle, tablodaki farklı satırlarla farklı özellik kümelerine sahip).

### <a name="entity-specific-schema"></a>Varlık özgü şeması

Üç tür varlık özgü telemetric veri girişi aşağıdaki sıklığı gönderilen vardır:

- Akış uç noktaları: her 30 saniye
- Canlı kanallar: dakikada
- Canlı arşiv: dakikada

**Akış uç noktası**

Özellik|Değer|Örnekler
---|---|---
PartitionKey|PartitionKey|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66
RowKey|RowKey|01688_00199
zaman damgası|zaman damgası|Zaman damgası Azure tablo 2016'dan otomatik-09-09T22:43:42.241Z
Tür|Tür|StreamingEndpoint
Ad|Ad|StreamingEndpointRequestLog
ObservedTime|ObservedTime|2016-09-09T22:42:36.924Z
ServiceId|Hizmet kimliği|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
Ana bilgisayar adı|Uç noktanın ana bilgisayar adı|builddemoserver.origin.mediaservices.Windows.NET
statusCode|HTTP kayıtlar durumu|200
ResultCode|Sonuç kodu ayrıntısı|S_OK
RequestCount|Toplamadaki toplam istek|3
BytesSent|Gönderilen toplam bayt sayısı|2987358
ServerLatency|Ortalama sunucu gecikme süresi (depolama alanıyla birlikte)|129
E2ELatency|Ortalama uçtan uca gecikme süresi|250

**Canlı kanal**

Özellik|Değer|Örnekler/notları
---|---|---
PartitionKey|PartitionKey|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66
RowKey|RowKey|01688_00199
zaman damgası|zaman damgası|Zaman damgası 2016 Azure tablosundan otomatik-09-09T22:43:42.241Z
Tür|Tür|Kanal
Ad|Ad|ChannelHeartbeat
ObservedTime|ObservedTime|2016-09-09T22:42:36.924Z
ServiceId|Hizmet kimliği|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
TrackType|İzleme video/ses/metin türü|video/ses
TrackName|İzleme adı|Video/audio_1
Bit hızı|İzleme bit hızı|785000
CustomAttributes||   
IncomingBitrate|Gerçek gelen bit hızı|784548
OverlapCount|Alma çakışıyor|0
DiscontinuityCount|Süreksizlik izlemek için|0
LastTimestamp|En son alınan veri zaman damgası|1800488800
NonincreasingCount|Parçaların sayısı atılan nedeniyle olmayan artan zaman damgası|2
UnalignedKeyFrames|(Kalitesi düzeylerini) fragment(s) aldık olup olmadığını hizalı değil çerçeveler burada anahtar |True
UnalignedPresentationTime|Olup fragment(s) (kalitesi düzeylerini/parçaları arasında) burada sunu zamanını hizalanmadıysa aldık|True
UnexpectedBitrate|Ses/video için hesaplanan/gerçek hızı > en fazla 40.000 bps ve IncomingBitrate izlerseniz doğru == veya IncomingBitrate ve actualBitrate % 50 farklı 0 |True
Sorunsuz|Gerekirse true, <br/>overlapCount, <br/>DiscontinuityCount, <br/>NonIncreasingCount, <br/>UnalignedKeyFrames, <br/>UnalignedPresentationTime, <br/>UnexpectedBitrate<br/> Tüm 0|True<br/><br/>Aşağıdaki koşullardan herhangi biri tuttuğunuzda false döndüren bileşik bir işlev iyi durumda:<br/><br/>-OverlapCount > 0<br/>-DiscontinuityCount > 0<br/>-NonincreasingCount > 0<br/>-UnalignedKeyFrames gerekmektedir<br/>-UnalignedPresentationTime gerekmektedir<br/>-UnexpectedBitrate gerekmektedir

**Canlı arşiv**

Özellik|Değer|Örnekler/notları
---|---|---
PartitionKey|PartitionKey|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66
RowKey|RowKey|01688_00199
zaman damgası|zaman damgası|Zaman damgası 2016 Azure tablosundan otomatik-09-09T22:43:42.241Z
Tür|Tür|Arşiv
Ad|Ad|ArchiveHeartbeat
ObservedTime|ObservedTime|2016-09-09T22:42:36.924Z
ServiceId|Hizmet kimliği|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
ManifestName|Program URL'si|asset-eb149703-ed0a-483c-91c4-e4066e72cce3/a0a5cfbf-71ec-4BD2-8c01-a92a2b38c9ba.ism
TrackName|İzleme adı|audio_1
TrackType|İzleme türü|Ses/video
CustomAttribute|Aynı ada sahip farklı izlemek ve bit hızı (çoklu Kamera Açısı) arasında ayıran dize onaltılı|
Bit hızı|İzleme bit hızı|785000
Sorunsuz|Gerekirse true, FragmentDiscardedCount == 0 & & ArchiveAcquisitionError == False|TRUE (Bu iki değer Ölçümde mevcut olmayan, ancak kaynak olayda var)<br/><br/>Aşağıdaki koşullardan herhangi biri tuttuğunuzda false döndüren bileşik bir işlev iyi durumda:<br/><br/>-FragmentDiscardedCount > 0<br/>-ArchiveAcquisitionError gerekmektedir

## <a name="general-qa"></a>Genel soru- cevap

### <a name="how-to-consume-metrics-data"></a>Ölçümleri verileri kullanmak üzere nasıl?

Ölçüm verilerini Azure tabloları bir dizi müşterinin depolama hesabındaki olarak depolanır. Bu veriler, aşağıdaki araçları kullanarak tüketilebilir:

- AMS SDK'SI
- Microsoft Azure Storage Gezgini (virgülle ayrılmış değer biçimindedir ve içinde işlenen Excel Excel'e destekler)
- REST API

### <a name="how-to-find-average-bandwidth-consumption"></a>Ortalama bant genişliği tüketimi bulmak nasıl?

Ortalama bant genişliği tüketimini BytesSent ortalama zaman dilimi içinde ' dir.

### <a name="how-to-define-streaming-unit-count"></a>Akış birim sayısını tanımlamak nasıl?

Akış birim sayısını bir akış uç noktası tarafından en yüksek verimlilik bölünmüş hizmetin akış uç noktalarını yoğun akışından olarak tanımlanabilir. Bir akış uç noktası en yüksek kullanılabilir verimini 160 MB / sn'dir.
Örneğin, bir müşterinin hizmetinden en yüksek verimlilik 40 MB/sn (en büyük değerini BytesSent zaman dilimi üzerinden) olduğunu varsayın. Ardından, akış birimi sayısı (40 MBps) eşittir * (8 bit/bayt) /(160 Mbps) 2 akış birimleri =.

### <a name="how-to-find-average-requestssecond"></a>Ortalama İstek/saniye bulmak nasıl?

İstek/saniye ortalama sayısını bulmak için bir zaman aralığı (RequestCount) isteklerinin ortalama sayısı işlem.

### <a name="how-to-define-channel-health"></a>Kanal durumu tanımlamak nasıl?

Aşağıdaki koşullardan herhangi biri tuttuğunuzda yanlış olacak şekilde, kanal durumu bileşik Boolean işlevi olarak tanımlanabilir:

- OverlapCount > 0
- DiscontinuityCount > 0
- NonincreasingCount > 0
- UnalignedKeyFrames gerekmektedir 
- UnalignedPresentationTime gerekmektedir 
- UnexpectedBitrate gerekmektedir


### <a name="how-to-detect-discontinuities"></a>Discontinuities algılamak nasıl?

Discontinuities algılamak için tüm kanal veri girişi bulun burada DiscontinuityCount > 0. Karşılık gelen ObservedTime zaman damgası discontinuities gerçekleştiği kez gösterir.

### <a name="how-to-detect-timestamp-overlaps"></a>Zaman damgası algılamak nasıl çakışıyor?

Zaman damgası çakışmalar algılamak için tüm kanal veri girişi bulun burada OverlapCount > 0. Karşılık gelen ObservedTime zaman damgası, zaman damgası çakışıyor kez oluştu gösterir.

### <a name="how-to-find-streaming-request-failures-and-reasons"></a>Akış istek hataları ve nedeni bulmak nasıl?

Akış istek hataları ve nedeni bulmak için burada ResultCode S_OK için eşit değil tüm akış uç noktası veri girişi bulun. Karşılık gelen StatusCode alan isteği hatanın nedenini gösterir.

### <a name="how-to-consume-data-with-external-tools"></a>Verileri dış araçları ile kullanmak üzere nasıl?

Telemetric veriler işlenir ve aşağıdaki araçları ile görselleştirilen:

- PowerBI
- Application Insights
- Azure İzleyicisi'ni (önceki adıyla Shoebox)
- AMS Canlı Panosu
- Azure Portal (bekleyen sürüm)

### <a name="how-to-manage-data-retention"></a>Veri saklama yönetmek nasıl?

Telemetri sistem veri saklama Yönetimi veya otomatik silme eski kayıtları sağlamaz. Bu nedenle, yönetmek ve depolama alanı tablosundan eski kayıtları el ile silmeniz gerekir. Depolama SDK'sı nasıl yapılacağını için başvurabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma

[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
