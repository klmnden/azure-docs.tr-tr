---
title: 'Hızlı Başlangıç: El yazısı metni ayıklama - REST, C# - Görüntü İşleme'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, C# ile Görüntü İşleme API’sini kullanarak bir görüntüden el yazısı metni ayıklayacaksınız.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: pafarley
ms.openlocfilehash: f63cebd7a4af5b2289470ef34a80c8680aa981fd
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49340350"
---
# <a name="quickstart-extract-handwritten-text-using-the-rest-api-and-c35-in-computer-vision"></a>Hızlı Başlangıç: Görüntü İşleme’de REST API ve C# kullanarak el yazısı metni ayıklama

Bu hızlı başlangıçta, Görüntü İşleme’nin REST API’sini kullanarak bir görüntüden el yazısı metni ayıklayacaksınız. [Metin Tanıma](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) ve [Metin Tanıma İşlemi Sonucunu Alma](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2cf1154055056008f201) yöntemleri ile, bir görüntüdeki el yazısı metni algılayabilir ve tanınan karakterleri makine tarafından kullanılabilir bir karakter akışı halinde ayıklayabilirsiniz.

> [!IMPORTANT]
> [OCR](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) yönteminden farklı olarak [Metin Tanıma](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) yöntemi zaman uyumsuz olarak çalıştırılır. Bu yöntem, başarılı bir yanıt gövdesinde herhangi bir bilgi döndürmez. Bunun yerine, Metin Tanıma yöntemi, `Operation-Content` yanıt üst bilgisi alanının değerindeki bir URI’yi döndürür. Daha sonra hem durumu denetlemek hem de Metin Tanıma yöntem çağrısının sonuçlarını döndürmek için [Metin Tanıma İşlemi Sonucunu Alma](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2cf1154055056008f201) yöntemini temsil eden bu URI’yi çağırırsınız.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

