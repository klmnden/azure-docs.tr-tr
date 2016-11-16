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
ms.date: 10/30/2016
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 64d085bff08d9a824204851d32504fac3e79024c


---
# <a name="what-is-site-recovery"></a>Site Recovery nedir?
Azure Site Recovery hizmetine hoş geldiniz! Bu makalede Site Recovery, genel hatlarıyla kısaca ele alınmaktadır.

Kuruluşunuz, planlı veya plansız kapalı kalma süreleri boyunca uygulamalar ile verileri güvende ve kullanılabilir durumda tutmanın yanı sıra mümkün olan en kısa zamanda normal çalışma koşullarına döndüren bir iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejisine ihtiyaç duyar.

Site Recovery, şirket içi sanal makinelerin ve fiziksel sunucuların çoğaltma işlemlerini yöneterek BCDR stratejinize katkıda bulunur. Sunucuları ve VM’leri, birincil şirket içi veri merkezinizden buluta (Azure) veya ikincil veri merkezine çoğaltabilirsiniz.

Birincil sitede kesinti olması durumunda, uygulamaları ve iş yüklerini erişilebilir ve kullanılabilir durumda tutmak için ikincil siteye yük devredebilirsiniz. Normal çalışma koşullarına dönüldüğünde de yükü birincil konuma geri alabilirsiniz.

