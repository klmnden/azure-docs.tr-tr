<properties
    pageTitle="Site Recovery nedir? | Microsoft Azure" 
    description="Bu sayfada, Azure Site Recovery hizmetine genel bir bakış sağlanır ve hizmetin dağıtımı açıklanır." 
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
    ms.date="02/22/2016" 
    ms.author="raynew"/>

#  Site Recovery nedir?

Azure Site Recovery'ye hoş geldiniz! Site Recovery hizmetine ilişkin hızlı bir genel bakış edinmek ve iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize nasıl katkı sağlayacağına yönelik bilgi edinmek için bu makale ile başlayın.

Azure, kaynak oluşturmak ve bu kaynaklarla çalışmak için iki dağıtım modeli kullanır: [Resource Manager ve klasik](../resource-manager-deployment-model.md). Bu makale, her iki model için de geçerlidir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

## Genel Bakış

Kuruluşunuzun BCDR stratejisinin önemli bir bölümü, planlı veya planlanmamış kesintiler meydana geldiğinde kurumsal iş yüklerini ve uygulamaları açık ve çalışır durumda tutma yöntemlerini ele alır.

Site Recovery, iş yükleri ve uygulamaların kurtarılmasını, yük devretmeyi ve çoğaltmayı düzenleyerek bu işlemi gerçekleştirmenize yardım eder. Böylece, birincil konumunuzun çökmesi durumunda ikincil konum kullanılabilir hale gelir. 

## Neden Site Recovery kullanmalısınız? 

Site Recovery'nin sağladığı avantajlar şunlardır:

- **BCDR sisteminizi basitleştirme** - Site Recovery, birden çok iş yükünün ve uygulamanın çoğaltma, yük devretme ve kurtarma işlemlerinin tek bir konumdan yürütülmesini kolaylaştırır. Site Recovery, çoğaltma ve yük devretme işlemlerini düzenler ancak uygulama verilerinize müdahale etmez veya uygulamanız hakkında bilgiler toplamaz.
- **Esnek çoğaltma sağlama** - Site Recovery kullanarak Hyper-V sanal makinelerinde, VMware sanal makinelerinde ve Windows/Linux fiziksel sunucularında çalışan iş yüklerini çoğaltabilirsiniz. 
- **Kolay yük devretme ve kurtarma** - Site Recovery, üretim ortamlarını etkilemeden olağanüstü durum kurtarma ayrıntılarını desteklemek için yük devretme testleri sunar. Ayrıca, beklenen kesintilere yönelik olarak sıfır veri kaybı sunan planlanan yük devretmeler veya beklenmeyen olağanüstü durumlar için minimum düzeyde veri kaybıyla sonuçlanan (çoğaltma sıklığına bağlı olarak) planlanmamış yük devretmeler çalıştırabilirsiniz. Yük devretmenin ardından birincil sitelerinizde yeniden çalışma işlemini gerçekleştirebilirsiniz. Yük devretme işlemini özelleştirebilmeniz ve çok katmanlı uygulamaları kurtarabilmeniz için Site Recovery, betikleri ve Azure otomasyonu çalışma kitaplarını içeren kurtarma planları sunar. 
- **İkincil veri merkezini ortadan kaldırma** - İkincil şirket içi siteye veya Azure'a çoğaltma işlemini gerçekleştirebilirsiniz. Azure'ı olağanüstü durum kurtarma için hedef olarak kullanma, ikincil bir site kullanmanın maliyetini ve karmaşıklığını ortadan kaldırır. Çoğaltılan veriler, sunduğu tam esneklikle birlikte Azure Storage hizmetinde depolanır.
- **Var olan BCDR teknolojileri ile tümleştirme** - Site Recovery, diğer uygulama BCDR özellikleriyle ortak çalışır. Örneğin, kullanılabilir grupların yük devretme işlemlerini yönetmek için SQL Server AlwaysOn yerel desteği dahil olmak üzere kurumsal iş yüklerinin SWL Server arka ucunu korumak için Site Recovery'yi kullanabilirsiniz. 

## Neleri çoğaltabilirim?

Site Recovery'nin çoğaltabileceği öğelerin özeti:

