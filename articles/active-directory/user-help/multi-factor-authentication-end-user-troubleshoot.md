---
title: İki aşamalı doğrulama - Azure AD sorunlarını giderme | Microsoft Docs
description: Bu belge kullanıcılar, Azure multi-Factor Authentication ile ilgili bir sorun yaşarsanız yapmanız gerekenler hakkında bilgi sağlar.
services: multi-factor-authentication
keywords: çok faktörlü kimlik doğrulama istemcisi, kimlik doğrulama sorunu, bağıntı kimliği
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.assetid: 8f3aef42-7f66-4656-a7cd-d25a971cb9eb
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/06/2017
ms.author: lizross
ms.reviewer: richagi
ms.custom: end-user
ms.openlocfilehash: deb75c2601fa55f7cdb1681d8f73e94d6b01310a
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39060078"
---
# <a name="get-help-with-two-step-verification"></a>İki aşamalı doğrulama konusunda yardım alın
Bu makalede, iki aşamalı doğrulama hakkında istemem en yaygın sorular yanıtlanmaktadır.

## <a name="why-do-i-have-to-perform-two-step-verification-can-i-turn-it-off"></a>İki aşamalı doğrulamayı gerçekleştirmek neden gerekiyor? Ben bunu kapatabilir miyim?

İki aşamalı doğrulama, kuruluşunuzun hesaplarınızı korumak üzere kullanmak için seçtiğiniz bir güvenlik özelliğidir. İki formları kimlik doğrulama kullandığından yalnızca bir paroladan daha güvenli: bildiğiniz bir şey ve bir şey, sahip olursunuz. Bildiğiniz bir şey, paroladır. İle kullandığınız telefon veya aygıt, yaygın olarak olan şeydir. Hesabınız iki aşamalı doğrulama ile korunuyorsa, bu, telefonunuza çok erişimleri yoktur çünkü bunlar parolanızı aşağıdaki şekilde alırsanız, kötü amaçlı bir korsana oturum açılamıyor, anlamına gelir.

Microsoft, iki aşamalı doğrulamayı sunar, ancak bu özelliği kullanmak, kuruluşunuzun seçer. Yalnızca hesabınızı korumak için bir parola kullanarak dışı bırakamazlar gibi şirketinizin destek birimi, bunu gerektiriyorsa, çıkma olamaz.

İki aşamalı doğrulama, kişisel bir Microsoft hesabınız için açık olan ve istiyorsanız ayarlarınızı değiştirmek için okuma [iki basamaklı doğrulama hakkında](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) yerine.

## <a name="i-dont-have-my-phone-with-me-today"></a>Ben telefonumu benimle bugün yok

İşte oturum açmak telefonunuza evde, ancak yine de bırakın bazı gün gerekir. Denemeniz gereken ilk şey, farklı bir doğrulama yöntemi ile oturum açar. İki aşamalı doğrulama için kaydettiğinizde, birden fazla telefon numarası ayarlamak mı? Farklı bir yöntem ile oturum açmayı deneyin için bu adımları izleyin:

1. Normalde yaptığınız gibi oturum açın.
2. İki aşamalı doğrulama sayfası açıldığında seçin **farklı bir doğrulama seçeneği kullanma**.

   ![Farklı bir kimlik doğrulama](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)

3. Kullanmak istediğiniz doğrulama seçeneğini belirleyin.
4. İki aşamalı doğrulama ile devam edin.

Görmüyorsanız **farklı bir doğrulama seçeneği kullanma** bağlantı olmadı ayarladığınız alternatif yöntemleri için iki aşamalı doğrulamayı ilk kez kaydolurken anlamına gelir. Hesabınızda oturum açma konusunda yardım almak için şirketinizin Destek birimine başvurun. Siz açtıktan sonra emin olun [ayarlarınızı yönetmek](multi-factor-authentication-end-user-manage-settings.md) sonraki açışlarında ek doğrulama yöntemleri eklemek için.

Görürseniz **farklı bir doğrulama seçeneği kullanma** bağlantı, ancak yoksa erişiminiz alternatif yöntemlerinizi iki, kişi hesabınızda oturum açma konusunda yardım almak için şirketinizin destek.

## <a name="i-lost-my-phone-or-got-a-new-number"></a>Telefonumu kayıp veya yeni bir sayı alındı
, Hesabınıza geri almanın iki yolu vardır. Bir ayarlamış olduğunuz, alternatif kimlik doğrulama telefon numaranızı kullanarak oturum açmanız davranıştır. Ayarlarınızı temizlemek için şirketinizin destek istemek için kullanılan saniyedir.

Telefonunuz kaybolur veya çalınırsa, ayrıca, uygulama parolaları sıfırlama ve temizleyin destek hatırlanan cihazlar şirketinizin bilgi öneririz.

