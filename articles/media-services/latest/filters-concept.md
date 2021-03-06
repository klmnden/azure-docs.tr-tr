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
ms.date: 05/23/2019
ms.author: juliako
ms.openlocfilehash: fdf29924da31db0347938df89e698cb258c2336b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66225422"
---
# <a name="filters"></a>Filtreler

İçeriğinizi müşterilere (canlı akış olayları veya isteğe bağlı Video) sunarken istemcinizi varsayılan varlığın bildirim dosyasında tanımlanan değerinden daha fazla esneklik gerekebilir. Azure Media Services sunar [dinamik bildirimlerini](filters-dynamic-manifest-overview.md) önceden tanımlanmış filtrelere göre. 

Filtreleri gibi şeyler müşterilerinize izin veren sunucu tarafı kurallar şunlardır: 

- Yalnızca bir bölümünü (tüm video oynatma) yerine bir video kayıttan yürütme. Örneğin:
  - Canlı etkinlik ("alt klip filtreleme"), alt klip göstermek için bildirim azaltmak veya
  - ("Kırpma video") bir video başlangıcını kesim.
- Yalnızca belirtilen yorumlama ve/veya içeriği ("işleme filtreleme") çalmak için kullanılan cihaz tarafından desteklenen belirtilen dil parçaları sunun. 
- ("Ayarlama sunu pencere") player DVR penceresinde sınırlı bir süre sağlamak amacıyla sunu penceresi DVR ayarlayın.

Medya Hizmetleri oluşturmanıza olanak tanır **hesap filtreleri** ve **varlık filtreleri** içeriğiniz için. Ayrıca, önceden oluşturulmuş filtrelerinizi ile ilişkilendirebilirsiniz. bir **akış Bulucu**.

## <a name="defining-filters"></a>Filtrelerin tanımlanması

İki tür filtre vardır: 

