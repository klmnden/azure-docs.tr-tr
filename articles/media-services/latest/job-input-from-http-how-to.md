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
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: bac11640f5256223c1053f03da42c763508af98f
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49377387"
---
# <a name="create-a-job-input-from-an-https-url"></a>Bir HTTPS URL'si iş girdisi oluşturma

Media Services v3 sürümünde kullanarak videolarınızı işleyin işleri gönderdiğinizde Media Services giriş videosunun nerede bulacağını söylemeniz gerekir. Seçeneklerden birini (Bu örnekte gösterildiği gibi) giriş işi bir HTTP (s) URL belirtmek içindir. AMS v3’ün şu anda HTTPS URL'leri üzerinden yığın halinde aktarım kodlamasını desteklemediğini unutmayın. Tam bir örnek için bkz. Bu [github örnek](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs).

## <a name="net-sample"></a>.NET örnek

Aşağıdaki kod, Giriş bir HTTPS URL'si ile bir iş oluşturma işlemini gösterir.

[!code-csharp[Main](../../../media-services-v3-dotnet-quickstarts/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="next-steps"></a>Sonraki adımlar

[Yerel bir dosyadan iş girdisi oluşturma](job-input-from-local-file-how-to.md).
