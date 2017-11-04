---
title: "Azure AD Connect: Sık sorulan sorular doğrudan kimlik doğrulama - | Microsoft Docs"
description: "Azure Active Directory doğrudan kimlik doğrulaması hakkında sık sorulan soruların yanıtları."
services: active-directory
keywords: "Azure AD Connect doğrudan kimlik doğrulama, Active Directory yükleyin gerekli bileşenleri Azure AD, SSO, çoklu oturum açma"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: billmath
ms.openlocfilehash: e1bd58797124210f7c31e90fb20d728289a04ba2
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2017
---
# <a name="azure-active-directory-pass-through-authentication-frequently-asked-questions"></a>Azure Active Directory doğrudan kimlik doğrulaması: Sık sorulan sorular

Bu makalede, Azure Active Directory (Azure AD) doğrudan kimlik doğrulaması hakkında sık sorulan sorular adres. Yeni içerik için geri denetleniyor tutun.

## <a name="which-of-the-azure-ad-sign-in-methods---pass-through-authentication-password-hash-synchronization-and-active-directory-federation-services-ad-fs---should-i-choose"></a>Azure AD oturum açma yöntemleri - geçişli kimlik doğrulaması, parola karma eşitlemesi ve Active Directory Federasyon Hizmetleri (AD FS) - hangisinin seçmeliyim?

Şirket içi ortamınız ve kuruluş gereksinimlerine bağlıdır. Bu makale için gözden bir [oturum açma çeşitli Azure AD yöntemlerinin karşılaştırılması](active-directory-aadconnect-user-signin.md).

## <a name="is-pass-through-authentication-a-free-feature"></a>Doğrudan kimlik doğrulama boş bir özellik mi var?

Doğrudan kimlik doğrulama boş bir özelliktir ve kullanmak için Azure AD Ücretli tüm sürümleri olması gerekmez.

## <a name="is-pass-through-authentication-available-in-microsoft-cloud-germanyhttpwwwmicrosoftdecloud-deutschland-and-microsoft-azure-government-cloudhttpsazuremicrosoftcomfeaturesgov"></a>Doğrudan kimlik doğrulama kullanılabilir [Microsoft Cloud Almanya](http://www.microsoft.de/cloud-deutschland) ve [Microsoft Azure kamu bulut](https://azure.microsoft.com/features/gov/)?

Hayır, geçişli kimlik doğrulaması yalnızca Azure AD dünya çapındaki örneğini kullanılabilir.

## <a name="does-conditional-accessactive-directory-conditional-accessmd-work-with-pass-through-authentication"></a>Mu [koşullu erişim](../active-directory-conditional-access.md) doğrudan kimlik doğrulama ile çalışır?

Evet, Azure multi-Factor Authentication dahil olmak üzere tüm koşullu erişim özelliklerini doğrudan kimlik doğrulaması ile çalışır.

## <a name="does-pass-through-authentication-support-alternate-id-as-the-username-instead-of-userprincipalname"></a>Doğrudan kimlik doğrulaması "Alternatif kimlik" yerine "userPrincipalName" kullanıcı adı olarak destekliyor mu?

Evet. Doğrudan kimlik doğrulamasını destekleyen `Alternate ID` içinde Azure AD Connect gösterildiği gibi yapılandırıldığında kullanıcı adı olarak [burada](active-directory-aadconnect-get-started-custom.md). Tüm Office 365 uygulamaları desteklemez `Alternate ID`. Destek deyimi için belirli uygulama belgelerine bakın.

## <a name="does-password-hash-synchronization-act-as-a-fallback-to-pass-through-authentication"></a>Parola karma eşitlemesi doğrudan kimlik doğrulama için bir geri dönüş olarak davranmak mu?

