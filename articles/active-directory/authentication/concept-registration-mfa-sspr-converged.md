---
title: Yakınsanmış kaydı için Azure AD SSPR ve mfa'yı (genel Önizleme)
description: Kayıt (genel Önizleme) Azure AD çok faktörlü kimlik doğrulamasını ve Self Servis parola sıfırlama
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry, michmcla
ms.openlocfilehash: af57faddcc1413747b4bb847e27287ba86562175
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42056096"
---
# <a name="converged-registration-for-self-service-password-reset-and-azure-multi-factor-authentication-public-preview"></a>Yakınsanmış kaydı için Self Servis parola sıfırlama ve Azure multi-Factor Authentication (genel Önizleme)

Şimdiye kadar kullanıcıların Azure multi-Factor Authentication (MFA) ve iki farklı portallarında Self Servis parola sıfırlama (SSPR) için kimlik doğrulama yöntemlerini kaydetmeniz gerekir. Çok sayıda kullanıcı benzer yöntemler Azure MFA ve SSPR için kullanılan ve her iki portallarında kaydetmemek olgu yanıltıcı. Azure mfa'yı veya, bir Yardım Masası çağrı baştaki gerektiğinde SSPR erişememe bazı kullanıcılar ve büyük olasılıkla upset bir kullanıcı bu girdilerinde gerektiriyordu. Artık, kullanıcıların bir kez kaydedebilir ve Azure mfa'yı hem de bu özellikler için kendi kimlik doğrulama yöntemlerini iki kez kaydetmek için gereksinimini ortadan kaldırır, SSPR avantajlarından yararlanın.  

