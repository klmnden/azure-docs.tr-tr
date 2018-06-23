---
title: Öngörüler için Bing görüntüyü karşıya yükleme | Microsoft Docs
description: Görüntüyü karşıya yükleme ve görüntü öngörüleri almak için Bing görüntü arama API'sini kullanan konsol uygulaması.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 12/07/2017
ms.author: v-gedod
ms.openlocfilehash: f0bf32a9951527a072fffe464f6b5f50d0f237a2
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351460"
---
# <a name="tutorial-bing-image-upload-for-insights"></a>Öğretici: Bing görüntü öngörüleri için karşıya yükleme

Bing görüntü arama API sağlayan bir `POST` imaj ve görüntüyü uygun bilgi aramak yüklemek için seçeneği. Bu C# konsol uygulaması görüntüyü görüntü hakkında bilgi almak için görüntü arama uç noktası kullanarak yükler.
Sonuçlar kısaca, aşağıdaki gibi JSON nesnelerinin şunlardır:

![[JSON sonuçları]](media/cognitive-services-bing-images-api/jsonResult.jpg)

Bu öğreticide, aşağıdaki işlemlerin nasıl yapılacağı açıklanmaktadır:

> [!div class="checklist"]
> * Görüntü arama kullanmasını `/details` uç bir `POST` isteği
> * İstek için üstbilgiler belirtin
> * URL parametreleri sonuçları belirtmek için kullanın
> * Görüntü verilerini karşıya yükleme ve Gönder `POST` isteği
> * JSON sonuçlarını konsola yazdırma

## <a name="app-components"></a>Uygulama bileşenleri

Eğitmen uygulama üç bölümleri içerir:

> [!div class="checklist"]
> * Bing görüntü arama uç noktasını ve gerekli üst bilgileri belirtmek için uç nokta yapılandırması
> * Görüntü dosyasını karşıya yükleme için `POST` uç nokta isteği
> * Sağlayıcıdan döndürülen ayrıntıları JSON sonuçları ayrıştırma `POST` isteği

## <a name="scenario-overview"></a>Senaryoya genel bakış
Vardır [üç görüntü arama uç noktaları](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/image-search-endpoint). `/details` Endpoint kullanabileceğiniz bir `POST` görüntü verilerini istek gövdesinde istekle.
```
https://api.cognitive.microsoft.com/bing/v7.0/images/details
```
`modules` URL parametresi aşağıdaki `/details?` sonuçları ne tür bir ayrıntıları içeren belirtir:
* `modules=All`
* `modules=RecognizedEntities` (kişiler veya yerler görüntüde görünür)

Belirtin `modules=All` içinde `POST` aşağıdaki içerir JSON metni almak için isteği:
* `bestRepresentativeQuery` -görüntüleri karşıya yüklenen görüntüsüne benzer döndüren bir Bing sorgu
* `detectedObjects` bir sınırlayıcı kutu veya etkin noktalar görüntüdeki gibi
* `image` Meta veriler
* `imageInsightsToken` -token için bir sonraki `GET` alır isteği `RecognizedEntities` (kişiler veya yerler görüntüde görünür) 
* `imageTags`
* `pagesIncluding` -Web görüntü içeren sayfaları
* `relatedSearches`
* `visuallySimilarImages`

Belirtin `modules=RecognizedEntities` içinde `POST` yalnızca alma isteği `imageInsightsToken`, kullanılan bir sonraki `GET` isteği. Kişileri tanımlayan veya görüntüde görünür yerleştirir.

## <a name="webclient-and-headers-for-the-post-request"></a>WebClient ve üstbilgileri POST isteği
Oluşturma bir `WebClient` nesne ve üstbilgileri ayarlayın. Bing arama API için tüm istekleri gerektiren bir `Ocp-Apim-Subscription-Key`. A `POST` görüntüyü karşıya yükleme isteği de belirtmelisiniz `ContentType: multipart/form-data`.

