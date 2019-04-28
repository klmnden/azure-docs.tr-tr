---
title: Azure medya Content Moderator, olası yetişkinlere yönelik ve müstehcen içerikleri algılama kullanmasını | Microsoft Docs
description: Video denetimi videoları olası yetişkinlere yönelik ve müstehcen içerikleri algılama yardımcı olur.
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
ms.date: 03/14/2019
ms.author: sajagtap
ms.openlocfilehash: eb16f5e1e72e5a9379ad530ab9677adba2ccbbcd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61465686"
---
# <a name="use-azure-media-content-moderator-to-detect-possible-adult-and-racy-content"></a>Azure medya Content Moderator, olası yetişkinlere yönelik ve müstehcen içeriğin algılamak için kullanın 

## <a name="overview"></a>Genel Bakış
**Azure medya Content Moderator** medya işlemci (MP) videolarınız için makine Yardımlı resim denetimi kullanmanıza olanak sağlar. Örneğin videolardaki yetişkinlere yönelik veya müstehcen içerikleri tespit edip belirlenen içeriklerin moderasyon ekibiniz tarafından gözden geçirilmesini isteyebilirsiniz.

**Azure medya Content Moderator** MP şu anda Önizleme aşamasındadır.

Bu makalede, ilgili ayrıntıları verir. **Azure medya Content Moderator** ve .NET için Media Services SDK ile kullanma işlemi gösterilmektedir.

## <a name="content-moderator-input-files"></a>Content Moderator giriş dosyaları
Video dosyaları. Şu anda aşağıdaki biçimleri desteklenir: MP4 MOV ve WMV.

## <a name="content-moderator-output-files"></a>Content Moderator çıktı dosyaları
JSON biçiminde yönetilen çıktı görüntüleri otomatik olarak algılanır ve ana kareleri içerir. Ana kareler olası yetişkinlere yönelik veya müstehcen içerik için güven puanları ile döndürülür. Ayrıca bir gözden geçirme önerilen olup olmadığını belirten bir Boole bayrağı içerirler. Gözden geçirme öneri bayrağı, yetişkinlere yönelik ve müstehcen puanları için iç eşiklerine dayalı değerler atanır.

## <a name="elements-of-the-output-json-file"></a>Çıkış JSON dosyasının öğeleri

İş algılanan görüntüleri ve ana kareler ve yetişkinlere yönelik veya müstehcen içerik içerdikleri hakkında meta veriler içeren bir JSON çıktı dosyası üretir.

Çıkış JSON aşağıdaki öğeleri içerir:

### <a name="root-json-elements"></a>Kök JSON öğeleri

| Öğe | Açıklama |
| --- | --- |
| version |Content Moderator sürümü. |
| Zaman Çizelgesi |Videoyu saniye başına "ticks". |
| uzaklık |Zaman damgası için saat farkıdır. Video API'leri 1.0 sürümünde, bu değer her zaman 0 olacaktır. Bu değer gelecekte değişebilir. |
| kare hızı |Videodaki saniye başına kare hızı. |
| Genişlik |Piksel cinsinden çıkış video çerçevenin genişliği.|
| Yükseklik |Çerçevenin çıkış video, piksel cinsinden yüksekliği.|
| TotalDuration |Video "dalgalanmasındaki." giriş süresi |
| [parçaları](#fragments-json-elements) |Meta verileri ayarlama parçaları olarak adlandırılan farklı parçalara öbekli. Her parça bir başlangıç süresi, aralığı sayısı ve olaylar ile otomatik olarak algılanan bir görüntüsü olur. |

### <a name="fragments-json-elements"></a>Parçaları JSON öğeleri

|Öğe|Açıklama|
|---|---|
| start |"Ticks." ilk olay başlangıç saati |
| süre |Uzunluğu parçasında "ticks." |
| interval |Her olay girişi parçasında "ticks." içinde aralığı |
| [Olayları](#events-json-elements) |Her olay, bir küçük resim temsil eder ve her küçük algılandı ve bu süre içinde izlenen ana kareleri içerir. Olayların bir dizidir. Dış dizi bir zaman aralığını temsil eder. İç dizi belirtilen noktada gerçekleşen 0 veya daha fazla olaydan oluşur.|

### <a name="events-json-elements"></a>Olayları JSON öğeleri

|Öğe|Açıklama|
|---|---|
| reviewRecommended | `true` veya `false` bağlı olarak **adultScore** veya **racyScore** iç eşiklerini aşan. |
| adultScore | Güvenilirlik puanı 0,00 için 0.99 ölçeğinde olası yetişkinlere yönelik içeriği. |
| racyScore | Güvenilirlik puanı 0,00 için 0.99 ölçeğinde olası müstehcen içerik. |
| index | Dizin çerçevenin bir ölçekte ilk çerçeve çerçeve dizinini son dizin. |
| timestamp | Konumu çerçevede "ticks." |
| shotIndex | Görüntüsü üst dizini. |


## <a name="content-moderation-quickstart-and-sample-output"></a>İçerik denetleme hızlı ve örnek çıktı

### <a name="task-configuration-preset"></a>Görev yapılandırması (hazır)
Bir görev oluştururken **Azure medya Content Moderator**, yapılandırma hazır belirtmeniz gerekir. İçerik denetleme için yalnızca aşağıdaki yapılandırma hazır olur.

    {
      "version":"2.0"
    }

### <a name="net-code-sample"></a>.NET kodu örneği

Aşağıdaki .NET kod örneği, Content Moderator işi çalıştırmak için Media Services .NET SDK'sını kullanır. Bu bir medya Hizmetleri varlık içeren aracılı için videoyu bir girdi olarak alır.
Bkz: [Content Moderator video hızlı](../../cognitive-services/Content-Moderator/video-moderation-api.md) tam kaynak kodunu ve Visual Studio projesi.


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
[Azure Media Services Analizi'ne genel bakış](media-services-analytics-overview.md)

[Azure medya analizi tanıtımları](https://azuremedialabs.azurewebsites.net/demos/Analytics.html)

## <a name="next-steps"></a>Sonraki adımlar

Content Moderator'ın hakkında daha fazla bilgi [video denetimi ve İnceleme çözüm](../../cognitive-services/Content-Moderator/video-moderation-human-review.md).

Tam kaynak kodunu ve Visual Studio projesinden [video denetimi hızlı](../../cognitive-services/Content-Moderator/video-moderation-api.md). 

Nasıl oluşturacağınızı öğrenin [video incelemeleri](../../cognitive-services/Content-Moderator/video-reviews-quickstart-dotnet.md) , denetlenen çıktısından ve [Orta dökümleri](../../cognitive-services/Content-Moderator/video-transcript-reviews-quickstart-dotnet.md) .NET içinde.

Ayrıntılı .NET denetleyin [denetimi ve İnceleme videosu](../../cognitive-services/Content-Moderator/video-transcript-moderation-review-tutorial-dotnet.md). 
