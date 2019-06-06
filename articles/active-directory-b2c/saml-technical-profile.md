---
title: Azure Active Directory B2C, özel bir ilkede SAML teknik profili tanımlama | Microsoft Docs
description: Azure Active Directory B2C, özel bir ilkede SAML teknik profili tanımlayın.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 12/21/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: dca330f20548d3a93091f89dc8ab2b3cb92f50e2
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66512703"
---
# <a name="define-a-saml-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Bir Azure Active Directory B2C özel İlkesi'nde bir SAML teknik profili tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory (Azure AD) B2C, SAML 2.0 kimlik sağlayıcısı için destek sağlar. Bu makalede, standartlaştırılmış bu protokolü destekleyen bir talep sağlayıcı ile etkileşim kurmak için bir teknik profil ayrıntılarını açıklar. SAML teknik profili ile bir SAML tabanlı kimlik sağlayıcısı ile gibi devredebilir [ADFS](active-directory-b2c-custom-setup-adfs2016-idp.md) ve [Salesforce](active-directory-b2c-setup-sf-app-custom.md). Bu Federasyon oturum sosyal var olan oturum veya Kurumsal kimlikleri mümkün kılar.

## <a name="metadata-exchange"></a>Meta veri değişimi

SAML Protokolü gibi bir hizmet sağlayıcısı veya kimlik sağlayıcısı SAML partinin yapılandırmasını kullanıma sunmak için kullanılan bilgileri meta verilerdir. Meta verileri, hizmetleri, oturum açma ve oturum kapatma, sertifikaları, oturum açma yöntemi ve diğer konumunu tanımlar. Kimlik sağlayıcısı, Azure AD B2C ile iletişim kurma bilmek meta verileri kullanır. Meta veri XML biçiminde yapılandırılır ve böylece tarafa meta verilerin bütünlüğünü doğrulamak için bir dijital imza ile imzalanmış. SAML kimlik sağlayıcısı ile Azure AD B2C birleştiriyor, bir SAML isteğini başlatarak ve SAML yanıt bekleyen bir hizmet sağlayıcısı olarak görev yapar. Ve bazı durumlarda, başlatılan kimlik sağlayıcısı olarak da bilinen olan istenmeyen SAML kimlik doğrulaması, kabul eder. 

Meta veriler her iki taraf "Statik meta veri" veya "Dinamik meta veriler" olarak yapılandırılabilir. Statik modunda, bir taraftan tüm meta verileri kopyalayın ve tarafa ayarlayın. Diğer taraf yapılandırmasını dinamik olarak okurken dinamik modunda, URL için meta verileri ayarlayın. İlkeleri aynıdır, kimlik sağlayıcınızda Azure AD B2C teknik profil meta verileri ayarlama ve Azure AD B2C'de kimlik sağlayıcısı meta verileri ayarlama.

Her bir SAML kimlik sağlayıcısı, kullanıma ve hizmet sağlayıcısı, bu durumda Azure AD B2C'yi ayarlayın ve Azure AD B2C meta veriler kimlik sağlayıcısında ayarlamak için farklı adımlar vardır. Bunun nasıl yapılacağı hakkında yönergeler için kimlik sağlayıcınızın belgelerine bakın.

Aşağıdaki örnek, bir URL adresi için bir Azure AD B2C'Teknik profili SAML meta verilerini gösterir:

```
https://your-tenant-name.b2clogin.com/your-tenant-name/your-policy/samlp/metadata?idptp=your-technical-profile
```

Aşağıdaki değerleri değiştirin:

- **Kiracı adı Your** Kiracı adınızla fabrikam.b2clogin.com gibi.
- **ilke Your** ilke adınızla. İlke sağlayıcısı SAML teknik profili yapılandırdığınız veya bu ilkeden devralan bir ilke kullanın.
- **Teknik profil Your** SAML kimlik sağlayıcısı teknik profil adı ile.

## <a name="digital-signing-certificates-exchange"></a>Dijital imza sertifikaları exchange

Azure AD B2C ile SAML kimlik sağlayıcınız arasında güven oluşturmak için geçerli bir X509 sağlamak gereken sertifikayı özel anahtarla. Sertifikayı özel anahtarla (.pfx dosyası) Azure AD B2C İlkesi anahtar deposuna yükleyin. Azure AD B2C, sağladığınız sertifika kullanarak SAML oturum açma isteği dijital olarak imzalar. 

Sertifika, aşağıdaki şekillerde kullanılır:

