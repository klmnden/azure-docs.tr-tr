---
title: Azure Site Recovery ile Azure vm'lerine geçiş Contoso uygulamayla yeniden barındırma | Microsoft Docs
description: Bilgi nasıl bir şirket içi uygulamayı rehost ve Azure Azure Site Recovery kullanarak şirket içi makineleri geçiş için bir yükseltme shift geçiş ile hizmet.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/11/2018
ms.author: raynew
ms.openlocfilehash: 13b36398afdf8eb4db3adeee4ebb821411d813f5
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35300795"
---
# <a name="contoso-migration-rehost-an-on-premises-app-to-azure-vms"></a>Contoso geçiş: Azure VM'ler için bir şirket içi uygulama yeniden barındırma


Bu makalede, nasıl Contoso yeniden barındırma SmartHotel uygulama Azure, Azure Vm'leri için sanal makineleri uygulama geçirerek gösterilmektedir.


Bu belge nasıl Contoso adlı kurgusal şirket için Microsoft Azure bulut şirket kaynaklarını geçirir Göster makaleleri bir dizi biridir. Seri arka plan bilgileri ve geçiş altyapı ayarlamak, geçiş için şirket içi kaynakları değerlendirmek ve geçişler farklı türlerde çalıştıran gösteren senaryolar içerir. Karmaşık senaryolar büyür ve zaman içinde ek makaleleri ekleyeceğiz.


