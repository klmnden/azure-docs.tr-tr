---
title: Azure Active Directory B2C, özel bir ilkede SAML teknik profili tanımlama | Microsoft Docs
description: Azure Active Directory B2C, özel bir ilkede SAML teknik profili tanımlayın.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 347a437a9f45f29348e97c616c985764135e5427
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51687478"
---
# <a name="define-a-saml-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Bir Azure Active Directory B2C özel İlkesi'nde bir SAML teknik profili tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory (Azure AD) B2C, SAML 2.0 kimlik sağlayıcısı için destek sağlar. Bu makalede, standartlaştırılmış bu protokolü destekleyen bir talep sağlayıcı ile etkileşim kurmak için bir teknik profil ayrıntılarını açıklar. Gibi kimlik sağlayıcısı SAML ile federasyona ile SAML teknik profili alarak [ADFS](active-directory-b2c-custom-setup-adfs2016-idp.md) ve [Salesforce](active-directory-b2c-setup-sf-app-custom.md), kullanıcılarınız kendi mevcut sosyal veya Kurumsal kimlik ile oturum açmanız izin verme.

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
- Azure AD B2C'ye gönderilen verilerin kimlik sağlayıcısı oturum açtığında kimlik sağlayıcısının sertifikanın özel anahtarı kullanarak. Azure AD B2C kimlik sağlayıcısının ortak sertifikasını kullanarak verileri doğrular. Her bir kimlik sağlayıcısı kurulumu için farklı adımlar varsa, bunun nasıl yapılacağı hakkında yönergeler için kimlik sağlayıcısı'nın belgelerine bakın. Azure AD B2C'de ilkenizin kimlik sağlayıcısının meta verileri kullanarak sertifika genel anahtarı erişmesi gerekir.

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
  <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
    <X509Data>
      <X509Certificate>valid certificate</X509Certificate>
    </X509Data>
  </KeyInfo>
