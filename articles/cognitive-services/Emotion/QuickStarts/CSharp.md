---
title: "Hızlı başlangıç: Bir görüntüdeki yüzlerin duygularını tanıma - Duygu Tanıma API'si, C#"
titlesuffix: Azure Cognitive Services
description: C# ile Duygu Tanıma API'sini kullanmaya başlamanıza yardımcı olacak bilgileri ve kod örneğini edinin.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: quickstart
ms.date: 11/02/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: 530d05887e585884b184635e01031c1332fad3fb
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48239379"
---
# <a name="quickstart-build-an-app-to-recognize-emotions-on-faces-in-an-image"></a>Hızlı başlangıç: Bir görüntüdeki yüzlerin duygularını tanımak için bir uygulama oluşturma.

> [!IMPORTANT]
> Duygu Tanıma API'si 15 Şubat 2019 tarihinde kullanım dışı bırakılacaktır. Duygu tanıma özelliği [Yüz Tanıma API'sinin](https://docs.microsoft.com/azure/cognitive-services/face/) bir parçası olarak genel kullanıma sunulmuştur.

Bu makale C# ile [Duygu Tanıma metodunu](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) kullanmaya başlamanıza yardımcı olacak bilgiler ve kod örneği sunar. Bunu kullanarak bir görüntüdeki kişi veya kişilerin duygularını tanımlayabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
* Bilişsel Hizmetler [Duygu Tanıma API'si Windows SDK'sını](https://www.nuget.org/packages/Microsoft.ProjectOxford.Emotion/) edinin.
* Ücretsiz [abonelik anahtarınızı](https://azure.microsoft.com/try/cognitive-services/) alın.

## <a name="emotion-recognition-c-example-request"></a>Duygu Tanıma C# isteği örneği

Visual Studio'da yeni bir Konsol çözümü oluşturun ve Program.cs içeriğini aşağıdaki kodla değiştirin. `string uri` değerini abonelik anahtarlarınızı aldığınız bölgeyi kullanacak şekilde değiştirin. **Ocp-Apim-Subscription-Key** değerini geçerli abonelik anahtarınız ile değiştirin. Abonelik anahtarını bulmak için Azure portala gidin. Sol taraftaki gezinti bölmesinde, **Anahtarlar** kısmında Duygu Tanıma API'si kaynağınıza göz atın. Benzer şekilde **Uç nokta** bölümünde listelenen kaynağınızın **Genel bakış** panelinde uygun bağlantı URI'sini bulabilirsiniz.

![API kaynağı anahtarlarınız](../../media/emotion-api/keys.png)

İsteğinizin yanıtını işlemek için `Newtonsoft.Json` gibi bir kitaplık kullanın. Bu şekilde bir JSON dizisini Belirteç olarak adlandırılan yönetilebilir nesneler dizisi şeklinde işleyebilirsiniz. Bu kitaplığı paketinize eklemek için Çözüm Gezgini'nde projenize sağ tıklayın ve **Nuget Paketlerini Yönet**'i seçin. Ardından **Newtonsoft** araması yapın. İlk sonuç **Newtonsoft.Json** olmalıdır. **Yükle**’yi seçin. Artık uygulamanızda bu kitaplığa başvurabilirsiniz.

![Newtonsoft.Json yükleme](../../media/emotion-api/newtonsoft-nuget.png)

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

## <a name="recognize-emotions-sample-response"></a>Duygu tanıma örneği yanıtı
Başarılı bir çağrı, yüz girişlerinden ve ilgili duygu puanlarından oluşan bir dizi döndürür. Bunlar yüz dikdörtgenine göre büyükten küçüğe doğru sıralanmış şekildedir. Yanıtın boş olması hiç yüz algılanmadığını gösterir. Duygu girişi aşağıdaki alanları içerir:

* faceRectangle: Dikdörtgen şeklinde yüzün görüntü içindeki konumu
* scores: Görüntüdeki her bir yüzün duygu puanı

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