```
            WebClient client = new WebClient();
            client.Headers["Ocp-Apim-Subscription-Key"] = accessKey;
            client.Headers["ContentType"] = "multipart/form-data"; 
```

## <a name="upload-the-image-and-get-results"></a>Görüntüyü karşıya yükleme ve sonuçları alın

`WebClient` Sınıfı içerir `UpLoadFile` için veri biçimleri yöntemi `POST` isteği. Biçimlendirir `RequestStream` ve çağrıları `HttpWebRequest`, karmaşıklık çok önleme.
Çağrı `WebClient.UpLoadFile` ile `/details` uç noktası ve resim dosyasını karşıya yükleyin. Yanıt için JSON kolayca dönüştürülür ikili verilerdir. 

JSON metnini örneği başlatılamıyor kullanmasını `SearchResult` yapısı (bkz [uygulama kaynak koduna](tutorial-image-post-source.md) bağlamı için).
```        
         const string uriBase = "https://api.cognitive.microsoft.com/bing/v7.0/images/details";

        // The image to upload. Replace with your file and path.
        const string imageFile = "ansel-adams-tetons-snake-river.jpg";
            
        byte[] resp = client.UploadFile(uriBase + "?modules=All", imageFile);
        var json = System.Text.Encoding.Default.GetString(resp);

        // Create result object for return
        var searchResult = new SearchResult()
        {
            jsonResult = json,
            relevantHeaders = new Dictionary<String, String>()
        };
```

## <a name="print-the-results"></a>Sonuçları Yazdır
Kod kalan JSON sonuç ayrıştırır ve konsola yazdırır.

```
        /// <summary>
        /// Formats the given JSON string by adding line breaks and indents.
        /// </summary>
        /// <param name="json">The raw JSON string to format.</param>
        /// <returns>The formatted JSON string.</returns>
        static string JsonPrettyPrint(string json)
        {
            if (string.IsNullOrEmpty(json))
                return string.Empty;

            json = json.Replace(Environment.NewLine, "").Replace("\t", "");

            StringBuilder sb = new StringBuilder();
            bool quote = false;
            bool ignore = false;
            char last = ' ';
            int offset = 0;
            int indentLength = 2;

            foreach (char ch in json)
            {
                switch (ch)
                {
                    case '"':
                        if (!ignore) quote = !quote;
                        break;
                    case '\\':
                        if (quote && last != '\\') ignore = true;
                        break;
                }

                if (quote)
                {
                    sb.Append(ch);
                    if (last == '\\' && ignore) ignore = false;
                }
                else
                {
                    switch (ch)
                    {
                        case '{':
                        case '[':
                            sb.Append(ch);
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', ++offset * indentLength));
                            break;
                        case '}':
                        case ']':
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', --offset * indentLength));
                            sb.Append(ch);
                            break;
                        case ',':
                            sb.Append(ch);
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', offset * indentLength));
                            break;
                        case ':':
                            sb.Append(ch);
                            sb.Append(' ');
                            break;
                        default:
                            if (quote || ch != ' ') sb.Append(ch);
                            break;
                    }
                }
                last = ch;
            }

            return sb.ToString().Trim();
        }
```
## <a name="get-request-using-the-imageinsightstoken"></a>ImageInsightsToken kullanarak isteği alma
Kullanılacak `ImageInsightsToken` sonuçlarıyla döndürülen bir `POST`, oluşturma bir `GET` isteği aşağıdaki gibi:
```
https://api.cognitive.microsoft.com/bing/v7.0/images/details?InsightsToken="bcid_A2C4BB81AA2C9EF8E049C5933C546449*ccid_osS7gaos*mid_BF7CC4FC4A882A3C3D56E644685BFF7B8BACEAF2
```
Kişisel kişi ya da görüntü yerlerde varsa, bu istek bunlarla ilgili bilgileri döndürür.
[Quickstarts](https://docs.microsoft.com/azure/cognitive-services/bing-image-search) çok sayıda kod örnekleri içerir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing görüntü arama API Başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)

