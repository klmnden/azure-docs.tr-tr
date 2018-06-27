---
title: Duygu tanıma API'si C# hızlı başlangıç | Microsoft Docs
description: Bilgi ve C# Bilişsel Services ile duygu API'sini kullanarak hızla başlamanıza yardımcı olmak için bir kod örneği alın.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 11/02/2017
ms.author: anroth
ms.openlocfilehash: 89735ae54395447e3cb421f45db3d6b99001ecd6
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37016574"
---
# <a name="emotion-api-c-quick-start"></a>Duygu tanıma API'si C# hızlı başlangıç

> [!IMPORTANT]
> Video API’si Önizlemesi 30 Ekim 2017 tarihinde sona erdi. Kolayca videoların öngörüleri ayıklamak için yeni deneyin [Video dizin oluşturucu API önizlemesi](https://azure.microsoft.com/services/cognitive-services/video-indexer/). Konuşma sözcükler, yüzler, karakterler ve duygular algılayarak arama sonuçları gibi içerik bulma deneyimlerini geliştirmek üzere de kullanabilirsiniz. Daha fazla bilgi için bkz: [Video dizin oluşturucu önizlemesi](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview) genel bakış.

Bu makalede, bilgi ve hızlı bir şekilde yardımcı olmak için bir kod örneği Başlarken kullanarak sağlanmaktadır [duygu tanıma API'si tanıması yöntemi](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) C# ile. Görüntüyü bir veya daha fazla kişiler tarafından ifade duygular tanımak için kullanabilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar
* Bilişsel hizmetler almak [duygu tanıma API'si Windows SDK](https://www.nuget.org/packages/Microsoft.ProjectOxford.Emotion/).
* Ücretsiz almak [abonelik anahtarı](https://azure.microsoft.com/try/cognitive-services/).

## <a name="emotion-recognition-c-example-request"></a>Duygu tanıma C# örnek istek

Visual Studio'da yeni bir konsol çözümü oluşturun ve ardından Program.cs aşağıdaki kodla değiştirin. Değişiklik `string uri` elde burada abonelik anahtarlarınızı bölge kullanılacak. Değiştir **Apim abonelik anahtar Ocp** değeri geçerli bir abonelik anahtarınızı ile. Abonelik anahtarı bulmak için Azure portalına gidin. Sol taraftaki gezinti bölmesi altında **anahtarları** bölümünde, duygu tanıma API'si kaynağınıza göz atın. Benzer şekilde, uygun alabilirsiniz URI'de bağlanmak **genel bakış** paneli altında listelenen kaynağınız için **Endpoint**.

![API kaynak anahtarlarınızı](../../media/emotion-api/keys.png)

İsteğiniz yanıtı işlemek için bir kitaplık gibi kullanın `Newtonsoft.Json`. Bu şekilde bir JSON dizesinde yönetilebilir nesneleri Tokens olarak adlandırılan bir dizi olarak işleyebilir. Bu kitaplık, pakete eklemek için Çözüm Gezgini'nde projenize sağ tıklayın ve seçin **Nuget paketlerini Yönet**. İçin arama **Newtonsoft**. İlk sonuca olmalıdır **Newtonsoft.Json**. **Yükle**’yi seçin. Bu kitaplık, uygulamanızda artık başvurabilirsiniz.

![Newtonsoft.Json yükleyin](../../media/emotion-api/newtonsoft-nuget.png)

```csharp
using System;
using System.IO;
using System.Net.Http.Headers;
using System.Net.Http;
using Newtonsoft.Json.Linq;

namespace CSHttpClientSample
{
    static class Program
    {
        static void Main()
        {
            Console.Write("Enter the path to a JPEG image file:");
            string imageFilePath = Console.ReadLine();

            MakeRequest(imageFilePath);

            Console.WriteLine("\n\n\nWait for the result below, then hit ENTER to exit...\n\n\n");
            Console.ReadLine(); // wait for ENTER to exit program
        }

        static byte[] GetImageAsByteArray(string imageFilePath)
        {
            FileStream fileStream = new FileStream(imageFilePath, FileMode.Open, FileAccess.Read);
            BinaryReader binaryReader = new BinaryReader(fileStream);
            return binaryReader.ReadBytes((int)fileStream.Length);
        }

        static async void MakeRequest(string imageFilePath)
        {
            var client = new HttpClient();

            // Request headers - replace this example key with your valid key.
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "<your-subscription-key>"); // 

            // NOTE: You must use the same region in your REST call as you used to obtain your subscription keys.
            //   For example, if you obtained your subscription keys from westcentralus, replace "westus" in the 
            //   URI below with "westcentralus".
            string uri = "https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize?";
            HttpResponseMessage response;
            string responseContent;

            // Request body. Try this sample with a locally stored JPEG image.
            byte[] byteData = GetImageAsByteArray(imageFilePath);

            using (var content = new ByteArrayContent(byteData))
            {
                // This example uses content type "application/octet-stream".
                // The other content types you can use are "application/json" and "multipart/form-data".
                content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
                response = await client.PostAsync(uri, content);
                responseContent = response.Content.ReadAsStringAsync().Result;
            }

            // A peek at the raw JSON response.
            Console.WriteLine(responseContent);

            // Processing the JSON into manageable objects.
            JToken rootToken = JArray.Parse(responseContent).First;

            // First token is always the faceRectangle identified by the API.
            JToken faceRectangleToken = rootToken.First;

            // Second token is all emotion scores.
            JToken scoresToken = rootToken.Last;

            // Show all face rectangle dimensions
            JEnumerable<JToken> faceRectangleSizeList = faceRectangleToken.First.Children();
            foreach (var size in faceRectangleSizeList) {
                Console.WriteLine(size);
            }

            // Show all scores
            JEnumerable<JToken> scoreList = scoresToken.First.Children();
            foreach (var score in scoreList) {
                Console.WriteLine(score);
            }
        }
    }
}
```

## <a name="recognize-emotions-sample-response"></a>Yanıt duygular örnek tanı
Başarılı bir çağrı yüz girişleri dizisi ve ilişkili duygu puanlarını döndürür. Azalan düzende yüz dikdörtgen boyutuna göre sıralanır. Boş bir yanıt hiçbir yüzeyleri algıladığını belirtir. Bir duygu giriş aşağıdaki alanları içerir:

* faceRectangle: yüz görüntüdeki dikdörtgen konumu
* Puanları: duygu puanlar görüntüdeki her yüz için 

```json
application/json 
[
  {
    "faceRectangle": {
      "left": 68,
      "top": 97,
      "width": 64,
      "height": 97
    },
    "scores": {
      "anger": 0.00300731952,
      "contempt": 5.14648448E-08,
      "disgust": 9.180124E-06,
      "fear": 0.0001912825,
      "happiness": 0.9875571,
      "neutral": 0.0009861537,
      "sadness": 1.889955E-05,
      "surprise": 0.008229999
    }
  }
]
