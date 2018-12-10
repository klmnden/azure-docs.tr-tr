---
title: Media Services v3 - Azure'ı kullanarak özel dönüştürme kodlama | Microsoft Docs
description: Bu konuda, Azure Media Services v3 özel dönüşüm kodlamak için nasıl kullanılacağı gösterilmektedir.
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
ms.openlocfilehash: c62d9132cdd7eb2ebcbecc3c417ad30d368a278a
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53138713"
---
# <a name="how-to-encode-with-a-custom-transform"></a>Özel bir dönüşüm ile kodlama

Azure Media Services ile kodlama, hızlı bir şekilde gösterildiği şekilde sektör en iyi yöntemlerine göre önerilen yerleşik hazır biriyle oluşturabileceğinize dair [akış dosyalarını](stream-files-tutorial-with-api.md) öğretici veya özel bir yapı seçebilirsiniz Senaryonuz ya da cihaz belirli gereksinimlerinizi hedeflemek için hazır. 

> [!Note]
> Azure Media Services v3 sürümünde, tüm kodlama hızları bit / saniye arasındadır. Bu, Media Encoder Standard hazır ayarları REST v2 farklıdır. Örneğin, bit hızı v2'de 128 belirtilmesi, ancak v3 sürümünde 128000 olacaktır.

## <a name="download-the-sample"></a>Örneği indirme

Aşağıdaki komutu kullanarak makinenize tam .NET Core örnek içeren GitHub deposunu kopyalayın:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials.git
 ```
 
Özel hazır örnek bulunan [EncodeCustomTransform](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/EncodeCustomTransform/) klasör.

## <a name="create-a-transform-with-a-custom-preset"></a>Dönüşüm özel önayarın ile oluşturma 

Yeni bir oluştururken [dönüştürme](https://docs.microsoft.com/rest/api/media/transforms), çıkış olarak üretmek için istediğinizi belirtmeniz gerekir. Gerekli parametre, aşağıdaki kodda gösterildiği gibi bir **TransformOutput** nesnesidir. Her **TransformOutput** bir **Ön ayar** içerir. **Ön ayar**, video ve/veya ses işleme işlemlerinin istenen **TransformOutput** nesnesini oluşturmak üzere kullanılacak adım adım yönergelerini açıklar. Aşağıdaki **TransformOutput** özel codec ve katman çıkış ayarları oluşturur.

Bir [Dönüşüm](https://docs.microsoft.com/rest/api/media/transforms) oluştururken ilk olarak aşağıdaki kodda gösterildiği gibi **Get** yöntemi ile bir dönüşümün zaten var olup olmadığını denetlemeniz gerekir.  Media Services v3 içinde **alma** varlıklar üzerinde yöntemleri dönüş **null** varlık (büyük/küçük harfe adına bir denetimi) yoksa.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/EncodeCustomTransform/MediaV3ConsoleApp/Program.cs#EnsureTransformExists)]

## <a name="next-steps"></a>Sonraki adımlar

[Akış dosyaları](stream-files-tutorial-with-api.md) 
