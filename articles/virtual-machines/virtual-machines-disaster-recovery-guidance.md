---
title: "Azure VM'ler için olağanüstü durum kurtarma senaryoları | Microsoft Docs"
description: "Azure sanal makineleri bir Azure hizmet kesintisi etkiler gerektiğinde, yapmanız gerekenler hakkında bilgi edinin."
services: virtual-machines
documentationcenter: 
author: kmouss
manager: timlt
editor: 
ms.assetid: 65272148-ff06-4bce-91f1-851d706d4d40
ms.service: virtual-machines
ms.workload: virtual-machines
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: kmouss;aglick
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fb986a41e33501ee71c93a48457ac4114e33c671
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="what-to-do-in-the-event-that-an-azure-service-disruption-impacts-azure-vms"></a>Azure sanal makineleri bir Azure hizmet kesintisi etkiler gerektiğinde, yapmanız gerekenler
Microsoft, biz sabit gereksinim duyduğunuzda hizmetlerimizle her zaman için kullanılabilir olduğundan emin olmak için çalışır. Bizim denetim ötesinde zorlar bazen bize planlanmamış hizmet kesintilerine neden şekillerde etkiler.

Microsoft açık kalma süresi ve bağlantı için taahhüdü olarak hizmetlerinin için bir hizmet düzeyi sözleşmesi (SLA) sağlar. SLA tek tek Azure Hizmetleri için şu adreste bulunabilir: [Azure hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/support/legal/sla/).

Azure, yüksek oranda kullanılabilir uygulamaları destekleyen birçok yerleşik platform özellikleri zaten var. Bu hizmetler hakkında daha fazla bilgi için okuma [Azure uygulamaları için yüksek kullanılabilirlik ve olağanüstü durum kurtarma](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Tüm bölge kesinti ana doğal afet ya da yaygın bir hizmet kesintisi nedeniyle yaşadığında bu makalede bir true olağanüstü durum kurtarma senaryosunda kapsar. Nadir oluşum bunlar, ancak tüm bölgesinin bir kesinti olma olasılığını için hazırlamanız gerekir. Tüm bir bölgeyi hizmet kesintisi yaşarsa, verilerinizin yerel olarak yedekli kopyasını geçici olarak kullanılamaz durumda olurdu. Coğrafi çoğaltma etkinleştirildiğinde, tabloları ve Azure Storage bloblarında üç ek kopyalarını farklı bir bölgede depolanır. Azure tam bölgesel bir kesintinin ya da bir olağanüstü durumda birincil bölge kurtarılabilir değil, tüm coğrafi olarak çoğaltılmış bölge için DNS girdilerini remaps.

Bu nadir örnekleri işlemek yardımcı olması için aşağıdaki kılavuzlar, Azure sanal makinesi uygulamanızın dağıtıldığı tüm bölgenin hizmet kesintisi olması durumunda Azure sanal makineleri için sunuyoruz.

## <a name="option-1-initiate-a-failover-by-using-azure-site-recovery"></a>Seçenek 1: Azure Site Recovery kullanarak bir yük devretme başlatın.
Tek bir tıklatmayla sağlasa da, uygulamanızın dakika kurtarabilmeleri için Azure Site Recovery Vm'leriniz için yapılandırabilirsiniz. Tercih ettiğiniz bir Azure bölgesine çoğaltabilir ve eşleştirilmiş bölgelere sınırlı değildir. Tarafından başlayabiliriz [sanal makinelerinizi çoğaltma](https://aka.ms/a2a-getting-started). Yapabilecekleriniz [bir kurtarma planı oluşturma](../site-recovery/site-recovery-create-recovery-plans.md) böylece uygulamanız için tüm yük devretme işlemini otomatik hale getirebilirsiniz. Yapabilecekleriniz [, yük devretme testi](../site-recovery/site-recovery-test-failover-to-azure.md) önceden üretim uygulama veya devam eden çoğaltmayı etkileyen olmadan. Bir birincil bölge kesintisi durumunda yeni [bir yük devretme başlatın](../site-recovery/site-recovery-failover.md) ve uygulamanızı hedef bölgede duruma getirin.


## <a name="option-2-wait-for-recovery"></a>Seçenek 2: Kurtarma için bekleyin
Bu durumda, herhangi bir eyleminiz gereklidir. Titizlikle hizmet kullanılabilirliği geri yüklemek için çalışıyoruz olduğunu bilirsiniz. Geçerli Hizmet durumu görebilirsiniz bizim [Azure hizmet sağlığı panosunu](https://azure.microsoft.com/status/).

Azure Site Recovery, coğrafi olarak yedekli depolamaya okuma erişimi veya coğrafi olarak yedekli depolama kesintisi önce ayarlamadıysanız bu iyi bir seçenektir. Coğrafi olarak yedekli depolama veya depolama hesabı için coğrafi olarak yedekli depolamaya okuma erişimi, VM sanal sabit diskleri (VHD) depolandığı ayarladıysanız, temel görüntü VHD kurtarmak için bakın ve yeni bir VM'den sağlayacak deneyin. Veri Eşitleme garanti olduğundan, tercih edilen bir seçenek değil. Sonuç olarak, bu seçeneğin çalışması için kesin değildir.


> [!NOTE]
> Bu işlem üzerinde herhangi bir denetim yoktur ve yalnızca bölge genelinde hizmet kesilmelerini meydana gelir unutmayın. Bu nedenle, aynı zamanda yüksek düzeyde kullanılabilirlik elde etmek için diğer uygulamaya özgü yedek bir stratejileri kullanmanız gerekir. Daha fazla bilgi için bölümüne bakarak [olağanüstü durum kurtarma veri stratejileri](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications#data-strategies-for-disaster-recovery).
>
>

## <a name="next-steps"></a>Sonraki adımlar

- Başlat [Azure sanal makinelerde çalışan uygulamalarınızı korumaya](https://aka.ms/a2a-getting-started) Azure Site RECOVERY'yi kullanarak

- Bir olağanüstü durum kurtarma ve yüksek kullanılabilirlik stratejisini uygulama hakkında daha fazla bilgi için bkz: [Azure uygulamaları için yüksek kullanılabilirlik ve olağanüstü durum kurtarma](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

- Bir bulut platformun özellikleri ayrıntılı teknik bir anlayış geliştirmek için bkz: [Azure dayanıklılık teknik kılavuz](../resiliency/resiliency-technical-guidance.md).


- Yönergeleri değil işaretini kaldırın veya Microsoft sizin adınıza işlemleri yapmak isterseniz, kişi varsa [müşteri desteği](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
