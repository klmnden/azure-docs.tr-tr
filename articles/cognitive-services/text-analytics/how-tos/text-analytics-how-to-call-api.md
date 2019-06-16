---
title: Metin Analizi API’sini çağırma
titlesuffix: Azure Cognitive Services
description: Metin analizi REST API'nin nasıl çağrılacağını öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 02/26/2019
ms.author: aahi
ms.openlocfilehash: 720a6c57d4f1a6079f78244559a25018349bd378
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60831427"
---
# <a name="how-to-call-the-text-analytics-rest-api"></a>Metin analizi REST API'nin nasıl çağrılacağını

Çağrılar **metin analizi API'si** , herhangi bir dilde formüle HTTP POST/GET çağrıları. Bu makalede REST kullanırız ve [Postman](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop) temel kavramları göstermek için.

Her isteğin erişim anahtarınız ve bir HTTP uç noktası eklemeniz gerekir. Yukarı kayıt sırasında seçtiğiniz bölgeye, hizmet URL'sini ve istekte kullanılan bir kaynak uç noktasını belirtir: `sentiment`, `keyphrases`, `languages`, ve `entities`. 

Metin analizi yönetmek için hiçbir veri varlıklarını zorlaştırıyorsa durum bilgisiz olduğunu hatırlayın. Metninizi alındığında analiz karşıya yüklendikten ve sonuçları hemen çağıran uygulamaya döndürülür.

> [!Tip]
> API nasıl çalıştığını görmek tek seferlik çağrıları için yerleşik POST istekleri gönderebilirsiniz **API sınama Konsolu**, tüm kullanılabilir [API belge sayfasında](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c6). Kurulumu yok ve tek gereksinim bir erişim anahtarı ve JSON belgeleri istek yapıştırmak üzeresiniz. 

## <a name="prerequisites"></a>Önkoşullar

Olmalıdır bir [Bilişsel hizmetler API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) metin analizi API'si ile ve [uç noktası ve erişim anahtarı](text-analytics-how-to-access-key.md) , oluşturulan, Bilişsel hizmetler için kaydolduğunuzda. 

<a name="json-schema"></a>

## <a name="json-schema-definition"></a>JSON şema tanımı

Giriş JSON içinde yapılandırılmamış ham metin olmalıdır. XML desteklenmiyor. Şema, aşağıdaki listede açıklanmıştır öğelerin oluşan basit bir işlemdir. 

Şu anda tüm metin analizi işlemler için aynı belgeleri gönderebilirsiniz: metninizdeki yaklaşımları, anahtar ifade, dil algılama ve varlık kimliği. (Şema için her bir analiz gelecekte farklı olabilir.)

| Öğe | Geçerli değerler | Gerekli mi? | Kullanım |
|---------|--------------|-----------|-------|
|`id` |Dize veri türünde, ancak uygulamada belge kimlikleri tamsayılar olma eğilimindedir. | Gerekli | Sistem, yapı çıktı sağladığınız kimlikleri kullanır. Dil kodları, anahtar ifadeleri ve duyarlılık puanlarını her kimliği istekteki proje Kimliğiyle için oluşturulur.|
|`text` | Yapılandırılmamış ham metni, en çok 5.120 karakter. | Gerekli | Dil algılama için metin herhangi bir dille ifade edilebilir. Yaklaşım analizi, anahtar ifade ayıklama ve varlık kimliği için metin olmalıdır bir [dil desteklenen](../text-analytics-supported-languages.md). |
|`language` | 2 karakterli [ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) için kod bir [dil desteklenmiyor](../text-analytics-supported-languages.md) | Değişir | Yaklaşım analizi, anahtar ifade ayıklama ve varlık bağlama için gerekli; dil algılama için isteğe bağlı. Hata yoktur, hariç tutarsanız, ancak analiz olmadan zayıflar. Dil kodu karşılık gelmelidir `text` size sağlar. |

Sınırları hakkında daha fazla bilgi için bkz. [metin Analizi'ne genel bakış > veri sınırları](../overview.md#data-limits). 

## <a name="set-up-a-request-in-postman"></a>Postman istekte ayarlama

Hizmet kabul boyutu 1 MB'a kadar istek. Postman'ı (veya başka bir Web API'si test aracı) kullanıyorsanız, kullanmak istediğiniz kaynak eklemek için uç noktayı kurmak ayarlayın ve istek üst erişim anahtarı sağlayın. Her işlem, ilgili kaynak için uç nokta ekleme gerektirir. 

1. Postman içinde:

   + Seçin **Post** istek türü olarak.
   + Portal sayfasında kopyalar endpoint yapıştırın.
   + Bir kaynak ekleyin.

   Kaynak uç noktası (bölgenizi değişebilir) aşağıdaki gibi şunlardır:

   + `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/sentiment`
   + `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/keyPhrases`
   + `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/languages`
   + `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/entities`

2. Üç istek üst bilgilerini ayarla:

   + `Ocp-Apim-Subscription-Key`: erişim anahtarınızı Azure portalından alınan.
   + `Content-Type`: uygulama/json.
   + `Accept`: uygulama/json.

   İsteğiniz aşağıdaki ekran görüntüsüne benzer görünmelidir varsayılarak bir **/keyPhrases** kaynak.

   ![Uç nokta ve üst bilgileri ile istek ekran görüntüsü](../media/postman-request-keyphrase-1.png)

4. Tıklayın **gövdesi** ve **ham** biçimi.

   ![İstek gövdesi ayarları ekran görüntüsü](../media/postman-request-body-raw.png)

5. İstenen analiz için geçerli bir biçimde bazı JSON belgeleri olarak yapıştırın. Belirli bir çözümleme hakkında daha fazla bilgi için aşağıdaki konulara bakın:

  + [Dil algılama](text-analytics-how-to-language-detection.md)  
  + [Anahtar ifade ayıklama](text-analytics-how-to-keyword-extraction.md)  
  + [Yaklaşım analizi](text-analytics-how-to-sentiment-analysis.md)  
  + [Varlık tanıma](text-analytics-how-to-entity-linking.md)  


6. Tıklayın **Gönder** isteği göndermek için. Dakika başına en fazla 100 istek gönderebilirsiniz. 

   Postman içinde bir öğe için istekte sağlanan her bir belge kimliği ile tek bir JSON belgesi olarak yanıt aşağı, sonraki penceresi görüntülenir.

## <a name="see-also"></a>Ayrıca bkz. 

 [Metin Analizine Genel Bakış](../overview.md)  
 [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dili Algıla](text-analytics-how-to-language-detection.md)
