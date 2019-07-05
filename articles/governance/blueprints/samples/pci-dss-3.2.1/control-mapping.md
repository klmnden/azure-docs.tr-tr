---
title: Örnek - PCI-DSS v3.2.1 şema - denetim eşleme
description: Azure İlkesi ve RBAC ödeme kartı sektör veri güvenliği standardı v3.2.1 şema örnek eşleme denetimi.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 06/24/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 38b1cc6249da98e11167416c8e18d06de1645679
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67540940"
---
# <a name="control-mapping-of-the-pci-dss-v321-blueprint-sample"></a>PCI-DSS v3.2.1 şema örnek denetimi eşleme

Aşağıdaki makalede, Azure şemaları PCI-DSS v3.2.1 şema örnek PCI-DSS v3.2.1 denetimlerinin nasıl eşlendiğini ayrıntıları. Denetimler hakkında daha fazla bilgi için bkz. [PCI-DSS v3.2.1](https://www.pcisecuritystandards.org/documents/PCI_DSS_v3-2-1.pdf).

Şu eşlemeler üzeresiniz **PCI-DSS v3.2.1:2018** kontrol eder. Gezinti, doğrudan bir özel denetim eşlemesi atlamak için sağ taraftaki kullanın. Eşlenen denetimleri birçoğu ile uygulanan bir [Azure İlkesi](../../../policy/overview.md) girişim. Tam girişim gözden geçirmek için açık **ilke** Azure portal ve select **tanımları** sayfası. Daha sonra bulmak ve seçmek  **[Önizleme] denetim PCI v3.2.1:2018 denetler ve denetim gereksinimlerini desteklemek için belirli VM uzantılarını dağıtma** yerleşik ilke girişimi.

## <a name="132-and-134-boundary-protection"></a>1.3.2 ve 1.3.4 sınır koruma

Bu şema yönetmenize ve denetlemenize ağları atayarak yardımcı [Azure İlkesi](../../../policy/overview.md) , esnek kurallara sahip ağ güvenlik grupları izleyen tanımları. Çok esnek olan kuralları istenmeyen ağ erişimini izin verebilir ve gözden geçirilmesi gerekir. Bu şema korumasız uç noktalar, uygulamaları ve depolama hesaplarına izleme Azure ilke tanımları atar. Uç noktaları ve güvenlik duvarı ve depolama hesapları ile sınırsız erişim tarafından korunmayan uygulamalar bilgi sistemi içinde yer alan bilgileri istenmeyen erişime izin verebilir.

- Depolama hesapları sınırsız ağ erişimi denetleyin
- Internet'e yönelik uç nokta erişimin kısıtlanması gerekir

## <a name="34a-41-41g-41h-and-653-cryptographic-protection"></a>3.4.a, 4.1 4.1.g ve 4.1.h 6.5.3 şifreleme koruma

Bu şema atayarak, politikasını cryptograph denetimleri kullanarak yardımcı olan [Azure İlkesi](../../../policy/overview.md) hangi belirli cryptograph denetimlerini ve denetim zorunlu tanımları zayıf şifreleme ayarlarını kullanır. Burada Azure kaynaklarınıza uygun olmayan şifreleme yapılandırmaları olabilir kaynakların, bilgi Güvenlik ilkenize uygun şekilde yapılandırıldığından emin olmak için düzeltici eylemleri yardımcı olabileceğini anlama. Özellikle, bu şema tarafından atanan ilkeler SQL veritabanlarında saydam veri şifrelemesi gerektirir; Depolama hesapları ve Otomasyon hesabı değişkenleri eksik şifreleme denetim. Hangi adresi depolama hesapları, işlev uygulamaları, WebApp, API Apps ve Redis Cache için güvenli bağlantıları denetim ve şifrelenmemiş Service Fabric iletişim denetim ilkeleri vardır.

- İşlev uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır
- Web uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır
- API uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır
- Azure Güvenlik Merkezi'nde şifrelenmemiş SQL veritabanını İzle
- Sanal makinelerde disk şifrelemesini uygulanmalıdır
- Otomasyon hesabı değişkenleri şifrelenmelidir
- Yalnızca güvenli bağlantılar Redis Cache'inizi etkinleştirilmelidir.
- Güvenli aktarım depolama hesapları için etkinleştirilmiş olmalıdır
- Service Fabric kümeleri ClusterProtectionLevel özelliği EncryptAndSign olarak ayarlanmış olmalıdır
- SQL veritabanlarında saydam veri şifrelemesi etkinleştirilmelidir.
- SQL veritabanı saydam veri şifrelemesi Dağıt

## <a name="51-62-66-and-1121-vulnerability-scanning-and-system-updates"></a>5.1, 6.2, 6.6 ve 11.2.1 güvenlik açığı taraması ve sistem güncelleştirmeleri

Bu şema atayarak bilgi sistemi güvenlik açıkları yönetmenize yardımcı olan [Azure İlkesi](../../../policy/overview.md) eksik sistem güncelleştirmeleri, işletim sistemi güvenlik açıkları, SQL güvenlik açıkları ve sanal makine izleyen tanımları Azure Güvenlik Merkezi'nde güvenlik açıklarını. Azure Güvenlik Merkezi, dağıtılan Azure kaynaklarınızın güvenlik durumunu gerçek zamanlı öngörülere sahip olmanıza olanak sağlayan bir raporlama yetenekleri sağlar.

- Azure Güvenlik Merkezi'nde eksik Endpoint Protection'ı izleme
- Windows Server için varsayılan Microsoft IaaSAntimalware uzantısını dağıtma
- SQL sunucularında tehdit algılama dağıtma
- Sistem güncelleştirmeleri makinelerinizde yüklü olması gerekir
- Güvenlik Yapılandırması makinelerinizde güvenlik açıkları düzeltilen
- Güvenlik açıklarını SQL veritabanlarınızda düzeltilen
- Güvenlik açıklarını bir güvenlik açığı değerlendirme çözümü tarafından düzeltilen

## <a name="711-712-and-713-separation-of-duties"></a>7.1.1. Görevler 7.1.2 ve 7.1.3'ten ayrımı

Tek bir Azure aboneliği sahibi olan Yönetim yedeklilik için izin vermez. Buna karşılık, çok fazla Azure aboneliğine sahip olan, güvenliği aşılmış sahip hesabı aracılığıyla bir ihlal olasılığını artırabilir. Bu şema atayarak Azure abonelik sahipleri uygun sayıda tutmanıza yardımcı olur. [Azure İlkesi](../../../policy/overview.md) hangi sahip Azure aboneliklerinin sayısı denetim tanımları. Abonelik sahibi izinleri yönetme, uygun görev ayrımı uygulamak yardımcı olabilir.

- Aboneliğinize atanmış birden fazla sahibi olmalıdır.
- En fazla 3 sahipleri, aboneliğiniz için belirlenen 

## <a name="32-721-831a-and-831b-management-of-privileged-access-rights"></a>3.2, 7.2.1, ayrıcalıklı erişim haklarının 8.3.1.a ve 8.3.1.b Yönetimi

Bu şema kısıtlamak ve ayrıcalıklı erişim hakları atayarak denetlemenize yardımcı olan [Azure İlkesi](../../../policy/overview.md) sahibi dış hesapların denetlemek için tanımları yazma ve/veya Okuma izinleri ve çalışan hesapları sahibi ve/veya yazma çok faktörlü kimlik doğrulaması etkin olmayan izinler. Azure rol tabanlı erişim denetimi (RBAC) kimlerin erişebildiğini yönetmek için Azure kaynaklarına uygular. Özel bir RBAC kurallar hata yapmaya açık olarak özel bir RBAC kuralları uygulamak nerede anlama, gereksinim ve uygun uygulama doğrulamanıza yardımcı olabilir. Bu blueprint ayrıca atar [Azure İlkesi](../../../policy/overview.md) SQL sunucuları için Azure Active Directory kimlik doğrulaması kullanımını denetlemek için tanımları. Azure Active Directory'yi kullanarak kimlik doğrulaması izin yönetimini kolaylaştırır ve veritabanı kullanıcı ve diğer Microsoft kimlik yönetimini merkezileştirir  
Hizmetler.
 
- Sahip izinlerine sahip dış hesapların aboneliğinizden kaldırılması gerekiyor
- Yazma izinleri olan dış hesapların aboneliğinizden kaldırılması gerekir
- Okuma izinleri olan dış hesapların aboneliğinizden kaldırılması gerekiyor
- Aboneliğinizde sahip izinleri ile hesapları MFA etkinleştirilmelidir
- MFA, aboneliğinizde etkin hesaplar yazma izinlerine sahip olmalıdır
- Aboneliğinizde Okuma izinleri olan hesaplar MFA etkinleştirilmelidir
- SQL sunucuları için bir Azure Active Directory Yöneticisi sağlanmalıdır
- Özel bir RBAC kurallarının kullanımını denetleme

## <a name="812-and-815-least-privilege-and-review-of-user-access-rights"></a>8.1.2 ve en az ayrıcalık 8.1.5 ve kullanıcı erişim haklarını gözden geçirilmesi

Azure, Azure'da kaynaklara erişimi olan rol tabanlı erişim denetimine (RBAC) yönetmenize yardımcı olan uygular. Azure portalını kullanarak, kimlerin erişebileceğini Azure kaynaklarını ve izinlerini gözden geçirebilirsiniz. Bu şema atar [Azure İlkesi](../../../policy/overview.md) tanımlarını gözden geçirme için amorti edilmiş ve yükseltilmiş izinleri olan dış hesapları dahil olmak üzere, öncelik verilmesi gerektiği hesapları denetleme.

- Kullanım dışı bırakılan hesapların aboneliğinizden kaldırılması gerekiyor
- Sahip izinleri ile kullanım dışı bırakılmış hesapların aboneliğinizden kaldırılması gerekiyor
- Sahip izinlerine sahip dış hesapların aboneliğinizden kaldırılması gerekiyor
- Yazma izinleri olan dış hesapların aboneliğinizden kaldırılması gerekir
- Okuma izinleri olan dış hesapların aboneliğinizden kaldırılması gerekiyor

## <a name="813-removal-or-adjustment-of-access-rights"></a>8.1.3 kaldırma veya erişim haklarını ayarlama

Azure rol tabanlı erişim denetimi (RBAC) kimlerin erişebileceğini yönetmenize yardımcı olmak üzere azure'daki kaynaklara uygular. Azure Active Directory ve RBAC kullanarak, kullanıcı rolleri, kuruluş değişiklikleri yansıtacak şekilde güncelleştirebilirsiniz. Gerektiğinde, hesapları oturum açma engellendi (veya kaldırıldı) hangi Azure kaynakları için erişim haklarını hemen kaldırır. Bu şema atar [Azure İlkesi](../../../policy/overview.md) kaldırılmak üzere davranılacak amorti edilmiş hesabı denetlemek için tanımları.

- Kullanım dışı bırakılan hesapların aboneliğinizden kaldırılması gerekiyor
- Sahip izinleri ile kullanım dışı bırakılmış hesapların aboneliğinizden kaldırılması gerekiyor

## <a name="823ab-824ab-and-825-password-based-authentication"></a>8.2.3.a,b ve 8.2.4.a,b 8.2.5 parola tabanlı kimlik doğrulaması

Bu şema atayarak güçlü parolalar zorunlu yardımcı olan [Azure İlkesi](../../../policy/overview.md) en az gücü ve diğer parola gereksinimlerini zorlamaz; Windows Vm'leri denetle tanımlar. Parola gücü ilkeyi ihlal VM'lerin tanıma tüm VM kullanıcı hesapları için parola ilkesiyle uyumlu olduğundan emin olmak için düzeltici eylemleri yardımcı olur.

- [Önizleme]: Audit Windows VMs that do not have a maximum password age of 70 days
- [Önizleme]: Deploy requirements to audit Windows VMs that do not have a maximum password age of 70 days
- [Önizleme]: Audit Windows VMs that do not restrict the minimum password length to 14 characters
- [Önizleme]: Deploy requirements to audit Windows VMs that do not restrict the minimum password length to 14 characters
- [Önizleme]: Audit Windows VMs that allow re-use of the previous 24 passwords
- [Önizleme]: Deploy requirements to audit Windows VMs that allow re-use of the previous 24 passwords

## <a name="103-and-1054-audit-generation"></a>10.3 ve 10.5.4 nesil denetleme

Bu şema sistem olaylarını atayarak kaydedilir olmanıza yardımcı olur. [Azure İlkesi](../../../policy/overview.md) Azure kaynaklarını denetim günlüğü ayarlarını tanımlar.
Tanılama günlükleri, Azure kaynaklarını içinde gerçekleştirilen işlemler hakkında bilgi sağlar. Azure günlüklerini eşitlenmiş iç saat kaynakları genelinde olayların süresi bağıntılı bir kayıt oluşturmak için kullanır.

- Azure Güvenlik Merkezi'nde denetlenmeyen SQL sunucularını izleme
- Tanılama ayarını denetle
- SQL sunucu düzeyi denetimi ayarları denetle
- SQL sunucularında denetim dağıtma
- Yeni Azure Resource Manager kaynaklarına depolama hesapları geçirilmelidir
- Sanal makineler için yeni Azure Resource Manager kaynaklarını geçirilmelidir

## <a name="1236-and-1237-information-security"></a>12.3.6 ve 12.3.7 bilgi güvenliği

Bu şema yönetmenize ve ağınızı atayarak denetlemenize yardımcı olur. [Azure İlkesi](../../../policy/overview.md) kabul edilebilir ağ konumlarını ve onaylanan şirket ürünleri denetim tanımları ortam için izin verilir. Bu, bu ilkelerin her birinde içinde ilke parametreleri aracılığıyla her bir şirket tarafından özelleştirilebilir.

- İzin verilen konumlar
- İzin verilen konumlar için kaynak grupları

## <a name="next-steps"></a>Sonraki adımlar

PCI-DSS v3.2.1 blueprint'in denetimi eşleme gözden geçirdikten sonra genel bakış ve bu örneğin nasıl dağıtılacağını öğrenmek için aşağıdaki makaleleri ziyaret edin:

> [!div class="nextstepaction"]
> [PCI-DSS v3.2.1 şema - genel bakış](./index.md)
> [PCI-DSS v3.2.1 blueprint - adımları dağıtma](./deploy.md)

## <a name="addition-articles-about-blueprints-and-how-to-use-them"></a>Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Şema yaşam döngüsü](../../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../../how-to/update-existing-assignments.md) öğrenin.
