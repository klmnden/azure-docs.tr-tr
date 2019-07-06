---
title: Konuşma sentezi biçimlendirme dili (SSML'yi) - konuşma Hizmetleri
titleSuffix: Azure Cognitive Services
description: Söyleniş ve metin okuma, prosody denetlemek için konuşma sentezi biçimlendirme dili kullanma.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 8285a76f8cd07863874f9c8e8eebe96f1cb968dd
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67604811"
---
# <a name="speech-synthesis-markup-language-ssml"></a>Konuşma Sentezi Biçimlendirme Dili (SSML)

Konuşma sentezi işaretleme dili (SSML'yi), geliştiricilerin nasıl Giriş bir metin belirtmenize olanak tanıyan bir XML-tabanlı işaretleme dili Sentezlenen konuşmaya metin okuma hizmeti kullanılarak dönüştürülür ' dir. Düz metne kıyasla oranı, birim ve diğer metin okuma çıkış Konuşmayı SSML'yi geliştiricilerin aralık, Söyleniş, ince ayar olanak tanır. Bir süre sonra duraklatma veya bir soru işareti ile bir cümle sona erdiğinde doğru tonlama kullanma gibi normal noktalama, otomatik olarak işlenir.

SSML'yi konuşma Hizmetleri uygulamasını World Wide Web Consortium üzerinde kişinin alan [konuşma sentezi işaretleme dili sürüm 1.0](https://www.w3.org/TR/speech-synthesis).

> [!IMPORTANT]
> Çince, Japonca ve Korece karakterler, faturalandırma için iki karakter olarak sayılır. Daha fazla bilgi için [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

## <a name="standard-neural-and-custom-voices"></a>Standart, sinir ve özel ses

Standart ve sinir ses arasından seçin veya kendi özel sesli benzersiz ürün veya marka oluşturun. 75'ten fazla standart sesleri 45'den fazla dil ve yerel ayar kullanılabilir ve 5 sinir sesleri 4 dil ve yerel ayarlar kullanılabilir. Desteklenen diller, yerel ayarlar ve sesler (sinir ve standart) tam listesi için bkz: [dil desteği](language-support.md).

Sinir, standart ve özel seslerle hakkında daha fazla bilgi için bkz: [metin okuma genel bakış](text-to-speech.md).

## <a name="supported-ssml-elements"></a>Desteklenen SSML'yi öğeleri

Her SSML'yi belge SSML'yi öğeleri (veya etiketleri) ile oluşturulur. Bu öğeleri aralık, prosody, birim ve ayarlamak için kullanılır. Aşağıdaki bölümlerde her öğe nasıl kullanıldığını ve gerekli veya isteğe bağlı bir öğe olduğunda ayrıntılı olarak açıklanmaktadır.  

> [!IMPORTANT]
> Öznitelik değerleri çift tırnak kullanmayı unutmayın. Standartlar için doğru biçimlendirilmeli, geçerli XML öznitelik değerleri çift tırnak içine alınmalıdır gerektirir. Örneğin, `<prosody volume="90">` doğru biçimlendirilmeli, geçerli öğe ancak `<prosody volume=90>` değil. SSML'yi tırnak içinde olmayan bir öznitelik değerleri algılamayabilir.

## <a name="create-an-ssml-document"></a>SSML'yi belge oluşturma

`speak` kök öğe olduğundan ve **gerekli** tüm SSML'yi belgeler. `speak` Öğesi sürüm, dil ve biçimlendirme sözlük tanımı gibi önemli bilgiler içerir.

**Söz dizimi**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="string"></speak>
```

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli / isteğe bağlı |
|-----------|-------------|---------------------|
| version | Belge biçimlendirme yorumlamak için kullanılan SSML'yi belirtiminin sürümünü gösterir. 1\.0 geçerli sürümüdür. | Gerekli |
| xml:lang | Kök belge dilini belirtir. Değer bir küçük harf, iki harfli dil kodunu içerebilir (örneğin, **tr**), veya dil kodu ve büyük harfli ülke/bölge (örneğin, **en-US**). | Gerekli |
| xmlns | SSML'yi belge biçimlendirme sözlük (öğe türleri ve öznitelik adları) tanımlayan belge URI'sini belirtir. Geçerli bir URI https://www.w3.org/2001/10/synthesis. | Gerekli |

## <a name="choose-a-voice-for-text-to-speech"></a>Sesli metin okuma için seçin

`voice` Öğesi gereklidir. Metin okuma için kullanılan ses belirtmek için kullanılır.

**Söz dizimi**

```xml
<voice name="string">
    This text will get converted into synthesized speech.
</voice>
```

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli / isteğe bağlı |
|-----------|-------------|---------------------|
| name | Metin okuma çıktısı için kullanılacak ses tanımlar. Desteklenen sesleri tam bir listesi için bkz. [dil desteği](language-support.md#text-to-speech). | Gerekli |

**Örnek**

> [!NOTE]
> Bu örnekte `en-US-Jessa24kRUS` ses. Desteklenen sesleri tam bir listesi için bkz. [dil desteği](language-support.md#text-to-speech).

```XML
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        This is the text that is spoken.
    </voice>
</speak>
```

## <a name="use-multiple-voices"></a>Birden çok kişilerden daha fazlasını kullanın

İçinde `speak` öğesi, birden çok seslerle metin okuma çıktı belirtebilirsiniz. Bu ses farklı dillerde olabilir. Her ses için metin içinde alınmalıdır bir `voice` öğesi.

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli / isteğe bağlı |
|-----------|-------------|---------------------|
| name | Metin okuma çıktısı için kullanılacak ses tanımlar. Desteklenen sesleri tam bir listesi için bkz. [dil desteği](language-support.md#text-to-speech). | Gerekli |

**Örnek**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        Good morning!
    </voice>
    <voice  name="en-US-Guy24kRUS">
        Good morning to you too Jessa!
    </voice>
</speak>
```

## <a name="adjust-speaking-styles"></a>Konuşma stillerini ayarlama

> [!IMPORTANT]
> Bu özellik yalnızca sinir sesleri ile çalışır.

Varsayılan olarak, metin okuma hizmeti için hem standart hem de sinir sesleri nötr bir konuşma tarzı kullanarak metin sentezler. Sinir kişilerden daha fazlasını, konuşma stilini cheerfulness, empati ve yaklaşım ile express ayarlayabilirsiniz `<mstts:express-as>` öğesi. Bu isteğe bağlı Azure konuşma Hizmetleri için benzersiz bir öğedir.

Şu anda, konuşma stil ayarlamalar sinir bu ses için desteklenir:
* `en-US-JessaNeural`
* `zh-CN-XiaoxiaoNeural`

Değişiklikler cümle düzeyinde uygulanır ve stil sesli değişir. Bir stil desteklenmiyorsa, hizmet konuşma varsayılan stili Konuşmayı nötr döndürür.

**Söz dizimi**

```xml
<mstts:express-as type="string"></mstts:express-as>
```

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli / isteğe bağlı |
|-----------|-------------|---------------------|
| türü | Konuşma tarzı belirtir. Şu anda, konuşma ses belirli stillerdir. | Sinir ses konuşma stilini ayarlama gereklidir. Kullanıyorsanız `mstts:express-as`, ardından türü sağlanmalıdır. Geçersiz bir değer sağlanmazsa, bu öğe yoksayılır. |

Hangi konuşma stilleri sinir her ses için desteklendiğini belirlemek için bu tabloyu kullanın.

| Ses | Tür | Açıklama |
|-------|------|-------------|
| `en-US-JessaNeural` | type=`cheerful` | Pozitif ve daha mutlu bir duygu tanıma ifade eder. |
| | type=`empathy` | İfade bir fikir caring ve anlama |
| `zh-CN-XiaoxiaoNeural` | type=`newscast` | Ton resmi, haber yayınları benzer ifade eder. |
| | type=`sentiment` | Bitişik bir ileti veya bir hikaye iletmez. |

**Örnek**

Bu SSML'yi parçacığı göstermektedir nasıl `<mstts:express-as>` konuşma stiline değiştirmek için kullanılan öğe `cheerful`.

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xmlns:mstts="https://www.w3.org/2001/mstts" xml:lang="en-US">
    <voice name="en-US-JessaNeural">
        <mstts:express-as type="cheerful">
            That'd be just amazing!
        </mstts:express-as>
    </voice>
</speak>
```

## <a name="add-or-remove-a-breakpause"></a>Ekleme veya kaldırma kesme/Duraklat

Kullanım `break` duraklamaları (veya sonları) kelimeler arasındaki eklemek veya metin okuma hizmet tarafından otomatik olarak eklenen duraklamaları önlemek için öğesi.

> [!NOTE]
> Bu öğe bu sözcük veya tümcecik Sentezlenen konuşma istemeyiz düşünüyorsanız metin okuma (TTS) varsayılan davranışını bir sözcük veya tümcecik geçersiz kılmak için kullanın. Ayarlama `strength` için `none` tex'i konuşma hizmeti tarafından otomatik olarak eklenen bir bürünsel sonu önlemek için.

**Söz dizimi**

```xml
<break strength="string" />
<break time="string" />
```

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli / isteğe bağlı |
|-----------|-------------|---------------------|
| gücü | Aşağıdaki değerlerden birini kullanarak bir duraklama göreli süre belirtir:<ul><li>Yok</li><li>x-zayıf</li><li>zayıf</li><li>Orta (varsayılan)</li><li>Tanımlayıcı</li><li>x tanımlayıcı</li></ul> | İsteğe bağlı |
| time | Bir duraklatma saniye ve milisaniye içinde mutlak süresini belirtir. Geçerli değerler 2s ve 500 örnekler | İsteğe bağlı |

| Strength | Açıklama |
|----------|-------------|
| Yok, ya da değer sağlanmadı | 0 ms |
| x-zayıf | 250 ms |
| zayıf | 500 ms |
| Orta | 750 ms |
| Tanımlayıcı | 1000 ms |
| x tanımlayıcı | 1250 ms |


**Örnek**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        Welcome to Microsoft Cognitive Services <break time="100ms" /> Text-to-Speech API.
    </voice>
</speak>
```

## <a name="specify-paragraphs-and-sentences"></a>Paragrafları ve cümlelerden belirtin

`p` ve `s` öğeleri paragraflar ve cümle, sırasıyla belirtmek için kullanılır. Bu öğeleri olmaması durumunda, metin okuma hizmet SSML'yi belge yapısı otomatik olarak belirler.

`p` Öğe, metin ve aşağıdaki öğeleri içerebilir: `audio`, `break`, `phoneme`, `prosody`, `say-as`, `sub`, `mstts:express-as`, ve `s`.

`s` Öğe, metin ve aşağıdaki öğeleri içerebilir: `audio`, `break`, `phoneme`, `prosody`, `say-as`, `mstts:express-as`, ve `sub`.

**Söz dizimi**

```XML
<p></p>
<s></s>
```

**Örnek**

```XML
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <p>
            <s>Introducing the sentence element.</s>
            <s>Used to mark individual sentences.</s>
        </p>
        <p>
            Another simple paragraph.
            Sentence structure in this paragraph is not explicitly marked.
        </p>
    </voice>
</speak>
```

## <a name="use-phonemes-to-improve-pronunciation"></a>Söyleniş geliştirmek için Fonem kullanın

`ph` Öğesi için kullanılan SSML'yi belgelerde fonetik söylenişi için. `ph` Öğe yalnızca metin, diğer bir öğeler içerebilir. Her zaman bir geri dönüş olarak okunabilir konuşma sağlayın.

Harf, sayı veya karakter birleşimi durumlarda yapılan telefonlar, fonetik harfler oluşur. Her telefon benzersiz bir konuşma sesine açıklar. Herhangi bir harf birden çok konuşulan sesleri yeri temsil edebilir Latin alfabesi aksine budur. ' % S'harfi "c" sözcükler "candy" ve "sonlandırma" farklı Söyleniş veya harf kombinasyonu farklı Söyleniş "th" sözcükler "şey" ve "Bu" göz önünde bulundurun.

**Söz dizimi**

```XML
<phoneme alphabet="string" ph="string"></phoneme>
```

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli / isteğe bağlı |
|-----------|-------------|---------------------|
| alfabetik | Belirtir söylenişi dizenin synthesizing kullanılacak fonetik alfabetik `ph` özniteliği. Alfabetik belirten dizeyi küçük harflerle belirtilmesi gerekir. Belirttiğiniz olası harfler aşağıda verilmiştir.<ul><li>IPA &ndash; uluslararası fonetik alfabetik</li><li>SAPI &ndash; konuşma tanıma API'si telefon ayarlayın.</li><li>UPS &ndash; Evrensel telefon ayarlayın</li></ul>Alfabetik yalnızca phoneme öğesinde uygulanır. Daha fazla bilgi için [fonetik alfabetik başvuru](https://msdn.microsoft.com/library/hh362879(v=office.14).aspx). | İsteğe bağlı |
| ph | Word'de Söyleniş belirtin telefonlar içeren bir dize `phoneme` öğesi. Belirtilen dizeyi tanınmayan telefonlar içeriyorsa, metin okuma (TTS) hizmeti SSML'yi belgenin tamamını reddeder ve belgede belirtilen konuşma çıkışındaki hiçbiri üretir. | Fonem kullanıyorsanız gereklidir. |

**Örnekler**

```XML
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <s>His name is Mike <phoneme alphabet="ups" ph="JH AU"> Zhou </phoneme></s>
    </voice>
</speak>
```

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <phoneme alphabet="ipa" ph="t&#x259;mei&#x325;&#x27E;ou&#x325;"> tomato </phoneme>
    </voice>
</speak>
```

## <a name="adjust-prosody"></a>Prosody Ayarla

`prosody` Öğe aralığı, countour, aralığı, oranı, süresi ve metin okuma çıkışı için birim değişiklikleri belirtmek için kullanılır. `prosody` Öğe, metin ve aşağıdaki öğeleri içerebilir: `audio`, `break`, `p`, `phoneme`, `prosody`, `say-as`, `sub`, ve `s`.

Konuşma tanıyıcının, bürünsel öznitelik değerleri üzerinde birçok farklılık gösterebileceğinden, atanan değerleri seçili sesin gerçek bürünsel değerlerin neler olması gerektiğini öneri olarak yorumlar. Metin okuma hizmet sınırları veya desteklenmeyen değerlerini değiştirir. 1 MHz aralığını ya da bir birim 120 desteklenmeyen değerler örnekleridir.

**Söz dizimi**

```XML
<prosody pitch="value" contour="value" range="value" rate="value" duration="value" volume="value"></prosody>
```

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli / isteğe bağlı |
|-----------|-------------|---------------------|
| Aralık | Metin için taban çizgisi aralığı gösterir. Aralıklı olarak express:<ul><li>Mutlak bir "Hz" (Hertz) tarafından izlenen bir sayı olarak ifade edilen değer. Örneğin, 600Hz.</li><li>Önünde bir sayı olarak ifade edilen göreli değer, "+" veya "-" ve "Hz" veya "st" tarafından izlenen bir miktar belirten aralığı değiştirmek için. Örneğin: +80 Hz veya - 2st. "st" olduğu bir sesi (yarı adım) standart diatonic ölçekte yarısını semitone, değişiklik birimidir gösterir.</li><li>Sabit değer:<ul><li>x-düşük</li><li>Düşük</li><li>Orta</li><li>Yüksek</li><li>x-yüksek</li><li>default</li></ul></li></ul>. | İsteğe bağlı |
| dağılımı | Dağılımı sinir ses için desteklenmiyor. Dağılımı değişiklikleri de aralık konuşma içerik için bir dizi hedef konuşma çıkışındaki belirtilen zaman konumlarda olarak temsil eder. Her hedef parametre çiftleri kümesi tanımlanır. Örneğin: <br/><br/>`<prosody contour="(0%,+20Hz) (10%,-2st) (40%,+10Hz)">`<br/><br/>Her parametre kümesi içindeki ilk değer sıklık değişimi konumunu metin süresi yüzdesi olarak belirtir. İkinci değer artırmak veya azaltmak aralık için göreli bir değer veya bir sabit listesi değeri kullanarak bir aralık için belirtir (bkz `pitch`). | İsteğe bağlı |
| Aralığı  | Aralık için metin aralığını temsil eden bir değer. Express `range` açıklamak için kullanılan aynı mutlak değerler, göreli değerler ya da sabit listesi değerlerini kullanarak `pitch`. | İsteğe bağlı |
| Oranı  | Metni konuşma hızını gösterir. Express `rate` olarak:<ul><li>Bir varsayılan çarpanı olarak davranan bir sayı olarak ifade edilen bir göreli değer. Örneğin, bir değeri *1* oranı değişiklik sonuçlanıyor. Değerini *.5* sonuçları oranını halving içinde. Değerini *3* sonuçları oranını tripling içinde.</li><li>Sabit değer:<ul><li>x-slow</li><li>Yavaş</li><li>Orta</li><li>Hızlı</li><li>x-hızlı</li><li>default</li></ul></li></ul> | İsteğe bağlı |
| duration  | Konuşma geçmesi gereken süreyi saniye ve milisaniye içinde metin sentezi (TTS) hizmeti okur. Örneğin, *2s* veya *1800ms*. | İsteğe bağlı |
| birim  | Konuşma sesinin birim düzeyini gösterir. Toplu olarak express:<ul><li>Gelen sayısı için 0,0 100.0 olarak aralığında ifade bir mutlak değeri *quietest* için *gürültülü*. Örneğin, 75. 100.0 varsayılandır.</li><li>Önünde bir sayı olarak ifade edilen göreli değer, "+" veya "-" birim değiştirmek için bir miktar belirtir. Örneğin + 10 veya-5.5.</li><li>Sabit değer:<ul><li>Sessiz</li><li>x-soft</li><li>yazılım</li><li>Orta</li><li>yüksek</li><li>x-loud</li><li>default</li></ul></li></ul> | İsteğe bağlı |

### <a name="change-speaking-rate"></a>Konuşma hızını değiştirme

Word veya cümle düzeyinde standart sesleri oranı Konuşmayı uygulanabilir. Konuşma hızını yalnızca sinir sesleri cümle düzeyinde uygulanabilir ise.

**Örnek**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Guy24kRUS">
        <prosody rate="+30.00%">
            Welcome to Microsoft Cognitive Services Text-to-Speech API.
        </prosody>
    </voice>
</speak>
```

### <a name="change-volume"></a>Birimi Değiştir

Birim değişiklikleri standart sesleri word veya cümle düzeyinde uygulanabilir. Birim değişiklikleri yalnızca sinir sesleri cümle düzeyinde uygulanabilir ise.

**Örnek**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <prosody volume="+20.00%">
            Welcome to Microsoft Cognitive Services Text-to-Speech API.
        </prosody>
    </voice>
</speak>
```

### <a name="change-pitch"></a>Aralık değiştirme

Aralık değişiklikleri standart sesleri word veya cümle düzeyinde uygulanabilir. Aralık değişiklikler yalnızca sinir sesleri cümle düzeyinde uygulanabilir ise.

**Örnek**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Guy24kRUS">
        Welcome to <prosody pitch="high">Microsoft Cognitive Services Text-to-Speech API.</prosody>
    </voice>
</speak>
```

### <a name="change-pitch-contour"></a>Değişiklik aralık dağılımı

> [!IMPORTANT]
> Aralık contour değişiklikleri sinir sesleri ile desteklenmez.

**Örnek**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <prosody contour="(80%,+20%) (90%,+30%)" >
            Good morning.
        </prosody>
    </voice>
</speak>
```

## <a name="next-steps"></a>Sonraki adımlar

* [Dil desteği: sesleri, yerel ayarlar, dilleri](language-support.md)
