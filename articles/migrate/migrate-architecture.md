---
title: Azure geçiş mimarisi | Microsoft Docs
description: Azure Geçişi hizmetine genel bir bakış sağlar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: raynew
ms.openlocfilehash: 16fbf8d3523ac2ab36447aa51a93a0bb5a9fad54
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811433"
---
# <a name="azure-migrate-architecture-and-processes"></a>Azure geçiş mimarisi ve işlemler

[Azure geçişi](migrate-overview.md) hub'ı keşfedin, değerlendirin ve uygulamalar ve altyapı iş yükleri Microsoft Azure'a geçirmek için yardımcı araçlar sağlar. Hub, Azure geçişi araçları ve üçüncü taraf bağımsız yazılım satıcısı (ISV) teklifleri içerir. 

 Bu makalede, Azure geçişi Server değerlendirmesi ve Azure geçişi sunucusu geçişi değerlendirme ve geçiş mimarisi ve işlemler özetlenir.

## <a name="assessment-with-azure-migrate-server-assessment"></a>Azure ile değerlendirme geçirme Server değerlendirmesi

Azure geçişi Server değerlendirmesi, VMware Vm'leri ve Hyper-V sanal makineleri, Azure'a geçiş için değerlendirebilirsiniz. Değerlendirme gibi çalışır:

1. **Değerlendirme için hazırlama**: Şirket içinde Azure geçişi ile basit bir Azure geçişi gerecin şirket içi VMware VM veya Hyper-V VM ve kayıt gereç olarak ayarlayın.
2. **Vm'leri bulmak**: Şirket içi Vm'leri bulmak için Azure geçişi Gereci çalıştırır. 
    - Hiçbir şey bulunan Vm'lerine yüklü olması gerekir.
    - Sanal makine meta verileri çekirdekleri, bellek, diskler, disk boyutları ve ağ bağdaştırıcıları hakkında bilgi içerir.
    - Performans verilerini CPU ve bellek kullanımı, disk IOPS, disk aktarım hızı (MB/sn) ve ağ çıktısı (MB/sn) hakkında bilgi içerir.
- **Toplayın ve makineleri değerlendirme**: Bulma tamamlandıktan sonra Azure Vm'leri Topladığınızdan portal, genellikle birlikte geçirmek istediğiniz sanal makinelerin oluşan gruplar halinde bulundu. Bir grup üzerinde bir değerlendirme çalıştırın.


## <a name="vmware-assessment"></a>VMware değerlendirmesi 

Diyagram, Azure geçişi Server değerlendirmesi'ni kullanarak VMware VM değerlendirmesi nasıl çalıştığı özetlenmiştir.

![VMware değerlendirmesi mimarisi](./media/migrate-architecture/sas-architecture-vmware.png)

İşlemi aşağıdaki gibidir:

1. **Azure geçişi Gereci dağıtmadan**:

    a. Azure portalından (OVA) şablonunu indirin.

    b. VM oluşturmak için vCenter Server makinesi içe aktarın.

    c. Bu VM'ye bağlanın, temel ayarlarını yapılandırın ve Azure geçişi ile kaydedin.

2. **Başlatma bulma**:

    a. Toplayıcı uygulamayı keşfi başlatmak için gerecinde bağlanın.

    b. Gereç Vm'lerden meta verileri ve performans verilerini Azure'a sürekli olarak gönderir.

    - Gereç her zaman Azure geçişi hizmetine bağlanır.
    - Ortam değişiklikleri sürekli bulma sırasında yakalanır. Örneğin bulma kapsamında Vm'leri ekleme veya VM diskleri veya NIC ekleme.

3. **Makineleri değerlendirme**:

    a. Bulma tamamlandıktan sonra Azure portalı Topladığınızdan Vm'leri gruplar halinde bulundu.  Bir grup genellikle birlikte geçirmek istediğiniz sanal makinelerin oluşur.

    b. Her grup için bir değerlendirme çalıştırın.

