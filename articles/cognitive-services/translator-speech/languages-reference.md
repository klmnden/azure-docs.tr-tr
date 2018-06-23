---
title: Microsoft Çeviricisi konuşma API dilleri yöntemi | Microsoft Docs
titleSuffix: Cognitive Services
description: Microsoft Çeviricisi konuşma API diller yöntemini kullanın.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 05/18/18
ms.author: v-jansko
ms.openlocfilehash: 5396e3be17345c3c36197a9b6cbace86e1f574c1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355702"
---
# <a name="speech-api-languages"></a>Konuşma API'SİNDE: dilleri

Microsoft Translator sürekli olarak kendi Hizmetleri tarafından desteklenen dillerin listesini genişletir. Konuşma çeviri hizmeti ile kullanmak için şu anda kullanılabilir dil kümesini bulmak için bu API'yi kullanın.

Kullanılabilir diller almak için API kullanımını gösteren kod örnekleri kullanılabilir [Microsoft Çeviricisi Github site](https://github.com/MicrosoftTranslator).

## <a name="implementation-notes"></a>Uygulama Notları

/Languages Al 

Geniş bir dil kümesini okuma, transcribed metni çevirin ve çeviri birleştirilen konuşma üretmek için transcribe kullanılabilir.

Bir istemcinin kullandığı `scope` sorgu parametresi olarak ilgileniyor dillerin hangi kümelerini tanımlayın.

* **Konuşma-metin:** sorgu parametresi kullandığınızda `scope=speech` metne konuşma transcribe kullanılabilir diller kümesi alınamadı.
* **Metin çeviri:** sorgu parametresi kullandığınızda `scope=text` dilleri transcribed metin çevirmek için kullanılabilecek bir dizi alınamadı.
* **Metin okuma:** sorgu parametresi kullandığınızda `scope=tts` diller ve seslerini Konuşma'ya geri çevrilen metin sentezlemek kullanılabilir kümesi alınamadı.

Bir istemci birden çok kümesi tercih virgülle ayrılmış bir listesi belirterek aynı anda alabilir. Örneğin, `scope=speech,text,tts`.

Başarılı yanıt her kümesi istenen bir özelliği olan bir JSON nesnesi aranır.

```
{
    "speech": {
        //... Set of languages supported for speech-to-text
    },
    "text": {
        //... Set of languages supported for text translation
    },
    "tts": {
        //... Set of languages supported for text-to-speech
    }
}
```

Bir istemcinin kullanabileceği beri `scope` desteklenen dillerin hangi kümelerini seçmek için sorgu parametresi döndürülmesi, gerçek bir yanıt yalnızca yukarıda gösterilen tüm özelliklerinin bir alt içerebilir.

Her bir özellik ile sağlanan değer aşağıdaki gibidir.

### <a name="speech-to-text-speech"></a>Konuşma metin (konuşma)

Konuşma metin özelliğiyle ilişkili değer `speech`, sözlük (anahtar, değer) çiftleri. Her anahtar konuşma metin için desteklenen bir dil tanımlar. Anahtar tanımlayıcısıdır API için istemci geçirir. Anahtarıyla ilişkilendirilmiş değeri, aşağıdaki özelliklere sahip bir nesnedir:

* `name`: Dil görünen adı.
* `language`: Dil yazılmış ilişkilendirilen dil etiketi. Aşağıda "Metin işlem" konusuna bakın.
Bir örnek verilmiştir:

```
{
    "speech": {
        "en-US": { name: "English", language: "en" }
    }
}
```

### <a name="text-translation-text"></a>Metin çeviri (metin)

İle ilişkili değer `text` özelliktir da burada her anahtar tanımlar metin çevirisi için desteklenen bir dil bir sözlük. Anahtarıyla ilişkilendirilmiş değeri dil açıklanır:

* `name`: Dil görünen adı.
* `dir`: Olan yönlülüğü `rtl` sağdan sola yazılan diller için veya `ltr` soldan sağa diller için.

Bir örnek verilmiştir:

```
{
    "text": {
        "en": { name: "English", dir: "ltr" }
    }
}
```

### <a name="text-to-speech-tts"></a>Metin okuma (tts)

Metin okuma özelliği ile tts, ilişkili da burada her anahtarı desteklenen bir sesli tanımlar sözlüğü değerdir. Sesli nesnesinin öznitelikleri şunlardır:

* `displayName`: Ses görünen adı.
* `gender`: Cinsiyeti sesinin (Erkek veya dişi).
* `locale`: Birincil dili alt etiket ve bölge alt etiket sesinin dil etiketi.
* `language`: Dil yazılmış ilişkilendirilen dil etiketi.
* `languageName`: Dil görünen adı.
* `regionName`: Bu dil için bölge adını görüntüler.

Bir örnek verilmiştir:

```
{
    "tts": {
        "en-US-Zira": {
            "displayName": "Zira",
            "gender": "female",
            "locale": "en-US",
            "languageName": "English",
            "regionName": "United States",
            "language": "en"
        }
}
```

### <a name="localization"></a>Yerelleştirme
Hizmet tüm adlar metin çeviri desteklenen tüm diller için 'Accept-Language' üstbilgisi dilini döndürür.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
Desteklenen diller kümesini tanımlayan nesne.

ModelExample değeri: 

Langagues {konuşma (nesne, isteğe bağlı), metin (nesne, isteğe bağlı), tts (nesne, isteğe bağlı)}

### <a name="headers"></a>Üst bilgiler

|Üst bilgi|Açıklama|Tür|
:--|:--|:--|
X-RequestId|Değer, istek tanımlamak için sunucu tarafından üretilen ve sorun giderme amacıyla kullanılır.|dize|

### <a name="parameters"></a>Parametreler

|Parametre|Açıklama|Parametre türü|Veri Türü|
|:--|:--|:--|:--|
|API sürümü    |İstemci tarafından istenilen API sürümü. İzin verilen değerler: `1.0`.|sorgu|dize|
|scope  |Desteklenen diller veya istemciye döndürülecek sesler kümesi. Bu parametre, anahtar sözcükleri virgülle ayrılmış listesi olarak belirtilir. Aşağıdaki anahtar sözcükler kullanılabilir:<ul><li>`speech`: Konuşma transcribe için desteklenen dil kümesini sağlar.</li><li>`tts`: Metin konuşma dönüştürme için desteklenen sesler kümesi sağlar.</li><li>`text`: Metin çevirmek için desteklenen dil kümesini sağlar.</li></ul>Bir değer belirtilmezse, değeri `scope` varsayılan olarak `text`.|sorgu|dize|
|X-ClientTraceId    |Bir istek izleme için kullanılan istemci tarafından oluşturulan bir GUID. İstemciler ilgili sorunları gidermeyi kolaylaştırmak için her istek ile yeni bir değer girin ve oturum gerekir.|üst bilgi|dize|
|Kabul dili    |Bazı alanları yanıt diller veya bölgeler adlarıdır. Adları döndürülen dil tanımlamak için bu parametreyi kullanın. Dili, doğru biçimlendirilmiş BCP 47 dil etiket sağlayarak belirtilir. Dil tanımlayıcıları ile döndürülen listesinden bir etiket seçin `text` kapsam. Desteklenmeyen diller için adları İngilizce dilinde sağlanır.<br/>Örneğin, değeri kullanmak `fr` değerini kullanın veya Fransızca adlarında isteği için `zh-Hant` isteği adlarına Geleneksel Çince.|üst bilgi|dize|
    
### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Neden|
|:--|:--|
|400|Hatalı istek. Geçerli olduğundan emin olmak için giriş parametrelerini denetleyin. Yanıt nesnesini hatanın ayrıntılı açıklamasını içerir.|
|429|Çok fazla istek sayısı.|
|500|Bir hata oluştu. Sorun devam ederse, istemci izleme tanımlayıcısı (X-ClientTraceId) rapor ya da istek tanımlayıcısıyla (X-RequestId).|
|503|Sunucu geçici olarak kullanılamıyor. Lütfen isteği yeniden deneyin. Sorun devam ederse, istemci izleme tanımlayıcısı (X-ClientTraceId) rapor ya da istek tanımlayıcısıyla (X-RequestId).|
