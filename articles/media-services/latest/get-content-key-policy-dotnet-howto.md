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
ms.date: 12/08/2018
ms.author: juliako
ms.openlocfilehash: 882f4650c0a3d558ee06c96658b779f9f0c76f76
ms.sourcegitcommit: 3ba9bb78e35c3c3c3c8991b64282f5001fd0a67b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2019
ms.locfileid: "54322508"
---
# <a name="get-a-signing-key-from-the-existing-policy"></a>Mevcut ilkeden bir imzalama anahtarı alma

v3 API’nin temel tasarım ilkelerinden biri API’yi daha güvenli hale getirmektir. v3 API’ler **Get** veya **List** işlemlerinde gizli diziler ve kimlik bilgileri döndürmez. Anahtarlar her zaman null, boş veya yanıttan ayıklanmış olur. Gizli dizileri ve kimlik bilgilerini almak için ayrı bir eylem yöntemi çağırmanız gerekir. Bazı API’ler gizli dizileri alır ve görüntülerken diğer API'lerin bunu yapmaması durumunda, ayrı eylemler farklı RBAC güvenlik izinleri ayarlamanızı sağlar. RBAC kullanarak erişimi yönetme hakkında daha fazla bilgi için bkz. [Erişimi yönetmek için RBAC kullanma](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest).

Bunun örnekleri: 

* StreamingLocator’un Get isteği ContentKey değerleri döndürmüyor, 
* ContentKeyPolicy’nin get isteği kısıtlama anahtarları döndürmüyor, 
* İşlerin HTTP Giriş URL’lerinde URL’nin sorgu dizesi bölümünü döndürmüyor (imzayı kaldırmak için).

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

[Erişim denetimi ile birden çok DRM içerik koruma sisteminin tasarımı](design-multi-drm-system-with-access-control.md) 
