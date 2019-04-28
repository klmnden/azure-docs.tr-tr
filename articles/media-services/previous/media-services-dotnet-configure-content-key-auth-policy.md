---
title: Media Services .NET SDK'sını kullanarak bir içerik anahtarı yetkilendirme ilkesini yapılandırma | Microsoft Docs
description: Media Services .NET SDK'sını kullanarak bir içerik anahtarı yetkilendirme ilkesini yapılandırmayı öğrenin.
services: media-services
documentationcenter: ''
author: mingfeiy
manager: femila
editor: ''
ms.assetid: 1a0aedda-5b87-4436-8193-09fc2f14310c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako;mingfeiy
ms.openlocfilehash: 744887a3b23518a5b970a3fda94bf990806bd2d8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61230279"
---
# <a name="dynamic-encryption-configure-a-content-key-authorization-policy"></a>Dinamik şifreleme: Bir içerik anahtarı yetkilendirme ilkesini yapılandırma
[!INCLUDE [media-services-selector-content-key-auth-policy](../../../includes/media-services-selector-content-key-auth-policy.md)]

## <a name="overview"></a>Genel Bakış
 Azure Media Services, 128 bit şifreleme anahtarları kullanılarak Gelişmiş Şifreleme Standardı (AES ile) korumalı MPEG-DASH, kesintisiz akış ve HTTP canlı akışı (HLS) akışlar sunmanıza olanak kullanabilirsiniz veya [PlayReady dijital hak yönetimi (DRM) ](https://www.microsoft.com/playready/overview/). Media Services ile Widevine DRM ile şifrelenmiş DASH akışları teslim edebilirsiniz. PlayReady ve Widevine, ortak şifreleme (ISO/IEC 23001-7 CENC) belirtimi uyarınca şifrelenir.

Media Services, kendisinden şifrelenmiş içeriği yürütmek için PlayReady/Widevine lisansları veya AES anahtarları istemciler elde edebilirsiniz bir anahtar/lisans teslim hizmeti de sağlar.

Media Services'ın bir varlık şifrelemek isterseniz, bir şifreleme anahtarı (CommonEncryption veya EnvelopeEncryption) ilişkilendirilecek varlıkla gerekir. Daha fazla bilgi için [.NET ile ContentKeys oluşturma](media-services-dotnet-create-contentkey.md). (Bu makalede anlatıldığı gibi) anahtar yetkilendirme ilkelerini yapılandırmak gerekir.

Bir akışa bir oynatıcı tarafından istendiğinde Media Services dinamik olarak içeriğinizi AES veya DRM şifreleme kullanarak şifrelemek için belirtilen anahtar kullanır. Oynatıcı, akışın şifresini çözmek için anahtar teslim hizmetinden anahtarı ister. Kullanıcı anahtarı almak için yetki verilip verilmediğini belirlemek için anahtar için belirtilen Yetkilendirme İlkeleri hizmet tarafından değerlendirilir.

Media Services, anahtar isteğinde bulunan kullanıcıların kimlik doğrulamasını yapmanın birden çok yöntemini destekler. İçerik anahtarı yetkilendirme ilkesinin bir veya daha fazla yetkilendirme kısıtlaması olabilir. Seçenekler şunlardır: açık veya belirteç kısıtlaması. Belirteç kısıtlamalı ilkenin beraberinde bir güvenlik belirteci hizmeti (STS) tarafından verilmiş bir belirteç bulunmalıdır. Media Services, basit web belirteci belirteçleri destekler ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) biçimi ve JSON Web Token ([JWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3)) biçimi.

Media Services, STS sağlamaz. Özel STS oluşturma ya da Azure Access Control Service sorunu belirteçleri kullanın. STS belirteci kısıtlama yapılandırmada (Bu makalede anlatıldığı gibi) belirtilen belirtilen anahtarı ve sorunu talepleri ile imzalanmış bir belirteç oluşturmak için yapılandırılmalıdır. Belirteç geçerliyse ve belirteçteki talepler içerik anahtarı için yapılandırılanlarla eşleşen, Media Services anahtar dağıtımı hizmetiyle şifreleme anahtarının istemciye döndürür.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [JWT belirteci kimlik doğrulaması](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)
- [Azure Media Services OWIN MVC tabanlı bir uygulamayı Azure Active Directory ile tümleştirin ve içerik anahtar teslim JWT taleplere göre kısıtlama](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/)

