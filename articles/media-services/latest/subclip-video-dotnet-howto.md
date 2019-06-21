---
title: Alt klip bir video Azure Media Services ile kodlama
description: Bu konuda bir video .NET SDK kullanarak Azure Media Services ile kodlama, alt klip açıklar
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 06/09/2019
ms.author: juliako
ms.openlocfilehash: 3d584ee742aa93cdecf4b04d942afb2ed83a7357
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67305058"
---
# <a name="subclip-a-video-when-encoding-with-media-services---net"></a>Alt klip medya Hizmetleri - .NET kodlama, video

Trim ya da bir video kullanarak kodlama, alt klip bir [iş](https://docs.microsoft.com/rest/api/media/jobs). Bu işlev ile çalışır [dönüştürme](https://docs.microsoft.com/rest/api/media/transforms) kullanarak oluşturulan [BuiltInStandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset) hazır veya [StandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset) hazır.

Aşağıdaki C# örnek, bir kodlama işi gönderir gibi bir varlık videoda kırpar bir iş oluşturur. 

## <a name="prerequisites"></a>Önkoşullar

Bu konu başlığı altında açıklanan adımları tamamlamak için için gerekenler:

- [Azure Media Services hesabı oluşturma](create-account-cli-how-to.md)
- Dönüşüm ve bir girişi oluşturun ve varlıkları çıktı. Dönüşüm ve girdi ve çıktı varlıkları nasıl oluşturulacağını görebilirsiniz [karşıya yükleme, kodlama ve .NET kullanarak video akışı](stream-files-tutorial-with-api.md) öğretici.
- Gözden geçirme [kodlama kavramı](encoding-concept.md) konu.

## <a name="example"></a>Örnek

```csharp
/// <summary>
/// Submits a request to Media Services to apply the specified Transform to a given input video.
/// </summary>
/// <param name="client">The Media Services client.</param>
/// <param name="resourceGroupName">The name of the resource group within the Azure subscription.</param>
/// <param name="accountName"> The Media Services account name.</param>
/// <param name="transformName">The name of the transform.</param>
/// <param name="jobName">The (unique) name of the job.</param>
/// <param name="inputAssetName">The name of the input asset.</param>
/// <param name="outputAssetName">The (unique) name of the  output asset that will store the result of the encoding job. </param>
// <SubmitJob>
private static async Task<Job> JobWithBuiltInStandardEncoderWithSingleClipAsync(
    IAzureMediaServicesClient client,
    string resourceGroupName,
    string accountName,
    string transformName,
    string jobName,
    string inputAssetName,
    string outputAssetName)
{
    var jobOutputs = new List<JobOutputAsset>
    {
        new JobOutputAsset(state: JobState.Queued, progress: 0, assetName: outputAssetName)
    };

    var clipStart = new AbsoluteClipTime()
    {
        Time = new TimeSpan(0, 0, 20)
    };

    var clipEnd = new AbsoluteClipTime()
    {
        Time = new TimeSpan(0, 0, 30)
    };

    var jobInput = new JobInputAsset(assetName: inputAssetName, start: clipStart, end: clipEnd);

    Job job = await client.Jobs.CreateAsync(
        resourceGroupName,
        accountName,
        transformName,
        jobName,
        new Job(input: jobInput, outputs: jobOutputs.ToArray(), name: jobName)
        {
            Description = $"A Job with transform {transformName} and single clip.",
            Priority = Priority.Normal,
        });

    return job;
}
```

## <a name="next-steps"></a>Sonraki adımlar

[Özel bir dönüşüm ile kodlama](customize-encoder-presets-how-to.md) 
