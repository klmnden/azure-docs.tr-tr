---
title: "Media Services .NET SDK kullanarak içerik anahtarı yetkilendirme ilkesini yapılandırma | Microsoft Docs"
description: "Media Services .NET SDK kullanarak içerik anahtarının yetkilendirme ilkesini yapılandırma hakkında bilgi edinin."
services: media-services
documentationcenter: 
author: Mingfeiy
manager: cfowler
editor: 
ms.assetid: 1a0aedda-5b87-4436-8193-09fc2f14310c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako;mingfeiy
ms.openlocfilehash: 75dd9107dca215a0b31db3d44bada69210fe9ac6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="dynamic-encryption-configure-content-key-authorization-policy"></a>Dinamik şifreleme: içerik anahtarı yetkilendirme ilkesini yapılandırma
[!INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]

## <a name="overview"></a>Genel Bakış
Microsoft Azure Media Services, Gelişmiş Şifreleme Standardı ((128-bit şifreleme anahtarları kullanılarak) AES ile) korumalı MPEG-DASH, kesintisiz akış ve HTTP canlı akış (HLS) akışlar sunmanıza olanak sağlar veya [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/). AMS Widevine DRM ile şifrelenmiş DASH akışları teslim etmenizi sağlar. PlayReady ve Widevine, Ortak Şifreleme (ISO/IEC 23001-7 CENC) belirtimi uyarınca şifrelenir.

Medya Hizmetleri de sağlar bir **anahtarı/lisans teslimat hizmeti** istemcilerin AES anahtarları veya şifrelenmiş içeriği yürütmek için PlayReady/Widevine lisansları elde edebilirsiniz.

Media Services'ın bir varlık şifrelemek istiyorsanız, bir şifreleme anahtarı ilişkilendirmeniz gerekir (**CommonEncryption** veya **EnvelopeEncryption**) varlık ile (açıklandığı gibi [burada](media-services-dotnet-create-contentkey.md)) ve anahtar için Yetkilendirme İlkeleri (Bu makalede anlatıldığı gibi) de yapılandırın.

Bir akış player tarafından istendiğinde Media Services belirtilen anahtarı dinamik olarak içeriğinizi AES veya DRM şifreleme kullanarak şifrelemek için kullanır. Akış şifresini çözmek için player anahtar teslim hizmetinden anahtarı ister. Kullanıcının anahtarını almak için yetkili olup olmadığına karar vermek için anahtar için belirtilen Yetkilendirme İlkeleri hizmet değerlendirir.

Media Services, anahtar isteğinde bulunan kullanıcıların kimlik doğrulamasını yapmanın birden çok yöntemini destekler. İçerik anahtarı yetkilendirme ilkesini bir veya daha fazla yetkilendirme kısıtlamaları olabilir: **açmak** veya **belirteci** kısıtlama. Belirteç kısıtlamalı ilkenin beraberinde bir Güvenli Belirteç Hizmeti (STS) tarafından verilmiş bir belirteç bulunmalıdır. Media Services belirteçleri destekler **basit Web belirteçleri** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) biçimi ve **JSON Web belirteci** ([JWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3)) biçimi.

Media Services, güvenli belirteç hizmetleri sağlamaz. Özel bir STS oluşturabilir veya Microsoft Azure ACS sorunu belirteçleri yararlanın. STS, belirteç kısıtlamasına yapılandırma dosyasında (Bu makalede anlatıldığı gibi) belirtilen belirtilen anahtarı ve sorunu talepleri imzalı bir belirteç oluşturmak için yapılandırılmalıdır. Media Services anahtar teslim hizmeti şifreleme anahtarını istemci için belirteç geçerliyse ve içerik anahtarı için yapılandırılmış talep belirteci eşleştiğinden döndürür.

Daha fazla bilgi için bkz.

