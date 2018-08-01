---
title: Güvenlik bilgilerinizi - Azure Active Directory yönetme | Microsoft Docs
description: İki adımlı doğrulama ayarlarınızı ile çalışmaya nasıl dahil olmak üzere güvenlik bilgilerinizi yönetmeyi öğrenin.
services: active-directory
author: eross-msft
manager: mtillman
ms.reviewer: sahenry
ms.service: active-directory
ms.component: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: lizross
ms.openlocfilehash: abd2984574f80f03f276861782ff9ee51348d07e
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39391386"
---
# <a name="manage-your-security-info-preview"></a>Güvenlik bilgilerinizi (Önizleme) yönetme

[!INCLUDE[preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

İş veya Okul hesabınızda oturum açın veya parolanızı sıfırlamak için güvenlik bilgilerinizi kullanabilirsiniz.

Kuruluşunuzun ayarlara bağlı olarak, oturum açtığınızda ifadesini onay kutusu görebilirsiniz **X için bir daha sorma gün**. Sürenizin bu onay kutusunu sağlar, cihazınıza reverification gerek kalmadan yöneticinizin tanır gün sayısı için oturum açmış.

## <a name="change-your-info"></a>Bilgilerinizi değiştirin
Güncelleştirme veya güvenlik bilgisi ekleyin veya yöneticiniz ve kuruluşunuz tarafından izin verilenden üzerinde göre varsayılan olarak değiştirin.

### <a name="to-change-your-info"></a>Bilgilerinizi değiştirmek için

1. İş veya Okul hesabınızda oturum açın.

2. Myapps.microsoft.com için sayfanın sağ üst köşesinden adınızı seçin ve ardından Git **profili**.

3. İçinde **hesabını yönetme** alanında **güvenlik bilgilerini Düzenle**.

    ![Profil düzenleme güvenlik bilgisi bağlantısının vurgulandığı ekran](media/security-info/security-info-profile.png)

4. Yöneticiniz kuruluşunuz için bu deneyim ayarladıysanız, varsayılan yönteminizi erişimi onaylama ve, geçerli güvenlik bilgisi ayrıntıları görmek için kullanın.

5. Üzerinde **hesabınızı güvenli tutmak** sayfasında, şunları yapabilirsiniz:

    - Seçin **güvenlik bilgisi ekleyin** ek yöntemler ekleme.

    - Seçin **Varsayılanı Değiştir** varsayılan yönteminizi değiştirmek için.

    - Seçin **kalem** bilgilerinizi güncelleştirmek için varolan bir yöntem yanındaki simge.

    ![Güvenlik bilgileri mevcut, düzenlenebilir bilgisi ekranla](media/security-info/security-info-edit.png)

6. Değişikliklerinizi yaptıktan sonra sayfadan ve değişikliklerinizi kaydedilir.

Bu seçenekleri göremiyorsanız veya myapps.microsoft.com sayfanın erişemiyor, kuruluşunuzun özel seçenekleri veya özel bir sayfa kullandığı mümkündür. Daha fazla yardım için yöneticinize başvurmanız gerekir.

## <a name="manage-your-security-info-for-a-lost-or-potentially-compromised-device"></a>Kayıp veya riskli olabilecek bir cihaz için güvenlik bilgilerinizi yönetin

Cihazınızı kaybeder veya Cihazınızı güvenliği tehlikeye girdiğinde, tüm cihazlarınızı önceden güvenilen için doğrulama işlemi gerçekleştirmeyi gerekir.

### <a name="to-manage-your-security-info-for-lost-or-potentially-compromised-devices"></a>Kayıp veya riskli olabilecek cihazlar için güvenlik bilgilerinizi yönetmek için

1. İş veya Okul hesabınızda oturum açın.

2. Myapps.microsoft.com için sayfanın sağ üst köşede adınızı seçin ve ardından Git **profili**.

3. İçinde **hesabını yönetme** alanında **unutursanız MFA hatırlanan cihazlarda**.
    
    Oturum açtıktan sonra multi-Factor Authentication işleminde yeniden gitmek zorunda kalırsınız bu seçeneğin belirlenmesiyle.

    ![Profil ekranı Unut bağlantısının vurgulandığı ile](media/security-info/security-info-forget.png)

## <a name="common-problems-and-solutions-with-your-security-info"></a>Genel sorunlar ve çözümleri ile güvenlik bilgileriniz

Bu makalede, iki aşamalı doğrulama ile ilgili sorunlar da dahil olmak üzere güvenlik bilgilerinizi gidermenize yardımcı olur.

|Sorun|Çözüm|
|-------|--------|
|Ben telefonumu benimle yok|Oturum açmak için iş veya Okul hesabına onun olası yoksa, telefonunuzun yanınızda her zaman, ancak siz devam istersiniz. Bu sorunu gidermek için e-posta adresinizi veya iş telefonu numaranızı gibi telefonunuzu gerektirmeyen farklı kimlik doğrulama yöntemi kullanarak oturum açabilirsiniz. Güvenlik bilgilerinize ek yöntemleri eklemek için adımları izleyebilirsiniz [bilgilerinizi değiştirmek](#change-your-info) bölümü.|
|Ben telefonumu kaybolur veya çalınırsa|Ne yazık ki, telefonunuzu veya çalınmasını kaybı oluşabilir. Bu durumda, kuruluşunuzun BT ekibiniz, uygulama parolalarını sıfırlayabilir ve tüm güvenilen cihazları listenizden cihazları temizleyin anımsanacak bilmek istiyorum önemle tavsiye edilir. İçindeki adımları izleyerek kendi güvenilen cihazları unutur [kayıp veya riskli olabilecek bir cihaz için güvenlik bilgilerinizi yönetmek](#manage-your-security-info-for-a-lost-or-potentially-compromised-device) bölümü.|
|Yeni bir telefon numarası aldım|Bu sorunu çözmek için iki yolu vardır. Telefon numarası, e-posta gibi gerektirmeyen bir alternatif kimlik doğrulama yöntemi kullanarak oturum açın veya bu bir seçenek değilse, kuruluşunuzun başvurabilirsiniz BT personeli kullanıcının ve bunları ayarlarınızı temizleyin. Güvenlik bilgilerinize ek yöntemleri eklemek için adımları izleyebilirsiniz [bilgilerinizi değiştirmek](#change-your-info) bölümü.|
|Varsayılan yöntemimi yanlış|Varsayılan yönteminizi güvenlik seçeneklerinizi güncelleştirebilirsiniz. Belirli Ayrıntılar için giderek [bilgilerinizi değiştirmek](#change-your-info) bölümü.|
|Bir metin almıyorum veya mobil aygıtımda çağırın|Başarıyla metinleri veya telefon çağrıları mobil cihazınıza geçmişte aldığınız, bu sorunu telefon sağlayıcısı, hesabı değil, olasılıkla olur. İyi hücre sinyal sahip olduğunuzu ve kısa mesaj ve telefon çağrılarını almak mümkün emin olun. Çağrı veya kısa mesaj arkadaşı sorabilir, bir test olarak.<br><br>Metin ve telefon iletileri başarıyla alabilir, ancak yine de bildirim edinmiş yapmadıysanız, farklı bir yöntemle deneyebilirsiniz. İçindeki adımları izleyerek güvenlik bilgilerinizi ek yöntemler ekleyebilirsiniz [bilgilerinizi değiştirmek](#change-your-info) bölümü. Eklemek için başka bir yöntem yoksa, şirketinizin Destek birimine ve böylece bir sonraki oturum açışınızda, yöntemleri ayarlayabilirsiniz ayarlarınızı temizlemek için isteyin.<br><br>Hatalı hücre alımı nedeniyle genellikle sorunlarla karşılaşırsanız, mobil Cihazınızda Microsoft Authenticator uygulamasını kullanmanızı öneririz. Uygulama oturum açmak için kullandığınız rastgele güvenlik kodları oluşturabilirsiniz ve bu kodları herhangi bir hücreyi sinyali veya internet bağlantısı gerekmez. Microsoft Authenticator uygulaması hakkında daha fazla bilgi için bkz. [Microsoft Authenticator uygulaması ile çalışmaya başlama](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/microsoft-authenticator-app-how-to) makalesi.|
|Bu tabloda seçeneklerin hiçbiri, sorunu çözüldü|Bu sorun giderme adımlarını denemenize rağmen sorunlarla karşılaşırsanız çalışmakta olan Şirketinizin Destek birimine, size yardımcı olması gerekir.|

## <a name="next-steps"></a>Sonraki adımlar

- Güvenlik bilgileri hakkında daha fazla bilgi [güvenlik bilgisi (Önizleme) genel bakış](user-help-security-info-overview.md).

- İki aşamalı doğrulamayı öğrenin [iki aşamalı doğrulama genel bakış](user-help-two-step-verification-overview.md) makalesi. 

- Güvenlik bilgileri alanında, cihazlarınızın ayarlama hakkında bilgi edinmek için bu nasıl yapılır makaleleri birini izleyin:

    - [Güvenlik bilgileri ' authenticator uygulamasını kullanmak için ayarlama](security-info-setup-auth-app.md)

    - [Güvenlik bilgilerini, telefon aramaları kullanacak şekilde](security-info-setup-phone-number.md)

    - [Güvenlik bilgilerini, kısa mesaj kullanacak şekilde](security-info-setup-text-msg.md)

    - [Güvenlik bilgileri e-posta kullanacak şekilde](security-info-setup-email.md)

    - [Güvenlik sorularını kullanmak için güvenlik bilgileri ' ayarlayın](security-info-setup-questions.md)

- Kaybolur veya, gelen unutulabilir, parolanızı sıfırlayamıyoruz [parola sıfırlama portalı](https://passwordreset.microsoftonline.com/) veya adımları [iş veya Okul parolanızı sıfırlama](user-help-reset-password.md) makalesi.

- İpuçları ve Yardım almak için oturum açma sorunlarını giderme alma [Microsoft hesabınızda oturum açamazsınız](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) makalesi.