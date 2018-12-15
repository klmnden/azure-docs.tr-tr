---
title: Azure Media Services işinde giriş yerel bir dosyadan oluştur | Microsoft Docs
description: Bu konuda, yerel bir dosyadan iş girdisi oluşturulacağını gösterir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: 9b366cca37e562e229e7d139426fc0b24978e366
ms.sourcegitcommit: b254db346732b64678419db428fd9eb200f3c3c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53412972"
---
# <a name="create-a-job-input-from-a-local-file"></a>Yerel bir dosyadan iş girdisi oluşturma

Media Services v3 sürümünde kullanarak videolarınızı işleyin işleri gönderdiğinizde Media Services giriş videosunun nerede bulacağını söylemeniz gerekir. Giriş videosunun, bu durumda, bir dosyayı (yerel olarak veya Azure Blob Depolama alanında depolanan) temel bir giriş varlığı oluşturma, medya hizmeti varlık depolanabilir. Bu konuda, yerel bir dosyadan iş girdisi oluşturulacağını gösterir. Tam bir örnek için bkz. Bu [GitHub örnek](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs).

## <a name="net-sample"></a>.NET örnek

Aşağıdaki kod, bir giriş varlık oluşturun ve iş için giriş olarak kullanma gösterilmektedir. CreateInputAsset işlevi aşağıdaki eylemleri gerçekleştirir:

* Varlığı oluşturur
* Varlığın [depolamadaki kapsayıcısına](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet?tabs=windows#upload-blobs-to-the-container) yazılabilir bir [SAS URL](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)’si alır
* SAS URL’sini kullanarak dosyayı depolamadaki kapsayıcıya yükler

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateInputAsset)]

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="next-steps"></a>Sonraki adımlar

[Bir HTTPS URL'si iş girdisi oluşturma](job-input-from-http-how-to.md).
