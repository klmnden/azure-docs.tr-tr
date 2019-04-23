---
title: Azure Bilişsel Hizmetler'in sunduğu metin analizi kullanarak yaklaşım analizi | Microsoft Docs
description: Metin Analizi REST API’sini kullanarak yaklaşımın nasıl algılanacağını öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: sample
ms.date: 02/26/2019
ms.author: aahi
ms.openlocfilehash: 0c42e7f8b1fffb9cf998f4cee8d30405a8df74a4
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60011308"
---
# <a name="example-how-to-detect-sentiment-with-text-analytics"></a>Örnek: Metin analiziyle duyguları algılamak nasıl

[Yaklaşım Analizi API’si](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9), metin girişini değerlendirir ve her belge için 0 (negatif) - 1 (pozitif) aralığında bir yaklaşım döndürür.

Bu yetenek; sosyal medya, müşteri incelemeleri ve tartışma forumlarında pozitif ve negatif yaklaşımları algılamak için kullanışlıdır. İçerik sizin tarafınızdan sağlanır; modeller ve eğitim verileri hizmet tarafından sağlanır.

Şu anda Yaklaşım Analizi, İngilizce, Almanca, İspanyolca ve Fransızca dillerini destekler. Diğer diller önizleme aşamasındadır. Daha fazla bilgi için bkz. [Desteklenen diller](../text-analytics-supported-languages.md).

> [!TIP]
> Metin analizi de sağlar Linux tabanlı bir Docker kapsayıcı görüntüsü yaklaşım analizi için böylece [yükleyin ve metin analizi kapsayıcı çalıştırın](text-analytics-how-to-install-containers.md) verilerinizi yakın.

## <a name="concepts"></a>Kavramlar

