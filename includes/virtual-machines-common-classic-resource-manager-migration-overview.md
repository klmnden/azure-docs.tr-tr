---
title: include dosyası
description: include dosyası
services: virtual-machines
author: jpconnock
ms.service: virtual-machines
ms.topic: include
ms.date: 05/18/2018
ms.author: jeconnoc
ms.custom: include file
ms.openlocfilehash: ca4063d31d93aab3814abed202b6b91b7726185f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60542935"
---
# <a name="platform-supported-migration-of-iaas-resources-from-classic-to-azure-resource-manager"></a>Iaas kaynaklarının Klasik modelden Azure Resource Manager'a Platform destekli geçiş
Bu makalede hizmet (Iaas) kaynaklar Klasikten Resource Manager dağıtım modelleri ve ayrıntıları olarak aboneliğinizde sanal ağ'ı kullanarak birlikte bulunan iki dağıtım modellerindeki kaynaklara bağlanma altyapı geçirme siteden siteye ağ geçitleri. Daha fazla bilgi edinebilirsiniz [Azure Resource Manager özelliklerine ve avantajlarına](../articles/azure-resource-manager/resource-group-overview.md). 

## <a name="goal-for-migration"></a>Geçiş hedefi
Resource Manager, Şablonlar aracılığıyla karmaşık uygulamalar dağıtmaya olanak tanır, VM uzantılarını kullanarak sanal makineleri yapılandırır ve erişim yönetimi ve etiketleme özelliklerini ekler. Azure Resource Manager sanal makineler için ölçeklenebilir, paralel dağıtım kullanılabilirlik kümelerine içerir. Yeni dağıtım modeline de bağımsız olarak işlem, ağ ve depolama yaşam döngüsü yönetimi sağlar. Son olarak, varsayılan bir sanal ağdaki sanal makinelerin zorlama ile güvenliği etkinleştirme bir odak yoktur.

Klasik dağıtım modelinden neredeyse tüm özellikleri, işlem, ağ ve Azure Resource Manager altında depolama için desteklenir. Azure Resource Manager'daki yeni özelliklerden yararlanmak için Klasik dağıtım modelinden mevcut dağıtımları geçirebilirsiniz.

## <a name="supported-resources-for-migration"></a>Geçiş için desteklenen kaynaklar
Bu Klasik Iaas kaynaklarının geçişi sırasında desteklenir.

* Virtual Machines
* Kullanılabilirlik Kümeleri
* Bulut Hizmetleri ve Sanal Makineler
* Depolama Hesapları
* Sanal Ağlar
* VPN Ağ Geçitleri
* Express route ağ geçidi _(aynı abonelikte bulunan sanal ağ yalnızca)_
* Ağ Güvenlik Grupları
* Yönlendirme Tabloları
* Ayrılmış IP’ler

## <a name="supported-scopes-of-migration"></a>Desteklenen kapsamları geçiş
İşlem, ağ ve depolama kaynaklarının geçişini tamamlamak için dört farklı yolu vardır:

* [Sanal makinelerin (değil, sanal ağ için)](#migration-of-virtual-machines-not-in-a-virtual-network)
* [(Bir sanal ağdaki) sanal makinelerin geçişi](#migration-of-virtual-machines-in-a-virtual-network)
* [Depolama hesapları geçişi](#migration-of-storage-accounts)
* [Geçiş eklenmemiş kaynakları](#migration-of-unattached-resources)

### <a name="migration-of-virtual-machines-not-in-a-virtual-network"></a>Sanal makinelerin (değil, sanal ağ için)
Resource Manager dağıtım modelinde, güvenlik, uygulamalarınız için varsayılan olarak uygulanır. Tüm sanal makineler, Resource Manager modelinde bir sanal ağda olması gerekir. Azure platformu yeniden başlatılır (`Stop`, `Deallocate`, ve `Start`) Vm'leri geçişin bir parçası olarak. Sanal makineler için geçirilecek sanal ağlar için iki seçeneğiniz vardır:

* Yeni bir sanal ağ oluşturmak ve yeni bir sanal ağa sanal makineyi geçirmek için platform isteyebilir.
* Mevcut bir sanal ağına Kaynak Yöneticisi'nde sanal makine geçirebilirsiniz.

> [!NOTE]
> Bu geçiş kapsamda hem yönetim düzlemi işlemleri ve veri düzlemi işlemleri geçiş sırasında bir süre için izin verilmeyebilir.
>

### <a name="migration-of-virtual-machines-in-a-virtual-network"></a>(Bir sanal ağdaki) sanal makinelerin geçişi
Çoğu sanal makine yapılandırmaları için yalnızca meta verileri Klasik ve Resource Manager dağıtım modelleri arasında geçiş. Temel sanal makinelerin aynı donanım, aynı depolama alanı ile aynı ağ üzerinde çalışıyor. Yönetim düzlemi işlemleri için belirli bir süre boyunca geçiş sırasında izin verilmeyebilir. Ancak, veri düzlemine çalışmaya devam eder. Diğer bir deyişle, VM'ler (Klasik) üzerinde çalışan uygulamalarınızı geçiş sırasında kapalı kalma süresi tabi değildir.

Aşağıdaki yapılandırmalar şu anda desteklenmemektedir. Bu yapılandırma desteği gelecekte bazı VM'ler eklenirse, kapalı kalma süresi etkileyebilir (durdurma serbest bırakın ve sanal makine işlemlerini yeniden gidin).

* Tek bir bulut hizmetinde birden fazla kullanılabilirlik var.
* Bir veya daha fazla kullanılabilirlik kümeleri ve bir kullanılabilirlik kümesinde tek bir bulut hizmetinde olmayan VM'lerin var.

> [!NOTE]
> Bu geçiş kapsamda yönetim düzlemi geçiş sırasında bir süre için izin verilmeyebilir. Veri düzlemi kapalı kalma süresi meydana daha önce açıklandığı gibi yapılandırmaları belirli.
>

### <a name="migration-of-storage-accounts"></a>Depolama hesapları geçişi
Sorunsuz geçiş izin vermek için Resource Manager Vm'leri Klasik depolama hesabında dağıtabilirsiniz. Bu özellik, işlem ve ağ kaynakları kullanabilirsiniz ve bağımsız olarak depolama hesapları geçirilmelidir. Sanal makineleriniz ve sanal ağ geçirdikten sonra geçiş işlemini tamamlamak için depolama hesaplarınıza geçirmek gerekir.

Depolama hesabınızda herhangi bir ilişkili diskler veya sanal makinelerinizdeki veriler yok ve yalnızca bloblar, dosyalar, tablolar ve Kuyruklar varsa Azure Resource Manager'a geçiş bağımlılıkları olmadan bir tek başına geçiş olarak yapılabilir.

> [!NOTE]
> Resource Manager dağıtım modeli Klasik görüntü ve diskleri kavramı yoktur. Ne zaman görüntüleri geçirilen, Klasik depolama hesabıdır ve diskleri bir Resource Manager yığınında görünür değildir ancak yedekleme VHD'leri depolama hesabında kalır.
>

### <a name="migration-of-unattached-resources"></a>Geçiş eklenmemiş kaynakları
Depolama hesapları ile ilişkilendirilmiş diskleri ya da sanal makinelerinizdeki veriler bağımsız olarak geçirilebilir.

Tüm sanal makineler ve sanal ağlar bağlı olmayan ağ güvenlik grupları, yönlendirme tablolarını ve ayrılmış IP'ler ayrıca birbirinden bağımsız olarak geçirilebilir.

<br>

## <a name="unsupported-features-and-configurations"></a>Desteklenmeyen özellikleri ve yapılandırmalar
Bazı özellikleri ve yapılandırmalar şu anda desteklenmiyor; Aşağıdaki bölümlerde önerilerimizle etrafında açıklanmaktadır.

### <a name="unsupported-features"></a>Desteklenmeyen özellikler
Aşağıdaki özellikler şu anda desteklenmemektedir. İsteğe bağlı olarak bu ayarları kaldırabilir, Vm'leri geçirme ve Resource Manager dağıtım modelinde ayarlar'ı yeniden etkinleştirin.

| Kaynak sağlayıcısı | Özellik | Öneri |
| --- | --- | --- |
| İşlem | İlişkilendirilmemiş sanal makine diskleri. | Depolama hesabı geçiş yaptığında bu diskleri arkasında VHD bloblarını geçişi |
| İşlem | Sanal makine görüntüleri. | Depolama hesabı geçiş yaptığında bu diskleri arkasında VHD bloblarını geçişi |
| Ağ | Uç nokta ACL'leri. | Uç nokta ACL'sini kaldırın ve geçişi yeniden deneyin. |
| Ağ | Application Gateway | Uygulama ağ geçidi geçişine başlamadan önce kaldırın ve geçiş tamamlandıktan sonra uygulama ağ geçidini yeniden oluşturun. |
| Ağ | VNet eşlemesi kullanarak sanal ağları. | Sanal ağ Resource Manager'a geçiş ve eş. Daha fazla bilgi edinin [VNet eşlemesi](../articles/virtual-network/virtual-network-peering-overview.md). |

### <a name="unsupported-configurations"></a>Desteklenmeyen yapılandırmalar
Aşağıdaki yapılandırmalar şu anda desteklenmemektedir.

| Hizmet | Yapılandırma | Öneri |
| --- | --- | --- |
| Resource Manager |Rol tabanlı erişim denetimi (RBAC) Klasik kaynaklar |Geçişten sonra kaynak URI değiştirdiğinden, geçişten sonra gerçekleşmesi gereken RBAC İlkesi güncelleştirmelerini planlamanızı öneririz. |
| İşlem |Bir VM ile ilişkili birden çok alt ağı |Yalnızca bir alt ağa başvuran için alt ağ yapılandırmasını güncelleştirin. Bu, (yani başka bir alt ağa başvuran) bir ikincil NIC bir VM'den kaldırın ve geçiş tamamlandıktan sonra yeniden bağlayın gerektirebilir. |
| İşlem |Bir sanal ağa ait ancak atanan açık bir alt ağ yoksa sanal makineler |İsteğe bağlı olarak, VM'yi silebilirsiniz. |
| İşlem |Uyarılar, otomatik ölçeklendirme ilkeleri olan sanal makineler |Geçiş geçer ve bu ayarları bırakılır. Geçiş yapmadan önce ortamınızı değerlendirmeniz önerilir. Alternatif olarak, geçiş tamamlandıktan sonra uyarı ayarlarını yeniden yapılandırabilirsiniz. |
| İşlem |XML VM Uzantıları (Bgınfo 1.*, Visual Studio hata ayıklayıcı, Web dağıtımı ve uzaktan hata ayıklama) |Bu işlem desteklenmez. Bu uzantıları geçişe devam etmek için sanal makineden kaldırın veya geçiş işlemi sırasında otomatik olarak bırakılır önerilir. |
| İşlem |Premium depolama ile önyükleme tanılaması |Önyükleme tanılaması özelliği VM'ler için geçişe devam etmeden önce devre dışı bırakın. Geçiş tamamlandıktan sonra Resource Manager yığınından önyükleme tanılamayı yeniden etkinleştirebilirsiniz. Ayrıca, artık bu bloblar için ücretlendirilir için ekran ve seri günlükleri için kullanılan BLOB'ları silinmelidir. |
| İşlem | Web/çalışan rollerini içeren bulut Hizmetleri | Bu şu anda desteklenmiyor. |
| İşlem | Birden fazla kullanılabilirlik içeren bulut Hizmetleri veya birden çok kullanılabilirlik kümeleri. |Bu şu anda desteklenmiyor. Lütfen sanal makineler aynı kullanılabilirlik kümesinde geçirmeden önce taşıyın. |
| İşlem | Azure Güvenlik Merkezi uzantısına sahip VM | Azure Güvenlik Merkezi, uzantılar, güvenlik izleme ve uyarılar yükseltmek için sanal makinelere otomatik olarak yükler. Azure Güvenlik Merkezi ilke abonelikte etkinse, bu uzantılar genellikle otomatik olarak yüklenir. Sanal makineleri geçirmek için güvenlik izleme uzantısı, sanal makinelerden merkezi kaldıracak abonelikte Güvenlik Merkezi ilkesini devre dışı bırakın. |
| İşlem | VM yedekleme ya da anlık görüntü uzantısı | Bu uzantılar, Azure Backup hizmeti ile yapılandırılmış bir sanal makineye yüklenir. Bu VM'lerin geçişini desteklenmiyor olsa da yönergeleri [burada](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-faq#vault) geçişten önce alınan yedeklemeler korumak için.  |
| Ağ |Sanal makineler ve web/çalışan rollerini içeren sanal ağlar |Bu şu anda desteklenmiyor. Lütfen Web/çalışan rollerini geçirmeden önce kendi sanal ağa taşıyın. Klasik sanal ağ geçirildikten sonra geçirilen Azure Resource Manager sanal ağı Klasik sanal benzer yapılandırmasını önceki gibi elde etmek için ağ ile eşlenebilir.|
| Ağ | Klasik Express Route bağlantı hatları |Bu şu anda desteklenmiyor. Bu bağlantı hatları, Iaas geçişine başlamadan önce Azure Resource Manager'a geçirilmesi gerekir. Daha fazla bilgi için bkz. [Resource Manager dağıtım modeline taşıma ExpressRoute bağlantı hatlarını Klasikten](../articles/expressroute/expressroute-move.md).|
| Azure App Service |App Service ortamları içeren sanal ağlar |Bu şu anda desteklenmiyor. |
| Azure HDInsight |HDInsight hizmetleri içeren sanal ağlar |Bu şu anda desteklenmiyor. |
| Microsoft Dynamics yaşam döngüsü Hizmetleri |Dynamics yaşam döngüsü Hizmetleri tarafından yönetilen sanal makineleri içeren sanal ağlar |Bu şu anda desteklenmiyor. |
| Azure AD Domain Services |Azure AD etki alanı hizmetleri içeren sanal ağlar |Bu şu anda desteklenmiyor. |
| Azure RemoteApp |Azure RemoteApp dağıtımlarını içeren sanal ağlar |Bu şu anda desteklenmiyor. |
| Azure API Management |Azure API Management dağıtımlarını içeren sanal ağlar |Bu şu anda desteklenmiyor. Iaas sanal ağ'ı geçirmek için herhangi bir kapalı kalma süresi işlemi API Management dağıtımınızın VNET değiştirin. |
