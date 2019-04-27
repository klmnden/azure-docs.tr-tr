---
title: REST kullanarak Azure Media Services içerik yayımlama
description: Akış URL'si oluşturmak için kullanılan bir Bulucu oluşturmayı öğrenin. Kod, REST API kullanır.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: ff332c30-30c6-4ed1-99d0-5fffd25d4f23
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: 974f0af461ecdc7de820191950b010035d02a601
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60598299"
---
# <a name="publish-azure-media-services-content-using-rest"></a>REST kullanarak Azure Media Services içerik yayımlama 
> [!div class="op_single_selector"]
> * [.NET](media-services-deliver-streaming-content.md)
> * [REST](media-services-rest-deliver-streaming-content.md)
> * [Portal](media-services-portal-publish.md)
> 
> 

Bir OnDemand akış Bulucusu oluşturma ve bir akış URL'si oluşturma bir hızı Uyarlamalı MP4 kümesine akışını yapabilirsiniz. [Bir varlığı kodlama](media-services-rest-encode-asset.md) makale Uyarlamalı bit hızı MP4 kümesi kodlama. İçeriğinizi şifrelenmişse, varlık teslim ilkesini yapılandırın (açıklandığı [bu](media-services-rest-configure-asset-delivery-policy.md) makale) önce bir Bulucu oluşturma. 

Bir OnDemand akış Bulucusu, aşamalı olarak indirilebilir MP4 dosyasına işaret eden URL'leri oluşturmak için de kullanabilirsiniz.  

Bu makalede bir OnDemand akış Bulucusu, varlığı yayımlayın ve kesintisiz, MPEG DASH ve HLS akış URL'lerini oluşturmak için nasıl oluşturulacağını gösterir. Aşamalı indirme URL'lerini oluşturmak için sık erişimli gösterir.

[Aşağıdaki](#types) bölüm değerleri kullanılan numaralandırma türleri REST çağrılarını gösterir.   

> [!NOTE]
> Varlıklar Media Services erişirken, HTTP isteklerini özel üstbilgi alanlarını ve değerlerini ayarlamanız gerekir. Daha fazla bilgi için [Media Services REST API geliştirme için Kurulum](media-services-rest-how-to-use.md).
> 

## <a name="connect-to-media-services"></a>Media Services’e bağlanmak

AMS API'ye bağlanma hakkında daha fazla bilgi için bkz: [Azure AD kimlik doğrulamasıyla Azure Media Services API'sine erişim](media-services-use-aad-auth-to-access-ams-api.md). 

>[!NOTE]
>İçin başarıyla bağlandıktan sonra https://media.windows.net, başka bir Media Services URI belirterek 301 bir yeniden yönlendirme alırsınız. Yeni bir URI'ya yapılan sonraki çağrılar yapmak gerekir.

## <a name="create-an-ondemand-streaming-locator"></a>Bir OnDemand akış Bulucusu oluşturma
Bir OnDemand akış Bulucusu oluşturma ve URL'leri almak için aşağıdakileri yapmanız gerekir:

1. İçeriğin şifreli değilse, bir erişim ilkesi tanımlayın.
2. Bir OnDemand akış Bulucusu oluşturma.
3. Akış planlıyorsanız, varlık, akış bildirim dosyası (.ism) alın. 
   
   Aşamalı indirmeyi planlıyorsanız, varlık MP4 dosyalarının adlarını alın. 
4. Bildirim dosyası veya MP4 dosyaları için URL'leri oluşturun. 
5. Yazma içeren bir AccessPolicy kullanarak akış Bulucusu oluşturmak ya da silme izinleri olamaz.

### <a name="create-an-access-policy"></a>Erişim ilkesi oluşturma

