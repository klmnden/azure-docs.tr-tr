---
title: Privileged Identity Management (PIM) - Azure Active Directory dağıtma | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) dağıtımını planlama açıklanır.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 02/08/2019
ms.author: rolyon
ms.custom: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7413fcf7992195753cba86a50b7d53a144b36023
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476436"
---
# <a name="deploy-azure-ad-privileged-identity-management-pim"></a>Azure AD Privileged Identity Management (PIM) dağıtma

Bu adım adım kılavuzda, Azure Active Directory (Azure AD) Privileged Identity Management (PIM), kuruluşunuzdaki dağıtımını planlama açıklar.

> [!TIP]
> Bu belge boyunca olarak işaretlenmiş olan öğeleri görürsünüz:
> 
> :heavy_check_mark: **Microsoft öneriyor**
> 
> Bunlar genel öneriler ve bunlar belirli Kurumsal ihtiyaçlarınızı uygularsanız, yalnızca uygulamalıdır.

## <a name="step-1-learn-about-pim"></a>1\.Adım PIM hakkında bilgi edinin

Azure AD Privileged Identity Management (PIM) Azure AD genelinde ayrıcalıklı yönetici rollerini yönetmenize yardımcı Azure kaynaklarını ve diğer Microsoft Çevrimiçi Hizmetler. Burada ayrıcalıklı kimlikleri Unutulan ve atanan bir dünyada, PIM tam zamanında erişim isteği onay iş akışları ve tamamen tümleşik erişim gözden geçirmeleri belirleyebilmeniz, açığa çıkarın ve ayrıcalıklı kötü amaçlı etkinlikleri engellemek gibi çözümler sağlar. gerçek zamanlı roller. Kuruluşunuzda, ayrıcalıklı rolleri yönetmek için PIM dağıtma, ayrıcalıklı rollerin etkinlikleri hakkında değerli Öngörüler görünmesini sırasında risk büyük ölçüde azaltır.

### <a name="business-value-of-pim"></a>PIM iş değeri

**Risk yönetme** -ilkesini zorunlu tutarak, kuruluşunuzun güvenliğini [en az ayrıcalık erişim](/windows-server/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models) ve tam zamanında erişim. Yükseltme için ayrıcalıklı rolleri ve zorunlu onaylar ve MFA kullanıcıları kalıcı atamalarını sayısını en aza indirerek, kuruluşunuzda ayrıcalıklı erişim için ilişkili güvenlik riskleri büyük ölçüde azaltabilir. En az ayrıcalık ve tam zamanında erişim zorlama, ayrıcalıklı rolleri erişimin geçmişini ve gelişmelerden güvenlik sorunlarını izlemek de izin verir.

**Uyumluluk ve idare adres** -PIM dağıtmaya devam eden kimlik yönetimi için bir ortam oluşturur. Tam zamanında yükseltme, ayrıcalıklı kimlikleri PIM, kuruluşunuzda ayrıcalıklı erişimi etkinlikleri izlemek bir yol sağlar. Görüntüleyebilir ve kuruluşunuz içinde kalıcı ve uygun rollerinin tüm atamaları için bildirimlerin olacaktır. Erişim gözden geçirme, düzenli olarak denetim ve gereksiz ayrıcalıklı kimlikleri kaldırabileceğiniz ve kuruluşunuzun En zorlu kimlik ve erişim güvenlik standartlarıyla uyumlu olduğundan emin olun.

**Maliyetleri azaltmak** -verimsizlikleri, İnsan hatası ve güvenlik sorunları PIM doğru dağıtarak ortadan kaldırarak maliyetleri azaltabilir. Net bir azalma, kurtarılır zor ve pahalı ayrıcalıklı kimlikleri ile ilişkili siber suçlar sonucudur. PIM ayrıca kuruluşunuz düzenlemeleri ve standartları ile uymak için söz konusu olduğunda, erişim bilgileri denetimle ilişkili maliyetini azaltmaya yardımcı olur.

Daha fazla bilgi için [Azure AD Privileged Identity Management nedir?](pim-configure.md).

### <a name="licensing-requirements"></a>Lisans gereksinimleri

PIM'i kullanmak için dizin aşağıdaki ücretli veya deneme sürümü lisansları birine sahip olmalıdır:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5
- Microsoft 365 M5

Daha fazla bilgi için [lisans PIM kullanmak için gereksinimler](subscription-requirements.md).

### <a name="key-pim-terminology"></a>Anahtar PIM terminolojisi

| Kavram veya sözleşme | Açıklama |
| --- | --- |
| Uygun | Bir kullanıcı rolü kullanmak için bir veya daha fazla eylem gerçekleştirmek gereken bir rol ataması. Bir kullanıcı rolü için uygun duruma getirildi, ayrıcalıklı görevleri gerçekleştirmek ihtiyaç duydukları zaman rolünü etkinleştirebilir anlamına gelir. Kalıcı uygun rol atamasını karşı kimseler verilen erişimi hiçbir fark yoktur. Tek fark, bazıları her zaman bu erişim olmanızın gerekmemesidir. |
| etkinleştirme | Bir kullanıcı için uygun bir rolü kullanmak için bir veya daha fazla eylemler gerçekleştirme işlemi. Bir iş gerekçesi sağlamak veya belirlenmiş onaylayanlar onayı isteyen bir çok faktörlü kimlik doğrulaması (MFA) denetimi gerçekleştirme işlemleri içerebilir. |
| Just-in-time (JIT) erişim | Kötü amaçlı veya yetkisiz kullanıcıların izinleri süresi dolduktan sonra erişmesini engelleyen kullanıcılar ayrıcalıklı görevlerin gerçekleştirilmesi için geçici izinleri aldıkları modeli. Yalnızca kullanıcıların gerektiğinde erişim izni verilir. |
| en az ayrıcalık erişim ilkesi | Her kullanıcı en düşük ayrıcalıkları ile yalnızca sağlanan önerilen bir güvenlik uygulaması gerçekleştirmek için yetkili görevleri gerçekleştirmek gerekli. Bu yöntem, genel yönetici sayısı en aza indirir ve bunun yerine özel yönetici rolleri belirli senaryolar için kullanır. |