[JWT belirteci kimlik doğrulaması](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Azure Media Services OWIN MVC tabanlı uygulama Azure Active Directory ile tümleştirme ve JWT talepleri temelinde içerik anahtar teslim kısıtlamak](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[Azure ACS sorunu belirteçleri kullanmak](http://mingfeiy.com/acs-with-key-services).

### <a name="some-considerations-apply"></a>Bazı dikkate alınması gereken noktalar vardır:
* AMS hesabınızı oluşturulduğunda bir **varsayılan** akış uç noktası ekleniyor hesabınızda **durduruldu** durumu. İçeriğinizi akış başlatmak ve dinamik paketleme ve dinamik şifreleme yararlanmak için akış uç noktanızı olması sahip **çalıştıran** durumu. 
* Varlığınızı Uyarlamalı bit hızlı MP4s ya da Uyarlamalı bit hızlı kesintisiz akış dosyaları içermelidir. Daha fazla bilgi için bkz: [bir varlık kodla](media-services-encode-asset.md).
* Karşıya yükleme ve kullanma, varlıkları kodlamak **AssetCreationOptions.StorageEncrypted** seçeneği.
* Aynı ilke yapılandırma gerektiren birden çok içerik anahtarı oluşturmayı planlıyorsanız, tek yetkilendirme ilkesi oluşturma ve birden çok içerik anahtarı ile yeniden önemle tavsiye edilir.
* Anahtar teslim hizmeti ContentKeyAuthorizationPolicy ve ilişkili nesnelerini (ilkesi seçenekleri ve kısıtlamaları) 15 dakika için önbelleğe alır.  Bir ContentKeyAuthorizationPolicy oluşturup, bir "Token" kısıtlama kullanılacağını belirtin, test ve ardından ilkesini "Açık" kısıtlama güncelleştirin, ilke ilkesini "Açık" sürümüne geçiş yapmadan önce yaklaşık 15 dakika sürer.
* Varlığınızın teslim ilkesini ekler veya güncelleştirirseniz, mevcut bulucuyu (varsa) silip yeni bir bulucu oluşturmanız gerekir.
* Şu anda, aşamalı indirme şifrelenemiyor.

## <a name="aes-128-dynamic-encryption"></a>AES-128 dinamik şifreleme
### <a name="open-restriction"></a>Açık kısıtlama
Açık sınırlama Sistem anahtarı anahtar istekte herkes dağıtılacak anlamına gelir. Bu kısıtlama sınama amacıyla yararlı olabilir.

Aşağıdaki örnek, bir açık yetkilendirme ilkesi oluşturur ve içerik anahtarı ekler.

    static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
    {
        // Create ContentKeyAuthorizationPolicy with Open restrictions
        // and create authorization policy
        IContentKeyAuthorizationPolicy policy = _context.
        ContentKeyAuthorizationPolicies.
        CreateAsync("Open Authorization Policy").Result;
        
        List<ContentKeyAuthorizationPolicyRestriction> restrictions =
            new List<ContentKeyAuthorizationPolicyRestriction>();

        ContentKeyAuthorizationPolicyRestriction restriction =
            new ContentKeyAuthorizationPolicyRestriction
            {
                Name = "HLS Open Authorization Policy",
                KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                Requirements = null // no requirements needed for HLS
            };

        restrictions.Add(restriction);

        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
            "policy", 
            ContentKeyDeliveryType.BaselineHttp, 
            restrictions, 
            "");

        policy.Options.Add(policyOption);

        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
    }


### <a name="token-restriction"></a>Belirteç kısıtlama
Bu bölümde, bir içerik anahtarı yetkilendirme ilkesi oluşturun ve içerik anahtarı ile ilişkilendirmek açıklar. Yetkilendirme İlkesi, kullanıcı (örneğin "doğrulama anahtarı" listede yer belirteci ile imzalandığı anahtarı yoksa) anahtarı alma yetkisi olup olmadığını belirlemek için hangi Yetkilendirme gereksinimlerin karşılanması gerekir açıklar.

Belirteç kısıtlamasına seçeneği yapılandırmak için belirtecin yetkilendirme gereksinimlerini tanımlamak için bir XML kullanmanız gerekir. Belirteç kısıtlamasına yapılandırma XML için aşağıdaki XML Şeması uygun olmalıdır.

#### <a id="schema"></a>Belirteç kısıtlamasına şeması
    <?xml version="1.0" encoding="utf-8"?>
    <xs:schema xmlns:tns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" xmlns:xs="http://www.w3.org/2001/XMLSchema">
      <xs:complexType name="TokenClaim">
        <xs:sequence>
          <xs:element name="ClaimType" nillable="true" type="xs:string" />
          <xs:element minOccurs="0" name="ClaimValue" nillable="true" type="xs:string" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenClaim" nillable="true" type="tns:TokenClaim" />
      <xs:complexType name="TokenRestrictionTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" name="AlternateVerificationKeys" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
          <xs:element name="Audience" nillable="true" type="xs:anyURI" />
          <xs:element name="Issuer" nillable="true" type="xs:anyURI" />
          <xs:element name="PrimaryVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
          <xs:element minOccurs="0" name="RequiredClaims" nillable="true" type="tns:ArrayOfTokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenRestrictionTemplate" nillable="true" type="tns:TokenRestrictionTemplate" />
      <xs:complexType name="ArrayOfTokenVerificationKey">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenVerificationKey" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
      <xs:complexType name="TokenVerificationKey">
        <xs:sequence />
      </xs:complexType>
      <xs:element name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
      <xs:complexType name="ArrayOfTokenClaim">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenClaim" nillable="true" type="tns:TokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenClaim" nillable="true" type="tns:ArrayOfTokenClaim" />
      <xs:complexType name="SymmetricVerificationKey">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:TokenVerificationKey">
            <xs:sequence>
              <xs:element name="KeyValue" nillable="true" type="xs:base64Binary" />
            </xs:sequence>
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="SymmetricVerificationKey" nillable="true" type="tns:SymmetricVerificationKey" />
    </xs:schema>

Yapılandırırken **belirteci** kısıtlanmış İlkesi, birincil doğrulama anahtarı **, belirtmelisiniz **veren** ve **İzleyici** parametreleri. ** Birincil doğrulama anahtarı **, belirteci imzalayan anahtarı içeren **veren** belirtecini veren güvenli belirteç hizmetidir. **İzleyici** (bazen adlı **kapsam**) belirteç veya belirteç erişim yetkisi verir kaynak amacı açıklar. Media Services anahtar teslim hizmeti, bu değerleri belirteci şablon değerleri eşleştiğini doğrular. 

Kullanırken **.NET için Media Services SDK'sı**, kullanabileceğiniz **TokenRestrictionTemplate** kısıtlama belirteci üretmek için sınıf.
Aşağıdaki örnek, bir belirteç kısıtlamasına bir yetkilendirme ilkesi oluşturur. Bu örnekte, istemci içeren bir belirteç sunmak gerekecektir: imzalama anahtarı (VerificationKey), bir belirteç verenin ve gerekli talep.

    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();

        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;

        List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                new List<ContentKeyAuthorizationPolicyRestriction>();

        ContentKeyAuthorizationPolicyRestriction restriction =
                new ContentKeyAuthorizationPolicyRestriction
                {
                    Name = "Token Authorization Policy",
                    KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                    Requirements = tokenTemplateString
                };

        restrictions.Add(restriction);

        //You could have multiple options 
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
                "Token option for HLS",
                ContentKeyDeliveryType.BaselineHttp,
                restrictions,
                null  // no key delivery data is needed for HLS
                );

        policy.Options.Add(policyOption);

        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);

        return tokenTemplateString;
    }

    static private string GenerateTokenRequirements()
    {
        TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);

        template.PrimaryVerificationKey = new SymmetricVerificationKey();
        template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();

        template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);

        return TokenRestrictionTemplateSerializer.Serialize(template);
    }

