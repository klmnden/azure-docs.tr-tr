---
title: Bir C# uygulamasından - Azure Bilişsel hizmetler Custom Vision Service'e kullanın | Microsoft Docs
description: Microsoft Bilişsel hizmetler özel görüntü işleme API'sini kullanan basit bir C# uygulaması keşfedin. Bir proje oluşturun, etiketler ekleyin, görüntüleri karşıya yüklemek, projenizi eğitmek ve varsayılan uç nokta kullanarak bir tahminde bulunmak.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: d3c2ffb0fd9578458bd07241eed4a87cf70d3c3c
ms.sourcegitcommit: a62cbb539c056fe9fcd5108d0b63487bd149d5c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2018
ms.locfileid: "42617443"
---
# <a name="use-the-custom-vision-service-from-a-c35-application"></a>Custom Vision Service'e c kullanın&#35; uygulama

Bir C# uygulamasından özel görüntü işleme hizmeti kullanmayı öğrenin. Oluşturulduktan sonra etiketler ekleyin, görüntüleri karşıya yüklemek, proje eğitmek, projenin varsayılan tahmin uç nokta URL'si almak ve program aracılığıyla resim test etmek için uç noktayı kullanın. Bu açık kaynaklı örneği, özel görüntü işleme hizmeti API'sini kullanarak Windows için kendi uygulamanızı oluşturmaya yönelik şablon olarak kullanın.

## <a name="prerequisites"></a>Önkoşullar

* Windows için Visual Studio 2017'in herhangi bir sürümü.

## <a name="get-the-custom-vision-sdk-and-samples"></a>Custom Vision SDK'sı ve örnekleri edinin
Bu örneği oluşturmak için özel görüntü işleme SDK'sı NuGet paketleri gerekir:

* [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training/)
* [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction/)

Görüntüleri ile birlikte indirebilirsiniz [C# örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/CustomVision).

## <a name="get-the-training-and-prediction-keys"></a>Eğitim ve tahmin anahtarları alma

Bu örnekte kullanılan anahtarlarını almak için şurayı ziyaret edin [Custom Vision web sayfası](https://customvision.ai) seçip __dişli simgesini__ sağ üst köşedeki. İçinde __hesapları__ bölümünde, değerleri kopyalayın __eğitim anahtarı__ ve __tahmin anahtar__ alanları.

![UI anahtarları görüntüsü](./media/csharp-tutorial/training-prediction-keys.png)

## <a name="understand-the-code"></a>Kodu anlama

Visual Studio'da bulunan proje açmak `Samples/CustomVision.Sample/` SDK proje dizininde.

Bu uygulamayı aldığınız önceki adlı yeni bir proje oluşturmak için eğitim anahtar kullanan __Yeni Projem__. Ardından, eğitmek ve bir sınıflandırıcı test etmek için görüntüleri yükler. Sınıflandırıcı bir ağaç olup olmadığını tanımlayan bir __köknar__ veya __Japonca tek tek seçme işlemi__.

Aşağıdaki kod parçacıkları, bu örnekte birincil işlevselliğini uygular:

* __Yeni özel görüntü işleme hizmeti projesi oluşturma__:

    ```csharp
     // Create a new project
    Console.WriteLine("Creating new project:");
    var project = trainingApi.CreateProject("My New Project");
    ```

* __Etiketler, bir proje oluşturma__:

    ```csharp
    // Make two tags in the new project
    var hemlockTag = trainingApi.CreateTag(project.Id, "Hemlock");
    var japaneseCherryTag = trainingApi.CreateTag(project.Id, "Japanese Cherry");
    ```

* __Karşıya yükleme ve etiket görüntüleri__:

    ```csharp
    // Add some images to the tags
    Console.WriteLine("\tUploading images");
    LoadImagesFromDisk();

    // Images can be uploaded one at a time
    foreach (var image in hemlockImages)
    {
        using (var stream = new MemoryStream(File.ReadAllBytes(image)))
        {
            trainingApi.CreateImagesFromData(project.Id, stream, new List<string>() { hemlockTag.Id.ToString() });
        }
    }

    // Or uploaded in a single batch 
    var imageFiles = japaneseCherryImages.Select(img => new ImageFileCreateEntry(Path.GetFileName(img), File.ReadAllBytes(img))).ToList();
    trainingApi.CreateImagesFromFiles(project.Id, new ImageFileCreateBatch(imageFiles, new List<Guid>() { japaneseCherryTag.Id }));
    ```

* __Sınıflandırıcı eğitme__:

    ```csharp
    // Now there are images with tags start training the project
    Console.WriteLine("\tTraining");
    var iteration = trainingApi.TrainProject(project.Id);

    // The returned iteration will be in progress, and can be queried periodically to see when it has completed
    while (iteration.Status == "Completed")
    {
        Thread.Sleep(1000);

        // Re-query the iteration to get it's updated status
        iteration = trainingApi.GetIteration(project.Id, iteration.Id);
    }
    ```

* __Varsayılan yineleme tahmin uç noktası için ayarlanmış__:

    ```csharp
    // The iteration is now trained. Make it the default project endpoint
    iteration.IsDefault = true;
    trainingApi.UpdateIteration(project.Id, iteration.Id, iteration);
    Console.WriteLine("Done!\n");
    ```

* __Tahmin uç noktası oluşturma__:
 
    ```csharp
    // Create a prediction endpoint, passing in obtained prediction key
    PredictionEndpoint endpoint = new PredictionEndpoint() { ApiKey = predictionKey };
    ```
 
* __Görüntü tahmin uç noktasına göndermesi__:

    ```csharp
    // Make a prediction against the new project
    Console.WriteLine("Making a prediction:");
    var result = endpoint.PredictImage(project.Id, testImage);

    // Loop over each prediction and write out the results
    foreach (var c in result.Predictions)
    {
        Console.WriteLine($"\t{c.TagName}: {c.Probability:P1}");
    }
    ```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

1. Eğitim ve tahmin anahtarları uygulama eklemek için aşağıdaki değişiklikleri yapın:

    * Ekleme, __eğitim anahtar__ aşağıdaki satırı:

        ```csharp
        string trainingKey = "<your key here>";
        ```

    * Ekleme, __tahmin anahtar__ aşağıdaki satırı:

        ```csharp
        string predictionKey = "<your key here>";
        ```

2. Uygulamayı çalıştırın. Uygulama çalışırken, aşağıdaki çıktıyı konsola yazılır:

    ```
    Creating new project:
            Uploading images
            Training
    Done!

    Making a prediction:
            Hemlock: 95.0%
            Japanese Cherry: 0.0%
    ```

3. Uygulamadan çıkmak için herhangi bir tuşa basın.
