---
title: Azure medya Hyperlapse ile medya dosyalarını | Microsoft Docs
description: Azure medya Hyperlapse kesintisiz zaman aralıklı videoları birinci kişi veya eylem kamera içeriği oluşturur. Bu konu, Media Indexer'ı kullanmayı gösterir.
services: media-services
documentationcenter: ''
author: asolanki
manager: johndeu
editor: ''
ms.assetid: 37d54db6-9cf3-4ae9-b3c6-0d29c744e965
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/28/2018
ms.author: adsolank
ms.openlocfilehash: 3b361202d2d80f879ae6d23ec29782bc6f837f96
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53632883"
---
# <a name="hyperlapse-media-files-with-azure-media-hyperlapse"></a>Azure medya Hyperlapse ile medya dosyalarını

> [!NOTE]
> Azure Media Services'ın bu önizleme özelliği yakında kullanımdan kaldırılacaktır. 19 Aralık 2018 tarihinden itibaren Media Services artık değişiklikleri veya geliştirmeleri için medya Hyperlapse hale getirir. 29 Mart 2019, kullanımdan kaldırılmış ve artık kullanılabilir olacaktır.

Azure medya Hyperlapse bir medya işlemci (ilk kişi veya eylem kamera içerikten kesintisiz zaman aralıklı videoları oluşturan MP) olur.  Bulut tabanlı eşdüzeye [Microsoft Research'ün telefon tabanlı Hyperlapse mobil ve Hyperlapse Pro Masaüstü](https://aka.ms/hyperlapse), Azure Media Services için Microsoft Hyperlapse yararlanan ölçekleri Azure Media Services medya işleme platform yatay ölçeklendirme ve paralel hale getirmek için Hyperlapse işleme toplu.

> [!IMPORTANT]
> Microsoft Hyperlapse birinci kişi içeriği taşıma kamera ile en iyi çalışacak şekilde tasarlanmıştır. Hala kamerası kayıtları çalışmaya devam ancak diğer içerik türleri için Azure medya Hyperlapse medya işleyicisini kalitesini ve performansını garanti edilemez.
> 
> 

İşin Sürüyor olarak bir Azure medya Hyperlapse Giriş bir MP4, MOV veya WMV varlık dosyası kareler video zaman aralıklı olmalıdır belirten bir yapılandırma dosyası ile birlikte ve hangi hızı (örneğin ilk 10.000 çerçeve 2 x).  Titreşimsiz ve zaman aralıklı bir işleme giriş video çıktıdır.

## <a name="hyperlapse-an-asset"></a>Hyperlapse bir varlık
İlk Azure Media Services için istenen giriş dosyanız karşıya gerekecektir.  Karşıya yükleme ve içeriği yönetme ile ilgili kavramları hakkında daha fazla bilgi edinmek için [içerik yönetimi makale](media-services-portal-vod-get-started.md).

### <a id="configuration"></a>Hyperlapse için yapılandırma hazır
İçeriğinizi Media Services hesabınızı olduktan sonra yapılandırma Önayarı oluşturmak gerekir.  Aşağıdaki tabloda, kullanıcı tanımlı alanlar açıklanmaktadır:

| Alan | Açıklama |
| --- | --- |
| StartFrame |Microsoft Hyperlapse işleme başlatılması gereken çerçeve. |
| NumFrames |İşlemek için çerçeve sayısı |
| Hız |Hangi giriş videosunu hızlandırma katsayısını faktörü. |

Bir uyumlu yapılandırma dosyasında XML ve JSON örneği verilmiştir:

**XML hazır:**
```xml
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
        <Sources>
            <Source StartFrame="0" NumFrames="10000" />
        </Sources>
        <Options>
            <Speed>12</Speed>
        </Options>
    </Preset>
```

**JSON hazır:**
```json
    {
        "Version":1.0,
        "Sources": [
            {
                "StartFrame":0,
                "NumFrames":2147483647
            }
        ],
        "Options": {
            "Speed":1,
            "Stabilize":false
        }
    }
```

### <a id="sample_code"></a> AMS .NET SDK ile Microsoft Hyperlapse
Aşağıdaki yöntem bir varlık olarak bir medya dosyasını yükler ve bir iş ile Azure medya Hyperlapse medya işleyicisini oluşturur.

> [!NOTE]
> Kapsamdaki bu kodun çalışması "içerik" adı ile bir CloudMediaContext olmanız gerekir.  Bunun hakkında daha fazla bilgi edinmek için [içerik yönetimi makale](media-services-dotnet-get-started.md).
> 
> [!NOTE]
> Dize bağımsız değişkeni "hyperConfig", JSON veya XML yukarıda açıklandığı gibi önceden uyumluluğunu yapılandırma olması beklenir.
> 
> 

```csharp
        static bool RunHyperlapseJob(string input, string output, string hyperConfig)
        {
            // create asset with input file
            IAsset asset = context
            .Assets
            .CreateAssetAndUploadSingleFile(input, "My Hyperlapse Input", AssetCreationOptions.None);

            // grab instances of Azure Media Hyperlapse MP
            IMediaProcessor mp = context
            .MediaProcessors
            .GetLatestMediaProcessorByName("Azure Media Hyperlapse");

            // create Job with Hyperlapse task
            IJob job = context
            .Jobs
            .Create(String.Format("Hyperlapse {0}", input));

            if (String.IsNullOrEmpty(hyperConfig))
            {
            // config cannot be empty
            return false;
            }

            hyperConfig = File.ReadAllText(hyperConfig);

            ITask hyperlapseTask = job.Tasks.AddNew("Hyperlapse task",
            mp,
            hyperConfig,
            TaskOptions.None);
            hyperlapseTask.InputAssets.Add(asset);
            hyperlapseTask.OutputAssets.AddNew("Hyperlapse output",
            AssetCreationOptions.None);

            job.Submit();

            // Create progress printing and querying tasks
            Task progressPrintTask = new Task(() =>
            {

            IJob jobQuery = null;
            do
            {
                var progressContext = context;
                jobQuery = progressContext.Jobs
                .Where(j => j.Id == job.Id)
                .First();
                Console.WriteLine(string.Format("{0}\t{1}\t{2}",
                DateTime.Now,
                jobQuery.State,
                jobQuery.Tasks[0].Progress));
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
                return false;                  
            }

        DownloadAsset(job.OutputMediaAssets.First(), output);
        return true;
    }

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }


    static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
    {
        IAsset asset = context.Assets.Create(assetName, options);

        var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
        assetFile.Upload(filePath);

        return asset;
    }

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = context.MediaProcessors
        .Where(p => p.Name == mediaProcessorName)
        .ToList()
        .OrderBy(p => new Version(p.Version))
        .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }
```

### <a id="file_types"></a>Desteklenen dosya türleri
* MP4
* MOV
* WMV

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>İlgili bağlantılar
[Azure Media Services Analizi'ne genel bakış](media-services-analytics-overview.md)

[Azure medya analizi tanıtımları](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