4. **Gözden geçirin, değerlendirme**: 

    a. Değerlendirme tamamlandıktan sonra portalda görüntüleyin.

    b. Daha fazla analiz için Excel biçiminde değerlendirmesi'ni indirin.



### <a name="vmware-ports"></a>VMware bağlantı noktaları
Azure geçişi, VMware için aşağıdaki bağlantı noktaları kullanır:

**Kaynak** | **Hedef** | **Bağlantı Noktası** | **Ayrıntılar**
--- | --- | --- | ---
Azure geçişi Gereci | Azure Geçişi hizmeti | Hedef-TCP 443 | Azure geçişi için meta verileri ve performans verilerini gönderin.
Azure geçişi Gereci | vCenter Server | Hedef-TCP 443 | Meta verileri ve performans verileri vCenter Server'a bağlanın. 443 varsayılan ancak farklı bir bağlantı noktası üzerinde dinleyen vCenter ile değiştirilebilir. 
RDP istemcisi | Azure geçişi Gereci | Hedef TCP3389 | Gereç RDP bağlantısı için tetikleyici bulma.

### <a name="collected-vmware-metadata"></a>VMware meta verileri toplandı

Gereç meta verileri ve performans ile ilgili verileri Vm'lerden Azure'a gönderir.

**Eylem** | **Ayrıntılar**
--- | ---
**Toplanan meta verileri** | vCenter VM adı<br/> vCenter VM yolu (konak/küme klasör)<br/> IP ve MAC adresleri<br/> İşletim sistemi<br/> Disk/çekirdek/NIC sayısı<br/> Bellek ve disk boyutu.
**Toplanan performans verileri** | CPU/bellek kullanımı<br/> Her disk verilerini (disk okuma/yazma performansı; disk okuma/yazma / saniye)<br/> NIC verileri (ağ içinde ağ).<br/><br/> Sürekli olarak Gereci vCenter Server'a bağlandıktan sonra performans verileri toplanır. Geçmiş verileri toplanmaz.

## <a name="hyper-v-assessment"></a>Hyper-V değerlendirmesi

Diyagramda, nasıl Azure geçişi Server değerlendirmesi Hyper-V değerlendirme çalışır SING özetler.

![Hyper-V değerlendirmesi mimarisi](./media/migrate-architecture/sas-architecture-hyper-v.png)

İşlemi aşağıdaki gibidir:

1. **Azure geçişi Gereci oluşturma**:

    a. Sıkıştırılmış biçimde VM, Azure portalından indirin.

    b. Bu, bir Hyper-V konağına içeri aktarın.

    c. VM gerecine bağlanma. Bunun için temel ayarları yapılandırma ve Azure geçişi ile kaydedin.

2. **Başlatma bulma**:

    a. Keşfi başlatmak için Azure geçişi gerecine bağlanma. Hiçbir şey bulunan Vm'lerine yüklü olması gerekir.

    b. Gerecin Azure geçişi için VM meta verileri ve performans verilerini sürekli olarak gönderir.

    - Gerecin Azure geçişi hizmetine (CIM oturumlarını kullanma) her zaman bağlı.
    - Ortam değişiklikleri sürekli bulma sırasında yakalanır. Örneğin bulma kapsamında Vm'leri ekleme veya VM diskleri veya NIC ekleme.


3. **Makineleri değerlendirme**:

    a. Bulma tamamlandıktan sonra Azure portalı Topladığınızdan Vm'leri gruplar halinde bulundu.  Bir grup genellikle birlikte geçirmek istediğiniz sanal makinelerin oluşur.

    b. Her grup için bir değerlendirme çalıştırın.

4. **Gözden geçirin, değerlendirme**:

    a. Değerlendirme tamamlandıktan sonra portalda görüntüleyin.

    b. Daha fazla analiz için Excel biçiminde değerlendirmesi'ni indirin.

### <a name="hyper-v-ports"></a>Hyper-V bağlantı noktası

Azure geçişi hizmeti Hyper-v için aşağıdaki bağlantı noktaları kullanır.

