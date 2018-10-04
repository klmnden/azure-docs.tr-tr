---
title: Varlık tanıma, metin analizi API'si ile kullanma
titleSuffix: Azure Cognitive Services
description: Metin analizi REST API'sini kullanarak varlıkları tanıması konusunda bilgi edinin.
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 10/01/2018
ms.author: ashmaka
ms.openlocfilehash: b2916e5c414562c55c35c9c5e7ab378963e004be
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48248083"
---
# <a name="how-to-use-named-entity-recognition-in-text-analytics-preview"></a>Adlandırılmış varlık tanıma, metin analizi (Önizleme) kullanma

[Varlık tanıma API'si](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634) yapılandırılmamış metinleri alır ve her JSON belgesi için bağlantılarla birlikte disambiguated varlıkların listesini daha fazla bilgi için web üzerinde (Wikipedia ve Bing) döndürür. 

## <a name="entity-linking-and-named-entity-recognition"></a>Varlık bağlama ve adlandırılmış varlık tanıma

Metin analizi `entities` supprts uç nokta her iki adlandırılmış varlık tanıma (NER) ve varlık bağlama.

### <a name="entity-linking"></a>Varlık Bağlama
Varlık bağlama metin (ör "Mars" erişebilir veya Latin afetler, war olarak kullanılıp kullanılmadığını belirleme) bulunan bir varlığın kimliğini ayırt etmek üzere yeteneğidir. Bu işlem varlıkların bağlı - Wikipedia Bilgi Bankası'nda olarak kullanılan tanınan için temel bir bilgi gerektirir `entities` uç nokta metin analizi.

Metin analizi, [2.1 önizleme sürümüne](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634), yalnızca varlık bağlama kullanılabilir.

### <a name="named-entity-recognition-ner"></a>Adlandırılmış varlık tanıma (NER)
Adlandırılmış varlık tanıma (NER) metin farklı varlıklarda tanımlamak ve bunları önceden tanımlanmış sınıflara kategorilere ayırma olanağıdır. Varlıkları desteklenen sınıflarını aşağıda listelenmiştir.

Metin analizi sürüm 2.1 Önizleme (`https://[region].api.cognitive.microsoft.com/text/analytics/v2.1-preview/entities`), varlık bağlama ve adlandırılmış varlık tanıma (NER) kullanılabilir.

### <a name="language-support"></a>Dil desteği

Varlık bağlama çeşitli dillerde kullanarak, karşılık gelen Bilgi Bankası her bir dilin kullanılması gerekir. Metin analizi bağlama, varlık için yani tarafından desteklenen her dil `entities` uç noktası bu dilde karşılık gelen Wikipedia topluluğunuza bağlantı. Yapılar boyutunu diller arasında değişir olduğundan, varlık işlevselliği'nın geri çağırma bağlama de değişir bekleniyor.

## <a name="supported-types-for-named-entity-recognition"></a>Adlandırılmış varlık tanıma için desteklenen türler:

| Tür  | Alt tür | Örnek |
|:-----------   |:------------- |:---------|
| Kişi        | YOK\*         | "Jeff", "Bill Gates'le"     |
| Konum      | YOK\*         | "Redmond, Washington", "İstanbul"  |
| Kuruluş  | YOK\*         | "Microsoft"   |
| Miktar      | Sayı        | "6", "altı"     | 
| Miktar      | Yüzde    | "%50", "yüzde Elli"| 
| Miktar      | Sıra       | "2", "saniye"     | 
| Miktar      | NumberRange   | "4-8"     | 
| Miktar      | Yaş           | "90 gün eski", "30 yıl eski"    | 
| Miktar      | Para birimi      | "$10.99"     | 
| Miktar      | Boyut     | "10 mil", "40 cm"     | 
| Miktar      | Sıcaklık   | "32 derece"    |
| DateTime      | YOK\*         | "6:30 PM 4 Şubat 2012"      | 
| DateTime      | Tarih          | "Mayıs 2 2017", "02/05/2017"   | 
| Tarih Saat     | Zaman          | "8 am", "8:00"  | 
| DateTime      | DateRange     | "2 Mayıs Mayıs 5 için"    | 
| DateTime      | timeRange     | "18: 00 için 7 pm"     | 
| DateTime      | Süre      | "1 dakika ve 45 saniye"   | 
| DateTime      | Ayarla           | "her Salı"     | 
| DateTime      | TimeZone      |    | 
| URL'si           | YOK\*         | "http://www.bing.com"    |
| Email         | YOK\*         | "support@contoso.com" |
\* Giriş ve ayıklanan varlıklar bağlı olarak, bazı varlıklar atlasa `SubType`.



## <a name="preparation"></a>Hazırlama

JSON belgelerini şu biçimde olmalıdır: kimliği, metin, dil

Şu anda desteklenen diller için lütfen bkz [bu liste](../text-analytics-supported-languages.md).

Belge boyutuna, belge başına 5. 000'in altında karakter uzunluğunda olmalıdır ve en fazla 1.000 olabilir koleksiyon başına öğe sayısı (Kimlikler). İstek gövdesinde koleksiyon gönderilir. Aşağıdaki örnek, varlık bağlama sonuna kadar gönderdiğiniz içeriğin bir örnektir.

```
{"documents": [{"id": "1",
                "language": "en",
                "text": "Jeff bought three dozen eggs because there was a 50% discount."
                },
               {"id": "2",
                "language": "en",
                "text": "The Great Depression began in 1929. By 1933, the GDP in America fell by 25%."
                }
               ]
}
```    
    
## <a name="step-1-structure-the-request"></a>1. adım: isteği yapısı

İstek tanımı hakkında ayrıntılı bilgi bulunabilir [metin analizi API'sini çağırmak nasıl](text-analytics-how-to-call-api.md). Kolaylık olması için aşağıdaki noktaları açıklandı:

+ Oluşturma bir **POST** isteği. Bu istek için API belgelerini gözden geçirin: [varlık bağlama API'si](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634)

+ Anahtar ifade ayıklama HTTP uç noktasına ayarlayın. Dahil etmelisiniz `/entities` kaynak: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1-preview/entities`

+ Metin analizi işlemleri için erişim anahtarı eklemek için bir istek üst bilgisi ayarlayın. Daha fazla bilgi için [uç noktalarını bulma ve erişim anahtarlarını](text-analytics-how-to-access-key.md).

+ İstek gövdesinde bu analiz için hazırlanmış JSON belgeleri koleksiyonu sağlar.

> [!Tip]
> Kullanım [Postman](text-analytics-how-to-call-api.md) veya **API sınama Konsolu** içinde [belgeleri](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634) istek yapısı ve hizmete gönderin.

## <a name="step-2-post-the-request"></a>2. adım: isteği gönderin.

Analiz isteği alındığında gerçekleştirilir. Hizmet, dakika başına en fazla 100 istek kabul eder. Her isteği en fazla 1 MB olabilir.

Durum bilgisi olmayan hizmetin olduğunu hatırlayın. Hesabınızdaki veri depolanmaz. Sonuçları hemen yanıt olarak döndürülür.

## <a name="step-3-view-results"></a>3. adım: Sonuçları görüntüle

Tüm POST isteklerinden bir JSON yanıtı kimlikleri biçimlendirilmiş ve özellikleri algılandı döndürür.

Çıktı, hemen döndürülür. JSON kabul eden bir uygulama sonuçları akış veya çıkış yerel sistemin bir dosyaya kaydedin ve verileri sıralama ve arama olanak tanıyan bir uygulamayı içeri aktarın.

Varlık bağlama için çıktının bir örneği sonraki gösterilmektedir:

```json
{
    "Documents": [
        {
            "Id": "1",
            "Entities": [
                {
                    "Name": "Jeff",
                    "Matches": [
                        {
                            "Text": "Jeff",
                            "Offset": 0,
                            "Length": 4
                        }
                    ],
                    "Type": "Person"
                },
                {
                    "Name": "three dozen",
                    "Matches": [
                        {
                            "Text": "three dozen",
                            "Offset": 12,
                            "Length": 11
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Number"
                },
                {
                    "Name": "50",
                    "Matches": [
                        {
                            "Text": "50",
                            "Offset": 49,
                            "Length": 2
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Number"
                },
                {
                    "Name": "50%",
                    "Matches": [
                        {
                            "Text": "50%",
                            "Offset": 49,
                            "Length": 3
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Percentage"
                }
            ]
        },
        {
            "Id": "2",
            "Entities": [
                {
                    "Name": "Great Depression",
                    "Matches": [
                        {
                            "Text": "The Great Depression",
                            "Offset": 0,
                            "Length": 20
                        }
                    ],
                    "WikipediaLanguage": "en",
                    "WikipediaId": "Great Depression",
                    "WikipediaUrl": "https://en.wikipedia.org/wiki/Great_Depression",
                    "BingId": "d9364681-98ad-1a66-f869-a3f1c8ae8ef8"
                },
                {
                    "Name": "1929",
                    "Matches": [
                        {
                            "Text": "1929",
                            "Offset": 30,
                            "Length": 4
                        }
                    ],
                    "Type": "DateTime",
                    "SubType": "DateRange"
                },
                {
                    "Name": "By 1933",
                    "Matches": [
                        {
                            "Text": "By 1933",
                            "Offset": 36,
                            "Length": 7
                        }
                    ],
                    "Type": "DateTime",
                    "SubType": "DateRange"
                },
                {
                    "Name": "Gross domestic product",
                    "Matches": [
                        {
                            "Text": "GDP",
                            "Offset": 49,
                            "Length": 3
                        }
                    ],
                    "WikipediaLanguage": "en",
                    "WikipediaId": "Gross domestic product",
                    "WikipediaUrl": "https://en.wikipedia.org/wiki/Gross_domestic_product",
                    "BingId": "c859ed84-c0dd-e18f-394a-530cae5468a2"
                },
                {
                    "Name": "United States",
                    "Matches": [
                        {
                            "Text": "America",
                            "Offset": 56,
                            "Length": 7
                        }
                    ],
                    "WikipediaLanguage": "en",
                    "WikipediaId": "United States",
                    "WikipediaUrl": "https://en.wikipedia.org/wiki/United_States",
                    "BingId": "5232ed96-85b1-2edb-12c6-63e6c597a1de",
                    "Type": "Location"
                },
                {
                    "Name": "25",
                    "Matches": [
                        {
                            "Text": "25",
                            "Offset": 72,
                            "Length": 2
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Number"
                },
                {
                    "Name": "25%",
                    "Matches": [
                        {
                            "Text": "25%",
                            "Offset": 72,
                            "Length": 3
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Percentage"
                }
            ]
        }
    ],
    "Errors": []
}
```


## <a name="summary"></a>Özet

Bu makalede, kavramlar ve iş akışı Bilişsel hizmetler metin analizi kullanarak varlık bağlama öğrendiniz. Özet:

+ [Varlıkları API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634) seçili diller için kullanılabilir.
+ İstek gövdesindeki JSON belgelerini bir kimliği, metin ve dil kodu içerir.
+ POST isteği olduğu için bir `/entities` kişiselleştirilmiş kullanarak uç nokta, [erişim anahtarı ve uç nokta](text-analytics-how-to-access-key.md) aboneliğiniz için geçerli.
+ Bağlantılı varlık (güven puanları, kaydırmalar ve her web bağlantılarının kimliği belge dahil) içeren yanıt çıktı, herhangi bir uygulamada kullanılabilir

## <a name="see-also"></a>Ayrıca bkz. 

 [Metin Analizine genel bakış](../overview.md)  
 [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)</br>
 [Metin analizi ürün sayfası](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Metin analizi API'si](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634)
