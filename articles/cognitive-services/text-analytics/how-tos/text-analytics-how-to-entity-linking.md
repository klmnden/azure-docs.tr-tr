---
title: Varlık bağlama metin analizi API'si ile kullanma
titleSuffix: Azure Cognitive Services
description: Tanımlama ve metin analizi REST API'sini kullanarak varlıkları çözme hakkında bilgi edinin.
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 09/12/2018
ms.author: ashmaka
ms.openlocfilehash: ad2168806f9ddd124faf66cdb5a0f51ed13dfadc
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45604748"
---
# <a name="how-to-identify-linked-entities-in-text-analytics-preview"></a>Metin analizi (Önizleme) bağlantılı varlıktaki belirleme

[Varlık bağlama API'si](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634) yapılandırılmamış metinleri alır ve her JSON belgesi için bağlantılarla birlikte disambiguated varlıkların listesini daha fazla bilgi için web üzerinde (Wikipedia ve Bing) döndürür. 

## <a name="entity-linking-vs-named-entity-recognition"></a>Varlık bağlama vs. Adlandırılmış Varlık Tanıma

Doğal dil işleme, varlık bağlama ve adlandırılmış varlık tanıma (NER) bir kolayca çakışabilir. Metin analizi Önizleme sürümünde `entities` uç noktası, sadece entity linking desteklenir.

Varlık bağlama metin (ör "Mars" erişebilir veya Latin afetler, war olarak kullanılıp kullanılmadığını belirleme) bulunan bir varlığın kimliğini ayırt etmek üzere yeteneğidir. Bu işlem varlıkların bağlı - Wikipedia Bilgi Bankası'nda olarak kullanılan tanınan için temel bir bilgi gerektirir `entities` uç nokta metin analizi.

### <a name="language-support"></a>Dil desteği

Varlık bağlama çeşitli dillerde kullanarak, karşılık gelen Bilgi Bankası her bir dilin kullanılması gerekir. Metin analizi bağlama, varlık için yani tarafından desteklenen her dil `entities` uç noktası bu dilde karşılık gelen Wikipedia topluluğunuza bağlantı. Yapılar boyutunu diller arasında değişir olduğundan, varlık işlevselliği'nın geri çağırma bağlama de değişir bekleniyor.


## <a name="preparation"></a>Hazırlama

JSON belgelerini şu biçimde olmalıdır: kimliği, metin, dil

Şu anda desteklenen diller için lütfen bkz [bu liste](../text-analytics-supported-languages.md).

Belge boyutuna, belge başına 5. 000'in altında karakter uzunluğunda olmalıdır ve en fazla 1.000 olabilir koleksiyon başına öğe sayısı (Kimlikler). İstek gövdesinde koleksiyon gönderilir. Aşağıdaki örnek, varlık bağlama sonuna kadar gönderdiğiniz içeriğin bir örnektir.

```
{"documents": [{"id": "1",
                "language": "en",
                "text": "I really enjoy the new XBox One S. It has a clean look, it has 4K/HDR resolution and it is affordable."
                },
               {"id": "2",
                "language": "en",
                "text": "The Seattle Seahawks won the Super Bowl in 2014."
                }
               ]
}
```    
    
## <a name="step-1-structure-the-request"></a>1. adım: isteği yapısı

İstek tanımı hakkında ayrıntılı bilgi bulunabilir [metin analizi API'sini çağırmak nasıl](text-analytics-how-to-call-api.md). Kolaylık olması için aşağıdaki noktaları açıklandı:

+ Oluşturma bir **POST** isteği. Bu istek için API belgelerini gözden geçirin: [varlık bağlama API'si](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634)

+ Anahtar ifade ayıklama HTTP uç noktasına ayarlayın. Dahil etmelisiniz `/entities` kaynak: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/entities`

+ Metin analizi işlemleri için erişim anahtarı eklemek için bir istek üst bilgisi ayarlayın. Daha fazla bilgi için [uç noktalarını bulma ve erişim anahtarlarını](text-analytics-how-to-access-key.md).

+ İstek gövdesinde bu analiz için hazırlanmış JSON belgeleri koleksiyonu sağlar.

> [!Tip]
> Kullanım [Postman](text-analytics-how-to-call-api.md) veya **API sınama Konsolu** içinde [belgeleri](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634) istek yapısı ve hizmete gönderin.

## <a name="step-2-post-the-request"></a>2. adım: isteği gönderin.

Analiz isteği alındığında gerçekleştirilir. Hizmet, dakika başına en fazla 100 istek kabul eder. Her isteği en fazla 1 MB olabilir.

Durum bilgisi olmayan hizmetin olduğunu hatırlayın. Hesabınızdaki veri depolanmaz. Sonuçları hemen yanıt olarak döndürülür.

## <a name="step-3-view-results"></a>3. adım: Sonuçları görüntüle

Tüm POST isteklerinden bir JSON yanıtı kimlikleri biçimlendirilmiş ve özellikleri algılandı döndürür.

Çıktı, hemen döndürülür. JSON kabul eden bir uygulama sonuçları akış veya çıkış yerel sistemin bir dosyaya kaydedin ve verileri sıralama ve arama olanak tanıyan bir uygulamayı içeri aktarın.

Varlık bağlama için çıktının bir örneği sonraki gösterilmektedir:

```
{
    "documents": [
        {
            "id": "1",
            "entities": [
                {
                    "name": "Xbox One",
                    "matches": [
                        {
                            "text": "XBox One",
                            "offset": 23,
                            "length": 8
                        }
                    ],
                    "wikipediaLanguage": "en",
                    "wikipediaId": "Xbox One",
                    "wikipediaUrl": "https://en.wikipedia.org/wiki/Xbox_One",
                    "bingId": "446bb4df-4999-4243-84c0-74e0f6c60e75"
                },
                {
                    "name": "Ultra-high-definition television",
                    "matches": [
                        {
                            "text": "4K",
                            "offset": 63,
                            "length": 2
                        }
                    ],
                    "wikipediaLanguage": "en",
                    "wikipediaId": "Ultra-high-definition television",
                    "wikipediaUrl": "https://en.wikipedia.org/wiki/Ultra-high-definition_television",
                    "bingId": "7ee02026-b6ec-878b-f4de-f0bc7b0ab8c4"
                }
            ]
        },
        {
            "id": "2",
            "entities": [
                {
                    "name": "2013 Seattle Seahawks season",
                    "matches": [
                        {
                            "text": "Seattle Seahawks",
                            "offset": 4,
                            "length": 16
                        }
                    ],
                    "wikipediaLanguage": "en",
                    "wikipediaId": "2013 Seattle Seahawks season",
                    "wikipediaUrl": "https://en.wikipedia.org/wiki/2013_Seattle_Seahawks_season",
                    "bingId": "eb637865-4722-4eca-be9e-0ac0c376d361"
                }
            ]
        }
    ],
    "errors": []
}
```

Kullanılabilir olduğunda, yanıt algılanan her varlık için Wikipedia kimliği, Wikipedia URL'si ve Bing kimliği içeriyor. Bu, bağlantılı varlıktaki ilgili bilgilerle uygulamanızı daha da geliştirmek için kullanılabilir.


## <a name="summary"></a>Özet

Bu makalede, kavramlar ve iş akışı Bilişsel hizmetler metin analizi kullanarak varlık bağlama öğrendiniz. Özet:

+ [Varlık bağlama API'si](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634) seçili diller için kullanılabilir.
+ İstek gövdesindeki JSON belgelerini bir kimliği, metin ve dil kodu içerir.
+ POST isteği olduğu için bir `/entities` kişiselleştirilmiş kullanarak uç nokta, [erişim anahtarı ve uç nokta](text-analytics-how-to-access-key.md) aboneliğiniz için geçerli.
+ Bağlantılı varlık (güven puanları, kaydırmalar ve her web bağlantılarının kimliği belge dahil) içeren yanıt çıktı, herhangi bir uygulamada kullanılabilir

## <a name="see-also"></a>Ayrıca bkz. 

 [Metin Analizine genel bakış](../overview.md)  
 [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)</br>
 [Metin analizi ürün sayfası](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Metin analizi API'si](//westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6)
