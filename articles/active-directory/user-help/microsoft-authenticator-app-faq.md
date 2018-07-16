---
title: Microsoft Authenticator uygulamasını - Azure AD ile Yardım | Microsoft Docs
description: Sık sorulan sorular ve cevaplar Microsoft Authentication uygulaması ve Azure multi-Factor Authentication ilgili bir listesini sağlar.
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
ms.openlocfilehash: d86bc84653e38a9b64a336b8ce9ed7e657129e8c
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39059833"
---
# <a name="microsoft-authenticator-app-faq"></a>Microsoft Authenticator uygulaması hakkında SSS

Bu makalede, Microsoft Authenticator uygulaması hakkında sık sorulan sorular yanıtlanmaktadır. Sorunuzun yanıtını görmüyorsanız, Git [Microsoft Authenticator uygulaması Forumu](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp). Ayrıca, bir uygulamada belirli bir özellikle ilgili başka bir SSS inceleyebilirsiniz [oturum telefonunuzla SSS](microsoft-authenticator-app-phone-signin-faq.md).

Microsoft Authenticator uygulamasını yerini Azure Authenticator uygulaması ve Azure multi-Factor Authentication'ı kullandığınızda önerilen uygulamasıdır. Microsoft Authenticator uygulaması [Android](https://go.microsoft.com/fwlink/?linkid=866594), [iOS](https://go.microsoft.com/fwlink/?linkid=866594) ve [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071)'da kullanılabilir.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="what-data-does-the-authenticator-store-on-my-behalf-and-how-can-i-delete-it"></a>Hangi verilerin Authenticator benim yerime depoluyor mu ve nasıl miyim silebilirsiniz?

Microsoft Authenticator, oluşturduğunuz bir hesap eklediğinizde, hesap bilgilerini depolar. Authenticator'ı kullandığınızda, bir tanılama günlüğü hata ayıklama amacıyla oluşturulan ve Microsoft herhangi bir öngörülemeyen sorunları tanılamanıza yardımcı yararlı verileri depolar. Günlük verilerini açarak erişebileceğiniz **yardımcı** > **günlükleri Gönder** > **günlükleri görüntüleyebilir**.

Hesabınızın kutucuğunu silerek verileri silebilirsiniz. Hesap kutucuğu sildiğinizde, günlükler de dahil olmak üzere uygulama tarafından kullanılan tüm hesap bilgilerini siler. 

Microsoft'un verilerinizi nasıl kullandığını daha fazla bilgi için ziyaret edin: https://servicetrust.microsoft.com/ViewPage/PrivacyGettingStarted

### <a name="what-are-the-codes-in-the-app-for-why-does-the-number-keep-counting-down"></a>Uygulama için kodları nelerdir? Neden sayım numarası koru?

Microsoft Authenticator uygulamasını açın, eklediğiniz hesaplar ve altı veya sekiz basamaklı bir sayı her biri görürsünüz. Sayım 30 saniyelik Zamanlayıcı görebilirsiniz.

Hesabınızda oturum açtığınızda bu kodları kullanılır. Kullanıcı kimliğiniz ve parolanızı girdikten sonra bir doğrulama kodu girmeniz istenebilir. Microsoft Authenticator uygulamasını açın ve şu anda gösteren kodu kopyalayın. Tamamlamak için oturum açma sayfasında bu kodu girin.

Böylece, hiçbir zaman aynı kodu iki kez kullanmak kodları her 30 saniyede değiştiren nedenidir. Anımsanması gereken yaptığınız gibi bir parola değil. Yalnızca birisi erişim telefonunuza doğrulama kodunuzu bilen olur.

Oturum açmak için telefon servis sahip hakkında endişelenmek zorunda olmadığınız için kodları internet veya veri, gerekli değildir. Uygulamayı kapattığınızda, arka planda çalışmaya devam etmez ve pil çekmediğinden. Uygulamayı kapatın ve oturum zamana kadar yoksay.  

