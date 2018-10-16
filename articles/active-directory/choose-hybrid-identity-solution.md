---
title: Bir Azure karma kimlik çözümü seçin | Microsoft Docs
description: Kullanılabilir karma kimlik çözümleri ve öneriler, kuruluşunuz için en iyi kimlik İdaresi karar vermeniz temel bir anlayış alın.
keywords: ''
author: jeffgilb
manager: mtillman
ms.reviewer: jsnow
ms.author: billmath
ms.date: 03/02/2018
ms.topic: article
ms.prod: ''
ms.service: azure
ms.technology: ''
ms.assetid: ''
ms.custom: it-pro
ms.openlocfilehash: 15a28cd5937be103aac888a5fc5485e39854a1b4
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49319957"
---
# <a name="microsoft-hybrid-identity-solutions"></a>Microsoft karma kimlik çözümleri

[Microsoft Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) karma kimlik çözümleri, şirket içi dizin nesnelerini yine de kullanıcılara şirket içi yönetme sırasında Azure AD ile eşitlemek etkinleştirin. Şirket içi Windows Server Active Directory'nizi Azure AD ile eşitlemek planlama kullanmak isteyip istemediğinizi yapmak için ilk karar kimlik eşitlenen ya da Federasyon kimliği. Eşitlenen kimlikler ve isteğe bağlı parola karmaları hem şirket içi hem de bulut tabanlı kurumsal kaynaklara erişmek için aynı parolayı kullanmalarına olanak sağlar. Çoklu oturum açma (SSO) veya şirket içi MFA gibi daha gelişmiş senaryo gereksinimleri için Active Directory Federasyon Hizmetleri (kimlikleri federasyona eklemek için AD FS) dağıtmanız gerekir. 

Karma kimlik yapılandırmak için kullanılabilen birkaç seçeneğiniz vardır. Bu makalede, dağıtım kolaylığı kuruluşunuz için en iyi seçmenize yardımcı olacak bilgiler sağlar ve belirli kimlik ve erişim yönetimi gerekiyor. En iyi hangi kimlik modeli kuruluşunuzun gereksinimlerine en uygun göz önünde bulundurun gibi ayrıca zaman, mevcut altyapı, karmaşıklığı ve maliyeti hakkında düşünmek gerekir. Bu faktörlerin her kuruluş için farklıdır ve zaman içinde değişebilir. Ancak, gereksinimlerinizi değiştirirseniz, bu da farklı bir kimlik modeline geçiş yapmak için esnekliğine sahipsiniz.

> [!TIP]
> Bu çözümler tüm tarafından sunulur [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).
>

## <a name="synchronized-identity"></a>Eşitlenen bir kimlik 

Eşitlenmiş şirket içi dizin nesnelerini (kullanıcılar ve gruplar) eşitlemek için en kolay yolu Azure AD'ye kimliğidir. 

![Eşitlenmiş karma kimlik](./media/choose-hybrid-identity-solution/synchronized-identity.png)