- Azure AD B2C'yi oluşturur ve Azure AD B2C sertifikanın özel anahtarı kullanarak bir SAML isteğini imzalar. Sertifikanın ortak anahtarı Azure AD B2C'yi kullanarak isteği doğrular kimlik sağlayıcısı SAML isteği gönderilir. Azure AD B2C ortak sertifika teknik profil meta verileri erişilebilir. Alternatif olarak, SAML kimlik sağlayıcınız el ile .cer dosyasını karşıya yükleyebilirsiniz.
- Azure AD B2C'ye gönderilen verilerin kimlik sağlayıcısı oturum açtığında kimlik sağlayıcısının sertifikanın özel anahtarı kullanarak. Azure AD B2C kimlik sağlayıcısının ortak sertifikasını kullanarak verileri doğrular. Her bir kimlik sağlayıcısı kurulumu için farklı adımlar varsa, bunun nasıl yapılacağı hakkında yönergeler için kimlik sağlayıcınızın belgelerine bakın. Azure AD B2C'de ilkenizin kimlik sağlayıcısının meta verileri kullanarak sertifika genel anahtarı erişmesi gerekir.

Çoğu senaryo için otomatik olarak imzalanan bir sertifika kullanılabilir. X X509 kullanmak için önerilen üretim ortamları için bir sertifika yetkilisi tarafından verilen sertifika. Ayrıca, bu belgenin sonraki bölümlerinde açıklandığı gibi bir üretim dışı ortamda için SAML imzalama her iki kenarı da devre dışı bırakabilirsiniz.

Meta veriler ve sertifika exchange Aşağıdaki diyagramda gösterilmiştir:

![Meta veriler ve sertifika exchange](media/saml-technical-profile/technical-profile-idp-saml-metadata.png)


## <a name="digital-encryption"></a>Dijital şifreleme

SAML yanıtını onayı şifrelemek için kimlik sağlayıcısı her zaman Azure AD B2C'Teknik profili içinde bir şifreleme sertifikası, ortak anahtar kullanır. Azure AD B2C, verilerin şifresini çözmek gerektiğinde, özel şifreleme sertifikası bölümünü kullanır.

SAML yanıtını onayı şifrelemek için:

1. Geçerli X509 karşıya sertifika (.pfx dosyası) Azure AD B2C İlkesi anahtar deposuna özel anahtara sahip.
2. Ekleme bir **CryptographicKey** tanımlayıcısına sahip öğe `SamlAssertionDecryption` teknik profiline **CryptographicKeys** koleksiyonu. Ayarlama **Storagereferenceıd** 1. adımda oluşturduğunuz ilke anahtarı adı.
3. Teknik profil meta verileri ayarlama **WantsEncryptedAssertions** için `true`.
4. Kimlik sağlayıcısı ile yeni Azure AD B2C teknik profil meta verileri güncelleştirin. Görmelisiniz **KeyDescriptor** ile **kullanın** özelliğini `encryption` , sertifikanın ortak anahtarı içeren.

Aşağıdaki örnek, meta verileri Azure AD B2C'Teknik profili şifreleme bölümünü gösterir:

```XML
<KeyDescriptor use="encryption">
  <KeyInfo xmlns="https://www.w3.org/2000/09/xmldsig#">
    <X509Data>
      <X509Certificate>valid certificate</X509Certificate>
    </X509Data>
  </KeyInfo>
</KeyDescriptor>
```
    
## <a name="protocol"></a>Protocol

**Adı** Protokolü öğesinin özniteliği ayarlanması gerekiyor `SAML2`. 

## <a name="output-claims"></a>Çıkış talep
 
**OutputClaims** ögesinin altında SAML kimlik sağlayıcısı tarafından döndürülen talepleri listesini `AttributeStatement` bölümü. İlkeniz için tanımlanan kimlik sağlayıcısı adını tanımlanan talep eşleştirmek gerekebilir. Ayarladığınız sürece kimlik sağlayıcısı tarafından döndürülen olmayan talepleri de içerebilir `DefaultValue` özniteliği.
 
SAML onaylaması okunacak **NamedId** içinde **konu** normalleştirilmiş bir talep, talep kümesine **PartnerClaimType** için `assertionSubjectName`. Emin **Nameıd** onaylama XML ilk değer. Birden fazla onaylama tanımladığınızda, Azure AD B2C son onaylama konu değerini seçer.
 
**OutputClaimsTransformations** öğe koleksiyonu içerebilir **OutputClaimsTransformation** çıkış talep değiştirmek veya yenilerini oluşturmak için kullanılan öğeleri.
 
Aşağıdaki örnek, Facebook kimlik sağlayıcısı tarafından döndürülen talepleri gösterir:

- **İssuerUserId** talep eşlendi **assertionSubjectName** talep.
- **First_name** talep eşlendi **givenName** talep.
- **Soyadı** talep eşlendi **Soyadı** talep.
- **DisplayName** Adı Eşleme olmadan talep.
- **E-posta** Adı Eşleme olmadan talep.
 
Teknik profil de kimlik sağlayıcısı tarafından döndürülen olmayan talepleri döndürür: 
 
