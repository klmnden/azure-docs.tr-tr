---
title: 'Hızlı Başlangıç: C# ile .NET SDK’sını kullanarak bir görüntüdeki yüzleri algılama'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Bilişsel Hizmetler’de Yüz Tanıma Windows C# istemci kitaplığını kullanarak bir görüntüdeki yüzleri algılayacaksınız.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: quickstart
ms.date: 09/14/2018
ms.author: pafarley
ms.openlocfilehash: a4b0b8b277ed6bc6e2bc3c7549d1e67d5f18c615
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49954972"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-net-sdk-with-c"></a>Hızlı Başlangıç: C# ile .NET SDK’sını kullanarak bir görüntüdeki yüzleri algılama

Bu hızlı başlangıçta, Yüz Tanıma Windows istemci kitaplığını kullanarak bir görüntüdeki insan yüzlerini algılayacaksınız.

## <a name="prerequisites"></a>Ön koşullar

* Örneği çalıştırmanız için bir abonelik anahtarınız olmalıdır. [Bilişsel Hizmetleri Deneme](https://azure.microsoft.com/try/cognitive-services/?api=face-api)'den ücretsiz deneme abonelik anahtarları alabilirsiniz.
* [Visual Studio 2017](https://www.visualstudio.com/downloads/)’nin herhangi bir sürümü.
* [Microsoft.Azure.CognitiveServices.Vision.Face 2.2.0-preview](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.2.0-preview) istemci kitaplığı NuGet paketi. Paketi indirmek gerekli değildir. Yükleme yönergeleri aşağıda verilmiştir.

## <a name="detectwithurlasync-method"></a>DetectWithUrlAsync yöntemi

> [!TIP]
> Güncel kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-vision-csharp-sdk-quickstarts/tree/master/Face)'dan Visual Studio çözümü olarak alın.

`DetectWithUrlAsync` ve `DetectWithStreamAsync` yöntemleri, sırasıyla uzak ve yerel görüntüler için [Yüz - Algılama API’sini](https://westcentralus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) sarmalar. Bu yöntemleri kullanarak bir görüntüdeki yüzleri algılayabilir ve aşağıdaki yüz özniteliklerini döndürebilirsiniz:

* Face ID: Birkaç Yüz Tanıma API'si senaryosunda kullanılan benzersiz kimlik.
* Yüz Dikdörtgeni: Görüntüdeki yüzün konumunu gösteren sol kısım, üst kısım, genişlik ve yükseklik.
* Yer İşaretleri: Yüz bileşenlerinin önemli konumlarına işaret eden 27 noktalık yüz yer işareti dizisi.
* Yaş, cinsiyet, gülümseme yoğunluğu, kafanın duruşu ve sakal ve bıyık gibi yüzdeki öznitelikler.

Örneği çalıştırmak için aşağıdaki adımları uygulayın:

1. Visual Studio'da yeni bir Visual C# Konsol Uygulaması oluşturun.
1. Yüz Tanıma istemci kitaplığı NuGet paketini yükleyin.
    1. Üst menüde **Araçlar**’a tıklayın, **NuGet Paket Yöneticisi**’ni ve ardından **Çözüm için NuGet Paketlerini Yönet**’i seçin.
    1. **Gözat** sekmesine tıklayın ve **Ön sürümü dahil et**’i seçin.
    1. **Ara** kutusuna "Microsoft.Azure.CognitiveServices.Vision.Face" yazın.
    1. Görüntülendiğinde **Microsoft.Azure.CognitiveServices.Vision.Face** seçeneğini belirleyin ve sonra proje adınızın yanındaki onay kutusuna ve **Yükleyin**.
1. *Program.cs* öğesini aşağıdaki kodla değiştirin.
1. `<Subscription Key>` değerini geçerli abonelik anahtarınızla değiştirin.
1. Gerekirse `faceEndpoint` değerini abonelik anahtarlarınız ile ilişkili Azure bölgesi ile değiştirin.
1. İsteğe bağlı olarak, <`LocalImage>` değerini yerel bir görüntünün yolu ve dosya adı ile değiştirin (ayarlanmazsa yoksayılır).
1. İsteğe bağlı olarak, `remoteImageUrl` öğesini farklı bir görüntüye ayarlayın.
1. Programı çalıştırın.

```csharp
using Microsoft.Azure.CognitiveServices.Vision.Face;
using Microsoft.Azure.CognitiveServices.Vision.Face.Models;

using System;
using System.Collections.Generic;
using System.IO;
using System.Threading.Tasks;

namespace DetectFace
{
    class Program
    {
        // subscriptionKey = "0123456789abcdef0123456789ABCDEF"
        private const string subscriptionKey = "<SubscriptionKey>";

        // You must use the same region as you used to get your subscription
        // keys. For example, if you got your subscription keys from westus,
        // replace "westcentralus" with "westus".
        //
        // Free trial subscription keys are generated in the westcentralus
        // region. If you use a free trial subscription key, you shouldn't
        // need to change the region.
        // Specify the Azure region
        private const string faceEndpoint =
            "https://westcentralus.api.cognitive.microsoft.com";

        // localImagePath = @"C:\Documents\LocalImage.jpg"
        private const string localImagePath = @"<LocalImage>";

        private const string remoteImageUrl =
            "https://upload.wikimedia.org/wikipedia/commons/3/37/Dagestani_man_and_woman.jpg";

        private static readonly FaceAttributeType[] faceAttributes =
            { FaceAttributeType.Age, FaceAttributeType.Gender };

        static void Main(string[] args)
        {
            FaceClient faceClient = new FaceClient(
                new ApiKeyServiceClientCredentials(subscriptionKey),
                new System.Net.Http.DelegatingHandler[] { });
            faceClient.Endpoint = faceEndpoint;

            Console.WriteLine("Faces being detected ...");
            var t1 = DetectRemoteAsync(faceClient, remoteImageUrl);
            var t2 = DetectLocalAsync(faceClient, localImagePath);

            Task.WhenAll(t1, t2).Wait(5000);
            Console.WriteLine("Press any key to exit");
            Console.ReadLine();
        }

        // Detect faces in a remote image
        private static async Task DetectRemoteAsync(
            FaceClient faceClient, string imageUrl)
        {
            if (!Uri.IsWellFormedUriString(imageUrl, UriKind.Absolute))
            {
                Console.WriteLine("\nInvalid remoteImageUrl:\n{0} \n", imageUrl);
                return;
            }

            try
            {
                IList<DetectedFace> faceList =
                    await faceClient.Face.DetectWithUrlAsync(
                        imageUrl, true, false, faceAttributes);

                DisplayAttributes(GetFaceAttributes(faceList, imageUrl), imageUrl);
            }
            catch (APIErrorException e)
            {
                Console.WriteLine(imageUrl + ": " + e.Message);
            }
        }

        // Detect faces in a local image
        private static async Task DetectLocalAsync(FaceClient faceClient, string imagePath)
        {
            if (!File.Exists(imagePath))
            {
                Console.WriteLine(
                    "\nUnable to open or read localImagePath:\n{0} \n", imagePath);
                return;
            }

            try
            {
                using (Stream imageStream = File.OpenRead(imagePath))
                {
                    IList<DetectedFace> faceList =
                            await faceClient.Face.DetectWithStreamAsync(
                                imageStream, true, false, faceAttributes);
                    DisplayAttributes(
                        GetFaceAttributes(faceList, imagePath), imagePath);
                }
            }
            catch (APIErrorException e)
            {
                Console.WriteLine(imagePath + ": " + e.Message);
            }
        }

        private static string GetFaceAttributes(
            IList<DetectedFace> faceList, string imagePath)
        {
            string attributes = string.Empty;

            foreach (DetectedFace face in faceList)
            {
                double? age = face.FaceAttributes.Age;
                string gender = face.FaceAttributes.Gender.ToString();
                attributes += gender + " " + age + "   ";
            }

            return attributes;
        }

        // Display the face attributes
        private static void DisplayAttributes(string attributes, string imageUri)
        {
            Console.WriteLine(imageUri);
            Console.WriteLine(attributes + "\n");
        }
    }
}
```

### <a name="detectwithurlasync-response"></a>DetectWithUrlAsync yanıtı

Başarılı bir yanıt, görüntüdeki her bir yüz için cinsiyeti ve yaşı görüntüler.

Ham JSON çıktısı örneği için bkz. [API Hızlı Başlangıçları: C# kullanarak bir görüntüdeki yüzleri algılama](CSharp.md).

```
https://upload.wikimedia.org/wikipedia/commons/3/37/Dagestani_man_and_woman.jpg
Male 37   Female 56
```

## <a name="next-steps"></a>Sonraki adımlar

Bir görüntüde yüzleri algılamak için Yüz Tanıma hizmetini kullanan bir WPF Windows uygulamasının nasıl oluşturulacağını öğreneceksiniz. Uygulama, her yüzün çevresine bir çerçeve çizer ve durum çubuğunda yüzün açıklamasını görüntüler.

> [!div class="nextstepaction"]
> [Öğretici: Görüntüdeki yüzleri algılamak ve çerçeve içine almak için WPF uygulaması oluşturma](../Tutorials/FaceAPIinCSharpTutorial.md)
