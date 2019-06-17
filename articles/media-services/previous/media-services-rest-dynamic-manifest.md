---
title: Filtre oluşturma ile Azure Media Services REST API | Microsoft Docs
description: Bu konuda, istemci akışı için bir stream'ın belirli bölümlerine kullanabilmesi için filtreler oluşturmayı açıklar. Media Services, bu seçmeli akış elde etmek için olan dinamik bildirimler oluşturur.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: f7d23daf-7cd2-49c7-a195-ab902912ab3c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako;cenkdin
ms.openlocfilehash: 5b023a152cf93ec6ff688674e991ad55db215965
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60767821"
---
# <a name="creating-filters-with-azure-media-services-rest-api"></a>İle Azure Media Services REST API filtreleri oluşturma 
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-dynamic-manifest.md)
> * [REST](media-services-rest-dynamic-manifest.md)
> 
> 

2\.17 sürümünden başlayarak, Media Services, varlıklarınız için filtrelerini tanımlamanızı sağlar. Bu filtreler müşterileriniz gibi şeyler seçmesine izin veren sunucu tarafı kurallar şunlardır: yalnızca bir bölümünü (yerine tüm video oynatma), bir video kayıttan yürütme ya da yalnızca bir alt kümesini (yerine, müşterinizin cihaz işleyebilir ses ve video yorumlama belirtin bir varlıkla ilişkilendirilen tüm yorumlama). Bu filtreleme varlıklarınızı aracılığıyla arşivlenmiş **dinamik bildirim**müşterinizin istek üzerine video akışı oluşturan s üzerinde belirtilen filtreleri temel.

Daha ayrıntılı filtreler ve dinamik bildirim ilgili bilgi için bkz. [dinamik bildirimlerin genel bakış](media-services-dynamic-manifest-overview.md).

Bu makalede, oluşturma, güncelleştirme ve filtreleri silmek için REST API'lerini kullanma gösterilmektedir. 

## <a name="types-used-to-create-filters"></a>Filtreler oluşturmak için kullanılan türleri
Aşağıdaki türleri filtreler oluşturulurken kullanılır:  