#### <a id="test"></a>Test simgesi
Anahtar yetkilendirme ilkesi için kullanılan belirteç kısıtlamasına dayalı bir test belirteci almak için aşağıdakileri yapın.

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate =
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

    // Generate a test token based on the the data in the given TokenRestrictionTemplate.
    // Note, you need to pass the key id Guid because we specified 
    // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
    Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);

    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
    Console.WriteLine();


## <a name="playready-dynamic-encryption"></a>PlayReady dinamik şifreleme
Media Services hakları ve PlayReady DRM çalışma zamanı için bir kullanıcının korumalı içeriği kayıttan çalışırken uygulamak istediğinize kısıtlamaları yapılandırmanıza olanak sağlar. 

İçeriğinizi PlayReady ile korurken, Yetkilendirme ilkesinde belirtmek için gereken şeyleri tanımlayan bir XML dizesi biri [PlayReady lisans şablonu](media-services-playready-license-template-overview.md). .NET için Media Services SDK **PlayReadyLicenseResponseTemplate** ve **PlayReadyLicenseTemplate** sınıfları, PlayReady lisans şablonu tanımlamanıza yardımcı olur.

[Bu konuda](media-services-protect-with-drm.md) içeriğinizi ile şifrelemek gösterilmiştir **PlayReady** ve **Widevine**.

### <a name="open-restriction"></a>Açık kısıtlama
Açık sınırlama Sistem anahtarı anahtar istekte herkes dağıtılacak anlamına gelir. Bu kısıtlama sınama amacıyla yararlı olabilir.

