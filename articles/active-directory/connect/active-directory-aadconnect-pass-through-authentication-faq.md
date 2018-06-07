---
title: 'Azure AD Connect: Sık sorulan sorular doğrudan kimlik doğrulama - | Microsoft Docs'
description: Azure Active Directory doğrudan kimlik doğrulaması hakkında sık sorulan soruların yanıtlarını
services: active-directory
keywords: Azure AD Connect doğrudan kimlik doğrulama, Active Directory yükleyin gerekli bileşenleri Azure AD, SSO, çoklu oturum açma
documentationcenter: ''
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/04/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 8363c49c4a52785fb5deacb3ac4998d38aca1430
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34593889"
---
# <a name="azure-active-directory-pass-through-authentication-frequently-asked-questions"></a>Azure Active Directory doğrudan kimlik doğrulaması: Sık sorulan sorular

Bu makalede, Azure Active Directory (Azure AD) doğrudan kimlik doğrulaması hakkında sık sorulan sorular giderir. Güncelleştirilmiş içerik için geri denetleniyor tutun.

## <a name="which-of-the-methods-to-sign-in-to-azure-ad-pass-through-authentication-password-hash-synchronization-and-active-directory-federation-services-ad-fs-should-i-choose"></a>Hangi yöntemin Azure AD, geçişli kimlik doğrulaması, oturum açmak için parola karma eşitlemesi ve Active Directory Federasyon Hizmetleri (AD FS) seçmem gerekir?

Şirket içi ortamınız ve kuruluş gereksinimlerine bağlıdır. Gözden geçirme [Azure AD Connect kullanıcı oturum açma seçenekleri](active-directory-aadconnect-user-signin.md) bir karşılaştırma için bir makale yöntemlerin çeşitli Azure AD oturum açma.

## <a name="is-pass-through-authentication-a-free-feature"></a>Doğrudan kimlik doğrulama boş bir özellik mi var?

Doğrudan kimlik doğrulaması boş bir özelliktir. Kullanmak için Azure AD Ücretli tüm sürümleri gerek yoktur.

## <a name="is-pass-through-authentication-available-in-the-microsoft-azure-germany-cloudhttpwwwmicrosoftdecloud-deutschland-and-the-microsoft-azure-government-cloudhttpsazuremicrosoftcomfeaturesgov"></a>Doğrudan kimlik doğrulama kullanılabilir [Microsoft Azure Almanya bulut](http://www.microsoft.de/cloud-deutschland) ve [Microsoft Azure kamu bulut](https://azure.microsoft.com/features/gov/)?

Hayır. Doğrudan kimlik doğrulaması, yalnızca Azure AD dünya çapındaki örneğini içinde kullanılabilir.

## <a name="does-conditional-accessactive-directory-conditional-access-azure-portalmd-work-with-pass-through-authentication"></a>Mu [koşullu erişim](../active-directory-conditional-access-azure-portal.md) doğrudan kimlik doğrulama ile çalışır?

Evet. Azure çok faktörlü kimlik doğrulaması dahil olmak üzere tüm koşullu erişim özelliklerini doğrudan kimlik doğrulaması ile çalışır.

## <a name="does-pass-through-authentication-support-alternate-id-as-the-username-instead-of-userprincipalname"></a>Doğrudan kimlik doğrulaması "Alternatif kimlik" yerine "userPrincipalName" kullanıcı adı olarak destekliyor mu?

Evet. Doğrudan kimlik doğrulamasını destekleyen `Alternate ID` içinde Azure AD Connect yapılandırıldığında kullanıcı adı olarak. Daha fazla bilgi için bkz: [Azure AD Connect özel yüklemesi](active-directory-aadconnect-get-started-custom.md). Tüm Office 365 uygulamaları desteklemez `Alternate ID`. Belirli uygulamanın belgelerine destek deyimine bakın.

## <a name="does-password-hash-synchronization-act-as-a-fallback-to-pass-through-authentication"></a>Parola karma eşitlemesi doğrudan kimlik doğrulama için bir geri dönüş olarak davranmak mu?

