---
title: "Hızlı Başlangıç: Bing görsel arama REST API'sini kullanarak görüntü Öngörüler elde edin veC#"
titleSuffix: Azure Cognitive Services
description: Bing görsel arama API'sine bir görüntüyü karşıya yükleme ve ilgili Öngörüler alma hakkında bilgi edinin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 3/28/2019
ms.author: scottwhi
ms.openlocfilehash: d2f5e87bd6c6780e8504abe1753e90eca5db763a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60507777"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-c"></a>Hızlı Başlangıç: Bing görsel arama REST API'sini kullanarak görüntü Öngörüler elde edin veC#

Bu hızlı başlangıçta, Bing görsel arama API'sine bir görüntüyü karşıya yükleme ve döndürdüğü öngörülerini görüntülemek için nasıl gösterir.

## <a name="prerequisites"></a>Önkoşullar

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)’nin herhangi bir sürümü.
* [Json.NET framework](https://www.newtonsoft.com/json), bir NuGet paketi olarak kullanılabilir.
* Linux/MacOS kullanıyorsanız, bu uygulamayı kullanarak çalıştırabilirsiniz [Mono](https://www.mono-project.com/).

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Proje oluşturma ve başlatma

1. Visual Studio'da BingSearchApisQuickStart adlı yeni bir konsol çözümü oluşturun. Aşağıdaki ad alanlarını ana kod dosyasına ekleyin:

    ```csharp
    using System;
    using System.Text;
    using System.Net;
    using System.IO;
    using System.Collections.Generic;
    ```

2. Karşıya yüklemek istediğiniz görüntüye abonelik anahtarınızı, uç noktası ve yol değişkenleri ekleyin:

    ```csharp
        const string accessKey = "<my_subscription_key>";
        const string uriBase = "https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch";
        static string imagePath = @"<path_to_image>";
    ```

3. Adlı bir yöntem oluşturma `GetImageFileName()` görüntünüzü olan yolu almak için:
    
    ```csharp
    static string GetImageFileName(string path)
            {
                return new FileInfo(path).Name;
            }
    ```

4. Görüntünün ikili verileri almak için bir yöntem oluşturun:

    ```csharp
    static byte[] GetImageBinary(string path)
    {
        return File.ReadAllBytes(path);
    }
    ```

## <a name="build-the-form-data"></a>Form verileri oluşturun

Yerel bir görüntüyü karşıya yüklemek için öncelikle API'sine göndermek için form verileri oluşturun. Form verileri içermelidir `Content-Disposition` üst bilgi, kendi `name` parametresi ayarlanması gerekir "Görüntü" ve `filename` herhangi bir dize parametresi ayarlanabilir. Form içeriğini görüntünün ikili verileri içerir. Karşıya yüklediğiniz en yüksek görüntü boyutu 1 MB'dir.

    ```
    --boundary_1234-abcd
    Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"
    
    ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...
    
    --boundary_1234-abcd--
    ```

1. Form POST verilerin biçimlendirilmesi için sınır dizeleri ekleyin. Veriler için başlangıç ve bitiş yeni satır karakterleri sınır dizeleri belirler:

    ```csharp
    // Boundary strings for form data in body of POST.
    const string CRLF = "\r\n";
    static string BoundaryTemplate = "batch_{0}";
    static string StartBoundaryTemplate = "--{0}";
    static string EndBoundaryTemplate = "--{0}--";
    ```

2. Form parametreleri eklemek için aşağıdaki değişkenleri kullanın:

    ```csharp
    const string CONTENT_TYPE_HEADER_PARAMS = "multipart/form-data; boundary={0}";
    const string POST_BODY_DISPOSITION_HEADER = "Content-Disposition: form-data; name=\"image\"; filename=\"{0}\"" + CRLF +CRLF;
    ```

3. Adlı bir işlev oluşturma `BuildFormDataStart()` başlangıç sınır dizeleri ve görüntü yolu kullanarak form verileri oluşturmak için:
    
    ```csharp
        static string BuildFormDataStart(string boundary, string filename)
        {
            var startBoundary = string.Format(StartBoundaryTemplate, boundary);

            var requestBody = startBoundary + CRLF;
            requestBody += string.Format(POST_BODY_DISPOSITION_HEADER, filename);

            return requestBody;
        }
    ```

4. Adlı bir işlev oluşturma `BuildFormDataEnd()` sınır dizeleri kullanarak form verilerinin ucu oluşturmak için:
    
    ```csharp
        static string BuildFormDataEnd(string boundary)
        {
            return CRLF + CRLF + string.Format(EndBoundaryTemplate, boundary) + CRLF;
        }
    ```

## <a name="call-the-bing-visual-search-api"></a>Bing görsel arama API'si çağırma

1. Bing görsel arama uç noktasını çağırmak ve JSON yanıtını döndürmek için bir işlev oluşturun. İşlevi başlangıç ve bitiş görüntü verilerini içeren bir bayt dizisi form verileri alır ve bir `contentType` değeri.

2. Kullanım bir `WebRequest` URI, contentType değeri ve üst bilgileri depolamak için.  

3. Kullanım `request.GetRequestStream()` , form ve resim veri yazma ve ardından yanıt alın. İşlevinizi aşağıdakine benzer olmalıdır:
        
    ```csharp
        static string BingImageSearch(string startFormData, string endFormData, byte[] image, string contentTypeValue)
        {
            WebRequest request = HttpWebRequest.Create(uriBase);
            request.ContentType = contentTypeValue;
            request.Headers["Ocp-Apim-Subscription-Key"] = accessKey;
            request.Method = "POST";

            // Writes the boundary and Content-Disposition header, then writes
            // the image binary, and finishes by writing the closing boundary.
            using (Stream requestStream = request.GetRequestStream())
            {
                StreamWriter writer = new StreamWriter(requestStream);
                writer.Write(startFormData);
                writer.Flush();
                requestStream.Write(image, 0, image.Length);
                writer.Write(endFormData);
                writer.Flush();
                writer.Close();
            }

            HttpWebResponse response = (HttpWebResponse)request.GetResponseAsync().Result;
            string json = new StreamReader(response.GetResponseStream()).ReadToEnd();

            return json;
        }
    ```

## <a name="create-the-main-method"></a>Main yöntemi oluşturma

1. İçinde `Main` yöntemi, uygulamanızın dosya adı ve ikili veri görüntünüzün Al:

    ```csharp
    var filename = GetImageFileName(imagePath);
    var imageBinary = GetImageBinary(imagePath);
    ```

2. POST gövdesini sınır biçimlendirme olarak ayarlayın. Ardından çağırın `startFormData()` ve `endFormData` form verilerini oluşturmak için:

    ```csharp
    // Set up POST body.
    var boundary = string.Format(BoundaryTemplate, Guid.NewGuid());
    var startFormData = BuildFormDataStart(boundary, filename);
    var endFormData = BuildFormDataEnd(boundary);
    ```

3. Oluşturma `ContentType` biçimlendirme tarafından değeri `CONTENT_TYPE_HEADER_PARAMS` ve form veri sınırı:

    ```csharp
    var contentTypeHdrValue = string.Format(CONTENT_TYPE_HEADER_PARAMS, boundary);
    ```

4. API yanıtı çağırarak alma `BingImageSearch()` ve yanıt yazdırın:

    ```csharp
    var json = BingImageSearch(startFormData, endFormData, imageBinary, contentTypeHdrValue);
    Console.WriteLine(json);
    Console.WriteLine("enter any key to continue");
    Console.readKey();
    ```

## <a name="using-httpclient"></a>HttpClient kullanma

Kullanırsanız `HttpClient`, kullanabileceğiniz `MultipartFormDataContent` form verilerini oluşturmak için sınıfı. Yalnızca önceki örnekte karşılık gelen yöntemlere değiştirilecek kod aşağıdaki bölümleri kullanın.

Değiştirin `Main` bu kodla yöntemi:

```csharp
        static void Main()
        {
            try
            {
                Console.OutputEncoding = System.Text.Encoding.UTF8;

                if (accessKey.Length == 32)
                {
                    if (IsImagePathSet(imagePath))
                    {
                        var filename = GetImageFileName(imagePath);
                        Console.WriteLine("Getting image insights for image: " + filename);
                        var imageBinary = GetImageBinary(imagePath);

                        var boundary = string.Format(BoundaryTemplate, Guid.NewGuid());
                        var json = BingImageSearch(imageBinary, boundary, uriBase, accessKey);

                        Console.WriteLine("\nJSON Response:\n");
                        Console.WriteLine(JsonPrettyPrint(json));
                    }
                }
                else
                {
                    Console.WriteLine("Invalid Bing Visual Search API subscription key!");
                    Console.WriteLine("Please paste yours into the source code.");
                }

                Console.Write("\nPress Enter to exit ");
                Console.ReadLine();
            }
            catch (Exception e)
            {
                Console.WriteLine(e.Message);
            }
        }
```

Değiştirin `BingImageSearch` bu kodla yöntemi:

```csharp
        /// <summary>
        /// Calls the Bing visual search endpoint and returns the JSON response.
        /// </summary>
        static string BingImageSearch(byte[] image, string boundary, string uri, string subscriptionKey)
        {
            var requestMessage = new HttpRequestMessage(HttpMethod.Post, uri);
            requestMessage.Headers.Add("Ocp-Apim-Subscription-Key", accessKey);

            var content = new MultipartFormDataContent(boundary);
            content.Add(new ByteArrayContent(image), "image", "myimage");
            requestMessage.Content = content;

            var httpClient = new HttpClient();

            Task<HttpResponseMessage> httpRequest = httpClient.SendAsync(requestMessage, HttpCompletionOption.ResponseContentRead, CancellationToken.None);
            HttpResponseMessage httpResponse = httpRequest.Result;
            HttpStatusCode statusCode = httpResponse.StatusCode;
            HttpContent responseContent = httpResponse.Content;

            string json = null;

            if (responseContent != null)
            {
                Task<String> stringContentsTask = responseContent.ReadAsStringAsync();
                json = stringContentsTask.Result;
            }

            return json;
        }
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Görsel arama tek sayfa web uygulaması oluşturma](../tutorial-bing-visual-search-single-page-app.md)
