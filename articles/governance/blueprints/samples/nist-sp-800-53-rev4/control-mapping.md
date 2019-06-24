---
title: -800-53 NIST SP R4 şema - denetim eşleme örneği
description: Azure İlkesi ve RBAC 800-53 R4 şema örnek NIST SP eşleme denetimi.
author: DCtheGeek
ms.author: dacoulte
ms.date: 06/24/2019
ms.topic: sample
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: b887c4e6812d201dc83465a578f71e1742e8e9cf
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342036"
---
# <a name="control-mapping-of-the-nist-sp-800-53-r4-blueprint-sample"></a>800-53 R4 şema örnek NIST SP eşleme denetimi

Aşağıdaki makalede eşlemelerini nasıl yapar? Azure şemaları NIST SP 800-53 R4 şema örnek için NIST SP 800-53 R4 denetimleri ayrıntılı olarak açıklanmaktadır. Denetimler hakkında daha fazla bilgi için bkz. [NIST SP 800-53](https://nvd.nist.gov/800-53).

Şu eşlemeler üzeresiniz **NIST SP 800-53'ün (açık 4)** kontrol eder. Gezinti, doğrudan bir özel denetim eşlemesi atlamak için sağ taraftaki kullanın. Eşlenen denetimleri birçoğu ile uygulanan bir [Azure İlkesi](../../../policy/overview.md) girişim. Tam girişim gözden geçirmek için açık **ilke** Azure portal ve select **tanımları** sayfası. Daha sonra bulmak ve seçmek  **[Önizleme]: SP NIST 800-53 R4 denetimleri denetim ve denetim gereksinimlerini desteklemek için belirli VM uzantılarını dağıtma** yerleşik ilke girişimi.

## <a name="ac-2-account-management"></a>AC 2 hesap yönetimi

Bu plan, kuruluşunuzun hesap yönetimi gereksinimleri ile uyumlu olmadığı hesapları gözden geçirmenize yardımcı olur. Bu şema, bir Abonelikteki okuma, yazma ve sahip izinleri olan dış hesapları denetleme ve hesapları kullanım dışı beş Azure ilke tanımları atar. Bu ilkeleri tarafından denetlenen hesapları inceleyerek, hesap yönetimi gereksinimlerinin karşılandığından emin olmak için uygun işlemleri gerçekleştirebilir.

- [Önizleme]: Audit deprecated accounts on a subscription
- [Önizleme]: Audit deprecated accounts with owner permissions on a subscription
- [Önizleme]: Audit external accounts with owner permissions on a subscription
- [Önizleme]: Audit external accounts with read permissions on a subscription
- [Önizleme]: Audit external accounts with write permissions on a subscription

## <a name="ac-2-7-account-management--role-based-schemes"></a>AC-2 (7) hesap yönetimi | Rol tabanlı düzenleri

Azure uygular [rol tabanlı erişim denetimi](../../../../role-based-access-control/overview.md) Azure'da kimlerin erişebileceğini kaynakları yönetmenize yardımcı olmak için (RBAC). Azure portalını kullanarak, kimlerin erişebileceğini Azure kaynaklarını ve izinlerini gözden geçirebilirsiniz. Bu şema Ayrıca iki atar [Azure İlkesi](../../../policy/overview.md) SQL sunucuları ve Service Fabric için Azure Active Directory kimlik doğrulaması kullanımını denetlemek için tanımları. Azure Active Directory'yi kullanarak kimlik doğrulaması, Basitleştirilmiş izin yönetimi ve veritabanı kullanıcıları ve diğer Microsoft hizmetlerinde merkezi kimlik yönetimi sağlar.

- SQL server için Azure Active Directory Yöneticisi sağlama denetleme
- Service fabric'te istemci kimlik doğrulaması için Azure Active Directory kullanımını denetleme

## <a name="ac-2-12-account-management--account-monitoring--atypical-usage"></a>AC-2 (12) hesap yönetimi | Hesabı izleme / alışılmadık kullanımı

Just-ın-time (JIT) sanal makine erişimi kilitler gelen trafiği Azure sanal makineler, Vm'lere gerektiğinde bağlanılabilmesi için kolay erişim sağlamanın yanı sıra saldırılara maruz kalma riskinizi azaltır. Tüm JIT isteklerini sanal makinelere erişmek için alışılmadık kullanımını izlemenize olanak sağlayan etkinlik günlüğü'nde günlüğe kaydedilir. Bu şema atar bir [Azure İlkesi](../../../policy/overview.md) yardımcı olan tanımı izleme sanal makinelerin tam zamanında erişim destekler ancak henüz yapılandırılmamış.

- [Önizleme]: Azure Güvenlik Merkezi'nde olası ağ yalnızca zamanında (JIT) erişimi izleme

## <a name="ac-4-information-flow-enforcement"></a>AC 4 bilgi akışını zorlama

Kaynak kaynak paylaşımı (CORS) bir dış etki alanından istenmesi için uygulama hizmetleri kaynakları izin verebilirsiniz. Microsoft, API, işlevi ve web uygulamaları ile etkileşim kurmak yalnızca gerekli etki alanlarına izin önerir. Bu şema atar bir [Azure İlkesi](../../../policy/overview.md) CORS kaynaklara erişim kısıtlamalarını Azure Güvenlik Merkezi'nde izlemenize yardımcı olması için tanımı.
CORS uygulamaları anlama, bilgi akışı denetimlerini uygulandığını doğrulamanıza yardımcı olabilir.

- [Önizleme]: Audit CORS resource access restrictions for a Web Application

## <a name="ac-5-separation-of-duties"></a>AC 5 ayrımı

Tek bir Azure aboneliği sahibi olan Yönetim yedeklilik için izin vermez. Buna karşılık, çok fazla Azure aboneliğine sahip olan, güvenliği aşılmış sahip hesabı aracılığıyla bir ihlal olasılığını artırabilir. Bu şema iki atayarak Azure abonelik sahipleri uygun sayıda tutmanıza yardımcı olur. [Azure İlkesi](../../../policy/overview.md) denetim sahipleri Azure aboneliklerinin sayısını tanımlar. Bu şema ayrıca dört atar yardımcı olan Azure ilke tanımlarını kontrol Windows sanal makinelerinde Administrators grubunun üyeliği. Abonelik sahibi ve sanal makine Yönetici izinlerini yönetme, uygun görev ayrımı uygulamak yardımcı olabilir.

- [Önizleme]: Audit maximum number of owners for a subscription
- [Önizleme]: Audit minimum number of owners for subscription
- Yöneticiler grubunun Windows Vm'leri içinde belirtilen üyeleri dışlar denetleme
- Denetim Yöneticiler grubunun Windows Vm'leri içinde belirtilen üyeleri içerir
- Yöneticiler grubunun Windows Vm'leri içinde belirtilen üyeleri dışlar denetlemek için VM uzantısı dağıtma
- Yöneticiler grubunun Windows Vm'leri içinde belirtilen üyelerini içerir denetlemek için VM uzantısı dağıtma

## <a name="ac-6-7-least-privilege--review-of-user-privileges"></a>AC-6 (7) en az ayrıcalık | Kullanıcı ayrıcalıklarını gözden geçirme

Azure uygular [rol tabanlı erişim denetimi](../../../../role-based-access-control/overview.md) Azure'da kimlerin erişebileceğini kaynakları yönetmenize yardımcı olmak için (RBAC). Azure portalını kullanarak, kimlerin erişebileceğini Azure kaynaklarını ve izinlerini gözden geçirebilirsiniz. Bu şema altı atar [Azure İlkesi](../../../policy/overview.md) tanımlarını gözden geçirme için öncelik hesapları denetleme. Bu hesap göstergeleri gözden geçirme en az ayrıcalık denetimleri uygulanan olmanıza yardımcı olabilir.

- [Önizleme]: Audit maximum number of owners for a subscription
- [Önizleme]: Audit minimum number of owners for subscription
- Yöneticiler grubunun Windows Vm'leri içinde belirtilen üyeleri dışlar denetleme
- Denetim Yöneticiler grubunun Windows Vm'leri içinde belirtilen üyeleri içerir
- Yöneticiler grubunun Windows Vm'leri içinde belirtilen üyeleri dışlar denetlemek için VM uzantısı dağıtma
- Yöneticiler grubunun Windows Vm'leri içinde belirtilen üyelerini içerir denetlemek için VM uzantısı dağıtma

## <a name="ac-17-1-remote-access--automated-monitoring--control"></a>Uzaktan erişim AC-17 (1) | İzleme otomatik / denetleme

Bu şema izleyin ve denetleyin yardımcı olan üç atayarak uzaktan erişim [Azure İlkesi](../../../policy/overview.md) monitörlerle Azure App Service uygulamasını devre dışı için hata ayıklama, uzaktan tanımları ve Linux denetim iki ilke tanımları parola olmadan hesaplardan gelen uzak bağlantılara izin veren sanal makineler. Bu plan Ayrıca, depolama hesapları için Kısıtlanmamış erişime izlemenize yardımcı olan bir Azure İlkesi tanım atar. Bu göstergeler izleme, güvenlik ilkeniz uzaktan erişim yöntemleri uymalarını yardımcı olabilir.

- [Önizleme]: Audit remote debugging state for a Function App
- [Önizleme]: Audit remote debugging state for a Web Application
- [Önizleme]: Audit remote debugging state for an API App
- [Önizleme]: Audit that Linux VMs do not allow remote connections from accounts without passwords
- [Önizleme]: Deploy VM extension to audit that Linux VMs do not allow remote connections from accounts without passwords
- Depolama hesapları sınırsız ağ erişimi denetleyin

## <a name="au-3-2-content-of-audit-records--centralized-management-of-planned-audit-record-content"></a>Denetim kayıtlarını AU-3 (2) içeriğini | Planlanan denetim kaydı içeriği için Merkezi Yönetim

Azure İzleyici tarafından toplanan günlük veri merkezi yapılandırması ve Yönetimi etkinleştirmek için bir Log Analytics çalışma alanında depolanır. Bu şema olayları günlüğe yedi atayarak kaydedilir olmanıza yardımcı olur. [Azure İlkesi](../../../policy/overview.md) denetim ve Azure sanal makinelerinde Log Analytics aracısını dağıtımını zorunlu tanımlar.

- [Önizleme]: Denetim günlüğü analiz aracı dağıtımı - VM görüntüsü (OS) listeden kaldırıldı
- [Önizleme]: Denetim günlüğü analiz aracı dağıtım VMSS - VM görüntüsü (OS) listeden kaldırıldı
- [Önizleme]: Audit Log Analytics Workspace for VM - Report Mismatch
- [Önizleme]: Deploy Log Analytics Agent for Linux VM Scale Sets (VMSS)
- [Önizleme]: Deploy Log Analytics Agent for Linux VMs
- [Önizleme]: Deploy Log Analytics Agent for Windows VM Scale Sets (VMSS)
- [Önizleme]: Deploy Log Analytics Agent for Windows VMs

## <a name="au-5-response-to-audit-processing-failures"></a>İşleme hataları denetlemek için 5 AU yanıt

Bu şema beş atar [Azure İlkesi](../../../policy/overview.md) denetim ve olay günlük kaydı yapılandırmasını izleyen tanımları. Bu yapılandırmalar izleme, bir denetim sistemi hatası veya yanlış yapılandırılması nedeniyle göstergesi sağlayın ve düzeltici eylemlerde yardımcı olur.

- [Önizleme]: Monitor unaudited SQL servers in Azure Security Center
- Tanılama ayarını denetle
- Gelişmiş veri güvenliği olmadan denetim yönetilen SQL örnekleri
- SQL sunucu düzeyi denetimi ayarları denetle
- Gelişmiş veri güvenliği olmadan denetim SQL sunucuları

## <a name="au-6-4-audit-review-analysis-and-reporting--central-review-and-analysis"></a>AU-6 (4) denetim gözden geçirme, analiz ve Raporlama | Merkezi inceleme ve çözümleme

Azure İzleyici tarafından toplanan günlük verilerini bir Log Analytics çalışma alanı etkinleştirme merkezi raporlama ve analiz depolanır. Bu şema olayları günlüğe yedi atayarak kaydedilir olmanıza yardımcı olur. [Azure İlkesi](../../../policy/overview.md) denetim ve Azure sanal makinelerinde Log Analytics aracısını dağıtımını zorunlu tanımlar.

- [Önizleme]: Denetim günlüğü analiz aracı dağıtımı - VM görüntüsü (OS) listeden kaldırıldı
- [Önizleme]: Denetim günlüğü analiz aracı dağıtım VMSS - VM görüntüsü (OS) listeden kaldırıldı
- [Önizleme]: Audit Log Analytics Workspace for VM - Report Mismatch
- [Önizleme]: Deploy Log Analytics Agent for Linux VM Scale Sets (VMSS)
- [Önizleme]: Deploy Log Analytics Agent for Linux VMs
- [Önizleme]: Deploy Log Analytics Agent for Windows VM Scale Sets (VMSS)
- [Önizleme]: Deploy Log Analytics Agent for Windows VMs

## <a name="au-12-audit-generation"></a>AU-12 denetim oluşturma

Bu şema sistem olaylarını 15 atayarak kaydedilir olmanıza yardımcı olur. [Azure İlkesi](../../../policy/overview.md) Azure kaynaklarını denetim günlüğü ayarlarını tanımlar. Bu ilke tanımları, Denetim ve Azure sanal makinelerinde Log Analytics aracısını dağıtımını ve diğer Azure kaynak türlerini denetim ayarlarını yapılandırmaya zorlayın. Bu ilke tanımları da yapılandırması Azure kaynaklarını gerçekleştirilen işlemler hakkında bilgi sağlamak için tanılama günlüklerini denetleyin. Ayrıca, Denetim ve gelişmiş veri güvenliği, SQL Server üzerinde yapılandırılır.

- [Önizleme]: Denetim günlüğü analiz aracı dağıtımı - VM görüntüsü (OS) listeden kaldırıldı
- [Önizleme]: Denetim günlüğü analiz aracı dağıtım VMSS - VM görüntüsü (OS) listeden kaldırıldı
- [Önizleme]: Audit Log Analytics Workspace for VM - Report Mismatch
- [Önizleme]: Deploy Log Analytics Agent for Linux VM Scale Sets (VMSS)
- [Önizleme]: Deploy Log Analytics Agent for Linux VMs
- [Önizleme]: Deploy Log Analytics Agent for Windows VM Scale Sets (VMSS)
- [Önizleme]: Deploy Log Analytics Agent for Windows VMs
- [Önizleme]: Monitor unaudited SQL servers in Azure Security Center
- Ağ güvenlik grupları için tanılama ayarları uygula
- Tanılama ayarını denetle
- Gelişmiş veri güvenliği olmadan denetim yönetilen SQL örnekleri
- SQL sunucu düzeyi denetimi ayarları denetle
- Gelişmiş veri güvenliği olmadan denetim SQL sunucuları
- SQL sunucularında gelişmiş veri güvenliği dağıtma
- SQL sunucularında denetim dağıtma

## <a name="cm-7-2-least-functionality--prevent-program-execution"></a>CM-7 (2) en az işlevselliği | Program yürütme engelleme

Azure Güvenlik Merkezi'ndeki Uyarlamalı uygulama denetimlerini engelleyen veya üzerindeki sanal makinelerinize çalıştırılmasını belirli yazılım bir akıllı, otomatik uçtan uca uygulama beyaz listeye ekleme çözümüdür. Uygulama denetimi, onaylanmamış uygulamanın çalışmasını engelleyen bir zorlama modunda çalıştırabilirsiniz. Bu şema yardımcı olan tanımı izleme olduğu bir uygulama beyaz önerilir ancak henüz yapılandırılmamış sanal makineler bir Azure İlkesi'ni atar.

- [Önizleme]: Monitor possible app whitelisting in Azure Security Center

## <a name="cm-7-5-least-functionality--authorized-software--whitelisting"></a>CM-7 (5) en az işlevselliği | Yazılım yetkili / beyaz listeye ekleme

Azure Güvenlik Merkezi'ndeki Uyarlamalı uygulama denetimlerini engelleyen veya üzerindeki sanal makinelerinize çalıştırılmasını belirli yazılım bir akıllı, otomatik uçtan uca uygulama beyaz listeye ekleme çözümüdür. Uygulama denetimi, sanal makineleriniz için onaylı uygulama listelerini oluşturmanıza yardımcı olur. Bu şema atar bir [Azure İlkesi](../../../policy/overview.md) yardımcı olan tanımı olduğu bir uygulama beyaz önerilir ancak henüz yapılandırılmamış sanal makineler izleyin.

- [Önizleme]: Monitor possible app whitelisting in Azure Security Center

## <a name="cm-11-user-installed-software"></a>CM 11 kullanıcı tarafından yüklenen yazılım

Azure Güvenlik Merkezi'ndeki Uyarlamalı uygulama denetimlerini engelleyen veya üzerindeki sanal makinelerinize çalıştırılmasını belirli yazılım bir akıllı, otomatik uçtan uca uygulama beyaz listeye ekleme çözümüdür. Uygulama denetimi zorlamak ve yazılım kısıtlama ilkeleri ile uyumluluğu izlemenize yardımcı olabilir. Bu şema atar bir [Azure İlkesi](../../../policy/overview.md) yardımcı olan tanımı olduğu bir uygulama beyaz önerilir ancak henüz yapılandırılmamış sanal makineler izleyin.

- [Önizleme]: Monitor possible app whitelisting in Azure Security Center

## <a name="cp-7-alternate-processing-site"></a>CP-7 alternatif işleme sitesi

Azure Site Recovery, ikincil bir konuma birincil konumdan sanal makineler üzerinde çalışan iş yükleri çoğaltır. Kesinti birincil sitede meydana gelirse, iş yükü ikincil konum başarısız olur. Bu şema atar bir [Azure İlkesi](../../../policy/overview.md) sanal makineler yapılandırılmış olağanüstü durum kurtarma denetimleri tanımı. Bu gösterge izleme gereken yedek denetimlerinin olmanıza yardımcı olabilir.

- Olağanüstü durum kurtarma yapılandırılmış olmayan sanal makineleri denetle

## <a name="ia-2-1-identification-and-authentication-organizational-users--network-access-to-privileged-accounts"></a>IA-2 (1) kimliğini ve kimlik doğrulaması (Kurumsal kullanıcılar) | Ayrıcalıklı hesaplar için ağ erişimi

Bu şema kısıtlamak ve iki atayarak ayrıcalıklı erişimi denetlemeye yardımcı olan [Azure İlkesi](../../../policy/overview.md) sahip hesapları denetleme ve/veya yazma, çok faktörlü kimlik doğrulaması etkin olmayan izinleri tanımlar. Çok faktörlü kimlik doğrulaması, kimlik doğrulama bilgilerini bir parça tehlikede olsa bile hesapları korunmasına yardımcı olur. Çok faktörlü kimlik doğrulama etkin olmadan hesapları izleyerek, büyük olasılıkla tehlikeye hesapları tanımlayabilirsiniz.

- [Önizleme]: Audit accounts with owner permissions who are not MFA enabled on a subscription
- [Önizleme]: Audit accounts with write permissions who are not MFA enabled on a subscription

## <a name="ia-2-2-identification-and-authentication-organizational-users--network-access-to-non-privileged-accounts"></a>IA-2 (2) tanımlama ve kimlik doğrulaması (Kurumsal kullanıcılar) | Ağ erişimi olmayan ayrıcalıklı hesaplar

Bu şema kısıtlamak ve atayarak erişim denetlemenize yardımcı olur. bir [Azure İlkesi](../../../policy/overview.md) hesaplarıyla denetlemek için tanım okuma çok faktörlü kimlik doğrulaması etkin olmayan izinleri. Çok faktörlü kimlik doğrulaması, kimlik doğrulama bilgilerini bir parça tehlikede olsa bile hesapları korunmasına yardımcı olur. Çok faktörlü kimlik doğrulama etkin olmadan hesapları izleyerek, büyük olasılıkla tehlikeye hesapları tanımlayabilirsiniz.

- [Önizleme]: Audit accounts with read permissions who are not MFA enabled on a subscription

## <a name="ia-5-authenticator-management"></a>IA-5 Authenticator Yönetimi

Bu şema beş atar [Azure İlkesi](../../../policy/overview.md) parola olmadan hesaplardan gelen uzak bağlantılara izin vermek ve/veya doğru izinlere sahip Linux sanal makineleri denetle tanımları üzerinde parola dosyası ayarlayın. Bu plan, ayrıca Windows sanal makineleri için parola şifreleme türü conjugation denetimleri bir ilke tanımı atar. İzleme, sistem kimlik doğrulayıcılar, kuruluşunuzun tanımlama ve kimlik doğrulama İlkesi ile uyumlu olmak bu göstergeleri yardımcı olur.

- [Önizleme]: Audit that Linux VMs do not have accounts without passwords
- [Önizleme]: Deploy VM extension to audit that Linux VMs do not have accounts without passwords
- [Önizleme]: Audit that Linux VMs have the passwd file permissions set to 0644
- [Önizleme]: Audit that Windows VMs store passwords using reversible encryption
- [Önizleme]: Deploy VM extension to audit that Linux VMs have the passwd file permissions set to 0644

## <a name="ia-5-1-authenticator-management--password-based-authentication"></a>IA-5 (1) Authenticator Yönetim | Parola tabanlı kimlik doğrulaması

Bu şema güçlü parolalar 12 atayarak zorunlu yardımcı olan [Azure İlkesi](../../../policy/overview.md) en az gücü ve diğer parola gereksinimlerini zorlamaz; bir Windows sanal makineleri denetle tanımlar. Farkındalık parola gücü ilkeyi ihlal sanal makinelerin tüm sanal makine kullanıcı hesapları için parola kuruluşunuzun parola ilkesiyle uyumlu olmak için düzeltici eylemleri yardımcı olur.

- [Önizleme]: Audit that Windows VMs cannot re-use the previous 24 passwords
- [Önizleme]: Audit that Windows VMs have a maximum password age of 70 days
- [Önizleme]: Audit that Windows VMs have a minimum password age of 1 day
- [Önizleme]: Audit that Windows VMs have the password complexity setting enabled
- [Önizleme]: Audit that Windows VMs restrict the minimum password length to 14 characters
- [Önizleme]: Audit that Windows VMs store passwords using reversible encryption
- [Önizleme]: Deploy VM extension to audit that Windows VMs cannot re-use the previous 24 passwords
- [Önizleme]: Deploy VM extension to audit that Windows VMs have a maximum password age of 70 days
- [Önizleme]: Deploy VM extension to audit that Windows VMs have a minimum password age of 1 day
- [Önizleme]: Deploy VM extension to audit that Windows VMs have the password complexity setting enabled
- [Önizleme]: Deploy VM extension to audit that Windows VMs restrict the minimum password length to 14 characters
- [Önizleme]: Deploy VM extension to audit that Windows VMs store passwords using reversible encryption

## <a name="ra-5-vulnerability-scanning"></a>RA-5 güvenlik açığı taraması

Bu şema dört atayarak bilgi sistemi güvenlik açıkları yönetmenize yardımcı olan [Azure İlkesi](../../../policy/overview.md) işletim sistemi güvenlik açıkları, SQL güvenlik açıkları ve Azure sanal makine güvenlik açıkları izleyen tanımları Güvenlik Merkezi. Azure Güvenlik Merkezi, dağıtılan Azure kaynaklarınızın güvenlik durumunu gerçek zamanlı öngörülere sahip olmanıza olanak sağlayan bir raporlama yetenekleri sağlar. Bu plan Ayrıca, Denetim ve gelişmiş veri güvenliği zorla SQL sunucularında üç ilke tanımları atar. Gelişmiş veri güvenliği, güvenlik açığı değerlendirmesi ve dağıtılmış kaynaklarınızın güvenlik açıkları anlamanıza yardımcı olması için Gelişmiş tehdit koruma özellikleri dahil.

- Gelişmiş veri güvenliği olmadan denetim yönetilen SQL örnekleri
- Gelişmiş veri güvenliği olmadan denetim SQL sunucuları
- SQL sunucularında gelişmiş veri güvenliği dağıtma
- [Önizleme]: Audit OS vulnerabilities on your virtual machine scale sets in Azure Security Center
- [Önizleme]: Monitor OS vulnerabilities in Azure Security Center
- [Önizleme]: Monitor SQL vulnerability assessment results in Azure Security Center
- [Önizleme]: Monitor VM Vulnerabilities in Azure Security Center

## <a name="sc-5-denial-of-service-protection"></a>SC-5 koruma hizmet reddi

Azure'nın dağıtılmış engelleme (DDoS) hizmeti standart katman, temel hizmet katmanı ek özellikleri ve risk azaltma özellikleri sağlar. Bu ek özellikler, Azure İzleyici tümleştirmesi ve sonrası saldırısı riskini azaltma raporları gözden geçirme olanağı içerir. Bu şema atar bir [Azure İlkesi](../../../policy/overview.md) DDoS standart katmanı etkin olduğunda, denetimleri tanımı. Hizmet katmanları arasında özellik farkı anlama adresi reddi hizmet korumalarını Azure ortamınız için en iyi çözümü seçmenize yardımcı olabilir.

- [Önizleme]: Audit standard tier of DDoS protection is enabled for a virtual network

## <a name="sc-7-boundary-protection"></a>SC-7 sınır koruma

Bu şema yönetmenize ve sistem sınırı atayarak denetlemenize yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) , esnek kurallara sahip ağ güvenlik grupları izleyen tanımı. Çok esnek olan kuralları istenmeyen ağ erişimini izin verebilir ve gözden geçirilmesi gerekir. Bu şema için ağ güvenlik grubu sağlamlaştırma önerileri Azure Güvenlik Merkezi'nde izleyen bir ilke tanımı ayrıca atar. Azure Güvenlik Merkezi, sanal makinelerin Internet'e trafik düzenleriyle analiz eder ve ağ güvenlik grubu kuralı önerileri olası saldırı yüzeyini azaltmak için sağlar.
Ayrıca, bu plan, ayrıca korumasız uç noktalar, uygulamaları ve depolama hesaplarına izleyen üç ilke tanımları atar. Uç noktaları ve güvenlik duvarı ve depolama hesapları ile sınırsız erişim tarafından korunmayan uygulamalar bilgi sistemi içinde yer alan bilgileri istenmeyen erişime izin verebilir.