- **Identityprovider** kimlik sağlayıcısının adını içeren talep.
- **AuthenticationSource** varsayılan değeri olan talep **socialIdpAuthentication**.
 
```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="assertionSubjectName" />
  <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="first_name" />
  <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="last_name" />
  <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
  <OutputClaim ClaimTypeReferenceId="email"  />
  <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="contoso.com" />
  <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
</OutputClaims>
```

## <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| PartnerEntity | Evet | SAML kimlik sağlayıcısı meta verileri URL'si. Kimlik sağlayıcısı meta verileri kopyalayın ve içinde CDATA öğesi Ekle `<![CDATA[Your IDP metadata]]>` |
| WantsSignedRequests | Hayır | Teknik profili tüm giden kimlik doğrulama isteklerinin imzalanmasını gerekli olup olmadığını gösterir. Olası değerler: `true` veya `false`. Varsayılan değer `true` şeklindedir. Değer ayarlandığında `true`, **SamlMessageSigning** şifreleme anahtarının belirtilmesi gerekir ve tüm giden kimlik doğrulama isteklerini imzalanmıştır. Değer ayarlanmışsa `false`, **SigAlg** ve **imza** parametreleri (sorgu dizesi veya parametre gönderin) istekten göz ardı edilir. Bu meta veriler ayrıca meta veriler denetimleri **AuthnRequestsSigned** çıktı kimlik sağlayıcısı ile paylaşılan Azure AD B2C teknik profil meta verilerinde özniteliği. Azure AD B2C, istek oturum olmayan değerini **WantsSignedRequests** teknik profili meta veri kümesine `false` ve kimlik sağlayıcısı meta verileri **WantAuthnRequestsSigned** olduğu kümesine `false` veya hiçbiri belirtilmemelidir. |
| XmlSignatureAlgorithm | Hayır | SAML isteğini imzalamak için Azure AD B2C kullanan yöntemi. Bu meta veri değerini denetler **SigAlg** parametre (sorgu dizesi veya parametre gönderin) SAML isteğindeki. Olası değerler: `Sha256`, `Sha384`, `Sha512`, veya `Sha1`. Her iki tarafında aynı değere sahip imza algoritması'ı yapılandırdığınızdan emin olun. Sertifikanızı destekleyen algoritmasını kullanın. | 
| WantsSignedAssertions | Hayır | Teknik profil imzalanmasını gelen tüm onayları gerekli olup olmadığını gösterir. Olası değerler: `true` veya `false`. Varsayılan değer `true` şeklindedir. Değer ayarlanmışsa `true`, tüm onayları bölüm `saml:Assertion` gönderilen Azure AD B2C'ye sağlayıcısı kimliği tarafından imzalanması gerekir. Değer ayarlanmışsa `false`, onaylamalar kimlik sağlayıcısı oturum olmamalıdır, ancak Azure AD B2C'yi aşması durumunda, imza doğrulanmaz. Bu meta veriler ayrıca meta verileri bayrağı denetler **WantsAssertionsSigned**, kimlik sağlayıcısı ile paylaşılan Azure AD B2C teknik profil meta verilerinde çıkış olduğu. Onaylamalar doğrulama devre dışı bırakırsanız, ayrıca yanıt İmza doğrulamasını devre dışı bırakmak isteyebilirsiniz (daha fazla bilgi için **ResponsesSigned**). |
| ResponsesSigned | Hayır | Olası değerler: `true` veya `false`. Varsayılan değer `true` şeklindedir. Değer ayarlanmışsa `false`, kimlik sağlayıcısı SAML yanıtını oturum olmamalıdır, ancak Azure AD B2C'yi aşması durumunda, imza doğrulanmaz. Değer ayarlanmışsa `true`, Azure AD B2C kimlik sağlayıcısı tarafından gönderilen SAML yanıtını imzalanır ve doğrulanması gerekir. SAML yanıtını doğrulaması devre dışı bırakırsanız, ayrıca onaylama İmza doğrulamasını devre dışı bırakmak isteyebilirsiniz (daha fazla bilgi için **WantsSignedAssertions**). |
| WantsEncryptedAssertions | Hayır | Teknik profil şifrelenmesini gelen tüm onayları gerekli olup olmadığını gösterir. Olası değerler: `true` veya `false`. Varsayılan değer `false` şeklindedir. Değer ayarlanmışsa `true`, Azure AD B2C kimlik sağlayıcısı tarafından gönderilen bir onayları imzalanmalıdır ve **SamlAssertionDecryption** şifreleme anahtarı belirtilmesi gerekiyor. Değer ayarlanmışsa `true`, Azure AD B2C teknik profil meta verileri içeren **şifreleme** bölümü. Kimlik sağlayıcısı meta verileri okur ve SAML yanıtını onayı Azure AD B2C teknik profil meta verilerinde sağlanan ortak anahtarla şifreler. Onaylamalar şifrelemeyi etkinleştirirseniz, ayrıca yanıt imza doğrulaması devre dışı bırakmanız gerekebilir (daha fazla bilgi için **ResponsesSigned**). | 
| IdpInitiatedProfileEnabled | Hayır | Bir çoklu oturum açma oturumu profili bir SAML kimlik sağlayıcısı profili tarafından başlatılan etkinleştirilip etkinleştirilmediğini gösterir. Olası değerler: `true` veya `false`. Varsayılan değer: `false`. Flow'da kimlik sağlayıcısı tarafından başlatılan harici olarak kullanıcının kimliği doğrulanır ve ardından belirteci kullanır, düzenleme adımlarının yürütür ve ardından bağlı olan taraf uygulaması için bir yanıt gönderir. Azure AD B2C, istenmeyen bir yanıtı gönderilir. |
| NameIdPolicyFormat | Hayır | İstenen konu temsil etmek için kullanılacak ad tanımlayıcı kısıtlamaları belirtir. Atlanırsa, tanımlayıcı istenen konu için kimlik sağlayıcısı tarafından desteklenen herhangi bir türde kullanılabilir. Örneğin, `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`. **NameIdPolicyFormat** kullanılabilir **NameIdPolicyAllowCreate**. Kimlik sağlayıcınızın kimliği ilkeleri hakkında hangi adı desteklenen kılavuzu belgelerine bakın. |
| NameIdPolicyAllowCreate | Hayır | Kullanırken **NameIdPolicyFormat**, belirtebilirsiniz `AllowCreate` özelliği **NameIDPolicy**. Bu meta veri değeri `true` veya `false` kimlik sağlayıcısı oturum açma akışı sırasında yeni bir hesap oluşturmak için izin verilip verilmeyeceğini belirtmek için. Bunun nasıl yapılacağı hakkında yönergeler için kimlik sağlayıcınızın belgelerine bakın. |
| AuthenticationRequestExtensions | Hayır | Azure AD arasında BC üzerinde anlaşılan isteğe bağlı bir protokol iletisi uzantı öğeleri ve kimlik sağlayıcısı. Uzantı, XML biçiminde görüntülenir. CDATA öğesi içindeki XML verileri ekleme `<![CDATA[Your IDP metadata]]>`. Kimlik sağlayıcınızın belgeleri uzantıları öğesi destekleyip desteklemediğini görmek için kontrol edin. |
| IncludeAuthnContextClassReferences | Hayır | Kimlik doğrulaması bağlamı sınıf tanımlayan bir veya daha fazla URI başvuruları belirtir. Örneğin, kullanıcı adı ve parolanızla yalnızca oturum açmasına izin vermek için değerine `urn:oasis:names:tc:SAML:2.0:ac:classes:Password`. Kullanıcı adı ve parola ile oturum korumalı bir oturum (SSL/TLS) izin vermek için belirtin `PasswordProtectedTransport`. Ara Kılavuz kimlik sağlayıcınızın belgelerine **AuthnContextClassRef** desteklenen bir URI'leri. |
| IncludeKeyInfo | Hayır | SAML kimlik doğrulama isteği bağlama ayarlandığında sertifikanın ortak anahtarı içerip içermediğini gösteren `HTTP-POST`. Olası değerler: `true` veya `false`. |

