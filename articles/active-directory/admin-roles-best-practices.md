---
title: En iyi uygulamalar Azure AD'de yönetici erişimi güvenli hale getirmenin | Microsoft Docs
description: Kuruluşunuzun yönetimsel erişim ve yönetici hesaplarını güvenli olduğundan emin olun. Sistem mimarları ve Azure AD yapılandıran BT uzmanları için Azure ve Microsoft Çevrimiçi Hizmetler.
services: active-directory
keywords: ''
author: curtand
ms.author: curtand
ms.date: 03/09/2018
ms.topic: article
ms.service: active-directory
ms.workload: identity
ms.custom: it-pro
ms.reviewer: martincoetzer, MarkMorow
ms.openlocfilehash: 98665ab215c98ea60273ce3aae2757cf20817a90
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="securing-privileged-access-for-hybrid-and-cloud-deployments-in-azure-ad"></a>Ayrıcalıklı erişim karma ve bulut dağıtımları için Azure AD'de güvenliğini sağlama

Modern kuruluşunuzdaki çoğunu veya tümünü iş varlıklar güvenliği yönetmek ve BT sistemleri yönetmek ayrıcalıklı hesaplar bütünlüğünü bağlıdır. Siber saldırganların genellikle de dahil olmak üzere kötü amaçlı aktörler yönetici hesapları ve diğer öğeleri hızlı bir şekilde hassas verilere ve kimlik bilgisi hırsızlığı saldırılarını kullanmasını sistemleri erişim elde etme girişiminde ayrıcalıklı erişim hedefleyin. Bulut için birleşik sorumlulukları bulut hizmeti sağlayıcısı ve müşteri hizmetleri, önleme ve yanıt verilebilir. Uç noktaları ve bulut için en son tehditler hakkında daha fazla bilgi için bkz: [Microsoft Güvenlik Intelligence rapor](https://www.microsoft.com/security/sir/default.aspx). Bu makalede geçerli planlarınızı ve burada açıklanan yönergeleri arasındaki boşlukları kapatma doğru bir yol haritası geliştirmenize yardımcı olabilir.

> [!NOTE] 
> Microsoft en yüksek güven, saydamlık, standartlara uygunluğu ve düzenleyici uyumluluk düzeyini için taahhüt eder. Microsoft Genel olay yanıtlama ekibi nasıl bulut Hizmetleri saldırıları etkilerini azaltır ve güvenlik Microsoft iş ürünlerini ve cloud services nasıl yapılandırıldığını hakkında daha fazla bilgi [Microsoft Trust Center - güvenlik](https://www.microsoft.com/en-us/trustcenter/security)ve adresindeki Microsoft Uyumluluk hedefleri [Microsoft Trust Center - Uyumluluk](https://www.microsoft.com/en-us/trustcenter/compliance).

<!--## Risk management, incident response, and recovery preparation

A cyber-attack, if successful, can shut down operations not just for a few hours, but in some cases for days or even weeks. The collateral damage, such as legal ramifications, information leaks, and media coverage, could potentially continue for years. To ensure effective company-wide risk containment, cybersecurity and IT pros must align their response and recovery processes. To reduce the risk of business disruption due to a cyber-attack, industry experts recommend you do the following:

* As part of your risk management operations, establish a crisis management team for your organization that is responsible for managing all types of business disruptions.

* Compare your current risk mitigations, incident response, and recovery plan with industry best practices for managing a business disruption before, during, and after a cyber-attack.

* Develop and implement a roadmap for closing the gaps between your current plans and the best practices described in this document.


## Securing privileged access for hybrid and cloud deployments

does the article really start here?-->
Çoğu kuruluş için iş varlıklar güvenliği yönetmek ve BT sistemleri yönetmek ayrıcalıklı hesaplar bütünlüğünü bağlıdır. Bir kuruluşun hassas verilere erişmek için altyapı sistemleri (örneğin, Active Directory ve Azure Active Directory) ayrıcalıklı erişim siber saldırganların odaklanılmaktadır. 

Bir birincil güvenlik çevre ağ giriş ve çıkış noktaları güvenliğini sağlama konusunda odaklanmak geleneksel yaklaşım SaaS uygulamaları ve kişisel aygıtları kullanımını artışa Internet'te nedeniyle daha az etkili olur. Karmaşık bir modern kurumsal ağ güvenlik çevre doğal yerini kuruluşun kimlik katmanı kimlik doğrulama ve yetkilendirme denetimleri alır. 

Ayrıcalıklı yönetici etkili bir şekilde bu yeni "güvenlik çevre." denetiminde hesaplardır Ortamında şirket içi, Bulut veya karma şirket içi olmasına bakılmaksızın ayrıcalıklı erişim korumak ve bulut barındırılan hizmetleri için önemlidir. Yönetim erişimi belirlendiği rakiplerin karşı korumaya, kuruluşunuzun sistemleri risklerinden yalıtmak için tam ve Düşünceli yaklaşımı benimsemeye gerektirir. 

Ayrıcalıklı erişimi güvenli hale getirme yapılmasını istiyor.
* İşlemler, Yönetim uygulamaları ve Bilgi Bankası Yönetimi
* Ana bilgisayar savunma, hesap korumalar ve kimlik yönetimi gibi teknik bileşenleri

Bu belge, öncelikle üzerinde kimlikleri güvenli ve erişim için bir yol haritası yönetilen oluşturma veya olduğu bildirildi Azure AD, Microsoft Azure, Office 365 ve diğer bulut hizmetlerini odaklanır. Sahip kuruluşlar için yönetim hesapları, şirket içi ve karma için yönergeler ayrıcalıklı erişim Active Directory'den yönetilen bkz: şirket içi [ayrıcalıklı erişimi güvenli hale getirme](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access). 

> [!NOTE] 
> Bu makaledeki Kılavuzu birincil olarak Azure Active Directory Premium planlarda P1 ve P2 dahil edilen özellikleri Azure Active Directory ifade eder. Azure Active Directory Premium P2 EMS E5 paketi ve Microsoft 365 E5 paketine dahil edilmiştir. Bu kılavuz, kuruluşunuzun zaten kullanıcılarınız için satın alınan Azure AD Premium P2 lisansına sahipse varsayar. Bu lisanslar yoksa bazı kılavuzunun kuruluşunuza uygulanamayabilir. Ayrıca, bu makale terim genel yönetici (veya genel yönetici) "Şirket Yönetici" veya "Kiracı Yönetici" eşanlamlıdır

## <a name="develop-a-roadmap"></a>Bir yol haritası geliştirin 

Microsoft, geliştirmek ve siber saldırganların karşı ayrıcalıklı erişimin güvenliğini sağlamak için bir yol haritasını takip edin önerir. Her zaman, var olan özellikler ve kuruluşunuz içindeki belirli gereksinimleri karşılamak için yol haritası de ayarlayabilirsiniz. Yol Haritası her aşaması, maliyet ve ayrıcalıklı erişim için şirket içi, Bulut ve karma varlıkları saldırmak rakiplerin zorluk tetiklemelidir. Microsoft, aşağıdaki dört yol haritası aşamaları önerir: Bu yol haritası zamanlamaları en etkili ve hızlı uygulamaları ilk olarak, Microsoft'un deneyimleri siber saldırı olay ve yanıt uygulaması ile temel önerilir. Bu yol haritası için zaman çizelgelerini yaklaşık değerlerdir.

![Zaman çizelgelerini ile yol haritası aşamaları](./media/admin-roles-best-practices/roadmap-timeline.png)

* Aşama 1 (24-48 saat): öneririz kritik öğeleri yapmak hemen

* (2-4 hafta) 2. Aşama: en sık kullanılan saldırı teknikleri azaltmak

* Aşama 3 (1-3 ay): derleme görünürlük ve yönetici etkinliği üzerinde tam denetimi oluşturma

* 4. Aşama (altı ay ve ötesine): daha fazla güvenlik platformunuz sağlamlaştırmak için savunmayı oluşturmaya devam etmek

Bu yol haritası çerçeve zaten dağıttıysanız Microsoft teknolojilerini kullanımını en üst düzeye çıkarmak için tasarlanmıştır. Ayrıca, anahtar geçerli ve gelecekteki güvenlik teknolojileri yararlanabilir ve güvenlik araçları zaten dağıttıysanız veya dağıtma dikkate alarak, diğer satıcılardan tümleştirebilirsiniz. 

## <a name="stage-1-critical-items-that-we-recommend-you-do-right-away"></a>1. Aşama: öneririz kritik öğeleri hemen yapın

![Aşama 1](./media/admin-roles-best-practices/stage-one.png)

Yol Haritası 1 aşaması hızlı ve kolay uygulamak kritik görevler üzerinde odaklanmıştır. Bu birkaç öğeler hemen ilk 24-48 temel düzeyde bir güvenli ayrıcalıklı erişim sağlamak için saat içinde yapmanızı öneririz. Ayrıcalıklı erişimi güvenli yol haritası bu aşamasında, aşağıdaki eylemleri içerir:

### <a name="general-preparation"></a>Genel Hazırlık

#### <a name="turn-on-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management üzerinde Aç

Zaten Azure AD Privileged Identity Management (PIM) üzerinde değil kapattıysanız, üretim kiracınızda bunu. Privileged Identity Management üzerinde etkinleştirdikten sonra bildirim alırsınız e-posta iletileri ayrıcalıklı erişim için rol değişiklikleri. Ek kullanıcılar yüksek ayrıcalıklı rollere dizininizde eklendiğinde bu bildirimler erken uyarı sağlamak.

Azure AD Privileged Identity Management, Azure AD Premium P2 veya EMS E5 dahil edilir. Bu çözümlerin uygulamaları ve kaynaklara erişim buluta ve şirket içi ortamına arasında korumanıza yardımcı. İçin Azure AD Premium P2 ya da EMS E5 varsa ve bu Haritası başvurulan yeteneklerini daha fazla değerlendirmek isterseniz, kaydolun [Enterprise Mobility + Security ücretsiz 90 günlük deneme sürümü](https://www.microsoft.com/cloud-platform/enterprise-mobility-security-trial). Bu lisans denemeler Azure AD Privileged Identity Management ve Azure AD kimlik koruması, Azure AD raporlama güvenliği, denetlemek ve Uyarıları Gelişmiş kullanarak etkinliğini izlemek için kullanın.

Sonra üzerinde Azure AD Privileged Identity Management etkinleştirdiniz:

1. Oturum [Azure portal](https://portal.azure.com/) üretim kiracınızın genel yönetici olan bir hesapla.

2. Privileged Identity Management kullanmak istediğiniz Kiracı seçmek için Azure Portalı'nı sağ üst köşesinde kullanıcı adınızı seçin.

3. Seçin **tüm hizmetleri** ve liste için filtre **Azure AD Privileged Identity Management**.

4. Açık ayrıcalıklı Kimlik Yönetimi'nden **tüm hizmetleri** listelemek ve panonuza sabitleyin.

Azure AD Privileged Identity Management kiracınızda kullanmak için ilk kişi otomatik olarak atanır **Güvenlik Yöneticisi** ve **ayrıcalıklı Rol Yöneticisi** Kiracı rollerinde. Yalnızca ayrıcalıklı rol yöneticileri, kullanıcıların Azure AD directory rol atamalarını yönetebilirsiniz. Ayrıca, Azure AD Privileged Identity Management eklendikten sonra ilk bulma ve atama deneyimi boyunca eşlik eder Güvenlik Sihirbazı'nı gösterilir. Şu anda hiçbir ek değişiklik yapmadan Sihirbazı'ndan çıkabilirsiniz. 

#### <a name="identify-and-categorize-accounts-that-are-in-highly-privileged-roles"></a>Tanımlamak ve üst düzey ayrıcalıklı rolleri hesapları kategorilere ayırma 

Üzerinde Azure AD Privileged Identity Management etkinleştirdikten sonra dizin genel yöneticisi rolleri, ayrıcalıklı Rol Yöneticisi, Exchange Online yönetici ve SharePoint Online yönetici kullanıcılar görüntüleyin. Azure AD PIM kiracınızda yoksa kullanabileceğiniz [PowerShell API'si](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0). Bu rolün genel olarak genel yönetici rolü Başlat: Bu yönetici rolü atanan bir kullanıcının, kuruluşunuzun abone, olup, bu rolü Office 365 portalında atanmış bağımsız olarak tüm bulut hizmetlerinde aynı izinlere sahip , Azure portal veya PowerShell için Microsoft Azure AD modülünü kullanarak. 

Bu rollerden artık gerekli olmayan tüm hesaplarını kaldırın ve yönetici rolleri atanmış geri kalan hesapları kategorilere ayırma:

* Tek tek yönetici kullanıcılara atanan ve yönetim dışı amaçlar (örneğin, kişisel e-posta) için de kullanılabilir
* Tek tek yönetici kullanıcılara atanan ve yönetimsel amaçlarla atanan
* Birden çok kullanıcı arasında paylaşılan
* BREAK cam Acil erişim senaryoları için
* Otomatik betikler için
* Dış kullanıcılar için

#### <a name="define-at-least-two-emergency-access-accounts"></a>En az iki Acil erişim hesapları tanımlayın 

Bunlar yanlışlıkla oturum açın veya var olan tek bir kullanıcının hesabı yönetici olarak etkinleştirmek için Azure AD kiracınıza sorunu nedeniyle Yönetim dışı Kilitlediğiniz bir durum içine almazsanız emin olun. Kuruluş bir şirket içi kimlik sağlayıcısından Federasyon, kullanıcıların şirket içi imzalayamazsınız şekilde Örneğin, bu kimlik sağlayıcısı kullanılamıyor olması olabilir. İki veya daha fazla Acil erişim hesapları, kiracınızda depolayarak yönetimsel erişim yanlışlıkla eksikliği etkisini en aza indirebileceğiniz.

Acil erişim hesapları, kuruluşların mevcut Azure Active Directory ortamında ayrıcalıklı erişimi kısıtlama yardımcı olur. Bu hesapları yüksek ayrıcalıklı ve belirli kişilere atanmaz. Acil Durum erişimi hesaplarını burada normal yönetim hesapları kullanılamaz 'Acil Durum' senaryoları için Acil Durum sınırlıdır. Kuruluşlar, denetleme ve Acil Durum hesabın kullanımı için gereklidir o zaman yalnızca ile AIM emin olmalısınız. 

Atanan veya genel yönetici rolü için uygun hesapları değerlendirin. Yalnızca bulut herhangi listelenmiyor varsa kullanarak hesapları *. onmicrosoft.com etki alanı (amaçlayan "Acil Durum" Acil Durum erişimi için) oluşturun. Daha fazla bilgi için bkz: [Azure AD'de Acil Durum erişimi yönetici hesaplarını yönetme](active-directory-admin-manage-emergency-access-accounts.md).

#### <a name="turn-on-multi-factor-authentication-and-register-all-other-highly-privileged-single-user-non-federated-admin-accounts"></a>Çok faktörlü kimlik doğrulamasını etkinleştirmek ve diğer tüm yüksek ayrıcalıklı tek kullanıcı Federasyon olmayan yönetici hesapları kaydetme 

Oturum açma bir veya daha fazla Azure AD yönetim rolleri kalıcı olarak atanan tüm bireysel kullanıcılar için Azure multi-Factor Authentication (MFA) gerektirir: Genel yönetici, ayrıcalıklı Rol Yöneticisi, Exchange Online yönetici ve SharePoint Çevrimiçi yönetici. Etkinleştirmek için kılavuz kullanın [yönetici hesapları için çok faktörlü kimlik doğrulama (MFA)](../multi-factor-authentication/multi-factor-authentication-get-started-user-states.md) ve tüm kullanıcılarla kayıtlı olduğundan emin olun [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup). Adım 2 ve 3. Adım Kılavuzu'nun altında daha fazla bilgi bulunabilir [veri ve Office 365'te hizmetlere erişimi korumaya](https://support.office.com/article/Protect-access-to-data-and-services-in-Office-365-a6ef28a4-2447-4b43-aae2-f5af6d53c68e). 

## <a name="stage-2-mitigate-the-most-frequently-used-attack-techniques"></a>2. Aşama: en sık kullanılan saldırı teknikleri azaltmak

![Aşama 2](./media/admin-roles-best-practices/stage-two.png)

Yol Haritası, Aşama 2 kimlik hırsızlığına ve kötüye en sık kullanılan saldırı teknikleri Azaltıcı odaklanmıştır ve yaklaşık 2-4 hafta içinde uygulanması için tasarlanmıştır. Ayrıcalıklı erişimi güvenli yol haritası bu aşamasında, aşağıdaki eylemleri içerir.

### <a name="general-preparation"></a>Genel Hazırlık

#### <a name="conduct-a-inventory-of-services-owners-and-admins"></a>Hizmetleri, sahipleri ve yöneticileri sayımı gerçekleştir

Kendi-cihazını getir (BYOD) ve çalışma alanından giriş ilkeleri ve kablosuz bağlantı işletmelerde büyüme artışı ile ağınıza bağlanan izlemek önemlidir. Etkin güvenlik denetim cihazlar, uygulamalar ve programlar tarafından desteklenmiyor, ağınızdaki çalıştıran genellikle ortaya çıkarır BT ve bu nedenle olabilecek güvenliğini sağlayın. Daha fazla bilgi için bkz: [Azure güvenlik yönetimi ve izlemeye genel bakış](../security/security-management-and-monitoring-overview.md). Aşağıdaki görevlerin tümüne stok işleminize eklediğinizden emin olun. 

* Yönetici rolleri ve Hizmetleri burada yönetebilirsiniz kullanıcılar tanımlayın.
* Azure AD PIM Aşama 1'de listelenen ötesinde ek roller dahil olmak üzere Azure AD yönetim erişimi, kuruluşunuzda hangi kullanıcıların olduğunu bulmak için kullanın.
* Azure AD içinde tanımlıdır roller, Office 365, kuruluşunuzdaki kullanıcılara atayabileceğiniz yönetici rolleri kümesi ile birlikte gelir. Her Yönetici rolü için genel iş işlevleri eşler ve kişiler için Office 365 Yönetim Merkezi'nden belirli görevleri yapmak için kuruluş izinleri verir. Office Yönetim Merkezi, kuruluşunuzda hangi kullanıcıların Azure AD'de yönetilmeyen rolleri aracılığıyla dahil olmak üzere, Office 365 Yönetici erişimi olduğunu bulmak için kullanın. Daha fazla bilgi için bkz: [hakkında Office 365 Yönetici rollerine](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) ve [Office 365 için en iyi yöntemler](https://support.office.com/article/Security-best-practices-for-Office-365-9295e396-e53d-49b9-ae9b-0b5828cdedc3).
* Kuruluşunuz, Azure, Intune veya Dynamics 365 gibi dayanan diğer Hizmetleri'ndeki sayım gerçekleştirin.
* Yönetici hesaplarınızdan (yönetim amacıyla, yalnızca kullanıcıların günlük hesaplarının kullanılan hesapları) kendisine bağlı e-posta adresleri çalışma sahip ve Azure MFA için kayıtlı veya MFA şirket içi kullanma emin olun.
* Kullanıcılar kendi iş gerekçesinin yönetimsel erişim için isteyin.
* Bu kişiler ve gerekli hizmetleri için yönetici erişimi kaldırın.

#### <a name="identify-microsoft-accounts-in-administrative-roles-that-need-to-be-switched-to-work-or-school-accounts"></a>İş veya Okul hesapları geçirilmesi için gereken yönetici rollerini Microsoft hesapları tanımlayın 

Bazı durumlarda, Azure AD kullanarak başladığındaki ilk genel Yöneticiler bir kuruluş için var olan Microsoft hesabı kimlik bilgilerini yeniden. Bu Microsoft hesapları, tek tek bulut tabanlı ya da eşitlenmiş hesapları tarafından değiştirilmelidir. 

#### <a name="ensure-separate-user-accounts-and-mail-forwarding-for-global-administrator-accounts"></a>Ayrı kullanıcı hesaplarını ve genel yönetici hesapları için iletme posta emin olun 

Kişisel e-posta hesaplarına phished siber saldırganlar tarafından düzenli olarak olduğu gibi genel yönetici hesapları kişisel e-posta adresleri olmalıdır. Internet riskleri ayrı yardımcı olmak için (kimlik avı saldırıları, istenmeyen Web'e gözatma) yönetici ayrıcalıkları yönetici ayrıcalıkları olan her kullanıcı için ayrılmış hesapları oluşturun. 

Zaten yapmadıysanız, kullanıcıların bunlar istemeden e-posta açın veya yönetici hesaplarına ile ilişkili programları çalıştırmak emin olmak için genel yönetim görevlerini gerçekleştirmek ayrı hesapları oluşturun. Bu hesapların çalışma posta kutusuna iletilen e-postalarına sahip olduğundan emin olun.  

#### <a name="ensure-the-passwords-of-administrative-accounts-have-recently-changed"></a>Yönetici hesaplarının parolaları en son değiştirilen olun

Tüm kullanıcıların ve yönetici hesaplarını imzalı parolalarını en az bir kez Son 90 gün içinde değiştirilen emin olun. Ayrıca, tüm paylaşılan hangi birden çok kullanıcı hesaplarında parolayı bilmeniz en son değiştirilen parolalarını beklendiğinden emin olun.

#### <a name="turn-on-password-synchronization"></a>Parola Eşitleme Aç

Parola Eşitleme, bulut tabanlı bir Azure için şirket içi Active Directory örneğinden kullanıcı parola karmaları karmalarını eşitlemek için kullanılan bir özelliğidir AD örneği. Active Directory Federasyon Hizmetleri (AD FS) veya diğer kimlik sağlayıcılardan ile Federasyon kullanmaya karar verirseniz, isteğe bağlı olarak yapabileceğiniz bile parola eşitlemesi yedek olarak durumda AD gibi şirket içi altyapınızı ayarlama veya ADFS sunucuları başarısız veya olur geçici olarak kullanılamıyor. Bu hizmete kendi şirket içi oturum açmak için kullandıkları aynı parolayı kullanarak oturum açmalarını sağlar AD örneği. Ayrıca, bir kullanıcı, aynı e-posta adresini ve parolasını Azure AD ile bağlı değil diğer hizmetlerdeki işlevden değilse tehlikeye, bilinen parolalarla bu parola karmaları karşılaştırarak güvenliği aşılmış kimlik bilgileri algılamak kimlik koruması sağlar.  Daha fazla bilgi için bkz: [parola karma eşitlemesi ile Azure AD Connect eşitleme uygulama](./connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md).

#### <a name="require-multi-factor-authentication-mfa-for-users-in-all-privileged-roles-as-well-as-exposed-users"></a>Sunulan kullanıcıların yanı sıra tüm ayrıcalıklı rolleri için çok faktörlü kimlik doğrulaması (MFA) gerektirir

Azure AD, yöneticilerin ve kullanıcıların hesaplarını bozulsa önemli bir etkisi olması gereken tüm diğer kullanıcılar dahil olmak üzere tüm kullanıcılarınız için (örneğin, finansal görevlileri) multi-Factor authentication (MFA) gerekli önerir. Bu, güvenliği aşılmış bir parola nedeniyle bir saldırı riskini azaltır.

Aç:

* [Yüksek Etkilenme hesapları için MFA](../multi-factor-authentication/multi-factor-authentication-security-best-practices.md) executive görevlileri bir kuruluşta hesapları gibi 
* [MFA her yönetici hesabı için tek bir kullanıcı ile ilişkili](../multi-factor-authentication/multi-factor-authentication-get-started-user-states.md) diğer bağlı SaaS uygulamaları için 
* MFA Microsoft SaaS uygulamaları için tüm yöneticileri için roller yöneticiler dahil Exchange Online ve Office portalı yönetilen

Windows Hello işletmeler için kullanırsanız, MFA gereksinimi, Windows Hello oturum açma deneyimi kullanarak karşılanabilir. Daha fazla bilgi için bkz: [Windows Hello](https://docs.microsoft.com/windows/uwp/security/microsoft-passport). 

#### <a name="configure-identity-protection"></a>Kimlik Koruması'nı yapılandır 

Azure AD kimlik koruması bir algoritma tabanlı izleme ve raporlama kuruluşunuzdaki kimlikleri etkileyen olası güvenlik açıklarını algılamak için kullanabileceğiniz Aracı ' dir. Bu algılanan kuşkulu etkinlikleri otomatik yanıtlar yapılandırın ve bunları gidermek için uygun eylemi gerçekleştirin. Daha fazla bilgi için bkz: [Azure Active Directory kimlik koruması](active-directory-identityprotection.md).

#### <a name="obtain-your-office-365-secure-score-if-using-office-365"></a>(Office 365 kullanıyorsanız), Office 365 güvenli puan alın

Puan rakamları (OneDrive, SharePoint ve Exchange gibi) kullanıyorsanız sonra etkinlikleri ve ayarlarınızı arar ve Microsoft tarafından oluşturulan bir taban çizgisi karşılaştırır hangi Office 365 Hizmetleri güvenli hale getirin. Nasıl hizalanmış, en iyi güvenlik uygulamaları ile bulunduğunuz dayalı bir puan elde edersiniz. Office 365 iş ekstra veya kurumsal bir aboneliği en güvenli puan erişebilirsiniz için yönetici izinleri (genel yönetici veya bir özel Yönetici rolü) olan herkes [ https://securescore.office.com ](https://securescore.office.com/).

#### <a name="review-the-office-365-security-and-compliance-guidance-if-using-office-365"></a>(Office 365 kullanıyorsanız) Office 365 güvenlik ve uyumluluk Kılavuzu gözden geçirin

[Güvenlik ve uyumluluk için plan](https://support.office.com/article/Plan-for-security-and-compliance-in-Office-365-dc4f704c-6fcc-4cab-9a02-95a824e4fb57) bir Office 365 müşteri nasıl ve Office 365 yapılandırma diğer EMS özelliklerden yararlanacak bir yaklaşım özetlenmektedir. Ardından, 3-6'da nasıl gözden geçirme adımları [veri ve Office 365'te hizmetlere erişimi korumaya](https://support.office.com/article/Protect-access-to-data-and-services-in-Office-365-a6ef28a4-2447-4b43-aae2-f5af6d53c68e) ve nasıl Kılavuzu [güvenlik ve uyumluluk Office 365'te izleme](https://support.office.com/article/Monitor-security-and-compliance-in-Office-365-b62f1722-fd39-44eb-8361-da61d21509b6).


#### <a name="configure-office-365-activity-monitoring-if-using-office-365"></a>Office 365 etkinliğini izleme (Office 365 kullanıyorsanız) yapılandırma

Kimin bir yönetici hesabı varsa ve kimlerin Office 365 bu portallarda oturum imzalama değil nedeniyle erişemeyebilir kullanıcıları tanımlamak sağlayarak kuruluşunuzdaki kişilerin Office 365 hizmetleri nasıl kullandığını izleyebilirsiniz. Daha fazla bilgi için bkz: [etkinlik raporları Office 365 Yönetim Merkezi'nde](https://support.office.com/article/Activity-Reports-in-the-Office-365-admin-center-0d6dfb17-8582-4172-a9a9-aed798150263).

#### <a name="establish-incidentemergency-response-plan-owners"></a>Olay/Acil Durum yanıt planı sahipleri oluşturun

Olay yanıtlama etkili bir şekilde gerçekleştirme karmaşık bir iş değil. Bu nedenle, bir başarılı olay yanıtlama özelliği kurulmasını önemli planlama ve kaynakları gerektirir. Bu, sürekli olarak için siber saldırıları izleyebilir ve olayların işlenmesi öncelik yordamlarına oluşturabilir gereklidir. Çözümleme ve raporlama verilerini toplama etkin yöntemleri, ilişkileri oluşturmak ve diğer iç grupları ile iletişim kurmak ve sahipleri planlamak için önemli. Daha fazla bilgi için bkz: [Microsoft Security Response Center](https://technet.microsoft.com/security/dn440717). 

#### <a name="secure-on-premises-privileged-administrative-accounts-if-not-already-done"></a>Yapmadıysanız güvenli şirket içi yönetim hesapları, ayrıcalıklı

Şirket içi Active Directory ile Azure Active Directory kiracınızın eşitlenmişse yer alan yönergeleri izleyin [güvenlik ayrıcalıklı erişim yol haritası](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access): aşama 1. Bu şirket içi yönetim görevlerini gerçekleştirmek için Active Directory yöneticileri ayrıcalıklı erişimli iş istasyonlarının dağıtma ve iş istasyonları için benzersiz bir yerel yönetici parolaları oluşturma gereken kullanıcılar için farklı yönetici hesapları oluşturma içerir ve sunucuları.

### <a name="additional-steps-for-organizations-managing-access-to-azure"></a>Kuruluşlar Azure erişimi yönetmek için ek adımlar

#### <a name="complete-an-inventory-of-subscriptions"></a>Abonelikleri envanterini tamamlayın

Üretim uygulamaları barındıran abonelik kuruluşunuzdaki tanımlamak için Enterprise portal ve Azure portalını kullanın. 

#### <a name="remove-microsoft-accounts-from-admin-roles"></a>Microsoft hesapları yönetici rollerini kaldırın

Xbox Live ve Outlook gibi diğer programları Microsoft hesaplarından Kurumsal abonelikler için yönetici hesapları olarak kullanılmamalıdır. Tüm Microsoft hesaplarından yönetici durumu kaldırın ve Active Directory ile değiştirin (örneğin, chris@contoso.com) iş veya Okul hesapları.

#### <a name="monitor-azure-activity"></a>Azure etkinliğini izleme

Azure etkinlik günlüğü Azure abonelik düzeyinde olayları geçmişini sağlar. Hakkında bilgi kimin oluşturulan, güncelleştirilmiş ve hangi kaynaklara silinir ve bu olaylar meydana geldiğinde sunar. Daha fazla bilgi için bkz: [denetim ve Azure aboneliğinizde önemli eylemler hakkında bildirim almak](../monitoring-and-diagnostics/monitor-quick-audit-notify-action-in-subscription.md).


### <a name="additional-steps-for-organizations-managing-access-to-other-cloud-apps-via-azure-ad"></a>Diğer Azure AD aracılığıyla bulut uygulamalarına erişimi yönetme kuruluşlar için ek adımlar 

#### <a name="configure-conditional-access-policies"></a>Koşullu erişim ilkelerini yapılandırma

Koşullu erişim ilkeleri, şirket içi ve bulut tarafından barındırılan uygulamalar için hazırlayın. Kullanıcılar çalışma alanına katılmış aygıtlar varsa, daha fazla bilgi almak [ayarlama şirket içi koşullu erişim Azure Active Directory cihaz kaydı kullanarak](active-directory-device-registration-on-premises-setup.md).


## <a name="stage-3-build-visibility-and-take-full-control-of-admin-activity"></a>3. Aşama: Görünürlük oluşturmak ve yönetici etkinliği tam denetimini alma

![Aşama 3](./media/admin-roles-best-practices/stage-three.png)

3. aşama aşama 2'den üzerinde Azaltıcı oluşturur ve yaklaşık 1-3 ay içinde uygulanması için tasarlanmıştır. Ayrıcalıklı erişimi güvenli yol haritası bu aşamasında, aşağıdaki bileşenleri içerir.

### <a name="general-preparation"></a>Genel Hazırlık

#### <a name="complete-an-access-review-of-users-in-administrator-roles"></a>Yönetici rolleri erişim incelenmesi tamamlayın

Daha fazla şirket kullanıcıları artan bir yönetilmeyen platformu açabilir bulut Hizmetleri üzerinden ayrıcalıklı erişim sağlamasını. Bu, Office 365, Azure aboneliği yöneticileri ve VM'ler veya SaaS uygulamaları aracılığıyla yönetici erişimi olan kullanıcılar için genel yönetici olma kullanıcıları içerir. Bunun yerine, kuruluşların günlük iş hareketleri ayrıcalıksız kullanıcılar olarak işlemek ve yalnızca yönetici hakları gerektiği gibi yapın tüm çalışanlar, özellikle yöneticileri, olması gerekir. Yönetici rolleri sayısı bu yana ilk benimseme büyümüştür beri tanımlamak ve yönetici ayrıcalıkları etkinleştirmek uygun olan her kullanıcı onaylamak için tam erişim inceler. 

Şunları yapın:

* Hangi kullanıcıların Azure AD yöneticileri, etkinleştirme isteğe bağlı, yalnızca zaman yönetici erişimi ve rol tabanlı güvenlik denetimleri belirleyin.
* Farklı bir rol için ayrıcalıklı yönetici erişimi için hiçbir Temizle gerekçe sahip kullanıcılar dönüştürme (uygun rol yok, bunları kaldırın).

#### <a name="continue-rollout-of-stronger-authentication-for-all-users"></a>Tüm kullanıcılar için daha güçlü kimlik doğrulaması sunum devam 

C-suite Yöneticiler, üst düzey yöneticiler, kritik gerektiren BT ve güvenlik personel ve diğer yüksek oranda sunulan kullanıcılara Azure MFA veya Windows Hello gibi modern, güçlü kimlik. 

#### <a name="use-dedicated-workstations-for-administration-for-azure-ad"></a>Adanmış iş istasyonları için Azure AD yönetim kullanın

Saldırganlar, bütünlük ve kimlik doğrulama programı mantığı değiştirir veya bir kimlik bilgisi girme yönetici snoops kötü amaçlı kod aracılığıyla veri kesintiye uğratabilir şekilde kuruluşun veriler ve sistemlerle erişmek için ayrıcalıklı hesapları hedeflemek deneyebilir. Ayrıcalıklı erişimli iş istasyonlarının (Patiler) Internet saldırıları ve tehdit vektörlerini korunan hassas görevler için adanmış bir işletim sistemi sunar. Bu önemli görevleri ve hesapları günlük ayırma iş istasyonları kullanmak ve aygıtların kimlik avı saldırıları, uygulama ve işletim sistemi güvenlik açıkları, çeşitli kimliğe bürünme saldırılarını ve tuş vuruşu gibi kimlik bilgisi hırsızlığı saldırılara karşı çok güçlü koruma sağlar günlüğe kaydetme, Pass--Hash ve geçişi anahtar. Ayrıcalıklı erişimli iş istasyonlarının dağıtarak admins sağlamlaştırılmış bir masaüstü ortamı dışındaki yönetici kimlik bilgilerini girin riskini azaltabilir. Daha fazla bilgi için bkz: [ayrıcalıklı erişimli iş istasyonlarının](https://docs.microsoft.com/en-us/windows-server/identity/securing-privileged-access/privileged-access-workstations).

#### <a name="review-national-institute-of-standards-and-technology-recommendations-for-handling-incidents"></a>Olayların işlenmesi için Ulusal Standartlar ve Enstitüsü Technology önerileri gözden geçirin 

Teknoloji'nın (NIST) ve National Institute of Standards özellikle olay ilgili verileri çözümlemek ve her olay için uygun yanıtı belirlemek için olay işleme için yönergeler sağlar. Daha fazla bilgi için bkz: [(NIST) bilgisayar Security Incident Handling Guide (SP 800 61, düzeltme 2)](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf).

#### <a name="implement-privileged-identity-management-pim-for-jit-to-additional-administrative-roles"></a>JIT ek yönetim rolleri için uygulama ayrıcalıklı Kimlik Yönetimi (PIM)

Azure Active Directory kullanmak [Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md) yeteneği. Ayrıcalıklı rollerin zaman sınırlı etkinleştirme olanak tanıyarak çalışır:

* Belirli bir görevi gerçekleştirmek için yönetici ayrıcalıkları etkinleştir
* Etkinleştirme işlemi sırasında MFA zorla
* Yöneticileri bant dışı değişiklikleri hakkında bilgilendirmek için uyarıları kullanın
* Önceden yapılandırılmış bir süre için belirli ayrıcalıklara tutulacak kullanıcıları etkinleştir
* Tüm ayrıcalıklı kimlikleri bulmak, Denetim raporları görüntülemek ve yönetici ayrıcalıkları etkinleştirmek uygun olan her kullanıcı tanımlamak için erişim incelemeler oluşturmak güvenlik yöneticileri izin ver

Azure AD Privileged Identity Management'ı zaten kullanıyorsanız, zaman sınırlı ayrıcalıkları üretildikten (örneğin, bakım pencerelerini) gerektiği gibi ayarlayın.

#### <a name="determine-exposure-to-password-based-sign-in-protocols-if-using-exchange-online"></a>(Exchange Online kullanıyorsanız) parola tabanlı oturum açma protokolleri maruz belirleme

Geçmişte, kullanıcı adı/parola birleşimleri aygıtlar, e-posta hesaplarına, telefonlar ve benzeri ekli protokolleri olduğu varsayılır. Ancak artık bulutta siber saldırıları için risk ile kullanıcıların kimlik bilgilerini güvenliği bozulursa kuruluşa yıkıcı ve bunları kendi e-posta kullanıcı adı ile oturum açmak becerisinden hariç her olası kullanıcı tanımlamak öneririz / güçlü kimlik doğrulaması gereksinimleri ve koşullu erişim uygulayarak parola. 

#### <a name="complete-a-roles-review-assessment-for-office-365-roles-if-using-office-365"></a>Office 365 rolleri için rolleri gözden geçirme değerlendirmesi (Office 365 kullanıyorsanız) tamamlayın

Tüm Yöneticileri kullanıcıların doğru rollerinde olup olmadığı değerlendirmek (silin ve bu değerlendirme göre yeniden atama).

#### <a name="review-the-security-incident-management-approach-used-in-office-365-and-compare-with-your-own-organization"></a>Office 365'te kullanılan güvenlik olay Yönetimi yaklaşımı gözden geçir ve kendi kuruluşunuz ile Karşılaştır

Bu rapordan indirebilirsiniz [Microsoft Office 365'te güvenlik olay Yönetimi](https://www.microsoft.com/download/details.aspx?id=54302).

#### <a name="continue-to-secure-on-premises-privileged-administrative-accounts"></a>Şirket içi ayrıcalıklı yönetici hesaplarını güvenli hale getirmek devam edin

Azure Active Directory'yi şirket içi Active Directory'ye bağlıysa, yer alan yönergeleri izleyin [güvenlik ayrıcalıklı erişim yol haritası](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access): Aşama 2. Bu tüm yöneticilerin ayrıcalıklı erişimli iş istasyonlarının dağıtma, MFA gerektirme, yalnızca yeterli yönetici DC bakımı için kullanarak, etki alanları, saldırı yüzeyini düşürmek, ATA saldırı algılama için dağıtma içerir.

### <a name="additional-steps-for-organizations-managing-access-to-azure"></a>Kuruluşlar Azure erişimi yönetmek için ek adımlar

#### <a name="establish-integrated-monitoring"></a>Tümleşik izleme oluştur

[Azure Güvenlik Merkezi](../security-center/security-center-intro.md) Azure tümleşik güvenlik izleme ve ilke yönetimi sağlar, aksi takdirde gözden kaçan gidebilir Tehditler ve güvenlik geniş ekosistemiyle çalışır algılanmasına yardımcı olur çözümleri.

#### <a name="inventory-your-privileged-accounts-within-hosted-virtual-machines"></a>Ayrıcalıklı hesaplarınızdaki barındırılan sanal makinelerin içinde stok

Çoğu durumda, tüm Azure abonelikleri veya kaynaklar için kullanıcıların sınırsız izinleri vermeniz gerekmez. Azure AD yönetim rolleri, kuruluşunuz içinde görevlerini kurabilmeleri ve belirli işlerini gerçekleştirmek için gereken kullanıcılar için sadece erişim miktarını verebilirsiniz için kullanabilirsiniz. Örneğin, bir yönetici başka bir SQL veritabanları aynı abonelik içindeki yönetebilir ancak yalnızca bir Abonelikteki sanal makineleri yönetmek olanak tanımak için Azure AD yönetici rolleri kullanın. Daha fazla bilgi için bkz: [Azure portalında rol tabanlı erişim denetimi ile çalışmaya başlama](role-based-access-control-what-is.md).

#### <a name="implement-pim-for-azure-ad-administrator-roles"></a>Yönetici rolleri için Azure AD PIM uygulama

Privileged Identity Management ile Azure AD yönetici rolleri yönetin, denetleyin ve Azure kaynaklarına erişimi izlemek için kullanın. Ayrıcalıklı hesapların PIM kullanarak ayrıcalıkları Etkilenme süresini azaltarak ve kullanımlarını raporları ve Uyarıları aracılığıyla, görünürlük artırma tarafından siber saldırılara karşı korur. Daha fazla bilgi için bkz: [Privileged Identity Management ile Azure kaynaklarına RBAC yönetme erişimi](pim-azure-resource.md).

#### <a name="use-azure-log-integrations-to-send-relevant-azure-logs-to-your-siem-systems"></a>İlgili göndermek için Azure günlük tümleştirmeler kullanmak Azure günlüklerinin SIEM sistemlerinizi 

Azure günlük tümleştirme, Azure kaynaklarınızı ham günlükleri, kuruluşunuzun varolan güvenlik bilgileri ve Olay yönetimi (SIEM) sistemlerinden tümleştirmenize olanak sağlar. [Azure günlük tümleştirme](../security/security-azure-log-integration-overview.md) Windows Olay Görüntüleyicisi günlükleri ve Azure etkinlik günlükleri, Azure Güvenlik Merkezi uyarılarını ve Azure tanılama günlüklerini Azure kaynaklarını Windows olaylarını toplar. 


### <a name="additional-steps-for-organizations-managing-access-to-other-cloud-apps-via-azure-ad"></a>Diğer Azure AD aracılığıyla bulut uygulamalarına erişimi yönetme kuruluşlar için ek adımlar

#### <a name="implement-user-provisioning-for-connected-apps"></a>Kullanıcı için bağlantılı uygulamalar sağlama uygulama

Azure AD oluşturulması, Bakım ve kullanıcı kimlikleri Dropbox, Salesforce, ServiceNow gibi bulut (SaaS) uygulamalarında kaldırılmasını otomatik hale getirmek ve benzeri izin verir. Daha fazla bilgi için bkz: [otomatikleştirmek kullanıcı sağlama ve Azure AD ile SaaS uygulamalarına etkinleştirmektir](active-directory-saas-app-provisioning.md).

#### <a name="integrate-information-protection"></a>Information protection tümleştirme

MCAS dosyaları araştırın ve Azure Information Protection sınıflandırma etiketlerini esas alan ilkeleri ayarlamak için daha fazla görünürlük ve denetim verilerinizin bulutta etkinleştirme sağlar. Tarama ve bulutta dosyaları sınıflandırabilir ve Azure Information protection etiketleri uygulayabilirsiniz. Daha fazla bilgi için bkz: [Azure Information Protection tümleştirme](https://docs.microsoft.com/cloud-app-security/azip-integration).

#### <a name="configure-conditional-access"></a>Koşullu erişimi yapılandırma

Bir grup, konum ve uygulama duyarlılık bağlı olarak koşullu erişim yapılandırma [SaaS uygulamaları](https://azure.microsoft.com/overview/what-is-saas/) ve Azure AD bağlı uygulamalar. 

#### <a name="monitor-activity-in-connected-cloud-apps"></a>Bağlantılı bulut uygulamalarında etkinliğini izleme

Kullanıcıların erişimini de bağlı uygulamalarda korumalı emin olmak için yararlanan öneririz [Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security). Bu, olanak tanıyarak, yönetici hesaplarını güvenli hale getirme ek olarak uygulamalar, bulut için Kurumsal erişim güvenliğini sağlar:

* Görünürlük ve bulut uygulamalarında denetimini genişletme
* Erişim, etkinlikler ve veri paylaşımını ilkeleri oluşturma
* Riskli etkinliklerin, anormal davranışları ve tehditleri otomatik olarak tanımla
* Veri sızıntısını önlemeye
* Risk ve otomatik tehdit önleme ve ilke zorlama simge durumuna küçült

Bulut uygulama güvenlik SIEM aracısını Cloud App Security Office 365 uyarılar ve etkinlikler merkezi izlemeyi etkinleştirmek için SIEM sunucunuz ile tümleşir. Uyarılar ve etkinlikler Cloud App Security çeker sunucunuz üzerinde çalışan ve SIEM sunucusuna akışları. Daha fazla bilgi için bkz: [SIEM tümleştirme](https://docs.microsoft.com/cloud-app-security/siem).

## <a name="stage-4-continue-building-defenses-to-a-more-proactive-security-posture"></a>4. Aşama: daha etkin bir güvenlik tutumunu savunmayı oluşturmaya devam etmek


![4. Aşama](./media/admin-roles-best-practices/stage-four.png)

Yol Haritası 4 aşaması aşama 3'üzerinde görünürlük oluşturur ve altı ay içinde ve dışında uygulanması için tasarlanmıştır. Strong geliştirmek bir yol haritası yardımcı Tamamlanıyor erişim korumaları şu anda bilinen ve bugün kullanılabilir olası saldırılara karşı ayrıcalıklı. Ne yazık ki, güvenlik tehditlerinin sürekli gelişmesi ve shift nedenle maliyet oluşturma ve ortamınıza hedefleme rakiplerin başarı oranını azaltmak odaklanan devam eden bir işlem olarak güvenlik görüntülemek öneririz.

Ayrıcalıklı güvenliğini sağlama erişim güvenlik çıkışların iş varlıklar için modern bir kuruluşta oluşturma için önemli bir ilk adım, ancak ilke, işlemler, bilgiler gibi öğeleri içerir bir tam güvenlik programı yalnızca bir parçası değil Güvenlik, sunucuları, uygulamalar, bilgisayarlar, cihazlar, bulut doku ve diğer bileşenleri devam eden güvenlik güvence sağlar. 

Ayrıcalıklı erişim hesaplarınızı yönetmeye ek olarak, aşağıdaki düzenli olarak gözden geçirmenizi öneririz:

* Yöneticileri günlük işlerini ayrıcalıksız kullanıcılar olarak yaptıklarını emin olun.
* Yalnızca gerektiğinde ayrıcalıklı erişim vermek ve daha sonra (just-in-time) kaldırın.
* Korumak ve ayrıcalıklı hesaplara ilgili denetim etkinliği gözden geçirin.

Tüm güvenlik yol haritası oluşturma ile ilgili daha fazla bilgi için bkz: [Microsoft bulut BT mimarisi kaynaklarına](https://docs.microsoft.com/office365/enterprise/microsoft-cloud-it-architecture-resources). Aşağıdaki konulardan birini yardımcı olmak için Microsoft Hizmetleri katılımcılarını sürece dahil etme hakkında daha fazla bilgi için Microsoft temsilcinize başvurun veya bkz [yapı kuruluşunuzu korumak için kritik siber savunma](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).

Bu son devam eden aşaması ayrıcalıklı erişimi güvenli yol haritası, aşağıdaki bileşenleri içerir.

### <a name="general-preparation"></a>Genel Hazırlık

#### <a name="review-admin-roles-in-azure-active-directory"></a>Azure Active Directory'de yönetici rollerini inceleyin 

Geçerli yerleşik Azure AD yönetim rolleri hala güncel olup olmadığını belirlemek ve kullanıcıları rolleri ve ilgili izinlere ihtiyacı temsilcilerine içinde yalnızca emin olun. Azure AD ile farklı işlevler hizmet için ayrı Yöneticiler belirleyebilirsiniz. Daha fazla bilgi için bkz: [Azure Active Directory'de yönetici rolleri atama](active-directory-assign-admin-roles-azure-portal.md).

#### <a name="review-users-who-have-administration-of-azure-ad-joined-devices"></a>Azure ad yönetim gözden geçirme kullanıcılar katılmış cihazlarda

Daha fazla bilgi için bkz: [karma yapılandırmak için Azure Active Directory'ye katılmış cihazlarda nasıl](device-management-hybrid-azuread-joined-devices-setup.md).

#### <a name="review-members-of-built-in-office-365-admin-roleshttpssupportofficecomarticleabout-office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d"></a>Gözden üyeleri [yerleşik Office 365 Yönetici rolleri](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)
Office 365 kullanıyorsanız.
‎
#### <a name="validate-incident-response-plan"></a>Olay yanıtı planınızı doğrula

Planınıza artırmak için Microsoft, düzenli olarak planınız beklendiği gibi çalıştığını doğrulamak önerir:

* Varolan yol ne kaçırılan görmek için haritanızı gidin
* Postmortem analize dayalı olarak, var olan düzeltmek veya yeni en iyi uygulamaları tanımlayın
* En iyi yöntemler ve güncelleştirilmiş olay yanıtı planınızı kuruluşunuz genelinde dağıtıldığından emin olmak


### <a name="additional-steps-for-organizations-managing-access-to-azure"></a>Kuruluşlar Azure erişimi yönetmek için ek adımlar 

İçin gerekip gerekmediğini belirlemek [başka bir hesap için bir Azure aboneliği sahipliğini aktarma](../billing/billing-subscription-transfer.md).
‎

## <a name="break-glass-what-to-do-in-an-emergency"></a>"Acil Durum": acil durumlarda yapmanız gerekenler

![Acil Durum](./media/admin-roles-best-practices/emergency.jpeg)

1. Anahtar yöneticileri ve güvenlik görevlileri olayı ile ilgili bilgileri ile bildirin.

2. Saldırı playbook gözden geçirin. 

3. Azure AD ile oturum açmak için "Acil Durum" hesabı kullanıcı adı/parola bileşimi erişin. 

4. Microsoft tarafından Yardım [Azure destek isteği açma](../azure-supportability/how-to-create-azure-support-request.md).

5. Bakmak [Azure AD oturum açma raporları](active-directory-reporting-azure-portal.md). Rapora dahil olduğunda ve bir olay meydana arasında bir gecikme olabilir.

6. Federasyon, karma ortamlar ve AD FS sunucusu kullanılabilir değilse, geçici olarak parola karması eşitlemesi kullanmak için federe kimlik doğrulamasını geçmeniz gerekebilir. AD FS sunucusunda kullanılabilir oluncaya kadar bu etki alanı Federasyon yönetilen kimlik doğrulaması için geri döner.

7. Ayrıcalıklı hesaplar için e-posta izleyin.

8. Olası legal ve adli araştırma için ilgili günlükleri yedeklerini kaydettiğinizden emin olun.

Microsoft Office 365 güvenlik olayları nasıl işlediği hakkında daha fazla bilgi için bkz: [Microsoft Office 365'te güvenlik olay Yönetimi](http://aka.ms/Office365SIM).

## <a name="faq-common-questions-we-receive-regarding-securing-privileged-access"></a>Sık sorulan sorular: Sık sorulan sorular güvenliğini sağlama ayrıcalıklı erişim ile ilgili aldığımız  


**S:** t güvenli erişim bileşenleri henüz yapmadıysanız uygulanırsa ne yapmalıyım?

**Yanıt:** MFA, ayrıcalıklı yönetici hesapları ve ayrı ayrı kullanıcı hesapları için genel yönetici hesaplarını atama, en az iki sonu Acil Durum hesabı tanımlayın.


**S:** bir ihlal sonra önce ele alınması gereken üst sorun nedir?

**Yanıt:** yüksek oranda sunulan kişiler için güçlü kimlik doğrulama gerektiren emin olun.


**S:** bizim ayrıcalıklı yöneticileri devre dışı bırakıldı ne olur?

**Yanıt:** her zaman güncel tutulan bir genel yönetici hesabı oluşturun.


**S:** sol yalnızca bir genel yönetici vardır ve bunlar ulaşılamıyor ne olur? 

**Yanıt:** hemen ayrıcalıklı erişim kazanmak için sonu cam hesaplarınızı birini kullanın.


**S:** nasıl ı Koruyabileceğiniz admins Kuruluşum içinde?

**Yanıt:** her zaman standart "ayrıcalıksız" kullanıcılar olarak günlük işlerini yapmak yöneticilere sahip.
 

**S:** Azure AD içinde yönetici hesapları oluşturmak için en iyi uygulamalar nelerdir?

**Yanıt:** ayırma ayrıcalıklı erişim belirli yönetim görevleri için.


**S:** kalıcı yönetici erişimi azaltmak için hangi Araçlar var?

**Yanıt:** ayrıcalıklı Kimlik Yönetimi (PIM) ve Azure AD yönetim rolleri.


**S:** yönetici hesapları Azure ad eşitleme Microsoft konumu nedir?

**Yanıt:** katman 0 yönetici hesapları (hesaplarını, grupları ve AD ormanı, etki alanı veya etki alanı denetleyicileri doğrudan veya dolaylı yönetici denetime sahip diğer varlıklar ve tüm varlıklar dahil), yalnızca kullanıldığı şirket içi AD hesapları ve öğeler Bulut için Azure AD için normal eşitlendi. 


**S:** biz kalmasını nasıl admins rastgele yönetici erişimi portalında atama?

**Yanıt:** ayrıcalıklı olmayan hesaplar tüm kullanıcılar ve çoğu Yöneticiler için kullanın. Bir yer olan birkaç yönetici hesapları ayrıcalıklı belirlemek için kuruluşun geliştirerek başlatın. Ve yeni oluşturulan yönetim kullanıcılarına izleyin.


## <a name="next-steps"></a>Sonraki adımlar

* [Microsoft Trust Center ürün güvenlik](https://www.microsoft.com/en-us/trustcenter/security) – güvenlik özellikleri, Microsoft bulut ürünleri ve Hizmetleri

* [Microsoft Trust Center - Uyumluluk](https://www.microsoft.com/en-us/trustcenter/compliance/complianceofferings) – Microsoft'un kapsamlı bulut Hizmetleri için Uyumluluk teklifleri kümesi

* [Risk değerlendirmesi gerçekleştirme hakkında yönergeler](https://www.microsoft.com/en-us/trustcenter/guidance/risk-assessment) -Microsoft bulut Hizmetleri için güvenlik ve uyumluluk gereksinimlerini yönetme

### <a name="other-ms-online-services"></a>Diğer MS Çevrimiçi Hizmetler 

* [Microsoft Intune güvenlik](https://www.microsoft.com/en-us/trustcenter/security/intune-security) – Intune mobil cihaz yönetimi, mobil uygulama yönetimi ve bulutta bilgisayarı yönetim özellikleri sağlar.

* [Microsoft Dynamics 365 güvenlik](https://www.microsoft.com/en-us/trustcenter/security/dynamics365-security) – Dynamics 365 Müşteri İlişkileri Yönetimi (CRM) ve kurumsal kaynak (ERP) özellikleri planlama birleştiren Microsoft bulut tabanlı çözümü değil.

 