</KeyDescriptor>
```

## <a name="identity-provider-initiated-flow"></a>Kimlik sağlayıcısı tarafından başlatılan akış

IDP tarafından başlatılan bir çoklu oturum açma oturumunda (istenmeyen istek), istenmeyen bir SAML yanıtını için hizmet sağlayıcısı, bu durumda, bir Azure AD B2C'Teknik profili gönderilir. Bu akış, kullanıcı web uygulaması aracılığıyla ilk geçmez, ancak kimlik sağlayıcısına yönlendirilir. Kimlik doğrulaması sayfası isteği gönderildiğinde, kullanıcıya kimlik sağlayıcısı tarafından sağlanır. Kullanıcı oturum açma tamamlandıktan ve ardından Azure AD B2C'ye bir onayları içeren bir SAML yanıtını istek yönlendirilir. Azure AD B2C onaylamaları okur ve yeni bir SAML belirteci verir ve ardından kullanıcının bağlı olan taraf uygulaması yönlendirir. Yeniden yönlendirmeleri yapılır **AssertionConsumerService** öğenin **konumu** özelliği.


![SAML IDP tarafından başlatılan](media/saml-technical-profile/technical-profile-idp-saml-idp-initiated.png) 

Bir kimlik sağlayıcısı oluşturma akış başlattığında aşağıdaki ilke gereksinimlerini göz önünde bulundurun:

- İlk düzenleme adımı işaret eden bir SAML teknik profili için exchange tek bir talep olması gerekir.
- Bir meta veri öğesi adlı teknik profil olmalıdır **IdpInitiatedProfileEnabled** kümesine `true`.
- Bağlı olan taraf SAML bağlı olan taraf İlkesi gerekir.
- Bağlı olan taraf İlkesi olmalıdır öğesi adlı bir meta veri **IdpInitiatedProfileEnabled** kümesine `true`.
- İstenmeyen yanıtın gönderilmesi gerekiyor `/your-tenant/your-policy/samlp/sso/assertionconsumer` uç noktası. Yanıtlı dahil herhangi bir geçiş durumu, bağlı olan tarafın iletilir. Aşağıdaki değerleri değiştirin: **Kiracı your** Kiracı adınızla. **ilke Your** bağlı olan taraf ilke adınızla.
    
## <a name="protocol"></a>Protokol

**Adı** Protokolü öğesinin özniteliği ayarlanması gerekiyor `SAML2`. 

## <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| PartnerEntity | Evet | SAML kimlik sağlayıcısı meta verileri URL'si. Kimlik sağlayıcısı meta verileri kopyalayın ve içinde CDATA öğesi Ekle `<![CDATA[Your IDP metadata]]>` |
| WantsSignedRequests | Hayır | Teknik profili tüm giden kimlik doğrulama isteklerinin imzalanmasını gerekli olup olmadığını gösterir. Olası değerler: `true` veya `false`. Varsayılan değer `true` şeklindedir. Değer ayarlandığında `true`, **SamlMessageSigning** şifreleme anahtarının belirtilmesi gerekir ve tüm giden kimlik doğrulama isteklerini imzalanmıştır. Değer ayarlanmışsa `false`, **SigAlg** ve **imza** parametreleri (sorgu dizesi veya parametre gönderin) istekten göz ardı edilir. Bu meta veriler ayrıca meta veriler denetimleri **AuthnRequestsSigned** çıktı kimlik sağlayıcısı ile paylaşılan Azure AD B2C teknik profil meta verilerinde özniteliği. |
| XmlSignatureAlgorithm | Hayır | SAML isteğini imzalamak için Azure AD B2C kullanan yöntemi. Bu meta veri değerini denetler **SigAlg** parametre (sorgu dizesi veya parametre gönderin) SAML isteğindeki. Olası değerler: `Sha256`, `Sha384`, `Sha512`, veya `Sha1`. Her iki tarafında aynı değere sahip imza algoritması'ı yapılandırdığınızdan emin olun. Sertifikanızı destekleyen algoritmasını kullanın. | 
| WantsSignedAssertions | Hayır | Teknik profil imzalanmasını gelen tüm onayları gerekli olup olmadığını gösterir. Olası değerler: `true` veya `false`. Varsayılan değer `true` şeklindedir. Değer ayarlanmışsa `true`, tüm onayları bölüm `saml:Assertion` gönderilen Azure AD B2C'ye sağlayıcısı kimliği tarafından imzalanması gerekir. Değer ayarlanmışsa `false`, onaylamalar kimlik sağlayıcısı oturum olmamalıdır, ancak Azure AD B2C'yi aşması durumunda, imza doğrulanmaz. Bu meta veriler ayrıca meta verileri bayrağı denetler **WantsAssertionsSigned**, kimlik sağlayıcısı ile paylaşılan Azure AD B2C teknik profil meta verilerinde çıkış olduğu. Onaylamalar doğrulama devre dışı bırakırsanız, ayrıca yanıt İmza doğrulamasını devre dışı bırakmak isteyebilirsiniz (daha fazla bilgi için **ResponsesSigned**). |
| ResponsesSigned | Hayır | Olası değerler: `true` veya `false`. Varsayılan değer `true` şeklindedir. Değer ayarlanmışsa `false`, kimlik sağlayıcısı SAML yanıtını oturum olmamalıdır, ancak Azure AD B2C'yi aşması durumunda, imza doğrulanmaz. Değer ayarlanmışsa `true`, Azure AD B2C kimlik sağlayıcısı tarafından gönderilen SAML yanıtını imzalanır ve doğrulanması gerekir. SAML yanıtını doğrulaması devre dışı bırakırsanız, ayrıca onaylama İmza doğrulamasını devre dışı bırakmak isteyebilirsiniz (daha fazla bilgi için **WantsSignedAssertions**). |
| WantsEncryptedAssertions | Hayır | Teknik profil şifrelenmesini gelen tüm onayları gerekli olup olmadığını gösterir. Olası değerler: `true` veya `false`. Varsayılan değer `false` şeklindedir. Değer ayarlanmışsa `true`, Azure AD B2C kimlik sağlayıcısı tarafından gönderilen bir onayları imzalanmalıdır ve **SamlAssertionDecryption** şifreleme anahtarı belirtilmesi gerekiyor. Değer ayarlanmışsa `true`, Azure AD B2C teknik profil meta verileri içeren **şifreleme** bölümü. Kimlik sağlayıcısı meta verileri okur ve SAML yanıtını onayı Azure AD B2C teknik profil meta verilerinde sağlanan ortak anahtarla şifreler. Onaylamalar şifrelemeyi etkinleştirirseniz, ayrıca yanıt imza doğrulaması devre dışı bırakmanız gerekebilir (daha fazla bilgi için **ResponsesSigned**). | 
| IdpInitiatedProfileEnabled | Hayır |Bir çoklu oturum açma oturumu profili bir SAML kimlik sağlayıcısı profili tarafından başlatılan etkinleştirilip etkinleştirilmediğini gösterir. Olası değerler: `true` veya `false`. Varsayılan değer: `false`. Flow'da kimlik sağlayıcısı tarafından başlatılan harici olarak kullanıcının kimliği doğrulanır ve ardından belirteci kullanır, düzenleme adımlarının yürütür ve ardından bağlı olan taraf uygulaması için bir yanıt gönderir. Azure AD B2C, istenmeyen bir yanıtı gönderilir. |

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













