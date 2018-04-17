# <a name="platform-supported-migration-of-iaas-resources-from-classic-to-azure-resource-manager"></a>Iaas Klasik kaynaklardan Azure Resource Manager'a Platform desteklenen geçişini
Bu makalede, biz altyapı geçiş Resource Manager dağıtım modellerinde klasik bir hizmet (Iaas) kaynaklara olarak nasıl etkinleştirme açıklanmaktadır. Daha fazla bilgi edinebilirsiniz [Azure Kaynak Yöneticisi özellikleri ve avantajları](../articles/azure-resource-manager/resource-group-overview.md). Biz, aboneliğinizde sanal ağ siteden siteye ağ geçitleri kullanarak bir arada iki dağıtım modelinden kaynaklarına bağlanma ayrıntılarını gösterir.

## <a name="goal-for-migration"></a>Geçiş için hedef
Resource Manager şablonları aracılığıyla karmaşık uygulamaları dağıtma sağlar, VM uzantıları kullanarak sanal makineleri yapılandırır ve erişim yönetimi ve etiketleme içerir. Azure Resource Manager kullanılabilirlik kümeleri içinde sanal makineler için ölçeklenebilir, paralel dağıtım içerir. Yeni dağıtım modelini de bağımsız olarak işlem, ağ ve depolama yaşam döngüsü yönetimini sağlar. Son olarak, bir sanal ağdaki sanal makinelerin zorlama ile varsayılan güvenlik etkinleştirmek için bir odak yoktur.

Klasik dağıtım modelinden neredeyse tüm özellikleri, işlem, ağ ve depolama altında Azure Resource Manager için desteklenir. Azure Kaynak Yöneticisi'nde yeni özelliklerden yararlanmak için Klasik dağıtım modelinden varolan dağıtımları geçirebilirsiniz.

## <a name="supported-resources-for-migration"></a>Geçiş için desteklenen kaynaklar
Geçiş sırasında bu Klasik Iaas kaynakları desteklenir

* Virtual Machines
* Kullanılabilirlik Kümeleri
* Cloud Services
* Depolama Hesapları
* Sanal Ağlar
* VPN Gateway’leri
* Rota ağ geçitleri express _(aynı Abonelikteki sanal ağ olarak yalnızca)_
* Ağ Güvenlik Grupları
* Yönlendirme Tabloları
* Ayrılmış IP’ler

## <a name="supported-scopes-of-migration"></a>Desteklenen kapsamları geçiş
İşlem, ağ ve depolama kaynaklarını geçişini tamamlamak için 4 farklı yolu vardır. Bunlar

