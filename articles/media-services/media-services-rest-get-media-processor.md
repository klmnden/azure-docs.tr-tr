---
title: "REST kullanarak medya işlemcisi örneği alma | Microsoft Docs"
description: "Kodlama, biçimine dönüştürmek, şifrelemek veya medya içeriği için Azure Media Services şifresini çözmek için bir medya işlemci bileşeni oluşturmayı öğrenin."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: f9ff1997-0da6-4528-aaed-792837e5be41
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: juliako
ms.openlocfilehash: 4e673a92a9740b96eac20cdf5673395bacca8b77
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="how-to-get-a-media-processor-instance"></a>Medya işlemcisi örneği alma
> [!div class="op_single_selector"]
> * [.NET](media-services-get-media-processor.md)
> * [REST](media-services-rest-get-media-processor.md)
> 
> 

## <a name="overview"></a>Genel Bakış
Medya işlemcileri belirli video veya ses işleme görevi, kodlama gibi kodlama, biçimini dönüştürme, şifreleme veya şifresi çözme medya içeriği işleyen bir bileşeni var. Media Services'e gönderilen tüm görevleri kodlamak, şifrelemek veya ses veya video içeriğin dönüştürülmesi için medya işlemcisi gerektirir. 

## <a name="azure-media-processors"></a>Azure medya işlemcileri 

Aşağıdaki konu medya işlemcileri listesi sağlar:

* [Kodlama medya işlemcileri](scenarios-and-availability.md#encoding-media-processors)
* [Analizi medya işlemcileri](scenarios-and-availability.md#analytics-media-processors)

>[!NOTE]
>Varlıklar Media Services erişirken, HTTP istekleri özel üstbilgi alanlarını ve değerlerini ayarlamanız gerekir. Daha fazla bilgi için bkz: [Media Services REST API geliştirme için Kurulum](media-services-rest-how-to-use.md).

## <a name="connect-to-media-services"></a>Media Services’e bağlanmak

AMS API'sine bağlanma hakkında daha fazla bilgi için bkz: [Azure AD kimlik doğrulaması ile Azure Media Services API erişim](media-services-use-aad-auth-to-access-ams-api.md). 


## <a name="get-a-media-processor"></a>Medya işlemcisi Al

Medya işlemcisi örneği tarafından adını almak nasıl aşağıdaki REST çağrısı gösterir (Bu durumda, **Medya Kodlayıcısı standart**). 

İsteği:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.17
    Host: media.windows.net

Yanıtı:

    . . .

    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }


## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Sonraki Adımlar
Medya işlemcisi örneği alma bildiğinize göre Git [bir varlık kodlama](media-services-rest-get-started.md) Medya Kodlayıcısı standart bir varlık kodlama nasıl kullanılacağını göstermektedir makalesi.