Daha fazla bilgi için [terminolojisi](pim-configure.md#terminology).

### <a name="high-level-overview-of-how-pim-works"></a>PIM nasıl çalıştığına ilişkin üst düzey genel bakış

1. PIM kullanıcıların ayrıcalıklı rolleri için uygun olacak şekilde ayarlanmıştır.
1. Ayrıcalıklı rolünü kullanmak uygun bir kullanıcının gerektiğinde bunlar PIM rolünü etkinleştirin.
1. PIM ayarları bağlı olarak rol için yapılandırılmış kullanıcı (örneğin, çok faktörlü kimlik doğrulaması gerçekleştirme, onay alma veya bir neden belirtme.) bazı adımlarını tamamlamanız gerekir
1. Kullanıcı rolünü başarıyla etkinleştirir. sonra önceden yapılandırılmış bir süre için rol alırlar.
1. Yöneticiler, Denetim günlüğüne tüm PIM etkinlikleri geçmişini görüntüleyebilirsiniz. Bunlar ayrıca kiracılarına güvenli hale getirebilir ve erişim gözden geçirmeleri ve uyarılar gibi PIM özellikleri kullanarak uyumluluk gereklerini karşılamak.

Daha fazla bilgi için [Azure AD Privileged Identity Management nedir?](pim-configure.md).

### <a name="roles-that-can-be-managed-by-pim"></a>PIM tarafından yönetilebilen rolleri

**Azure AD rolleri** – tüm Azure Active Directory'de (örneğin, genel yönetici, Exchange yönetici ve Güvenlik Yöneticisi gibi) bu rolleridir. Daha fazla bilgi roller ve bunların işlevleri hakkında [Azure Active Directory'de Yönetici rolü izinleri](../users-groups-roles/directory-assign-admin-roles.md). Yöneticilerinize atamak için hangi rollerin belirleme konusunda daha fazla yardım için bkz: [göreviyle ayrıcalıklı rolleri az](../users-groups-roles/roles-delegate-by-task.md).

**Azure kaynağı rolleri** – bu rol bir Azure kaynak, kaynak grubu, abonelik veya yönetim grubu bağlı. PIM katkıda bulunan, sahibi ve kullanıcı erişimi Yöneticisi gibi iki yerleşik role tam zamanında erişim sağlar yanı [özel roller](../../role-based-access-control/custom-roles.md). Azure kaynak rolleri hakkında daha fazla bilgi için bkz. [rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/overview.md).