**Kaynak** | **Hedef** | **Bağlantı Noktası** | **Ayrıntılar**
--- | --- | --- | ---
Azure geçişi Gereci | Azure Geçişi hizmeti | Hedef-TCP 443 | Azure geçişi için meta verileri ve performans verilerini gönderin.
Azure geçişi Gereci | Hyper-V konak/küme | Hedef varsayılan WinRM bağlantı noktaları-HTTP:5985; HTTPS:5986 | Ana bilgisayar için meta verileri ve performans verilerini bağlanın. Bağlantı için kullanılan CIM oturumu
RDP istemcisi | Azure geçişi Gereci | Hedef TCP3389 | Gereç RDP bağlantısı için tetikleyici bulma.

## <a name="collected-hyper-v-vm-metadata"></a>Hyper-V VM meta verileri toplandı

Gereç meta verileri ve performans ile ilgili verileri Vm'lerden Azure'a gönderir.


**Eylem** | **Ayrıntılar**
--- | ---
**Toplanan meta verileri** | VM adı<br/> VM yolu (konak/küme klasör)<br/> IP ve MAC adresleri<br/> İşletim sistemi<br/> Disk/çekirdek/NIC sayısı<br/> Bellek ve disk boyutu<br/> Disk IOPS, disk aktarım hızı (MB/sn) ağ çıktısı (MB/sn)
**Toplanan performans verileri** |  CPU/bellek kullanımı<br/> Her disk verilerini (disk okuma/yazma performansı; disk okuma/yazma / saniye)<br/> NIC verileri (ağ içinde ağ).<br/><br/> Sürekli olarak gereç Hyper-V ana bilgisayara bağlandıktan sonra performans verileri toplanır. Geçmiş verileri toplanmaz.




## <a name="migration-with-azure-migrate-server-migration"></a>Azure ile geçiş geçirme sunucusu geçişi

Azure geçişi sunucusu geçişi kullanarak, aşağıdaki Azure'a geçirebilirsiniz:

- Şirket içi VMware sanal makineleri
- Şirket içi Hyper-V Vm'leri
- Şirket içi fiziksel sunucuları
- AWS Windows VM örnekleri
- GCP Windows VM örnekleri

Geçiş işlemi biraz farklıdır, hangi bağlı olarak, geçiş yapıyorsanız.


## <a name="migrate-vmware-vms"></a>VMware Vm'lerini geçirme

Azure geçişi sunucusu geçişi geçirme şirket içi VMware Vm'leri için iki yöntem sunar:

- **Aracısız çoğaltma**: Herhangi bir şey yükler gerek kalmadan sanal makineleri geçirin.
- **Aracı tabanlı çoğaltma**. VM çoğaltma için bir aracı yükleyin.

Aracısız çoğaltma dağıtım açısından daha kolay olsa da, şu anda bir dizi sınırlandırması vardır. [Daha fazla bilgi edinin](server-migrate-overview.md) Bu sınırlamalar hakkında.
 

### <a name="agent-based-migration"></a>Aracı tabanlı geçiş

VMware Vm'lerini geçiş aracı tabanlı tabloda özetlendiği gibi çeşitli dağıtılması için bileşenler gerektirir.

**Bileşen** | **Gereksinim** | **Ayrıntılar**
--- | --- | ---
**Configuration server makinesi** | Tek bir VMware VM'kaynağından indirilen bir OVF şablon dağıtılan şirket içi.<br/><br/> Makine yapılandırma sunucusu ve işlem sunucusu dahil tüm şirket içi Site Recovery bileşenlerini çalıştırır. | **Yapılandırma sunucusu**: Şirket içi ile Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir.<br/><br/> **İşlem sunucusu**: Varsayılan olarak yapılandırma sunucusuna yüklenir. Bu çoğaltma verilerini alıp; Bu, önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir; ve Azure'a gönderir. İşlem sunucusu ayrıca Vm'lerinde Azure Site Recovery Mobility hizmetini yükler ve şirket içi makineleri otomatik olarak bulunmasını gerçekleştirir.
**Ulaşım hizmeti** | Çoğalttığınız her VMware sanal makinede Mobility hizmetinin yüklenmesi gerekir. | İşlem sunucusundan otomatik yükleme izin öneririz. Alternatif olarak, hizmetini el ile yükleme veya System Center Configuration Manager gibi bir otomatik dağıtım yöntemini kullanın.





