---
title: include dosyası
description: include dosyası
services: virtual-machines
author: shants123
ms.service: virtual-machines
ms.topic: include
ms.date: 07/05/2018
ms.author: shants
ms.custom: include file
ms.openlocfilehash: abf10177f8ce86309043da92d1f2b690775b6d89
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37910040"
---
Azure sanal makine konak altyapısının güvenilirlik, performans ve güvenliğini iyileştirmek için düzenli olarak güncelleştirmeler yapar. Bu güncelleştirmeler barındırma ortamındaki yazılım bileşenlerine (işletim sistemi, hiper yönetici ve konak üzerinde dağıtılmış olan çeşitli aracılar) düzeltme eki uygulama, ağ bileşenlerini yükseltme ve donanımların kullanımdan çıkarılması gibi işlemler olabilir. Bu güncelleştirmelerin çoğu barındırılan sanal makinelere sunucuları etkilenmeden gerçekleştirilir. Ancak, güncelleştirmeleri bir etkiye sahip olduğu durumlar da vardır:

- Bir yeniden başlatma daha az güncelleştirme Mümkünse, Azure VM konak güncelleştirildiğinde ya da VM tamamen zaten güncelleştirilmiş bir konağa taşındığında duraklatmak için Bakımı koruma bellek kullanır.

- Bakım yeniden başlatma gerektirirse, ne zaman bunu planlı bakım, bir bildirim alın. Bu gibi durumlarda, aynı zamanda bakım kendiniz için en uygun zamanda başlayabileceğiniz bir zaman penceresi verilir.

Bu sayfayı Microsoft Azure'nın bakım her iki tür nasıl gerçekleştirdiğini açıklar. Planlanmamış olaylar (kesintileri) hakkında daha fazla bilgi için sanal makinelerin kullanılabilirliğini yönetme [Windows] bakın (../articles/virtual-machines/windows/manage-availability.md) veya [Linux](../articles/virtual-machines/linux/manage-availability.md).

Bir sanal makinede çalışan uygulamalar için Azure meta veri hizmeti kullanarak gelecek güncelleştirmeleri hakkında bilgi toplayabilir [Windows](../articles/virtual-machines/windows/instance-metadata-service.md) veya [Linux] (../articles/virtual-machines/linux/instance-metadata-service.md).

Planlı bakım yönetme "nasıl yapılır" için "İşleme planlı bakım bildirimlerini" için bilgi [Linux](../articles/virtual-machines/linux/maintenance-notifications.md) veya [Windows](../articles/virtual-machines/windows/maintenance-notifications.md).

## <a name="memory-preserving-maintenance"></a>Bakımı koruma bellek

Güncelleştirmeleri tam bir yeniden başlatma gerektirmez, bellek koruma bakım mekanizmaları sanal makineye etkisini sınırlamak için kullanılır. Sanal makine en fazla 30 barındırma ortamı gerekli güncelleştirmeleri ve yamaları geçerli ya da sanal makine zaten güncelleştirilmiş bir konağa taşır, RAM belleği koruma saniye duraklatıldı. Ardından sanal makine sürdürülür ve sanal makinenin saati otomatik olarak eşitlenir. 

Hata etki alanı tarafından uygulanan hata etki alanı bu rebootful olmayan bakım işlemleridir ve hiçbir uyarı sistem durumu sinyali alınırsa ilerleme durduruldu.

Bazı uygulamalar, bu türden güncelleştirmeler etkilenebilir. Gerçek zamanlı Olay işleme, medya akışı veya kodlama dönüştürme veya yüksek aktarım hızı senaryoları, ağ gibi gerçekleştiren uygulamalar tolere 30 bir saniyelik duraklama için tasarlanmamış olabilir. <!-- sooooo, what should they do? --> Farklı bir konağa sanal makine taşınırken durumunda, önemli bazı iş yükleri için sanal makineyi duraklatma sonlanan birkaç dakika içinde küçük bir performans düşüşü görebilirsiniz. 


## <a name="maintenance-requiring-a-reboot"></a>Yeniden başlatma gerektiren bakım

VM'ler için planlı bakım yeniden başlatılması gerektiğinde önceden bilgilendirilirsiniz. Planlı bakım için iki aşaması vardır: Self Servis ve zamanlanmış bakım penceresinde.