Daha fazla bilgi için [PIM'de yönetemezsiniz rolleri](pim-roles.md).

## <a name="step-2-plan-your-deployment"></a>2\.Adım Dağıtımınızı planlama

Bu bölüm, PIM kuruluşunuza dağıtmadan önce yapmanız gerekenler üzerinde odaklanır. Yönergeleri izleyin ve bu bölümde açıklanan kavramlar, kuruluşunuzun ayrıcalıklı kimlikleri için uyarlanmış iyi planı oluşturmak için yol gösterecek şekilde anlamak önemlidir.

### <a name="identify-your-stakeholders"></a>Proje katılımcılarını belirleme

Aşağıdaki bölümde, projeye katılan tüm proje katılımcılarını belirleme ve oturum, gözden geçirin veya bilgi sahibi olmak gereken yardımcı olur. Bu, Azure AD rolleri için PIM ve Azure kaynak rolleri için PIM dağıtmak için ayrı tablolar içerir. Proje katılımcıları, aşağıdaki tabloda, kuruluşunuz için uygun şekilde ekleyin.

- Bu nedenle oturum kapatma bu projede =
- R giriş sağlama ve bu projeyi gözden geçirme =
- Ben bu projenin Informed =

#### <a name="stakeholders-pim-for-azure-ad-roles"></a>Proje katılımcıları: PIM için Azure AD rolleri

| Ad | Role | Eylem |
| --- | --- | --- |
| Ad ve e-posta | **Kimlik Mimarı ya da Azure genel yönetici**<br/>Bu değişiklik, kuruluşunuzdaki çekirdek Kimlik Yönetimi altyapısı ile nasıl hizalanacağını tanımlama sorumlu Kimlik Yönetimi ekibinin bir temsilcisi. | SO/R/I |
| Ad ve e-posta | **Hizmet sahibi / satır Yöneticisi**<br/>Bir hizmet veya Hizmetleri grubunun BT sahipleri bir temsilcisinden. Yardımcı olmak için kendi team PIM alma ve kararların anahtar oldukları. | SO/R/I |
| Ad ve e-posta | **Güvenlik sahibi**<br/>Bir Temsilcisi güvenlik ekibinin planın kuruluşunuzun güvenlik gereksinimlerini karşıladığını oturum devre dışı. | SO/R |
| Ad ve e-posta | **BT yöneticisi destek / Yardım Masası**<br/>Bu değişikliğin bir Yardım Masası açısından desteklenebilirliği üzerinde giriş sağlayabilen bir temsilcisi BT destek kuruluştan. | R / I |
| Ad ve pilot kullanıcılarına e-posta | **Ayrıcalıklı rol kullanıcılar**<br/>Grubun kendisi için privileged Identity management uygulanır. PIM uygulandıktan sonra kendi rollerini etkinleştirebilmelerini nasıl bilmeniz gerekir. | I |

#### <a name="stakeholders-pim-for-azure-resource-roles"></a>Proje katılımcıları: PIM için Azure kaynak rolleri

| Ad | Role | Eylem |
| --- | --- | --- |
| Ad ve e-posta | **Abonelik / kaynak sahibi**<br/>Her bir abonelik veya için PIM dağıtmak istediğiniz kaynak BT sahipleri bir temsilcisinden | SO/R/I |
| Ad ve e-posta | **Güvenlik sahibi**<br/>Bir Temsilcisi güvenlik ekibinin planın kuruluşunuzun güvenlik gereksinimlerini karşıladığını oturum devre dışı. | SO/R |
| Ad ve e-posta | **BT yöneticisi destek / Yardım Masası**<br/>Bu değişikliğin bir Yardım Masası açısından desteklenebilirliği üzerinde giriş sağlayabilen bir temsilcisi BT destek kuruluştan. | R / I |
| Ad ve pilot kullanıcılarına e-posta | **RBAC rolü kullanıcıları**<br/>Grubun kendisi için privileged Identity management uygulanır. PIM uygulandıktan sonra kendi rollerini etkinleştirebilmelerini nasıl bilmeniz gerekir. | I |

### <a name="enable-pim"></a>PIM'i etkinleştirin

Planlama işleminin bir parçası, önce onay ve izleyerek PIM'i etkinleştirin bizim [PIM belge kullanmaya başlamak](pim-getting-started.md). PIM etkinleştirme dağıtımınıza yardımcı olmak için özel olarak tasarlanmış bazı özelliklere erişmenizi sağlar.

Amacınız, Azure kaynakları için PIM dağıtmak için ise, uygulamanız gereken bizim [PIM belgede yönetmek için Azure kaynaklarını bulun](pim-resource-roles-discover-resources.md). Yalnızca her bir kaynak, kaynak grubu ve abonelik sahipleri PIM bulunacak mümkün olacaktır. Genel Yöneticiyseniz çalışılırken, Azure kaynakları için PIM dağıtmak, yapabilecekleriniz [tüm Azure Aboneliklerini yönetmek için erişimini yükseltme](../../role-based-access-control/elevate-access-global-admin.md?toc=%2fazure%2factive-directory%2fprivileged-identity-management%2ftoc.json) kendiniz dizinindeki tüm Azure kaynakları için bulma erişmesini sağlamak için. Ancak, onay her, abonelik sahipleri kaynaklarını PIM ile yönetmeden önce elde etmenizi öneriyoruz.

### <a name="enforce-principle-of-least-privilege"></a>En az ayrıcalık ilkesini zorlama

Kuruluşunuz için Azure AD hem de en az ayrıcalık ilkesini zorlanmasına emin olmak önemlidir ve sizin Azure kaynağı rolleri.

#### <a name="azure-ad-roles"></a>Azure AD rolleri

Azure AD rolleri için yöneticilerin çoğu yalnızca bir gereksinim duyduğunuzda Yöneticiler önemli sayıda genel Yönetici rolüne veya iki özel yönetici rolleri atamak, kuruluşlar için yaygın değildir. Ayrıcalıklı rol atamaları da bırakılması eğilimindedir yönetilmeyen.

> [!NOTE]
> Yaygın görülen tehlikeleri rol temsilcisi seçme içinde:
>
> - Exchange yöneticisine, yalnızca ihtiyaç duydukları gündelik işlerini gerçekleştirmek için bir Exchange Yöneticisi rolüne olsa da genel Yönetici rolüne verilir.
> - Yöneticinin hem güvenlik hem de SharePoint yönetici rolleri gerekir ve yalnızca bir rol temsilci daha kolay olduğundan, genel Yönetici rolüne Office yönetici atanır.
> - Yönetici, uzun zaman önce bir görevi gerçekleştirmek için bir güvenlik yöneticisi rolü atanmış, ancak hiçbir zaman kaldırıldı.

Azure AD rolleri için en az ayrıcalık ilkesini uygulamak için aşağıdaki adımları izleyin.

1. Okuma ve anlama ayrıntı düzeyi rollerin anladığınızdan [kullanılabilir Azure AD yönetici rollerini](../users-groups-roles/directory-assign-admin-roles.md#available-roles). Siz ve takımınız da başvurmalıdır [yönetici rolleri Azure AD'de kimlik görev tarafından](../users-groups-roles/roles-delegate-by-task.md), belirli görevler için en az ayrıcalıklı rolünü açıklar.

1. Rol, kuruluşunuzda ayrıcalıklı var listesi. Kullanabileceğiniz [PIM Sihirbazı](pim-security-wizard.md#run-the-wizard) aşağıdaki gibi bir sayfaya almak için.

    ![Ayrıcalıklı roller bölmesinde kimin ayrıcalıklı rolleri gösteren keşfedin](./media/pim-deployment-plan/discover-privileged-roles-users.png)

1. Kuruluşunuzdaki tüm genel Yöneticiler için rolü neden bunlar gerektiğini öğrenin. Kişinin iş tarafından bir veya daha fazla ayrıntılı yönetici rolleri gerçekleştirilemiyorsa önceki belge okuma bağlı olarak, bunları genel yönetici rolünden kaldırmak ve gerekir atamaları uygun şekilde olun Azure Active Directory içinde (bir başvuru olarak: Microsoft şu anda yalnızca genel Yönetici rolüne sahip yaklaşık 10 yönetici yok. Daha fazla bilgi [Microsoft PIM kullanma](https://www.microsoft.com/itshowcase/Article/Content/887/Using-Azure-AD-Privileged-Identity-Management-for-elevated-access)).

1. Diğer Azure AD rolleri için atamaları listesini gözden geçirin, rolü artık isteyen yöneticiler tanımlamak ve bunları kendi atamalarından kaldırın.

3 ve 4 numaralı adımları otomatikleştirmek için PIM erişimi gözden geçirme işlevde kullanabilir. Yer alan adımları uygulayarak [PIM'de erişim gözden geçirmesi Azure AD rolleri için başlatmak](pim-how-to-start-security-review.md), bir veya daha fazla üyesi olan her bir Azure AD rol için erişim gözden geçirmesi ayarlayabilirsiniz.

![Bir Azure AD rolleri için erişim gözden geçirme bölmesinde oluşturma](./media/pim-deployment-plan/create-access-review.png)

Gözden geçirenler ayarlamalısınız **üyeler (kendi)** . Bu bir e-posta rolün tüm üyeleri için erişim gerekip gerekmediğini doğrulamak için alın gönderir. Ayrıca açmanız **onayda nedeni gerekli kıl** Gelişmiş ayarlarda, böylece kullanıcılar durum neden ihtiyaç duydukları rolü. Bu bilgilere dayanarak, kullanıcıların gereksiz rollerini kaldırın ve genel yöneticiler söz konusu olduğunda daha ayrıntılı yönetici rolleri atamak mümkün olacaktır.

Erişim gözden geçirmeleri, roller, erişimi gözden geçirmek için kişilere bildirmek için e-postaları dayanır. E-postaları bağlı olmayan hesapları ayrıcalıklı, bu hesapları ikincil e-posta alanı doldurmak emin olun. Daha fazla bilgi için [proxyAddresses özniteliği Azure AD'de](https://support.microsoft.com/help/3190357/how-the-proxyaddresses-attribute-is-populated-in-azure-ad).

#### <a name="azure-resource-roles"></a>Azure kaynağı rolleri

Azure abonelikleri ve kaynaklar için her bir abonelik veya kaynak rolleri gözden geçirmek için benzer bir erişim gözden geçirme işlemini ayarlayabilirsiniz. Bu işlemin her abonelik veya gereksiz atamaların kaldırmak için de kaynak bağlı sahip ve kullanıcı erişimi Yöneticisi atamaları en aza indirmek için hedeftir. Ancak, daha iyi anlamak belirli rolleri (özellikle özel roller) sahip oldukları kuruluşlar genellikle her bir abonelik veya kaynak sahibine gibi görevler verin.

Genel yönetici rolüne sahip bir BT yöneticisi olarak, çalışılırken, kuruluşunuzda Azure kaynakları için PIM dağıtmak, yapabilecekleriniz [tüm Azure Aboneliklerini yönetmek için erişimini yükseltme](../../role-based-access-control/elevate-access-global-admin.md?toc=%2fazure%2factive-directory%2fprivileged-identity-management%2ftoc.json) her bir aboneliğe erişim elde etmek için. Ardından, her abonelik sahibi bulun ve gereksiz atamalarını kaldırın ve sahip rolü ataması en aza indirmek için çalışır.

Bir Azure aboneliğine sahip rolüne sahip kullanıcılar ayrıca kullanabiliyor [erişim gözden geçirmeleriyle Azure kaynakları için](pim-resource-roles-start-access-review.md) denetim ve benzer şekilde, Azure AD rolleri için daha önce açıklanan işlemi gereksiz rol atamalarını kaldırmak için.

### <a name="decide-which-role-assignments-should-be-protected-by-pim"></a>Hangi rol atamaları PIM tarafından korunması gereken karar verin

Ayrıcalıklı rol atamalarını kuruluşunuzdaki temizlendikten sonra PIM ile korumak için hangi rollerin karar vermeniz gerekir.

Bir rol PIM tarafından korunuyorsa, uygun kullanıcı atanmış rol tarafından verilen ayrıcalıklarını Yükselt gerekir. Yükseltme süreci de onay alma, çok faktörlü kimlik doğrulaması gerçekleştirme ve/veya bir neden neden etkinleştirmekte olduğunuz sağlama dahil olabilir. PIM ayrıca indirmeyi bildirimler aracılığıyla izleyebilirsiniz ve PIM ve Azure AD, olay günlüklerini denetleme.

PIM ile korumak için hangi rollerin zor olabilir ve her kuruluş için farklı olacaktır seçme. Bu bölümde Azure AD için sunduğumuz en iyi uygulama önerileri ve Azure kaynak rolleri.

#### <a name="azure-ad-roles"></a>Azure AD rolleri

Azure AD rolleri, izinleri en çok sayıda koruma önceliğini belirlemek önemlidir. Kullanım desenlerini tüm PIM müşteriler arasında dayalı olarak, PIM tarafından yönetilen ilk 10 Azure AD rolleri açıklanmıştır:

1. Genel yönetici
1. Güvenlik yöneticisi
1. Kullanıcı Yöneticisi
1. Exchange Yöneticisi
1. SharePoint yöneticisi
1. Intune Yöneticisi
1. Güvenlik okuyucusu
1. Hizmet yöneticisi
1. Faturalama yöneticisi
1. Skype Kurumsal Yöneticisi

> [!TIP]
> :heavy_check_mark: **Microsoft öneriyor** tüm genel Yöneticiler ve güvenlik yöneticileri en tehlikeye olduğunda zarar yapabilirsiniz olanları oldukları gibi bir ilk adım olarak PIM kullanarak yönetin.

Hangi veri ve izni kuruluşunuz için en önemli olduğunu göz önünde bulundurmanız önemlidir. Örneğin, bazı kuruluşlar kendi Power BI Yönetici rolü veya sahip oldukları verilere erişmek ve/veya çekirdek iş akışları değiştirme olanağı gibi PIM kullanarak takımlar yönetici rollerine korumak isteyebilirsiniz.

Konuk kullanıcıları atanmış olan herhangi bir rolü varsa, özellikle de saldırıya açık.

> [!TIP]
> :heavy_check_mark: **Microsoft öneriyor** güvenliği aşılmış Konuk kullanıcı hesapları ile ilişkilendirilmiş riskini azaltmak için PIM kullanarak Konuk kullanıcılar ile tüm rolleri yönetin.

Dizin Okuyucu, ileti merkezi okuyucu ve güvenlik okuyucu bazen daha az önemli yazmak için izne sahip değilsiniz gibi diğer rollere göre düşünülen gibi okuyucu rolleri. Ancak, bazı müşteriler bu hesaplara erişim elde etmiştir saldırganlar, kişisel bilgileri (PII) gibi hassas verileri okuyabiliyor olabileceğinden bu rolleri de koruma gördük. Bu, kuruluşunuzda okuyucu rolleri PIM kullanarak yönetilen gerekip gerekmediğine karar verirken dikkate.

#### <a name="azure-resource-roles"></a>Azure kaynağı rolleri

Azure kaynakları için PIM kullanarak hangi rol atamaları yönetilmelidir karar verirken, öncelikle kuruluşunuz için en önemli abonelikler/kaynak belirlemeniz gerekir. Bu tür abonelikler/kaynak örnekleri şunlardır:

- En hassas verileri ana bilgisayar kaynakları
- Çekirdek kaynakları, müşterilere yönelik uygulamaları bağlıdır

Hangi abonelikler/kaynak en önemli sorun karar sahip bir genel yönetici, abonelik sahiplerine her abonelik tarafından yönetilen kaynaklar listesi toplamak için kuruluşunuzda ulaşmanızı. Ardından, kaynakları (düşük, Orta, yüksek) tehlikede olur durumda önem düzeyine göre gruplandırmak için abonelik sahipleriyle birlikte çalışması gerekir. Bu önem derecesi düzeyine dayalı PIM ile kaynakları yöneten öncelik vermelisiniz.

> [!TIP]
> :heavy_check_mark: **Microsoft öneriyor** abonelik/kaynak sahipleri içindeki hassas abonelikler/kaynak tüm rolleri için PIM iş akışı ayarlamak için kritik Hizmetleri ile çalışırsınız.

Azure kaynakları için PIM zamana bağlı hizmet hesaplarını destekler. Normal bir kullanıcı hesabı ele almanız nasıl tam olarak aynı hizmet hesaplarını düşünmelisiniz.

Abonelikler/kadar kritik olmayan kaynaklar için tüm rolleri için PIM ayarlayın gerekmez. Ancak, yine de sahip ve kullanıcı erişimi yöneticisi rollerini PIM ile korumanız gerekir.

> [!TIP]
> :heavy_check_mark: **Microsoft öneriyor** sahip rolü ve kullanıcı erişimi yöneticisi rolü PIM kullanarak tüm abonelikler/kaynak yönetme.

### <a name="decide-which-role-assignments-should-be-permanent-or-eligible"></a>Hangi rol atamaları kalıcı veya uygun karar verin

PIM tarafından yönetilecek rollerin listesini verdikten sonra hangi kullanıcıların uygun rol kalıcı olarak etkin rolü karşı almalısınız karar vermeniz gerekir. Kalıcı olarak etkin rollerin, Azure Active Directory ve Azure kaynakları arasında uygun roller yalnızca PIM'de atanabilirken atanmış normal rolleridir.

> [!TIP]
> :heavy_check_mark: **Microsoft öneriyor** sıfır kalıcı etkin atamalar hem Azure AD rolleri hem de önerilen dışında Azure kaynak rolleri için sahip olduğunuz [iki sonu cam Acil Durum erişim hesapları](../users-groups-roles/directory-emergency-access.md), hangi olmalıdır kalıcı genel yönetici rolü.

Sıfır ayakta yönetici öneririz, ancak bazen bunu hemen başarmak, kuruluşlar için zordur. Bu karar verirken dikkate alınacak noktalar şunlardır:

- Yükseltme – sıklığını kullanıcının yalnızca bir kez ayrıcalıklı ataması gerekiyorsa kalıcı atamaya olmamalıdır. Öte yandan, kullanıcı rolü için gündelik işlerini gerekir ve PIM kullanarak üretkenliği önemli ölçüde azaltır, bunlar kalıcı rol için kabul edilebilir.
- Uygun rol verilen kişi satıcılarla iletişim kurmayı ve yükseltme işlemini zorlamayı zor, noktasına çok uzak takım ya da yüksek sıralamaya executive ise – kuruluşunuza özel durumları, kalıcı bir rol için kabul edilebilir.

> [!TIP]
> :heavy_check_mark: **Microsoft öneriyor** , yinelenen erişimi ayarlamak için gözden geçirmeleri kalıcı rol atamaları sahip olan kullanıcıların (sahip olduğunuz herhangi). Bu dağıtım planı son bölümünde yinelenen erişim gözden geçirme hakkında daha fazla bilgi edinin

### <a name="draft-your-pim-settings"></a>PIM ayarlarınızı taslak

PIM çözümünüzü uygulamadan önce kuruluşunuzun kullandığı tüm ayrıcalıklı rol için PIM ayarlarınızı taslak iyi bir uygulamadır. Bu bölümde, bazı örnekleri (bunlar yalnızca başvuru ve kuruluşunuz için farklı olabilir) belirli rolleri için PIM ayarları bulunur. Bu ayarların her biri Microsoft'un önerilerle birlikte ayrıntılı sonraki tablolarda açıklanmıştır.

#### <a name="pim-settings-for-azure-ad-roles"></a>Azure AD rolleri için PIM ayarları

| Role | MFA gerektirme | Bildirim | Olay bileti | Onay iste | Onaylayan | Etkinleştirme süresi | Kalıcı yönetici |
| --- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Genel Yönetici | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | Başka genel Yöneticiler | 1 Saat | Acil Durum erişim hesapları |
| Exchange Yöneticisi | :heavy_check_mark: | :heavy_check_mark: | : x:. | : x:. | None | 2 saat | None |
| Yardım Masası Yöneticisi | : x:. | : x:. | :heavy_check_mark: | : x:. | None | 8 saat | None |

#### <a name="pim-settings-for-azure-resource-roles"></a>Azure kaynak rolleri için PIM ayarları

| Role | MFA gerektirme | Bildirim | Onay iste | Onaylayan | Etkinleştirme süresi | Etkin yönetici | Etkin zaman aşımı | Uygun bir süre sonu |
| --- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Önemli abonelik sahibi | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | Diğer abonelik sahipleri | 1 Saat | None | yok | 3 ay |
| Daha az kritik aboneliklerinin kullanıcı erişimi Yöneticisi | :heavy_check_mark: | :heavy_check_mark: | : x:. | None | 1 Saat | None | yok | 3 ay |
| Sanal makine Katılımcısı | : x:. | :heavy_check_mark: | : x:. | None | 3 saat | None | yok | 6 ay |

Aşağıdaki tabloda, ayarların her biri açıklanmaktadır.

| Ayar | Açıklama |
| --- | --- |
| Role | Ayarları için tanımladığınız rolün adı. |
| MFA gerektirme | Olup uygun kullanıcı MFA rolünü etkinleştirmeden önce gerçekleştirmeniz gerekir.<br/><br/> :heavy_check_mark: **Microsoft öneriyor** özellikle rolleri Konuk kullanıcılar varsa, tüm yönetici rolleri için MFA zorlama. |
| Bildirim | Uygun bir kullanıcı rolünü etkinleştirirken genel yönetici, true olarak ayarlanmış ayrıcalıklı Rol Yöneticisi ve kuruluşun Güvenlik Yöneticisi bir e-posta bildirimi alıp almayacağını.<br/><br/>**Not:** Bazı kuruluşların yönetici hesaplarında bu e-posta bildirimleri almak için bağlı bir e-posta adresi yoksa, Yöneticiler bu e-postaları alacak şekilde bir alternatif e-posta adresi ayarlayın tamamlamalıdır. |
| Olay bileti | Olup uygun kullanıcı rolünü etkinleştirirken bir olay bileti numarası kaydetmek gerekir. Bu ayar, istenmeyen etkinleştirmeleri azaltmak için iç bir olay numarasına sahip her etkinleştirme tanımlamak bir kuruluş yardımcı olur.<br/><br/> :heavy_check_mark: **Microsoft öneriyor** PIM ile iç sisteminize bağlamak için olay bileti sayıların yararlanma. Bu bağlam etkinleştirmesi için gereken onaylayanlar için özellikle yararlıdır. |
| Onay iste | Olup uygun kullanıcı rolü etkinleştirmek için onay alması gerekiyor.<br/><br/> :heavy_check_mark: **Microsoft öneriyor** en izne sahip roller için onay ayarlamak için. Tüm PIM müşterilerin kullanım modellerini bağlı olarak, genel yönetici, yönetici kullanıcı, Exchange yönetici, Güvenlik Yöneticisi ve parola Yöneticisi en yaygın onay kurulumun rolleridir. |
| Onaylayan | Onay isteği onaylaması gereken kişilerin listesini uygun rolü etkinleştirmek için gerekli olursa. Varsayılan olarak, PIM onaylayan kalıcı veya uygun olup olmadığını ayrıcalıklı rol yöneticisi olan tüm kullanıcılar olarak ayarlar.<br/><br/>**Not:** Bir kullanıcı hem de Azure AD rolüne ve onaylayan rolüne uygun ise, bunlar kendilerini onaylanacak mümkün olmayacaktır.<br/><br/> :heavy_check_mark: **Microsoft öneriyor** kullanıcıların belirli bir rolü ve sık sık kullanıcılar yerine hakkında bir genel yönetici en bilgili olması için onaylayanları seçin. |
| Etkinleştirme süresi | Bir kullanıcı rolü kendisinden önce etkin süre sona erecek. |
| Kalıcı yönetici | Rolü için kalıcı bir yöneticisinin kullanıcıların listesini (etkinleştirmek hiçbir zaman sahip).<br/><br/> :heavy_check_mark: **Microsoft öneriyor** genel Yöneticiler hariç tüm roller için sıfır ayakta yönetici. Okuma daha içinde ilgili kim uygun yapılmalıdır ve kimin Bu plan kalıcı olarak etkin bölümünü olmalıdır. |
| Etkin yönetici | Azure kaynakları için etkin, hiçbir zaman rolünü kullanmak için etkinleştirmeye sahip olacak kullanıcıların listesini yöneticisidir. Kullanıcı bu rolü zaman kaybedecek için sona erme süresini ayarlandığından bu gibi kalıcı yönetici Azure AD rolleri olarak adlandırılır değil. |
| Etkin zaman aşımı | Azure kaynak rolleri için bir etkin bir rol ataması bu süre yapılandırılan sonra süresi dolar. 15 günden 1 ay, 3 ay, 6 aylık, 1 yıl seçebilirsiniz veya kalıcı olarak etkin. |
| Uygun bir süre sonu | Bir Azure kaynak rolleri için uygun rol atamasını bu süre yapılandırılan sonra süresi dolar. 15 günden 1 ay, 3 ay, 6 aylık, 1 yıl seçebilirsiniz veya kalıcı olarak uygun. |

## <a name="step-3-implement-your-solution"></a>Adım 3. Çözümünüzü uygulama

Doğru planlama temel bir uygulamayı Azure Active Directory ile başarıyla dağıtabilirsiniz temelidir.  Bu akıllı güvenlik ve ekleme başarılı dağıtımlar için zaman azaltırken basitleştirir tümleştirme sağlar.  Bu birleşim, uygulamanızı kolayca süresini, son kullanıcılarınız için azaltma oluştu tümleşik sağlar.

### <a name="identify-test-users"></a>Test kullanıcılarını belirleme

Kullanıcılar ve kullanıcı gruplarını uygulama doğrulamak için bir kümesini tanımlamak için bu bölümü kullanın. Planlama bölümünde seçtiğiniz ayarlara bağlı olarak, her rol için test etmek istediğiniz kullanıcıları belirleyin.

> [!TIP]
> :heavy_check_mark: **Microsoft öneriyor** işlemle öğrenmeniz ve dağıtım için bir iç advocator haline test kullanıcılar olacak hizmet sahiplerini her Azure AD rolüne olun.

Bu tabloda, her rol için ayarları çalışıp çalışmadığını doğrularsınız test kullanıcıları belirleyin.

| Rol adı | Test kullanıcıları |
| --- | --- |
| &lt;Rol adı&gt; | &lt;Kullanıcı rolünü test etmek için&gt; |
| &lt;Rol adı&gt; | &lt;Kullanıcı rolünü test etmek için&gt; |

### <a name="test-implementation"></a>Test uygulaması

Test kullanıcılarını tanımladığınıza göre PIM test kullanıcılarınız için yapılandırmak için bu adımı kullanın. Kuruluşunuz Azure portalının içindeki PIM'ın kullanıcı arabirimini kullanmak yerine kendi iç uygulamaya PIM iş akışı birleştirmek isterse, PIM tüm işlemleri aracılığıyla da desteklenir bizim [graph API](https://docs.microsoft.com/graph/api/resources/privilegedidentitymanagement-root).

#### <a name="configure-pim-for-azure-ad-roles"></a>Azure AD rolleri için PIM'i yapılandırın

1. [Azure AD rol ayarlarını yapılandırma](pim-how-to-change-default-settings.md) , planlanan üzerinde temel.

1. Gidin **Azure AD rolleri**, tıklayın **rolleri**ve ardından, yapılandırdığınız rolü seçin.

1. Zaten bir kalıcı yönetici olmaları durumunda test kullanıcı grubu için bunları kendileri için arama ve bunları kalıcı uygun duruma getirmek için kendi satırında üç noktaya tıklayarak dönüştürme uygun yapabilirsiniz. Rol atamaları sahip değilseniz henüz yapabilecekleriniz [yeni bir hak atama yapmak](pim-how-to-add-role-to-user.md#make-a-user-eligible-for-a-role).

1. Test etmek istediğiniz tüm roller için 1-3. adımları tekrarlayın.

1. Test kullanıcıları ayarladıktan sonra bunları nasıl bağlantısını göndermemelidir [kendi Azure AD rol etkinleştirme](pim-how-to-activate-role.md).

#### <a name="configure-pim-for-azure-resource-roles"></a>Azure kaynak rolleri için PIM'i yapılandırın

1. [Azure kaynak rol ayarlarını yapılandırma](pim-resource-roles-configure-role-settings.md) bir abonelik veya test etmek istediğiniz kaynak içinde bir rol için.

1. Gidin **Azure kaynaklarını** bu aboneliği ve'ı tıklatın **rolleri**, yapılandırdığınız rolü seçin.

1. Zaten etkin bir yönetici ise test kullanıcı grubu için bunları için arama yaparak uygun yapabileceğiniz ve [kendi rol atamasını Güncelleştir](pim-resource-roles-assign-roles.md#update-or-remove-an-existing-role-assignment). Rol sahip değilseniz henüz yapabilecekleriniz [yeni rol atama](pim-resource-roles-assign-roles.md#assign-a-role).

1. Test etmek istediğiniz tüm roller için 1-3. adımları tekrarlayın.

1. Test kullanıcıları ayarladıktan sonra bunları nasıl bağlantısını göndermemelidir [Azure kaynak rolleri etkinleştirme](pim-resource-roles-activate-your-roles.md).

Bu aşama, tüm yapılandırmaları için ayarlanmış olup olmadığını rolleri düzgün biçimde çalıştığından doğrulamak için kullanmanız gerekir. Testlerinizi belgelemek için aşağıdaki tabloyu kullanın. Bu aşama, etkilenen kullanıcılar ile iletişimi iyileştirmek için de kullanmalısınız.

| Role | Etkinleştirme sırasında beklenen bir davranış | Gerçek sonuçlar |
| --- | --- | --- |
| Genel Yönetici | (1) MFA gerektirme<br/>(2) onayı iste<br/>(3) onaylayana bildirim alır ve onaylayabilirsiniz<br/>(4) rolünü önceden belirlenmiş bir süre sonra süresi dolar. |  |
| Aboneliğin sahibi *X* | (1) MFA gerektirme<br/>(2) uygun atamayı yapılandırılan süre sonra süresi dolar. |  |

### <a name="communicate-pim-to-affected-stakeholders"></a>PIM etkilenen proje katılımcılarıyla iletişim

PIM dağıtma kullanıcılar ayrıcalıklı rol için ek adımlar başlatacaktır. PIM ile ayrıcalıklı kimlikleri ilgili güvenlik sorunları önemli ölçüde azaltır, ancak değişiklik Kiracı genelinde dağıtımdan önce etkili bir şekilde iletilmesi gerekir. Etkilenen Yöneticiler sayısına bağlı olarak, kuruluşların iç bir belge, bir video veya değişikliği hakkında bir e-posta oluşturmak genellikle seçin. Bu iletişimin sık dahil içerir:

- PIM nedir
- Kuruluş için avantajı nedir
- Kimler etkilenecek
- Ne zaman PIM kullanıma sunulacak
- Hangi ek adımlar, rolü etkinleştirmek, kullanıcılar için gerekli olacaktır
    - Bağlantılar belgelerimize gönderme:
    - [Azure AD rollerini etkinleştir](pim-how-to-activate-role.md)
    - [Azure kaynağı rolleri etkinleştirin](pim-resource-roles-activate-your-roles.md)
- İletişim bilgileri veya Yardım Masası bağlantısını PIM ile ilişkili herhangi bir sorunu

> [!TIP]
> :heavy_check_mark: **Microsoft öneriyor** PIM akışı geçmek, Yardım Masası/destek ekibinizle saat ayarlamanıza olanak (kuruluşunuzun iç bir BT destek varsa team). Bunları, iletişim bilgilerinizi yanı sıra ilgili belgeler sağlar.

### <a name="move-to-production"></a>Üretime taşıma

Test tamamlandı ve başarılı olduğunda, PIM test aşamalardaki PIM yapılandırmanızda tanımlanan her rolünün tüm kullanıcılar için tüm adımları tekrarlayarak üretime taşıyın. İçin Azure AD rolleri için PIM, kuruluşlar çoğunlukla test ve PIM önce test ve diğer rolleri için PIM alınıyor genel yöneticileri geri alın. Bu sırada Azure kaynakları için kuruluşlar genellikle test ve PIM aynı anda bir Azure aboneliği geri alın.

### <a name="in-the-case-a-rollback-is-needed"></a>Bir geri alma durumunda gereklidir

Üretim ortamında istediğiniz gibi çalışması PIM başarısız olursa, aşağıdaki geri alma adımları, PIM ayarlamadan önce geri bilinen bir iyi duruma geri dönmek için yardımcı olabilir:

#### <a name="azure-ad-roles"></a>Azure AD rolleri

1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. Açık **Azure AD Privileged Identity Management**.
1. Tıklayın **Azure AD rolleri** ve ardından **rolleri**.
1. Yapılandırdığınız her rol için üç nokta simgesine tıklayın ( **...** ) uygun bir atamayı sahip tüm kullanıcılar için.
1. Tıklayın **kalıcı hale getirmek** rol ataması kalıcı hale getirme seçeneği.

#### <a name="azure-resource-roles"></a>Azure kaynağı rolleri

1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. Açık **Azure AD Privileged Identity Management**.
1. Tıklayın **Azure kaynaklarını** ve bir abonelik veya kaynak istediğiniz geri almak için'a tıklayın.
1. Tıklayın **rolleri**.
1. Yapılandırdığınız her rol için üç nokta simgesine tıklayın ( **...** ) uygun bir atamayı sahip tüm kullanıcılar için.
1. Tıklayın **kalıcı hale getirmek** rol ataması kalıcı hale getirme seçeneği.

## <a name="step-4-next-steps-after-deploying-pim"></a>4\. adımı. PIM dağıttıktan sonra sonraki adımlar

PIM üretimde başarıyla dağıtmak, kuruluşunuzun güvenlik altına alma açısından sds'de önemli bir kimlikleri ayrıcalıklı olur. Güvenlik ve uyumluluk için kullanmanız gereken ek PIM özellikleri dağıtımı PIM ile gelir.

### <a name="use-pim-alerts-to-safeguard-your-privileged-access"></a>Ayrıcalıklı erişim güvenliğini sağlamak için kullanım PIM uyarıları

Kiracınız için daha iyi koruma PIM'ın yerleşik uyarı işlevi kullanmalıdır. Daha fazla bilgi için [güvenlik uyarıları](pim-how-to-configure-security-alerts.md#security-alerts). Bu uyarılar şunlardır: Yöneticiler ayrıcalıklı rolleri kullanarak değil, rollerini PIM dışında atanmış rolleri etkinleştirilmekte çok sık ve daha fazlası. Kuruluşunuzun tam olarak korumak için düzenli olarak, uyarılar listesini sırasıyla izleyin ve sorunları giderin gerekir. Görüntüleyebilir ve aşağıdaki şekilde uyarıları düzeltin:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. Açık **Azure AD Privileged Identity Management**.
1. Tıklayın **Azure AD rolleri** ve ardından **uyarılar**.

> [!TIP]
> :heavy_check_mark: **Microsoft öneriyor** hemen yüksek önem derecesine sahip olarak işaretlenmiş tüm uyarıları işleme. Orta ve düşük öneme sahip uyarılar için bildirim alın ve bir güvenlik tehdidi yok düşünüyorsanız, değişiklik gerekir.

Herhangi bir belirli uyarılar kullanışlı olmayan veya kuruluşunuz için geçerli değil, her zaman uyarılar sayfasında uyarıyı kapatabilirsiniz. Her zaman daha sonra Azure AD Ayarları sayfasında bu işten çıkarma geri dönebilirsiniz.

### <a name="set-up-recurring-access-reviews-to-regularly-audit-your-organizations-privileged-identities"></a>Düzenli olarak kuruluşunuzun ayrıcalıklı kimlikleri denetlemek için yinelenen erişim gözden geçirmeleri ayarlayın

Erişim gözden geçirmeleri, ayrıcalıklı roller ya da belirli gözden geçirenler atanan kullanıcılar, her kullanıcı privileged Identity gerekip gerekmediğini sormanız için en iyi yoludur. Erişim gözden geçirmeleri, saldırı yüzeyini ve uyumlu kalın azaltmak istiyorsanız mükemmeldir. Erişim gözden geçirmesi başlatma hakkında daha fazla bilgi için bkz. [rolleri Azure AD erişim gözden geçirmeleriyle](pim-how-to-start-security-review.md) ve [Azure kaynağı rolleri erişim gözden geçirmeleriyle](pim-resource-roles-start-access-review.md). Bazı kuruluşlar için düzenli bir erişim gözden geçirmesi gerçekleştirme yasalara ve yönetmeliklere ederken diğerleri için uyumlu kalmak için gerekli, erişim gözden geçirmesi kuruluşunuzda en az ayrıcalık sorumlusu zorlamak için en iyi yolu.

> [!TIP]
> :heavy_check_mark: **Microsoft öneriyor** üç aylık erişim gözden geçirmeleri, Azure AD için ayarlama ve Azure kaynak rolleri.

Azure kaynak rolleri için Gözden Geçiren rolünü bulunduğu aboneliğin sahibi olsa da çoğu durumda, Azure AD rolleri için Gözden Geçiren kullanıcı kendilerini sayısıdır. Ancak, genellikle şirket herhangi belirli bir kişinin e-posta adresine bağlı olmayan hesapları burada ayrıcalıklı durum geçerlidir. Bu durumlarda okur ve hiç erişim gözden geçirmeleri.

> [!TIP]
> :heavy_check_mark: **Microsoft öneriyor** düzenli olarak işaretlenmiş e-posta adresine bağlı olmayan ayrıcalıklı rol atamalarını sahip tüm hesaplar için bir ikincil e-posta adresi ekleyin

### <a name="get-the-most-out-of-your-audit-log-to-improve-security-and-compliance"></a>Güvenlik ve Uyumluluğu geliştirmek için en iyi denetim günlüğünüzün dışında Al

Denetim günlüğü, güncel olarak takip edin ve yönetmelikleri ile uyumlu bir yerdir. PIM şu anda kuruluşunuzun tüm geçmişi, Denetim günlüğü dahil içinde 30 günlük geçmiş depolar:

- Etkinleştirme/devre dışı bırakma uygun roller
- Rol ataması etkinliklerini içindeki ve dışındaki PIM
- Rol ayarlarında değişiklik
- Onay kurulumun rolü etkinleştirme isteğini/onayla/reddet etkinlikleri
- Uyarıları güncelleştir

Bunlar erişebileceğiniz bir genel yönetici veya ayrıcalıklı Rol Yöneticisi olduğunuz denetim günlükleri. Daha fazla bilgi için [denetim geçmişi için Azure AD rolleri](pim-how-to-use-audit-log.md) ve [denetim geçmişi Azure kaynak rolleri için](azure-pim-resource-rbac.md).

> [!TIP]
> :heavy_check_mark: **Microsoft öneriyor** tüm okuma en az bir yönetici olmasını haftalık olarak olaylarını denetleme ve denetim olaylarınızı aylık olarak dışarı aktarın.

Otomatik olarak, denetim olaylarını daha uzun bir süre saklamak istiyorsanız, PIM'ın denetim günlüğü içine otomatik olarak eşitlenip [Azure AD denetim günlükleri](../reports-monitoring/concept-audit-logs.md).

> [!TIP]
> :heavy_check_mark: **Microsoft öneriyor** ayarlamak için [Azure günlük izleme](../reports-monitoring/concept-activity-logs-azure-monitor.md) denetim olaylarını güvenlik ve uyumluluk gereksinimi için bir Azure depolama hesabında arşivlemek için.
