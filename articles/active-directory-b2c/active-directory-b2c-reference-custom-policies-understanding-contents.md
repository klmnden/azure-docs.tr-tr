---
title: "Azure Active Directory B2C: Başlangıç paketinin özel ilkelerini anlama | Microsoft Docs"
description: "Bir konu Azure Active Directory B2C özel ilkeler hakkında"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: mtillman
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/25/2017
ms.author: joroja
ms.openlocfilehash: fccb6cfddc8629de7db0310340f07bffd1ff8a65
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="understanding-the-custom-policies-of-the-azure-ad-b2c-custom-policy-starter-pack"></a>Azure AD B2C özel ilke başlangıç paketinin özel ilkelerini anlama

Bu bölümde birlikte B2C_1A_base ilkesinin tüm çekirdek öğeleri listeler **başlangıç paketi** ve devralma aracılığıyla kendi ilkelerinizi yazmak için de *B2C_1A_base_extensions İlkesi*.

Bu nedenle, bu daha özellikle odaklanılmaktadır. önceden tanımlanmış talep türleri, talep dönüştürmeleri, içerik tanımları, Talep sağlayıcı kendi teknik profillerini ve çekirdek kullanıcı Yolculuklar ile.

> [!IMPORTANT]
> Microsoft veya bundan böyle temin edilen bilgilere göre zımni hiçbir garanti vermez. Değişiklikleri GA süreden önce herhangi bir zamanda GA zaman ya da sonra sunulmasının.

Kendi ilkelerinizi ve B2C_1A_base_extensions İlkesi, bu tanımları geçersiz kılın ve bu ana ilke gerektiği gibi ek olanları sağlayarak genişletir.

Çekirdek öğelerini *B2C_1A_base İlkesi* talep türleri, talep dönüştürmeleri ve içerik tanımlar. Bu öğeleri açıktır. kendi ilkelerinizi de olarak başvurulan kullanılabilir *B2C_1A_base_extensions İlkesi*.

## <a name="claims-schemas"></a>Talep şemaları

Başka bir talep şemaları, üç bölüme ayrılmıştır:

1.  Kullanıcı Yolculuklar düzgün çalışması gerekli olan minimum talepleri listeler ilk bölümü.
2.  Talepleri listeler ikinci bir bölümü, özellikle login.microsoftonline.com kimlik doğrulaması için diğer talep sağlayıcılardan geçirilecek sorgu dizesi parametreleri ve diğer özel parametreler için gereklidir. **Lütfen bu talepler değiştirmeyin**.
3.  Ve sonunda kullanıcıdan, toplanan herhangi bir ek, isteğe bağlı talep listeleyen üçüncü bir bölüm dizinde saklanan ve oturum açma sırasında belirteçleri gönderilir. Bu bölümde kullanıcıdan toplanan ve/veya belirteçte gönderilen yeni talep türü eklenebilir.

> [!IMPORTANT]
> Talep şema parolaları ve kullanıcı adları gibi belirli talepler kısıtlamaları içerir. Herhangi bir talep sağlayıcısı olarak Azure AD güven Framework (TF) ilkesi değerlendirir ve tüm kısıtlamaları premium ilkesinde modelled. Daha fazla kısıtlama eklemek ya da başka bir talep sağlayıcı kendi kısıtlamaları olan kimlik bilgisi depolama için kullanmak için bir ilke değiştirilmesi.

Kullanılabilir talep türleri aşağıda listelenmiştir.

### <a name="claims-that-are-required-for-the-user-journeys"></a>Kullanıcı Yolculuklar için gerekli olan talepleri

Aşağıdaki talep kullanıcı Yolculuklar düzgün çalışması gereklidir:

| Talep türü | Açıklama |
|-------------|-------------|
| *Kullanıcı Kimliği* | Kullanıcı adı |
| *signInName* | Oturum adı |
| *Tenantıd* | Azure AD B2C Premium kullanıcı nesnesinin Kiracı tanımlayıcısını (ID) |
| *objectID* | Azure AD B2C Premium kullanıcı nesnesinin nesne tanımlayıcısını (ID) |
| *Parola* | Parola |
| *#newpassword* | |
| *reenterPassword* | |
| *passwordPolicies* | Parola gücünü, sona erme vb. belirlemek için Azure AD B2C Premium tarafından kullanılan parola ilkeleri. |
| *Sub* | |
| *alternativeSecurityId* | |
| *Identityprovider* | |
| *görünen adı* | |
| *strongAuthenticationPhoneNumber* | Kullanıcının telefon numarası |
| *Verified.strongAuthenticationPhoneNumber* | |
| *E-posta* | Kullanıcıyla iletişim kurmak için kullanılan e-posta adresi |
| *signInNamesInfo.emailAddress* | Kullanıcı oturum açmak için kullandığınız e-posta adresi |
| *otherMails* | Kullanıcıyla iletişim kurmak için kullanılan e-posta adresleri |
| *userPrincipalName* | Azure AD B2C Premium içinde depolanan gibi kullanıcı adı |
| *upnUserName* | Kullanıcı asıl adı oluşturmak için kullanıcı adı |
| *mailNickName* | Azure AD B2C Premium içinde depolanan gibi kullanıcının posta takma adı |
| *newUser* | |
| *yürütülen-SelfAsserted-giriş* | Talep, öznitelikler kullanıcıdan toplanan olup olmadığını belirtir |
| *yürütülen-PhoneFactor-giriş* | Talep, yeni bir telefon numarası kullanıcıdan toplanan olup olmadığını belirtir |
| *authenticationSource* | Kullanıcının sosyal kimlik sağlayıcısı, login.microsoftonline.com veya yerel hesap doğrulanmış olduğunu belirtir |

