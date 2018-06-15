---
title: .NET SDK'sı ile varlık teslim ilkeleri yapılandırma | Microsoft Docs
description: Bu konu Azure Media Services .NET SDK ile farklı varlık teslim ilkeleri yapılandırmak nasıl gösterir.
services: media-services
documentationcenter: ''
author: Mingfeiy
manager: cfowler
editor: ''
ms.assetid: 3ec46f58-6cbb-4d49-bac6-1fd01a5a456b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/05/2018
ms.author: juliako
ms.openlocfilehash: aee2477e0633974cba42ab26e102323cb9606810
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33789456"
---
# <a name="configure-asset-delivery-policies-with-net-sdk"></a>.NET SDK'sı ile varlık teslim ilkeleri yapılandırma
[!INCLUDE [media-services-selector-asset-delivery-policy](../../../includes/media-services-selector-asset-delivery-policy.md)]

## <a name="overview"></a>Genel Bakış
Teslim şifrelenen varlıklarına planlıyorsanız, Media Services içerik teslim iş akışı'ndaki adımları birini varlıklar için teslim ilkeleri yapılandırıyor. Media Services, varlık teslim edilmesini istediğiniz varlık teslim ilkesini bildirir: hangi Akış Protokolü varlığınız dinamik olarak paketlenir (örneğin, MPEG DASH, HLS, kesintisiz akış veya tümü için), dinamik olarak Varlığınızı şifrelemek isteyip istemediğinizi ve nasıl içine (Zarf veya ortak şifreleme).

Bu makalede ele neden ve nasıl oluşturulacağı ve varlık teslim ilkeleri yapılandırın.

>[!NOTE]
>AMS hesabınız oluşturulduğunda, hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumda olması gerekir. 
>
>Ayrıca, dinamik paketleme ve dinamik şifreleme kullanabilmek için varlığınız Uyarlamalı bit hızlı MP4s ya da Uyarlamalı bit hızlı kesintisiz akış dosyaları içermelidir.

Aynı varlık için farklı ilkeler uygulayabilirsiniz. Örneğin, MPEG DASH ve HLS için AES zarfı kesintisiz akış ve şifreleme için PlayReady şifreleme uygulayabilirsiniz. Herhangi bir teslim ilkesinde tanımlanmayan tüm protokollerin (örneğin, protokol olarak yalnızca HLS‘yi belirten tek bir ilke ekliyorsunuz) akışla aktarılması engellenir. Bunun tek istisnası, hiçbir varlık teslim ilkesinin tanımlanmadığı durumdur. Bu halde tüm protokollere açık bir şekilde izin verilir.

Bir depolama şifrelenmiş varlık teslim etmek istiyorsanız, varlığın teslim ilkesini yapılandırmanız gerekir. Varlığınızı akışı önce akış sunucusu depolama şifreleme kaldırır ve belirtilen teslim ilkesini kullanarak içeriğinizi akışlarını. Örneğin, Gelişmiş Şifreleme Standardı (AES) Zarf şifreleme anahtarıyla şifrelenir, varlık teslim etmek için ilke türünü ayarlamak **DynamicEnvelopeEncryption**. Depolama şifrelemesi kaldırmak ve varlık temiz akışını ilke türünü ayarlayın **NoDynamicEncryption**. Bu ilke türünü yapılandırmanız nasıl gösteren örnekler izleyin.

Varlık teslim ilkesini nasıl yapılandırdığınıza bağlı olarak, dinamik olarak paketini, şifrelemek ve aşağıdaki akış protokolleri akış: kesintisiz akış, HLS ve MPEG DASH.

Aşağıdaki liste, kesintisiz, HLS ve tire akış için kullandığınız biçimleri gösterir.

Kesintisiz akış:

{akış uç noktası adı-media services hesabı adı}.streaming.mediaservices.windows.net/{konum kimliği}/{dosya adı}.ism/Manifest

HLS:

{uç nokta adı media services hesabı name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl) akış

MPEG DASH

{uç nokta adı media services hesabı name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf) akış

