---
title: Azure Active Directory ile parola olmadan bir dünyada anlama | Microsoft Docs
description: Bu kılavuz, şirket, CIO, CISOs, baş kimlik mimarları, Kurumsal mimarlar ve güvenlik yardımcı olur ve kullanıcıların Azure Active Directory uygulaması için bir parolasız kimlik doğrulama yöntemi seçme için sorumlu BT karar alma mekanizmaları.
services: active-directory
keywords: parolasız, azuread
author: martincoetzer
ms.author: martinco
ms.date: 07/09/2019
ms.topic: article
ms.service: active-directory
ms.workload: identity
ms.openlocfilehash: 00f66b6010bead3de131095a47ba1e419d2511c0
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723447"
---
# <a name="a-world-without-passwords-with-azure-active-directory"></a>Azure Active Directory ile parola olmadan bir dünyada

Bu, ilişki parolalarla kesme zamanı geldi. Parolalar geçmişte bize iyi geçmiştir, ancak günümüzün dijital çalışma alanında bunlar görece kolay bir saldırı vektörüdür için bilgisayar korsanlarının haline geldi. Bilgisayar korsanlarının parolaları seviyorum ve neden, en yaygın olarak düşünün, Reddedilen Parola Azure Active Directory'de (Azure AD) yıl, ay, mevsim benzer terimleri dahil etmek veya takım yerel bir spor görmek zor değildir. Ayrıca, [araştırma gösterilen](https://aka.ms/passwordguidance) ilgili geleneksel önerileri parola yönetimi uzunluk gereksinimlerini, karmaşıklık gereksinimleri ve değişiklik sıklığı gibi çeşitli nedenlerden dolayı ters etki İnsan yapısı.