- [Visual Studio 2015](https://visualstudio.microsoft.com/downloads/) veya üzerine sahip olmanız gerekir.
- Görüntü İşleme için bir abonelik anahtarınız olması gerekir. Bir abonelik anahtarı almak için bkz. [Abonelik Anahtarları Alma](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="create-and-run-the-sample-application"></a>Örnek uygulamayı oluşturma ve çalıştırma

Örneği Visual Studio’da oluşturmak için aşağıdaki adımları uygulayın:

1. Visual C# Konsol Uygulaması şablonunu kullanarak Visual Studio’da yeni bir Visual Studio çözümü oluşturun.
1. Newtonsoft.Json NuGet paketini yükleyin.
    1. Menüde **Araçlar**’a tıklayın, **NuGet Paket Yöneticisi**’ni ve ardından **Çözüm için NuGet Paketlerini Yönet**’i seçin.
    1. **Gözat** sekmesine tıklayın ve **Arama** kutusuna "Newtonsoft.Json" yazın.
    1. Görüntülendiğinde **Newtonsoft.Json**’ı seçin, sonra proje adınızın yanındaki onay kutusuna ve **Yükle**’ye tıklayın.
1. `Program.cs` içindeki kodu aşağıdaki kodla değiştirin, ardından kodda gereken yerlerde aşağıdaki değişiklikleri yapın:
    1. `subscriptionKey` değerini abonelik anahtarınızla değiştirin.
    1. Gerekirse `uriBase` değerini, abonelik anahtarlarınızı aldığınız Azure bölgesinden [Metin Tanıma](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) yönteminin uç nokta URL’si ile değiştirin.
1. Programı çalıştırın.
1. İstemde yerel görüntü yolunu girin.

```csharp
using Newtonsoft.Json.Linq;
using System;
using System.IO;
using System.Linq;
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
        // Free trial subscription keys are generated in the West Central US region.
        // If you use a free trial subscription key, you shouldn't need to change
        // this region.
        const string uriBase =
            "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/recognizeText";

        static void Main()
        {
            // Get the path and filename to process from the user.
            Console.WriteLine("Handwriting Recognition:");
            Console.Write(
                "Enter the path to an image with handwritten text you wish to read: ");
            string imageFilePath = Console.ReadLine();

            if (File.Exists(imageFilePath))
            {
                // Call the REST API method.
                Console.WriteLine("\nWait a moment for the results to appear.\n");
                ReadHandwrittenText(imageFilePath).Wait();
            }
            else
            {
                Console.WriteLine("\nInvalid file path");
            }
            Console.WriteLine("\nPress Enter to exit...");
            Console.ReadLine();
        }

        /// <summary>
        /// Gets the handwritten text from the specified image file by using
        /// the Computer Vision REST API.
        /// </summary>
        /// <param name="imageFilePath">The image file with handwritten text.</param>
        static async Task ReadHandwrittenText(string imageFilePath)
        {
            try
            {
                HttpClient client = new HttpClient();

                // Request headers.
                client.DefaultRequestHeaders.Add(
                    "Ocp-Apim-Subscription-Key", subscriptionKey);

                // Request parameter.
                string requestParameters = "mode=Handwritten";

                // Assemble the URI for the REST API method.
                string uri = uriBase + "?" + requestParameters;

                HttpResponseMessage response;

                // Two REST API methods are required to extract handwritten text.
                // One method to submit the image for processing, the other method
                // to retrieve the text found in the image.

                // operationLocation stores the URI of the second REST API method,
                // returned by the first REST API method.
                string operationLocation;

                // Reads the contents of the specified local image
                // into a byte array.
                byte[] byteData = GetImageAsByteArray(imageFilePath);

                // Adds the byte array as an octet stream to the request body.
                using (ByteArrayContent content = new ByteArrayContent(byteData))
                {
                    // This example uses the "application/octet-stream" content type.
                    // The other content types you can use are "application/json"
                    // and "multipart/form-data".
                    content.Headers.ContentType =
                        new MediaTypeHeaderValue("application/octet-stream");

                    // The first REST API method, Recognize Text, starts
                    // the async process to analyze the written text in the image.
                    response = await client.PostAsync(uri, content);
                }

                // The response header for the Recognize Text method contains the URI
                // of the second method, Get Recognize Text Operation Result, which
                // returns the results of the process in the response body.
                // The Recognize Text operation does not return anything in the response body.
                if (response.IsSuccessStatusCode)
                    operationLocation =
                        response.Headers.GetValues("Operation-Location").FirstOrDefault();
                else
                {
                    // Display the JSON error data.
                    string errorString = await response.Content.ReadAsStringAsync();
                    Console.WriteLine("\n\nResponse:\n{0}\n",
                        JToken.Parse(errorString).ToString());
                    return;
                }

                // If the first REST API method completes successfully, the second 
                // REST API method retrieves the text written in the image.
                //
                // Note: The response may not be immediately available. Handwriting
                // recognition is an asynchronous operation that can take a variable
                // amount of time depending on the length of the handwritten text.
                // You may need to wait or retry this operation.
                //
                // This example checks once per second for ten seconds.
                string contentString;
                int i = 0;
                do
                {
                    System.Threading.Thread.Sleep(1000);
                    response = await client.GetAsync(operationLocation);
                    contentString = await response.Content.ReadAsStringAsync();
                    ++i;
                }
                while (i < 10 && contentString.IndexOf("\"status\":\"Succeeded\"") == -1);

                if (i == 10 && contentString.IndexOf("\"status\":\"Succeeded\"") == -1)
                {
                    Console.WriteLine("\nTimeout error.\n");
                    return;
                }

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
    "status": "Succeeded",
    "recognitionResult": {
        "lines": [
            {
                "boundingBox": [
                    99,
                    195,
                    1309,
                    45,
                    1340,
                    292,
                    130,
                    442
                ],
                "text": "when you write them down",
                "words": [
                    {
                        "boundingBox": [
                            152,
                            191,
                            383,
                            154,
                            341,
                            421,
                            110,
                            458
                        ],
                        "text": "when"
                    },
                    {
                        "boundingBox": [
                            436,
                            145,
                            607,
                            118,
                            565,
                            385,
                            394,
                            412
                        ],
                        "text": "you"
                    },
                    {
                       "boundingBox": [
                            644,
                            112,
                            873,
                            76,
                            831,
                            343,
                            602,
                            379
                        ],
                        "text": "write"
                    },
                    {
                        "boundingBox": [
                            895,
                            72,
                            1092,
                            41,
                            1050,
                            308,
                            853,
                            339
                        ],
                        "text": "them"
                    },
                    {
                        "boundingBox": [
                            1140,
                            33,
                            1400,
                            0,
                            1359,
                            258,
                            1098,
                            300
                        ],
                        "text": "down"
                    }
                ]
            },
            {
                "boundingBox": [
                    142,
                    222,
                    1252,
                    62,
                    1269,
                    180,
                    159,
                    340
                ],
                "text": "You remember things better",
                "words": [
                    {
                        "boundingBox": [
                            140,
                            223,
                            267,
                            205,
                            288,
                            324,
                            162,
                            342
                        ],
                        "text": "You"
                    },
                    {
                        "boundingBox": [
                            314,
                            198,
                            740,
                            137,
                            761,
                            256,
                            335,
                            317
                        ],
                        "text": "remember"
                    },
                    {
                        "boundingBox": [
                            761,
                            134,
                            1026,
                            95,
                            1047,
                            215,
                            782,
                            253
                        ],
                        "text": "things"
                    },
                    {
                        "boundingBox": [
                            1046,
                            92,
                            1285,
                            58,
                            1307,
                            177,
                            1068,
                            212
                        ],
                        "text": "better"
                    }
                ]
            },
            {
                "boundingBox": [
                    155,
                    405,
                    537,
                    338,
                    557,
                    449,
                    175,
                    516
                ],
                "text": "by hand",
                "words": [
                    {
                        "boundingBox": [
                            146,
                            408,
                            266,
                            387,
                            301,
                            495,
                            181,
                            516
                        ],
                        "text": "by"
                    },
                    {
                        "boundingBox": [
                            290,
                            383,
                            569,
                            334,
                            604,
                            443,
                            325,
                            491
                        ],
                        "text": "hand"
                    }
                ]
            }
        ]
    }
}
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse Visual Studio çözümünü silin. Bunu yapmak için Dosya Gezgini’ni açın, Visual Studio çözümünü oluşturduğunuz klasöre gidin ve klasörü silin.

## <a name="next-steps"></a>Sonraki adımlar

Optik karakter tanıma (OCR) gerçekleştirmek için Görüntü İşleme kullanan temel bir Windows uygulaması keşfedin. Akıllı kırpılmış küçük resimler oluşturun. Buna ek olarak, bir görüntüdeki yüzler gibi görsel özellikleri algılayın, kategorilere ayırın, etiketleyin ve açıklayın. Görüntü İşleme API'lerini hızlı bir şekilde denemeniz için [Open API test konsolu](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console) konusuna göz atın.

> [!div class="nextstepaction"]
> [Görüntü İşleme API’si C# Öğreticisi](../Tutorials/CSharpTutorial.md)
