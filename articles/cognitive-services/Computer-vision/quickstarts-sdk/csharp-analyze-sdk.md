---
title: "Hızlı Başlangıç: Görüntü analiz etme - C# SDK'sı - Görüntü İşleme"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta Görüntü İşleme Windows C# istemci kitaplığını kullanarak bir görüntüyü analiz edeceksiniz.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 09/14/2018
ms.author: pafarley
ms.openlocfilehash: 81a7b32ef2970efc7f53ec8d25350efb217d7b36
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49343662"
---
# <a name="quickstart-analyze-an-image-using-the-computer-vision-sdk-and-c"></a>Hızlı Başlangıç: Görüntü İşleme SDK'sını ve C# dilini kullanarak bir görüntüyü analiz etme

Bu hızlı başlangıçta, Görüntü İşleme Windows istemci kitaplığını kullanarak görsel özellikleri ayıklamak için yerel ve uzak bir görüntüyü analiz edeceksiniz.

## <a name="prerequisites"></a>Ön koşullar

* Görüntü İşleme’yi kullanmak için, bir abonelik anahtarınızın olması gerekir; bkz. [Abonelik Anahtarlarını Alma](../Vision-API-How-to-Topics/HowToSubscribe.md).
* [Visual Studio 2015 veya 2017](https://www.visualstudio.com/downloads/)'nin herhangi bir sürümü.
* [Microsoft.Azure.CognitiveServices.Vision.ComputerVision](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision) istemci kitaplığı NuGet paketi. Paketi indirmek gerekli değildir. Yükleme yönergeleri aşağıda verilmiştir.

## <a name="analyzeimageasync-method"></a>AnalyzeImageAsync metodu

> [!TIP]
> Güncel kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-vision-csharp-sdk-quickstarts/tree/master/ComputerVision)'dan Visual Studio çözümü olarak alın.

`AnalyzeImageAsync` ve `AnalyzeImageInStreamAsync` yöntemleri sırasıyla uzak ve yerel görüntüler için [Görüntü Analizi API’sini](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) sarmalar. Bu metotları kullanarak görüntü içeriğine göre görsel özellikleri ayıklayabilir ve aşağıdakiler gibi döndürülecek özellikleri seçebilirsiniz:

* Görüntü içeriğiyle ilgili etiketlerin ayrıntılı listesi.
* Görüntü içeriğinin tam bir cümlelik açıklaması.
* Görüntünün içerdiği yüzlerin koordinatları, cinsiyeti ve yaşı.
* ImageType (küçük resim veya çizgi çizim).
* Baskın renk, vurgu rengi veya bir görüntünün siyah beyaz olup olmadığı.
* Bu [sınıflandırmada](../Category-Taxonomy.md) tanımlanan kategori.
* Görüntü, yetişkinlere yönelik veya müstehcen cinsel içerik içeriyor mu?

Örneği çalıştırmak için aşağıdaki adımları uygulayın:

1. Visual Studio'da yeni bir Visual C# Konsol Uygulaması oluşturun.
1. Görüntü İşleme istemci kitaplığı NuGet paketini yükleyin.
    1. Menüde **Araçlar**’a tıklayın, **NuGet Paket Yöneticisi**’ni ve ardından **Çözüm için NuGet Paketlerini Yönet**’i seçin.
    1. **Gözat** sekmesine tıklayın ve **Arama** kutusuna "Microsoft.Azure.CognitiveServices.Vision.ComputerVision" yazın.
    1. Görüntülendiğinde **Microsoft.Azure.CognitiveServices.Vision.ComputerVision** öğesini seçin ve projenizin adının yanındaki onay kutusuna tıklayıp **Yükle**’ye tıklayın.
1. `Program.cs` öğesini aşağıdaki kodla değiştirin.
1. `<Subscription Key>` değerini geçerli abonelik anahtarınızla değiştirin.
1. Gerekirse `computerVision.Endpoint` değerini abonelik anahtarlarınız ile ilişkili Azure bölgesi ile değiştirin.
1. `<LocalImage>` değerini yerel görüntünün yolu ve dosya adıyla değiştirin.
1. İsteğe bağlı olarak, `remoteImageUrl` öğesini farklı bir görüntüye ayarlayın.
1. Programı çalıştırın.