- [Önizleme]: Monitor Internet-facing virtual machines for Network Security Group traffic hardening recommendations
- [Önizleme]: Monitor permissive network access in Azure Security Center
- [Önizleme]: Monitor unprotected network endpoints in Azure Security Center
- [Önizleme]: Monitor unprotected web application in Azure Security Center
- Depolama hesapları sınırsız ağ erişimi denetleyin

## <a name="sc-7-3-boundary-protection--access-points"></a>SC-7 (3) sınır koruma | Erişim noktaları

Just-ın-time (JIT) sanal makine erişimi kilitler gelen trafiği Azure sanal makineler, Vm'lere gerektiğinde bağlanılabilmesi için kolay erişim sağlamanın yanı sıra saldırılara maruz kalma riskinizi azaltır. JIT VM erişimi, Azure kaynaklarınıza dış bağlantı sayısı sınırı yardımcı olur. Bu şema atar bir [Azure İlkesi](../../../policy/overview.md) yardımcı olan tanımı izleme sanal makinelerin tam zamanında erişim destekler ancak henüz yapılandırılmamış.

- [Önizleme]: Azure Güvenlik Merkezi'nde olası ağ yalnızca zamanında (JIT) erişimi izleme

## <a name="sc-7-4-boundary-protection--external-telecommunications-services"></a>SC-7 (4) sınır koruma | Dış telekomünikasyon Hizmetleri

