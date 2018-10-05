---
title: Alma bulutta Azure mfa'yı kullanmaya başlama
description: Microsoft Azure multi-Factor Authentication ile koşullu erişim kullanmaya başlayın
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 09/01/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: 07e9126b73b0282f61567fe8d06f785bf9a04fef
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48803488"
---
# <a name="deploy-cloud-based-azure-multi-factor-authentication"></a>Bulut tabanlı Azure multi-Factor Authentication'ı dağıtma

Azure multi-Factor Authentication (Azure MFA) ile çalışmaya başlama basit bir işlemdir.

Başlamadan önce aşağıdaki önkoşulların karşılandığından emin olun:

* Azure AD kiracınızda genel yönetici hesabı. Bu adımın yardıma ihtiyacınız olursa makalemizi bkz [Azure AD'yi kullanmaya başlama](../get-started-azure-ad.md).
* Doğru lisansları kullanıcılara atanır. Daha fazla bilgiye ihtiyacınız varsa, bkz [Azure multi-Factor Authentication'ı alma](concept-mfa-licensing.md).

## <a name="choose-how-to-enable"></a>Etkinleştirme seçin

**Koşullu erişim ilkesi tarafından etkinleştirilen** -bu yöntem, bu makalede ele alınmıştır. Kullanıcılarınız için iki aşamalı doğrulamayı etkinleştirmek için en esnek yöntemdir. Yalnızca koşullu erişim ilkesi kullanarak etkinleştirmek için bulutta Azure MFA çalışır ve Azure AD premium özelliğidir.

**Azure AD kimlik koruması tarafından etkin** -iki aşamalı kimlik doğrulaması oturum açma riski tüm bulut uygulamaları için yalnızca temel, Azure AD kimlik koruması risk ilkesini bu yöntemi kullanır. Bu yöntem, Azure Active Directory P2 lisansı gerektirir. Bu yöntem hakkında daha fazla bilgi bulunabilir [kullanıcı riski ilkesi yapılandırma](../identity-protection/howto-user-risk-policy.md).

**Kullanıcı durumunu değiştirerek etkin** -Bu, iki aşamalı doğrulama gerektirme geleneksel yöntemidir. Bu, hem de bulutta Azure MFA ve Azure MFA sunucusu ile çalışır. Bu yöntemi kullanarak, kullanıcıların iki aşamalı doğrulamanın gerektirir **her** oturum açın ve koşullu erişim ilkeleri geçersiz kılar. Bu yöntem hakkında daha fazla bilgi bulunabilir [bir kullanıcı için iki aşamalı doğrulama gerektirme](howto-mfa-userstates.md).

> [!Note]
> Lisanslar ve fiyatlandırma hakkında daha fazla bilgi bulunabilir [Azure AD'ye](https://azure.microsoft.com/pricing/details/active-directory/
) ve [multi-Factor Authentication](https://azure.microsoft.com/pricing/details/multi-factor-authentication/) fiyatlandırma sayfalarına.

## <a name="choose-authentication-methods"></a>Kimlik doğrulama yöntemi seçin

Kuruluşunuzun gereksinimlerine göre kullanıcılarınız için en az bir kimlik doğrulama yöntemini etkinleştirin. Microsoft Authenticator uygulamasını kullanıcılar için etkin olduğunda en iyi kullanıcı deneyimi sağlar buluyoruz. Hangi yöntemler kullanılabilir ve bunların nasıl ayarlanacağını anlamak ihtiyacınız varsa bkz [kimlik doğrulama yöntemleri nelerdir](concept-authentication-methods.md).

## <a name="get-users-to-enroll"></a>Kullanıcıların kaydolmalarına Al

Koşullu erişim ilkesi etkinleştirdikten sonra kullanıcılar, ilkeyle korunan bir uygulama kullandıkları başlatıldığında kaydetmeye zorlanır. Tüm bulut uygulamalarının tüm kullanıcılar için MFA gerektiren bir ilke etkinleştirirseniz, bu eylem sıkıntılarına otomatik çözüm sağlamaya kullanıcılarınıza ve Yardım masanız için neden olabilir. Önceden adresindeki kayıt portalına kullanarak kimlik doğrulama yöntemlerini kaydedin açmasına istemeniz önerilir [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup). Çoğu kuruluş, posterler, tablo kartları ve e-posta iletilerini oluşturma benimsenmesini yardımcı bulun.

## <a name="enable-multi-factor-authentication-with-conditional-access"></a>Koşullu erişim ile çok faktörlü kimlik doğrulamasını etkinleştirme

Oturum [Azure portalında](https://portal.azure.com) bir genel yönetici hesabını kullanarak.

### <a name="choose-verification-options"></a>Doğrulama seçenekleri seçin

Azure multi-Factor Authentication'ı etkinleştirmeden önce kuruluşunuzun sağlarlar hangi doğrulama seçeneklerini belirlemeniz gerekir. Genel seçenekleri çoğu kullanmanız mümkün olduğu gibi alıştırmanın amacı doğrultusunda, telefon ve metin iletisi telefon çağrısı sağlar. Kimlik doğrulama yöntemleri ve bunların kullanım ile ilgili daha fazla bilgi makalesinde bulunabilir [kimlik doğrulama yöntemleri nelerdir?](concept-authentication-methods.md)

1. Gözat **Azure Active Directory**, **kullanıcılar**, **çok faktörlü kimlik doğrulaması**.

   ![Azure portalında Azure AD kullanıcıları dikey penceresinden multi-Factor Authentication portalına erişim](media/howto-mfa-getstarted/users-mfa.png)

1. Yeni sekmede açılır göz atın **hizmet ayarları**.
1. Altında **doğrulama seçenekleri**, tüm kullanıcıların yararlanabileceği yöntemler için kutuları işaretleyin.

   ![Multi-Factor Authentication hizmeti Ayarlar sekmesinde doğrulama yöntemlerini yapılandırma](media/howto-mfa-getstarted/mfa-servicesettings-verificationoptions.png)

4. **Kaydet**'e tıklayın.
5. Kapat **hizmet ayarları** sekmesi.

### <a name="create-conditional-access-policy"></a>Koşullu erişim ilkesi oluşturma

1. Oturum [Azure portalında](https://portal.azure.com) bir genel yönetici hesabını kullanarak.
1. **Azure Active Directory**, **Koşullu erişim** sayfasına gidin.
1. Seçin **yeni ilke**.
1. İlkeniz için anlamlı bir ad sağlayın.
1. Altında **kullanıcılar ve gruplar**:
   * Üzerinde **INCLUDE** sekmesinde **tüm kullanıcılar** radyo düğmesi
   * Önerilen: Üzerinde **hariç** sekmesinde, onay kutusunu için **kullanıcılar ve gruplar** ve kullanıcıların kendi kimlik doğrulama yöntemlerini erişimi olmadığında, özel durumlar için kullanılacak bir grubu seçin.
   * **Bitti**’ye tıklayın.
1. Altında **bulut uygulamaları**seçin **tüm bulut uygulamaları** radyo düğmesi.
   * : İsteğe bağlı **hariç** sekmesinde, kuruluşunuz için MFA gerektirmeyen bir bulut uygulamaları seçin.
   * **Bitti**’ye tıklayın.
1. Altında **koşullar** bölümü:
   * İsteğe bağlı: Azure kimlik koruması etkinleştirilirse, ilkenin bir parçası oturum açma riskini değerlendirmek seçebilirsiniz.
   * İsteğe bağlı: güvenilen konumları yapılandırılmış veya adlandırılmış konumlar, dahil etmek veya ilkeden konumların dışlamak için belirtebilirsiniz.
1. Altında **Grant**, emin **erişim ver** radyo düğmesi seçili.
    * İçin kutuyu **çok faktörlü kimlik doğrulaması gerektiren**.
    * **Seç**'e tıklayın.
1. Skip **oturumu** bölümü.
1. Ayarlama **ilkesini etkinleştir** geç **üzerinde**.
1. **Oluştur**’a tıklayın.

![Pilot grubundaki Azure portalı kullanıcıları için mfa'yı etkinleştirmek için bir koşullu erişim ilkesi oluşturma](media/howto-mfa-getstarted/conditionalaccess-newpolicy.png)

### <a name="test-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication'ı test etme

Koşullu erişim ilkenizi çalıştığını doğrulamak için MFA gerektirmemelidir bir kaynağa, ardından MFA gerektiren Azure portalında oturum açmayı test edin.

1. InPrivate modunda veya gizli modda yeni bir tarayıcı penceresi açın ve [https://account.activedirectory.windowsazure.com](https://account.activedirectory.windowsazure.com) adresine gidin.
   * Bu makale ve bunu, mfa'yı tamamlamak için istemelisiniz değil, Not Önkoşullar kısmında bir parçası olarak oluşturulan test kullanıcısı oturum açın.
   * Tarayıcı penceresini kapatın.
2. InPrivate modunda veya gizli modda yeni bir tarayıcı penceresi açın ve [https://portal.azure.com](https://portal.azure.com) adresine gidin.
   * Oturum açın test önkoşullar bölümünde bu makalede ve artık olması gerektiğini unutmayın bir parçası olarak oluşturulan kullanıcı gerekli kaydolun ve Azure multi-Factor Authentication'ı kullanın.
   * Tarayıcı penceresini kapatın.

## <a name="next-steps"></a>Sonraki adımlar

Tebrikler, bulutta Azure multi-Factor Authentication özelliğini ayarladığınıza ayarlayın.

Güvenilen IP'ler, özel sesli mesajları ve sahtekarlık uyarısı gibi diğer ayarları yapılandırmak için bu makaleye bakın [Azure multi-Factor Authentication'ı yapılandırma ayarlarını](howto-mfa-mfasettings.md).

Azure multi-Factor Authentication makalesinde bulunabilir, kullanıcı ayarlarını yönetme hakkında bilgi [bulutta Azure multi Factor Authentication ile kullanıcı ayarlarını yönetme](howto-mfa-userdevicesettings.md).

[Azure multi-Factor Authentication ve Azure AD Self Servis parola sıfırlama için yakınsanmış kaydını etkinleştirmek](concept-registration-mfa-sspr-converged.md).
