---
title: API - Azure Bilişsel hizmetler | Microsoft Docs
description: Tam ve kullanımı kolay API için kılavuz Azure özel karar hizmeti, bulut tabanlı bir API deneyimiyle keskinleştirir bağlamsal karar vermek için.
services: cognitive-services
author: slivkins
manager: slivkins
ms.service: cognitive-services
ms.topic: article
ms.date: 05/11/2018
ms.author: slivkins
ms.reviewer: marcozo, alekh
ms.openlocfilehash: 403b17e33394016a07a7b33ba1bcbfe6afdcc05b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354952"
---
# <a name="api"></a>API

Azure özel karar hizmeti için her bir kararı adlı iki API'ler sağlar: [sıralaması API](#ranking-api) Eylemler sıralamasını giriş ve [ödül API](#reward-api) ödül çıktısını almak için. Ayrıca, sağladığınız bir [eylem API kümesini](#action-set-api-customer-provided) Azure özel karar hizmet eylemleri belirtmek için. Bu makalede, bu üç API'leri yer almaktadır. Tipik bir senaryo, aşağıdaki makaleler sıralamasını özel karar hizmet en iyi duruma getirir, göstermek için kullanılır.

## <a name="ranking-api"></a>API sıralaması

Standart bir derecelendirme API'sini kullanan [JSONP](https://en.wikipedia.org/wiki/JSONP)-gecikme süresi en iyi duruma getirme ve atlamak için stil iletişim deseni [kaynak aynı ilke](https://en.wikipedia.org/wiki/Same-origin_policy). İkinci JavaScript, sayfanın kaynak dışından verileri getirme engelliyor.

Bu kod parçacığında (burada makalelerin kişiselleştirilmiş bir listesi görüntülenir) ön sayfanızı HTML head ekleyin:

```html
// define the "callback function" to render UI
<script> function callback(data) { … } </script>
// "data" provides the ranking of URLs, see example below.

// call to Ranking API
<script src="https://ds.microsoft.com/api/v2/<appId>/rank/<actionSetId>?<parameters>" async></script>
// action set id is the name of the corresponding RSS/Atom feed.

// same call with multiple action sets:
// <script src="https://ds.microsoft.com/api/v2/<appId>/rank/<A1>/<A2>/.../<An>?<parameters>" async></script>
```

> [!IMPORTANT]
> Geri çağırma işlevi sıralaması API çağırmadan önce tanımlanmalıdır.

> [!TIP]
> Gecikme süresini artırmak için sıralama API HTTPS yerine HTTP olarak sunulan `http://ds.microsoft.com/api/v2/<appId>/rank/*`.
> Ancak, bir HTTPS uç noktası ön sayfa HTTPS sunulan varsa kullanılmalıdır.

Parametreleri kullanılmadığı zaman sıralaması API gelen HTTP yanıtı JSONP olarak biçimlendirilmiş bir dize değil:

```json
callback({
   "ranking":[{"id":"url1"}, {"id":"url2"}, {"id":"url3"}],
   "eventId":"<opaque event string>",
   "appId":"<your app id>",
   "actionSets":[{"id":"<A1>","lastRefresh":"2017-04-29T22:34:25.3401438Z"},
                 {"id":"<A2>","lastRefresh":"2017-04-30T22:34:25.3401438Z"}]});
```

Tarayıcı için bir çağrı olarak bu dize ardından yürütür `callback()` işlevi.

Önceki örnekte geri çağırma işlevi için parametre aşağıdaki şema içeriyor:

- `ranking` Görüntülenecek URL'leri sıralamasını sağlar.
- `eventId` Özel karar hizmeti tarafından Bu derecelendirme karşılık gelen tıklama ile eşleşmesi için dahili olarak kullanılır.
- `appId` birden çok uygulama özel karar aynı Web sayfasında çalıştıran hizmetinin ayırt etmek geri çağırma işlevi sağlar.
- `actionSets` Derecelendirme API çağrısında son başarılı yenileme UTC zaman damgası ile birlikte kullanılan her eylem listeler. Özel karar hizmet eylem kümesi akışları düzenli aralıklarla yeniler. Bazı eylem kümelerini geçerli değilse, örneğin, geri çağırma işlevini geri dönüş için kendi varsayılan derecelendirme gerekebilir.

