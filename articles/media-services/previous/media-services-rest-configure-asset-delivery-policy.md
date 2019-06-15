---
title: Media Services REST API'si kullanarak varlık teslim ilkelerini yapılandırma | Microsoft Docs
description: Bu konuda, Media Services REST API'si kullanarak farklı varlık teslim ilkeleri nasıl yapılandırılacağı gösterilmektedir.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.assetid: 5cb9d32a-e68b-4585-aa82-58dded0691d0
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: 5e4e565b0b5272de19458617a9c4bd3509907cce
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60817390"
---
# <a name="configuring-asset-delivery-policies"></a>Varlık teslim ilkelerini yapılandırma
[!INCLUDE [media-services-selector-asset-delivery-policy](../../../includes/media-services-selector-asset-delivery-policy.md)]

Dinamik olarak şifrelenmiş varlıklar teslim etmeyi planlıyorsanız, medya Hizmetleri içerik teslim iş akışındaki adımlar birini varlıklar teslim ilkelerini yapılandırıyor. Varlık teslim ilkesini, varlık teslim edilmesini istediğiniz Media Services bildirir: dinamik olarak şifrelemek isteyip istemediğinizi hangi akış protokolüne varlığınız dinamik olarak (örneğin, MPEG DASH, HLS, kesintisiz akış veya tümü için), paketlenmesi gereken varlığınız ve nasıl (Zarf veya ortak şifreleme).

Bu konuda ele alınmıştır neden ve nasıl oluşturulup varlık teslim ilkelerini yapılandırma.

> [!NOTE]
> AMS hesabınız oluşturulduğunda hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumda olması gerekir. 
>
> Ayrıca, dinamik paketleme ve dinamik şifreleme kullanabilmek için varlığınız bir dizi hızı Uyarlamalı MP4 veya uyarlamalı bit hızlı kesintisiz akış dosyaları içermelidir.

Aynı varlık için farklı ilkeler uygulayabilirsiniz. Örneğin, MPEG DASH ve HLS için AES zarfı kesintisiz akış ve şifreleme için PlayReady şifreleme uygulayabilirsiniz. Herhangi bir teslim ilkesinde tanımlanmayan tüm protokollerin (örneğin, protokol olarak yalnızca HLS‘yi belirten tek bir ilke ekliyorsunuz) akışla aktarılması engellenir. Bunun tek istisnası, hiçbir varlık teslim ilkesinin tanımlanmadığı durumdur. Bu halde tüm protokollere açık bir şekilde izin verilir.

Bir depolama şifrelenmiş varlık iletmek istiyorsanız, varlık teslim ilkesini yapılandırmanız gerekir. Varlığınızı akışla önce akış sunucusu depolama şifrelemesi kaldırır ve belirtilen teslim ilkesini kullanarak içeriğinizi akışları. Örneğin, Gelişmiş Şifreleme Standardı (AES) Zarf şifreleme anahtarıyla şifrelenmiş varlığınız sunmak için ilke türünü ayarlayın **DynamicEnvelopeEncryption**. Depolama şifrelemesi kaldırmak ve açık bir varlıkta akış için ilke türünü ayarlayın **NoDynamicEncryption**. Aşağıdaki ilke türlerinden yapılandırma gösteren örnekler izleyin.

Varlık teslim ilkesini nasıl yapılandırdığınıza bağlı olarak dinamik olarak paketleme, dinamik olarak şifreleme ve aşağıdaki akış protokollerine akış mümkün olacaktır: Akış, HLS, MPEG DASH akışlarında kesintisiz.

Aşağıdaki liste, akışa kesintisiz, HLS, DASH kullandığınız biçimleri gösterir.

Kesintisiz akış:

{akış uç noktası adı-media services hesabı adı}.streaming.mediaservices.windows.net/{konum kimliği}/{dosya adı}.ism/Manifest

HLS:

