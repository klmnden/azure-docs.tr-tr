---
title: 'Azure AD Connect: Geçişli kimlik doğrulaması - sık sorulan sorular | Microsoft Docs'
description: Azure Active Directory geçişli kimlik doğrulaması hakkında sık sorulan soruların yanıtlarını
services: active-directory
keywords: Azure AD, SSO, Azure AD Connect geçişli kimlik doğrulaması, Active Directory Yükleme gerekli bileşenleri çoklu oturum açma
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/15/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: cd41bdfb90e5e8428a747832c10fe23a2cac4560
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67109394"
---
# <a name="azure-active-directory-pass-through-authentication-frequently-asked-questions"></a>Azure Active Directory geçişli kimlik doğrulaması: Sık sorulan sorular

Bu makalede Azure Active Directory (Azure AD) geçişli kimlik doğrulaması hakkında sık sorulan soruları ele alır. Güncelleştirilmiş içerik için geri kontrol etmeyi unutmayın.

## <a name="which-of-the-methods-to-sign-in-to-azure-ad-pass-through-authentication-password-hash-synchronization-and-active-directory-federation-services-ad-fs-should-i-choose"></a>Hangi yöntemin Azure AD'ye, geçişli kimlik doğrulaması, oturum açmak için parola karma eşitlemesi ve Active Directory Federasyon Hizmetleri (AD FS) seçmem gerekir?

