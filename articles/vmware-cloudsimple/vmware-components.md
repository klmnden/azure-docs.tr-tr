---
title: Azure VMware çözümü CloudSimple - özel bir buluta VMware bileşenleri tarafından
description: VMware bileşenleri üzerine özel buluta nasıl yüklendiğini açıklar
author: sharaths-cs
ms.author: dikamath
ms.date: 04/30/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 62511118edb4f8b5061f90138bac2aa2b5d3cfe3
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165145"
---
# <a name="private-cloud-vmware-components"></a>Özel bulut VMware bileşenleri

Özel bir bulut yalıtılmış bir VMware yığınınızın (ESXi konaklarını ve vCenter, Vsan'a ve NSX) olan bir yönetim etki alanındaki bir vCenter sunucusu tarafından yönetilen ortamı.  CloudSimple hizmet VMware Azure konumları Azure çıplak metal altyapısı üzerinde yerel olarak dağıtmanızı sağlar.  Özel bulut, Azure bulut geri kalanı ile tümleştirilmiştir.  Bir özel bulut, aşağıdaki VMware yığın bileşenleri ile dağıtılır:

* **VMware ESXi -** hiper yönetici azure'da adanmış düğümler
* **VMware vCenter -** Merkezi Yönetimi için vSphere ortam özel bulut Gereci
* **VMware vSAN -** hiper yakınsama altyapısı çözümünü
* **VMware NSX veri merkezi -** ağ sanallaştırma ve güvenlik yazılımı  

## <a name="vmware-component-versions"></a>VMware bileşen sürümü

Bir özel bulut VMware yığınınızın aşağıdaki yazılım sürümü ile dağıtılır.

| Bileşen | Version | Lisanslı sürüm |
|-----------|---------|------------------|
| ESXi | 6.7U1 | Kurumsal artı |
| vCenter | 6.7U1 | vCenter standart |
| vSAN | 6.7 | Enterprise |
| NSX veri merkezi | 2.3 | Gelişmiş |

## <a name="esxi"></a>ESXi

Özel bulut oluşturma sırasında VMware ESXi sağlanan CloudSimple düğümlerine yüklenir.  ESXi hiper iş yükü sanal makineleri (VM) dağıtmak için sağlar.  Düğüm, özel bulutunuzda hiper yakınsama altyapısı (işlem ve depolama) sağlar.  Düğümler, özel buluta vSphere kümesinin parçasıdır.  Her düğüm, dört sahip fiziksel ağ arabirimlerinin ağ underlay bağlı.  İki fiziksel ağ arabirimi oluşturmak için kullanılan bir **vSphere dağıtılmış anahtarı (VDS)** vCenter ve iki oluşturmak için kullanılan bir **NSX yönetilen sanal dağıtılmış anahtarı (VDS N)** .  Ağ arabirimleri, yüksek kullanılabilirlik için etkin-etkin modda yapılandırılır.

VMware ESXi hakkında daha fazla bilgi edinin

## <a name="vcenter-server-appliance"></a>vCenter server gerecinde

vCenter server gerecinde (VCSA) VMware çözümü CloudSimple tarafından kimlik doğrulaması, yönetim ve düzenleme işlevleri sağlar. Özel bulutunuzu oluştururken VCSA katıştırılmış Platform Hizmetleri denetleyici (PSC) ile dağıtılır.  Özel bulutunuzun dağıttığınızda oluşturduğunuz vSphere kümede VCSA dağıtılır.  Her özel bulut kendi VCSA sahiptir.  Bir özel bulutun genişletme özel buluta VCSA düğümler ekler.

### <a name="vcenter-single-sign-on"></a>vCenter çoklu oturum açma

VCSA katıştırılmış Platform Hizmetleri denetleyicisinde ile ilişkili bir **vCenter çoklu oturum açma etki alanı**.  Etki alanı adı **cloudsimple.local**.  Varsayılan kullanıcı **CloudOwner@cloudsimple.com** vCenter erişmek sizin için oluşturulur.  Şirket içi/Azure active directory'nizde ekleyebilirsiniz [kimlik kaynakları için vCenter](https://docs.azure.cloudsimple.com/set-vcenter-identity/).

## <a name="vsan-storage"></a>vSAN depolama

Özel Bulutlar, tamamen flash vsan'ı tam olarak yapılandırılmış depolama, küme yerel ile oluşturulur.  Aynı SKU'ların en az üç düğüm ile vsan'ı veri deposu vSphere küme oluşturmak için gereklidir.  Yinelenenleri kaldırma ve sıkıştırma vSAN veri deposundaki varsayılan olarak etkindir.  VSphere kümedeki her düğümde iki disk grubu oluşturulur. Her disk grubu, bir önbellek diski ve üç kapasite diskleri içerir.

Varsayılan vSAN saklama İlkesi vSphere kümede oluşturulur ve vSAN veri deposuna uygulanır.  Bu ilke, sanal makine depolama nesneleri nasıl sağlanan ve gerekli hizmet düzeyini garanti etmek için veri deposu içinde ayrılmış belirler.  Saklama ilkesi tanımlar **hata toleransı (FTT)** ve **hata toleransı yöntemi**.  Yeni depolama ilkeleri oluşturabilir ve bunları sanal makinelere uygulayabilirsiniz. SLA korumak için % 25 yedek kapasiteyi vSAN veri deposundaki korunmalıdır.  

### <a name="default-vsan-storage-policy"></a>Varsayılan vSAN saklama İlkesi

Aşağıdaki tabloda, varsayılan vSAN depolama ilke parametreleri gösterir.

| VSphere küme düğümleri sayısı | FTT | Hata toleransı yöntemi |
|------------------------------------|-----|--------------------------|
| 3 ila 4 düğüm | 1 | RAID (yansıtma) - 1, 2 kopya oluşturur |
| 5-16 düğümleri | 2 | RAID (yansıtma) - 1, 3 kopya oluşturur |

## <a name="nsx-data-center"></a>NSX veri merkezi

NSX Data Center özel bulutunuzda ağ sanallaştırma, mikro Segmentasyonu ve ağ güvenlik özellikleri sağlar.  Özel bulutunuzun NSX aracılığıyla NSX veri merkezi tarafından desteklenen tüm hizmetleri yapılandırabilirsiniz.  Özel bir bulut oluştururken aşağıdaki NSX bileşenleri yüklenir ve yapılandırılır.

* NSXT Yöneticisi
* Aktarım bölgeleri
* Konak ve Edge yukarı bağlantı profili
* Edge taşıma ve Ext1 Ext2 için mantıksal anahtar
* ESXi aktarım düğümü için IP havuzu
* Edge aktarım düğümü için IP havuzu
* Kenar düğümleri
* Denetleyici ve Edge VM'ler için benzeşim karşıtlığını DRS kuralı
* Katman 0 yönlendirici
* BGP yönlendiricisinde Tier0 etkinleştir

## <a name="vsphere-cluster"></a>vSphere küme

ESXi ana bilgisayarları, özel bulutun yüksek kullanılabilirlik sağlamak için bir küme olarak yapılandırılır.  Özel bir bulut oluştururken, vSphere yönetim bileşenlerinin ilk kümeye dağıtılır.  Yönetim bileşenler için bir kaynak havuzu oluşturulur ve bu kaynak havuzundaki tüm yönetim Vm'leri dağıtılır. Özel bulut küçültmek için ilk küme silinemiyor.  vSphere küme sağlar yüksek kullanılabilirlik için Vm'leri kullanarak **vSphere HA**.  Tolerans hataları kümedeki kullanılabilir düğüm sayısını temel alır.  Formül kullanabileceğiniz ```Number of nodes = 2N+1``` burada ```N``` hata toleransı sayısıdır.

### <a name="vsphere-cluster-limits"></a>vSphere kümesi sınırları

| Resource | Sınır |
|----------|-------|
| Özel bir bulut oluşturmak için düğüm sayısı alt sınırı (ilk vSphere küme) | 3 |
| Bir özel bulutta en fazla bir vSphere düğüm sayısını küme | 16 |
| En fazla özel bulutta düğüm sayısı | 64 |
| Özel bir bulutta sayısı vSphere kümeleri | 21 |
| Yeni bir vSphere küme düğümlerinde en düşük sayısı | 3 |

## <a name="vmware-infrastructure-maintenance"></a>VMware altyapı Bakımı

Bazen VMware altyapısı yapılandırmada değişiklik yapmak gereklidir. Şu anda, bu aralık 1-2 ayda oluşabilir, ancak sıklığı zaman içinde reddetme beklenir. Bu tür bir bakım genellikle normal tüketim CloudSimple Hizmetleri kesintiye uğratmadan yapılabilir. Bir VMware bakım aralığı sırasında aşağıdaki hizmetleri herhangi bir etkisi çalışmaya devam eder:

* VMware yönetim düzlemi ve uygulamalar
* vCenter erişim
* Tüm ağ ve depolama
* Tüm Azure trafiğinin

## <a name="updates-and-upgrades"></a>Güncelleştirmeler ve yükseltmeler

CloudSimple yaşam döngüsü yönetimi ve özel bulutta VMware yazılımının (ESXi ve vCenter, PSC ve NSX) sorumludur.

Yazılım güncelleştirmeleri içerir:

* **Düzeltme ekleri**. Güvenlik düzeltme ekleri veya VMware tarafından yayımlanan hata düzeltmeleri.
* **Güncelleştirmeleri**. Bir VMware yığın bileşenin podverze değiştirin.
* **Yükseltmeler**. Bir VMware yığın bileşenin sürümle değiştirin.

Vmware'den kullanılabilir duruma geldiğinde CloudSimple kritik güvenlik düzeltme eki sınar. SLA'sı CloudSimple bir hafta içinde özel bulut ortamları için güvenlik düzeltme ekini yapar.

Üç aylık bakım güncelleştirmeleri için VMware yazılım bileşenlerini CloudSimple sağlar. VMware yazılım'ın yeni bir ana sürüm kullanılabilir olduğunda CloudSimple yükseltme için uygun bakım penceresi koordine etmek için müşteriler ile çalışır.  

## <a name="next-steps"></a>Sonraki adımlar

* [CloudSimple Bakım ve güncelleştirmeler](cloudsimple-maintenance-updates.md)