> [!IMPORTANT]
> Belirtilen eylem kümelerini işlenen ve büyük olasılıkla, makaleler varsayılan sıralamasını oluşturmak üzere ayıklandı. Varsayılan derecelendirme kaldırılmasında sonra bir HTTP yanıtı döndürdü. Varsayılan derecelendirme burada tanımlanır:
>
> - (Birden fazla 15 döndürülmezse) her eylem kümesi içinde 15 en son makaleleri makaleleri ayıklanır.
> - Birden çok eylem kümelerini belirtildiğinde, API çağrısı olduğu gibi aynı sırayla birleştirilir. Özgün makalelerinin sıralama her eylem kümesinde korunur. Yinelenen lehinde önceki kopyalar kaldırılır.
> - İlk `n` makaleler, makaleler, birleştirilmiş listesinden tutulur nerede `n=20` varsayılan olarak.

### <a name="ranking-api-with-parameters"></a>API parametrelerle sıralaması

Derecelendirme API Bu parametreler sağlar:

- `details=1` ve `details=2` listelenen her makale hakkında ek ayrıntılar ekler `ranking`.
- `limit=<n>` makaleler sayısı üst sınırından varsayılan derecelendirme belirtir. `n` arasında olmalıdır `2` ve `30` (veya başka şekilde kesilir `2` veya `30`sırasıyla).
- `dnt=1` Kullanıcı tanımlama bilgilerini devre dışı bırakır.

Parametreleri birleştirilebilir standart, sorgu dizesi sözdiziminde, örneğin `details=2&dnt=1`.

> [!IMPORTANT]
> Varsayılan ayar Avrupa'da olmalıdır `dnt=1` müşteri için tanımlama bilgisi başlık kabul edene kadar. Ayrıca reşit olmayanların hedef Web siteleri için varsayılan ayar olması gerekir. Daha fazla bilgi için bkz: [kullanım koşulları](https://www.microsoft.com/cognitive-services/en-us/legal/CognitiveServicesTerms20160804).

`details=1` Öğesi ekler her makalenin `guid`, eylem API kümesini tarafından sunulur. HTTP yanıtı:

```json
callback({
   "ranking":[{"id":"url1","details":[{"guid":"123"}]},
              {"id":"url2","details":[{"guid":"456"}]},
              {"id":"url3","details":[{"guid":"789"}]}],
   "eventId":"<opaque event string>",
   "appId":"<your app id>",
   "actionSets":[{"id":"<A1>","lastRefresh":"timeStamp1"},
                 {"id":"<A2>","lastRefresh":"timeStamp2"}]});
```

