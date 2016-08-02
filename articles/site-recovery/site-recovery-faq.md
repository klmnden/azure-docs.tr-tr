<properties 
    pageTitle="Site Recovery: Sıkça sorulan sorular | Microsoft Azure" 
    description="Bu makale, Azure Site Recovery hakkındaki yaygın sorulara yanıtlar sunar."
    services="site-recovery" 
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags 
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na" 
    ms.workload="storage-backup-recovery"
    ms.date="03/27/2016"
    ms.author="raynew"/>


# Azure Site Recovery: Sık sorulan sorular (SSS)

Azure Site Recovery hizmeti hakkında sık sorulan soruları gözden geçirmek için bu makaleyi okuyun.

Makaleyi okuduktan sonra hala sorularınız varsa bunları bu makalenin alt kısmına veya [Azure kurtarma Hizmetleri Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr)'una gönderebilirsiniz.

## Genel

### Site Recovery ne işe yarar?

Site Recovery, şirket içi sanal makinelerden ve fiziksel sunuculardan Azure'a veya ikincil bir veri merkezinde çoğaltmayı düzenleyen ve otomatik hale getirerek iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize katkı sunar. [Daha fazla bilgi edinin](site-recovery-overview.md).

### Site Recovery neleri çoğaltabilir?

- **Hyper-V sanal makineleri**: Site Recovery desteklenen bir Hyper-V VM'de çalışan herhangi bir iş yükünü kopyalayabilir. 
- **Fiziksel sunucular**: Site Recovery, desteklenen Windows/Linux fiziksel sunucusu üzerinde çalışan herhangi bir iş yükünü çoğaltabilir.
- **VMware sanal makineleri**: Site Recovery, desteklenen bir VMware VM üzerinden çalışan herhangi bir iş yükünü çoğaltabilir.


### Hyper-V'de neye sahip olmam gerekir?

Hyper-V ana bilgisayar sunucusu için sahip olmanız gerekenler dağıtım senaryosuna bağlıdır. Aşağıdaki makalelerden Hyper-V önkoşullarını inceleyin:

- [Hyper-V VM'lerini (VMM olmadan) Azure'a çoğaltma](site-recovery-hyper-v-site-to-azure.md#before-you-start)
- [Hyper-V VM'lerini (VMM ile) Azure'a çoğaltma](site-recovery-vmm-to-azure.md#before-you-start)
- [Hyper-V sanal makinelerini ikincil veri merkezine çoğaltma](site-recovery-vmm-to-vmm.md#before-you-start)

Hyper-V ana bilgisayar sunucusunda çalışan konuklar için:

- İkincil bir siteye çoğaltma yapıyorsanız [Hyper-V VM'leri için desteklenen konuk işletim sistemlerinden](https://technet.microsoft.com/library/mt126277.aspx) daha fazla bilgi edinin.
- Azure'a çoğaltma yapıyorsanız Site Recovery [Azure'da desteklenen](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx) aynı konuk işletim sistemlerini destekler.


### Hyper-V, istemci işletim sisteminde çalışıyorsa VM'leri koruyabilir miyim?

Hayır, VM'lerin desteklenen bir Windows sunucusu makinesinde çalışan Hyper-V ana bilgisayar sunucusunda bulunması gerekir. Bir istemci bilgisayarı korumanız gerekirse bu bilgisayarı fiziksel bir makine olarak [Azure](site-recovery-vmware-to-azure-classic.md)'a veya [ikincil veri merkezine](site-recovery-vmware-to-vmware.md) çoğaltabilirsiniz.


### Site Recovery ile hangi iş yüklerini koruyabilirim?

Site Recovery, desteklenen VM veya fiziksel sunucu üzerinde çalışan herhangi bir iş yükünü çoğaltabilir. Site Recovery, uygulamaya duyarlı çoğaltma işlemini desteklediğinden uygulamalar akıllı duruma kurtarılabilir. SharePoint, Exchange, Dynamics, SQL Server ve Active Directory gibi Microsoft uygulamalarıyla tümleşir ve Oracle, SAP, IBM ve Red Hat gibi lider satıcılarla yakın iş birliği kurar. İş yükü koruması hakkında [daha fazla bilgi edinin](site-recovery-workload.md).


### Site Recovery ile çoğaltabilmem için Hyper-V ana bilgisayarlarını System Center VMM bulutlarında mı barındırmalıyım? 

İkincil bir veri merkezine çoğaltma yapmak istiyorsanız Hyper-V VM'lerinin, VMM bulutundaki Hyper-V ana bilgisayar sunucularında bulunması gerekir. Azure'a çoğaltmak yapmak istiyorsanız VM'leri VMM bulutlarıyla ve VMM bulutları olmadan Hyper-V ana bilgisayar sunucularında çoğaltabilirsiniz. [Daha fazla bilgi edinin](site-recovery-hyper-v-site-to-azure.md). 

### Tek bir VMM sunucusuna sahip olsam da Site Recovery'yi VMM ile dağıtabilir miyim? 

Evet. VM'leri Azure'a VMM bulutundaki Hyper-V sunucularında çoğaltabilirsiniz veya aynı sunucudaki VMM bulutlarının arasında çoğaltabilirsiniz. Şirket içinden şirket içine çoğaltma işleminde hem birincil hem de ikincil sitelerde VMM sunucusuna sahip olmanızı öneririz. [Daha fazla bilgi edinin](site-recovery-single-vmm.md)

### Hangi fiziksel sunucuları koruyabilirim?

Azure'da veya ikincil sitede Windows ve Linux işletim sistemleri tarafından desteklenen fiziksel sunucuları koruyabilirsiniz. Azure veya ikincil site çoğaltmalarına ilişkin işletim sistemi gereksinimleri hakkında [bilgi edinin](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment). Azure depolama alanına çoğaltılan fiziksel sunucuların yük devretme işlemi gerçekleştiğinde Azure'da VM olarak çalışacağını unutmayın. Azure'dan şirket içi sitenize yeniden çalıştırdığınızda, bir fiziksel sunucuya yeniden çalıştırma şu anda desteklenmez. Yalnızca VMware üzerinden çalışan bir sanal makinede yeniden çalıştırma işlemini yapabilirsiniz.

### Hangi VMware VM'lerini koruyabilirim?

VMware VM'leri korumak için bir vSphere hiper yöneticisine ve VMware araçlarında çalışan sanal makinelere sahip olmanız gerekir. Ayrıca, hiper yöneticileri yönetmek için bir VMware vCenter sunucusuna sahip olmanızı öneririz. Azure'da veya ikincil bir sitede VMware sunucularını ve VM'leri çoğaltmaya yönelik gereksinimlerinin tamamı hakkında [daha fazla bilgi edinin](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment).

### Site Recovery ile şubelerim için olağanüstü durum kurtarma işlemini yönetebilir miyim?

Evet. Şubelerinizde gerçekleştirdiğiniz çoğaltma ve yük devretme işlemlerini düzenlemek için Site Recovery'yi kullandığınızda, tüm şubelerinizin iş yüklerini tek bir merkezi konumdan görüntüleyebilir ve düzenleyebilirsiniz. Şubelerinizi ziyaret etmenize gerek kalmadan kolaylıkla tüm şubelerinizin olağanüstü durum kurtarma işlemlerini yönetebilir ve yük devretmeler çalıştırabilirsiniz. 

## Güvenlik


### Çoğaltılan veriler Site Recovery hizmetine gönderilir mi?

Hayır, Site Recovery çoğaltılan verilere müdahale etmez veya sanal makinelerde ya da fiziksel sunucularda çalışan uygulamalar hakkında bilgi toplamaz.

Çoğaltma verileri şirket içi Hyper-V ana bilgisayarları, VMware hiper yöneticileri veya fiziksel sunucular ile Azure depolama alanı ya da ikincil siteniz arasında değiştirilir.  Site Recovery bu verilere müdahale edemez. Yalnızca çoğaltma ve yük devretme işlemlerini düzenlemek için gereken meta veriler Site Recovery hizmetine gönderilir. 

Site Recovery, ISO 27001:2005 sertifikalıdır ve HIPAA, DPA ile FedRAMP JAB değerlendirmelerini tamamlama aşamasındadır.

### Uyumluluk sağlamak için şirket içi ortamlarımızdaki meta verilerin bile aynı coğrafi bölge içinde kalması gerekir. Site Recovery bu olanağı sağlayabilir mi?

Evet. Bir bölgede Site Recovery kasası oluşturduğunuzda yük devretme ile çoğaltmayı düzenlemek ve etkinleştirmek için ihtiyacımız olan tüm meta verilerin bölgenin coğrafi sınırları içinde kalmasını sağlarız.

### Site Recovery çoğaltma işlemini şifreleyebilir mi?
Şirket içi siteler arasında sanal makineleri ve fiziksel sunucuları çoğaltma için aktarım sırasında şifreleme desteklenir. Azure'a çoğaltma yapan sanal makineler ve fiziksel sunucular için hem aktarım sırasında şifreleme hem de bekleyen şifreleme (Azure'da) desteklenir. 



## Çoğaltma

### Sanal makinelerin Azure'a çoğaltılması için herhangi bir önkoşul var mı?

Azure'a çoğaltmak istediğiniz sanal makinelerin [Azure gereksinimleri](site-recovery-best-practices.md#azure-virtual-machine-requirements) ile uyumlu olması gerekir.

### Hyper-V 2.nesil sanal makinelerini Azure'a çoğaltabilir miyim?

Evet. Site Recovery, yük devretme sırasında 2.nesil VM'leri 1. nesil VM'lere dönüştürür. Yeniden çalışmada ise VM, 2.nesil VM'ye geri döndürülür. [Daha fazla bilgi edinin](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).

### Azure'a çoğaltma işlemi gerçekleştirirken Azure VM'leri için nasıl ödeme yaparım? 

Normal çoğaltma sırasında, veriler coğrafi olarak yedekli Azure depolama alanına çoğaltılır ve herhangi bir IaaS sanal makine ücreti ödemeniz gerekmez.  Azure'a yük devretme işleminde ise Site Recovery otomatik olarak Azure IaaS sanal makineleri oluşturur ve ardından Azure'da kullandığınız işlem kaynakları için faturalandırılırsınız.

### Site Recovery iş akışlarını otomatikleştirmek için kullanabileceğim bir SDK var mı?

Evet. Rest API'si, PowerShell veya Azure SDK kullanarak Site Recovery iş akışlarını otomatikleştirebilirsiniz. [PowerShell ve Azure Resource Manager kullanarak Site Recovery'yi dağıtma](site-recovery-deploy-with-powershell-resource-manager.md) hakkında daha fazla bilgi edinin.

### Azure'a çoğaltma yaparsam ne tür bir depolama hesabına sahip olmam gerekir?

[Standart coğrafi yedekli depolama](../storage/storage-introduction.md#replication-for-durability-and-high-availability) içeren bir depolama hesabına sahip olmanız gerekir. Premium depolama şu anda desteklenmiyor.

### Verileri ne sıklıkta çoğaltabilirim?

- **Hyper-V**: Windows Server 2012 R2 üzerinde çalışan Hyper-V VM'lerini her 30 saniyede, 5 veya 15 dakikada bir çoğaltabilirsiniz. Bir SAN çoğaltması ayarlarsanız çoğaltma işlemi zaman uyumlu olur.
- **VMware ve fiziksel sunucular:** Burada çoğaltma sıklığı geçerli değildir. Çoğaltma sürekli olacaktır. 

### Çoğaltmayı var olan kurtarma sitesinden başka bir kurtarma sitesine genişletebilir miyim?

Genişletilmiş veya zincir çoğaltma desteklenmez. Bu özellik hakkında [bize geri bildirim gönderin](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959-support-for-exisiting-extended-replication/).


### Azure'a ilk kez çoğaltma yaparken çevrimdışı bir çoğaltma gerçekleştirebilir miyim? 

Bu özellik desteklenmez. Bu özellik hakkında [bize geri bildirim gönderin](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from/).

### Belirli diskleri çoğaltmanın dışında tutabilir miyim?

Bu özellik desteklenmez. Bu özellik hakkında [bize geri bildirim gönderin](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6418801-exclude-disks-from-replication/).

### Sanal makineleri dinamik disklerle çoğaltabilir miyim?

Dinamik diskler, Hyper-V sanal makineleri çoğaltılırken desteklenir. Ayrıca dinamik diskler, [gelişmiş dağıtım](site-recovery-vmware-to-azure-classic.md) kullanıyorsanız VMware VM'lerini ve fiziksel makineleri çoğaltırken de desteklenir. İşletim sistemi diskinin bir temel disk olması gerektiğini unutmayın.

### Hyper-V çoğaltma trafiği için ayrılan bant genişliğini kısıtlayabilir miyim?
- Hyper-V VM'lerini iki şirket içi site arasında çoğaltıyorsanız Windows QoS kullanabilirsiniz. Aşağıda örnek bir betik verilmiştir: 

        New-NetQosPolicy -Name ASRReplication -IPDstPortMatchCondition 8084 -ThrottleRate (2048*1024)
        gpupdate.exe /force

- Hyper-V VM'lerini Azure'da çoğaltıyorsanız şu örnek PowerShell cmdlet'ini kullanarak azaltmayı yapılandırabilirsiniz:

        Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth (512*1024) -NonWorkHourBandwidth (2048*1024)

- Hyper-V trafiğini azaltma hakkında [daha fazla bilgi edinin](https://support.microsoft.com/en-us/kb/3056159).
- Azure'da VMware çoğaltma işlemini azaltma hakkında [daha fazla bilgi edinin](site-recovery-vmware-to-azure-classic.md#capacity-planning).


## Yük devretme

### Azure'a yük devrettikten sonra Azure sanal makinelerine nasıl erişirim? 

Azure VM'lerine güvenli bir İnternet bağlantısı, siteden siteye VPN veya Azure ExpressRoute üzerinden erişebilirsiniz. VPN bağlantısı üzerinden kurulan iletişimler, VM'nin bulunduğu Azure ağındaki dahili bağlantı noktalarına gider. İnternet üzerinden kurulan iletişimler ise VM'lere yönelik olarak Azure bulut hizmetindeki genel uç noktalara eşlenir.

### Azure'a yük devretme konusunda, Azure; verilerimin esnekliğini nasıl garanti eder?

Azure esneklik için tasarlanmıştır. Site Recovery, zaten gerektiğinde Azure SLA ile uyumlu olarak ikincil bir Azure veri merkezine yük devretme çözümü ile geliştirilmiştir. Böyle bir durum gerçekleştiğinde, meta verilerinizin ve kasalarınızın kasanız için seçtiğiniz aynı coğrafi bölge içinde kalmasını sağlarız. 

### İki site arasında çoğaltma yapıyorsam, birincil sitemde beklenmeyen bir kesinti yaşanırsa nasıl bir durumla karşılaşırım?

İkincil siteden planlanmamış bir yük devretme tetikleyebilirsiniz. Bu yük devretme işlemini gerçekleştirmek için Site Recovery'nin birincil siteye bağlanması gerekmez.

### Yük devretme işlemi otomatik midir?

Yük devretme işlemi otomatik değildir. Yük devretmeleri tek bir tıklamayla portaldan başlatabilirsiniz. Alternatif olarak, bir yük devretme tetiklemek için [Site Recovery PowerShell cmdlet](https://msdn.microsoft.com/library/dn850420.aspx)'lerini kullanabilirsiniz. Yeniden çalışma, Site Recovery portalında gerçekleştirilen bir dizi eylemdir.

Yük devretmeyi otomatikleştirmek için şirket içi Orchestrator veya sanal makine hatasını algılamak için Operations Manager kullanabilirsiniz. Ardından, SDK kullanarak yük devretmeyi tetiklersiniz.


## 
### Ben bir hizmet sağlayıcıyım. Site Recovery adanmış veya paylaşılan altyapı modelleri için çalışıyor mu?
Evet, Site Recovery hem adanmış hem de paylaşılan altyapı modellerini destekler.

### Hizmet sağlayıcılarıyla ilgili olarak, kiracımın kimliği Site Recovery hizmetiyle paylaşılır mı?
Hayır. Kiracılarınızın Site Recovery portalına erişmesi gerekmez. Yalnızca hizmet sağlayıcı yöneticisi portal ile etkileşim sağlar.


### Kiracılarımın uygulama verileri Azure'a gider mi?

Hizmet sağlayıcının sahip olduğu siteler arasında çoğaltma yapılırken uygulama verileri asla Azure'a gitmez. Aktarım sırasında veriler şifrelenir ve doğrudan hizmet sağlayıcının siteleri arasında çoğaltılır.

Azure'da çoğaltma yaparken, uygulama verileri Azure depolama alanına gönderilir ancak Site Recovery hizmetine gönderilmez. Veriler aktarım sırasında şifrelenir ve Azure'da şifrelenmiş olarak kalır. 

### Kiracılarıma herhangi bir Azure hizmeti için fatura gönderilir mi?

Hayır. Azure'ın faturalama bağlantısı doğrudan hizmet sağlayıcı ile kurulur. Hizmet sağlayıcılar, kiracıları için belirli faturaları oluşturmaktan sorumludur.

### Azure'a çoğaltma işlemi gerçekleştirirken sanal makinelerin Azure'da sürekli olarak çalışması gerekir mi?

Hayır. Veriler, aboneliğinizdeki coğrafi yedekli Azure depolama hesabına çoğaltılır. Yük devretme testi (DR detayı) veya gerçek bir yük devretme gerçekleştirdiğinizde, Site Recovery otomatik olarak aboneliğinizde sanal makineler oluşturur.

### Azure'a çoğaltma yaptığımda kiracı düzeyinde yalıtım sağlanır mı?

Evet.

### Şu anda hangi platformları destekliyorsanız?

Azure Paketi, Bulut Platformu Sistemi ve System Center (2012 sürümü veya sonraki bir sürüm) dağıtımlarını destekliyoruz. [Sanal makineler için korumayı yapılandırma](https://technet.microsoft.com/library/dn850370.aspx) başlığından Azure Paketi tümleştirmesi hakkında daha fazla bilgi edinin.

### Tek bir Azure Paketi'ni ve tek bir VMM sunucusu dağıtımını destekliyor musunuz?

Evet, Hyper-V sanal makinelerini Azure'da veya hizmet sağlayıcı siteleri arasında çoğaltabilirsiniz.  Hizmet sağlayıcı siteleri arasında çoğaltma yaparsanız Azure runbook tümleştirmesinin kullanılamayacağını unutmayın.


## Sonraki adımlar

- [Site Recovery'ye genel bakış](site-recovery-overview.md) başlığını okuyun.
- [Site Recovery mimarisi](site-recovery-components.md) hakkında bilgi edinin.  



<!--HONumber=Jun16_HO2-->


