---
title: Azure güvenlik Teknik Özellikleri | Microsoft Docs
description: Geniş işlem örnekleri dahil bulut tabanlı bilgi işlem Hizmetleri & Yukarı ve aşağı otomatik olarak uygulamanızı veya Kurumsal ihtiyaçlarını karşılamak üzere ölçeği hizmetleri hakkında bilgi edinin.
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2017
ms.author: TomSh
ms.openlocfilehash: b0cef0a261b0362fcb9776e63c10e96aedc408b9
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="azure-security-technical-capabilities"></a>Azure güvenlik Teknik Özellikleri

Geçerli ve gelecekteki Azure yardımcı olmak için müşteriler anlamak ve çeşitli güvenlikle ilgili bulunan becerilerinden ve Azure platformu çevreleyen, Microsoft teknik incelemeler, güvenlik genel bakışlar, en iyi yöntemler, bir dizi geliştirmiştir ve Denetim listeleri. Konular avantajlarına ve derinliği bakımından aralığı ve düzenli aralıklarla güncelleştirilir. Bu belge aşağıdaki soyut bölümde özetlendiği gibi serisi bir parçası değil. Bu Azure güvenlik seri hakkında daha fazla bilgi (URL'de) bulunabilir.

## <a name="azure-platform"></a>Azure platformu

[Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/) bulut platformu altyapısı oluşur ve uygulama hizmetleriyle tümleşik Veri Hizmetleri gelişmiş analizler ve geliştirici araçları ve Hizmetleri, barındırılan içinde Microsoft'un genel bulut veri merkezleri. Müşteriler çok sayıda farklı kapasiteler ve senaryolarından, temel işlem, ağ ve depolama, mobil ve nesnelerin interneti gibi tam bulut senaryosu için web uygulama hizmetleri için Azure kullanın ve açık kaynaklı teknolojilerle kullanılır ve karma bulut olarak dağıtılan veya bir müşterinin veri merkezi içinde barındırılan. Şirketler, maliyet tasarrufu yardımcı olmak için yapı taşları hızla yenilik ve sistemler proaktif olarak yönetirken azure bulut teknolojisi sağlar. Oluşturmanıza veya BT varlıklar için bir bulut sağlayıcısı geçirmek, uygulamalar ve hizmetler ve bulut tabanlı varlıklarınızı güvenliği yönetmek için sağladıkları denetimleri ile verileri korumak için bir kuruluşun yeteneklerini güvenmek.

Microsoft Azure güvenli, tutarlı bir uygulama platformu ve altyapı-bir hizmet olarak farklı bir bulut skillsets ve tümleşik Veri Hizmetleri ile proje karmaşıklık düzeyi dahilinde çalışmaya ekiplerin sunan yalnızca bulut bilgi işlem sağlayıcısıdır ve çerçeveler ve şirket içi de Azure bulut hizmetlerine içinde dağıtma ile bulut tümleştirme için seçim sağlayan araçlar, hem Microsoft hem de Microsoft olmayan platformlar mevcut olduğunda, veri'ten açığa analytics açın Böylece, şirket içi veri merkezleri. Microsoft güvenilir bulutun bir parçası olarak, müşteriler endüstri lideri güvenlik, güvenilirlik, uyumluluk, gizlilik ve geniş ağ kişiler, iş ortakları ve işlemler kuruluşlar bulutta desteklemek için Azure üzerinde kullanır.

Microsoft Azure ile şunları yapabilirsiniz:

- Bulut ile yenilik hızlandırmak.

- Güç iş kararları & Öngörüler uygulamalarla.

- Ücretsiz olarak oluşturun ve her yerden dağıtın.

- İşlerini koruyun.

## <a name="scope"></a>Kapsam

Bu teknik odak noktası işlemiyle ilgili güvenlik özellikleri ve işlevleri Microsoft Azure'nın temel bileşenleri, yani destekleyen [Microsoft Azure depolama](https://docs.microsoft.com/azure/storage/storage-introduction), [Microsoft Azure SQL veritabanları](https://docs.microsoft.com/azure/sql-database/), [ Microsoft Azure'nın sanal makine modeli](https://docs.microsoft.com/azure/virtual-machines/)ve araçları ve tüm yönetmek altyapı. Bu güvenlik ve gizlilik verilerini koruma konusunda rolleri karşılamak için Microsoft Azure teknik özellikleri müşterilerin kullanımına incelemeyi odaklanır.

Bu paylaşılan sorumluluk modelini anlama önemini buluta taşıma müşteriler için gereklidir. Bulut sağlayıcıları için güvenlik ve uyumluluk çalışmalarını önemli avantajlar sunar, ancak bu avantajlar, kullanıcılar, uygulamaları ve hizmet teklifleri koruma gelen müşteri muaf olmasını değil.

Iaas çözümler için müşteri sorumludur veya güvenli hale getirme ve işletim sistemi, ağ yapılandırması, uygulamalar, kimlik, istemciler ve verilerin yönetilmesi için paylaşılan bir sorumluluğa sahiptir.  PaaS çözümleri derleme Iaas dağıtımlarda müşteri hala sorumludur veya güvenli hale getirme ve uygulamalar, kimlik, istemciler ve verilerin yönetilmesi için paylaşılan bir sorumluluğa sahiptir. SaaS çözümleri, Nonetheless, müşteri sorumlu olmayı sürdürür. Bunlar veri doğru şekilde sınıflandırılır ve bunların kullanıcılar ve son nokta cihazları yönetmek için bir sorumluluk paylaştıkları emin olmalısınız.

Bu belge, Azure Web siteleri, Azure Active Directory, Hdınsight, Media Services ve çekirdek bileşenleri üzerinde katmanlanmış diğer hizmetler gibi ilgili Microsoft Azure platform bileşenleri ayrıntılı kapsamını sağlamaz. Genel bilgiler en düşük düzeyde bulunmakla okuyucular Microsoft tarafından sağlanan ve bu teknik yazıda sağlanan bağlantılar dahil diğer başvuruları açıklandığı gibi Azure temel kavramlarını varsayılır.

## <a name="available-security-technical-capabilities-to-fulfil-user-customer-responsibility---big-picture"></a>Kullanıcı (müşteri) sorumluluk - büyük resmi karşılamak için kullanılabilir güvenlik Teknik Özellikleri

Microsoft Azure, müşterilere yardımcı olmak Hizmetleri güvenlik, gizlilik ve uyumluluk gereksinimlerini sağlar. Aşağıdaki resimde, endüstri standartlarına göre güvenli ve uyumlu uygulama altyapısı oluşturmak kullanıcılar için kullanılabilir çeşitli Azure hizmetlerine açıklanmasına yardımcı olur.

![Kullanılabilir güvenlik teknik özellikleri büyük resmi](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig1.png)

## <a name="manage-and-control-identity-and-user-access-protect"></a>Yönetmek ve denetlemek kimlik ve kullanıcı erişimi (Koru)

Azure, kullanıcı kimliklerini ve kimlik bilgilerini yönetme ve erişimi denetlemek sağlayarak iş ve kişisel bilgilerinizin korunmasına yardımcı olur.

### <a name="azure-active-directory"></a>Azure Active Directory

Microsoft kimlik ve erişim yönetimi çözümlerini Yardım BT doğrulama çok faktörlü kimlik doğrulama ve koşullu erişim gibi ek düzeylerini etkinleştirme uygulamaları ve kaynaklara erişim kurumsal veri merkezi genelinde ve buluta koruma ilkeleri. Raporlama, Denetim ve yardımcı olası güvenlik sorunlarını azaltmak uyarı Gelişmiş Güvenlik ile izleme şüpheli etkinlik. [Azure Active Directory Premium](https://docs.microsoft.com/azure/active-directory/active-directory-editions) çoklu oturum açma bulut binlerce (SaaS) uygulamaları sağlar ve web uygulamaları için şirket içi erişebilirsiniz.

Azure Active Directory (Azure AD) sunduğu güvenlik avantajlarından yeteneğini içerir:

- Oluşturun ve her kullanıcı için tek bir kimliği kullanıcıları, grupları ve aygıtları eşitlenmiş şekilde kalmasının karma kuruluşunuz yönetin.

- Önceden tümleştirilmiş SaaS uygulamaları binlerce de dahil olmak üzere uygulamalarınıza tek oturum açma erişim sağlar.

- Uygulama erişimi güvenliğini kural tabanlı çok faktörlü kimlik doğrulamasını hem şirket içi zorlayarak etkinleştirmek ve bulut uygulamalarında.

- Şirket içi web uygulamaları Azure AD uygulama proxy'si aracılığıyla güvenli uzaktan erişim sağlayın.

[Azure Active Directory portalında](http://aad.portal.azure.com/) kullanılabilir Azure portalında bir parçası. Bu panodan kuruluşunuz durumu özetini almak ve kolayca dizini, kullanıcılar ya da uygulama erişimini yönetme içine daha yakından inceleyin.

![Azure Active Directory](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig2.png)

Çekirdek Azure kimlik yönetimi özellikleri şunlardır:

- Çoklu oturum açma

- Multi-factor authentication

- Güvenlik İzleme, uyarılar ve makine öğrenme tabanlı raporlar

- Tüketici kimliği ve erişim yönetimi

- Cihaz kaydı

- Ayrıcalıklı Kimlik Yönetimi

- Kimlik koruması

#### <a name="single-sign-on"></a>Çoklu oturum açma

[Çoklu oturum açma (SSO)](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/) tüm uygulamaları ve iş, yalnızca bir kez tek bir kullanıcı hesabı kullanarak oturum açma tarafından yapmak için gereken kaynaklar erişebildiklerinden anlamına gelir. Oturum açtıktan sonra tüm gerek olmadan uygulamaları gerekli kimlik doğrulaması için erişebilirsiniz (örneğin, bir parola yazın) ikinci kez.

Birçok kuruluş yazılım son kullanıcının üretkenliği için Office 365, kutusunu ve Salesforce gibi bir hizmet (SaaS) uygulamaları olarak kullanır. Geçmişte, tek tek oluşturun ve her SaaS uygulamasının kullanıcı hesaplarını güncelleştirmek BT personeli gerekli ve kullanıcıların her SaaS uygulaması için bir parola hatırlaması gerekiyordu.

[Azure AD şirket içi Active Directory bulutunu genişletir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis), kendi etki alanına katılmış aygıtlar ve şirket kaynaklarına değil yalnızca birincil kuruluş hesabını kullanmak için etkinleştirme kullanıcılar oturum, ancak aynı zamanda tüm web ve SaaS uygulamaları için gerekli işlerini.

Yalnızca kullanıcıların kullanıcı adları ve parolalar birden çok kümelerini yönetme gerekmez, uygulama erişimini otomatik olarak sağlanan veya XML'deki sağlanan dayalı olarak kuruluş grupları ve durumlarını çalışan olarak olabilir. [Azure AD güvenlik ve erişim yönetimi denetimleri tanıtır](https://docs.microsoft.com/azure/active-directory/active-directory-sso-integrate-saas-apps) SaaS uygulamalarında kullanıcıların erişimini merkezi olarak yönetmenize olanak tanır.

#### <a name="multi-factor-authentication"></a>Multi-factor authentication

[Azure multi-Factor Authentication (MFA)](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) birden fazla doğrulama yöntemi kullanılmasını gerektiren ve kullanıcı oturum açmalarına ve işlemlerine önemli bir ikinci güvenlik katmanı ekleyen kimlik doğrulama yöntemidir. [MFA koruma sağlanmasına yardımcı olan](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-how-it-works) veri ve basit bir oturum açma işlemi için kullanıcı talebine toplantı sırasında uygulamalara erişim. Güçlü kimlik doğrulama seçeneklerini çeşitli aracılığıyla sunar — telefon araması, SMS mesajı veya mobil uygulama bildirimi veya doğrulama kodu ve üçüncü taraf OAuth belirteçleri.

#### <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Güvenlik İzleme, uyarılar ve makine öğrenme tabanlı raporlar

Güvenlik İzleme ve uyarılar ve tutarsız erişim desenlerini tanımlamak makine öğrenme tabanlı raporlar, işinizin korunmasına yardımcı olabilir. Bütünlük ve kuruluşunuzun dizininin güvenlik görünürlük elde etmek için Azure Active Directory'nin erişim ve kullanım raporlarını kullanabilirsiniz. Bu bilgileri kullanarak bir dizin yönetici böylece bunlar bu riskleri azaltmak yeterli planlayabilirsiniz olası güvenlik riskleri burada bulunan daha iyi belirleyebilirsiniz.

Azure portal veya aracılığıyla [Azure Active Directory portalında](http://aad.portal.azure.com/), [raporları](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-guide) aşağıdaki yollarla kategorilere ayrılır:

- Anomali raporları – oturum açma anormal olarak bulduk olaylarını içerir. Amacımız, bu tür etkinlik farkında olun ve bir olay şüpheli olup hakkında karar kullanabilmek etkinleştirmeniz olmaktır.

- Tümleşik uygulama raporları – bulut uygulamalarını, kuruluşunuzda nasıl kullanıldığını içine Öngörüler sağlar. Azure Active Directory bulut uygulamalarını binlerce ile tümleştirme sağlar.

- Hata raporlarını – dış uygulamalara hesap sağlamada oluşabilecek hatalar gösterir.

- Kullanıcıya özgü raporları – belirli bir kullanıcı için etkinlik verilerdeki aygıt/oturum görüntüler.

- Etkinlik günlükleri – son 24 saat, son 7 gün veya son 30 gün ve Grup etkinlik değişikliklerini ve parola sıfırlama ve kayıt etkinlik tüm Denetlenen olayları kaydını içerir.

#### <a name="consumer-identity-and-access-management"></a>Tüketici kimliği ve erişim yönetimi

[Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) yüz milyonlarca kimlikleri için ölçeklendirilebilen bir yüksek oranda kullanılabilir, genel Identity management hizmeti tüketiciye yönelik uygulamalar için. Bu hizmet mobil platformlar ve web platformlarıyla tümleştirilebilir. Tüketicileriniz, ister mevcut sosyal hesaplarını kullanarak ister yeni kimlik bilgileri oluşturarak özelleştirilebilir bir deneyimle uygulamalarınızda oturum açabilir.

İsteyen son, uygulama geliştiricileri de [kaydolun ve tüketici oturumlarını](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview) uygulamalarına kendi kodlarını yazardı. Ayrıca, kullanıcı adları ile parolaları depolamak için şirket içi veritabanlarını veya sistemleri kullanırlardı. Kuruluşunuz Azure Active Directory B2C tüketici kimlik yönetimini uygulamalarıyla güvenli, standartlara dayalı bir platform ve çok sayıda ilkeler yardımıyla bütünleştirmek için daha iyi bir yol sunar.

Azure Active Directory B2C kullandığınızda tüketicileriniz uygulamalarınız için var olan sosyal hesaplarını (Facebook, Google, Amazon, LinkedIn) kullanarak veya yeni kimlik bilgileri (e-posta adresi ve parola veya kullanıcı adı ve parola) oluşturarak kaydolabilir.

#### <a name="device-registration"></a>Cihaz kaydı

[Azure AD cihaz kaydı](https://docs.microsoft.com/azure/active-directory/device-management-introduction) temelidir aygıt tabanlı [koşullu erişim](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup) senaryoları. Bir cihaz kaydedildiğinde Azure AD cihaz kaydı kullanıcı oturum açtığında cihazın kimliğini doğrulamak için kullanılan bir kimliğe sahip cihazı sağlar. Kimliği doğrulanmış cihaz ve cihaz öznitelikleri böylece bulutta ve şirket içinde barındırılan uygulamalar için koşullu erişim ilkelerini zorlamak üzere kullanılabilir.

İle birlikte kullanıldığında bir [mobil cihaz Yönetimi (MDM)](https://www.microsoft.com/itshowcase/Article/Content/588/Mobile-device-management-at-Microsoft) çözüm gibi Intune, Azure Active Directory'deki cihaz öznitelikleri cihaz hakkındaki ek bilgilerle güncelleştirilir. Bu durum, güvenlik ve uyumluluğa yönelik standartlarınızı karşılamak için cihazlardan erişimi zorlayan koşullu erişim kuralları oluşturmanıza olanak sağlar.

#### <a name="privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi

[Azure Active Directory (AD) Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) yönetmenize, denetlemenize ve ayrıcalıklı kimliklerinizi izlemek ve Azure AD'de de gibi diğer Microsoft online services Office 365 veya Microsoft Intune gibi kaynaklarına erişim sağlar.

Bazen kullanıcıların Azure veya Office 365 kaynakları veya diğer SaaS uygulamaları ayrıcalıklı işlemleri gerçekleştirmek gerekir. Bu, genellikle bunları Azure AD'de kalıcı ayrıcalıklı erişim vermek kuruluş sahip anlamına gelir. Kuruluşlar, yeterince yönetici ayrıcalıklarını bu kullanıcıların ne yaptıklarını izleyemez bulutta barındırılan kaynaklar için büyüyen bir güvenlik riski olmasıdır. Ayrıca, ayrıcalıklı erişimi olan bir kullanıcı hesabı ihlal edilmesi durumunda, bir ihlali, genel bulutun güvenlik etkileyebilir. Azure AD Privileged Identity Management bu riski gidermeye yardımcı olur.

Azure AD Privileged Identity Management sağlar:

- Hangi kullanıcıların Azure AD admins olduğuna bakın

- İsteğe bağlı, "Microsoft Online Services yönetim erişimi, Office 365 ve Intune gibi tam zamanında" etkinleştirme

- Yönetici erişim geçmişine ve değişiklikler hakkında raporlar yönetici atamaları alma

- Ayrıcalıklı bir rol için erişim hakkında uyarı alın

#### <a name="identity-protection"></a>Kimlik koruması

[Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) , birleştirilmiş görünüme risk olaylarına ve olası güvenlik açıklarını kuruluşunuzdaki kimlikleri etkileyen sağlayan bir güvenlik hizmetidir. Kimlik koruma, var olan Azure Active Directory'nin anomali algılama özellikleri (Azure AD anormal etkinlik raporları kullanılabilir) kullanır ve anormallikleri gerçek zamanlı olarak algılayabilir yeni risk olayı türleri sunar.

## <a name="secured-resource-access-in-azure"></a>Azure'da güvenli kaynak erişimi

Azure erişim denetimi faturalandırma açısından başlatır. Ziyaret ederek erişilen bir Azure hesabı sahibinin [Azure hesap Merkezi](https://account.windowsazure.com/subscriptions), Hesap Yöneticisi'nin (AA) değil. Abonelik faturalama için bir kapsayıcı olsa da, bunlar aynı zamanda bir güvenlik sınırı hareket: her abonelik bir Hizmet Yöneticisi (kimin eklemek, kaldırmak ve Azure portalını kullanarak bu abonelik Azure kaynaklarında değiştirme SA) sahiptir. Yeni bir aboneliğin varsayılan SA AA olur ancak AA Azure hesap Merkezi'nde SA değiştirebilirsiniz.

![Azure'da güvenli kaynak erişimi](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig3.png)

Abonelikler, bir dizin ile bir ilişkilendirme de vardır. Dizin, bir kullanıcı kümesini tanımlar. Bu iş veya Okul dizini oluşturulan kullanıcılardan veya dış kullanıcıların (yani, Microsoft Accounts) olabilir. Abonelikler, bir alt kümesini Hizmet Yöneticisi (SA) veya ortak yönetici (CA) olarak atanmış olan dizin kullanıcılar tarafından erişilebilir; eski nedeniyle, Microsoft Accounts (eskiden Windows Live kimliği) SA veya CA dizinde bulunduğundan olmadan atanabilir, yalnızca istisnadır.

Güvenlik odaklı şirketler çalışanlar gereksinim duydukları izinleri tam vermiş odaklanmanız gerekir. Çok fazla izinler saldırganlar bir hesaba getirebilir. Çok az izinleri çalışanlar verimli bir şekilde işlerini alınamıyor anlamına gelir. [Azure rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview) yardımcı olur, Azure için ayrıntılı erişim yönetimi sunarak bu sorunu gidermek.

![Güvenli kaynak erişimi ](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig4.png)

RBAC kullanarak, ekibiniz içinde görevleri kurabilmeleri ve işlerini yapmak için gereksinim duydukları kullanıcılara sadece erişim miktarını verebilirsiniz. Kısıtlanmamış izinlerini herkes vermek yerine, Azure aboneliği veya kaynakları yalnızca belirli eylemleri izin verebilirsiniz. Örneğin, bir çalışan başka bir SQL veritabanları aynı abonelik içindeki yönetebilirsiniz sırada bir Abonelikteki sanal makineleri yönetme izin vermek için RBAC kullanın.

![Güvenli kaynak erişimi Azure (RBAC)](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig5.png)

## <a name="azure-data-security-and-encryption-protect"></a>Azure veri güvenliği ve şifreleme (koruma)

Veri koruma bulutta anahtarlarından birini verilerinizi ortaya çıkabilecek ve bu durum için hangi denetimlerin kullanılabilir olası durumlar için hesap. Azure veri güvenlik ve şifreleme için en iyi yöntemler önerileri aşağıdaki verilerinin durumları olabilir.

- : Çalışmıyorken Bu depolama nesneleri, kapsayıcıları ve statik olarak fiziksel medyada mevcut türleri manyetik veya optik disk olması tüm bilgileri içerir.

- Aktarım sırasında: Ne zaman veri bileşenleri, konumlara veya ağ bir hizmet veri yolu (Başlangıç, bulut şirket içi ve ExpressRoute gibi karma bağlantılar dahil olmak üzere tersi,) programları gibi üzerinde arasında ya da bir giriş/çıkış işlemi sırasında aktarıldığı , da olduğu düşünülen hareket halinde.

### <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme

Bekleyen şifreleme elde etmek için her birini yapın:

Aşağıdaki tabloda ayrıntılı verileri şifrelemek için önerilen şifreleme modelleri en az biri destekler.

| Şifreleme modeli |  |  |  |
| ----------------  | ----------------- | ----------------- | --------------- |
| Sunucu şifreleme | Sunucu şifreleme | Sunucu şifreleme | İstemci şifreleme
| Sunucu tarafı şifreleme hizmeti yönetilen anahtarları kullanma | Azure anahtar kasası Customer-Managed tuşlarını kullanarak sunucu tarafı şifreleme | Şirket içi müşteri kullanarak sunucu tarafı şifreleme anahtarları yönetilen |
| • Azure kaynak sağlayıcıları şifreleme ve şifre çözme işlemi gerçekleştirir <br> • Microsoft anahtarları yönetir <br>• Tam bulut işlevi | • Azure kaynak sağlayıcıları şifreleme ve şifre çözme işlemi gerçekleştirir<br>• Müşteri anahtarları Azure anahtar kasası aracılığıyla denetler<br>• Tam bulut işlevi | • Azure kaynak sağlayıcıları şifreleme ve şifre çözme işlemi gerçekleştirir <br>• Müşteri anahtarları şirket içi denetler <br> • Tam bulut işlevi| • Azure Hizmetleri şifresi çözülmüş verileri göremezsiniz <br>• Müşterilerin anahtarlarını şirket içi tutun (veya diğer depoları güvenli). Anahtarları Azure hizmetlerine kullanılabilir değil <br>• Azaltılmış bulut işlevi|

### <a name="enabling-encryption-at-rest"></a>Bekleyen şifreleme etkinleştirme

**Tüm konumlarını depoları verilerinizi belirle**

Tüm verileri şifrelemek için şifreleme REST belirtilir. Bunun yapılması, önemli veri ya da tüm kalıcı konumları eksik olasılığını ortadan kaldırır. Uygulamanız tarafından depolanan tüm verileri sıralar. 

> [!Note] 
> Yalnızca "uygulama verileri" veya "PII' ancak meta veriler (abonelik eşlemeleri, sözleşme bilgileri PII) dahil olmak üzere uygulama için ilgili herhangi bir veri hesap.

Veri depolamak için kullandığınız hangi depoları göz önünde bulundurun. Örneğin:

- Harici depolama (örneğin, SQL Azure, belge DB, Hdınsights, Data Lake, vb.)

- Geçici depolama (Kiracı verileri içeren tüm yerel önbelleği)

- Bellek içi önbellek (sayfa dosyasına konmuş.)

### <a name="leverage-the-existing-encryption-at-rest-support-in-azure"></a>Azure rest desteği varolan şifreleme yararlanın

Kullandığınız her mağaza için Rest destek varolan şifreleme yararlanın.

- Azure Storage: Bkz [bekleyen veri için Azure Storage hizmeti şifreleme](https://docs.microsoft.com/azure/storage/storage-service-encryption),

- SQL Azure: Bkz [saydam veri şifreleme (TDE), her zaman şifreli SQL](https://msdn.microsoft.com/library/mt163865.aspx)

- VM & yerel disk depolama ([Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption))

VM ve yerel disk depolama için Azure Disk şifrelemesi kullanan desteklendiği yerlerde:

#### <a name="iaas"></a>IaaS

Iaas VM'ler (Windows veya Linux) hizmetleriyle kullanması gereken [Azure Disk şifrelemesi](https://microsoft.sharepoint.com/teams/AzureSecurityCompliance/Security/SitePages/Azure%20Disk%20Encryption.aspx) müşteri verilerini içeren birimler şifrelemek için.

#### <a name="paas-v2"></a>PaaS v2

Çalışan hizmetleri üzerinde PaaS v2 Service Fabric kullanan Azure disk şifrelemesi sanal makine ölçek kümesi için [VMSS] PaaS v2 Vm'leri şifrelemek için kullanabilirsiniz.

#### <a name="paas-v1"></a>PaaS v1

Azure Disk şifrelemesi PaaS v1 üzerinde şu anda desteklenmiyor. Bu nedenle, kalan kalıcı verileri şifrelemek için uygulama düzeyinde şifreleme kullanmanız gerekir.  Bu, içerir, ancak uygulama verilerini, geçici dosyaları, günlükler ve kilitlenme bilgi dökümleri için sınırlı değildir.

Çoğu Hizmetleri depolama kaynak sağlayıcısı şifreleme özelliğinden denemelisiniz. Bazı hizmetler açık şifreleme yapmanız gerekir, örneğin, herhangi bir anahtar malzemesi kalıcı (Sertifikalar, kök / ana anahtarları) anahtar kasasında depolanan gerekir.

Hizmet tarafı şifreleme müşteri tarafından yönetilen anahtarlarla destekliyorsa var. bize anahtarını almak müşteri için bir yol olması gerekir. Azure anahtar kasası (AKV) ile tümleştirerek Bunu yapmak için desteklenen ve önerilen yolu. Bu durumda müşteriler ekleyin ve bunların anahtarları Azure anahtar Kasası'yönetin. Bir müşteri AKV aracılığıyla kullanılacağını öğrenebilirsiniz [anahtar kasası ile çalışmaya başlama](http://go.microsoft.com/fwlink/?linkid=521402).

Azure anahtar kasası ile tümleştirmek için bir anahtar şifre çözme için gerektiğinde AKV istek için kodu ekleyin.

- Bkz: [Azure anahtar kasası – adım adım](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step/) AKV ile tümleştirme hakkında bilgi için.

Yönetilen müşteri anahtarları destekliyorsa, hangi anahtar kasası (veya anahtar kasası URI) kullanılacağını belirtmek müşteri UX sağlamanız gerekir.

Konak, altyapı ve Kiracı verileri, sistem hatası nedeniyle anahtarları kaybı şifrelenmesi bekleyen şifreleme içerir veya kötü amaçlı etkinliği tüm şifrelenmiş verileri anlamına gelebilir kaybolur. Bu nedenle, Rest çözüm, şifreleme sistem hataları ve kötü amaçlı etkinliği için esnek bir kapsamlı olağanüstü durum kurtarma Öykü olduğunu kritik önem taşır.

Bekleyen şifreleme uygulama hizmetleri genellikle şifreleme anahtarları hala açıktır veya bırakılıyor veri sürücüsünde ana bilgisayar (örneğin, sayfa dosyası konak işletim sistemi.) şifrelenmemiş Bu nedenle, hizmetleri, hizmetler için konak birimi şifrelenir emin olmalısınız. Bu işlem kolaylaştırmak için takım kullandığı ana şifreleme dağıtımını etkinleştirilmiş [Bitlocker](https://technet.microsoft.com/library/dn306081.aspx) NKP ve DCM hizmeti ve konak birimi şifrelemek için aracı uzantıları.

Çoğu hizmetler standart Azure Vm'lerinde uygulanır. Bu tür hizmetlerin almalısınız [ana şifreleme](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) otomatik olarak zaman işlem etkinleştirir. İşlem içinde çalışan hizmetler için yönetilen kümeleri ana şifreleme Windows Server 2016 toplu olarak otomatik olarak etkinleştirilir.

### <a name="encryption-in-transit"></a>Şifreleme aktarım sırasında

Aktarımdaki verileri koruma temel veri koruma stratejinizin parçası olmalıdır. Verileri geri ve İleri birçok konumlardan geçtiğinden genel, her zaman SSL/TLS protokolleri farklı konumlar arasında veri değişimi için kullanmanız önerilir. Bazı durumlarda, şirket içi ve bulut arasındaki tüm iletişim kanalını ayırmak isteyebilirsiniz sanal özel ağ (VPN) kullanarak altyapı.

Şirket içi altyapınızı ve Azure arasında taşıma verileri için HTTPS veya VPN gibi uygun güvenlik önlemleri göz önünde bulundurmalısınız.

Azure birden çok iş istasyonlarında bulunan şirket içi erişimi güvenli hale getirmek için gereken kuruluşlar için kullanmak [Azure siteden siteye VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-site-to-site-create).

Azure bulunan bir iş istasyonu şirket içi erişimi güvenli hale getirmek için gereken kuruluşlar için kullanmak [noktası siteye VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-point-to-site-create).

Büyük veri kümeleri taşınabilir ayrılmış bir yüksek hızlı WAN bağlantısı üzerinden gibi [ExpressRoute](https://azure.microsoft.com/services/expressroute/). ExpressRoute kullanmayı seçerseniz, ayrıca uygulama düzeyi kullanarak verileri şifreleyebilir [SSL/TLS](https://support.microsoft.com/kb/257591) veya diğer protokoller için ek koruma.

Azure Portalı aracılığıyla Azure Storage ile etkileşim, tüm işlemleri HTTPS oluşur. [Storage REST API'sini](https://msdn.microsoft.com/library/azure/dd179355.aspx) HTTPS de kullanılabilir ile etkileşim kurmasına üzerinden [Azure Storage](https://azure.microsoft.com/services/storage/) ve [Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/).

Aktarımdaki verileri korumak için başarısız olan kuruluşlar için daha açıktır [man-in--middle saldırıları](https://technet.microsoft.com/library/gg195821.aspx), [gizli dinleme](https://technet.microsoft.com/library/gg195641.aspx)ve oturumu ele geçirme. Bu tür saldırıları gizli verilere erişmesini ilk adımı olabilir.

Azure VPN seçeneği hakkında daha fazla makalesini okuyarak bilgi [planlama ve tasarım VPN ağ geçidi için](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design).

### <a name="enforce-file-level-data-encryption"></a>Dosya düzeyinde veri şifrelemeyi zorunlu kılma

[Azure RMS](https://technet.microsoft.com/library/jj585026.aspx) dosyalarınızın ve e-posta güvenli hale getirmek için şifreleme, kimlik ve yetkilendirme ilkelerini kullanır. Azure RMS birden çok cihazda çalışır — telefonları, Tablet ve PC'leri hem kuruluşunuz içinde hem de kuruluşunuz dışındaki koruma tarafından. Azure RMS bile, kuruluşunuzun sınırları dışına çıktığında veriler devam koruma düzeyini eklediğinden bu olası bir yetenektir.

Dosyalarınızı korumak için Azure RMS kullandığınızda, endüstri standardı şifreleme ile tam destek kullanmakta olduğunuz [FIPS 140-2](http://csrc.nist.gov/groups/STM/cmvp/standards.html). Veri koruma için Azure RMS yararlanan, denetimi altında olmayan depolama kopyaladığınız olsa dahi koruma dosyayla beraber kalır güvence sahip bir bulut depolama hizmeti gibi BT'nin. Aynı oluştuğunda e-posta ile paylaşılan dosyaları dosya yönergeleri içeren bir e-posta eki olarak korunur korumalı ekin nasıl açılacağına dair.
Azure RMS benimseme için planlama yaparken şunları öneririz:

- Yükleme [RMS sharing uygulaması](https://technet.microsoft.com/library/dn339006.aspx). Bu uygulama tarafından bir Office Eklentisi-kullanıcıların kolayca dosyaları doğrudan koruyabilmeniz için Office uygulamalarıyla ile tümleşir.

- Uygulamaları ve Hizmetleri Azure RMS'yi destekleyecek şekilde yapılandırma

- Oluşturma [özel şablonlar](https://technet.microsoft.com/library/dn642472.aspx) iş gereksinimlerinizi yansıtır. Örneğin: tüm üst gizliliği uygulanması gereken üst gizli veriler için bir şablon ilgili e-postaları.

Üzerinde zayıf kuruluşlar [veri sınıflandırması](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) ve dosya koruması veri sızıntısını daha açıktır. Uygun dosya koruma kuruluşlar iş Öngörüler elde, uygunsuz kullanım için izleyebilir ve kötü amaçlı erişimi engellemek için dosyaları mümkün olmayacaktır.

> [!Note]
> Azure RMS hakkında daha fazla makalesini okuyarak bilgi [Azure Rights Management ile çalışmaya başlama](https://technet.microsoft.com/library/jj585016.aspx).

## <a name="secure-your-application-protect"></a>Uygulamanızı güvenlik altına (koruma)
Azure altyapı ve uygulamanızın üzerinde çalıştığı platform güvenliğini sağlamak için sorumlu olsa da, bu, uygulamanızın güvenliğini sağlamak için sorumluluğundadır. Diğer bir deyişle, geliştirme, dağıtma ve uygulama kodu ve içerik güvenli bir şekilde yönetmek gerekir. Bu, uygulama kodu veya içeriği hala tehditlerine karşı savunmasız olabilir.

### <a name="web-application-firewall-waf"></a>Web uygulaması güvenlik duvarı (WAF)
[Web uygulaması Güvenlik Duvarı (WAF)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) özelliğidir [uygulama ağ geçidi](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) web uygulamalarınızdan ortak açığından yararlanma girişimi ve güvenlik açıkları merkezi korunmasını sağlar.

Web uygulaması güvenlik duvarı bu işlemi [OWASP çekirdek kural kümeleri](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) 3.0 veya 2.2.9’daki kurallara göre yapar. Web uygulamaları, bilinen yaygın güvenlik açıklarından yararlanan kötü amaçlı saldırıların giderek daha fazla hedefi olmaktadır. Bu açıklardan yararlanma örnekleri arasında SQL ekleme saldırıları, siteler arası komut dosyası saldırıları yaygındır. Uygulama kodunda bu tür saldırıların önlenmesi zor olabilir ve uygulama topolojisinin birden fazla katmanında ayrıntılı bakım, düzeltme eki uygulama ve izleme işlemleri gerektirebilir. Merkezi bir web uygulaması güvenlik duvarı, güvenlik yönetimini çok daha kolay hale getirir ve yetkisiz erişim ya da izinsiz giriş tehditlerine karşı uygulama yöneticilerine daha iyi güvence verir. Bir WAF çözümü, bilinen bir güvenlik açığına merkezi bir konumda düzeltme eki uygulayarak güvenlik tehdidine karşı, web uygulamalarının her birinin güvenliğini sağlamaya göre daha hızlı tepki verebilir. Var olan uygulama ağ geçitleri, web uygulaması güvenlik duvarı bulunan bir uygulama ağ geçidine kolaylıkla dönüştürülebilir.

Web uygulaması güvenlik duvarının koruma sağladığı bazı yaygın web güvenlik açıkları şunlardır:

- SQL ekleme koruması

- Siteler arası komut dosyası koruması

- Komut ekleme, HTTP isteği kaçakçılığı, HTTP yanıtı bölme ve uzak dosya ekleme saldırıcı gibi Yaygın Web Saldırıları Koruması

- HTTP protokolü ihlallerine karşı koruma

- Eksik konak kullanıcısı-aracısı ve kabul üst bilgileri gibi HTTP protokolü anormalliklerine karşı koruma

- Robotlar, gezginler ve tarayıcıları önleme

- Ortak uygulama yapılandırma hataları (diğer bir deyişle, Apache, IIS, vb.) algılanması

> [!Note]
> Aşağıdaki kuralları daha ayrıntılı bir listesi ve bunların korumaları bakın için [çekirdek kural kümeleri](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview#core-rule-sets):

Azure uygulamanız için hem gelen hem de giden trafiği güvenli hale getirmek için birkaç kullanımı kolay özellikler de sağlar. Azure de dışarıdan sağlayarak müşteriler kendi uygulama kodu güvenli yardımcı olur, web uygulamanızın güvenlik açıkları için taramak üzere işlevi sağlıyordu.

- [Uygulamanız için Azure Active Directory kimlik doğrulaması kurulumu](https://azure.microsoft.com/blog/azure-websites-authentication-authorization/)

- [Aktarım Katmanı Güvenliği (TLS/SSL) - HTTPS etkinleştirerek uygulamanızı trafiğinin güvenliğini](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-ssl)

  - [Tüm gelen trafiği HTTPS bağlantısı üzerinden zorla](http://microsoftazurewebsitescheatsheet.info/)

  - [Katı taşıma güvenliği (HSTS) etkinleştir](http://microsoftazurewebsitescheatsheet.info/#enable-http-strict-transport-security-hsts)

- [İstemcinin IP adresine göre uygulamanıza erişimi kısıtlama](http://microsoftazurewebsitescheatsheet.info/#filtering-traffic-by-ip)

- [İstemcinin davranıştan - istek sıklığı ve eşzamanlılık uygulamanıza erişimi kısıtlama](http://microsoftazurewebsitescheatsheet.info/#dynamic-ip-restrictions)

- [Web uygulaması kodunuza Tinfoil Security Scanning kullanarak güvenlik açıkları için tarama](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/)

- [Web uygulamanıza bağlanmak için istemci sertifikaları gerektirmek için TLS karşılıklı kimlik doğrulamasını yapılandırma](https://docs.microsoft.com/azure/app-service/app-service-web-configure-tls-mutual-auth)

- [Uygulamanızı güvenli dış kaynaklara bağlanmak için kullanılması için bir istemci sertifikası yapılandırma](https://azure.microsoft.com/blog/using-certificates-in-azure-websites-applications/)

- [Uygulamanızı sağlayan gelen araçları önlemek için standart sunucu başlıkları Kaldır](https://azure.microsoft.com/blog/removing-standard-server-headers-on-windows-azure-web-sites/)

- [Uygulamanızı noktadan siteye VPN kullanarak özel bir ağdaki kaynaklara güvenli bir şekilde bağlanın](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet)

- [Uygulamanızı karma bağlantılar kullanarak özel bir ağdaki kaynaklara güvenli bir şekilde bağlanın](https://docs.microsoft.com/azure/app-service/app-service-hybrid-connections)

Azure uygulama hizmeti Azure Cloud Services ve sanal makineler tarafından kullanılan kötü amaçlı yazılımdan koruma çözümünü kullanır. Bilgi edinmek için bu hakkında daha fazla başvurmak bizim [kötü amaçlı yazılımdan koruma belgelerine](https://docs.microsoft.com/azure/security/azure-security-antimalware).

## <a name="secure-your-network-protect"></a>Ağınızın güvenliğini sağlamaya (koruma)
Microsoft Azure uygulama ve hizmet bağlantı gereksinimlerini desteklemek için sağlam bir ağ altyapısı içerir. Azure'da, şirket içi arasında bulunan kaynaklar arasındaki ağ bağlantısı mümkündür ve Azure barındırılan kaynakları ve kitaplığa ve Internet ve Azure.

[Azure ağ altyapısı](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-networking-guidelines) güvenli bir şekilde Azure kaynaklarını birbiriyle bağlanmanıza olanak sağlayan [sanal ağlar (Vnet'ler)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview). Bir VNet kendi ağ bulutta gösterimidir. Bir VNet Azure bulut ağ aboneliğinize adanmış mantıksal bir yalıtım ' dir. Şirket içi ağlarınız sanal ağlara bağlanabilir.

![Ağınızın güvenliğini sağlamaya (koruma)](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig6.png)

Temel ağ düzeyinde erişim denetimi (IP adresini ve TCP veya UDP protokollerini temel alınarak) ihtiyacınız sonra kullanabileceğiniz [ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg). Ağ güvenlik grubu (NSG) temel bir durum bilgisi olan paket güvenlik duvarı filtreleme olduğunu ve temelinde erişimi denetlemenize olanak sağlayan bir [5-tanımlama grubu](https://www.techopedia.com/definition/28190/5-tuple).

Azure ağı, Azure sanal ağlarda ağ trafiğini yönlendirme davranışını özelleştirme yeteneği destekler. Bunu yapılandırarak yapmak [kullanıcı tanımlı rotaları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) azure'da.

[Zorlanan tünel](https://www.petri.com/azure-forced-tunneling) hizmetlerinizi Internet'teki cihazlar için bir bağlantı başlatmak için izin verilmiyor emin olmak için kullanabileceğiniz bir mekanizmadır.

Azure destekler, şirket içi ağınız ve Azure sanal ağı ile WAN bağlantısının bağlantı ayrılmış [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction). Azure ve sitenizi arasındaki bağlantıyı genel Internet üzerinden geçmez adanmış bir bağlantı kullanır. Azure uygulamanız birden çok veri merkezlerinde çalıştığından, kullanabileceğiniz [Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) akıllıca uygulama örnekleri arasında kullanıcılardan rota isteklerine. Ayrıca, Internet'ten erişilebilir olması durumunda Azure üzerinde çalışmayan hizmetlerine trafiğini yönlendirebilir.

## <a name="virtual-machine-security-protect"></a>Sanal makine güvenliği (koruma)

[Azure sanal makineleri](https://docs.microsoft.com/azure/virtual-machines/) çözümleri Çevik bir şekilde bilgi işlem çok çeşitli dağıtmanıza olanak tanır. Microsoft Windows, Linux, Microsoft SQL Server, Oracle, IBM, SAP ve Azure BizTalk Hizmetleri desteği sayesinde, istediğiniz iş yükünü istediğiniz dilde ve neredeyse tüm işletim sistemlerinde dağıtabilirsiniz.

Azure ile kullandığınız [kötü amaçlı yazılımdan koruma yazılımı](https://docs.microsoft.com/azure/security/azure-security-antimalware) Microsoft, Symantec, eğilim mikro ve Kaspersky sanal makinelerinizi kötü amaçlı dosyalar, reklam ve diğer tehditlere korumaya gibi güvenlik satıcılardan.

Azure Cloud Services ve sanal makineleri için Microsoft Antimalware tanımlamak ve virüsler, casus yazılım ve diğer kötü amaçlı yazılım kaldırma yardımcı olan gerçek zamanlı koruma bir özelliktir. Microsoft Antimalware kötü amaçlı veya istenmeyen yazılım kendini yüklemeye veya Azure sistemleriniz üzerinde çalışmaya girişiminde bilinen yapılandırılabilir uyarır sağlar.

[Azure yedekleme](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) sıfır sermaye yatırımı ve en az uygulama verilerinizi korur ölçeklenebilir bir çözüm işletim maliyetlerini. Uygulama hataları verilerinizi bozabilir ve insan hataları, uygulamalarınızda hatalar oluşturabilir. Azure Backup ile Windows ve Linux çalıştıran sanal makinelerinizi korunur.

[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) yardımcı olur, çoğaltma, yük devretme ve kurtarma iş yükleri ve uygulamalar, birincil konumunuzun çökmesi durumunda ikincil konum kullanılabilir; böylece düzenlemek.

## <a name="ensure-compliance-cloud-services-due-diligence-checklist-protect"></a>Uyumluluk sağlamak: Bulut hizmetlerini son tespitlerini denetim listesi (koruma)

Geliştirilmiş Microsoft [bulut Hizmetleri son Tespitlerini denetim](https://aka.ms/cloudchecklist.download) çalışma kuruluşlara yardımcı olmak için durum tespitlerini gibi bunlar buluta taşımayı dikkate alın. Tüm boyutunu ve türünü, bir kuruluş için bir yapı sağlar — özel işletmelerin ve kamu tüm düzeyleri ve kar amacı gütmeyen kuruluşlar dahil, ortak kesimli kuruluşlar — kendi performansını, hizmet, veri yönetimi ve idare hedefler tanımlayacak ve gereksinimleri. Bu farklı bulut hizmeti sağlayıcıları, sonuçta bir bulut hizmet sözleşmesi için temel oluşturan teklifleri karşılaştırmanızı sağlar.

Denetim listesi yan tümcesi by yan için yeni bir uluslararası standart bulut hizmet sözleşmelerini, ISO/IEC 19086 ile hizalar bir çerçeve sağlar. Bu standart, kuruluşların, bulut hizmet teklifleri karşılaştırma için bir ortak plan oluşturma ve bulut benimseme hakkında kararlar yardımcı konuları birleştirilmiş bir dizi sunar.

Denetim listesi, bulut hizmet sağlayıcısı seçme için yapılandırılmış yönerge ve tutarlı, tekrarlanabilir bir yaklaşım sağlayarak bulut için kapsamlı vetted taşıma yükseltir.

Bulut benimseme artık yalnızca bir teknoloji bir karardır. Denetim listesi gereksinimleri kuruluş her yönüyle touch çünkü bunlar tüm anahtar iç karar-alıcılar komitesini bir araya topla sunar — CIO ve CISO yanı sıra yasal, risk yönetimi, tedarik ve uyumluluk uzmanları. Bu karar verme işlemi ve benimseme için getirerek beklenmeyen roadblocks olasılığını azaltır ses mantığı yerdeki kararlarında verimliliğini artırır.

Buna ek olarak, Denetim listesi:

- Bulut benimseme işleminin başında karar verenler için anahtar tartışma konularını gösterir.

- Düzenlemelere ve kuruluşun kendi hedefleri hakkında kapsamlı iş tartışmalara gizlilik, kişisel bilgileri (PII) ve veri güvenliği için destekler.

- Kuruluşların bir bulut projesi etkileyebilecek olası sorunları tanımlamaya yardımcı olur.

- Aynı şartları, tanımları, ölçümleri ve farklı bir bulut hizmet sağlayıcılarının sunduğu karşılaştırma işlemini basitleştirmek için her sağlayıcı için sonuçlara sorular, tutarlı bir kümesini sağlar.

## <a name="azure-infrastructure-and-application-security-validation-detect"></a>Azure altyapı ve uygulama güvenlik doğrulaması (Algıla)

[Azure işlem güvenliği](https://docs.microsoft.com/azure/security/azure-operational-security) verilerini, uygulamaları ve diğer Microsoft Azure varlıkları korumak için Hizmetleri, denetimleri ve kullanıcılar için kullanılabilir özellikleri gösterir.

![Güvenlik doğrulaması (Algıla)](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig7.png)

Azure işlem güvenliği Microsoft Security Development Lifecycle (SDL), Microsoft Güvenlik Yanıt Merkezi programı da dahil olmak üzere Microsoft'a özgü çeşitli özellikleri aracılığıyla elde edilen bilgilerden içerir çerçevesi üzerine inşa edilmiştir ve siber güvenlik tehdit derin farkındalığınızı.

### <a name="microsoft-operations-management-suiteoms"></a>Microsoft operations management suite(OMS)

[Microsoft Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) karma bulut BT yönetimi çözümüdür. Tek başına kullanıldığında veya var olan System Center dağıtımınızı genişletmek için OMS, en fazla esneklik ve denetim için bulut tabanlı yönetim altyapınızın sunar.

![Microsoft operations management suite(OMS)](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig8.png)

OMS ile şirket içi, Azure, AWS, Windows Server, Linux, VMware ve OpenStack, rekabet çözümleri daha düşük bir maliyetle de dahil olmak üzere tüm bulut örnek yönetebilirsiniz. Bulut ilk dünya için yerleşik, OMS kuruluşunuzu yeni iş zorlayan ve yeni iş yükleri, uygulamaları ve bulut ortamlarını uyum sağlamak için diğer bir deyişle hızlı ve en düşük maliyetli şekilde yönetme için yeni bir yaklaşım sunmaktadır.

### <a name="log-analytics"></a>Log Analytics

[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics), yönetilen kaynaklardan toplanan verileri merkezi bir depoda birleştirerek OMS için izleme hizmetleri sağlar. Bu verilere olaylar, performans verileri ya da API aracılığıyla sağlanan özel veriler dahil olabilir. Toplanan veriler uyarı, analiz ve dışarı aktarma için kullanılabilir hale gelir.

![Log Analytics](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig9.png)

Bu yöntem, çeşitli kaynaklardan verilerini bir araya olanak tanır, araya getirebilirsiniz şekilde varolan ile Azure hizmetlerinizi verilerden ortamı şirket. Ayrıca, veri toplama işlemini veriler üzerinde gerçekleştirilen eylemden ayırarak tüm eylemlerin her tür veri üzerinde kullanılabilmesini mümkün kılar.

### <a name="azure-security-center"></a>Azure Güvenlik Merkezi

[Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro), Azure kaynaklarınızın güvenliğine yönelik artırılmış görünürlük ve denetim yoluyla tehditleri engellemenize, algılamanıza ve yanıtlamanıza yardımcı olur. Aboneliklerinizde, tümleşik güvenlik izleme ve ilke yönetimi sağlar; normal koşullarda gözden kaçabilecek tehditleri algılamaya yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle çalışır.

Güvenlik Merkezi, olası güvenlik açıklarını tanımlamak için Azure kaynaklarınızın güvenlik durumunu inceler. Gerekli denetimlerin yapılandırılması işlemi boyunca bir öneri listesi size rehberlik eder.

Örneklere şunlar dahildir:

- Kötü amaçlı yazılımı tanımlama ve kaldırmada yardım etmesi için kötü amaçlı yazılımdan koruma yazılımı hazırlama

- Ağ güvenlik gruplarını ve kurallarını trafiği denetlemek VM'ler için yapılandırma

- Web uygulamalarınızı hedefleyen saldırılara karşı korumaya yardımcı olmak için web uygulaması güvenlik duvarı hazırlama

- Eksik sistem güncelleştirmelerini dağıtma

- Önerilen taban çizgileriyle eşleşmeyen işletim sistemi yapılandırmalarını ele alma

Güvenlik Merkezi, Azure kaynaklarınızdan, ağdan ve kötü amaçlı yazılımdan koruma programları ve güvenlik duvarları gibi iş ortağı çözümlerinden günlük verilerini otomatik olarak toplar, çözümler ve tümleştirir. Tehditler algılandığında bir güvenlik uyarısı oluşturulur. Örneklere şunların algılanması dahildir:

- Bilinen kötü amaçlı IP adresleri ile iletişim kuran tehlikeye atılmış VM'ler

- Windows hata raporlama kullanılarak algılanan gelişmiş kötü amaçlı yazılım

- Sanal makineleri karşı deneme yanılma saldırıları

- Tümleşik kötü amaçlı yazılımdan koruma programlarından ve güvenlik duvarlarından güvenlik uyarıları

### <a name="azure-monitor"></a>Azure İzleyicisi

[Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview) kaynakları belirli türde bilgileri işaretçiler sağlar. Görselleştirme, sorgu, yönlendirme, uyarı, otomatik ölçeklendirme ve Otomasyon verileri hem Azure altyapısı (Etkinlik günlüğü) ve tek tek her Azure kaynak (tanılama günlükleri) sunar.

Bulut uygulamalarını birçok taşıma bölümleriyle karmaşıktır. İzleme, uygulamanızı kurma kalmasını sağlamak için veri ve sağlıklı bir durumda çalışmasını sağlar. Ayrıca olası sorunları stave veya olanları sorun gidermeye yardımcı olur.

![Azure İzleyici](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig10.png) Ayrıca, uygulamanız hakkında ayrıntılı Öngörüler elde için izleme verilerini kullanabilirsiniz. Bu bilgi, uygulama performansı veya devamlılığını iyileştirmek için yardımcı veya aksi halde el ile müdahale gerektiren Eylemler otomatikleştirmek.

Ağınızda güvenlik denetimi, ağ güvenlik açıklarını algılama ve BT güvenliği ve Mevzuat idare modeli ile uyumluluğu sağlamak için önemlidir. Güvenlik grubu görünümü ile yapılandırılmış ağ güvenlik grubu ve güvenlik kuralları yanı sıra, etkin güvenlik kuralları alabilir. Uygulanacak kurallar listesi ile ağ güvenlik açığı açık bağlantı noktalarını ve ss belirleyebilirsiniz.

### <a name="network-watcher"></a>Ağ izleyicisi

[Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-watcher) izleme ve bir ağ düzeyinde içinde için ve azure'dan koşullar tanılama sağlayan bölgesel bir hizmettir. Ağ Tanılama ve görselleştirme Ağ İzleyicisi ile kullanılabilen araçlar anlamak, tanılama ve Azure ağınızdaki serisidir yardımcı olur. Bu hizmet içeren paket yakalama, sonraki atlama IP akış, güvenlik grubu görünümü, NSG akış günlükleri doğrulayın. Senaryo düzeyi izleme kaynak tek tek ağ izleme aksine ağ kaynaklarına bir uçtan uca görünümünü sağlar.

### <a name="storage-analytics"></a>Depolama analizi

[Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) bir depolama birimi hizmeti istekleriyle ilgili toplu işlem istatistiklerini ve kapasite verilerini dahil ölçümleri depolayabilirsiniz. İşlem düzeyinde hem de API işlemi ve aynı zamanda depolama hizmeti düzeyinde bildirilir ve kapasite depolama hizmet düzeyinde bildirilir. Ölçüm verilerini depolama hizmeti kullanım çözümlemek, depolama birimi hizmeti karşı yapılan istekleri ile sorunlarını tanılamak ve bir hizmeti kullanan uygulamaların performansını artırmak için kullanılabilir.

### <a name="application-insights"></a>Application Insights

[Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) birden çok platformdaki web geliştiricileri için genişletilebilir bir uygulama performansı Yönetimi (APM) hizmetidir. Canlı web uygulamanızı izlemek için kullanabilirsiniz. Performans anormalliklerini otomatik olarak algılar. Uygulamanızla kullanıcıların ne anlamak için ve sorunları tanılamanıza yardımcı olmak için güçlü analytics araçlar içerir. Performansı ve kullanılabilirliği sürekli geliştirmenize yardımcı olmak amacıyla tasarlanmıştır. .NET, Node.js ve J2EE gibi çok çeşitli platformlarda, şirket içi veya bulutta barındırılan uygulamalar için yararlıdır. DevOps işleminizi ile tümleşir ve bağlantı noktaları için çeşitli geliştirme araçlarına sahiptir.

Şunları izler:

- **İstek oranları, yanıt süreleri ve hata oranları**: Hangi sayfaların günün hangi saatlerinde popüler olduğunu ve kullanıcılarınızın konumunu öğrenin. En iyi performansı hangi sayfaların gösterdiğini görün. Daha fazla istek olduğunda yanıt süreleriniz ve hata oranlarınız yükseliyorsa bir kaynak atama sorununuz olabilir.

- **Bağımlılık oranları, yanıt süreleri ve hata oranları**: Dış hizmetlerin sizi yavaşlatıp yavaşlatmadığını öğrenin.

- **Özel durumlar** - toplu istatistikler çözümlemek veya belirli örnekleri seçin ve yığın izleme ve ilgili istekleri ayrıntıya gidin. Hem sunucu hem de tarayıcı özel durumları raporlanır.

- **Sayfa görüntüleme sayısı ve yükleme performansı**: Kullanıcılarınızın tarayıcıları tarafından gerçekleştirilir.

- **AJAX çağrılarından web sayfaları** -oranları, yanıt sürelerini ve başarısızlık oranları.

- **Kullanıcı ve oturum sayısı.**

- Windows veya Linux sunucu makinelerinizden CPU, bellek ve ağ kullanımı gibi **performans sayaçları**.

- Docker veya Azure’dan **konak tanılama**.

- Uygulamanızdan **tanılama izleme günlükleri**: İzleme olayları ile istekler arasında bağıntı kurmanıza imkan tanır.

- **Özel olayları ve ölçümleri** , kendiniz öğeleri satışı gibi iş olaylarını izlemek için istemci veya sunucu kodu veya Kazanılan oyun yazma.

Uygulamanızın altyapısı genellikle bir sanal makine, depolama hesabı, sanal ağ veya web uygulaması, veritabanı, veritabanı sunucusu ya da 3. taraf hizmetler gibi birçok bileşenden meydana gelir.  Bu bileşenleri ayrı varlıklar olarak değerlendirmez, bunun yerine bunları tek bir varlığın ilgili ve birbirine bağımlı parçaları olarak kabul edersiniz. Bunları gruplar halinde dağıtmak, yönetmek ve izlemek isteyebilirsiniz. [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) bir grup olarak çözümünüzdeki kaynaklar ile çalışmanıza olanak tanır.

Çözümünüzdeki tüm kaynakları tek ve eşgüdümlü bir işlemle dağıtabilir, güncelleştirebilir veya silebilirsiniz. Dağıtım için bir şablon kullanabilirsiniz. Üstelik bu şablon test, hazırlık ve üretim gibi farklı ortamlarda da çalışabilir. Resource Manager kaynaklarınızı dağıttıktan sonra yönetmenize yardımcı olmak için güvenlik, denetleme ve etiketleme özellikleri sunar.

**Resource Manager kullanmanın avantajları**

Resource Manager çeşitli avantajlar sunar:

- Çözümünüzdeki tüm kaynakları ayrı ayrı ele almak yerine bunları grup halinde dağıtabilir, yönetebilir ve izleyebilirsiniz.

- Çözümünüzü geliştirme yaşam döngüsü boyunca defalarca dağıtabilirsiniz. Kaynaklarınızın tutarlı bir durumda dağıtılması da size güven verir.

- Altyapınızı betikler yerine bildirim temelli şablonları kullanarak yönetebilirsiniz.

- Doğru sırayla dağıtılmalarını sağlamak kaynaklarınız arasındaki bağımlılıkları tanımlayabilirsiniz.

- Rol Tabanlı Erişim Denetimi (RBAC) yönetim platformuyla doğrudan tümleşik olduğu için kaynak grubunuzdaki tüm hizmetlere erişim denetimi uygulayabilirsiniz.

- Aboneliğinizdeki tüm kaynakları mantıksal olarak düzenlemek için kaynaklarınıza etiketler ekleyebilirsiniz.

- Aynı etiketi paylaşan bir kaynak grubunun maliyetlerini görüntüleyerek kuruluşunuzun faturalarına açıklık getirebilirsiniz.

> [!Note]
> Resource Manager çözümlerinizi dağıtmanın ve yönetmenin yeni bir yolunu sunar. Değişiklikler hakkında bilgi edinmek için önceki dağıtım modelini ve istediğiniz kullandıysanız, bkz: [anlama Resource Manager dağıtımını ve klasik dağıtımı](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model).

## <a name="next-steps"></a>Sonraki adımlar

Güvenlik hakkında daha fazla bizim kapsamlı güvenlik konuların bazıları okuyarak bulun:

- [Denetme ve günlüğe kaydetme](https://www.microsoft.com/en-us/trustcenter/security/auditingandlogging)

- [Siber Suçlar](https://www.microsoft.com/en-us/trustcenter/security/cybercrime)

- [Tasarım ve işlem güvenliği](https://www.microsoft.com/en-us/trustcenter/security/designopsecurity)

- [Şifreleme](https://www.microsoft.com/en-us/trustcenter/security/encryption)

- [Kimlik ve erişim yönetimi](https://www.microsoft.com/en-us/trustcenter/security/identity)

- [Ağ güvenliği](https://www.microsoft.com/en-us/trustcenter/security/networksecurity)

- [Tehdit yönetimi](https://www.microsoft.com/en-us/trustcenter/security/threatmanagement)
