---
title: Metin analizi REST API ile dil algılama | Microsoft Docs
description: Azure Bilişsel Hizmetler'in sunduğu metin analizi REST API kullanarak dil tespit etme.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: sample
ms.date: 02/26/2019
ms.author: aahi
ms.openlocfilehash: 4ccb8665c9880e21897c81ed4b4ff534e52bb6d1
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60002281"
---
# <a name="example-how-to-detect-language-with-text-analytics"></a>Örnek: Metin analizi diliyle tespit etme

[Dil Algılama API’si](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7), metin girişini değerlendirir ve her bir belge için analizin gücünü belirten bir puan ile dil tanımlayıcılarını döndürür. Metin Analizi 120’ye kadar dili tanır.

Bu özellik, dilin bilinmediği rastgele metni toplayan içerik depoları için kullanışlıdır. Giriş belgesinde hangi dilin kullanıldığını belirlemek için bu analizin sonuçlarını ayrıştırabilirsiniz. Yanıt ayrıca modelin güvenilirliğini yansıtan bir puan da (0 ile 1 arasında bir değer) döndürür.

> [!TIP]
> Metin analizi de Linux tabanlı bir Docker kapsayıcı görüntüsü dil için algılama sağlar, böylece [yükleyin ve metin analizi kapsayıcı çalıştırın](text-analytics-how-to-install-containers.md) verilerinizi yakın.

## <a name="preparation"></a>Hazırlık

JSON belgelerini şu biçimde olmalıdır: Kimlik, metin

Belge boyutuna, belge başına altında 5.120 karakter uzunluğunda olmalıdır ve en fazla 1.000 olabilir koleksiyon başına öğe sayısı (Kimlikler). Koleksiyon, istek gövdesinde gönderilir. Aşağıda, dil algılama için gönderebileceğiniz bir içerik örneği verilmiştir.

   ```
    {
        "documents": [
            {
                "id": "1",
                "text": "This document is in English."
            },
            {
                "id": "2",
                "text": "Este documento está en inglés."
            },
            {
                "id": "3",
                "text": "Ce document est en anglais."
            },
            {
                "id": "4",
                "text": "本文件为英文"
            },                
            {
                "id": "5",
                "text": "Этот документ на английском языке."
            }
        ]
    }
```

## <a name="step-1-structure-the-request"></a>1. Adım: Yapı isteği

İstek tanımıyla ilgili ayrıntılara [Metin Analizi API’sini çağırma](text-analytics-how-to-call-api.md) bölümünden erişilebilir. Kolaylık olması için aşağıdaki noktalar yeniden belirtilmektedir:

