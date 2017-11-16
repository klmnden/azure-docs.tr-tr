---
title: "MFA için en iyi yöntemler | Microsoft Docs"
description: "Bu belgede Azure MFA ile Azure hesaplarını kullanarak geçici en iyi yöntemler sağlar"
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 3be7d968-96bb-4320-8701-869fd04a2595
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: joflore
ms.reviewer: richagi
ms.custom: it-pro
ms.openlocfilehash: 2be36bce1b4cffdab2d25d150bd5a0e8451e422d
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="security-best-practices-for-using-azure-multi-factor-authentication-with-azure-ad-accounts"></a>Azure AD hesapları ile Azure çok faktörlü kimlik doğrulaması kullanmak için güvenlik en iyi uygulamalar

İki aşamalı doğrulama, kimlik doğrulama işlemi geliştirmek istiyor çoğu kuruluş için tercih edilen seçimdir. Azure multi-Factor Authentication (MFA), kullanıcılar için basit bir oturum açma deneyimi sağlarken kendi güvenlik ve uyumluluk gereksinimlerini karşılayan şirketler yardımcı olur. Bu makalede, Azure MFA benimseme için planlarken göz önünde bulundurmanız bazı ipuçları yer almaktadır.

## <a name="deploy-azure-mfa-in-the-cloud"></a>Bulutta Azure MFA dağıtma

Tüm kullanıcılar için Azure MFA'yı etkinleştirmek için iki yolu vardır.

* Her bir kullanıcı (ya da Azure MFA, Azure AD Premium veya Enterprise Mobility + Security) için lisans satın alma
* Multi-Factor Auth sağlayıcısı ve ödeme kullanıcı başına veya kimlik doğrulaması başına oluşturur.

### <a name="licenses"></a>Lisanslar
![EMS](./media/multi-factor-authentication-security-best-practices/ems.png)

Azure AD Premium veya Enterprise Mobility + güvenlik lisansları varsa zaten Azure MFA gerekir. Kuruluşunuzun tüm kullanıcılar için iki aşamalı doğrulama özelliği genişletmek için ek bir şey gerekmez. Yalnızca bir kullanıcıya bir lisans atamanız gerekir ve ardından MFA üzerinde kapatabilirsiniz.

Çok faktörlü kimlik doğrulamayı ayarlama, aşağıdaki ipuçlarını göz önünde bulundurun:

* Bir kimlik doğrulaması başına çok faktörlü yetki sağlayıcı oluşturmayın. Bunu yaparsanız, doğrulama istekleri zaten lisanslara sahip kullanıcılardan ödeme bitiş.
* Tüm kullanıcılar için yeterince lisansa sahip değilseniz, bir kullanıcı başına çok faktörlü yetki Sağlayıcı'nın, kuruluşunuzun kalan kapsayacak şekilde oluşturabilirsiniz. 
* Azure AD Connect, yalnızca bir Azure AD dizini ile şirket içi Active Directory ortamınızı eşitlenip eşitlenmediğini gerekli. Şirket içi örneğini Active Directory ile eşitlenmemiş bir Azure AD dizini kullanıyorsanız, Azure AD Connect gerekmez.

### <a name="multi-factor-auth-provider"></a>Multi-Factor Auth sağlayıcısı
![Multi-Factor Auth sağlayıcısı](./media/multi-factor-authentication-security-best-practices/authprovider.png)

Daha sonra Azure MFA dahil lisansları yoksa, MFA kimlik doğrulama sağlayıcısı oluşturabilirsiniz. 

Kimlik doğrulama sağlayıcısı oluştururken, bir dizin seçin ve aşağıdaki ayrıntıları göz önünde bulundurun gerekir:

* Multi-Factor Auth sağlayıcısı oluşturmak için Azure AD dizini gerekli değildir, ancak daha fazla işlevsellik biriyle alın. Auth sağlayıcısı Azure AD dizini ile ilişkilendirdiğinizde, aşağıdaki özellikler etkindir:  
  * Tüm kullanıcılarınız için iki aşamalı doğrulamayı genişletme  
  * Genel Yöneticiler Yönetim Portalı, özel Karşılama ve raporlar gibi ek özellikler sunar.
* Şirket içi Active Directory ortamınızı bir Azure AD diziniyle eşitliyorsanız DirSync veya AAD eşitleme gerekir. Şirket içi örneğini Active Directory ile eşitlenmemiş bir Azure AD dizini kullanıyorsanız, DirSync veya AAD eşitleme gerekmez.
* En iyi şekilde, iş gereksinimlerinize en uygun tüketim modelini seçin. Kullanım modeli seçtikten sonra değiştiremezsiniz. İki model şunlardır:
  * Kimlik doğrulaması başına: her doğrulama için giderler. Bu model, belirli kullanıcılar için değil, belirli bir uygulama erişen herkesin için iki aşamalı doğrulamayı istiyorsanız kullanın.
  * Etkin kullanıcı başına: Azure MFA için etkinleştirme her bir kullanıcı için giderler. Bazı kullanıcılar Azure AD Premium veya Enterprise Mobility Suite lisansları ve bazı olmadan varsa, bu model kullanın.