## <a name="considerations"></a>Dikkat edilmesi gerekenler
* AssetDeliveryPolicy silmeden önce tüm varlık ile ilişkili akış Bulucuyu silmeniz gerekir. İsterseniz, yeni bir AssetDeliveryPolicy ile daha sonra yeni akış Bulucuyu oluşturabilirsiniz.
* Hiçbir varlık teslim ilkesini ayarlandığında akış Bulucusu bir depolama şifrelenmiş varlık oluşturulamıyor.  Varlık şifrelenmiş depolama yoksa, sistem, bir Bulucu oluşturmanız ve varlık bir varlık teslim ilkesini olmadan temiz akışını olanak tanır.
* Tek bir varlık ile ilişkili birden çok varlık teslim ilkeleri olabilir, ancak yalnızca belirli bir AssetDeliveryProtocol işlemek için bir yol belirtebilirsiniz.  Sistem hangisinin, bir istemci bir kesintisiz akış bir istekte bulunduğunda uygulanacak bilmediğinden, bir hatayla sonuçlanır AssetDeliveryProtocol.SmoothStreaming protokolü belirtmek iki teslim ilkeleri bağlamaya çalışır anlamına gelir.
* Bir varlığı ile var olan bir akış Bulucu varsa varlık için yeni bir ilke bağlayamazsınız (, var olan bir varlık ilkesinden bağlantısını veya varlık ile ilişkili bir teslim ilkesini güncelleştirin).  İlk bu akış Bulucusu kaldırmak, ilkeleri ayarlamak ve akış Bulucusu yeniden oluşturmanız gerekmez.  Akış Bulucusu yeniden oluşturur ancak içerik kaynağı veya bir aşağı akış CDN tarafından önbelleğe beri sorunları istemciler için neden olmaz emin olmalısınız aynı locatorId kullanabilirsiniz.

## <a name="clear-asset-delivery-policy"></a>Clear varlık teslim ilkesini

Aşağıdaki **ConfigureClearAssetDeliveryPolicy** dinamik şifreleme geçerli değil ve aşağıdaki protokollerden birini akışta teslim etmeyi yöntemini belirtir: MPEG DASH, HLS ve kesintisiz akış protokollerini. Bu ilke şifrelenmiş depolama varlıklarınızı uygulamak isteyebilirsiniz.