{Akış uç noktası adı-media services hesabı name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG DASH

{Akış uç noktası adı-media services hesabı name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


Varlık yayımlama ve akış URL'si oluşturma yönergeleri için bkz. [Akış URL'si oluşturma](media-services-deliver-streaming-content.md).

## <a name="considerations"></a>Dikkat edilmesi gerekenler
* Bu varlık için bir OnDemand (akış) Bulucu bulunduğu sürece bir varlıkla ilişkili bir AssetDeliveryPolicy nelze odstranit. İlkeyi silmeden önce ilkeyi varlığından kaldırmak için önerilir.
* Akış Bulucusu, hiçbir varlık teslim ilkesinin ayarlandığında bir depolama şifrelenmiş varlığında oluşturulamıyor.  Varlık şifrelenmiş depolama yoksa, sistem, bir Bulucu oluşturmanız ve açık bir varlık teslim İlkesi olmadan varlıkta akışını olanak tanır.
* Tek bir varlık ile ilişkili birden çok varlık teslim ilkesi olabilir, ancak yalnızca belirli bir AssetDeliveryProtocol işlemek için bir yol belirtebilirsiniz.  Sistem hangisinin, bir istemci bir kesintisiz akış bir istekte bulunduğunda uygulanacak bilmediğinden, bir hatayla sonuçlanır AssetDeliveryProtocol.SmoothStreaming Protokolü iki teslim ilkelerini bağlantı denerseniz anlamına gelir.
* Bir varlık ile var olan bir akış Bulucu varsa, yeni bir ilke için bir varlık bağlama, varlık var olan bir ilkeden bağlantısını veya varlıkla ilişkili bir teslim ilkesini olamaz.  Akış Bulucusu kaldırın ilkeleri ayarlayın ve ardından akış Bulucusu yeniden oluşturmak öncelikle olması.  Akış Bulucusu yeniden ancak içerik kaynağı veya bir aşağı akış CDN tarafından önbelleğe gerektiğinden sorunları istemciler için neden olmaz emin olun, aynı locatorId kullanabilirsiniz.

> [!NOTE]
> 
> Varlıklar Media Services erişirken, HTTP isteklerini özel üstbilgi alanlarını ve değerlerini ayarlamanız gerekir. Daha fazla bilgi için [Media Services REST API geliştirme için Kurulum](media-services-rest-how-to-use.md).

## <a name="connect-to-media-services"></a>Media Services’e bağlanmak

AMS API'ye bağlanma hakkında daha fazla bilgi için bkz: [Azure AD kimlik doğrulamasıyla Azure Media Services API'sine erişim](media-services-use-aad-auth-to-access-ams-api.md). 

## <a name="clear-asset-delivery-policy"></a>NET varlık teslim İlkesi
### <a id="create_asset_delivery_policy"></a>Varlık teslim ilkesini oluşturma
Aşağıdaki HTTP isteği, dinamik şifreleme uygulamak ve şu protokolden herhangi birini stream'de sunmak için belirten varlık teslim ilkesi oluşturur:  MPEG DASH, HLS ve kesintisiz akış protokoller. 

