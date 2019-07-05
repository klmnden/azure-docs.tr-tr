---
title: Azure Active Directory ile bir güçlü Identity foundation için dört adımı
description: Bu konuda, karma kimlik müşteriler güçlü Identity foundation'ı oluşturmak için izleyebileceğiniz dört adımlar açıklanmaktadır.
services: active-directory
author: martincoetzer
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/20/2019
ms.subservice: hybrid
ms.author: martincoetzer
ms.collection: M365-identity-device-management
ms.openlocfilehash: 96b5e8ab63c1784ff073c7ba38cd4a6319db43c5
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67452741"
---
# <a name="four-steps-to-a-strong-identity-foundation-with-azure-active-directory"></a>Azure Active Directory ile bir güçlü Identity foundation için dört adımı

Uygulama ve veri erişimi yönetmek artık Çevre ağları ve güvenlik duvarları gibi geleneksel ağ güvenlik sınırı stratejileri üzerinde uygulamaların buluta hızlı taşıma nedeniyle güvenebilirsiniz. Artık kuruluşların kimin ve ne kuruluşun uygulama ve veri erişimi olan kullanıcıların kimlik çözümü güvenmesi gerekir. Daha fazla olan kuruluşlar, çalışanların iş ve Internet'e bağlandıklarında her yerden cihazlarını kullanmak için kendi cihazlarını getirmesine izin vermiş olursunuz. Uyumlu ve güvenli aygıtları sağlayarak uygulamak için bir kuruluş seçer kimlik çözümdeki önemli bir konu haline gelmiştir. Günümüzün dijital çalışma [kimliğidir birincil denetim düzlemi](https://www.microsoft.com/security/technology/identity-access-management?rtc=1) buluta taşıyarak kuruluşunuzun.

Kuruluşlar, bir Azure Active Directory (Azure AD) karma kimlik çözümü benimsenmesi Otomasyon, temsilci seçme, Self Servis ve çoklu oturum açma özellikleri aracılığıyla üretkenliği kilidini premium özelliklerine erişiminiz olur. Her yerde doğru kişilere güvenli verimlilik kurmak için doğru kaynakları doğru erişimi olmasını sağlayarak bu erişimi yönetmek üzere BT ekibinize sağlarken işlerini yapmak ihtiyaç duydukları gelen şirket kaynaklarına erişmek çalışanlarınıza sağlar.

Dayalı olarak bizim dersleri üzerinde bu en iyi uygulamalar listesi oluşturmak için önerilen eylemleri hızlı bir şekilde dağıtmanıza yardımcı olacak bir *güçlü* Identity foundation, kuruluşunuzdaki:

* Uygulamalar için bir kolayca bağlayın
* Her kullanıcı için tek bir kimlik otomatik olarak oluştur
* Kullanıcılarınızı güvenli bir şekilde desteklemenize
* Öngörüleriniz kullanıma hazır hale getirme

## <a name="step-1---connect-to-apps-easily"></a>1\. adım - uygulamalar için bir kolayca bağlayın

Azure AD ile uygulamalarınızı bağlayarak, çoklu oturum açma (SSO) sağlayarak son kullanıcı üretkenliğini ve güvenliğini geliştirmek ve kullanıcı sağlamayı gerçekleştirebilirsiniz. Uygulamalarınızı Azure AD, tek bir yerden yöneterek yönetim yükünü en aza indirmek ve tek bir denetim noktası için güvenlik ve uyumluluk ilkeleri elde edin.

Bu bölüm, uygulamalara güvenli uzaktan erişim iç uygulamaları ve uygulamalarınızı Azure AD'ye geçirmek avantajları etkinleştirme, kullanıcı erişimini yönetmek için seçeneklerinizi kapsar.

### <a name="make-apps-available-to-your-users-seamlessly"></a>Uygulamaları kullanıcılarınıza sorunsuz bir şekilde kullanılabilmesini

