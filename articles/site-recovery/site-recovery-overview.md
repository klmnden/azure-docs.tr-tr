---
title: Azure Site Recovery nedir? | Microsoft Belgeleri
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
ms.date: 11/01/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: b4b39cd23557093edaec97f7ef7a3e354f1ecd03
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="about-site-recovery"></a>Site Recovery Hakkında

Azure Site Recovery hizmetine hoş geldiniz! Bu makalede hizmet, genel hatlarıyla kısaca ele alınmaktadır.

## <a name="business-continuity-and-disaster-recovery-bcdr-with-azure-recovery-services"></a>Azure Kurtarma Hizmetleri ile iş sürekliliği ve olağanüstü durum kurtarma (BCDR)

Bir kuruluş olarak, planlı veya plansız kesintiler sırasında verilerinizi güvenli, uygulamaları/iş yüklerini ise çalışır halde tutmak için bir yol bulmanız gerekir.

Azure Kurtarma Hizmetleri, BCDR stratejinize katkıda bulunur:

- **Site Recovery hizmeti**: Site Recovery, bir site kullanılamaz hale gelirse uygulamalarınızın kullanılabilen VM’lerde ve fiziksel sunucularda çalışmaya devam etmesini sağlayarak iş sürekliliğine yardımcı olur. Site Recovery, VM ve fiziksel sunucularda çalışan iş yüklerini çoğaltarak birincil sitenin kullanılamaz hale gelmesi durumunda bunların ikincil bir konumda kullanılabilir kalmasını sağlar. Birincil site yeniden çalışmaya başladığında birincil sitede iş yüklerini kurtarır.
- **Backup hizmeti**: Buna ek olarak, [Azure Backup](https://docs.microsoft.com/azure/backup/) hizmeti verilerinizi Azure’a yedekleyerek verilerin güvenli ve kurtarılabilir halde kalmasını sağlar.

Site Recovery, şunlar için çoğaltmayı yönetebilir:

- Azure bölgeleri arasında çoğaltılan Azure VM’leri.
- Azure’a veya ikincil bir siteye çoğaltılan şirket içi sanal makineler ve fiziksel sunucular.


## <a name="what-does-site-recovery-provide"></a>Site Recovery ne sağlar?

**Özellik** | **Ayrıntılar**
--- | ---
**Basit bir BCDR çözümü dağıtma** | Site Recovery hizmetini kullanarak Azure portalındaki tek bir konumdan çoğaltma, yük devretme ve yeniden çalışmayı yönetebilirsiniz.
**Azure VM’lerini çoğaltma** | BCDR stratejinizi Azure VM’lerinin Azure bölgeleri arasında çoğaltılmasını sağlama şeklinde ayarlayabilirsiniz.
**Şirket içi VM’leri şirket dışına çoğaltma** | Şirket içi VM’leri ve fiziksel sunucuları Azure’a veya ikincil bir şirket içi konuma çoğaltabilirsiniz. Azure’a çoğaltma seçeneği, ikincil bir veri merkezi kullanmanın maliyetini ve karmaşıklığını ortadan kaldırır.
**Dilediğiniz iş yükünü çoğaltın** | Desteklenen Azure VM’lerinde, şirket içi Hyper-V VM'lerinde, VMware VM'lerinde ve Windows/Linux fiziksel sunucularında çalışan tüm iş yüklerini çoğaltın.
**Verileri esnek ve güvenli tutun** | Site Recovery, uygulama verilerini kesintiye uğratmadan çoğaltma işlemlerini yönetir. Çoğaltılan veriler, dayanıklılık özelliği sunan Azure Depolama'da saklanır. Yük devretme işlemi gerçekleştiğinde, çoğaltılan veriler temel alınarak Azure VM'leri oluşturulur.
**RTO ve RPO hedeflerinizi yakalayın** | Kurtarma süresi hedeflerini (RTO) ve kurtarma noktası hedeflerini (RPO) kuruluş sınırları içinde tutun. Site Recovery, Azure VM’leri ve VMware VM’leri için sürekli çoğaltma sağlar ve Hyper-V için çoğaltma sıklığı 30 saniyeye kadar düşer. [Azure Traffic Manager](https://azure.microsoft.com/blog/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/) tümleştirmesi sayesinde kurtarma süresi hedeflerini (RTO) daha da düşürebilirsiniz.
**Yük devretme boyunca uygulamalarınızın tutarlı kalmasını sağlayın** | Uygulama içinde tutarlı anlık görüntülerle kurtarma noktaları yapılandırabilirsiniz. Uygulama içinde tutarlı anlık görüntüler, disk verilerini, bellekteki tüm verileri ve devam eden tüm işlemleri yakalar.
**Kesintisiz test edin** | Sürmekte olan çoğaltmayı etkilemeden kolayca yük devretme testleri çalıştırarak olağanüstü durum kurtarma tatbikatlarını destekleyebilirsiniz.
**Esnek yük devretme işlemleri gerçekleştirin** | Beklenen kesintilere yönelik olarak sıfır veri kaybıyla planlı yük devretme işlemleri çalıştırabileceğiniz gibi, beklenmeyen olağanüstü durumlar için en düşük düzeyde veri kaybıyla sonuçlanan (çoğaltma sıklığına bağlı olarak) plansız yük devretme işlemleri de çalıştırabilirsiniz. Yeniden kullanılabilir olduğunda, birincil sitenize kolayca geri dönebilirsiniz.
**Kurtarma planları oluşturma** | Kurtarma planlarını kullanarak birden çok VM’de çok katmanlı uygulamaların yük devretme ve kurtarma işlemlerini özelleştirebilir ve sıralayabilirsiniz. Makineleri planlar içinde gruplandırabilir, betikler ve el ile gerçekleştirilen eylemler ekleyebilirsiniz. Kurtarma planları, Azure Otomasyonu runbook'ları ile tümleştirilebilir.
**Mevcut BCDR teknolojileriyle tümleştirin** | Site Recovery, diğer BCDR teknolojileriyle tümleşir. Örneğin, kullanılabilir grupların yük devretme işlemlerini yönetmek amacıyla SQL Server AlwaysOn yerel desteği dahil olmak üzere kurumsal iş yüklerinin SWL Server arka ucunu korumak için Site Recovery'yi kullanabilirsiniz.
**Otomasyon kitaplığıyla tümleştirin** | Zengin Azure Otomasyonu kitaplığı, indirilebilen ve Site Recovery ile tümleştirilebilen üretime hazır ve uygulamaya özgü betikler sağlar.
**Ağ ayarlarını yönetin** | Site Recovery, Azure ile tümleşerek IP adresleri ayırma, yük dengeleyicileri yapılandırma ve etkili ağ değişimleri için Azure Traffic Manager ile tümleştirme dahil olmak üzere uygulama ağ gereksinimlerini basitleştirir.


## <a name="what-can-i-replicate"></a>Neleri çoğaltabilirim?

**Destekleniyor** | **Ayrıntılar**
--- | ---
**Neleri çoğaltabilirim?** | Azure bölgeleri arasında Azure VM’leri.<br/><br/>  Şirket içi VMware VM’lerinden, Hyper-V VM’lerinden, fiziksel sunuculardan (Windows ve Linux) Azure’a.<br/><br/> Şirket içi VMware VM’lerinden, Hyper-V VM’lerinden, fiziksel sunuculardan Virtual Machine Manager’a (VMM).
**Hangi bölgeler Site Recovery için desteklenir?** | [Desteklenen bölgeler](https://azure.microsoft.com/regions/services/) |
**Çoğaltılan makineler için hangi işletim sistemleri gerekir?** | [Azure VM gereksinimleri](site-recovery-support-matrix-azure-to-azure.md#support-for-replicated-machine-os-versions)</br></br>[VMware VM gereksinimleri](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)<br/><br/> Hyper-V VM’ler için Azure ve Hyper-V tarafından desteklenen tüm [konuk işletim sistemleri](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows) desteklenir.<br/><br/> [Fiziksel sunucu gereksinimleri](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)
**Hangi VMware sunucularına/ana bilgisayarlarına ihtiyacımız var?** | VMware VM’leri [desteklenen vSphere konaklarında/vCenter sunucularında](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers) bulunabilir.
**Hangi iş yüklerini çoğaltabilirim?** | Desteklenen bir çoğaltma makinesinde çalışan tüm iş yüklerini çoğaltabilirsiniz. Ayrıca, Site Recovery ekibi [çeşitli uygulamalar](site-recovery-workload.md#workload-summary) için uygulamaya özgü testler gerçekleştirdi.



## <a name="next-steps"></a>Sonraki adımlar
* [İş yükü desteği](site-recovery-workload.md) hakkında daha fazla bilgi edinin.
* [Bölgeler arasında Azure VM çoğaltma](azure-to-azure-quickstart.md) ile çalışmaya başlayın. 