```csharp
using Microsoft.Azure.CognitiveServices.Vision.ComputerVision;
using Microsoft.Azure.CognitiveServices.Vision.ComputerVision.Models;

using System;
using System.Collections.Generic;
using System.IO;
using System.Threading.Tasks;

namespace ImageAnalyze
{
    class Program
    {
        // subscriptionKey = "0123456789abcdef0123456789ABCDEF"
        private const string subscriptionKey = "<SubscriptionKey>";

        // localImagePath = @"C:\Documents\LocalImage.jpg"
        private const string localImagePath = @"<LocalImage>";

        private const string remoteImageUrl =
            "http://upload.wikimedia.org/wikipedia/commons/3/3c/Shaki_waterfall.jpg";

        // Specify the features to return
        private static readonly List<VisualFeatureTypes> features =
            new List<VisualFeatureTypes>()
        {
            VisualFeatureTypes.Categories, VisualFeatureTypes.Description,
            VisualFeatureTypes.Faces, VisualFeatureTypes.ImageType,
            VisualFeatureTypes.Tags
        };

        static void Main(string[] args)
        {
            ComputerVisionClient computerVision = new ComputerVisionClient(
                new ApiKeyServiceClientCredentials(subscriptionKey),
                new System.Net.Http.DelegatingHandler[] { });

            // You must use the same region as you used to get your subscription
            // keys. For example, if you got your subscription keys from westus,
            // replace "westcentralus" with "westus".
            //
            // Free trial subscription keys are generated in the westcentralus
            // region. If you use a free trial subscription key, you shouldn't
            // need to change the region.

            // Specify the Azure region
            computerVision.Endpoint = "https://westcentralus.api.cognitive.microsoft.com";

            Console.WriteLine("Images being analyzed ...");
            var t1 = AnalyzeRemoteAsync(computerVision, remoteImageUrl);
            var t2 = AnalyzeLocalAsync(computerVision, localImagePath);

            Task.WhenAll(t1, t2).Wait(5000);
            Console.WriteLine("Press ENTER to exit");
            Console.ReadLine();
        }

        // Analyze a remote image
        private static async Task AnalyzeRemoteAsync(
            ComputerVisionClient computerVision, string imageUrl)
        {
            if (!Uri.IsWellFormedUriString(imageUrl, UriKind.Absolute))
            {
                Console.WriteLine(
                    "\nInvalid remoteImageUrl:\n{0} \n", imageUrl);
                return;
            }

            ImageAnalysis analysis =
                await computerVision.AnalyzeImageAsync(imageUrl, features);
            DisplayResults(analysis, imageUrl);
        }

        // Analyze a local image
        private static async Task AnalyzeLocalAsync(
            ComputerVisionClient computerVision, string imagePath)
        {
            if (!File.Exists(imagePath))
            {
                Console.WriteLine(
                    "\nUnable to open or read localImagePath:\n{0} \n", imagePath);
                return;
            }

            using (Stream imageStream = File.OpenRead(imagePath))
            {
                ImageAnalysis analysis = await computerVision.AnalyzeImageInStreamAsync(
                    imageStream, features);
                DisplayResults(analysis, imagePath);
            }
        }

        // Display the most relevant caption for the image
        private static void DisplayResults(ImageAnalysis analysis, string imageUri)
        {
            Console.WriteLine(imageUri);
            Console.WriteLine(analysis.Description.Captions[0].Text + "\n");
        }
    }
}
```

## <a name="analyzeimageasync-response"></a>AnalyzeImageAsync yanıtı

Başarılı bir yanıtta her görüntü için en ilgili resim yazısı görüntülenecektir.

Ham JSON çıktısı örneği için bkz. [API Hızlı Başlangıçları: C# ile yerel görüntü analizi](../QuickStarts/CSharp-analyze.md#examine-the-response).

```
http://upload.wikimedia.org/wikipedia/commons/3/3c/Shaki_waterfall.jpg
a large waterfall over a rocky cliff
```

## <a name="next-steps"></a>Sonraki adımlar

Görüntü analiz etmek, ünlüleri ve önemli yerleri algılamak, küçük resim oluşturmak ve yazdırılan ve el yazısı metinleri ayıklamak için kullanılan Görüntü İşleme API'lerini keşfedin.

> [!div class="nextstepaction"]
> [Görüntü İşleme API'lerini keşfedin](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
