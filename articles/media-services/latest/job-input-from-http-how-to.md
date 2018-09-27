---
title: Azure Media Services işinde Giriş bir HTTPS URL'si oluşturun | Microsoft Docs
description: Bu konuda, bir HTTP (s) URL'SİNDEN iş girdisi oluşturulacağını gösterir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 09/24/2018
ms.author: juliako
ms.openlocfilehash: 8b8bfb578b9a7190e93da721531041bb93a9a03c
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47224741"
---
# <a name="create-a-job-input-from-an-https-url"></a>Bir HTTPS URL'si iş girdisi oluşturma

Media Services v3 sürümünde kullanarak videolarınızı işleyin işleri gönderdiğinizde Media Services giriş videosunun nerede bulacağını söylemeniz gerekir. Seçeneklerden birini (Bu örnekte gösterildiği gibi) giriş işi bir HTTP (s) URL belirtmek içindir. Şu anda AMS v3 HTTPS URL'leri öbekli aktarım kodlamasını desteklemediğini unutmayın. Tam bir örnek için bkz. Bu [github örnek](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs).

## <a name="net-sample"></a>.NET örnek

Aşağıdaki kod, Giriş bir HTTPS URL'si ile bir iş oluşturma işlemini gösterir.

[!code-csharp[Main](../../../media-services-v3-dotnet-quickstarts/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="next-steps"></a>Sonraki adımlar

[Yerel bir dosyadan iş girdisi oluşturma](job-input-from-local-file-how-to.md).
