---
title: Özel ilkeler - Azure Active Directory B2C için Geliştirici Notları | Microsoft Docs
description: Yapılandırma ve Azure AD B2C ile özel ilkelerinin korunması geliştiriciler için notlar.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 1d0be4ec2ed8feb308839377e0494ef2f4b78368
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66510206"
---
# <a name="developer-notes-for-custom-policies-in-azure-active-directory-b2c"></a>Özel ilkeleri Azure Active Directory B2C için Geliştirici Notları

Özel ilke yapılandırmasının Azure Active Directory B2C'de genel kullanıma sunulmuştur. Yapılandırma, bu yöntem, Gelişmiş kimlik geliştiricileri karmaşık kimlik çözümleri oluşturma yöneliktir. Özel ilkeleri kimlik deneyimi çerçevesi gücünü Azure AD B2C kiracıları içinde kullanılabilir hale getirir. Gelişmiş kimlik geliştiriciler özel ilkeleri kullanarak Tamamlanıyor kılavuzlarına ve başvuru belgeleri okuma biraz zaman yatırım planlamanız gerekir.

Özel ilke seçenekleri çoğunu artık genel kullanıma açık olsa da, teknik profil türleri ve içerik tanımı yazılım yaşam döngüsünün farklı aşamalarında API'lerden gibi temel özellikleri vardır. Çok daha fazlası geliyor. Aşağıdaki tabloda daha ayrıntılı bir düzeyde kullanılabilirlik düzeyini belirtir.  

## <a name="features-that-are-generally-available"></a>Genel kullanıma sunulan özellikler

- Özel ilkeler kullanarak yazar ve karşıya yükleme özel kimlik doğrulama kullanıcı yolculuklarından.  
    - Kullanıcı yolculuklarından değişimleri adım adım talep sağlayıcıları arasında açıklanmaktadır.  
    - Koşullu dallanmayı içinde kullanıcı yolculuklarından tanımlayın.  
- Kendi özel kimlik doğrulama kullanıcı yolculuklarından Hizmetleri REST API özellikli birlikte.  
- Openıdconnect protokolünü ile uyumlu olan kimlik sağlayıcıları ile federasyona eklemek.  
- SAML 2.0 protokolü kullanan kimlik sağlayıcıları ile federasyona eklemek.   

## <a name="responsibilities-of-custom-policy-feature-set-developers"></a>Özel ilke özellik kümesini geliştiricilerin sorumlulukları

El ile ilke yapılandırması, temel alınan platformu Azure AD B2C'yi ve benzersiz bir güven framework oluşturulmasını sonuçlarında alt düzey erişim verir. Özel kimlik sağlayıcıları, güven ilişkilerini birçok olası permütasyon ile dış hizmetler ve adım adım iş akışı tümleştirmeleri tasarım ve yapılandırma sistemli bir yaklaşım gerektirir. 

Özel ilke özellik kümesi kullanan geliştiriciler, aşağıdaki yönergelere uymanız gerekir:

- Özel ilkeler ve anahtar/gizli yönetim yapılandırma dili ile Windows'un öğrenin. Daha fazla bilgi için [TrustFrameworkPolicy](trustframeworkpolicy.md). 
- Senaryolar ve özel tümleştirmeler sahipliğini al. Çalışmanızı belge ve canlı site kuruluşunuz hakkında bilgilendirin.  
- Sistemli senaryo testi gerçekleştirin. 
- Yazılım geliştirme ve en az bir geliştirme ve test ortamının en iyi yöntemler ve bir üretim ortamı hazırlama izleyin. 
- Kimlik sağlayıcıları ve Hizmetleri ile tümleştirme yeni gelişmeler hakkında bilgi sahibi olmak. Örneğin, gizli dizileri ve zamanlanmış hem de zamanlanmamış değişikliklerin hizmetine değişiklikleri takip edin. 
- Etkin izleme işlevini ayarlama ve üretim ortamlarında yanıt verme hızını izleyin. Application Insights ile tümleştirme hakkında daha fazla bilgi için bkz. [Azure Active Directory B2C: Günlükleri toplama](active-directory-b2c-custom-guide-eventlogger-appins.md). 
- İlgili kişi e-posta adreslerini Azure aboneliğinde güncel kalmasını sağlayın ve Microsoft Canlı site takım e-postaları yanıt vermeye devam edebilir. 
- Microsoft Canlı site ekibi tarafından Bunu yapmak için önerilir, zamanında gerekeni yapın.

## <a name="terms-for-features-in-public-preview"></a>Genel Önizleme özelliklerine yönelik terimleri