Just-ın-time (JIT) sanal makine erişimi kilitler gelen trafiği Azure sanal makineler, Vm'lere gerektiğinde bağlanılabilmesi için kolay erişim sağlamanın yanı sıra saldırılara maruz kalma riskinizi azaltır. JIT VM erişimi, trafik akışı ilkesi için özel durumlar erişim isteği ve onay süreçlerinin kolaylaştırarak yönetmenize yardımcı olur. Bu şema atar bir [Azure İlkesi](../../../policy/overview.md) yardımcı olan tanımı izleme sanal makinelerin tam zamanında erişim destekler ancak henüz yapılandırılmamış.

- [Önizleme]: Azure Güvenlik Merkezi'nde olası ağ yalnızca zamanında (JIT) erişimi izleme

## <a name="sc-28-1-protection-of-information-at-rest--cryptographic-protection"></a>Bekleyen bilgi SC-28 (1) koruması | Şifreleme koruma

Bu şema 9 atayarak bekleyen bilgileri korumak için cryptograph denetimlerin kullanımına politikasını yardımcı olan [Azure İlkesi](../../../policy/overview.md) belirli cryptograph denetimlerini ve denetim zorunlu tanımları zayıf şifreleme kullanın Ayarlar. Burada Azure kaynaklarınıza uygun olmayan şifreleme yapılandırmaları olabilir kaynakların, bilgi Güvenlik ilkenize uygun şekilde yapılandırıldığından emin olmak için düzeltici eylemleri yardımcı olabileceğini anlama. Özellikle, bu şema tarafından atanan ilke tanımları, data lake depolama hesapları için şifreleme gerektirir; SQL veritabanlarında saydam veri şifrelemesi; ve SQL veritabanları, sanal makine disklerini ve Otomasyon hesabı değişkenleri eksik şifrelemesini denetler.

