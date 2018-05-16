---
title: Yerel bir dosyadan bir Azure Media Services işi girdi oluştur | Microsoft Docs
description: Bu konuda yerel bir dosyadan bir iş girişi oluşturulacağını gösterir.
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
ms.openlocfilehash: 94e7192e13397ad8ec973d92f4c538f430c9cd60
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="create-a-job-input-from-a-local-file"></a>Yerel bir dosyadan bir iş girişi oluştur

Media Services v3 kullanarak videolarınızı işleyin işleri gönderdiğinizde Media Services giriş video nerede bulabileceğinizi bildiren sahip. Giriş video (yerel olarak veya Azure Blob storage'da depolanır) bir dosyayı temel bir giriş varlığı oluşturun; bu durumda medya hizmeti varlık depolanabilir. Bu konuda yerel bir dosyadan bir iş girişi oluşturulacağını gösterir. Bu tam bir örnek için bkz [github örnek](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs).

## <a name="net-sample"></a>.NET örnek

Aşağıdaki kod, bir giriş varlığı oluşturun ve iş için giriş olarak kullanmak üzere gösterilmiştir. CreateInputAsset işlevi aşağıdaki eylemleri gerçekleştirir:

* Varlık oluşturur 
* Varlığın [depolamadaki kapsayıcısına](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet?tabs=windows#upload-blobs-to-the-container) yazılabilir bir [SAS URL](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)’si alır
* SAS URL’sini kullanarak dosyayı depolamadaki kapsayıcıya yükler

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateInputAsset)]

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="next-steps"></a>Sonraki adımlar

[Bir HTTP (s) URL'SİNDEN bir iş girişi oluşturma](job-input-from-http-how-to.md).
