---
title: Yakınsanmış kaydı için Azure AD SSPR ve mfa'yı (genel Önizleme)
description: Kayıt (genel Önizleme) Azure AD multi-Factor Authentication ve Self Servis parola sıfırlama
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 01/30/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry, michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1923a34c5c49f16b32ed5b5d0913a65ce0b1ea21
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2019
ms.locfileid: "56453384"
---
# <a name="converged-registration-for-self-service-password-reset-and-azure-multi-factor-authentication-public-preview"></a>Yakınsanmış kaydı için Self Servis parola sıfırlama ve Azure multi-Factor Authentication (genel Önizleme)

Şimdiye kadar kullanıcıların Azure multi-Factor Authentication (MFA) ve iki farklı portallarında Self Servis parola sıfırlama (SSPR) için kimlik doğrulama yöntemlerini kaydetmeniz gerekir. Çok sayıda kullanıcı, benzer yöntemler Azure MFA ve SSPR için kullanılan ve her iki portallarında kaydetmemek yanıltıcı. Bazı kullanıcılar ardından gerekli, Yardım masanıza yapılan aramaları için önde gelen Azure mfa'yı veya SSPR kullanacak şekilde aktaramadık. 

Artık, kullanıcıların bir kez kaydedebilir ve Azure mfa'yı hem SSPR avantajlarından yararlanın. Kullanıcıların kendi kimlik doğrulama yöntemlerini bu özellikler için iki kez kaydetmek gerekmez.  

