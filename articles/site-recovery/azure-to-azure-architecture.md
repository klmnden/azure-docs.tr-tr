---
title: Azure Site recovery'de Azure'a çoğaltma mimarisi | Microsoft Docs
description: Bu makalede, Azure Site Recovery hizmetini kullanarak Azure Vm'leri için Azure bölgeleri arasında olağanüstü durum kurtarma ayarlama çoğaltırken kullanılan bileşenler ve genel bir bakış sağlar.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: raynew
ms.openlocfilehash: 25cf3914274e73e0789aa87e9288649d1b0cb1eb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66399585"
---
# <a name="azure-to-azure-disaster-recovery-architecture"></a>Azure'dan Azure'a olağanüstü durum kurtarma mimarisi


Bu makalede mimarisi, bileşenleri ve olağanüstü durum kurtarma için Azure kullanarak sanal makineleri (VM'ler) dağıtırken kullanılan işlemleri [Azure Site Recovery](site-recovery-overview.md) hizmeti. Olağanüstü durum kurtarma ayarlama, Azure Vm'lerini sürekli olarak gelen farklı bir hedef bölgeye çoğaltma. Bir kesinti oluşursa, VM'lerin yükünü ikincil bölgeye yük ve makinelere oradan erişebilirsiniz. Normalde her şeyi yeniden çalışırken, yeniden çalışma ve birincil konumda çalışmaya devam.



## <a name="architectural-components"></a>Mimari bileşenler

Azure Vm'leri için olağanüstü durum kurtarma katılan bileşenleri aşağıdaki tabloda özetlenmiştir.

**Bileşen** | **Gereksinimler**
--- | ---
**Kaynak bölgede VM'ler** | Daha fazla Azure Vm'lerinde birini bir [kaynak bölgede desteklenen](azure-to-azure-support-matrix.md#region-support).<br/><br/> Vm'leri çalıştırıyor olabilir herhangi [işletim sistemi desteklenen](azure-to-azure-support-matrix.md#replicated-machine-operating-systems).
**Kaynak VM depolama** | Azure Vm'lerini yönetilen veya yönetilmeyen diskler depolama hesabı arasında yayıldı.<br/><br/>[Hakkında bilgi edinin](azure-to-azure-support-matrix.md#replicated-machines---storage) Azure depolama desteklenir.
**Kaynak VM ağları** | VM'ler kaynak bölgedeki bir sanal ağ (VNet) içinde bir veya daha fazla alt ağ içinde yer alabilir. [Daha fazla bilgi edinin](azure-to-azure-support-matrix.md#replicated-machines---networking) ağ gereksinimleri hakkında.
**Önbellek depolama hesabı** | Bir önbellek depolama hesabı kaynak ağı gerekir. Çoğaltma sırasında VM değişiklikler, hedef depolama gönderilmeden önce önbellekte depolanır.<br/><br/> Bir önbellek kullanarak VM'de çalışan üretim uygulamaları üzerinde en az etki sağlar.<br/><br/> [Daha fazla bilgi edinin](azure-to-azure-support-matrix.md#cache-storage) önbellek depolama gereksinimleri hakkında. 
**Hedef kaynaklar** | Hedef kaynaklar, çoğaltma sırasında ve bir yük devretme gerçekleştiğinde kullanılır. Site Recovery varsayılan olarak hedef kaynağı ayarlayabilirsiniz veya oluşturun/bunları özelleştirebilirsiniz.<br/><br/> Hedef bölgede VM'ler oluşturabilir ve aboneliğinizi hedef bölgede gereken VM boyutları desteklemek için yeterli kaynaklara sahip olduğunu denetleyin. 

![Kaynak ve hedef çoğaltma](./media/concepts-azure-to-azure-architecture/enable-replication-step-1.png)

## <a name="target-resources"></a>Hedef kaynaklar

Bir sanal makine için çoğaltmayı etkinleştirdiğinizde Site Recovery, hedef kaynaklarını otomatik olarak oluşturma seçeneği sunar. 

**Hedef kaynak** | **Varsayılan ayar**
--- | ---
**Hedef aboneliği** | Kaynak abonelik ile aynıdır.
**Hedef kaynak grubu** | Yük devretme sonrasında Vm'leri ait olduğu kaynak grubu.<br/><br/> Bu kaynak bölgesi dışında herhangi bir Azure bölgesinde olabilir.<br/><br/> Site Recovery, hedef bölgede "asr" sonekine sahip yeni bir kaynak grubu oluşturur.<br/><br/>
**Hedef VNet** | Çoğaltılan VM'ler yük devretmeden sonra bulunduğu olduğu sanal ağ (VNet). Ağ eşleme, kaynak ve hedef sanal ağlar arasında ve bunun tersi de oluşturulur.<br/><br/> Site Recovery "asr" sonekine yeni bir VNet ve alt ağ oluşturur.
**Hedef depolama hesabı** |  VM'ye yönetilen disk kullanmıyorsa, çoğaltılan verileri için depolama hesabı budur.<br/><br/> Site Recovery, kaynak depolama hesabını yansıtmak için hedef bölgede yeni bir depolama hesabı oluşturur.
**Yönetilen çoğaltma diskleri** | VM'ye yönetilen disk kullanıyorsa, çoğaltılan verileri için yönetilen diskler budur.<br/><br/> Site Recovery, kaynak yansıtmak için depolama bölgede yönetilen çoğaltma diskleri oluşturur.
**Hedef kullanılabilirlik kümeleri** |  Kullanılabilirlik kümesi çoğaltma vm'lerinin yük devretme işleminden sonra yer.<br/><br/> Site Recovery, bir kullanılabilirlik kümesi hedef bölgede "asr" sonekine sahip bir kullanılabilirlik kümesinde kaynak konumda bulunan VM'ler için oluşturur. Bir kullanılabilirlik kümesi varsa, kullanıldığı ve yeni bir tane oluşturulmaz.
**Hedef kullanılabilirlik alanları** | Hedef bölge kullanılabilirlik destekliyorsa, Site Recovery kaynak bölgede kullanılan aynı bölge sayısına atar.

### <a name="managing-target-resources"></a>Hedef kaynaklarını yönetme

Hedef kaynakları aşağıda gösterildiği gibi yönetebilirsiniz:

- Çoğaltmayı etkinleştirme gibi hedef ayarlarını değiştirebilirsiniz.
- Çoğaltma zaten çalışmaya başladıktan sonra hedef ayarlarını değiştirebilirsiniz. Özel durum, (tek bir örneği, kümesi veya bölge) bir kullanılabilirlik türüdür. Bu ayarı değiştirmek için çoğaltmayı devre dışı bırakın, ayarını değiştirin ve ardından etkinleştirmeniz gerekir.



## <a name="replication-policy"></a>Çoğaltma ilkesi 

Azure VM çoğaltmayı etkinleştirdiğinizde, varsayılan olarak Site Recovery tabloda özetlenen varsayılan ayarlarla yeni bir çoğaltma ilkesi oluşturur.

**İlke ayarı** | **Ayrıntılar** | **Varsayılan**
--- | --- | ---
**Kurtarma noktası bekletme** | Site Recovery kurtarma noktalarının ne kadar süreyle korur belirtir | 24 saat
**Uygulamayla tutarlı anlık görüntü sıklığı** | Ne sıklıkta Site Recovery uygulamayla tutarlı bir anlık görüntüsünü alır. | 60 dakikada bir.

### <a name="managing-replication-policies"></a>Çoğaltma ilkelerini yönetme

Yönetme ve varsayılan çoğaltma ilkeleri ayarları aşağıdaki gibi değiştirin:
- Çoğaltmayı etkinleştirme gibi ayarları değiştirebilirsiniz.
- Herhangi bir zamanda bir çoğaltma ilkesi oluşturun ve çoğaltmayı etkinleştirdiğinizde geçerlidir.

### <a name="multi-vm-consistency"></a>Çoklu VM tutarlılığı

VM'lerin birlikte çoğaltılır ve paylaşılan kilitlenmeyle tutarlı istediğiniz ve yük devretme sırasında uygulamayla tutarlı kurtarma noktaları, bunları bir çoğaltma grubunda birlikte toplayabilirsiniz. Çoklu VM tutarlılığı, iş yükü performansını etkiler ve yalnızca tüm makineler arasında tutarlılık gerektiren iş yükleri çalıştıran VM'ler için kullanılmalıdır. 



## <a name="snapshots-and-recovery-points"></a>Anlık görüntüler ve kurtarma noktaları

Kurtarma noktaları, belirli bir noktada gerçekleştirilen VM disklerinin anlık görüntülerden oluşturulur. Bir VM'ye yük devretmeden sonra hedef konumda VM geri yüklemek için bir kurtarma noktası kullanın.

Devretmek, genellikle VM bozulma ya da veri kaybı başlatır ve sanal makine verilerini VM üzerinde çalışan uygulamalar ve işletim sistemi için tutarlı olduğundan emin olmak istiyoruz. Bu, alınan anlık görüntülere türüne bağlıdır.

Site Recovery anlık görüntüleri aşağıdaki gibi alır:

1. Bir sıklık belirtirseniz, site Recovery varsayılan ve uygulamayla tutarlı anlık görüntü veri kilitlenme ile tutarlı anlık görüntüleri alır.
2. Kurtarma noktası anlık görüntülerden oluşturulan ve çoğaltma ilkesi saklama ayarlarına uygun olarak depolanır.

### <a name="consistency"></a>Tutarlılık

Aşağıdaki tabloda, tutarlılık farklı türleri açıklanmaktadır.

### <a name="crash-consistent"></a>Kilitlenmeyle tutarlı

**Açıklama** | **Ayrıntılar** | **Öneri**
--- | --- | ---
Kilitlenme ile tutarlı bir anlık görüntü anlık görüntü alındığında diskte olan verileri yakalar. Herhangi bir şey bellekte içermez.<br/><br/> Bu VM kilitlenmesi veya güç kablosunu anlık görüntünün alındığı anlık sunucudan çekilmiştir varsa var olacaktır diskteki verileri denk içerir.<br/><br/> Bir kilitlenme tutarlı işletim sistemi veya VM üzerindeki uygulamalar için veri tutarlılığı garanti etmez. | Site Recovery, varsayılan olarak her beş dakikada kilitlenmeyle tutarlı kurtarma noktalarını oluşturur. Bu ayar değiştirilemez.<br/><br/>  | Bugün, çoğu uygulama iyi kilitlenmeyle tutarlı noktalarından kurtarabilirsiniz.<br/><br/> Kilitlenmeyle tutarlı kurtarma noktalarını çoğaltma işletim sistemleri ve DHCP sunucuları ve yazdırma sunucuları gibi uygulamalar için genellikle yeterli.

### <a name="app-consistent"></a>Uygulamayla tutarlı

**Açıklama** | **Ayrıntılar** | **Öneri**
--- | --- | ---
Uygulamayla tutarlı kurtarma noktaları, uygulamayla tutarlı anlık görüntülerden oluşturulur.<br/><br/> Uygulamayla tutarlı bir anlık görüntü, kilitlenmeyle tutarlı bir anlık görüntüdeki tüm bilgilerin yanı sıra bellekteki tüm verileri ve devam eden işlemler içeriyor. | Uygulamayla tutarlı anlık görüntüler Birim Gölge Kopyası Hizmeti (VSS) kullanın:<br/><br/>   (1) bir anlık görüntü başlatıldığında, VSS bir birimde yazarken kopyalama (İNEK) işlemi gerçekleştirin.<br/><br/>   2) VSS, İNEK gerçekleştirmeden önce her uygulama, bellekte verilerini diske Temizleme için gereken makinede bildirir.<br/><br/>   3) VSS (Bu durum Site Recovery'de) yedekleme/olağanüstü durum kurtarma uygulama anlık görüntü verilerini okuyun ve devam etmek için daha sonra sağlar. | Uygulamayla tutarlı anlık görüntüler, belirttiğiniz sıklığına uygun olarak alınır. Her zaman bu sıklığı kurtarma noktası tutma için ayarladığınız daha az olmalıdır. Örneğin, 24 saatlik varsayılan ayarı kullanarak kurtarma noktalarını tutuyorsanız 24 saatten az sıklığı ayarlamanız gerekir.<br/><br/>Bunlar, daha karmaşık ve kilitlenme ile tutarlı anlık görüntüleri tamamlanması uzun sürebilir.<br/><br/> Bunlar, çoğaltma için etkinleştirilmiş bir VM'de çalışan uygulamaların performansını etkiler. 

## <a name="replication-process"></a>Çoğaltma işlemi

Bir Azure sanal makine için çoğaltmayı etkinleştirdiğinizde, şunlar olur:

1. Site Recovery Mobility hizmeti uzantısı VM'de otomatik olarak yüklenir.
2. Uzantı, VM'nin Site Recovery ile kaydeder.
3. VM için sürekli çoğaltma başlar.  Disk yazma işlemleri hemen kaynak konumundaki önbellek depolama hesabına aktarılır.
4. Site RECOVERY'yi önbelleğindeki verileri işler ve hedef depolama hesabına gönderir veya yönetilen diskleri çoğaltma için.
5. Veriler işlendikten sonra beş dakikada kilitlenmeyle tutarlı kurtarma noktaları oluşturulur. Uygulamayla tutarlı kurtarma noktaları, çoğaltma ilkesinde belirtilen ayarına göre oluşturulur.

![Çoğaltma işlemi, 2. Adım'ı etkinleştir](./media/concepts-azure-to-azure-architecture/enable-replication-step-2.png)

**Çoğaltma işlemi**

## <a name="connectivity-requirements"></a>Bağlantı gereksinimleri

 Azure VM'ler, çoğaltma giden bağlantı gerekir. Site Recovery, sanal makineye gelen bağlantılara hiçbir zaman işlemesi gerekmez. 

### <a name="outbound-connectivity-urls"></a>Giden bağlantı (URL)

VM'ler için giden erişim URL'ler ile denetlenir ise, bu URL'ler izin verir.

| **URL** | **Ayrıntılar** |
| ------- | ----------- |
| *.blob.core.windows.net | Verilerin VM’den kaynak bölgedeki önbellek depolama hesabına yazılmasına izin verir. |
| login.microsoftonline.com | Site Recovery hizmet URL’leri için yetkilendirme ve kimlik doğrulama özellikleri sağlar. |
| *.hypervrecoverymanager.windowsazure.com | VM’nin Site Recovery hizmetiyle iletişim kurmasına izin verir. |
| *.servicebus.windows.net | VM’nin Site Recovery izleme ve tanılama verilerini yazmasına izin verir. |

### <a name="outbound-connectivity-for-ip-address-ranges"></a>IP adresi aralıkları için giden bağlantı

VM'ler için IP adreslerini kullanarak giden bağlantıyı denetlemek için bu adresleri sağlar.

#### <a name="source-region-rules"></a>Kaynak bölge kuralları

**Kural** |  **Ayrıntılar** | **Hizmet etiketi**
--- | --- | --- 
HTTPS Gidene izin ver: bağlantı noktası 443 | Depolama hesabı kaynak bölgede karşılık gelen aralıklarına izin verin | Depolama alanı. \<bölge adı >.
HTTPS Gidene izin ver: bağlantı noktası 443 | Azure Active Directory (Azure AD) karşılık gelen aralıklarına izin verin.<br/><br/> Azure AD adresleri gelecekte eklenirse, yeni ağ güvenlik grubu (NSG) kuralları oluşturmanız gerekir.  | AzureActiveDirectory
HTTPS Gidene izin ver: bağlantı noktası 443 | Erişime izin ver [Site Recovery uç noktaları](https://aka.ms/site-recovery-public-ips) hedef konuma karşılık gelir. 

#### <a name="target-region-rules"></a>Hedef bölge kuralları

**Kural** |  **Ayrıntılar** | **Hizmet etiketi**
--- | --- | --- 
HTTPS Gidene izin ver: bağlantı noktası 443 | Depolama hesapları hedef bölgede karşılık gelen aralıklarına izin verin. | Depolama alanı. \<bölge adı >.
HTTPS Gidene izin ver: bağlantı noktası 443 | Azure AD'ye karşılık gelen aralıklarına izin verin.<br/><br/> Azure AD adresleri gelecekte eklenirse, yeni NSG kuralları oluşturmanız gerekir.  | AzureActiveDirectory
HTTPS Gidene izin ver: bağlantı noktası 443 | Erişime izin ver [Site Recovery uç noktaları](https://aka.ms/site-recovery-public-ips) kaynak konumuna karşılık gelir. 


#### <a name="control-access-with-nsg-rules"></a>NSG kuralları ile erişimi denetleme

Azure ağ/alt kullanarak gelen ve giden ağ trafiği filtreleyerek VM bağlantısı denetimi [NSG kuralları](https://docs.microsoft.com/azure/virtual-network/security-overview), aşağıdaki gereksinimleri dikkate alın:

- Kaynak Azure bölgesi için NSG kuralları, çoğaltma trafiği için giden erişime izin vermelidir.
- Bir test ortamında bunları üretime koymadan önce kuralları oluşturma öneririz.
- Kullanım [hizmet etiketleri](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) tek tek IP adreslerine izin vermek yerine.
    - Hizmet etiketlerini güvenlik kuralı oluştururken karmaşıklığını en aza indirmek için bir araya toplanan IP adresi ön eki grubunu temsil eder.
    - Microsoft, hizmet etiketleri zaman içinde otomatik olarak güncelleştirir. 
 
Daha fazla bilgi edinin [giden bağlantı](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges) Site Recovery için ve [Nsg'ler bağlantıyla denetleme](concepts-network-security-group-with-site-recovery.md).


### <a name="connectivity-for-multi-vm-consistency"></a>Çoklu VM tutarlılığı için bağlantı

Çoklu VM tutarlılığını etkinleştirirseniz çoğaltma grubundaki makineler birbiriyle 20004 numaralı bağlantı noktası üzerinden iletişim kurar.
- VM’ler arasında 20004 numaralı bağlantı noktası üzerinden gerçekleştirilen iç iletişimi engelleyen bir güvenlik duvarı gereci olmadığından emin olun.
- Linux VM’lerinin çoğaltma grubunun bir parçası olmasını istiyorsanız, 20004 numaralı bağlantı noktası üzerinden giden trafiğin, belirli Linux sürümünün kılavuzuna göre el ile açıldığından emin olun.




## <a name="failover-process"></a>Yük devretme işlemi

Bir yük devretme başlatın, VM'ler hedef kaynak grubu, hedef sanal ağ, hedef alt ağ oluşturulur ve hedef kullanılabilirlik kümesi. Yük devretme sırasında herhangi bir kurtarma noktasını kullanabilirsiniz.

![Yük devretme işlemi](./media/concepts-azure-to-azure-architecture/failover.png)

## <a name="next-steps"></a>Sonraki adımlar

[Hızlı bir şekilde çoğaltmak](azure-to-azure-quickstart.md) Azure VM'LERİNİ ikincil bir bölgeye.
