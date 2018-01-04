---
title: ".NET için Azure Media Services SDK'sını kullanarak medya işlemcisi oluşturma | Microsoft Docs"
description: "Kodlama, biçimine dönüştürmek, şifrelemek veya medya içeriği için Azure Media Services şifresini çözmek için bir medya işlemci bileşeni oluşturmayı öğrenin. Kod örnekleri, C# dilinde yazılmıştır ve .NET için Media Services SDK'sını kullanın."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: dbf9496f-c6f0-42a7-aa36-70f89dcb8ea2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako
ms.openlocfilehash: c2cbe41b71afa8acc184f9d7f4cfe94686de783e
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="how-to-get-a-media-processor-instance"></a>Nasıl yapılır: bir medya işlemci örneği Al
> [!div class="op_single_selector"]
> * [.NET](media-services-get-media-processor.md)
> * [REST](media-services-rest-get-media-processor.md)
> 
> 

## <a name="overview"></a>Genel Bakış
Medya işlemcisi kodlama gibi kodlama, ek olarak, belirli işleme görevi işleyen bir bileşenidir Media Services'de şifreleme veya medya içeriği çözme dönüştürme biçimi. Kodlama, şifreleme veya medya içeriği biçimine dönüştürmek için bir görev oluştururken genellikle medya işlemcisi oluşturun.

## <a name="azure-media-processors"></a>Azure medya işlemcileri 

Aşağıdaki konu medya işlemcileri listesi sağlar:

* [Kodlama medya işlemcileri](scenarios-and-availability.md#encoding-media-processors)
* [Analizi medya işlemcileri](scenarios-and-availability.md#analytics-media-processors)

## <a name="get-media-processor"></a>Medya işlemcisi Al

Aşağıdaki yöntem, medya işlemci örneği elde gösterilmektedir. Kod örneği adlı bir modül düzeyi değişkeni kullanımını varsayar **_bağlamı** bölümde açıklandığı gibi sunucu bağlamına başvurmak için [nasıl yapılır: Media Services programsal olarak bağlanmak](media-services-use-aad-auth-to-access-ams-api.md).

    private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

        if (processor == null)
        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

        return processor;
    }


## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Sonraki Adımlar
Medya işlemcisi örneği alma bildiğinize göre Git [bir varlık kodlama](media-services-dotnet-encode-with-media-encoder-standard.md) konu Medya Kodlayıcısı standart bir varlık kodlama için nasıl kullanılacağını gösterir.