**Makale** | **Ayrıntılar** | **Durum**
--- | --- | ---
[Makale 1: genel bakış](contoso-migration-overview.md) | Contoso'nun geçiş stratejisi, makale serisi ve kullanırız örnek uygulamaları genel bir bakış sağlar. | Kullanılabilir
[Makale 2: bir Azure altyapısı dağıtın](contoso-migration-infrastructure.md) | Nasıl kendi şirket içi ve Azure altyapı Contoso bu geçiş için hazırlar açıklar. Aynı alt tüm Contoso geçiş senaryoları için kullanılır. | Kullanılabilir
[Makale 3: şirket içi kaynakları değerlendirin](contoso-migration-assessment.md)  | Contoso VMware üzerinden çalışan kendi şirket içi iki katmanlı SmartHotel uygulamasının bir değerlendirme nasıl çalıştığı gösterilmektedir. Uygulama VM'ler ile değerlendirmek [Azure geçirmek](migrate-overview.md) hizmet ve uygulama SQL Server veritabanı ile [Azure veritabanı geçiş Yardımcısı](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Kullanılabilir
[Makale 4: Rehost Azure VM'ler ve SQL yönetilen örneği](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso SmartHotel uygulamayı Azure'a nasıl geçirir gösterir. VM ön uç uygulamasını kullanarak geçirmek [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), uygulamayı ve veritabanı kullanma [Azure veritabanı geçiş](https://docs.microsoft.com/azure/dms/dms-overview) yönetilen bir SQL örneğine geçirmek için hizmet. | Kullanılabilir
Makale 5: Rehost Azure vm'lerine (Bu makalede) | Contoso SmartHotel uygulama yalnızca Site Recovery kullanarak sanal makineleri geçirmek nasıl gösterir.
[Makale 6: Azure VM'ler ve SQL Server kullanılabilirlik gruplarını yeniden barındırma](contoso-migration-rehost-vm-sql-ag.md) | Contoso SmartHotel uygulama nasıl geçirir gösterir. Sanal makineleri uygulama ve veritabanı geçiş hizmeti uygulama veritabanı için SQL Server kullanılabilirlik grubu geçirmek için geçirmek için Site Recovery kullanırlar. | Kullanılabilir
[Makale 7: Azure vm'lerde Linux uygulama yeniden barındırma](contoso-migration-rehost-linux-vm.md) | Contoso Linux osTicket uygulama Azure Site Recovery kullanarak sanal makineleri nasıl geçirir gösterir. | Kullanılabilir
[Makale 8: Linux uygulama Rehost Azure VM'ler ve Azure MySQL sunucusu](contoso-migration-rehost-linux-vm-mysql.md) | MySQL çalışma ekranı kullanarak Azure MySQL Server örneğine uygulama veritabanı geçirir ve nasıl Contoso Linux osTicket uygulama Azure Site Recovery kullanarak sanal makineleri geçirir gösterilmiştir. | Kullanılabilir

Bu makalede, iki katmanlı Windows Contoso geçirirsiniz. Azure için VMware Vm'lerinde çalışan NET SmartHotel uygulama. Bu uygulamayı kullanmak istiyorsanız, açık kaynak sağlanır ve buradan indirebilirsiniz [github](https://github.com/Microsoft/SmartHotel360).



## <a name="business-drivers"></a>İş sürücüleri

BT liderlik ekibindeki yakından bu geçişle elde etmek istedikleri anlamak için iş ortaklarıyla çalışmıştır:

- **Adres iş büyümesi**: Contoso artmaktadır ve sonuç olarak şirket içi sistemlerini ve altyapı Basıncı yoktur.
- **Sınırlamak risk**: SmartHotel uygulama Contoso iş için kritik olur. Azure ile sıfır risk geçmek istiyorsunuz.
- **Genişletme**: Contoso uygulaması değiştirmek istediğiniz değil. Yalnızca kararlı olduğundan emin olmak isterler.


## <a name="migration-goals"></a>Geçiş hedefleri

Contoso bulut ekip hedeflerine bu geçiş için aşağı sabitlenmiş. Bu hedefleri en iyi geçiş yöntemini belirlemek için kullanılır:

- Bugün VMware yaptığı gibi geçişten sonra Azure uygulamasında aynı performans özellikleri olması gerekir.  Uygulama içi olarak bulutta kritik olarak kalır. 
- Bu uygulamada yatırım contoso istememektedir.  İş için önemlidir, ancak mevcut haliyle basitçe buluta güvenle taşımak istedikleri.
- Contoso ops modeli Bu uygulama için değiştirmek istediğiniz değil. Bulutta ile artık aynı şekilde etkileşim isterler.
- Contoso herhangi bir uygulamanın işlevsellik değiştirmek istediğiniz değil. Yalnızca uygulama konumunu değiştirir.

## <a name="proposed-architecture"></a>Önerilen mimarisi

Geçerli ortamı İşte

- Uygulama arasında iki VM katmanlı (**WEBVM** ve **SQLVM**).
- Sanal makineleri VMware ESXi ana bilgisayarda bulunan **contosohost1.contoso.com** (sürüm 6.5).
- VMware ortamı vCenter Server 6.5 tarafından yönetilen (**vcenter.contoso.com**), bir VM üzerinde çalışır.
- Contoso olan bir şirket içi veri merkezi (contoso-datacenter)'ile bir şirket içi etki alanı denetleyicisi (**contosodc1 adlı**).
- Geçiş tamamlandıktan sonra şirket içi sanal makineleri Contoso veri merkezinde hizmetten.

![Senaryo mimarisi](./media/contoso-migration-rehost-vm/architecture.png) 

## <a name="migration-process"></a>geçiş işlemi

Contoso uygulaması ön uç ve veritabanı VM'ler Azure Site Recovery kullanarak sanal makineleri geçirme:

- İlk adım olarak, bunlar hazırlamak ve Site kurtarma için Azure bileşenleri ayarlamak ve şirket içi VMware altyapı hazırlayın.
- Zaten sahip oldukları kendi [Azure altyapı](contoso-migration-infrastructure.md) yerinde, böylece yalnızca ihtiyaç duydukları özellikle Site Recovery için birkaç Azure bileşenleri eklemek.
- Olan her şeyi hazır, bunlar Vm'lerini çoğaltma başlatabilirsiniz.
-Çoğaltma etkin ve çalışıyor, bunlar VM'yi Azure'a devrederek tarafından geçirmek sonra.

![geçiş işlemi](./media/contoso-migration-rehost-vm/migraton-process.png) 



### <a name="azure-services"></a>Azure hizmetleri

**Hizmet** | **Açıklama** | **Maliyet**
--- | --- | ---
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | Hizmet düzenler ve geçiş ve olağanüstü durum kurtarma Azure VM'ler için yönetir ve sanal makineleri ve fiziksel sunucuları şirket içi.  | Azure'a çoğaltma sırasında Azure depolama ücretleri ücrete.  Azure VM'ler oluşturulur ve yük devretme oluştuğunda ücretler, uygulanır. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/site-recovery/) ücretleri ve fiyatlandırma hakkında.


## <a name="prerequisites"></a>Önkoşullar

(Ve Contoso) Bu senaryo çalıştırmak gerekenleri aşağıda verilmiştir.

**Gereksinimleri** | **Ayrıntılar**
--- | ---
**Azure aboneliği** | Bir aboneliği erken makaleleri sırasında bu dizide oluşturmuş. Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.<br/><br/> Ücretsiz bir hesap oluşturursanız, aboneliğinizin yöneticisi siz olur ve tüm eylemleri gerçekleştirebilirsiniz.<br/><br/> Var olan bir abonelik kullanın ve Yönetici değilseniz, sahibi veya katkıda bulunan izinleri atamak için yönetici ile çalışmak için gerekir.<br/><br/> Daha ayrıntılı izinler gerekiyorsa, gözden [bu makalede](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Azure altyapı** | [Bilgi nasıl](contoso-migration-infrastructure.md) Contoso Azure altyapıyı ayarlayın.<br/><br/> Özel hakkında daha fazla bilgi [ağ](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) ve [depolama](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) Site Recovery için gereksinimleri.
**Şirket içi sunucular** | Şirket içi vCenter sunucuları 5.5, 6.0 veya 6.5 sürümü çalıştırması gerekir<br/><br/> ESXi konakları sürüm 5.5, 6.0 veya 6.5 çalıştırmanız gerekir<br/><br/> Bir veya daha fazla VMware Vm'lerini ESXi ana bilgisayarında çalışan.
**Şirket içi sanal makineleri** | Sanal makineleri karşılamalıdır [Azure gereksinimleri](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).


## <a name="scenario-steps"></a>Senaryo adımları

Contoso geçiş nasıl çalışacağını aşağıda verilmiştir:

> [!div class="checklist"]
> * **1. adım: Azure Site Recovery için hazırlama**: Çoğaltılan verileri ve bir kurtarma Hizmetleri kasası tutmak için bir Azure depolama hesabı oluşturun.
> * **2. adım: şirket içi VMware Site Recovery için hazırlama**: hesapları VM Keşif ve aracı yüklemesine hazırlamak ve yük devretme sonrasında Azure Vm'lerine bağlanmak hazırlayın.
> * **3. adım: Çoğaltma sanal makineleri**: çoğaltmayı ayarlama ve Azure depolama alanına sanal makineleri çoğaltma işlemi başlatma.
> * **4. adım: Site Recovery ile sanal makineleri geçirmek**: her şeyi çalıştığından emin olmak için yük devretme testi çalıştırın ve ardından sanal makineleri Azure'a geçirmek için tam bir yük devretme çalıştırın.




## <a name="step-1-prepare-azure-for-the-site-recovery-service"></a>1. adım: Azure Site Recovery hizmeti için hazırlama

Contoso sanal makineleri Azure'a geçirmek için gereken Azure bileşenleri şunlardır:

- Yük devretme sırasında oluşturulduğunda, Azure Vm'leri yer alacağı bir VNet.
- Çoğaltılan verileri tutmak için bir Azure depolama hesabı. 
- Azure kurtarma Hizmetleri kasasına.

Bunlar bu aşağıdaki gibi ayarlayın:

1. Site Recovery için kullanabilecekleri bir ağ zaten kurma Contoso olduğunda bunlar [Azure altyapı dağıtılan](contoso-migration-infrastructure.md)

    - Bir üretim uygulaması SmartHotel uygulamadır ve sanal makineleri Azure üretim ağa (VNET-üretim-EUS2) birincil Doğu ABD 2 bölgede geçirilecektir.
    - Her iki VM, üretim kaynakları için kullanılan ContosoRG kaynak grubundaki yerleştirilir.
    - Uygulama ön uç VM (WEBVM) üretim ağındaki ön uç alt ağına (üretim-FE-EUS2) geçirir.
    - Uygulama veritabanı (SQLVM) VM alt ağı (üretim-DB-EUS2), veritabanı üretim ağında geçirirsiniz.

2. Contoso birincil bölgede bir Azure depolama hesabı (contosovmsacc20180528) oluşturur.
    - Depolama hesabının, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.
    - Standart depolama ve LRS çoğaltma genel amaçlı bir hesap ile kullanırlar. 

    ![Site Kurtarma depolama alanı](./media/contoso-migration-rehost-vm/asr-storage.png)

3. Ağ ve depolama hesabıyla yerinde Contoso şimdi bir kurtarma Hizmetleri kasası (ContosoMigrationVault) oluşturur ve birincil Doğu ABD 2 bölge ContosoFailoverRG kaynak grubunda yerleştirir.

    ![Kurtarma Hizmetleri kasası](./media/contoso-migration-rehost-vm/asr-vault.png)

**Daha fazla yardım gerekiyor mu?**

[Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) Azure Site Recovery için ayarlama.


## <a name="step-2-prepare-on-premises-vmware-for-site-recovery"></a>2. adım: şirket içi VMware Site Recovery için hazırlama

İşte Contoso şirket içi hazırlar:

- VM bulma otomatikleştirmek için vCenter sunucusu veya vSphere ESXi ana bilgisayarında, bir hesap.
- VMware Vm'lerinde Mobility hizmeti otomatik olarak yüklenmesini sağlayan bir hesap. 
- VM ayarları, Contoso yük devretme sonrasında çoğaltılmış Azure Vm'lerinin bağlanabilmesi için şirket içi.


### <a name="prepare-an-account-for-automatic-discovery"></a>Otomatik bulma için bir hesap hazırlama

Site Recovery aşağıdakiler için VMware sunucularına erişmesi gerekir:

- Sanal makineleri otomatik olarak bulur. 
- Çoğaltma, yük devretme ve yeniden çalışma VM'ler için yönetirler.
- En az bir salt okunur hesap gereklidir. Hesap oluşturma ve diskleri kaldırma ve sanal makine kapatma gibi işlemleri çalıştırabilir ve olmalıdır.

Contoso hesabı gibi ayarlar:

1. Bunlar vCenter düzeyinde bir rol oluşturur.
2. Bunlar bu rol gerekli izinleri atayın.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Bir hesabı Mobility hizmeti yüklemesi için hazırlama

Her VM mobilite hizmetinin yüklenmesi gerekir.

- VM çoğaltma etkinleştirilmişse, site Recovery Mobility hizmeti otomatik itme yüklemesi yapabilirsiniz.
- Site Recovery VM'ler göndermeli yüklemesi için erişebilmesi için bir hesap gereklidir. Çoğaltma ayarladığınızda, bu hesabı belirtin.
- Hesap etki alanı veya yerel, sanal makinelerin yüklemek için gerekli izinlere sahip olabilir.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Yük devretmeden sonra Azure VM'lerine bağlanmak için hazırlık yapma

Yük devretme sonrasında, Contoso Azure Vm'lerine bağlanmak istiyor. Bunu yapmak için geçiş işleminden önce aşağıdakileri yapın:

1. Internet üzerinden erişim için bunlar:

 - Yük devretmeden önce şirket içi VM üzerinde RDP etkinleştir
 - İçin TCP ve UDP kurallarının eklendiğinden emin olun **ortak** profili.
 - RDP izin onay **Windows Güvenlik Duvarı** > **izin verilen uygulamaları** tüm profiller için.
 
2. Siteden siteye VPN üzerinden erişim için bunlar:

 - RDP şirket içi makinede etkinleştirin.
 - RDP izin **Windows Güvenlik Duvarı** -> **verilen uygulamalar ve Özellikler**, için **etki alanı ve özel** ağlar.
 - Şirket içi VM için işletim sisteminin SAN ilkesinin Ayarla **OnlineAll**.

Ayrıca, bir yük devretme çalıştırdığınızda, bunlar aşağıdakileri denetleyin gerekir:

- Olmamalıdır bekleyen hiçbir Windows güncelleştirmeleri VM üzerinde bir yük devretme tetiklendiğinde. Varsa, güncelleştirme tamamlanana kadar VM'de oturum oturum değiştiremezler.
- Yük devretme sonrasında, bunlar denetleyebilirsiniz **önyükleme tanılama** ekran VM görüntüsünü görüntülemek için. Bu işe yaramazsa, bunlar VM çalıştıran ve bunlar gözden olduğunu doğrulamalısınız [sorun giderme ipuçları](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


**Daha fazla yardım gerekiyor mu?**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) oluşturma ve Otomatik bulma için bir rol atama.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) mobilite hizmetinin göndermeli yüklemesi için bir hesap oluşturma.


## <a name="step-3-replicate-the-on-premises-vms"></a>3. adım: şirket içi Vm'lerini çoğaltma

Bir geçiş için Azure çalıştırmadan önce Contoso ayarlamak ve çoğaltmayı etkinleştirmek gerekir.

### <a name="set-a-replication-goal"></a>Bir çoğaltma hedefi ayarlama

1. Kasada kasa adı (ContosoVMVault) altında çoğaltma hedefi seçin (**Başlarken** > **Site Recovery** > **altyapıyı hazırlama** .
2. Bunlar, kendi makinelerine bulunduğu VMware üzerinden çalışan ve Azure'a çoğaltma şirket olduğunu belirtin.

    ![Çoğaltma hedefi](./media/contoso-migration-rehost-vm/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Dağıtım Planlama onaylayın

Devam etmek için bunların bunlar seçerek dağıtım planlamasını tamamladınız onaylaması **Evet, yaptım**. Bu senaryoda, Contoso yalnızca iki sanal makineleri geçiriyorsanız ve dağıtım planlama olması gerekmez.


### <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Contoso kaynak ortamlarına yapılandırmak gerekir. Bunu yapmak için bunlar bir OVF şablonunu indirebilir ve Site Recovery yapılandırma sunucusu yüksek oranda kullanılabilir olarak dağıtmak için kullanın, şirket içi VMware VM. Yapılandırma sunucunun da çalışır durumda sonra bunlar bu kasaya kaydedin.

Yapılandırma sunucusu bir dizi bileşen çalıştırır:

- Şirket içi ve Azure arasındaki iletişimi düzenler ve veri çoğaltma yöneten yapılandırma sunucusu bileşeni.
- Bir çoğaltma ağ geçidi olarak davranır işlem sunucusu. Çoğaltma verilerini alıp bunları önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir ve Azure depolamaya gönderir.
- İşlem sunucusu aynı zamanda çoğaltmak istediğiniz VM’lere Mobility Hizmetini yükler ve şirket içi VMware VM’lerinin otomatik olarak bulunmasını sağlar.



Contoso adımları aşağıdaki gibi gerçekleştirin:

1. OVF şablondan indirdikleri kasasına **altyapıyı hazırlama** > **kaynak** > **yapılandırma sunucusu**.
    
    ![OVF indirin](./media/contoso-migration-rehost-vm/add-cs.png)

2. Bunlar, oluşturma ve VM dağıtmak için VMware şablonu içeri aktarın.

    ![OVF şablonu](./media/contoso-migration-rehost-vm/vcenter-wizard.png)

3.  VM üzerinde ilk kez açtığınızda, Windows Server 2016 yükleme deneyimi ön. Lisans sözleşmesini kabul edin ve bir yönetici parolası girin.
4. Yükleme tamamlandıktan sonra bunların VM'ye yönetici olarak oturum açın. İlk oturum açma sırasında varsayılan olarak Azure Site kurtarma Yapılandırması aracını çalıştırır.
5. Aracı, bunlar yapılandırma sunucusunu kasaya kaydetmek için bir ad belirtin.
6. Araç, VM’nin Azure bağlanıp bağlanamadığını denetler. Bağlantı kurulduktan sonra kullanıcılar Azure aboneliği ile oturum açın. Kimlik bilgilerini yapılandırma sunucusu kaydedeceksiniz kasa erişimi olmalıdır.

    [Yapılandırma sunucusu kaydetme](./media/contoso-migration-rehost-vm/config-server-register2.png)

7. Araç bazı yapılandırma görevleri gerçekleştir ve sonra yeniden başlatılır.
8. Kullanıcılar için makine yeniden oturum açın ve yapılandırma sunucusu yönetim Sihirbazı'nı otomatik olarak başlatır.
9. Sihirbazda, çoğaltma trafiği almaya NIC seçerler. Bunu yapılandırıldıktan sonra bu ayar değiştirilemez.
10. Bunlar, abonelik, kaynak grubu ve yapılandırma sunucunun kaydedileceği kasayı seçin.
        ![Kasa](./media/contoso-migration-rehost-vm/cswiz1.png) 

10. Bunlar, indirin ve MySQL Server ve VMWare Powerclı yükleyin. 
11. Doğrulama sonrasında, bunlar vCenter sunucusu veya vSphere ana bilgisayar FQDN veya IP adresini belirtin. Bunlar varsayılan bağlantı noktası bırakın ve Azure'da sunucu için bir kolay ad belirtin.
12. Bunlar, otomatik bulma için oluşturduğunuz hesabı ve Mobility hizmetinin otomatik olarak yüklemek için kullanılan kimlik bilgilerini belirtin. Windows makineler için hesap VM'ler üzerinde yerel yönetici ayrıcalıkları gerekiyor.

    ![vCenter](./media/contoso-migration-rehost-vm/cswiz2.png)

7. Kayıt tamamlandığında Azure portalında Contoso çift VMware sunucusu ve yapılandırma sunucusu listelendiğini denetler **kaynak** kasası sayfasında. Bulma, 15 dakika veya daha fazla sürebilir. 
8. Site Recovery belirtilen ayarları kullanarak VMware sunucularına bağlanır ve sanal makineleri bulur.

### <a name="set-up-the-target"></a>Hedefi ayarlama

Şimdi Contoso hedef çoğaltma ayarlarını belirtir.

1. İçinde **altyapıyı hazırlama** > **hedef**, hedef ayarlarını seçin.
2. Site Recovery, bir Azure depolama hesabı ve ağ belirtilen hedef konumda olup olmadığını denetler.

### <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

Artık Contoso bir çoğaltma ilkesi oluşturabilirsiniz.

1. İçinde **altyapıyı hazırlama** > **çoğaltma ayarları** > **Çoğaltma İlkesi** >  **oluşturma ve İlişkilendirme**, bir ilke oluşturmak **ContosoMigrationPolicy**.
2. Bunlar varsayılan ayarları kullanın:
    - **RPO eşiği**: varsayılan 60 dakika. Bu değer kurtarma noktalarının hangi sıklıkta oluşturulacağını tanımlar. Devamlı çoğaltma bu sınırı aşarsa bir uyarı oluşturulur.
    - **Kurtarma noktası bekletme**. Varsayılan 24 saatlik. Bu değer ne kadar bekletme penceresi için her kurtarma noktası belirtir. Çoğaltılan VM’ler bir aralıktaki herhangi bir noktaya kurtarılabilir.
    - **Uygulamayla tutarlı anlık görüntü sıklığı**. Varsayılan bir saat. Bu değer oluşturulmasında ve uygulamayla tutarlı anlık görüntü sıklığı belirtir.

        ![Çoğaltma ilkesi oluşturma](./media/contoso-migration-rehost-vm/replication-policy.png)

5. İlke, yapılandırma sunucusu ile otomatik olarak ilişkilendirilir. 

    ![Çoğaltma İlkesi ilişkilendirme](./media/contoso-migration-rehost-vm/replication-policy2.png)

### <a name="enable-replication-for-webvm"></a>WEBVM için çoğaltmayı etkinleştirme

Her yerde Contoso VM'ler için etkinleştirebilirsiniz. Bunların WebVM ile başlar.

1. İçinde **uygulama çoğaltma** > **kaynak** > **+ Çoğalt** kaynak ayarlarını seçin.
2. Bunlar, bunlar, sanal makineleri etkinleştirmek istiyorsanız, vCenter sunucusu ve yapılandırma sunucusu seçin gösterir.

    ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-vm/enable-replication1.png)

3. Bunlar hedef ayarları, kaynak grubu ve Azure ağ ve depolama hesabı seçin.

     ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-vm/enable-replication2.png)

4. Contoso seçer **WebVM** çoğaltma için çoğaltma ilkesi denetler ve çoğaltmayı etkinleştirir.

    - Bu aşamada Contoso yalnızca çünkü VNet ve alt ağ seçilmelidir ve Contoso uygulaması VM'ler farklı alt ağlarda yerleştirme WEBVM seçer.
    - Çoğaltma etkin olduğunda site Recovery Mobility hizmeti VM üzerinde otomatik olarak yükler.

    ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-vm/enable-replication3.png)

5. Çoğaltma sürüyor izlemek **işleri**. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.
6. İçinde **Essentials** Azure portalında, Azure'a çoğaltma VM'ler için yapısı Contoso görebilirsiniz.


### <a name="enable-replication-for-sqlvm"></a>SQLVM için çoğaltmayı etkinleştirme

Şimdi Contoso SQLVM makinenin çoğaltıldığını, aynı işlemi yukarıdaki kullanarak başlatabilirsiniz.

1. Bunlar kaynak ayarları seçin.

    ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-vm/enable-replication1.png)

2. Bunlar daha sonra hedef ayarlarını belirtin.

     ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-vm/enable-replication2-sqlvm.png)

3. Çoğaltma için SQLVM seçerler. 

    ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-vm/enable-replication3-sqlvm.png)

4. Bunlar WEBVM için kullanılan aynı çoğaltma ilkesi uygulamak ve çoğaltmayı etkinleştirin.

    ![Altyapı görünümü](./media/contoso-migration-rehost-vm/essentials.png)

**Daha fazla yardım gerekiyor mu?**

- Bu adımları tam bir kılavuz okuyabilirsiniz [şirket içi VMware Vm'leri için olağanüstü durum kurtarma kümesi](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Ayrıntılı yönergeler size yardımcı olmak için kullanılabilir [kaynak ortamını ayarlama](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [yapılandırma sunucusu dağıtmak](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), ve [çoğaltma ayarlarını yapılandırın](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- Hakkında daha fazla bilgiyi [çoğaltma etkinleştirme](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-4-migrate-the-vms"></a>4. adım: sanal makineleri geçirme 

Contoso hızlı sınama yük devretme ve sanal makineleri geçirmek için tam bir yük devretme çalıştırın.

### <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

Yük devretme testi her şeyin beklendiği gibi çalıştığını sağlamaya yardımcı olur. 

1. Contoso çalıştıran yük devretme sınaması için son noktası zamanında (**son işlenen**).
2. Seçtikleri **yük devretme işlemine başlamadan önce makineyi kapatın**, böylece yük devretme tetiklemeden önce kaynak VM kapatmak Site Recovery çalışır. Kapatma işlemi başarısız olsa bile yük devretme devam eder. 
3. Sınama yük devretme çalıştırır: 

    - Tüm geçiş için gereken koşulların karşılandığından emin olmak için çalışır bir ön koşullarını denetleyin.
    - Yük devretme, bir Azure sanal makinesi oluşturulabilmesi için verileri işler. En son kurtarma noktasını seçerseniz verilerden bir kurtarma noktası oluşturulur.
    - Önceki adımda işlenen veriler kullanılarak bir Azure sanal makinesi oluşturulur.
    
3. Yük devretme işlemi tamamlandıktan sonra çoğaltma Azure portalında Azure VM görüntülenir. Contoso VM sağ ağa bağlı uygun boyutta olduğundan ve çalışır durumda olduğunu denetler. 
4. Yük devretme sınaması doğruladıktan sonra bunlar yük devretmeyi, temizleme ve kayıt ve gözlemlerinizi kaydetmek. 

### <a name="create-and-customize-a-recovery-plan"></a>Oluşturma ve bir kurtarma planı özelleştirme

 Yük devretme sınaması beklenen şekilde çalıştığını doğruladıktan sonra Contoso geçiş için bir kurtarma planı oluşturur. 

- Bir kurtarma planı sırayı belirtir yük oluşur ve nasıl Azure VM'ler çevrimiçi Azure'da gidersiniz gösterir.
- İki katmanlı uygulama olduğuna göre verileri VM (SQLVM) (WEBVM) ön uç önce başlar. böylece bunlar kurtarma planını özelleştirin.

1. İçinde **kurtarma planları (Site Recovery)** > **+ kurtarma planı**, bir plan oluşturur ve sanal makineleri ekleyin.

    ![Kurtarma planı](./media/contoso-migration-rehost-vm/recovery-plan.png)

2. Planı oluşturduktan sonra bunların özelleştirin (**kurtarma planları** > **SmartHotelMigrationPlan** > **Özelleştir**).
2.  WEBVM öğesinden Kaldır **Grup 1: Başlangıç**.  Bu, ilk başlatma eylemini SQLVM yalnızca etkiler sağlar.
3.  İçinde **+ grup** > **Ekle korunan öğeler**, Grup 2'ye WEBVM ekledikleri: başlatın.  Sanal makineleri iki farklı gruplarında olması gerekir.


### <a name="migrate-the-vms"></a>Vm'leri geçirme


Şimdi Contoso geçiş işlemini tamamlamak için tam bir yük devretme çalıştırabilirsiniz.

1. Kurtarma planı seçin > **yük devretme**.
2. Bunların en son kurtarma noktası üzerinden vermesine seçin ve o Site Recovery yük devretme tetiklemeden önce şirket içi VM kapatma çalışmanız gerekir. Bunlar yük devretme işleminin ilerleyişini takip edebilirsiniz **işleri** sayfası.

    ![Yük devretme](./media/contoso-migration-rehost-vm/failover1.png)


3. Yük devretme sonrasında, bunlar Azure VM Azure portalında beklendiği gibi göründüğünü doğrulayın.

    ![Yük devretme](./media/contoso-migration-rehost-vm/failover2.png)  

3. Doğrulamadan sonra bunların her VM için geçişi tamamlayın. Bu sanal makine için çoğaltmayı durdurur ve onun için Site Recovery Faturalaması durdurulur.

    ![Yük devretme](./media/contoso-migration-rehost-vm/failover3.png)

**Daha fazla yardım gerekiyor mu?**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) yük devretme testi çalıştırma. 
- [Bilgi](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) bir kurtarma planı oluşturma.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) Azure'a yük devrediliyor.

