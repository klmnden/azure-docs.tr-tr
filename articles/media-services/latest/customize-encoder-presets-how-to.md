---
title: Media Services v3 .NET - Azure ile özel dönüştürme kodlama | Microsoft Docs
description: Bu konuda, .NET kullanarak özel bir dönüştürme kodlamak için Azure Media Services v3 kullanmayı gösterir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: seodec18
ms.date: 03/11/2019
ms.author: juliako
ms.openlocfilehash: ed2ae50aa9d7a26ed6e0569264ee981f7be35525
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60733683"
---
# <a name="how-to-encode-with-a-custom-transform---net"></a>-.NET gibi özel bir dönüşüm ile kodlama

Azure Media Services ile kodlama, hızlı bir şekilde gösterildiği şekilde sektör en iyi yöntemlerine göre önerilen yerleşik hazır biriyle oluşturabileceğinize dair [akış dosyalarını](stream-files-tutorial-with-api.md) öğretici. Ayrıca, özel bir senaryonuz ya da cihaz belirli gereksinimlerinizi hedeflemek için önceden de oluşturabilirsiniz.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Özel önayarların kullanılmasına oluştururken, aşağıdaki maddeler geçerlidir:

* Yükseklik ve genişlik AVC içeriğin tüm değerleri 4'ün katı olmalıdır.
* Azure Media Services v3 sürümünde, tüm kodlama bit hızlarına dönüştürme bit / saniye cinsindendir. Bu Önayarı kilobit/saniye birimi olarak kullanılan v2 Apı'lerimiz ile farklıdır. V2'de hızı (kilobit/saniye) 128 belirtilmemişse, örneğin, v3 sürümünde, 128000 için (bit/saniye) ayarlanır.

## <a name="prerequisites"></a>Önkoşullar 

[Bir Media Services hesabı oluşturma](create-account-cli-how-to.md). <br/>Kaynak grubu adı ve Media Services hesap adını hatırlamak emin olun. 

## <a name="download-the-sample"></a>Örneği indirme

Aşağıdaki komutu kullanarak makinenize tam .NET Core örnek içeren GitHub deposunu kopyalayın:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials.git
 ```
 
Özel hazır örnek bulunan [EncodeCustomTransform](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/EncodeCustomTransform/) klasör.

## <a name="create-a-transform-with-a-custom-preset"></a>Dönüşüm özel önayarın ile oluşturma 

Yeni bir oluştururken [dönüştürme](https://docs.microsoft.com/rest/api/media/transforms), çıkış olarak üretmek için istediğinizi belirtmeniz gerekir. Gerekli parametre, aşağıdaki kodda gösterildiği gibi bir [TransformOutput](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#transformoutput) nesnesidir. Her **TransformOutput** bir **Ön ayar** içerir. **Ön ayar**, video ve/veya ses işleme işlemlerinin istenen **TransformOutput** nesnesini oluşturmak üzere kullanılacak adım adım yönergelerini açıklar. Aşağıdaki **TransformOutput** özel codec ve katman çıkış ayarları oluşturur.

Bir [Dönüşüm](https://docs.microsoft.com/rest/api/media/transforms) oluştururken ilk olarak aşağıdaki kodda gösterildiği gibi **Get** yöntemi ile bir dönüşümün zaten var olup olmadığını denetlemeniz gerekir. Media Services v3 içinde **alma** varlıklar üzerinde yöntemleri dönüş **null** varlık (büyük/küçük harfe adına bir denetimi) yoksa.

### <a name="example"></a>Örnek

Aşağıdaki örnek, bu dönüşüm kullanıldığında oluşturulan istiyoruz çıkışları kümesini tanımlar. İlk ses kodlama için bir AacAudio katmanı ve video kodlama için iki H264Video katmanları ekleriz. Böylece çıkış dosya adlarında kullanılabilir video katmanlar, biz etiketleri atayın. Ardından, küçük resimler de içerecek şekilde çıkış istiyoruz. Aşağıdaki örnekte biz görüntüleri giriş videosunun çözünürlüğünün % 50 ve üç zaman damgalarının - {% 25, % 50, 75} giriş videosunun uzunluğunu oluşturulan PNG biçiminde belirtin. Son olarak, biz çıkış dosyaları - bir video ve ses biçimi belirtin ve başka küçük resimleri için. Birden çok H264Layers sahibiz olduğundan, katman başına benzersiz adlar oluşturmak makroları gerekir. Biz kullanabilir bir `{Label}` veya `{Bitrate}` makrosu, bu örnek önceki gösterir.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/EncodeCustomTransform/MediaV3ConsoleApp/Program.cs#EnsureTransformExists)]

## <a name="next-steps"></a>Sonraki adımlar

[Akış dosyaları](stream-files-tutorial-with-api.md) 
