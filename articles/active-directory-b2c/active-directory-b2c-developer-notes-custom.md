---
title: Azure Active Directory B2C'de özel ilkelerini kullanma Geliştirici Notları | Microsoft Docs
description: Yapılandırma ve Azure AD B2C ile özel ilkeler koruma geliştiriciler için notları.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 10/13/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: ce323972dcdbf673311b407f427bc452fbe6dc3a
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34709735"
---
# <a name="release-notes-for-azure-active-directory-b2c-custom-policy-public-preview"></a>Azure Active Directory B2C özel ilke genel Önizleme için sürüm notları
Özel ilke özellik kümesi artık genel Önizleme için tüm Azure Active Directory B2C altında değerlendirme için kullanılabilir (Azure AD B2C) müşteriler. Bu özellik kümesi, Gelişmiş kimlik geliştiricileri en karmaşık kimlik çözümleri oluşturma yöneliktir.  

Günümüzde, bu özellik kümesi XML dosyasını düzenleyerek aracılığıyla doğrudan kimlik deneyimi Framework yapılandırmak geliştiricilere gerektirir. Bu yapılandırma, güçlü ve karmaşık yöntemidir. Gelişmiş kimlik kimlik deneyimi Framework kullanan geliştiriciler kılavuzlarına tamamladıktan ve başvuru belgeleri okuma biraz zaman yatırım planlamanız gerekir. 

## <a name="features-included-in-this-public-preview"></a>Bu genel önizlemede bulunan özellikler
Genel önizlemede sunulan yeni özelliklerle geliştiriciler aşağıdaki görevleri gerçekleştirebilirsiniz:<br>

* Özel ilkeler kullanarak yazar ve karşıya yükleme özel kimlik doğrulama kullanıcı Yolculuklar. 
   * Kullanıcı Yolculuklar alışverişleri adım adım talep sağlayıcıları arasında açıklanmaktadır. 
   * Koşullu kullanıcı Yolculuklar dallanma tanımlayın. 
* Özel kimlik doğrulama kullanıcı Yolculuklar REST API etkin Hizmetleri'nde tümleştirin.  
* Standart Openıdconnect ile uyumlu olan kimlik sağlayıcıları ile Federasyon ekleyin. <br>
* SAML 2.0 protokolünü kullanan kimlik sağlayıcıları ile Federasyon ekleyin. 

## <a name="terms-of-the-public-preview"></a>Genel Önizleme koşulları

* Yalnızca değerlendirme amacıyla yeni özellikleri kullanmak için önerilir.<br>
* Yeni özellikler, bir üretim ortamında kullanılması amaçlanmamıştır.<br>
* Hizmet düzeyi sözleşmelerine (SLA) için yeni özellikleri geçerli değildir. <br>
* Destek istekleri normal destek kanallarını Dosyalanan. <br>
* Genel kullanılabilirlik taahhüt edilen tarih yoktur.<br>
* Bizim tedbirli ve herhangi bir nedenle, Microsoft bayrak ve reddetme veya senaryoları ve bir müşteri kimlik ve erişim yönetimi (CIAM) platformu olarak hizmet vermek için Azure AD B2C ürün kurucu kapsamını aşan kullanıcı Yolculuklar kısıtlama.

## <a name="responsibilities-of-custom-policy-feature-set-developers"></a>Özel ilke özellik kümesi geliştiricilerin sorumlulukları
El ile ilke yapılandırması, Azure AD B2C temel platform düşük düzeyli erişim verir ve benzersiz, tam olarak özelleştirilebilir güven framework oluşturulmasında sonuçlanır. Olası alternatifler özel kimlik sağlayıcıları, güven ilişkileri dış hizmetler ve adım adım iş akışları ile tümleştirmeleri bunları kullanan gelişmiş geliştiriciler üzerinde büyük taleplerini yerleştirin.

Genel Önizlemesi'nden tam olarak yararlanmak için özel İlkesi özellik kümesini kullanan geliştiriciler için aşağıdaki yönergelere uyması öneririz:
* Anahtar/parolalar yönetimi ve kimlik deneyimi Framework yapılandırma dili ile Windows'un öğrenin.
* Senaryolar ve özel tümleştirmeler sahipliğini alın.
* Sistemli senaryo test gerçekleştirin.
* Yazılım geliştirme ve en iyi yöntemlerle en az bir geliştirme ve test ortamı ve bir üretim ortamında hazırlama izleyin.
* Kimlik sağlayıcılar ve Hizmetleri ile tümleştirme yeni gelişmeler hakkında bilgi sahibi olma. Örneğin, değişiklikleri gizli ve hizmet zamanlanmış ve zamanlanmamış değişiklikleri takip edin.
* Etkin izleme işlevini ayarlama ve üretim ortamlarını yanıtlama izleyin.
* İletişim e-posta adresleri Azure aboneliğindeki güncel tutun ve Microsoft live site takım e-postalar için yanıt verebilir durumda kalır.
* Microsoft live site ekibi tarafından bunu tavsiye zaman zamanında eylemi gerçekleştirin. 

