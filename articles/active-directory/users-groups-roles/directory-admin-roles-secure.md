---
title: En iyi uygulamalar için güvenli yönetici erişimi - Azure Active Directory | Microsoft Docs
description: Kuruluşunuzun yönetici erişimi ve yönetici hesaplarını güvenli olduğundan emin olun. Sistem mimarları ve Azure AD'yi yapılandırma BT uzmanları için Azure ve Microsoft Online Services.
services: active-directory
keywords: ''
author: curtand
manager: mtillman
ms.author: curtand
ms.date: 03/18/2019
ms.topic: article
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.custom: it-pro
ms.reviewer: martincoetzer; MarkMorow
ms.collection: M365-identity-device-management
ms.openlocfilehash: 81d09978c3333a5b76c09f8c7dac85998d342f03
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60472903"
---
# <a name="securing-privileged-access-for-hybrid-and-cloud-deployments-in-azure-ad"></a>Azure AD'de karma ve bulut dağıtımları için ayrıcalıklı erişim güvenliğini sağlama

Modern bir kuruluştaki çoğu veya tüm iş varlıklarının güvenliği, BT sistemlerini yöneten ayrıcalıklı hesapların bütünlüğüne bağlıdır. Siber saldırganlar genellikle dahil olmak üzere kötü amaçlı aktörler yönetici hesapları ve diğer öğeleri hızlı bir şekilde hassas verileri ve sistemleri kimlik bilgisi hırsızlığı saldırılarını kullanarak erişim elde etme girişiminde ayrıcalıklı erişimin hedefleyin. Bulut Hizmetleri, engelleme ve yanıt bulut hizmeti sağlayıcısına ve müşteri birleşik sorumluluklarını olan. Uç noktaları ve bulut için en son tehditler hakkında daha fazla bilgi için bkz. [Microsoft Güvenlik zekası raporu](https://www.microsoft.com/security/operations/security-intelligence-report). Bu makalede, geçerli planlarınızı ve burada açıklanan yönergeleri arasındaki boşlukları kapatma doğru bir yol haritası geliştirmenize yardımcı olabilir.

> [!NOTE]
> Microsoft, en yüksek düzeyde güven, şeffaflık, standartlara uyumluluk ve yasal uyumluluk için taahhüt eder. Hakkında daha fazla nasıl Microsoft küresel olay yanıtı ekibi saldırılarına karşı bulut Hizmetleri etkilerini azaltır ve bir Microsoft iş ürünlerini ve bulut Hizmetleri, güvenlik nasıl oluşturulduğunu [Microsoft Trust Center - güvenlik](https://www.microsoft.com/trustcenter/security)ve adresindeki Microsoft Uyumluluk hedefleri [Microsoft Trust Center - Uyumluluk](https://www.microsoft.com/trustcenter/compliance).

<!--## Risk management, incident response, and recovery preparation

A cyber-attack, if successful, can shut down operations not just for a few hours, but in some cases for days or even weeks. The collateral damage, such as legal ramifications, information leaks, and media coverage, could potentially continue for years. To ensure effective company-wide risk containment, cybersecurity and IT pros must align their response and recovery processes. To reduce the risk of business disruption due to a cyber-attack, industry experts recommend you do the following:

* As part of your risk management operations, establish a crisis management team for your organization that is responsible for managing all types of business disruptions.

* Compare your current risk mitigations, incident response, and recovery plan with industry best practices for managing a business disruption before, during, and after a cyber-attack.

* Develop and implement a roadmap for closing the gaps between your current plans and the best practices described in this document.


## Securing privileged access for hybrid and cloud deployments

does the article really start here?-->
Çoğu kuruluş için iş varlıklarının güvenliği, BT sistemlerini yöneten ayrıcalıklı hesapların bütünlüğüne bağlıdır. Siber saldırganlar bir kuruluşun hassas verilere erişmek için altyapı sistemleri (örneğin, Active Directory ve Azure Active Directory) ayrıcalıklı erişim odaklanın. 

Giriş ve çıkış noktalarını birincil güvenlik çevresi olarak ağ güvenliğini sağlama üzerinde odaklanın geleneksel yaklaşım, Internet'teki SaaS uygulamaları ve kişisel cihazların kullanımını artış nedeniyle daha az etkilidir. Karmaşık bir modern kuruluşta ağ güvenliği çevresinin yerini doğal bir kuruluşun kimlik katmanındaki kimlik doğrulama ve yetkilendirme denetimleri ' dir.

Ayrıcalıklı yönetim hesaplarının etkili bir şekilde bu yeni "güvenlik çevresi." denetimi sizdedir Şirket içi, Bulut veya karma şirket içi ortamı olmasına bakılmaksızın ayrıcalıklı erişim için korumak ve bulut barındırılan hizmetleri için önemlidir. Belirlenen saldırganlara karşı yönetim erişiminin korunması, kuruluşunuzdaki sistemlerin risklerine karşı yalıtmak için bir eksiksiz ve dikkatli bir yaklaşıma gerçekleştirilecek gerektirir. 

Ayrıcalıklı erişim güvenliğini sağlama değişiklik yapılmasını gerektirir

* İşlemler, Yönetim uygulamaları ve Bilgi Bankası Yönetimi
* Konak savunmaları, hesap korumaları ve kimlik yönetimi gibi teknik bileşenleri

Bu belge, öncelikle üzerinde güvenli kimlik ve erişim için bir yol haritası yönetilen oluşturma veya olduğu bildirildi Azure AD, Microsoft Azure, Office 365 ve diğer bulut Hizmetleri odaklanır. Sahip kuruluşlarda şirket yönetim hesapları, bkz: şirket içi ve karma yönelik yönergeler ayrıcalıklı erişim Active Directory'den yönetilen [ayrıcalıklı erişim güvenliğini sağlama](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access). 

> [!NOTE] 
> Bu makaledeki Kılavuzu birincil olarak Azure Active Directory Premium P1 ve P2 planlarında dahil edilen özellikler Azure Active Directory ifade eder. Azure Active Directory Premium P2, EMS E5 paketine ve Microsoft 365 E5 paketine dahil edilir. Bu kılavuz, kullanıcılarınız için satın alınan Azure AD Premium P2 lisansı kuruluşunuzda zaten varsa varsayar. Bu lisansları yoksa bazı yönergeler kuruluşunuz için geçerli. Ayrıca, bu makale boyunca terimi genel yönetici (veya genel yönetici) "Şirket Yöneticisi" veya "Kiracı Yöneticisi" ile eşanlamlıdır

## <a name="develop-a-roadmap"></a>Bir yol haritası geliştirin 

Microsoft, geliştirin ve siber saldırganlara karşı ayrıcalıklı erişimin güvenliğini sağlamak için bir yol haritasının izlenmesini önerir. Her zaman, mevcut özelliklerinize ve kuruluşunuzdaki belirli gereksinimlere uyum sağlayacak şekilde haritanız ayarlayabilirsiniz. Yol haritasının her aşaması, maliyetini ve zorluğunu ayrıcalıklı erişim, şirket içi, Bulut ve karma varlıkları için saldırılara karşı saldırganlar için harekete geçirmelidir. Microsoft, aşağıdaki dört yol haritası aşamaları önerir: Bu yol haritası zamanlamaları en etkili ve en hızlı uygulamaları ilk olarak, Microsoft'un deneyimleri siber saldırı olay ve yanıt uygulaması ile temel önerilir. Bu yol haritasının zaman çizelgeleri yaklaşık değerlerdir.

![Yol haritasının zaman çizelgelerini ile aşamaları](./media/directory-admin-roles-secure/roadmap-timeline.png)

* Aşama 1 (24-48 saat): Öneririz kritik öğeleri hemen yapın

* (2-4 hafta) 2. Aşama: En sık kullanılan saldırı tekniklerini azaltma

* (1-3 ay) 3. Aşama: Görünürlük oluşturun ve yönetici etkinliğine ilişkin tam denetim oluşturma

* 4. Aşama (altı ay ve ötesi): Daha fazla güvenlik platformunuz sağlamlaştırmak için savunma oluşturmaya devam edin

Bu yol haritası çerçeve, zaten dağıttığınız Microsoft teknolojileri kullanımını en üst düzeye çıkarmak için tasarlanmıştır. Mevcut ve gelecek temel güvenlik teknolojilerden ve dağıtmış olmanız veya dağıtma dikkate alarak, diğer satıcıların güvenlik araçları tümleştirin. 

## <a name="stage-1-critical-items-that-we-recommend-you-do-right-away"></a>1. Aşama: Öneririz kritik öğeleri hemen yapın

![İlk yapmak için kritik öğeleri 1. Aşama](./media/directory-admin-roles-secure/stage-one.png)

Yol haritasının 1 aşaması, hızlı ve kolay uygulamak kritik görev üzerinde odaklanmıştır. Hemen ilk 24-48 temel düzeyde bir güvenli ayrıcalıklı erişim sağlamak için saat içinde bu birkaç öğe yapmanız önerilir. Yol haritasının ayrıcalıklı erişim güvenliğinin bu aşamada, aşağıdaki eylemleri içerir:

### <a name="general-preparation"></a>Genel Hazırlık

#### <a name="turn-on-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management üzerinde Aç

Zaten Azure AD Privileged Identity Management (PIM) üzerinde tuşları değil, üretim kiracınızın bunu. İçin Privileged Identity Management'ı etkinleştirdikten sonra bildirim alacaksınız. e-posta iletileri için ayrıcalıklı erişim rol değişiklikleri. Dizininizdeki yüksek ayrıcalıklı rollere ek kullanıcılar eklendiğinde bu bildirimleri erken uyarı sağlamak.

Azure AD Privileged Identity Management, Azure AD Premium P2 veya EMS E5 dahil edilir. Bu çözümleri buluta ve şirket içi ortam arasında uygulamalarına ve kaynaklarına erişimi korumaya yardımcı olur. Zaten Azure AD Premium P2 veya EMS E5 yoksa ve bu yol haritasında bulunan başvurulan özelliklerinin daha fazla değerlendirmek istiyorsanız, kaydolmaya [Enterprise Mobility + Security ücretsiz 90 günlük deneme](https://www.microsoft.com/cloud-platform/enterprise-mobility-security-trial). Azure AD Privileged Identity Management ve Azure AD kimlik koruması, Gelişmiş Güvenlik raporlama, Denetim ve uyarılar Azure AD kullanarak etkinliğini izlemek için bu lisans denemeler kullanın.

Sonra Azure AD Privileged Identity Management üzerinde geçiyor:

1. Oturum [Azure portalında](https://portal.azure.com/) üretim kiracınızın genel yönetici olan bir hesapla.

2. Kiracı, Privileged Identity Management'ı kullanmak istediğiniz seçmek için Azure portalının sağ üst köşesinde kullanıcı adınızı seçin.

3. Seçin **tüm hizmetleri** ve için listeyi filtreleyin **Azure AD Privileged Identity Management**.

4. Ayrıcalıklı Kimlik Yönetimi'nden açın **tüm hizmetleri** listelemek ve panonuza sabitleyin.

Kiracınızda Azure AD Privileged Identity Management'ı kullanan ilk kişinin otomatik olarak atanır **Güvenlik Yöneticisi** ve **ayrıcalıklı Rol Yöneticisi** Kiracı rolleri. Yalnızca ayrıcalıklı rol Yöneticileri kullanıcıların Azure AD dizini rol atamalarını yönetebilir. Ayrıca, Azure AD Privileged Identity Management ekledikten sonra ilk Keşif ve atama deneyiminizde size yol gösterir Güvenlik Sihirbazı'nı gösterilir. Şu anda ek değişiklik yapmadan Sihirbazı çıkabilirsiniz. 

#### <a name="identify-and-categorize-accounts-that-are-in-highly-privileged-roles"></a>Tanımlamak ve yüksek ayrıcalıklı rolleri hesaplarının kategorilere ayırma 

Üzerinde Azure AD Privileged Identity Management'ı etkinleştirdikten sonra dizin genel yöneticisi rolleri, ayrıcalıklı Rol Yöneticisi, Exchange Online yönetici ve SharePoint Online yönetici kullanıcıları görüntüleyin. Kiracınızda Azure AD PIM yoksa kullanabileceğiniz [PowerShell API'si](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0). Bu rol genel olarak genel yönetici rolü Başlat: Bu yönetici rolü atanan bir kullanıcı için kuruluşunuzun abone, olup, Microsoft 365'te bu role atanan bağımsız olarak tüm bulut hizmetlerinde aynı izinlere sahiptir. Yönetim Merkezi, Azure portal veya PowerShell için Microsoft Azure AD modülünü kullanarak. 

Artık gerekli olmayan tüm hesapları, bu rolleri kaldırın. Ardından, yönetici rollerine atandığı geri kalan hesapları kategorilere ayırın:

* Tek tek yönetici kullanıcılara atanan ve (örneğin, kişisel e-posta) yönetim dışı amaçlar için de kullanılabilir
* Tek tek yönetici kullanıcılara atanan ve yalnızca yönetim amacıyla belirtilen
* Birden çok kullanıcı arasında paylaşılan
* Kesme cam Acil Durum erişim senaryoları için
* Otomatik betikler için
* Dış kullanıcılar için

#### <a name="define-at-least-two-emergency-access-accounts"></a>En az iki Acil Durum erişim hesapları tanımlayın 

Burada bunlar yanlışlıkla oturum açın veya var olan tek bir kullanıcının hesabı yönetici olarak etkinleştirmek için Azure AD kiracınızın bağlantı kurma sorunu nedeniyle Yönetim dışı kilitlenemiyor bir durum halinde elde etmezsiniz emin olun. Kuruluş bir şirket içi kimlik sağlayıcısına birleştiriliyorsa, kullanıcılar şirket içinde oturum için örneğin, bu kimlik sağlayıcısı kullanılamayabilir. Yönetici erişimi yanlışlıkla eksikliği etkisini kiracınızda iki veya daha fazla Acil Durum erişim hesapları depolayarak azaltabilirsiniz.

Acil Durum erişim hesapları, kuruluşların mevcut Azure Active Directory ortamında ayrıcalıklı erişimi kısıtlamasına yardımcı olur. Bu hesaplar, son derece ayrıcalıklı olan ve belirli kişilere atanmaz. Acil Durum erişim hesapları 'Acil Durum' senaryoları normal yönetim hesapları kullanılamaz olduğu için Acil sınırlıdır. Kuruluşlar, denetlemek ve Acil Durum hesabın kullanımını azaltma, gerekli olduğu o zaman yalnızca bir yandan emin olmanız gerekir.

Atanan veya genel yönetici rolü için uygun olan hesaplarını değerlendirin. Yalnızca bulutta yer alan tüm listelenmiyor varsa kullanarak hesapları *. onmicrosoft.com etki alanı (hedeflenen) "Acil Durum" Acil Durum erişim için alıcı oluşturun. Daha fazla bilgi için [Azure AD'de Acil Durum erişimi yönetici hesaplarını yönetme](directory-emergency-access.md).

#### <a name="turn-on-multi-factor-authentication-and-register-all-other-highly-privileged-single-user-non-federated-admin-accounts"></a>Çok faktörlü kimlik doğrulamasını etkinleştirmek ve diğer tüm Federasyon olmayan üst düzeyde ayrıcalıklı tek kullanıcı yönetici hesapları kaydetme

Azure multi-Factor Authentication (MFA), bir veya daha fazla Azure AD yönetim rolleri kalıcı olarak atanan tüm bireysel kullanıcılar için oturum açma sırasında gerektirir: Genel yönetici, ayrıcalıklı Rol Yöneticisi, Exchange Online yönetici ve SharePoint Online yönetici. Etkinleştirmek için kılavuzu kullanın [yönetici hesaplarınız için multi-Factor Authentication (MFA)](../authentication/howto-mfa-userstates.md) ve bu kullanıcılar, kaydolduğundan emin olun [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup). Adım 2 ve 3. Adım Kılavuzu'nun altında daha fazla bilgi bulunabilir [veri ve Office 365 hizmetlerine erişimi koruma](https://support.office.com/article/Protect-access-to-data-and-services-in-Office-365-a6ef28a4-2447-4b43-aae2-f5af6d53c68e). 

## <a name="stage-2-mitigate-the-most-frequently-used-attack-techniques"></a>2. Aşama: En sık kullanılan saldırı tekniklerini azaltma

![2. Aşama sık kullanılan Mitigate saldırıları](./media/directory-admin-roles-secure/stage-two.png)

En sık Azaltıcı üzerinde yol haritası odaklar aşaması 2 kullanılan saldırı tekniklerini kimlik bilgisi hırsızlığı ve kötüye kullanım ve yaklaşık 2-4 hafta içinde uygulanabilir. Yol haritasının ayrıcalıklı erişim güvenliğinin bu aşamada, aşağıdaki eylemleri içerir.

### <a name="general-preparation"></a>Genel Hazırlık

#### <a name="conduct-an-inventory-of-services-owners-and-admins"></a>Hizmetleri, sahipleri ve yöneticileri envanterini gerçekleştir

Kendi-cihazını getir (KCG) ve evden çalışma ilkeleri ve kablosuz bağlantı işletmelerde büyüme artışı ile ağınıza bağlanan izlemek önemlidir. Geçerli güvenlik denetim genellikle cihazlar, uygulamalar ve programlar tarafından desteklenmeyen ağınızda çalışan ortaya çıkarır, BT ve bu nedenle erişiyorlarsa güvenli hale getirin. Daha fazla bilgi için [Azure güvenlik yönetimi ve izlemeye genel bakış](../../security/security-management-and-monitoring-overview.md). Aşağıdaki görevlerin tümünü Envanter işleminizde eklediğinizden emin olun. 

* Yönetici rolleri ve Hizmetleri burada yönetebilirsiniz olan kullanıcıları belirleyin.
* Azure AD PIM Aşama 1'de listelenen ötesinde ek roller dahil olmak üzere, Azure AD'ye yönetici erişimi kuruluşunuzdaki hangi kullanıcıların olduğunu bulmak için kullanın.
* Azure AD'de tanımlanan rollerin, Office 365 Yönetici rolleri, kuruluşunuzdaki kullanıcılara atayabileceğiniz bir dizi birlikte gelir. Her Yönetici rolü yaygın iş işleve eşlenir ve buna kişilerin belirli görevleri yapılacağı, kuruluş izinleri verir [Microsoft 365 Yönetim merkezini](https://admin.microsoft.com). Kuruluşunuzdaki hangi kullanıcıların Azure AD'de yönetilmeyen rolleri aracılığıyla dahil olmak üzere, Office 365 Yönetici erişimi olduğunu bulmak için Microsoft 365 Yönetim merkezini kullanın. Daha fazla bilgi için [hakkında Office 365 Yönetici rolleri](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) ve [Office 365 için en iyi güvenlik uygulamaları](https://support.office.com/article/Security-best-practices-for-Office-365-9295e396-e53d-49b9-ae9b-0b5828cdedc3).
* Kuruluşunuz, Azure, Intune veya Dynamics 365 gibi dayanan diğer Hizmetleri'nde sayım gerçekleştirin.
* Yönetici hesaplarınızdan (yönetim amacıyla, yalnızca kullanıcıların günlük hesaplarının kullanılan hesapları) e-posta adreslerini bağlı çalışma ve Azure MFA için kayıtlı veya şirket içi MFA kullanma olduğundan emin olun.
* Kendi iş gerekçesi yönetimsel erişim için kullanıcılara sor.
* Bu kişiler ve gerekli hizmetleri için yönetici erişimi kaldırın.

#### <a name="identify-microsoft-accounts-in-administrative-roles-that-need-to-be-switched-to-work-or-school-accounts"></a>İş veya Okul hesapları için geçiş için gereken yönetim rolleri, Microsoft hesapları tanımlayın

Bazı durumlarda, bir kuruluş için ilk genel Yöneticiler Azure AD kullanarak başladığında mevcut Microsoft hesabı kimlik bilgileri yeniden kullanın. Bu Microsoft hesaplarını tek tek bulut tabanlı ya da eşitlenmiş hesaplar tarafından değiştirilmelidir. 

#### <a name="ensure-separate-user-accounts-and-mail-forwarding-for-global-administrator-accounts"></a>Ayrı kullanıcı hesaplarını ve posta iletme genel yönetici hesapları için emin olun. 

Kişisel e-posta hesaplarına phished siber saldırganlar tarafından düzenli olarak olduğu gibi kişisel e-posta adresleri, genel yönetici hesapları olmamalıdır. İnternet risklerini ayırmaya yardımcı olmak için (kimlik avı saldırıları, yanlışlıkla Web'e göz atma) yönetim ayrıcalıklarından yönetici ayrıcalıkları olan her kullanıcı için adanmış hesapları oluşturun. 

Zaten yapmadıysanız, bunlar istemeden e-posta açın veya kendi yönetici hesaplarıyla ilişkili programları çalıştırmak emin olmak için genel yönetim görevlerini gerçekleştirmek kullanıcılar için ayrı hesaplar oluşturun. Çalışma ayarlanmamıştır iletilen e-postasını bu hesaplara sahip olduğunuzdan emin olun.  

#### <a name="ensure-the-passwords-of-administrative-accounts-have-recently-changed"></a>Yönetim hesapları, parolaları en son değiştirilen emin olun.

Tüm kullanıcıların parolalarını en az bir kez Son 90 gün içinde değiştirilen ve Yönetici hesaplarına imzalı emin olun. Ayrıca, tüm paylaşılan hangi birden çok kullanıcı hesaplarında parolayı biliyor en son değiştirilen parolalarını çalıştırılmış emin olun.

#### <a name="turn-on-password-hash-synchronization"></a>Parola Karması eşitleme üzerinde Aç

Parola Karması eşitleme, karma bir bulut tabanlı Azure şirket içi Active Directory örneğinden kullanıcı parola karmalarının eşitlemek için kullanılan bir özelliktir AD örneği. Hatta Active Directory Federasyon Hizmetleri (AD FS) veya diğer kimlik sağlayıcıları ile Federasyon kullanmaya karar verirseniz, isteğe bağlı olarak kullanabilirsiniz. parola karma eşitlemesini ayarladıktan yedek olarak durumda AD gibi şirket içi altyapınızı ayarlamak veya ADFS sunucularının, başarısız veya olur geçici olarak kullanılamıyor. Bu hizmete kendi şirket içi oturum açmak için kullandıkları aynı parolayı kullanarak oturum açmalarını sağlar AD örneği. Ayrıca, bu parola karmalarının tehlikeye için bir kullanıcı kendi aynı e-posta adresi ve parola Azure AD'ye bağlı diğer sistemlerde yararlanmıştır, bilinen parola ile karşılaştırarak tehlikeye atılmış kimlik bilgilerine algılamak kimlik koruması sağlar.  Daha fazla bilgi için [Azure AD Connect eşitlemesi ile parola karması eşitlemeyi uygulama](../hybrid/how-to-connect-password-hash-synchronization.md).

#### <a name="require-multi-factor-authentication-mfa-for-users-in-all-privileged-roles-as-well-as-exposed-users"></a>İfşa edilen kullanıcı yanı sıra tüm ayrıcalıklı rollerdeki kullanıcılar için çok faktörlü kimlik doğrulaması (MFA) gerekli

Azure AD yöneticileri ve hesabını aşılmış ise büyük bir etkiye sahip tüm diğer kullanıcılar da dahil olmak üzere tüm kullanıcılarınız için (örneğin, finansal görevlileri) çok faktörlü kimlik doğrulaması (MFA) gerekli önerir. Bu, gizliliği bozulan parola nedeniyle bir saldırı riskini azaltır.

Aç:

* [Koşullu erişim ilkelerini kullanarak MFA](../authentication/howto-mfa-getstarted.md) kuruluşunuzdaki tüm kullanıcılar için.

Windows Hello için iş kullanırsanız, MFA gereksinimi, Windows Hello oturum deneyimi kullanarak karşılanabilir. Daha fazla bilgi için [Windows Hello](https://docs.microsoft.com/windows/uwp/security/microsoft-passport). 

#### <a name="configure-identity-protection"></a>Kimlik Koruması'nı yapılandır 

Azure AD kimlik koruması bir algoritma tabanlı izleme ve raporlama, kuruluşunuzun kimliklerini etkileyen olası güvenlik açıklarını algılama için kullanabileceğiniz Aracı ' dir. Bu algılanan şüpheli etkinlikler için otomatik yanıtlar yapılandırabilir ve bunları gidermek için uygun eylemi gerçekleştirin. Daha fazla bilgi için [Azure Active Directory kimlik koruması](../active-directory-identityprotection.md).

#### <a name="obtain-your-office-365-secure-score-if-using-office-365"></a>Office 365 güvenli puanı (Office 365 kullanıyorsanız) edinmek

Puan cevabı (OneDrive, SharePoint ve Exchange gibi) kullanıyorsanız sonra ayarları ve etkinlikleri arar ve Microsoft tarafından oluşturulan bir taban çizgisi karşılaştırır hangi Office 365 hizmetlerine güvenli hale getirin. Nasıl hizalanmış, en iyi güvenlik yöntemleri bulunduğunuz dayalı bir puan elde edersiniz. Bir Office 365 iş ekstra veya Kurumsal abonelik en güvenli puanı erişmek için (genel yönetici veya bir özel Yönetici rolü) yönetici izinleri olan herkes [ https://securescore.office.com ](https://securescore.office.com/).

#### <a name="review-the-office-365-security-and-compliance-guidance-if-using-office-365"></a>(Office 365 kullanılıyorsa) Office 365 güvenlik ve uyumluluk Kılavuzu gözden geçirin

[Güvenlik ve uyumluluk planı](https://support.office.com/article/Plan-for-security-and-compliance-in-Office-365-dc4f704c-6fcc-4cab-9a02-95a824e4fb57) yaklaşımlar Office 365 müşterisi ve Office 365 yapılandırma diğer EMS özellikleri yararlanın. Ardından, gözden geçirme nasıl 3-6 adımları [veri ve Office 365 hizmetlerine erişimi koruma](https://support.office.com/article/Protect-access-to-data-and-services-in-Office-365-a6ef28a4-2447-4b43-aae2-f5af6d53c68e) ve nasıl yapılır Kılavuzu [güvenlik ve uyumluluk Office 365'te izleme](https://support.office.com/article/Monitor-security-and-compliance-in-Office-365-b62f1722-fd39-44eb-8361-da61d21509b6).

#### <a name="configure-office-365-activity-monitoring-if-using-office-365"></a>Office 365 Etkinlik izleme (Office 365 kullanıyorsanız) yapılandırma

Kullanan bir yönetici hesabınız ve kimin Office 365 bu portallarda oturum imzalama nedeniyle erişemeyebilir kullanıcıları belirleyin olanak tanıyarak, kuruluşunuzdaki kişilerin Office 365 hizmetleri nasıl kullandığını izleyebilirsiniz. Daha fazla bilgi için [etkinliği raporları Microsoft 365 Yönetim merkezinde](https://support.office.com/article/Activity-Reports-in-the-Office-365-admin-center-0d6dfb17-8582-4172-a9a9-aed798150263).

#### <a name="establish-incidentemergency-response-plan-owners"></a>Olay/Acil Durum yanıt planı sahipleri oluştur

Olay yanıtı etkili bir şekilde gerçekleştirmek karmaşık bir iştir. Bu nedenle, başarılı bir olay yanıtı özelliği oluşturma önemli planlama ve kaynak gerektirir. Bu, sürekli olarak için siber saldırılara karşı izleyin ve olayların işlenmesini öncelik yordamları oluşturmak önemlidir. Toplama etkin, çözümleme ve raporlama verilerini ilişkiler kurun ve diğer iç gruplar ile iletişim kurmak ve sahipleri planlamak için önemli yöntemlerdir. Daha fazla bilgi için [Microsoft Security Response Center](https://technet.microsoft.com/security/dn440717). 

#### <a name="secure-on-premises-privileged-administrative-accounts-if-not-already-done"></a>Yapmadıysanız güvenli şirket içi yönetim hesapları, ayrıcalıklı

Azure Active Directory kiracınızın şirket içi Active Directory ile eşitlenirse, ardından sunulan yönergeleri [ayrıcalıklı güvenlik erişimi yol haritası](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access): 1. aşaması. Bu şirket içi yönetim görevlerini gerçekleştirmek için Active Directory yöneticileri için ayrıcalıklı erişim iş istasyonları dağıtma ve iş istasyonları için benzersiz yerel yönetici parolaları oluşturma gereken kullanıcılar için ayrı yönetim hesapları oluşturma içerir ve sunucuları.

### <a name="additional-steps-for-organizations-managing-access-to-azure"></a>Azure'a erişimi yönetme kuruluşlar için ek adımlar

#### <a name="complete-an-inventory-of-subscriptions"></a>Abonelikleri envanterini tamamlayın

Üretim uygulamaları barındıran abonelik kuruluşunuzdaki tanımlamak için Enterprise portal ve Azure Portalı'nı kullanın.

#### <a name="remove-microsoft-accounts-from-admin-roles"></a>Microsoft hesapları yönetici rollerini kaldırın

Xbox Live ve Outlook gibi diğer programları Microsoft hesapları için Kurumsal abonelikler yönetici hesapları kullanılmamalıdır. Yönetim durumu tüm Microsoft hesaplarını kaldırın ve Azure Active Directory ile değiştirin (örneğin, chris@contoso.com) iş veya Okul hesapları.

#### <a name="monitor-azure-activity"></a>Azure Etkinlik izleme

Azure etkinlik günlüğü, azure'da abonelik düzeyindeki olayların geçmişini sağlar. Kimin oluşturulabilir, güncelleştirilen ve Silinen hangi kaynakları hakkında ve bu olaylar meydana geldiğinde bilgi sunar. Daha fazla bilgi için [denetim ve Azure aboneliğinizdeki önemli eylemleri hakkında bildirimlerin](../../azure-monitor/platform/quick-audit-notify-action-subscription.md).

### <a name="additional-steps-for-organizations-managing-access-to-other-cloud-apps-via-azure-ad"></a>Kuruluşlar Azure AD ile diğer bulut uygulamalarına erişimi yönetmek için ek adımlar

#### <a name="configure-conditional-access-policies"></a>Koşullu erişim ilkelerini yapılandırabilirsiniz.

Koşullu erişim ilkeleri, şirket içinde ve bulutta barındırılan uygulamalar için hazırlayın. Kullanıcılar çalışma alanına katılmış cihazlar varsa, daha fazla bilgi edinin [ayarlama şirket içi koşullu erişimi Azure Active Directory cihaz kaydı kullanarak](../active-directory-device-registration-on-premises-setup.md).


## <a name="stage-3-build-visibility-and-take-full-control-of-admin-activity"></a>3. Aşama: Görünürlük oluşturup yönetici etkinliğine ilişkin tam denetim

![3. Aşama yönetici etkinliğine ilişkin denetimi ele](./media/directory-admin-roles-secure/stage-three.png)

3. aşama aşama 2'den aşamadaki risk azaltmalarını temel oluşturur ve yaklaşık 1-3 ayda uygulanacak şekilde tasarlanmıştır. Bu yol haritasının ayrıcalıklı erişim güvenliğinin aşaması şu bileşenleri içerir.

### <a name="general-preparation"></a>Genel Hazırlık

#### <a name="complete-an-access-review-of-users-in-administrator-roles"></a>Yönetici rolleri kullanıcıların erişim gözden geçirmesi tamamlama

Daha fazla şirket kullanıcıları ayrıcalıklı artan bir yönetilmeyen platformu açabilir, bulut Hizmetleri aracılığıyla erişmesini. Bu, kullanıcılar Office 365, Azure aboneliği yöneticilerinin ve vm'lere veya SaaS uygulamaları aracılığıyla yönetici erişimi olan kullanıcılar için genel yönetici olma dahildir. Bunun yerine, kuruluşların ayrıcalıksız kullanıcılar olarak günlük işlemi işleyeceği ve yalnızca gerektiğinde yönetici hakları olması tüm çalışanlar, özellikle yöneticilere sahip olmalıdır. Yönetici rollerindeki kullanıcı sayısı bu yana ilk benimseme büyümüştür olduğundan, tanımlamak ve yönetici ayrıcalıkları etkinleştirmek uygun olan her kullanıcı onaylamak için tam erişim gözden geçirmeleri. 

Şunları yapın:

* Hangi kullanıcıların Azure AD yöneticileri, etkinleştirme isteğe bağlı, tam zamanında yönetimsel erişim ve rol tabanlı güvenlik denetimleri olduğunu belirleyin.
* Dönüştürme için farklı bir rol için ayrıcalıklı yönetici erişimi yok Temizle gerekçe sahip kullanıcılar (herhangi bir uygun rol varsa bunları kaldırın).

#### <a name="continue-rollout-of-stronger-authentication-for-all-users"></a>Tüm kullanıcılar için güçlü bir kimlik doğrulaması piyasaya devam edin 

C-suite Yöneticiler, üst düzey yöneticiler, önemli gerektiren BT ve personel güvenlik ve diğer üst düzeyde riskli kullanıcıların modern, güçlü kimlik doğrulaması, Azure mfa'yı veya Windows Hello gibi. 

#### <a name="use-dedicated-workstations-for-administration-for-azure-ad"></a>Özel iş istasyonları için Azure AD yönetim kullanın

Saldırganlar, bütünlük ve verilerine bir programın mantığı değiştirir veya yönetici kimlik bilgilerini girerek snoops kötü amaçlı bir kodun güvenilirliğini kesintiye uğratabilir şekilde bir kuruluşa ait veri ve sistemlere erişmek için ayrıcalıklı hesapları hedef girişiminde bulunabilir. Ayrıcalıklı erişim iş istasyonları (Paw'lar), Internet saldırıları ve tehdit vektörlerinden korunan hassas görevler için ayrılmış bir işletim sistemi sağlar. Bu hassas görevler ve hesapların günlük ayırarak kullanım iş istasyonları ve cihazları kimlik avı saldırıları, uygulama ve işletim sistemi güvenlik açıkları, çeşitli kimliğe bürünme saldırıları ve tuş vuruşu gibi kimlik bilgisi hırsızlığı saldırılarını karşı çok güçlü koruma sağlar günlüğe kaydetme, Pass--Hash ve Pass--Ticket. Ayrıcalıklı erişim iş istasyonları dağıtarak yöneticileri sıkı bir masaüstü ortamı dışındaki yönetici kimlik bilgilerini girin riskini azaltabilir. Daha fazla bilgi için [ayrıcalıklı erişim iş istasyonları](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations).

#### <a name="review-national-institute-of-standards-and-technology-recommendations-for-handling-incidents"></a>Olayların işlenmesi için Ulusal Standartlar ve teknoloji önerileri gözden geçirin 

Ulusal Standartlar ve teknoloji (NIST) özellikle olayı ile ilgili verileri çözümleme ve her olay için uygun yanıt belirlemek için olay işleme için yönergeler sağlanır. Daha fazla bilgi için [(NIST) bilgisayar Security Incident Handling Guide (SP 800-61, düzeltme 2)](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf).

#### <a name="implement-privileged-identity-management-pim-for-jit-to-additional-administrative-roles"></a>Privileged Identity Management (PIM) için ek yönetim rolleri için JIT uygulayın.

Azure Active Directory kullanan [Azure AD Privileged Identity Management](../privileged-identity-management/pim-configure.md) yeteneği. Ayrıcalıklı rol etkinleştirme süre sınırlı olanak sağlayarak çalışır:

* Belirli bir görevi gerçekleştirmek için yönetici ayrıcalıkları etkinleştir
* Etkinleştirme işlemi sırasında MFA zorlama
* Yöneticileri bant dışı değişiklikleri hakkında bilgilendirmek için uyarıları kullanın
* Önceden yapılandırılmış bir süre boyunca belirli ayrıcalıklara tutulacak kullanıcıları etkinleştir
* Güvenlik yöneticileri tüm ayrıcalıklı kimlikleri bulmak, Denetim raporları görüntülemek ve yönetici ayrıcalıkları etkinleştirmek uygun olan her bir kullanıcıyı tanımlamak için erişim gözden geçirmeleri oluşturmak izin ver

Azure AD Privileged Identity Management'ı zaten kullanıyorsanız, süre sınırlamalı ayrıcalıklar zaman dilimlerine (örneğin, bakım pencerelerini) gerektiği gibi ayarlayın.

#### <a name="determine-exposure-to-password-based-sign-in-protocols-if-using-exchange-online"></a>Parola tabanlı oturum açma protokolleri maruz kalma riskinizi (Exchange Online kullanıyorsanız) belirleyin.

Geçmişte, kullanıcı adı/parola birleşimleri, cihazlar, e-posta hesaplarına, telefonlar ve benzeri gömülü protokolleri varsayılır. Ancak artık bulutta siber saldırılardan risk ile kimlik bilgilerini güvenliği bozulursa kuruluşa yıkıcı ve e-posta aracılığıyla kullanıcı adı oturum açmak airdrop hariç her olası kullanıcıyı tanımlamak öneririz / güçlü kimlik doğrulama gereksinimleri ve koşullu erişim uygulayarak parolası. Engelleyebilirsiniz [koşullu erişim kullanarak eski bir kimlik doğrulama](https://docs.microsoft.com/en-us/azure/active-directory/conditional-access/block-legacy-authentication). Üzerinde Lütfen ayrıntıları kontrol etmek [temel kimlik doğrulaması engellemeyle](https://docs.microsoft.com/en-us/exchange/clients-and-mobile-in-exchange-online/disable-basic-authentication-in-exchange-online) çevrimiçi Exchnage aracılığıyla. 

#### <a name="complete-a-roles-review-assessment-for-office-365-roles-if-using-office-365"></a>Bir Office 365 rolleri için rolleri gözden geçirme değerlendirmesi (Office 365 kullanıyorsanız) tamamlayın

Tüm yöneticilerin kullanıcıların doğru rollerinde olup olmadığını değerlendirme (siler ve bu değerlendirme göre yeniden).

#### <a name="review-the-security-incident-management-approach-used-in-office-365-and-compare-with-your-own-organization"></a>Office 365'te kullanılan güvenlik olay Yönetimi yaklaşımı gözden geçirin ve kendi kuruluş ile Karşılaştır

Bu rapordan indirebileceğiniz [Microsoft Office 365'te güvenlik olay Yönetimi](https://www.microsoft.com/download/details.aspx?id=54302).

#### <a name="continue-to-secure-on-premises-privileged-administrative-accounts"></a>Şirket içinde ayrıcalıklı yönetici hesaplarını güvenli hale getirmek devam edin

Azure Active Directory, şirket içi Active Directory'ye bağlıysa, ardından sunulan yönergeleri [ayrıcalıklı güvenlik erişimi yol haritası](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access): 2. aşaması. Bu tüm yöneticiler için ayrıcalıklı erişim iş istasyonları dağıtma, MFA gerektirme, yalnızca yeteri kadar DC bakımı için kullanarak yönetici, etki alanları saldırı yüzeyini azaltmayı, ATA saldırı algılama için dağıtma içerir.

### <a name="additional-steps-for-organizations-managing-access-to-azure"></a>Azure'a erişimi yönetme kuruluşlar için ek adımlar

#### <a name="establish-integrated-monitoring"></a>Tümleşik izleme oluştur

[Azure Güvenlik Merkezi](../../security-center/security-center-intro.md) Azure aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar, aksi takdirde gözden kaçan gidebilir tehditleri ve güvenlik geniş ekosistemiyle çalışır algılamaya yardımcı olur çözümler.

#### <a name="inventory-your-privileged-accounts-within-hosted-virtual-machines"></a>Ayrıcalıklı hesaplarınızdaki barındırılan sanal makineleri içinde stok

Çoğu durumda, tüm Azure Abonelikleriniz veya kaynak için kullanıcılar Kısıtlanmamış izinler vermeniz gerekmez. Azure AD yönetim rolleri, kuruluşunuzda görevlerini ayırmak ve sadece erişim miktarını belirli işlerini yapabilmesi için gereken kullanıcılara vermek için kullanabilirsiniz. Örneğin, bir yöneticinin başka bir SQL veritabanını aynı abonelik içindeki yönetebilir, ancak yalnızca bir Abonelikteki sanal makineleri yönetme izin vermek için Azure AD yönetici rollerini kullanın. Daha fazla bilgi için [Azure portalında rol tabanlı erişim denetimi ile çalışmaya başlama](../../role-based-access-control/overview.md).

#### <a name="implement-pim-for-azure-ad-administrator-roles"></a>Azure AD yönetici rolleri için PIM uygulayın

Privileged Identity Management ile Azure AD yönetici rollerini yönetebilir, denetleyebilir ve Azure kaynaklarına erişimi izlemek için kullanın. PIM kullanarak ayrıcalıklı hesaplar tarafından ayrıcalıkların tehditlere maruz kalabileceği süreyi azaltarak ve raporlar ve uyarılar aracılığıyla kullanımlarının görünürlüğünüzü artırmak siber saldırılara karşı korur. Daha fazla bilgi için [Azure kaynaklarına Privileged Identity Management ile RBAC yönetme erişimi](../../role-based-access-control/pim-azure-resource.md).

#### <a name="use-azure-log-integrations-to-send-relevant-azure-logs-to-your-siem-systems"></a>İlgili göndermek için Azure günlük tümleştirme kullanan Azure günlüklerini SIEM Sistemlerinizle 

Azure günlük tümleştirmesi, kuruluşunuzun mevcut güvenlik bilgileri ve Olay yönetimi (SIEM) sistemlerine Azure kaynaklarınızdan gelen ham günlüklerine tümleştirmenize olanak tanır. [Azure günlük tümleştirmesi](../../security/security-azure-log-integration-overview.md) Windows olayları Windows Olay Görüntüleyicisi günlükleri ve Azure kaynaklarından Azure etkinlik günlükleri, Azure Güvenlik Merkezi uyarıları ve Azure tanılama günlükleri toplar. 


### <a name="additional-steps-for-organizations-managing-access-to-other-cloud-apps-via-azure-ad"></a>Kuruluşlar Azure AD ile diğer bulut uygulamalarına erişimi yönetmek için ek adımlar

#### <a name="implement-user-provisioning-for-connected-apps"></a>Bağlı uygulamalar için kullanıcı sağlamayı uygulayın

Azure AD, oluşturma, Bakım ve Dropbox ve Salesforce, ServiceNow gibi bulut (SaaS) uygulamaları kullanıcı kimliklerini kaldırılmasını otomatik hale getirmek ve benzeri olanak tanır. Daha fazla bilgi için [kullanıcı sağlamayı ve Azure AD ile SaaS uygulamalarına sağlama kaldırmayı otomatikleştirme](../manage-apps/user-provisioning.md).

#### <a name="integrate-information-protection"></a>Information protection tümleştirme

MCAS, ilkeleri, Azure Information Protection sınıflandırma etiketleri temelinde dosyaları araştırmanıza ve daha fazla görünürlük ve denetim bulutta verilerinizin etkinleştirme sağlar. Tarama ve bulutta dosyaları sınıflandırabilir ve Azure Information protection etiketleri uygulayın. Daha fazla bilgi için [Azure Information Protection tümleştirmesi](https://docs.microsoft.com/cloud-app-security/azip-integration).

#### <a name="configure-conditional-access"></a>Koşullu erişimi yapılandırma

Bir grubu, konum ve uygulama hassaslığı için dayalı koşullu erişimi yapılandırma [SaaS uygulamaları](https://azure.microsoft.com/overview/what-is-saas/) ve Azure AD bağlı uygulamalar. 

#### <a name="monitor-activity-in-connected-cloud-apps"></a>Bağlantılı bulut uygulamalarında etkinliğini izleme

Kullanıcıların erişimini de bağlı uygulamalarda korunduğundan emin olmak için yararlanarak öneririz [Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security). Bu bulut uygulamalarında, yönetici hesaplarınızdan olanak tanıyarak güvenli hale getirme yanı sıra Kurumsal erişim güvenliğini sağlar:

* Bulut uygulamaları için görünürlük ve denetimi genişletme
* Erişim, etkinlikleri ve veri paylaşımı için ilke oluşturma
* Riskli etkinlikler, anormal davranışları ve tehditlerden otomatik olarak tanımla
* Veri sızıntısını önlemeye
* Risk ve otomatik tehdit önleme ve ilke zorlama simge durumuna küçült

Cloud App Security SIEM Aracısı Cloud App Security, Office 365 uyarıların ve etkinliklerin merkezi izlemeyi etkinleştirmek için SIEM sunucunuz ile tümleştirilir. Uyarıları ve etkinlikleri, Cloud App Security'den çeker ve bunları SIEM sunucusuna akışları sunucunuzda çalışır. Daha fazla bilgi için [SIEM tümleştirmesi](https://docs.microsoft.com/cloud-app-security/siem).

## <a name="stage-4-continue-building-defenses-to-a-more-proactive-security-posture"></a>4. Aşama: Bir güvenlik duruşu için savunma oluşturmaya devam edin

![4. Aşama önleyici güvenlik duruşunu benimseyin](./media/directory-admin-roles-secure/stage-four.png)

Yol haritasının aşaması 4 aşama 3'ten görünürlüğünü kısıtladınız oluşturur ve altı ay içinde ve dışında uygulanacak şekilde tasarlanmıştır. Strong geliştirdiğiniz bir yol haritası yardımcı Tamamlanıyor Erişim Koruması şu anda bilinen ve günümüzde kullanılabilir olası saldırılara karşı ayrıcalıklı. Ne yazık ki, güvenlik tehditleri sürekli evrim geçiren ve güvenlik maliyetini artırmaya ve ortamınızı hedefleyen saldırganlar başarı oranını azaltarak odaklanan devamlı bir süreç olarak görüntülemek öneririz; bu nedenle.

Ayrıcalıklı güvenli hale getirme erişim modern bir kuruluştaki iş varlıkları için güvenlik Güvenceleri kurmak için bir ilk önemli adımdır ancak bu ilke, işlemler, bilgi gibi öğeler içeren tam bir güvenlik programının tek parçası değil Güvenlik, sunucular, uygulamalar, bilgisayarlar, cihazlar, bulut yapısı ve diğer bileşenleri sürekli güvenlik Güvenceleri sağlar. 

Ayrıcalıklı erişim hesaplarınızı yönetmeye ek olarak aşağıdaki düzenli olarak gözden geçirmenizi öneririz:

* Yöneticiler, işlerimizi ayrıcalıksız kullanıcılar olarak yapıyor olun.
* Yalnızca gerektiğinde ayrıcalıklı erişim vermek ve daha sonra (tam zamanında) kaldırın.
* Korumak ve ayrıcalıklı hesaplara ilişkin denetim etkinliği gözden geçirin.

Tüm güvenlik yol haritası oluşturma hakkında daha fazla bilgi için bkz: [Microsoft bulut BT mimarisi kaynaklarına](https://docs.microsoft.com/office365/enterprise/microsoft-cloud-it-architecture-resources). Bu konulardan herhangi birine ile yardımcı olmak üzere Microsoft hizmetlerini kullanma hakkında daha fazla bilgi için Microsoft temsilcinize başvurun veya bakın [derleme kuruluşunuzu korumak için kritik siber savunmaları](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).

Bu yol haritasının ayrıcalıklı erişim güvenliğinin son devam eden aşaması şu bileşenleri içerir.

### <a name="general-preparation"></a>Genel Hazırlık

#### <a name="review-admin-roles-in-azure-active-directory"></a>Azure Active Directory'de yönetici rolleri gözden geçirin 

Geçerli yerleşik Azure AD yönetim rolleri hala güncel olup olmadığını belirlemek ve yalnızca ilgili izinler için ihtiyaç duydukları temsilcileri ve rolleri kullanıcılara olduğundan emin olun. Azure AD ile farklı işlevler sunmak için ayrı yöneticileri atayabilirsiniz. Daha fazla bilgi için [Azure Active Directory'de yönetici rolleri atama](directory-assign-admin-roles.md).

#### <a name="review-users-who-have-administration-of-azure-ad-joined-devices"></a>Azure ad yönetim gözden geçirme kullanıcılar katılmış cihazlar

Daha fazla bilgi için [karma yapılandırmak için Azure Active Directory'ye katılmış cihazlarda nasıl](../device-management-hybrid-azuread-joined-devices-setup.md).

#### <a name="review-members-of-built-in-office-365-admin-roleshttpssupportofficecomarticleabout-office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d"></a>Gözden geçirin, üyelerinin [yerleşik Office 365 Yönetici rolleri](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)
Office 365 kullanıyorsanız.
‎
#### <a name="validate-incident-response-plan"></a>Olay yanıtlama planı doğrula

Planınızı geliştirmek için Microsoft, düzenli olarak planınızı beklendiği gibi çalıştığını doğrulamak önerir:

* Mevcut yol alınmamış olduğunu görmek için haritanızı gidin
* Postmortem analize dayalı olarak, varolan gözden geçirin veya yeni en iyi tanımlayın
* Kuruluşunuzda en iyi yöntemler ve güncelleştirilmiş olay yanıtlama planı dağıtılmasını sağlar


### <a name="additional-steps-for-organizations-managing-access-to-azure"></a>Azure'a erişimi yönetme kuruluşlar için ek adımlar 

Değiştirmeniz gerekip gerekmediğini belirlemek [bir Azure aboneliğinin sahipliğini başka bir hesaba](../../billing/billing-subscription-transfer.md).
‎

## <a name="break-glass-what-to-do-in-an-emergency"></a>"Acil Durum": acil durumlarda yapmanız gerekenler

![Acil Durum sonu Acil Durum erişim hesapları](./media/directory-admin-roles-secure/emergency.jpeg)

1. Anahtar yöneticileri ve profilinizle ilgili bilgileri ve olay ile güvenlik sorumluları bildirin.

2. Saldırı playbook gözden geçirin. 

3. Azure AD'de oturum açmak için "Acil Durum" hesabı kullanıcı adı/parola bileşimini erişin. 

4. Microsoft tarafından Yardım [Azure destek isteği açma](../../azure-supportability/how-to-create-azure-support-request.md).

5. Bakmak [Azure AD oturum açma raporlar](../reports-monitoring/overview-reports.md). Rapora dahil olduğunda ve bir olayın gerçekleşmesi arasında bir gecikme olabilir.

6. Federasyon, karma ortamlar ve AD FS sunucu kullanılabilir değilse, geçici olarak parola karması eşitleme için kullanılacak federe kimlik doğrulamasını geçmeniz gerekebilir. AD FS sunucu kullanılabilir olana kadar bu etki alanı Federasyon yönetilen kimlik doğrulaması için geri döner.

7. Ayrıcalıklı hesaplar için e-posta izleyin.

8. Olası legal ve adli araştırma için ilgili günlük yedeklerini kaydettiğinizden emin olun.

Microsoft Office 365 güvenlik olaylarına nasıl işlediği hakkında daha fazla bilgi için bkz. [Microsoft Office 365'te güvenlik olay Yönetimi](https://aka.ms/Office365SIM).

## <a name="faq-common-questions-we-receive-regarding-securing-privileged-access"></a>SSS: Aldığımız güvenliğini sağlama ayrıcalıklı erişim ile ilgili sık sorulan sorular  

**S:** Herhangi bir güvenli erişim bileşeni henüz uygulamadığınız varsa ne yapmalıyım?

**Yanıt:** En az iki sonu Acil Durum hesabı tanımlamak, MFA, ayrıcalıklı yönetim hesapları ve ayrı kullanıcı hesapları için genel yönetici hesaplarından atayın.

**S:** Bir ihlalden sonra önce ele alınması gereken ilk sorun nedir?

**Yanıt:** Üst düzeyde riskli kişiler için güçlü kimlik doğrulama gerektiren emin olun.

**S:** Bizim ayrıcalıklı yöneticilerin devre dışı bırakıldı ne olur?

**Yanıt:** Her zaman güncel tutulan bir genel yönetici hesabı oluşturun.

**S:** Varsa yalnızca genel yönetici sola ve ulaşılamıyor ne olacak? 

**Yanıt:** Kesme cam hesaplarınız hemen ayrıcalıklı erişim elde etmek için kullanın.

**S:** Yöneticilerin kuruluş içinde nasıl koruyabilirim?

**Yanıt:** Yöneticiler her zaman standart "ayrıcalıksız" kullanıcılar olarak gündelik işlerini yapmak sahiptir.

**S:** Azure AD içindeki yönetici hesapları oluşturmak için en iyi uygulamalar nelerdir?

**Yanıt:** Belirli yönetim görevleri için rezerve ayrıcalıklı erişim.

**S:** Kalıcı yönetici erişimi azaltmak için hangi araçları var?

**Yanıt:** Privileged Identity Management (PIM) ve Azure AD yönetim rolleri.

**S:** Yönetici hesaplarını Azure AD'ye eşitleme Microsoft konumu nedir?

**Yanıt:** Katman 0 Yönetici hesabı (hesapları, grupları ve AD ormanı, etki alanı veya etki alanı denetleyicileri doğrudan veya dolaylı yönetimsel denetime sahip diğer varlıkları ve tüm varlıklar dahil), yalnızca kullanılan AD hesaplar ve bu normalde değil şirket Azure AD bulut için eşitlenir.

**S:** Yönetici portalı'nda rastgele yönetici erişimi atamadan nasıl saklarız?

**Yanıt:** Tüm kullanıcılar ve çoğu yöneticileri için ayrıcalıklı olmayan hesapları kullanın. Tarafından hangi birkaç yönetici hesapları ayrıcalıklı belirlemek için kuruluşunuzun bir ayak izini geliştirmeye başlayın. Ve yönetici kullanıcılar için yeni oluşturulan izleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Microsoft Güvenlik ürün Trust Center](https://www.microsoft.com/trustcenter/security) – Microsoft güvenlik özelliklerinin bulut ürünleri ve Hizmetleri

* [Microsoft Trust Center - Uyumluluk](https://www.microsoft.com/trustcenter/compliance/complianceofferings) – Microsoft'un kapsamlı uyumluluk teklifleri cloud Services

* [Bir risk değerlendirmesi gerçekleştirmek hakkında yönergeler](https://www.microsoft.com/trustcenter/guidance/risk-assessment) -Microsoft bulut Hizmetleri için güvenlik ve uyumluluk gereksinimlerini yönetme

### <a name="other-microsoft-online-services"></a>Diğer Microsoft Çevrimiçi Hizmetler

* [Microsoft Intune güvenlik](https://www.microsoft.com/trustcenter/security/intune-security) – Intune mobil cihaz yönetimi, mobil uygulama yönetimi ve buluttan bilgisayar yönetimi olanakları sağlar.

* [Microsoft Dynamics 365 güvenlik](https://www.microsoft.com/trustcenter/security/dynamics365-security) – Dynamics 365, müşteri ilişkileri yönetimi (CRM) ve kurumsal kaynak planlama (ERP) özelliklerini birleştiren Microsoft bulut tabanlı çözümüdür.
