---
title: Site Recovery nedir? | Microsoft Belgeleri
description: "Bu sayfada, Azure Site Recovery hizmetine genel bir bakış sağlanmış ve dağıtım senaryoları özetlenmiştir."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: cfreeman
editor: 
ms.assetid: e9b97b00-0c92-4970-ae92-5166a4d43b68
ms.service: site-recovery
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 02/26/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: e4bb13a73f6338d2d844a0561edc65063c685d59
ms.openlocfilehash: e554a0ba87efb0272e092a121ba96edc9d9eb011
ms.lasthandoff: 03/02/2017


---
# <a name="what-is-site-recovery"></a>Site Recovery nedir?

Azure Site Recovery hizmetine hoş geldiniz! Bu makalede hizmete genel bakış sağlanmakta ve daha fazla bilgi için bağlantılar verilmektedir.

İş kesintileri, doğal olaylar ve işletimsel hatalardan kaynaklanır. Kuruluşunuz, planlı veya plansız kapalı kalma süreleri boyunca verileri güvende ve uygulamaları kullanılabilir durumda tutmanın yanı sıra mümkün olan en kısa zamanda normal çalışma koşullarına döndüren bir iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejisine ihtiyaç duyar.

Azure kurtarma hizmetleri, BCDR stratejisine katkıda bulunur. Verilerinizi güvenli ve kurtarılabilir durumda tutmak için [Azure Backup](https://docs.microsoft.com/en-us/azure/backup/)’ı kullanın. Site Recovery iş yüklerinin hata durumunda ulaşılabilir durumda kalmalarını sağlamak için bunları çoğaltır, kurtarır ve yük devretme gerçekleştirir.

## <a name="what-does-site-recovery-provide"></a>Site Recovery ne sağlar?

- **Bulutta olağanüstü durum kurtarma**: VM'lerde ve fiziksel sunucularda çalışan iş yüklerini ikinci bir site yerine Azure'da çoğaltabilirsiniz. Bu işlem, ikincil bir veri merkezi kullanmanın maliyetini ve karmaşıklığını ortadan kaldırır.
- **Karma ortamlar için esnek çoğaltma**: Desteklenen şirket içi Hyper-V VM'lerinde, VMware VM'lerinde ve Windows/Linux fiziksel sunucularında çalışan tüm iş yüklerini çoğaltabilirsiniz.
- **Geçirme**: Şirket içi AWS örneklerini Azure VM'lerine veya bir bölgedeki Azure VM'lerini başka bir bölgeye geçirmek için Site Recovery'yi kullanabilirsiniz.
- **Basitleştirilmiş BCDR**: Azure portalındaki tek bir konumdan çoğaltma dağıtabilirsiniz.  Basit yük devretmenin yanı sıra tek ve birden çok makineyi yeniden çalıştırabilirsiniz.
- **Esneklik**: Site Recovery, uygulama verilerini kesintiye uğratmadan çoğaltma ve yük devretme işlemlerini yönetir.
Çoğaltılan veriler, dayanıklılık özelliği sunan Azure Depolama'da saklanır. Yük devretme işlemi gerçekleştiğinde, çoğaltılan veriler temel alınarak Azure VM'leri oluşturulur.
- **Çoğaltma performansı**: Site Recovery, Hyper-V için düşük çoğaltma sıklığı (30 saniye) ve VMware için sürekli çoğaltma sağlar. Veri kurtarma noktalarının oluşturulma sıklığını denetlemek için kurtarma noktası hedefi (RPO) belirleyebilir ve Site Recovery'nin otomatikleştirilmiş kurtarma işlemlerine ek olarak [Azure Traffic Manager](https://azure.microsoft.com/en-us/blog/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/) tümleştirmesi sayesinde kurtarma süresi hedefini (RTO) azaltabilirsiniz.
- **Uygulamayla tutarlılık**: Makineler, uygulamayla tutarlı anlık görüntüler kullanılarak çoğaltılır. Uygulamayla tutarlı anlık görüntüler, disk verilerinin yanı sıra bellekteki tüm verileri ve devam eden tüm işlemleri yakalar.
- **Kesintisiz test gerçekleştirme**: Üretim ortamlarını etkilemeden kolayca yük devretme testleri gerçekleştirerek, olağanüstü durum kurtarma tatbikatlarını destekleyebilirsiniz.
- **Esnek yük devretme ve kurtarma**: Beklenen kesintilere yönelik olarak sıfır veri kaybıyla planlı yük devretme işlemleri çalıştırabileceğiniz gibi, beklenmeyen olağanüstü durumlar için en düşük düzeyde veri kaybıyla sonuçlanan (çoğaltma sıklığına bağlı olarak) plansız yük devretme işlemleri de çalıştırabilirsiniz. Yeniden kullanılabilir olduğunda, birincil sitenize kolayca geri dönebilirsiniz.
- **Özel kurtarma planları**: Kurtarma planları, birden çok VM’e dağılmış olan çok katmanlı uygulamalarda, yük devretme ve kurtarma işlemlerinin modellerini oluşturmanıza ve bu işlemleri özelleştirmenize olanak sağlar. Planlar içindeki grupları sıralayabilir, betik ve elle gerçekleştirilen eylem ekleyebilirsiniz. Kurtarma planları, Azure Otomasyonu runbook'ları ile tümleştirilebilir.
- **Çok katmanlı uygulamalar**: Çok katmanlı uygulamalarda sıralı yük devretme ve kurtarma gerçekleştirilmesini sağlayacak kurtarma planları oluşturabilirsiniz. Bir kurtarma planı içinde yer alan farklı katmanlardaki (veritabanı, web, uygulama gibi) makineleri gruplayabilir ve her grubun yük devretme ve başlangıç seçeneklerini özelleştirebilirsiniz.
* **Mevcut BCDR teknolojileriyle tümleştirme**: Site Recovery, diğer BCDR teknolojileriyle tümleştirilebilir. Örneğin, kullanılabilir grupların yük devretme işlemlerini yönetmek amacıyla SQL Server AlwaysOn yerel desteği dahil olmak üzere kurumsal iş yüklerinin SWL Server arka ucunu korumak için Site Recovery'yi kullanabilirsiniz.
* **Otomasyon kitaplığıyla tümleştirme**: Zengin Azure Otomasyonu kitaplığı, indirilebilen ve Site Recovery ile tümleştirilebilen üretime hazır ve uygulamaya özgü betikler sağlar.
* **Basit ağ yönetimi** - Site Recovery'deki gelişmiş ağ yönetimi ve Azure; IP adresini koruma, yük dengeleyicileri yapılandırma ve etkili ağ değişimleri için Azure Traffic Manager ile tümleştirme dahil olmak üzere uygulama ağ gereksinimlerini basitleştirir.

## <a name="which-azure-portal"></a>Hangi Azure portalı?

* Site Recovery hem yeni [Azure portalında](https://portal.azure.com) hem de [klasik Azure portalında](https://manage.windowsazure.com/) dağıtılabilir.
* Klasik Azure portalında, klasik hizmet yönetim modeliyle Site Recovery’yi destekleyebilirsiniz.
* Azure portalında, klasik modeli veya yeni [Resource Manager dağıtım modelini](../azure-resource-manager/resource-manager-deployment-model.md) destekleyebilirsiniz.
- Klasik portal yalnızca var olan Site Recovery dağıtımlarının bakımını yapmak için kullanılmalıdır. Klasik portalda yeni kasalar oluşturamazsınız.

## <a name="whats-supported"></a>Neler desteklenir?

**Destekleniyor** | **Ayrıntılar**
--- | ---
**Hangi bölgeler Site Recovery için desteklenir?** | [Desteklenen bölgeler](https://azure.microsoft.com/en-us/regions/services/) |
**Neleri çoğaltabilirim?** | Şirket içi VMware VM'leri, Hyper-V VM'leri, Windows ve Linux fiziksel sunucuları.
**Çoğaltılan makineler için hangi işletim sistemleri gerekir?** | VMware VM’leri için [desteklenen işletim sistemleri](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)<br/><br/> Hyper-V VM’ler için Azure ve Hyper-V tarafından desteklenen tüm [konuk işletim sistemleri](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows) desteklenir.<br/><br/> Fiziksel sunucular için [işletim sistemleri](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)
**Nereye çoğaltabilirim?** | Azure depolama alanına veya bir ikincil veri merkezine<br/><br/> Hyper-V için, yalnızca System Center VMM bulutlarında barındırılan Hyper-V ana bilgisayarlarındaki VM’ler ikincil veri merkezine çoğaltılabilir.
**Hangi VMware sunucularına/ana bilgisayarlarına ihtiyacımız var?** | Çoğaltmak istediğiniz VMware VM'leri [desteklenen vSphere ana bilgisayarları/vCenter sunucuları](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers) ile yönetilebilir
**Hangi iş yüklerini çoğaltabilirim?** | Desteklenen bir çoğaltma makinesinde çalışan tüm iş yüklerini çoğaltabilirsiniz. Ayrıca, Site Recovery ekibi [çeşitli uygulamalar](site-recovery-workload.md#workload-summary) için uygulamaya özgü testler gerçekleştirdi.

## <a name="next-steps"></a>Sonraki adımlar
* [İş yükü desteği](site-recovery-workload.md) hakkında daha fazla bilgi edinin
* [Site Recovery mimarisi ve bileşenleri](site-recovery-components.md) hakkında daha fazla bilgi edinin

