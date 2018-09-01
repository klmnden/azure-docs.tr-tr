---
title: Custom Vision Service'e tahmin uç - Azure Bilişsel hizmetler | Microsoft Docs
description: Program aracılığıyla özel görüntü işleme hizmeti sınıflandırıcınızı görüntülerle test etmek için API'sini kullanmayı öğrenin.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: d7f9b90db06811e16cd0cd6ad2b32a27912cfee5
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43341802"
---
# <a name="use-the-prediction-endpoint-to-test-images-programmatically-with-a-custom-vision-service-classifier"></a>Custom Vision Service'e sınıflandırıcı program aracılığıyla görüntülerle test etmek için tahmin uç noktası kullan

Modelinizi eğitin sonra görüntüleri programlı olarak tahmin API'ye göndererek test edebilirsiniz. 

> [!NOTE]
> Bu belge, C# Prediction API'deki görüntüye göndermek için kullanmayı gösterir. Daha fazla bilgi ve API kullanma örnekleri için bkz. [tahmin API Başvurusu](https://go.microsoft.com/fwlink/?linkid=865445).

## <a name="get-the-url-and-prediction-key"></a>URL ve tahmin anahtarını alma

Gelen [Custom Vision web sayfası](https://customvision.ai), projenizi seçin ve ardından __performans__ sekmesi. Tahmin API'sini kullanma hakkında bilgi görüntülemek için de dahil olmak üzere __tahmin anahtar__seçin __tahmin URL__. Bir Azure kaynağına bağlı projeleri için __tahmin anahtar__ içinde bulunabilir [Azure portalı](https://portal.azure.com) altında ilişkili Azure kaynak sayfası __anahtarları__. Aşağıdaki bilgileri kullanmak için uygulamaya kopyalayın:

* __URL__ kullanmaya yönelik bir __görüntü dosyası__.
* __Tahmin-key__ değeri.

> [!TIP]
> Çoklu yinelemelere varsa, varsayılan ayarlayarak hangisinin kullanılan kontrol edebilirsiniz. Yinelemeden seçin __yinelemeler__ bölümüne ve ardından __varsayılan yap__ sayfanın üstünde.

![Performans Sekmesi tahmin URL çevreleyen kırmızı bir dikdörtgen gösterilir.](./media/use-prediction-api/prediction-url.png)

## <a name="create-the-application"></a>Uygulama oluşturma

1. Visual Studio'dan yeni bir C# konsol uygulaması oluşturun.

2. Aşağıdaki kodu kullanın gövdesi olarak __Program.cs__ dosya.

    > [!IMPORTANT]
    > Aşağıdaki bilgileri değiştirin:
    >
    > * Ayarlama __ad alanı__ projenizin adı.
    > * Ayarlama __tahmin anahtar__ daha önce ile başlayan satırı içinde alınan değeri `client.DefaultRequestHeaders.Add("Prediction-Key",`.
    > * Ayarlama __URL__ daha önce ile başlayan satırı içinde alınan değeri `string url =`.

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

Uygulama çalışırken, bir görüntü dosyasına yolunu girin. Görüntü API'sine gönderilir ve sonuçları bir JSON belgesi olarak döndürülür. Aşağıdaki JSON yanıtı örneğidir

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

[Mobil kullanım için modelini dışarı aktarma](export-your-model.md)
