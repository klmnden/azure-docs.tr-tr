---
title: ContentDefinitions - Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C'de özel bir ilke ContentDefinitions öğesi belirtin.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: e36eb3816d6f465552c4db740508d5e7f5fa1331
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60313327"
---
# <a name="contentdefinitions"></a>ContentDefinitions

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Görünümü herhangi özelleştirebilirsiniz [teknik profil otomatik olarak onaylanan](self-asserted-technical-profile.md). Azure Active Directory (Azure AD) B2C kod müşterinizin tarayıcıda çalışan ve çıkış noktaları arası kaynak paylaşımı (CORS) olarak adlandırılan modern bir yaklaşımı kullanır. 

Kullanıcı arabirimini özelleştirmek için bir URL belirtin. **ContentDefinition** öğesi özelleştirilmiş HTML içeriğe sahip. Otomatik olarak onaylanan teknik profilde veya **OrchestrationStep**, söz konusu içerik tanımı tanımlayıcısı için noktası. İçerik tanımı içerebilir bir **LocalizedResourcesReferences** listesini belirtir öğesi yerelleştirilmiş kaynaklar yüklenemedi. Azure AD B2C kullanıcı arabirimi öğeleri, URL'den yüklenir ve ardından sayfanın kullanıcıya görüntüleyen bir HTML içeriği ile birleştirir.

**ContentDefinitions** öğesi, bir kullanıcı yolculuğunda kullanılabilir HTML5 şablonları için URL'leri içeriyor. HTML5 sayfa URI'si, belirtilen kullanıcı arabirimi adım için kullanılır. Örneğin, oturum açma veya kaydolma, parola sıfırlama veya hata sayfaları. LoadUri HTML5 dosyası için geçersiz kılarak görünümünü değiştirebilirsiniz. Yeni içerik tanımlarını ihtiyaçlarınıza göre oluşturabilirsiniz. Bu öğe, belirtilen yerelleştirme tanımlayıcısı için yerelleştirilmiş kaynaklar başvuru içerebilir [yerelleştirme](localization.md) öğesi.

Aşağıdaki örnek, içerik tanımı tanımlayıcısı ve yerelleştirilmiş kaynaklar tanımını gösterir:

```XML
<ContentDefinition Id="api.localaccountsignup">
  <LoadUri>~/tenant/default/selfAsserted.cshtml</LoadUri>
  <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
  <DataUri>urn:com:microsoft:aad:b2c:elements:selfasserted:1.1.0</DataUri>
  <Metadata>
    <Item Key="DisplayName">Local account sign up page</Item>
  </Metadata>
  <LoalizedResourcesReferences MergeBehavior="Prepend">
    <LocalizedResourcesReference Language="en" LocalizedResourcesReferenceId="api.localaccountsignup.en" />
    <LocalizedResourcesReference Language="es" LocalizedResourcesReferenceId="api.localaccountsignup.es" />
    ...
```

Meta verileri **LocalAccountSignUpWithLogonEmail** teknik profili, içerik tanımı tanımlayıcısı içeren kendi kendine onaylanan **ContentDefinitionReferenceId** ayarlayın `api.localaccountsignup`

```XML
<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
  <DisplayName>Email signup</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ContentDefinitionReferenceId">api.localaccountsignup</Item>
    ...
  </Metadata>
  ...
```


## <a name="contentdefinition"></a>ContentDefinition

**ContentDefinition** öğesi aşağıdaki öznitelik içeriyor:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kimlik | Evet | İçerik tanımı için bir tanımlayıcı. Belirtilen bir değerdir **tanım kimlikleri içerik** bölümde daha sonra bu sayfayı. |

**ContentDefinition** öğesi aşağıdaki öğeleri içerir:

| Öğe | Oluşumlar | Açıklama |
| ------- | ----------- | ----------- |
| LoadUri | 1:1 | İçerik tanımı için HTML5 sayfasının URL'sini içeren bir dize. |
| RecoveryUri | 0:1 | İçerik tanımı için ilgili bir hata görüntülemek için HTML sayfasının URL'sini içeren bir dize. | 
| DataUri | 1:1 | Çağırmak için kullanıcı deneyimi sağlayan adım için bir HTML dosyasının göreli URL içeren bir dize. |  
| Meta Veriler | 1:1 | İçerik tanımı tarafından kullanılan meta veriler içeren anahtar/değer çiftleri koleksiyonu. | 
| LocalizedResourcesReferences | 0:1 | Yerelleştirilmiş kaynaklar başvuruları koleksiyonu. Bir kullanıcı arabirimi ve talepleri özniteliğini yerelleştirme özelleştirmek için bu öğeyi kullanırsınız. |

