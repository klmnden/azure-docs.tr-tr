---
title: Azure Active Directory B2C'de özel ilkeler kullanarak Geliştirici Notları | Microsoft Docs
description: Yapılandırma ve Azure AD B2C ile özel ilkelerinin korunması geliştiriciler için notlar.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 10/13/2017
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 0a5255974c7399f9307d8b06a58f4f977be89829
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55172893"
---
# <a name="release-notes-for-azure-active-directory-b2c-custom-policy-public-preview"></a>Azure Active Directory B2C özel ilke genel önizlemesi için sürüm notları
Özel ilke özellik kümesi değerlendirme altındaki tüm Azure Active Directory B2C genel Önizleme için kullanıma sunulmuştur (Azure AD B2C) müşteriler. Bu özellik kümesi, Gelişmiş kimlik geliştiricileri en karmaşık kimlik çözümleri oluşturmaya yöneliktir.  

Günümüzde, bu özellik kümesi yapılandırma XML dosyasını düzenleyerek aracılığıyla doğrudan kimlik deneyimi çerçevesi, geliştiricilerin gerektirir. Bu yapılandırma, güçlü ve karmaşık yöntemidir. Gelişmiş kimlik kimlik deneyimi çerçevesi kullanan geliştiriciler Tamamlanıyor kılavuzlarına ve başvuru belgeleri okuma biraz zaman yatırım planlamanız gerekir. 

## <a name="features-included-in-this-public-preview"></a>Bu genel Önizleme aşamasında bulunan özellikleri
Genel önizlemede kullanıma sunulan yeni özelliklerle geliştiriciler aşağıdaki görevleri gerçekleştirebilirsiniz:<br>

* Yazar ve özel kimlik doğrulama kullanıcı yolculuklarından özel ilkeler kullanarak yükleyin. 
   * Kullanıcı yolculuklarından değişimleri adım adım talep sağlayıcıları arasında açıklanmaktadır. 
   * Koşullu dallanmayı içinde kullanıcı yolculuklarından tanımlayın. * Kendi özel kimlik doğrulama kullanıcı yolculuklarından Hizmetleri REST API özellikli tümleştirin.  
* Standart Openıdconnect ile uyumlu olan kimlik sağlayıcıları ile Federasyon ekleyin. <br>
* Federasyon için SAML 2.0 protokolünü kullanan kimlik sağlayıcıları ile ekleyin. 

## <a name="terms-of-the-public-preview"></a>Genel Önizleme koşulları

