---
title: Azure Media Services'da filtrelerin tanımlanması
description: Bu konuda, istemci akışı için bir stream'ın belirli bölümlerine kullanabilmesi için filtreler oluşturmayı açıklar. Media Services, bu seçmeli akış elde etmek için olan dinamik bildirimler oluşturur.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 02/12/2019
ms.author: juliako
ms.openlocfilehash: 09de372ffdb48c00fde9a43c07f8f8b574462d1f
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2019
ms.locfileid: "56405741"
---
# <a name="define-account-filters-and-asset-filters"></a>Hesap filtreleri ve varlık filtrelerini tanımlayın  

İçeriğinizi müşterilere (Canlı etkinlik veya isteğe bağlı Video akışı) sunarken istemcinizi varsayılan varlığın bildirim dosyasında tanımlanan değerinden daha fazla esneklik gerekebilir. Azure Media Services hesap filtreleri ve içeriğiniz için varlık filtrelerini tanımlamanızı sağlar. 

Filtreleri gibi şeyler müşterilerinize izin veren sunucu tarafı kurallar şunlardır: 

- Yalnızca bir bölümünü (tüm video oynatma) yerine bir video kayıttan yürütme. Örneğin:
  - Canlı etkinlik ("alt klip filtreleme"), alt klip göstermek için bildirim azaltmak veya
  - ("Kırpma video") bir video başlangıcını kesim.
- Yalnızca belirtilen yorumlama ve/veya içeriği ("işleme filtreleme") çalmak için kullanılan cihaz tarafından desteklenen belirtilen dil parçaları sunun. 
- ("Ayarlama sunu pencere") player DVR penceresinde sınırlı bir süre sağlamak amacıyla sunu penceresi DVR ayarlayın.

Medya Hizmetleri sunan [dinamik bildirimlerini](filters-dynamic-manifest-overview.md) önceden tanımlanmış filtrelere göre. İstemcilerinize filtreleri tanımladıktan sonra bunları akış URL'SİNDE kullanabilirsiniz. Filtreler hızı Uyarlamalı akış için uygulanabilir: Apple HTTP canlı akış (HLS), MPEG-DASH ve kesintisiz akış.

Aşağıdaki tabloda, filtrelerle URL'leri bazı örnekler gösterilmektedir:

|Protokol|Örnek|
|---|---|
|HLS V4|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl,filter=myAccountFilter)`|
|HLS V3|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3,filter=myAccountFilter)`|
|MPEG DASH|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf,filter=myAssetFilter)`|
|Kesintisiz Akış|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=myAssetFilter)`|

## <a name="define-filters"></a>Filtreleri tanımlar

Varlık filtreleri iki tür vardır: 