+ Bir **POST** isteği oluşturun. Bu istek için API belgelerini gözden geçirin: [Dil algılama API'si](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7)

+ Dil algılama, Azure veya bir örneklenmiş bir metin analizi kaynak kullanarak HTTP uç noktasına ayarlayın [metin analizi kapsayıcı](text-analytics-how-to-install-containers.md). `/languages` kaynağını içermelidir: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/languages`

+ Metin Analizi işlemlerine yönelik erişim anahtarını dahil etmek için bir istek üst bilgisi ayarlayın. Daha fazla bilgi için bkz. [Uç noktaları ve erişim anahtarlarını bulma](text-analytics-how-to-access-key.md).

+ İstek gövdesinde, bu analiz için hazırladığınız JSON belgeleri koleksiyonunu sağlayın

> [!Tip]
> İsteği yapılandırmak ve hizmete GÖNDERMEK için [Postman](text-analytics-how-to-call-api.md) kullanın veya [belgelerdeki](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7) **API testi konsolu**’nu açın.

## <a name="step-2-post-the-request"></a>2. Adım: POST isteği

İstek alındığında analiz gerçekleştirilir. Hizmet dakikada en fazla 100 istek kabul eder. Her istek maksimum 1 MB olabilir.

Hizmetin durum bilgisi olmadığını unutmayın. Hesabınızda bir veri depolanmaz. Sonuçlar hemen yanıtta döndürülür.


## <a name="step-3-view-results"></a>3. Adım: Sonuçları görüntüleme

Tüm POST istekleri, kimlikler ve algılanan özelliklerle JSON tarafından biçimlendirilmiş bir yanıt döndürür.

Hemen çıktı döndürülür. Sonuçları, JSON kabul eden bir uygulamada akışa alabilir veya çıktıyı yerel sistemde bir dosyaya kaydedebilir, sonra da verileri sıralamanıza, aramanıza ve işlemenize olanak sağlayan bir uygulamaya içeri aktarabilirsiniz.

Örnek istek için sonuçlar, aşağıdaki JSON gibi görünmelidir. Bunun birden çok öğe içeren tek bir belge olduğuna dikkat edin. Çıktı İngilizce dilindedir. Dil tanımlayıcıları bir kolay ad ve [ISO 639-1](https://www.iso.org/standard/22109.html) biçiminde dil kodu içerir.

1,0 pozitif puanı, analizin mümkün olan en yüksek güvenilirlik düzeyini ifade eder.



```
{
    "documents": [
        {
            "id": "1",
            "detectedLanguages": [
                {
                    "name": "English",
                    "iso6391Name": "en",
                    "score": 1
                }
            ]
        },
        {
            "id": "2",
            "detectedLanguages": [
                {
                    "name": "Spanish",
                    "iso6391Name": "es",
                    "score": 1
                }
            ]
        },
        {
            "id": "3",
            "detectedLanguages": [
                {
                    "name": "French",
                    "iso6391Name": "fr",
                    "score": 1
                }
            ]
        },
        {
            "id": "4",
            "detectedLanguages": [
                {
                    "name": "Chinese_Simplified",
                    "iso6391Name": "zh_chs",
                    "score": 1
                }
            ]
        },
        {
            "id": "5",
            "detectedLanguages": [
                {
                    "name": "Russian",
                    "iso6391Name": "ru",
                    "score": 1
                }
            ]
        }
    ],
```

### <a name="ambiguous-content"></a>Belirsiz içerik

Çözümleyici, girişi ayrıştıramazsa (örneğin, yalnızca Arapça sayılardan oluşan bir metin bloğu gönderdiğinizi varsayın), `(Unknown)` değerini döndürür.

```
    {
      "id": "5",
      "detectedLanguages": [
        {
          "name": "(Unknown)",
          "iso6391Name": "(Unknown)",
          "score": "NaN"
        }
      ]
```
### <a name="mixed-language-content"></a>Karışık dil içeriği

Aynı belgedeki karışık dil içeriği, içerikteki en büyük temsille birlikte dili, ancak söz konusu değerlendirmenin marjinal gücünü yansıtan daha düşük pozitif bir derecelendirme ile birlikte döndürür. Aşağıdaki örnekte giriş, İngilizce, İspanyolca ve Fransızca dillerinin birleşimidir. Çözümleyici, hakim dili belirlemek için her bir kesimdeki karakterleri sayar.

**Girdi**

```
{
  "documents": [
    {
      "id": "1",
      "text": "Hello, I would like to take a class at your University. ¿Se ofrecen clases en español? Es mi primera lengua y más fácil para escribir. Que diriez-vous des cours en français?"
    }
  ]
}
```

**Çıktı**

Sonuçta elde edilen çıktı, daha zayıf bir güvenilirlik düzeyini belirten 1,0’dan daha küçük bir puanla hakim dilden oluşur.

```
{
  "documents": [
    {
      "id": "1",
      "detectedLanguages": [
        {
          "name": "Spanish",
          "iso6391Name": "es",
          "score": 0.9375
        }
      ]
    }
  ],
  "errors": []
}
```

## <a name="summary"></a>Özet

Bu makalede, Bilişsel Hizmetler’de Metin Analizi’ni kullanarak dil algılama için kavramları ve iş akışını öğrendiniz. Aşağıda, önceden açıklanmış ve gösterilmiş olan ana noktaların hızlı bir anımsatıcısı yer almaktadır:

+ [Dil algılama API’si](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7) 120 dilde mevcuttur.
+ İstek gövdesindeki JSON belgelerini bir kimliği ve metin ekleyin.
+ POST isteği, aboneliğiniz için geçerli olan kişiselleştirilmiş bir [erişim anahtarı ve uç nokta](text-analytics-how-to-access-key.md) kullanılarak `/languages` uç noktasına yapılır.
+ Her belge kimliği için dil tanımlayıcılarından oluşan yanıt çıktısı, Excel ve Power BI da dahil olmak üzere JSON kabul eden tüm uygulamalarda akışa alınabilir.

## <a name="see-also"></a>Ayrıca bkz. 

 [Metin Analizine genel bakış](../overview.md)  
 [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)</br>
 [Metin Analizi ürün sayfası](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yaklaşımı analiz etme](text-analytics-how-to-sentiment-analysis.md)
