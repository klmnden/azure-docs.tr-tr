---
title: Azure multi-Factor Authentication - Azure Active Directory için Güvenlik Kılavuzu
description: Bu belgede Azure hesapları ile Azure mfa'yı kullanarak geçici bir Rehber sağlanır
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 436b7899b1a9d4f9cab1ca2581ff9b5b162de8ac
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58368221"
---
# <a name="security-guidance-for-using-azure-multi-factor-authentication-with-azure-ad-accounts"></a>Güvenlik Kılavuzu, Azure AD hesapları ile Azure multi-Factor Authentication kullanma

İki aşamalı doğrulama, kendi kimlik doğrulama işlemi iyileştirmek istiyorsanız çoğu kuruluş için tercih edilen seçenektir. Azure multi-Factor Authentication (MFA) kullanıcıları için basit bir oturum açma deneyimi sunarken, güvenlik ve uyumluluk gereksinimlerini karşılamak şirketler yardımcı olur. Bu makalede, Azure MFA'ın benimseme için planlama yaparken göz önünde bulundurmanız gereken bazı ipuçları verilmektedir.

## <a name="deploy-azure-mfa-in-the-cloud"></a>Bulutta Azure MFA dağıtma

İçin iki yolla [tüm kullanıcılarınız için Azure mfa'yı etkinleştirme](howto-mfa-getstarted.md).

* (Ya da Azure MFA, Azure AD Premium veya Enterprise Mobility + Security) her kullanıcı için lisans satın alın
* Bir multi-Factor Auth sağlayıcısı ve ödeme kullanıcı başına veya kimlik doğrulaması başına oluşturma

### <a name="licenses"></a>Lisanslar

![Lisansları kullanıcılara uygulamak, etkinleştirin ve bildir](./media/multi-factor-authentication-security-best-practices/ems.png)

Azure AD Premium veya Enterprise Mobility + Security lisansları varsa Azure mfa'yı zaten sahip. Kuruluşunuz, tüm kullanıcılar için iki aşamalı doğrulama özelliğini genişletmek için ek herhangi bir şey gerekmez. Yalnızca bir kullanıcıya bir lisans ataması gerekir ve ardından MFA'yı etkinleştirebilirsiniz.

Çok faktörlü kimlik doğrulamasını ayarlama, aşağıdaki ipuçlarını göz önünde bulundurun:

* Kimlik doğrulaması başına multi-Factor Auth sağlayıcısı oluşturmayın. Bunu yaparsanız, doğrulama istekleri zaten lisanslara sahip kullanıcılardan ödeme son.
* Tüm kullanıcılar için yeterince lisansa sahip değilseniz, bir kullanıcı başına çok faktörlü yetki Sağlayıcı'nın kuruluşunuzun rest kapsayacak şekilde oluşturabilirsiniz. 
* Azure AD Connect, yalnızca bir Azure AD diziniyle eşitliyorsanız şirket içi Active Directory ortamınızı gerekli. Şirket içi Active Directory örneği ile eşitlenmemiş ise bir Azure AD dizini kullanıyorsanız, Azure AD Connect ihtiyacınız yoktur.

### <a name="multi-factor-auth-provider"></a>Multi-Factor Auth sağlayıcısı

![Multi-Factor Authentication sağlayıcısı](./media/multi-factor-authentication-security-best-practices/authprovider.png)

Azure mfa'yı içeren lisansları yoksa sonra yapabilecekleriniz [MFA kimlik doğrulama sağlayıcısı oluşturma](concept-mfa-authprovider.md).

Kimlik doğrulama sağlayıcısı oluştururken, bir dizin seçin ve aşağıdaki ayrıntıları göz önünde bulundurun gerekir:

* Multi-Factor Auth sağlayıcısı oluşturmak için Azure AD dizini gerekmez, ancak daha fazla işlevsellik biriyle alın. Kimlik doğrulama sağlayıcısı bir Azure AD dizini ile ilişkilendirdiğinizde, aşağıdaki özellikleri etkinleştirilir:
  * Tüm kullanıcılarınız için iki aşamalı doğrulamayı genişletmek
  * Genel Yöneticiler için Yönetim Portalı, özel karşılamalar ve raporlar gibi ek özellikler sunar.
* Şirket içi Active Directory ortamınızı bir Azure AD dizini ile eşitleyin, DirSync veya AAD eşitleme gerekir. Şirket içi Active Directory örneği ile eşitlenmemiş ise bir Azure AD dizini kullanıyorsanız, DirSync veya AAD eşitleme ihtiyacınız yoktur.
* İşletmenizi en uygun kullanım modeli seçin. Kullanım modeli seçin sonra değiştiremezsiniz. İki modeli vardır:
  * Kimlik doğrulaması başına: her doğrulama için ücretleri. Belirli kullanıcılar için değil, belirli bir uygulama erişen herkesin için iki aşamalı doğrulama istiyorsanız bu modeli kullanın.
  * Etkin kullanıcı başına: her kullanıcı için Azure mfa'yı etkinleştirmek için ücretleri. Azure AD Premium veya Enterprise Mobility Suite lisansı bulunan bazı kullanıcılarınızın ve bazı olmadan varsa bu modeli kullanın.

### <a name="supportability"></a>Desteklenebilirlik

Çoğu kullanıcı kimlik doğrulaması için yalnızca parola kullanmaya alışkın olduğundan, şirketiniz bu işlem ile ilgili tüm kullanıcılar için tanıma getirir önemlidir. Bu tanıma, kullanıcılar için mfa'yı ilgili önemsiz sorunlar için Yardım Masasını arayın. olasılığını azaltabilirsiniz. Ancak, geçici olarak mfa'yı devre dışı bırakma gerekli olduğu bazı senaryolar vardır. Bu senaryoları nasıl ele alınacağını anlamak için aşağıdaki yönergeleri kullanın:

* Mobil uygulaması ya da telefon bir bildirim ya da telefon aramasına almadığını çünkü burada kullanıcı oturum açamaz senaryoları işlemek için teknik destek ekibinize eğitin. Teknik destek için [bir kerelik geçiş etkinleştirme](howto-mfa-mfasettings.md#one-time-bypass) "iki aşamalı doğrulamayı atlayarak" bir kereliğine kimlik doğrulaması yapmalarına izin vermek için. Geçiş geçicidir ve belirtilen sayıda saniye geçtikten sonra süresi dolar.
* Göz önünde bulundurun [güvenilen IP'ler özelliği](howto-mfa-mfasettings.md#trusted-ips) iki aşamalı doğrulamayı en aza indirmek için bir yol olarak Azure mfa'yı içinde. Bu özellik ile yönetilen veya Federasyon Kiracı yöneticileri şirketin yerel Intranet üzerinden oturum açan kullanıcılar için iki aşamalı doğrulamayı atlayabilir. Özellikler, Azure AD Premium, Enterprise Mobility Suite ve Azure multi-Factor Authentication lisansınız Azure AD kiracıları için kullanılabilir.

## <a name="best-practices-for-an-on-premises-deployment"></a>Bir şirket içi dağıtımı için en iyi yöntemler

Mfa'yı etkinleştirmek için kendi altyapınızda şirketinizin karar sonra yapmanız [Azure multi-Factor Authentication sunucusu şirket içi dağıtma](howto-mfaserver-deploy.md). MFA sunucusu bileşenleri Aşağıdaki diyagramda gösterilmiştir:

![Varsayılan MFA sunucusu bileşenleri](./media/multi-factor-authentication-security-best-practices/server.png) \*varsayılan olarak yüklü olmayan \** varsayılan olarak etkin değildir ancak yüklü

Azure multi-Factor Authentication sunucusu bulut Federasyon kullanarak kaynakları ve şirket kaynaklarının güvenliğini sağlayabilirsiniz. AD FS yüklü ve Azure AD kiracınız ile Federasyon olması gerekir.
Çok faktörlü kimlik doğrulama sunucusu Kurulumu aşağıdaki ayrıntıları dikkate alın:

* İlk doğrulama adımı gerçekleştirilir, Active Directory Federasyon Hizmetleri (AD FS) kullanarak Azure AD kaynaklarını güvenli hale getirme, AD FS kullanarak şirket içi. İkinci adım, talebin onaylanmasıyla şirket içinde gerçekleştirilir.
* Azure multi-Factor Authentication sunucusu AD FS federasyon sunucunuza yüklemeniz gerekmez. Ancak, bir Windows Server 2012 AD FS çalıştıran R2 üzerinde AD FS için multi-Factor Authentication bağdaştırıcısı yüklü olmalıdır. Desteklenen bir sürüm olduğu sürece sunucuyu farklı bir bilgisayara yükleyin ve AD FS bağdaştırıcısını ayrı olarak AD FS federasyon sunucunuza yükleyin. 
* Multi-Factor Authentication AD FS Bağdaştırıcısı Yükleme Sihirbazı Active Directory'nizde PhoneFactor Admins adlı bir güvenlik grubu oluşturur ve ardından, AD FS hizmeti hesabını bu gruba ekler. Etki alanı denetleyicinizde PhoneFactor Admins grubunun oluşturulduğunu ve AD FS hizmet hesabının bu gruba üye olduğunu doğrulayın. Gerekirse, AD FS hizmeti hesabınızı etki alanı denetleyicinizde PhoneFactor Admins grubuna el ile ekleyin.

### <a name="user-portal"></a>Kullanıcı Portalı

Kullanıcı Portalı Self Servis özellikleri ve tam bir dizi kullanıcı yönetim özelliği sağlar. Bu işlem, bir Internet Information Server (IIS) web sitesinde çalışır. Bu bileşen yapılandırmak için aşağıdaki yönergeleri kullanın:

* IIS 6 ya da üstünü kullanın
* Yükleme ve ASP.NET v2.0.507207 kaydetme
* Bu sunucuyu bir çevre ağında dağıtılan emin olun

### <a name="app-passwords"></a>Uygulama Parolaları

Kuruluşunuz SSO Azure AD ile birleştirildiyse ve Azure mfa'yı kullanıyor olacak, sonra aşağıdaki ayrıntılara dikkat edin:

* Uygulama parolası, Azure AD tarafından doğrulanır ve bu nedenle, Federasyon atlar. Federasyon yalnızca uygulama parolaları ayarlanırken kullanılır.
* Federasyon (SSO) kullanıcılar için parolalar Kurumsal kimlikte depolanır. Kullanıcının şirketten ayrılması durumunda, bu bilgileri DirSync kullanılarak kurumsal Kimliğe için akış gerekir. Hesabı devre dışı bırakma/silme işlemi devre dışı bırakma/silme, uygulama parolaları Azure AD'ye geciktirir eşitleme, üç saate kadar sürebilir.
* Şirket için İstemci Erişimi Denetimi ayarları Uygulama Parolası tarafından onaylanmaz.
* Günlüğe kaydetme ve denetim şirket içi kimlik doğrulama yeteneği, uygulama parolaları için kullanılabilir.
* Burada kimlik doğrulaması ile bağlı istemciler, iki aşamalı doğrulamayı kullanırken Kurumsal kullanıcı adı ve parolaları ve uygulama parolaları oluşan birleşimlerin kullanıldığı bazı gelişmiş Mimari Tasarım gerektirebilir. Bir şirket içi altyapı karşı kimlik doğrulaması istemcileri için bir kuruluş kullanıcı adı ve parola kullanırsınız. Azure AD karşı kimlik doğrulaması istemcileri için uygulama parolasını kullanmanız gerekir.
* Varsayılan olarak, kullanıcılar uygulama parolaları oluşturamaz. Kullanıcıların uygulama parolaları, select oluşturmasına izin vermeniz gerekiyorsa **kullanıcıların tarayıcı olmayan uygulamalara oturum açmak için uygulama parolaları oluşturmasına izin** seçeneği.

## <a name="additional-considerations"></a>Dikkat edilecek diğer noktalar

Ek hususlar için bu listeyi kullanın ve şirket içi yönergeler her bileşeni için dağıtılabilir:

* Azure Multi-Factor Authentication’ı [Active Directory Federasyon Hizmetleri](multi-factor-authentication-get-started-adfs.md) ile ayarlayın.
* [RADIUS Kimlik Doğrulaması](howto-mfaserver-dir-radius.md) ile Azure MFA Sunucusu’nu kurun ve yapılandırın.
* Ayarlama ve Azure MFA sunucusu ile yapılandırma [IIS kimlik doğrulaması](howto-mfaserver-iis.md).
* Ayarlama ve Azure MFA sunucusu ile yapılandırma [Windows kimlik doğrulaması](howto-mfaserver-windows.md).
* Ayarlama ve Azure MFA sunucusu ile yapılandırma [LDAP kimlik doğrulaması](howto-mfaserver-dir-ldap.md).
* Ayarlama ve Azure MFA sunucusu ile yapılandırma [Uzak Masaüstü Ağ geçidi ve Azure multi-Factor Authentication sunucusu RADIUS kullanan](howto-mfaserver-nps-rdg.md).
* Ayarlama ve Azure MFA sunucusu arasında eşitlemeyi yapılandırın ve [Windows Server Active Directory](howto-mfaserver-dir-ad.md).
* [Azure Multi-Factor Authentication Sunucusu Mobil Uygulama Web Hizmeti’ni dağıtın](howto-mfaserver-deploy-mobileapp.md).
* [Gelişmiş VPN yapılandırma Azure multi-Factor Authentication ile](howto-mfaserver-nps-vpn.md) LDAP veya RADIUS kullanarak Cisco ASA, Citrix Netscaler ve Juniper/Pulse Secure VPN cihazları için.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure MFA için bazı en iyi vurgulanıyor olsa da, MFA dağıtım planlaması sırasında kullanabileceğiniz diğer kaynak yok. Aşağıdaki listede, bu işlem sırasında yardımcı olabilecek bazı önemli makaleler vardır:

* [Azure multi-Factor authentication'da raporları](howto-mfa-reporting.md)
* [İki aşamalı doğrulama kayıt deneyimi](../user-help/multi-factor-authentication-end-user-first-time.md)
* [Azure multi-Factor Authentication SSS](multi-factor-authentication-faq.md)
