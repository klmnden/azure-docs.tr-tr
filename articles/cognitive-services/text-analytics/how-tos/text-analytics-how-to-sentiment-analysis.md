---
title: Metin analizi REST API (Azure'da Microsoft Bilişsel hizmetler) nasıl yapılır düşünceleri analizi | Microsoft Docs
description: Bu izlenecek yol öğreticide Azure üzerinde Microsoft Bilişsel Hizmetleri'ndeki metin Analytics REST API kullanarak düşünceleri algılamak nasıl.
services: cognitive-services
author: HeidiSteen
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 12/11/2017
ms.author: heidist
ms.openlocfilehash: 7ffd8bbe47409b459fdd308cd8d670d32f56649b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352235"
---
# <a name="how-to-detect-sentiment-in-text-analytics"></a>Metin analizi düşünceleri algılamak nasıl

[Düşünceleri analiz API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9) metin girişi değerlendirir ve 1 (olumlu) 0 (negatif) arasında her belge için bir düşünceleri puan döndürür.

Bu yetenek, sosyal medya, müşteri incelemelerini ve tartışma forumları pozitif ve negatif düşünceleri algılamak için kullanışlıdır. İçeriği sizin tarafınızdan sağlanır; modelleri ve eğitim veri hizmeti tarafından sağlanır.

Şu anda, İngilizce, Almanca, İspanyolca ve Fransızca düşünceleri analiz destekler. Diğer diller önizlemede. Daha fazla bilgi için bkz: [desteklenen diller](../text-analytics-supported-languages.md).

## <a name="concepts"></a>Kavramlar

Metin analizi, machine learning sınıflandırma algoritmasıdır düşünceleri puanı 0 ile 1 arasında oluşturmak için kullanılır. Negatif düşünceleri yakın 0 puanları belirtmek sırada yakın 1 puanları pozitif düşünceleri gösterir. Model, düşünceleri ilişkilendirmeleri metinle kapsamlı bir gövde ile pretrained. Şu anda, kendi eğitim verilerini sağlamak mümkün değil. Model metin işleme, konuşma bölümü analiz, word yerleştirme ve word ilişkilendirmeleri de dahil olmak üzere metin Çözümleme sırasında teknikler birleşimini kullanır. Algoritma hakkında daha fazla bilgi için bkz: [Tanıtımı metin analizi](https://blogs.technet.microsoft.com/machinelearning/2015/04/08/introducing-text-analytics-in-the-azure-ml-marketplace/).

Düşünceleri analiz düşünceleri metin belirli bir varlık için ayıklama aksine tüm belgeyi gerçekleştirilir. Uygulamada, belgeleri büyük bir metin bloğunu yerine bir veya iki cümle içerdiğinde geliştirmek için doğruluğu Puanlama eğilimi yoktur. Objectivity değerlendirme aşamasında, bir belgenin tamamına amacı veya düşünceleri içeren model belirler. Bir belge çoğunlukla hedefi yoksa başka bir işleme ile.50 bir puan sonuçta düşünceleri algılama tümcecik ilerleme değil. Ardışık düzeninde etmeden belgeler için sıradaki aşama bir puan belgede algılanan düşünceleri derecesini bağlı olarak.50 altında veya üstünde oluşturur.

## <a name="preparation"></a>Hazırlama

Üzerinde çalışılacak metin küçük parçalarını verdiğinizde düşünceleri analiz daha yüksek kaliteli sonuç üretir. Bu ters anahtar tümcecik ayıklama, gerçekleştirir daha iyi metin büyük bloklarını. Her iki işlemlerinden en iyi sonuçları almak için girişleri uygun şekilde yeniden yapılandırma göz önünde bulundurun.

JSON belgeleri şu biçimde olmalıdır: kimliği, metin, dil

Belge boyutu, belge başına 5. 000'in altında karakter uzunluğunda olmalıdır ve en çok 1.000 olabilir koleksiyon başına öğeleri (Kimlikler). Koleksiyon istek gövdesinde gönderilir. Düşünceleri analize gönderme içeriğine bir örnek verilmiştir.

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

+ Oluşturma bir **POST** isteği. Bu istek için API belgelerini gözden geçirin: [düşünceleri analiz API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9)

+ Anahtar tümcecik ayıklama için HTTP uç noktası olarak ayarlayın. İçermesi gerekir `/sentiment` kaynak: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment`

+ Metin analizi işlemleri için erişim anahtarı eklemek için bir istek üstbilgisini ayarlayın. Daha fazla bilgi için bkz: [uç noktalarını bulma ve erişim anahtarları](text-analytics-how-to-access-key.md).

+ İstek gövdesinde Bu çözümleme için hazırlanmış JSON belgeleri koleksiyonu sağlar.

> [!Tip]
> Kullanım [Postman](text-analytics-how-to-call-api.md) veya açın **API sınama Konsolu** içinde [belgelerine](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9) istek yapısı ve hizmete gönderin.

## <a name="step-2-post-the-request"></a>2. adım: isteği gönderme

Çözümleme isteği alındığında gerçekleştirilir. Hizmet dakika başına en fazla 100 isteklerini kabul eder. Her istek en fazla 1 MB olabilir.

Hizmet durum bilgisi olmayan bir geri çağırma. Hiçbir veri hesabınızda depolanır. Sonuçlar hemen yanıt olarak döndürülür.


## <a name="step-3-view-results"></a>3. adım: Sonuçları görüntüleme

Düşünceleri Çözümleyicisi metin daha pozitif veya negatif bir puan 0 ve 1 aralığında atama olarak sınıflandırır. Değer 0,5 yakın nötr ya da belirsiz. Bir puan 0,5 nötr olma durumunu gösterir. Bir dize için düşünceleri çözümlenemiyor veya hiçbir düşünceleri sahip olduğunda, puan her zaman olduğu 0,5 tam olarak. Örneğin, İngilizce dil kodu ile İspanyolca dizesindeki geçirirseniz, puan 0,5 ' dir.

Çıktı hemen döndürülür. JSON kabul eden bir uygulama sonuçları akış veya yerel sistemindeki bir dosyaya çıkış kaydedin ve sıralama, arama ve verileri işlemek izin veren bir uygulamayı alma.

Aşağıdaki örnek yanıt belge koleksiyonu için bu makaledeki gösterir.

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

Bu makalede, kavramlar ve metin analizi Bilişsel Hizmetleri'ni kullanarak düşünceleri analiz için iş akışı öğrendiniz. Özet:

+ [Düşünceleri analiz API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9) Seçilen diller için kullanılabilir.
+ İstek gövdesinde JSON belgeleri bir kimliği, metin ve dil kodu içerir.
+ POST isteğini olduğu için bir `/sentiment` kişiselleştirilmiş bir kullanarak uç noktasını, [erişim anahtarı ve bir uç nokta](text-analytics-how-to-access-key.md) , aboneliğiniz için geçerli.
+ JSON, Excel ve Power BI birkaçıdır dahil olmak üzere kabul eden herhangi bir uygulama için bir düşünceleri puanı her belge kimliği oluşur, yanıt çıktısı gönderilebilen.

## <a name="see-also"></a>Ayrıca bkz. 

 [Metin analizi genel bakış](../overview.md)  
 [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)</br>
 [Metin analizi ürün sayfası](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Anahtar tümcecikleri Ayıkla](text-analytics-how-to-keyword-extraction.md)
