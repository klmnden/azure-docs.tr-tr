---
title: Şirket içi Azure AD uygulama ara sunucusu ile uygulama yayımlama
description: Uygulama proxy'si harici olarak uzak kullanıcılar için şirket içi web uygulamalarında yayımlama için kullanılacak neden anlayın. Uygulama Ara sunucusu mimarisi, bağlayıcı, kimlik doğrulama yöntemleri ve güvenlik avantajları hakkında bilgi edinin.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: overview
ms.workload: identity
ms.date: 05/31/2019
ms.author: mimart
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5f23b20d460952ae582c292c8015851b9dc2ea98
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67108169"
---
# <a name="using-azure-ad-application-proxy-to-publish-on-premises-apps-for-remote-users"></a>Yayımlamak için Azure AD uygulama ara sunucusu kullanarak uzak kullanıcılara yönelik uygulamaların şirket

Azure Active Directory (Azure AD), kullanıcılar, uygulamaları ve verileri bulutta ve şirket içi korumaya yönelik çok sayıda özellik sunar. Özellikle, Azure AD uygulama proxy'si özelliği, harici olarak şirket içi web uygulamalarında yayımlama için isteyen BT uzmanları tarafından uygulanabilir. Dahili uygulamalara erişmesi gereken uzak kullanıcılar daha sonra bunları güvenli bir şekilde erişebilirsiniz.

Dahili uygulamalardan ağınızın dışından güvenli bir şekilde erişim olanağı modern çalışma alanında daha önemli hale gelir. KCG (kendi cihazını Getir) ve mobil cihazlar gibi senaryoları sayesinde, BT uzmanları, iki amaçlarını karşılamak için istendi:

* Son kullanıcıların her yerden ve dilediğiniz zaman üretken güçlendirin
* Kurumsal varlıkları her zaman koruyun

Birçok kuruluşun denetimi sizdedir ve kaynakları şirket ağlarına sınırlar içinde bulunduğunda korumalı inanıyoruz. Ancak, günümüzün dijital çalışma alanında yönetilen mobil cihazlar ve kaynaklar ve bulut Hizmetleri, sınır genişletmiştir. Artık kullanıcılarınızın kimliklerini ve cihazlarını ve uygulamalarını üzerinde depolanan verileri korumaya karmaşıklığını yönetmek gerekir.

Belki de Office 365 ve diğer SaaS uygulamalarına, hem de barındırılan uygulamaları şirket içi web erişmesi gereken bulut kullanıcıları yönetmek için zaten Azure AD kullanıyorsanız. Azure AD zaten varsa, şirket içi uygulamalarınızı sorunsuz ve güvenli erişim izni vermek için bir denetim düzlemi olarak yararlanabilirsiniz. Veya belki de yine de buluta taşıma söz. Bu durumda, uygulama proxy'si ekleme ve güçlü Identity foundation oluşturmaya yönelik ilk adım olarak bulut yolculuğunuza başlayabilirsiniz.

Kapsamlı olsa da aşağıdaki liste bir karma bir arada bulunma senaryosunda uygulama proxy'si uygulayarak etkinleştirebilirsiniz şeylerden bazıları gösterilmektedir:

* Şirket içi web uygulamaları dışarıdan bir DMZ olmadan basitleştirilmiş bir şekilde yayımlayın
* Cihazlar, kaynakları ve Bulut ve şirket içi uygulamalarda çoklu oturum açma (SSO) desteği
* Bulut ve şirket içi uygulamalar için çok faktörlü kimlik doğrulaması desteği
* Bulut özellikleri, Microsoft Cloud security ile hızlı bir şekilde yararlanın
* Kullanıcı hesabı yönetimini merkezden gerçekleştirin
* Kimlik ve güvenlik denetimi merkezileştirin
* Otomatik olarak ekleyin veya grup üyeliğine dayalı uygulamalara kullanıcı erişimini Kaldır

Bu makalede, Azure AD'nin açıklanır ve uygulama proxy'si uzak kullanıcıların bir çoklu oturum açma (SSO) deneyimi sağlar. Kullanıcıların güvenli bir VPN veya çift bağlantılı bir sunucu ve güvenlik duvarı kuralları olmadan şirket içi uygulamalara bağlanın. Bu makalede nasıl uygulama proxy'si, şirket içi web uygulamaları için bulut güvenlik avantajları ve özellikleri getirir anlamanıza yardımcı olur. Ayrıca, olası topolojiler ve mimarisini açıklar.

## <a name="remote-access-in-the-past"></a>Daha önce uzaktan erişim