* (Değil, sanal ağ için) sanal makinelerin geçişi
* (Sanal ağındaki) sanal makinelerin geçişi
* Depolama hesapları geçişi
* Eklenmemiş kaynaklar (ağ güvenlik grupları, yol tablolarını & ayrılmış IP'ler)

### <a name="migration-of-virtual-machines-not-in-a-virtual-network"></a>(Değil, sanal ağ için) sanal makinelerin geçişi
Resource Manager dağıtım modelinde, güvenlik uygulamalarınız için varsayılan olarak uygulanır. Tüm sanal makineleri Resource Manager modelinde bir sanal ağ içinde olması gerekir. Azure platformu yeniden başlatma (`Stop`, `Deallocate`, ve `Start`) geçiş işleminin bir parçası olarak VM'ler. Sanal makineler için geçirilecek sanal ağlar için iki seçeneğiniz vardır:

* Yeni bir sanal ağ oluşturmak ve sanal makineyi yeni bir sanal ağ geçirmek için platform isteyebilir.
* Mevcut sanal ağda Kaynak Yöneticisi'nde sanal makine geçirebilirsiniz.

> [!NOTE]
> Bu geçiş kapsamda Yönetim düzeyi işlemleri ve veri düzlemi işlemleri geçiş sırasında bir süre için izin verilmeyebilir.
>
>

### <a name="migration-of-virtual-machines-in-a-virtual-network"></a>(Sanal ağındaki) sanal makinelerin geçişi
VM yapılandırmaların çoğu için yalnızca meta veri Klasik ve Resource Manager dağıtım modelleri arasında geçiriyor. Temel alınan sanal makineleri aynı donanım, aynı depolama ile aynı ağ üzerinde çalışıyor. Yönetim düzeyi işlemleri zaman geçiş sırasında belirli bir süre için izin verilmeyebilir. Ancak, veri düzlemi çalışmaya devam eder. Diğer bir deyişle, (Klasik) VM'ler üzerinde çalıştırılan uygulamalarınızın kapalı kalma süresi geçiş sırasında maruz değil.

Aşağıdaki yapılandırmalar şu anda desteklenmemektedir. Destek gelecekte bazı VM'ler eklenirse, bu yapılandırma kapalı kalma süresi neden olabilecek (Dur ayırması ve VM işlemlerini yeniden Git).

* Birden fazla kullanılabilirlik tek bulut hizmetinde kümesi vardır.
* Bir veya daha fazla kullanılabilirlik kümeleri ve bir kullanılabilirlik kümesinde tek bulut hizmetinde olmayan VM'ler var.

> [!NOTE]
> Bu geçiş kapsamda Yönetim düzeyi geçiş sırasında bir süre için izin verilmeyebilir. Veri düzlemi kapalı kalma süresinin daha önce açıklandığı gibi yapılandırmalar belirli.
>
>

### <a name="storage-accounts-migration"></a>Depolama hesapları geçişi
Sorunsuz geçiş izin vermek için Resource Manager sanal makineleri Klasik depolama hesabında dağıtabilirsiniz. Bu özelliği, işlem ve ağ kaynaklarını olabilir ve depolama hesapları bağımsız olarak geçirilmelidir. Sanal makineler ve sanal ağ üzerinden geçirmek sonra geçiş işlemini tamamlamak için depolama hesaplarınızı geçirmek gerekir.

> [!NOTE]
> Resource Manager dağıtım modeli Klasik görüntüler ve diskleri kavramı yoktur. Ne zaman geçirilen, Klasik görüntüleri depolama hesabıdır ve diskleri Kaynak Yöneticisi yığınında görünür değildir ancak VHD'ler yedekleme depolama hesabında kalır.
>
>

### <a name="unattached-resources-network-security-groups-route-tables--reserved-ips"></a>Eklenmemiş kaynaklar (ağ güvenlik grupları, yol tablolarını & ayrılmış IP'ler)
Tüm sanal makineler ve sanal ağlar bağlı olmayan ağ güvenlik grupları, yol tablolarını & ayrılmış IP'ler bağımsız olarak geçirilebilir.

<br>

## <a name="unsupported-features-and-configurations"></a>Desteklenmeyen özellikler ve yapılandırmalar
Şu anda bazı özellikler ve yapılandırmalar desteklemiyoruz. Aşağıdaki bölümlerde etrafında bizim öneriler açıklanmaktadır.

### <a name="unsupported-features"></a>Desteklenmeyen özellikler
Aşağıdaki özellikleri şu anda desteklenmemektedir. İsteğe bağlı olarak bu ayarları kaldırabilir, sanal makineleri geçirmek ve Resource Manager dağıtım modeli ayarlarında yeniden etkinleştirin.

| Kaynak sağlayıcısı | Özellik | Öneri |
| --- | --- | --- |
| İşlem | İlişkilendirilmemiş sanal makine disklerini. | Depolama hesabı geçirildiğinde bu diskleri arkasında VHD BLOB'ları geçirilen |
| İşlem | Sanal makine görüntüler. | Depolama hesabı geçirildiğinde bu diskleri arkasında VHD BLOB'ları geçirilen |
| Ağ | Uç nokta ACL'lerini. | Uç nokta ACL'lerini kaldırın ve geçiş yeniden deneyin. |
| Ağ | Application Gateway | Geçiş işlemine başlamadan önce uygulama ağ geçidi kaldırın ve geçiş tamamlandıktan sonra uygulama ağ geçidini yeniden oluşturun. |
| Ağ | VNet eşlemesi kullanarak sanal ağlar. | Resource Manager sanal ağını geçirin, ardından eş. Daha fazla bilgi edinmek [VNet eşlemesi](../articles/virtual-network/virtual-network-peering-overview.md). |

### <a name="unsupported-configurations"></a>Desteklenmeyen yapılandırmalar
Aşağıdaki yapılandırmalar şu anda desteklenmemektedir.

| Hizmet | Yapılandırma | Öneri |
| --- | --- | --- |
| Resource Manager |Rol tabanlı erişim denetimi (RBAC) Klasik kaynaklar için |Kaynakları URI'sini geçişten sonra değiştirdiğinden, geçişten sonra gerçekleşmesi gereken RBAC İlkesi güncelleştirmelerini plan yapmanız önerilir. |
| İşlem |Bir VM ile ilişkili birden çok alt ağı |Yalnızca bir alt ağ başvurmak için alt ağ yapılandırmasını güncelleştirin. Bu sanal makineden (yani başka bir alt ağa başvuran) ikincil bir NIC kaldırın ve geçiş tamamlandıktan sonra yeniden gerektirebilir. |
| İşlem |Bir sanal ağa ait olan ancak atanan açık bir alt ağ yoksa sanal makineler |İsteğe bağlı olarak VM silebilirsiniz. |
| İşlem |Uyarılar, otomatik ölçeklendirme ilkeleri sanal makineler |Geçiş geçtiği ve bu ayarları bırakılır. Geçiş yapmadan önce ortamınızı değerlendirmek önerilir. Alternatif olarak, geçiş tamamlandıktan sonra uyarı ayarlarını yapılandırabilirsiniz. |
| İşlem |XML VM Uzantıları (Bgınfo 1.*, Visual Studio hata ayıklayıcısı, Web dağıtımı ve uzaktan hata ayıklama) |Bu işlem desteklenmiyor. Bu uzantılar geçişe devam etmek için sanal makineden kaldırın veya geçiş işlemi sırasında otomatik olarak bırakılacak önerilir. |
| İşlem |Premium depolama ile önyükleme tanılama |Önyükleme tanılaması özelliğini VM'ler için geçirme işlemine devam etmeden önce devre dışı bırakın. Geçiş tamamlandıktan sonra kaynak yöneticisi yığınından önyükleme tanılaması yeniden etkinleştirebilirsiniz. Ayrıca, artık bu BLOB'lar için ücretlendirilirsiniz şekilde ekran ve seri günlükleri için kullanılan BLOB'ları silinmelidir. |
| İşlem | Web/çalışan rolü içeren bulut Hizmetleri | Bu şu anda desteklenmiyor. |
| İşlem | Birden fazla kullanılabilirlik içeren bulut Hizmetleri ayarlayın veya birden çok kullanılabilirlik ayarlar. |Bu şu anda desteklenmiyor. Lütfen sanal makineler aynı kullanılabilirlik kümesinde geçirmeden önce taşıyın. |
| İşlem | Azure Güvenlik Merkezi uzantısına sahip VM | Azure Güvenlik Merkezi, kendi güvenlik izleme ve uyarılar yükseltmek için sanal makinelerde uzantıları otomatik olarak yükler. Azure Güvenlik Merkezi ilke abonelikte etkinse, bu uzantılar genellikle otomatik olarak yüklenir. Sanal makineleri geçirmek için lütfen güvenlik izleme uzantısı sanal makinelerden merkezi kaldıracak aboneliğe ilişkin Güvenlik Merkezi ilke devre dışı. |
| İşlem | Yedekleme ya da anlık görüntü uzantısına sahip VM | Bu uzantılar, Azure Backup hizmeti ile yapılandırılmış bir sanal makineye yüklenir. Bu sanal makineleri geçirmek için yönergeleri izleyin [burada](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-faq#vault).  |
| Ağ |Sanal makineler ve web/çalışan rolleri içeren sanal ağlar |Bu şu anda desteklenmiyor. Geçirmeden önce Web/çalışan rolleri, kendi sanal ağa taşıyın. Klasik sanal ağ geçirildikten sonra geçirilen Azure Resource Manager sanal ağı Klasik sanal benzer yapılandırmasını eskisi elde etmek için ağ ile eşlenemez.|
| Ağ | Klasik Expressroute bağlantı hatları |Bu şu anda desteklenmiyor. Bu bağlantı hatları Azure Resource Manager Iaas geçiş işlemine başlamadan önce geçirilmesi gerekir. Bunu hakkında daha fazla bilgi edinmek için [Resource Manager dağıtım modeline taşıma ExpressRoute bağlantı hatları Klasik](../articles/expressroute/expressroute-move.md).|
| Azure App Service |Uygulama hizmeti ortamları içeren sanal ağlar |Bu şu anda desteklenmiyor. |
| Azure Hdınsight |Hdınsight hizmetleri içeren sanal ağlar |Bu şu anda desteklenmiyor. |
| Microsoft Dynamics yaşam döngüsü Hizmetleri |Dynamics yaşam döngüsü Hizmetleri tarafından yönetilen sanal makineleri içeren sanal ağlar |Bu şu anda desteklenmiyor. |
| Azure AD Domain Services |Azure AD etki alanı hizmetleri içeren sanal ağlar |Bu şu anda desteklenmiyor. |
| Azure RemoteApp |Azure RemoteApp dağıtımlarını içeren sanal ağlar |Bu şu anda desteklenmiyor. |
| Azure API Management |Azure API Management dağıtımlarını içeren sanal ağlar |Bu şu anda desteklenmiyor. Iaas VNET geçirmek için herhangi bir kapalı kalma süresi işlemi API Management dağıtımında VNET değiştirin. |