Kuruluşunuz için bu yeni deneyim etkinleştirmeden önce bu makalede gözden geçirmenizi öneririz yanı sıra sunduğumuz [son kullanıcı belgelerinin](https://aka.ms/securityinfoguide) etkisini anlamak için yönetim deneyimine kullanıcılarınızı sahip olursunuz. Kullanabileceğiniz [son kullanıcı belgelerinin](https://aka.ms/securityinfoguide) eğitmek ve yeni deneyimi ve başarılı bir dağıtım sağlamak için kullanıcılarınıza hazırlamak için.

|     |
| --- |
| Azure multi-Factor Authentication ve Self Servis parola sıfırlama için yakınsanmış kaydı Azure Active Directory genel Önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz: [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Azure multi-Factor Authentication hem de Self Servis parola sıfırlama tek bir deneyimle için kimlik doğrulama yöntemlerini kaydedin açmasına etkinleştirmek için aşağıdaki adımları tamamlayın:

1. Azure portalına genel yönetici veya Kullanıcı Yöneticisi olarak oturum açın.
2. Gözat **Azure Active Directory**, **kullanıcı ayarları**, **erişim paneli Önizleme özellikleri için ayarları yönetme**.
3. Altında **kullanıcılar kaydetme ve güvenlik bilgilerinizi yönetmek için Önizleme özelliklerini kullanabilir**, etkinleştirmek seçebileceğiniz bir **seçili** için kullanıcı ve grup **tüm** kullanıcılar.

Kullanıcılar artık giderek [ https://aka.ms/setupsecurityinfo ](https://aka.ms/setupsecurityinfo) MFA ve SSPR için kendi kimlik doğrulama yöntemlerini kaydedilecek. Bu yeni deneyimi, kullanıcıların göreceği hakkında daha fazla bilgi edinmek için müşterilerimize [son kullanıcı belgelerinin](https://aka.ms/securityinfoguide).  

> [!NOTE]
> Bu deneyim, kaydetme veya telefon numarasını onaylamak kullanıcıları bir kez etkinleştirmeniz veya bu yöntemler MFA ve SSPR ilkelerinde etkinleştirilip etkinleştirilmediğini yeni deneyim aracılığıyla mobil uygulama için mfa'yı ve SSPR kullanmayı imkanına sahip olursunuz. Bu deneyim devre dışı bırakırsanız, aka.ms/ssprsetup önceki SSPR kayıt sayfasına gidin kullanıcılar sayfası erişebilmeniz için önce MFA gerçekleştirmek için gerekir.  

## <a name="how-it-works"></a>Nasıl çalışır?

Bir kullanıcı kimlik doğrulama yöntemleri ayrı MFA ve SSPR kayıt deneyimler aracılığıyla daha önce kaydedildiyse, bu bilgileri tekrar kaydetmek gerekmez. Ancak, MFA veya SSPR için kaydedin açmasına ayarlarınızı ihtiyacınız varsa oturum açarken güvenlik bilgilerini gözden geçirmek için bir istem görebilirsiniz.

Kullanıcı MFA için kullanılabilecek bir yöntem kaydedildiyse, yeni deneyimi erişebilmeniz için önce MFA gerçekleştirmek için istenir.

Kayıt için mfa'yı veya SSPR zorunlu ve bir kullanıcı henüz kayıtlı değil, oturum açtığında kaydetmek için istenir.

Oturum açma sırasında kaydetmek isteyip istemediğiniz sorulur kullanıcılar aşağıdaki deneyimi göreceksiniz:

![Yakınsanmış kaydı. Yeni bir kullanıcı olarak yöntemlerini ayarlama](./media/concept-registration-mfa-sspr-converged/concept-registration-add-methods.png)

> [!NOTE]
> Bu deneyim, yalnızca bir kullanıcı oturum açma sırasında kaydetmek için ne zaman istemde gösterilir. Bu makalenin sonraki bölümlerinde açıklanan deneyimi farklı bir sürümü aka.ms/setupsecurityinfo, yeni deneyimi doğrudan Git kullanıcıları görürsünüz.

Gösterilen kimlik doğrulama yöntemleri, MFA veya SSPR ilkeleri etkin yöntemlere bağlı olarak değişir. Kullanıcı MFA ilkesini, SSPR İlkesi veya her ikisi ile uyumlu olması için gereken kimlik doğrulama yöntemleri en az sayıda kayıt istenir. Hangi kimlik doğrulama yöntemlerini kullanıcı kaydedebilir esneklik varsa, bunlar seçebilirsiniz **güvenlik bilgisi seçin** diğer kimlik doğrulama yöntemlerini seçmesine.  

> [!NOTE]
> Hem mobil uygulama bildirimi hem de mobil uygulama kodu kullanımını etkinleştirirseniz, bir bildirim kullanarak Microsoft Authenticator uygulamasını kaydetme kullanıcıların kimliklerini doğrulamak için hem bildirim hem de kodu kullanabilirsiniz.

Önceki MFA kayıt deneyimi, kullanıcıların uygulama parolası yeni kayıt deneyimi geçerken kaydetmek için istenmez. Bunun yerine, uygulama parolaları yeni deneyiminde kaydetmek için uygulama parolaları öğreticimize listelenen adımları izlemelidir.  

Bir kullanıcı, kayıt işlemi tamamlandıktan sonra varsayılan MFA yöntemleri otomatik olarak ayarlanır. Kullanıcının kayıtlı Doğrulayıcı uygulama, uygulama için varsayılan yöntemi ayarlanır. Kullanıcı, bir kimlik doğrulayıcı uygulama kaydedilmedi ve telefon numarası yalnızca kayıtlı varsayılan yöntemini telefon araması olarak ayarlanır. Kullanıcılar, kendi varsayılan giderek değiştirebilirsiniz [ https://aka.ms/setupsecurityinfo ](https://aka.ms/setupsecurityinfo) seçerek **Varsayılanı Değiştir**.  

Kayıt zorlanmaz, kullanıcıların kendi kimlik doğrulama yöntemleri, yönetebileceği [ https://aka.ms/setupsecurityinfo ](https://aka.ms/setupsecurityinfo). Kullanıcı MFA gerçekleştirmek için kullanılan bir yöntem daha önce kaydedildiyse, sayfanın erişebilmeniz için önce MFA gerçekleştirmek için istenecektir.  

![Yakınsanmış kaydı. Kayıtlı kullanıcı olarak yöntemlerini Düzenle](./media/concept-registration-mfa-sspr-converged/concept-registration-edit-methods.png)

Bu sayfada, önceden kaydedilmiş kimlik doğrulama yöntemleri ve kimlik doğrulama yöntemleri için ofis telefonu gibi kayıtlı kullanıcılar görür. Ayrıca kullanıcılar eklemek, düzenlemek veya silmek kendi kimlik doğrulama yöntemlerini (ofis telefonu hariç).  

Bu yeni deneyim için Denetim günlükleri, Denetim günlüğü kimlik doğrulama yöntemleri kategorisinde yok.  

## <a name="known-issues"></a>Bilinen sorunlar

**Kullanıcı kısa mesaj kullanarak telefon kaydederken telefon araması set varsayılan MFA yöntemi**

   * Bazı kullanıcılar, kısa mesaj kullanarak telefon numarasını kaydettikten sonra varsayılan MFA yöntemleri telefon araması olarak ayarlandığını görebilirsiniz. Kullanıcılar, kendi varsayılan yöntemini makalede bulunan yönergeleri takip ederek değiştirerek bu sorunu giderebilir [güvenlik bilgilerinizi (Önizleme) yönetme](../user-help/security-info-manage-settings.md#change-your-info).

**Kullanıcı yönetici kendi varsayılan yöntemini devre dışı bırakır. sonra yeni kayıt deneyimi erişemiyor**

   * Bazı kullanıcılar önceden kaydedilmiş varsayılan MFA yöntemi, yönetici tarafından devre dışı ise yeni kayıt deneyimi erişmek mümkün olmayabilir. Örnek bir senaryo aşağıda verilmiştir:
      1. Kullanıcı daha önce kaydedilen telefon numarasını ve kendi varsayılan yöntemini telefon araması olarak ayarlayın.
      2. Kiracı için bir MFA yöntemi olarak telefon araması yönetici devre dışı bırakır.
      3. Kullanıcı oturum açma sırasında Kiracı SSPR ilkeyi karşılamak için ek bir yöntem kaydedilecek gerektiğinden kaydetmek için istenir.
      4. Kullanıcı kaydetmek çalışır, ancak ayarlanmış bir varsayılan yöntemi olmaması nedeniyle sayfasına erişemezseniz ve bir döngüde takılı.

## <a name="next-steps"></a>Sonraki adımlar

[Azure'ı dağıtmayı öğrenin AD Self Servis parola sıfırlama](howto-sspr-deployment.md)

[Oturum açarken çok faktörlü kimlik doğrulaması isteme hakkında bilgi edinin](howto-mfa-getstarted.md)

[Son kullanıcı kimlik doğrulama yöntemini yapılandırma belgeleri](https://aka.ms/securityinfoguide)