* [Hesap filtreleri](https://docs.microsoft.com/rest/api/media/accountfilters) (Genel) - Azure Media Services hesabı olan herhangi bir varlığı uygulanabilir, hesabın bir ömre sahiptir.
* [Varlık filtreleri](https://docs.microsoft.com/rest/api/media/assetfilters) (yerel) - yalnızca bir varlık ile filtre oluşturma sırasında ilişkilendirilmiş uygulanabilir, bir varlığın ömrünü sahip. 

**Hesap filtreleri** ve **varlık filtreleri** türleri tanımlama/tanımlayan filtre için tam olarak aynı özelliklere sahiptir. Oluştururken dışında **varlık filtre**, istediğiniz filtreyi ilişkilendirmek varlık adı belirtmeniz gerekir.

Senaryonuza bağlı olarak, hangi türde bir filtre daha uygun (varlık filtre Hesap Filtresi) mı karar verin. Hesap filtreleri trim belirli bir varlık için varlık filtreleri nerede kullanılabilir (işleme filtreleme) cihaz profilleri için uygundur.

Filtreler tanımlamak için aşağıdaki özellikleri kullanın. 

|Ad|Açıklama|
|---|---|
|firstQuality|Filtrenin ilk kalite hızı.|
|presentationTimeRange|Sunu zaman aralığı. Bu özellik, bildirim başlangıç/bitiş noktaları, sunu penceresi uzunluğu ve canlı bir başlangıç konumu filtreleme için kullanılır. <br/>Daha fazla bilgi için [PresentationTimeRange](#presentationtimerange).|
|Parçaları|Parçaları seçimi koşulları. Daha fazla bilgi için [izler](#tracks)|

### <a name="presentationtimerange"></a>presentationTimeRange

Bu özelliği kullanmak **varlık filtreleri**. Özellik ayarlamak için önerilmez **hesap filtreleri**.

|Ad|Açıklama|
|---|---|
|**endTimestamp**|Video isteğe bağlı (VoD) için geçerlidir.<br/>Canlı akış sunum için daha sessiz bir şekilde göz ardı uygulanan ve sunu sona erer ve akış olduğunda VoD.<br/>Bu bir sonraki en yakın GOP başına yuvarlanır sunum, mutlak bitiş noktasını temsil eden bir uzun bir değerdir. Birim ölçeği olduğundan bir endTimestamp 1800000000 biri için 3 dakika olacaktır.<br/>Çalma listesi (manifest) olacak parça kırpılacak startTimestamp ve endTimestamp kullanın.<br/>Örneğin, startTimestamp 40000000 ve endTimestamp = 100000000 = varsayılan ölçeği kullanarak parçaları arasındaki 4 saniye ve 10 saniyelik VoD sunu içeren bir çalma listesi üretir. Sınır bir parçasını ayrımı idare etmeye, tüm parça bildirime dahil edilir.|
|**forceEndTimestamp**|Yalnızca canlı akış için geçerlidir.<br/>EndTimestamp özelliği mevcut olup olmadığını gösterir. TRUE ise endTimestamp belirtilmesi gerekir veya bir hatalı istek kodunu döndürdü.<br/>İzin verilen değerler: yanlış, doğru.|
|**liveBackoffDuration**|Yalnızca canlı akış için geçerlidir.<br/> Bu değer, bir istemci için arama Canlı son konumunu tanımlar.<br/>Bu özelliği kullanarak, Canlı kayıttan yürütme konumu gecikme ve oyuncu için sunucu tarafı arabelleği oluşturun.<br/>Bu özellik için ölçeği (aşağıya bakın) birimidir.<br/>Geri süresi Canlı en fazla 300 saniye (3000000000) ' dir.<br/>Örneğin, bir değer gerçek Canlı edge'den Gecikmeli 20 saniye, en son kullanılabilir içerik 2000000000 anlamına gelir.|
|**presentationWindowDuration**|Yalnızca canlı akış için geçerlidir.<br/>Bir kayan bir pencere içinde bir çalma listesi dahil parçaların uygulanacak presentationWindowDuration kullanın.<br/>Bu özellik için ölçeği (aşağıya bakın) birimidir.<br/>Örneğin, presentationWindowDuration ayarlamak iki dakikalık kayan pencere uygulanacak 1200000000 =. Canlı Edge 2 dakika içinde Media Çalma listesinde dahil edilir. Sınır bir parçasını ayrımı idare etmeye, tüm parça çalma listesi dahil edilir. En düşük sunu pencere süresi 60 saniyedir.|
|**startTimestamp**|Video (VoD) isteğe bağlı veya canlı akış için geçerlidir.<br/>Bu akışın bir mutlak başlangıç noktasını temsil eden bir uzun bir değerdir. Değerin yuvarlanmış en yakın sonraki GOP başlatma. Birim ölçeği olduğundan bir startTimestamp 150000000, 15 saniye olacaktır.<br/>Çalma listesi (manifest) olacak parça kırpılacak startTimestamp ve endTimestampp kullanın.<br/>Örneğin, startTimestamp 40000000 ve endTimestamp = 100000000 = varsayılan ölçeği kullanarak parçaları arasındaki 4 saniye ve 10 saniyelik VoD sunu içeren bir çalma listesi üretir. Sınır bir parçasını ayrımı idare etmeye, tüm parça bildirime dahil edilir.|
|**Zaman Çizelgesi**|Tüm zaman damgaları ve süreleri bir saniye içinde artış sayısını belirtilen bir sunu zaman aralığı içinde geçerlidir.<br/>10000000 - on milyon artışlarla bir her artış 100 nanosaniyelik uzun ise olacağı saniye içinde varsayılandır.<br/>Örneğin, 30 saniyede bir startTimestamp ayarlamak istiyorsanız, varsayılan zaman ölçeğini kullanırken 300000000 değerini kullanırsınız.|

### <a name="tracks"></a>Parçaları

Akışınız (canlı akış ve isteğe bağlı Video) parçaları dinamik olarak oluşturulan bildirime eklenmelidir filtre izleme özelliği koşulları (FilterTrackPropertyConditions) listesine göre belirttiğiniz. Filtreler mantıksal kullanılarak birleştirilir **ve** ve **veya** işlemi.

Filtre izleme özelliği koşulları parça türleri, değerleri (aşağıdaki tabloda açıklanmıştır) ve işlemler (eşittir, eşit değildir) açıklanmaktadır. 

|Ad|Açıklama|
|---|---|
|**Bit hızı**|Bit hızını parça filtreleme için kullanın.<br/><br/>Saniyedeki bit bit hızlarında çeşitli önerilen değerdir. Örneğin, "0-2427000".<br/><br/>Not: 250000 (bit / saniye) gibi belirli hızı yer alan bir değer kullanabilirsiniz, ancak tam bit hızlarına dönüştürme başka bir varlığından dalgalanma gibi bu yaklaşım önerilmez.|
|**FourCC**|Filtreleme için izleme FourCC değerini kullanın.<br/><br/>Belirtilen codec biçim öğesinin ilk öğesinin değeridir [RFC 6381](https://tools.ietf.org/html/rfc6381). Şu anda aşağıdakileri destekler: <br/>Video: "Avc1", "hev1", "hvc1"<br/>Ses: "Mp4a", "AB-3."<br/><br/>Bir varlık parçalar FourCC değerlerini belirlemek için almak ve bildirim dosyasını inceleyin.|
|**Dil**|Filtreleme için izleme dili kullanın.<br/><br/>Belirtilen RFC 5646 eklemek istediğiniz bir dil etiketi değerdir. Örneğin, "en".|
|**Ad**|Filtreleme için izleme adını kullanın.|
|**Tür**|İzleme türü, filtreleme için kullanın.<br/><br/>Aşağıdaki değerlerine izin verilir: "Görüntü", "ses" veya "metin".|

### <a name="example"></a>Örnek

Aşağıdaki örnek, canlı akış bir filtre tanımlar: 

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

## <a name="associating-filters-with-streaming-locator"></a>Filtreler akış Bulucu ile ilişkilendirme

Bir listesini belirtebilirsiniz [varlık veya hesap filtreleri](filters-concept.md) üzerinde [akış Bulucu](https://docs.microsoft.com/rest/api/media/streaminglocators/create#request-body). [Dinamik Paketleyici](dynamic-packaging-overview.md) bu olanlar istemcinizin URL'SİNDE belirtir birlikte filtrelerinin listesi için geçerlidir. Bu birleşim oluşturur bir [dinamik bildirim](filters-dynamic-manifest-overview.md), URL'deki filtreleri + akış Bulucu üzerinde belirttiğiniz filtreleri temel. 

Aşağıdaki örneklere bakın:

* [İlişkilendirme filtrelerle akış Bulucu - .NET](filters-dynamic-manifest-dotnet-howto.md#associate-filters-with-streaming-locator)
* [Akış Bulucu - CLI ile ilişkilendirme filtreleri](filters-dynamic-manifest-cli-howto.md#associate-filters-with-streaming-locator)

## <a name="updating-filters"></a>Filtreleri güncelleştiriliyor
 
**Akış bulucuları** filtreleri güncelleştirildiği sırada güncelleştirilebilir değil. 

Bir etkin olarak yayımlanan ilişkili bir filtre tanımını güncelleştirmek için önerilmez **akış Bulucu**, özellikle CDN etkinleştirildiğinde. Akış sunucuları ve CDN'ler döndürülecek eski önbelleğe alınmış veri kaybına neden olabilir, iç önbellekler olabilir. 

Filtre tanımını değiştirilmesi gerekiyorsa, yeni bir filtre oluşturmak ve eklemeyi göz önünde bulundurun **akış Bulucu** URL veya yeni bir yayımlama **akış Bulucu** filtre doğrudan başvuruyor.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler filtre program aracılığıyla oluşturma işlemini göstermektedir.  

- [REST API'leri ile filtre oluşturma](filters-dynamic-manifest-rest-howto.md)
- [.NET ile filtre oluşturma](filters-dynamic-manifest-dotnet-howto.md)
- [CLI ile filtre oluşturma](filters-dynamic-manifest-cli-howto.md)