### <a name="some-considerations-apply"></a>Bazı önemli noktalar geçerlidir
* Media Services hesabınız oluşturulduğunda hesabınıza “Durdurulmuş” durumda bir varsayılan akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için akış uç noktanızı "Çalışıyor" durumunda olması gerekir. 
* Varlığınız bir dizi hızı Uyarlamalı MP4 veya uyarlamalı bit hızlı kesintisiz akış dosyaları içermelidir. Daha fazla bilgi için [bir varlığı kodlama](media-services-encode-asset.md).
* Karşıya yükleme ve varlıklarınızı AssetCreationOptions.StorageEncrypted seçeneğini kullanarak kodlayın.
* Aynı ilke yapılandırmasını gerektiren birden çok içerik anahtarı kullanmayı planlıyorsanız, tek bir yetkilendirme ilkesi oluşturma ve birden çok içerik anahtarı ile yeniden öneririz.
* Anahtar dağıtımı hizmetiyle ContentKeyAuthorizationPolicy ve ilgili nesneleri (ilkesi seçenekleri ve kısıtlamaları) 15 dakika için önbelleğe alır. ContentKeyAuthorizationPolicy oluşturabilir ve sonra ilke için açık kısıtlama güncelleştirme belirteç kısıtlamasına kullanın ve test için belirtin. Bu işlem, ilke açık sürümüne ilke anahtarları önce yaklaşık 15 dakika sürer.
* Varlığınızın teslim ilkesini ekler veya güncelleştirirseniz, varsa mevcut bulucuyu silip yeni bir bulucu oluşturmanız gerekir.
* Şu anda, ilerlemeli indirmeler şifrelenemiyor.
* Bir Media Services akış uç noktası, joker karakter olarak ön yanıt CORS 'Access-Control-Allow-Origin' üst bilgisi değeri ayarlar. '\*'. Bu değer, Azure Media Player, Roku ve JWPlayer ve diğerleri dahil olmak üzere de çoğu oyuncuları ile çalışır. Kimlik bilgilerini modunda "ekleme" olarak, kendi dashjs içinde XMLHttpRequest joker izin vermez çünkü dashjs kullanan bazı oyuncuların ancak çalışmıyor "\*" 'Access-Control-Allow-Origin' değeri olarak. Tek bir etki alanı istemcinizden barındırıyorsanız ön yanıt üst bilgisinde dashjs içinde bu sınırlama için bir geçici çözüm olarak, Media Services, etki alanı belirtebilirsiniz. Yardım için Azure portalından bir destek bileti açın.

## <a name="aes-128-dynamic-encryption"></a>AES-128 dinamik şifreleme
### <a name="open-restriction"></a>Açık kısıtlama
Açık kısıtlama Sistem anahtarı bir anahtar istekte herkese sunar anlamına gelir. Bu kısıtlama, sınama amacıyla yararlı olabilir.

Aşağıdaki örnek, bir açık yetkilendirme ilkesi oluşturur ve içerik anahtarı ekler:
```csharp
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

        // Add ContentKeyAuthorizationPolicy to ContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
    }
```

### <a name="token-restriction"></a>Belirteç kısıtlama
Bu bölümde, bir içerik anahtarı yetkilendirme ilkesi oluşturma ve içerik anahtarıyla ilişkilendirmek açıklar. Yetkilendirme İlkesi, kullanıcı anahtarı almak için yetkili olup olmadığını belirlemek için hangi Yetkilendirme gereksinimleri karşılanmalıdır açıklar. Örneğin, doğrulama anahtar listesi belirteç birlikte imzalandığı anahtar içeriyor mu?

Belirteç kısıtlamasına seçeneği yapılandırmak için belirtecin yetkilendirme gereksinimlerini tanımlamak için bir XML kullanmanız gerekir. Belirteç kısıtlamasına yapılandırma XML aşağıdaki XML şemaya uygun olmalıdır:
```csharp
#### Token restriction schema
    <?xml version="1.0" encoding="utf-8"?>
    <xs:schema xmlns:tns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" xmlns:xs="https://www.w3.org/2001/XMLSchema">
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
```
Belirteç kısıtlamalı ilkenin yapılandırdığınızda, birincil doğrulama anahtarı, veren ve İzleyici parametrelerini belirtmeniz gerekir. Birincil doğrulama anahtarı belirteç birlikte imzalandığı anahtarını içerir. Belirteci veren STS veren olur. Belirtecin amacı (kapsam olarak da adlandırılır) İzleyici açıklayan veya kaynak belirteci erişimini yetkilendirir. Media Services anahtar dağıtımı hizmetiyle belirtecindeki bu değerleri şablon değerleri eşleştiğini doğrular.