### <a name="claims-required-for-query-string-parameters-and-other-special-parameters"></a>Sorgu dizesi parametreleri ve diğer özel parametreler için gereken talepleri

Aşağıdaki talep varsayılan olarak, diğer talep sağlayıcıları (bazı sorgu dizesi parametreleri de dahil olmak üzere) özel parametrelere geçirmek için gereklidir:

| Talep türü | Açıklama |
|-------------|-------------|
| *nux* | Yerel hesap kimlik doğrulaması için login.microsoftonline.com için geçirilen özel parametresi |
| *NCA* | Yerel hesap kimlik doğrulaması için login.microsoftonline.com için geçirilen özel parametresi |
| *istemi* | Yerel hesap kimlik doğrulaması için login.microsoftonline.com için geçirilen özel parametresi |
| *Mkt* | Yerel hesap kimlik doğrulaması için login.microsoftonline.com için geçirilen özel parametresi |
| *LC* | Yerel hesap kimlik doğrulaması için login.microsoftonline.com için geçirilen özel parametresi |
| *grant_type* | Yerel hesap kimlik doğrulaması için login.microsoftonline.com için geçirilen özel parametresi |
| *Kapsam* | Yerel hesap kimlik doğrulaması için login.microsoftonline.com için geçirilen özel parametresi |
| *client_id* | Yerel hesap kimlik doğrulaması için login.microsoftonline.com için geçirilen özel parametresi |
| *objectIdFromSession* | Nesne Kimliği SSO oturumundan alındıktan göstermek için varsayılan oturum yönetimi sağlayıcısı tarafından sağlanan parametre |
| *isActiveMFASession* | Kullanıcının etkin bir MFA oturumu olduğunu belirtmek için MFA oturum yönetimi tarafından sağlanan parametresi |

### <a name="additional-optional-claims-that-can-be-collected"></a>Toplanabilir ek (isteğe bağlı) talepleri

Aşağıdaki talep kullanıcılardan toplanan, dizinde saklanan ve belirteçte gönderilen ek taleplerdir. Önce özetlendiği gibi ek talep bu listeye eklenebilir.

| Talep türü | Açıklama |
|-------------|-------------|
| *givenName* | Kullanıcının verilen adı (ilk adı olarak da bilinir) |
| *Soyadı* | Kullanıcının soyadı (Aile adı ve Soyadı olarak da bilinir) |
| *Extension_picture* | Sosyal kullanıcının resim |

## <a name="claim-transformations"></a>Talep dönüştürmeleri

Kullanılabilir talep dönüştürmeleri, aşağıda listelenmiştir.

| Talep dönüştürme | Açıklama |
|----------------------|-------------|
| *CreateOtherMailsFromEmail* | |
| *CreateRandomUPNUserName* | |
| *CreateUserPrincipalName* | |
| *CreateSubjectClaimFromObjectID* | |
| *CreateSubjectClaimFromAlternativeSecurityId* | |
| *CreateAlternativeSecurityId* | |

## <a name="content-definitions"></a>İçerik tanımları

Bu bölümde zaten bildirilen içerik tanımları açıklanmaktadır *B2C_1A_base* ilkesi. Bu içerik tanımları başvurulan, geçersiz ve/veya kendi ilkelerinizi de olarak gerektiği şekilde genişletilmiş uygulanmadıkça *B2C_1A_base_extensions* ilkesi.

| Talep sağlayıcı | Açıklama |
|-----------------|-------------|
| *Facebook* | |
| *Yerel hesap oturum açma* | |
| *PhoneFactor* | |
| *Azure Active Directory* | |
| *Kendi kendine uygulanan* | |
| *Yerel hesap* | |
| *Oturum yönetimi* | |
| *Trustframework ilke altyapısı* | |
| *TechnicalProfiles* | |
| *Belirteci veren* | |