* [Filtre](https://docs.microsoft.com/rest/api/media/operations/filter)
* [AssetFilter](https://docs.microsoft.com/rest/api/media/operations/assetfilter)
* [PresentationTimeRange](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange)
* [FilterTrackSelect ve FilterTrackPropertyCondition](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect)

> [!NOTE]
> 
> Varlıklar Media Services erişirken, HTTP isteklerini özel üstbilgi alanlarını ve değerlerini ayarlamanız gerekir. Daha fazla bilgi için [Media Services REST API geliştirme için Kurulum](media-services-rest-how-to-use.md).

## <a name="connect-to-media-services"></a>Media Services’e bağlanmak

AMS API'ye bağlanma hakkında daha fazla bilgi için bkz: [Azure AD kimlik doğrulamasıyla Azure Media Services API'sine erişim](media-services-use-aad-auth-to-access-ams-api.md). 

## <a name="create-filters"></a>Filtre oluşturma
### <a name="create-global-filters"></a>Genel filtre oluşturma
Genel bir filtre oluşturmak için aşağıdaki HTTP isteklerini kullanın:  

#### <a name="http-request"></a>HTTP isteği
İstek üst bilgileri

    POST https://media.windows.net/API/Filters HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.17 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host:media.windows.net 

İstek gövdesi 

    {  
       "Name":"GlobalFilter",
       "PresentationTimeRange":{  
          "StartTimestamp":"0",
          "EndTimestamp":"9223372036854775807",
          "PresentationWindowDuration":"12000000000",
          "LiveBackoffDuration":"0",
          "Timescale":"10000000"
       },
       "Tracks":[  
          {  
             "PropertyConditions":
                  [  
                {  
                   "Property":"Type",
                   "Value":"audio",
                   "Operator":"Equal"
                },
                {  
                   "Property":"Bitrate",
                   "Value":"0-2147483647",
                   "Operator":"Equal"
                }
             ]
          }
       ]
    }




#### <a name="http-response"></a>HTTP yanıtı
    HTTP/1.1 201 Created 

### <a name="create-local-assetfilters"></a>Yerel AssetFilters oluşturma
Yerel AssetFilter oluşturmak için aşağıdaki HTTP isteklerini kullanın:  

#### <a name="http-request"></a>HTTP isteği
İstek üst bilgileri

    POST https://media.windows.net/API/AssetFilters HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.17 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net  

İstek gövdesi 

    {   
       "Name":"AssetFilter", 
       "ParentAssetId":"nb:cid:UUID:536e555d-1500-80c3-92dc-f1e4fdc6c592", 
       "PresentationTimeRange":{   
          "StartTimestamp":"0", 
          "EndTimestamp":"9223372036854775807", 
          "PresentationWindowDuration":"12000000000", 
          "LiveBackoffDuration":"0", 
          "Timescale":"10000000" 
       }, 
       "Tracks":[   
          {   
             "PropertyConditions": 
                  [   
                {   
                   "Property":"Type", 
                   "Value":"audio", 
                   "Operator":"Equal" 
                }, 
                {   
                   "Property":"Bitrate", 
                   "Value":"0-2147483647", 
                   "Operator":"Equal" 
                } 
             ] 
          } 
       ] 
    } 

#### <a name="http-response"></a>HTTP yanıtı
    HTTP/1.1 201 Created 
    . . . 

## <a name="list-filters"></a>Liste filtreleri
### <a name="get-all-global-filters-in-the-ams-account"></a>Tüm genel alma **filtre**AMS hesabına s
İstek filtreleri listelemek için aşağıdaki HTTP kullanın: 

#### <a name="http-request"></a>HTTP isteği
    GET https://media.windows.net/API/Filters HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.17 
    Host: media.windows.net 

### <a name="get-assetfilters-associated-with-an-asset"></a>Alma **AssetFilter**bir varlıkla ilişkilidir.
#### <a name="http-request"></a>HTTP isteği
    GET https://media.windows.net/API/Assets('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592')/AssetFilters HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.17 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net 

### <a name="get-an-assetfilter-based-on-its-id"></a>Alma bir **AssetFilter** kimliğine göre
#### <a name="http-request"></a>HTTP isteği
    GET https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__TestFilter') HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.17 
    x-ms-client-request-id: 00000000


## <a name="update-filters"></a>Güncelleştirme filtreleri
Bir filtre ile yeni özellik değerlerini güncelleştirmek için PATCH, PUT veya MERGE kullanın.  Bu işlemler hakkında daha fazla bilgi için bkz: [düzeltme eki, YERLEŞTİRME, birleştirme](https://msdn.microsoft.com/library/dd541276.aspx).

Bir filtre güncelleştirirseniz, kuralları yenilemek akış uç noktası için iki dakikaya kadar sürebilir. İçerik için bu filtreyi kullanarak hizmet (ve bu önbellekler proxy'leri ve CDN önbelleğe), bu filtre güncelleştirme player hatalarına neden olabilir. Filtre güncelleştirdikten sonra önbelleğini temizleyin. Bu seçenek, mümkün değildir. farklı bir filtre kullanmayı düşünün.  

### <a name="update-global-filters"></a>Genel filtrelerin güncelleştir
Genel filtre güncelleştirmek için aşağıdaki HTTP isteklerini kullanın: 

#### <a name="http-request"></a>HTTP isteği
İstek bağlıkları: 

    MERGE https://media.windows.net/API/Filters('filterName') HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.17 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net 
    Content-Length: 384

İstek gövdesi: 

    { 
       "Tracks":[   
          {   
             "PropertyConditions": 
             [   
                {   
                   "Property":"Type", 
                   "Value":"audio", 
                   "Operator":"Equal" 
                }, 
                {   
                   "Property":"Bitrate", 
                   "Value":"0-2147483647", 
                   "Operator":"Equal" 
                } 
             ] 
          } 
       ] 
    } 

### <a name="update-local-assetfilters"></a>Yerel AssetFilters güncelleştir
Yerel bir filtre güncelleştirmek için aşağıdaki HTTP isteklerini kullanın: 

#### <a name="http-request"></a>HTTP isteği
İstek bağlıkları: 

    MERGE https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__TestFilter')  HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.17 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net 

İstek gövdesi: 

    { 
       "Tracks":[   
          {   
             "PropertyConditions": 
             [   
                {   
                   "Property":"Type", 
                   "Value":"audio", 
                   "Operator":"Equal" 
                }, 
                {   
                   "Property":"Bitrate", 
                   "Value":"0-2147483647", 
                   "Operator":"Equal" 
                } 
             ] 
          } 
       ] 
    } 


## <a name="delete-filters"></a>Filtreleri Sil
### <a name="delete-global-filters"></a>Genel filtrelerin Sil
Genel bir filtreyi silmek için aşağıdaki HTTP isteklerini kullanın:

#### <a name="http-request"></a>HTTP isteği
    DELETE https://media.windows.net/api/Filters('GlobalFilter') HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <ENCODED JWT TOKEN>  
    x-ms-version: 2.17 
    Host: media.windows.net 


### <a name="delete-local-assetfilters"></a>Yerel AssetFilters Sil
Yerel AssetFilter silmek için aşağıdaki HTTP isteklerini kullanın:

#### <a name="http-request"></a>HTTP isteği
    DELETE https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__LocalFilter') HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.17 
    Host: media.windows.net 

## <a name="build-streaming-urls-that-use-filters"></a>Akış Filtreleri kullanan URL'lerini oluşturun
Yayımlama ve varlıklarınızı teslim hakkında daha fazla bilgi için bkz: [müşterilerin genel bakış için içerik teslim](media-services-deliver-content-overview.md).

Aşağıdaki örnekler, akış URL'leri için filtreleri ekleyin gösterilmektedir.

**MPEG DASH** 

    http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

**Apple HTTP canlı akış (HLS) V4**

    http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

**Apple HTTP canlı akış (HLS) V3**

    http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

**Kesintisiz akış**

    http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)

    
## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.
[Dinamik bildirimlere genel bakış](media-services-dynamic-manifest-overview.md)

