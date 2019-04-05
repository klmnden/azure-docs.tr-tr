---
title: Program aracılığıyla sınıflandırıcı - özel görüntü işleme görüntülerle test etmek için tahmin uç noktası kullan
titlesuffix: Azure Cognitive Services
description: Özel Görüntü İşleme sınıflandırıcınızla programlama yoluyla görüntüleri test etmek için API’nin nasıl kullanılacağını öğrenin.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: article
ms.date: 04/02/2019
ms.author: anroth
ms.openlocfilehash: 1ee6edbf49bbcd2014afcf29ed3b737168a3b5bc
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59046079"
---
# <a name="use-your-model-with-the-prediction-api"></a>Modelinizi tahmin API'sini kullanma

Modelinizi olduğunuz eğitme sonra görüntüleri programlı olarak tahmin API uç noktasına göndererek test edebilirsiniz.

> [!NOTE]
> Bu belgede, Tahmin API’sine görüntü göndermek için C# kullanımı gösterilmektedir. Daha fazla bilgi ve örnekler için bkz. [tahmin API Başvurusu](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Prediction_3.0/operations/5c82db60bf6a2b11a8247c15).

## <a name="publish-your-trained-iteration"></a>Eğitilen yinelemenizdeki yayımlama

[Özel Görüntü İşleme web sayfasından](https://customvision.ai) projenizi ve __Performans__ sekmesini seçin.

Prediction API'deki görüntülere göndermek için ilk seçerek yapılabilir tahmin için yineleme yayımlamak ihtiyacınız olacak __Yayımla__ ve yayımlanmış bir yineleme için bir ad belirtin. Bu model Custom Vision Azure kaynağınızın Prediction API'deki için erişilebilir hale getirir.

![Yayımla düğmesine çevreleyen kırmızı bir dikdörtgen ile Performans sekmesinde gösterilir.](./media/use-prediction-api/unpublished-iteration.png)

Modelinizi başarıyla yayımlandıktan sonra sol kenar çubuğunda yineleme yanında görünen bir "Yayımlanmış" etiket görürsünüz ve adını yineleme açıklamasında görüntülenir.

![Performans sekmesi, yayımlanmış etiket ve yayımlanan yineleme adı çevreleyen kırmızı bir dikdörtgen gösterilir.](./media/use-prediction-api/published-iteration.png)

## <a name="get-the-url-and-prediction-key"></a>URL ve tahmin anahtarını alma

Modelinizi yayımlandıktan sonra seçerek gerekli bilgileri alabileceğiniz __tahmin URL__. Bu yedekleme Prediction API'deki kullanmaya yönelik bilgileri içeren bir iletişim kutusu açar dahil olmak üzere __tahmin URL__ ve __tahmin anahtar__.

![Performans Sekmesi tahmin URL'yi düğmesini çevreleyen kırmızı bir dikdörtgen gösterilir.](./media/use-prediction-api/published-iteration-prediction-url.png)

![Performans Sekmesi tahmin URL değeri için bir resim dosyası ve tahmin anahtar değer kullanarak çevreleyen kırmızı bir dikdörtgen gösterilir.](./media/use-prediction-api/prediction-api-info.png)

> [!TIP]
> __Tahmin anahtar__ içinde bulunabilir [Azure portalında](https://portal.azure.com) özel görüntü işleme Azure kaynak altında projenizle ilişkili sayfası __anahtarları__ dikey penceresi.

Bu kılavuzda, yerel görüntüyü kullanın, böylece altında URL'yi kopyalayın **bir resim dosyası varsa** geçici bir konuma. Buna karşılık gelen kopyalama __tahmin anahtar__ değeri de.

## <a name="create-the-application"></a>Uygulama oluşturma

1. Visual Studio'da yeni bir oluşturma C# konsol uygulaması.

1. __Program.cs__ dosyasının gövdesi olarak aşağıdaki kodu kullanın.

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
                client.DefaultRequestHeaders.Add("Prediction-Key", "<Your prediction key>");

                // Prediction URL - replace this example URL with your valid Prediction URL.
                string url = "<Your prediction URL>";

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

1. Aşağıdaki bilgileri değiştirin:
   * Ayarlama `namespace` projenizin ad alanı.
   * Yer tutucusunu değiştirin `<Your prediction key>` daha önce aldığınız anahtar değere sahip.
   * Yer tutucusunu değiştirin `<Your prediction URL>` daha önce aldığınız URL.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı çalıştırdığınızda, konsolunda bir görüntü dosyasının yolunu girin istenir. Görüntü ardından tahmin API'sine gönderilir ve tahmin sonuçlarını JSON biçimli bir dize olarak döndürülür. Bir yanıt örneği verilmiştir.

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

Bu kılavuzda, özel görüntüleri görüntü sınıflandırıcı/algılayıcısı ve bir yanıt ile programlı olarak gönderme işleminin nasıl yapılacağını öğrendiniz C# SDK. Ardından, uçtan uca senaryolar ile öğrenin C#, veya farklı bir dil SDK'sını kullanmaya başlayın.

* [Hızlı Başlangıç: .NET SDK'sı](csharp-tutorial.md)
* [Hızlı Başlangıç: Python SDK'sı](python-tutorial.md)
* [Hızlı Başlangıç: Java SDK](java-tutorial.md)
* [Hızlı Başlangıç: Node SDK](node-tutorial.md)
* [Hızlı Başlangıç: Go SDK'sı](go-tutorial.md)
