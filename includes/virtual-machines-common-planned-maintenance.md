---
title: include dosyası
description: include dosyası
services: virtual-machines
author: shants123
ms.service: virtual-machines
ms.topic: include
ms.date: 12/14/2018
ms.author: shants
ms.custom: include file
ms.openlocfilehash: c26c037455b6d14a906894ec39bf46630826950b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60301716"
---
Azure platformu güvenilirlik, performans ve sanal makineler için konak altyapısının güvenliğini iyileştirmek için düzenli olarak güncelleştirir. Yazılım bileşenlerini barındırma ortamında donanım yetkisinin alınması için ağ iletişimi bileşenlerinin yükseltme düzeltme eki uygulama öğesinden bu güncelleştirmeleri aralığı. Bu güncelleştirmelerin çoğu barındırılan sanal makinelere herhangi bir etkisi vardır. Ancak, burada güncelleştirmeleri etkisi ve Azure güncelleştirmeleri için en az etkili ücretlerinin durumlar vardır:

- Rebootful olmayan güncelleştirme mümkünse VM konak güncelleştirildi veya canlı yaparken duraklatıldı zaten güncelleştirilmiş bir konağa geçişi.

- Bakım yeniden başlatma gerektirirse, ne zaman bunu planlı bakım, bir bildirim alın. Azure bakım kendiniz için en uygun zamanda başlayabileceğiniz bir zaman penceresi de sunar. Azure Vm'leri platform planlı bakım için yeniden başlatılması gerektiğinde servis taleplerini azaltmak için teknolojileri yatırım yapıyor. 

Bu sayfada Azure bakım her iki türdeki nasıl gerçekleştireceğini açıklar. Planlanmamış olaylar (kesinti) hakkında daha fazla bilgi için için sanal makinelerin kullanılabilirliğini yönetme [Windows](../articles/virtual-machines/windows/manage-availability.md) veya [Linux](../articles/virtual-machines/linux/manage-availability.md).

Yaklaşan bakımlar hakkında bildirim içinde VM için zamanlanmış olaylar kullanarak alabileceğiniz [Windows](../articles/virtual-machines/windows/scheduled-events.md) veya [Linux](../articles/virtual-machines/linux/scheduled-events.md).

Planlı bakım yönetme "nasıl yapılır" için "İşleme planlı bakım bildirimlerini" için bilgi [Linux](../articles/virtual-machines/linux/maintenance-notifications.md) veya [Windows](../articles/virtual-machines/windows/maintenance-notifications.md).

## <a name="maintenance-not-requiring-a-reboot"></a>Bakım yeniden başlatma gerektiren değil

Yeniden başlatma gerektirmez çoğu bakım için sanal makine için 10 saniyeden az duraklatma hedeftir. Bakımı koruma bazı durumlarda bellekte 30 saniyeye kadar sanal Makineyi duraklatır ve RAM belleği korur mekanizması kullanılmaz. Ardından sanal makine sürdürülür ve sanal makinenin saati otomatik olarak eşitlenir. Azure giderek daha fazla dinamik geçiş teknolojilerini kullanarak ve duraklatma süresi azaltmak için bakım mekanizması koruma bellek artırma.

Hata etki alanı tarafından uygulanan hata etki alanı bu rebootful olmayan bakım işlemleridir ve hiçbir uyarı sistem durumu sinyali alınırsa ilerleme durduruldu. 

Bazı uygulamalar, bu türden güncelleştirmeler etkilenebilir. VM'yi farklı bir ana bilgisayara geçirilmesine Canlı olması durumunda, önemli bazı iş yükleri için VM duraklatma sonlanan birkaç dakika içinde küçük bir performans düşüşü görebilirsiniz. Bu tür uygulamalar için zamanlanmış olaylar yararlanabileceğiyle [Windows](../articles/virtual-machines/windows/scheduled-events.md) veya [Linux](../articles/virtual-machines/linux/scheduled-events.md) VM bakım için hazırlanmanıza ve Azure bakım sırasında hiçbir etkiye sahip. Azure, ayrıca bakım denetim özellikleri gibi son derece hassas uygulamalar için çalışmaktadır. 


## <a name="maintenance-requiring-a-reboot"></a>Yeniden başlatma gerektiren bakım

Vm'leri planlı bakım için yeniden başlatılması gerektiğinde nadir durumlarda, önceden bildirilir. Planlı bakım için iki aşaması vardır: Self Servis ve zamanlanmış bakım penceresinde.