### <a name="agent-based-replication-process"></a>Aracı tabanlı çoğaltma işlemi

![Çoğaltma işlemi](./media/migrate-architecture/v2a-architecture-henry.png)

1. Bir sanal makine için çoğaltmayı etkinleştirdiğinizde, Azure için ilk çoğaltma başlar. 
    - Çoğaltma, blok düzeyinde, yakın sürekli, sanal makinede çalışan Mobility Hizmeti Aracısı kullanılarak yapılır.
    - Çoğaltma İlkesi ayarları uygulanır:
        - **RPO eşiği**. Bu ayar, çoğaltma etkilemez. İzleme ile yardımcı olur. Bir olay tetiklenir ve geçerli RPO, belirttiğiniz eşik sınırını aşarsa, isteğe bağlı olarak bir e-posta, gönderdi.
        - **Kurtarma noktası bekletme**. Bu ayar, bir kesinti oluştuğunda gitmek için istediğiniz zaman içinde ne kadar geriye belirtir. Premium depolama maksimum bekletme 24 saattir. Standart depolama alanında, değer 72 saattir. 
        - **Uygulamayla tutarlı anlık görüntüleri**. Uygulamayla tutarlı anlık görüntü olması gerçekleştirebileceğiniz her 1 uygulama gereksinimlerinize bağlı olarak 12 saat. Anlık görüntü, standart Azure blob anlık görüntüleridir. Bir VM'de çalışan Mobility Aracısı, bu ayar ve -belirli bir noktaya tutarlı bir uygulama olarak çoğaltma akışında noktası yer işaretleri uygun olarak bir VSS anlık görüntüsünün ister.

2. Trafik, internet üzerinden genel uç noktaları Azure depolama alanına çoğaltır.

    - Alternatif olarak, Azure ExpressRoute ile kullanabileceğiniz [Microsoft eşlemesi](../expressroute/expressroute-circuit-peerings.md#microsoftpeering).
    - Trafiği bir siteden siteye sanal özel ağ (VPN) bir şirket içi siteden Azure'a çoğaltılması desteklenmez.
3. Delta değişikliklerinin azure'a çoğaltılması ilk çoğaltma sonlandırıldıktan sonra başlar. Bir makine için izlenen değişiklikler işlem sunucusuna gönderilir.
2. İletişim şu şekilde olur:

    - Vm'leri şirket içi yapılandırma Sunucusu'ndaki HTTPS 443 numaralı bağlantı noktasında gelen çoğaltma yönetimi için iletişim.
    - Yapılandırma sunucusu Azure çoğaltması HTTPS 443 giden bağlantı noktası üzerinden düzenler.
    - Vm'leri, gelen çoğaltma verilerini HTTPS 9443 numaralı bağlantı noktasında (yapılandırma sunucusu makinesinde çalışan) işlem sunucusuna gönderir. Bu bağlantı noktası değiştirilebilir.
    - İşlem sunucusu çoğaltma verilerini alıp, en iyi duruma getirir ve şifreler ve Azure depolamaya bağlantı noktası 443 üzerinden giden gönderir.
5. Çoğaltma verilerinin bir önbellek depolama hesabına azure'da ilk land kaydeder. Bu günlükler işlenir ve bir Azure'da depolanan verileri yönetilen diski (Azure Site Recovery çekirdek diski). Bu diskteki kurtarma noktaları oluşturulur.







## <a name="next-steps"></a>Sonraki adımlar

- Azure Geçişi hakkında [sık sorulan soruları gözden geçirin](resources-faq.md).
- Topluluktan Yardım almak için ziyaret [geçirme Azure MSDN Forumu](https://social.msdn.microsoft.com/Forums/home?forum=AzureMigrate&filter=alltypes&sort=lastpostdesc) veya [Stack Overflow](https://stackoverflow.com/search?q=azure+migrate).