Kuruluşunuz için bu yeni deneyim etkinleştirmeden önce bu makalede gözden geçirmenizi öneririz ve [kullanıcı belgeleri](https://aka.ms/securityinfoguide) kullanıcılarınızı deneyimi sunacak etkisini anlamak için. Kullanıcı belgeleri, eğitmek ve kullanıcılarınız için yeni deneyim hazırlamak ve başarılı bir dağıtım sağlamak için kullanabilirsiniz.

|     |
| --- |
| Azure multi-Factor Authentication ve Self Servis parola sıfırlama için yakınsanmış kaydı Azure Active Directory (Azure AD) genel Önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

Azure multi-Factor Authentication hem de Self Servis parola sıfırlama tek bir deneyimle için kimlik doğrulama yöntemlerini kaydedin açmasına etkinleştirmek için aşağıdaki adımları tamamlayın:

1. Azure portalına genel yönetici veya Kullanıcı Yöneticisi olarak oturum açın.
2. Gözat **Azure Active Directory** > **kullanıcı ayarları** > **erişim paneli Önizleme özellikleri için ayarları yönetme**.
3. Altında **kullanıcılar kaydetme ve güvenlik bilgilerinizi yönetmek için Önizleme özelliklerini kullanabilir**, etkinleştirmek seçebileceğiniz bir **seçili** için kullanıcı ve grup **tüm** kullanıcılar.

Kullanıcılar artık giderek [ https://aka.ms/setupsecurityinfo ](https://aka.ms/setupsecurityinfo) MFA ve SSPR için kendi kimlik doğrulama yöntemlerini kaydedilecek. Bu yeni deneyimi, kullanıcıların göreceği hakkında daha fazla bilgi için bkz: [kullanıcı belgeleri](https://aka.ms/securityinfoguide).  

> [!NOTE]
> Bu deneyim etkinleştirdikten sonra bu yöntemler MFA ve SSPR ilkelerinde etkinleştirilip etkinleştirilmediğini kaydetmek veya telefon numarası ya da yeni deneyim aracılığıyla mobil uygulama onaylayın kullanıcılar bunları MFA ve SSPR için kullanabilirsiniz. Bu deneyim devre dışı bırakırsanız, https:/aka.ms/ssprsetup önceki SSPR kayıt sayfasına gidin kullanıcılar sayfası erişebilmeniz için önce çok faktörlü kimlik doğrulaması gerçekleştirmek için gerekir.  

## <a name="how-it-works"></a>Nasıl çalışır?

Bir kullanıcı kimlik doğrulama yöntemleri ayrı MFA ve SSPR kayıt deneyimler aracılığıyla daha önce kaydedildiyse, bu bilgileri tekrar kaydetmek gerekmez. Ancak, MFA veya SSPR için kaydedin açmasına ayarlarınızı ihtiyacınız varsa oturum açarken güvenlik bilgilerini incelemek isteyen bir ileti görebilirsiniz.

Kullanıcı MFA için kullanılabilecek bir yöntem kaydedildiyse, yeni deneyimi erişebilmeniz için önce çok faktörlü kimlik doğrulaması gerçekleştirmek üzere istenir.

Kayıt için mfa'yı veya SSPR zorunlu ve bir kullanıcı henüz kayıtlı değil, oturum açtığında kaydetmek için istenir.

Oturum açma sırasında kaydetmek isteyip istemediğiniz sorulur kullanıcılar aşağıdaki deneyimi bakın:

![Yakınsanmış kaydı. Yöntemleri, yeni bir kullanıcı olarak ayarlayın.](./media/concept-registration-mfa-sspr-converged/concept-registration-add-methods.png)

> [!NOTE]
> Bu deneyim, yalnızca, bir kullanıcı oturum açma sırasında kaydetmek için istenir gösterilir. Deneyimini doğrudan Git kullanıcılar https://aka.ms/setupsecurityinfo bu makalenin sonraki bölümlerinde açıklanan deneyimi farklı bir sürümüne bakın.

Görüntülenen kimlik doğrulaması yöntemleri, yöntemleri, MFA ve SSPR ilkeleri etkin göre değiştirin. Kullanıcı MFA ilkesini, SSPR İlkesi veya her ikisi ile uyumlu olması için gereken kimlik doğrulama yöntemleri en az sayıda kayıt istenir. Hangi kimlik doğrulama yöntemlerini kullanıcı kaydedebilir esneklik varsa, bunlar seçebilirsiniz **güvenlik bilgisi seçin** diğer kimlik doğrulama yöntemlerini seçmesine.  

> [!NOTE]
> Hem mobil uygulama bildirimi hem de mobil uygulama kodu kullanımını etkinleştirirseniz, öğe bir bildirim aracılığıyla Microsoft Authenticator uygulamasını kaydetme kullanıcıların kimliklerini doğrulamak için hem bildirim hem de kodu kullanabilirsiniz.

Önceki MFA kayıt deneyimi, bir uygulama parolası yeni kayıt deneyimi geçerken kaydetmek için kullanıcıların istenmez. Bunun yerine, bunlar uygulama parolaları yeni deneyime kaydedin için uygulama parolaları Öğreticisi listelenen adımları izlemelidir.  

Bir kullanıcı kayıt tamamlandıktan sonra kendi varsayılan MFA yöntemini otomatik olarak ayarlanır. Bir doğrulayıcı uygulama kullanıcının kayıtlı varsayılan yöntemi uygulamasına ayarlanır. Kullanıcı, bir kimlik doğrulayıcı uygulama kaydedilmedi ve telefon numarası yalnızca kayıtlı varsayılan yöntemini telefon araması olarak ayarlanır. Kullanıcılar, kendi varsayılan giderek değiştirebilirsiniz https://aka.ms/setupsecurityinfo seçerek **Varsayılanı Değiştir**.  

Kayıt zorlanmaz, kullanıcıların kendi kimlik doğrulama yöntemleri, yönetebileceği https://aka.ms/setupsecurityinfo. Kullanıcı MFA için kullanılabilecek bir yöntem daha önce kaydedildiyse, sayfanın erişebilmeniz için önce çok faktörlü kimlik doğrulaması gerçekleştirmek üzere istenir.  

![Yakınsanmış kaydı. Yöntemler, kayıtlı kullanıcı olarak düzenleyin.](./media/concept-registration-mfa-sspr-converged/concept-registration-edit-methods.png)

Bu sayfada, önceden kaydedilmiş kimlik doğrulama yöntemleri ve kimlik doğrulama yöntemleri için ofis telefonu gibi kayıtlı kullanıcılar görür. Ayrıca kullanıcılar eklemek, düzenlemek veya silmek kendi kimlik doğrulama yöntemlerini (ofis telefonu hariç).  

Bu yeni deneyim için Denetim günlükleri, Denetim günlüğü kimlik doğrulama yöntemleri kategorisinde yok.  

## <a name="known-issues"></a>Bilinen sorunlar

**Kullanıcı kısa mesaj kullanarak telefon kaydederken telefon araması set varsayılan MFA yöntemi**

   * Bazı kullanıcılar, kısa mesaj kullanarak telefon numarasını kaydettikten sonra varsayılan MFA yöntemleri telefon araması olarak ayarlandığını görebilirsiniz. Kullanıcılar tarafından başvurulan makalelerdeki yönergeleri takip ederek kendi varsayılan yöntemini değiştirerek bu sorunu giderebilir [güvenlik bilgilerinizi (Önizleme) değiştirme hakkında genel bakış](../user-help/security-info-add-update-methods-overview.md).

**Bir yönetici kendi varsayılan yöntemini devre dışı bırakır. sonra kullanıcı yeni kaydı deneyimini erişemiyor**

   * Bazı kullanıcılar, yönetici kullanıcıların önceden kaydedilmiş varsayılan MFA yöntemi devre dışı bırakılmışsa yeni kayıt deneyimi erişmek mümkün olmayabilir. Örnek bir senaryo aşağıda verilmiştir:
      1. Kullanıcı daha önce kaydedilen telefon numarasını ve kendi varsayılan yöntemini telefon araması olarak ayarlayın.
      2. Kiracı için bir MFA yöntemi olarak telefon araması yönetici devre dışı bırakır.
      3. Kullanıcı oturum açma sırasında Kiracı SSPR ilkeyi karşılamak için ek bir yöntem kaydedilecek gerektiğinden kaydetmek için istenir.
      4. Kullanıcı kaydetmek çalışır, ancak sayfasına erişemezseniz ve varsayılan bir yöntem olmadığından bir döngüde takılı.

## <a name="next-steps"></a>Sonraki adımlar

[Azure'ı dağıtmayı öğrenin AD Self Servis parola sıfırlama](howto-sspr-deployment.md)

[Oturum açmak için çok faktörlü kimlik doğrulaması isteme hakkında bilgi edinin](howto-mfa-getstarted.md)

[Kullanıcı kimlik doğrulama yöntemleri yapılandırma hakkında bilgi edinin](https://aka.ms/securityinfoguide)
