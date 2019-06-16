---
title: REST kullanarak medya işlemci örneği alma | Microsoft Docs
description: Kodlama, biçim dönüştürme, şifrelemek veya Azure Media Services için medya içeriğin şifresini için medya işlemci bileşeninin oluşturmayı öğrenin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: f9ff1997-0da6-4528-aaed-792837e5be41
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: d342cff6d322195ee88a74215f814be7d702aa5e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60761985"
---
# <a name="how-to-get-a-media-processor-instance"></a>Medya işlemci örneği alma
> [!div class="op_single_selector"]
> * [.NET](media-services-get-media-processor.md)
> * [REST](media-services-rest-get-media-processor.md)
> 
> 

## <a name="overview"></a>Genel Bakış
Medya işlemcileri belirli bir video veya ses işleme görevi, kodlama, biçim dönüştürme, şifreleme ve şifresini çözmeyi medya içeriği gibi işler bir bileşenidir. Media Services'e gönderilen tüm görevleri, kodlama, şifreleme veya video veya ses içeriğini dönüştürmek için medya işleyicisi gerektirir. 

## <a name="azure-media-processors"></a>Azure medya işlemcileri 

Aşağıdaki konuda medya işlemcileri listesi sağlar:

* [Kodlama medya işleyicileri](scenarios-and-availability.md#encoding-media-processors)
* [Analiz medya işlemcileri](scenarios-and-availability.md#analytics-media-processors)

>[!NOTE]
>Varlıklar Media Services erişirken, HTTP isteklerini özel üstbilgi alanlarını ve değerlerini ayarlamanız gerekir. Daha fazla bilgi için [Media Services REST API geliştirme için Kurulum](media-services-rest-how-to-use.md).

## <a name="connect-to-media-services"></a>Media Services’e bağlanmak

AMS API'ye bağlanma hakkında daha fazla bilgi için bkz: [Azure AD kimlik doğrulamasıyla Azure Media Services API'sine erişim](media-services-use-aad-auth-to-access-ams-api.md). 


## <a name="get-a-media-processor"></a>Medya işlemcisi Al

Medya işlemci örneği ada göre alma REST çağrısını gösterir (Bu durumda, **Media Encoder Standard**). 

İstek:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.17
    Host: media.windows.net

Yanıt:

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
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Sonraki Adımlar
Medya işlemci örneği alma artık bildiğinize göre Git [bir varlığı kodlama](media-services-rest-get-started.md) makalesini Medya Kodlayıcısı standart bir varlığı kodlama nasıl kullanılacağını gösterir.