### <a name="datauri"></a>DataUri

**DataUri** öğesi sayfa tanımlayıcısını belirtmek için kullanılır. Azure AD B2C'yi yüklemek ve kullanıcı Arabirimi öğeleri ve istemci tarafı JavaScript başlatmak için sayfa tanımlayıcısını kullanır. Değerinin biçimi `urn:com:microsoft:aad:b2c:elements:page-name:version`.  Aşağıdaki tabloda kullanabileceğiniz sayfası tanımlayıcıları.

| Değer |   Açıklama |
| ----- | ----------- |
| `urn:com:microsoft:aad:b2c:elements:globalexception:1.1.0` | Bir özel durum ya da bir hata oluştuğunda bir hata sayfası görüntüler. |
| `urn:com:microsoft:aad:b2c:elements:idpselection:1.0.0` | Oturum açma sırasında kullanıcıların seçebileceği kimlik sağlayıcılarını listeler. | 
| `urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0` | Bir e-posta adresi veya kullanıcı adına göre yerel bir hesap ile oturum imzalamak için bir form görüntüler. Bu değer ayrıca sağlar "bana oturum açma işlevini tut" ve "parolanızı mı unuttunuz?" bağlantı. | 
| `urn:com:microsoft:aad:b2c:elements:unifiedssd:1.0.0` | Bir e-posta adresi veya kullanıcı adına göre yerel bir hesap ile oturum imzalamak için bir form görüntüler. |
| `urn:com:microsoft:aad:b2c:elements:multifactor:1.1.0` | Telefon numaraları, metin ya da ses kaydolma veya oturum açma sırasında kullanarak doğrular. |
| `urn:com:microsoft:aad:b2c:elements:selfasserted:1.1.0` | Kullanıcıların kendi profili güncelle olanak sağlayan bir form görüntüler. | 


### <a name="localizedresourcesreferences"></a>LocalizedResourcesReferences

**LocalizedResourcesReferences** öğesi aşağıdaki öğeleri içerir:

| Öğe | Oluşumlar | Açıklama |
| ------- | ----------- | ----------- |
| LocalizedResourcesReference | 1:n | İçerik tanımı için yerelleştirilmiş kaynak başvuruları listesi. | 

**LocalizedResourcesReferences** öğesi aşağıdaki öznitelikler içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Dil | Evet | RFC 5646 - her ilke için desteklenen bir dil içeren bir dize tanımlayan diller için etiketleri. |
| LocalizedResourcesReferenceId | Evet | Tanımlayıcısını **LocalizedResources** öğesi. |

Aşağıdaki örnek, bir kaydolma veya oturum açma içerik tanımı gösterilmektedir:

```XML
<ContentDefinition Id="api.signuporsignin">
  <LoadUri>~/tenant/default/unified.cshtml</LoadUri>
  <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
  <DataUri>urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0</DataUri>
  <Metadata>
    <Item Key="DisplayName">Signin and Signup</Item>
  </Metadata>
</ContentDefinition>
```

Aşağıdaki örnekte, İngilizce, Fransızca ve İspanyolca için yerelleştirme başvurusuyla kaydolma veya oturum açma içerik tanımı gösterilmektedir:

```XML
<ContentDefinition Id="api.signuporsignin">
  <LoadUri>~/tenant/default/unified.cshtml</LoadUri>
  <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
  <DataUri>urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0</DataUri>
  <Metadata>
    <Item Key="DisplayName">Signin and Signup</Item>
  </Metadata>
  <LocalizedResourcesReferences MergeBehavior="Prepend">
    <LocalizedResourcesReference Language="en" LocalizedResourcesReferenceId="api.signuporsignin.en" />
    <LocalizedResourcesReference Language="fr" LocalizedResourcesReferenceId="api.signuporsignin.rf" />
    <LocalizedResourcesReference Language="es" LocalizedResourcesReferenceId="api.signuporsignin.es" />
</LocalizedResourcesReferences>
</ContentDefinition>
```

Yerelleştirme desteğini içerik tanımlarınızı ekleme konusunda bilgi için bkz [yerelleştirme](localization.md).

