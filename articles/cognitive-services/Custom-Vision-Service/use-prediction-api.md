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
ms.date: 03/26/2019
ms.author: anroth
ms.openlocfilehash: 715fa526c83608c9922315e3a0d89b67b31e0d16
ms.sourcegitcommit: fbfe56f6069cba027b749076926317b254df65e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58472736"
---
#  <a name="use-your-model-with-the-prediction-api"></a>Modelinizi tahmin API'sini kullanma

Modelinizi eğittikten sonra görüntüleri Tahmin API’sine göndererek programlama yoluyla test edebilirsiniz.

> [!NOTE]
> Bu belgede, Tahmin API’sine görüntü göndermek için C# kullanımı gösterilmektedir. Daha fazla bilgi ve API kullanma örnekleri için bkz. [Tahmin API’si başvurusu](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Prediction_3.0/operations/5c82db60bf6a2b11a8247c15).

## <a name="publish-your-trained-iteration"></a>Eğitilen yinelemenizdeki yayımlama

[Özel Görüntü İşleme web sayfasından](https://customvision.ai) projenizi ve __Performans__ sekmesini seçin.

Prediction API'deki görüntülere göndermek için ilk seçerek yapılabilir tahmin için yineleme yayımlamak ihtiyacınız olacak __Yayımla__ ve yayımlanmış bir yineleme için bir ad belirtin. Bu, modelinizi tahmin API için özel görüntü işleme Azure kaynağınızın erişilebilir olmasını sağlayacaktır. 

![Yayımla düğmesine çevreleyen kırmızı bir dikdörtgen ile Performans sekmesinde gösterilir.](./media/use-prediction-api/unpublished-iteration.png)

Model başarıyla yayımlandıktan sonra sol kenar yanı sıra yineleme açıklamasını yayımlanan yinelemede adını yinelemede yanında görünen bir "Yayımlanmış" etiket görürsünüz.

![Performans sekmesi, yayımlanmış etiket ve yayımlanan yineleme adı çevreleyen kırmızı bir dikdörtgen gösterilir.](./media/use-prediction-api/published-iteration.png)

## <a name="get-the-url-and-prediction-key"></a>URL ve tahmin anahtarını alma

Modelinizi yayımlandıktan sonra seçerek Prediction API'deki kullanma hakkında bilgi alabileceğiniz __tahmin URL__. Bu tahmin API'sini kullanma hakkında bilgi ile aşağıda gösterilene benzer bir iletişim açar dahil olmak üzere __tahmin URL__ ve __tahmin anahtar__.

![Performans Sekmesi tahmin URL'yi düğmesini çevreleyen kırmızı bir dikdörtgen gösterilir.](./media/use-prediction-api/published-iteration-prediction-url.png)

![Performans Sekmesi tahmin URL değeri için bir resim dosyası ve tahmin anahtar değer kullanarak çevreleyen kırmızı bir dikdörtgen gösterilir.](./media/use-prediction-api/prediction-api-info.png)

> [!TIP]
> __Tahmin anahtar__ içinde bulunabilir [Azure portalı](https://portal.azure.com) Custom Vision Azure kaynağını projenize altında ilişkili sayfası __anahtarları__. 

İletişim kutusundan kullanmak için aşağıdaki bilgileri uygulamada kopyalayın:

* __Tahmin URL__ kullanmaya yönelik bir __görüntü dosyası__.
* __Tahmin-Key__ değeri.

## <a name="create-the-application"></a>Uygulama oluşturma

1. Visual Studio’dan yeni bir C# konsol uygulaması oluşturun.

1. __Program.cs__ dosyasının gövdesi olarak aşağıdaki kodu kullanın.

    > [!IMPORTANT]
    > Aşağıdaki bilgileri değiştirin:
    >
    > * __Ad alanı__’nı projenizin adına ayarlayın.
    > * Ayarlama __tahmin anahtar__ daha önce ile başlayan satırı içinde alınan değeri `client.DefaultRequestHeaders.Add("Prediction-Key",`.
    > * Ayarlama __tahmin URL__ daha önce ile başlayan satırı içinde alınan değeri `string url =`.

    ```csharp
    using System;
    using System.IO;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Threading.Tasks;

    namespace CVSPredictionSample
    {
        public static class Program
        {
            public static void Main()
            {
                Console.Write("Enter image file path: ");
                string imageFilePath = Console.ReadLine();

                MakePredictionRequest(imageFilePath).Wait();

                Console.WriteLine("\n\nHit ENTER to exit...");
                Console.ReadLine();
            }

            public static async Task MakePredictionRequest(string imageFilePath)
            {
                var client = new HttpClient();

                // Request headers - replace this example key with your valid Prediction-Key.
                client.DefaultRequestHeaders.Add("Prediction-Key", "3b9dde6d1ae1453a86bfeb1d945300f2");

                // Prediction URL - replace this example URL with your valid Prediction URL.
                string url = "https://southcentralus.api.cognitive.microsoft.com/customvision/v3.0/Prediction/8622c779-471c-4b6e-842c-67a11deffd7b/classify/iterations/Cats%20vs.%20Dogs%20-%20Published%20Iteration%203/image";

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

            private static byte[] GetImageAsByteArray(string imageFilePath)
            {
                FileStream fileStream = new FileStream(imageFilePath, FileMode.Open, FileAccess.Read);
                BinaryReader binaryReader = new BinaryReader(fileStream);
                return binaryReader.ReadBytes((int)fileStream.Length);
            }
        }
    }
    ```

## <a name="use-the-application"></a>Uygulamayı kullanma

Uygulama çalışırken, konsolunda bir görüntü dosyasının yolunu girer. Görüntü tahmin API'sine gönderilir ve tahmin sonuçları bir JSON belgesi olarak döndürülür. Aşağıdaki JSON yanıtı örneğidir.

```json
{
    "Id":"7796df8e-acbc-45fc-90b4-1b0c81b73639",
    "Project":"8622c779-471c-4b6e-842c-67a11deffd7b",
    "Iteration":"59ec199d-f3fb-443a-b708-4bca79e1b7f7",
    "Created":"2019-03-20T16:47:31.322Z",
    "Predictions":[
        {"TagId":"d9cb3fa5-1ff3-4e98-8d47-2ef42d7fb373","TagName":"cat", "Probability":1.0},
        {"TagId":"9a8d63fb-b6ed-4462-bcff-77ff72084d99","TagName":"dog", "Probability":0.1087869}
    ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

[Mobil kullanım için modeli dışarı aktarma](export-your-model.md)

[.NET SDK'ları ile çalışmaya başlama](csharp-tutorial.md)

[Python SDK'ları ile çalışmaya başlama](python-tutorial.md)

[Java SDK'ları ile çalışmaya başlama](java-tutorial.md)

[Düğüm SDK'ları ile çalışmaya başlama](node-tutorial.md)

[Go SDK ile çalışmaya başlama](go-tutorial.md)
