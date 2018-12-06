---
title: Translator metin çevirisi API'si Çevir yöntemi
titleSuffix: Azure Cognitive Services
description: Translator metin API'si Çevir yöntemi kullanın.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: reference
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: bf13ca603927c85784e446157a79cd96fb70ca05
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52956997"
---
# <a name="translator-text-api-30-translate"></a>Translator metin çevirisi API'si 3.0: Çevir

Metni çevirir.

## <a name="request-url"></a>İstek URL'si

Gönderme bir `POST` isteği:

```HTTP
https://api.cognitive.microsofttranslator.com/translate?api-version=3.0
```

## <a name="request-parameters"></a>İstek parametreleri

Sorgu dizesinde geçirilen istek Parametreler şunlardır:

<table width="100%">
  <th width="20%">Sorgu parametresi</th>
  <th>Açıklama</th>
  <tr>
    <td>API sürümü</td>
    <td>*Gerekli parametre*.<br/>İstemci tarafından istenen API sürümü. Değer olmalıdır `3.0`.</td>
  </tr>
  <tr>
    <td>başlangıç</td>
    <td>*İsteğe bağlı parametre*.<br/>Giriş metninin dilini belirtir. Hangi dillerin bakarak gelen çevirmek kullanılabilir olduğunu bulmak [desteklenen diller](./v3-0-languages.md) kullanarak `translation` kapsam. Varsa `from` parametresi belirtilmezse, otomatik dil algılama kaynak dili belirlemek için uygulanır.</td>
  </tr>
  <tr>
    <td>-</td>
    <td>*Gerekli parametre*.<br/>Çıkış metnini dilini belirtir. Hedef Dil olmalıdır [desteklenen diller](./v3-0-languages.md) dahil `translation` kapsam. Örneğin, `to=de` Almanca çevrilemedi.<br/>Sorgu dizesinde parametresini tekrarlayarak birden fazla dili için aynı anda çevirmek mümkündür. Örneğin, `to=de&to=it` Almanca ve İtalyanca çevrilemedi.</td>
  </tr>
  <tr>
    <td>textType</td>
    <td>*İsteğe bağlı parametre*.<br/>Çevrildikten metin düz metin veya HTML metin olup olmadığını tanımlar. Herhangi bir HTML doğru biçimlendirilmeli, tam bir öğe olması gerekir. Olası değerler şunlardır: `plain` (varsayılan) veya `html`.</td>
  </tr>
  <tr>
    <td>category</td>
    <td>*İsteğe bağlı parametre*.<br/>Çeviri kategorisi (etki alanı) belirten bir dize. Bu parametre ile oluşturulan, özelleştirilmiş bir sistemden çevirileri almak için kullanılan [özel Translator](../customization.md). Varsayılan değer: `general`.</td>
  </tr>
  <tr>
    <td>ProfanityAction</td>
    <td>*İsteğe bağlı parametre*.<br/>Profanities çevirileri nasıl değerlendirilmesi gerektiğini belirtir. Olası değerler şunlardır: `NoAction` (varsayılan), `Marked` veya `Deleted`. Küfür değerlendirilecek şekilde anlamak için bkz: [küfür işleme](#handle-profanity).</td>
  </tr>
  <tr>
    <td>ProfanityMarker</td>
    <td>*İsteğe bağlı parametre*.<br/>Çevirileri profanities nasıl işaretlenmelidir belirtir. Olası değerler şunlardır: `Asterisk` (varsayılan) veya `Tag`. Küfür değerlendirilecek şekilde anlamak için bkz: [küfür işleme](#handle-profanity).</td>
  </tr>
  <tr>
    <td>includeAlignment</td>
    <td>*İsteğe bağlı parametre*.<br/>Çevrilmiş metin kaynak metin hizalama yansıtma eklenip eklenmeyeceğini belirtir. Olası değerler şunlardır: `true` veya `false` (varsayılan). </td>
  </tr>
  <tr>
    <td>includeSentenceLength</td>
    <td>*İsteğe bağlı parametre*.<br/>Giriş metni ve çevrilen metni tümce sınırları eklenip eklenmeyeceğini belirtir. Olası değerler şunlardır: `true` veya `false` (varsayılan).</td>
  </tr>
  <tr>
    <td>suggestedFrom</td>
    <td>*İsteğe bağlı parametre*.<br/>Giriş metni dili tanımladıysanız, bir geri dönüş dil belirtir. Otomatik dil algılama uygulandığı zaman `from` parametresi atlanırsa. Algılama başarısız olursa `suggestedFrom` dil kabul edilir.</td>
  </tr>
  <tr>
    <td>fromScript</td>
    <td>*İsteğe bağlı parametre*.<br/>Giriş metninin betiği belirtir.</td>
  </tr>
  <tr>
    <td>toScript</td>
    <td>*İsteğe bağlı parametre*.<br/>Çevrilen metnin betiği belirtir.</td>
  </tr>
  <tr>
    <td>AllowFallback</td>
    <td>*İsteğe bağlı parametre*.<br/>Özel bir sistemde mevcut değil, hizmet genel sistem geri izin verildiğini belirtir. Olası değerler şunlardır: `true` (varsayılan) veya `false`.<br/><br/>`allowFallback=false` Çeviri sistemleri için eğitim yalnızca kullanması gerektiğini belirtir `category` istek tarafından belirtilen. Dil Y X diline yönelik bir çeviri pivot diliyle E, ardından tüm sistemler zincirindeki zincir gerektiriyorsa (X -> E ve E -> Y) özel ve aynı kategoride olması gerekir. Belirli bir kategoriye sahip hiçbir sistemi bulunursa istek bir 400 durum kodu döndürür. `allowFallback=true` Özel bir sistemde mevcut değil, hizmet genel sistem geri izin verildiğini belirtir.
</td>
  </tr>
</table> 

İstek üst bilgileri ekleyin:

<table width="100%">
  <th width="20%">Üst bilgiler</th>
  <th>Açıklama</th>
  <tr>
    <td>_Bir yetkilendirme_<br/>_Üst bilgi_</td>
    <td>*Gerekli istek üst bilgisi*.<br/>Bkz: [kimlik doğrulaması için kullanılabilir seçenekler](./v3-0-reference.md#authentication).</td>
  </tr>
  <tr>
    <td>Content-Type</td>
    <td>*Gerekli istek üst bilgisi*.<br/>Akıştaki içerik türünü belirtir. Olası değerler şunlardır: `application/json`.</td>
  </tr>
  <tr>
    <td>İçerik uzunluğu</td>
    <td>*Gerekli istek üst bilgisi*.<br/>İstek gövdesi uzunluğu.</td>
  </tr>
  <tr>
    <td>X-ClientTraceId</td>
    <td>*İsteğe bağlı*.<br/>İstek benzersiz olarak tanımlanabilmesi için bir istemci tarafından oluşturulan GUID. İzleme kimliği adlı bir sorgu parametresi kullanarak sorgu dizesinde eklerseniz bu başlığı atlayabilirsiniz `ClientTraceId`.</td>
  </tr>
</table> 

## <a name="request-body"></a>İstek gövdesi

İstek gövdesinde bir JSON dizisidir. Her dizi öğesi bir dize özelliği adlı bir JSON nesnesidir `Text`, çeviri dizesini temsil eder.

```json
[
    {"Text":"I would really like to drive your car around the block a few times."}
]
```

Aşağıdaki sınırlamalar geçerlidir:

* Dizi en fazla 25 öğeleri olabilir.
* İstekte bulunan tüm metin alanları dahil olmak üzere 5000 karakterden uzun olamaz.

## <a name="response-body"></a>Yanıt gövdesi

Başarılı bir yanıt için Giriş dizisinin her bir dizede tek bir sonuç ile bir JSON dizisidir. Sonuç nesnesi, aşağıdaki özellikleri içerir:

  * `detectedLanguage`: Aşağıdaki özelliklerle algılanan dilin açıklayan bir nesne:

      * `language`: Algılanan dilin kodunu temsil eden bir dize.

      * `score`: Sonuçta güvenle gösteren bir float değeri. Sıfır ile bir puan arasındadır ve düşük puanı düşük güven gösterir.

    `detectedLanguage` Özelliği varsa yalnızca sonuç nesnesinde otomatik dil algılama istendiğinde.

  * `translations`: Çeviri sonuçları bir dizi. Dizinin boyutu hedef aracılığıyla belirtilen dil sayısı ile eşleşen `to` sorgu parametresi. Dizideki her öğe şunları içerir:

    * `to`: Hedef Dil dil kodunu temsil eden bir dize.

    * `text`: Çevrilmiş metin vererek bir dize.

    * `transliteration`: Bir nesne tarafından belirtilen komut dosyasındaki çevrilmiş metnin vererek `toScript` parametresi.

      * `script`: Hedef betiği belirtme bir dize.   

      * `text`: Çevrilmiş metin hedef betikte vererek bir dize.

    `transliteration` Nesne değilse dahil harf çevirisi gerçekleşmez.

    * `alignment`: Bir nesne adlı tek bir dize özelliği `proj`, hangi haritalar, çevrilmiş metni için metin girişi. Hizalama bilgileri yalnızca zaman sağlanır istek parametresi `includeAlignment` olduğu `true`. Hizalama, aşağıdaki biçimde bir dize değeri olarak döndürülür: `[[SourceTextStartIndex]:[SourceTextEndIndex]–[TgtTextStartIndex]:[TgtTextEndIndex]]`.  İki nokta üst üste başlangıç ayırır ve dilleri uç dizini, dash ayırır ve sözcükler alanı ayırır. Bir sözcük ile sıfır, bir veya birden çok sözcük başka bir dil yeteri kadar ve hizalanmış sözcükleri bitişik olmayan olabilir. Hizalama bilgi kullanılabilir duruma geldiğinde hizalama öğesi boş olur. Bkz: [hizalama bilgi elde](#obtain-alignment-information) örneği ve kısıtlamaları için.

    * `sentLen`: Giriş ve çıkış metinleri cümle sınırları döndürüyor bir nesne.

      * `srcSentLen`: Giriş metni cümleleri uzunluklarının temsil eden bir tamsayı dizisi. Dizinin uzunluğunu cümleler sayısıdır ve her cümle uzunluğunu değerlerdir.

      * `transSentLen`: Çevrilmiş metin cümleleri uzunluklarının temsil eden bir tamsayı dizisi. Dizinin uzunluğunu cümleler sayısıdır ve her cümle uzunluğunu değerlerdir.

    Tümce sınırları, yalnızca zaman dahil edilen istek parametresi `includeSentenceLength` olduğu `true`.

  * `sourceText`: Bir nesne adlı tek bir dize özelliği `text`, kaynak dili varsayılan betikte giriş metni sağlar. `sourceText` özelliği, yalnızca giriş dili için her zamanki betiği değil bir betik ifade edilir olduğunda. Örneğin, giriş ardından Latin kodda yazılmış Arapça oluşturduysanız `sourceText.text` aynı Arapça metni Arap komut dosyasına dönüştürülür.

Örnek JSON yanıtları verilmiştir [örnekler](#examples) bölümü.

## <a name="response-headers"></a>Yanıt üst bilgileri

<table width="100%">
  <th width="20%">Üst bilgiler</th>
  <th>Açıklama</th>
    <tr>
    <td>X-RequestId</td>
    <td>İstek tanımlamak için hizmeti tarafından oluşturulan değeri. Sorun giderme amacıyla kullanılır.</td>
  </tr>
  <tr>
    <td>X-MT-sistem</td>
    <td>Çeviri için istenen her 'to' dil için çeviri için kullanılan sistem türü belirtir. Dizeleri, virgülle ayrılmış listesini değerdir. Her bir dizenin bir tür gösterir:<br/><ul><li>Özel bir sistem özel - istek içerir ve en az bir özel sistemi çevirisi sırasında kullanıldı.</li><li>Takım - tüm diğer isteklerden</li></td>
  </tr>
</table> 

## <a name="response-status-codes"></a>Yanıt durum kodları

Bir isteği döndüren olası HTTP durum kodları şunlardır: 

<table width="100%">
  <th width="20%">Durum Kodu</th>
  <th>Açıklama</th>
  <tr>
    <td>200</td>
    <td>Başarılı.</td>
  </tr>
  <tr>
    <td>400</td>
    <td>Sorgu parametrelerden biri eksik veya geçerli değil. İstek parametreleri yeniden denemeden önce düzeltin.</td>
  </tr>
  <tr>
    <td>401</td>
    <td>İsteğin kimliği doğrulanamadı. Kimlik bilgilerinin belirtilen ve geçerli olup olmadığını denetleyin.</td>
  </tr>
  <tr>
    <td>403</td>
    <td>İstek yetkili değil. Ayrıntıları hata iletisine bakın. Bu, genellikle bir deneme aboneliği ile sağlanan tüm ücretsiz çevirileri kullanılmış olan olduğunu gösterir.</td>
  </tr>
  <tr>
    <td>408</td>
    <td>Bir kaynak eksik olduğu için istek yerine getirilemedi. Ayrıntıları hata iletisine bakın. Özel bir kullanırken `category`, bu da genellikle özel çeviri sistemi henüz isteklere hizmet kullanılabilir olmadığını gösterir. Bekleme süresi (örneğin, 1 dakika) sonra isteği yeniden denenmesi gerekiyor.</td>
  </tr>
  <tr>
    <td>429</td>
    <td>Çağıran, çok fazla istek gönderiyor.</td>
  </tr>
  <tr>
    <td>500</td>
    <td>Beklenmeyen bir hata oluştu. Sorun devam ederse, raporu ile: tarih ve saat hatanın yanıt üst bilgisinden istek tanımlayıcısı `X-RequestId`ve istemci tanımlayıcısı istek üst bilgisinden `X-ClientTraceId`.</td>
  </tr>
  <tr>
    <td>503</td>
    <td>Sunucu geçici olarak kullanılamıyor. İsteği yeniden deneyin. Sorun devam ederse, raporu ile: tarih ve saat hatanın yanıt üst bilgisinden istek tanımlayıcısı `X-RequestId`ve istemci tanımlayıcısı istek üst bilgisinden `X-ClientTraceId`.</td>
  </tr>
</table> 

## <a name="examples"></a>Örnekler

### <a name="translate-a-single-input"></a>Tek bir giriş Çevir

Bu örnek, tek bir cümleden İngilizce için Basitleştirilmiş Çince Çevir gösterilmektedir.

# <a name="curltabcurl"></a>[Curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'Hello, what is your name?'}]"
```

---

Yanıt gövdesi aşağıdaki gibidir:

```
[
    {
        "translations":[
            {"text":"你好, 你叫什么名字？","to":"zh-Hans"}
        ]
    }
]
```

`translations` Tek parça içinde giriş metin çevirisi sağlayan bir öğe dizisi içerir.

### <a name="translate-a-single-input-with-language-auto-detection"></a>Otomatik dil algılama ile tek bir giriş Çevir

Bu örnek, tek bir cümleden İngilizce için Basitleştirilmiş Çince Çevir gösterilmektedir. İstek giriş dili belirtmiyor. Kaynak dili otomatik algılama yerine kullanılır.

# <a name="curltabcurl"></a>[Curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&to=zh-Hans" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'Hello, what is your name?'}]"
```

---

Yanıt gövdesi aşağıdaki gibidir:

```
[
    {
        "detectedLanguage": {"language": "en", "score": 1.0},
        "translations":[
            {"text": "你好, 你叫什么名字？", "to": "zh-Hans"}
        ]
    }
]
```
Yanıt, önceki örnekte yanıt olarak benzerdir. Yanıt, ayrıca otomatik dil algılama istendi olduğundan, giriş metni için algılanan dili hakkında bilgi içerir. 

### <a name="translate-with-transliteration"></a>İle alfabeye çevirme

Şimdi önceki örnekte alfabeye ekleyerek genişletin. Aşağıdaki isteği Latin kodda yazılmış bir Çince ister.

# <a name="curltabcurl"></a>[Curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&to=zh-Hans&toScript=Latn" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'Hello, what is your name?'}]"
```

---

Yanıt gövdesi aşağıdaki gibidir:

```
[
    {
        "detectedLanguage":{"language":"en","score":1.0},
        "translations":[
            {
                "text":"你好, 你叫什么名字？",
                "transliteration":{"script":"Latn", "text":"nǐ hǎo , nǐ jiào shén me míng zì ？"},
                "to":"zh-Hans"
            }
        ]
    }
]
```

Çeviri sonucu artık içeren bir `transliteration` Latin karakterler kullanarak çevrilmiş metin veren özellik.

### <a name="translate-multiple-pieces-of-text"></a>Birden çok parça metin çevirme

Aynı anda birden çok dizeyi çevirme istek gövdesinde bir dizeler dizisi belirtme sorunudur.

# <a name="curltabcurl"></a>[Curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'Hello, what is your name?'}, {'Text':'I am fine, thank you.'}]"
```

---

Yanıt gövdesi aşağıdaki gibidir:

```
[
    {
        "translations":[
            {"text":"你好, 你叫什么名字？","to":"zh-Hans"}
        ]
    },            
    {
        "translations":[
            {"text":"我很好，谢谢你。","to":"zh-Hans"}
        ]
    }
]
```

### <a name="translate-to-multiple-languages"></a>Birden çok dil için çeviri

Bu örnek, bir istek çeşitli dillerde aynı girişi Çevir gösterilmektedir.

# <a name="curltabcurl"></a>[Curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans&to=de" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'Hello, what is your name?'}]"
```

---

Yanıt gövdesi aşağıdaki gibidir:

```
[
    {
        "translations":[
            {"text":"你好, 你叫什么名字？","to":"zh-Hans"},
            {"text":"Hallo, was ist dein Name?","to":"de"}
        ]
    }
]
```

### <a name="handle-profanity"></a>Tanıtıcı küfür

Normalde Translator hizmeti, mevcut çeviri kaynağında küfür korur. Küfür derecesini ve sözcükler Küfürlü yapan bağlam kültürler arasında farklılık gösterir ve hedef dilde küfür derecesini sonucunda yükseltilmiş veya azalır.

Kaynak metin küfür varlığını bakılmaksızın çevirisini küfür girmeyi önlemek istiyorsanız, filtreleme seçeneğini küfür kullanabilirsiniz. Seçenek profanities (kendi sonrası işleme ekleme seçeneği vererek) uygun etiketlerle işaretlemek istediğiniz ya da hiçbir eyleme girişilmedi istediğiniz silinmiş küfür görmek isteyip istemediğinizi seçmenizi sağlar. Kabul edilen değerler, `ProfanityAction` olan `Deleted`, `Marked` ve `NoAction` (varsayılan).

<table width="100%">
  <th width="20%">ProfanityAction</th>
  <th>Eylem</th>
  <tr>
    <td>`NoAction`</td>
    <td>Bu varsayılan davranıştır. Küfür kaynaktan hedefe geçer.<br/><br/>
    **Örnek kaynak (Japonca)**: 彼はジャッカスです。<br/>
    **Örnek çeviri (İngilizce)**: bir jackass He's.
    </td>
  </tr>
  <tr>
    <td>`Deleted`</td>
    <td>Küfürlü sözcükleri değiştirme yapmadan çıktı CİHAZDAN kaldırılır.<br/><br/>
    **Örnek kaynak (Japonca)**: 彼はジャッカスです。<br/>
    **Örnek çeviri (İngilizce)**: He's bir.
    </td>
  </tr>
  <tr>
    <td>`Marked`</td>
    <td>Küfürlü sözcükleri çıktıda bir işaretçi tarafından değiştirilir. İşaretin bağımlı `ProfanityMarker` parametresi.<br/><br/>
İçin `ProfanityMarker=Asterisk`, Küfürlü sözcükleri yerine `***`:<br/>
    **Örnek kaynak (Japonca)**: 彼はジャッカスです。<br/>
    **Örnek çeviri (İngilizce)**: He's bir \* \* \*.<br/><br/>
İçin `ProfanityMarker=Tag`, Küfürlü sözcükleri tarafından XML etiketleri arasına &lt;küfür&gt; ve &lt;/profanity&gt;:<br/>
    **Örnek kaynak (Japonca)**: 彼はジャッカスです。<br/>
    **Örnek çeviri (İngilizce)**: He's bir &lt;küfür&gt;jackass&lt;/profanity&gt;.
  </tr>
</table> 

Örneğin:

# <a name="curltabcurl"></a>[Curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de&profanityAction=Marked" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'This is a fucking good idea.'}]"
```

---

Bu döndürür:

```
[
    {
        "translations":[
            {"text":"Das ist eine *** gute Idee.","to":"de"}
        ]
    }
]
```

Karşılaştırın:

# <a name="curltabcurl"></a>[Curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de&profanityAction=Marked&profanityMarker=Tag" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'This is a fucking good idea.'}]"
```

---

Bu son istek döndürür:

```
[
    {
        "translations":[
            {"text":"Das ist eine <profanity>verdammt</profanity> gute Idee.","to":"de"}
        ]
    }
]
```

### <a name="translate-content-with-markup-and-decide-whats-translated"></a>Biçimlendirme içerikle çevirin ve karar neler çevrilir?

Bir HTML sayfasından içerik biçimlendirmeyi içeren içeriği veya bir XML belgesindeki içeriği çevirmek için yaygındır. Sorgu parametresini `textType=html` etiket ile içerik çevirirken. Ayrıca, bazen çeviri belirli bir içeriğe dışlamak yararlıdır. Öznitelik kullanabileceğiniz `class=notranslate` özgün dilinde kalması gereken içeriği belirtmek için. Aşağıdaki örnekte, ilk içeriğini `div` öğesi değil çevrilir, ikinci içeriği çalışırken `div` öğesi çevrilir.

```
<div class="notranslate">This will not be translated.</div>
<div>This will be translated. </div>
```

Bir örnek istek göstermek için aşağıda verilmiştir.

# <a name="curltabcurl"></a>[Curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans&textType=html" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'<div class=\"notranslate\">This will not be translated.</div><div>This will be translated.</div>'}]"
```

---

Yanıt.:

```
[
    {
        "translations":[
            {"text":"<div class=\"notranslate\">This will not be translated.</div><div>这将被翻译。</div>","to":"zh-Hans"}
        ]
    }
]
```

### <a name="obtain-alignment-information"></a>Hizalama bilgi alın

Hizalama bilgilerini almak için belirtin `includeAlignment=true` üzerinde sorgu dizesi.

# <a name="curltabcurl"></a>[Curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=fr&includeAlignment=true" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'The answer lies in machine translation.'}]"
```

---

Yanıt.:

```
[
    {
        "translations":[
            {
                "text":"La réponse se trouve dans la traduction automatique.",
                "to":"fr",
                "alignment":{"proj":"0:2-0:1 4:9-3:9 11:14-11:19 16:17-21:24 19:25-40:50 27:37-29:38 38:38-51:51"}
            }
        ]
    }
]
```

Hizalama bilgileri ile başlayan `0:2-0:1`, anlamına gelecek şekilde, kaynak metnin ilk üç karakter (`The`) çevrilmiş metnin ilk iki karakter eşlemesi (`La`).

Aşağıdaki kısıtlamalara dikkat edin:

* Hizalama, yalnızca bir alt kümesi dil çiftleri için döndürülür:
  - herhangi bir dili İngilizce;
  - herhangi diğer dili İngilizce, Basitleştirilmiş Çince, Geleneksel Çince ve Letonca-İngilizce dışındaki;
  - Korece-Japonca veya Kore dili için Japonca.
* Tamamlanmış bir çeviri cümlenin ise hizalama almazsınız. Tamamlanmış bir çeviri örneği olan "Bu bir test", "I beğendiğiniz" ve diğer yüksek sıklık düzeyi cümleleri.

### <a name="obtain-sentence-boundaries"></a>Tümce sınırları alın

Kaynak metni ve çevrilen metni tümce uzunluğu hakkında bilgi almak için bu seçeneği belirtin `includeSentenceLength=true` üzerinde sorgu dizesi.

# <a name="curltabcurl"></a>[Curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=fr&includeSentenceLength=true" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'The answer lies in machine translation. The best machine translation technology cannot always provide translations tailored to a site or users like a human. Simply copy and paste a code snippet anywhere.'}]"
```

---

Yanıt.:

```
[
    {
        "translations":[
            {
                "text":"La réponse se trouve dans la traduction automatique. La meilleure technologie de traduction automatique ne peut pas toujours fournir des traductions adaptées à un site ou des utilisateurs comme un être humain. Il suffit de copier et coller un extrait de code n’importe où.",
                "to":"fr",
                "sentLen":{"srcSentLen":[40,117,46],"transSentLen":[53,157,62]}
            }
        ]
    }
]
```

### <a name="translate-with-dynamic-dictionary"></a>Dinamik sözlük ile Çevir

Bir sözcük veya tümcecik uygulamak istediğiniz çeviri zaten biliyorsanız, istek içinde biçimlendirmesi olarak sağlayabilirsiniz. Dinamik sözlük yalnızca uygun ve ürün adları gibi bileşik isimleri için güvenlidir.

Biçimlendirme sağlamak için aşağıdaki sözdizimini kullanır.

``` 
<mstrans:dictionary translation=”translation of phrase”>phrase</mstrans:dictionary>
```

Örneğin, "word wordomatic sözlük girişi var." İngilizce cümlenin düşünün. Word korumak için _wordomatic_ çeviri isteği gönder:

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'The word <mstrans:dictionary translation=\"wordomatic\">word or phrase</mstrans:dictionary> is a dictionary entry.'}]"
```

Sonuç olur:

```
[
    {
        "translations":[
            {"text":"Das Wort "wordomatic" ist ein Wörterbucheintrag.","to":"de"}
        ]
    }
]
```

Bu özellik ile aynı şekilde çalışan `textType=text` veya `textType=html`. Özelliği kullanılmamalıdır. Çeviri özelleştirme uygun ve çok daha iyi özel Translator kullanarak yoludur. Özel Translator bağlamını ve istatistiksel olasılıklar tam olarak kullanmayı sağlar. Varsa veya iş ya da ifade bağlamda gösteren eğitim verilerini oluşturmak bir durumsa, çok daha iyi sonuçlar alırsınız. [Özel Translator hakkında daha fazla bilgi](../customization.md).
 





