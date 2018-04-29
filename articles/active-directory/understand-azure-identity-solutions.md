---
title: Azure kimlik anlama | Microsoft Docs
description: Microsoft Azure kimlik çözümü koşulları, kavramlar ve öneriler, kuruluşunuz için en iyi Kimlik Yönetimi karar vermeniz temel bir anlayış alın.
keywords: ''
author: jeffgilb
manager: mtillman
ms.reviewer: jsnow
ms.author: jeffgilb
ms.date: 7/17/2017
ms.topic: article
ms.prod: ''
ms.service: azure
ms.technology: ''
ms.assetid: ''
ms.custom: it-pro
ms.openlocfilehash: bd8e122324ab2d4c783fb6d4e09a9f4f197f91ca
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="understand-azure-identity-solutions"></a>Azure kimlik çözümleri anlama
Microsoft Azure Active Directory (Azure AD) dizin hizmetleri, kimlik yönetimi ve uygulamaya erişim yönetimi sağlayan bir kimlik ve erişim yönetimi bulut çözümüdür. Azure AD hızla [çoklu oturum açma (SSO) etkinleştirir](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-sso) 000 1, kullanıcının önceden tümleştirilmiş ticari ve özel uygulamalar [Azure AD uygulama galerisinde](https://azure.microsoft.com/marketplace/active-directory/all/). Çoğu bu uygulamaların, büyük olasılıkla zaten Office 365, Salesforce.com, kutusunu, ServiceNow ve Workday gibi kullanın.

Tek bir oluşturulduğunda Azure AD dizini otomatik olarak bir Azure aboneliğiyle ilişkili. Azure'da kimlik hizmet olarak sonra Azure AD bulut tabanlı kaynaklar için tüm kimlik yönetimi ve erişim denetim işlevleri sağlar. Bu kaynaklar kullanıcılar, uygulamalara ve grupları tek bir kiracıya (kuruluş) için aşağıdaki çizimde gösterildiği gibi içerebilir:

![Azure Active Directory](./media/understand-azure-identity-solutions/azure-ad.png)

Microsoft Azure, tek tek kuruluşunuzun gereksinimlerini karşılamak için karmaşıklık değişen düzeylerde Identity service (Idaas) yararlanmak için çeşitli yollar sunar. Bu makalede geri kalanı için kullanılabilir seçenekler arasından en iyi seçim yapmanızı önerileri yanı sıra temel Azure kimlik terminolojisi ve kavramları anlamanıza yardımcı olur.

## <a name="terms-to-know"></a>Bilmeniz koşulları

Kuruluşunuz için Azure kimlik çözümünü karar verebilirsiniz önce Azure kimlik hizmetleri hakkında konuşurken yaygın olarak kullanılan terimler temel bir anlayış gerekir.

|Bilmeniz terimi| Açıklama|
|-----|-----|
|Azure aboneliği |Abonelikleri Azure bulut Hizmetleri için ödeme yapmak için kullanılır ve genellikle bir kredi kartına bağlı. Birden fazla abonelik olabilir, ancak kaynakları abonelikler arasında paylaşmak zor olabilir.|
|Azure Kiracı | Azure AD kiracısı tek bir kuruluşun temsilcisidir. Kuruluş Azure, Intune veya Office 365 gibi Microsoft bulut hizmeti aboneliği kaydolduğunda, otomatik olarak oluşturulan Azure AD ayrılmış, güvenilen bir örneği değil. Kiracılar ya da bir ortamda ayrılmış (tek kiracılı) veya diğer kuruluşlarla (çok kullanıcılı) paylaşılan bir ortamda hizmetlerine erişebilir.|
|Azure AD dizini | Her bir Azure Kiracı ayrılmış, güvenilen bir Azure sahip kiracının kullanıcılar, gruplar ve uygulamalar içeren AD dizini. Kimlik gerçekleştirmek ve yönetim işlevleri için Kiracı kaynaklarına erişmek için kullanılır. Çünkü benzersiz bir Azure AD dizini, Azure, Microsoft Intune veya Office 365 gibi Microsoft bulut hizmeti için kaydolduğunuzda, kuruluşunuz temsil etmek için sağlanan otomatik olarak, koşulları bazen göreceğiniz *Kiracı*, *Azure AD*, ve *Azure AD dizini* birbirinin yerine kullanılır. |
|Özel etki alanı | Önce bir Microsoft bulut hizmeti aboneliği için kaydolduğunuzda, kiracınız (kuruluş) kullanan bir *. onmicrosoft.com* etki alanı adı. Bununla birlikte, çoğu kuruluş bir veya şirket kaynaklarına erişmek için iş ve son kullanıcıların yapmak için kullanılan daha fazla etki alanı adları kullanın. Böylece etki alanı adı gibi kullanıcılarınız için tanıdık, özel etki alanı adınızı Azure AD'ye ekleyebilirsiniz *alice@contoso.com* yerine *alice@contoso.onmicrosoft.com*. |
|Azure AD hesabı | Azure AD kullanarak oluşturulan kimlikleri bunlar veya Office 365 gibi başka bir Microsoft bulut hizmeti. Bunlar, Azure AD'de depolanan ve herhangi bir kuruluşun bulut hizmeti aboneliklerinizde erişilebilir. |
|Azure Abonelik Yöneticisi| Hesap Yöneticisi kaydolup veya Azure aboneliği satın kullanıcıdır. Kullanabileceklerini [hesap Merkezi'nde](https://account.azure.com/Subscriptions) abonelikleri oluşturma gibi çeşitli yönetim görevlerini gerçekleştirmek için aboneliklerinizi iptal edin, bir abonelik için faturalama değiştirme veya Hizmet Yöneticisi değiştirin. |
|Azure AD genel yönetici | Azure AD genel yöneticilerin tüm Azure AD yönetim özelliklerine tam erişimi vardır. Bir Microsoft bulut hizmeti aboneliği için otomatik olarak kaydolduğunda kişiye varsayılan olarak genel yönetici olur. Birden çok genel yönetici olabilir, ancak yalnızca genel Yöneticiler herhangi birini atayabilirsiniz [diğer yönetici rollerini](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal) kullanıcılara. |
|Microsoft hesabı | Microsoft hesapları (tarafından kişisel kullanım için oluşturduğunuz) tüketiciye yönelik Microsoft ürünleri için erişim sağlamak ve Outlook (Hotmail), OneDrive, Xbox LIVE veya Office 365 gibi hizmetler bulut. Bu kimlikleri oluşturulur ve Microsoft tarafından çalıştırılan Microsoft tüketici kimlik hesap sistemi depolanır.|
|İş veya okul hesapları | İş veya Okul hesapları (iş/akademik kullanmak için bir yönetici tarafından verilen), iş düzeyi gibi Microsoft bulut Hizmetleri, Azure, Intune veya Office 365 Kurumsal erişim sağlar.|


## <a name="concepts-to-understand"></a>Anlamak için kavramları

Temel Azure kimlik koşulları bildiğinize göre bunlar hakkında daha fazla bilgi yardımcı Azure kimlik kavramları hakkında bilgi sahibi Azure kimlik hizmet karar olun.

|Kavram anlamak için |Açıklama|
|-----|-----|
|[Azure aboneliklerinin Azure Active Directory ile ilişkisi](https://docs.microsoft.com/azure/active-directory/active-directory-how-subscriptions-associated-directory) |Her Azure aboneliği kullanıcıların, hizmetleri ve aygıtların kimliğini doğrulamak için Azure AD dizini bir güven ilişkisi yok. *Birden çok abonelik aynı Azure AD dizini güvenebilir ancak bir abonelik yalnızca tek bir güvenecek Azure AD dizini*. Bu güven ilişkisi, daha fazla abonelik alt kaynakları gibi diğer Azure kaynaklarının (Web siteleri, veritabanları ve benzeri) sahip bir aboneliğe sahip ilişki benzemez. Bir aboneliğin süresi dolarsa Azure AD dışında aboneliğiyle ilişkili kaynaklara erişim de durdurulur. Ancak, siz de başka bir aboneliği bu dizinle ilişkilendirebilir ve Kiracı kaynaklarını yönetmeye devam Azure AD dizini Azure içinde kalır.|
|[Works lisanslama nasıl Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-licensing-get-started-azure-portal) | Satın alma veya Enterprise Mobility Suite, Azure AD Premium veya Azure AD temel etkinleştirme, dizininize abonelik geçerlilik süresi ve ön ödemeli lisansları dahil, güncelleştirilir. Abonelik etkinleştirildikten sonra hizmet Azure AD genel yönetici tarafından yönetilen ve lisanslı kullanıcılar tarafından kullanılır. Atanan veya kullanılabilir lisans sayısı gibi abonelik bilgilerinizi Azure Portalı'nda kullanılabilir **Azure Active Directory** > **lisansları** dikey. Bu ayrıca, lisans anlaşmalarını yönetmek için en iyi yerdir.|
|[Azure portalında rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/role-based-access-control/overview)|Azure rol tabanlı erişim denetimi (RBAC) Azure kaynakları için ayrıntılı erişim yönetimini sağlamaya yardımcı olur. Çok fazla izinler, kullanıma ve saldırganlar için hesap. Çok az izinleri anlamına gelir çalışanlar verimli bir şekilde işlerini alınamıyor. RBAC kullanarak, tüm kaynak gruplarına uygulanan üç temel rol göre ihtiyaç duydukları izinleri tam çalışanlar verebilirsiniz: sahibi, katkıda bulunan, okuyucu. En fazla 2.000 kendi oluşturabilirsiniz [özel RBAC rolleri](https://docs.microsoft.com/azure/role-based-access-control/custom-roles) belirli ihtiyaçlarınızı karşılaması için. |
|[Karma kimlik](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect)|Karma kimlik elde edilir, şirket içi Windows Server Active Directory (AD DS) ile tümleştirerek Azure AD kullanarak [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect). Bu, Office 365, Azure ve şirket içi uygulamalar veya Azure AD ile tümleşik SaaS uygulamaları için kullanıcılarınız için ortak bir kimlik sağlamanıza olanak verir. Karma kimlik ile şirket içi ortamınıza bulut kimlik ve erişim için etkili bir şekilde genişletir.|

### <a name="the-difference-between-windows-server-ad-ds-and-azure-ad"></a>Windows Server AD DS ve Azure AD arasındaki fark
Azure Active Directory (Azure AD) ve şirket içi Active Directory (Active Directory etki alanı Hizmetleri veya AD DS), dizin verilerini depolar ve kullanıcılar ve kullanıcı oturum açma işlemleri, kimlik ve dizin aramaları gibi kaynakları arasındaki iletişimi yönetme sistemleridir.

Zaten şirket içi Windows Server Active Directory etki alanı Hizmetleri ile (AD DS) biliyorsanız, büyük olasılıkla bir kimlik hizmeti temel kavramı anlamak daha sonra ilk Windows 2000 Server ile kullanıma sunuldu. Ancak, ayrıca Azure AD yalnızca bir etki alanı denetleyicisi bulutta olmadığını anlamak önemlidir. Tam olarak bulut tabanlı özellikleri çekirdeğin ve kuruluşunuzun modern tehditlere karşı korumak düşünüyorum, tamamen yeni bir yol gerektiren Azure service (Idaas) olarak kimlik sağlama tamamen yeni bir yoludur. 

AD DS, Windows Server'da, fiziksel veya sanal makinelerde dağıtılabilir anlamına gelir, bir sunucu rolüdür. Üzerinde X.500 göre hiyerarşik bir yapı vardır. Nesneleri bulmak bulunulması LDAP kullanarak ve birincil kimlik doğrulaması için Kerberos kullanan DNS kullanır. Makine etki alanına katılma yanı sıra Active Directory kuruluş birimlerini (OU) ve Grup İlkesi nesneleri (GPO'lar) etkinleştirir ve güvenler etki alanları arasında oluşturulur.

BT kendi güvenlik çevre AD DS kullanılarak yıl, ancak çalışanlar, müşterileri ve ortakları yeni bir denetim düzlemi olabilmesi kimlik gereksinimlerini destekleyen modern, çevre-daha küçük kuruluşlar için korumalı. Azure AD, kimlik denetim Düzlemi ' dir. Güvenlik, burada Azure AD şirket kaynaklarına ve korur erişim kullanıcılar için ortak bir kimlik sağlayarak buluta Kurumsal güvenlik duvarı taşındı (ya da şirket içi veya bulutta). Bu, kullanıcılarınızın neredeyse her aygıttan ve işlerini yapmak için gereksinim duydukları uygulamalara güvenli bir şekilde erişmek için esneklik sunar. Şirket verilerini korumak için BT gereksinimlerini güvenli olması koşuluyla makine öğrenme yetenekleri ve kapsamlı raporlama tarafından yedeklenen sorunsuz risk tabanlı veri koruması, ayrıca denetimleridir.

Azure AD içinde Azure AD bulut sunucuları ve Office 365 gibi uygulamalar için bir kiracı oluşturabileceğiniz anlamına gelir çoklu müşteri ortak dizin hizmetidir. Kullanıcılar ve gruplar, düz bir yapı OU veya GPO'ları olmadan oluşturulur. Kimlik doğrulaması, SAML gibi protokoller üzerinden gerçekleştirilir WS-Federation ve OAuth. Azure AD sorgu mümkündür, ancak LDAP kullanmak yerine, AD grafik API'si olarak adlandırılan bir REST API'si kullanmanız gerekir. Bu tüm iş HTTP ve HTTPS.

### <a name="extend-office-365-management-and-security-capabilities"></a>Office 365 Yönetim ve güvenlik yetenekleri genişletme
Zaten Office 365 kullanıyor? Sayısal dönüşüm tüm iş gücüne yönelik güvenli üretkenliği etkinleştirirken, kaynaklarınızı güvenli hale getirmek için yerleşik Office 365 özellikleri Azure AD ile genişleterek hızlandırabilir. Office 365 özelliklere ek Azure AD kullandığınızda, tüm uygulama Portföy tüm uygulamalar için çoklu oturum açmayı etkinleştiren bir kimlikle güvenliğini sağlayabilirsiniz. Yalnızca cihaz durumu, ancak kullanıcı, konum, uygulama ve risk de göre koşullu erişim özelliklerinizi genişletebilirsiniz. Gerektiğinde çok faktörlü kimlik doğrulama (MFA) özellikleri daha da fazla koruma sağlar. Kullanıcı ayrıcalıkları, ek gözetim elde ve isteğe bağlı, yalnızca zaman yönetimsel erişim sağlar. Kullanıcılarınızın daha üretken ve Azure AD unutulmuş parola, uygulama erişim isteklerini sıfırlama ve grup oluşturma ve yönetme gibi sağlar Self Servis özellikleri sayesinde daha az Yardım Masası biletleri oluşturun.

> [!TIP]
> Office 365 ile Azure AD Kimlik Yönetimi'ni kullanma hakkında daha fazla bilgi edinmek istiyorsanız? [E-kitap almak](https://info.microsoft.com/Extend-Office-365-security-with-EMS.html).

## <a name="microsoft-azure-identity-solutions"></a>Microsoft Azure kimlik çözümleri

Microsoft Azure tam olarak korunur olup olmadığını, kullanıcıların kimliklerini yönetmek için birkaç yöntem sunar şirket içi, yalnızca bulutta veya hatta herhangi bir yerde arasındaki. Bu seçenekler şunlardır: yazılanları (DIY) AD DS'de Azure, Azure Active Directory (Azure AD), karma kimlik ve Azure AD etki alanı Hizmetleri.

### <a name="do-it-yourself-diy-ad-ds"></a>Yazılanları (Dıy) AD DS
Bulut yalnızca küçük bir yer ihtiyaç duyan şirketler için **yazılanları (DIY) AD DS** Azure içinde kullanılabilir. Bu seçenek Azure sanal makinelerde (VM'ler) olarak dağıtım için oldukça uygun olan birçok Windows Server AD DS senaryolarını destekler. Örneğin, uzak ağa bağlı bir uzak veri merkezinde çalışan bir etki alanı denetleyicisi bir Azure VM oluşturabilirsiniz. Buradan, VM kimlik doğrulama performansını iyileştiren ve uzak kullanıcılardan kimlik doğrulama isteklerini desteklemek gerçekleştirebilir. Bu ayrıca az sayıda etki alanı denetleyicileri ve Azure üzerinde tek bir sanal ağa barındırarak nispeten düşük maliyetli yerine aksi maliyetli olağanüstü durum kurtarma sitelerinde için oldukça uygun bir seçenektir. Son olarak, Windows Server AD DS gerektirir, ancak şirket içi ağ veya şirket Windows Server Active Directory üzerinde hiçbir bağımlılık içeriyor SharePoint gibi Azure'da bir uygulamayı dağıtmak gerekebilir. Bu durumda, ayrı bir ormanda SharePoint server grubun gereksinimlerini karşılamak için Azure üzerinde dağıtabilirsiniz. Ayrıca, şirket içi ağ ve şirket içi Active Directory bağlantısı gerektiren ağ uygulamaları dağıtmak için de desteklenir.

### <a name="azure-active-directory-azure-ad"></a>Azure Active Directory (Azure AD)
**Azure AD tek başına** bir tam olarak bulut tabanlı kimlik ve erişim yönetimi Service (Idaas) çözümü olarak. Azure AD, güçlü bir kullanıcı ve grupları yönetmek için özellikler kümesi sağlar. Bu çözüm, Office 365 gibi Microsoft web hizmetlerini ve Microsoft tarafından sunulmayan birçok hizmet olarak yazılım (SaaS) uygulamasını da kapsayan şirket içi uygulamalara ve bulut uygulamalarına güvenli erişim sağlanmasına yardımcı olur. Azure AD gelen üç sürümlerinde: ücretsiz, temel ve Premium. Azure AD kuruluş verimliliğini artırır ve yeni bir denetim düzlemi Azure machine learning tarafından korunan ve diğer çevre güvenlik duvarı dışındaki güvenlik Gelişmiş güvenlik özelliklerini genişletir.

### <a name="hybrid-identity"></a>Karma kimlik
Bunun yerine şirket içi veya bulut tabanlı kimlik çözümleri arasında seçim yapın daha birçok İleri düşünmeye Cıo'lar ve şirketin uzun vadeli yönü bekleme başlamıştır, işletmeler, bulut üzerinden kendi şirket içi dizinlere genişletme **karma kimlik** çözümler. Karma kimlik ile kullanan müşteriler tamamıyla küresel bir kimliği alın ve uygulamaları kullanıcılara güvenli ve verimli erişim sağlayan erişim yönetimi çözümü işlerini yapmaları gerekir.

> [!TIP]
> Cıo'lar kendi BT stratejilerini merkezi bir parçası Azure Active Directory nasıl yaptığınız hakkında daha fazla bilgi için indirme [CIO'ın Kılavuzu Azure Active Directory'ye](https://aka.ms/AzureADCIOGuide).

### <a name="azure-ad-domain-services"></a>Azure AD Domain Services
**Azure AD etki alanı Hizmetleri** basit Azure VM yapılandırma denetimi ve ağ uygulama geliştirme ve test için şirket içi kimlik gereksinimlerini karşılamak üzere bir yol için AD DS kullanmak için bulut tabanlı bir seçenek sağlar. Azure AD Etki Alanı Hizmetleri'ni kaldırın ve şirket içi shift değil yöneliktir Azure VM'ler için AD DS altyapısı Azure AD etki alanı Hizmetleri tarafından yönetilir. Bunun yerine, yönetilen etki alanlarında Azure VM'ler, geliştirme, test ve AD DS kimlik doğrulama yöntemleri buluta gerektiren şirket içi uygulamalar hareketini desteklemek için kullanılmalıdır.

## <a name="common-scenarios-and-recommendations"></a>Yaygın senaryolar ve öneriler

Burada, bazı ortak kimlik ve erişim senaryoları için hangi Azure kimlik seçeneği her biri için en uygun olabilecek öneriler bulunmaktadır.

|Kimlik senaryosu| Öneri|
|-----|-----|
|Kuruluşumun içi Windows Server Active Directory içinde büyük yatırımlar yaptı, ancak kimlik buluta genişletmek istiyoruz.| En yaygın olarak kullanılan Azure kimlik çözümüdür [karma kimlik](https://docs.microsoft.com/azure/active-directory/active-directory-hybrid-identity-design-considerations-overview). Zaten Yatırımlar şirket içi AD DS yaptıysanız, Azure AD Connect'i kullanarak bulut kimliğine kolayca genişletebilirsiniz.|
|İşimi bulutta doğdu ve şirket içi kimlik çözümleri hiçbir Yatırımlar sunuyoruz.| [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) hiçbir yalnızca bulut işletmeler için en iyi seçenek şirket içi Yatırımlar.|
|Basit Azure VM yapılandırması gerekir ve karşılamak üzere Denetim içi uygulama geliştirme ve test etme için kimlik gereksinimleri.|[Azure AD etki alanı Hizmetleri](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview) basit Azure VM yapılandırma denetimi için AD DS kullanmanız gerekir ya da geliştirme veya eski, dizin kullanan şirket içi uygulamaları bulutta geçirme bakarken, iyi bir seçimdir.|  
|Azure'da birkaç sanal makineleri destekleyen gerekir, ancak Şirketim hala yoğun bir şekilde şirket içi Active Directory (AD DS) yatırım.|Kullanmak [Dıy AD DS](https://msdn.microsoft.com/library/azure/jj156090.aspx) şirket içi birkaç sanal makineyi desteklemek ve büyük AD DS sahip gerektiğinde Azure VM kullanmak için. |

## <a name="where-can-i-learn-more"></a>Burada daha fazla bilgi edinebilirsiniz?
Bir ton harika kaynaklarının çevrimiçi tüm Azure AD hakkında bilgi edinmenize yardımcı olmak için sahibiz. Başlamanıza yardımcı olmak için harika makalelerin bir listesi aşağıda verilmiştir:

* [Dizininizi Azure AD Connect ile karma yönetimi için etkinleştirme](active-directory-aadconnect.md)
* [Şimdiye kadar bağlı dünya için ek güvenlik](authentication/multi-factor-authentication.md)
* [Kullanıcı sağlama ve Azure Active Directory ile SaaS uygulamalarına sağlamayı otomatikleştirme](active-directory-saas-app-provisioning.md)
* [Azure AD Raporlama ile çalışmaya başlama](active-directory-reporting-getting-started.md)
* [Her yerden, parolaları yönetme](active-directory-passwords-update-your-own-password.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)
* [Kullanıcı sağlama ve Azure Active Directory ile SaaS uygulamalarına sağlamayı otomatikleştirme](active-directory-saas-app-provisioning.md)
* [Şirket içi uygulamalara güvenli uzaktan erişim sağlama](active-directory-application-proxy-get-started.md)
* [Azure Active Directory grupları ile kaynaklara erişimi yönetme](active-directory-manage-groups.md)
* [Hangi Microsoft Azure Active Directory lisanslaması nedir?](active-directory-licensing-whatis-azure-portal.md)
* [Nasıl ı Kuruluşum içinde kullanılan onaylanmamış bulut uygulamaları bulabilir](active-directory-cloudappdiscovery-whatis.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure kimlik kavramları ve sizin için kullanılabilir seçenekleri anladığınıza göre seçtiğiniz seçeneği uygulama başlamak için aşağıdaki kaynakları kullanın:

[Azure karma kimlik çözümleri hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/choose-hybrid-identity-solution)

[Bir Azure kavram kanıtı ortamda daha fazla bilgi edinin](https://aka.ms/aad-poc)

[Üretimde Azure AD dağıtma](https://aka.ms/aad-onboard)