### <a name="use-an-alternate-phone-number"></a>Alternatif bir telefon numarası kullanın
İkincil bir telefon numarası ya da farklı bir cihaz üzerinde bir kimlik doğrulayıcı uygulamasını dahil olmak üzere birden çok doğrulama seçenekleri ayarladıysanız, oturum açmak için bunlardan birini kullanabilirsiniz.

Alternatif bir telefon numarası kullanarak oturum açmak için şu adımları izleyin:

1. Normalde yaptığınız gibi oturum açın.
2. Daha fazla hesabınızı doğrulamak için istendiğinde, **farklı bir doğrulama seçeneği kullanma**.

   ![Farklı bir kimlik doğrulama](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)

3. Telefon numarası veya erişimi olan bir cihaz seçin.
4. Sonra hesabınızdaki geri işiniz [ayarlarınızı yönetmek](multi-factor-authentication-end-user-manage-settings.md) kimlik doğrulama telefon numaranızı değiştirmek için.

### <a name="clear-your-settings"></a>Ayarlarınızı Temizle
İkincil kimlik doğrulama telefon numarasını yapılandırmadıysanız, Yardım için şirketinizin Destek birimine sahip. Clear sahip ayarlarınızı değiştirene kadar oturum açın, size istenir [kaydetmek için iki aşamalı doğrulamayı](multi-factor-authentication-end-user-first-time.md) yeniden.

## <a name="i-am-not-receiving-a-text-or-call-on-my-phone"></a>Bir metin almıyorum veya üzerinde telefonumu Ara
Neden oturum açın, ancak bir metin ya da telefon aramasına almamayı deneyebilirsiniz birkaç nedeni vardır. Başarıyla metinleri veya telefon çağrıları telefonunuza geçmişte aldığınız, ardından bunun değil hesabınıza telefon sağlayıcısı ile ilgili bir sorun olabilir. İyi hücre sinyaliniz ve almaya çalışıyorsanız, kısa mesaj metin iletileri almasına mümkün olduğundan emin olun emin olun. Bir arkadaşınıza veya kısa mesaj çağırmak için sorun, bir test olarak.

Bir metin veya çağrı için birkaç dakika süre bekledi, farklı bir seçenek hesabınıza en hızlı yolu denemektir.

1. Seçin **farklı bir doğrulama seçeneği kullanma** sayfasında, doğrulama için bekliyor.

    ![Farklı bir kimlik doğrulama](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)
2. Kullanmak istediğiniz telefon numarası veya teslim yöntemini seçin.

    Birden çok doğrulama kodları aldıysanız, yeni bir tane kullanın.

Yapılandırılmış başka bir yöntem yoksa, şirketinizin Destek birimine ve ayarlarınızı temizlemek için isteyin. Oturum, sonraki açışınızda, istenir [çok faktörlü kimlik doğrulamasını ayarlama](multi-factor-authentication-end-user-first-time.md) yeniden.

Genellikle bozuk hücre sinyali nedeniyle gecikmeler varsa, kullanmanızı öneririz [Microsoft Authenticator uygulamasını](microsoft-authenticator-app-how-to.md) telefonunuzu üzerinde. Uygulama oturum açmak için kullandığınız rastgele güvenlik kodları oluşturabilirsiniz ve bu kodları herhangi bir hücreyi sinyali veya internet bağlantısı gerekmez.

## <a name="app-passwords-are-not-working"></a>Uygulama parolaları çalışmıyor
İlk olarak, uygulama parolası doğru girdiğinizden emin olun. İki aşamalı doğrulamayı desteklemeyen ancak yalnızca eski Masaüstü uygulamaları için normal parolanızı oluşturulan uygulama parolasını değiştirir. Oturum açma hala çalışmıyorsa, deneyin ve [yeni bir uygulama parolası oluşturmanız](multi-factor-authentication-end-user-app-passwords.md).  Bu adımlar da işe yaramazsa, şirketinizin Destek birimine ve bunları [, var olan uygulama parolalarını Sil](../authentication/howto-mfa-userdevicesettings.md) ve ardından yeni bir tane oluşturabilirsiniz.

## <a name="i-didnt-find-an-answer-to-my-problem"></a>Ben sorunumu bir yanıt bulamadık.
Bu sorun giderme adımlarını çalıştınız, ancak hala sorunlarla karşılaşırsanız çalıştıran, şirketinizin Destek birimine başvurun. Size yardımcı olması çözebilmeleri.

## <a name="related-topics"></a>İlgili konular
* [İki adımlı doğrulama ayarlarınızı yönetme](multi-factor-authentication-end-user-manage-settings.md)  
* [Microsoft Authenticator uygulaması hakkında SSS](microsoft-authenticator-app-faq.md)