- [Önizleme]: Monitor unencrypted SQL databases in Azure Security Center
- [Önizleme]: Monitor unencrypted VM Disks in Azure Security Center
- Güvenli aktarım depolama hesapları için Denetim
- Gelişmiş veri güvenliği olmadan denetim yönetilen SQL örnekleri
- Gelişmiş veri güvenliği olmadan denetim SQL sunucuları
- Saydam veri şifreleme durumunu denetle
- SQL sunucularında gelişmiş veri güvenliği dağıtma
- SQL veritabanı saydam veri şifrelemesi Dağıt
- Data Lake Store hesaplarında şifrelemeyi zorla

## <a name="si-2-flaw-remediation"></a>SI-2 hatasını düzeltme

Bu şema bilgileri sistem standartlarımızı altı atayarak yönetmenize yardımcı olan [Azure İlkesi](../../../policy/overview.md) eksik sistem güncelleştirmeleri, işletim sistemi güvenlik açıkları, SQL güvenlik açıkları ve sanal makine izleyen tanımları Azure Güvenlik Merkezi'nde güvenlik açıklarını. Azure Güvenlik Merkezi, dağıtılan Azure kaynaklarınızın güvenlik durumunu gerçek zamanlı öngörülere sahip olmanıza olanak sağlayan bir raporlama yetenekleri sağlar. Bu plan Ayrıca işletim sisteminin sanal makine ölçek kümeleri için Otomatik yükseltme sağlayan bir ilke tanımı atar.