Azure AD için yöneticilerin [uygulamalarınıza eklemek](https://docs.microsoft.com/azure/active-directory/manage-apps/add-application-portal) için kurumsal uygulamalar galeride [Azure portalında](https://portal.azure.com/). Kurumsal uygulamaları galeri uygulamalarına ekleme, uygulamaları, kimlik sağlayıcısı olarak Azure AD'yi kullanacak şekilde yapılandırmak kolaylaştırır. Koşullu erişim ilkeleriyle uygulamasına kullanıcı erişimini yönetmek ve çoklu oturum açma (SSO) kullanıcıların parolalarını tekrar tekrar girmeniz gerekmez ve hem şirket içinde otomatik olarak imzalanmış uygulamaları yapılandırmanıza da sağlar ve bulut tabanlı uygulamalar.

Uygulamalar için Azure AD galeri eklendikten sonra kullanıcılar, kendilerine atanan ve arama ve diğer uygulamalar, gerektiğinde istek uygulamaları görebilirsiniz. Azure AD sağlar [çeşitli yöntemler](https://docs.microsoft.com/azure/active-directory/manage-apps/end-user-experiences) kullanıcılarının kendi uygulamalarına erişmelerini sağlamak için:

* Erişim paneli / uygulamalarım
* Office 365 uygulama Başlatıcısı
* Birleştirilmiş uygulamalarda doğrudan oturum açma
* Oturum açma doğrudan bağlantılar

Uygulamalara kullanıcı erişimi hakkında daha fazla bilgi için bkz: **adım 3 – uygulamanızın kullanıcıları açısından güç katan** bu makaledeki.

### <a name="migrate-apps-from-active-directory-federation-services-to-azure-ad"></a>Azure AD ile Active Directory Federasyon Hizmetleri'nden uygulama geçirme

Geçirme çoklu oturum açma yapılandırması Active Directory Federasyon Hizmetleri (ADFS) gelen Azure AD'ye güvenlik, daha tutarlı yönetilebilirlik ve işbirliği ek özellikler sağlar. En iyi sonuçlar için Azure AD'ye AD FS'den uygulamalarınızı geçiş yapmanızı öneririz. Uygulama kimlik doğrulaması ve yetkilendirme, Azure AD'ye getirme ile aşağıdaki avantajları sağlar:

* Maliyet Yönetimi
* Risk yönetme
* Verimliliği artırma
* Uyumluluk ve idare

Daha fazla bilgi için bkz. [bilgisayarınızı uygulamaları Azure Active Directory'ye geçirme](https://aka.ms/migrateapps/whitepaper) teknik incelemesi.

### <a name="enable-secure-remote-access-to-apps"></a>Uygulamalara güvenli uzaktan erişim'i etkinleştir

[Azure AD uygulama proxy'si](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-application-proxy) kuruluşların şirket içi uygulamaları buluta güvenli bir şekilde iç uygulamalara erişmek isteyen uzak kullanıcılar için yayımlamak basit bir çözüm sağlar. Bir çoklu oturum açma sonra Azure ad, kullanıcılar hem buluttaki hem de şirket içi uygulamaları dış URL'ler veya bir iç uygulama portal erişebilir.

Azure AD uygulama ara sunucusu, aşağıdaki avantajları sunar:

* Şirket içi kaynaklarınızı Azure AD'ye genişletme
  * Bulut ölçeğinde güvenlik ve koruma
  * Koşullu erişim ve etkinleştirmek kolay bir multi-Factor Authentication gibi özellikleri
* VPN ve geleneksel ters proxy çözümleri gibi çevre ağdaki bir bileşen yok
* Gerekli olan herhangi bir gelen bağlantı
* Çoklu oturum açma (SSO) cihazlar, kaynakları ve bulutta ve şirket içi uygulamalar
* Dilediğiniz zaman ve her yerde üretken olmasını son kullanıcılara da güç kazandırır

### <a name="discover-shadow-it-with-microsoft-cloud-app-security"></a>Microsoft Cloud App Security ile gölge BT uygulamalarını bulmayı

Modern kuruluşlarda, BT departmanlarının işlerini yapmak için kullanıcı tarafından kullanılan tüm bulut uygulamaları farkında değildir. BT yöneticileri, kaç bulut istenirse çalışanlarına düşündüğünü uygulamaların kullanması, ortalama, 30 veya 40'varsayalım. Gerçekte, 1000'den fazla ayrı uygulamaları kuruluşunuzdaki çalışanlar tarafından kullanılan bir ortalamadır. hiç kimse geçirmemiştir ve güvenlik ve uyumluluk ilkelerinizle uyumlu olmayabilir olmayan tasdikli uygulamalar, çalışanların % 80 kullanın.

[Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) (MCAS) sahip kullanıcılar popüler olan kullanışlı uygulamalar tanımlamanıza yardımcı olabilir, BT yaptırım uygulayın ve kurumsal uygulamaları Galerisine ekleyebilirsiniz, böylece kullanıcılar SSO ve koşullu erişim gibi özelliklerinden yararlanın.

*"** Cloud App Security** kişilerimiz düzgün bir şekilde Bulut ve SaaS uygulamalarına, hizmetlerimizi Accenture korunmasına yardımcı olma temel güvenlik ilkelerini destekleyecek şekilde kullandığınızdan emin olun yardımcı oluyor." *--- [John Blasi, Müdür, bilgi güvenliği Accenture yönetme](https://customers.microsoft.com/story/accenture-professional-services-cloud-app-security)

Gölge BT algılama yanı sıra MCAS ayrıca risk düzeyi belirleyebilirsiniz, uygulamaların Kurumsal verilere, olası veri sızıntılarına ve uygulamaların bulundurduğu diğer güvenlik risklerini yetkisiz erişimi engellemeye.

## <a name="step-2---establish-one-identity-for-every-user-automatically"></a>2\. adım: her kullanıcı için tek bir kimlik otomatik olarak oluştur

Bir Azure AD karma kimlik çözümü şirket içi ve bulut tabanlı dizinleri araya mevcut şirket içi Active Directory yatırımınızı var olan kimliklerinizi bulutta sağlayarak yeniden kullanmanıza izin verir. Çözümü şirket içi kimlikleri şirket içi Active Directory'ı kimlikleri doğru birincil kaynağı olarak var olan tüm idare çözümleri ile çalışan BT tutar sırasında Azure AD ile eşitler. Microsoft'un Azure AD karma kimlik çözümü, şirket içi ve bulut tabanlı özellikleri, kimlik doğrulaması ve yetkilendirme konumlarından tüm kaynaklara genel bir kullanıcı kimliği oluşturma yayılır.

Şirket içi dizinlerinizi Azure AD ile tümleştirme, kullanıcılarınızın daha üretken yapar ve kullanıcılar birden çok hesap uygulamalar ile hizmetler arasındaki her iki bulut erişmek için bir ortak kimlik sağlayarak kullanmasını önler ve şirket içi kaynaklara. Birden çok hesabı kullanmaktır sorunlu noktası son kullanıcılar ve BT benzer. Son kullanıcı açısından bakıldığında, ilgili bir sorun birden fazla parolayı hatırlamak zorunda kalmadan birden çok hesapları anlamına gelir. Bunu önlemek için çok sayıda kullanıcı güvenlik açısından bozuk her hesap için aynı parolayı yeniden kullanın. Bir BT açısından bakıldığında, yeniden genellikle daha fazla parola sıfırlama ve son kullanıcı şikayetlerinin birlikte Yardım Masası maliyetleri doğurur.

Azure AD Connect için şirket içi kimliklerinizi bulut uygulamalarına erişimini kullanılabilir Azure ad eşitleme için kullanılan bir araçtır. Azure AD'de Kimlikleridir sonra Salesforce veya Concur gibi SaaS uygulamaları için sağlayabilirsiniz.

Bu bölümde, biz, şirket içi kullanım alanını azaltmaya ve yüksek kullanılabilirlik, bulut için modern kimlik doğrulaması sağlamak için önerileri listeler.

> [!NOTE]
> Azure AD Connect hakkında daha fazla bilgi edinmek istiyorsanız bkz [Azure AD Connect Sync nedir?](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-whatis)

### <a name="set-up-a-staging-server-for-azure-ad-connect-and-keep-it-up-to-date"></a>Azure AD Connect için bir hazırlık sunucusu ayarlamanız ve güncel tutun

Azure AD Connect sağlama işleminde önemli bir rol oynar. Eşitleme sunucusu herhangi bir nedenle çevrimdışı olması durumunda, değişiklikler şirket içi bulutta güncelleştirilmez ve kullanıcılara erişim sorunlarına neden. Eşitleme sunucusu çevrimdışı geçtikten sonra hızlıca eşitlenmesini sürdürmek yöneticilerin sağlayan bir yük devretme stratejisi tanımlamak önemlidir.

Birincil Azure olay yüksek kullanılabilirlik sağlamak için AD Connect sunucusu çevrimdışı ayrı dağıtmanız önerilir gider [sunucusu hazırlama](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-staging-server) Azure AD Connect için. Bir sunucu dağıtımı "basit yapılandırma anahtarı hazırlık sunucusu üretime yükseltmek" silebilirler. Bir yedek sunucu hazırlama modunda yapılandırılmış olması, test ve yeni yapılandırma değişiklikleri dağıtıp eskisinin yetkisini alma, yeni bir sunucu tanıtmak de sağlar.

> [!TIP]
> Azure AD Connect, düzenli olarak güncelleştirilir. Bu nedenle, hazırlık sunucusu performans geliştirmeleri, hata düzeltmeleri ve her yeni sürümü sağlayan yeni özellikler yararlanabilmek güncel tutmanız önerilir.

### <a name="enable-cloud-authentication"></a>Bulut kimlik doğrulamasını etkinleştirme

Kuruluşlar şirket içi Active Directory ile Azure AD Connect kullanarak Azure AD'de kendi dizininde genişletin ve uygun kimlik doğrulama yöntemi yapılandırın. [Doğru kimlik doğrulama yöntemi seçme](https://docs.microsoft.com/azure/security/azure-ad-choose-authn) kuruluşunuz için uygulamaları buluta taşıma yolculuğunuza ilk adımı. Tüm bulut veri ve kaynaklara erişim denetimleri beri kritik bir bileşenidir.

Azure ad'deki bulut kimlik doğrulaması için şirket içi dizin nesnelerini etkinleştirmek için basit ve önerilen yöntem etkinleştirmektir [parola karması eşitleme](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-password-hash-synchronization) (PHS). Alternatif olarak, bazı kuruluşlar etkinleştirme düşünebilirsiniz [geçişli kimlik doğrulaması](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-pta-quick-start) (PTA).

PHS veya PTA seçseniz etkinleştirmeyi unutmayın [sorunsuz çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sso) kullanıcıların bulut uygulamalarını sürekli olarak kullanıcı adı ve parola uygulamada Windows 7 ve 8 cihazlar şirket kullanırken girmeden erişmesine izin vermek için Ağ. Çoklu oturum açma, kullanıcılar uygulamaya özgü parolalar ve oturum her uygulamaya unutmamanız gerekir. Benzer şekilde, Office 365 kutusu ve Salesforce gibi her bir uygulama için kullanıcı hesaplarını oluşturmak BT personelinin gerekir. Kullanıcıların parolalarını unutmayın yanı sıra her bir uygulamasına oturum açmak için kullanacağınız gerekir. Standartlaştırılmış tek oturum açma için bir mekanizma tüm kurumsal sağlayarak, en iyi kullanıcı deneyimi, azaltma riski, rapor olanağı ve İdaresi için önemlidir.

Zaten AD FS veya başka bir şirket içi kimlik doğrulama sağlayıcısı kullanan kuruluşlar için Azure AD'yi kimlik sağlayıcınız olarak taşımak karmaşıklığını azaltmak ve kullanılabilirliği geliştirmek. Federasyon kullanmak için belirli kullanım durumları yoksa, Federasyon kimlik doğrulamasını ya da PHS ve sorunsuz çoklu oturum açma veya PTA ve sorunsuz çoklu oturum açma şirket azaltılmış Ayak izi ve bulut ile sunduğu esneklik avantajlarından faydalanmak için geçiş öneririz Geliştirilmiş kullanıcı deneyimi. Daha fazla bilgi için [parola karması eşitleme için Azure Active Directory Federasyon'ten geçiş](https://docs.microsoft.com/azure/active-directory/hybrid/plan-migrate-adfs-password-hash-sync).

### <a name="enable-automatic-deprovisioning-of-accounts"></a>Otomatik hesap sağlama kaldırmayı etkinleştir

Otomatik sağlama ve sağlamayı kaldırma uygulamalarınıza etkinleştirme, birden çok sistem üzerinde yaşam döngüsünü kimlikleri yöneten için en iyi stratejidir. Azure AD destekler [otomatik, ilke tabanlı sağlama ve sağlamayı kaldırma](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-automatic-user-provisioning-portal) uygulayan popüler SaaS uygulamalarını ServiceNow ve Salesforce ve diğerleri gibi çeşitli kullanıcı hesaplarının [SCIM 2.0 Protokol](https://docs.microsoft.com/azure/active-directory/manage-apps/use-scim-to-provision-users-and-groups). El ile karşıya CSV dosyaları veya özel kod gerektiren geleneksel sağlama çözümleri aksine sağlama hizmeti bulutta barındırılır ve özellikleri ayarlanabilir ve Azure portalını kullanarak yönetilen bağlayıcılar önceden tümleştirilmiş. Otomatik yetkiyi kaldırma işleminin önemli bir avantajı, bunların kuruluştan ayrılan anında kullanıcıların kimliklerini anahtar SaaS uygulamalarından kaldırarak kuruluşunuzun güvenli yardımcı emin olur.

Otomatik kullanıcı hesabı sağlama ve nasıl çalıştığı hakkında daha fazla bilgi için bkz: [sağlama kaldırmayı Azure Active Directory ile SaaS uygulamalarına kullanıcı sağlamayı otomatikleştirin ve](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

## <a name="step-3---empower-your-users-securely"></a>3\. adım - güvenli olmalarını sağlayın

Günümüzün dijital çalışma alanında, verimlilik ile Bakiye güvenliği için önemlidir. Bununla birlikte, son kullanıcıların genellikle geri, üretkenliği ve bulut uygulamalarına erişim yavaş güvenlik önlemleri gönderin. Bu adres yardımcı olmak için Azure AD yönetim yükünü en aza indirerek üretken vermesine olanak sağlayan bir Self Servis özellikleri sağlar.

Bu bölümde, uyuşmazlıkları dikkatli kalırken kullanıcıların sağlayarak kuruluşunuzun kaldırmak için öneriler listelenir.

### <a name="enable-self-service-password-reset-for-all-users"></a>Tüm kullanıcılar için Self Servis parola sıfırlama etkinleştirme

Azure'nın [Self Servis parola sıfırlama](https://docs.microsoft.com/azure/active-directory/authentication/quickstart-sspr) (SSPR) sunar sıfırlama ve bunların parolalarını veya hesaplarını yönetici müdahalesi olmadan kilidini açmasına izin vermek, BT yöneticileri için basit bir gösterir. Sistem, kullanıcıların sisteme erişimini izleyen ayrıntılı raporlama içerir, ayrıca kötüye kullanım veya uygunsuz kullanım konusunda uyaran bildirimler sağlar.

Parola sıfırlama gerçekleştirdiğinde, varsayılan olarak, Azure AD hesaplarının kilidini açar. Ancak, Azure AD Connect'i etkinleştirdiğinizde [tümleştirme şirket içi](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-howitworks#on-premises-integration), parolayı sıfırlamak zorunda kalmadan hesaplarının kilidini açabileceğinizi bu iki işlemi birbirinden ayırmak için seçeneğiniz de vardır.

### <a name="ensure-all-users-are-registered-for-mfa-and-sspr"></a>Tüm kullanıcılar için mfa'yı ve SSPR kayıtlı emin olun

Azure MFA ve SSPR için kullanıcıların kayıtlı emin olmak için siz ve kuruluşunuz tarafından kullanılabilir raporlar sağlar. Kullanıcılar, kayıtlı olmayabilirsiniz işlemi eğitilmesi gerekebilir.

Mfa'yı [oturum açma işlemleri raporu](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-reporting) MFA kullanımı hakkında bilgi içerir ve kuruluşunuzdaki MFA nasıl çalıştığı ile Öngörüler sağlar. Erişimi oturum açma Etkinliği (ve denetimleri ve risk olaylarını) için Azure AD, sorun giderme, kullanım analizini ve adli araştırmalar için önemlidir.

Benzer şekilde, [Self Servis parola yönetimi raporu](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-reporting) kimin sahip olduğunu belirlemek için kullanılabilir (veya taşınmadığından) SSPR için kayıtlı.

### <a name="self-service-app-management"></a>Self Servis uygulama yönetimi

Kullanıcılarınıza kendi erişim panellerine uygulamalardan Self bulabilmesi için önce etkinleştirmeniz gerekir [Self Servis uygulama erişiminin](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-manage-self-service-access) Self bulmak ve istek izin vermek istiyorsanız uygulamalara erişim. Self Servis uygulama erişimi, kullanıcıların uygulamaları kendi kendine bulmasına ve isteğe bağlı olarak bu uygulamalara erişimi onaylamak iş grubuna izin izin vermek için harika bir yoludur. Bu kullanıcılar için atanan kimlik bilgilerini yönetmek iş grubuna izin verebilirsiniz [üzerinde parola çoklu oturum uygulamaları](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-password-sso-gallery#configure-the-application-for-password-single-sign-on) doğrudan gelen erişim panelleri üzerinden.

### <a name="self-service-group-management"></a>Self servis grup yönetimi

Büyük esneklik ve uygun ölçekte yönetme becerisini izin uygulamaları için kullanıcı atama en iyi gruplar kullanırken eşlenir:

* Öznitelik tabanlı dinamik grup üyeliği kullanma
* Uygulama sahipleri için temsilci seçme

Azure AD güvenlik grupları ve Office 365 grupları ile kaynaklara erişimi yönetme olanağı sağlar. Bu grupları onaylayabilir veya üyelik istekleri reddetme ve grup üyeliği denetimi temsilci bir grup sahibi tarafından yönetilebilir. Bilinen [Self Servis Grup Yönetimi](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-self-service-management), bu özellik, oluşturmak ve kendi isteklerini işlemek için Yöneticiler zorunda kalmadan grupları yönetmek için bir yönetici rolü atanmamış grup sahiplerine izin vererek zamandan tasarruf sağlar.

## <a name="step-4---operationalize-your-insights"></a>4\. adım - öngörülerinizi kullanıma hazır hale getirme

Denetim ve güvenlikle ilgili olaylar ve ilgili uyarıları günlük kullanıcıların üretken ve kuruluşunuzun güvenli olduğundan emin olmak için verimli bir stratejisi için temel bileşenleridir. Güvenlik günlüklerini ve raporları soruyu yanıtlama gibi yardımcı olabilir:

* İçin ödeme yaptığınız kullanıyorsunuz?
* Bir şey şüpheli veya kötü amaçlı kiracımdaki'olmuyor var mı?
* Kimin bir güvenlik olayı sırasında etkilendiğini?

Kuşkulu etkinlikleri ve ağ ve iç saldırıları denenmedi veya başarılı dış sızma gösterebilecek desenleri algılamak Yardım elektronik kaydıyla, güvenlik günlüklerini ve raporları sağlar. Denetim kullanabileceğiniz kullanıcı etkinliğini, belge yönetmeliklere uygunluk, adli analiz ve daha fazlasını yapın. Uyarılar, bildirimler güvenlik olayları sağlar.

### <a name="assign-least-privileged-admin-roles-for-operations"></a>İşlemleri için en az ayrıcalıklı yönetici rollerini atama

Yaklaşımınızı işlemlerine dikkat etmeniz olarak dikkate alınması gereken yönetimlerinin birkaç düzeyi vardır. İlk düzeyi üzerinde genel yöneticileri yönetim yükünü yerleştirir. Her zaman genel yönetici rolünü kullanarak, küçük şirketler için uygun olabilir. Ancak çünkü bu kişiler üzerinde ve dışında olan görevleri yönetme olanağı sağlar Yardım Masası personeli ve belirli görevlerden sorumlu yöneticiler daha büyük kuruluşlar için genel yönetici rolü atama bir güvenlik riski olabilir neler yapabileceğini olmaları.

Bu durumda, İleri düzey yönetim dikkate almanız gerekir. Azure AD kullanarak, son kullanıcılara "daha az ayrıcalıklı rollerdeki görevleri yönetebilir sınırlı yöneticileri" olarak belirleyebilirsiniz. Örneğin, Yardım Masası personeli atayabilirsiniz [güvenlik okuyucusu](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#security-reader) güvenlikle ilgili özellikler salt okunur erişimi olan yönetme olanağı sağlamak için rol. Veya belki de atamak için mantıklıdır [kimlik doğrulaması yönetici](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#authentication-administrator) kişiler parolası olmayan kimlik bilgilerini sıfırlama veya okuyamadı ve Azure hizmet durumu olanağı vermek için rol.

Daha fazla bilgi için bkz. [Azure Active Directory'de Yönetici rolü izinleri](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles).

### <a name="monitor-hybrid-components-azure-ad-connect-sync-ad-fs-using-azure-ad-connect-health"></a>İzleme karma bileşenleri (Azure AD Connect eşitleme, AD FS) Azure AD Connect Health'i kullanma

Azure AD Connect ve AD FS, büyük olasılıkla yaşam döngüsü yönetimi ve kimlik doğrulaması Kes ve kesintiler için sonuç olarak müşteri adayı kritik bileşenleridir. Bu nedenle, izleme ve bu bileşenlerden raporlama için Azure AD Connect Health dağıtmanız gerekir.

Daha fazla bilgi için okuma Git [İzleyicisi'ni AD FS kullanarak Azure AD Connect Health](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-adfs).

### <a name="use-azure-monitor-to-collect-data-logs-for-analytics"></a>Analiz için veri günlüklerini toplamak için Azure İzleyicisi'ni kullanın

[Azure İzleyici](https://docs.microsoft.com/azure/azure-monitor/overview) Gelişmiş analiz ve akıllı makine öğrenimi ayrıntılı Öngörüler sağlayan birleşik bir izleme portalı için tüm Azure AD günlükleri. Azure İzleyici ile ölçümlerini ve günlüklerini portalındaki ve durumunu ve performansını kaynaklarınızın daha fazla görünürlük elde etmek için API'ler aracılığıyla kullanabilir. Bu, çok çeşitli ürün tümleştirmeleri geleneksel üçüncü taraf SIEM sistemleri destekleyen API'ler ve veri dışarı aktarma seçeneği aracılığıyla etkinleştirme sırasında cam deneyimi portalındaki tek bir panel sağlar. Azure İzleyici ayrıca, kaynaklarınızı etkileyen sorunları hakkında otomatik eylemler gerçekleştirin veya bildirim almak için uyarı kuralları yapılandırma olanağı sağlar.

![Azure İzleyici](./media/four-steps/image1.png)

### <a name="create-custom-dashboards-for-your-leadership-and-your-day-to-day"></a>Liderlik ve gün, gün için özel panolar oluşturma

SIEM çözümünden sahip olmayan kuruluşlarda indirebileceği [Power BI içerik paketi](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-power-bi-content-pack) Azure AD için. Power BI içerik paketi yardımcı olmak için önceden oluşturulmuş raporları içerir, kullanıcılarınıza nasıl benimseyin ve dizininizdeki tüm etkinlikleri içgörüler sağlayan Azure AD özellikleri kullanmak anlayın. Ayrıca kendi oluşturabilirsiniz [özel Pano](https://docs.microsoft.com/power-bi/service-dashboards) ve günlük etkinliklerini bildirmek için liderlik ekibinizle paylaşın. Panolar, işinizi izleyin ve en önemli ölçümlerinizi bir bakışta görmek için harika bir yoludur. Panodaki görselleştirmeler, bir temel alınan veri kümesine ya da birden çok ve bir bağlantılı rapordan gelebilir. Şirket içi bir Pano birleştirir ve bulut burada konumundan bağımsız olarak birleştirilmiş bir görünüm verileri.

![Power BI özel Panosu](./media/four-steps/image2.png)

### <a name="understand-your-support-call-drivers"></a>Destek çağrısı sürücülerinizi anlama

Bu makalede ana hatlarıyla karma kimlik çözümü uygularken, sonuçta destek çağrılarınızı azaltma göreceksiniz. Sık karşılaşılan sorunları gibi Azure'un Self Servis parola sıfırlama, uygulama tarafından Unutulan parolalar ve hesap kilitlemeleri azaltıldığından Self Servis uygulama erişimi etkinleştirirken sağlayan kullanıcıların Self bulmasına ve bağlı olmadan uygulamalara erişim isteği BT personelinizin.

SSPR veya Self Servis uygulama erişiminin düzgün yapılandırılıp yapılandırılmadığını doğrulamak için destek çağrısı sürücülerinizi çözümlemek veya sistematik olarak olabilir herhangi bir yeni sorunlar olup olmadığını destek aramalarını azalma gözlemlerseniz yoksa öneririz ele alınan.

*"Dijital dönüşüm yolculuğumuz güvenilir bir kimlik ve erişim yönetimi sağlayıcısı bize, iş ortakları ve etkili bir ekosistem için bulut hizmeti sağlayıcıları arasında sorunsuz henüz güvenli tümleştirmeyi kolaylaştırmak için ihtiyacımız; Azure AD bize algılama ve yanıtlama riskleri olanak sağladı görünürlük ve gerekli özellikleri sunan en iyi seçenek sağladı."* --- [Yazan Almasri, genel bilgi güvenliği Yöneticisi, Aramex](https://customers.microsoft.com/story/aramex-azure-active-directory-travel-transportation-united-arab-emirates-en)

### <a name="monitor-your-usage-of-apps-to-drive-insights"></a>Sürücü ınsights uygulamaları kullanımınızı izleyin

Gölge BT üzerinden kullanarak kuruluşunuzda uygulama kullanımını izleme, keşfetme yanı sıra [Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) kuruluşunuz promise bulut uygulamalarının tüm avantajlarından yararlanmanıza tutmasına da yardımcı olabilir. Varlıklarınızın aracılığıyla Gelişmiş etkinlik görünürlüğü sağlayarak denetimi tutabilir ve bulut uygulamaları genelinde kritik veri korumasını artırmak yardımcı olabilir. MCAS kullanarak kuruluşunuzdaki uygulama kullanımını izleme aşağıdaki soruları yanıtlamanıza yardımcı olabilir:

* Hangi tasdiksiz uygulamaları, verileri depolamak için çalışanların kullanıyorsunuz?
* Nerede ve ne zaman hassas verileri bulutta depolandığını?
* Hassas verileri bulutta kimin eriştiği?

*"Cloud App Security ile hızla anomalileri saptayın ve harekete."* --- [Eric LePenske, üst düzey bilgi güvenliği Yöneticisi Accenture](https://customers.microsoft.com/story/accenture-professional-services-cloud-app-security)

## <a name="summary"></a>Özet

Karma kimlik çözümü uygulamak için birçok yönden vardır, ancak bu dört adım denetim listesini hızla daha üretken ve güvende olmasına olanak tanıyacak bir kimlik altyapısını gerçekleştirmenize yardımcı olur.

* Uygulamalar için bir kolayca bağlayın
* Her kullanıcı için tek bir kimlik otomatik olarak oluştur
* Kullanıcılarınızı güvenli bir şekilde desteklemenize
* Öngörüleriniz kullanıma hazır hale getirme

Bu belge, kuruluşunuz için bir tanımlayıcı Identity foundation oluşturma için kullanışlı bir yol haritası olduğunu umuyoruz.

## <a name="identity-checklist"></a>Kimlik denetim listesi

Kuruluşunuzda daha sağlam bir kimlik altyapısı yolculuğunuzda başladığınızda başvurusu için aşağıdaki denetim listesini yazdırma öneririz.

### <a name="today"></a>Bugün

|Açtınız mı?|Öğe|
|:-|:-|
||Pilot Self - Servis parola sıfırlama (SSPR) bir grup için|
||Azure AD Connect Health'i kullanarak hibrit bileşenlerini izleyin|
||İşlem için en az ayrıcalıklı yönetici rollerini atama|
||Microsoft Cloud App Security ile gölge BT uygulamalarını bulmayı|
||Analiz için veri günlüklerini toplamak için Azure İzleyicisi'ni kullanın|

### <a name="next-two-weeks"></a>Sonraki iki hafta

|Açtınız mı?|Öğe|
|:-|:-|
||Bir uygulamayı kullanıcılarınız için kullanılabilir hale getirme|
||Tercih ettiğiniz bir SaaS uygulaması için Azure AD sağlama pilot|
||Azure AD Connect için bir hazırlık sunucusu kurulumu ve güncel tutun|
||Uygulamaları Azure AD'ye ADFS geçirmeye başlayın|
||Liderlik ve gün, gün için özel panolar oluşturma|

### <a name="next-month"></a>Gelecek ay

|Açtınız mı?|Öğe|
|:-|:-|
||Sürücü ınsights uygulamaları kullanımınızı izleyin|
||Pilot güvenli uzaktan erişim uygulamaları|
||Tüm kullanıcılar için mfa'yı ve SSPR kayıtlı emin olun|
||Bulut kimlik doğrulamasını etkinleştirme|

### <a name="next-three-months"></a>Önümüzdeki üç ay

|Açtınız mı?|Öğe|
|:-|:-|
||Self Servis uygulama yönetimini etkinleştirme|
||Self Servis Grup Yönetimi etkinleştir|
||Sürücü ınsights uygulamaları kullanımınızı izleyin|
||Destek çağrısı sürücülerinizi anlama|

## <a name="next-steps"></a>Sonraki adımlar

Özellikleri kullanarak güvenli duruşunuzu nasıl artırabileceğinizi öğrenin, Azure Active Directory'nin bu beş adımı denetim - [kimlik altyapınızın güvenliğini sağlamak için beş adımı](https://aka.ms/securitysteps).

Kimlik özellikleri Azure AD'de yönetilen yönetim çözümleri ve kuruluşların hızlı benimseme ve kendi kimlik yönetimi daha geleneksel yönetimden taşıma sağlayan özellikleri sunarak buluta geçiş sürecinizi hızlandırmaya nasıl yardımcı olabileceğini öğrenin Şirket içi sistemler Azure AD'ye - [Azure AD sunan bulut yönetilen yönetimi için şirket içi iş yüklerini](https://aka.ms/cloudgoverned).