## <a name="technical-profiles"></a>Teknik profilleri

Bu bölümde Talep sağlayıcı başına zaten tanımlanmış teknik profilleri gösterilmektedir *B2C_1A_base* ilkesi. Bu teknik profiller daha fazla başvurulan, geçersiz ve/veya kendi ilkelerinizi de olarak gerektiği şekilde genişletilmiş uygulanmadıkça *B2C_1A_base_extensions* ilkesi.

### <a name="technical-profiles-for-facebook"></a>Facebook için teknik profilleri

| Teknik profili | Açıklama |
|-------------------|-------------|
| *Facebook OAUTH* | |

### <a name="technical-profiles-for-local-account-signin"></a>Yerel hesap oturum açma için teknik profilleri

| Teknik profili | Açıklama |
|-------------------|-------------|
| *Oturum açma etkileşimsiz* | |

### <a name="technical-profiles-for-phone-factor"></a>Telefon faktörü için teknik profilleri

| Teknik profili | Açıklama |
|-------------------|-------------|
| *PhoneFactor giriş* | |
| *PhoneFactor InputOrVerify* | |
| *PhoneFactor doğrulayın* | |

### <a name="technical-profiles-for-azure-active-directory"></a>Azure Active Directory için teknik profilleri

| Teknik profili | Açıklama |
|-------------------|-------------|
| *AAD-genel* | Diğer AAD xxx teknik profilleri dahil teknik profili |
| *AAD UserWriteUsingAlternativeSecurityId* | Sosyal oturum açma teknik profili |
| *AAD UserReadUsingAlternativeSecurityId* | Sosyal oturum açma teknik profili |
| *AAD UserReadUsingAlternativeSecurityId NoError* | Sosyal oturum açma teknik profili |
| *AAD UserWritePasswordUsingLogonEmail* | Yerel hesaplar için teknik profili |
| *AAD UserReadUsingEmailAddress* | Yerel hesaplar için teknik profili |
| *AAD UserWriteProfileUsingObjectId* | ObjectID kullanarak kullanıcı kaydını güncelleştirmek için teknik profili |
| *AAD UserWritePhoneNumberUsingObjectId* | ObjectID kullanarak kullanıcı kaydını güncelleştirmek için teknik profili |
| *AAD UserWritePasswordUsingObjectId* | ObjectID kullanarak kullanıcı kaydını güncelleştirmek için teknik profili |
| *AAD UserReadUsingObjectId* | Teknik profili kullanıcı kimlik doğrulaması yaptıktan sonra verileri okumak için kullanılır |

### <a name="technical-profiles-for-self-asserted"></a>Kendi kendine uygulanan için teknik profilleri

| Teknik profili | Açıklama |
|-------------------|-------------|
| *SelfAsserted sosyal* | |
| *SelfAsserted ProfileUpdate* | |

### <a name="technical-profiles-for-local-account"></a>Yerel hesap için teknik profilleri

| Teknik profili | Açıklama |
|-------------------|-------------|
| *LocalAccountSignUpWithLogonEmail* | |

### <a name="technical-profiles-for-session-management"></a>Oturum yönetimi için teknik profilleri

| Teknik profili | Açıklama |
|-------------------|-------------|
| *SM sekmeyi* | |
| *SM AAD* | |
| *SM SocialSignup* | AAD oturum oturum arasında belirsizliğini ortadan kaldırmak ve oturum açma profili adı kullanılıyor |
| *SM SocialLogin* | |
| *SM MFA* | |

### <a name="technical-profiles-for-trustframework-policy-engine-technicalprofiles"></a>Trustframework ilke altyapısı TechnicalProfiles için teknik profilleri

Şu anda hiçbir teknik profilleri tanımlanmış **Trustframework ilke altyapısı TechnicalProfiles** Talep sağlayıcı.

### <a name="technical-profiles-for-token-issuer"></a>Belirteç Verenin için teknik profilleri

| Teknik profili | Açıklama |
|-------------------|-------------|
| *JwtIssuer* | |

## <a name="user-journeys"></a>Kullanıcı Yolculuklar

Bu bölümde zaten bildirilen kullanıcı Yolculuklar gösterilmektedir *B2C_1A_base* ilkesi. Bu kullanıcı Yolculuklar daha fazla başvurulan, geçersiz ve/veya kendi ilkelerinizi de olarak gerektiği şekilde genişletilmiş uygulanmadıkça *B2C_1A_base_extensions* ilkesi.

| Kullanıcı gezisine | Açıklama |
|--------------|-------------|
| *Kaydolma* | |
| *Oturum açma* | |
| *SignUpOrSignIn* | |
| *EditProfile* | |
| *PasswordReset* | |
