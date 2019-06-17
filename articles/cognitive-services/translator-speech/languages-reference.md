---
title: Translator konuşma tanıma API'si dilleri yöntemi
titleSuffix: Azure Cognitive Services
description: Translator konuşma tanıma API'si dilleri yöntemi kullanın.
services: cognitive-services
author: Jann-Skotdal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-speech
ms.topic: conceptual
ms.date: 05/18/2018
ms.author: v-jansko
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 7498ba08b9ce7b6aae10f38a393eb8cba37f3f4e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60827910"
---
# <a name="translator-speech-api-languages"></a>Translator konuşma tanıma API'si: Languages

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-translator-speech-deprecation-note.md)]

Translator konuşma çevirisi, hizmetleri tarafından desteklenen dillerin listesi sürekli olarak genişletir. Translator konuşma çevirisi hizmeti ile kullanmak için kullanılabilen dilleri kümesini bulmak için bu API'yi kullanın.

Mevcut diller almak için API kullanımını gösteren kod örnekleri web'da [Microsoft Translator GitHub site](https://github.com/MicrosoftTranslator).

## <a name="implementation-notes"></a>Uygulama Notları

### <a name="get-languages"></a>/Languages Al

Geniş bir dil kümesini okuma, transcribed metinleri çevirin ve Sentezlenen konuşma çevirisi üretmek için özelliği kullanılabilir.

Bir istemcinin kullandığı `scope` sorgu parametresi, ilgileniyor hangi dillerin kümelerini tanımlamak için.

* **Konuşmayı metne dönüştürme:** Sorgu parametresi kullandığınızda `scope=speech` Konuşmayı metne konuşmaların kullanılabilen dilleri kümesini almak için.
* **Metin çevirisi:** Sorgu parametresi kullandığınızda `scope=text` transcribed metni çevirmek kullanılabilen dilleri kümesini almak için.
* **Metin okuma:**  Sorgu parametresi kullandığınızda `scope=tts` diller ve sesler çevrilen metni yeniden konuşmaya sentezlemek kullanılabilir kümesini almak için.

Bir istemci birden fazla eşzamanlı olarak virgülle ayrılmış listesi belirterek alabilirsiniz. Örneğin, `scope=speech,text,tts`.

Başarılı bir yanıt her talep kümesi özelliğine sahip bir JSON nesnesi aranır.

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

Bir istemci kullanabildiğinden `scope` desteklenen dillerin hangi kümelerini seçmek için sorgu parametresi döndürülmesi, gerçek bir yanıt yalnızca yukarıda gösterilen tüm özelliklerinin bir alt kümesi içerebilir.

Her bir özellik ile sağlanan değer aşağıdaki gibidir.

### <a name="speech-to-text-speech"></a>Konuşmayı metne dönüştürme (okuma)

Konuşma metin özelliği ile ilişkili değer `speech`, bir sözlük (anahtarın, değer) çiftleri. Her anahtar, Konuşmayı metne dönüştürme için desteklenen bir dil tanımlar. Anahtar tanımlayıcısı olan istemci API'sine geçirir. Anahtarıyla ilişkilendirilmiş değeri, aşağıdaki özelliklere sahip bir nesnedir:

* `name`: Dil adını görüntüler.
* `language`: İlişkili dil yazılan dil etiketi. Aşağıdaki "Metin işlem" bölümüne bakın.
Bir örnek verilmiştir:

```
{
    "speech": {
        "en-US": { name: "English", language: "en" }
    }
}
```

### <a name="text-translation-text"></a>Metin çevirisi (metin)

İlişkili değer `text` özelliği de burada her anahtar tanımlar metin çevirisi için desteklenen bir dil bir sözlük. Dil anahtarıyla ilişkilendirilmiş değeri açıklanmaktadır:

* `name`: Dil adını görüntüler.
* `dir`: Olan yönlülüğü `rtl` sağdan sola diller için veya `ltr` sağdan sola diller için.

Bir örnek verilmiştir:

```
{
    "text": {
        "en": { name: "English", dir: "ltr" }
    }
}
```

### <a name="text-to-speech-tts"></a>Metin okuma (tts)

Metin okuma özelliği, tts, ilişkili değer de burada her anahtar, desteklenen bir ses tanımlar sözlüktür. Bir ses nesnesinin öznitelikleri şunlardır:

* `displayName`: Ses için görünen ad.
* `gender`: Cinsiyet (Erkek veya Kadın) sesinin.
* `locale`: Birincil dili uzantılarından ve bölge uzantılarından ses dil etiketi.
* `language`: İlişkili dil yazılan dil etiketi.
* `languageName`: Dil adını görüntüler.
* `regionName`: Bu dil için görünen ad alanının.

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
Hizmet 'Accept-Language' üst bilgisi, metin çevirisi desteklenen tüm diller için dil tüm adlarını döndürür.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
Desteklenen dil kümesini tanımlayan nesne.

ModelExample değer:

Langagues {konuşma (object, isteğe bağlı), metin (object, isteğe bağlı), tts (object, isteğe bağlı)}

### <a name="headers"></a>Üst bilgiler

|Üstbilgi|Açıklama|Tür|
:--|:--|:--|
X-RequestId|Değer, istek tanımlamak için sunucu tarafından oluşturulan ve sorun giderme amacıyla kullanılır.|string|

### <a name="parameters"></a>Parametreler

|Parametre|Açıklama|Parametre türü|Veri Türü|
|:--|:--|:--|:--|
|API sürümü    |İstemci tarafından istenen API sürümü. İzin verilen değerler: `1.0`.|sorgu|string|
|scope  |Desteklenen diller ya da ses istemciye döndürmek için ayarlar. Bu parametre, anahtar sözcükleri virgülle ayrılmış listesi olarak belirtilir. Aşağıdaki anahtar sözcükler vardır:<ul><li>`speech`: Konuşma tanıma özelliği için desteklenen dil kümesini sağlar.</li><li>`tts`: Metni konuşmaya dönüştürme için desteklenen kişilerden daha fazlasını sunmaktadır.</li><li>`text`: Metin çevirme için desteklenen dil kümesini sağlar.</li></ul>Bir değer belirtilmezse, değerini `scope` varsayılan olarak `text`.|sorgu|string|
|X-ClientTraceId    |Bir istek izleme için kullanılan istemci tarafından oluşturulan GUID. İlgili sorunları gidermeyi kolaylaştırmak için istemciler her istek ile yeni bir değer sağlayın ve oturumu.|üst bilgi|string|
|Kabul dil    |Bazı alanlar yanıt diller ya da bölgelerdeki adlarıdır. Adları döndürülen dil tanımlamak için bu parametreyi kullanın. Dil, doğru biçimlendirilmiş BCP 47 dil etiketi sağlayarak belirtilir. Etiket ile döndürülen Dil tanımlayıcıları listesinden seçin `text` kapsam. Desteklenmeyen diller için adlarını İngilizce dilinde sağlanır.<br/>Örneğin, değerini kullanın `fr` Fransızca adlarında istek veya değeri kullanmak için `zh-Hant` isteği adlarına Geleneksel Çince.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletilerini

|HTTP durum kodu|Reason|
|:--|:--|
|400|Hatalı istek. Geçerli olduklarından emin olmak için giriş parametrelerini denetleyin. Yanıt nesnesini hatanın daha ayrıntılı bir açıklama içerir.|
|429|Çok fazla istek sayısı.|
|500|Bir hata oluştu. Sorun devam ederse, istemci izleme tanımlayıcısı (X-ClientTraceId) rapor ya da istek tanımlayıcısı (X-RequestId).|
|503|Sunucu geçici olarak kullanılamıyor. Lütfen isteği yeniden deneyin. Sorun devam ederse, istemci izleme tanımlayıcısı (X-ClientTraceId) rapor ya da istek tanımlayıcısı (X-RequestId).|
