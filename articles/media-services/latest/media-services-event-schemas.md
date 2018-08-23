---
title: Media Services olayları Azure Event Grid şemaları
description: Media Services olayları Azure Event Grid ile sağlanan özellikleri tanımlar
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: reference
ms.date: 08/17/2018
ms.author: juliako
ms.openlocfilehash: a6a6c459e170627d26aa1445f4f4dd193965fe70
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2018
ms.locfileid: "42056258"
---
# <a name="azure-event-grid-schemas-for-media-services-events"></a>Media Services olayları Azure Event Grid şemaları

Bu makalede, Media Services olaylarını şemaları ve özellikleri sağlar.

Örnek betikler ve öğreticiler listesi için bkz: [Media Services olay kaynağı](../../event-grid/event-sources.md#azure-subscriptions).

## <a name="available-event-types"></a>Kullanılabilir olay türleri

Media Services, aşağıdaki olay türlerini gösterir:

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.Media.JobStateChange| İş durumunu değiştirir. |
| Microsoft.Media.LiveEventConnectionRejected | Kodlayıcı'nın bağlantı girişimini reddetti. |
| Microsoft.Media.LiveEventEncoderConnected | Kodlayıcı Canlı etkinlik ile bağlantı kurar. |
| Microsoft.Media.LiveEventEncoderDisconnected | Kodlayıcı bağlantısını keser. |
| Microsoft.Media.LiveEventIncomingDataChunkDropped | Medya sunucusu çok geç veya çakışan bir zaman damgası olduğundan veri öbeği düşene (yeni veri öbeğin zaman damgası olan önceki verileri öbek, son saatten daha az). |
| Microsoft.Media.LiveEventIncomingStreamReceived | Medya sunucusu, her parça için ilk veri öbeği stream ya da bağlantı alır. |
| Microsoft.Media.LiveEventIncomingStreamsOutOfSync | Ses medya sunucusu algılar ve video akışları eşitlenmiş halde değil. Kullanıcı deneyimi olmayan etkilenebilir, çünkü bir uyarı olarak kullanın. |
| Microsoft.Media.LiveEventIncomingVideoStreamsOutOfSync | Medya sunucusu herhangi bir dış kodlayıcıdan gelen iki video akışları eşitlenmemiş algılar. Kullanıcı deneyimi olmayan etkilenebilir, çünkü bir uyarı olarak kullanın. |
| Microsoft.Media.LiveEventIngestHeartbeat | Canlı etkinlik çalıştırıldığında, her parça için her 20 saniyede yayımladı. Sağlar sistem durumu özetini alın. |
| Microsoft.Media.LiveEventTrackDiscontinuityDetected | Medya sunucusu süreksizlik gelen izde algılar. |

İçin iki kategorisi vardır **canlı** olayları: akış düzeyinde olaylar ve izleme düzeyi olaylar. 

Stream düzeyinde olaylar, akış veya bağlantı oluşturulur. Her olayda bir `StreamId` bağlantı veya akış tanımlayan bir parametre. Her akış veya bağlantı farklı türde bir veya daha fazla parça vardır. Örneğin, bir bağlantıdan bir kodlayıcı, bir ses kaydı ve dört video parçaları sahip olabilir. Akış olayı türü şunlardır:

* LiveEventConnectionRejected
* LiveEventEncoderConnected
* LiveEventEncoderDisconnected

İzleme düzeyi olaylar, parça oluşturulur. İzleme olay türleri şunlardır:

* LiveEventIncomingDataChunkDropped
* LiveEventIncomingStreamReceived
* LiveEventIncomingStreamsOutOfSync
* LiveEventIncomingVideoStreamsOutOfSync
* LiveEventIngestHeartbeat
* LiveEventTrackDiscontinuityDetected

## <a name="jobstatechange"></a>JobStateChange

Aşağıdaki örnek, şemasını gösterir **JobStateChange** olay: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "transforms/VideoAnalyzerTransform/jobs/<job-id>",
    "eventType": "Microsoft.Media.JobStateChange",
    "eventTime": "2018-04-20T21:26:13.8978772",
    "id": "b9d38923-9210-4c2b-958f-0054467d4dd7",
    "data": {
      "previousState": "Processing",
      "state": "Finished"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Veri nesnesi, aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| previousState | dize | Olay önce iş durumu. |
| durum | dize | Bu durumda bildirilmesini işi yeni durumu. Örneğin, "kuyruğa alındı: iş kaynakları bekliyor" veya "zamanlanmış: işi başlatmak hazır".|

Nereden iş durumu aşağıdakilerden biri olabilir değerleri: *sıraya alınan*, *zamanlanmış*, *işleme*, *tamamlandı*, *hata*, *İptal*, *iptal ediliyor*

## <a name="liveeventconnectionrejected"></a>LiveEventConnectionRejected

Aşağıdaki örnek, şemasını gösterir **LiveEventConnectionRejected** olay: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaServices/<account-name>",
    "subject": "/LiveEvents/MyLiveEvent1",
    "eventType": "Microsoft.Media.LiveEventConnectionRejected",
    "eventTime": "2018-01-16T01:57:26.005121Z",
    "id": "b303db59-d5c1-47eb-927a-3650875fded1",
    "data": { 
      "StreamId":"Mystream1",
      "IngestUrl": "http://abc.ingest.isml",
      "EncoderIp": "118.238.251.xxx",
      "EncoderPort": 52859,
      "ResultCode": "MPE_INGEST_CODEC_NOT_SUPPORTED"
    },
    "dataVersion": "1.0"
  }
]
```

Veri nesnesi, aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| Streamıd | dize | Bağlantı ve akış tanımlayıcısı. Alma URL'si bu kimliği eklemek için Kodlayıcı veya müşteri sorumludur. |  
| IngestUrl | dize | Canlı etkinliği tarafından sağlanan URL alın. |  
| EncoderIp | dize | Kodlayıcı IP'si. |
| EncoderPort | dize | Gelen bu akış nereden geldiğini bir kodlayıcının bağlantı noktası. |
| ResultCode | dize | Bunun nedeni bağlantı reddedildi. Sonuç kodları, aşağıdaki tabloda listelenmiştir. |

Sonuç kodları şunlardır:

| Sonuç kodu | Açıklama |
| ----------- | ----------- |
| MPE_RTMP_APPID_AUTH_FAILURE | Yanlış alma URL'si |
| MPE_INGEST_ENCODER_CONNECTION_DENIED | Kodlayıcı IP içinde IP hazır değilse izin verilenler listesi yapılandırılmış |
| MPE_INGEST_RTMP_SETDATAFRAME_NOT_RECEIVED | Kodlayıcı stream hakkında meta veri gönderin. |
| MPE_INGEST_CODEC_NOT_SUPPORTED | Belirtilen codec desteklenmez. |
| MPE_INGEST_DESCRIPTION_INFO_NOT_RECEIVED | Alma ve üst bilgisi bu akış için önce bir parça aldı. |
| MPE_INGEST_MEDIA_QUALITIES_EXCEEDED | Belirtilen kalitelerini sayısı izin verilen maksimum sınırı aşıyor. |
| MPE_INGEST_BITRATE_AGGREGATED_EXCEEDED | Toplam hızı maksimum izin verilen sınırı aşıyor. |
| MPE_RTMP_FLV_TAG_TIMESTAMP_INVALID | RTMP kodlayıcıdan video veya ses FLVTag için zaman damgası geçersiz. |

## <a name="liveeventencoderconnected"></a>LiveEventEncoderConnected

Aşağıdaki örnek, şemasını gösterir **LiveEventEncoderConnected** olay: 

```json
[
  { 
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventEncoderConnected",
    "eventTime": "2018-08-07T23:08:09.1710643",
    "id": "<id>",
    "data": {
      "ingestUrl": "http://mle1-amsts03mediaacctgndos-ts031.channel.media.azure-test.net:80/ingest.isml",
      "streamId": "15864-stream0",
      "encoderIp": "131.107.147.xxx",
      "encoderPort": "27485"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Veri nesnesi, aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| Streamıd | dize | Bağlantı ve akış tanımlayıcısı. Kodlayıcı veya müşteri bu kimliği alma URL'si sağlamaktan sorumludur. |
| IngestUrl | dize | Canlı etkinliği tarafından sağlanan URL alın. |
| EncoderIp | dize | Kodlayıcı IP'si. |
| EncoderPort | dize | Gelen bu akış nereden geldiğini bir kodlayıcının bağlantı noktası. |

## <a name="liveeventencoderdisconnected"></a>LiveEventEncoderDisconnected

Aşağıdaki örnek, şemasını gösterir **LiveEventEncoderDisconnected** olay: 

```json
[
  { 
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventEncoderDisconnected",
    "eventTime": "2018-08-07T23:08:09.1710872",
    "id": "<id>",
    "data": {
      "ingestUrl": "http://mle1-amsts03mediaacctgndos-ts031.channel.media.azure-test.net:80/ingest.isml",
      "streamId": "15864-stream0",
      "encoderIp": "131.107.147.xxx",
      "encoderPort": "27485",
      "resultCode": "S_OK"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Veri nesnesi, aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| Streamıd | dize | Bağlantı ve akış tanımlayıcısı. Alma URL'si bu kimliği eklemek için Kodlayıcı veya müşteri sorumludur. |  
| IngestUrl | dize | Canlı etkinliği tarafından sağlanan URL alın. |  
| EncoderIp | dize | Kodlayıcı IP'si. |
| EncoderPort | dize | Gelen bu akış nereden geldiğini bir kodlayıcının bağlantı noktası. |
| ResultCode | dize | Kesme Kodlayıcı nedeni. Normal bağlantıyı kes olabilir veya bir hata oluştu. Sonuç kodları, aşağıdaki tabloda listelenmiştir. |

Hata sonuç kodları şunlardır:

| Sonuç kodu | Açıklama |
| ----------- | ----------- |
| MPE_RTMP_SESSION_IDLE_TIMEOUT | RTMP oturumu için izin verilen süre sınırı boşta kaldıktan sonra zaman aşımına uğradı. |
| MPE_RTMP_FLV_TAG_TIMESTAMP_INVALID | RTMP kodlayıcıdan video veya ses FLVTag için zaman damgası geçersiz. |
| MPE_CAPACITY_LIMIT_REACHED | Çok hızlı veri gönderen Kodlayıcı. |
| Bilinmeyen hata kodları | Bu hata kodları karma eşleme girişleri çoğaltmak için bellek hatası değişebilir. |

Normal bağlantıyı kes sonuç kodları şunlardır:

| Sonuç kodu | Açıklama |
| ----------- | ----------- |
| S_OK | Kodlayıcı bağlantısı başarıyla kesildi. |
| MPE_CLIENT_TERMINATED_SESSION | Kodlayıcı (RTMP) bağlantısı kesildi. |
| MPE_CLIENT_DISCONNECTED | Kodlayıcı (FMP4) bağlantısı kesildi. |
| MPI_REST_API_CHANNEL_RESET | Kanal sıfırlama komutu aldı. |
| MPI_REST_API_CHANNEL_STOP | Kanal Durdur komutu aldı. |
| MPI_REST_API_CHANNEL_STOP | Kanal bakıma alınıyor. |
| MPI_STREAM_HIT_EOF | EOF stream Kodlayıcı tarafından gönderilir. |

## <a name="liveeventincomingdatachunkdropped"></a>LiveEventIncomingDataChunkDropped

Aşağıdaki örnek, şemasını gösterir **LiveEventIncomingDataChunkDropped** olay: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaServices/<account-name>",
    "subject": "/LiveEvents/MyLiveEvent1",
    "eventType": "Microsoft.Media.LiveEventIncomingDataChunkDropped",
    "eventTime": "2018-01-16T01:57:26.005121Z",
    "id": "03da9c10-fde7-48e1-80d8-49936f2c3e7d",
    "data": { 
      "TrackType": "Video",
      "TrackName": "Video",
      "Bitrate": 300000,
      "Timestamp": 36656620000,
      "Timescale": 10000000,
      "ResultCode": "FragmentDrop_OverlapTimestamp"
    },
    "dataVersion": "1.0"
  }
]
```

Veri nesnesi, aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| TrackType | dize | İzleme türü (Ses / Video). |
| TrackName | dize | İzleme adı. |
| Bit hızı | integer | İzleme hızı. |
| Zaman damgası | dize | Bırakılan veri öbeğin zaman damgası. |
| Zaman Çizelgesi | dize | Zaman damgası ölçeğini. |
| ResultCode | dize | Verileri öbek açılan açıklaması. **FragmentDrop_OverlapTimestamp** veya **FragmentDrop_NonIncreasingTimestamp**. |

## <a name="liveeventincomingstreamreceived"></a>LiveEventIncomingStreamReceived

Aşağıdaki örnek, şemasını gösterir **LiveEventIncomingStreamReceived** olay: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventIncomingStreamReceived",
    "eventTime": "2018-08-07T23:08:10.5069288Z",
    "id": "7f939a08-320c-47e7-8250-43dcfc04ab4d",
    "data": {
      "ingestUrl": "http://mle1-amsts03mediaacctgndos-ts031.channel.media.azure-test.net:80/ingest.isml/Streams(15864-stream0)15864-stream0",
      "trackType": "video",
      "trackName": "video",
      "bitrate": 2962000,
      "encoderIp": "131.107.147.xxx",
      "encoderPort": "27485",
      "timestamp": "15336831655032322",
      "duration": "20000000",
      "timescale": "10000000"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Veri nesnesi, aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| TrackType | dize | İzleme türü (Ses / Video). |
| TrackName | dize | İzleme adı (ya da sunucu Kodlayıcı tarafından veya, RTMP olması durumunda, oluşturur sağlanan *TrackType_Bitrate* biçimi). |
| Bit hızı | integer | İzleme hızı. |
| IngestUrl | dize | Canlı etkinliği tarafından sağlanan URL alın. |
| EncoderIp | dize  | Kodlayıcı IP'si. |
| EncoderPort | dize | Gelen bu akış nereden geldiğini bir kodlayıcının bağlantı noktası. |
| Zaman damgası | dize | Alınan verileri öbek ilk zaman damgası. |
| Zaman Çizelgesi | dize | Ölçeği, zaman damgası gösterilir. |

## <a name="liveeventincomingstreamsoutofsync"></a>LiveEventIncomingStreamsOutOfSync

Aşağıdaki örnek, şemasını gösterir **LiveEventIncomingStreamsOutOfSync** olay: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventIncomingStreamsOutOfSync",
    "eventTime": "2018-08-10T02:26:20.6269183Z",
    "id": "b9d38923-9210-4c2b-958f-0054467d4dd7",
    "data": {
      "minLastTimestamp": "319996",
      "typeOfStreamWithMinLastTimestamp": "Audio",
      "maxLastTimestamp": "366000",
      "typeOfStreamWithMaxLastTimestamp": "Video"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Veri nesnesi, aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| MinLastTimestamp | dize | Son zaman damgaları tüm parçalar (ses veya video) arasında en az. |
| TypeOfTrackWithMinLastTimestamp | dize | En düşük son zaman damgası ile izleme (ses veya video) türü. |
| MaxLastTimestamp | dize | Tüm parçaları (ses veya video) arasında tüm zaman damgaları en fazla. |
| TypeOfTrackWithMaxLastTimestamp | dize | En son zaman damgası ile izleme (ses veya video) türü. |

## <a name="liveeventincomingvideostreamsoutofsync"></a>LiveEventIncomingVideoStreamsOutOfSync

Aşağıdaki örnek, şemasını gösterir **LiveEventIncomingVideoStreamsOutOfSync** olay: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaServices/<account-name>",
    "subject": "/LiveEvents/LiveEvent1",
    "eventType": "Microsoft.Media.LiveEventIncomingVideoStreamsOutOfSync",
    "eventTime": "2018-01-16T01:57:26.005121Z",
    "id": "6dd4d862-d442-40a0-b9f3-fc14bcf6d750",
    "data": {
      "FirstTimestamp": "2162058216",
      "FirstDuration": "2000",
      "SecondTimestamp": "2162057216",
      "SecondDuration": "2000"
    },
    "dataVersion": "1.0"
  }
]
```

Veri nesnesi, aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| FirstTimestamp | dize | Zaman damgası türü video izler/kalite düzeylerinden birini alındı. |
| FirstDuration | dize | İlk zaman damgası ile verileri öbek süresi. |
| SecondTimestamp | dize  | Zaman damgası, bazı diğer izleme/kalite düzeyini video türü alındı. |
| SecondDuration | dize | İkinci zaman damgası ile verileri öbek süresi. |

## <a name="liveeventingestheartbeat"></a>LiveEventIngestHeartbeat

Aşağıdaki örnek, şemasını gösterir **LiveEventIngestHeartbeat** olay: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventIngestHeartbeat",
    "eventTime": "2018-08-07T23:17:57.4610506",
    "id": "7f450938-491f-41e1-b06f-c6cd3965d786",
    "data": {
      "trackType": "audio",
      "trackName": "audio",
      "bitrate": 160000,
      "incomingBitrate": 155903,
      "lastTimestamp": "15336837535253637",
      "timescale": "10000000",
      "overlapCount": 0,
      "discontinuityCount": 0,
      "nonincreasingCount": 0,
      "unexpectedBitrate": false,
      "state": "Running",
      "healthy": true
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Veri nesnesi, aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| TrackType | dize | İzleme türü (Ses / Video). |
| TrackName | dize | İzleme adı (ya da sunucu Kodlayıcı tarafından veya, RTMP olması durumunda, oluşturur sağlanan *TrackType_Bitrate* biçimi). |
| Bit hızı | integer | İzleme hızı. |
| IncomingBitrate | integer | Kodlayıcıdan gelen veri öbekleri göre hesaplanan hızı. |
| LastTimestamp | dize | Son 20 saniye cinsinden bir parçası için alınan son zaman damgası. |
| Zaman Çizelgesi | dize | Ölçeği zaman damgaları cinsinden ifade edilir. |
| OverlapCount | integer | Veri öbeği sayısı son 20 saniye cinsinden zaman damgaları çakışan. |
| DiscontinuityCount | integer | Son 20 saniye içinde gözlemlenen discontinuities sayısı. |
| NonIncreasingCount | integer | Geçmişteki zaman damgalı veri öbeği sayısı son 20 saniye içinde alınmadı. |
| UnexpectedBitrate | bool | Son 20 saniye cinsinden izin verilenden fazla sınırı tarafından beklenen ve gerçek bit hızlarına dönüştürme farklıysa. True ise ve yalnızca, IncomingBitrate olan > = 2 * hızı veya IncomingBitrate < = hızı/2 veya IncomingBitrate = 0. |
| Durum | dize | Canlı etkinlik durumu. |
| Sorunsuz | bool | Belirtir olup olmadığını alma sayıları ve bayrakları göre kötü durumda. Sağlıklı true ise, OverlapCount 0 = & & DiscontinuityCount 0 = & & NonIncreasingCount 0 = & & UnexpectedBitrate = false. |

## <a name="liveeventtrackdiscontinuitydetected"></a>LiveEventTrackDiscontinuityDetected

Aşağıdaki örnek, şemasını gösterir **LiveEventTrackDiscontinuityDetected** olay: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventTrackDiscontinuityDetected",
    "eventTime": "2018-08-07T23:18:06.1270405Z",
    "id": "5f4c510d-5be7-4bef-baf0-64b828be9c9b",
    "data": {
      "trackName": "video",
      "previousTimestamp": "15336837615032322",
      "trackType": "video",
      "bitrate": 2962000,
      "newTimestamp": "15336837619774273",
      "discontinuityGap": "575284",
      "timescale": "10000000"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Veri nesnesi, aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| TrackType | dize | İzleme türü (Ses / Video). |
| TrackName | dize | İzleme adı (ya da sunucu Kodlayıcı tarafından veya, RTMP olması durumunda, oluşturur sağlanan *TrackType_Bitrate* biçimi). |
| Bit hızı | integer | İzleme hızı. |
| PreviousTimestamp | dize | Önceki parça zaman damgası. |
| NewTimestamp | dize | Zaman damgası geçerli parça. |
| DiscontinuityGap | dize | Yukarıdaki iki zaman damgaları arasındaki boşluk. |
| Zaman Çizelgesi | dize | Hangi zaman damgası hem süreksizlik boşluk ölçeğinde temsil edilir. |

## <a name="common-event-properties"></a>Ortak olay özellikleri

Bir olay aşağıdaki üst düzey veri vardır:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| konu başlığı | dize | EventGrid konu. Bu özellik, Media Services hesabı kaynak kimliği vardır. |
| Konu | dize | Media Services kanalın Media Services hesabı altında kaynak yolu. İş için kaynak kimliği konusu ve konu verin birleştiriliyor. |
| olay türü | dize | Bu olay kaynağı için kayıtlı olay türlerinden biri. Örneğin, "Microsoft.Media.JobStateChange". |
| eventTime | dize | Olayın oluşturulduğu zamandan, sağlayıcının UTC saatini temel alan. |
| id | dize | Olayın benzersiz tanımlayıcısı. |
| veri | object | Media Services olay verileri. |
| dataVersion | dize | Veri nesnesinin şema sürümü. Yayımcı, şema sürümü tanımlar. |
| metadataVersion | dize | Olay meta verilerinin şema sürümü. Event Grid, şemanın en üst düzey özellikleri tanımlar. Event Grid, bu değeri sağlar. |

## <a name="next-steps"></a>Sonraki adımlar

[İş durumu değişikliği olayları için kaydolun](job-state-events-cli-how-to.md)