**Self Servis penceresi** Vm'leriniz üzerinde Bakımı sizin başlatmanıza olanak tanır. Bu süre boyunca, kendi durumunu görmek ve Bakım isteğiniz sonucunu denetlemek için her bir VM sorgulayabilirsiniz.

Self Servis bakım başlattığınızda, sanal makinenizin zaten güncelleştirildi ve yeniden destekleyen bir düğüme taşınır. VM yeniden geçici disk kaybolur ve sanal ağ arabirimiyle ilişkilendirilmiş dinamik IP adresleri güncelleştirildi.

Self Servis bakım başlatın ve işlemi sırasında bir hata ise, işlem durdurulur, VM güncelleştirilmez ve Self Servis bakım yeniden dene seçeneğine sahip olursunuz. 

Self Servis penceresi geçtiğinde **zamanlanan bakım penceresinden** başlar. Bu zaman penceresi boyunca yine de bakım penceresi için sorgu, ancak artık bakım kendiniz başlatabilirsiniz.

Görmek için yeniden başlatma gerektiren bakım yönetme hakkında daha fazla bilgi için "İşleme planlı bakım bildirimlerini" [Linux](../articles/virtual-machines/linux/maintenance-notifications.md) veya [Windows](../articles/virtual-machines/windows/maintenance-notifications.md). 

### <a name="availability-considerations-during-scheduled-maintenance"></a>Zamanlanan bakım sırasında kullanılabilirlik konuları 

Zamanlanmış bakım penceresini beklemek karar verirseniz, sanal makinelerinizin yüksek kullanılabilirliğini korumak için dikkate alınması gereken birkaç nokta vardır. 

#### <a name="paired-regions"></a>Eşleştirilmiş bölgeler

Her Azure bölgesi aynı coğrafyadaki başka bir bölgeyle eşleştirilir, bölgesel çift birlikte yapmaları. Planlanan bakım sırasında Azure bölgesi çiftinin tek bir bölgedeki Vm'leri yalnızca güncelleştirir. Örneğin, Orta Kuzey ABD’de Sanal Makineler güncelleştirilirken Azure aynı anda Orta Güney ABD’deki bir Sanal Makineyi güncelleştirmez. Ancak, Kuzey Avrupa gibi diğer bölgelerde, Doğu ABD ile aynı zamanda bakım yapılabilir. Bölge çiftlerinin nasıl yararlanabileceğiniz understanding bölgeler arasında daha iyi Vm'lerinizi dağıtın. Daha fazla bilgi için [Azure bölge çiftleri](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

#### <a name="availability-sets-and-scale-sets"></a>Kullanılabilirlik kümeleri ve ölçek kümeleri

Bir iş yükü Azure sanal makinelerinde dağıtırken uygulamanız için yüksek kullanılabilirlik sağlamak üzere bir kullanılabilirlik kümesi içindeki sanal makineler oluşturabilirsiniz. Bu, kesinti veya rebootful bakım olayları sırasında en az bir sanal makinenin kullanılabilir olmasını sağlar.

Bir kullanılabilirlik kümesi içinde tek tek sanal makineleri, (UD) en fazla 20 güncelleştirme etki alanlarına dağıtılır. Planlanan bakım sırasında belirli bir zamanda yalnızca bir tek güncelleştirme etki alanı etkilenir. Etkilenmesine güncelleştirme etki alanlarının sırası, mutlaka sırayla gerçekleşmediğinden olduğunu unutmayın. 

Sanal makine ölçek kümeleri, özdeş Vm'lerden oluşan bir küme tek bir kaynak olarak dağıtıp yönetmenize olanak sağlayan bir Azure işlem kaynağıdır. Ölçek kümesi, bir kullanılabilirlik kümesindeki VM'ler gibi güncelleştirme etki alanları arasında otomatik olarak dağıtılır. Tıpkı kullanılabilirlik kümeleri ile ölçek kümeleri ile yalnızca bir tek güncelleştirme etki alanı planlanan bakım sırasında herhangi bir belirli zamanda etkilenmez.

Sanal makinelerinizi yüksek kullanılabilirlik için yapılandırma hakkında daha fazla bilgi için sanal makinelerinizin kullanılabilirliğini yönetme görmek için [Windows](../articles/virtual-machines/windows/manage-availability.md) veya [Linux](../articles/virtual-machines/linux/manage-availability.md).