Bir AssetDeliveryPolicy oluştururken belirtebilirsiniz değerleri hakkında bilgi için bkz [AssetDeliveryPolicy tanımlarken kullanılan türler](#types) bölümü.   

İstek:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.17
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    Host: media.windows.net

    {"Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null}

Yanıt:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 363
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    x-ms-request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 06:21:27 GMT

    {"odata.metadata":"https://media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element",
    "Id":"nb:adpid:UUID:92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd",
    "Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null,
    "Created":"2015-02-08T06:21:27.6908329Z",
    "LastModified":"2015-02-08T06:21:27.6908329Z"}

### <a id="link_asset_with_asset_delivery_policy"></a>Varlık teslim ilkesini bağlantı varlığı
Aşağıdaki HTTP isteği belirtilen varlık için varlık teslim ilkesini bağlar.

İstek:

    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A86933344-9539-4d0c-be7d-f842458693e0')/$links/DeliveryPolicies HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.17
    x-ms-client-request-id: 56d2763f-6e72-419d-ba3c-685f6db97e81
    Host: media.windows.net

    {"uri":"https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')"}

Yanıt:

    HTTP/1.1 204 No Content


## <a name="dynamicenvelopeencryption-asset-delivery-policy"></a>Varlık teslim ilkesini DynamicEnvelopeEncryption
### <a name="create-content-key-of-the-envelopeencryption-type-and-link-it-to-the-asset"></a>EnvelopeEncryption türünün içerik anahtarı oluşturun ve varlığına bağlayın
DynamicEnvelopeEncryption teslim İlkesi belirtirken EnvelopeEncryption türü için bir içerik anahtarı, varlık bağlama emin olmanız gerekir. Daha fazla bilgi için bkz. [Bir içerik anahtarı oluşturma](media-services-rest-create-contentkey.md)).

### <a id="get_delivery_url"></a>Teslim URL'sini alma
Önceki adımda oluşturulan içerik anahtarının belirtilen teslim yöntemini teslim URL'sini alma. Bir istemci, bir AES anahtarı istemek için döndürülen URL'yi kullanır. veya bir PlayReady korumalı içeriği kayıttan yürütme için sırayla lisansı.

HTTP istek gövdesinde almak için URL türünü belirtin. PlayReady, içeriğinizi Media Services PlayReady lisans edinme URL'si isteği koruyorsanız 1 için keyDeliveryType kullanıyor: {"keyDeliveryType": 1}. İçeriğinizi Zarf şifreleme ile koruyorsanız, bir anahtar edinme keyDeliveryType 2 belirterek istek URL'si: {"keyDeliveryType": 2}.

İstek:

    POST https://media.windows.net/api/ContentKeys('nb:kid:UUID:dc88f996-2859-4cf7-a279-c52a9d6b2f04')/GetKeyDeliveryUrl HTTP/1.1
    Content-Type: application/json
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.17
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    Host: media.windows.net
    Content-Length: 21

    {"keyDeliveryType":2}

Yanıt:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 198
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    x-ms-request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 21:42:30 GMT

    {"odata.metadata":"media.windows.net/api/$metadata#Edm.String","value":"https://amsaccount1.keydelivery.mediaservices.windows.net/?KID=dc88f996-2859-4cf7-a279-c52a9d6b2f04"}


### <a name="create-asset-delivery-policy"></a>Varlık teslim ilkesini oluşturma
Aşağıdaki HTTP isteği oluşturur **AssetDeliveryPolicy** dinamik Zarf şifreleme uygulamak üzere yapılandırılmış (**DynamicEnvelopeEncryption**) için **HLS** protokol (Bu örnekte, diğer protokolleri akış verilerinden engellenir). 

Bir AssetDeliveryPolicy oluştururken belirtebilirsiniz değerleri hakkında bilgi için bkz [AssetDeliveryPolicy tanımlarken kullanılan türler](#types) bölümü.   

İstek:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.17
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net

    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]"}


Yanıt:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 460
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3Aec9b994e-672c-4a5b-8490-a464eeb7964b')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    x-ms-request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 09 Feb 2015 05:24:38 GMT

    {"odata.metadata":"media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element","Id":"nb:adpid:UUID:ec9b994e-672c-4a5b-8490-a464eeb7964b","Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]","Created":"2015-02-09T05:24:38.9167436Z","LastModified":"2015-02-09T05:24:38.9167436Z"}


### <a name="link-asset-with-asset-delivery-policy"></a>Varlık teslim ilkesini bağlantı varlığı
Bkz: [varlık teslim ilkesini bağlantı varlığı](#link_asset_with_asset_delivery_policy)

## <a name="dynamiccommonencryption-asset-delivery-policy"></a>Varlık teslim ilkesini DynamicCommonEncryption
### <a name="create-content-key-of-the-commonencryption-type-and-link-it-to-the-asset"></a>CommonEncryption türünün içerik anahtarı oluşturun ve varlığına bağlayın
DynamicCommonEncryption teslim İlkesi belirtirken CommonEncryption türü için bir içerik anahtarı, varlık bağlama emin olmanız gerekir. Daha fazla bilgi için bkz. [Bir içerik anahtarı oluşturma](media-services-rest-create-contentkey.md)).

### <a name="get-delivery-url"></a>Teslim URL'sini alma
İçerik anahtarı önceki adımda oluşturduğunuz PlayReady teslim yöntemi için teslim URL'yi alın. Bir istemci, korumalı içeriği kayıttan yürütme için sırayla PlayReady lisans istemek için döndürülen URL'yi kullanır. Daha fazla bilgi için [teslim URL'sini alma](#get_delivery_url).

### <a name="create-asset-delivery-policy"></a>Varlık teslim ilkesini oluşturma
Aşağıdaki HTTP isteği oluşturur **AssetDeliveryPolicy** dinamik ortak şifreleme uygulamak üzere yapılandırılmış (**DynamicCommonEncryption**) için **kesintisiz akış**Protokolü (Bu örnekte, diğer protokolleri akış verilerinden engellenir). 

Bir AssetDeliveryPolicy oluştururken belirtebilirsiniz değerleri hakkında bilgi için bkz [AssetDeliveryPolicy tanımlarken kullanılan türler](#types) bölümü.   

İstek:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.17
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net

    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":1,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\/PlayReady\/"}]"}


Widevine DRM kullanarak içeriğinizi korumak istiyorsanız, (Bu 7 değerine sahiptir) WidevineLicenseAcquisitionUrl kullanılacak assetdeliveryconfiguration'ı değerlerini güncelleştirin ve bir lisans teslimat hizmeti URL'sini belirtin. Widevine lisansları teslim etmenize yardımcı olmak için şu AMS ortaklarını kullanabilirsiniz: [Axinom](https://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](https://ezdrm.com/), [castLabs](https://castlabs.com/company/partners/azure/).

Örneğin: 

    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":2,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":7,\"Value\":\"https:\\/\\/example.net\/WidevineLicenseAcquisition\/"}]"}

