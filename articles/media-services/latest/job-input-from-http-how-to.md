---
title: Bir HTTP (s) URL'SİNDEN bir Azure Media Services işi girişi oluşturun | Microsoft Docs
description: Bu konuda, bir HTTP (s) URL'SİNDEN iş giriş oluşturulacağını gösterir.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: a47dc3e4939dd27d7ac11be823a09b8a6cba1f60
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="create-a-job-input-from-an-https-url"></a>Bir HTTP (s) URL'SİNDEN bir iş girişi oluşturun

Media Services v3 kullanarak videolarınızı işleyin işleri gönderdiğinizde Media Services giriş video nerede bulabileceğinizi bildiren sahip. Seçeneklerden birini bir HTTP (s) URL'si (Bu örnekte gösterildiği gibi) giriş işi belirtmektir. Bu tam bir örnek için bkz [github örnek](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs).

## <a name="net-sample"></a>.NET örnek

Aşağıdaki kod bir HTTPS giriş URL'si ile bir iş oluşturulacağını gösterir.

```csharp
private static Job SubmitJob(IAzureMediaServicesClient client, string transformName, string jobName)
{
    Asset outputAsset = client.Assets.CreateOrUpdate(Guid.NewGuid().ToString() + "-output", new Asset());

    JobInputHttp jobInput = 
        new JobInputHttp(files: new[] { "https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/Ignite-short.mp4" });

    JobOutput[] jobOutputs =
    {
        new JobOutputAsset(outputAsset.Name),
    };

    Job job = client.Jobs.CreateOrUpdate(
        transformName,
        jobName,
        new Job
        {
            Input = jobInput,
            Outputs = jobOutputs,
        });

    return job;
}
```

## <a name="next-steps"></a>Sonraki adımlar

[Yerel bir dosyadan bir iş girişi oluşturma](job-input-from-local-file-how-to.md).
