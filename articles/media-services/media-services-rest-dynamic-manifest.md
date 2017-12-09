---
title: "Filtreleri oluşturma ile Azure Media Services REST API | Microsoft Docs"
description: "Bu konuda, istemci akış belirli bölümlerine bir akış için kullanabilmeniz için filtreleri oluşturmayı açıklar. Media Services bu seçmeli akış elde etmek için dinamik bildirimleri oluşturur."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: f7d23daf-7cd2-49c7-a195-ab902912ab3c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako;cenkdin
ms.openlocfilehash: 76d2721138668d9f0a908af3fa42840309b068ef
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="creating-filters-with-azure-media-services-rest-api"></a>İle Azure Media Services REST API filtreleri oluşturma
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-dynamic-manifest.md)
> * [REST](media-services-rest-dynamic-manifest.md)
> 
> 

2.11 sürümünden başlayarak, Media Services, varlıklarınızı filtrelerini tanımlamanızı sağlar. Bu filtreler müşterilerinizin gibi şeyler seçmesine izin veren sunucu tarafı kurallardır: kayıttan yürütme (yerine tüm video oynatma), bir video yalnızca bir bölümünü veya yalnızca bir alt kümesini, müşterinizin aygıt (yerine varlıkla ilişkilendirilen tüm yorumlama) işleyebilir ses ve video yorumlama belirtin. Bu varlıklarınızı filtreleme aracılığıyla arşivlenmiş **dinamik bildirim**video akışını sağlamak için Müşteri'nin istek üzerine oluşturulan s tabanlı üzerinde belirtilen filtreler.

Daha ayrıntılı filtreler ve dinamik bildirim ilgili bilgi için bkz: [dinamik bildirimleri genel bakış](media-services-dynamic-manifest-overview.md).

Bu konu, REST API'ları oluşturmak, güncelleştirmek ve filtreleri silmek için nasıl kullanılacağını gösterir. 

## <a name="types-used-to-create-filters"></a>Filtreleri oluşturmak için kullanılan türleri
Aşağıdaki türlerden filtreleri oluşturulurken kullanılır:  

* [Filtre](https://docs.microsoft.com/rest/api/media/operations/filter)
* [AssetFilter](https://docs.microsoft.com/rest/api/media/operations/assetfilter)
* [PresentationTimeRange](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange)
* [FilterTrackSelect ve FilterTrackPropertyCondition](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect)

>[!NOTE]

>Varlıklar Media Services erişirken, HTTP istekleri özel üstbilgi alanlarını ve değerlerini ayarlamanız gerekir. Daha fazla bilgi için bkz: [Media Services REST API geliştirme için Kurulum](media-services-rest-how-to-use.md).

## <a name="connect-to-media-services"></a>Media Services’e bağlanmak

AMS API'sine bağlanma hakkında daha fazla bilgi için bkz: [Azure AD kimlik doğrulaması ile Azure Media Services API erişim](media-services-use-aad-auth-to-access-ams-api.md). 

>[!NOTE]
>Başarıyla https://media.windows.net için bağladıktan sonra başka bir Media Services URI belirleme 301 bir yeniden yönlendirme alırsınız. Yeni bir URI yapılan sonraki çağrılar yapmanız gerekir.

## <a name="create-filters"></a>Filtre oluşturma
### <a name="create-global-filters"></a>Genel filtrelerin oluşturma
Genel filtre oluşturmak için aşağıdaki HTTP isteklerini kullanın:  

#### <a name="http-request"></a>HTTP isteği
İstek üstbilgileri

    POST https://media.windows.net/API/Filters HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
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
İstek üstbilgileri

    POST https://media.windows.net/API/AssetFilters HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
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

## <a name="list-filters"></a>Filtreleri Listele
### <a name="get-all-global-filters-in-the-ams-account"></a>Tüm genel alma **filtre**AMS hesabının s
İstekleri filtreler listelemek için aşağıdaki HTTP kullanın: 

#### <a name="http-request"></a>HTTP isteği
    GET https://media.windows.net/API/Filters HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    Host: media.windows.net 

### <a name="get-assetfilters-associated-with-an-asset"></a>Alma **AssetFilter**bir varlıkla ilişkilendirilen s
#### <a name="http-request"></a>HTTP isteği
    GET https://media.windows.net/API/Assets('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592')/AssetFilters HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net 

### <a name="get-an-assetfilter-based-on-its-id"></a>Alma bir **AssetFilter** kimliğine göre
#### <a name="http-request"></a>HTTP isteği
    GET https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__TestFilter') HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000


## <a name="update-filters"></a>Güncelleştirme filtreleri
PUT veya birleştirme ile yeni özellik değerlerinin bir filtre güncelleştirmek için kullanım düzeltme.  Bu işlemler hakkında daha fazla bilgi için bkz: [düzeltme eki, YERLEŞTİRME, birleştirme](http://msdn.microsoft.com/library/dd541276.aspx).

Bir filtre güncelleştirirseniz, kuralları yenilemek akış uç 2 dakika kadar sürebilir. İçerik bu filtre kullanarak sunulduğu (ve proxy'ler ve CDN önbellekleri önbelleğe alınmış), bu filtre güncelleştiriliyor player hatalarına neden olabilir. Bu filtre güncelleştirdikten sonra önbelleğini temizlemek için önerilir. Bu seçenek yoksa, başka bir filtre kullanmayı düşünün.  

### <a name="update-global-filters"></a>Genel filtrelerin güncelleştir
Genel filtre güncelleştirmek için aşağıdaki HTTP isteklerini kullanın: 

#### <a name="http-request"></a>HTTP isteği
İstek üstbilgilerini: 

    MERGE https://media.windows.net/API/Filters('filterName') HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
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
İstek üstbilgilerini: 

    MERGE https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__TestFilter')  HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
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
Genel filtre silmek için aşağıdaki HTTP isteklerini kullanın:

#### <a name="http-request"></a>HTTP isteği
    DELETE https://media.windows.net/api/Filters('GlobalFilter') HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    Host: media.windows.net 


### <a name="delete-local-assetfilters"></a>Yerel AssetFilters Sil
Yerel AssetFilter silmek için aşağıdaki HTTP isteklerini kullanın:

#### <a name="http-request"></a>HTTP isteği
    DELETE https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__LocalFilter') HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    Host: media.windows.net 

## <a name="build-streaming-urls-that-use-filters"></a>Akış filtreleri kullanın URL'lerini derleme
Yayımlama ve varlıklarınızı teslim etmek hakkında daha fazla bilgi için bkz: [müşteriler genel bakış için içerik teslim](media-services-deliver-content-overview.md).

Aşağıdaki örnekler, akış URL'leri filtreleri ekleme gösterir.

**MPEG DASH** 

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

**Apple HTTP canlı akış (HLS) V4**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

**Apple HTTP canlı akış (HLS) V3**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

**Kesintisiz akış**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)

    
## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.
[Dinamik bildirimleri genel bakış](media-services-dynamic-manifest-overview.md)

