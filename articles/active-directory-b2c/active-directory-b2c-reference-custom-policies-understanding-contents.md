---
title: Azure Active Directory B2C'de özel ilkeler başlatıcısının paketi anlama | Microsoft Docs
description: Azure Active Directory B2C özel ilkeler bir konu.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/25/2017
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: d128875611c8f7a6c0b65ef46d8f127e3b79efee
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64703478"
---
# <a name="understanding-the-custom-policies-of-the-azure-ad-b2c-custom-policy-starter-pack"></a>Azure AD B2C özel ilke başlangıç paketi özel ilkelerini anlama

Bu bölümde ile birlikte gelen B2C_1A_base ilkesinin tüm temel öğeleri listelenir **başlangıç paketi** ve kendi ilkelerinizi devralma yoluyla yazmak için yararlanılarak *B2C_1A_base_extensions İlkesi* .

Bu nedenle, bu özellikle fazla odaklanır zaten tanımlanmış talep türleri, talep dönüştürmeleri, içerik tanımları, Talep sağlayıcı, teknik profil ve çekirdek kullanıcı yolculuklarından ile.

> [!IMPORTANT]
> Microsoft hiçbir açık veya zımni bundan sonra sağlanan bilgileri sağlar. Değişiklikleri GA süreden önce herhangi bir zamanda GA zamanında veya sonrasında tanıtılmak.

Kendi ilkelerinizi hem B2C_1A_base_extensions İlkesi, bu tanımları geçersiz kılın ve gerektiğinde bulunmakla sağlayarak bu ana ilke genişletin.

Çekirdek öğelerini *B2C_1A_base ilke* talep türleri, talep dönüşümleri ve içerik tanımlarını. Bu öğeleri kendi ilkelerinizi de olarak başvurulması maruz kullanılabilir *B2C_1A_base_extensions ilke*.

## <a name="claims-schemas"></a>Talep şemaları

Başka bir talep, şemalar, üç bölümlere ayrılmıştır:

1.  Kullanıcı yolculuklarından düzgün çalışması gerekli olan minimum talepleri listeler ilk bölümü.
2.  Talepleri listeler bir ikinci bölümü, özellikle login.microsoftonline.com kimlik doğrulaması için diğer talep sağlayıcıları geçirilecek sorgu dizesi parametreleri ve diğer özel parametreler için gereklidir. **Lütfen bu talepler değiştirmeyin**.
3.  Ve kullanıcıdan toplanan herhangi bir ek, isteğe bağlı talepleri listeler üçüncü bir bölümünü sonunda dizinde saklanan ve oturum açma sırasında belirteçlerinde gönderilen. Bu bölümde yeni talep türü kullanıcıdan toplanan ve/veya belirtecinde gönderilen eklenebilir.

> [!IMPORTANT]
> Talep şeması, parola ve kullanıcı adları gibi belirli bir talep kısıtlamalar içerir. Güven Framework (TF) ilkesi herhangi bir talep sağlayıcısı olarak Azure AD'ye değerlendirir ve tüm kısıtlamalar özel ilke modelled. Daha fazla kısıtlama ekleyin veya başka bir talep sağlayıcı kendi kısıtlamaları olan kimlik bilgilerini depolama için bir ilke değiştirilmiş.

Kullanılabilir talep türleri aşağıda listelenmiştir.

### <a name="claims-that-are-required-for-the-user-journeys"></a>Kullanıcı yolculuklarından için gerekli olan talepleri

Aşağıdaki talep için kullanıcı yolculuklarından düzgün çalışması gereklidir:

| Talep türü | Açıklama |
|-------------|-------------|
| *Kullanıcı Kimliği* | Kullanıcı adı |
| *signInName* | Oturum adı |
| *tenantId* | Azure AD B2C kullanıcı nesnesinin Kiracı tanımlayıcısını (ID) |
| *objectId* | Azure AD B2C kullanıcı nesnesinin nesne tanımlayıcısını (ID) |
| *Parola* | Parola |
| *newPassword* | |
| *reenterPassword* | |
| *passwordPolicies* | Parola gücü, bitiş tarihi belirlemek için Azure AD B2C tarafından kullanılan parola ilkeleri. |
| *alt* | |
| *alternativeSecurityId* | |
| *identityProvider* | |
| *displayName* | |
| *strongAuthenticationPhoneNumber* | Kullanıcının telefon numarası |
| *Verified.strongAuthenticationPhoneNumber* | |
| *E-posta* | Kullanıcıyla iletişim kurmak için kullanılan e-posta adresi |
| *signInNamesInfo.emailAddress* | Kullanıcının oturum açmak için kullanabileceğiniz e-posta adresi |
| *otherMails* | Kullanıcıyla iletişim kurmak için kullanılan e-posta adresleri |
| *userPrincipalName* | Azure AD B2C'de depolanan gibi kullanıcı adı |
| *upnUserName* | Kullanıcı asıl adı oluşturmak için kullanıcı adı |
| *mailNickName* | Azure AD B2C'de depolanan gibi kullanıcının posta takma adı |
| *newUser* | |
| *executed-SelfAsserted-Input* | Öznitelikleri kullanıcıdan toplanan olup olmadığını belirten bir talep |
| *executed-PhoneFactor-Input* | Yeni bir telefon numarası kullanıcıdan toplanan olup olmadığını belirten bir talep |
| *authenticationSource* | Kullanıcının sosyal kimlik sağlayıcısı, login.microsoftonline.com veya yerel hesap doğrulanmış olduğunu belirtir |

