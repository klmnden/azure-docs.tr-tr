---
title: Azure karma kimlik çözümü seçme | Microsoft Docs
description: Kullanılabilir karma kimlik çözümleri ve öneriler, kuruluşunuz için en iyi Kimlik Yönetimi karar vermeniz temel bir anlayış alın.
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
ms.openlocfilehash: 9f9099c0ebd65ba84e171314e6f04d858648a805
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
ms.locfileid: "29800746"
---
# <a name="microsoft-hybrid-identity-solutions"></a>Microsoft karma kimlik çözümleri
[Microsoft Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) karma kimlik çözümleri hala kullanıcıların şirket içi yönetme sırasında Azure AD ile şirket içi dizin nesneleri eşitleme olanak tanır. Şirket içi Windows Server Active Directory'nizi Azure AD ile eşitlemek planlama kullanmak isteyip istemediğinizi yapmak için ilk karar kimlik eşitlenen veya kimlik Federasyon. Eşitlenen kimlikler ve isteğe bağlı olarak parola karmaları şirket içi ve bulut tabanlı kurumsal kaynaklara erişmek için aynı parolayı kullanmalarına olanak sağlar. Çoklu oturum açma (SSO) veya şirket içi MFA gibi daha gelişmiş senaryo gereksinimleri için Active Directory Federasyon Hizmetleri (kimlikleri birleştirmek için AD FS) dağıtmanız gerekir. 

Karma kimlik yapılandırmak için kullanılabilen birkaç seçeneğiniz vardır. Bu makalede dağıtım kolaylığı üzerinde göre kuruluşunuz için en iyisi seçmenize yardımcı olacak bilgiler sağlar ve belirli kimlik ve erişim yönetimi gerekiyor. En iyi hangi kimlik modelinde, kuruluşunuzun gereksinimlerine uygun göz önünde bulundurun gibi ayrıca zaman, mevcut altyapınızı, karmaşıklığı ve maliyeti hakkında düşünmek gerekir. Bu faktörlerin her kuruluş için farklıdır ve zaman içinde değişebilir. Ancak, gereksinimlerinizi değiştirirseniz, bu da farklı kimlik modeline geçiş esnekliğine sahip.