- [Önizleme]: Audit any missing system updates on virtual machine scale sets in Azure Security Center
- [Önizleme]: Audit OS vulnerabilities on your virtual machine scale sets in Azure Security Center
- [Önizleme]: Monitor missing system updates in Azure Security Center
- [Önizleme]: Monitor OS vulnerabilities in Azure Security Center
- [Önizleme]: Monitor SQL vulnerability assessment results in Azure Security Center
- [Önizleme]: Monitor VM Vulnerabilities in Azure Security Center
- Vmss'de uygulama durum denetimleriyle otomatik işletim sistemi yükseltmesi zorla

## <a name="si-3-malicious-code-protection"></a>SI-3 kötü amaçlı kod koruma

Bu şema endpoint protection kötü amaçlı kod koruması üç atayarak dahil yönetmenize yardımcı olan [Azure İlkesi](../../../policy/overview.md) eksik endpoint protection'ı Azure Güvenlik Merkezi'nde sanal makineler için izleyen tanımları ve Windows sanal makinelerde Microsoft kötü amaçlı yazılımdan koruma çözümü uygular.

- [Önizleme]: Audit the endpoint protection solution on virtual machine scale sets in Azure Security Center
- [Önizleme]: Monitor missing Endpoint Protection in Azure Security Center
- Windows Server için varsayılan Microsoft IaaSAntimalware uzantısını dağıtma