Hayır. Doğrudan kimlik doğrulama _yok_ otomatik olarak parola karması eşitlemesi için yük devretme. Yalnızca bir geri dönüş için görür [doğrudan kimlik doğrulama bugün desteklemiyor senaryoları](active-directory-aadconnect-pass-through-authentication-current-limitations.md#unsupported-scenarios). Kullanıcı oturum açma hatalarını önlemek için doğrudan kimlik doğrulaması için yapılandırmalısınız [yüksek kullanılabilirlik](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability).

## <a name="can-i-install-an-azure-ad-application-proxymanage-appsapplication-proxymd-connector-on-the-same-server-as-a-pass-through-authentication-agent"></a>Yükleyebilmek için bir [Azure AD uygulama proxy'si](../manage-apps/application-proxy.md) doğrudan kimlik doğrulama Aracısı ile aynı sunucuda bağlayıcı?

Evet. Doğrudan kimlik doğrulama Aracısı sürümü 1.5.193.0 rebranded sürümleri veya daha sonra bu yapılandırmayı desteklemez.

## <a name="what-versions-of-azure-ad-connect-and-pass-through-authentication-agent-do-you-need"></a>Azure AD Connect ve doğrudan kimlik doğrulama Aracısı hangi sürümlerine gerekiyor?

Bu özelliğin çalışması için sürüm 1.1.486.0 gerekir ve daha sonra Azure AD Connect ve 1.5.58.0 veya doğrudan kimlik doğrulama aracısı için daha sonra. Tüm yazılım sunucularda Windows Server 2012 R2 veya sonraki sürümünü yükleyin.

## <a name="what-happens-if-my-users-password-has-expired-and-they-try-to-sign-in-by-using-pass-through-authentication"></a>My kullanıcının parolasının süresi dolmuş ne olur ve bunlar doğrudan kimlik doğrulaması kullanarak oturum açmaya çalışan?

Yapılandırdıysanız [parola geri yazma](../active-directory-passwords-update-your-own-password.md) belirli bir kullanıcı için doğrudan kimlik doğrulaması kullanarak oturum açtığında, değiştirin ya parolalarını sıfırlayın. Parolalar, şirket içi Active beklendiği gibi dizinine geri yazılır.

Belirli bir kullanıcı için parola geri yazma özelliğini yapılandırmadıysanız ya da kullanıcının geçerli bir Azure AD yoksa lisansı atanmış, kullanıcı parolalarını bulutta güncelleştirilemiyor. Parolasının süresi olsa bile kullanıcılar parolalarını güncelleştirilemiyor. Kullanıcı, bunun yerine bu iletiyi görür: "kuruluşunuz parolanızı bu sitede güncelleştirmenize izin vermiyor. Kuruluşunuzun önerdiği yönteme göre güncelleştirmek veya yardıma gereksiniminiz varsa yöneticinize sorun." Kullanıcı veya yönetici parolalarını şirket içi Active Directory'de sıfırlamanız gerekir.

## <a name="how-does-pass-through-authentication-protect-you-against-brute-force-password-attacks"></a>Nasıl doğrudan kimlik doğrulama yanılma parola saldırılara karşı koruma sağlar mı?

Okuma [Azure Active Directory doğrudan kimlik doğrulaması: Akıllı kilitleme](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) daha fazla bilgi için.

## <a name="what-do-pass-through-authentication-agents-communicate-over-ports-80-and-443"></a>Ne doğrudan kimlik doğrulama Aracısı 80 ve 443 numaralı bağlantı noktaları üzerinden iletişim?

- Kimlik Doğrulama Aracısı, tüm özellik işlemleri için bağlantı noktası 443 üzerinden HTTPS istekleri olun.
- Kimlik Doğrulama Aracısı HTTP isteklerinin SSL sertifika iptal listeleri (CRL'ler) indirmek için 80 numaralı bağlantı noktası üzerinden olun.

     >[!NOTE]
     >En son güncelleştirmeleri özellik gerektirir bağlantı noktalarının sayısı azalır. Azure AD Connect veya kimlik doğrulama Aracısı eski sürümlerini varsa, bu bağlantı noktalarının açık de tutun: 5671, 8080, 9090, 9091, 9350, 9352 ve 10100 10120.

## <a name="can-the-pass-through-authentication-agents-communicate-over-an-outbound-web-proxy-server"></a>Doğrudan kimlik doğrulama aracıları giden web proxy sunucu iletişim kurabilir?

Evet. Web Proxy Otomatik Bulma (WPAD) şirket içi ortamınızda etkinleştirilirse, kimlik doğrulama aracıları otomatik olarak bulun ve ağ üzerinde bir web proxy sunucusu kullanma girişimi.

## <a name="can-i-install-two-or-more-pass-through-authentication-agents-on-the-same-server"></a>Aynı sunucuda iki veya daha fazla doğrudan kimlik doğrulama aracısı yükleyebilir miyim?

Hayır, bir doğrudan kimlik doğrulama Aracısı yalnızca tek bir sunucuya yükleyebilirsiniz. Yüksek kullanılabilirlik için doğrudan kimlik doğrulama yapılandırmak istiyorsanız,'ndaki yönergeleri izleyin [Azure Active Directory doğrudan kimlik doğrulaması: Hızlı Başlangıç](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability).

## <a name="how-do-i-remove-a-pass-through-authentication-agent"></a>Doğrudan kimlik doğrulama Aracısı nasıl kaldırılsın mı?

Doğrudan kimlik doğrulama Aracısı çalıştığı sürece etkin kalır ve sürekli olarak kullanıcı oturum açma isteklerini işler. Bir kimlik doğrulama aracısı kaldırmak istiyorsanız, Git **Denetim Masası -> Programlar -> Programlar ve Özellikler** ve her ikisi de kaldırma **Microsoft Azure AD Connect kimlik doğrulama Aracısı** ve  **Microsoft Azure AD Connect aracı güncelleştirici** programlar.

Doğrudan kimlik doğrulama dikey işaretlerseniz [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) önceki adımı tamamladıktan sonra kimlik doğrulama olarak gösteren Aracısı görürsünüz **devre dışı**. Bu _beklenen_. Kimlik Doğrulama Aracısı birkaç gün sonra otomatik olarak listeden bırakılır.

## <a name="i-already-use-ad-fs-to-sign-in-to-azure-ad-how-do-i-switch-it-to-pass-through-authentication"></a>Zaten AD FS için Azure AD oturum açmak için kullandığım. Nasıl, geçişli kimlik doğrulaması için geçiş yapabilirim?

AD FS ile Azure AD Connect Sihirbazı oturum açmak için yöntemi olarak yapılandırdıysanız, geçişli kimlik doğrulaması için oturum açmak için kullanıcının kullandığı yöntemini değiştirin. Bu değişiklik, Kiracı'geçişli kimlik doğrulamasını etkinleştirir ve dönüştürür _tüm_ , Federasyon etki alanlarına yönetilen etki alanları. Doğrudan kimlik doğrulama kiracınıza oturum açmak için tüm sonraki istekleri işler. Şu anda, farklı etki alanları arasında AD FS ve geçişli kimlik doğrulaması bir birleşimini kullanmak için Azure AD Connect içinde desteklenen yolu yoktur.

AD FS oturum açmak için yöntemi olarak yapılandırılan varsa _dışında_ Azure AD Connect Sihirbazı, geçişli kimlik doğrulaması için kullanıcı oturum açma değişiklik yöntemi. Bu değişiklik yapabilirsiniz **yapılandırmayın** seçeneği. Bu değişiklik, Kiracı'doğrudan kimlik doğrulama sağlar, ancak tüm Federasyon etki alanı oturum açma için AD FS kullanmaya devam eder. El ile yönetilen etki alanlarını bazılarını veya tümünü bu Federasyon etki alanı dönüştürmek için PowerShell kullanın. Bu değişikliği yaptıktan sonra *yalnızca* doğrudan kimlik doğrulama, yönetilen etki alanlarına oturum açmak için tüm istekleri işler.

>[!IMPORTANT]
>Doğrudan kimlik doğrulama için yalnızca bulutta Azure oturum açma işlemek olmayan AD kullanıcıları.

## <a name="can-i-use-pass-through-authentication-in-a-multi-forest-active-directory-environment"></a>Doğrudan kimlik doğrulaması bir Çoklu orman Active Directory ortamında kullanabilir miyim?

Evet. Birden çok orman ortamlarına Active Directory ormanlar arasında orman güvenleri varsa desteklenir ve ad soneki yönlendirmesi doğru yapılandırılmışsa.

## <a name="how-many-pass-through-authentication-agents-do-i-need-to-install"></a>Yüklemek doğrudan kimlik doğrulama aracı gerekiyor mu?

Birden çok doğrudan kimlik doğrulama aracı yükleme sağlar [yüksek kullanılabilirlik](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability). Ancak, belirleyici Yük Dengeleme arasında kimlik doğrulaması aracıları sağlamaz.

En yüksek ve Kiracı'görmeyi beklediğiniz oturum açma isteklerinin ortalama yük göz önünde bulundurun. Bir kıyaslama tek bir kimlik doğrulama Aracısı 300-400 kimlik doğrulamaları standart bir 4 çekirdekli CPU, 16 GB RAM sunucu saniye başına işleyebilir.

Ağ trafiği tahmin etmek için aşağıdaki boyutlandırma kılavuzluğu kullanın:
- Her istek bir yükü boyutu vardır (0.5K + 1 K * num_of_agents) bayt; yani, verileri Azure AD kimlik doğrulama aracısı için. Burada, "num_of_agents" Kiracı'kimlik doğrulama aracı sayısı kayıtlı belirtir.
- Her yanıtı yükü boyutu 1 K bayt; yine de sahip istiyor musunuz? yani, verileri kimlik doğrulama Aracısı Azure ad.

Çoğu müşteri için iki veya üç kimlik doğrulama Aracısı toplam, yüksek kullanılabilirlik ve kapasite için yeterlidir. Oturum açma gecikme süresini artırmak için etki alanı denetleyicileriniz yakın kimlik doğrulama aracısı yüklemeniz gerekir.

>[!NOTE]
>Kiracı başına 12 kimlik doğrulaması aracıların sistem sınırı yoktur.

## <a name="can-i-install-the-first-pass-through-authentication-agent-on-a-server-other-than-the-one-that-runs-azure-ad-connect"></a>Azure AD Connect çalıştıran biri başka bir sunucuda ilk doğrudan kimlik doğrulama aracısı yükleyebilir miyim?

Hayır, bu senaryo olan _değil_ desteklenir.

## <a name="how-can-i-disable-pass-through-authentication"></a>Doğrudan kimlik doğrulama nasıl devre dışı bırakabilirim?

Azure AD Connect Sihirbazı'nı yeniden çalıştırın ve oturum açma kullanıcı yöntemi olarak başka bir yönteme doğrudan kimlik doğrulamasını değiştirin. Bu değişiklik, Kiracı'geçişli kimlik doğrulaması devre dışı bırakır ve kimlik doğrulama sunucusundan kaldırır. Diğer sunucularda kimlik doğrulaması aracıları el ile kaldırmanız gerekir.

## <a name="what-happens-when-i-uninstall-a-pass-through-authentication-agent"></a>Doğrudan kimlik doğrulama Aracısı kaldırdığınızda ne olur?

Bir sunucudan doğrudan kimlik doğrulama aracısını kaldırırsanız, sunucunun oturum açma istekleri kabul etmeyi durdurmasına neden olur. Kiracı'oturum açma kullanıcı özelliği çiğnemekten önlemek için doğrudan kimlik doğrulama Aracısı kaldırmadan önce başka bir kimlik doğrulama Aracısı çalıştıran sahip emin olun.

## <a name="next-steps"></a>Sonraki adımlar
- [Geçerli sınırlamalar](active-directory-aadconnect-pass-through-authentication-current-limitations.md): hangi senaryoları desteklenir ve hangilerinin olmayan öğrenin.
- [Hızlı Başlangıç](active-directory-aadconnect-pass-through-authentication-quick-start.md): Azure AD doğrudan kimlik doğrulamasını başlamak ve çalıştırmak.
- [Akıllı kilitleme](active-directory-aadconnect-pass-through-authentication-smart-lockout.md): kullanıcı hesapları korumak için Kiracı akıllı kilitleme özelliği yapılandırmayı öğrenin.
- [Teknik derinlemesine](active-directory-aadconnect-pass-through-authentication-how-it-works.md): doğrudan kimlik doğrulaması özelliğinin nasıl çalıştığını anlayın.
- [Sorun giderme](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): doğrudan kimlik doğrulama özelliği ile ortak sorunları çözmeyi öğrenin.
- [Güvenlik derinlemesine](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md): doğrudan kimlik doğrulama özelliği hakkında ayrıntılı teknik bilgi alın.
- [Azure AD sorunsuz SSO](active-directory-aadconnect-sso.md): tamamlayıcı bu özellik hakkında daha fazla bilgi edinin.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): yeni özellik istekleri dosya için Azure Active Directory Forumu kullanın.

