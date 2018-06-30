---
title: Microsoft Kimlik Doğrulayıcı uygulamayla - Azure AD Yardım | Microsoft Docs
description: Sık sorulan sorular ve yanıtlar Microsoft Authentication uygulamasını ve Azure multi-Factor Authentication ile ilgili bir listesini sağlar.
services: multi-factor-authentication
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.assetid: f04d5bce-e99e-4f75-82d1-ef6369be3402
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/08/2018
ms.author: lizross
ms.reviewer: librown
ms.custom: end-user
ms.openlocfilehash: 9027b09c186dfb7661fc63f200f4d2e5da96a70a
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37130110"
---
# <a name="microsoft-authenticator-app-faq"></a>Microsoft Authenticator uygulaması hakkında SSS

Bu makalede, Microsoft Authenticator uygulaması hakkında sık sorulan sorular yanıtlanmaktadır. Sorunuzun yanıtını görmüyorsanız, Git [Microsoft Authenticator uygulama Forumu](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp). Ayrıca, bir uygulamada belirli bir özellikle ilgili başka bir SSS gözden geçirebilirsiniz [oturum telefonunuz SSS oturum](microsoft-authenticator-app-phone-signin-faq.md).

Microsoft Authenticator uygulamasını Azure Authenticator uygulamasını değiştirildi ve Azure çok faktörlü kimlik doğrulaması kullandığınızda önerilen uygulamadır. Microsoft Authenticator uygulaması [Android](https://go.microsoft.com/fwlink/?linkid=866594), [iOS](https://go.microsoft.com/fwlink/?linkid=866594), ve [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071).

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="what-data-does-the-authenticator-store-on-my-behalf-and-how-can-i-delete-it"></a>Hangi verilerin Doğrulayıcı benim yerime depoluyor mu ve nasıl ı silebilmek için?

Microsoft Authenticator bir hesap eklediğinizde, oluşturduğunuz hesap bilgileri depolar. Doğrulayıcı kullandığınızda, tanılama günlük hata ayıklama amacıyla oluşturulur ve Microsoft öngörülemeyen sorunları tanılamanıza yardımcı yararlı verileri depolar. Günlük verileri açarak erişebilir **yardımcı** > **günlükleri Gönder** > **görüntülemek günlükleri**.

Hesap kutucuğuna silerek verileri silebilirsiniz. Hesabı döşeme silme günlükleri de dahil olmak üzere uygulama tarafından kullanılan tüm hesap bilgileri siler. 

Microsoft, verilerinizin nasıl kullandığı hakkında daha fazla bilgi için ziyaret edin: https://servicetrust.microsoft.com/ViewPage/PrivacyGettingStarted

### <a name="what-are-the-codes-in-the-app-for-why-does-the-number-keep-counting-down"></a>Uygulama için kodlarında nelerdir? Neden sayım numarası koru?

Microsoft Authenticator uygulamasını açtığınızda, eklediğiniz hesapları ve altı veya sekiz basamaklı bir sayı her birine bakın. Sayım 30 saniyelik süreölçer görebilirsiniz.

Hesabınızda oturum açtığınızda bu kodları kullanılır. Kullanıcı adınızı ve parolanızı girdikten sonra doğrulama kodunu girmeniz istenebilir. Microsoft Authenticator uygulamasını açın ve şu anda gösteren kodu kopyalayın. Bu kodu tamamlamak için oturum açma sayfasındaki girin.

Her 30 saniyede kodlarını değiştirmek nedeni, böylelikle, hiçbir zaman aynı kodu iki kez kullanın. Anımsaması beklenen bir parola gibi değil. Yalnızca birisi erişim telefonunuza doğrulama kodunuzu bilir olur.

Oturum açmak için telefon hizmeti sahip hakkında endişelenmeye gerek kalmadan kodları Internet veya verileri gerekmez. Uygulamasını kapatın, arka planda çalışmaya devam etmez ve pil tüketir değil. Uygulamayı kapatın ve oturum zamana kadar yok sayın.  

### <a name="i-only-get-notifications-when-i-have-the-app-open-if-the-app-isnt-open-i-dont-get-any-notifications"></a>Uygulamaya sahip olduğunda yalnızca bildirimleri açık alıyorum. Herhangi bir bildirim, uygulama açık değilse, elde etmezsiniz.

Bildirimleri almak, ancak ses çıkarıyor yok veya üzerinde olan, ses açma/kapama rağmen Titret, ilk uygulama ayarlarını kontrol edin. Ses kullanabilir veya kendi bildirimlerle Titret sağlamak.

Bildirimleri hiç alamazsanız, aşağıdaki durumlarda denetleyin:

- Telefonunuz rahatsız etmeyin veya sessiz modda mı? Bu mod bildirimleri gönderme uygulamaları kullanmaya devam edebilir.
- Diğer uygulamalardan bildirimleri alabilir miyim? Aksi durumda, telefonunuza veya Android veya Apple bildirimleri kanaldan ağ bağlantıları ile ilgili bir sorun olabilir. İlk seçenek, telefon ayarlarınızı ele alabilir, ancak ikinci seçeneği ile ilgili Yardım için hizmet sağlayıcınıza konuşun gerekebilir.
- Uygulama, ancak diğer bazı hesapları için bildirimleri alabilir miyim? Yanıt Evet ise, uygulamanızdan sorunlu hesap kaldırın ve yeniden anında iletme bildirimleri etkinleştirmek için ekleyin.

Bu sorun giderme önerileri denedi ancak hala sorunlarınız varsa, tanılama günlüklerini gönderebilirsiniz. Uygulama Ayarları'na gidin ve ardından **Yardım ve geri bildirim** ve **günlüklerini gönderme**. Ardından, Git [Microsoft Authenticator uygulama Forumu](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp) ve ne görmesini sorun ve hangi adımları şu ana kadar çalıştınız bize bildirin.

### <a name="im-already-using-the-microsoft-authenticator-application-for-verification-codes-how-do-i-switch-to-one-click-push-notifications"></a>Zaten Microsoft Authenticator uygulama doğrulama kodları için kullanıyorum. Tek tıklamayla anında iletme bildirimleri nasıl geçiş yapabilirim?
Bir oturum açma anında iletme bildirimi aracılığıyla onaylama yalnızca kişisel Microsoft hesapları için kullanılabilir, veya iş ve Okul Google veya Facebook gibi üçüncü taraf hesapları için değil, Microsoft hesapları. Bir iş veya Okul Microsoft hesabınız varsa, bu seçenek devre dışı bırakmak, kuruluşunuzun seçebilirsiniz.

Kişisel hesabınız için bir Microsoft hesabı kullanın ve anında iletme bildirimleri geçmek istiyorsanız hesabınızı yeniden eklemeniz gerekir. Hesabınıza yeniden kaydedilecek ve anında iletme bildirimlerini ayarlama.  

Microsoft Authenticator kullanmak için iş veya Okul hesabı varsa, kuruluşunuzun tek tıklatmayla bildirimleri izin verilip verilmeyeceğini belirler.

### <a name="do-one-click-push-notifications-work-for-non-microsoft-accounts"></a>Tek tıklamayla anında iletme bildirimleri için Microsoft olmayan hesapların çalışıyor mu?
Hayır, anında iletme bildirimleri yalnızca Microsoft hesapları ve Azure Active Directory hesaplarıyla çalışır. İşiniz veya okulunuz Azure AD hesapları kullanıyorsa, bu özellik devre dışı bırakabilir.  

### <a name="i-got-a-new-device-or-restored-my-device-from-a-backup-how-do-i-set-up-my-accounts-in-the-microsoft-authenticator-app-again"></a>Yeni bir cihaz var, veya cihazımı bir yedekten geri bildirimi. Nasıl my hesaplarında Microsoft Authenticator uygulamasını yeniden ayarlayabilirim?
İOS cihazı kullanıyorsanız, açık olan **otomatik yedekleme**ve hesaplarınızı yedeğini eski aygıtınızda; oluşturduysanız yeni aygıtınızda hesabı kimlik bilgilerinizi kurtarmak için yedekleme kullanabilirsiniz. Daha fazla bilgi için bkz: [yedekleme ve kurtarma hesap kimlik bilgilerini Microsoft Authenticator uygulamasıyla](../../../../multi-factor-authentication/end-user/microsoft-authenticator-app-backup-and-recovery.md) makalesi. 

### <a name="i-lost-my-device-or-moved-on-to-a-new-device-how-do-i-make-sure-notifications-dont-continue-to-go-to-my-old-device"></a>I cihazımı kaybolabilir veya yeni bir cihaz taşındı. Bildirimleri eski cihazımı gitmek devam yok nasıl emin?  
Yeni iOS Cihazınızı Microsoft Authenticator uygulamasını ekleme otomatik olarak uygulamanın eski aygıtınızdan kaldırılması anlamına gelmez. Uygulama eski aygıtınızdan bile silme yeterli değildir. Hem uygulama eski cihazınızdan silin ve Microsoft veya kuruluşunuzun eski aygıt unutursanız ve hesabınızdan kaldırılamadı söyleyin.
- **Kişisel bir Microsoft hesabı kullanarak bir aygıttan uygulamayı kaldırmak için.** İki aşamalı doğrulama alanına gidin, [hesabı güvenlik](https://account.microsoft.com/security) sayfasında ve eski cihazınız için doğrulama devre dışı bırakmak seçin.  
- **Microsoft hesabı okul veya iş kullanarak bir aygıttan uygulamayı kaldırmak için.** İki aşamalı doğrulama alanına gidin, [MyApps](https://myapps.microsoft.com/) sayfa veya kuruluşunuzun özel portal ve eski cihazınız için doğrulama devre dışı bırakmak seçin. 



### <a name="how-do-i-remove-an-account-from-the-app"></a>Uygulamadan nasıl bir hesap kaldırılsın mı?
* iOS: ana ekranından sağdan sola bir hesap kutucuğuna. **Sil**’i seçin.
* Windows Phone: ana ekranından menü düğmesi seçip **Düzenle hesapları**. Dokunun **X** hesap adının yanındaki.
* Android: ana ekranından menü düğmesi seçip **Düzenle hesapları**. Dokunun **X** hesap adının yanındaki.

Kuruluşunuz ile kayıtlı bir cihaz varsa, hesabınızı kaldırmayı fazladan bir adımı tamamlamak gerekebilir. Bu aygıtlar üzerinde Microsoft Authenticator uygulamasını otomatik olarak bir cihaz Yöneticisi olarak kaydedilir. Uygulama tamamen kaldırmak istiyorsanız, önce uygulama ayarları'nda uygulama kaydı gerekir.

### <a name="why-does-the-app-request-so-many-permissions"></a>Uygulama çok fazla sayıda izinler neden istek mu?
İşte tam bir listesi için sorulan izinleri ve uygulamada nasıl kullanılır. Gördüğünüz özel izinler elinizde telefon türüne bağlıdır.

* **Kamera**: bir iş, okul veya Microsoft dışı hesabı eklediğinizde, QR kodlarını tarama için kullanılır.
* **Kişiler ve telefon**: Kişisel Microsoft hesabınızla oturum açtığınızda telefonunuzda var olan hesapları bulma işlemini basitleştirmek için kullanılır.
* **SMS**: telefon numaranızı kayıt numarası eşleştiğinden emin olmak için kullanılır. Ne zaman ilk olarak kişisel Microsoft hesabınızla oturum açın.  Size bir SMS mesajı telefonuna 6-8 basamaklı doğrulama kodunu içeren uygulama indirdiğiniz göndereceğiz. Bu kod Bul ve uygulamada girin istiyoruz. Bunun yerine, onu sizin için metin iletisi bulunur.
* **Çizim diğer uygulamalar**: kimliğinizi doğrular size bildirim üzerinde çalışıyor olabilecek diğer uygulama de görüntülenir.
* **İnternet'ten veri alma**: Bu izin, bildirim göndermek için gereklidir.
* **Telefon Uyumasını engellemesine**: Cihazınızı kuruluşunuza kaydedin, kuruluşunuzun bu ilkeyi telefonunuzda değiştirebilirsiniz.
* **Denetim titreşimi**: kimliğinizi doğrulamak için bir uyarı aldığınızda, bir titreşim isteyip istemediğinizi seçebilirsiniz.
* **Parmak izi donanım kullanmak**: kimliğinizi doğrulamanız her bazı iş ve Okul hesapları ek bir PIN gerektir. İşlemini kolaylaştırmak için PIN girmek yerine parmak izinizi kullanmanıza izin veriyoruz.
* **Ağ bağlantılarını görüntülemek**: bir Microsoft hesabı eklediğinizde, uygulama ağ/Internet bağlantısı gerektirir.
* **Depolama alanınızın içeriğini okuma**: uygulama ayarları aracılığıyla teknik bir sorun bildirdiğinde bu izni yalnızca kullanılır. Sorunu tanılamak için depolama alanınızı bazı bilgileri toplanır.
* **Tam ağ erişimi**: Bu izin, kimliğinizi doğrulamak için bildirim göndermek için gereklidir.
* **Başlangıçta çalıştırılmasını**: telefonunuzu yeniden başlatırsanız, kimliğinizi doğrulamak için bildirimler aldığınız devam bu izni sağlar.

### <a name="why-does-the-microsoft-authenticator-app-allow-you-to-approve-a-request-without-unlocking-the-device"></a>Neden Microsoft Doğrulayıcı uygulama cihazın kilidini açmadan bir isteği onaylamak izin veriyor mu?

Tüm kanıtlamak için gereken olduğundan, telefonunuz yanınızda olduğunu doğrulama isteklerini onaylamak için bir cihazın kilidini açmak zorunda değilsiniz. İki aşamalı doğrulama, iki şey – bildiğiniz bir şey ve sahip olduğunuz bir şey kanıtlayan gerektirir. Bildiğiniz parolanızı şeydir. Sahip olduğunuz şey telefonunuza (Microsoft Authenticator uygulamasıyla ayarlanır ve bir MFA kanıtını kayıtlı.). Bu nedenle, telefon sahip ve isteği onaylama ikinci faktörlü kimlik doğrulama ölçütlerini karşılayan.

### <a name="what-does-the-lock-icon-in-the-account-list-mean"></a>Kilit simgesi hesap listesindeki ne anlama geliyor?

Cihaz Azure AD'de kayıtlı ve hesabına kayıtlı asma kilit simgesini gösterir. Cihaz kaydı iOS için Microsoft Intune kayıt sırasında gerçekleşir.

## <a name="next-steps"></a>Sonraki adımlar

### <a name="contact-us"></a>Bizimle iletişim kurun
Sorunuzun yanıtını burada cevaplanıp değildi, sizden duymak istiyoruz. Git [Microsoft Authenticator uygulama Forumu](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp) sorunuzu gönderin ve topluluktan yardım alın veya bu sayfada bir yorum yazın.


### <a name="related-topics"></a>İlgili konular
* [İki basamaklı doğrulama hakkında](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) Microsoft hesapları için
* [İki aşamalı doğrulamayı sorununuz](multi-factor-authentication-end-user-troubleshoot.md) iş veya Okul hesabınız için?
* [Telefonunuzdan oturum açmak için Microsoft Authenticator kullanın](microsoft-authenticator-app-phone-signin-faq.md)