### <a name="supportability"></a>Desteklenebilirlik
Kimlik doğrulaması için yalnızca parola kullanarak kullanıcıların çoğunun alışmanızı olduğundan, şirketiniz bu işlemi ile ilgili tüm kullanıcılara tanıma getirir önemlidir. Bu tanıma kullanıcılar için MFA ilişkili ikincil sorunları için Yardım masanıza başvurun olasılığını azaltır. Ancak, geçici olarak MFA devre dışı bırakma gerekli olduğu bazı senaryolar vardır. Bu senaryoları nasıl ele alınacağını anlamak için aşağıdaki kılavuzları kullanın:

* Mobil uygulama ya da telefon bildirim veya telefon görüşmesi almıyor olduğundan, kullanıcı oturum açamaz senaryoları işlemek için teknik destek ekibinize eğitmek. Teknik destek için [bir kerelik geçiş etkinleştirmek](multi-factor-authentication-whats-next.md#one-time-bypass) "iki aşamalı doğrulamayı atlayarak" bir kereliğine kimlik doğrulaması yapmalarına izin vermek için. Atlama geçicidir ve belirtilen sayıda saniye geçtikten sonra süresi dolar.
* Göz önünde bulundurun [güvenilen IP'leri yetenek](multi-factor-authentication-whats-next.md#trusted-ips) iki aşamalı doğrulamayı en aza indirmek için bir yol olarak Azure MFA içinde. Bu özellik ile yönetilen veya Federasyon Kiracı Yöneticiler şirketin yerel intranetten imzalama kullanıcılar için iki aşamalı doğrulamayı devre dışı bırakabilir. Özellikler, Azure AD Premium, Enterprise Mobility Suite ya da Azure multi-Factor Authentication lisanslara sahip Azure AD kiracılar için kullanılabilir.

## <a name="best-practices-for-an-on-premises-deployment"></a>Bir şirket içi dağıtım için en iyi yöntemler
MFA'yı etkinleştirmek için kendi altyapınızda şirketiniz karar verdiyseniz, Azure multi-Factor Authentication sunucusu şirket içi dağıtmak için gerekir. MFA sunucusu bileşenlerini aşağıdaki çizimde gösterilmektedir:

![Varsayılan MFA sunucusu bileşenleri: konsol, eşitleme altyapısı, Yönetim Portalı, bulut hizmeti](./media/multi-factor-authentication-security-best-practices/server.png) \*varsayılan olarak yüklenmeyen \** varsayılan olarak etkin değildir ancak yüklü

Azure multi-Factor Authentication sunucusu bulut Federasyon kullanarak kaynaklarına ve şirket içi kaynakların güvenliğini sağlayabilirsiniz. AD FS sahip ve Azure AD kiracınıza Federasyon olması gerekir.
Çok faktörlü kimlik doğrulama sunucusu kurma, aşağıdaki ayrıntıları göz önünde bulundurun:

* İlk doğrulama adımı gerçekleştirilen sonra Active Directory Federasyon Hizmetleri (AD FS) kullanarak Azure AD kaynaklarını güvenli hale getirme, AD FS kullanarak şirket içi. İkinci adım, talebin onaylanmasıyla şirket içinde gerçekleştirilir.
* Azure multi-Factor Authentication sunucusu, AD FS federasyon sunucusu yüklemeniz gerekmez. Ancak, AD FS için multi-Factor Authentication bağdaştırıcısı bir Windows Server 2012 AD FS çalıştıran R2 üzerinde yüklü olmalıdır. Desteklenen bir sürüm olduğu sürece sunucuyu farklı bir bilgisayara yükleyin ve AD FS bağdaştırıcısını ayrı olarak, AD FS federasyon sunucusuna yükleyin. 
* Multi-Factor Authentication AD FS Bağdaştırıcısı Yükleme Sihirbazı'nı, Active Directory'de PhoneFactor Admins adlı bir güvenlik grubu oluşturur ve ardından AD FS hizmeti hesabını bu gruba ekler. Etki alanı denetleyicinizde PhoneFactor Admins grubunun oluşturulduğunu ve AD FS hizmet hesabının bu gruba üye olduğunu doğrulayın. Gerekirse, AD FS hizmeti hesabınızı etki alanı denetleyicinizde PhoneFactor Admins grubuna el ile ekleyin.

### <a name="user-portal"></a>Kullanıcı Portalı
Kullanıcı Portalı Self Servis kapasitelerini ve tam bir kullanıcı yönetimi özellik kümesi sağlar. Bir Internet Information Server (IIS) web sitesini çalıştırır. Bu bileşen yapılandırmak için aşağıdaki yönergeleri kullanın:

* IIS 6 veya daha büyük kullanın
* Yükleyin ve ASP.NET v2.0.507207 kaydedin
* Bu sunucuyu bir çevre ağında dağıtılan emin olun

### <a name="app-passwords"></a>Uygulama Parolaları
Kuruluşunuz SSO Azure AD ile birleştirildiyse ve Azure MFA kullanma olacak, ardından aşağıdaki ayrıntıları dikkat edin:

* Uygulama parolası, Azure AD tarafından doğrulanır ve bu nedenle Federasyon atlar. Federasyon yalnızca uygulama parolaları ayarlarken kullanılır.
* Federasyon (SSO) kullanıcıları için parolalar kuruluş kimliği saklanır. Kullanıcının şirketten ayrılması durumunda DirSync kullanılarak kurumsal kimliğe akış bu bilgileri vardır. Hesap devre dışı bırakma/silme, devre dışı bırakma/silme, uygulama parolaları Azure AD'de gecikmeler eşitleme için en fazla üç saat sürebilir.
* Şirket için İstemci Erişimi Denetimi ayarları Uygulama Parolası tarafından onaylanmaz.
* Günlüğe kaydetme ve denetim şirket içi kimlik doğrulama özelliği için uygulama parolaları kullanılabilir.
* Bazı gelişmiş mimari tasarımları istemcilerle burada kimlik doğrulamasında bağlı olarak iki aşamalı doğrulamayı kullanırken, kuruluş kullanıcı adı ve parolaları ve uygulama parolaları birleşimini kullanarak gerektirebilir. Bir şirket içi altyapı karşı kimlik doğrulaması istemcileri için bir kuruluş kullanıcı adı ve parola kullanırsınız. Azure AD karşı kimlik doğrulaması istemcileri için uygulama parolası kullanırsınız.
* Varsayılan olarak, kullanıcıların uygulama parolaları oluşturulamıyor. Kullanıcıların uygulama parolaları, select oluşturmasına izin vermeniz gerekiyorsa, **kullanıcıların tarayıcı olmayan uygulamalara oturum açmak için uygulama parolaları oluşturmasına izin** seçeneği.

## <a name="additional-considerations"></a>Ek hususlar
Ek hususlar için bu listeyi kullanın ve şirket içi yönergeler her bileşen için dağıtılabilir:

- Azure Multi-Factor Authentication’ı [Active Directory Federasyon Hizmetleri](multi-factor-authentication-get-started-adfs.md) ile ayarlayın.
- [RADIUS Kimlik Doğrulaması](multi-factor-authentication-get-started-server-radius.md) ile Azure MFA Sunucusu’nu kurun ve yapılandırın.
- Ayarlama ve yapılandırma ile Azure MFA sunucusu [IIS kimlik doğrulaması](multi-factor-authentication-get-started-server-iis.md).
- Ayarlama ve yapılandırma ile Azure MFA sunucusu [Windows kimlik doğrulaması](multi-factor-authentication-get-started-server-windows.md).
- Ayarlama ve yapılandırma ile Azure MFA sunucusu [LDAP kimlik doğrulaması](multi-factor-authentication-get-started-server-ldap.md).
- Ayarlama ve yapılandırma ile Azure MFA sunucusu [Uzak Masaüstü Ağ geçidi ve Azure multi-Factor Authentication sunucusu RADIUS kullanan](multi-factor-authentication-get-started-server-rdg.md).
- Ayarlama ve Azure MFA sunucusu arasında eşitlemeyi yapılandırın ve [Windows Server Active Directory](multi-factor-authentication-get-started-server-dirint.md).
- [Azure Multi-Factor Authentication Sunucusu Mobil Uygulama Web Hizmeti’ni dağıtın](multi-factor-authentication-get-started-server-webservice.md).
- [Gelişmiş VPN yapılandırma Azure çok faktörlü kimlik doğrulamasıyla](multi-factor-authentication-advanced-vpn-configurations.md) LDAP veya RADIUS kullanarak Cisco ASA, Citrix Netscaler ve Juniper/Pulse Secure VPN cihazları için.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure MFA için bazı en iyi uygulamaları vurgular olsa da, MFA dağıtımınızı planlarken kullanabileceğiniz kaynaklar vardır. Aşağıdaki liste, bu süreçte yardımcı olabilecek bazı anahtar makaleler vardır:

* [Azure multi-Factor Authentication raporlarında](multi-factor-authentication-manage-reports.md)
* [İki aşamalı doğrulama kayıt deneyimi](multi-factor-authentication-end-user-first-time.md)
* [Azure çok faktörlü kimlik doğrulaması ile ilgili SSS](multi-factor-authentication-faq.md)