## <a name="cryptographic-keys"></a>Şifreleme anahtarları

**CryptographicKeys** öğesi aşağıdaki öznitelikler içerir:

| Öznitelik |Gerekli | Açıklama |
| --------- | ----------- | ----------- |
| SamlMessageSigning |Evet | X509 SAML iletileri imzalamak için kullanılacak sertifikayı (RSA anahtar kümesi). Azure AD B2C, istekleri oturum ve bunları kimlik sağlayıcısına göndermek için bu anahtarı kullanır. |
| SamlAssertionDecryption |Evet | X509 SAML iletilerin şifresini çözmek için kullanılacak sertifikayı (RSA anahtar kümesi). Bu sertifika, kimlik sağlayıcısı tarafından sağlanmalıdır. Azure AD B2C kimlik sağlayıcısı tarafından gönderilen verilerin şifresini çözmek için bu sertifikayı kullanır. |
| MetadataSigning |Hayır | X509 SAML meta verilerini imzalamak için kullanılacak sertifikayı (RSA anahtar kümesi). Azure AD B2C, meta verilerini imzalamak için bu anahtarı kullanır.  |

## <a name="examples"></a>Örnekler

- [ADFS özel ilkeleri kullanarak bir SAML kimlik sağlayıcısı olarak Ekle](active-directory-b2c-custom-setup-adfs2016-idp.md)
- [SAML aracılığıyla Salesforce hesaplarını kullanarak oturum açın](active-directory-b2c-setup-sf-app-custom.md)