Hayır, geçişli kimlik doğrulaması _yok_ otomatik olarak parola karması eşitlemesi için yük devretme. Yalnızca bir geri dönüş için görür [doğrudan kimlik doğrulama bugün desteklemiyor senaryoları](active-directory-aadconnect-pass-through-authentication-current-limitations.md#unsupported-scenarios). Kullanıcı oturum açma hatalarını önlemek için doğrudan kimlik doğrulaması için yapılandırmalısınız [yüksek kullanılabilirlik](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability).

## <a name="can-i-install-an-azure-ad-application-proxyactive-directory-application-proxy-get-startedmd-connector-on-the-same-server-as-a-pass-through-authentication-agent"></a>Yükleyebilmek için bir [Azure AD uygulama proxy'si](../active-directory-application-proxy-get-started.md) doğrudan kimlik doğrulama Aracısı ile aynı sunucuda bağlayıcı?

Evet, bu yapılandırma doğrudan kimlik doğrulama Aracısı rebranded sürümlerle desteklenir (sürümleri 1.5.193.0 veya üstü).

## <a name="what-versions-of-azure-ad-connect-and-pass-through-authentication-agent-do-you-need"></a>Azure AD Connect ve doğrudan kimlik doğrulama Aracısı hangi sürümlerine gerekiyor?

Sürüm 1.1.486.0 gerekir ve daha sonra Azure AD Connect ve 1.5.58.0 veya daha sonra bu özelliğin çalışması doğrudan kimlik doğrulama aracısı için. Tüm yazılım sunucularda Windows Server 2012 R2 veya üstü yüklü olmalıdır.

## <a name="what-happens-if-my-users-password-has-expired-and-they-try-to-sign-in-using-pass-through-authentication"></a>My kullanıcının parolasının süresi dolmuş ne olur ve bunlar doğrudan kimlik doğrulaması kullanarak oturum çalıştığınızda?

Yapılandırdıysanız [parola geri yazma](../active-directory-passwords-update-your-own-password.md) belirli bir kullanıcı için ve geçişli kimlik doğrulaması kullanarak oturum açtığında, bunlar değiştirme veya parolalarını sıfırlama. Parolalar, şirket içi Active beklendiği gibi dizinine geri yazılır.

Ancak, belirli bir kullanıcı için parola geri yazma yapılandırılmamışsa veya kullanıcının geçerli bir Azure AD yoksa lisansı atanmış, kullanıcı parolalarını bulutta güncelleştirilemiyor. Parolasının süresi olsa bile kullanıcılar parolalarını güncelleştirilemiyor. Kullanıcı, bunun yerine bu iletiyi görür: "kuruluşunuz parolanızı bu sitede güncelleştirmenize izin vermiyor. Lütfen kuruluşunuzun önerdiği yönteme göre güncelleştirmek veya yardıma gereksiniminiz varsa yöneticinize sorun." Şirket içi Active Directory'de parolasını sıfırlamak için kullanıcı veya yönetici gerekir.

## <a name="how-does-pass-through-authentication-protect-you-against-brute-force-password-attacks"></a>Nasıl doğrudan kimlik doğrulama yanılma parola saldırılarına karşı koruma sağlar mı?

Okuma [bu makalede](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) daha fazla bilgi için.

## <a name="what-do-pass-through-authentication-agents-communicate-over-ports-80-and-443"></a>Ne doğrudan kimlik doğrulama Aracısı 80 ve 443 numaralı bağlantı noktaları üzerinden iletişim?

- Kimlik Doğrulama Aracısı, tüm özellik işlemleri için bağlantı noktası 443 üzerinden HTTPS istekleri olun.
- Kimlik Doğrulama Aracısı SSL sertifika iptal listelerini indirmek için bağlantı noktası 80 üzerinden HTTP isteklerini olun.

     >[!NOTE]
     >En son güncelleştirmeleri özelliği tarafından gerekli bağlantı noktalarının sayısı azalır. Azure AD Connect veya kimlik doğrulama Aracısı eski sürümlerini varsa, bu bağlantı noktaları da açık tutun: 5671, 8080, 9090, 9091, 9350, 9352 ve 10100 10120.

## <a name="can-the-pass-through-authentication-agents-communicate-over-an-outbound-web-proxy-server"></a>Doğrudan kimlik doğrulama aracıları giden web proxy sunucu iletişim kurabilir?

Evet. WPAD (Web Proxy Otomatik Bulma) şirket içi ortamınızda etkinleştirilirse, kimlik doğrulama aracıları otomatik olarak bulun ve ağ üzerinde bir web proxy sunucusu kullanma girişimi.

## <a name="can-i-install-two-or-more-pass-through-authentication-agents-on-the-same-server"></a>Aynı sunucuda iki veya daha fazla doğrudan kimlik doğrulama aracısı yükleyebilir miyim?

Hayır, bir doğrudan kimlik doğrulama Aracısı yalnızca tek bir sunucuya yükleyebilirsiniz. Yüksek kullanılabilirlik için doğrudan kimlik doğrulama yapılandırmak istiyorsanız, bu yönergeleri [makale](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability) yerine.

## <a name="i-already-use-active-directory-federation-services-ad-fs-for-azure-ad-sign-in-how-do-i-switch-it-to-pass-through-authentication"></a>I zaten Azure AD oturum açma için Active Directory Federasyon Hizmetleri (AD FS) kullanın. Nasıl, geçişli kimlik doğrulaması için geçiş yapabilirim?

AD FS Azure AD Connect Sihirbazı'nı kullanarak, oturum açma yöntemi yapılandırdıysanız, geçişli kimlik doğrulaması için oturum açma kullanıcı yöntemini değiştirin. Bu değişiklik, Kiracı'geçişli kimlik doğrulamasını etkinleştirir ve dönüştürür _tüm_ , federe etki alanlarına yönetilen etki alanları. Tüm sonraki oturum açma istekleri kiracınızda doğrudan kimlik doğrulaması tarafından işlenir. Şu anda, farklı etki alanları arasında AD FS ve geçişli kimlik doğrulaması bir birleşimini kullanmak için Azure AD Connect içinde desteklenen yolu yoktur.

AD FS oturum açma yöntemi olarak yapılandırılıp yapılandırılmadığını _dışında_ Azure AD Connect Sihirbazı'nın oturum açma kullanıcı yöntemi için doğrudan kimlik doğrulama ("yapılandırmayın" seçeneğinden) değiştirin. Bu değişiklik, Kiracı'geçişli kimlik doğrulaması sağlar. Ancak, tüm federe etki oturum açma için AD FS kullanmaya devam etmek. El ile yönetilen etki alanlarına bazılarını veya tümünü bu federe etki alanlarını dönüştürmek için PowerShell kullanın. Bundan sonra yönetilen etki alanları tüm oturum açma istekleri (_yalnızca_) doğrudan kimlik doğrulaması tarafından işlenir.

>[!IMPORTANT]
>Değil, doğrudan kimlik doğrulamasını işleyecek oturum açma yalnızca bulut Azure AD kullanıcıları.

## <a name="can-i-use-pass-through-authentication-in-a-multi-forest-ad-environment"></a>Doğrudan kimlik doğrulaması bir Çoklu orman AD ortamında kullanabilir miyim?

Evet. Birden çok orman ortamlarına AD ormanlar arasında orman güvenleri varsa desteklenir ve ad soneki yönlendirmesi doğru yapılandırılmışsa.

## <a name="how-many-pass-through-authentication-agents-do-i-need-to-install"></a>Yüklemek doğrudan kimlik doğrulama aracı gerekiyor mu?

Birden çok doğrudan kimlik doğrulama aracı yükleme sağlar [yüksek kullanılabilirlik](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability). Ancak belirleyici Yük Dengeleme arasında kimlik doğrulaması aracıları sağlamaz.

En yüksek ve Kiracı'görmeyi beklediğiniz oturum açma isteklerinin ortalama yük göz önünde bulundurun. Bir kıyaslama tek bir kimlik doğrulama Aracısı 300000 için 400.000 kimlik doğrulamaları standart bir 4 çekirdekli CPU, 16 GB RAM sunucu saniye başına işleyebilir. Çoğu müşteri için iki veya üç kimlik doğrulama Aracısı toplam, yüksek kullanılabilirlik ve kapasite için yeterlidir.

Etki alanı denetleyicileriniz yakın oturum açma gecikme süresini artırmak için kimlik doğrulama Aracısı yüklemenizi öneririz.

## <a name="can-i-install-the-first-pass-through-authentication-agent-on-a-server-other-than-the-one-that-runs-azure-ad-connect"></a>Azure AD Connect çalıştıran biri başka bir sunucuda ilk doğrudan kimlik doğrulama aracısı yükleyebilir miyim?

Hayır, bu senaryo olan _değil_ desteklenir.

## <a name="how-many-pass-through-authentication-agents-should-i-install"></a>Kaç tane doğrudan kimlik doğrulama Aracısı yüklediğimde?

Öneririz:

- Toplam iki veya üç kimlik doğrulama aracısını yükleyin. Bu yapılandırma, müşterilerin çoğu için yeterlidir.
- Etki alanı denetleyicilerinizde kimlik doğrulaması aracıları yüklemek (veya onları mümkün olduğunca yakın gibi) oturum açma gecikme süresini artırmak için.
- (Burada kimlik doğrulama aracısı yüklü) sunucuları aynı AD ormanına parolaları doğrulanması gereken kullanıcılar olarak eklendiğinden emin olun.

>[!NOTE]
>Kiracı başına 12 kimlik doğrulaması aracıların sistem sınırı yoktur.

## <a name="how-can-i-disable-pass-through-authentication"></a>Doğrudan kimlik doğrulama nasıl devre dışı bırakabilirim?

Azure AD Connect Sihirbazı'nı yeniden çalıştırın ve oturum açma kullanıcı yöntemi olarak başka bir yönteme doğrudan kimlik doğrulamasını değiştirin. Bu değişiklik, Kiracı'geçişli kimlik doğrulaması devre dışı bırakır ve kimlik doğrulama sunucusundan kaldırır. Kimlik doğrulama aracıları diğer sunuculardan el ile kaldırmanız gerekir.

## <a name="what-happens-when-i-uninstall-a-pass-through-authentication-agent"></a>Doğrudan kimlik doğrulama Aracısı kaldırdığınızda ne olur?

Doğrudan kimlik doğrulama Aracısı bir sunucudan kaldırmayı, oturum açma istekleri kabul etmeyi durdurmasına neden olur. Başka bir kullanıcı oturum açma, Kiracı'çiğnemekten önlemek için kimlik doğrulama Aracısı bu işlemi yapmadan önce çalışan sahip emin olun.

## <a name="next-steps"></a>Sonraki adımlar
- [**Geçerli sınırlamalar** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) -hangi senaryoları desteklenir ve hangilerinin olmayan öğrenin.
- [**Hızlı Başlangıç** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) - hale getirmek ve Azure AD doğrudan kimlik doğrulama çalıştıran.
- [**Akıllı kilitleme** ](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) -Kiracı üzerinde yapılandırma akıllı kilitleme özelliğini kullanıcı hesaplarını korumak için.
- [**Teknik derinlemesine** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) -bu özellik nasıl çalıştığını anlayın.
- [**Sorun giderme** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) -özelliği ile ilgili genel sorunları çözmeyi öğrenin.
- [**Güvenlik derinlemesine** ](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) -özelliği hakkında ek ayrıntılı teknik bilgi.
- [**Azure AD sorunsuz SSO** ](active-directory-aadconnect-sso.md) -tamamlayıcı bu özellik hakkında daha fazla bilgi edinin.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - yeni özellik istekleri dosyalama için.