Daha önce tüm DMZ veya çevre ağ içinde iç kaynaklara uzaktan kullanıcıların erişimini kolaylaştırırken saldırganlardan korumak için Denetim düzlemi oluştu. Ancak, şirket kaynaklarına erişmek için dış istemciler tarafından kullanılan DMZ'deki dağıtılan ters proxy çözümleri ve VPN bulut Dünyası için uygun değildir. Bunlar, genellikle aşağıdaki dezavantajları olumsuz:

* Donanım maliyetleri
* (Düzeltme eki uygulama, izleme bağlantı noktaları, vb.) güvenliğini sağlama
* Uç cihazlarında kullanıcıların kimliğini doğrulama
* Çevre ağındaki web sunucuları için kullanıcıların kimliğini doğrulama
* Dağıtım ve VPN istemci yazılımını yapılandırma uzak kullanıcılar için VPN erişimi sağlama. Ayrıca, etki alanına katılmış sunucular çevre ve dış saldırılara karşı savunmasız olabilir koruma.

Günümüzün bulut öncelikli dünyada, Azure AD'ye kimin ve ne ağınıza alır idealdir. Azure AD uygulama proxy'si, modern kimlik doğrulaması ve SaaS uygulamaları ve kimlik sağlayıcıları gibi bulut tabanlı teknolojiler ile tümleşir. Kullanıcılar uygulamalarına her yerden erişmek Bu tümleştirme sağlar. Yalnızca uygulama proxy'si, günümüzün dijital çalışma alanı için daha uygun, daha güvenli VPN ve ters proxy çözümleri ve uygulamak daha kolay olan. Uzak kullanıcıların şirket içi uygulamalarınızı, O365 ve Azure AD ile tümleştirilmiş diğer SaaS uygulamalarına eriştikleri aynı şekilde erişebilirsiniz. Değiştirmeniz veya uygulamalarınızı uygulaması Ara sunucusu ile çalışacak şekilde güncelleştirmeniz gerekmez. Ayrıca, uygulama ara sunucusu, güvenlik duvarı üzerinden gelen bağlantı açmanız gerekmez. Uygulama Ara sunucusu ile yalnızca ayarlayın ve unutursanız.

## <a name="the-future-of-remote-access"></a>Uzaktan erişim geleceği

