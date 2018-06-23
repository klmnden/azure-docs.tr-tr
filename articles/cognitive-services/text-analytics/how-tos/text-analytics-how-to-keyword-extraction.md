---
title: Metin analizi REST API (Azure'da Microsoft Bilişsel hizmetler) içinde nasıl yapılır anahtar tümcecik ayıklama | Microsoft Docs
description: Bu izlenecek yol öğreticide Azure üzerinde Microsoft Bilişsel Hizmetleri'ndeki metin Analytics REST API kullanarak anahtar tümcecikleri çıkarmayı.
services: cognitive-services
author: HeidiSteen
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 3/07/2018
ms.author: heidist
ms.openlocfilehash: 78b100e737242fa9f56e50275ef2038d8895349e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352234"
---
# <a name="how-to-extract-key-phrases-in-text-analytics"></a>Metin analizi anahtar tümcecikleri çıkarmayı

[Anahtar tümcecik ayıklama API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) yapılandırılmamış metin değerlendirir ve her JSON belgesi için anahtar tümcecikleri listesini döndürür. 

Bu özellik hızlı bir şekilde bir koleksiyonda belgelerin ana noktalarını tanımlamak gerektiğinde kullanışlı olur. Örneğin, "yemek Lezzetli harika personel vardı", hizmet döndürür ve ana Konuşmayı noktalarını verilen giriş metni: "yemek" ve "harika personeli".

Şu anda, İngilizce, Almanca, İspanyolca ve Japonca anahtar tümcecik ayıklama destekler. Diğer diller önizlemede. Daha fazla bilgi için bkz: [desteklenen diller](../text-analytics-supported-languages.md).

## <a name="preparation"></a>Hazırlama

Daha büyük öbek üzerinde çalışmak için metin verdiğinizde anahtar tümcecik ayıklama en iyi şekilde çalışır. Bu ters düşünceleri çözümlemesinden gerçekleştirir daha iyi metin küçük bloklarını. Her iki işlemlerinden en iyi sonuçları almak için girişleri uygun şekilde yeniden yapılandırma göz önünde bulundurun.

JSON belgeleri şu biçimde olmalıdır: kimliği, metin, dil

Belge boyutu, belge başına 5. 000'in altında karakter uzunluğunda olmalıdır ve en çok 1.000 olabilir koleksiyon başına öğeleri (Kimlikler). Koleksiyon istek gövdesinde gönderilir. Aşağıdaki örnekte bir çizimi için anahtar tümcecik ayıklama gönderme içerik var.

```
    {
        "documents": [
            {
                "language": "en",
                "id": "1",
                "text": "We love this trail and make the trip every year. The views are breathtaking and well worth the hike!"
            },
            {
                "language": "en",
                "id": "2",
                "text": "Poorly marked trails! I thought we were goners. Worst hike ever."
            },
            {
                "language": "en",
                "id": "3",
                "text": "Everyone in my family liked the trail but thought it was too challenging for the less athletic among us. Not necessarily recommended for small children."
            },
            {
                "language": "en",
                "id": "4",
                "text": "It was foggy so we missed the spectacular views, but the trail was ok. Worth checking out if you are in the area."
            },                
            {
                "language": "en",
                "id": "5",
                "text": "This is my favorite trail. It has beautiful views and many places to stop and rest"
            }
        ]
    }
```    
    
## <a name="step-1-structure-the-request"></a>1. adım: isteği yapısı

İstek tanımının ayrıntıları bulunabilir [metin Analytics API'sini çağırmak nasıl](text-analytics-how-to-call-api.md). Kolaylık olması için aşağıdaki noktaları açıklandı:

+ Oluşturma bir **POST** isteği. Bu istek için API belgelerini gözden geçirin: [anahtarı tümcecikleri API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6)

+ Anahtar tümcecik ayıklama için HTTP uç noktası olarak ayarlayın. İçermesi gerekir `/keyphrases` kaynak: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases`

+ Metin analizi işlemleri için erişim anahtarı eklemek için bir istek üstbilgisini ayarlayın. Daha fazla bilgi için bkz: [uç noktalarını bulma ve erişim anahtarları](text-analytics-how-to-access-key.md).

+ İstek gövdesinde Bu çözümleme için hazırlanmış JSON belgeleri koleksiyonu sağlayın

> [!Tip]
> Kullanım [Postman](text-analytics-how-to-call-api.md) veya açın **API sınama Konsolu** içinde [belgelerine](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) bir istek yapısı ve hizmete gönderme.

## <a name="step-2-post-the-request"></a>2. adım: isteği gönderme

Çözümleme isteği alındığında gerçekleştirilir. Hizmet dakika başına en fazla 100 isteklerini kabul eder. Her istek en fazla 1 MB olabilir.

Hizmet durum bilgisi olmayan bir geri çağırma. Hiçbir veri hesabınızda depolanır. Sonuçlar hemen yanıt olarak döndürülür.

## <a name="step-3-view-results"></a>3. adım: Sonuçları görüntüleme

Tüm POST isteklerinden JSON yanıt kimlikleri biçimlendirilmiş ve özellikleri algılandı döndürür.

Çıktı hemen döndürülür. JSON kabul eden bir uygulama sonuçları akış veya yerel sistemindeki bir dosyaya çıkış kaydedin ve sıralama, arama ve verileri işlemek izin veren bir uygulamayı alma.

Anahtar tümcecik ayıklama çıktısı örneği sonraki gösterilir:

```
    "documents": [
        {
            "keyPhrases": [
                "year",
                "trail",
                "trip",
                "views"
            ],
            "id": "1"
        },
        {
            "keyPhrases": [
                "marked trails",
                "Worst hike",
                "goners"
            ],
            "id": "2"
        },
        {
            "keyPhrases": [
                "trail",
                "small children",
                "family"
            ],
            "id": "3"
        },
        {
            "keyPhrases": [
                "spectacular views",
                "trail",
                "area"
            ],
            "id": "4"
        },
        {
            "keyPhrases": [
                "places",
                "beautiful views",
                "favorite trail"
            ],
            "id": "5"
        }
```

Belirtildiği gibi Çözümleyicisi'ni bulur ve gerekli olmayan sözcükler atar ve tek terimleri veya konu veya cümlenin bir nesne olarak görünen tümcecikleri tutar. 

## <a name="summary"></a>Özet

Bu makalede, kavramlar ve iş akışı için metin analizi Bilişsel Hizmetleri'ni kullanarak anahtar tümcecik ayıklama öğrendiniz. Özet:

+ [Anahtar tümcecik ayıklama API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) Seçilen diller için kullanılabilir.
+ İstek gövdesinde JSON belgeleri bir kimliği, metin ve dil kodu içerir.
+ POST isteğini olduğu için bir `/keyphrases` kişiselleştirilmiş bir kullanarak uç noktasını, [erişim anahtarı ve bir uç nokta](text-analytics-how-to-access-key.md) , aboneliğiniz için geçerli.
+ Anahtar sözcükler ve tümcecikleri her belge kimliği oluşur, yanıt çıktısı JSON, Excel ve Power BI birkaçıdır dahil olmak üzere kabul eden herhangi bir uygulama için akışı.

## <a name="see-also"></a>Ayrıca bkz. 

 [Metin analizi genel bakış](../overview.md)  
 [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)</br>
 [Metin analizi ürün sayfası](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Metin analizi API](//westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6)
