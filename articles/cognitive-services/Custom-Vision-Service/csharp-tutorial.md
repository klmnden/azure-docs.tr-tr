---
title: Özel görme Hizmeti'yle bir C# uygulamasından - Azure Bilişsel hizmetler | Microsoft Docs
description: Microsoft Bilişsel Hizmetleri'nde özel görme API'sini kullanan temel bir C# uygulama keşfedin. Bir proje oluşturun, etiket ekleme, resimler yükleyin, projenizin eğitmek ve varsayılan uç nokta kullanarak tahminde bulunmak.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: 80cb022808748ed2c60dff7c363d6020cb4043a8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352870"
---
# <a name="use-the-custom-vision-service-from-a-c35-application"></a>C özel görme Hizmeti'yle&#35; uygulama

Bir C# uygulaması özel görme hizmetinden kullanmayı öğrenin. Oluşturulduktan sonra etiket ekleme, resimler yükleyin, proje Eğitimi, projenin varsayılan tahmin uç noktasının URL'sini elde etmek ve program aracılığıyla bir görüntü sınamak için uç nokta kullanın. Bu açık kaynak örneği özel görme hizmet API'sini kullanarak Windows için kendi uygulamanızı oluşturmak için şablon olarak kullanın.

## <a name="prerequisites"></a>Önkoşullar

* Herhangi bir sürümünü Visual Studio 2015 veya Windows için 2017.

* [Özel görme hizmeti SDK'sını](http://github.com/Microsoft/Cognitive-CustomVision-Windows/). Bu örnek ve bu belgede kullanılan görüntülerini içerir.

## <a name="get-the-training-and-prediction-keys"></a>Eğitim ve tahmin anahtarı alma

Bu örnekte kullanılan anahtarlarını almak için şurayı ziyaret edin [özel görme web sayfası](https://customvision.ai) seçip __dişli simgesi__ sağ üst. İçinde __hesapları__ bölümünde, değerleri kopyalamak __eğitim anahtar__ ve __tahmin anahtar__ alanları.

![UI anahtarları görüntüsü](./media/csharp-tutorial/training-prediction-keys.png)

## <a name="understand-the-code"></a>Kodu anlama

Visual Studio'da bulunan projesini açın `Samples/CustomVision.Sample/` SDK proje dizininde.

Bu uygulama, alınan önceki adlı yeni bir proje oluşturmak için eğitim anahtar kullanan __My yeni proje__. Sonra eğitmek ve bir sınıflandırıcı sınamak için görüntüleri yükler. Bir ağaç olup sınıflandırıcı tanımlayan bir __köknar__ veya __Japonca tek tek__.

Aşağıdaki kod parçacıkları Bu örnek birincil işlevlerini uygular:

* __Yeni bir özel görme hizmet projesi oluşturun__:

    ```csharp
     // Create a new project
    Console.WriteLine("Creating new project:");
    var project = trainingApi.CreateProject("My New Project");
    ```

* __Bir proje ile etiketler oluşturma__:

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

* __Sınıflandırıcı eğitmek__:

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
 
* __Resim tahmin uç noktasına gönderme__:

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

1. Uygulama eğitim ve tahmin anahtarları eklemek için aşağıdaki değişiklikleri yapın:

    * Ekle, __eğitim anahtar__ aşağıdaki satırı için:

        ```csharp
        string trainingKey = "<your key here>";
        ```

    * Ekle, __tahmin anahtar__ aşağıdaki satırı için:

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