Aşağıdaki örnek, bir açık yetkilendirme ilkesi oluşturur ve içerik anahtarı ekler.

    static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
    {

        // Create ContentKeyAuthorizationPolicy with Open restrictions 
        // and create authorization policy          

        List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
        {
            new ContentKeyAuthorizationPolicyRestriction 
            { 
                Name = "Open", 
                KeyRestrictionType = (int)ContentKeyRestrictionType.Open, 
                Requirements = null
            }
        };

        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);

        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;


        contentKeyAuthorizationPolicy.Options.Add(policyOption);

        // Associate the content key authorization policy with the content key.
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;
    }

### <a name="token-restriction"></a>Belirteç kısıtlama
Belirteç kısıtlamasına seçeneği yapılandırmak için belirtecin yetkilendirme gereksinimlerini tanımlamak için bir XML kullanmanız gerekir. XML gösterilen XML şemasına uygun olmalıdır belirteci kısıtlama Yapılandırması [bu](#schema) bölümü.

    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();

        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;

        List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
        {
            new ContentKeyAuthorizationPolicyRestriction 
            { 
                Name = "Token Authorization Policy", 
                KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                Requirements = tokenTemplateString, 
            }
        };

        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);

        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;

        policy.Options.Add(policyOption);

        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKeyAuthorizationPolicy.Options.Add(policyOption);

        // Associate the content key authorization policy with the content key
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;

        return tokenTemplateString;
    }

    static private string GenerateTokenRequirements()
    {

        TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);

        template.PrimaryVerificationKey = new SymmetricVerificationKey();
        template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();


        template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);

        return TokenRestrictionTemplateSerializer.Serialize(template);
    } 

    static private string ConfigurePlayReadyLicenseTemplate()
    {
        // The following code configures PlayReady License Template using .NET classes
        // and returns the XML string.

        //The PlayReadyLicenseResponseTemplate class represents the template for the response sent back to the end user. 
        //It contains a field for a custom data string between the license server and the application 
        //(may be useful for custom app logic) as well as a list of one or more license templates.
        PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();

        // The PlayReadyLicenseTemplate class represents a license template for creating PlayReady licenses
        // to be returned to the end users. 
        //It contains the data on the content key in the license and any rights or restrictions to be 
        //enforced by the PlayReady DRM runtime when using the content key.
        PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
        //Configure whether the license is persistent (saved in persistent storage on the client) 
        //or non-persistent (only held in memory while the player is using the license).  
        licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;

        // AllowTestDevices controls whether test devices can use the license or not.  
        // If true, the MinimumSecurityLevel property of the license
        // is set to 150.  If false (the default), the MinimumSecurityLevel property of the license is set to 2000.
        licenseTemplate.AllowTestDevices = true;


        // You can also configure the Play Right in the PlayReady license by using the PlayReadyPlayRight class. 
        // It grants the user the ability to playback the content subject to the zero or more restrictions 
        // configured in the license and on the PlayRight itself (for playback specific policy). 
        // Much of the policy on the PlayRight has to do with output restrictions 
        // which control the types of outputs that the content can be played over and 
        // any restrictions that must be put in place when using a given output.
        // For example, if the DigitalVideoOnlyContentRestriction is enabled, 
        //then the DRM runtime will only allow the video to be displayed over digital outputs 
        //(analog video outputs won’t be allowed to pass the content).

        //IMPORTANT: These types of restrictions can be very powerful but can also affect the consumer experience. 
        // If the output protections are configured too restrictive, 
        // the content might be unplayable on some clients. For more information, see the PlayReady Compliance Rules document.

        // For example:
        //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);

        responseTemplate.LicenseTemplates.Add(licenseTemplate);

        return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
    }


Anahtar yetkilendirme ilkesi için bkz. kullanılan belirteç kısıtlamasına dayalı bir test belirteci almak için [bu](#test) bölümü. 

## <a id="types"></a>ContentKeyAuthorizationPolicy tanımlarken kullanılan türleri
### <a id="ContentKeyRestrictionType"></a>ContentKeyRestrictionType
    public enum ContentKeyRestrictionType
    {
        Open = 0,
        TokenRestricted = 1,
        IPRestricted = 2,
    }

### <a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType
    public enum ContentKeyDeliveryType
    {
      None = 0,
      PlayReadyLicense = 1,
      BaselineHttp = 2,
      Widevine = 3
    }

### <a id="TokenType"></a>TokenType
    public enum TokenType
    {
        Undefined = 0,
        SWT = 1,
        JWT = 2,
    }



## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a>Sonraki adım
İçerik anahtarının yetkilendirme ilkesini yapılandırmış, Git [varlık teslim ilkesini yapılandırmak nasıl](media-services-dotnet-configure-asset-delivery-policy.md) konu.

