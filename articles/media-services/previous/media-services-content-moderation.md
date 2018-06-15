---
title: Azure medya içeriği yöneticiyi olası yetişkin ve saldırganlardan içeriği algılamak için kullanmak | Microsoft Docs
description: Video yönetimini olası yetişkin ve saldırganlardan içeriği videoları algılanmasına yardımcı olur.
services: media-services
documentationcenter: ''
author: sanjeev3
manager: mikemcca
editor: ''
ms.assetid: a245529f-3150-4afc-93ec-e40d8a6b761d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/06/2018
ms.author: sajagtap
ms.openlocfilehash: e44308f38a138c0e186e41fc8310f8b480cd4e09
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33788672"
---
# <a name="use-azure-media-content-moderator-to-detect-possible-adult-and-racy-content"></a>Azure medya içeriği denetleyici olası yetişkin ve saldırganlardan içeriği algılamak için kullanın

## <a name="overview"></a>Genel Bakış
**Azure medya içerik denetleyici** medya işlemcisi (MP) videolarınızı için makine destekli yönetimini kullanmanıza olanak sağlar. Örneğin, videoları olası yetişkin ve saldırganlardan içerikleri algılama ve işaretli içerik İnsan yönetimini ekipleriniz tarafından gözden geçirmek isteyebilirsiniz.

**Azure medya içerik denetleyici** MP şu anda önizlemede.

Bu makalede, ilgili ayrıntıları verir **Azure medya içerik denetleyici** ve Media Services SDK'sı ile .NET için nasıl kullanılacağını gösterir.

## <a name="content-moderator-input-files"></a>Denetleyici giriş dosyaları içerik
Video dosyaları. Şu anda aşağıdaki biçimlerden desteklenir: MP4, MOV ve WMV.

## <a name="content-moderator-output-files"></a>İçerik denetleyici çıktı dosyaları
JSON biçiminde aracılı çıktı görüntüleri otomatik olarak algılanır ve ana kareleri içerir. Ana kare olası Yetişkin veya saldırganlardan içerik güvenirlik puanlarını ile döndürülür. Ayrıca bir gözden geçirme önerilen olup olmadığını gösteren bir Boole bayrağı içerirler. Gözden geçirme öneri bayrağı yetişkin ve saldırganlardan puanları için iç eşiklerine dayalı değerler atanır.

## <a name="elements-of-the-output-json-file"></a>Çıkış JSON dosyasının öğeleri

İş algılanan görüntüleri ve ana kare ve yetişkin veya saldırganlardan içerik içerdikleri hakkında meta veriler içeren bir JSON çıktı dosyası oluşturur.

Çıkış JSON aşağıdaki öğeleri içerir:

### <a name="root-json-elements"></a>Kök JSON öğeleri