### <a name="claims-required-for-query-string-parameters-and-other-special-parameters"></a>Sorgu dizesi parametreleri ve diğer özel parametreler için gerekli talepler

Aşağıdaki talep diğer talep sağlayıcıları için (bazı sorgu dizesi parametreleri dahil) özel parametreleri geçirmek için gereklidir:

| Talep türü | Açıklama |
|-------------|-------------|
| *nux* | Yerel hesap kimlik doğrulaması için login.microsoftonline.com için geçirilen özel parametresi |
| *nca* | Yerel hesap kimlik doğrulaması için login.microsoftonline.com için geçirilen özel parametresi |
| *istemi* | Yerel hesap kimlik doğrulaması için login.microsoftonline.com için geçirilen özel parametresi |
| *mkt* | Yerel hesap kimlik doğrulaması için login.microsoftonline.com için geçirilen özel parametresi |
| *LC* | Yerel hesap kimlik doğrulaması için login.microsoftonline.com için geçirilen özel parametresi |
| *grant_type değeri* | Yerel hesap kimlik doğrulaması için login.microsoftonline.com için geçirilen özel parametresi |
| *Kapsam* | Yerel hesap kimlik doğrulaması için login.microsoftonline.com için geçirilen özel parametresi |
| *client_id* | Yerel hesap kimlik doğrulaması için login.microsoftonline.com için geçirilen özel parametresi |
| *objectIdFromSession* | Nesne Kimliğini bir SSO oturumundan alınan olduğunu göstermek için varsayılan oturum yönetimi sağlayıcısı tarafından sağlanan parametre |
| *isActiveMFASession* | Kullanıcının etkin bir MFA oturuma sahip olduğunu göstermek için mfa'yı oturum yönetimi tarafından sağlanan parametre |

### <a name="additional-optional-claims-that-can-be-collected"></a>Toplanabilir (isteğe bağlı) ek talep

Kullanıcılardan toplanan, dizinde saklanan ve belirtecinde gönderilen ek talep taleplerdir. Ek talep, önce belirtildiği gibi bu listeye eklenebilir.

| Talep türü | Açıklama |
|-------------|-------------|
| *givenName* | Kullanıcının verilen adı (ilk adı olarak da bilinir) |
| *Soyadı* | Kullanıcının soyadı (Aile adı veya son adı olarak da bilinir) |
| *Extension_picture* | Sosyal kullanıcının resmi |

## <a name="claim-transformations"></a>Talep dönüştürmeleri

Kullanılabilir talep dönüştürme işlemlerini aşağıda listelenmiştir.

| Talep dönüştürme | Açıklama |
|----------------------|-------------|
| *CreateOtherMailsFromEmail* | |
| *CreateRandomUPNUserName* | |
| *CreateUserPrincipalName* | |
| *CreateSubjectClaimFromObjectID* | |
| *CreateSubjectClaimFromAlternativeSecurityId* | |
| *CreateAlternativeSecurityId* | |

## <a name="content-definitions"></a>İçerik tanımları

Bu bölümde zaten bildirilmiş içerik tanımlarını açıklanmaktadır *B2C_1A_base* ilkesi. Bu içerik tanımlarını başvurulan, geçersiz kılınan ve/veya kendi ilkelerinizi de olarak gerektiği şekilde genişletilmiş için açıktır *B2C_1A_base_extensions* ilkesi.

| Talep sağlayıcı | Açıklama |
|-----------------|-------------|
| *Facebook* | |
| *Yerel hesapla oturum aç* | |
| *PhoneFactor* | |
| *Azure Active Directory* | |
| *Onaylanan kendi kendine* | |
| *Yerel hesap* | |
| *Oturum yönetimi* | |
| *Trustframework ilkesini altyapısı* | |
| *TechnicalProfiles* | |
| *Belirteci veren* | |

