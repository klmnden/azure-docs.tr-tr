---
title: API Başvurusu - özel karar alma hizmeti
titlesuffix: Azure Cognitive Services
description: Custom Decision Service için eksiksiz bir API Kılavuzu.
services: cognitive-services
author: slivkins
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-decision-service
ms.topic: conceptual
ms.date: 05/11/2018
ms.author: slivkins
ms.openlocfilehash: be9966f5d8e8d94aa3f49aac91b35b105195b108
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60510950"
---
# <a name="api"></a>API

Azure Custom Decision Service, her bir kararı adlı iki API sağlar: [sıralaması API](#ranking-api) eylemlerin sıralamasını giriş ve [ödül API](#reward-api) ödül çıkarmak için. Ayrıca, sağladığınız bir [eylem API kümesini](#action-set-api-customer-provided) Azure Custom Decision Service eylemleri belirtmek için. Bu makalede, bu üç API'leri ele alınmıştır. Tipik bir senaryo aşağıda Custom Decision Service makaleleri sıralamasını iyileştirir göstermek için kullanılır.

## <a name="ranking-api"></a>API sıralaması

Standart bir derecelendirme API kullanan [JSONP](https://en.wikipedia.org/wiki/JSONP)-Stili iletişim deseni gecikme süresini en iyi duruma getirmek ve atlama [aynı çıkış noktası İlkesi](https://en.wikipedia.org/wiki/Same-origin_policy). İkinci JavaScript, sayfanın kaynağı dışında verileri getiriliyor öğesinden engeller.

Bu kod parçacığı, ön sayfanızın (burada kişiselleştirilmiş makalelerinin listesi görüntülenir) HTML baş ekleyin:

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
> Geri çağırma işlevi sıralaması API'yi çağırmadan önce tanımlanmalıdır.

> [!TIP]
> Gecikme süresini iyileştirmek için sıralama API HTTPS yerine HTTP olarak sunulan `https://ds.microsoft.com/api/v2/<appId>/rank/*`.
> Ancak, ön sayfa HTTPS sunulan, HTTPS uç noktasının kullanılmalıdır.

Parametreleri kullanılmadığında JSONP biçimli bir dize sıralama API gelen HTTP yanıtında olduğu:

```json
callback({
   "ranking":[{"id":"url1"}, {"id":"url2"}, {"id":"url3"}],
   "eventId":"<opaque event string>",
   "appId":"<your app id>",
   "actionSets":[{"id":"<A1>","lastRefresh":"2017-04-29T22:34:25.3401438Z"},
                 {"id":"<A2>","lastRefresh":"2017-04-30T22:34:25.3401438Z"}]});
```

Daha sonra tarayıcı `callback()` işlevine bir çağrı olarak bu dizeyi yürütür.

Önceki örnekte geri çağırma işlevi için parametre aşağıdaki şemayı sahiptir:

- `ranking` Görüntülenecek URL'leri sıralamasını sağlar.
- `eventId` Custom Decision Service tarafından Bu derecelendirme karşılık gelen bir tıklama ile eşleşmesi için dahili olarak kullanılır.
- `appId` aynı Web sayfasında çalıştırılan Custom Decision Service, birden çok uygulama arasında ayrım yapmak geri çağırma işlevine izin verir.
- `actionSets` Son başarılı yenileme UTC zaman damgası ile birlikte sıralaması API çağrısında kullanılan her bir eylemin listeler. Özel karar alma hizmeti, eylem kümesi akışları düzenli aralıklarla yeniler. Örneğin, eylem kümelerini bazıları geçerli değil, kendi varsayılan derecelendirmeden dönmesi geri çağırma işlevi gerekebilir.

> [!IMPORTANT]
> Belirtilen eylem kümelerini işlendi ve büyük olasılıkla, varsayılan sıralaması makalelerinin oluşturmak için ayıklama. Varsayılan derecelendirme yeniden sonra HTTP yanıtında. Varsayılan derecelendirme burada tanımlanır:
>
> - Her eylem kümesinde (15'den fazla döndürülmezse) makaleleri 15 en son makalelere ayıklanır.
> - Birden çok eylem kümesi belirtildiğinde, API çağrısı ile aynı sırada birleştirilir. Özgün sıralaması makalelerinin her eylem kümesinde korunur. Yinelenen önceki kopyalar ile değiştiriliyor kaldırılır.
> - İlk `n` makaleler, makaleler, birleştirilmiş listesinden tutulur burada `n=20` varsayılan olarak.

### <a name="ranking-api-with-parameters"></a>API parametrelerle sıralaması

Derecelendirme API, bu parametreleri sağlar:

- `details=1` ve `details=2` listelenen her bir makaleyi hakkında ek ayrıntılar ekler `ranking`.
- `limit=<n>` içinde varsayılan derecelendirme makale düzeyde sayısını belirtir. `n` arasında olmalıdır `2` ve `30` (veya başka şekilde kesilir `2` veya `30`sırasıyla).
- `dnt=1` Kullanıcı tanımlama bilgilerini devre dışı bırakır.

Parametreleri birleştirilebilir standart, sorgu dizesi sözdiziminde, örneğin `details=2&dnt=1`.

> [!IMPORTANT]
> Varsayılan ayar Avrupa'daki olmalıdır `dnt=1` müşteri için tanımlama bilgisi başlığı kabul edene kadar. Reşit olmayanların hedef Web siteleri için varsayılan ayar de olması gerekir. Daha fazla bilgi için [kullanım koşullarını](https://www.microsoft.com/cognitive-services/en-us/legal/CognitiveServicesTerms20160804).

`details=1` Öğe ekler her makalenin `guid`, eylem API kümesini tarafından sunulur. HTTP yanıtı:

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

`details=2` Öğesi ekler Custom Decision Service, makalelerdeki SEO etiketleri ayıklamak, daha fazla ayrıntı [özellik kazandırma sayesinde kod](https://github.com/Microsoft/mwt-ds/tree/master/Crawl):

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

`<details>` Öğesi:

```json
[{"guid":"123"}, {"description":"some text", "ds_id":"234", "image":"ImageUrl1", "title":"some text"}]
```

## <a name="reward-api"></a>Ödül API

Özel karar alma hizmeti kullanır, yalnızca üst yuvada tıklar. Her tıklatın, bir ödül 1 yorumlanır. Bir eksikliği bir ödül 0 yorumlanır. Tarafından oluşturulan olay kimlikleri, kullanarak tıklama ile ilgili sonuçlarımızda eşleştirilir [sıralaması API](#ranking-api) çağırın. Gerekirse, olay kimlikleri oturum tanımlama bilgileri ile geçirilebilir.

Üst yuvası click işlemek için bu kodu ön sayfanızda yerleştirin:

```javascript
$.ajax({
    type: "POST",
    url: '//ds.microsoft.com/api/v2/<appId>/reward/' + data.eventId,
    contentType: "application/json" })
```

Burada `data` bağımsız değişkenidir `callback()` , daha önce açıklandığı gibi işlev. Kullanarak `data` tıklatın işleme kodunu bazı dikkat gerektirir. Bu konuda bir örnek gösterilir [öğretici](custom-decision-service-tutorial-news.md#use-the-apis).

Yalnızca test için ödül API aracılığıyla çağrılabilir [cURL](https://en.wikipedia.org/wiki/CURL):

```sh
curl -v https://ds.microsoft.com/api/v2/<appId>/reward/<eventId> -X POST -d 1 -H "Content-Type: application/json"
```

Bir HTTP yanıtının 200 (Tamam) beklenen etkisidir. (Bir Azure depolama hesabı anahtarı portalda sağlandıysa) 1 ödül bu olay günlüğünde görebilirsiniz.

## <a name="action-set-api-customer-provided"></a>Eylem API kümesini (müşteri tarafından sağlanan)

Yüksek bir düzeyde, eylem API kümesini (Eylemler) makalelerin listesini döndürür. Her bir makaleyi URL'sini ve (isteğe bağlı olarak) makale başlığı ve yayın tarihi ile belirtilir. Portalda birkaç eylem ayarlar belirtebilirsiniz. Farklı bir eylem API kümesini her işlem kümesi için farklı bir URL olarak kullanılmalıdır.

Her eylem API kümesini iki şekilde uygulanabilir: bir RSS akışı veya Atom akışı olarak. Ya da standardına uygun ve doğru bir XML döndür. RSS için bir örnek aşağıda verilmiştir:

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

Her üst düzey `<item>` öğesi eylem açıklar:

- `<link>` zorunludur ve bir eylem kimliği olarak kullanılır
- `<date>` küçük veya bu öğeleri 15 eşit olup olmadığını göz ardı edilir; Aksi takdirde, zorunludur.
  - 15'ten fazla öğe varsa, en son 15 olanları kullanılır.
  - Standart biçim RSS veya Atom, sırasıyla olmalıdır:
    - [RFC 822](https://tools.ietf.org/html/rfc822) RSS: Örneğin, `"Fri, 28 Apr 2017 18:02:06 GMT"`
    - [RFC 3339](https://tools.ietf.org/html/rfc3339) için Atom: Örneğin, `"2016-12-19T16:39:57-08:00"`
- `<title>` isteğe bağlıdır ve makalede özellikler oluşturmak için kullanılır.
- `<guid>` İsteğe bağlı ve sistemdeki geri çağırma işlevine geçirilen (varsa `?details` parametresi belirtildiğinde sıralaması API çağrısında).

Diğer öğeler içinde bir `<item>` göz ardı edilir.

Atom sürümü kullanır ve aynı XML sözdizimi kurallarına akış.

> [!TIP]
> Sisteminizi kendi makale kimlikleri kullanıyorsa, kullanıcılar tarafından geri çağırma işlevine kullanılarak geçirilebilir `<guid>`.