* [Hesap filtreleri](https://docs.microsoft.com/rest/api/media/accountfilters) (Genel) - Azure Media Services hesabı olan herhangi bir varlığı uygulanabilir, hesabın bir ömre sahiptir.
* [Varlık filtreleri](https://docs.microsoft.com/rest/api/media/assetfilters) (yerel) - yalnızca bir varlık ile filtre oluşturma sırasında ilişkilendirilmiş uygulanabilir, bir varlığın ömrünü sahip. 

[Hesap filtre](https://docs.microsoft.com/rest/api/media/accountfilters) ve [varlık filtre](https://docs.microsoft.com/rest/api/media/assetfilters) türleri tanımlama/tanımlayan filtre için tam olarak aynı özelliklere sahiptir. Oluştururken dışında **varlık filtre**, istediğiniz filtreyi ilişkilendirmek varlık adı belirtmeniz gerekir.

Senaryonuza bağlı olarak, hangi türde bir filtre daha uygun (varlık filtre Hesap Filtresi) mı karar verin. Hesap filtreleri trim belirli bir varlık için varlık filtreleri nerede kullanılabilir (işleme filtreleme) cihaz profilleri için uygundur.

Filtreler tanımlamak için aşağıdaki özellikleri kullanın. 

|Ad|Açıklama|
|---|---|
|firstQuality|Filtrenin ilk kalite hızı.|
|presentationTimeRange|Sunu zaman aralığı. Bu özellik, bildirim başlangıç/bitiş noktaları, sunu penceresi uzunluğu ve canlı bir başlangıç konumu filtreleme için kullanılır. <br/>Daha fazla bilgi için [PresentationTimeRange](#PresentationTimeRange).|
|Parçaları|Parçaları seçimi koşulları. Daha fazla bilgi için [izler](#tracks)|

### <a name="presentationtimerange"></a>PresentationTimeRange

Bu özelliği kullanmak **varlık filtreleri**. Özellik ayarlamak için önerilmez **hesap filtreleri**.

|Ad|Açıklama|
|---|---|
|**endTimestamp**|Mutlak bitiş zamanı sınırını. Video isteğe bağlı (VoD) için geçerlidir. Canlı bir sunumu için daha sessiz bir şekilde göz ardı uygulanan ve sunu sona erer ve akış olduğunda VoD.<br/><br/>Değerin bir mutlak akış uç noktasını temsil eder. Yuvarlatılmış en yakın sonraki GOP başlatma.<br/><br/>StartTimestamp ve EndTimestamp içerecek şekilde kırpmanıza çalma listesi (manifest) kullanın. Örneğin, StartTimestamp 40000000 ve EndTimestamp = = 100000000 StartTimestamp ve EndTimestamp arasında medya içeren bir çalma listesi üretir. Sınır bir parçasını ayrımı idare etmeye, tüm parça bildirime dahil edilir.<br/><br/>Ayrıca bkz **forceEndTimestamp** izleyen tanımı.|
|**forceEndTimestamp**|Canlı Filtreleri için geçerlidir.<br/><br/>**forceEndTimestamp** gösteren bir Boole değeri olup olmadığını **endTimestamp** geçerli bir değere ayarlandı. <br/><br/>Değer ise **true**, **endTimestamp** değeri belirtilmelidir. Ardından belirtilmezse, hatalı istek döndürdü.<br/><br/>Örneğin, giriş video 5 dakikada başlatılır bir filtre tanımlamak istediğiniz ve akış sonuna kadar bağlanabilmelerini, ayarlarsanız **forceEndTimestamp** false ve ayarın atlanarak **endTimestamp**.|
|**liveBackoffDuration**|Canlı yalnızca geçerlidir. Özelliği, Canlı kayıttan yürütme konumunu tanımlamak için kullanılır. Bu kural kullanarak, Canlı kayıttan yürütme konumu gecikme ve oyuncu için sunucu tarafı arabelleği oluşturun. LiveBackoffDuration göre Canlı konumdur. En fazla Canlı geri alma süresi 300 saniyedir.|
|**presentationWindowDuration**|Canlı için geçerlidir. Kullanım **presentationWindowDuration** çalma listesine bir kayan pencereye uygulamak için. Örneğin, presentationWindowDuration ayarlamak iki dakikalık kayan pencere uygulanacak 1200000000 =. Canlı Edge 2 dakika içinde Media Çalma listesinde dahil edilir. Sınır bir parçasını ayrımı idare etmeye, tüm parça çalma listesi dahil edilir. En düşük sunu pencere süresi 60 saniyedir.|
|**startTimestamp**|VoD veya canlı akış için geçerlidir. Değeri akışa ilişkin bir mutlak başlangıç noktasını temsil eder. Değerin yuvarlanmış en yakın sonraki GOP başlatma.<br/><br/>Kullanım **startTimestamp** ve **endTimestamp** içerecek şekilde kırpmanıza çalma listesi (bildirim). Örneğin, startTimestamp 40000000 ve endTimestamp = = 100000000 StartTimestamp ve EndTimestamp arasında medya içeren bir çalma listesi üretir. Sınır bir parçasını ayrımı idare etmeye, tüm parça bildirime dahil edilir.|
|**Zaman Çizelgesi**|VoD veya canlı akış için geçerlidir. Zaman damgaları tarafından kullanılan ölçeği ve yukarıda belirtilen süre. Varsayılan zaman ölçeğini 10000000 ' dir. Alternatif bir ölçeği kullanılabilir. 10000000 HNS (yüz içerir) varsayılandır.|

### <a name="tracks"></a>Parçaları

Akışınız (Canlı veya isteğe bağlı Video) parçaları dinamik olarak oluşturulan bildirime eklenmelidir filtre izleme özelliği koşulları (FilterTrackPropertyConditions) listesine göre belirttiğiniz. Filtreler mantıksal kullanılarak birleştirilir **ve** ve **veya** işlemi.

Filtre izleme özelliği koşulları parça türleri, değerleri (aşağıdaki tabloda açıklanmıştır) ve işlemler (eşittir, eşit değildir) açıklanmaktadır. 

|Ad|Açıklama|
|---|---|
|**Bit hızı**|Bit hızını parça filtreleme için kullanın.<br/><br/>Saniyedeki bit bit hızlarında çeşitli önerilen değerdir. Örneğin, "0-2427000".<br/><br/>Not: 250000 (bit / saniye) gibi belirli hızı yer alan bir değer kullanabilirsiniz, ancak tam bit hızlarına dönüştürme başka bir varlığından dalgalanma gibi bu yaklaşım önerilmez.|
|**FourCC**|Filtreleme için izleme FourCC değerini kullanın.<br/><br/>Belirtilen codec biçim öğesinin ilk öğesinin değeridir [RFC 6381](https://tools.ietf.org/html/rfc6381). Şu anda aşağıdakileri destekler: <br/>Video: "Avc1", "hev1", "hvc1"<br/>Ses: "Mp4a", "AB-3."<br/><br/>Bir varlık parçalar FourCC değerleri belirlemek için [almak ve bildirim dosyası inceleyin](#get-and-examine-manifest-files).|
|**Dil**|Filtreleme için izleme dili kullanın.<br/><br/>Belirtilen RFC 5646 eklemek istediğiniz bir dil etiketi değerdir. Örneğin, "en".|
|**Ad**|Filtreleme için izleme adını kullanın.|
|**Tür**|İzleme türü, filtreleme için kullanın.<br/><br/>Aşağıdaki değerlerine izin verilir: "Görüntü", "ses" veya "metin".|

## <a name="example"></a>Örnek

```json
{
  "properties": {
    "presentationTimeRange": {
      "startTimestamp": 0,
      "endTimestamp": 170000000,
      "presentationWindowDuration": 9223372036854776000,
      "liveBackoffDuration": 0,
      "timescale": 10000000,
      "forceEndTimestamp": false
    },
    "firstQuality": {
      "bitrate": 128000
    },
    "tracks": [
      {
        "trackSelections": [
          {
            "property": "Type",
            "operation": "Equal",
            "value": "Audio"
          },
          {
            "property": "Language",
            "operation": "NotEqual",
            "value": "en"
          },
          {
            "property": "FourCC",
            "operation": "NotEqual",
            "value": "EC-3"
          }
        ]
      },
      {
        "trackSelections": [
          {
            "property": "Type",
            "operation": "Equal",
            "value": "Video"
          },
          {
            "property": "Bitrate",
            "operation": "Equal",
            "value": "3000000-5000000"
          }
        ]
      }
    ]
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler filtre program aracılığıyla oluşturma işlemini göstermektedir.  

- [REST API'leri ile filtre oluşturma](filters-dynamic-manifest-rest-howto.md)
- [.NET ile filtre oluşturma](filters-dynamic-manifest-dotnet-howto.md)
- [CLI ile filtre oluşturma](filters-dynamic-manifest-cli-howto.md)

