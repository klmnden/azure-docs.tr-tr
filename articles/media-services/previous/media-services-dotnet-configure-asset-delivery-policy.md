---
title: .NET SDK'sı ile varlık teslim ilkelerini yapılandırma | Microsoft Docs
description: Bu konuda, Azure Media Services .NET SDK ile farklı varlık teslim ilkelerini yapılandırma gösterilmektedir.
services: media-services
documentationcenter: ''
author: Mingfeiy
manager: femila
editor: ''
ms.assetid: 3ec46f58-6cbb-4d49-bac6-1fd01a5a456b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: b5e733c93fef8920c73c8cf460dac7a7051fddb5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61465618"
---
# <a name="configure-asset-delivery-policies-with-net-sdk"></a>.NET SDK'sı ile varlık teslim ilkelerini yapılandırma
[!INCLUDE [media-services-selector-asset-delivery-policy](../../../includes/media-services-selector-asset-delivery-policy.md)]

## <a name="overview"></a>Genel Bakış
Şifrelenmiş teslim varlıklarına planlıyorsanız, medya Hizmetleri içerik teslim iş akışındaki adımlar birini varlıklar teslim ilkelerini yapılandırıyor. Varlık teslim ilkesini, varlık teslim edilmesini istediğiniz Media Services bildirir: dinamik olarak şifrelemek isteyip istemediğinizi hangi akış protokolüne varlığınız dinamik olarak (örneğin, MPEG DASH, HLS, kesintisiz akış veya tümü için), paketlenmesi gereken varlığınız ve nasıl (Zarf veya ortak şifreleme).

Bu makalede ele alınmaktadır neden ve nasıl oluşturulup varlık teslim ilkelerini yapılandırma.

>[!NOTE]
>AMS hesabınız oluşturulduğunda, hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumda olması gerekir. 
>
>Ayrıca, dinamik paketleme ve dinamik şifreleme kullanabilmek için varlığınız bir dizi hızı Uyarlamalı MP4 veya uyarlamalı bit hızlı kesintisiz akış dosyaları içermelidir.

Aynı varlık için farklı ilkeler uygulayabilirsiniz. Örneğin, MPEG DASH ve HLS için AES zarfı kesintisiz akış ve şifreleme için PlayReady şifreleme uygulayabilirsiniz. Herhangi bir teslim ilkesinde tanımlanmayan tüm protokollerin (örneğin, protokol olarak yalnızca HLS‘yi belirten tek bir ilke ekliyorsunuz) akışla aktarılması engellenir. Bunun tek istisnası, hiçbir varlık teslim ilkesinin tanımlanmadığı durumdur. Bu halde tüm protokollere açık bir şekilde izin verilir.

Bir depolama şifrelenmiş varlık iletmek istiyorsanız, varlık teslim ilkesini yapılandırmanız gerekir. Varlığınızı akışla önce akış sunucusu depolama şifrelemesi kaldırır ve belirtilen teslim ilkesini kullanarak içeriğinizi akışları. Örneğin, Gelişmiş Şifreleme Standardı (AES) Zarf şifreleme anahtarıyla şifrelenmiş varlığınız sunmak için ilke türünü ayarlayın **DynamicEnvelopeEncryption**. Depolama şifrelemesi kaldırmak ve açık bir varlıkta akış için ilke türünü ayarlayın **NoDynamicEncryption**. Aşağıdaki ilke türlerinden yapılandırma gösteren örnekler izleyin.

Varlık teslim ilkesini nasıl yapılandırdığınıza bağlı olarak dinamik olarak paketleyebilir, şifrelemek ve aşağıdaki akış protokollerine akış: Kesintisiz akış, HLS ve MPEG DASH.

Aşağıdaki liste, kesintisiz, HLS ve DASH akış için kullandığınız biçimleri gösterir.

Kesintisiz akış:

{akış uç noktası adı-media services hesabı adı}.streaming.mediaservices.windows.net/{konum kimliği}/{dosya adı}.ism/Manifest

HLS:

{Akış uç noktası adı-media services hesabı name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG DASH

{Akış uç noktası adı-media services hesabı name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

## <a name="considerations"></a>Dikkat edilmesi gerekenler
* AssetDeliveryPolicy silmeden önce tüm varlıkla ilişkili akış Bulucuyu silmeniz gerekir. Yeni akış bulucuları, daha sonra isterseniz, yeni bir AssetDeliveryPolicy ile da oluşturabilirsiniz.
* Akış Bulucusu, hiçbir varlık teslim ilkesinin ayarlandığında bir depolama şifrelenmiş varlığında oluşturulamıyor.  Varlık şifrelenmiş depolama yoksa, sistem, bir Bulucu oluşturmanız ve açık bir varlık teslim İlkesi olmadan varlıkta akışını olanak tanır.
* Tek bir varlık ile ilişkili birden çok varlık teslim ilkesi olabilir, ancak yalnızca belirli bir AssetDeliveryProtocol işlemek için bir yol belirtebilirsiniz.  Sistem hangisinin, bir istemci bir kesintisiz akış bir istekte bulunduğunda uygulanacak bilmediğinden, bir hatayla sonuçlanır AssetDeliveryProtocol.SmoothStreaming Protokolü iki teslim ilkelerini bağlantı denerseniz anlamına gelir.
* Bir varlık ile var olan bir akış Bulucu varsa, yeni bir ilke varlığına bağlayamazsınız (varlık var olan bir ilkeden bağlantısını, veya varlıkla ilişkili bir teslim ilkesini güncelleştirin).  Akış Bulucusu kaldırın ilkeleri ayarlayın ve ardından akış Bulucusu yeniden oluşturmak öncelikle olması.  Akış Bulucusu yeniden ancak içerik kaynağı veya bir aşağı akış CDN tarafından önbelleğe gerektiğinden sorunları istemciler için neden olmaz emin olun, aynı locatorId kullanabilirsiniz.

## <a name="clear-asset-delivery-policy"></a>NET varlık teslim İlkesi

Aşağıdaki **ConfigureClearAssetDeliveryPolicy** yöntemi, dinamik şifreleme uygulamak ve şu protokolden herhangi birini stream'de sunmak için belirtir:  MPEG DASH, HLS ve kesintisiz akış protokoller. Şifrelenmiş depolama varlıklarınızı Bu ilkeyi uygulamak isteyebilirsiniz.

Bir AssetDeliveryPolicy oluştururken belirtebilirsiniz değerleri hakkında bilgi için bkz [AssetDeliveryPolicy tanımlarken kullanılan türler](#types) bölümü.

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
## <a name="dynamiccommonencryption-asset-delivery-policy"></a>Varlık teslim ilkesini DynamicCommonEncryption

Aşağıdaki **CreateAssetDeliveryPolicy** yöntemi oluşturur **AssetDeliveryPolicy** dinamik ortak şifreleme uygulamak üzere yapılandırılmış (**DynamicCommonEncryption**) bir kesintisiz akış protokolüne (diğer protokolleri engellenir akış verilerinden). Yöntem iki parametre alır: **Varlık** (varlık teslim ilkesini uygulamak istediğiniz) ve **IContentKey** (içerik anahtarı **CommonEncryption** türü, daha fazla bilgi için bkz: [Bir içerik anahtarı oluşturma](media-services-dotnet-create-contentkey.md#common_contentkey)).

Bir AssetDeliveryPolicy oluştururken belirtebilirsiniz değerleri hakkında bilgi için bkz [AssetDeliveryPolicy tanımlarken kullanılan türler](#types) bölümü.

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

Azure Media Services, Widevine şifreleme eklemenize olanak sağlar. Aşağıdaki örnek, PlayReady ve Widevine için varlık teslim ilkesini eklenen gösterir.

```csharp
    static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        // Get the PlayReady license service URL.
        Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);


        // GetKeyDeliveryUrl for Widevine attaches the KID to the URL.
        // For example: https://amsaccount1.keydelivery.mediaservices.windows.net/Widevine/?KID=268a6dcb-18c8-4648-8c95-f46429e4927c.  
        // The WidevineBaseLicenseAcquisitionUrl (used below) also tells Dynamic Encryption 
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
> Widevine ile şifrelerken yalnızca DASH teslim et mümkün olacaktır. Varlık teslim Protokolü DASH belirttiğinizden emin olun.
> 
> 

## <a name="dynamicenvelopeencryption-asset-delivery-policy"></a>Varlık teslim ilkesini DynamicEnvelopeEncryption
Aşağıdaki **CreateAssetDeliveryPolicy** yöntemi oluşturur **AssetDeliveryPolicy** dinamik Zarf şifreleme uygulamak üzere yapılandırılmış (**DynamicEnvelopeEncryption**) Kesintisiz akış, HLS ve DASH protokollerine (bazı protokoller belirtmek karar verirseniz, bunlar akış verilerinden engellenir). Yöntem iki parametre alır: **Varlık** (varlık teslim ilkesini uygulamak istediğiniz) ve **IContentKey** (içerik anahtarı **EnvelopeEncryption** türü, daha fazla bilgi için bkz: [Bir içerik anahtarı oluşturma](media-services-dotnet-create-contentkey.md#envelope_contentkey)).

Bir AssetDeliveryPolicy oluştururken belirtebilirsiniz değerleri hakkında bilgi için bkz [AssetDeliveryPolicy tanımlarken kullanılan türler](#types) bölümü.   

```csharp
    private static void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {

        //  Get the Key Delivery Base Url by removing the Query parameter.  The Dynamic Encryption service will
        //  automatically add the correct key identifier to the url when it generates the Envelope encrypted content
        //  manifest.  Omitting the IV will also cause the Dynamic Encryption service to generate a deterministic
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

## <a id="types"></a>AssetDeliveryPolicy tanımlarken kullanılan türler

### <a id="AssetDeliveryProtocol"></a>AssetDeliveryProtocol

Aşağıdaki enum değerleri için varlık teslim Protokolü ayarlayabilirsiniz açıklar.

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

Aşağıdaki enum değerleri için varlık teslim İlkesi türü ayarlayabilirsiniz açıklar.  
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
```
### <a id="AssetDeliveryPolicyConfigurationKey"></a>AssetDeliveryPolicyConfigurationKey

Aşağıdaki enum değerleri için bir varlık teslim ilkesini belirli yapılandırmasını almak için kullanılan anahtarları yapılandırmak için ayarlayabilirsiniz açıklar.
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

