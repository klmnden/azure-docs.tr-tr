---
title: Media Services v3 .NET SDK - Azure kullanarak mevcut ilkeden bir imzalama anahtarı alın | Microsoft Docs
description: Bu konu, Media Services v3 .NET SDK'sını kullanarak mevcut ilkeden bir imzalama anahtarı almak nasıl gösterir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: seodec18
ms.date: 04/15/2019
ms.author: juliako
ms.openlocfilehash: 58b6f49f4bbbd93fefb9b616f92baf7ef30f7deb
ms.sourcegitcommit: fec96500757e55e7716892ddff9a187f61ae81f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59615838"
---
# <a name="get-a-signing-key-from-the-existing-policy"></a>Mevcut ilkeden bir imzalama anahtarı alma

v3 API’nin temel tasarım ilkelerinden biri API’yi daha güvenli hale getirmektir. V3 API'ler döndürmeyen parolaları veya kimlik üzerinde **alma** veya **listesi** operations. Ayrıntılı açıklama buraya bakın: Daha fazla bilgi için [RBAC ve Media Services hesapları](rbac-overview.md)

Bu makaledeki örnek .NET mevcut ilkeden bir imzalama anahtarı almak için nasıl kullanılacağını gösterir. 
 
## <a name="download"></a>İndirme 

Aşağıdaki komutu kullanarak makinenize tam .NET örneği içeren bir GitHub deposunu kopyalayın:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```
 
Gizli dizileri örnekle ContentKeyPolicy bulunan [EncryptWithDRM](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/tree/master/AMSV3Tutorials/EncryptWithDRM) klasör.

## <a name="get-contentkeypolicy-with-secrets"></a>Gizli diziler ContentKeyPolicy Al 

Anahtarı almak için kullanın **GetPolicyPropertiesWithSecretsAsync**aşağıdaki örnekte gösterildiği gibi.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="next-steps"></a>Sonraki adımlar

[Erişim denetimi ile çoklu DRM'ye sahip içerik koruma sistemi tasarlama](design-multi-drm-system-with-access-control.md) 