Bir AssetDeliveryPolicy oluştururken belirtebilirsiniz değerleri hakkında bilgi için bkz: [AssetDeliveryPolicy tanımlarken kullanılan türleri](#types) bölümü.

```csharp
    static public void ConfigureClearAssetDeliveryPolicy(IAsset asset)
    {
        IAssetDeliveryPolicy policy =
        _context.AssetDeliveryPolicies.Create("Clear Policy",
        AssetDeliveryPolicyType.NoDynamicEncryption,
        AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);
        
        asset.DeliveryPolicies.Add(policy);
    }
```
## <a name="dynamiccommonencryption-asset-delivery-policy"></a>DynamicCommonEncryption varlık teslim ilkesini

Aşağıdaki **CreateAssetDeliveryPolicy** yöntemi oluşturur **AssetDeliveryPolicy** dinamik ortak şifreleme uygulamak için yapılandırılmış (**DynamicCommonEncryption**) bir kesintisiz akış protokolüne (diğer protokoller engellenir akışla aktarılması). Yöntemi iki parametre alır: **varlık** (varlık teslim ilkesini uygulamak istediğiniz) ve **IContentKey** (içerik anahtarı **CommonEncryption** türü için Daha fazla bilgi için bkz: [bir içerik anahtarı oluşturma](media-services-dotnet-create-contentkey.md#common_contentkey)).

Bir AssetDeliveryPolicy oluştururken belirtebilirsiniz değerleri hakkında bilgi için bkz: [AssetDeliveryPolicy tanımlarken kullanılan türleri](#types) bölümü.

```csharp
    static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);
        
        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
            {
                {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
            };
    
            var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                    "AssetDeliveryPolicy",
                AssetDeliveryPolicyType.DynamicCommonEncryption,
                AssetDeliveryProtocol.SmoothStreaming,
                assetDeliveryPolicyConfiguration);
    
            // Add AssetDelivery Policy to the asset
            asset.DeliveryPolicies.Add(assetDeliveryPolicy);
    
            Console.WriteLine();
            Console.WriteLine("Adding Asset Delivery Policy: " +
                assetDeliveryPolicy.AssetDeliveryPolicyType);
     }
```

Azure Media Services Widevine şifreleme eklemenize olanak sağlar. Aşağıdaki örnek, PlayReady ve Widevine için varlık teslim ilkesini eklenmekte olan gösterir.

```csharp
    static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        // Get the PlayReady license service URL.
        Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);


        // GetKeyDeliveryUrl for Widevine attaches the KID to the URL.
        // For example: https://amsaccount1.keydelivery.mediaservices.windows.net/Widevine/?KID=268a6dcb-18c8-4648-8c95-f46429e4927c.  
        // The WidevineBaseLicenseAcquisitionUrl (used below) also tells Dynamaic Encryption 
        // to append /? KID =< keyId > to the end of the url when creating the manifest.
        // As a result Widevine license acquisition URL will have KID appended twice, 
        // so we need to remove the KID that in the URL when we call GetKeyDeliveryUrl.

        Uri widevineUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine);
        UriBuilder uriBuilder = new UriBuilder(widevineUrl);
        uriBuilder.Query = String.Empty;
        widevineUrl = uriBuilder.Uri;

        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
        {
            {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
            {AssetDeliveryPolicyConfigurationKey.WidevineLicenseAcquisitionUrl, widevineUrl.ToString()}

        };

        var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.Dash,
            assetDeliveryPolicyConfiguration);


        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

    }
```
> [!NOTE]
> Widevine ile şifrelerken yalnızca tire kullanarak teslim etmek mümkün olacaktır. Varlık teslim Protokolü tire belirttiğinizden emin olun.
> 
> 

## <a name="dynamicenvelopeencryption-asset-delivery-policy"></a>DynamicEnvelopeEncryption varlık teslim ilkesini
Aşağıdaki **CreateAssetDeliveryPolicy** yöntemi oluşturur **AssetDeliveryPolicy** dinamik Zarf şifreleme uygulamak için yapılandırılmış (**DynamicEnvelopeEncryption**) Kesintisiz akış, HLS ve tire protokollere (bazı protokoller belirtmeyin karar verirseniz, bunlar akışla aktarılması engellenir). Yöntemi iki parametre alır: **varlık** (varlık teslim ilkesini uygulamak istediğiniz) ve **IContentKey** (içerik anahtarı **EnvelopeEncryption** türü Daha fazla bilgi için bkz: [bir içerik anahtarı oluşturma](media-services-dotnet-create-contentkey.md#envelope_contentkey)).

Bir AssetDeliveryPolicy oluştururken belirtebilirsiniz değerleri hakkında bilgi için bkz: [AssetDeliveryPolicy tanımlarken kullanılan türleri](#types) bölümü.   

```csharp
    private static void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {

        //  Get the Key Delivery Base Url by removing the Query parameter.  The Dynamic Encryption service will
        //  automatically add the correct key identifier to the url when it generates the Envelope encrypted content
        //  manifest.  Omitting the IV will also cause the Dynamice Encryption service to generate a deterministic
        //  IV for the content automatically.  By using the EnvelopeBaseKeyAcquisitionUrl and omitting the IV, this
        //  allows the AssetDelivery policy to be reused by more than one asset.
        //
        Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);
        UriBuilder uriBuilder = new UriBuilder(keyAcquisitionUri);
        uriBuilder.Query = String.Empty;
        keyAcquisitionUri = uriBuilder.Uri;

        // The following policy configuration specifies: 
        //   key url that will have KID=<Guid> appended to the envelope and
        //   the Initialization Vector (IV) to use for the envelope encryption.
        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string> 
        {
            {AssetDeliveryPolicyConfigurationKey.EnvelopeBaseKeyAcquisitionUrl, keyAcquisitionUri.ToString()},
        };

        IAssetDeliveryPolicy assetDeliveryPolicy =
            _context.AssetDeliveryPolicies.Create(
                        "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicEnvelopeEncryption,
                        AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.Dash,
                        assetDeliveryPolicyConfiguration);

        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        Console.WriteLine();
        Console.WriteLine("Adding Asset Delivery Policy: " + assetDeliveryPolicy.AssetDeliveryPolicyType);
    }
```

## <a id="types"></a>AssetDeliveryPolicy tanımlarken kullanılan türleri

### <a id="AssetDeliveryProtocol"></a>AssetDeliveryProtocol

Aşağıdaki liste, varlık teslim protokolü için ayarlayabileceğiniz değerler açıklar.

```csharp
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
```
### <a id="AssetDeliveryPolicyType"></a>AssetDeliveryPolicyType

Aşağıdaki liste varlık teslim İlkesi türü için ayarlayabileceğiniz değerler açıklar.  
```csharp
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
```
### <a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType

Aşağıdaki liste, içerik anahtarının istemciye teslim yöntemini yapılandırmak için kullanabileceğiniz değerleri açıklanmaktadır.
  ```csharp  
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        ///
        </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquistion protocol
        ///
        </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        ///
        </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquistion protocol
        ///
        </summary>
        Widevine = 3

    }
```
### <a id="AssetDeliveryPolicyConfigurationKey"></a>AssetDeliveryPolicyConfigurationKey

Aşağıdaki liste, belirli bir yapılandırma için bir varlık teslim ilkesini almak için kullanılan anahtarları yapılandırmak için ayarlayabilirsiniz değerleri açıklanmaktadır.
```csharp
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
```
## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