Metin Analizi, 0 ile 1 arasında bir yaklaşım puanı oluşturmak için makine öğrenmesi sınıflandırma algoritması kullanır. 1’e yakın puanlar pozitif yaklaşımı, 0’a yakın puanlar ise negatif yaklaşımı gösterir. Model, yaklaşım ilişkilendirmeleri ile kapsamlı bir metin gövdesi kullanılarak önceden eğitilir. Şu anda kendi eğitim verilerinizi sağlamanız mümkün değildir. Model, metin analizi sırasında metin işleme, sözcük türü analizi, sözcük yerleştirme ve sözcük ilişkilendirmeleri de dahil olmak üzere, birçok tekniğin birleşimini kullanır. Algoritma hakkında daha fazla bilgi için bkz. [Metin Analizi Tanıtımı](https://blogs.technet.microsoft.com/machinelearning/2015/04/08/introducing-text-analytics-in-the-azure-ml-marketplace/).

Metindeki belirli bir varlık için yaklaşımı ayıklamanın tersine yaklaşım analizi, belgenin tamamında gerçekleştirilir. Uygulamada, belgeler büyük bir metin bloğu yerine bir veya iki cümle içerdiğinde puanlama doğruluğu artış gösterme eğilimindedir. Nesnellik değerlendirmesi aşamasında model, belgenin tamamının nesnel mi olduğunu yoksa yaklaşım mı içerdiğini belirler. Çoğunlukla nesnel olan bir belge, yaklaşım algılama aşamasına ilerlemez ve daha fazla işlem olmadan 0,5 puanla sonuçlanır. İşlem hattında ilerleyen belgeler için bir sonraki aşama, belgede algılanan yaklaşım derecesine bağlı olarak 0,5 üzerinde veya altında bir puan oluşturur.

## <a name="preparation"></a>Hazırlık

Yaklaşım analizi, üzerinde çalışılacak küçük metin öbekleri sunduğunuzda daha yüksek kaliteli bir sonuç üretir. Bu, büyük metin öbekleri üzerinde daha iyi performans gösteren anahtar ifade ayıklamasının tersidir. Her iki işlemden de en iyi sonuçları elde etmek için girişleri uygun şekilde yeniden yapılandırın.

JSON belgelerini şu biçimde olmalıdır: Metin, dil kimliği

Belge boyutuna, belge başına altında 5.120 karakter uzunluğunda olmalıdır ve en fazla 1.000 olabilir koleksiyon başına öğe sayısı (Kimlikler). Koleksiyon, istek gövdesinde gönderilir. Aşağıda, yaklaşım analizi için gönderebileceğiniz bir içerik örneği verilmiştir.

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

## <a name="step-1-structure-the-request"></a>1. Adım: Yapı isteği

İstek tanımıyla ilgili ayrıntılara [Metin Analizi API’sini çağırma](text-analytics-how-to-call-api.md) bölümünden erişilebilir. Kolaylık olması için aşağıdaki noktalar yeniden belirtilmektedir:

+ Bir **POST** isteği oluşturun. Bu istek için API belgelerini gözden geçirin: [Yaklaşım analizi API'si](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9)

+ Yaklaşım analizi, Azure veya bir örneklenmiş bir metin analizi kaynak kullanarak HTTP uç noktasına ayarlayın [metin analizi kapsayıcı](text-analytics-how-to-install-containers.md). `/sentiment` kaynağını içermelidir: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/sentiment`

+ Metin Analizi işlemlerine yönelik erişim anahtarını dahil etmek için bir istek üst bilgisi ayarlayın. Daha fazla bilgi için bkz. [Uç noktaları ve erişim anahtarlarını bulma](text-analytics-how-to-access-key.md).

+ İstek gövdesinde, bu analiz için hazırladığınız JSON belgeleri koleksiyonunu sağlayın.

> [!Tip]
> İsteği yapılandırmak ve hizmete GÖNDERMEK için [Postman](text-analytics-how-to-call-api.md) kullanın veya [belgelerdeki](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9) **API testi konsolu**’nu açın.

## <a name="step-2-post-the-request"></a>2. Adım: POST isteği

İstek alındığında analiz gerçekleştirilir. Hizmet dakikada en fazla 100 istek kabul eder. Her istek maksimum 1 MB olabilir.

Hizmetin durum bilgisi olmadığını unutmayın. Hesabınızda bir veri depolanmaz. Sonuçlar hemen yanıtta döndürülür.


## <a name="step-3-view-results"></a>3. Adım: Sonuçları görüntüleme

Yaklaşım çözümleyicisi, 0 ile 1 aralığında bir puan atayarak metni baskın şekilde pozitif veya negatif olarak sınıflandırır. 0,5’e yakın değerler nötr veya belirsizdir. 0,5 puanı, nötr olma durumunu belirtir. Bir dizenin yaklaşım analizi yapılamadığında veya bir yaklaşımı olmadığında puan her zaman tam olarak 0,5’tir. Örneğin, İngilizce dil koduyla İspanyolca bir dize geçirirseniz puan 0,5 olur.

Hemen çıktı döndürülür. Sonuçları, JSON kabul eden bir uygulamada akışa alabilir veya çıktıyı yerel sistemde bir dosyaya kaydedebilir, sonra da verileri sıralamanıza, aramanıza ve işlemenize olanak sağlayan bir uygulamaya içeri aktarabilirsiniz.

Aşağıdaki örnekte, bu makaledeki belge koleksiyonu için yanıt gösterilmektedir.

```
{
    "documents": [
        {
            "score": 0.9999237060546875,
            "id": "1"
        },
        {
            "score": 0.0000540316104888916,
            "id": "2"
        },
        {
            "score": 0.99990355968475342,
            "id": "3"
        },
        {
            "score": 0.980544924736023,
            "id": "4"
        },
        {
            "score": 0.99996328353881836,
            "id": "5"
        }
    ],
    "errors": []
}
```

## <a name="summary"></a>Özet

Bu makalede, Bilişsel Hizmetler’de Metin Analizi’ni kullanarak yaklaşım analizi için kavramları ve iş akışını öğrendiniz. Özet:

+ [Yaklaşım analizi API’si](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9), seçili diller için kullanılabilir.
+ İstek gövdesindeki JSON belgelerini bir kimliği, metin ve dil kodu içerir.
+ POST isteği, aboneliğiniz için geçerli olan kişiselleştirilmiş bir [erişim anahtarı ve uç nokta](text-analytics-how-to-access-key.md) kullanılarak `/sentiment` uç noktasına yapılır.
+ Her belge kimliği için bir yaklaşım puanından oluşan yanıt çıktısı, Excel ve Power BI da dahil olmak üzere JSON kabul eden tüm uygulamalarda akışa alınabilir.

## <a name="see-also"></a>Ayrıca bkz. 

 [Metin Analizine genel bakış](../overview.md)  
 [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)</br>
 [Metin Analizi ürün sayfası](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Anahtar ifadeleri ayıklama](text-analytics-how-to-keyword-extraction.md)
