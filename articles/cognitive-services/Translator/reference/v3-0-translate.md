---
title: Microsoft Çeviricisi metin API Çevir yöntemi | Microsoft Docs
titleSuffix: Cognitive Services
description: Microsoft Çeviricisi metin API Çevir yöntemini kullanın.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: d8d5e1e2fac747fa733f1d92c08008b7eac2a1bc
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355630"
---
# <a name="text-api-30-translate"></a>Metin API 3.0: Çevir

Metin çevirir.

## <a name="request-url"></a>İstek URL'si

Gönderme bir `POST` isteği:

```HTTP
https://api.cognitive.microsofttranslator.com/translate?api-version=3.0
```

## <a name="request-parameters"></a>İstek parametreleri

Sorgu dizesinde geçirilen isteği Parametreler şunlardır:

<table width="100%">
  <th width="20%">Sorgu parametresi</th>
  <th>Açıklama</th>
  <tr>
    <td>API sürümü</td>
    <td>*Gerekli parametre*.<br/>İstemci tarafından istenilen API sürümü. Değeri olmalıdır `3.0`.</td>
  </tr>
  <tr>
    <td>başlangıç</td>
    <td>*İsteğe bağlı bir parametre*.<br/>Giriş metni dilini belirtir. Hangi dilleri bakarak gelen çevirmek için kullanılabilecek bulma [desteklenen diller](.\v3-0-languages.md) kullanarak `translation` kapsam. Varsa `from` parametresi belirtilmezse, otomatik dil algılama kaynak dili belirlemek için uygulanır.</td>
  </tr>
  <tr>
    <td>-</td>
    <td>*Gerekli parametre*.<br/>Çıktı metin dilini belirtir. Hedef Dil biri olmalıdır [desteklenen diller](.\v3-0-languages.md) dahil `translation` kapsam. Örneğin, `to=de` Almanca çevrilemedi.<br/>Sorgu dizesinde parametresini tekrarlayarak birden fazla dili için aynı anda Çevir mümkündür. Örneğin, `to=de&to=it` Almanca ve İtalyanca çevrilemedi.</td>
  </tr>
  <tr>
    <td>textType</td>
    <td>*İsteğe bağlı bir parametre*.<br/>Çevrildiğini metin düz metin veya HTML metin olup olmadığını tanımlar. Herhangi bir HTML doğru biçimlendirilmiş ve eksiksiz öğesi olması gerekir. Olası değerler şunlardır: `plain` (varsayılan) veya `html`.</td>
  </tr>
  <tr>
    <td>category</td>
    <td>*İsteğe bağlı bir parametre*.<br/>Çeviri kategorisi (etki alanı) belirten bir dize. Bu parametre ile oluşturulan özelleştirilmiş bir sistemden çevirileri almak için kullanılan [özel Çeviricisi](../customization.md). Varsayılan değer: `general`.</td>
  </tr>
  <tr>
    <td>ProfanityAction</td>
    <td>*İsteğe bağlı bir parametre*.<br/>Profanities çevirileri nasıl ele gerektiğini belirtir. Olası değerler şunlardır: `NoAction` (varsayılan), `Marked` veya `Deleted`. Uygunsuz metin kabul yolları anlamak için bkz: [uygunsuz metin işleme](#handle-profanity).</td>
  </tr>
  <tr>
    <td>ProfanityMarker</td>
    <td>*İsteğe bağlı bir parametre*.<br/>Profanities çevirileri nasıl işaretlenmelidir belirtir. Olası değerler şunlardır: `Asterisk` (varsayılan) veya `Tag`. Uygunsuz metin kabul yolları anlamak için bkz: [uygunsuz metin işleme](#handle-profanity).</td>
  </tr>
  <tr>
    <td>includeAlignment</td>
    <td>*İsteğe bağlı bir parametre*.<br/>Kaynak metni hizalama projeksiyondan çevrilmiş metne dahil edilip edilmeyeceğini belirtir. Olası değerler şunlardır: `true` veya `false` (varsayılan). </td>
  </tr>
  <tr>
    <td>includeSentenceLength</td>
    <td>*İsteğe bağlı bir parametre*.<br/>Giriş metni ve çevrilmiş metni için cümle sınırları dahil edilip edilmeyeceğini belirtir. Olası değerler şunlardır: `true` veya `false` (varsayılan).</td>
  </tr>
  <tr>
    <td>suggestedFrom</td>
    <td>*İsteğe bağlı bir parametre*.<br/>Giriş metni dilinin tanımladıysanız bir geri dönüş dili belirtir. Dil otomatik algılamayı uygulandığı zaman `from` parametresi atlanırsa. Algılama başarısız olursa, `suggestedFrom` dil varsayılacak.</td>
  </tr>
  <tr>
    <td>fromScript</td>
    <td>*İsteğe bağlı bir parametre*.<br/>Giriş metni betik belirtir.</td>
  </tr>
  <tr>
    <td>toScript</td>
    <td>*İsteğe bağlı bir parametre*.<br/>Komut dosyasının çevrilmiş metni belirtir.</td>
  </tr>
</table> 

İstek üstbilgilerini şunları içerir:

<table width="100%">
  <th width="20%">Üst bilgiler</th>
  <th>Açıklama</th>
  <tr>
    <td>_Bir yetkilendirme_<br/>_Üstbilgi_</td>
    <td>*Gerekli istek üstbilgisi*.<br/>Bkz: [kimlik doğrulama için kullanılabilir seçenekleri](./v3-0-reference.md#authentication).</td>
  </tr>
  <tr>
    <td>Content-Type</td>
    <td>*Gerekli istek üstbilgisi*.<br/>Yükü içerik türünü belirtir. Olası değerler şunlardır: `application/json`.</td>
  </tr>
  <tr>
    <td>Content-Length</td>
    <td>*Gerekli istek üstbilgisi*.<br/>İstek gövdesini uzunluğu.</td>
  </tr>
  <tr>
    <td>X-ClientTraceId</td>
    <td>*İsteğe bağlı*.<br/>İstek benzersiz şekilde tanımlamak için bir istemci tarafından üretilen GUID. İzleme kimliği adlı bir sorgu parametresini kullanarak sorgu dizesinde eklerseniz, bu başlığı atlayabilirsiniz `ClientTraceId`.</td>
  </tr>
</table> 

## <a name="request-body"></a>İstek gövdesi

İstek gövdesi bir JSON dizisidir. Her dizi öğesi adlı bir dize özelliği ile bir JSON nesnesidir `Text`, çevir dizesini temsil eder.

```json
[
    {"Text":"I would really like to drive your car around the block a few times."}
]
```

Aşağıdaki sınırlamalar uygulanır:

* Dizi en çok 25 öğeler bulunabilir.
* İstekte bulunan tüm metin, boşluklar dahil olmak üzere 5.000 karakterden uzun olamaz.

## <a name="response-body"></a>Yanıt gövdesi

Başarılı yanıt Giriş dizisindeki her dize için bir sonuç içeren bir JSON dizisidir. Sonuç nesnesi aşağıdaki özellikleri içerir:

  * `detectedLanguage`: Aşağıdaki özellikleri aracılığıyla algılanan dil açıklayan bir nesneyi:

      * `language`: Algılanan dil kodunu temsil eden bir dize.

      * `score`: Güvenilirlik sonucundaki gösteren bir float değeri. Sıfır ile bir puan arasındadır ve düşük puanı düşük güvenilirlik gösterir.

    `detectedLanguage` Özelliği yalnızca sonuç nesnesinde mevcut dil otomatik algılamayı istendiğinde.

  * `translations`: Çeviri sonuçlarını bir dizi. Dizinin boyutu hedef aracılığıyla belirtilen dilleri sayısıyla eşleştiğini `to` sorgu parametresi. Dizideki her öğe içerir:

    * `to`: Hedef Dil dil kodu temsil eden bir dize.

    * `text`: Çevrilmiş metni vermiş bir dize.

    * `transliteration`: Belirtilen komut çevrilmiş metni vermiş bir nesne `toScript` parametresi.

      * `script`: Hedef komut dosyasını belirten bir dize.   

      * `text`: Hedef komut dosyasında çevrilmiş metni vermiş bir dize.

    `transliteration` Nesne değilse dahil harf çevirisi gerçekleşmez.

    * `alignment`: Bir nesne adlı tek bir dize özelliği `proj`, hangi eşlemeleri giriş çevrilmiş metni metin. Hizalama bilgileri yalnızca zaman sağlanır istek parametresi `includeAlignment` olan `true`. Hizalama, aşağıdaki biçimde bir dize değeri olarak döndürülür: `[[SourceTextStartIndex]:[SourceTextEndIndex]–[TgtTextStartIndex]:[TgtTextEndIndex]]`.  İki nokta üst üste başlangıç ayırır ve bitiş dizini, tire dilleri ayırır ve sözcükleri alan ayırır. Bir sözcük sıfır, bir veya birden çok sözcük başka bir dilde hizalamak ve hizalanmış sözcükler bitişik olmayan olabilir. Hiçbir hizalama bilgileri kullanılabilir olduğunda hizalama öğesi boş olur. Bkz: [hizalama bilgileri elde](#obtain-alignment-information) örneği ve kısıtlamaları için.

    * `sentLen`: Giriş ve çıkış metinleri cümle sınırları döndürüyor bir nesne.

      * `srcSentLen`: Giriş metni cümlelerde uzunlukları temsil eden tamsayı dizisi. Dizi uzunluğu cümleleri sayısı ve her tümcenin uzunluğu değerlerdir.

      * `transSentLen`: Çevrilmiş metni cümlelerde uzunlukları temsil eden tamsayı dizisi. Dizi uzunluğu cümleleri sayısı ve her tümcenin uzunluğu değerlerdir.

    Tümce sınırları, yalnızca zaman dahil istek parametresi `includeSentenceLength` olan `true`.

  * `sourceText`: Bir nesne adlı tek bir dize özelliği `text`, kaynak dili varsayılan komut dosyası giriş metni sağlar. `sourceText` özelliği, yalnızca giriş normal komut dosyası dili için değil bir komut dosyası cinsinden olduğunda. Örneğin, giriş Latin komut dosyasında, ardından yazılan Arapça olsaydı `sourceText.text` aynı Arapça metin Arap komut dosyasına dönüştürülür.

JSON yanıtları örneği verilmiştir [örnekler](#examples) bölümü.

## <a name="response-status-codes"></a>Yanıtı durum kodları

Bir istek döndürür olası HTTP durum kodları şunlardır: 

<table width="100%">
  <th width="20%">Durum Kodu</th>
  <th>Açıklama</th>
  <tr>
    <td>200</td>
    <td>Başarılı.</td>
  </tr>
  <tr>
    <td>400</td>
    <td>Sorgu parametrelerden biri eksik veya geçerli değil. İstek parametreleri denemeden önce düzeltin.</td>
  </tr>
  <tr>
    <td>401</td>
    <td>İsteğin kimliği doğrulanamadı. Kimlik bilgilerinin belirtilen ve geçerli olduğunu denetleyin.</td>
  </tr>
  <tr>
    <td>403</td>
    <td>İstek yetkili değil. Ayrıntılar hata iletisine bakın. Bu, genellikle bir deneme aboneliği ile sağlanan tüm boş çevirileri kullanılmış olduğunu gösterir.</td>
  </tr>
  <tr>
    <td>429</td>
    <td>Çağrıyı yapan çok sayıda istek gönderiyor.</td>
  </tr>
  <tr>
    <td>500</td>
    <td>Beklenmeyen bir hata oluştu. Sorun devam ederse, kendisiyle Rapor: tarih ve saat hata, istek tanımlayıcısı yanıt başlığından `X-RequestId`ve istemci tanımlayıcısı istek üstbilgisinden `X-ClientTraceId`.</td>
  </tr>
  <tr>
    <td>503</td>
    <td>Sunucu geçici olarak kullanılamıyor. İsteği yeniden deneyin. Sorun devam ederse, kendisiyle Rapor: tarih ve saat hata, istek tanımlayıcısı yanıt başlığından `X-RequestId`ve istemci tanımlayıcısı istek üstbilgisinden `X-ClientTraceId`.</td>
  </tr>
</table> 

## <a name="examples"></a>Örnekler

### <a name="translate-a-single-input"></a>Tek bir giriş Çevir

Bu örnek, Basitleştirilmiş Çince için tek bir cümle İngilizce'den Çevir gösterilmektedir.

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

`translations` Dizi giriş metnin tek parça çevrilmesi sağlayan bir öğe içeriyor.

### <a name="translate-a-single-input-with-language-auto-detection"></a>Dil otomatik algılamayı ile tek bir giriş Çevir

Bu örnek, Basitleştirilmiş Çince için tek bir cümle İngilizce'den Çevir gösterilmektedir. İstek giriş dili belirtmiyor. Otomatik algılamayı kaynak dil yerine kullanılır.

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
Yanıt, önceki örnekten yanıta benzer. Dil otomatik algılamayı istendi olduğundan, yanıt ayrıca giriş metni için algılanan dili hakkında bilgiler içerir. 

### <a name="translate-with-transliteration"></a>Harf çevirisi ile Çevir

Şimdi önceki örnekte, harf çevirisi ekleyerek genişletebilir. Aşağıdaki isteği Latin komut dosyasında yazılan Çince ister.

# <a name="curltabcurl"></a>[Curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&to=zh-Latn" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'Hello, what is your name?'}]"
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
                "transliteration":{"text":"nǐ hǎo , nǐ jiào shén me míng zì ？","script":"Latn"},
                "to":"zh-Hans"
            }
        ]
    }
]
```

Çeviri sonuç artık içeren bir `transliteration` Latin karakterler kullanarak çevrilmiş metin verir özelliği.

### <a name="translate-multiple-pieces-of-text"></a>Birden çok parça metin Çevir

Aynı anda birden çok dizenin çevirme istek gövdesinde bir dizeler dizisi belirtmenin sorunudur.

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

### <a name="translate-to-multiple-languages"></a>Birden çok dil için çevir

Bu örnek, bir istek çeşitli dillerde aynı giriş Çevir gösterilmektedir.

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

### <a name="handle-profanity"></a>Uygunsuz metin işleme

Normalde Çeviricisi hizmet mevcut çeviri kaynağında uygunsuz metin korur. Uygunsuz metin derecesini ve sözcükleri saygısız içerikli yapar bağlam kültürler arasında farklı ve sonuç olarak hedef dilde uygunsuz metin derecesini yükseltilmiş azaltılmış veya olabilir.

Kaynak metin uygunsuz metin varlığını bakılmaksızın çevirisini uygunsuz metin alma önlemek istiyorsanız seçeneği filtreleme uygunsuz metin kullanabilirsiniz. Seçenek uygunsuz metin silindi, profanities (sonrası işleminizde ekleme seçeneğini vermiş) uygun etiketleriyle işaretlemek istiyorsanız veya eylem olup olmadığını görmek isteyip istemediğinizi seçmenizi sağlar. Kabul edilen değerlerini `ProfanityAction` olan `Deleted`, `Marked` ve `NoAction` (varsayılan).

<table width="100%">
  <th width="20%">ProfanityAction</th>
  <th>Eylem</th>
  <tr>
    <td>`NoAction`</td>
    <td>Bu varsayılan davranıştır. Uygunsuz metin kaynaktan hedefe geçer.<br/><br/>
    **Örnek kaynak (Japonca)**: 彼はジャッカスです。<br/>
    **Örnek çeviri (İngilizce)**: kendisine bir jackass değil.
    </td>
  </tr>
  <tr>
    <td>`Deleted`</td>
    <td>Saygısız içerikli sözcükleri değiştirme yapmadan çıktısından kaldırılır.<br/><br/>
    **Örnek kaynak (Japonca)**: 彼はジャッカスです。<br/>
    **Örnek çeviri (İngilizce)**: gerçekleştirilmesine bir.
    </td>
  </tr>
  <tr>
    <td>`Marked`</td>
    <td>Saygısız içerikli sözcükler çıktıda bir işaret değiştirilir. İşaretin bağlıdır `ProfanityMarker` parametresi.<br/><br/>
İçin `ProfanityMarker=Asterisk`, saygısız içerikli sözcükler değiştirildiğini `***`:<br/>
    **Örnek kaynak (Japonca)**: 彼はジャッカスです。<br/>
    **Örnek çeviri (İngilizce)**: gerçekleştirilmesine bir \* \* \*.<br/><br/>
İçin `ProfanityMarker=Tag`, saygısız içerikli sözcükler tarafından XML etiketleri arasına &lt;uygunsuz metin&gt; ve &lt;/profanity&gt;:<br/>
    **Örnek kaynak (Japonca)**: 彼はジャッカスです。<br/>
    **Örnek çeviri (İngilizce)**: gerçekleştirilmesine bir &lt;uygunsuz metin&gt;jackass&lt;/profanity&gt;.
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

Karşılaştırılacak:

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

### <a name="translate-content-with-markup-and-decide-whats-translated"></a>Biçimlendirme içerikle çevirin ve ne çevrilir karar verin

Bir HTML sayfasından içeriği gibi biçimlendirme içeren içerik veya bir XML belgesi içerikten çevirmek için yaygın bir durumdur. Sorgu parametresi dahil `textType=html` etiketleri içerikle çevirirken. Ayrıca, bazen belirli içerik çevrilmesi dışlamak yararlıdır. Öznitelik kullanabilirsiniz `class=notranslate` özgün dilinde kalacağı içeriği belirtmek için. Aşağıdaki örnekte, ilk içeriğini `div` öğesi değil çevrilir, ikinci içerik sırasında `div` öğesi çevrilir.

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

Yanıt şöyledir:

```
[
    {
        "translations":[
            {"text":"<div class=\"notranslate\">This will not be translated.</div><div>这将被翻译。</div>","to":"zh-Hans"}
        ]
    }
]
```

### <a name="obtain-alignment-information"></a>Hizalama bilgi edinin

Hizalama bilgilerini almak için belirtin `includeAlignment=true` sorgu dizesinde.

# <a name="curltabcurl"></a>[Curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=fr&includeAlignment=true" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'The answer lies in machine translation.'}]"
```

---

Yanıt şöyledir:

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

Hizalama bilgileri ile başlayan `0:2-0:1`, anlamına kaynak metindeki ilk üç karakter (`The`) çevrilmiş metindeki ilk iki karakter eşlem (`La`).

Aşağıdaki kısıtlamalar dikkat edin:

* Hizalama yalnızca bir alt kümesi dil çiftleri için döndürülen:
  - başka bir dil için İngilizce;
  - herhangi diğer bir dilden-Basitleştirilmiş Çince, Geleneksel Çince ve Letonca-İngilizce dışında İngilizce;
  - Kore dili için Japonca veya Kore dili için Japonca.
* Tümce tamamlanmış bir çeviri ise hizalama almazsınız. Tamamlanmış bir çeviri örneğidir "Bu olan bir test", "I sevdiğiniz" ve diğer yüksek yoğunlukta cümleleri.

### <a name="obtain-sentence-boundaries"></a>Tümce sınırları alın

Kaynak metni ve çevrilmiş metni cümle uzunluğu hakkında bilgi almak için belirtmek `includeSentenceLength=true` sorgu dizesinde.

# <a name="curltabcurl"></a>[Curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=fr&includeSentenceLength=true" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'The answer lies in machine translation. The best machine translation technology cannot always provide translations tailored to a site or users like a human. Simply copy and paste a code snippet anywhere.'}]"
```

---

Yanıt şöyledir:

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

Bir sözcük veya tümcecik uygulamak istediğiniz çeviri zaten biliyorsanız, istek içinde biçimlendirmesi olarak sağlayabilir. Dinamik sözlük yalnızca uygun ve ürün adları gibi bileşik isimleri için güvenlidir.

Biçimlendirme sağlamak için aşağıdaki sözdizimini kullanır.

``` 
<mstrans:dictionary translation=”translation of phrase”>phrase</mstrans:dictionary>
```

Örneğin, İngilizce cümle "word wordomatic sözlük girdisi var." göz önünde bulundurun Word korumak için _wordomatic_ çevirisi'nde isteği gönder:

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'The word <mstrans:dictionary translation=\"wordomatic\">word or phrase</mstrans:dictionary> is a dictionary entry.'}]"
```

Sonuç verilmiştir:

```
[
    {
        "translations":[
            {"text":"Das Wort "wordomatic" ist ein Wörterbucheintrag.","to":"de"}
        ]
    }
]
```

Bu özellik ile aynı şekilde çalışır `textType=text` veya ile `textType=html`. Özellik kullanılmamalıdır. Çeviri özelleştirme en uygun ve çok daha iyi yolu, özel Çeviricisi kullanmaktır. Özel Çeviricisi bağlamını ve istatistiksel olasılıklar tam kullanımını sağlar. Varsa veya iş ya da tümcecik bağlamda gösterir eğitim verileri oluşturmak destekleyebilir kadar daha iyi sonuçlar elde. [Özel Çeviricisi hakkında daha fazla bilgi](../customization.md).
 