### <a name="i-only-get-notifications-when-i-have-the-app-open-if-the-app-isnt-open-i-dont-get-any-notifications"></a>Uygulama olduğunda yalnızca bildirimleri açık alabilirim. Herhangi bir bildirim, uygulama açık değilse, elde etmezsiniz.

Bildirimleri almak, ancak etkisiz hale yok veya üzerinde olan, Etikan rağmen vibrate, ilk uygulama ayarlarını denetleyin. Ses kullanın veya kendi bildirimlerle vibrate için uygulamayı etkinleştirin.

Bildirimleri hiç alamazsanız, aşağıdaki durumlarda denetleyin:

- Telefonunuzu rahatsız etmeyin veya sessiz modda mı? Bu mod uygulamaları bildirimleri göndermesini tutabilirsiniz.
- Diğer uygulamalardan bildirimler alabilir? Aksi takdirde, telefon ya da Android veya Apple bildirimleri kanaldan ağ bağlantıları ile ilgili bir sorun olabilir. İlk seçenek telefon ayarlarınızı adres, ancak ikinci seçeneği ile ilgili Yardım için hizmet sağlayıcınıza konuşmak gerekebilir.
- Uygulama, ancak diğer bazı hesaplar için bildirimler alabilir miyim? Yanıt Evet ise, uygulamanızdan sorunlu hesabı kaldırıp yeniden anında iletme bildirimlerini etkinleştirmesi ekleyin.

Bu sorun giderme önerilerini çalıştı, ancak yine de sorunlar yaşayıp tanılama günlüklerinizi gönderebilirsiniz. Uygulama Ayarları'na gidin ve ardından **Yardım ve geri bildirim** ve **günlükleri gönderin**. Ardından, Git [Microsoft Authenticator uygulaması Forumu](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp) ve hangi görme sorunu ve hangi adımları şu ana kadar çalıştınız bize bildirin.

### <a name="im-already-using-the-microsoft-authenticator-application-for-verification-codes-how-do-i-switch-to-one-click-push-notifications"></a>Doğrulama kodları için Microsoft Authenticator uygulaması zaten kullanıyorum. Tek tıklamayla anında iletme bildirimleri nasıl geçiş yapabilirim?
Bir oturum açma aracılığıyla anında iletme bildirimi onaylama yalnızca kişisel Microsoft hesapları için kullanılabilir, ya da iş ve Okul Microsoft hesapları, Google veya Facebook gibi üçüncü taraf hesapları için değil. Bir iş veya Okul Microsoft hesabınız varsa, kuruluşunuz bu seçeneği devre dışı bırakmak seçebilirsiniz.

Kişisel hesabınızla bir Microsoft hesabı kullanmanız ve anında iletme bildirimleri geçmek istiyorsanız hesabınıza yeniden eklemeniz gerekir. Hesabınızı kullanarak cihazı yeniden kaydedin ve anında iletme bildirimleri ayarlayın.  

İş için Microsoft Authenticator'ı kullanın veya Okul hesabınız varsa, kuruluşunuzun tek tıklamayla bildirimleri izin verilip verilmeyeceğini belirler.

### <a name="do-one-click-push-notifications-work-for-non-microsoft-accounts"></a>Tek tıklamayla anında iletme bildirimleri için Microsoft olmayan hesapların çalışır?
Hayır, anında iletme bildirimleri yalnızca Microsoft hesapları ve Azure Active Directory hesaplarıyla çalışır. İş veya Okul hesabınız, Azure AD hesapları kullanılıyorsa, bu özelliği devre dışı bırakabilir.  

