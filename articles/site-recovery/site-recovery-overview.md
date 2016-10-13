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
    ms.date="10/05/2016"
    ms.author="raynew"/>


#  Site Recovery nedir?

Azure Site Recovery'ye hoş geldiniz! Site Recovery hizmetine ilişkin hızlı bir genel bakış edinmek ve iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize nasıl katkı sağlayacağına yönelik bilgi edinmek için bu makale ile başlayın.

## Genel Bakış

Kuruluşlar, planlı ve planlanmayan kesinti süreleri boyunca uygulamaların, iş yüklerinin ve verilerin çalışır durumda, kullanılabilir olmasına yönelik izlenecek yolu belirleyen ve mümkün olan en kısa sürede normal çalışma koşullarına dönmeyi sağlayan BCDR stratejisine gereksinim duyar. BCDR stratejinizin işletme verilerini güvende tutması, kurtarılabilir şekilde saklaması ve bir olağanüstü durum sırasında iş yüklerinin sürekli olarak kullanılabilir kalmasını sağlaması gerekir. 

Site Recovery, şirket içi fiziksel sunucuların ve sanal makinelerin buluta (Azure) veya ikincil bir veri merkezine çoğaltılmasını düzenleyerek BCDR stratejinize katkı sağlayan bir Azure hizmetidir. Kesinti birincil konumunuzda meydana gelirse uygulamaları ve iş yüklerini kullanılabilir durumda tutmak için ikincil konuma yük devredersiniz. Normal çalışma koşullarına dönüldüğünde de yükü birincil konuma geri alabilirsiniz. [Site Recovery nedir?](site-recovery-overview.md) bölümünden daha fazla bilgi edinebilirsiniz.

## Azure portalında Site Recovery

