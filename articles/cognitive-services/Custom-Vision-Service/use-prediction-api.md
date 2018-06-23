---
title: Özel görme hizmet tahmin uç - Azure Bilişsel hizmetler kullanın | Microsoft Docs
description: Program aracılığıyla özel görme hizmet sınıflandırıcı görüntülerle test etmek için API kullanmayı öğrenin.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: 54f9d9fec1f40c167341dec6a8699b6a558419da
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353303"
---
# <a name="use-the-prediction-endpoint-to-test-images-programmatically-with-a-custom-vision-service-classifier"></a>Bir özel görme hizmet sınıflandırıcı program aracılığıyla görüntülerle test etmek için tahmini uç noktası kullan

Modelinizi eğitmek sonra görüntüleri programlı olarak tahmin API'sine göndererek test edebilirsiniz. 

> [!NOTE]
> Bu belgeyi kullanarak C# tahmin API görüntüye göndermek için gösterir. Daha fazla bilgi ve API kullanma örnekleri için bkz: [tahmin API Başvurusu](https://go.microsoft.com/fwlink/?linkid=865445).

## <a name="get-the-url-and-prediction-key"></a>URL ve tahmin anahtarı alma

Gelen [özel görme web sayfası](https://customvision.ai), projenizi seçin ve ardından __performans__ sekmesi. Tahmin API kullanma hakkında bilgi görüntülemek için seçin __tahmin URL__. Kullanmak için aşağıdaki bilgileri uygulamada kopyalayın:

* __URL__ kullanmak için bir __görüntü dosyası__.
* __Tahmin anahtar__ değeri.

> [!TIP]
> Birden çok kez varsa, varsayılan olarak ayarlayarak hangisinin kullanılan kontrol edebilirsiniz. Gelen yinelemeyi seçmek __yineleme__ bölümünde ve ardından __olun varsayılan__ sayfanın üst kısmındaki.

![Performans sekmesini tahmin URL çevreleyen için kırmızı bir dikdörtgen gösterilir.](./media/use-prediction-api/prediction-url.png)

## <a name="create-the-application"></a>Uygulama oluşturma

1. Visual Studio'da yeni bir C# konsol uygulaması oluşturun.

2. Aşağıdaki kod gövdesi olarak kullanmak __Program.cs__ dosya.

    > [!IMPORTANT]
    > Aşağıdaki bilgileri değiştirin:
    >
    > * Ayarlama __ad alanı__ projenizin adına.
    > * Ayarlama __tahmin anahtar__ önceki ile başlayan satırı içindeki alınan değer `client.DefaultRequestHeaders.Add("Prediction-Key",`.
    > * Ayarlama __URL__ önceki ile başlayan satırı içindeki alınan değer `string url =`.

    ```csharp
    using System;
    using System.IO;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Threading.Tasks;

    namespace CSPredictionSample
    {
        static class Program
        {
            static void Main()
            {
                Console.Write("Enter image file path: ");
                string imageFilePath = Console.ReadLine();

                MakePredictionRequest(imageFilePath).Wait();

                Console.WriteLine("\n\n\nHit ENTER to exit...");
                Console.ReadLine();
            }

            static byte[] GetImageAsByteArray(string imageFilePath)
            {
                FileStream fileStream = new FileStream(imageFilePath, FileMode.Open, FileAccess.Read);
                BinaryReader binaryReader = new BinaryReader(fileStream);
                return binaryReader.ReadBytes((int)fileStream.Length);
            }

            static async Task MakePredictionRequest(string imageFilePath)
            {
                var client = new HttpClient();

                // Request headers - replace this example key with your valid subscription key.
                client.DefaultRequestHeaders.Add("Prediction-Key", "13hc77781f7e4b19b5fcdd72a8df7156");

                // Prediction URL - replace this example URL with your valid prediction URL.
                string url = "http://southcentralus.api.cognitive.microsoft.com/customvision/v1.0/prediction/d16e136c-5b0b-4b84-9341-6a3fff8fa7fe/image?iterationId=f4e573f6-9843-46db-8018-b01d034fd0f2";

                HttpResponseMessage response;

                // Request body. Try this sample with a locally stored image.
                byte[] byteData = GetImageAsByteArray(imageFilePath);

                using (var content = new ByteArrayContent(byteData))
                {
                    content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
                    response = await client.PostAsync(url, content);
                    Console.WriteLine(await response.Content.ReadAsStringAsync());
                }
            }
        }
    }
    ```

## <a name="use-the-application"></a>Uygulamayı kullanma

Uygulama çalışırken, bir görüntü dosyasına giden yolu girin. Görüntü API'sine gönderildi ve sonuçları bir JSON belgesi olarak döndürülür. Aşağıdaki JSON yanıtı örneğidir

```json
{
    "Id":"3f76364c-b8ae-4818-a2b2-2794cfbe377a",
    "Project":"2277aca4-7aff-4742-8afb-3682e251c913",
    "Iteration":"84105bfe-73b5-4fcc-addb-756c0de17df2",
    "Created":"2018-05-03T14:15:22.5659829Z",
    "Predictions":[
        {"TagId":"35ac2ad0-e3ef-4e60-b81f-052a1057a1ca","Tag":"dog","Probability":0.102716163},
        {"TagId":"28e1a872-3776-434c-8cf0-b612dd1a953c","Tag":"cat","Probability":0.02037274}
    ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

[Model mobil kullanım için dışarı aktarma](export-your-model.md)