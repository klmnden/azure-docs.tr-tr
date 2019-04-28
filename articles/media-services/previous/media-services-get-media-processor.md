---
title: .NET için Azure Media Services SDK'sını kullanarak Medya işleyicisi oluşturma | Microsoft Docs
description: Kodlama, biçim dönüştürme, şifrelemek veya Azure Media Services için medya içeriğin şifresini için medya işlemci bileşeninin oluşturmayı öğrenin. İçinde yazılan kod örneklerini C# ve .NET için Media Services SDK'sını kullanın.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: dbf9496f-c6f0-42a7-aa36-70f89dcb8ea2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: af6badda426f1bb81d8528cfda9b8c02d55712b3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61463863"
---
# <a name="how-to-get-a-media-processor-instance"></a>Nasıl yapılır: Medya işlemci örneği Al
> [!div class="op_single_selector"]
> * [.NET](media-services-get-media-processor.md)
> * [REST](media-services-rest-get-media-processor.md)
> 
> 

## <a name="overview"></a>Genel Bakış
Media Services'da Medya işleyicisi, kodlama gibi ek olarak, belirli bir işleme görevi işleyen bir bileşen olan şifreleme veya şifre çözme medya içeriği dönüştürme biçimi. Genellikle, kodlama, şifreleme veya medya içeriklerinin biçimine dönüştürmek için bir görev oluştururken medya işlemcisi oluşturursunuz.

## <a name="azure-media-processors"></a>Azure medya işlemcileri 

Aşağıdaki konuda medya işlemcileri listesi sağlar:

* [Kodlama medya işleyicileri](scenarios-and-availability.md#encoding-media-processors)
* [Analiz medya işlemcileri](scenarios-and-availability.md#analytics-media-processors)

## <a name="get-media-processor"></a>Medya işlemcisi Al

Aşağıdaki yöntemi, bir medya işlemci örneği alma işlemi gösterilmektedir. Kod örneğinde adlı Modül düzeyinde bir değişkene kullanımını varsayar **_bağlamı** bölümünde açıklandığı gibi sunucu bağlamı başvurmak için [nasıl yapılır: Media Services'e program aracılığıyla bağlanmak](media-services-use-aad-auth-to-access-ams-api.md).

    private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

        if (processor == null)
        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

        return processor;
    }


## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Sonraki Adımlar
Medya işlemci örneği alma artık bildiğinize göre Git [bir varlığı kodlama](media-services-dotnet-encode-with-media-encoder-standard.md) konu Medya Kodlayıcısı standart bir varlığı kodlama için nasıl kullanılacağını gösterir.