## <a name="clean-up-after-migration"></a>Geçişten sonra Temizleme

Tam geçiş ile SmartHotel uygulama katmanları artık Azure Vm'lerinde çalışıyor.

Şimdi, Contoso temizleme adımları tamamlaması gerekir:  

- WEBVM makine vCenter stoktan kaldırın.
- SQLVM makine vCenter stoktan kaldırın.
- WEBVM ve SQLVM yerel yedekleme işlerden kaldırın.
- VM'ler için yeni konum ve IP adreslerinin göstermek için iç belgeleri güncelleştirin.
- Sanal makineleri ile etkileşim herhangi bir kaynağa gözden geçirin ve herhangi bir ilgili ayarları veya belge yeni yapılandırmayı yansıtacak şekilde güncelleştirin.

## <a name="review-the-deployment"></a>Dağıtım gözden geçirin

Uygulamayı şimdi çalıştıran Contoso artık tam olarak faaliyete ve Azure'da güvenli gerekir.

### <a name="security"></a>Güvenlik

Contoso güvenlik ekibine güvenlik sorunları belirlemek için Azure VM'ler, inceler.

- Erişimi denetlemek için bunlar ağ güvenlik grupları (Nsg'ler) VM'ler için gözden geçirin. Nsg'ler yalnızca uygulama için izin verilen trafik, ulaşabileceği emin olmak için kullanılır.
- Bunlar, ayrıca Azure Disk şifrelemesi ve KeyVault kullanarak disk üzerindeki verilerin güvenliğini sağlama düşünün.

[Daha fazla bilgi](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) VM'ler için güvenlik uygulamaları hakkında.

### <a name="backups"></a>Yedeklemeler

Contoso Azure Yedekleme hizmetini kullanarak sanal makinelerin verilerini yedeklemek için geçiyor. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="licensing-and-cost-optimization"></a>Lisans ve maliyet en iyi duruma getirme

1. Contoso Vm'leri için var olan lisans var ve Azure karma avantajı özelliğinden yararlanır.  Bunlar, mevcut Azure Bu fiyatlandırma avantajlarından yararlanmak için VM'ler dönüştürür.
2. Contoso Azure maliyeti Cloudyn, Microsoft ofisine lisanslı yönetim olanağı sağlar. Bunu kullanan ve Azure ve diğer bulut kaynakları yönetmenize yardımcı olan bir çok bulut maliyeti yönetimi çözümüdür. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/cost-management/overview) Azure maliyeti yönetimi hakkında. 

## <a name="conclusion"></a>Sonuç

Bu makalede, Azure Site Recovery hizmetini kullanarak sanal makineleri VM'ler uygulama geçirerek Contoso Azure SmartHotel uygulamada rehosted. 


## <a name="next-steps"></a>Sonraki adımlar

Serideki sonraki makalede biz Contoso SmartHotel uygulama ön uç Site Recovery hizmetini kullanarak azure'da VM nasıl yeniden barındırma Göster ve veritabanı için bir Azure SQL AlwaysOn Kullanılabilirlik grubu geçirilir.