## <a name="si-3-1-malicious-code-protection--central-management"></a>SI-3 (1) kötü amaçlı kod koruma | Merkezi Yönetim

Bu şema endpoint protection kötü amaçlı kod, iki atayarak de dahil olmak üzere yönetmenize yardımcı olan [Azure İlkesi](../../../policy/overview.md) eksik endpoint protection'ı Azure Güvenlik Merkezi'nde sanal makineler için izleyen tanımları. Azure Güvenlik Merkezi, merkezi yönetim ve dağıtılan Azure kaynaklarınızın güvenlik durumunu gerçek zamanlı öngörülere sahip olmanızı sağlayan raporlama yetenekleri sağlar.

- [Önizleme]: Audit the endpoint protection solution on virtual machine scale sets in Azure Security Center
- [Önizleme]: Monitor missing Endpoint Protection in Azure Security Center

## <a name="si-4-information-system-monitoring"></a>SI-4 bilgi sistemi izleme

Bu şema denetim ve günlüğe kaydetme ve veri güvenliği Azure kaynaklarında zorlamayı tarafından sisteminize izlemenize yardımcı olur. Özellikle, atanan ilkeler, Denetim ve Log Analytics aracısını dağıtımının yanı sıra, SQL veritabanları, depolama hesapları ve ağ kaynakları için Gelişmiş güvenlik ayarları zorlayın. Bu özellikler, uygun eylemi böylece anormal davranış ve saldırı göstergeleri algılamanıza yardımcı olabilir.

