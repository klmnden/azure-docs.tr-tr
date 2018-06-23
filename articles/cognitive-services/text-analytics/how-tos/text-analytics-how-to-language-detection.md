---
title: Nasıl yapılır dil algılama metin Analytics REST API'sindeki (Azure'da Microsoft Bilişsel hizmetler) | Microsoft Docs
description: Bu izlenecek yol öğreticide Azure üzerinde Microsoft Bilişsel Hizmetleri'ndeki metin Analytics REST API kullanarak dili Algıla yapma.
services: cognitive-services
author: HeidiSteen
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 3/07/2018
ms.author: heidist
ms.openlocfilehash: f8e2d9a36533c298addcf42d3cb2061e9c2d1ac7
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352229"
---
# <a name="how-to-detect-language-in-text-analytics"></a>Metin analizi dil algılamak nasıl

[Dil algılama API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7) giriş ve her belge için metin değerlendirir ve analiz gücünü gösteren bir puan ile Dil tanımlayıcıları döndürür. Metin analizi en fazla 120 dilleri tanır.

Bu toplama rastgele metin içeriği depolar için bu özelliği dil bilinmeyen olduğu yararlıdır. Hangi dilde girdi belgesinde kullanıldığını belirlemek için bu çözümleme sonuçlarını ayrıştıramıyor. Yanıt ayrıca (0 ile 1 arasında bir değer) modeli güvenirlik yansıtan bir puan döndürür.

## <a name="preparation"></a>Hazırlama

JSON belgeleri şu biçimde olmalıdır: kimliği, metin

Belge boyutu, belge başına 5. 000'in altında karakter uzunluğunda olmalıdır ve en çok 1.000 olabilir koleksiyon başına öğeleri (Kimlikler). Koleksiyon istek gövdesinde gönderilir. Dil algılama için gönderme içeriğine bir örnek verilmiştir.

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
                "text": "Этот документ находится на английском языке."
            }
        ]
    }
```

## <a name="step-1-structure-the-request"></a>1. adım: isteği yapısı

İstek tanımının ayrıntıları bulunabilir [metin Analytics API'sini çağırmak nasıl](text-analytics-how-to-call-api.md). Kolaylık olması için aşağıdaki noktaları açıklandı:

+ Oluşturma bir **POST** isteği. Bu istek için API belgelerini gözden geçirin: [dil algılama API'si](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7)

+ Dil algılama için HTTP uç noktası olarak ayarlayın. İçermesi gerekir `/languages` kaynak: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages`

+ Metin analizi işlemleri için erişim anahtarı eklemek için bir istek üstbilgisini ayarlayın. Daha fazla bilgi için bkz: [uç noktalarını bulma ve erişim anahtarları](text-analytics-how-to-access-key.md).

+ İstek gövdesinde Bu çözümleme için hazırlanmış JSON belgeleri koleksiyonu sağlayın

> [!Tip]
> Kullanım [Postman](text-analytics-how-to-call-api.md) veya açın **API sınama Konsolu** içinde [belgelerine](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7) bir istek yapısı ve hizmete gönderme.

## <a name="step-2-post-the-request"></a>2. adım: isteği gönderme

Çözümleme isteği alındığında gerçekleştirilir. Hizmet dakika başına en fazla 100 isteklerini kabul eder. Her istek en fazla 1 MB olabilir.

Hizmet durum bilgisi olmayan bir geri çağırma. Hiçbir veri hesabınızda depolanır. Sonuçlar hemen yanıt olarak döndürülür.


## <a name="step-3-view-results"></a>3. adım: Sonuçları görüntüleme

Tüm POST isteklerinden JSON yanıt kimlikleri biçimlendirilmiş ve özellikleri algılandı döndürür.

Çıktı hemen döndürülür. JSON kabul eden bir uygulama sonuçları akış veya yerel sistemindeki bir dosyaya çıkış kaydedin ve sıralama, arama ve verileri işlemek izin veren bir uygulamayı alma.

Örnek istek için sonuçları aşağıdaki JSON gibi görünmelidir. Birden çok öğe içeren bir belge olduğuna dikkat edin. Çıktı İngilizce'dir. Dil tanımlayıcıları dahil kolay bir ad ve bir dil kodu [ISO 639-1](https://www.iso.org/standard/22109.html) biçimi.

Pozitif skoru 1.0 analiz yüksek olası güvenirlik düzeyini ifade eder.



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

### <a name="ambiguous-content"></a>Belirsiz içeriği

Çözümleyici giriş ayrıştırılamıyor varsa (örneğin, yalnızca Arapça rakamları oluşan bir metin bloğunu gönderdiğiniz varsayın), onu döndürür `(Unknown)`.

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
### <a name="mixed-language-content"></a>Karışık dil içerik

Karışık dil içeriği aynı belge içinde içerik büyük gösterimi ile ancak bu değerlendirme Marjinal gücünü yansıtan alt pozitif bir derecelendirme dilini döndürür. Aşağıdaki örnekte, bir giriş İngilizce, İspanyolca ve Fransızca bir karışımı olabilir. Çözümleyici baskın dil belirlemek için her segment karakter sayar.

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

Sonuçta çıktı daha zayıf bir güven düzeyini gösteren baskın dili, değerinden 1.0 puanı oluşur.

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

Bu makalede, kavramlar ve metin analizi Bilişsel Hizmetleri'ni kullanarak dil algılama için iş akışı öğrendiniz. Daha önce açıklandığı ve gösterildiği önemli noktalarından hızlı anımsatıcısı şunlardır:

+ [Dil algılama API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7) 120 diller için kullanılabilir.
+ İstek gövdesinde JSON belgeleri bir kimliği ve metin içerir.
+ POST isteğini olduğu için bir `/languages` kişiselleştirilmiş bir kullanarak uç noktasını, [erişim anahtarı ve bir uç nokta](text-analytics-how-to-access-key.md) , aboneliğiniz için geçerli.
+ JSON, Excel ve Power BI birkaçıdır dahil olmak üzere kabul eden herhangi bir uygulama için Dil tanımlayıcıları için her belge kimliği oluşur, yanıt çıktısı gönderilebilen.

## <a name="see-also"></a>Ayrıca bkz. 

 [Metin analizi genel bakış](../overview.md)  
 [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)</br>
 [Metin analizi ürün sayfası](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Düşünceleri Çözümle](text-analytics-how-to-sentiment-analysis.md)
