---
title: Parola olmadan Azure AD oturum açma Microsoft Authenticator uygulamasını (genel Önizleme)
description: Microsoft Authenticator uygulamasını olmadan parolanızı (genel Önizleme) kullanarak Azure AD için oturum açın
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 09/20/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: librown
ms.openlocfilehash: 3a9fba644bd379f3f54cf07cf35c0a54029756da
ms.sourcegitcommit: 02ce0fc22a71796f08a9aa20c76e2fa40eb2f10a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51287192"
---
# <a name="password-less-phone-sign-in-with-the-microsoft-authenticator-app-public-preview"></a>Parola olmadan telefonla oturum açma ile Microsoft Authenticator uygulamasını (genel Önizleme)

Microsoft Authenticator uygulamasını herhangi bir Azure AD hesabı için parola kullanmadan oturum açmak için kullanılabilir. Teknolojinin benzer [Windows iş için Hello](/windows/security/identity-protection/hello-for-business/hello-identity-verification), Microsoft Authenticator, bir cihaza bağlanır ve bir biyometrik veya PIN kodu kullanan bir kullanıcı kimlik bilgisi etkinleştirmek için anahtar tabanlı kimlik doğrulaması kullanır.

![Bir tarayıcı, Microsoft Authenticator uygulamasındaki oturum açma denemesi onaylanacak kullanıcı sorarak oturum örneği](./media/howto-authentication-phone-sign-in/phone-sign-in-microsoft-authenticator-app.png)

Telefonla oturum açma, Microsoft Authenticator uygulamasını etkinleştirilmiş bir kişiyi, bir kullanıcı adı girildikten sonra kullanıcıdan bir parola görmenin yerine, kendi uygulamasında bir sayıya dokunun bildiren bir ileti görürsünüz. Uygulamada, kullanıcı sayısıyla gerekir, onaylama seçin sonra PIN'ini sağlamak veya biyometrik, ardından kimlik doğrulama tamamlanır.

## <a name="enable-my-users"></a>Kullanıcılarım etkinleştir

Bir yönetici, genel Önizleme için önce bu kiracıda kimlik bilgileri kullanılmasına izin vermek için powershell aracılığıyla bir ilke eklemeniz gerekir. "Bilinen sorunlar" bölümü, bu adımı gerçekleştirmeden önce gözden geçirin.

### <a name="tenant-prerequisites"></a>Kiracı önkoşulları

* Azure Active Directory
* Son kullanıcıların Azure multi-Factor Authentication için etkinleştirilen
* Kullanıcılar cihazlarını kaydedebilir.

### <a name="steps-to-enable"></a>Etkinleştirme adımları

Azure Active Directory V2 PowerShell modülü genel önizleme sürümünü en son sürümüne sahip olun. Aşağıdaki komutları çalıştırarak bunu doğrulamak için kaldırıp yükleyin isteyebilirsiniz:

1. `Uninstall-Module -Name AzureADPreview`
2. `Install-Module -Name AzureADPreview`

Aşağıdaki PowerShell komutlarını kullanarak parola olmadan telefon oturum Önizleme etkinleştirebilirsiniz:

1. `Connect-AzureAD`
   1. Kimlik doğrulama iletişim kutusunda kiracıda bir hesapla oturum açın. Hesap ya da bir güvenlik yöneticisi veya genel yönetici olması gerekir.
1. `New-AzureADPolicy -Type AuthenticatorAppSignInPolicy -Definition '{"AuthenticatorAppSignInPolicy":{"Enabled":true}}' -isOrganizationDefault $true -DisplayName AuthenticatorAppSignIn`

## <a name="how-do-my-end-users-enable-phone-sign-in"></a>Telefonla oturum açma son Kullanıcılarım hizmetini nasıl?

Genel Önizleme için oluşturmak veya bu yeni kimlik bilgilerini kullanmak için kullanıcıların zorlamak için hiçbir yolu yoktur. Son kullanıcı yalnızca parola olmadan oturum açma sonra kendi Kiracı yönetici olarak etkin ve kullanıcı telefonla oturum açma etkinleştirmek için Microsoft Authenticator uygulamasının güncelleştirilmiş karşılaşır.

> [!NOTE]
> Yani bu özellik uygulamada bir kiracı için ilke etkinleştirildiğinde, kullanıcılar bu akış hemen karşılaşabileceğinizi olasılığı 2017 Mart itibaren kaldırıldı. Farkında olması ve kullanıcılarınızın bu değişikliğe hazırlanmak.
>

1. Azure multi-Factor authentication'da kaydetme
1. Microsoft Authenticator yüklü iOS 8.0 veya üzeri çalıştıran cihazlar ya da Android 6.0 veya üzeri en son sürümü.
1. İş veya Okul hesabı ile anında iletme bildirimleri uygulamaya eklenir. Son Kullanıcı belgelerini bulunabilir [ https://aka.ms/authappstart ](https://aka.ms/authappstart).

Kullanıcı, anında iletme bildirimleri Microsoft Authenticator uygulamasını ayarlama ile MFA hesabına sahiptir. sonra makalesindeki adımları izleyebilirsiniz [telefonunuzla parolanız yerine telefonunuzla oturum](../user-help/microsoft-authenticator-app-phone-signin-faq.md) oturum phone kaydı tamamlamak için.

## <a name="known-issues"></a>Bilinen Sorunlar

### <a name="ad-fs-integration"></a>AD FS tümleştirmesi

Bir kullanıcı Microsoft Authenticator parola olmadan kimlik bilgisi etkin olduğundan, bu kullanıcı için kimlik doğrulaması her zaman bir onay bildirimi göndermeyi varsayılan olacaktır. Bu mantık için ADFS oturum açma doğrulama için ek bir adım kullanıcıya sorulmadan "Bunun yerine parolanızı kullanın."'ye yönlendirilmesini karma kiracısındaki kullanıcılar engeller. Bu işlem, ayrıca tüm şirket içi koşullu erişim ilkeleri ve geçişli kimlik doğrulaması akışları atlayacaktır. Bu işlem bir login_hint ise, belirtilen bir kullanıcı AD FS'ye autoforwarded ve parola olmadan kimlik bilgilerini kullanma seçeneğini atlama istisnadır.

### <a name="azure-mfa-server"></a>Azure MFA sunucusu

Bir kuruluşun şirket içi Azure MFA sunucusu ile MFA için etkinleştirilen son kullanıcılar yine de oluşturabilir ve oturum açma tek bir parola olmadan telefon kimlik bilgilerini kullanın. Bu değişiklik, kullanıcı kimlik bilgileriyle Microsoft Authenticator'ın birden çok yükleme (5 +) yükseltmek çalışırsa, bir hataya neden olabilir.  

### <a name="device-registration"></a>Cihaz Kaydı

Bu yeni ve güçlü kimlik bilgisi oluşturmak için gereken önkoşullar biri olan tek bir kullanıcıya Azure AD kiracısı içinde bulunduğu cihaz kaydedilir. Cihaz kayıt kısıtlamaları nedeniyle, bir cihazın yalnızca tek bir kiracıda kaydedilebilir. Bu sınır, telefonla oturum açma için yalnızca bir iş veya Okul hesabı Microsoft Authenticator uygulamasını etkinleştirilebilir anlamına gelir.

## <a name="next-steps"></a>Sonraki adımlar

[Cihaz kaydı hakkında bilgi edinin](../devices/overview.md#getting-devices-under-the-control-of-azure-ad)

[Azure multi-Factor Authentication hakkında bilgi edinin](../authentication/howto-mfa-getstarted.md)