- [Önizleme]: Denetim günlüğü analiz aracı dağıtımı - VM görüntüsü (OS) listeden kaldırıldı
- [Önizleme]: Denetim günlüğü analiz aracı dağıtım VMSS - VM görüntüsü (OS) listeden kaldırıldı
- [Önizleme]: Audit Log Analytics Workspace for VM - Report Mismatch
- [Önizleme]: Deploy Log Analytics Agent for Linux VM Scale Sets (VMSS)
- [Önizleme]: Deploy Log Analytics Agent for Linux VMs
- [Önizleme]: Deploy Log Analytics Agent for Windows VM Scale Sets (VMSS)
- [Önizleme]: Deploy Log Analytics Agent for Windows VMs
- Gelişmiş veri güvenliği olmadan denetim yönetilen SQL örnekleri
- Gelişmiş veri güvenliği olmadan denetim SQL sunucuları
- SQL sunucularında gelişmiş veri güvenliği dağıtma
- Gelişmiş tehdit koruması depolama hesaplarında dağıtma
- SQL sunucularında denetim dağıtma
- Sanal ağlar oluşturulduğunda Ağ İzleyicisi Dağıt
- SQL sunucularında tehdit algılama dağıtma

## <a name="si-4-18-information-system-monitoring--analyze-traffic--covert-exfiltration"></a>SI-4 (18) bilgi sistemi izleme | Trafiğini analiz / gizli Sızdırma

Azure depolama için Gelişmiş tehdit koruması, erişim veya depolama hesapları yararlanma olağan dışı ve zararlı olabilecek girişimleri algılar. Koruma uyarıları, anormal erişim desenleri, anormal ayıklar/karşıya yüklemelerden ve depolama şüpheli etkinlik içerir. Bu göstergeler bilgileri gizli Sızdırma algılamanıza yardımcı olabilir.

- Gelişmiş tehdit koruması depolama hesaplarında dağıtma

## <a name="next-steps"></a>Sonraki adımlar

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Şema yaşam döngüsü](../../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../../how-to/update-existing-assignments.md) öğrenin.