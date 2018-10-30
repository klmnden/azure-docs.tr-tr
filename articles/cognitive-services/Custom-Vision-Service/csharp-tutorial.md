---
title: 'Öğretici: C# için Özel Görüntü İşleme SDK’sı ile görüntü sınıflandırma projesi oluşturma'
titlesuffix: Azure Cognitive Services
description: Bir proje oluşturun, etiketler ekleyin, görüntüleri karşıya yükleyin, projenizi eğitin ve varsayılan uç noktayı kullanarak bir tahminde bulunun.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: tutorial
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: e046fe452a13384ae7929be805c6252d6ad2fbf9
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49953052"
---
# <a name="tutorial-create-an-image-classification-project-with-the-custom-vision-sdk-for-c"></a>Öğretici: C# için Özel Görüntü İşleme SDK’sı ile görüntü sınıflandırma projesi oluşturma

C# uygulamasında Özel Görüntü İşleme Hizmeti SDK’sının nasıl kullanılacağını öğrenin. Oluşturulduktan sonra etiketler ekleyebilir, görüntüleri karşıya yükleyebilir, projeyi eğitebilir, projenin varsayılan tahmin uç nokta URL’sini alabilir ve bir görüntüyü programlama yoluyla test etmek için uç noktayı kullanabilirsiniz. Özel Görüntü İşleme API’sini kullanarak Windows için kendi uygulamanızı derlemek için şablon olarak bu açık kaynak örneği kullanın.

## <a name="prerequisites"></a>Ön koşullar

* Windows için Visual Studio 2017’nin herhangi bir sürümü.

## <a name="get-the-custom-vision-sdk-and-samples"></a>Özel Görüntü İşleme SDK’sını ve örneklerini alma
Bu örneği derlemek için Özel Görüntü İşleme SDK’sı NuGet Paketleri gerekir:

* [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training/)
* [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction/)

[C# Örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/CustomVision) ile birlikte görüntüleri indirebilirsiniz.

## <a name="get-the-training-and-prediction-keys"></a>Eğitim ve tahmin anahtarlarını alma

Bu örnekte kullanılan anahtarları almak için [Özel Görüntü İşleme web sayfasını](https://customvision.ai) ziyaret edin ve sağ üst kısımdaki __dişli simgesini__ seçin. __Hesaplar__ bölümünde, __Eğitim Anahtarı__ ve __Tahmin Anahtarı__ alanlarından değerleri kopyalayın.

![Anahtarlar kullanıcı arabiriminin görüntüsü](./media/csharp-tutorial/training-prediction-keys.png)

## <a name="understand-the-code"></a>Kodu anlama

Visual Studio’da, SDK projesinin `Samples/CustomVision.Sample/` dizininde bulunan projeyi açın.

Bu uygulama, __Yeni Projem__ adlı yeni bir proje oluşturmak için daha önce aldığınız eğitim anahtarını kullanır. Daha sonra bir sınıflandırıcıyı eğitip test etmek için görüntüleri karşıya yükler. Sınıflandırıcı, bir ağacın __Kanada Çamı__ mı yoksa __Japon Vişnesi__ mi olduğunu belirler.

Aşağıdaki kod parçacıkları, bu örneğin birincil işlevlerini uygular:

* __Yeni bir Özel Görüntü İşleme Hizmeti projesi oluşturma__:

    ```csharp
     // Create a new project
    Console.WriteLine("Creating new project:");
    var project = trainingApi.CreateProject("My New Project");
    ```

* __Projede etiketler oluşturma__:

    ```csharp
    // Make two tags in the new project
    var hemlockTag = trainingApi.CreateTag(project.Id, "Hemlock");
    var japaneseCherryTag = trainingApi.CreateTag(project.Id, "Japanese Cherry");
    ```

* __Görüntüleri karşıya yükleme ve etiketleme__:

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

* __Sınıflandırıcıyı eğitme__:

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

* __Tahmin uç noktası için varsayılan bir yineleme ayarlama__:

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
 
* __Tahmin uç noktasına görüntü gönderme__:

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

1. Uygulamaya eğitim ve tahmin anahtarları eklemek için aşağıdaki değişiklikleri yapın:

    * Aşağıdaki satıra __eğitim anahtarınızı__ ekleyin:

        ```csharp
        string trainingKey = "<your key here>";
        ```

    * Aşağıdaki satıra __tahmin anahtarınızı__ ekleyin:

        ```csharp
        string predictionKey = "<your key here>";
        ```

2. Uygulamayı çalıştırın. Uygulama çalıştırılırken, aşağıdaki çıktı konsola yazılır:

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
