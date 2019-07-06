---
title: 'Hızlı Başlangıç: Yazdırılan metin (OCR) - REST ayıklayın,C#'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, C# ile Görüntü İşleme API’si kullanarak bir görüntüden yazdırılan metni ayıklayacaksınız.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 07/03/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 18a00c2aebd05d2ed2da54e436d245c70bf25051
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67604470"
---
# <a name="quickstart-extract-printed-text-ocr-using-the-computer-vision-rest-api-and-c"></a>Hızlı Başlangıç: Görüntü işleme REST API'sini kullanarak yazdırılan metin (OCR) ayıklayın veC#

Bu hızlı başlangıçta, görüntü işleme'nın REST API'sini kullanarak bir görüntüden optik karakter tanıma (OCR) ile yazdırılan metin ayıklayacaksınız. İle [OCR](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) özelliği, bir resimdeki yazılı metni algılayın ve tanınan karakterleri makine tarafından kullanılabilir bir karakter akışı halinde ayıklayın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

- [Visual Studio 2015](https://visualstudio.microsoft.com/downloads/) veya üzerine sahip olmanız gerekir.
- Görüntü İşleme için bir abonelik anahtarınız olması gerekir. Ücretsiz bir deneme anahtarından alabilirsiniz [Bilişsel Hizmetler'i deneyin](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision). Veya yönergeleri [Bilişsel Hizmetler hesabı oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) görüntü işleme için abone ve anahtarınızı alın.

## <a name="create-and-run-the-sample-application"></a>Örnek uygulamayı oluşturma ve çalıştırma

Örneği Visual Studio’da oluşturmak için aşağıdaki adımları uygulayın:

1. Visual C# Konsol Uygulaması şablonunu kullanarak Visual Studio’da yeni bir Visual Studio çözümü oluşturun.
1. Newtonsoft.Json NuGet paketini yükleyin.
    1. Menüde **Araçlar**’a tıklayın, **NuGet Paket Yöneticisi**’ni ve ardından **Çözüm için NuGet Paketlerini Yönet**’i seçin.
    1. **Gözat** sekmesine tıklayın ve **Arama** kutusuna "Newtonsoft.Json" yazın.
    1. Görüntülendiğinde **Newtonsoft.Json**’ı seçin, sonra proje adınızın yanındaki onay kutusuna ve **Yükle**’ye tıklayın.
1. `Program.cs` içindeki kodu aşağıdaki kodla değiştirin, ardından kodda gereken yerlerde aşağıdaki değişiklikleri yapın:
    1. `subscriptionKey` değerini abonelik anahtarınızla değiştirin.
    1. Gerekirse `uriBase` değerini, abonelik anahtarlarınızı aldığınız Azure bölgesinden [OCR](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) yönteminin uç nokta URL’si ile değiştirin.
1. Programı çalıştırın.
1. İstemde yerel görüntü yolunu girin.

```csharp
using Newtonsoft.Json.Linq;
using System;
using System.IO;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Threading.Tasks;

namespace CSHttpClientSample
{
    static class Program
    {
        // Replace <Subscription Key> with your valid subscription key.
        const string subscriptionKey = "<Subscription Key>";

        // You must use the same Azure region in your REST API method as you used to
        // get your subscription keys. For example, if you got your subscription keys
        // from the West US region, replace "westcentralus" in the URL
        // below with "westus".
        //
        // Free trial subscription keys are generated in the "westus" region.
        // If you use a free trial subscription key, you shouldn't need to change
        // this region.
        const string uriBase =
            "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/ocr";

        static void Main()
        {
            // Get the path and filename to process from the user.
            Console.WriteLine("Optical Character Recognition:");
            Console.Write("Enter the path to an image with text you wish to read: ");
            string imageFilePath = Console.ReadLine();

            if (File.Exists(imageFilePath))
            {
                // Call the REST API method.
                Console.WriteLine("\nWait a moment for the results to appear.\n");
                MakeOCRRequest(imageFilePath).Wait();
            }
            else
            {
                Console.WriteLine("\nInvalid file path");
            }
            Console.WriteLine("\nPress Enter to exit...");
            Console.ReadLine();
        }

        /// <summary>
        /// Gets the text visible in the specified image file by using
        /// the Computer Vision REST API.
        /// </summary>
        /// <param name="imageFilePath">The image file with printed text.</param>
        static async Task MakeOCRRequest(string imageFilePath)
        {
            try
            {
                HttpClient client = new HttpClient();

                // Request headers.
                client.DefaultRequestHeaders.Add(
                    "Ocp-Apim-Subscription-Key", subscriptionKey);

                // Request parameters. 
                // The language parameter doesn't specify a language, so the 
                // method detects it automatically.
                // The detectOrientation parameter is set to true, so the method detects and
                // and corrects text orientation before detecting text.
                string requestParameters = "language=unk&detectOrientation=true";

                // Assemble the URI for the REST API method.
                string uri = uriBase + "?" + requestParameters;

                HttpResponseMessage response;

                // Read the contents of the specified local image
                // into a byte array.
                byte[] byteData = GetImageAsByteArray(imageFilePath);

                // Add the byte array as an octet stream to the request body.
                using (ByteArrayContent content = new ByteArrayContent(byteData))
                {
                    // This example uses the "application/octet-stream" content type.
                    // The other content types you can use are "application/json"
                    // and "multipart/form-data".
                    content.Headers.ContentType =
                        new MediaTypeHeaderValue("application/octet-stream");

                    // Asynchronously call the REST API method.
                    response = await client.PostAsync(uri, content);
                }

                // Asynchronously get the JSON response.
                string contentString = await response.Content.ReadAsStringAsync();

                // Display the JSON response.
                Console.WriteLine("\nResponse:\n\n{0}\n",
                    JToken.Parse(contentString).ToString());
            }
            catch (Exception e)
            {
                Console.WriteLine("\n" + e.Message);
            }
        }

        /// <summary>
        /// Returns the contents of the specified file as a byte array.
        /// </summary>
        /// <param name="imageFilePath">The image file to read.</param>
        /// <returns>The byte array of the image data.</returns>
        static byte[] GetImageAsByteArray(string imageFilePath)
        {
            // Open a read-only file stream for the specified file.
            using (FileStream fileStream =
                new FileStream(imageFilePath, FileMode.Open, FileAccess.Read))
            {
                // Read the file's contents into a byte array.
                BinaryReader binaryReader = new BinaryReader(fileStream);
                return binaryReader.ReadBytes((int)fileStream.Length);
            }
        }
    }
}
```

## <a name="examine-the-response"></a>Yanıtı inceleme

Başarılı bir yanıt JSON biçiminde döndürülür. Örnek uygulama aşağıdaki örneğe benzer şekilde başarılı bir yanıtı ayrıştırıp konsol penceresinde görüntüler:

```json
{
    "language": "en",
    "textAngle": -1.5000000000000335,
    "orientation": "Up",
    "regions": [
        {
            "boundingBox": "154,49,351,575",
            "lines": [
                {
                    "boundingBox": "165,49,340,117",
                    "words": [
                        {
                            "boundingBox": "165,49,63,109",
                            "text": "A"
                        },
                        {
                            "boundingBox": "261,50,244,116",
                            "text": "GOAL"
                        }
                    ]
                },
                {
                    "boundingBox": "165,169,339,93",
                    "words": [
                        {
                            "boundingBox": "165,169,339,93",
                            "text": "WITHOUT"
                        }
                    ]
                },
                {
                    "boundingBox": "159,264,342,117",
                    "words": [
                        {
                            "boundingBox": "159,264,64,110",
                            "text": "A"
                        },
                        {
                            "boundingBox": "255,266,246,115",
                            "text": "PLAN"
                        }
                    ]
                },
                {
                    "boundingBox": "161,384,338,119",
                    "words": [
                        {
                            "boundingBox": "161,384,86,113",
                            "text": "IS"
                        },
                        {
                            "boundingBox": "274,387,225,116",
                            "text": "JUST"
                        }
                    ]
                },
                {
                    "boundingBox": "154,506,341,118",
                    "words": [
                        {
                            "boundingBox": "154,506,62,111",
                            "text": "A"
                        },
                        {
                            "boundingBox": "248,508,247,116",
                            "text": "WISH"
                        }
                    ]
                }
            ]
        }
    ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Optik karakter tanıma (OCR) gerçekleştirmek için Görüntü İşleme kullanan temel bir Windows uygulaması keşfedin. Akıllı kırpılmış küçük resimler oluşturun. Buna ek olarak, bir görüntüdeki yüzler gibi görsel özellikleri algılayın, kategorilere ayırın, etiketleyin ve açıklayın.

> [!div class="nextstepaction"]
> [Görüntü işleme API'si C# Öğreticisi](../Tutorials/CSharpTutorial.md)
