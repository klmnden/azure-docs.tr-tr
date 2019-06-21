---
title: Örnek - UK resmi şema - denetim eşleme
description: UK resmi şema örnek eşleme denetimi.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 06/13/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 2eb6e62bc891a147a89107f5e3de7c2b605bbd09
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67276983"
---
# <a name="control-mapping-of-the-uk-official-blueprint-sample"></a>UK resmi şema örnek denetimi eşleme

Aşağıdaki makalede UK resmi şema örnek Birleşik Krallık resmi denetimlerinin nasıl eşlendiğini ayrıntıları.
Denetimler hakkında daha fazla bilgi için bkz. [UK resmi](https://www.gov.uk/government/publications/government-security-classifications).

Şu eşlemeler üzeresiniz **UK resmi** kontrol eder. Gezinti, doğrudan bir özel denetim eşlemesi atlamak için sağ taraftaki kullanın. Eşlenen denetimleri birçoğu ile uygulanan bir [Azure İlkesi](../../../policy/overview.md) girişim. Tam girişim gözden geçirmek için açık **ilke** Azure portal ve select **tanımları** sayfası. Daha sonra bulmak ve seçmek  **[Önizleme] UK resmi denetim denetler ve denetim gereksinimlerini desteklemek için belirli VM uzantılarını dağıtma** yerleşik ilke girişimi.

## <a name="1-data-in-transit-protection"></a>1 veri aktarım koruma

Blueprint bilgi aktarımı Azure Hizmetleri ile güvenli iki atayarak olmanıza yardımcı olur. [Azure İlkesi](../../../policy/overview.md) depolama hesapları ve Redis önbelleği için güvenli bağlantıları denetlemek için tanımları.

- Yalnızca güvenli bağlantılar Redis Cache'inizi etkinleştirilmelidir.
- Güvenli aktarım depolama hesapları için etkinleştirilmiş olmalıdır

## <a name="23-data-at-rest-protection"></a>2.3 veri rest koruma

Bu şema 11 atayarak, politikasını cryptograph denetimlerin kullanımına yardımcı olan [Azure İlkesi](../../../policy/overview.md) belirli cryptograph denetimlerini ve denetim zorunlu tanımları zayıf şifreleme ayarları kullanın.
Burada Azure kaynaklarınıza uygun olmayan şifreleme yapılandırmaları olabilir kaynakların, bilgi Güvenlik ilkenize uygun şekilde yapılandırıldığından emin olmak için düzeltici eylemleri yardımcı olabileceğini anlama. Özellikle, bu şema tarafından atanan ilkeler şifreleme data lake depolama hesapları için gerektirir; SQL veritabanlarında saydam veri şifrelemesi; Depolama hesapları, SQL veritabanları, sanal makine disklerini ve Otomasyon hesabı değişkenleri eksik şifrelemesini denetle; Depolama hesapları ve Redis önbelleği için güvenli bağlantıları denetim; sanal makine zayıf parola şifrelemesini denetler; ve Service Fabric şifrelenmemiş iletişimin denetleyebilirsiniz.

- Azure Güvenlik Merkezi'nde şifrelenmemiş SQL veritabanlarını izleme
- Sanal makinelerde disk şifrelemesini uygulanmalıdır
- Otomasyon hesabı değişkenleri şifrelenmelidir
- Güvenli aktarım depolama hesapları için etkinleştirilmiş olmalıdır
- Güvenli aktarım depolama hesapları için etkinleştirilmiş olmalıdır
- Service Fabric kümeleri ClusterProtectionLevel özelliği EncryptAndSign olarak ayarlanmış olmalıdır
- SQL veritabanlarında saydam veri şifrelemesi etkinleştirilmelidir.
- SQL veritabanı saydam veri şifrelemesi Dağıt
- Data Lake Store hesapları şifreleme iste
- İzin verilen konumlar (kodlanmış "UK Güney" ve "UK Batı" için sabit kaldırıldı)
- İzin verilen konumlar için kaynak gruplarını (kodlanmış "UK Güney" ve "UK Batı" için sabit kaldırıldı)

## <a name="52-vulnerability-management"></a>5.2 güvenlik açığı Yönetimi

Bu şema beş atayarak bilgi sistemi güvenlik açıkları yönetmenize yardımcı olan [Azure İlkesi](../../../policy/overview.md) eksik sistem güncelleştirmeleri, işletim sistemi güvenlik açıkları, SQL eksik endpoint protection izleyen tanımları Güvenlik Açıkları ve sanal makine güvenlik açıklarını. Bu Öngörüler hakkında dağıtılmış kaynaklarınızın güvenlik durumunu gerçek zamanlı bilgi sağlayan ve düzeltme eylemleri önceliğini belirlemeye yardımcı olabilir.

- Azure Güvenlik Merkezi'nde eksik Endpoint Protection'ı izleme
- Sistem güncelleştirmeleri makinelerinizde yüklü olması gerekir
- Güvenlik Yapılandırması makinelerinizde güvenlik açıkları düzeltilen
- Güvenlik açıklarını SQL veritabanlarınızda düzeltilen
- Güvenlik açıklarını bir güvenlik açığı değerlendirme çözümü tarafından düzeltilen

## <a name="53-protective-monitoring"></a>5.3 koruyucu izleme

Bu şema atayarak bilgi sistemi varlıkları korumanıza yardımcı olur. bir [Azure İlkesi](../../../policy/overview.md) izleyen tanımı sınırsız depolama hesapları. Bu plan Ayrıca, beyaz liste etkinliği izleyen bir Azure İlkesi tanım atar.

- Depolama hesapları sınırsız ağ erişimi denetleyin
- Uyarlamalı uygulama denetimleri sanal makinelerde etkinleştirilmelidir.
- SQL sunucularında tehdit algılama dağıtma
- Windows Server için varsayılan Microsoft Iaas kötü amaçlı yazılımdan koruma uzantısını dağıtma

## <a name="9-secure-user-management--10-identity-and-authentication"></a>9 güvenli kullanıcı yönetimi / 10 kimlik ve kimlik doğrulaması

Azure, Azure'da kaynaklara erişimi olan rol tabanlı erişim denetimine (RBAC) yönetmenize yardımcı olan uygular. Azure portalını kullanarak, kimlerin erişebileceğini Azure kaynaklarını ve izinlerini gözden geçirebilirsiniz. Bu şema kısıtlamak ve altı atayarak ayrıcalıklı erişim hakları denetlemenize yardımcı olur. [Azure İlkesi](../../../policy/overview.md) sahip dış hesapların denetim ve/veya okuma/yazma izinleri ve hesapları, sahibi okuyun ve/veya yazma için tanımları çok faktörlü kimlik doğrulaması etkin olmayan izinler.

- Aboneliğinizde sahip izinleri ile hesapları MFA etkinleştirilmelidir
- MFA, aboneliğinizde etkin hesaplar yazma izinlerine sahip olmalıdır
- Aboneliğinizde Okuma izinleri olan hesaplar MFA etkinleştirilmelidir
- Bir abonelikte sahip izinleri olan dış hesapları denetleme
- Bir abonelikteki yazma izinleri olan dış hesapları denetleyin
- Bir Abonelikteki Okuma izinleri olan dış hesapları denetleme

Bu şema SQL Server'lar ve Service Fabric için Azure Active Directory kimlik doğrulaması kullanımını denetlemek için iki Azure ilke tanımları atar. Azure Active Directory'yi kullanarak kimlik doğrulaması, Basitleştirilmiş izin yönetimi ve veritabanı kullanıcıları ve diğer Microsoft hizmetlerinde merkezi kimlik yönetimi sağlar.

- SQL sunucuları için bir Azure Active Directory Yöneticisi sağlanmalıdır
- Yalnızca Azure Active Directory Service Fabric kümeleri için istemci kimlik doğrulaması kullanmanız gerekir

Bu şema ayrıca amorti edilmiş ve dış hesapları dahil olmak üzere, gözden geçirmeniz için öncelik hesapları denetlemek için beş Azure ilke tanımları atar. Gerektiğinde, hesapları oturum açma engellendi (veya kaldırıldı) hangi Azure kaynakları için erişim haklarını hemen kaldırır. Bu şema iki Azure İlkesi tanım kaldırılmak üzere davranılacak amorti denetim hesabına atar.

- Kullanım dışı bırakılan hesapların aboneliğinizden kaldırılması gerekiyor
- Sahip izinleri ile kullanım dışı bırakılmış hesapların aboneliğinizden kaldırılması gerekiyor
- Sahip izinlerine sahip dış hesapların aboneliğinizden kaldırılması gerekiyor
- Yazma izinleri olan dış hesapların aboneliğinizden kaldırılması gerekir

Bu şema, ayrıca Linux VM denetimleri bir Azure İlkesi tanım yanlış ayarladıysanız, sizi uyarmak için parola dosya izinleri atar. Bu tasarım, Doğrulayıcı tehlikede olmayan emin olmak için düzeltici eylemlerde sağlar.

- [Önizleme]: Audit Linux VM /etc/passwd file permissions are set to 0644

Bu şema güçlü parolalar en az gücü ve diğer parola gereksinimlerini zorlamaz; Windows Vm'leri denetle 10 Azure ilke tanımları atayarak zorunlu yardımcı olur. Parola gücü ilkeyi ihlal VM'lerin tanıma tüm VM kullanıcı hesapları için parola ilkesiyle uyumlu olduğundan emin olmak için düzeltici eylemleri yardımcı olur.

- [Önizleme]: Deploy requirements to audit Windows VMs that do not have the password complexity setting enabled
- [Önizleme]: Deploy requirements to audit Windows VMs that do not have a maximum password age of 70 days
- [Önizleme]: Deploy requirements to audit Windows VMs that do not have a minimum password age of 1 day
- [Önizleme]: Deploy requirements to audit Windows VMs that do not restrict the minimum password length to 14 characters
- [Önizleme]: Deploy requirements to audit Windows VMs that allow re-use of the previous 24 passwords
- [Önizleme]: Audit Windows VMs that do not have the password complexity setting enabled
- [Önizleme]: Audit Windows VMs that do not have a maximum password age of 70 days
- [Önizleme]: Audit Windows VMs that do not have a minimum password age of 1 day
- [Önizleme]: Audit Windows VMs that do not restrict the minimum password length to 14 characters
- [Önizleme]: Audit Windows VMs that allow re-use of the previous 24 passwords

Bu blueprint ayrıca yedi Azure ilke tanımları atayarak Azure kaynaklarına erişimini denetlemesine yardımcı olur. Bu ilkeler kaynak türlerinin kullanımını denetleyin ve daha esnek olarak izin verebileceği yapılandırmaları kaynaklarına erişim. Yetkili kullanıcıların Azure kaynaklarına erişim sağlamak için düzeltici eylemleri Yardım kısıtlanmış, bu ilkeleri can ihlal anlama kaynaklar.

- [Önizleme]: Deploy requirements to audit Linux VMs that have accounts without passwords
- [Önizleme]: Deploy requirements to audit Linux VMs that allow remote connections from accounts without passwords
- [Önizleme]: Audit Linux VMs that have accounts without passwords
- [Önizleme]: Audit Linux VMs that allow remote connections from accounts without passwords
- Yeni Azure Resource Manager kaynaklarına depolama hesapları geçirilmelidir
- Sanal makineler için yeni Azure Resource Manager kaynaklarını geçirilmelidir
- Yönetilen disk kullanmayan Vm'leri denetle

## <a name="11-external-interface-protection"></a>11 dış arabirimi koruma

25'ten fazla ilkeleri için uygun güvenli kullanıcı yönetimi kullanarak dışında bu şema atayarak Hizmet Arabirimleri yetkisiz erişimden korunmasına yardımcı olur bir [Azure İlkesi](../../../policy/overview.md) izleyiciler sınırsız tanımı Depolama hesabı. Sınırsız erişimi olan depolama hesapları, bilgi sistemi içinde yer alan bilgileri istenmeyen erişime izin verebilir. Bu plan Ayrıca, Uyarlamalı uygulama denetimleri sanal makinelerde sağlayan bir ilke de atar.

- Depolama hesapları sınırsız ağ erişimi denetleyin
- Uyarlamalı uygulama denetimleri sanal makinelerde etkinleştirilmelidir.

## <a name="12-secure-service-administration"></a>12 güvenli Hizmet Yönetimi

Azure, Azure'da kaynaklara erişimi olan rol tabanlı erişim denetimine (RBAC) yönetmenize yardımcı olan uygular. Azure portalını kullanarak, kimlerin erişebileceğini Azure kaynaklarını ve izinlerini gözden geçirebilirsiniz. Bu şema kısıtlamak ve ayrıcalıklı erişim hakları beş atayarak denetlemenize yardımcı olur. [Azure İlkesi](../../../policy/overview.md) sahip dış hesapların denetim ve/veya izinleri ve hesapları sahip yazma ve/veya yazma izinleri için tanımları çok faktörlü kimlik doğrulaması etkin sahip değil.

Bir bulut hizmetinin yönetimi için kullanılan sistemler ilgili hizmete erişim sağlamaya yüksek ayrıcalıklı. Kendi güvenliğinin aşılmasına güvenlik denetimlerini atlama çalabilir ve büyük miktarda veriyi işlemek için araçlar da dahil olmak üzere, önemli bir etkisi yoktur. Tüm güvenlik hizmeti olumsuz kötüye kullanılma riski azaltmak için işletimsel hizmetini yönetmek için hizmet sağlayıcısının yöneticileri tarafından kullanılan yöntemleri tasarlanmalıdır. Bu ilkesine uygulanmadı, bir saldırganın güvenlik denetimlerini atlama çalabilir ve büyük miktarda veriyi işlemek için yol olabilir.

- Aboneliğinizde sahip izinleri ile hesapları MFA etkinleştirilmelidir
- MFA, aboneliğinizde etkin hesaplar yazma izinlerine sahip olmalıdır
- Sahip izinlerine sahip dış hesapların aboneliğinizden kaldırılması gerekiyor
- Yazma izinleri olan dış hesapların aboneliğinizden kaldırılması gerekir

Bu şema SQL Server'lar ve Service Fabric için Azure Active Directory kimlik doğrulaması kullanımını denetlemek için iki Azure ilke tanımları atar. Azure Active Directory'yi kullanarak kimlik doğrulaması, Basitleştirilmiş izin yönetimi ve veritabanı kullanıcıları ve diğer Microsoft hizmetlerinde merkezi kimlik yönetimi sağlar.

- SQL sunucuları için bir Azure Active Directory Yöneticisi sağlanmalıdır
- Yalnızca Azure Active Directory Service Fabric kümeleri için istemci kimlik doğrulaması kullanmanız gerekir

Bu şema dört Azure ilke tanımlarını gözden geçirme için amorti edilmiş ve yükseltilmiş izinleri olan dış hesapları dahil olmak üzere, öncelik verilmesi gerektiği hesapları denetlemek için de atar. Gerektiğinde, hesapları oturum açma engellendi (veya kaldırıldı) hangi Azure kaynakları için erişim haklarını hemen kaldırır. Bu şema iki Azure İlkesi tanım kaldırılmak üzere davranılacak amorti denetim hesabına atar.

- Kullanım dışı bırakılan hesapların aboneliğinizden kaldırılması gerekiyor
- Sahip izinleri ile kullanım dışı bırakılmış hesapların aboneliğinizden kaldırılması gerekiyor
- Sahip izinlerine sahip dış hesapların aboneliğinizden kaldırılması gerekiyor
- Yazma izinleri olan dış hesapların aboneliğinizden kaldırılması gerekir

Bu şema, ayrıca Linux VM denetimleri bir Azure İlkesi tanım yanlış ayarladıysanız, sizi uyarmak için parola dosya izinleri atar. Bu tasarım, Doğrulayıcı tehlikede olmayan emin olmak için düzeltici eylemlerde sağlar.

- [Önizleme]: Audit Linux VM /etc/passwd file permissions are set to 0644

## <a name="13-audit-information-for-users"></a>Kullanıcılar için 13 denetim bilgileri

Bu şema sistem olaylarını 6 atayarak kaydedilir olmanıza yardımcı olur. [Azure İlkesi](../../../policy/overview.md) Azure kaynaklarını denetim günlüğü ayarlarını tanımlar. Atanmış bir ilke, aynı zamanda sanal makine için belirtilen log analytics çalışma alanı günlükleri göndermeyen olup olmadığını denetler.

- Azure Güvenlik Merkezi'nde denetlenmeyen SQL sunucularını izleme
- Tanılama ayarını denetle
- SQL sunucu düzeyi denetimi ayarları denetle
- [Önizleme]: Deploy Log Analytics Agent for Linux VMs
- [Önizleme]: Deploy Log Analytics Agent for Windows VMs
- Sanal ağlar oluşturulduğunda Ağ İzleyicisi Dağıt

## <a name="next-steps"></a>Sonraki adımlar

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Şema yaşam döngüsü](../../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../../how-to/update-existing-assignments.md) öğrenin.