Azure, kaynak oluşturmak ve bu kaynaklarla çalışmak için iki [dağıtım modeli](../resource-manager-deployment-model.md) kullanır: Azure Resource Manager modeli ve klasik hizmet yönetimi modeli. Ayrıca Azure iki portala sahiptir: Klasik dağıtım modelini destekleyen [klasik Azure portalı](https://manage.windowsazure.com/) ve her iki dağıtım modeline de destek sağlayan [Azure portalı](https://portal.azure.com).

Site Recovery hem klasik portalda hem de Azure portalında kullanılabilir. Klasik Azure portalında Site Recovery hizmetini klasik hizmet yönetimi modeliyle destekleyebilirsiniz. Azure portalında klasik modeli veya Resource Manager dağıtımlarını destekleyebilirsiniz. Azure portalı ile dağıtma hakkında [daha fazla bilgi edinin](site-recovery-overview.md#site-recovery-in-the-azure-portal).

Bu makaledeki bilgiler hem klasik hem de Azure portalı dağıtımları için geçerlidir. Geçerli olduğu yerlerde farklılıklar belirtilmiştir.


## Neden Site Recovery kullanmalısınız? 

Site Recovery'nin sağladığı avantajlar şunlardır:

- **BCDR sisteminizi basitleştirme** - Site Recovery, birden çok iş yükünün ve uygulamanın çoğaltma, yük devretme ve kurtarma işlemlerinin tek bir konumdan yürütülmesini kolaylaştırır. Site Recovery, çoğaltma ve yük devretme işlemlerini düzenler ancak uygulama verilerinize müdahale etmez veya uygulamanız hakkında bilgiler toplamaz.
- **Esnek çoğaltma sağlama** - Site Recovery kullanarak Hyper-V sanal makinelerinde, VMware sanal makinelerinde ve Windows/Linux fiziksel sunucularında çalışan iş yüklerini çoğaltabilirsiniz. 
- **Kolay yük devretme ve kurtarma** - Site Recovery, üretim ortamlarını etkilemeden olağanüstü durum kurtarma ayrıntılarını desteklemek için yük devretme testleri sunar. Ayrıca, beklenen kesintilere yönelik olarak sıfır veri kaybı sunan planlanan yük devretmeler veya beklenmeyen olağanüstü durumlar için minimum düzeyde veri kaybıyla sonuçlanan (çoğaltma sıklığına bağlı olarak) planlanmamış yük devretmeler çalıştırabilirsiniz. Yük devretmenin ardından birincil sitelerinizde yeniden çalışma işlemini gerçekleştirebilirsiniz. Yük devretme işlemini özelleştirebilmeniz ve çok katmanlı uygulamaları kurtarabilmeniz için Site Recovery, betikleri ve Azure otomasyonu çalışma kitaplarını içeren kurtarma planları sunar. 
- **İkincil veri merkezini ortadan kaldırma** - İkincil şirket içi siteye veya Azure'a çoğaltma işlemini gerçekleştirebilirsiniz. Azure'ı olağanüstü durum kurtarma için hedef olarak kullanma, ikincil bir site kullanmanın maliyetini ve karmaşıklığını ortadan kaldırır. Çoğaltılan veriler, sunduğu tam esneklikle birlikte Azure Storage hizmetinde depolanır.
- **Var olan BCDR teknolojileri ile tümleştirme** - Site Recovery, diğer uygulama BCDR özellikleriyle ortak çalışır. Örneğin, kullanılabilir grupların yük devretme işlemlerini yönetmek için SQL Server AlwaysOn yerel desteği dahil olmak üzere kurumsal iş yüklerinin SWL Server arka ucunu korumak için Site Recovery'yi kullanabilirsiniz. 

## Neleri çoğaltabilirim?

Site Recovery kullanarak çoğaltabileceğiniz öğelerin özeti aşağıda verilmiştir.

![Şirket içinden şirket içine](./media/site-recovery-overview/asr-overview-graphic.png)

**ÇOĞALTILAN** | **ÇOĞALTMA KAYNAĞI (ŞİRKET İÇİ)** | **ÇOĞALTMA HEDEFİ** | **MAKALE**
---|---|---|---
VMware Sanal Makineleri | VMware sunucusu | Azure | [Daha fazla bilgi edinin](site-recovery-vmware-to-azure-classic.md)
VMware Sanal Makineleri | VMware sunucusu | İkincil VMware sitesi | [Daha fazla bilgi edinin](site-recovery-vmware-to-vmware.md) 
Hyper-V Sanal Makineleri | VMM bulutundaki Hyper-V ana bilgisayarı | Azure | [Daha fazla bilgi edinin](site-recovery-vmm-to-azure.md) 
Hyper-V Sanal Makineleri | VMM bulutundaki Hyper-V ana bilgisayarı | İkincil VMM sitesi | [Daha fazla bilgi edinin](site-recovery-vmm-to-vmm.md)
Hyper-V Sanal Makineleri | SAN depolama alanı içeren VMM bulutundaki Hyper-V ana bilgisayarı| SAN depolama alanı içeren ikincil VMM sitesi | [Daha fazla bilgi edinin](site-recovery-vmm-san.md)
Hyper-V Sanal Makineleri | Hyper-V ana bilgisayarı (VMM yok) | Azure | [Daha fazla bilgi edinin](site-recovery-hyper-v-site-to-azure.md)
Fiziksel Windows/Linux sunucuları | Fiziksel sunucu | Azure | [Daha fazla bilgi edinin](site-recovery-vmware-to-azure-classic.md)
Fiziksel Windows/Linux sunucuları üzerinde çalışan iş yükleri | Fiziksel sunucu | İkincil veri merkezi | [Daha fazla bilgi edinin](site-recovery-vmware-to-vmware.md) 


## Hangi iş yüklerini koruyabilirim?

Site Recovery uygulamayla tutarlı BCDR sağladığından iş yükleri ve uygulamalar kesinti olduğunda tutarlı bir şekilde çalışmaya devam eder. Site Recovery şunları sağlar: 

- **Uygulamayla tutarlı anlık görüntüler** - Tek veya N-katmanlı uygulamalara yönelik uygulamayla tutarlı anlık görüntüleri kullanan çoğaltma.
- **Yakın zaman uyumlu çoğaltma** - Hyper-V için düşük çoğaltma sıklığı (30 saniye) ve VMware için sürekli çoğaltma.
- **SQL Server AlwaysOn ile tümleştirme** - Site Recovery kurtarma planlarında kullanılabilir grupların yük devretme işlemlerini yönetebilirsiniz. 
- **Esnek kurtarma planları** - Tek bir tıklama ile tüm uygulama yığınını kurtarmanıza olanak sağlayan Azure Otomasyonu runbook'ları, elle yapılan işlemler ve dış betikler içeren kurtarma planları oluşturabilir ve bunları özelleştirebilirsiniz.
- **Automation kitaplığı** - Zengin Azure Automation kitaplığı, indirilebilen ve Site Recovery ile tümleştirilebilen üretime hazır ve uygulamaya özgü betikler sağlar.
- **Basit ağ yönetimi** - Site Recovery'deki gelişmiş ağ yönetimi ve Azure; IP adresini koruma, yük dengeleyicileri yapılandırma ve etkili ağ değişimleri için Azure Traffic Manager ile tümleştirme dahil olmak üzere uygulama ağ gereksinimlerini basitleştirir.


## Sonraki adımlar

- [Site Recovery hangi iş yüklerini koruyabilir?](site-recovery-workload.md) başlığından daha fazla bilgi edinin.
- [Site Recovery nasıl çalışır?](site-recovery-components.md) başlığından Site Recovery mimarisi hakkında daha fazla bilgi edinin.
 



<!--HONumber=Oct16_HO1-->


