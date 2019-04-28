---
title: Media Services varlıklar - Azure bilgisayarınıza | Microsoft Docs
description: İlgili varlıklar bilgisayarınıza indirmek öğrenin. İçinde yazılan kod örneklerini C# ve .NET için Media Services SDK'sını kullanın.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 8908a1dd-3ffb-4f18-955d-4c8e2d82fc5d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 21fcc6ae09718ffbb22e1d438926586dd3cde71d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61465669"
---
# <a name="how-to-deliver-an-asset-by-download"></a>Nasıl yapılır: Bir varlık indirme ile teslim etme  
Bu makalede, medya varlıklar Media Services'e sunmaya yönelik seçenekler açıklanmaktadır. Çok sayıda uygulama senaryolarında Media Services içerik teslim edebilirsiniz. Kodlama sonra oluşturulan medya varlıkları veya akış Bulucusu kullanarak erişebilir. Bir içerik teslim ağı (CDN) kullanarak, Gelişmiş performans ve ölçeklenebilirlik için içerik sunabilirsiniz.

Bu örnekte, medya varlıkları Media Services'den yerel bilgisayarınıza indirmek gösterilmektedir. İş kimliği ve erişim ile Media Services hesabıyla ilişkili işleri kod sorgular, **OutputMediaAssets** (olan bir işin çalıştırılmasının sonuçlarını kümesi bir veya daha fazla çıkış medya varlıklarının) koleksiyonu. Bu örnek, bir işten Çıkış medya varlıklarının indirmek gösterilmektedir, ancak diğer varlıklar indirmek için aynı yaklaşımı uygulayabilirsiniz.

>[!NOTE]
>Farklı AMS ilkeleri için sınır 1.000.000 ilkedir (örneğin, Bulucu ilkesi veya ContentKeyAuthorizationPolicy için). Aynı günleri / erişim izinlerini, örneğin, ilkeleri kalmasına yerinde uzun bir süredir (karşıya yükleme olmayan ilkeler) yöneliktir bulucular için her zaman aynı ilke Kimliğini kullanın. Daha fazla bilgi için [bu makaleye](media-services-dotnet-manage-entities.md#limit-access-policies) bakın.

```csharp
    // Download the output asset of the specified job to a local folder.
    static IAsset DownloadAssetToLocal( string jobId, string outputFolder)
    {
        // This method illustrates how to download a single asset. 
        // However, you can iterate through the OutputAssets
        // collection, and download all assets if there are many. 

        // Get a reference to the job. 
        IJob job = GetJob(jobId);

        // Get a reference to the first output asset. If there were multiple 
        // output media assets you could iterate and handle each one.
        IAsset outputAsset = job.OutputMediaAssets[0];

        // Create a SAS locator to download the asset
        IAccessPolicy accessPolicy = _context.AccessPolicies.Create("File Download Policy", TimeSpan.FromDays(30), AccessPermissions.Read);
        ILocator locator = _context.Locators.CreateLocator(LocatorType.Sas, outputAsset, accessPolicy);

        BlobTransferClient blobTransfer = new BlobTransferClient
        {
            NumberOfConcurrentTransfers = 20,
            ParallelTransferThreadCount = 20
        };

        var downloadTasks = new List<Task>();
        foreach (IAssetFile outputFile in outputAsset.AssetFiles)
        {
            // Use the following event handler to check download progress.
            outputFile.DownloadProgressChanged += DownloadProgress;

            string localDownloadPath = Path.Combine(outputFolder, outputFile.Name);

            Console.WriteLine("File download path:  " + localDownloadPath);

            downloadTasks.Add(outputFile.DownloadAsync(Path.GetFullPath(localDownloadPath), blobTransfer, locator, CancellationToken.None));

            outputFile.DownloadProgressChanged -= DownloadProgress;
        }

        Task.WaitAll(downloadTasks.ToArray());

        return outputAsset;
    }

    static void DownloadProgress(object sender, DownloadProgressChangedEventArgs e)
    {
        Console.WriteLine(string.Format("{0} % download progress. ", e.Progress));
    }
```


## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.
[İçerik akışı sağlama](media-services-deliver-streaming-content.md)