Üç tür kullanıcı hesapları ele geçirmek için yaygın olarak kullanılan saldırılar, kimlik avı ve İhlale yol açmak üzere yeniden parola ilaç olan. Azure AD özellikleri gibi [akıllı kilitleme](https://docs.microsoft.com/azure/active-directory/authentication/howto-password-smart-lockout), [yasaklanmış parolalar](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad-on-premises), ve [parola koruması](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad-on-premises) bu tür saldırılara karşı korunmasına yardımcı olabilir. Benzer şekilde, uygulama [çok faktörlü kimlik doğrulaması](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks) (MFA) veya iki aşamalı doğrulama, ikinci bir form kimlik doğrulaması gerektirerek ek güvenlik sağlar. Ancak, uzun vadede, parolasız çözüm kimlik doğrulaması en güvenli yöntem sağlamaya yönelik en iyi çözümdür.

Bu makalede, anlamanıza ve Microsoft'un parolasız çözümler ve bir veya daha fazla aşağıdaki seçenekler arasında seçtiğiniz Yardım uygulamak amacıyla Yolculuğunuzun başlangıçtır:

* **İş için Windows Hello**. Windows 10, Windows iş için Hello parolaları güçlü iki öğeli kimlik doğrulaması bilgisayarları ve mobil cihazlarda değiştirir. Bu kimlik doğrulaması, kullanıcı kimlik bilgisi bir cihaza bağlanır ve bir biyometrik veya PIN kodu kullanan yeni bir tür oluşur.

* **Parolasız oturum açma Microsoft Authenticator ile**. Microsoft Authenticator uygulamasını, bir Azure AD hesabınızda parola kullanmadan oturum açmak için kullanılabilir. Benzer şekilde, Windows iş için Hello teknolojisi, Microsoft Authenticator anahtar tabanlı kimlik doğrulaması, bir cihaza bağlanır ve bir biyometrik veya PIN kodu kullanan bir kullanıcı kimlik bilgilerini sağlamak için kullanır.

* **FIDO2 güvenlik anahtarları**. FIDO2 her Web sitesi arasında benzersizdir ve Windows Hello veya dış güvenlik anahtarları gibi yerel bir cihazda depolanan şifreleme oturum açma kimlik bilgileri sağlar. Bu güvenlik anahtarları, kimlik avı, parola hırsızlığı riskleri dayanıklıdır ve yeniden yürütme saldırıları. Biyometrim veya üzerinden kullanıcı doğrulama ile birlikte, iki Faktörlü doğrulama toplantı modern güvenlik gereksinimlerini çözümüdür.

## <a name="the-future-of-passwordless-authentication"></a>Parolasız kimlik doğrulama geleceği

Bugünlerde, bankalar, kredi kartı şirketleri ve diğer kuruluşlar ve Çevrimiçi Hizmetler genellikle hesabınızı iki kez kimliğinizi doğrulamanızı gerektiren korumak: kullanarak parolanızı, daha sonra tekrar telefon, metin veya uygulama tarafından bir kez. Çok faktörlü kimlik doğrulaması, paylaşılan parola güvenlik sorunu giderir olsa da çalınırsa ya da tahmin, bu parolaları unutmayın çalışılırken, dileriz faktörü adres değil. Günümüzün bulut dönemi kullanıcılar ve kuruluşlar istediklerinizi olduğu yüksek oranda güvenli olduğundan parolasız kimlik doğrulama yöntemleri *ve* kullanışlı.

![Kolaylık sağlamak ve güvenlik](./media/azure-ad/azure-ad-pwdless-image1.png)

Windows Hello iş, parolasız oturum açma için Microsoft Authenticator ve yüksek oranda güvenli ve kullanmak uygun olan bir kimlik doğrulama yöntemiyle kullanıcılar sağlayan basit, yaygın bir mimari herkes FIDO2 güvenlik anahtarları. Ortak/özel anahtar teknolojisi, yerel bir hareketi gibi bir biyometrik veya PIN ve özel anahtarlar, tek bir cihaz ve güvenli bir şekilde bağlı depolanır ve hiçbir zaman paylaşılan gerek üç temel alır.

## <a name="windows-hello-for-business"></a>İş İçin Windows Hello

Windows 10, Windows iş için Hello parolaları güçlü iki öğeli kimlik doğrulama PC ve cihazlarındaki değiştirir. Biyometrik hareketi veya PIN, kullanıcıların Azure AD ile kimlik doğrulaması olanak sağlayan bir cihaza bağlanır ve kullanıcı kimlik bilgileri yeni bir tür kimlik doğrulaması oluşan bir şirket içi Active Directory yanı sıra. İmzalama basitçe cihaza Windows Hello for Business kullanan kolaydır. Ya da bir PIN veya parmak izi veya yanı sıra yüz tanıma gibi biyometrik hareketi kullanın.

### <a name="windows-hello-for-business-scenarios"></a>Windows Hello iş senaryoları için

Windows iş için Hello belirlenen kendi Windows PC sahip bilgi çalışanları için idealdir. Biyometrik ve kimlik bilgileri doğrudan erişim sahibi dışındaki engeller kullanıcının bilgisayara bağlanır. PKI tümleştirmesi ve çoklu oturum açma (SSO) için yerleşik destek ile Windows iş için Hello bir basit ve kullanışlı bir yöntemle sorunsuz bir şekilde şirket kaynaklarına şirket içi Exchange'e erişimini ve bulutta sağlar.

### <a name="windows-hello-for-business-deployment-considerations"></a>Windows Hello için iş dağıtım konuları

Windows iş için Hello cihaz kayıt, sağlama ve kimlik doğrulaması gerçekleştirmek için çeşitli bileşenleri kullanan bir dağıtılmış sistemidir. Bu nedenle, kuruluş içindeki birden fazla takım arasındaki doğru planlama dağıtım gerektirir. Windows iş için Hello [planlama kılavuzunun](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-planning-guide) türüne Windows Hello iş dağıtım ve göz önünde bulundurmanız gerekir seçenekleri kararları vermenize yardımcı olmak için kullanılabilir.

Karma veya şirket içi dağıtımlar için sahip olduğunuz beklenir:

* İyi bağlantılı bir çalışma ağ

* İnternet erişimi

* Mfa'yı Windows Hello sırasında iş sağlama için desteklemek için multi-Factor Authentication sunucusu

* Uygun ad çözümleme, iç ve dış adları

* Active Directory ve etki alanı denetleyicileri site başına yeterli sayıda kimlik doğrulamasını desteklemek için

* Active Directory Sertifika Hizmetleri 2012 veya üzeri

* Windows 10, sürüm 1903 çalıştıran bir veya daha fazla iş istasyonu bilgisayarlar

İş dağıtımları Kurumsal sertifikalara etki alanı denetleyicileri için bir güven kökü olarak kullanmak için tüm türleri Windows Hello beri süresi dolmuş sertifikaları tarafından otomatik olarak yenilemek istersiniz [otomatik kayıt sunucusu ve kullanıcı için yapılandırma sertifikaları](https://docs.microsoft.com/windows-server/networking/core-network-guide/cncg/server-certs/configure-server-certificate-autoenrollment). Şirket içi dağıtımlar için kimlik sağlayıcısı Windows Server Active Directory Federasyon Hizmetleri (AD FS) rolünü çalıştıran şirket içi sunucudur. Federasyon sistemleri genellikle bir grup olarak bilinen sunucular, yük dengeli bir dizi gerektirir. Bu grupta bir iç ağa ve kimlik doğrulaması istekler için yüksek kullanılabilirlik sağlamak için çevre ağ topolojisi içinde yapılandırılır.

Windows Hello çalışma alanlarında iş istemek için bir Grup İlkesi ayarladığınızda, kuruluşunuzdaki kullanıcılar Windows Hello'yu kullanma açıklayarak hazırlama isteyebilirsiniz. Birisi bir yeni cihazın kurulumunu yapacak ayarladığında, örneğin, bunlar arasında seçmeniz istenir **bu cihaz kuruluşuma ait** veya **kişisel cihazımı budur**. Şirket cihazları için bunlar eski seçmelisiniz. Kullanıcıların da seçmeniz gerekir, hangi bağlantı seçeneği olduğu gerekir: **Azure AD'ye katılma** veya **bir yerel hesabı (etki alanına daha sonra) ayarlama**.

Biyometrik hareket kullanarak güne güne sonra PIN'ini sonunda unutursanız doğrulamak için alışkın olan kullanıcılar yaygındır. Yardım Masası çağrıları önlemek için kullanıcıların sıfırlama olanağı sağladığından önerilir Unutulan [parolaları](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-deployment) ve [PIN'ler](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-features#pin-reset).

Windows iş için Hello dağıtırken seçim yapabileceğiniz birçok seçenek vardır. Birden fazla seçenek sağlayan, neredeyse her kuruluşun Windows iş için Hello dağıtabilirsiniz sağlar. Aşağıdaki desteklenen dağıtım türleri göz önünde bulundurun:

* [Anahtar güveni dağıtım hibrit Azure AD'ye katıldı](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-hybrid-key-trust)

* [Sertifika güven dağıtımı hibrit Azure AD'ye katıldı](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-hybrid-cert-trust)

* [Azure AD'ye katılım tek oturum açma dağıtım kılavuzları](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-hybrid-aadj-sso)

* [Şirket içi anahtar güven dağıtımı](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-deployment-key-trust)

* [Şirket içi sertifika güven dağıtımı](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-deployment-cert-trust)

Çok sayıda seçenek sağlayan, karmaşık görünen dağıtım yapar. Ancak, çoğu kuruluş büyük olasılıkla Bunlar zaten Windows Hello iş dağıtımı için bağımlı olduğu altyapısının çoğu uyguladık belirler. Ne olursa olsun, Windows iş için Hello dağıtılmış bir sistemde olduğunu ve doğru planlama önerilir olduğunu anlamak önemlidir.

Okumanızı öneririz [bir Windows Hello iş dağıtımını planlama](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-planning-guide) uygun belirli kuruluşunuz için en iyi dağıtım modeline karar vermenize yardımcı olacak. Sonra yaptığınız planlamasına göre başvurmak [Windows Hello için iş Dağıtım Kılavuzu](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-deployment-guide) başarılı dağıtımı Windows iş için Hello, mevcut ortamdaki sağlamaya yardımcı olmak için.

### <a name="how-windows-hello-for-business-works"></a>Windows iş için Hello işleyişi

#### <a name="user-sets-up-windows-hello-for-business"></a>Kullanıcı Windows Hello iş için ayarlar

Kullanıcı kaydı sırasında bir ilk iki aşamalı doğrulama sonra Windows Hello kullanıcının cihazında ayarlanır ve Windows bir Biyometri, bir PIN veya parmak izi veya yanı sıra yüz tanıma gibi olabilen bir hareket ayarlamak için kullanıcıya sorar. Bir kez ayarlandıktan sonra kullanıcı kimliklerini doğrulamak için hareket sağlar. Windows sonra Windows Hello kullanıcıların kimliğini doğrulamak için kullanır.

Windows 10 Cihazınızı yeteneklerine bağlı olarak, bir donanım güvenilir platform Modülü (TPM) veya bir yazılım TPM olarak bilinen yerleşik güvenli bir kuşatma ya da gerekir. TPM, yüz tanıma, parmak izi veya kilidini açmak için PIN gerektiren özel anahtarı depolar. Biyometrik veri Dolaşımda değil ve hiçbir zaman dış cihazları veya sunuculara gönderilir. Windows Hello yalnızca biyometrik kimlik verilerini cihazda depoladığı için bir saldırgan biyometrik verileri çalmaya tehlikeye atabilir tek koleksiyon noktası yok.

> [!TIP]
> Yüzeysel olarak, bir parola gibi bir PIN hissettirir ancak gerçekten daha güvenlidir. Bir parola ve PIN arasında önemli bir fark, PIN söz konusu cihaz üzerinde oluşturulduğuna bağlıdır ' dir. Parola çalan bir kişinin yerden hesabınızda oturum açabilirsiniz. Ancak bunlar PIN'İNİZİ çalmak, bunların fiziksel Cihazınızı çok çalmaya gerekir! PIN cihazda yerel olduğundan, onun iletim kesildi veya bir sunucudan çalınırsa Ayrıca, her yerde aktarılır, bu nedenle değil.

#### <a name="user-using-windows-hello-for-business-for-sign-in"></a>Windows Hello for Business için oturum açma kullanan kullanıcı

Windows Hello biyometrik iş için ve PIN kullanma asimetrik (ortak/özel anahtar) kimlik doğrulaması için şifreleme. Kimlik doğrulaması sırasında böylece bir adam-de-ortadaki adam (Mıtm) saldırısı sonuçta elde edilen güvenlik belirteci veya yapıt çalabilir ve diğer yerlerden anlayamazsanız hangi kimlik doğrulama işlemi güvenliğini TLS oturum anahtarı şifreleme bağlıdır.

Windows iş için Hello, Azure AD'ye kullanıcı kimliğini doğrulayan bir kullanışlı oturum açma deneyimi sağlar ve Active Directory kaynakları. Azure AD'ye katılmış cihazlar için Azure kimlik doğrulaması oturum açma sırasında ve isteğe bağlı olarak Active Directory ile kimlik doğrulaması yapabilir. Hibrit Azure Active Directory'ye katılmış cihazlarda oturum açma sırasında Active Directory ile kimlik doğrulaması ve Azure Active Directory kimlik doğrulaması arka planda.

![Kaynak görüntü bakın](./media/azure-ad/azure-ad-pwdless-image2.jpeg)

Aşağıdaki adımlar, Azure Active Directory oturum açma kimlik doğrulaması göstermektedir.

![Windows 10 kilit ekranı](./media/azure-ad/azure-ad-pwdless-image3.png)

1. Windows kullanarak kullanıcı işaretlerini biyometrik veya PIN hareket. Hareket Windows Hello için iş özel anahtar kilidini açar ve bulut AP sağlayıcısı olarak başvurulan bulut kimlik doğrulaması güvenlik desteği sağlayıcısı gönderilir.

2. Bulut AP sağlayıcısı, Azure Active Directory'den nonce ister.

3. Azure AD, 5 dakika boyunca geçerli nonce döndürür.

4. Bulut AP sağlayıcı nonce kullanıcının özel anahtarla imzalar ve Azure Active Directory'ye imzalı nonce döndürür.

5. Azure Active Directory, kullanıcının nonce imza karşı güvenli bir şekilde kayıtlı ortak anahtar kullanılarak imzalanmış nonce doğrular. Ardından Azure AD imzası doğruladıktan sonra döndürülen imzalı nonce doğrular. Azure AD nonce doğruladıktan sonra cihazın aktarım anahtarıyla şifrelenir ve bulut AP sağlayıcıya döndüren oturum anahtarı ile bir PRT oluşturur.

6. Bulut AP sağlayıcı oturum anahtarıyla şifrelenmiş PRT alır. Cihazın özel Aktarım anahtarını kullanarak, bulut AP sağlayıcı oturum anahtarının şifresini çözer ve cihazın TPM kullanarak oturum anahtarı korur.

7. Sonra kullanıcı Windows yanı sıra bulut erişebilir ve şirket içi uygulamaları yeniden kimlik doğrulaması gerek kalmadan Windows kimlik doğrulaması başarılı yanıt bulut AP sağlayıcı döndürür (SSO).

Kimlik doğrulama işlemi Windows iş için Hello içeren diğer senaryolarda daha derin göz için bkz: [Windows Hello iş ve kimlik doğrulaması için](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-how-it-works-authentication#Azure-AD-join-authentication-to-Active-Directory-using-a-Key).

#### <a name="user-manages-their-windows-hello-for-business-credentials"></a>Kullanıcı, Windows Hello iş kimlik bilgilerini yönetir

[Microsoft PIN sıfırlama Hizmetleri](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-features#pin-reset) PIN'ini gerekirse sıfırlamalarına olanak sağlayan Azure AD'de bir özelliktir. Grup İlkesi, Microsoft Intune veya uyumlu bir MDM kullanarak yönetici güvenli bir şekilde gerek kalmadan Unutulan PIN ayarları aracılığıyla veya kilit ekranı üzerinde sıfırlamalarına olanak sağlayan Microsoft PIN sıfırlama hizmeti kullanmak için Windows 10 cihazları yapılandırabilirsiniz yeniden kayıt.

Bazen kullanıcıların parolalarını kullanmaya geri döner gerekmez. [Self Servis parola sıfırlama](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-deployment) (SSPR), kullanıcıların BT personeli bağlanmaya gerek kalmadan kullanıcıların parolalarını sıfırlamalarına olanak sağlayan başka bir Azure AD özelliğidir. Kullanıcılar, kaydolması gerekir veya Self Servis parola sıfırlama hizmetini kullanmadan önce kayıtlı olması. Kayıt sırasında bir veya daha fazla kimlik doğrulama yöntemleri ve kuruluşun etkin kullanıcı seçer. Hızlı bir şekilde Engellemesi alın ve nerede veya günün saatini ne olursa olsun çalışmaya devam etmek kullanıcıların SSPR sağlar. Kullanıcıların kendilerini engellemesini izin vererek, kuruluşunuzda parola güvenlikle ilgili en yaygın sorunlar için yüksek destek maliyetlerini ve üretken olmayan saat azaltabilir.

## <a name="passwordless-sign-in-with-microsoft-authenticator"></a>Microsoft Authenticator ile parolasız oturum açın

Parolasız oturum açma Microsoft Authenticator ile telefonla oturum açma kullanarak Azure AD hesapları için oturum açmak için kullanılan başka bir parolasız çözümüdür. Yine de bildiğiniz bir şey ve bir şey var, ancak oturum açma parolanızı girmeyi Atla sağlar ve tüm kimlik doğrulama gerçekleştirir, parmak izi, yüz tanıma veya PIN kullanarak mobil Cihazınızda telefon sağlayarak kimliğinizi doğrulamanız gerekir.

### <a name="microsoft-authenticator-passwordless-scenarios"></a>Microsoft Authenticator parolasız senaryoları

Microsoft Authenticator uygulaması, kullanıcıların kimliklerini doğrulama ve kendi iş veya kişisel hesap kimlik doğrulaması sağlar. Ayrıca, bir kerelik geçiş koduyla bir parola genişletmek, anında iletme bildirimi veya parola gereksinimini tamamen değiştirmek için de kullanılabilir. Bir parola kullanmak yerine, kullanıcıların kimliklerini parmak izini tarama, Iris ya da yanı sıra yüz tanıma veya PIN mobil telefon numaraları onaylayın. Ne Windows Hello için kullandığı benzer güvenli teknolojisini temel alan bu araç, basit bir uygulama kullanıcılar için uygun bir seçeneği yaparak mobil bir cihazda oturum paketlenmiştir. Microsoft Authenticator uygulaması, Android ve iOS için kullanılabilir.

### <a name="microsoft-authenticator-deployment-considerations"></a>Microsoft Authenticator dağıtım konuları

Parolasız oturum'ın Azure AD'ye yapmak için Microsoft Authenticator uygulamasını kullanmak için Önkoşullar şunlardır:

* Son kullanıcıların Azure multi-Factor Authentication için etkinleştirilen

* Kullanıcıların Microsoft Intune veya bir üçüncü taraf mobil cihaz Yönetimi (MDM) çözümü kullanarak cihazlarını kaydetmek için

Bu gereksinimleri karşılandıktan varsayıldığında, yöneticiler parolasız telefonla oturum açma kiracıdaki kullanarak etkinleştirin [Windows PowerShell.](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-phone-sign-in#enable-my-users) Telefonla oturum açma kiracıda etkinleştirildikten sonra son kullanıcıların iş veya Okul hesabı seçerek telefonlarını kullanarak oturum açın kullanmamayı seçebilirsiniz **hesapları** belirleyerek bu uygulamanın ekran **etkinleştirme telefonla oturum açma** .

Bir yönetici tarafından etkin parolasız oturum açma varsayarak, son kullanıcıların aşağıdaki gereksinimleri karşılaması gerekir:

* Azure çok faktörlü kimlik doğrulamasına kayıtlı

* En son sürümünü Microsoft Authenticator yüklü iOS 8.0 veya üzeri çalıştıran cihazlar ya da Android 6.0 veya üzeri

* Uygulamaya eklenen anında iletme bildirimleri ile iş veya Okul hesabı

Erişimin kaybedilmesini olasılığını önlemek için hesabınızı veya yeni bir cihaz hesaplarında yeniden oluşturmak zorunda dışında Microsoft Authenticator kullanmanız önerilir [hesabı kimlik bilgilerinizi yedekleme](https://docs.microsoft.com/azure/active-directory/user-help/user-help-auth-app-backup-recovery) buluta. Yedeklemeden sonra uygulama bilgilerinizi yeni bir cihazda olabilecek erişimin kaybedilmesini önleme kurtarmak için de kullanabilirsiniz out veya hesaplarını yeniden oluşturmak zorunda.

Çoğu kullanıcı kimlik doğrulaması için yalnızca parola kullanmaya alışkın olduğundan, kuruluşunuz kullanıcıların bu işlem ile ilgili bilgilendirici önemlidir. Farkındalık, kullanıcılar Yardım masanız için çağırmanızı olasılığını azaltabilirsiniz [sorunları](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-phone-sign-in#known-issues) Microsoft Authenticator uygulamasını kullanarak imzalama ile ilgili.

> [!NOTE]
> Bir olası hata bu çözüm için bir gezici kullanıcı olduğu bir konumda noktasıdır Internet bağlantısı olmayan olduğu. FIDO2 güvenlik anahtarları ve Windows iş için Hello da aynı sınırlama tabi değildir.

### <a name="how-passwordless-sign-in-with-microsoft-authenticator-works"></a>Microsoft Authenticator works ile nasıl parolasız oturum açın

#### <a name="user-sets-up-passwordless-sign-in-with-microsoft-authenticator"></a>Kullanıcı Microsoft Authenticator ile parolasız oturum açma ayarlar

Microsoft Authenticator uygulaması için bir Azure AD hesap oturum açmak için parolasız bir çözüm olarak kullanılabilmesi için önce bir yönetici ve son kullanıcılar tarafından adımlar gerçekleştirilmelidir.

İlk olarak, yönetici gerekecektir [bir kimlik bilgisi olarak uygulama kullanımını etkinleştirme](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-phone-sign-in#enable-my-users) Windows PowerShell kullanarak Kiracı içinde. Son kullanıcılar için Azure multi-Factor Authentication (Azure MFA) etkinleştirmek ve Microsoft Authenticator uygulamasını biri olarak yapılandırmak yönetici ayrıca gerekir [doğrulama yöntemlerini](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-mfasettings#verification-methods).

Son kullanıcıların gerekecektir [yükleyip](https://docs.microsoft.com/azure/active-directory/user-help/user-help-auth-app-download-install) Microsoft Authenticator uygulamasını ve [kendi hesabını ayarlama](https://docs.microsoft.com/azure/active-directory/user-help/security-info-setup-auth-app) doğrulama yöntemlerinden biri olarak Microsoft Authenticator uygulamasını kullanmak için.

> [!VIDEO https://www.youtube.com/embed/uWbkLuI4g30]

#### <a name="user-using-microsoft-authenticator-for-passwordless-sign-in"></a>Parolasız oturum açmak için Microsoft Authenticator'ı kullanarak kullanıcı

Microsoft Authenticator uygulamasını herhangi bir Azure AD hesabı için parola kullanmadan oturum açmak için kullanılabilir. Windows 10 kilit ekranında oturum açma seçeneği olarak Microsoft Authenticator uygulamasını içermez, ancak kullanıcılar yine de kullanıcı adı girin ve ardından varolup olmadığını doğrulamak için mobil cihazlarında bir anında iletme bildirimi alırsınız. Kullanıcılar, oturum açma ekranında bir sayı eşleşen ve yüz tarama, parmak izi veya özel anahtar kilidini açmak ve kimlik doğrulamasını tamamlamak için PIN sağlama varlıklarını onaylayın. Bu çok faktörlü doğrulama yöntemi, bir paroladan daha güvenli ve daha uygun bir parola ve bir kod girerek daha.

![Doğrulayıcı oturum açma](./media/azure-ad/azure-ad-pwdless-image4.png)

Kullanıcı Microsoft Authenticator uygulaması sürümü kullanılan Azure AD bulabilmesi için tanımlanması gerekiyor beri Windows iş için Hello ancak biraz daha karmaşık olduğundan parolasız kimlik doğrulaması Microsoft Authenticator'ı kullanarak, aynı temel deseni izler. kullanılır.

![Authenticator işlemi](./media/azure-ad/azure-ad-pwdless-image5.png)

1. Kullanıcı, kullanıcı adlarını girer.

2. Azure AD kullanıcı güçlü bir kimlik bilgisi yok ve güçlü kimlik bilgileri akışı başlatan algılar.

3. Aracılığıyla Apple anında iletilen bildirim servisi (APNS) iOS cihazlarında veya aracılığıyla Firebase Cloud Messaging (FCM) Android cihazlarda uygulamaya bildirim gönderilir.

4. Kullanıcı, anında iletme bildirimi alır ve uygulama açılır.

5. Uygulama Azure AD'ye çağırır ve durum kanıtı sınaması ve nonce alır.

6. Kullanıcı, kendi biyometrik veya PIN özel anahtar kilidini açmak için girerek sınamayı tamamlayan.

7. Nonce özel anahtarla imzalanır ve Azure AD'ye geri gönderilir.

8. Azure AD, ortak/özel anahtar doğrulama gerçekleştirir ve bir belirteç döndürür.

#### <a name="user-manages-their-passwordless-sign-in-with-microsoft-authenticator-credentials"></a>Kullanıcıların parolasız oturum açma Microsoft Authenticator kimlik bilgilerine sahip kullanıcı yönetir

İle [kayıt birleştirilmiş](https://docs.microsoft.com/azure/active-directory/authentication/concept-registration-mfa-sspr-combined), kullanıcılar kaydetme ve Azure multi-Factor Authentication hem de Self Servis parola sıfırlama avantajlarından yararlanın. Kullanıcıları kaydetmek ve giderek bu ayarları yönetmek, [Profilim sayfa](https://aka.ms/mysecurityinfo). SSPR etkinleştirmeye ek olarak, kayıt destekler, birden çok kimlik doğrulama yöntemleri ve Eylemler birleştirilmiş.

## <a name="fido2-security-keys"></a>FIDO2 güvenlik anahtarları

FIDO2 en son sürümüne FIDO Alliance standart ve iki bileşenden - W3C Web kimlik doğrulaması (WebAuthN) standart ve karşılık gelen FIDO Alliance İstemci Kimliği Doğrulayıcı Protokolü (CTAP2) vardır. FIDO2 standartlara uygun donanım, mobil ve birçok uygulamaları ve mobil ve Masaüstü ortamlarında Web siteleri ile kolayca kimlik doğrulaması için Doğrulayıcı Biyometri tabanlı yararlanın olanak tanıyın.

Microsoft ve endüstri ortakları FIDO2 güvenlik cihazlarda Windows paylaşılan cihazlar kolay ve güvenli kimlik doğrulamasını etkinleştirmek Hello ile birlikte çalışmaktadır. FIDO2 güvenlik anahtarları izin, bilginizle yürütmek ve güvenli bir şekilde kimlik doğrulaması için bir [Azure AD'ye](https://aka.ms/azuread418)-kuruluşunuzun bir parçası olan alanına katılmış Windows 10 cihaz.

WebAuthN sağlayan geliştirme ve güçlü uygulaması, web uygulamaları ve Hizmetleri tarafından parolasız kimlik doğrulama API tanımlar. CTAP protokolü ile WebAuthN çalışmaya ve hizmet kimlik doğrulayan uyumlu FIDO güvenlik anahtarları gibi dış cihazları sağlar. Web kimlik doğrulaması ile kullanıcılar, yüz tanıma, parmak izi, PIN veya taşınabilir FIDO2 güvenlik anahtarları ile çevrimiçi hizmetleri için parola yerine güçlü ortak anahtar kimlik bilgileri yararlanarak oturum açabilir. Şu anda Microsoft Edge ve Chrome desteği WebAuthN desteklendiği ve Firefox geliştirme.

Cihazlar ve FIDO2 WebAuthN ve CTAP protokollerini kullanan belirteçleri güçlü kimlik doğrulaması hakkında bir çapraz platform çözümü parola kullanmadan getirin. Microsoft iş ortakları, çeşitli USB güvenlik anahtarları ve NFC etkin akıllı kartlar gibi güvenlik anahtar form faktörleri üzerinde çalışıyoruz.

### <a name="fido2-security-keys-scenarios"></a>FIDO2 güvenlik anahtarları senaryoları

FIDO2 güvenlik anahtarları, kilit ekranında Windows 10 kimlik bilgisi sağlayıcısı güvenlik anahtarı seçerek Azure AD ile oturum açmak için kullanılabilir. Bir kullanıcı adı veya parola gerekli olduğu için ideal çözüm ilk satırı çalışanlarına bilgisayarlar birden çok kullanıcı arasında paylaşmak için kolaylaştırır. Bir kullanıcının kimlik bilgilerini ayrı fiziksel cihazlarından şirket ilkelerini dikte Bunlar ayrıca mükemmel kimlik doğrulama seçeneği değildir. Kullanıcılar, Windows 10'da 1809 veya üzeri bir sürüm FIDO2 anahtarının içinde Microsoft Edge tarayıcısı kullanarak web siteleri için oturum açmak de seçebilirsiniz.

### <a name="fido2-security-keys-deployment-considerations"></a>FIDO2 güvenlik anahtarları dağıtım konuları

Yöneticiler, Azure AD'de FIDO2 desteğini etkinleştir ve yetenek kullanıcılara veya gruplara atayın. İlkeleri nasıl anahtarları sağlanır ve izin verme veya engelleme belirli bir donanım güvenlik anahtar kümesini kısıtlamalarını yapılandırmak için de oluşturulabilir. Anahtarlar, son kullanıcıların fiziksel olarak dağıtılmalıdır.

**Parolasız etkinleştirme gereksinimleri Azure AD'de oturum ve FIDO2 güvenlik anahtarları kullanarak web siteleri aşağıdakileri içerir:**

* Azure AD

* Azure Multi-Factor Authentication

* Birleşik kayıt Önizleme

* FIDO2 güvenlik anahtar Önizleme uyumlu FIDO2 güvenlik anahtarı gerektirir.

* Web kimlik doğrulaması (WebAuthN) Microsoft Edge Windows 10 sürüm 1809 veya üzerini gerektirir

* Azure AD tabanlı Windows oturum açma gerektirir FIDO2 alanına katılmış Windows 10 sürüm 1809 veya üzerini (Windows 10'da 1903 veya üzeri bir sürümü en iyi deneyimi olan)

FIDO2 uyumlu büyüklükleri USB NFC ve Bluetooth cihazları içerir. Bu yana bazı platformlarda, özel gereksinimlerinizi karşılayan form faktörü seçin ve tarayıcılar henüz FIDO2 uyumlu olmayan öneririz.

Ayrıca her kuruluş, kullanıcılar için bir protokol oluşturun ve yöneticiler izlemek için gereken güvenlik öneririz anahtar kaybolabilir veya çalınabilir. Kullanıcılar, Yöneticiler veya kullanıcı kendi güvenlik anahtarları kullanıcının profilinden silin ve yeni bir sağlama kaybolan veya çalınan anahtar bildirmeniz gerekir.

### <a name="how-fido2-security-keys-works"></a>FIDO2 güvenlik anahtarları nasıl çalışır?

#### <a name="user-sets-up-fido2-security-key"></a>Kullanıcı FIDO2 güvenlik anahtarı ayarlar.

Yöneticiler, ancak [anahtarları'el ile sağlama](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-passwordless-enable) ve son kullanıcılara dağıtın, aracılığıyla sağlama ve Windows 10 kilit ekranında FIDO2 kimlik bilgisi sağlayıcı etkinleştirme desteklenecektir [Intune](https://docs.microsoft.com/intune/windows-enrollment-methods). Yöneticiler kullanmak de gerekir [Azure portalında](https://portal.azure.com/) parolasız kimlik doğrulama yöntemi olarak donanım belirteci cihazları etkinleştirme.

FIDO2 güvenlik anahtarları dağıtımı da gerektirir kullanıcılar kendi anahtarları kullanarak kayıt [kayıt birleştirilmiş](https://docs.microsoft.com/azure/active-directory/authentication/concept-registration-mfa-sspr-combined). Birleşik kayıt ile kullanıcıların bir kez kaydedin ve Azure multi-Factor Authentication hem tek oturum açma parolasını sıfırlama (SSPR) avantajlarından yararlanın.

Donanım belirteç varsayılan çok faktörlü kimlik doğrulaması yöntemi seçmenin yanı sıra, ayrıca bir ek doğrulama seçeneğinin belirlenebilmesi önerilir.

* Microsoft Authenticator--bildirimi

* Authenticator uygulaması veya donanım belirteci--kod

* Telefon araması

* Kısa mesaj

#### <a name="user-using-fido2-security-key-for-sign-in"></a>Kullanıcı oturum açma için FIDO2 güvenlik anahtarı kullanma

FIDO2 ortak/özel anahtar şifreleme ve kimlik doğrulayıcısı bir özel anahtar gidermek ve bir ortak anahtar teslim etmek Windows Hello ve güvenlik anahtarları gibi yerleşik platform Doğrulayıcı etkinleştirmek için kullanılan form faktörü arasında bir soyutlama katmanı sağlar. tanımlayıcı olarak dış kaynaklara erişmek için kullanılabilir. FIDO2 güvenlik anahtarları, özel anahtarı depolayan ve Biyometri ya da PIN kilidini açmak için gerektirir kendi yerleşik güvenli kuşatma ile bulunur. Kimlik bilgilerini yeniden kullanılamaz durumdayken, veya hizmetler arasında paylaşılan ve ihlallerinden kimlik avı ve Mıtm saldırılarına veya sunucu tabi değildir.

![FIDO2 oturum açma](./media/azure-ad/azure-ad-pwdless-image6.png)

Güvenli kimlik doğrulaması, form faktörü bağımsız FIDO2 güvenlik anahtarları sağlar. Güvenlik anahtarı kimlik bilgisini tutar ve parmak izi (tümleşik güvenlik anahtarını) veya Windows oturum açma işleminde girilmesi için bir PIN gibi ek bir ikinci faktörle korunmalıdır. Microsoft iş ortakları, çeşitli güvenlik anahtar form faktörleri üzerinde çalışıyoruz. NFC etkin akıllı kartlar ve USB güvenlik anahtarları bazı örnekler içerir.

> [!NOTE]
> Güvenlik anahtarı belirli özellikleri ve Uzantılar'dan FIDO2 CTAP Protokolü şekilde uygulamalıdır [Microsoft uyumlu](https://aka.ms/fido2securitykeys). Microsoft, bu çözümler Windows 10 ve Azure Active Directory ile uyumluluk için test.

![FIDO2 oturum açma işlemi](./media/azure-ad/azure-ad-pwdless-image9.png)

1. Kullanıcı bilgisayarda oturum FIDO2 cihaz yararlanmanıza imkan sağlar.

2. Windows FIDO2 güvenlik anahtarı algılar.

3. Windows kimlik doğrulama isteği gönderir.

4. Azure AD, bir geçici öğe geri gönderir.

5. Kullanıcı FIDO2 güvenlik anahtarın güvenli kuşatma içinde depolanan özel anahtar kilidini açmak için kendi hareketi tamamlar.

6. FIDO2 güvenlik anahtarı nonce özel anahtarla imzalar.

7. Azure AD ile imzalı nonce PRT belirteç isteği gönderilir.

8. Azure AD imzalı nonce FIDO2 ortak anahtarı kullanarak doğrular.

9. Azure AD, şirket kaynaklarına erişimi etkinleştirme için PRT döndürür.

#### <a name="user-manages-their-fido2-security-key-credentials"></a>Kullanıcı FIDO2 güvenlik anahtar kimlik bilgilerini yönetir.

Benzer şekilde, Microsoft Authenticator uygulamasını, son kullanıcılar için birleşik kayıt deneyimini, kimlik bilgileri yönetimi FIDO2 güvenlik anahtarları için kullanır.

## <a name="deciding-a-passwordless-method"></a>Parolasız bir yönteme karar verme

Bu üç parolasız seçenekler arasından seçim, şirketinizin güvenlik, platform ve uygulama gereksinimleri bağlıdır.

Microsoft parola olmadan teknoloji Seçim yapılırken dikkate alınacak bazı faktörler şunlardır:

||**İş İçin Windows Hello**|**Microsoft Authenticator uygulamasını ile parolasız oturum açın**|**FIDO2 güvenlik anahtarları**|
|:-|:-|:-|:-|
|**Önkoşul**| Windows 10, sürüm 1809 veya üzeri<br>Azure Active Directory| Microsoft Authenticator uygulaması<br>Telefon (iOS ve Android cihazlarda çalışan Android 6.0 veya üstü.)|Windows 10, sürüm 1809 veya üzeri<br>Azure Active Directory|
|**Modu**|Platform|Yazılım|Donanım|
|**Sistemleri ve cihazların**|Bilgisayar ile bir yerleşik Güvenilir Platform Modülü (TPM)<br>PIN ve Biyometri tanıma |PIN ve Biyometri tanıma telefonda|Microsoft uyumlu FIDO2 güvenlik cihazlar|
|**Kullanıcı deneyimi**|Windows cihazları ile bir PIN veya biyometrik tanıma (yanı sıra yüz, Iris veya parmak izi) kullanarak oturum açın.<br>Windows Hello kimlik doğrulaması cihaza bağlanır; Kullanıcının, hem cihaz hem de bir oturum açma gibi şirket kaynaklarına erişmek için PIN veya biyometrik faktörü bileşen olmalıdır.|Parmak izini tarama, yanı sıra yüz veya Iris tanıma ile bir cep telefonunu kullanarak oturum açın veya SABİTLEYİN.<br>Kullanıcıların iş veya kişisel hesap için kendi PC ya da cep telefonu oturum açın.|FIDO2 güvenlik cihazı (Biyometri, PIN ve NFC) kullanarak oturum açın<br>Kullanıcı kuruluş denetimlerine göre cihaz erişebilir ve temel alınarak PIN, biyometrik cihazların USB güvenlik anahtarları ve NFC etkin akıllı kartları, anahtarlar veya giyilebilir cihazlar gibi kullanarak kimlik doğrulaması.|
|**Etkinleştirilen senaryolar**| Parola olmadan deneyimi ile Windows cihazı.<br>Adanmış iş PC için çoklu oturum açma olanağı için geçerli cihaz ve uygulamaları.|Parola olmadan herhangi bir cep telefonu kullanarak çözüm.<br>İş veya kişisel uygulamalar web üzerindeki herhangi bir CİHAZDAN erişim için geçerlidir.|Biyometri, PIN ve NFC kullanılarak çalışanları için parola olmadan deneyim.<br>Paylaşılan bilgisayarlar ve cep telefonu kaydının uygulanabilir bir seçenek (Yardım Masası personeli, genel bilgi veya hastane takım sunamıyoruz gibi) olduğu için geçerlidir|

Gereksinimlerinize ve kullanıcıların hangi yöntemin destekleyecek seçmek için aşağıdaki tabloyu kullanın.

|Kişi|Senaryo|Ortam|Parolasız teknolojisi|
|:-|:-|:-|:-|
|**Yönetici**|Bir cihaz yönetim görevleri için güvenli erişim|Atanan Windows 10 cihaz|Windows Hello için iş ve/veya FIDO2 güvenlik anahtarı|
|**Yönetici**|Windows olmayan cihazlarda yönetim görevleri| Mobile veya windows olmayan cihaz|Microsoft Authenticator uygulamasını ile parolasız oturum açın|
|**Bilgi çalışanı**|Üretkenlik iş|Atanan Windows 10 cihaz|Windows Hello için iş ve/veya FIDO2 güvenlik anahtarı|
|**Bilgi çalışanı**|Üretkenlik iş| Mobile veya windows olmayan cihaz|Microsoft Authenticator uygulamasını ile parolasız oturum açın|
|**Cephe çalışan**|Bilgi noktaları Fabrika, tesis, perakende veya veri girişi|Paylaşılan Windows 10 cihazları|FIDO2 güvenlik anahtarları|

## <a name="getting-started"></a>Başlarken

Parolasız kimlik doğrulaması wave gelecek ve daha güvenli bir ortam yolu kullanılır. Kuruluşlar için bu değişiklik planlama başlamanız önerilir ve bağımlılıklarını parolaları üzerinde azaltır. Başlamak için aşağıdaki hedefleri göz önünde bulundurun:

* Kullanıcılara MFA, Microsoft Authenticator uygulamasını + koşullu erişim için etkinleştirin.

* Piyasaya çıkma Azure AD parola koruması ve güncelleştirme ilkeleri.

* Kullanıcıların toplam kaydı SSPR için etkinleştirin.

* Taşınabilirlik için Microsoft Authenticator uygulamasını dağıtın.

* Windows iş için Hello dağıtma (1903: güncel kalın).

* Telefonlar kullanamayan kullanıcılarına FIDO2 cihazları dağıtın.

* Mümkün olduğunda, parola tabanlı kimlik doğrulama devre dışı bırakın.

Bu hedeflere ulaşmak için aşağıdaki yaklaşımı önerilir:

1. Azure MFA ve koşullu erişim gibi özellikleri tam olarak yararlanmak Azure Active Directory etkinleştirin.

2. Ek koruma sağlamak çok faktörlü kimlik doğrulamasını etkinleştirin.

3. Kullanıcıların parola kullanmaya geri döner gerek durumunda Self Servis parola sıfırlama etkinleştirin.

4. Microsoft Authenticator telefonla oturum açma için eklenen mobility dağıtın.

5. Windows iş için Hello tüm Windows 10 cihazlarınıza dağıtın.

6. FIDO2 güvenlik anahtarları için hazırlayın.

> [!NOTE]
> Azure Active Directory'ye başvuran [lisans sayfası](https://azure.microsoft.com/pricing/details/active-directory/) parolasız yöntemleri için lisanslama gereksinimleri hakkında ayrıntılı bilgi için.

## <a name="conclusion"></a>Sonuç

Geçtiğimiz birkaç yılda, Microsoft için parola olmadan bir dünyayı mümkün taahhütte devam etti. Windows 10 ile sunulan Microsoft Windows iş için Hello, güçlü, çoklu oturum Azure Active Directory ve Active Directory sağlayan iki öğeli kimlik bilgisi donanımları korunuyor. Benzer şekilde, Windows iş için Hello teknolojisi, Microsoft Authenticator uygulamasını anahtar tabanlı kimlik doğrulaması, bir mobil cihaza bağlanır ve bir biyometrik veya PIN kodu kullanan bir kullanıcı kimlik bilgilerini sağlamak için kullanır. Artık FIDO2 güvenlik anahtarları, bilginizle yürütmek ve kilit ekranında Windows 10 kimlik bilgisi sağlayıcısı güvenlik anahtarı seçerek Azure AD'de oturum olanak sağlar. Parolasız bu çözümleri üç kimlik avı, parola ilaç riskini azaltmak ve yeniden yürütme saldırıları ve kullanıcılar oturum açın ve verilere her yerden erişmek için yüksek oranda güvenli ve kolay bir yol sağlar.

Biyometri ve ortak anahtar şifrelemesinde yaygın olarak erişilebilir cihazlar gibi modern çok faktörlü kimlik doğrulaması Teknoloji benimseme atayamayacağına şirketin kimlik riskini azaltabilirsiniz en önemli adımlardan biridir. Güvenli kimlik doğrulaması için uzun vadeli bir yaklaşım olduğundan parolasız giderek ve hala geliştirilmektedir. Gelişen gereksinimlerini göz önünde bulundurulduğunda, kuruluşların kendileri için parolasız teknolojileri taşımaya başlamak için bir plan yaparak hazırlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Genel bir bakış [parolasız nedir?](https://docs.microsoft.com/azure/active-directory/authentication/concept-authentication-passwordless)
* [Azure AD'de parolasız etkinleştirme](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-passwordless-enable)
