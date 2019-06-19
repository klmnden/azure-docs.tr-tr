---
title: Varlık tanıma, metin analizi API'si ile kullanma
titleSuffix: Azure Cognitive Services
description: Metin analizi REST API'sini kullanarak varlıkları tanıması konusunda bilgi edinin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 04/16/2019
ms.author: aahi
ms.openlocfilehash: 881c829424124cca843beb7005306edd53be2673
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67203981"
---
# <a name="how-to-use-named-entity-recognition-in-text-analytics"></a>Adlandırılmış varlık tanıma metin analizi kullanma

[Adlandırılmış varlık tanıma API'si](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634) yapılandırılmamış metinleri alır ve her JSON belgesi için bağlantılarla birlikte disambiguated varlıkların listesini daha fazla bilgi için web üzerinde (Wikipedia ve Bing) döndürür. 

## <a name="entity-linking-and-named-entity-recognition"></a>Varlık bağlama ve adlandırılmış varlık tanıma

Metin analizi `entities` hem adlandırılmış varlık tanıma (NER) ve varlık bağlama uç noktası destekler.

### <a name="entity-linking"></a>Varlık Bağlama
Varlık bağlama metni (örneğin, "Mars" erişebilir veya Latin afetler, war olarak kullanılıp kullanılmadığını belirleme) bulunan bir varlığın kimliğini ayırt etmek üzere yeteneğidir. Bu işlem varlıkların bağlı - Wikipedia Bilgi Bankası'nda olarak kullanılan tanınan için temel bir bilgi gerektirir `entities` uç nokta metin analizi.

### <a name="named-entity-recognition-ner"></a>Adlandırılmış varlık tanıma (NER)
Adlandırılmış varlık tanıma (NER) metin farklı varlıklarda tanımlamak ve bunları önceden tanımlanmış sınıflara kategorilere ayırma olanağıdır. Varlıkları desteklenen sınıflarını aşağıda listelenmiştir.

Metin analizi, [sürüm 2.1](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634), varlık bağlama ve adlandırılmış varlık tanıma (NER) çeşitli diller için kullanılabilir. Bkz: [dil desteği](../language-support.md#sentiment-analysis-key-phrase-extraction-and-named-entity-recognition) makale daha fazla bilgi için. 

### <a name="language-support"></a>Dil desteği

Varlık bağlama çeşitli dillerde kullanarak, karşılık gelen Bilgi Bankası her bir dilin kullanılması gerekir. Metin analizi bağlama, varlık için yani tarafından desteklenen her dil `entities` uç noktası bu dilde karşılık gelen Wikipedia topluluğunuza bağlantı. Yapılar boyutunu diller arasında değişir olduğundan, varlık işlevselliği'nın geri çağırma bağlama de değişir bekleniyor.

## <a name="supported-types-for-named-entity-recognition"></a>Adlandırılmış varlık tanıma için desteklenen türler:

| Tür  | SubType | Örnek |
|:-----------   |:------------- |:---------|
| Kişi        | YOK\*         | "Jeff", "Bill Gates'le"     |
| Location      | YOK\*         | "Redmond, Washington", "Paris"  |
| Kuruluş  | YOK\*         | “Microsoft”   |
| Miktar      | Sayı        | "6", "altı"     | 
| Miktar      | Yüzde    | "%50", "yüzde elli"| 
| Miktar      | Sıra       | "2.", "ikinci"     | 
| Miktar      | Sayı Aralığı   | "4 ile 8 arası"     | 
| Miktar      | Yaş           | "90 gün eski", "30 yıl eski"    | 
| Miktar      | Para birimi      | “10,99 ABD Doları”     | 
| Miktar      | Boyut     | "10 mil", "40 cm"     | 
| Miktar      | Sıcaklık   | "32 derece"    |
| DateTime      | YOK\*         | “18:30 4 Şubat 2012”      | 
| DateTime      | Tarih          | “2 Mayıs 2017”, “02.05.2017”   | 
| DateTime      | Zaman          | "8 am", "8:00"  | 
| DateTime      | Tarih Aralığı     | “2 - 5 Mayıs Arası”    | 
| DateTime      | Saat Aralığı     | “18.00 ve 19.00 Arası”     | 
| DateTime      | Süre      | "1 dakika 45 saniye"   | 
| DateTime      | Ayarla           | "Her Salı"     | 
| DateTime      | TimeZone      |    | 
| URL'si           | YOK\*         | "https:\//www.bing.com"    |
| Email         | YOK\*         | "support@contoso.com" |

\* Giriş ve ayıklanan varlıklar bağlı olarak, bazı varlıklar atlasa `SubType`.  Listelenen tüm desteklenen varlık türleri yalnızca İngilizce, Basitleştirilmiş Çince, Fransızca, Almanca ve İspanyolca dil için kullanılabilir.



## <a name="preparation"></a>Hazırlık

JSON belgelerini şu biçimde olmalıdır: Metin, dil kimliği

Şu anda desteklenen diller için bkz. [bu liste](../text-analytics-supported-languages.md).

Belge boyutuna, belge başına altında 5.120 karakter uzunluğunda olmalıdır ve en fazla 1.000 olabilir koleksiyon başına öğe sayısı (Kimlikler). Koleksiyon, istek gövdesinde gönderilir. Aşağıdaki örnek, varlık bağlama sonuna kadar gönderdiğiniz içeriğin bir örnektir.

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
    
## <a name="step-1-structure-the-request"></a>1\. adım: Yapı isteği

İstek tanımıyla ilgili ayrıntılara [Metin Analizi API’sini çağırma](text-analytics-how-to-call-api.md) bölümünden erişilebilir. Kolaylık olması için aşağıdaki noktalar yeniden belirtilmektedir:

+ Bir **POST** isteği oluşturun. Bu istek için API belgelerini gözden geçirin: [Varlık bağlama API'si](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634)

+ Varlık ayıklama için HTTP uç noktası ayarlayın. `/entities` kaynağını içermelidir: `https://[your-region].api.cognitive.microsoft.com/text/analytics/v2.1/entities`

+ Metin Analizi işlemlerine yönelik erişim anahtarını dahil etmek için bir istek üst bilgisi ayarlayın. Daha fazla bilgi için bkz. [Uç noktaları ve erişim anahtarlarını bulma](text-analytics-how-to-access-key.md).

+ İstek gövdesinde, bu analiz için hazırladığınız JSON belgeleri koleksiyonunu sağlayın

> [!Tip]
> İsteği yapılandırmak ve hizmete GÖNDERMEK için [Postman](text-analytics-how-to-call-api.md) kullanın veya [belgelerdeki](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634) **API testi konsolu**’nu açın.

## <a name="step-2-post-the-request"></a>2\. adım: POST isteği

İstek alındığında analiz gerçekleştirilir. Hizmet, dakika başına ikinci ve 1000 istek başına en fazla 100 istek kabul eder. Her istek maksimum 1 MB olabilir.

Hizmetin durum bilgisi olmadığını unutmayın. Hesabınızda bir veri depolanmaz. Sonuçlar hemen yanıtta döndürülür.

## <a name="step-3-view-results"></a>3\. adım: Sonuçları görüntüleme

Tüm POST istekleri, kimlikler ve algılanan özelliklerle JSON tarafından biçimlendirilmiş bir yanıt döndürür.

Hemen çıktı döndürülür. Sonuçları, JSON kabul eden bir uygulamada akışa alabilir veya çıktıyı yerel sistemde bir dosyaya kaydedebilir, sonra da verileri sıralamanıza, aramanıza ve işlemenize olanak sağlayan bir uygulamaya içeri aktarabilirsiniz.

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

+ [Varlıkları API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634) seçili diller için kullanılabilir.
+ İstek gövdesindeki JSON belgelerini bir kimliği, metin ve dil kodu içerir.
+ POST isteği, aboneliğiniz için geçerli olan kişiselleştirilmiş bir [erişim anahtarı ve uç nokta](text-analytics-how-to-access-key.md) kullanılarak `/entities` uç noktasına yapılır.
+ Bağlantılı varlık (güven puanları, kaydırmalar ve her web bağlantılarının kimliği belge dahil) içeren yanıt çıktı, herhangi bir uygulamada kullanılabilir

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Metin Analizi API’si](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634)

* [Metin Analizine genel bakış](../overview.md)  
* [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)</br>
* [Metin Analizi ürün sayfası](//go.microsoft.com/fwlink/?LinkID=759712) 