### <a name="i-got-a-new-device-or-restored-my-device-from-a-backup-how-do-i-set-up-my-accounts-in-the-microsoft-authenticator-app-again"></a>Yeni bir cihaz var, veya cihazımı bir yedeklemeden geri bildirimi. Nasıl my hesaplarında Microsoft Authenticator uygulamasını yeniden ayarlayabilirim?
Bir iOS cihazı kullanıyorsanız, açık **otomatik yedekleme**ve hesaplarınızı yedeğini eski Cihazınızda; oluşturduysanız Bu yedekleme yeni Cihazınızda hesabı kimlik bilgilerinizi kurtarmak için kullanabilirsiniz. Daha fazla bilgi için bkz. [yedekleme ve kurtarma hesabı kimlik bilgileriyle Microsoft Authenticator uygulamasını](microsoft-authenticator-app-backup-and-recovery.md) makalesi. 

### <a name="i-lost-my-device-or-moved-on-to-a-new-device-how-do-i-make-sure-notifications-dont-continue-to-go-to-my-old-device"></a>Cihazımı kayıp, veya yeni bir cihaza taşınmış bildirimi. Eski cihazımı Git bildirimleri devam etme nasıl emin olabilirim?  
Yeni iOS Cihazınızı Microsoft Authenticator uygulamasını ekleme otomatik olarak uygulamanın eski cihazınızın kaldırılması anlamına gelmez. Uygulamanın eski cihazınızın bile silme yeterli değildir. Hem eski cihazınızın uygulamayı silin ve Microsoft veya kuruluşunuz eski cihaz unutursanız ve hesabınızdan kaydı söyleyin.
- **Uygulamayı kişisel bir Microsoft hesabı kullanarak bir CİHAZDAN kaldırmak için.** İki aşamalı doğrulama alanına gidin, [hesap güvenliğini](https://account.microsoft.com/security) sayfasında ve eski cihazınız için doğrulama devre dışı bırakmak seçin.  
- **Uygulama bir iş kullanarak bir CİHAZDAN kaldırabilir veya Microsoft Okul için.** İki aşamalı doğrulama alanına gidin, [MyApps](https://myapps.microsoft.com/) sayfası veya kuruluşunuzun özel Portal ve eski cihazınız için doğrulama devre dışı bırakmak seçin. 



### <a name="how-do-i-remove-an-account-from-the-app"></a>Bir hesap uygulamadan nasıl kaldırabilirim?
* iOS: ana ekranında geçirme sola bir hesap kutucuğu. **Sil**’i seçin.
* Windows Phone: ana ekranında menü düğmesine, ardından **hesapları Düzenle**. Dokunun **X** yanında hesap adı.
* Android: ana ekranında menü düğmesine, ardından **hesapları Düzenle**. Dokunun **X** yanında hesap adı.

Kuruluşunuz ile kayıtlı bir cihazınız varsa, hesabı kaldırmak için fazladan bir adım tamamlamak gerekebilir. Bu cihazlarda, Microsoft Authenticator uygulamasını otomatik olarak bir cihaz Yöneticisi olarak kaydedilir. Uygulamasını tamamen kaldırmak istiyorsanız, önce uygulama ayarları uygulamada kaydını kaldırmak gerekir.

### <a name="why-does-the-app-request-so-many-permissions"></a>Uygulama, çok sayıda izinler neden istek?
İçin sorulan izinlerin tam bir listesi aşağıdadır ve uygulamada nasıl kullanılır. Gördüğünüz belirli izinlere sahip olduğunuz phone türüne bağlıdır.

* **Kamera**: iş, okul veya Microsoft olmayan hesaba eklediğinizde QR kodu taramak için kullanılır.
* **Kişiler ve telefon**: Kişisel Microsoft hesabınızla oturum açtığınızda, var olan hesapları telefonunuzda bularak kolaylaştırmak için kullanılır.
* **SMS**: telefon numaranızı kayıt numarası eşleştiğinden emin olmak için kullanılır. Kişisel Microsoft hesabınızla ilk kez oturum açtığınızda.  Kısa mesaj telefonuna bir 6-8 basamaklı doğrulama kodunu içeren uygulama indirdiğiniz göndereceğiz. Bu kodu bulun ve uygulamaya girmesi isteyen yerine, bunu sizin için SMS mesajı halinde bulunur.
* **Diğer uygulamalar üzerinde çizim**: kimlik doğrulayan size bildirim de çalışıyor olabilecek herhangi başka bir uygulamada görüntülenir.
* **İnternet'ten veri alma**: bildirim göndermek için bu izni gereklidir.
* **Telefon Uyumasını engellemesine**: Cihazınızı kuruluşunuza kaydedin, kuruluşunuz bu ilke telefonunuzda değiştirebilirsiniz.
* **Denetim Titreşim**: kimliğinizi doğrulamak için bir bildirim aldığınızda, bir titreşim isteyip istemediğinizi seçebilirsiniz.
* **Parmak izi donanımı**: bazı iş ve Okul hesapları, kimliğinizi doğrulamak her, ek bir PIN'i zorunlu kıl. İşlemi kolaylaştırmak için PIN girmek yerine parmak izinizi kullanmanıza izin veriyoruz.
* **Ağ bağlantıları görüntüleyin**: bir Microsoft hesabı eklediğinizde, uygulama ağ/Internet bağlantısı gerektirir.
* **Depolama alanınızı içeriğini okuma**: Bu izin yalnızca uygulama ayarlarını aracılığıyla teknik bir sorun bildirdiğinde kullanılır. Sorunu tanılamak için depolama alanınızı bazı bilgileri toplanır.
* **Tam ağ erişimi**: Bu izin, kimliğinizi doğrulamak için bildirim göndermek için gereklidir.
* **Başlangıçta Çalıştır**: telefonunuzu başlatırsanız, kimliğinizi doğrulamak için bildirimleri aldığınız devam bu izni sağlar.

### <a name="why-does-the-microsoft-authenticator-app-allow-you-to-approve-a-request-without-unlocking-the-device"></a>Neden Microsoft Authenticator uygulaması cihazın kilidini açmadan bir isteği onaylamak izin veriyor mu?

Doğrulama istekleri kanıtlamak için ihtiyacınız olduğundan, telefonunuz yanınızda olduğunu onaylamak için cihazınızın kilidini gerekmez. İki aşamalı doğrulama, iki şey – bildiğiniz bir şey ve sahip olduğunuz bir şey kanıtlama gerektirir. Bildiğiniz parolanızı şeydir. Sahip olduğunuz şey olan telefonunuzu (ile Microsoft Authenticator uygulamasını ayarlama ve bir MFA kavram kayıtlı.) Bu nedenle, bir telefon olması ve isteği onaylama ikinci faktörlü kimlik doğrulama ölçütlerini karşılayan.

### <a name="what-does-the-lock-icon-in-the-account-list-mean"></a>Hesap listesi kilit simgesi ne anlama geliyor?

Cihaz Azure AD'ye kaydettiniz ve hesabına kayıtlı asma kilit simgesini gösterir. Cihaz kaydı iOS için Microsoft Intune kayıt sırasında gerçekleşir.

## <a name="next-steps"></a>Sonraki adımlar

### <a name="contact-us"></a>Bizimle iletişim kurun
Sorunuzu burada cevaplanmamış değildi, görüşlerinizi almak isteriz. Git [Microsoft Authenticator uygulaması Forumu](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp) sorunuzu gönderin ve topluluktan yardım alın veya bu sayfada bir yorum yazın.


### <a name="related-topics"></a>İlgili konular
* [İki basamaklı doğrulama hakkında](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) Microsoft hesapları
* [İki aşamalı doğrulama konusunda sorun mu yaşıyorsunuz](multi-factor-authentication-end-user-troubleshoot.md) iş veya Okul hesabınız için?
* [Telefonunuzdan oturum açmak için Microsoft Authenticator'ı kullanın](microsoft-authenticator-app-phone-signin-faq.md)