Gözden geçirme [bu kılavuzda](https://docs.microsoft.com/azure/security/azure-ad-choose-authn) bir karşılaştırma çeşitli Azure AD oturum açma yöntemleri ve kuruluşunuz için doğru oturum açma yöntemini seçin.

## <a name="is-pass-through-authentication-a-free-feature"></a>Geçişli kimlik doğrulaması, ücretsiz bir özellik mi var?

Geçişli kimlik doğrulaması ücretsiz bir özelliktir. Bunu kullanmak için Azure AD Ücretli tüm sürümleri gerekmez.

## <a name="is-pass-through-authentication-available-in-the-microsoft-azure-germany-cloudhttpswwwmicrosoftdecloud-deutschland-and-the-microsoft-azure-government-cloudhttpsazuremicrosoftcomfeaturesgov"></a>Geçişli kimlik doğrulaması kullanılabilir [Microsoft Azure Almanya bulut](https://www.microsoft.de/cloud-deutschland) ve [Microsoft Azure kamu bulutundaki](https://azure.microsoft.com/features/gov/)?

Hayır. Geçişli kimlik doğrulaması, yalnızca dünya çapındaki örneğini Azure AD içinde kullanılabilir.

## <a name="does-conditional-accessactive-directory-conditional-access-azure-portalmd-work-with-pass-through-authentication"></a>Mu [koşullu erişim](../active-directory-conditional-access-azure-portal.md) geçişli kimlik doğrulaması ile çalışabilir?

Evet. Azure multi-Factor Authentication dahil, tüm koşullu erişim özelliklerini, geçişli kimlik doğrulaması ile çalışır.

## <a name="does-pass-through-authentication-support-alternate-id-as-the-username-instead-of-userprincipalname"></a>Geçişli kimlik doğrulaması yapılandırmalıdır "userPrincipalName" yerine "Alternatif kimlik" destekliyor mu?

Evet, geçişli kimlik doğrulamasını destekleyen `Alternate ID` Azure AD Connect'e bağlanan yapılandırıldığında yapılandırmalıdır. Şirket içi Active Directory eşitleme Azure AD Connect bir önkoşul gerekli `UserPrincipalName` Azure AD'ye özniteliği. Daha fazla bilgi için [Azure AD Connect özel yüklemesi](how-to-connect-install-custom.md). Tüm Office 365 uygulamaları desteklemez `Alternate ID`. Belirli bir uygulamanın belgelerine destek deyimine bakın.

## <a name="does-password-hash-synchronization-act-as-a-fallback-to-pass-through-authentication"></a>Parola Karması eşitleme, geçişli kimlik doğrulaması için bir geri dönüş olarak davranmak mu?

Hayır. Geçişli kimlik doğrulaması _yok_ otomatik olarak yük devretme için parola karması eşitleme. Kullanıcı oturum açma hatalarını önlemek için geçişli kimlik doğrulaması için yapılandırmalısınız [yüksek kullanılabilirlik](how-to-connect-pta-quick-start.md#step-4-ensure-high-availability).

## <a name="what-happens-when-i-switch-from-password-hash-synchronization-to-pass-through-authentication"></a>Parola Karması eşitlemeyi geçişli kimlik doğrulaması için geçtiğinizde ne olur?

Geçişli kimlik doğrulaması oturum açma yönteminden parola karması eşitleme geçişli kimlik doğrulaması için geçiş yapmak için Azure AD Connect kullandığınızda, birincil oturum açma yöntemi, kullanıcılarınızın yönetilen etki alanlarında haline gelir. Parola Karması eşitleme tarafından daha önce eşitlendi, tüm kullanıcıların parola karmalarının Azure AD'de depolanan kalmasını unutmayın.

## <a name="can-i-install-an-azure-ad-application-proxymanage-appsapplication-proxymd-connector-on-the-same-server-as-a-pass-through-authentication-agent"></a>Yükleyebilmek için bir [Azure AD uygulama proxy'si](../manage-apps/application-proxy.md) geçişli kimlik doğrulaması Aracısı ile aynı sunucuda bağlayıcı?

Evet. Geçişli kimlik doğrulaması Aracısı sürümü 1.5.193.0 rebranded sürümleri veya daha sonra bu yapılandırmayı desteklemez.

## <a name="what-versions-of-azure-ad-connect-and-pass-through-authentication-agent-do-you-need"></a>Hangi Azure AD Connect'i ve geçişli kimlik doğrulaması Aracısı sürümleri ihtiyacınız var?

Bu özelliğin çalışması için sürüm 1.1.750.0 gerekir ve daha sonra Azure AD Connect ve 1.5.193.0 veya doğrudan kimlik doğrulama aracısı için daha sonra. Tüm yazılım, Windows Server 2012 R2 veya üzeri sunuculara yükleyin.

## <a name="what-happens-if-my-users-password-has-expired-and-they-try-to-sign-in-by-using-pass-through-authentication"></a>Geçişli kimlik doğrulaması kullanarak oturum açmak, Bilgisayarım kullanıcı parolasının süresi dolmuşsa ne olur ve bunlar denemek ister misiniz?

Yapılandırdıysanız [parola geri yazma](../authentication/concept-sspr-writeback.md) belirli bir kullanıcı için ve geçişli kimlik doğrulaması kullanarak kullanıcı oturum açtığında, bunlar değiştirebilir veya parolalarını sıfırlayabilir. Parolalar şirket içi Active beklendiği gibi dizine geri yazılır.

Belirli bir kullanıcı için parola geri yazma özelliğini yapılandırmadıysanız ya da kullanıcının geçerli bir Azure AD yoksa, lisans atanmış, kullanıcı parolalarını bulutta güncelleştirilemiyor. Parolasının süresi dolan olsa bile parolalarını update yapılamıyor. Kullanıcı, bunun yerine bu iletiyi görür: "Kuruluşunuz bu sitede parolanızı güncelleştirmenize izin vermez. Kuruluşunuzun önerdiği yönteme göre güncelleştirin veya yardıma ihtiyacınız varsa yöneticinize sorun." Kullanıcı veya yönetici parolalarını şirket içi Active Directory'de sıfırlamanız gerekir.

## <a name="how-does-pass-through-authentication-protect-you-against-brute-force-password-attacks"></a>Nasıl geçişli kimlik doğrulaması parola deneme yanılma saldırılarına karşı koruma sağlar mı?

[Akıllı kilitleme hakkında bilgi edinin](../authentication/howto-password-smart-lockout.md).

## <a name="what-do-pass-through-authentication-agents-communicate-over-ports-80-and-443"></a>Ne geçişli kimlik doğrulama aracılarının 80 ve 443 bağlantı noktaları üzerinden iletişim?

- Kimlik doğrulama aracılarının tüm özellik işlemler için bağlantı noktası 443 üzerinden HTTPS istekleri olun.
- Kimlik doğrulama aracılarının HTTP isteklerinin SSL sertifika iptal listelerini (CRL'ler) indirmek için 80 numaralı bağlantı noktası üzerinden olun.

     >[!NOTE]
     >En son güncelleştirmeleri özelliği gereken bağlantı noktaları sayısı azaltıldı. Azure AD Connect kimlik doğrulaması aracısı veya eski sürümleri varsa, bu bağlantı noktaları de açık tutun: 5671, 8080, 9090 9091, 9350, 9352 ve 10100 10120.

## <a name="can-the-pass-through-authentication-agents-communicate-over-an-outbound-web-proxy-server"></a>Doğrudan kimlik doğrulama aracılarının bir giden web proxy sunucusu iletişim kurabilir?

Evet. Web Proxy Otomatik Bulma (WPAD) şirket içi ortamınızda etkinleştirilirse, kimlik doğrulama aracılarının otomatik olarak bulun ve ağ üzerinde bir web proxy sunucusu kullanma girişimi.

Ortamınızda WPAD yoksa, Azure AD ile iletişim kurmak geçişli kimlik doğrulaması Aracısı izin vermek için proxy bilgilerini (aşağıda gösterildiği gibi) ekleyebilirsiniz:
- Geçişli kimlik doğrulaması Aracısı sunucuda yüklemeden önce Internet Explorer'da proxy bilgilerini yapılandırın. Bu, kimlik doğrulama Aracısı yüklemesinin tamamlanması izin verir, ancak bunu hala olarak görünecek **Inactive** Yönetici portalı.
- Sunucusunda, "C:\Program Files\Microsoft Azure AD Connect kimlik doğrulaması aracısı için" gidin.
- "AzureADConnectAuthenticationAgentService" yapılandırma dosyasını düzenleyin ve aşağıdaki satırları ekleyin (Değiştir "http\:/ / contosoproxy.com:8080", gerçek proxy adresine sahip):

```
   <system.net>
      <defaultProxy enabled="true" useDefaultCredentials="true">
         <proxy
            usesystemdefault="true"
            proxyaddress="http://contosoproxy.com:8080"
            bypassonlocal="true"
         />
     </defaultProxy>
   </system.net>
```

## <a name="can-i-install-two-or-more-pass-through-authentication-agents-on-the-same-server"></a>Aynı sunucuda iki veya daha fazla geçişli kimlik doğrulama aracılarının yükleyebilir miyim?

Hayır, bir geçişli kimlik doğrulaması Aracısı yalnızca tek bir sunucuya yükleyebilirsiniz. Geçişli kimlik doğrulaması, yüksek kullanılabilirlik için yapılandırmak istiyorsanız [Buradaki yönergeleri izleyerek](how-to-connect-pta-quick-start.md#step-4-ensure-high-availability).

## <a name="do-i-have-to-manually-renew-certificates-used-by-pass-through-authentication-agents"></a>Geçişli kimlik doğrulaması aracısı tarafından kullanılan sertifikaları el ile yenilemeniz gerekir mi?

Her bir geçişli kimlik doğrulaması aracısı ve Azure AD arasındaki iletişim, sertifika tabanlı kimlik doğrulaması kullanılarak sağlanır. Bunlar [sertifikaları otomatik olarak yenilenir birkaç her ay Azure AD tarafından](how-to-connect-pta-security-deep-dive.md#operational-security-of -the-authentication-agents). Bu sertifikaları el ile yenilemek için gerek yoktur. Gerektiği gibi eski süresi dolmuş sertifikaları temizleyebilirsiniz.

## <a name="how-do-i-remove-a-pass-through-authentication-agent"></a>Geçişli kimlik doğrulaması Aracısı nasıl kaldırabilirim?

Geçişli kimlik doğrulaması aracısının çalıştığı sürece, etkin kalır ve sürekli olarak kullanıcı oturum açma isteklerini işler. Bir kimlik doğrulama aracısı kaldırmak istiyorsanız, Git **Denetim Masası -> Programlar -> Programlar ve Özellikler** ve her ikisi de kaldırırsanız **Microsoft Azure AD Connect kimlik doğrulaması Aracısı** ve  **Microsoft Azure AD Connect aracı güncelleştirici** programlar.

Geçişli kimlik doğrulaması blade onaylarsanız [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) önceki adımı tamamladıktan sonra kimlik doğrulama aracısı olarak gösteren görürsünüz **Inactive**. Bu _beklenen_. Kimlik doğrulaması Aracısı, birkaç gün sonra otomatik olarak listeden çıkarılır.

## <a name="i-already-use-ad-fs-to-sign-in-to-azure-ad-how-do-i-switch-it-to-pass-through-authentication"></a>Zaten AD FS için Azure AD'de oturum kullanıyorum. Nasıl, doğrudan kimlik doğrulamaya geçiş yapabilirim?

Geçişli kimlik doğrulaması için AD FS (veya diğer Federasyon teknolojileri) geçiriyorsanız, yayımlanan ayrıntılı dağıtım kılavuzunu izleyin öneririz [burada](https://github.com/Identity-Deployment-Guides/Identity-Deployment-Guides/blob/master/Authentication/Migrating%20from%20Federated%20Authentication%20to%20Pass-through%20Authentication.docx?raw=true).

## <a name="can-i-use-pass-through-authentication-in-a-multi-forest-active-directory-environment"></a>Çok ormanlı Active Directory ortamında geçişli kimlik doğrulaması kullanabilir miyim?

Evet. Çok ormanlı ortamları, Active Directory ormanlarınız arasında orman güveni varsa desteklenir ve ad soneki yönlendirme doğru şekilde yapılandırıldıysa.

## <a name="does-pass-through-authentication-provide-load-balancing-across-multiple-authentication-agents"></a>Geçişli kimlik doğrulaması, birden çok kimlik doğrulama aracılarının arasında yük dengelemeyi sağlar mı?

Hayır, birden fazla geçişli kimlik doğrulama aracılarının yüklenmesi yalnızca sağlar [yüksek kullanılabilirlik](how-to-connect-pta-quick-start.md#step-4-ensure-high-availability). Kimlik doğrulama aracılarının arasında belirleyici dengelemeyi sağlamaz. Herhangi bir kimlik doğrulama Aracısı (rastgele) belirli bir kullanıcı oturum açma isteği işleyebilir.

## <a name="how-many-pass-through-authentication-agents-do-i-need-to-install"></a>Yükleme kaç geçişli kimlik doğrulama aracılarının gerekiyor?

Birden fazla geçişli kimlik doğrulama aracılarının yüklenmesi sağlar [yüksek kullanılabilirlik](how-to-connect-pta-quick-start.md#step-4-ensure-high-availability). Ancak, kimlik doğrulama aracılarının arasında belirleyici dengelemeyi sağlamaz.

Kiracınızda görmeyi beklediğiniz oturum açma isteklerinin ortalama yük ve yoğun göz önünde bulundurun. Bir ölçüt tek bir kimlik doğrulama Aracısı ikinci bir standart bir 4 çekirdekli CPU, 16 GB RAM sunucu kimlik doğrulama işlemi 300-400 başa çıkabilir.

Ağ trafiğini tahmin etmek için aşağıdaki boyutlandırma yönergeleri kullanın:
- Her isteğin yükünün boyutuna sahip (0.5K + 1 K * num_of_agents) bayt; yani, verileri Azure ad kimlik doğrulama Aracısı. Burada, kimlik doğrulama aracılarının sayısını kiracınızda kayıtlı "num_of_agents" gösterir.
- Her yanıt yükü boyut olan 1 K bayta sahip; Azure AD'ye başka bir deyişle, veriler kimlik doğrulaması Aracısı'ndan.

Çoğu müşteri için toplam iki veya üç kimlik doğrulama aracılarının, yüksek kullanılabilirlik ve kapasite için yeterlidir. Oturum açma gecikme süresini iyileştirmek için etki alanı denetleyicilerinizin yakın kimlik doğrulama aracılarının yüklemeniz gerekir.

>[!NOTE]
>Kiracı başına 40 kimlik doğrulama aracılarının sistem sınırı yoktur.

## <a name="can-i-install-the-first-pass-through-authentication-agent-on-a-server-other-than-the-one-that-runs-azure-ad-connect"></a>Azure AD Connect'i çalıştıran farklı bir sunucuda ilk geçişli kimlik doğrulaması Aracısı yükleyebilirim?

Hayır, bu senaryo kullanılabilir _değil_ desteklenir.

## <a name="why-do-i-need-a-cloud-only-global-administrator-account-to-enable-pass-through-authentication"></a>Geçişli kimlik doğrulamasını etkinleştirmek için bir yalnızca bulut genel yönetici hesabıyla neden gerekiyor?

Etkinleştirmek veya doğrudan bir yalnızca bulut genel yönetici hesabını kullanarak kimlik doğrulamasını devre dışı önerilir. Hakkında bilgi edinin [bir yalnızca bulut genel yönetici hesabı ekleyerek](../active-directory-users-create-azure-portal.md). Bu şekilde yapmak için kiracınızın dışında kilitli kalmamanızı sağlar.

## <a name="how-can-i-disable-pass-through-authentication"></a>Geçişli kimlik doğrulaması nasıl devre dışı bırakabilirim?

Azure AD Connect Sihirbazı'nı yeniden çalıştırın ve kullanıcı oturum açma yöntemi, başka bir yönteme geçişli kimlik doğrulamasını değiştirin. Bu değişiklik, kiracıda geçişli kimlik doğrulaması devre dışı bırakır ve kimlik doğrulama Aracısı sunucudan kaldırır. Kimlik doğrulama aracılarının diğer sunuculardan el ile kaldırmanız gerekir.

## <a name="what-happens-when-i-uninstall-a-pass-through-authentication-agent"></a>Geçişli kimlik doğrulaması Aracısı kaldırabilirim ne olur?

Geçişli kimlik doğrulaması Aracısı bir sunucudan kaldırırsanız, sunucunun oturum açma istekleri kabul etmeyi durdurmasına neden olur. Kiracınızda oturum açma kullanıcı özelliği bozmayı önlemek için geçişli kimlik doğrulaması Aracısı kaldırmadan önce başka bir kimlik doğrulama Aracısı çalıştıran sahip olun.

## <a name="i-have-an-older-tenant-that-was-originally-setup-using-ad-fs--we-recently-migrated-to-pta-but-now-are-not-seeing-our-upn-changes-synchronizing-to-azure-ad--why-are-our-upn-changes-not-being-synchronized"></a>Özgün AD FS kullanarak Kurulum, daha eski bir kiracınız var.  Size yakın zamanda PTA için geçirilen, ancak artık Azure AD ile eşitliyorsanız UPN değişikliklerimizi görmüyor.  Bizim UPN neden olan olmayan eşitlenmesini değiştirir?

Y: Aşağıdaki durumlarda, şirket içi UPN değişikliklerinizi eşitlenmeyebilir:

- Azure AD kiracınız 15 Haziran 2015 tarihinden önce oluşturulduysa
- Kimlik doğrulaması için AD FS kullanarak Azure AD kiracınız ile başlangıçta federe
- Kimlik doğrulaması olarak PTA kullanan kullanıcılar yönetilen için geçiş

UPN değişiklikleri engellemek için varsayılan davranışı, 15 Haziran 2015 tarihinden önce oluşturulan kiracılar olduğu olmasıdır.  UPN değişiklik yapılmasını engelleyin kaldırma gerekiyorsa, aşağıdaki PowerShell cmdlt'sini çalıştırmanız gerekir:  

`Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers-Enable $True`

Kiracılar, 15 Haziran 2015 tarihinden sonra oluşturulan UPN değişiklikleri eşitleme varsayılan davranışa sahip.   



## <a name="next-steps"></a>Sonraki adımlar
- [Geçerli sınırlamalar](how-to-connect-pta-current-limitations.md): Hangi senaryolar desteklenir ve hangilerinin olmayan öğrenin.
- [Hızlı Başlangıç](how-to-connect-pta-quick-start.md): Azure AD geçişli kimlik doğrulaması ve çalışır duruma getirin.
- [AD FS'den doğrudan kimlik doğrulamaya geçiş](https://github.com/Identity-Deployment-Guides/Identity-Deployment-Guides/blob/master/Authentication/Migrating%20from%20Federated%20Authentication%20to%20Pass-through%20Authentication.docx?raw=true) -geçişli kimlik doğrulaması için AD FS (veya diğer Federasyon teknolojileri) geçirmek için ayrıntılı bir kılavuz.
- [Akıllı kilitleme](../authentication/howto-password-smart-lockout.md): Akıllı kilitleme özelliğini kiracınızda kullanıcı hesapları korumak için yapılandırmayı öğrenin.
- [Teknik yakından bakışın](how-to-connect-pta-how-it-works.md): Geçişli kimlik doğrulaması özelliği nasıl çalıştığını anlayın.
- [Sorun giderme](tshoot-connect-pass-through-authentication.md): Geçişli kimlik doğrulaması özelliği olan yaygın sorunların nasıl çözümleneceğini öğrenin.
- [Güvenliğe derinlemesine bakış](how-to-connect-pta-security-deep-dive.md): Geçişli kimlik doğrulaması özelliği hakkında ayrıntılı teknik bilgilerini edinin.
- [Azure AD sorunsuz çoklu oturum açma](how-to-connect-sso.md): Tamamlayıcı bu özellik hakkında daha fazla bilgi edinin.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): Yeni özellik istekleriniz dosya için Azure Active Directory Forumu kullanın.