## <a name="site-recovery-in-the-azure-portal"></a>Azure portalında Site Recovery
Azure’da kaynak oluşturmak ve bunlarla çalışmak için iki farklı [dağıtım modeli](../resource-manager-deployment-model.md) vardır. Azure Resource Manager modeli ve klasik hizmet yönetimi modeli. Azure’da ayrıca iki portal bulunur. [Klasik Azure Portalı](https://manage.windowsazure.com/) ve [Azure portalı](https://portal.azure.com).

* Site Recovery, hem klasik portalda hem de Azure portalında dağıtılabilir.
* Klasik Azure portalında, klasik hizmet yönetim modeliyle Site Recovery’yi destekleyebilirsiniz.
* Azure portalında, klasik modeli veya Resource Manager dağıtımlarını destekleyebilirsiniz.

Bu makaledeki bilgiler hem klasik hem de Azure portalı dağıtımları için geçerlidir. İki portal arasında farklılık gösteren durumlar, ilgili yerlerde belirtilmiştir.

## <a name="why-deploy-site-recovery"></a>Neden Site Recovery’yi dağıtayım?
Site Recovery'nin sağladığı avantajlar şunlardır:

* **BCDR sisteminizi basitleştirme**—Azure portalı sayesinde, birden çok iş yükünde çoğaltma, yük devretme ve kurtarma işlemlerini tek bir konumdan gerçekleştirebilirsiniz. Site Recovery, uygulama verilerini kesintiye uğratmadan çoğaltma ve yük devretme işlemlerini yönetir.
* **Esnek çoğaltma olanağı sağlama**—Desteklenen Hyper-V VM’lerinde, VMware VM’lerinde ve Windows/Linux fiziksel sunucularında çalışan tüm iş yüklerini çoğaltabilirsiniz.
* **İkincil veri merkezini kaldırma** - İş yüklerini, ikincil bir site yerine Azure’a çoğaltabilirsiniz. Bu işlem, ikincil bir veri merkezi kullanmanın maliyetini ve karmaşıklığını ortadan kaldırır. Çoğaltılan veriler, dayanıklılık özelliği sunan Azure Depolama’da saklanır. Yük devretme işlemi gerçekleştiğinde, çoğaltılan verilerle Azure VM’leri oluşturulur.
* **Kolayca çoğaltma testi yapma**—Üretim ortamlarını etkilemeden kolayca yük devretme testleri gerçekleştirerek, olağanüstü durum kurtarma tatbikatlarını destekleyebilirsiniz.
* **Yük devretme ve kurtarma**—Beklenen kesintilere yönelik olarak sıfır veri kaybıyla planlı yük devretme işlemleri çalıştırabileceğiniz gibi, beklenmeyen olağanüstü durumlar için en düşük düzeyde veri kaybıyla sonuçlanan (çoğaltma sıklığına bağlı olarak) plansız yük devretme işlemleri de çalıştırabilirsiniz. Yeniden kullanılabilir olduğunda, birincil sitenize geri dönebilirsiniz.
* **Birden çok VM’de yük devretme işlemi**—Betikler ve Azure Otomasyonu runbook’ları içeren kurtarma planları ayarlayabilirsiniz. Kurtarma planları, birden çok VM’ye dağılmış olan çok katmanlı uygulamalarda, yük devretme ve kurtarma işlemlerinin modellerini oluşturmanıza ve bu işlemleri özelleştirmenize olanak sağlar.
* **Mevcut BCDR teknolojileri ile tümleştirme**—Site Recovery, diğer BCDR teknolojileriyle tümleştirilebilir. Örneğin, kullanılabilir grupların yük devretme işlemlerini yönetmek amacıyla SQL Server AlwaysOn yerel desteği dahil olmak üzere kurumsal iş yüklerinin SWL Server arka ucunu korumak için Site Recovery'yi kullanabilirsiniz.

## <a name="what-can-i-replicate"></a>Neleri çoğaltabilirim?
Site Recovery kullanarak çoğaltabileceğiniz öğelerin özeti aşağıda verilmiştir.

![Şirket içinden şirket içine](./media/site-recovery-overview/asr-overview-graphic.png)

| **ÇOĞALTILAN** | **ÇOĞALTMA HEDEFİ** |
| --- | --- |
| Şirket içi VMware sanal makineleri |[Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [İkincil site](site-recovery-vmware-to-vmware.md) |
| VMM bulutlarında yönetilen şirket içi Hyper-V sanal makineleri |[Azure](site-recovery-vmm-to-azure.md)<br/><br/> [İkincil site](site-recovery-vmm-to-vmm.md) |
| SAN depolama alanı ile birlikte VMM bulutlarında yönetilen şirket içi Hyper-V sanal makineleri |[İkincil site](site-recovery-vmm-san.md) |
| VMM içermeyen şirket içi Hyper-V sanal makineleri |[Azure](site-recovery-hyper-v-site-to-azure.md) |
| Şirket içi fiziksel Windows/Linux sunucuları |[Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [İkincil site](site-recovery-vmware-to-vmware.md) |

## <a name="how-does-site-recovery-protect-workloads"></a>Site Recovery, iş yüklerini nasıl korur?
Site Recovery uygulamayla tutarlı çoğaltma sağladığından, iş yükleri ve uygulamalar kesinti olduğunda tutarlı bir şekilde çalışmaya devam eder.

* **Uygulamayla tutarlı anlık görüntüler**—Makineler, tek veya çok katmanlı uygulamalara yönelik uygulamayla tutarlı anlık görüntüler kullanılarak çoğaltılır. Uygulamayla tutarlı anlık görüntüler, disk verilerinin yanı sıra bellekteki tüm verileri ve devam eden tüm işlemleri yakalar.
* **Neredeyse zaman uyumlu çoğaltma**—Site Recovery, Hyper-V için düşük çoğaltma sıklığı (30 saniye) ve VMware için sürekli çoğaltma sağlar.
* **Esnek kurtarma planları** - Dış betikler ve el ile yapılan eylemlerle kurtarma planlarını oluşturabilir ve özelleştirebilirsiniz. Azure Otomasyonu runbook’ları ile tümleştirme, bir uygulama yığının tamamını tek bir tıklamayla kurtarmanızı sağlar.
* **SQL Server AlwaysOn ile tümleştirme**—Kurtarma planlarını kullanarak, kullanılabilirlik gruplarının yük devretme işlemlerini yönetebilirsiniz.
* **Automation kitaplığı** - Zengin Azure Automation kitaplığı, indirilebilen ve Site Recovery ile tümleştirilebilen üretime hazır ve uygulamaya özgü betikler sağlar.
* **Basit ağ yönetimi** - Site Recovery'deki gelişmiş ağ yönetimi ve Azure; IP adresini koruma, yük dengeleyicileri yapılandırma ve etkili ağ değişimleri için Azure Traffic Manager ile tümleştirme dahil olmak üzere uygulama ağ gereksinimlerini basitleştirir.

## <a name="next-steps"></a>Sonraki adımlar
* [Site Recovery hangi iş yüklerini koruyabilir?](site-recovery-workload.md) başlığından daha fazla bilgi edinin.
* [Site Recovery nasıl çalışır?](site-recovery-components.md) başlığından Site Recovery mimarisi hakkında daha fazla bilgi edinin.




<!--HONumber=Nov16_HO2-->


