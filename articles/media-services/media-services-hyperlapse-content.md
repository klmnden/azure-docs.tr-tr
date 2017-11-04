---
title: "Azure medya Hyperlapse ile medya dosyalarını | Microsoft Docs"
description: "Azure medya Hyperlapse kesintisiz zaman onlara videolar ilk kişi veya eylem kamera içeriğini oluşturur. Bu konu, Media Indexer kullanmayı gösterir."
services: media-services
documentationcenter: 
author: asolanki
manager: johndeu
editor: 
ms.assetid: 37d54db6-9cf3-4ae9-b3c6-0d29c744e965
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/02/2017
ms.author: adsolank
ms.openlocfilehash: 02f634c2af04b6b372642ab0e6a17a5d29f16450
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="hyperlapse-media-files-with-azure-media-hyperlapse"></a>Azure medya Hyperlapse ile medya dosyalarını
Azure medya Hyperlapse bir medya işlemci (ilk kişi veya eylem kamera içerikten kesintisiz zaman onlara videolar oluşturan MP) olur.  Bulut tabanlı eşdüzeyi [Microsoft Research'ın masaüstü Hyperlapse Pro ve telefon tabanlı Hyperlapse Mobile](http://aka.ms/hyperlapse), Microsoft Hyperlapse yatay ölçeklendirmenizi ve paralel hale Azure Media Services medya işleme platform yoğun ölçeğini Azure Media Services yararlanan için Hyperlapse toplu işleme.

> [!IMPORTANT]
> Microsoft Hyperlapse en iyi ilk kişinin içeriğine taşıma kamera ile çalışmak üzere tasarlanmıştır.  Halen kamera görüntülerinin çalışmaya devam ancak performansına ve kalitesine Azure medya Hyperlapse medya işlemcisi diğer içerik türleri için garanti edilemez.  Azure Media Services için Microsoft Hyperlapse hakkında daha fazla bilgi edinmek ve bazı örnek videolar görmek için kullanıma [giriş blog gönderisi](http://aka.ms/azurehyperlapseblog) public preview sürümünden.
> 
> 

İş geçen olarak bir Azure medya Hyperlapse Giriş bir MP4, MOV veya WMV varlık dosyası videonun hangi çerçeveler zaman onlara olmalıdır belirten bir yapılandırma dosyası ile birlikte ve hangi hızı (örn. ilk 10.000 çerçeve 2 x).  Çıktı, sabit ve zaman onlara yorumlama video giriş şeklindedir.

En son Azure medya Hyperlapse güncelleştirmeler için bkz: [Media Services bloglar](https://azure.microsoft.com/blog/topics/media-services/).

## <a name="hyperlapse-an-asset"></a>Hyperlapse bir varlığı
İlk Azure Media Services için istenen giriş dosyanızı karşıya yüklemek gerekir.  Karşıya yükleme ve içeriği yönetme ile ilgili kavramları hakkında daha fazla bilgi için okuma [içerik yönetimi makale](media-services-portal-vod-get-started.md).

### <a id="configuration"></a>Hyperlapse yapılandırma hazır
İçeriğinizi Media Services hesabınızı eklendiğinde, yapılandırma hazır oluşturmak gerekir.  Aşağıdaki tabloda, kullanıcı tanımlı alanlar açıklanmaktadır:

| Alan | Açıklama |
| --- | --- |
| StartFrame |Bağlı Microsoft Hyperlapse işleme başlaması gereken çerçeve. |
| NumFrames |İşlenecek çerçeve sayısı |
| Hız |Hangi giriş video hızlandırmak faktörü. |

XML ve JSON uyumluluğunu yapılandırma dosyası örneği verilmiştir:

**XML hazır:**

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
        <Sources>
            <Source StartFrame="0" NumFrames="10000" />
        </Sources>
        <Options>
            <Speed>12</Speed>
        </Options>
    </Preset>

**JSON hazır:**

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

### <a id="sample_code"></a>Microsoft Hyperlapse AMS .NET SDK'sı
Aşağıdaki yöntem Azure medya Hyperlapse medya işlemcisi bir işi oluşturur ve bir medya dosyası bir varlık olarak yükler.

> [!NOTE]
> Bu kodun çalışması "context" adı ile kapsamda zaten bir CloudMediaContext yüklü olmalıdır.  Bunun hakkında daha fazla bilgi edinmek için okuma [içerik yönetimi makale](media-services-dotnet-get-started.md).
> 
> [!NOTE]
> Dize bağımsız değişkeni "hyperConfig" JSON veya XML yukarıda açıklandığı gibi önceden belirlenmiş bir uyumluluğunu yapılandırma olması beklenir.
> 
> 

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

### <a id="file_types"></a>Desteklenen dosya türleri
* MP4
* MOV
* WMV

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>İlgili bağlantılar
[Azure Media Services Analytics a genel bakış](media-services-analytics-overview.md)

[Azure medya analizi gösterileri](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

