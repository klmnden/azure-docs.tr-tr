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
ms.date: 02/13/2019
ms.author: juliako
ms.openlocfilehash: f6eee912bb3bba112bd13969f1a8d9cb5748e387
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65413823"
---
# <a name="create-a-job-input-from-an-https-url"></a>Bir HTTPS URL'si iş girdisi oluşturma

Media Services v3 sürümünde işlenecek İşleri videolarınıza gönderirken Media Services'a giriş videosunun yerini de bildirmeniz gerekir. Seçeneklerden birini (Bu örnekte gösterildiği gibi) giriş işi bir HTTPS URL'si belirtmek içindir. AMS v3’ün şu anda HTTPS URL'leri üzerinden yığın halinde aktarım kodlamasını desteklemediğini unutmayın. Tam bir örnek için bkz. Bu [GitHub örnek](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs).

> [!TIP]
> Geliştirmeye başlamadan önce gözden [Media Services v3 API'leri ile geliştirme](media-services-apis-overview.md) (API, adlandırma kurallarını, vb. erişme hakkında bilgi içerir.)

## <a name="net-sample"></a>.NET örnek

Aşağıdaki kod, Giriş bir HTTPS URL'si ile bir iş oluşturma işlemini gösterir.

[!code-csharp[Main](../../../media-services-v3-dotnet-quickstarts/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="job-error-codes"></a>İş hata kodları

Bkz: [hata kodları](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode).

## <a name="next-steps"></a>Sonraki adımlar

[Yerel bir dosyadan iş girdisi oluşturma](job-input-from-local-file-how-to.md).