## <a name="features-by-stage-and-known-issues"></a>Özellikler tarafından aşama ve bilinen sorunlar
Özel ilke/kimlik deneyimi Framework yeteneklerini sabit ve hızlı geliştirilme aşamasındadır.  Bu tablo, Özellikler/bileşen kullanılabilirliği dizinidir.

Yığın taşması içinde sorularınızı [https://aka.ms/aadb2cso](https://aka.ms/aadb2cso)


### <a name="identity-providers-tokens-protocols"></a>Kimlik sağlayıcılar, belirteçleri, protokolleri
Dış bileşenler ve uygulamaları olan arabirimler

| Özellik | Geliştirme | Önizleme | GA | Notlar |
|---------------------------------------------|-------------|---------|----|-------|
| IDP-Openıdconnect |  | x |  | Örneğin, Google + |
| IDP-OAUTH2 |  | x |  | Örneğin, Facebook  |
| IDP-OAUTH1 |  | x |  | Örneğin, Twitter |
| IDP-SAML |  | x |  | Örneğin, Salesforce, ADFS |
| IDP-WSFED | x |  |  |  |
| Bağlı olan taraf OAUTH |  | x |  |  |
| Bağlı olan taraf OIDC |  | x |  |  |
| Bağlı olan taraf SAML | x |  |  |  |
| Bağlı olan taraf WSFED | x |  |  |  |
| REST API'si temel ve sertifikası kimlik doğrulaması ile |  | x |  | Örneğin, Azure işlevleri |


### <a name="component-support"></a>Bileşen desteği


| Özellik | Geliştirme | Önizleme | GA | Notlar |
|-------------------------------------------|-------------|---------|----|-------|
| Azure Multi Factor Authentication |  | x |  |  |
| Yerel dizin olarak Azure Active Directory |  | x |  |  |
| 2FA için Azure e-posta alt sistem |  | x |  |  |
| Birden çok dil desteği|  | x |  |  |
| Parola karmaşıklığı | x |  |  |  |


### <a name="content-definition"></a>İçerik tanımı

| Özellik | Geliştirme | Önizleme | GA | Notlar |
|-----------------------------------------------------------------------------|-------------|---------|----|-------|
|   Hata sayfası, api.error |  | x |  |  |
|   IDP Seçimi sayfasında, api.idpselections |  | x |  |  |
|   Kaydolma için IDP seçimi api.idpselections.signup |  | x |  |  |
|   Parolanızı mı unuttunuz api.localaccountpasswordreset |  | x |  |  |
|   Yerel hesap oturum açma api.localaccountsignin |  | x |  |  |
|   Yerel hesap kaydolma, api.localaccountsignup |  | x |  |  |
|   MFA sayfası, api.phonefactor |  | x |  |  |
|   Kendi kendine uygulanan-örneğin sosyal hesap SIG li api.selfasserted |  | x |  |  |
|   Profil güncelleştirme otomatik olarak uygulanan api.selfasserted.profileupdate |  | x |  |  |
|   Birleşik kaydolun veya oturum açma sayfası, api.signuporsignin |  | x |  |  |


### <a name="app-ief-integration"></a>Uygulama IEF tümleştirme
| Özellik | Geliştirme | Önizleme | GA | Notlar |
|--------------------------------------------------|-------------|---------|----|-------------------------------------------------|
| Sorgu dizesi parametresi id_token_hint | x |  |  |  |
| Sorgu dizesi parametresi domain_hint |  | x |  | kullanılabilir talebi olarak için IDP geçirilebilir |
| Sorgu dizesi parametresi login_hint |  | x |  | kullanılabilir talebi olarak için IDP geçirilebilir |
| JSON client_assertion UserJourney yerleştirin | x |  |  | kullanım dışı kalacaktır |
| JSON id_token_hint UserJourney yerleştirin | x |  |  | JSON iletmek için Git İleri yaklaşımı |


### <a name="session-management"></a>Oturum yönetimi

| Özellik | Geliştirme | Önizleme | GA | Notlar |
|---------------------------------|-------------|---------|----|-------|
| SSO oturum sağlayıcısı |  | x |  |  |
| Dış oturum açma oturumu sağlayıcısı |  | x |  |  |
| SAML SSO oturum sağlayıcısı |  | x |  |  |


### <a name="security"></a>Güvenlik
| Özellik | Geliştirme | Önizleme | GA | Notlar |
|---------------------------------------------|-------------|---------|----|-------|
| İlke anahtarları, el ile karşıya yükleme oluşturma |  | x |  |  |
| İlke anahtarları - RSA/Cert, gizli |  | x |  |  |


### <a name="developer-interface"></a>Geliştirici arabirimi
| Özellik | Geliştirme | Önizleme | GA | Notlar |
|---------------------------------------------|-------------|---------|----|-------|
| Azure Portal-IEF UX |  | x |  |  |
| Uygulama Öngörüler UserJourney günlükleri  |  | x |  |  |
| Uygulama Öngörüler olay günlükleri |x|  |  |  |



## <a name="next-steps"></a>Sonraki adımlar
[Özel ilkelerini kullanmaya başlama](active-directory-b2c-get-started-custom.md).