> [!TIP]
> Bu çözümler tüm tarafından sunulur [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

## <a name="synchronized-identity"></a>Eşitlenen kimliği 
Eşitlenen en basit yolu, şirket içi dizin nesneleri (kullanıcılar ve gruplar) eşitleme için Azure AD ile kimliğidir. 

![Eşitlenen karma kimlik](./media/choose-hybrid-identity-solution/synchronized-identity.png)

Eşitlenen kimlik kolay ve hızlı yöntem olsa da, kullanıcılarınızın hala bulut tabanlı kaynaklar için ayrı bir parola korumak için gerekir. Bunu önlemek için şunları da yapabilirsiniz (isteğe bağlı) [kullanıcı parola karmasını eşitleme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-synchronization#what-is-password-synchronization) Azure AD dizininiz için. Parola karmaları eşitleme bulut tabanlı kuruluş kaynaklarına aynı kullanıcı adı ve şirket içi kullandıkları parolayla oturum açmalarını sağlar. Azure AD Connect, şirket içi dizininize değişiklikler ve Azure AD dizininizi eşitlenmiş tutar düzenli olarak denetler. Bir kullanıcı özniteliği ya da parola değiştirilen şirket içi Active Directory olduğunda, Azure AD içinde otomatik olarak güncelleştirilir. 

Yalnızca Office 365, SaaS uygulamaları ve diğer Azure AD tabanlı kaynaklara oturum açmak, kullanıcılar etkinleştirmek için gereken çoğu kuruluş için varsayılan parola eşitleme seçeneği önerilir. Sizin için işe yaramazsa, geçişli kimlik doğrulaması ve AD FS arasında karar vermeniz gerekir.

> [!TIP]
> Kullanıcı parolalarını şirket içi Windows Server Active Directory gerçek kullanıcı parolası temsil eden bir karma değer biçiminde depolanır. Bir karma değer, tek yönlü matematiksel işlevi (karma algoritma) sonucudur. Bir parola düz metin sürümü için tek yönlü işlevin sonucu dönmek için bir yöntem yoktur. Şirket içi ağınızda oturum açmak için parola karması kullanamazsınız. Parolalarını eşitlemek için opt, Azure AD Connect şirket içi Active Directory'den parola karmaları ayıklar ve Azure AD ile eşitlenmeden önce parola karması işleme ek güvenlik uygular. Parola Eşitleme, ayrıca Self Servis parola sıfırlama Azure AD'de etkinleştirmek için parola geri yazma ile birlikte kullanılabilir. Ayrıca, etki alanına katılmış bilgisayarlarda, şirket ağına bağlanan kullanıcılar için çoklu oturum açma (SSO) etkinleştirebilirsiniz. Çoklu oturum açma ile etkin kullanıcılar yalnızca güvenli bir şekilde bulut kaynaklarına erişmek için bir kullanıcı adı girmeniz gerekir. 

## <a name="pass-through-authentication"></a>Geçişli kimlik doğrulaması
[Azure AD doğrudan kimlik doğrulama](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication) kullanarak şirket içi Active Directory'nizi Azure AD tabanlı hizmetler için bir basit parola doğrulama çözümü sağlar. Kuruluşunuz için güvenlik ve uyumluluk ilkeleri kullanıcıların parolalarını bile karma bir formda gönderme izin vermez ve etki alanına katılmış cihazlar için Masaüstü SSO desteklemek yeterlidir, geçişli kimlik doğrulaması kullanmayı değerlendir önerilir. Doğrudan kimlik doğrulaması, herhangi bir AD FS ile karşılaştırıldığında dağıtım altyapısı basitleştirir DMZ dağıtımında gerektirmez. Azure AD kullanarak kullanıcılar oturum açtığında, bu kimlik doğrulama yöntemi kullanıcıların parolalarını şirket içi Active Directory'nizi karşı doğrudan doğrular.

![Geçişli kimlik doğrulaması](./media/choose-hybrid-identity-solution/pass-through-authentication.png)

Doğrudan kimlik doğrulama, karmaşık ağ altyapısı için gerek yoktur ve bulutta şirket içi parolaları depolamak gerekmez. Çoklu oturum açma ile birlikte geçişli kimlik doğrulaması için Azure AD oturum açarken gerçekten tümleşik bir deneyim sağlar ya da diğer bulut Hizmetleri.

Doğrudan kimlik doğrulaması için parola doğrulama isteklerini dinleyen bir basit şirket içi Aracısı kullanan Azure AD Connect ile yapılandırılır. Aracı kolayca Yük Dengeleme ve yüksek kullanılabilirlik sağlamak için birden fazla makine dağıtılabilir. Tüm iletişimler yalnızca giden olduğundan, bir çevre ağınızda yüklenecek bağlayıcı gereksinimi yoktur. Bağlayıcısı için sunucu bilgisayar gereksinimleri aşağıdaki gibidir:

- Windows Server 2012 R2 veya üzeri
- Üzerinden kullanıcıların doğrulandığı ormandaki bir etki alanına katılmış

## <a name="federated-identity-ad-fs"></a>Federe kimlik (AD FS)
Kullanıcıların Office 365 ve diğer bulut hizmetlerine erişme üzerinde daha fazla denetim için çoklu oturum açma (SSO) kullanarak dizin eşitlemeyi ayarlayabilirsiniz [Active Directory Federasyon Hizmetleri (AD FS)](https://docs.microsoft.com/windows-server/identity/ad-fs/overview/whats-new-active-directory-federation-services-windows-server-2016). Kullanıcı oturum açma işlemleri AD FS ile federasyonunu kullanıcı kimlik bilgilerini doğrulayan bir şirket içi sunucu kimlik doğrulaması atar. Bu modelde, şirket içi Active Directory kimlik bilgileri hiçbir zaman Azure AD'ye aktarılır.

![Federe kimlik](./media/choose-hybrid-identity-solution/federated-identity.png)

Kimlik Federasyonu olarak da bilinen bu oturum açma yöntemi, tüm kullanıcı kimlik doğrulaması denetimli şirket içi ve daha ayrıntılı düzeylerini erişim denetimi uygulamak yöneticilerin verir sağlar. AD FS ile Kimlik Federasyonu en karmaşık bir seçenektir ve şirket içi ortamınızdaki ek sunuculara dağıtılması gerekir. Kimlik Federasyonu Ayrıca, Active Directory ve AD FS altyapınızı için 7 x 24 destek sağlamayı kaydeder. Şirket içi Internet erişimi engelliyorsa, etki alanı denetleyicisi veya AD FS sunucuları kullanıcılar oturum açamaz bulut Hizmetleri, kullanılamadığından bu yüksek düzeyde desteği gereklidir.

> [!TIP]
> Active Directory Federasyon Hizmetleri (AD FS) Federasyon kullanmaya karar verirseniz, AD FS altyapınızın başarısız olursa, isteğe bağlı olarak parola eşitleme yedek olarak ayarlayabilirsiniz.


## <a name="common-scenarios-and-recommendations"></a>Yaygın senaryolar ve öneriler
Burada, bazı ortak karma kimlik ve erişim yönetim senaryoları için hangi karma kimlik seçeneği (veya Seçenekler) her biri için uygun olabilir öneriler bulunmaktadır.

|İçin gerekir:|PWS ve SSO<sup>1</sup>| PTA ve SSO<sup>2</sup> | AD FS<sup>3</sup>|
|-----|-----|-----|-----|
|Yeni kullanıcı, kişi ve grup hesapları buluta my şirket içi Active Directory'de otomatik olarak oluşturulan eşitleyin.|![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png)| ![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png) |![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png)|
|My Kiracı Office 365 karma senaryolar için ayarlama|![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png)| ![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png) |![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Oturum açma ve şirket içi parolalarını kullanarak bulut hizmetlerine erişim Kullanıcılarım etkinleştir|![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png)| ![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png) |![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Tekli Kurumsal kimlik bilgilerini kullanarak oturum uygulama|![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png)| ![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png) |![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Parola karmaları bulutta depolanan emin olun| |![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png)|![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Şirket içi çok faktörlü kimlik doğrulaması çözümlerle| | |![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Kullanıcılarım için destek akıllı kart kimlik doğrulaması<sup>4</sup>| | |![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Office portalında ve Windows 10 Masaüstü parola süre sonu bildirimleri görüntüleme| | |![Önerilen](./media/choose-hybrid-identity-solution/ic195031.png)|

> <sup>1</sup> Parola Eşitleme ile çoklu oturum açma. 

> <sup>2</sup> doğrudan kimlik doğrulaması ve çoklu oturum açma. 

> <sup>3</sup> çoklu oturum açma AD FS ile Federasyon.

> <sup>4</sup> AD FS, kuruluşunuzdaki izin vermek için PKI ile tümleştirilebilir sertifikaları kullanarak oturum açın. Bu sertifikalar, MDM veya GPO ya da akıllı kart sertifikaları (PIV/Önbellek kartları dahil) veya iş (güven sertifikası) için Hello gibi güvenilir sağlama kanallar aracılığıyla dağıtılan yazılımla sertifikalarının olabilir. Akıllı kart kimlik doğrulaması desteği hakkında daha fazla bilgi için bkz: [bu blog](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/).


## <a name="next-steps"></a>Sonraki adımlar
[Bir Azure kavram kanıtı ortamda daha fazla bilgi edinin](https://aka.ms/aad-poc)

[Azure AD Connect'i yükleme](http://go.microsoft.com/fwlink/?LinkId=615771)

[Karma kimlik eşitleme izleme](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health)

