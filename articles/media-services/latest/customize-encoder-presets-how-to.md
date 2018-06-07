---
title: Azure Media Services v3 kullanarak özel dönüşüm kodlamak | Microsoft Docs
description: Bu konu Azure Media Services v3 özel bir dönüşüm kodlamak için nasıl kullanılacağını gösterir.
services: media-services
documentationcenter: ''
author: Juliako
manager: cflower
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 05/17/2018
ms.author: juliako
ms.openlocfilehash: d298070877a366d04b2df1ef8ac63b08f8771de0
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655798"
---
# <a name="how-to-encode-with-a-custom-transform"></a>İle özel bir dönüşüm kodlama

Azure Media Services ile kodlama, hızlı şekilde endüstrideki en iyi uygulamaları temel önerilen yerleşik hazır ayarlarından birini kullanmaya [akış dosyalarını](stream-files-tutorial-with-api.md) Öğreticisi veya özel bir yapı seçebilirsiniz Senaryo veya aygıt gereksinimlerinizi hedeflemek için hazır. 

> [!Note]
> Azure Media Services v3 kodlama bit hızlarını saniyede bit tümü. Bu Medya Kodlayıcısı standart hazır ayarları REST v2'den farklı değildir. Örneğin, v2, bit hızı 128 belirtilmesi, ancak v3 128000 olacaktır.

## <a name="download-the-sample"></a>Örneği indirme

Tam .NET Core örnek olarak aşağıdaki komutu kullanarak, makine içeren bir GitHub deposunu kopyalayın:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials.git
 ```
 
Özel hazır örnek bulunan [EncodeCustomTransform](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/EncodeCustomTransform/) klasör.

## <a name="create-a-transform-with-a-custom-preset"></a>Özel bir hazır bir dönüşüm oluşturun 

Yeni bir oluştururken [dönüştürme](https://docs.microsoft.com/rest/api/media/transforms), çıkış olarak üretmek için istediğinizi belirtmeniz gerekir. Gerekli parametre, aşağıdaki kodda gösterildiği gibi bir **TransformOutput** nesnesidir. Her **TransformOutput** bir **Ön ayar** içerir. **Ön ayar**, video ve/veya ses işleme işlemlerinin istenen **TransformOutput** nesnesini oluşturmak üzere kullanılacak adım adım yönergelerini açıklar. Aşağıdaki **TransformOutput** özel codec ve katman çıkış ayarları oluşturur.

Bir [Dönüşüm](https://docs.microsoft.com/rest/api/media/transforms) oluştururken ilk olarak aşağıdaki kodda gösterildiği gibi **Get** yöntemi ile bir dönüşümün zaten var olup olmadığını denetlemeniz gerekir.  Media Services v3 içinde **almak** varlıkların yöntemlerini dönmek **null** varlık (büyük küçük harf duyarsız adına bir onay) yoksa.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/EncodeCustomTransform/MediaV3ConsoleApp/Program.cs#EnsureTransformExists)]

## <a name="next-steps"></a>Sonraki adımlar

[Akış dosyaları](stream-files-tutorial-with-api.md) 