![Şirket içinden şirket içine](./media/site-recovery-overview/asr-overview-graphic.png)

**ÇOĞALTILAN** | **ÇOĞALTMA KAYNAĞI** | **ÇOĞALTMA HEDEFİ** | **MAKALE**
---|---|---|---
VMware VM'lerinde çalışan iş yükleri | Şirket içi VMware sunucusu | Azure depolama alanı | [Dağıtma](site-recovery-vmware-to-azure-classic.md)
VMware VM'lerinde çalışan iş yükleri | Şirket içi VMware sunucusu | İkincil VMware sitesi | [Dağıtma](site-recovery-vmware-to-vmware.md) 
Hyper-V VM'lerinde çalışan iş yükleri | VMM bulutundaki şirket içi Hyper-V ana bilgisayar sunucusu | Azure depolama alanı | [Dağıtma](site-recovery-vmm-to-azure.md)
Hyper-V VM'lerinde çalışan iş yükleri | VMM bulutundaki şirket içi Hyper-V ana bilgisayar sunucusu | İkincil VMM sitesi | [Dağıtma](site-recovery-vmm-to-vmm.md)
Hyper-V VM'lerinde çalışan iş yükleri | SAN depolama alanı içeren VMM bulutundaki şirket içi Hyper-V ana bilgisayar sunucusu| SAN depolama alanı içeren ikincil VMM sitesi | [Dağıtma](site-recovery-vmm-san.md)
Hyper-V VM'lerinde çalışan iş yükleri | Şirket içi Hyper-V sitesi (VMM yok) | Azure depolama alanı | [Dağıtma](site-recovery-hyper-v-site-to-azure.md)
Fiziksel Windows/Linux sunucuları üzerinde çalışan iş yükleri | Şirket içi fiziksel sunucu | Azure depolama alanı | [Dağıtma](site-recovery-vmware-to-azure-classic.md)
Fiziksel Windows/Linux sunucuları üzerinde çalışan iş yükleri | Şirket içi fiziksel sunucu | İkincil veri merkezi | [Dağıtma](site-recovery-vmware-to-vmware.md) 


## Hangi iş yüklerini koruyabilirim?

Site Recovery uygulamayla tutarlı BCDR sağladığından iş yükleri ve uygulamalar kesinti olduğunda tutarlı bir şekilde çalışmaya devam eder. Site Recovery şunları sağlar: 

- **Uygulamayla tutarlı anlık görüntüler** - Tek veya N-katmanlı uygulamalara yönelik uygulamayla tutarlı anlık görüntüleri kullanan çoğaltma.
**Yakın zaman uyumlu çoğaltma** - Hyper-V için düşük çoğaltma sıklığı (30 saniye) ve VMware için sürekli çoğaltma.
**SQL Server AlwaysOn ile tümleştirme** - Site Recovery kurtarma planlarında kullanılabilir grupların yük devretme işlemlerini yönetebilirsiniz. 
- **Esnek kurtarma planları** - Tek bir tıklama ile tüm uygulama yığınını kurtarmanıza olanak sağlayan Azure Automation Runbook'ları, elle yapılan işlemler ve dış betikler içeren kurtarma planları oluşturabilir ve bunları özelleştirebilirsiniz.
- **Automation kitaplığı** - Zengin Azure Automation kitaplığı, indirilebilen ve Site Recovery ile tümleştirilebilen üretime hazır ve uygulamaya özgü betikler sağlar. 
-**Basit ağ yönetimi** - Site Recovery'deki gelişmiş ağ yönetimi ve Azure; IP adresini koruma, yük dengeleyicileri yapılandırma ve etkili ağ değişimleri için Azure Traffic Manager ile tümleştirme de dahil olmak üzere uygulama ağ gereksinimlerini basitleştirir.


## Sonraki adımlar

- [Site Recovery hangi iş yüklerini koruyabilir?](site-recovery-workload.md) başlığından daha fazla bilgi edinin.
- [Site Recovery nasıl çalışır?](site-recovery-components.md) başlığından Site Recovery mimarisi hakkında daha fazla bilgi edinin.
 



<!---HONumber=Jun16_HO2-->