**Self Servis penceresi** Vm'lerinizde bakım başlatmanıza olanak tanır. Bu süre boyunca, kendi durumunu görmek ve Bakım isteğiniz sonucunu denetlemek için her bir VM sorgulayabilirsiniz.

Self Servis bakım başlattığınızda, sanal makinenizin zaten güncelleştirilmiş bir düğüme yeniden. VM yeniden geçici disk kaybolur ve sanal ağ arabirimiyle ilişkilendirilmiş dinamik IP adresleri güncelleştirildi.

Self Servis bakım başlatın ve işlemi sırasında bir hata ise, işlem durdurulur, VM güncelleştirilmez ve Self Servis bakım yeniden dene seçeneğine sahip olursunuz. 

Self Servis penceresi geçtiğinde **zamanlanan bakım penceresinden** başlar. Bu zaman penceresi sırasında bakım penceresi için hala sorgulayabilirsiniz ancak bakım kendiniz başlatın.

Görmek için yeniden başlatma gerektiren bakım yönetme hakkında daha fazla bilgi için "İşleme planlı bakım bildirimlerini" [Linux](../articles/virtual-machines/linux/maintenance-notifications.md) veya [Windows](../articles/virtual-machines/windows/maintenance-notifications.md). 

### <a name="availability-considerations-during-scheduled-maintenance"></a>Zamanlanan bakım sırasında kullanılabilirlik konuları 

Zamanlanmış bakım penceresini beklemek karar verirseniz, sanal makinelerinizin yüksek kullanılabilirliğini korumak için dikkate alınması gereken birkaç nokta vardır. 

#### <a name="paired-regions"></a>Eşleştirilmiş bölgeler

Her Azure bölgesi aynı coğrafyadaki başka bir bölgeyle eşleştirilir ve bölgesel çift birlikte yapmaları. Zamanlanan bakım aşamasında, Azure bölgesi çiftinin tek bir bölgedeki Vm'leri yalnızca güncelleştirir. Örneğin, Kuzey Orta ABD VM'yi güncelleştirilirken Azure Orta Güney ABD, herhangi bir VM aynı anda güncelleştirme olmaz. Ancak, Kuzey Avrupa gibi diğer bölgelerde, Doğu ABD ile aynı zamanda bakım yapılabilir. Bölge çiftlerinin nasıl yararlanabileceğiniz understanding bölgeler arasında daha iyi Vm'lerinizi dağıtın. Daha fazla bilgi için [Azure bölge çiftleri](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

#### <a name="availability-sets-and-scale-sets"></a>Kullanılabilirlik kümeleri ve ölçek kümeleri

Bir iş yükü Azure sanal makinelerinde dağıtırken uygulamanız için yüksek kullanılabilirlik sağlamak üzere bir kullanılabilirlik kümesi içindeki sanal makineler oluşturabilirsiniz. Bu, kesinti veya rebootful bakım olayları sırasında en az bir VM kullanılabilir olmasını sağlar.

Bir kullanılabilirlik kümesi içinde tek tek sanal makineleri, (UD) en fazla 20 güncelleştirme etki alanlarına dağıtılır. Planlanan bakım sırasında belirli bir zamanda yalnızca bir tek güncelleştirme etki alanı güncelleştirilir. Güncelleştirilen güncelleştirme etki alanlarının sırası, mutlaka sıralı olarak gerçekleşmez. 

Sanal makine ölçek kümeleri, özdeş Vm'lerden oluşan bir küme tek bir kaynak olarak dağıtıp yönetmenize olanak sağlayan bir Azure işlem kaynağıdır. Ölçek kümesi, bir kullanılabilirlik kümesindeki VM'ler gibi güncelleştirme etki alanları arasında otomatik olarak dağıtılır. Tıpkı kullanılabilirlik kümeleri ile ölçek kümeleri ile yalnızca bir tek güncelleştirme etki alanı planlanan bakım sırasında herhangi bir belirli zamanda güncelleştirilir.

Vm'lerinizi yüksek kullanılabilirlik için yapılandırma hakkında daha fazla bilgi için sanal makinelerinizin kullanılabilirliğini yönetme görmek için [Windows](../articles/virtual-machines/windows/manage-availability.md) veya [Linux](../articles/virtual-machines/linux/manage-availability.md).