`details=2` Öğesi ekler özel karar hizmet makaleleri SEO etiketleri ayıklamak daha fazla ayrıntı [featurization kod](https://github.com/Microsoft/mwt-ds/tree/master/Crawl):

- `title` gelen `<meta property="og:title" content="..." />` veya `<meta property="twitter:title" content="..." />` veya `<title>...</title>`
- `description` gelen `<meta property="og:description" ... />` veya `<meta property="twitter:description" content="..." />` veya `<meta property="description" content="..." />`
- `image` Kaynak `<meta property="og:image" content="..." />`
- `ds_id` Kaynak `<meta name=”microsoft:ds_id” content="..." />`

HTTP yanıtı:

```json
callback({
   "ranking":[{"id":"url1","details":<details>},
              {"id":"url2","details":<details>},
              {"id":"url3","details":<details>}],
   "eventId":"<opaque event string>",
   "appId":"<your app id>",
   "actionSets":[{"id":"<A1>","lastRefresh":"timeStamp1"},
                 {"id":"<A2>","lastRefresh":"timeStamp2"}]});
```

`<details>` Öğe:

```json
[{"guid":"123"}, {"description":"some text", "ds_id":"234", "image":"ImageUrl1", "title":"some text"}]
```

## <a name="reward-api"></a>Ödül API

Özel karar hizmet kullanır üzerinde yalnızca üst yuvası tıklatır. Her tıklatma 1 ödül yorumlanır. Tıklama olmaması 0 ödül yorumlanır. Tıklama ile ilgili derecelendirmeleri tarafından oluşturulan olay kimlikleri kullanılarak eşleştirilir [sıralaması API](#ranking-api) çağırın. Gerekirse, olay kimlikleri oturum tanımlama bilgileri ile geçirilebilir.

Üst yuvası tıklama işlemek için bu kodu ön sayfanızda koyun:

```javascript
$.ajax({
    type: "POST",
    url: '//ds.microsoft.com/api/v2/<appId>/reward/' + data.eventId,
    contentType: "application/json" })
```

Burada `data` bağımsız değişkenidir `callback()` , daha önce açıklandığı gibi işlev. Kullanarak `data` tıklatın işleme kodunu bazı dikkat gerektirir. Bu örnek gösterilen [Öğreticisi](custom-decision-service-tutorial-news.md#use-the-apis).

Yalnızca sınama için ödül API aracılığıyla çağrılabilir [cURL](https://en.wikipedia.org/wiki/CURL):

```sh
curl -v https://ds.microsoft.com/api/v2/<appId>/reward/<eventId> -X POST -d 1 -H "Content-Type: application/json"
```

Bir HTTP yanıtının 200 (Tamam) beklenen etkisidir. (Portalda bir Azure depolama hesabı anahtarı sağlanmadı değilse), 1 ödül bu olay günlüğünde görebilirsiniz.

## <a name="action-set-api-customer-provided"></a>Eylem API kümesini (müşteri sağlanan)

Yüksek bir düzeyde, eylem API kümesini makalelerin (Eylemler) listesini döndürür. Her makalede URL'sini ve (isteğe bağlı) makale başlığı ve yayın tarihi ile belirtilir. Portalda birkaç eylem ayarlar belirtebilirsiniz. Farklı bir eylem API kümesini her eylem kümesi için farklı bir URL olarak kullanılmalıdır.

Her eylem API kümesini iki yolla uygulanabilir: bir RSS veya Atom akışı olarak. Ya da bir standardına uygun ve doğru XML Döndür gerekir. RSS için örnek aşağıda verilmiştir:

```xml
<rss version="2.0">
<channel>
   <item>
      <title><![CDATA[title (possibly with url) ]]></title>
      <link>url</link>
      <guid>guid (could be a your internal database id)</guid>
      <pubDate>date</pubDate>
    </item>
   <item>
       ....
   </item>
</channel>
</rss>
```

Her düzey `<item>` öğesi eylem açıklar:

- `<link>` zorunludur ve bir eylem kimliği olarak kullanılır
- `<date>` 15 öğeleri eşit veya daha az ise, göz ardı edilir; Aksi takdirde zorunludur.
  - 15'ten fazla öğe varsa, 15 en son olanları kullanılır.
  - Bu standart biçiminde RSS veya Atom, sırasıyla olması gerekir:
    - [RFC 822](https://tools.ietf.org/html/rfc822) RSS: Örneğin, `"Fri, 28 Apr 2017 18:02:06 GMT"`
    - [RFC 3339](https://tools.ietf.org/html/rfc3339) Atom için: Örneğin, `"2016-12-19T16:39:57-08:00"`
- `<title>` isteğe bağlıdır ve makalede belirtilen özellikleri oluşturmak için kullanılır.
- `<guid>` İsteğe bağlı ve geri çağırma işlevi sisteme aracılığıyla geçirilen (varsa `?details` parametresi belirtilmişse sıralaması API çağrısında).

İçindeki diğer öğeler bir `<item>` göz ardı edilir.

Atom sürüm kullandığı aynı XML sözdizimi ve kuralları akış.

> [!TIP]
> Sisteminiz kendi makale kimlikleri kullanıyorsa, bunlar geri çağırma işlevi tarafından içine kullanılarak geçirilebilir `<guid>`.