> [!NOTE]
> Widevine ile şifrelerken yalnızca DASH teslim et mümkün olacaktır. Varlık teslim Protokolü tire (2) belirttiğinizden emin olun.
> 
> 

### <a name="link-asset-with-asset-delivery-policy"></a>Varlık teslim ilkesini bağlantı varlığı
Bkz: [varlık teslim ilkesini bağlantı varlığı](#link_asset_with_asset_delivery_policy)

## <a id="types"></a>AssetDeliveryPolicy tanımlarken kullanılan türler

### <a name="assetdeliveryprotocol"></a>AssetDeliveryProtocol

Aşağıdaki enum değerleri için varlık teslim Protokolü ayarlayabilirsiniz açıklar.

    [Flags]
    public enum AssetDeliveryProtocol
    {
        /// <summary>
        /// No protocols.
        /// </summary>
        None = 0x0,

        /// <summary>
        /// Smooth streaming protocol.
        /// </summary>
        SmoothStreaming = 0x1,

        /// <summary>
        /// MPEG Dynamic Adaptive Streaming over HTTP (DASH)
        /// </summary>
        Dash = 0x2,

        /// <summary>
        /// Apple HTTP Live Streaming protocol.
        /// </summary>
        HLS = 0x4,

        ProgressiveDownload = 0x10, 
 
        /// <summary>
        /// Include all protocols.
        /// </summary>
        All = 0xFFFF
    }

### <a name="assetdeliverypolicytype"></a>AssetDeliveryPolicyType

Aşağıdaki enum değerleri için varlık teslim İlkesi türü ayarlayabilirsiniz açıklar.  

    public enum AssetDeliveryPolicyType
    {
        /// <summary>
        /// Delivery Policy Type not set.  An invalid value.
        /// </summary>
        None,

        /// <summary>
        /// The Asset should not be delivered via this AssetDeliveryProtocol. 
        /// </summary>
        Blocked, 

        /// <summary>
        /// Do not apply dynamic encryption to the asset.
        /// </summary>
        /// 
        NoDynamicEncryption,  

        /// <summary>
        /// Apply Dynamic Envelope encryption.
        /// </summary>
        DynamicEnvelopeEncryption,

        /// <summary>
        /// Apply Dynamic Common encryption.
        /// </summary>
        DynamicCommonEncryption
        }

### <a name="contentkeydeliverytype"></a>ContentKeyDeliveryType

Aşağıdaki liste, içerik anahtarının istemciye teslim yöntemini yapılandırmak için kullanabileceğiniz değerleri açıklanmaktadır.
    
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        ///
        </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquisition protocol
        ///
        </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        ///
        </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquisition protocol
        ///
        </summary>
        Widevine = 3

    }


### <a name="assetdeliverypolicyconfigurationkey"></a>AssetDeliveryPolicyConfigurationKey

Aşağıdaki enum değerleri için bir varlık teslim ilkesini belirli yapılandırmasını almak için kullanılan anahtarları yapılandırmak için ayarlayabilirsiniz açıklar.

    public enum AssetDeliveryPolicyConfigurationKey
    {
        /// <summary>
        /// No policies.
        /// </summary>
        None,

        /// <summary>
        /// Exact Envelope key URL.
        /// </summary>
        EnvelopeKeyAcquisitionUrl,

        /// <summary>
        /// Base key url that will have KID=<Guid> appended for Envelope.
        /// </summary>
        EnvelopeBaseKeyAcquisitionUrl,

        /// <summary>
        /// The initialization vector to use for envelope encryption in Base64 format.
        /// </summary>
        EnvelopeEncryptionIVAsBase64,

        /// <summary>
        /// The PlayReady License Acquisition Url to use for common encryption.
        /// </summary>
        PlayReadyLicenseAcquisitionUrl,

        /// <summary>
        /// The PlayReady Custom Attributes to add to the PlayReady Content Header
        /// </summary>
        PlayReadyCustomAttributes,

        /// <summary>
        /// The initialization vector to use for envelope encryption.
        /// </summary>
        EnvelopeEncryptionIV,

        /// <summary>
        /// Widevine DRM acquisition url
        /// </summary>
        WidevineLicenseAcquisitionUrl
    }

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