* Yalnızca değerlendirme amacıyla yeni özellikleri kullanmak öneririz.<br>
* Yeni özellikler, bir üretim ortamında kullanılması amaçlanmamıştır.<br>
* Hizmet düzeyi sözleşmeleri (SLA'lar) yeni özellikleri için geçerli değildir. <br>
* Destek istekleri normal destek kanalları Dosyalanan. <br>
* Genel kullanılabilirlik taahhüt tarih yoktur.<br>
* Takdirimize ve herhangi bir nedenle, Microsoft bayrak ve reddedecek veya senaryoları ve Azure AD B2C ürün verdiğimiz bir müşteri kimliği ve erişim yönetimi (CIAM) platformu olarak görev yapacak kapsamını aşan kullanıcı yolculuklarından kısıtlama.

## <a name="responsibilities-of-custom-policy-feature-set-developers"></a>Özel ilke özellik kümesini geliştiricilerin sorumlulukları
El ile ilke yapılandırması, temel alınan platformu Azure AD B2C'in alt düzey erişim verir ve benzersiz, tamamen özelleştirilebilir güven framework'ün oluşturulmasında sonuçlanır. Özel kimlik sağlayıcıları, güven ilişkilerini olası permütasyon ile dış hizmetler ve adım adım iş akışı tümleştirmeleri büyük taleplerini bunları kullanan gelişmiş geliştiriciler yerleştirin.

Genel Önizleme tam olarak yararlanmak için özel ilke özellik kümesi kullanan geliştiriciler'ın aşağıdaki yönergelere uyunuz öneririz:
* Kimlik deneyimi çerçevesi ve anahtar/gizli yönetimi yapılandırma dili ile Windows'un öğrenin.
* Senaryolar ve özel tümleştirmeler sahipliğini al.
* Sistemli senaryo testi gerçekleştirin.
* Yazılım geliştirme ve en az bir geliştirme ve test ortamının en iyi yöntemler ve bir üretim ortamı hazırlama izleyin.
* Kimlik sağlayıcıları ve Hizmetleri ile tümleştirme yeni gelişmeler hakkında bilgi sahibi olmak. Örneğin, gizli dizileri ve zamanlanmış hem de zamanlanmamış değişikliklerin hizmetine değişiklikleri takip edin.
* Etkin izleme işlevini ayarlama ve üretim ortamlarında yanıt verme hızını izleyin.
* İlgili kişi e-posta adreslerini Azure aboneliğinde güncel kalmasını sağlayın ve Microsoft Canlı site takım e-postaları yanıt vermeye devam edebilir.
* Microsoft Canlı site ekibi tarafından Bunu yapmak için önerilir, zamanında gerekeni yapın. 

## <a name="features-by-stage-and-known-issues"></a>Aşama ve bilinen sorunlar göre Özellikler
Özel ilke/kimlik deneyimi çerçevesi yetenekler, sabit ve hızlı geliştirilme aşamasındadır.  Bu tablo, Özellikler/bileşen kullanılabilirliği dizinidir.

Sorularınızı Stack Overflow'da içinde [https://aka.ms/aadb2cso](https://aka.ms/aadb2cso)


### <a name="identity-providers-tokens-protocols"></a>Kimlik sağlayıcıları, belirteçleri, protokolleri
Dış bileşenler ve uygulamalar ile arabirimi

| Özellik | Geliştirme | Önizleme | GA | Notlar |
|---------------------------------------------|-------------|---------|----|-------|
| IDP Openıdconnect |  | x |  | Örneğin, Google + |
| IDP OAUTH2 |  | x |  | Örneğin, Facebook  |
| IDP OAUTH1 |  | x |  | Örneğin, Twitter |
| IDP-SAML |  | x |  | Örneğin, Salesforce, ADFS |
| IDP WSFED | x |  |  |  |
| Bağlı olan taraf OAUTH |  | x |  |  |
| Bağlı olan taraf OIDC |  | x |  |  |
| Bağlı olan taraf SAML | x |  |  |  |
| Bağlı olan taraf WSFED | x |  |  |  |
| Temel ve fs'nizi kimlik doğrulama ile REST API |  | x |  | Örneğin, Azure işlevleri |


### <a name="component-support"></a>Bileşen desteği


| Özellik | Geliştirme | Önizleme | GA | Notlar |
|-------------------------------------------|-------------|---------|----|-------|
| Azure Multi Factor Authentication |  | x |  |  |
| Yerel dizin olarak Azure Active Directory |  | x |  |  |
| 2fa'yı Azure e-posta alt sistem |  | x |  |  |
| Çok dil desteği|  | x |  |  |
| Parola karmaşıklığı | x |  |  |  |


### <a name="content-definition"></a>İçerik tanımı

| Özellik | Geliştirme | Önizleme | GA | Notlar |
|-----------------------------------------------------------------------------|-------------|---------|----|-------|
|   Hata sayfası, api.error |  | x |  |  |
|   IDP Seçimi sayfasında, api.idpselections |  | x |  |  |
|   Kaydolma, IDP seçimi api.idpselections.signup |  | x |  |  |
|   Parolayı unuttum api.localaccountpasswordreset |  | x |  |  |
|   Yerel hesap oturum açma api.localaccountsignin |  | x |  |  |
|   Yerel hesap kaydolma, api.localaccountsignup |  | x |  |  |
|   MFA sayfası, api.phonefactor |  | x |  |  |
|   Kendi kendine onaylanan-örneğin sosyal hesap kaydolma, api.selfasserted |  | x |  |  |
|   Profil güncelleştirmesi, kendi kendine onaylanan api.selfasserted.profileupdate |  | x |  |  |
|   Birleşik kaydolma veya oturum açma sayfası, api.signuporsignin |  | x |  |  |


### <a name="app-ief-integration"></a>Uygulama IEF tümleştirme
| Özellik | Geliştirme | Önizleme | GA | Notlar |
|--------------------------------------------------|-------------|---------|----|-------------------------------------------------|
| Sorgu dizesi parametresi id_token_hint | x |  |  |  |
| Sorgu dizesi parametresi domain_hint |  | x |  | Talep, IDP için geçirilen kullanılabilir |
| Sorgu dizesi parametresi login_hint |  | x |  | Talep, IDP için geçirilen kullanılabilir |
| JSON client_assertion UserJourney Ekle | x |  |  | kullanım dışı bırakılacak |
| JSON id_token_hint UserJourney Ekle | x |  |  | JSON geçirilecek yaklaşım İleri Git |


### <a name="session-management"></a>Oturum yönetimi

| Özellik | Geliştirme | Önizleme | GA | Notlar |
|---------------------------------|-------------|---------|----|-------|
| SSO oturum sağlayıcısı |  | x |  |  |
| Dış oturum açma oturumu sağlayıcısı |  | x |  |  |
| SAML SSO oturum sağlayıcısı |  | x |  |  |


### <a name="security"></a>Güvenlik
| Özellik | Geliştirme | Önizleme | GA | Notlar |
|---------------------------------------------|-------------|---------|----|-------|
| İlke anahtarları, el ile karşıya yükleme oluşturun |  | x |  |  |
| İlke anahtarları - RSA/Cert, gizli dizileri |  | x |  |  |


### <a name="developer-interface"></a>Geliştirici arabirimi
| Özellik | Geliştirme | Önizleme | GA | Notlar |
|---------------------------------------------|-------------|---------|----|-------|
| Azure portalı-IEF UX |  | x |  |  |
| Application Insights UserJourney günlükleri  |  | x |  |  |
| Application Insights olay günlükleri |x|  |  |  |



## <a name="next-steps"></a>Sonraki adımlar
[Özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md).