>[!NOTE]
>Farklı AMS ilkeleri için sınır 1.000.000 ilkedir (örneğin, Bulucu ilkesi veya ContentKeyAuthorizationPolicy için). Aynı günleri / erişim izinlerini, örneğin, ilkeleri kalmasına yerinde uzun bir süredir (karşıya yükleme olmayan ilkeler) yöneliktir bulucular için her zaman aynı ilke Kimliğini kullanın. Daha fazla bilgi için [bu makaleye](media-services-dotnet-manage-entities.md#limit-access-policies) bakın.

İstek:

    POST https://media.windows.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.17
    x-ms-client-request-id: 6bcfd511-a561-448d-a022-a319a89ecffa
    Host: media.windows.net
    Content-Length: 68

    {"Name":"access policy","DurationInMinutes":43200.0,"Permissions":1}

Yanıt:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 311
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https:/media.windows.net/api/AccessPolicies('nb%3Apid%3AUUID%3A69c80d98-7830-407f-a9af-e25f4b0d3e5f')
    Server: Microsoft-IIS/8.5
    request-id: a877528a-bdb4-4414-9862-273f8e64f882
    x-ms-request-id: a877528a-bdb4-4414-9862-273f8e64f882
    x-ms-client-request-id: 6bcfd511-a561-448d-a022-a319a89ecffa
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 18 Feb 2015 06:52:09 GMT

    {"odata.metadata":"https://media.windows.net/api/$metadata#AccessPolicies/@Element","Id":"nb:pid:UUID:69c80d98-7830-407f-a9af-e25f4b0d3e5f","Created":"2015-02-18T06:52:09.8862191Z","LastModified":"2015-02-18T06:52:09.8862191Z","Name":"access policy","DurationInMinutes":43200.0,"Permissions":1}

### <a name="create-an-ondemand-streaming-locator"></a>Bir OnDemand akış Bulucusu oluşturma
Belirtilen varlık ve varlık ilke için bir Bulucu oluşturun.

İstek:

    POST https://media.windows.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.17
    x-ms-client-request-id: ac159492-9a0c-40c3-aacc-551b1b4c5f62
    Host: media.windows.net
    Content-Length: 181

    {"AccessPolicyId":"nb:pid:UUID:1480030d-c481-430a-9687-535c6a5cb272","AssetId":"nb:cid:UUID:cc1e445d-1500-80bd-538e-f1e4b71b465e","StartTime":"2015-02-18T06:34:47.267872Z","Type":2}

Yanıt:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 637
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/Locators('nb%3Alid%3AUUID%3Abe245661-2bbd-4fc6-b14f-9cf9a1492e5e')
    Server: Microsoft-IIS/8.5
    request-id: 5bd5864a-0afd-44c0-a67a-4044a2c9043b
    x-ms-request-id: 5bd5864a-0afd-44c0-a67a-4044a2c9043b
    x-ms-client-request-id: ac159492-9a0c-40c3-aacc-551b1b4c5f62
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 18 Feb 2015 06:58:37 GMT

    {"odata.metadata":"https://media.windows.net/api/$metadata#Locators/@Element","Id":"nb:lid:UUID:be245661-2bbd-4fc6-b14f-9cf9a1492e5e","ExpirationDateTime":"2015-03-20T06:34:47.267872+00:00","Type":2,"Path":"https://amstest1.streaming.mediaservices.windows.net/be245661-2bbd-4fc6-b14f-9cf9a1492e5e/","BaseUri":"https://amstest1.streaming.mediaservices.windows.net","ContentAccessComponent":"be245661-2bbd-4fc6-b14f-9cf9a1492e5e","AccessPolicyId":"nb:pid:UUID:1480030d-c481-430a-9687-535c6a5cb272","AssetId":"nb:cid:UUID:cc1e445d-1500-80bd-538e-f1e4b71b465e","StartTime":"2015-02-18T06:34:47.267872+00:00","Name":null}

### <a name="build-streaming-urls"></a>Akış URL'lerini oluşturun
Kullanım **yolu** kesintisiz, HLS ve MPEG DASH URL'lerini oluşturmak için bir Bulucu oluşturulduktan sonra döndürülen değer. 

Kesintisiz akış: **Yol** + bildirim dosyası adı + "/ MANIFEST"

örnek:

    https://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest

HLS: **Yol** + bildirim dosyası adı + "/ manifest(format=m3u8-aapl)"

örnek:

    https://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=m3u8-aapl)


ÇİZGİ: **Yol** + bildirim dosyası adı + "/ manifest(format=mpd-time-csf)"

örnek:

    https://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=mpd-time-csf)


### <a name="build-progressive-download-urls"></a>Aşamalı indirme URL'leri oluşturun
Kullanım **yolu** aşamalı indirme URL'sini oluşturmak için bir Bulucu oluşturulduktan sonra döndürülen değer.   

URL: **Yol** + varlık dosyası mp4 adı

örnek:

    https://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

## <a id="types"></a>Numaralandırma türleri
    [Flags]
    public enum AccessPermissions
    {
        None = 0,
        Read = 1,
        Write = 2,
        Delete = 4,
        List = 8,
    }

    public enum LocatorType
    {
        None = 0,
        Sas = 1,
        OnDemandOrigin = 2,
    }

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca bkz.
[Media Services işlemlerini REST API'si genel bakış](media-services-rest-how-to-use.md)

[Varlık teslim ilkesini yapılandırma](media-services-rest-configure-asset-delivery-policy.md)