Eşitlenmiş kimlik kolay ve hızlı yöntem olsa da, kullanıcılarınızın yine de bulut tabanlı kaynaklar için ayrı bir parola sürdürmeniz gerekir. Bunu önlemek için ayrıca (isteğe bağlı olarak) [kullanıcı parola karması eşitleme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-synchronization#what-is-password-synchronization) Azure AD dizininize. Parola karmalarının eşitleme, kullanıcıların bulut tabanlı kurumsal kaynaklara aynı kullanıcı adı ve şirket içinde kullandıkları parolayla oturum sağlar. Azure AD Connect, şirket içi dizininiz ve Azure AD dizinini eşitlenmiş tutar değişiklikler için düzenli olarak denetler. Bir kullanıcı özniteliği veya parola değiştirilen şirket içi Active Directory olduğunda, Azure AD'de otomatik olarak güncelleştirilir. 

Yalnızca Office 365, SaaS uygulamaları ve diğer Azure AD tabanlı kaynakları için oturum açmak kullanıcıları etkinleştirmek için gereken çoğu kuruluş için varsayılan parola eşitleme seçeneği önerilir. Sizin için işe yaramazsa, geçişli kimlik doğrulaması ve AD FS arasında karar vermeniz gerekir.

> [!TIP]
> Kullanıcı parolalarını şirket içi Windows Server Active Directory asıl kullanıcı parolayı temsil eden bir karma değer biçiminde depolanır. Bir karma değer, tek yönlü matematiksel işlev (karma algoritma) sonucudur. Parola düz metin sürümüne bir tek yönlü işlevin sonucu geri almak için bir yöntem yoktur. Şirket içi ağınızda oturum açmak için parola karması kullanamazsınız. Parola Eşitleme için iyileştirilmiş, Azure AD Connect şirket içi Active Directory'den parola karmalarının ayıklar ve ek güvenlik için parola karmasını Azure AD'ye eşitlenen önce işlem uygular. Parola Eşitleme, ayrıca Self Servis parola sıfırlama Azure AD'de etkinleştirmek için parola geri yazma ile birlikte kullanılabilir. Ayrıca, kullanıcılar şirket ağına bağlanan etki alanına katılmış bilgisayarlarda için çoklu oturum açma (SSO) etkinleştirebilirsiniz. Çoklu oturum açma ile etkin kullanıcılar yalnızca bulut kaynaklarını güvenli bir şekilde erişmek için bir kullanıcı adı girmeniz gerekir. 
>

## <a name="pass-through-authentication"></a>Geçişli kimlik doğrulaması

[Azure AD geçişli kimlik doğrulaması](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication) kullanarak şirket içi Active Directory'nizi Azure AD tabanlı hizmetler için bir basit parola doğrulama çözümü sağlar. Kuruluşunuz için güvenlik ve uyumluluk ilkeleri, kullanıcıların parolalarını bile karma bir biçimde gönderme vermez ve etki alanına katılmış cihazlar için Masaüstü SSO'yu desteklemek yeterlidir, geçişli kimlik doğrulaması kullanarak değerlendirmeniz önerilir. Geçişli kimlik doğrulaması, AD FS ile karşılaştırıldığında dağıtım altyapısına basitleştiren dmz'deki herhangi bir dağıtıma gerektirmez. Bu kimlik doğrulama yöntemi, kullanıcılar Azure AD kullanarak oturum açtığında, kullanıcıların parolalarını şirket içi Active Directory'nizde doğrudan karşı doğrular.

![Geçişli kimlik doğrulaması](./media/choose-hybrid-identity-solution/pass-through-authentication.png)

Geçişli kimlik doğrulaması ile karmaşık ağ altyapısı için gerek yoktur ve şirket içi parolalarını bulutta depolamak gerekmez. Çoklu oturum açma ile birlikte geçişli kimlik doğrulaması, Azure AD'de oturum açarken tamamen tümleşik bir deneyim sağlar veya diğer bulut Hizmetleri.

Geçişli kimlik doğrulaması, parola doğrulama isteklerini dinleyen bir basit şirket içi Aracısı kullanan Azure AD Connect ile yapılandırılır. Aracı Yük Dengeleme ve yüksek kullanılabilirlik sağlamak için birden çok makineye kolayca dağıtılabilir. Tüm iletişimler giden olduğundan, yalnızca bir DMZ'ye yüklenmesi Bağlayıcısı gereksinimi yoktur. Bağlayıcı sunucu bilgisayar gereksinimleri aşağıdaki gibidir:

- Windows Server 2012 R2 veya üzeri
- Kullanıcılara doğrulanmış ormanındaki bir etki alanına katıldı

## <a name="federated-identity-ad-fs"></a>Federe kimlik (AD FS)

Kullanıcıların Office 365 ve diğer bulut hizmetlerine erişme üzerinde daha fazla denetim için çoklu oturum açma (SSO) kullanarak dizin eşitlemeyi ayarlayabilirsiniz [Active Directory Federasyon Hizmetleri (AD FS)](https://docs.microsoft.com/windows-server/identity/ad-fs/overview/whats-new-active-directory-federation-services-windows-server). Kullanıcı oturum açma işlemleri AD FS ile Federasyon kimlik doğrulaması kullanıcı kimlik bilgilerini doğrular bir şirket içi sunucusuna aktarır. Bu modelde, şirket içi Active Directory kimlik bilgilerini asla Azure AD'ye geçirilir.

![Federal Kimlik](./media/choose-hybrid-identity-solution/federated-identity.png)

Kimlik Federasyonu olarak da bilinen bu oturum açma yöntemi, tüm kullanıcı kimlik doğrulaması denetimli şirket içinde ve daha ayrıntılı düzeyde erişim denetimi uygulamak yöneticilerin sağlayan sağlar. AD FS ile Federasyon kimlik en karmaşık seçenek ve şirket içi ortamınızda ek sunucuları dağıtma gerektirir. Kimlik Federasyonu Ayrıca, Active Directory ve AD FS altyapınızı için 24 x 7 destek sağlamayı taahhüt eder. Şirket içi İnternet'e erişimi engelliyorsa, etki alanı denetleyicisi veya AD FS sunucuları kullanıcılar oturum açamıyorum bulut Hizmetleri için kullanılamıyor, çünkü bu yüksek seviyedeki destektir gereklidir.

> [!TIP]
> Active Directory Federasyon Hizmetleri (AD FS) ile Federasyon kullanmaya karar verirseniz, AD FS altyapınızı başarısız olursa, isteğe bağlı olarak parola eşitleme yedek olarak ayarlayabilirsiniz.
>

## <a name="common-scenarios-and-recommendations"></a>Ortak senaryolar ve öneriler

Aşağıda, bazı yaygın karma kimlik ve erişim yönetimi senaryoları için hangi karma kimlik seçeneği (veya Seçenekler) her biri için uygun olabileceği hakkında önerilerle birlikte verilmiştir.

|İstiyorum:|PWS ve SSO<sup>1</sup>| PTA ve SSO<sup>2</sup> | AD FS<sup>3</sup>|
|-----|-----|-----|-----|
|Yeni kullanıcı, kişi ve grup hesapları şirket içi Active Directory dizinimde buluta otomatik olarak oluşturulan eşitleyin.|![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png)| ![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png) |![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Kiracıma Office 365 karma senaryolar için ayarlayın|![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png)| ![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png) |![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Kullanıcılarım oturum açın ve şirket içi parolalarını kullanarak bulut hizmetlerine erişmek etkinleştirme|![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png)| ![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png) |![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Tek şirket kimlik bilgilerini kullanarak oturum açmayı uygulamak|![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png)| ![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png) |![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Bulutta hiç parola karmaları depolanır emin olun.| |![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png)|![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Şirket içi multi-Factor authentication çözümleri etkinleştirme| | |![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Kullanıcılarım için destek akıllı kart kimlik<sup>4</sup>| | |![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Office Portalı'nda ve Windows 10 Masaüstü parola süre sonu bildirimleri görüntüleme| | |![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png)|

> <sup>1</sup> Parola Eşitleme ile çoklu oturum açma.
>
> <sup>2</sup> geçişli kimlik doğrulaması ve çoklu oturum açma. 
>
> <sup>3</sup> çoklu oturum açma AD FS ile Federasyon.
>
> <sup>4</sup> AD FS Kurumsal izin vermek için PKI ile tümleştirilebilir sertifikaları kullanarak oturum açın. Bu sertifikalar, MDM veya GPO ya da akıllı kart sertifikaları (PIV/CAC kartları dahil) veya (güven sertifikası) iş için Hello gibi güvenilen sağlama kanallar aracılığıyla dağıtılan yazılım sertifikaları olabilir. Akıllı kart kimlik doğrulaması desteği hakkında daha fazla bilgi için bkz. [bu blog](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/).
>

## <a name="next-steps"></a>Sonraki adımlar

[Bir Azure kavram kanıtı ortamda daha fazla bilgi edinin](https://aka.ms/aad-poc)
[Azure AD Connect'i yükleme](http://go.microsoft.com/fwlink/?LinkId=615771)
[izleme karma kimlik eşitleme](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health)
