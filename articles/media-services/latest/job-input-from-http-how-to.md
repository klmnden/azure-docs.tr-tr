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
ms.openlocfilehash: d429665de64dacc5818d1d26c2a9029531cd39b3
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="create-a-job-input-from-an-https-url"></a>Bir HTTP (s) URL'SİNDEN bir iş girişi oluşturun

Media Services v3 kullanarak videolarınızı işleyin işleri gönderdiğinizde Media Services giriş video nerede bulabileceğinizi bildiren sahip. Seçeneklerden birini bir HTTP (s) URL'si (Bu örnekte gösterildiği gibi) giriş işi belirtmektir. Bu tam bir örnek için bkz [github örnek](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs).

## <a name="net-sample"></a>.NET örnek

Aşağıdaki kod bir HTTPS giriş URL'si ile bir iş oluşturulacağını gösterir.

[!code-csharp[Main](../../../media-services-v3-dotnet-quickstarts/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="next-steps"></a>Sonraki adımlar

[Yerel bir dosyadan bir iş girişi oluşturma](job-input-from-local-file-how-to.md).
