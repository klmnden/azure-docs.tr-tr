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
ms.date: 09/25/2018
ms.author: juliako
ms.openlocfilehash: 66bd03b03289f568c019588f1b8ac1317ab9c076
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47222037"
---
# <a name="create-a-job-input-from-a-local-file"></a>Yerel bir dosyadan iş girdisi oluşturma

Media Services v3 sürümünde kullanarak videolarınızı işleyin işleri gönderdiğinizde Media Services giriş videosunun nerede bulacağını söylemeniz gerekir. Giriş videosunun, bu durumda, bir dosyayı (yerel olarak veya Azure Blob Depolama alanında depolanan) temel bir giriş varlığı oluşturma, medya hizmeti varlık depolanabilir. Bu konuda, yerel bir dosyadan iş girdisi oluşturulacağını gösterir. Tam bir örnek için bkz. Bu [github örnek](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs).

## <a name="net-sample"></a>.NET örnek

Aşağıdaki kod, bir giriş varlık oluşturun ve iş için giriş olarak kullanma gösterilmektedir. CreateInputAsset işlevi aşağıdaki eylemleri gerçekleştirir:

* Varlığı oluşturur 
* Varlığın [depolamadaki kapsayıcısına](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet?tabs=windows#upload-blobs-to-the-container) yazılabilir bir [SAS URL](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)’si alır
* SAS URL’sini kullanarak dosyayı depolamadaki kapsayıcıya yükler

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateInputAsset)]

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="next-steps"></a>Sonraki adımlar

[Bir HTTPS URL'si iş girdisi oluşturma](job-input-from-http-how-to.md).