.NET için Media Services SDK'sı kullandığınızda, TokenRestrictionTemplate sınıfı kısıtlama belirtecini oluşturmak için kullanabilirsiniz.
Aşağıdaki örnek, bir belirteç kısıtlamasına bir yetkilendirme ilkesi oluşturur. Bu örnekte, istemci bir imzalama anahtarı (VerificationKey), belirteci veren ve gerekli talepleri içeren bir belirteç sunması gerekir.
```csharp
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

        // Add ContentKeyAuthorizationPolicy to ContentKey
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
```
#### <a name="test-token"></a>Test simgesi
Anahtar yetkilendirme ilkesi için kullanılan belirteç kısıtlamasına dayalı bir test belirteci almak için aşağıdakileri yapın:
```csharp
    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate =
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

    // Generate a test token based on the data in the given TokenRestrictionTemplate.
    // Note, you need to pass the key id Guid because we specified 
    // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
    Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);

    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
    Console.WriteLine();
```

## <a name="playready-dynamic-encryption"></a>PlayReady dinamik şifreleme
Korumalı içeriği kayıttan yürütme bir kullanıcı çalıştığında zorlamak için PlayReady DRM çalışma zamanının istediğiniz kısıtlamaları ve haklarını yapılandırması için Media Services'ı kullanabilirsiniz. 

PlayReady ile içeriğinizi korumanıza, yetkilendirme ilkenizde belirtmeniz gerekir şeylerden biri olduğunu tanımlayan bir XML dizesi [PlayReady lisans şablonu](media-services-playready-license-template-overview.md). .NET için Media Services SDK PlayReadyLicenseResponseTemplate ve PlayReadyLicenseTemplate sınıfları, PlayReady lisans şablonu tanımlama yardımcı olur.

PlayReady ve Widevine içeriğinizi şifrelemek öğrenmek için bkz. [kullanım PlayReady ve/veya Widevine dinamik ortak şifreleme](media-services-protect-with-playready-widevine.md).

### <a name="open-restriction"></a>Açık kısıtlama
Açık kısıtlama Sistem anahtarı bir anahtar istekte herkese sunar anlamına gelir. Bu kısıtlama, sınama amacıyla yararlı olabilir.

Aşağıdaki örnek, bir açık yetkilendirme ilkesi oluşturur ve içerik anahtarı ekler:

```csharp
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
```

### <a name="token-restriction"></a>Belirteç kısıtlama
Belirteç kısıtlamasına seçeneği yapılandırmak için belirtecin yetkilendirme gereksinimlerini tanımlamak için bir XML kullanmanız gerekir. Belirteç kısıtlamasına yapılandırma XML "Belirteci kısıtlama schema" bölümünde gösterilen XML şemasına uygun olmalıdır.

```csharp
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

        // Add ContentKeyAuthorizationPolicy to ContentKey
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
        // It grants the user the ability to play back the content subject to the zero or more restrictions 
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
```

Anahtar yetkilendirme ilkesi için kullanılan belirteç kısıtlamasına dayalı bir test belirteci almak için bkz: "[Test belirteci](#test-token)" bölümü. 

## <a id="types"></a>ContentKeyAuthorizationPolicy tanımlarken kullanılan türler
### <a id="ContentKeyRestrictionType"></a>ContentKeyRestrictionType

```csharp
    public enum ContentKeyRestrictionType
    {
        Open = 0,
        TokenRestricted = 1,
        IPRestricted = 2,
    }
```

### <a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType

```csharp 
    public enum ContentKeyDeliveryType
    {
      None = 0,
      PlayReadyLicense = 1,
      BaselineHttp = 2,
      Widevine = 3
    }
```

### <a id="TokenType"></a>TokenType

```csharp
    public enum TokenType
    {
        Undefined = 0,
        SWT = 1,
        JWT = 2,
    }
```

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
İçerik anahtarının yetkilendirme ilkesini yapılandırdıktan sonra bkz. [varlık teslim ilkesi yapılandırma](media-services-dotnet-configure-asset-delivery-policy.md).

