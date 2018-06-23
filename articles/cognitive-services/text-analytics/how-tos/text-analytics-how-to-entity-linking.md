---
title: Nasıl yapılır kullanın varlık bağlama metin Analytics REST API (Azure'da Microsoft Bilişsel hizmetler) | Microsoft Docs
description: Tanımlamaya ve çözümlemeye bu gözden geçirme öğreticide Azure üzerinde Microsoft Bilişsel Hizmetleri'ndeki metin Analytics REST API kullanarak varlıkları öğrenin.
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.technology: text-analytics
ms.topic: article
ms.date: 5/02/2018
ms.author: ashmaka
ms.openlocfilehash: 55bec1a0223b70749a97a30e2da92ef15128038c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352714"
---
# <a name="how-to-identify-linked-entities-in-text-analytics-preview"></a>Metin analizi (Önizleme) bağlantılı varlıklarda belirleme

[Varlık bağlama API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634) yapılandırılmamış metin alır ve her JSON belgesi için bağlantılar ile birlikte disambiguated varlıklar listesi daha fazla bilgi için (Wikipedia ve Bing) Web'de döndürür. 

## <a name="entity-linking-vs-named-entity-recognition"></a>Varlık bağlama vs. Adlandırılmış Varlık Tanıma

Doğal dil işlemede varlık bağlama ve adlandırılmış varlık tanıma (NER) kavramlarını kolayca çakışabilir. Metin analizi Önizleme sürümünde `entities` uç noktası, yalnızca varlık bağlama desteklenir.

Bağlama varlık tanımlamak ve metin (örneğin "Mars" gezegen veya war, Roma Tanrı olarak kullanılmakta olup olmadığını belirleme) bulunan bir varlık kimliğini belirsizliğini ortadan kaldırmak için yeteneğidir. Bu işlem bilgi varlıkları bağlı - Wikipedia için Bilgi Bankası'nda olarak kullanılan tanınan için temel gerektirir `entities` endpoint metin analizi.

### <a name="language-support"></a>Dil desteği

Çeşitli dillerde bağlama varlık kullanarak her dilde karşılık gelen bir Bilgi Bankası kullanılması gerekir. Metin analizi bağlama bir varlık için bu tarafından desteklenen her dil anlamına gelir `entities` uç noktası o dilde karşılık gelen Wikipedia gövde bağlantı. Corpora boyutu diller arasında değişir olduğundan, geri çağırma işlevleri'nin bağlama varlık da değişir beklenir.


## <a name="preparation"></a>Hazırlama

JSON belgeleri şu biçimde olmalıdır: kimliği, metin, dil

Şu anda desteklenen diller için lütfen bkz. [bu listeyi](../text-analytics-supported-languages.md).

Belge boyutu, belge başına 5. 000'in altında karakter uzunluğunda olmalıdır ve en çok 1.000 olabilir koleksiyon başına öğeleri (Kimlikler). Koleksiyon istek gövdesinde gönderilir. Aşağıdaki örnekte, varlık bağlama son gönderme içeriğinin bir çizimidir.

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

İstek tanımının ayrıntıları bulunabilir [metin Analytics API'sini çağırmak nasıl](text-analytics-how-to-call-api.md). Kolaylık olması için aşağıdaki noktaları açıklandı:

+ Oluşturma bir **POST** isteği. Bu istek için API belgelerini gözden geçirin: [varlık bağlama API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634)

+ Anahtar tümcecik ayıklama için HTTP uç noktası olarak ayarlayın. İçermesi gerekir `/entities` kaynak: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/entities`

+ Metin analizi işlemleri için erişim anahtarı eklemek için bir istek üstbilgisini ayarlayın. Daha fazla bilgi için bkz: [uç noktalarını bulma ve erişim anahtarları](text-analytics-how-to-access-key.md).

+ İstek gövdesinde Bu çözümleme için hazırlanmış JSON belgeleri koleksiyonu sağlayın

> [!Tip]
> Kullanım [Postman](text-analytics-how-to-call-api.md) veya açın **API sınama Konsolu** içinde [belgelerine](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634) bir istek yapısı ve hizmete gönderme.

## <a name="step-2-post-the-request"></a>2. adım: isteği gönderme

Çözümleme isteği alındığında gerçekleştirilir. Hizmet dakika başına en fazla 100 isteklerini kabul eder. Her istek en fazla 1 MB olabilir.

Hizmet durum bilgisi olmayan bir geri çağırma. Hiçbir veri hesabınızda depolanır. Sonuçlar hemen yanıt olarak döndürülür.

## <a name="step-3-view-results"></a>3. adım: Sonuçları görüntüleme

Tüm POST isteklerinden JSON yanıt kimlikleri biçimlendirilmiş ve özellikleri algılandı döndürür.

Çıktı hemen döndürülür. JSON kabul eden bir uygulama sonuçları akış veya yerel sistemindeki bir dosyaya çıkış kaydedin ve sıralama, arama ve verileri işlemek izin veren bir uygulamayı alma.

Bağlama varlık için bir çıkış sonraki gösterilir:

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

Kullanılabilir olduğunda yanıt Wikipedia kimliği, Wikipedia URL ve Bing kimliği için algılanan her varlık içeriyor. Bunlar daha fazla bağlantılı varlığı ile ilgili bilgiler ile uygulamanızı geliştirmek için kullanılabilir.


## <a name="summary"></a>Özet

Bu makalede, kavramlar ve metin analizi Bilişsel Hizmetleri'ni kullanarak varlık bağlama için iş akışı öğrendiniz. Özet:

+ [Varlık bağlama API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634) Seçilen diller için kullanılabilir.
+ İstek gövdesinde JSON belgeleri bir kimliği, metin ve dil kodu içerir.
+ POST isteğini olduğu için bir `/entities` kişiselleştirilmiş bir kullanarak uç noktasını, [erişim anahtarı ve bir uç nokta](text-analytics-how-to-access-key.md) , aboneliğiniz için geçerli.
+ (Belge kimliği puanları, uzaklıkları ve her biri için web bağlantıları güvenirlik dahil) bağlantılı varlıklarının oluşur yanıt çıktısı herhangi bir uygulamada kullanılabilir

## <a name="see-also"></a>Ayrıca bkz. 

 [Metin analizi genel bakış](../overview.md)  
 [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)</br>
 [Metin analizi ürün sayfası](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Metin analizi API](//westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6)