## <a name="content-definition-ids"></a>İçerik tanımı kimlikleri

ID özniteliği **ContentDefinition** öğesi için içerik tanımı ilişkili sayfa türü belirtir. Öğe, HTML5/CSS özel bir şablon uygulamak için gittiği bağlamı tanımlar. Aşağıdaki tabloda kimlik deneyimi çerçevesi ve bunlara ile ilgili sayfa türleri tarafından tanınan içerik tanımı kimliklerini açıklar. Rastgele bir kimliğe sahip kendi içerik tanımlarını oluşturabilirsiniz

| Kimlik | Varsayılan şablon | Açıklama | 
| -- | ---------------- | ----------- |
| **api.error** | [Exception.cshtml](https://login.microsoftonline.com/static/tenant/default/exception.cshtml) | **Hata sayfası** - görüntüler bir hata sayfası bir özel durum olduğunda veya bir hatayla karşılaştı. |
| **api.idpselections** | [idpSelector.cshtml](https://login.microsoftonline.com/static/tenant/default/idpSelector.cshtml) | **Kimlik sağlayıcısı seçim sayfası** -oturum açma sırasında kullanıcıların seçebileceği kimlik sağlayıcıları listeler. Seçenekler, genellikle Kurumsal kimlik sağlayıcıları, Facebook ve Google + veya yerel hesaplar gibi sosyal kimlik sağlayıcıları şunlardır. |
| **api.idpselections.signup** | [idpSelector.cshtml](https://login.microsoftonline.com/static/tenant/default/idpSelector.cshtml) | **Kimlik sağlayıcısı seçim için kaydolma** -kullanıcıların kayıt sırasında seçebileceği kimlik sağlayıcıları listeler. Seçenekler, genellikle Kurumsal kimlik sağlayıcıları, Facebook ve Google + veya yerel hesaplar gibi sosyal kimlik sağlayıcıları şunlardır. |
| **api.localaccountpasswordreset** | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Parolanızı mı unuttunuz sayfasını** -kullanıcı parola sıfırlama başlatmak için tamamlaması gereken bir form görüntüler. |
| **api.localaccountsignin** | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Yerel hesap oturum açma sayfası** -bir e-posta adresi veya kullanıcı adına göre yerel bir hesap ile oturum imzalamak için bir form görüntüler. Form, metin girişi kutusunu ve parola giriş kutusu içerebilir. |
| **api.localaccountsignup** | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Yerel hesap kaydolma sayfası** -e-posta adresi veya bir kullanıcı adı temel alan bir yerel hesap kaydolma için bir form görüntüler. Form gibi çeşitli giriş denetimleri içerebilen: bir metin girişi kutusunu, parola girişi kutusu, radyo düğmesi, tekli seçim açılır kutuları ve çoklu seçim kutuları işaretleyin. |
| **api.phonefactor** | [multifactor-1.0.0.cshtml](https://login.microsoftonline.com/static/tenant/default/multifactor-1.0.0.cshtml) | **Çok faktörlü kimlik doğrulaması sayfası** -telefon numaraları, metin ya da ses, kaydolma veya oturum açma sırasında kullanarak doğrular. |
| **api.selfasserted** | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Sosyal hesap kaydolma sayfası** -var olan bir hesap bir sosyal kimlik sağlayıcısı ile kaydolduğunda kullanıcıların tamamlamalısınız bir form görüntüler. Bu sayfa, önceki sosyal hesap kaydolma sayfası, parola giriş alanları dışında benzerdir. |
| **api.selfasserted.profileupdate** | [updateprofile.html](https://login.microsoftonline.com/static/tenant/default/updateProfile.cshtml) | **Profili güncelleştirme sayfasını** -kullanıcıların profillerini güncelleştirmek için erişebileceği bir form görüntüler. Bu sayfa, sosyal hesap kaydolma sayfası, parola giriş alanları dışında benzerdir. |
| **api.signuporsignin** | [Unified.HTML](https://login.microsoftonline.com/static/tenant/default/unified.cshtml) | **Birleşik kaydolma veya oturum açma sayfası** -kullanıcı oturum açma ve kaydolma işlemini gerçekleştirir. Kullanıcıların Kurumsal kimlik sağlayıcıları, Facebook veya Google + veya yerel hesaplar gibi sosyal kimlik sağlayıcılarını kullanabilirsiniz. |
 