| Öğe | Açıklama |
| --- | --- |
| sürüm |İçerik denetleyici sürümü. |
| Zaman Çizelgesi |Videonun saniyede "çizgilerine". |
| uzaklık |Zaman damgaları zaman uzaklığı. Video API'leri 1.0 sürümünde, bu değer her zaman 0 olacaktır. Bu değer gelecekte değişebilir. |
| Kare hızı |Videonun Saniyedeki çerçeve sayısı. |
| Genişlik |Çıkış video çerçeve, piksel cinsinden genişliği.|
| Yükseklik |Çıkış video çerçeve, piksel cinsinden yüksekliği.|
| TotalDuration |"Çizgilerine" içinde video giriş süresi |
| [Parçaları](#fragments-json-elements) |Meta verileri ayarlama parçaları olarak adlandırılan farklı kesimler halinde öbekli. Her parça bir otomatik olarak algılanır görüntüsü başlangıç, süre, aralığı sayısı ve olay ' dir. |

### <a name="fragments-json-elements"></a>Parçaları JSON öğeleri

|Öğe|Açıklama|
|---|---|
| start |"Çizgilerine." ilk olay başlangıç saati |
| Süre |"Çizgilerine.", parçadaki uzunluğu |
| interval |Her olay girişi parçadaki "çizgilerine." içinde aralığı |
| [Olayları](#events-json-elements) |Her olay bir küçük temsil eder ve her klip algılandı ve bu süre içinde izlenen ana kareleri içerir. Olayların bir dizidir. Dış dizi bir zaman aralığı temsil eder. İç dizi 0 veya zamandaki o noktada daha fazla olayları oluşur.|

### <a name="events-json-elements"></a>Olayları JSON öğeleri

|Öğe|Açıklama|
|---|---|
| reviewRecommended | `true` veya `false` mı bağlı olarak **adultScore** veya **racyScore** iç eşiklerini aşan. |
| adultScore | 0,00 için 0.99 bir ölçekte olası yetişkinlere yönelik içeriğe güvenirlik puan. |
| racyScore | 0,00 için 0.99 bir ölçekte olası saldırganlardan içerik güvenirlik puan. |
| dizin | İlk kare bir ölçekte çerçeve dizini dizin son çerçeve dizini. |
| timestamp | "Çizgilerine." çerçevede konumu |
| shotIndex | Görüntüsü üst dizini. |


## <a name="content-moderation-quickstart-and-sample-output"></a>İçerik yönetimini quickstart ve örnek çıkış

### <a name="task-configuration-preset"></a>Görev yapılandırması (hazır)
Bir görev oluştururken **Azure medya içerik denetleyici**, bir yapılandırma hazır belirtmeniz gerekir. İçerik Yönetimi için yalnızca aşağıdaki yapılandırma hazır olur.

    {
      "version":"2.0"
    }

### <a name="net-code-sample"></a>.NET kodu örneği

Aşağıdaki .NET kod örneği, bir içerik denetleyici işini çalıştırmak için Media Services .NET SDK'sını kullanır. Hizmetleri varlık bir medya aracılı için videoyu içeren girdi olarak alır.
Bkz: [içerik denetleyici video quickstart](../../cognitive-services/Content-Moderator/video-moderation-api.md) için tam kaynak kodu ve Visual Studio projesi.


```csharp
    /// <summary>
    /// Run the Content Moderator job on the designated Asset from local file or blob storage
    /// </summary>
    /// <param name="asset"></param>
    static void RunContentModeratorJob(IAsset asset)
    {
        // Grab the presets
        string configuration = File.ReadAllText(CONTENT_MODERATOR_PRESET_FILE);

        // grab instance of Azure Media Content Moderator MP
        IMediaProcessor mp = _context.MediaProcessors.GetLatestMediaProcessorByName(MEDIA_PROCESSOR);

        // create Job with Content Moderator task
        IJob job = _context.Jobs.Create(String.Format("Content Moderator {0}",
                asset.AssetFiles.First() + "_" + Guid.NewGuid()));

        ITask contentModeratorTask = job.Tasks.AddNew("Adult and racy classifier task",
                mp, configuration,
                TaskOptions.None);
        contentModeratorTask.InputAssets.Add(asset);
        contentModeratorTask.OutputAssets.AddNew("Adult and racy classifier output",
            AssetCreationOptions.None);

        job.Submit();


        // Create progress printing and querying tasks
        Task progressPrintTask = new Task(() =>
        {
            IJob jobQuery = null;
            do
            {
                var progressContext = _context;
                jobQuery = progressContext.Jobs
                .Where(j => j.Id == job.Id)
                    .First();
                    Console.WriteLine(string.Format("{0}\t{1}",
                    DateTime.Now,
                    jobQuery.State));
                    Thread.Sleep(10000);
             }
             while (jobQuery.State != JobState.Finished &&
             jobQuery.State != JobState.Error &&
             jobQuery.State != JobState.Canceled);
        });
        progressPrintTask.Start();

        Task progressJobTask = job.GetExecutionProgressTask(
        CancellationToken.None);
        progressJobTask.Wait();

        // If job state is Error, the event handling 
        // method for job progress should log errors.  Here we check 
        // for error state and exit if needed.
        if (job.State == JobState.Error)
        {
            ErrorDetail error = job.Tasks.First().ErrorDetails.First();
            Console.WriteLine(string.Format("Error: {0}. {1}",
            error.Code,
            error.Message));
        }

        DownloadAsset(job.OutputMediaAssets.First(), OUTPUT_FOLDER);
    }

For the full source code and the Visual Studio project, check out the [Content Moderator video quickstart](../../cognitive-services/Content-Moderator/video-moderation-api.md).

### JSON output

The following example of a Content Moderator JSON output was truncated.

> [!NOTE]
> Location of a keyframe in seconds = timestamp/timescale

    {
    "version": 2,
    "timescale": 90000,
    "offset": 0,
    "framerate": 50,
    "width": 1280,
    "height": 720,
    "totalDuration": 18696321,
    "fragments": [
    {
      "start": 0,
      "duration": 18000
    },
    {
      "start": 18000,
      "duration": 3600,
      "interval": 3600,
      "events": [
        [
          {
            "reviewRecommended": false,
            "adultScore": 0.00001,
            "racyScore": 0.03077,
            "index": 5,
            "timestamp": 18000,
            "shotIndex": 0
          }
        ]
      ]
    },
    {
      "start": 18386372,
      "duration": 119149,
      "interval": 119149,
      "events": [
        [
          {
            "reviewRecommended": true,
            "adultScore": 0.00000,
            "racyScore": 0.91902,
            "index": 5085,
            "timestamp": 18386372,
            "shotIndex": 62
          }
        ]
      ]
    }
    ]
    }
```

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>İlgili bağlantılar
[Azure Media Services Analytics a genel bakış](media-services-analytics-overview.md)

[Azure medya analizi gösterileri](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

## <a name="next-steps"></a>Sonraki adımlar

İçerik denetleyicinin hakkında daha fazla bilgi [video denetleme ve gözden geçirme çözüm](../../cognitive-services/Content-Moderator/video-moderation-human-review.md).

Tam kaynak kodunu ve Visual Studio projeden alın [video yönetimini quickstart](../../cognitive-services/Content-Moderator/video-moderation-api.md). 

Nasıl oluşturulacağını öğrenin [video incelemeleri](../../cognitive-services/Content-Moderator/video-reviews-quickstart-dotnet.md) , aracılı çıktısından ve [Orta dökümleri](../../cognitive-services/Content-Moderator/video-transcript-reviews-quickstart-dotnet.md) .NET içinde.

Ayrıntılı .NET denetleyin [denetleme ve gözden geçirme videosu](../../cognitive-services/Content-Moderator/video-transcript-moderation-review-tutorial-dotnet.md). 
