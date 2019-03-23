---
title: 'Örnek: Program aracılığıyla sınıflandırıcı - özel görüntü işleme görüntülerle test etmek için tahmin uç noktası kullan'
titlesuffix: Azure Cognitive Services
description: Özel Görüntü İşleme sınıflandırıcınızla programlama yoluyla görüntüleri test etmek için API’nin nasıl kullanılacağını öğrenin.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: article
ms.date: 03/21/2019
ms.author: anroth
ms.openlocfilehash: e50933ea0231b4be22c2d0f82d33fd02dd0918f5
ms.sourcegitcommit: 87bd7bf35c469f84d6ca6599ac3f5ea5545159c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58351618"
---
# <a name="use-the-prediction-endpoint-to-test-images-programmatically"></a>Görüntüleri programlı olarak test etmek için tahmin uç noktası kullan

Modelinizi eğittikten sonra görüntüleri Tahmin API’sine göndererek programlama yoluyla test edebilirsiniz.

> [!NOTE]
> Bu belgede, Tahmin API’sine görüntü göndermek için C# kullanımı gösterilmektedir. Daha fazla bilgi ve API kullanma örnekleri için bkz. [Tahmin API’si başvurusu](https://go.microsoft.com/fwlink/?linkid=865445).

## <a name="get-the-url-and-prediction-key"></a>URL ve tahmin anahtarını alma

[Özel Görüntü İşleme web sayfasından](https://customvision.ai) projenizi ve __Performans__ sekmesini seçin. __Tahmin anahtarı__ da dahil olmak üzere Tahmin API’sini kullanma hakkında bilgileri görüntülemek için __Tahmin URL__‘sini seçin. Bir Azure kaynağına bağlı projeleri için __tahmin anahtar__ içinde bulunabilir [Azure portalında](https://portal.azure.com) altında ilişkili Azure kaynak sayfası __anahtarları__. Aşağıdaki bilgileri uygulamada kullanmak üzere kopyalayın:

* __Görüntü dosyası__ kullanmak için __URL__.
* __Tahmin anahtarı__ değeri.

> [!TIP]
> Birden çok yinelemeniz varsa, varsayılan olarak ayarlayarak hangisinin kullanıldığını denetleyebilirsiniz. __Yinelemeler__ bölümünden yinelemeyi seçin ve sonra sayfanın üst kısmından __Varsayılan yap__’ı seçin.

![Performans sekmesi, Tahmin URL’sini çevreleyen bir kırmızı dikdörtgen ile gösterilir.](./media/use-prediction-api/prediction-url.png)

## <a name="create-the-application"></a>Uygulama oluşturma

1. Visual Studio’dan yeni bir C# konsol uygulaması oluşturun.

1. __Program.cs__ dosyasının gövdesi olarak aşağıdaki kodu kullanın.

    > [!IMPORTANT]
    > Aşağıdaki bilgileri değiştirin:
    >
    > * __Ad alanı__’nı projenizin adına ayarlayın.
    > * Daha önce `client.DefaultRequestHeaders.Add("Prediction-Key",` ile başlayan satırda aldığınız __Tahmin Anahtarı__ değerini ayarlayın.
    > * Daha önce `string url =` ile başlayan satırda aldığınız __URL__ değerini ayarlayın.

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
                string url = "https://southcentralus.api.cognitive.microsoft.com/customvision/v1.0/prediction/d16e136c-5b0b-4b84-9341-6a3fff8fa7fe/image?iterationId=f4e573f6-9843-46db-8018-b01d034fd0f2";

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

Uygulamayı çalıştırırken bir görüntü dosyasının yolunu girersiniz. Görüntü, API’ye gönderilir ve sonuçlar bir JSON belgesi olarak döndürülür. Aşağıdaki JSON, yanıtın bir örneğidir

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

[Mobil kullanım için modeli dışarı aktarma](export-your-model.md)