Günümüzün dijital çalışma alanında, kullanıcılar birden çok cihaz ve uygulamaları her yerde çalışır. Yalnızca kullanıcı kimliği sabittir. İşte bu ilk adım bir güvenli ağ bugün kullanmaktır [Azure AD'nin Kimlik Yönetimi](https://docs.microsoft.com/azure/security/security-identity-management-overview) özellikleri, güvenlik denetim düzlemi olarak. Kimlik, Denetim düzlemi kullanan bir model genellikle aşağıdaki bileşenlerden oluşur:

* Kullanıcılar ve kullanıcı ile ilgili bilgileri izlemek için bir kimlik sağlayıcısı.
* Şirket kaynaklarına erişimi olan cihazların bir listesini korumak için cihaz dizin. Bu dizin, karşılık gelen cihaz bilgilerini içerir (örneğin, cihaz, bütünlük yazın vs.).
* Güvenlik yöneticileri tarafından ortaya konan ilkesi için kullanıcı ve cihaz belirlemek için ilke değerlendirme hizmeti uyar.
* Kuruluş kaynaklarına erişim vermek veya reddetmek için yeteneği.

Uygulama Ara sunucusu ile Azure AD web apps yayımlanan şirket içi erişmesi gereken kullanıcıları ve bulut izleme tutar. Bu, bu uygulamaları için bir merkezi yönetim noktası sağlar. Gerekli olmasa da Azure AD koşullu erişim etkinleştirmeniz önerilir. Kullanıcıların kimlik doğrulaması ve erişim kazanmak için koşullar tanımlayarak, başka uygulamalara erişim doğru kişilere inceletme emin olun.

**Not:** Azure AD uygulama ara sunucusu olarak bir VPN ya da ters proxy değiştirme iç kaynaklara erişmesi gereken dolaşım (veya uzak) kullanıcılar için tasarlanmıştır anlamak önemlidir. Kurumsal ağ üzerindeki iç kullanıcılar için tasarlanmamıştır. Uygulama proxy'si gereksiz yere kullanan iç kullanıcılar beklenmedik ve istenmeyen performans sorunlarına yol açabilir.

![Azure Active Directory ve tüm uygulamalar](media/what-is-application-proxy/azure-ad-and-all-your-apps.png)

### <a name="an-overview-of-how-app-proxy-works"></a>Uygulama proxy'si nasıl çalıştığına ilişkin bir genel bakış

Uygulama Ara sunucusu, Azure portalında yapılandırmanız, Azure AD bir hizmettir. Dış ortak HTTP/HTTPS URL'si uç noktası, kuruluşunuzdaki bir iç uygulama sunucusu URL'si bağlandığı Azure bulutunda yayımlamanıza olanak sağlar. Bu şirket içi web uygulamaları, çoklu oturum açmayı desteklemek için Azure AD ile tümleştirilebilir. Web uygulamaları Office 365 ve diğer SaaS uygulamalarına eriştikleri aynı şekilde şirket erişim sonra son kullanıcıların kullanabilirsiniz.

Bu özellik bileşenlerinin bir şirket içi sunucu ve kimlik sağlayıcısı olan Azure AD üzerinde çalışan basit bir aracı uygulama Proxy Bağlayıcısı bulutta çalışan uygulama proxy'si hizmeti içerir. Her üç bileşenin erişmek için kullanıcı ile çoklu oturum açma deneyimini şirket web uygulamaları sağlamak için birlikte çalışır.

Oturum açtıktan sonra dış kullanıcılar şirket içi web uygulamaları alışık olduğunuz bir URL'yi kullanarak erişebilir veya [MyApps erişim paneli](https://docs.microsoft.com/azure/active-directory/user-help/my-apps-portal-end-user-access) Masaüstü veya iOS/MAC cihazlarından. Örneğin, uygulama ara sunucusu uzaktan erişim sağlayabilir ve çoklu oturum açma için Uzak Masaüstü, SharePoint siteleri, Tableau, Qlik, satır iş kolu (LOB) uygulamaları ve web üzerinde Outlook'u.

![Azure AD uygulama ara sunucusu mimarisi](media/what-is-application-proxy/azure-ad-application-proxy-architecture.png)

### <a name="authentication"></a>Kimlik Doğrulaması

Bir uygulama için çoklu oturum açmayı yapılandırmak için birkaç yolu vardır ve uygulamanızın kullandığı kimlik doğrulamasını seçtiğiniz yönteme bağlıdır. Uygulama Ara sunucusu, aşağıdaki uygulama türlerini destekler:

* Web uygulamaları
* Web için zengin uygulamalar farklı cihazlarda kullanıma sunmak istiyorsanız API'leri
* Uzak Masaüstü Ağ Geçidi barındırılan uygulamalar
* Active Directory Authentication Library (ADAL) ile tümleşik olan zengin istemci uygulamaları

Uygulama Ara sunucusu aşağıdaki yerel kimlik doğrulama protokolünü kullanan uygulamalar ile çalışır:

* **[Tümleşik Windows kimlik doğrulaması (IWA)](application-proxy-configure-single-sign-on-with-kcd.md).** IWA için Kerberos uygulama kullanıcıların kimliğini doğrulamak için Kerberos Kısıtlı temsilci (KCD) uygulama ara sunucusu bağlayıcıları kullanın.

Uygulama Ara sunucusu, ayrıca üçüncü taraf tümleştirme sunan veya belirli yapılandırma senaryoları aşağıdaki kimlik doğrulama protokollerini destekler:

* [**Üst bilgi tabanlı kimlik doğrulaması**](application-proxy-configure-single-sign-on-with-ping-access.md). Bu oturum açma yöntemi PingAccess adlı bir üçüncü taraf kimlik doğrulama hizmeti kullanır ve uygulama için kimlik doğrulama üst bilgileri kullandığında kullanılır. Bu senaryoda, kimlik doğrulaması PingAccess tarafından işlenir.
* [**Forms veya parola tabanlı kimlik doğrulama**](application-proxy-configure-single-sign-on-password-vaulting.md). Bu kimlik doğrulama yöntemi ile kullanıcıların uygulama için bir kullanıcı adı ve parola ile bunların erişim ilk kez oturum. İlk oturum açma işleminden sonra Azure AD kullanıcı ve uygulama parolasını sağlar. Bu senaryoda, kimlik doğrulaması, Azure AD tarafından işlenir.
* [**SAML kimlik doğrulaması**](application-proxy-configure-single-sign-on-on-premises-apps.md). SAML tabanlı çoklu oturum açma SAML 2.0 veya WS-Federasyon protokollerini kullanan uygulamalar için desteklenir. SAML çoklu oturum açma ile Azure AD kullanıcının Azure AD hesabı kullanarak uygulamaya kimliğini doğrular.

Desteklenen yöntemler hakkında daha fazla bilgi için bkz. [tek bir oturum açma yöntemini seçme](what-is-single-sign-on.md#choosing-a-single-sign-on-method).

### <a name="security-benefits"></a>Güvenlik açısından faydalı

Uygulama proxy'si ve Azure AD tarafından sağlanan uzak erişim çözümü desteği güvenlik avantaj da dahil olmak üzere, müşteriler, avantajlarından yararlanmak:

* **Kimliği doğrulanmış erişim**. Uygulama proxy'si ile uygulama yayımlamak için en uygun [ön kimlik doğrulama](application-proxy-security.md#authenticated-access) yalnızca kimliği doğrulanmış bağlantılar, ağ isabet emin olmak için. Ön kimlik doğrulama ile yayımlanan uygulamalar için uygulama proxy'si hizmeti aracılığıyla şirket içi ortamınıza geçerli bir belirteç olmadan geçirmek için hiçbir trafiğe izin verilir. Yalnızca kimliği doğrulanmış kimliğe arka uç uygulaması erişebileceği ön kimlik doğrulama, yapısı gereği, hedeflenen saldırıları, çok sayıda engeller.
* **Koşullu erişim**. Ağ bağlantıları kurulan önce daha zengin ilke denetimleri uygulanabilir. Koşullu erişim ile arka uç uygulamanızın isabet olanak trafiği kısıtlamalar tanımlayabilirsiniz. Oturum açma kimlik doğrulaması ve kullanıcı riski profili gücünü konuma göre kısıtlayan ilkeler oluşturursunuz. Koşullu erişim geliştikçe tümleştirme gibi ek güvenlik Microsoft Cloud App Security (MCAS) sağlamak için daha fazla denetim eklenmektedir. MCAS tümleştirme için şirket içi uygulama yapılandırmanıza olanak sağlar [gerçek zamanlı izleme](application-proxy-integrate-with-microsoft-cloud-application-security.md) izlemek ve denetlemek için koşullu erişim yararlanarak gerçek zamanlı oturumlarda koşullu erişim ilkelerine bağlı.
* **Trafiği sonlandırma**. Oturum ile arka uç sunucusuna yeniden belirlenirken tüm trafiği arka uç uygulaması için uygulama ara Sunucusu hizmetine bulutta sonlandırılır. Bu bağlantı stratejisi, arka uç sunucuları HTTP trafiği için açık değildir anlamına gelir. Güvenlik duvarınızı Saldırıya uğramış olmadığından bunlar hedeflenen (--hizmet reddi) DoS saldırılarına karşı daha iyi korunur.
* **Tüm erişim giden**. Uygulama Ara sunucusu bağlayıcıları 80 ve 443 bağlantı noktaları üzerinden giden bağlantılara izin uygulama proxy'si hizmeti yalnızca bulutta kullanın. Herhangi bir gelen bağlantı ile güvenlik duvarı bağlantı noktalarının gelen bağlantıları veya bileşenleri için DMZ'deki açmaya gerek yoktur. Tüm bağlantılar, giden ve güvenli bir kanal üzerinden gerçekleştirilir.
* **Güvenlik analizi ve Machine Learning (ML) göre zeka**. Azure Active Directory'nin parçası olduğu için uygulama proxy'si yararlanabilir [Azure AD kimlik koruması](https://docs.microsoft.com/azure/active-directory/identity-protection/overview) (gerektirir [Premium P2 lisansı](https://azure.microsoft.com/pricing/details/active-directory/)). Azure AD kimlik koruması, Microsoft'un veri akışlarından ile makine öğrenimi güvenlik bilgileri birleştirir [dijital Suçlar birimi](https://news.microsoft.com/stories/cybercrime/index.html) ve [Microsoft Security Response Center](https://www.microsoft.com/msrc) proaktif olarak tanımlamak için ele geçirilen hesaplar. Kimlik koruması, yüksek riskli oturum açma işlemleri gerçek zamanlı koruma sunar. İşlem içine anonymizing ağlarına virüs bulaşmış cihazlardan veya alışılmadık ve tahmin edilemez konumlardan erişir gibi faktörleri göz önünde bulundurarak risk profili oturumunun artırmak için alır. Bu riski profili, gerçek zamanlı koruma için kullanılır. Bu raporlar ve olaylar çoğunu zaten SIEM Sistemlerinizle ile tümleştirmeye yönelik bir API aracılığıyla kullanılabilir.

* **Hizmet olarak uzaktan erişim**. Koruma ve şirket içi sunucular, uzaktan erişimi etkinleştirmek için düzeltme eki uygulama hakkında endişelenmeniz gerekmez. Her zaman en son güvenlik düzeltme eklerinin ve yükseltmelerini almak için uygulama ara sunucusu, Microsoft sahibi, bir internet ölçek hizmetidir. Çok sayıda saldırıları için hala yüklenmemiş yazılım hesaplar. Göre departmanı, anlarında güvenlik, olabildiğince çok [hedeflenmiş saldırılara yüzde 85'i önlenebilir](https://www.us-cert.gov/ncas/alerts/TA15-119A). Bu hizmet modeli ile uç sunucularınızın artık yönetme ağır yükü gerçekleştirmek ve bunları gerektiği şekilde düzeltme eki uygulama karıştırmak gerekmez.

* **Intune tümleştirmesi**. Intune ile şirket trafik kişisel trafiği ayrı olarak yönlendirilir. Uygulama proxy'si, şirket trafiğin kimliğinin doğrulanmasını sağlar. [Uygulama Ara sunucusu ve Intune Managed Browser](https://docs.microsoft.com/intune/app-configuration-managed-browser#how-to-configure-application-proxy-settings-for-protected-browsers) özelliği de birlikte uzak kullanıcıların güvenli bir şekilde iç Web siteleri iOS ve Android cihazlarından erişim sağlamak için kullanılabilir.

### <a name="roadmap-to-the-cloud"></a>Bulut yol haritası

Şirket içi ortamınız için Azure AD uygulama ara sunucusu uygulama başka bir önemli avantaj genişletiyor. Aslında, uygulama proxy'si ekleme, kuruluşunuz ve uygulamaları buluta taşıma konusunda bir önemli adımdır. Bulutta ve şirket içi kimlik doğrulaması uzağa taşıyarak, kendi şirket içi ayak izini azaltmak ve Azure AD'nin kimlik yönetimi özellikleri, Denetim düzlemi kullanın. En düşük ile veya mevcut uygulamaları güncelleştirme, çoklu oturum açma, çok faktörlü kimlik doğrulaması ve merkezi yönetimi gibi özellikler buluta erişebilirsiniz. Uygulama Ara sunucusu için gerekli bileşenleri yükleme, bir uzaktan erişim altyapısı oluşturma için basit bir işlemdir. Ve buluta taşıyarak, en son Azure AD özellikleri, güncelleştirmeleri ve yüksek kullanılabilirlik ve olağanüstü durum kurtarma gibi işlevleri erişebilirsiniz.

Uygulamalarınızı Azure AD'ye geçirmek hakkında daha fazla bilgi için bkz: [bilgisayarınızı uygulamaları Azure Active Directory'ye geçirme](https://aka.ms/migrateapps/whitepaper) teknik incelemesi.

## <a name="architecture"></a>Mimari

Aşağıdaki diyagram, Azure AD kimlik doğrulama hizmetleri genel gösterir ve çoklu oturum açma için birlikte sağlamak için uygulama proxy'si iş şirket uygulamalarını son kullanıcılara.

![Azure AD uygulama ara sunucusu kimlik doğrulama akışı](media/what-is-application-proxy/azure-ad-application-proxy-authentication-flow.png)

1. Kullanıcı, bir uç noktası aracılığıyla uygulama eriştiğini sonra kullanıcının Azure AD oturum açma sayfasına yönlendirilir. Koşullu erişim ilkelerini yapılandırdıysanız, belirli koşullar şu anda kuruluşunuzun güvenlik gereksinimlerine uygun emin olmak için denetlenir.
2. Bir başarılı oturum açma işleminden sonra Azure AD kullanıcının istemci cihaza bir belirteç gönderir.
3. İstemci belirteci güvenlik asıl adı (SPN) ve kullanıcı asıl adı (UPN) belirteçten alır. uygulama ara Sunucusu hizmetine gönderir.
4. Uygulama proxy'si uygulaması Ara sunucusu tarafından devralındığında isteği iletir [bağlayıcı](application-proxy-connectors.md).
5. Bağlayıcı kullanıcı adına gerekli tüm ek kimlik doğrulama gerçekleştirir (*isteğe bağlı kimlik doğrulama yöntemine bağlı olarak*), uygulama sunucusunun iç uç nokta istekleri ve şirket içi isteği gönderir uygulama.
6. Uygulama sunucusu yanıttan bağlayıcı aracılığıyla uygulama ara Sunucusu hizmetine gönderilir.
7. Yanıt, kullanıcıya uygulama proxy'si hizmetten gönderilir.

|**Bileşen**|**Açıklama**|
|:-|:-|
|Uç Nokta|Uç nokta bir URL olduğundan veya bir [son kullanıcı portalı](end-user-experiences.md). Kullanıcılar, uygulamalar, Ağ üzerindeyken dışında bir dış URL erişerek ulaşabilirsiniz. Ağınızdaki kullanıcılar, bir URL veya bir son kullanıcı portalı uygulamaya erişebilir. Kullanıcılar bu uç noktaları birine gittiğinizde, Azure AD'de kimlik doğrulaması yapmak ve şirket içi uygulama Bağlayıcısı üzerinden yönlendirilir.|
|Azure AD|Azure AD, bulutta depolanan Kiracı dizinini kullanarak kimlik doğrulaması gerçekleştirir.|
|Uygulama proxy'si hizmeti|Bu uygulama proxy'si hizmeti Azure AD parçası olarak bulutta çalışır. Kullanıcı oturum açma belirteci için uygulama ara sunucusu Bağlayıcısı geçirir. Uygulama proxy'si, istekte erişilebilir üst bilgileri iletir ve kendi protokol başına üstbilgileri için istemci IP adresini ayarlar. Gelen istek için proxy bu başlığı zaten varsa, istemci IP adresi üstbilgisinin değerini virgülle ayrılmış liste sonuna eklenir.|
|Uygulama Ara sunucusu Bağlayıcısı|Bağlayıcı ağınızdaki bir Windows Server üzerinde çalışan basit bir aracıdır. Bağlayıcıyı şirket içi uygulama ile bulutta uygulama proxy'si hizmeti arasındaki iletişimi yönetir. Yalnızca bağlayıcı giden bağlantılar kullanır, gelen bağlantı noktalarının açık veya her şeyi bir DMZ'ye yerleştirmeniz gerekmez. Bağlayıcılar, durum bilgisiz olduğundan ve gerektiği şekilde buluttan bilgi isteme. Nasıl gibi bağlayıcılar hakkında daha fazla bilgi için Yük Dengeleme ve kimlik doğrulaması için bkz [anlamak Azure AD uygulama ara sunucusu bağlayıcıları](application-proxy-connectors.md).|
|Active Directory (AD)|Active Directory etki alanı hesapları için kimlik doğrulaması gerçekleştirmek üzere şirket içinde çalıştırır. Bağlayıcı, çoklu oturum açma yapılandırıldığında, gereken ek kimlik doğrulaması gerçekleştirmek için AD ile iletişim kurar.|
|Şirket içi uygulama|Son olarak, kullanıcının şirket içi uygulamaya erişebilir.|

Azure AD uygulama proxy'si, bulut tabanlı uygulama proxy'si hizmeti ve şirket içi Bağlayıcısı oluşur. Bağlayıcı, uygulama proxy'si hizmeti isteklerini dinler ve iç uygulamaları bağlantıları işler. Tüm iletişim SSL üzerinden oluşur ve her zaman, uygulama proxy'si hizmeti bağlayıcı kaynaklanan unutulmaması önemlidir. Diğer bir deyişle, yalnızca giden iletişimleridir. Bağlayıcı, tüm çağrıları için uygulama proxy'si hizmeti kimlik doğrulaması için bir istemci sertifikası kullanır. Bağlantı güvenliği tek özel durumu istemci sertifikasını burada kurulur ilk kurulum adımdır. Uygulama proxy'si bkz [başlık altında](application-proxy-security.md#under-the-hood) daha fazla ayrıntı için.

### <a name="application-proxy-connectors"></a>Uygulama Proxy Bağlayıcısı

[Uygulama Ara sunucusu bağlayıcılarını](application-proxy-connectors.md) dağıtılan basit aracıları, buluttaki uygulama ara Sunucusu hizmetine giden bağlantı kolaylaştıran şirket içinde. Bağlayıcılar, arka uç uygulaması erişimi olan bir Windows Server üzerinde yüklenmelidir. Kullanıcılar, bu yollar trafiklerini bağlayıcı olarak aracılığıyla uygulamalara, aşağıda gösterildiği uygulama proxy'si bulut hizmetine bağlanır.

![Azure AD uygulama ara sunucusu ağ bağlantıları](media/what-is-application-proxy/azure-ad-application-proxy-network-connections.png)

Kurulumu ve kaydı bir bağlayıcı ve uygulama proxy'si hizmeti arasında şu şekilde gerçekleştirilir:

1. BT yöneticisi, 80 ve 443 giden trafik için bağlantı noktalarını açar ve bağlayıcının, uygulama proxy'si hizmeti ve Azure AD tarafından gereken birkaç URL'lere erişim sağlar.
2. Yönetici, Azure portalında oturum açtığında ve şirket içi Windows server üzerinde bağlayıcı'yı yüklemek için bir yürütülebilir dosyayı çalıştırır.
3. "Uygulama ara Sunucusu hizmetine dinlemek" Bağlayıcısı'nı başlatır.
4. Yönetici, şirket içi uygulamanızı Azure AD'ye ekler ve URL'leri kullanıcıların, uygulamalarına bağlanmak gereken gibi ayarları yapılandırır.

Daha fazla bilgi için [bir Azure AD uygulama ara sunucusu dağıtımını planlama](application-proxy-deployment-plan.md).

Her zaman yedeklilik ve ölçek için birden fazla bağlayıcı dağıtmak önerilir. Bağlayıcılar, hizmet ile birlikte tüm yüksek kullanılabilirlik görevlerini ölçeklendirilmesini ve eklenebilir veya dinamik olarak kaldırılmış. Yeni bir istek geldiğinde her zaman bu kullanılabilir bağlayıcıların birine yönlendirilir. Bir bağlayıcı çalışırken hizmete bağlandığında etkin kalır. Bir bağlayıcıyı geçici olarak kullanılamıyorsa, giden trafiğin yanıt vermiyor. Kullanılmayan bağlayıcılar etkin değil olarak etiketlenmiş ve kaldıktan sonra 10 gün kaldırıldı.

Bağlayıcılar ayrıca server connector'ın daha yeni bir sürümü olup olmadığını öğrenmek için yoklama. El ile güncelleştirme yapabileceğiniz rağmen Application Proxy Connector Updater hizmetini çalıştıran sürece bağlayıcılar otomatik olarak güncelleştirecektir. Birden çok Bağlayıcılarla kiracılar için Otomatik Güncelleştirmeler ortamınızdaki kesinti süresini önlemek için her grupta bir anda tek bir bağlayıcıyı hedefleyin.

**Not**: Uygulama proxy'si izleyebilirsiniz [sürüm geçmişi sayfası](application-proxy-release-version-history.md) güncelleştirmeleri, RSS akışına abone olarak yayımlanan bildirilmesini sağlamak için.

Her uygulama ara sunucusu bağlayıcısını atanan bir [bağlayıcı grubu](application-proxy-connector-groups.md). Bağlayıcılar aynı bağlayıcı grubunda yüksek kullanılabilirlik için tek bir birim olarak davranmasına ve Yük Dengeleme. Yeni gruplar oluşturabilir, Azure Portalı'nda bağlayıcılar atamanız ve ardından belirli uygulamalar sunmak için özel bağlayıcılar atayın. En az iki bağlayıcı her bir bağlayıcı grubu için yüksek kullanılabilirlik sağlamak için önerilir.

Bağlayıcı grupları, aşağıdaki senaryoları desteklemek için ihtiyacınız olduğunda yararlıdır:

* Coğrafi uygulama yayımlama
* Uygulama kesimleme/yalıtımı
* Bulutta veya şirket içinde çalışan web uygulamalarını yayımlama

Bağlayıcılarınızı yükleyin ve ağınızın en iyi duruma getirme görmek yeri seçme hakkında daha fazla bilgi için [Azure Active Directory Uygulama proxy'si kullanılırken ağ topolojisi hakkında önemli noktalar](application-proxy-network-topology.md).

## <a name="other-use-cases"></a>Diğer kullanım örnekleri

Bu noktaya kadar tüm Bulut ve şirket içi uygulamalar için çoklu oturum açma sağlarken şirket içi uygulamaları dışarıdan yayımlamak için uygulama proxy'si kullanarak odaklandık. Ancak, uygulama proxy'si için söz olan diğer kullanım örnekleri vardır. Bunlara aşağıdakiler dahildir:

* **REST API'leri güvenle yayınlamamızı**. İş mantığı olan veya API'ler çalıştıran şirket içi veya buluttaki sanal makineler üzerinde barındırılan, uygulama proxy'si genel bir uç nokta için API erişim sağlar. API uç noktası erişimi denetim kimlik doğrulaması ve yetkilendirme gelen bağlantı noktalarını gerek kalmadan sağlar. Masaüstü bilgisayarlar, iOS, MAC ve Intune kullanarak Android cihazları için çok faktörlü kimlik doğrulaması ve cihaz tabanlı koşullu erişim gibi Azure AD Premium özellikleri aracılığıyla ek güvenlik sağlar. Daha fazla bilgi için bkz. [proxy uygulamaları ile etkileşim kurmak yerel istemci uygulamaları etkinleştirme](application-proxy-configure-native-client-application.md) ve [Azure Active Directory ile API Management OAuth 2.0 kullanarak bir API'yi koruma](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad).
* **Uzak Masaüstü Hizmetleri** **(RDS)** . Standart RDS dağıtımlarına açık gelen bağlantıları gerektirir. Ancak, [uygulama ara sunucusu ile bir RDS dağıtımının](application-proxy-integrate-with-remote-desktop-services.md) bağlayıcı hizmetini çalıştıran sunucudan kalıcı giden bağlantı vardır. Bu şekilde, daha fazla uygulamalarını son kullanıcılara Uzak Masaüstü Hizmetleri üzerinden şirket içi uygulamaları yayımlama tarafından sunabilir. Dağıtım RDS'yi için saldırı yüzeyini sınırlı sayıda iki aşamalı doğrulama ve koşullu erişim denetimleri ile de azaltabilir
* **WebSockets kullanarak bağlanan uygulamaları yayımlama**. Destek [Qlik Sense](application-proxy-qlik.md) genel Önizleme aşamasındadır ve diğer uygulamalara gelecekte genişletilir.
* **Yerel istemci uygulamalarının proxy uygulamalarla etkileşimi etkinleştirmek**. Azure AD uygulama proxy'si web uygulamaları yayımlamak için kullanabileceğiniz, ancak yayımlamak için de kullanılabilir [yerel istemci uygulamaları](application-proxy-configure-native-client-application.md) Azure AD Authentication Library (ADAL) ile yapılandırılır. Bir tarayıcı erişilen Web uygulamaları sırada bir cihazda yüklü için yerel istemci uygulamaları, web uygulamalarından farklı.

## <a name="conclusion"></a>Sonuç

Şu şekilde çalışır ve kullandığımız araçları gelişiyor. Çalışmak için daha fazla çalışanların kendi aygıtlarını getiren ve hizmet olarak yazılım-a-(SaaS) uygulamaları yaygın kullanımı sayesinde şekilde kuruluşların yönetmek ve güvenli verilerine de gelişmeye gerekir. Şirketler artık kendi kenarlık çevreleyen bir moat tarafından korunan yalnızca kendi duvarları içinde çalışır. Çapraz geçen zamankinden--çok daha fazla konumlara veri şirket içi ve bulut ortamlarında. Bu evrim kullanıcıların üretkenlik ve işbirliği olanağı artırmak yardımcı olmuştur, ancak daha zor de koruma hassas verileri sağlar.

Karma bir arada bulunma senaryoda kullanıcıları yönetmek için Azure AD şu anda kullanmakta olduğunuz ya da bulut yolculuğunuzda başlangıç ilgilendiğiniz olsun, Azure AD uygulama proxy'si uygulama sağlayarak şirket içi kaplama alanınızı uzak boyutunu azaltmaya yardımcı olabilir Hizmet olarak erişin.

Kuruluşlar, uygulama aşağıdaki avantajlardan yararlanmak için Proxy bugün avantajından yararlanmaya başlaması gereken:

* Şirket içi uygulamaları, geleneksel VPN veya diğer şirket içi web çözümleri ve DMZ yaklaşım yayımlama koruma ile ilgili ek yükü olmadan harici olarak yayımlama
* Çoklu oturum açmayı tüm uygulamalara, Office 365 veya diğer SaaS uygulamalarına ve şirket içi uygulamalar dahil olun
* Bulut ölçeği güvenlik Azure AD'ye yetkisiz erişimi önlemek için Office 365 telemetri burada yararlanır
* Intune tümleştirmesi, Kurumsal trafiği emin olmak için kimlik doğrulaması
* Kullanıcı hesabı yönetimini merkezileştirme
* En son güvenlik düzeltme eklerinin emin olmak için otomatik güncelleştirmeleri etkinleştirmiş
* Yeni özellikler yayınlandıkça; en son SAML çoklu oturum açma desteği olan ve daha ayrıntılı yönetim uygulama tanımlama

## <a name="next-steps"></a>Sonraki adımlar

* İşletim ve Azure AD uygulama proxy'si yönetme planlama hakkında daha fazla bilgi için bkz. [bir Azure AD uygulama ara sunucusu dağıtımını planlama](application-proxy-deployment-plan.md).
* Canlı bir Tanıtıma zamanlamak veya ücretsiz 90 günlük deneme için değerlendirme almak için bkz: [Enterprise Mobility + Security ile Başlarken](https://www.microsoft.com/cloud-platform/enterprise-mobility-security-trial).