- Genel Önizleme özellikleri yalnızca değerlendirme amacıyla kullanmanızı öneririz. 
- Hizmet düzeyi sözleşmeleri (SLA'lar) genel Önizleme özellikleri için geçerli değildir.
- Genel Önizleme özellikleri için destek istekleri normal destek kanalları Dosyalanan. 

## <a name="features-by-stage-and-known-issues"></a>Aşama ve bilinen sorunlar göre Özellikler

Özel ilke/kimlik deneyimi çerçevesi özellikleri sürekli ve hızlı geliştirilme aşamasındadır. Aşağıdaki tabloda, özellikleri ve bileşen kullanılabilirliği için kullanılan bir dizindir.

### <a name="identity-providers-tokens-protocols"></a>Kimlik sağlayıcıları, belirteçleri, protokolleri

| Özellik | Geliştirme | Önizleme | GA | Notlar |
|-------- | ----------- | ------- | -- | ----- |
| IDP Openıdconnect |  |  | X | Örneğin, Google +.  |
| IDP OAUTH2 |  |  | X | Örneğin, Facebook.  |
| IDP-OAUTH1 (twitter) |  | X |  | Örneğin, Twitter. |
| IDP-OAUTH1 (twitter ex) |  |  |  | Desteklenmiyor |
| IDP-SAML |  |   | X | Örneğin, Salesforce, ADFS. |
| IDP WSFED | X |  |  |  |
| Bağlı olan taraf OAUTH1 |  |  |  | Desteklenmiyor. |
| Bağlı olan taraf OAUTH2 |  |  | X |  |
| Bağlı olan taraf OIDC |  |  | X |  |
| Bağlı olan taraf SAML | X |  |  |  |
| Bağlı olan taraf WSFED | X |  |  |  |
| REST API ile temel ve sertifika kimlik doğrulaması |  |  | X | Örneğin, Azure Logic Apps. |

### <a name="component-support"></a>Bileşen desteği

| Özellik | Geliştirme | Önizleme | GA | Notlar |
| ------- | ----------- | ------- | -- | ----- |
| Azure Multi Factor Authentication |  |  | X |  |
| Yerel dizin olarak Azure Active Directory |  |  | X |  |
| Azure e-posta alt sistemi için e-posta doğrulama |  |  | X |  |
| Çok dil desteği|  |  | X |  |
| Koşul doğrulamaları |  |  | X | Örneğin, parola karmaşıklığını. |
| Üçüncü taraf e-posta hizmeti sağlayıcıları kullanma | X |  |  |  |

### <a name="content-definition"></a>İçerik tanımı

| Özellik | Geliştirme | Önizleme | GA | Notlar |
| ------- | ----------- | ------- | -- | ----- |
| Hata sayfası, api.error |  |  | X |  |
| IDP Seçimi sayfasında, api.idpselections |  |  | X |  |
| Kaydolma, IDP seçimi api.idpselections.signup |  |  | X |  |
| Parolayı unuttum api.localaccountpasswordreset |  |  | X |  |
| Yerel hesap oturum açma api.localaccountsignin |  |  | X |  |
| Yerel hesap kaydolma, api.localaccountsignup |  |  | X |  |
| MFA sayfası, api.phonefactor |  |  | X |  |
| Otomatik olarak onaylanan sosyal hesap kaydolma, api.selfasserted |  |  | X |  |
| Profil güncelleştirmesi, kendi kendine onaylanan api.selfasserted.profileupdate |  |  | X |  |
| Birleşik kaydolma veya oturum açma sayfasında, "disableSignup" parametresiyle bir api.signuporsignin |  |  | X |  |
| JavaScript / sayfa sözleşme |  | X |  |  |

### <a name="app-ief-integration"></a>Uygulama IEF tümleştirme

| Özellik | Geliştirme | Önizleme | GA | Notlar |
| ------- | ----------- | ------- | -- | ----- |
| Sorgu dizesi parametresi domain_hint |  |  | X | Kullanılabilir talebi olarak için IDP geçirilebilir. |
| Sorgu dizesi parametresi login_hint |  |  | X | Kullanılabilir talebi olarak için IDP geçirilebilir. |
| JSON client_assertion UserJourney Ekle | X |  |  | Kullanımdan kaldırılacaktır. |
| JSON id_token_hint UserJourney Ekle |  | X |  | JSON geçirmek için İleri Git yaklaşım. |
| IDP BELİRTECİ uygulamaya geçirin |  | X |  | Örneğin, Facebook'tan uygulaması. |

### <a name="session-management"></a>Oturum yönetimi

| Özellik | Geliştirme | Önizleme | GA | Notlar |
| ------- | ----------- | ------- | -- | ----- |
| SSO oturum sağlayıcısı |  |  | X |  |
| Dış oturum açma oturumu sağlayıcısı |  |  | X |  |
| SAML SSO oturum sağlayıcısı |  |  | X |  |
| Varsayılan SSO oturum sağlayıcısı |  |  | X |  |

### <a name="security"></a>Güvenlik

| Özellik | Geliştirme | Önizleme | GA | Notlar |
|-------- | ----------- | ------- | -- | ----- |
| İlke anahtarları, el ile karşıya yükleme oluşturun |  |  | X |  |
| İlke anahtarları - RSA/Cert, gizli dizileri |  |  | X |  |
| İlkeyi karşıya yükleme |  |  | X |  |

### <a name="developer-interface"></a>Geliştirici arabirimi

| Özellik | Geliştirme | Önizleme | GA | Notlar |
| ------- | ----------- | ------- | -- | ----- |
| Azure portalı-IEF UX |  |  | X |  |
| Application Insights UserJourney günlükleri |  | X |  | Geliştirme sırasında sorun giderme için kullanılır.  |
| Application Insights olay günlükleri (aracılığıyla düzenleme adımlarının) |  | X |  | Kullanıcı akışları üretimde izlemek için kullanılır. |

## <a name="next-steps"></a>Sonraki adımlar
[Özel ilkeler ve kullanıcı akışları ile farklar hakkında daha fazla bilgi](active-directory-b2c-overview-custom.md).
