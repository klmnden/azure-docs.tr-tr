---
title: İki aşamalı doğrulama - Azure Active Directory sorunlarını giderme | Microsoft Docs
description: Kullanıcılara Azure multi-Factor Authentication ve iki aşamalı doğrulama ile ilgili bir sorunla çalıştırırsanız yapmanız gerekenler hakkında yönergeler sağlar.
services: active-directory
author: eross-msft
manager: daveba
ms.assetid: 8f3aef42-7f66-4656-a7cd-d25a971cb9eb
ms.workload: identity
ms.service: active-directory
ms.subservice: user-help
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: lizross
ms.reviewer: kexia
ms.collection: M365-identity-device-management
ms.openlocfilehash: b74049833b055caa112c346b74798893f2c0febf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60476671"
---
# <a name="get-help-with-two-step-verification"></a>İki aşamalı doğrulama konusunda yardım alın

İki aşamalı doğrulama, kuruluşunuzun hesaplarınızı korumak için kullandığı bir güvenlik özelliğidir. İki aşamalı doğrulama bildiğiniz ve sahip olduğunuz bir şey olmak üzere iki kimlik doğrulaması biçimini kullandığı için yalnızca parola kullanmaktan daha güvenlidir. Sizinle sahip olduğunuz şey telefonunuz veya bir cihaz ederken bildiğiniz parolanızı bir şeydir. İki aşamalı doğrulamayı kullanmaya parolanızı aldıkları olsa bile, oturum kötü amaçlı bir bilgisayar korsanlarının durdurmak için yardımcı olabilir.

Microsoft, iki aşamalı doğrulama sunarken, özelliği kullanıp kullanmadığını belirler, kuruluşunuzun var. Yalnızca hesabınızı korumak için bir parola kullanarak dışı bırakamazlar gibi kuruluşunuz, gerektiriyorsa, çıkma olamaz.

>[!Note]
>Kişisel Microsoft hesabınızla iki aşamalı doğrulamayı kullanma hakkında daha fazla bilgi arıyorsanız bkz [iki basamaklı doğrulama hakkında](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) makalesi.

## <a name="why-do-i-need-to-use-another-verification-method"></a>Başka bir doğrulama yöntemi kullanmak neden ihtiyacım var?

Neden alternatif bir doğrulama yöntemi, hesabınızla birlikte kullanılacak gerekebilir birkaç nedeni vardır. Örneğin:

- **Telefon veya aygıt unuttunuz demektir.** İşte oturum açmak telefonunuza evde, ancak yine de bırakın bazı gün gerekir. İlk olarak, telefonunuzu gerekli olmayan farklı bir yöntemi kullanarak imzalama denemelisiniz.

- **Telefonunuz kaybolur veya yeni bir telefon numarası alındı.** Telefonunuz kaybolur veya yeni bir sayı değil, farklı bir yöntemi kullanarak oturum açın veya ayarlarınızı temizlemek için yöneticinize başvurun. Telefonunuz kaybolur veya çalınırsa, bu nedenle uygun güncelleştirmeleri hesabınıza sağlanabilir bilmeniz, yöneticiniz izin vererek kesinlikle öneririz. Ayarlarınızı temizlendikten sonra açmanız istenir [kaydetmek için iki aşamalı doğrulamayı](multi-factor-authentication-end-user-first-time.md) bir sonraki oturum açışınızda.

- **Kimlik doğrulaması metin ya da telefon aramasına alamıyor.** Neden, metin ya da telefon aramasına alamayabilirsiniz birkaç nedeni vardır. Geçmiş metinlerinizi veya telefon çağrıları başarıyla yönettiniz ise bunun değil hesabınıza telefon sağlayıcısı ile ilgili bir sorun olabilir. Genellikle hatalı bir sinyal nedeniyle gecikmeler varsa, kullanmanızı öneririz [Microsoft Authenticator uygulamasını](user-help-auth-app-download-install.md) mobil Cihazınızda. Bu uygulama, herhangi bir hücreyi sinyali veya Internet bağlantısı gerektirmeden oturum açma için rastgele güvenlik kodları oluşturabilirsiniz.<br><br>SMS mesajı almaya çalıştığınız metin arkadaşı en güncel bir kod edinin ve çeşitli iletiler gönderildiyse emin emin olmak için bir test olarak kullandığınız isteyin.

- **Uygulama parolalarınızı çalışmıyor.** Uygulama parolaları için iki aşamalı doğrulamayı desteklemeyen eski masaüstü uygulamalarının normal parolanızı değiştirin. İlk olarak, doğru parolayı girdiğinizden emin olun. Bunu düzeltelim değil, oturum açmayı deneyin [yeni bir uygulama parolası oluşturmanız](multi-factor-authentication-end-user-app-passwords.md) veya yöneticinize başvurarak [, var olan uygulama parolalarını Sil](../authentication/howto-mfa-userdevicesettings.md) bu nedenle yeni bir tane oluşturabilirsiniz.

## <a name="sign-in-using-another-verification-method"></a>Başka bir doğrulama yöntemi kullanarak oturum açın

1. hesabınızda normal şekilde oturum açın ve seçin **başka bir yöntemle oturum** bağlantısını **iki aşamalı doğrulama** sayfası.

    ![Oturum açma doğrulama yöntemini değiştirme](./media/multi-factor-authentication-end-user-troubleshoot/two-factor-auth-signin-another-way.png)

    >[!Note]
    >Görmüyorsanız **başka bir yöntemle oturum** bağlantı geldiğini herhangi diğer doğrulama yöntemlerini ayarlamasını yapmadığınızı fark. Hesabınızda oturum Yardım için yöneticinize başvurmanız gerekir. Oturum açtıktan sonra ek doğrulama yöntemlerini eklediğinizden emin olun. Doğrulama yöntemleri ekleme hakkında daha fazla bilgi için bkz. [iki adımlı doğrulama ayarlarınızı yönetme](multi-factor-authentication-end-user-manage-settings.md) makalesi.<br><br>Bağlantıya bakın, ancak yine de diğer doğrulama yöntemlerini görmüyor, hesabınızda oturum açarken yardım için yöneticinize başvurun zorunda kalırsınız.

2. Alternatif doğrulama yönteminizi seçin ve iki aşamalı doğrulama işlemine devam.

3. Hesabınızı geri işiniz sonra (gerekirse) doğrulama yöntemlerinizi güncelleştirebilirsiniz. Daha fazla bilgi için eklemek veya yöntemlerinizi değiştirilmesi, [iki adımlı doğrulama ayarlarınızı yönetme](multi-factor-authentication-end-user-manage-settings.md) makalesi.

## <a name="i-didnt-find-an-answer-to-my-problem"></a>Sorunumu yanıt bulun gelmedi

Bu adımları denemenize rağmen sorunlarla karşılaşırsanız çalışmakta olan, daha fazla yardım için yöneticinize başvurun.

## <a name="related-topics"></a>İlgili konular

* [İki adımlı doğrulama ayarlarınızı yönetme](multi-factor-authentication-end-user-manage-settings.md)

* [Microsoft Authenticator uygulaması hakkında SSS](user-help-auth-app-faq.md)
