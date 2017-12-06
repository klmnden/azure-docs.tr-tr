---
title: "Azure AD: parola sıfırlama | Microsoft Docs"
description: "Self Servis parola sıfırlama çalışmanızı erişimi yeniden kazanmak veya Okul kullanıcı hesabı için kullanın"
services: active-directory
keywords: 
documentationcenter: 
author: barlanmsft
manager: femila
ms.reviewer: sahenry
ms.assetid: 7ba69b18-317a-4a62-afa3-924c4ea8fb49
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: barlan
ms.custom: end-user
ms.openlocfilehash: 8a2fdc89241c659505d9e61e843c1ddf438f8c53
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2017
---
# <a name="reset-your-work-or-school-password"></a>İş veya Okul parolanızı sıfırlama

Parolanızı mı unuttunuz, hiçbir zaman şirket destek birinden alınan, hesabınıza kilitli veya değiştirmek istiyorsanız, size yardımcı olabilir. Parolanızı biliyorsanız ve değiştirmek yeterlidir, devam [parolamı Değiştir](#change-my-password) bölümü.

   > [!NOTE]
   > Xbox, hotmail.com veya outlook.com gibi kişisel hesabınıza geri alabilmeniz çalışıyorsanız, öneri deneyin [olamaz oturum açtığınızda Microsoft hesabınızı](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) makalesi.
   >

## <a name="reset-or-unlock-my-password-for-a-work-or-school-account"></a>İş veya okul hesabımın parolasını sıfırlama veya kilidini açma

Azure Active Directory (Azure AD) hesabınızı şunlardan biri nedeniyle erişemiyor olabilir:

* Parolanızı çalışmıyor ve onu sıfırlamak istiyor.
* Parolanızı biliyorsanız, ancak hesabınız kilitlendi ve kilidini açmak istiyor.

Azure AD Self Servis parola sıfırlama (SSPR) erişmek ve hesabınıza geri alabilmeniz için aşağıdaki adımları kullanın.

1. Herhangi bir iş veya Okul **oturum açma** sayfasında, **hesabınıza erişemiyor?** bağlamak ve ardından **iş veya Okul hesabı** veya doğrudan gitmek [ Parola sıfırlama sayfası](https://passwordreset.microsoftonline.com/).

    ![Hesabınıza erişemiyor musunuz?][Login]

2. İşiniz veya okulunuz girin **kullanıcı kimliği**, ekranda görün ve ardından karakterleri girerek bir robot olmayan kanıtlamak **sonraki**.

   > [!NOTE]
   > Bu işlevsellik, BT personeliniz etkin değilse, BT personeliniz e-posta veya kendi web portalı yardımcı olacak bir "yöneticinize başvurun" bağlantısı görüntülenir.
   >
   > Hesabınızın kilidini açmak gerekiyorsa, bu noktada seçeneğini **ı parolamı biliyorum ancak oturum açamıyorum.**
   >

3. BT personeliniz SSPR nasıl yapılandırdığına bağlı olarak bir veya daha fazla aşağıdaki kimlik doğrulama yöntemlerini görmeniz gerekir. Siz veya BT personeliniz bu bilgilerin bazıları içindeki adımları izleyerek için doldurulmuş [Self Servis parola sıfırlama için kaydetme](active-directory-passwords-reset-register.md) makalesi.

   * **Alternatif e-posta adresime gönder**
   * **Cep telefonuma kısa mesaj gönder**
   * **Cep telefonumu ara**
   * **Ofis telefonumu ara**
   * **Güvenlik sorularımı yanıtla**

   Bir seçenek belirleyin, doğru yanıtları sağlayın ve ardından **sonraki**.

   ![Kimlik doğrulama verilerinizi doğrulama][Verification]

4. Adım 3 farklı bir seçenek ile tekrarlamanız gerekebilir ve BT personeliniz daha fazla doğrulama gerekebilir.
5. Üzerinde **yeni bir parola seçmesi** sayfasında, yeni bir parola girin, parolanızı onaylayın ve ardından **son**. İş veya Okul parolanızı uymaları gereken belirli gereksinimleri olabilir. 8-16 karakter uzunluğunda ve büyük ve küçük harfleri, bir sayı ve bir özel karakter içeren bir parola seçin öneririz.
6. İleti gördüğünüzde **parolanızı sıfırlama**, yeni parolanızla oturum açabilirsiniz.

    ![Parolanız sıfırlandı][Complete]

Artık, hesabınıza erişmeniz mümkün olması gerekir. Hesabınıza erişemiyor, kuruluşunuzun başvurmalıdır daha fazla yardım için BT personeli.

Bir hesap gibi geldiği bir onay e-postası "Microsoft adına \<kuruluşunuz >." Bunun gibi bir e-posta almak ve Self Servis parola sıfırlama hesabınız erişimi yeniden kazanmak için kullanmadıysanız, kuruluşunuzun kişi BT personeli.

## <a name="change-my-password"></a>Parolamı değiştirme

Parolanızı zaten biliyor ve değiştirmek istiyorsanız, aşağıdaki adımları kullanın.

### <a name="change-your-password-from-the-office-365-portal"></a>Office 365 portalından parolanızı değiştirme

Office Portalı aracılığıyla normalde uygulamalarınızı erişim, bu yöntemi kullanın:

1. Oturum açın, [Office 365 hesabı](https://www.office.com) mevcut parolanızla.
2. Profilinizi sağ üst tarafındaki seçin ve ardından **görüntülemek hesap**.
3. Seçin **güvenlik ve gizlilik** > **parola**.
4. Eski parolanızı girin, ayarlayın ve yeni parolanızı onaylayın ve ardından **gönderme**.

### <a name="change-your-password-from-the-azure-access-panel"></a>Azure Erişim Paneli’nden parolanızı değiştirme

Normalde, uygulamalarınızın Azure erişim paneli (MyApps) erişim, bu yöntemi kullanın:

1. Oturum [Azure erişim paneli](https://myapps.microsoft.com/) mevcut parolanızla.
2. Profilinizi sağ üst tarafındaki seçin ve ardından **profil**.
3. Seçin **parola değiştirme**.
4. Eski parolanızı girin, ayarlayın ve yeni parolanızı onaylayın ve ardından **gönderme**.

## <a name="reset-password-at-sign-in"></a>Oturum açma sırasında parola sıfırlama

Yöneticiniz işlevselliği etkinleştirmişse, bağlantı şimdi görebilirsiniz **parola sıfırlama** oturum açma, Windows 10 sonbaharda oluşturucuları Güncelleştirme ekranında.

![Oturum açma ekranı][LoginScreen]

Seçin **parola sıfırlama** normal web tabanlı deneyimi erişmek oturum açmak zorunda kalmadan parolanızı sıfırlayabilirsiniz böylece SSPR deneyimi oturum açma ekranından açmak için bağlantı.

1. Kullanıcı Kimliğinizi doğrulayın ve seçin **sonraki**.
2. Seçin ve doğrulama için iletişim yöntemi onaylayın. Farklı bir seçenek ile bu adımı tekrarlamanız gerekebilir ve BT personeliniz daha fazla doğrulama gerekebilir.

   ![İletişim yöntemi][ContactMethod]

3. Üzerinde **yeni bir parola oluşturmasını** sayfasında, yeni bir parola girin, parolanızı onaylayın ve ardından **sonraki**. Parolanız 8-16 karakterden uzun olduğundan ve büyük ve küçük harfler, sayılar ve özel karakterler oluşan öneririz.

   ![Parola sıfırlama][ResetPassword]

4. İleti gördüğünüzde **parolanızı sıfırlama**seçin **son**.

Artık, hesabınıza erişmeniz mümkün olması gerekir. Aksi takdirde, kuruluşunuzun başvurun daha fazla yardım için BT personeli.

## <a name="common-problems-and-their-solutions"></a>Ortak sorunlar ve çözümleri

 Bazı ortak hata durumları ve bunların çözümleri şunlardır:

| Hata durumu| Hangi hata görürüm?| Çözüm |
| --- | --- | --- |
| Parolamı değiştirmeye çalıştığınızda hata bakın. | Ne yazık ki parolanızı bir sözcük, tümcecik veya parolanızı kolayca guessable yapar düzeni içerir. Lütfen farklı bir parola ile yeniden deneyin. | Tahmin edilmesi daha fazla difficlt olan bir parola seçin. |
| Kullanıcı Kimliğimi girdikten sonra "Lütfen sistem yöneticinize başvurun" sayfası alıyorum | Lütfen sistem yöneticinize başvurun. <br> <br> Kullanıcı hesabı parolanızı Microsoft tarafından yönetilmediğini algıladık. Sonuç olarak, otomatik olarak parolanızı sıfırlamak alamıyoruz. <br> <br> Daha fazla yardım için BT personeliniz başvurmanız gerekir. | Parolanızı, şirket içi ortamınızda, BT personeliniz yönetir çünkü bu iletiyi görüyorsunuz. "Hesabınıza erişemiyor" bağlantısından parolanızı sıfırlayamazsınız. <br> <br> Parolanızı sıfırlamak için doğrudan Yardım için BT personeliniz başvurun ve bunlar bu özellik sizin için etkinleştirebileceğiniz şekilde parolanızı sıfırlamak istediğiniz bildirmek.|
| Kullanıcı Kimliğimi girdikten sonra bir "hesabınız parola sıfırlama için etkin değil" hatası alıyorum | Hesabınız parola sıfırlama için etkinleştirilmedi. <br> <br> Üzgünüz, ancak, BT personeliniz hesabınızı bu hizmeti kullanmak için ayarlama değil. <br> <br> İsterseniz, biz parolanızı sıfırlaması için kuruluşunuzdaki yönetici başvurabilirsiniz. | BT personeliniz parola sıfırlama "hesabınıza erişemiyor" bağlantısından, kuruluşunuz için etkinleştirilmemiş veya bu özelliği kullanmak için lisanslı kurmadı çünkü bu iletiyi görüyorsunuz. <br> <br> Parolanızı sıfırlamak için "yönetici bağlantı başvurun" seçin şirketiniz için bir e-posta göndermek için BT personeli, adı ve bunlar bu özellik sizin için etkinleştirebileceğiniz şekilde parolanızı sıfırlamak istediğiniz bildirmek. |
| Kullanıcı Kimliğimi girdikten sonra bir "Biz hesabınızı doğrulanamadı" hatası alıyorum | Hesabınız doğrulanamıyor. <br> <br> İsterseniz, biz parolanızı sıfırlaması için kuruluşunuzdaki yönetici başvurabilirsiniz. | Parola sıfırlama için etkin ancak hizmeti kullanmak için kaydolmadınız çünkü bu iletiyi görüyorsunuz. Parola sıfırlama için kaydetme için hesabınıza erişim artık sonra için http://aka.ms/ssprsetup gidin. <br> <br> Parolanızı sıfırlamak için şirketinizin e-posta göndermek için "bir yöneticiye başvurun" bağlantıyı seçin BT personeli. |

## <a name="next-steps"></a>Sonraki adımlar

* [Self servis parola sıfırlamayı kullanmak için kaydolma](active-directory-passwords-reset-register.md)
* [Parola sıfırlama kayıt sayfası](http://aka.ms/ssprsetup)
* [Parola sıfırlama portalı](https://passwordreset.microsoftonline.com/)
* [Microsoft hesabınızla oturum açamıyorsunuz](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant)

[Login]: ./media/active-directory-passwords-update-your-own-password/reset-1-login.png "Oturum açma sayfası Hesabınıza erişemiyor musunuz?"
[Verification]: ./media/active-directory-passwords-update-your-own-password/reset-2-verification.png "Kimlik doğrulama verilerinizi doğrulayın"
[Change]: ./media/active-directory-passwords-update-your-own-password/reset-3-change.png "Parolanızı değiştirme"
[Complete]: ./media/active-directory-passwords-update-your-own-password/reset-4-complete.png "Parolanız sıfırlandı"
[LoginScreen]: ./media/active-directory-passwords-update-your-own-password/login-screen.png "Windows 10 sonbaharda oluşturucuları güncelleştirme oturum açma ekranı sıfırlama parola bağlantısı"
[ContactMethod]: ./media/active-directory-passwords-update-your-own-password/reset-contact-method-screen.png "Kimlik doğrulama verilerinizi doğrulayın"
[ResetPassword]: ./media/active-directory-passwords-update-your-own-password/reset-password-screen.png "Parolanızı değiştirme"

