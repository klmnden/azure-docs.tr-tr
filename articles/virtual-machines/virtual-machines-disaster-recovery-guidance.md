---
title: Azure Vm'leri için olağanüstü durum kurtarma senaryoları | Microsoft Docs
description: Bir Azure hizmet kesintisi Azure sanal makineleri etkiler. olay, yapmanız gerekenler öğrenin.
services: virtual-machines
documentationcenter: ''
author: kmouss
manager: jeconnoc
editor: ''
ms.assetid: 65272148-ff06-4bce-91f1-851d706d4d40
ms.service: virtual-machines
ms.workload: virtual-machines
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: kmouss;aglick
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: dc71e8564b35f4fdd4153a04c66a3d8c5df88c30
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61478853"
---
# <a name="what-to-do-in-the-event-that-an-azure-service-disruption-impacts-azure-vms"></a>Azure Vm'leri bir Azure hizmet kesintisi etkiler olay, yapmanız gerekenler
Microsoft'ta, sabit ihtiyaç duyduğunuzda hizmetlerimizin her zaman sizin için kullanılabilir olduğundan emin olmak için çalışıyoruz. Bize zorlar denetimimiz dışında bazen plansız bir hizmet kesintilerine neden şekillerde etkiler.

Microsoft olarak çalışma süresi ve bağlantı için bir taahhüt hizmetlerinin için bir hizmet düzeyi sözleşmesi (SLA) sağlar. Tek tek Azure hizmetlerinin SLA'sı şu yolda bulunabilir: [Azure hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/support/legal/sla/).

Azure, yüksek kullanılabilirliğe sahip uygulamalar destekleyen birçok yerleşik platform özellikleri zaten var. Bu hizmetler hakkında daha fazla bilgi için okuma [olağanüstü durum kurtarma ve Azure uygulamaları için yüksek kullanılabilirlik](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Bu makale, bir tam bölge ana doğal afet veya yaygın hizmet kesintisi nedeniyle kesinti yaşandığında gerçek bir olağanüstü durum kurtarma senaryosuna kapsar. Nadir oluşum bunlar, ancak tüm bir bölgenin kesinti olma olasılığını için hazırlamanız gerekir. Bir bölge tamamen bir hizmet kesintisi oluşursa, yerel olarak yedekli kopyalar, verilerinizin geçici olarak kullanılamaz. Coğrafi çoğaltma etkinleştirildiğinde, Azure depolama BLOB'ları ve tabloları üç ek kopya farklı bir bölgede depolanır. Tam bölgesel bir kesinti veya birincil bölgenin kurtarılabilir değil bir olağanüstü durum olması durumunda, Azure DNS girdilerini coğrafi olarak çoğaltılmış bölgeye yeniden eşlemesi.

Bu ender örnekleri işlemek yardımcı olması için aşağıdaki yönergeler, Azure sanal makine uygulamanızın dağıtıldığı tüm bölgesinin bir hizmet kesintisi olması durumunda Azure sanal makineleri için sunuyoruz.

## <a name="option-1-initiate-a-failover-by-using-azure-site-recovery"></a>1. seçenek: Azure Site Recovery kullanarak bir yük devretme başlatın.
Uygulamanızı yalnızca birkaç tıklama ile dakika kurtarabilmeniz için Azure Site Recovery yapılandırabilirsiniz. Tercih ettiğiniz bir Azure bölgesine çoğaltabilirsiniz ve eşleştirilmiş bölgeler ile kısıtlı değildir. Tarafından oluşturabileceğinize dair [sanal makinelerinizi çoğaltma](https://aka.ms/a2a-getting-started). Yapabilecekleriniz [bir kurtarma planı oluşturma](../site-recovery/site-recovery-create-recovery-plans.md) böylece uygulamanız için tüm yük devretme işlemini otomatik hale getirebilirsiniz. Yapabilecekleriniz [, yük devretme testi](../site-recovery/site-recovery-test-failover-to-azure.md) üretim uygulama veya sürmekte olan çoğaltmayı etkilemeden önceden. Birincil bölge kesintisi olması durumunda, yeni [bir yük devretme başlatın](../site-recovery/site-recovery-failover.md) ve hedef bölgede uygulamanızı getirin.


## <a name="option-2-wait-for-recovery"></a>2. seçenek: Kurtarma işleminin tamamlanmasını bekleyip
Bu durumda, sizin herhangi bir eylemi gerekli değildir. Yapıyorduk hizmet kullanılabilirliğini geri yüklemeye çalışıyoruz olduğunu bilirsiniz. Geçerli hizmet durumunu görebilirsiniz bizim [Azure hizmet durumu Panosu](https://azure.microsoft.com/status/).

Azure Site Recovery, okuma erişimli coğrafi olarak yedekli depolama veya coğrafi olarak yedekli depolama kesintisi önce ayarlamadıysanız en iyi seçenek budur. Coğrafi olarak yedekli depolama veya okuma erişimli coğrafi olarak yedekli depolama için depolama hesabı, VM sanal sabit diskleri (VHD'ler) depolandığı ayarladıysanız, temel VHD görüntüsünü kurtarmak için Ara ve yeni bir VM'den sağlamayı deneyin. Veri Eşitleme garanti olduğundan, tercih edilen bir seçenek değil. Sonuç olarak, bu seçeneğin çalışması için garanti edilmez.


> [!NOTE]
> Bu işlem üzerinde herhangi bir denetim yok ve yalnızca bölge genelinde hizmet kesintileri için meydana gelir unutmayın. Bu nedenle, aynı zamanda yüksek düzeyde kullanılabilirlik elde etmek için diğer uygulamaya özgü yedekleme stratejiler hakkında durumda. Daha fazla bilgi için üzerinde bölümüne bakın. [olağanüstü durum kurtarma stratejileri veri](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications#data-strategies-for-disaster-recovery).
>
>

## <a name="next-steps"></a>Sonraki adımlar

- Başlangıç [Azure sanal makineler'de çalışan uygulamalarınızı korumaya](https://aka.ms/a2a-getting-started) Azure Site Recovery kullanarak

- Bir olağanüstü durum kurtarma ve yüksek kullanılabilirlik stratejisinin gerçekleştirme hakkında daha fazla bilgi için bkz: [olağanüstü durum kurtarma ve Azure uygulamaları için yüksek kullanılabilirlik](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

- Bir bulut platformunun özelliklerinden ayrıntılı teknik bir anlayış geliştirmek için bkz: [Azure dayanıklılık teknik Kılavuzu](../resiliency/resiliency-technical-guidance.md).


- Yönergeleri değil temizleyin veya sizin adınıza işlem yapmak için Microsoft istiyorsanız ulaşın ise [müşteri desteği](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