## <a name="technical-profiles"></a>Teknik profiller

Bu bölümde Talep sağlayıcı başına zaten bildirildi teknik profiller gösterilmektedir *B2C_1A_base* ilkesi. Bu teknik profiller daha fazla başvurulan, geçersiz kılınan ve/veya kendi ilkelerinizi de olarak gerektiği şekilde genişletilmiş için açıktır *B2C_1A_base_extensions* ilkesi.

### <a name="technical-profiles-for-facebook"></a>Facebook için teknik profiller

| Teknik profili | Açıklama |
|-------------------|-------------|
| *Facebook OAUTH* | |

### <a name="technical-profiles-for-local-account-signin"></a>Teknik profilleri için yerel hesapla oturum aç

| Teknik profili | Açıklama |
|-------------------|-------------|
| *Etkileşimli olmayan oturum açma* | |

### <a name="technical-profiles-for-phone-factor"></a>Phonefactor raporlarına için teknik profiller

| Teknik profili | Açıklama |
|-------------------|-------------|
| *PhoneFactor giriş* | |
| *PhoneFactor-InputOrVerify* | |
| *PhoneFactor doğrulayın* | |

### <a name="technical-profiles-for-azure-active-directory"></a>Azure Active Directory için teknik profiller

| Teknik profili | Açıklama |
|-------------------|-------------|
| *AAD-genel* | Diğer AAD-xxx teknik profiller tarafından bulunan teknik profili |
| *AAD-UserWriteUsingAlternativeSecurityId* | Sosyal oturum açma bilgileri için teknik profili |
| *AAD-UserReadUsingAlternativeSecurityId* | Sosyal oturum açma bilgileri için teknik profili |
| *AAD UserReadUsingAlternativeSecurityId NoError* | Sosyal oturum açma bilgileri için teknik profili |
| *AAD UserWritePasswordUsingLogonEmail* | Yerel hesaplar için teknik profili |
| *AAD UserReadUsingEmailAddress* | Yerel hesaplar için teknik profili |
| *AAD-UserWriteProfileUsingObjectId* | ObjectID kullanarak kullanıcı kaydı güncelleştirmek için teknik profili |
| *AAD-UserWritePhoneNumberUsingObjectId* | ObjectID kullanarak kullanıcı kaydı güncelleştirmek için teknik profili |
| *AAD UserWritePasswordUsingObjectId* | ObjectID kullanarak kullanıcı kaydı güncelleştirmek için teknik profili |
| *AAD-UserReadUsingObjectId* | Teknik profili, kullanıcı kimlik doğrulaması yaptıktan sonra veri okumak için kullanılır |

### <a name="technical-profiles-for-self-asserted"></a>Kendi kendine onaylanan için teknik profiller

| Teknik profili | Açıklama |
|-------------------|-------------|
| *SelfAsserted sosyal* | |
| *SelfAsserted-ProfileUpdate* | |

### <a name="technical-profiles-for-local-account"></a>Yerel hesap için teknik profiller

| Teknik profili | Açıklama |
|-------------------|-------------|
| *LocalAccountSignUpWithLogonEmail* | |

### <a name="technical-profiles-for-session-management"></a>Oturum yönetimi için teknik profiller

| Teknik profili | Açıklama |
|-------------------|-------------|
| *SM Noop* | |
| *SM-AAD* | |
| *SM SocialSignup* | AAD oturum oturum arasında belirsizliğinin ortadan kaldırılmasını ve oturum açma için profil adı kullanılıyor. |
| *SM SocialLogin* | |
| *SM MFA* | |

### <a name="technical-profiles-for-the-trust-framework-policy-engine"></a>Güven framework ilke altyapısı için teknik profiller

Şu anda teknik profil için tanımlanan **Trustframework İlkesi altyapısı TechnicalProfiles** talep sağlayıcısı.

### <a name="technical-profiles-for-token-issuer"></a>Belirteci veren teknik profilleri

| Teknik profili | Açıklama |
|-------------------|-------------|
| *JwtIssuer* | |

## <a name="user-journeys"></a>Kullanıcı yolculuklarından

Bu bölümde zaten bildirilmiş kullanıcı yolculuklarından gösterilmektedir *B2C_1A_base* ilkesi. Bu kullanıcı yolculuklarından daha fazla başvurulan, geçersiz kılınan ve/veya kendi ilkelerinizi de olarak gerektiği şekilde genişletilmiş için açıktır *B2C_1A_base_extensions* ilkesi.

| Kullanıcı yolculuğu | Açıklama |
|--------------|-------------|
| *Kaydolma* | |
| *Oturum açma* | |
| *SignUpOrSignIn* | |
| *EditProfile* | |
| *PasswordReset* | |
