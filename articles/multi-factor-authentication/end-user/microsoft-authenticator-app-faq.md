---
title: "Microsoft Authenticator uygulama Yardım ve Destek | Microsoft Docs"
description: "Sık sorulan sorular ve yanıtlar Microsoft Authentication uygulamasını ve Azure multi-Factor Authentication ile ilgili bir listesini sağlar."
services: multi-factor-authentication
documentationcenter: 
author: barlanmsft
manager: mtillman
ms.assetid: f04d5bce-e99e-4f75-82d1-ef6369be3402
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: barlan
ms.reviewer: librown
ms.custom: end-user
ms.openlocfilehash: 073b8adfcbe5fdcc2a6d1dba820a1101fac83218
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="microsoft-authenticator-app-faq"></a>Microsoft Authenticator uygulaması hakkında SSS

Bu makalede Microsoft Authenticator uygulaması hakkında aldığımız sık sorulan sorular yanıtlanmaktadır. Sorunuzun yanıtını görmüyorsanız, Git [Microsoft Authenticator uygulama Forumu](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp). Ayrıca belirli bir özellikle ilgili başka bir SSS uygulama üzerinde sahip olduğumuz [oturum telefonunuz SSS oturum](microsoft-authenticator-app-phone-signin-faq.md).

Microsoft Authenticator uygulamasını Azure Authenticator uygulamasını değiştirildi ve Azure çok faktörlü kimlik doğrulaması kullandığınızda önerilen uygulamadır. Microsoft Authenticator uygulaması [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), ve [iOS](http://go.microsoft.com/fwlink/?Linkid=825073).

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

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

Bu sorun giderme öneriler denedi ancak hala sorunlarınız varsa, bize tanılama için günlüklerinizi gönderin. Uygulama Ayarları'na gidin ve ardından **Yardım ve geri bildirim** ve **günlüklerini gönderme**. Ardından, Git [Microsoft Authenticator uygulama Forumu](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp) ve ne görmesini sorun ve hangi adımları şu ana kadar çalıştınız bize bildirin.

### <a name="im-already-using-the-microsoft-authenticator-application-for-verification-codes-how-do-i-switch-to-one-click-push-notifications"></a>Zaten Microsoft Authenticator uygulama doğrulama kodları için kullanıyorum. Tek tıklamayla anında iletme bildirimleri nasıl geçiş yapabilirim?
Bir oturum açma anında iletme bildirimi aracılığıyla onaylama yalnızca kişisel Microsoft hesapları için kullanılabilir, veya iş ve Okul Google veya Facebook gibi üçüncü taraf hesapları için değil, Microsoft hesapları. Bir iş veya Okul Microsoft hesabınız varsa, bu seçenek devre dışı bırakmak, kuruluşunuzun seçebilirsiniz.

Kişisel hesabınız için bir Microsoft hesabı kullanın ve anında iletme bildirimleri geçmek istiyorsanız hesabınızı yeniden eklemeniz gerekir. Hesabınıza yeniden kaydedilecek ve anında iletme bildirimlerini ayarlama.  

Microsoft Authenticator kullanmak için iş veya Okul hesabı varsa, kuruluşunuzun tek tıklatmayla bildirimleri izin verilip verilmeyeceğini belirler.

### <a name="do-one-click-push-notifications-work-for-non-microsoft-accounts"></a>Tek tıklamayla anında iletme bildirimleri için Microsoft olmayan hesapların çalışıyor mu?
Hayır, anında iletme bildirimleri yalnızca Microsoft hesapları ve Azure Active Directory hesaplarıyla çalışır. İşiniz veya okulunuz Azure AD hesapları kullanıyorsa, bu özellik devre dışı bırakabilir.  

### <a name="i-restored-my-device-from-a-backup-and-my-account-codes-are-missing-or-not-working-what-happened"></a>I cihazımı bir yedekten geri yüklenmiş ve my hesap kodları eksik veya çalışmıyor. Ne oldu?
Güvenlik nedeniyle, biz uygulama yedeklemelerden hesapları geri yüklemeyin.  Uygulama geri yükledikten sonra hesaplarınızı silin ve yeniden ekleyin.

### <a name="i-got-a-new-device-how-do-i-remove-the-microsoft-authenticator-app-from-my-old-device-and-move-to-the-new-one"></a>Yeni bir cihaz aldım. Nasıl eski aygıttan Microsoft Authenticator uygulamasını kaldırın ve yeni bir taşıma?
Microsoft Authenticator uygulaması için yeni bir cihaz ekleme otomatik olarak onu diğer cihazlardan kaldırmaz. Hangi cihazların hesabınız için yapılandırılan yönetmek için iki aşamalı doğrulamayı yönetmek için kullanın ve eski uygulamaları kaldırmak için seçin aynı Web sitesini ziyaret edin.

Kişisel Microsoft hesapları için bu Web sitesidir, [hesap güvenliği](https://account.microsoft.com/security) sayfası. İş veya Okul Microsoft hesapları için bu Web sitesi ya da olabilir [MyApps](https://myapps.microsoft.com) veya Kuruluşunuzda ayarlanmış özel bir portal.

### <a name="how-do-i-remove-an-account-from-the-app"></a>Uygulamadan nasıl bir hesap kaldırılsın mı?
* iOS: ana ekranından sağdan sola bir hesap kutucuğuna. **Sil**’i seçin.
* Windows Phone: ana ekranından menü düğmesi seçip **Düzenle hesapları**. Dokunun **X** hesap adının yanındaki.
* Android: ana ekranından menü düğmesi seçip **Düzenle hesapları**. Dokunun **X** hesap adının yanındaki.

Kuruluşunuz ile kayıtlı bir cihaz varsa, hesabınızı kaldırmayı fazladan bir adımı tamamlamak gerekebilir. Bu aygıtlar üzerinde Microsoft Authenticator uygulamasını otomatik olarak bir cihaz Yöneticisi olarak kaydedilir. Uygulama tamamen kaldırmak istiyorsanız, önce uygulama ayarları'nda uygulama kaydı gerekir.

### <a name="why-does-the-app-request-so-many-permissions"></a>Uygulama çok fazla sayıda izinler neden istek mu?
İşte tam bir listesi için isteyebilir izinleri ve uygulamada nasıl kullanılır. Gördüğünüz özel izinler elinizde telefon türüne bağlıdır.

* **Kamera**: bir iş, okul veya Microsoft dışı hesabı eklediğinizde, QR kodlarını tarayabilmek için kameranıza kullanırız.
* **Kişiler ve telefon**: Kişisel Microsoft hesabınızla oturum açın, biz telefonunuzda kullandığınız var olan hesapları bularak işlemi basitleştirmeye çalışın.
* **SMS**: ilk olarak kişisel Microsoft hesabınızla oturum açtığınızda, telefon numaranız kaydında sahibiz bir eşleştiğinden emin olmak sahibiz. Size bir SMS mesajı telefon uygulama indirdiğiniz göndereceğiz. İleti 6-8 basamaklı doğrulama kodunu içerir. Bu kod Bul ve uygulamada girin istiyoruz. Bunun yerine, biz, SMS mesajı halinde sizin için bulun.
* **Çizim diğer uygulamalar**: kimliğinizi doğrulamak için bir bildirim aldığında, biz bu bildirim çalışıyor olabilecek diğer uygulama görüntüler.
* **İnternet'ten veri alma**: Bu izin, bildirim göndermek için gereklidir.
* **Telefon Uyumasını engellemesine**: Cihazınızı kuruluşunuza kaydedin, bu ilkeyi telefonunuzda değiştirebilirsiniz.
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
