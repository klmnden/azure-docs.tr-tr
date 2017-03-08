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
ms.date: 02/23/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: 31c0d8a3b525d24861090adb0edf0351fae7467e
ms.openlocfilehash: de89ea7309736ac1636e7950d1cd7e68f9ec2f0e
ms.lasthandoff: 02/23/2017


---
# <a name="what-is-site-recovery"></a>Site Recovery nedir?

Azure Site Recovery hizmetine hoş geldiniz!

Bu makalede Azure Site Recovery hizmetine genel bakış sağlanmakta ve daha fazla bilgi için bağlantılar verilmektedir.

İş kesintileri, doğal olaylar ve işletimsel hatalardan kaynaklanır. Kuruluşunuz, planlı veya plansız kapalı kalma süreleri boyunca uygulamalar ile verileri güvende ve kullanılabilir durumda tutmanın yanı sıra mümkün olan en kısa zamanda normal çalışma koşullarına döndüren bir iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejisine ihtiyaç duyar.

Azure kurtarma hizmetleri, BCDR stratejinize katkıda bulunur. Verileri güvenli ve kurtarılabilir durumda tutmak için [Azure Backup](https://docs.microsoft.com/en-us/azure/backup/)'ı kullanın. Hata durumunda ulaşılabilir durumda kalmalarını sağlamak için Site Recovery'yi kullanarak iş yüklerini çoğaltın, kurtarın ve yük devretme gerçekleştirin.

## <a name="benefits"></a>Avantajlar

Site Recovery'nin BCDR stratejinize katkıları şunlardır:

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
- **Zengin kurtarma planları sunar**: Kurtarma planları, birden çok VM’ye dağılmış olan çok katmanlı uygulamalarda, yük devretme ve kurtarma işlemlerinin modellerini oluşturmanıza ve bu işlemleri özelleştirmenize olanak sağlar. Planlar içindeki grupları sıralayabilir, betik ve elle gerçekleştirilen eylem ekleyebilirsiniz. Kurtarma planları, Azure Otomasyonu runbook'ları ile tümleştirilebilir.
- **Çok katmanlı uygulamalar**: Çok katmanlı uygulamalarda sıralı yük devretme ve kurtarma gerçekleştirilmesini sağlayacak kurtarma planları oluşturabilirsiniz. Bir kurtarma planı içinde yer alan farklı katmanlardaki (veritabanı, web, uygulama gibi) makineleri gruplayabilir ve her grubun yük devretme ve başlangıç seçeneklerini özelleştirebilirsiniz.
* **Mevcut BCDR teknolojileriyle tümleştirme**: Site Recovery, diğer BCDR teknolojileriyle tümleştirilebilir. Örneğin, kullanılabilir grupların yük devretme işlemlerini yönetmek amacıyla SQL Server AlwaysOn yerel desteği dahil olmak üzere kurumsal iş yüklerinin SWL Server arka ucunu korumak için Site Recovery'yi kullanabilirsiniz.
* **Otomasyon kitaplığıyla tümleştirme**: Zengin Azure Otomasyonu kitaplığı, indirilebilen ve Site Recovery ile tümleştirilebilen üretime hazır ve uygulamaya özgü betikler sağlar.
* **Basit ağ yönetimi** - Site Recovery'deki gelişmiş ağ yönetimi ve Azure; IP adresini koruma, yük dengeleyicileri yapılandırma ve etkili ağ değişimleri için Azure Traffic Manager ile tümleştirme dahil olmak üzere uygulama ağ gereksinimlerini basitleştirir.

## <a name="site-recovery-in-the-azure-portal"></a>Azure portalında Site Recovery

* Site Recovery hem yeni [Azure portalında](https://portal.azure.com) hem de [klasik Azure portalında](https://manage.windowsazure.com/) dağıtılabilir.
* Klasik Azure portalında, klasik hizmet yönetim modeliyle Site Recovery’yi destekleyebilirsiniz.
* Azure portalında, klasik modeli veya yeni [Resource Manager dağıtım modelini](../azure-resource-manager/resource-manager-deployment-model.md) destekleyebilirsiniz.
- Klasik portal yalnızca var olan Site Recovery dağıtımlarının bakımını yapmak için kullanılmalıdır. Klasik portalda yeni kasalar oluşturamazsınız.

Bu makaledeki bilgiler hem klasik hem de Azure portalı dağıtımları için geçerlidir. İki portal arasında farklılık gösteren durumlar, ilgili yerlerde belirtilmiştir.

## <a name="whats-supported"></a>Desteklenen durumlar

**destekleniyor** | **Ayrıntılar**
--- | ---
**Hangi bölgeler desteklenir?** | [Desteklenen bölgeler](https://azure.microsoft.com/en-us/regions/services/) |
**Neleri çoğaltabilirim?** | Şirket içi VMware VM'leri, Hyper-V VM'leri, Windows ve Linux fiziksel sunucuları.
**Çoğaltılan makineler için desteklenen işletim sistemleri nelerdir?** | VMware VM'leri için [işletim sistemleri](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)<br/><br/> Hyper-V VM'ler için Azure ve Hyper-V tarafından desteklenen tüm [konuk işletim sistemleri](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows) desteklenir.<br/><br/> Fiziksel sunucular için [işletim sistemleri](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)
**Nereye çoğaltabilirim?** | Azure veya ikincil veri merkezi<br/><br/> Yalnızca System Center VMM bulutlarında barındırılan Hyper-V ana bilgisayarlarındaki VM'ler ikincil veri merkezine çoğaltılabilir.
**Hangi VMware sunucularına/ana bilgisayarlarına ihtiyacımız var?** | Çoğaltmak istediğiniz VMware VM'leri [desteklenen vSphere ana bilgisayarları/vCenter sunucuları](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers) ile yönetilebilir
**Hangi iş yüklerini çoğaltabilirim?** | Desteklenen bir çoğaltma makinesinde çalışan tüm iş yüklerini çoğaltabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* [Site Recovery hangi iş yüklerini koruyabilir?](site-recovery-workload.md) başlığından daha fazla bilgi edinin.
* [Site Recovery nasıl çalışır?](site-recovery-components.md) başlığından Site Recovery mimarisi hakkında daha fazla bilgi edinin.

