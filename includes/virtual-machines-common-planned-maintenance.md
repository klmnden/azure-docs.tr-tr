---
title: include dosyası
description: include dosyası
services: virtual-machines
author: shants123
ms.service: virtual-machines
ms.topic: include
ms.date: 4/30/2019
ms.author: shants
ms.custom: include file
ms.openlocfilehash: adf99b941a775f105d8c65da3ac6c11dc7257120
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65416378"
---
Azure platformu güvenilirlik, performans ve sanal makineler için konak altyapısının güvenliğini iyileştirmek için düzenli olarak güncelleştirir. Yazılım bileşenlerini barındırma ortamında donanım yetkisinin alınması için ağ iletişimi bileşenlerinin yükseltme düzeltme eki uygulama öğesinden bu güncelleştirmeleri aralığı. Bu güncelleştirmelerin çoğu barındırılan sanal makinelere herhangi bir etkisi vardır. Ancak, burada güncelleştirmeleri etkisi ve Azure güncelleştirmeleri için en az etkili ücretlerinin durumlar vardır:

- Rebootful olmayan güncelleştirme mümkünse VM konak güncelleştirildi veya canlı yaparken duraklatıldı zaten güncelleştirilmiş bir konağa geçişi.

- Bakım yeniden başlatma gerektirirse, ne zaman bunu planlı bakım, bir bildirim alın. Azure bakım kendiniz için en uygun zamanda başlayabileceğiniz bir zaman penceresi de sunar. Bakım gerçekleştirmek için Acil olmadığı sürece kendine kendine bakım zaman penceresi genellikle 30 gündür. Ayrıca, Azure Vm'leri platform planlı bakım için yeniden başlatılması gerektiğinde servis taleplerini azaltmak için teknolojileri yatırım yapıyor. 

Bu sayfada Azure bakım her iki türdeki nasıl gerçekleştireceğini açıklar. Planlanmamış olaylar (kesinti) hakkında daha fazla bilgi için için sanal makinelerin kullanılabilirliğini yönetme [Windows](../articles/virtual-machines/windows/manage-availability.md) veya [Linux](../articles/virtual-machines/linux/manage-availability.md).

Yaklaşan bakımlar hakkında bildirim içinde VM için zamanlanmış olaylar kullanarak alabileceğiniz [Windows](../articles/virtual-machines/windows/scheduled-events.md) veya [Linux](../articles/virtual-machines/linux/scheduled-events.md).

Planlı bakım yönetme "nasıl yapılır" için "İşleme planlı bakım bildirimlerini" için bilgi [Linux](../articles/virtual-machines/linux/maintenance-notifications.md) veya [Windows](../articles/virtual-machines/windows/maintenance-notifications.md).

## <a name="maintenance-not-requiring-a-reboot"></a>Bakım yeniden başlatma gerektiren değil

Yukarıda belirtildiği gibi çoğu platform güncelleştirmelerinin müşteri Vm'leri sıfır etki ile gerçekleştirilir. Sıfır etkisi güncelleştirme mümkün olduğunda, Azure müşteri Vm'leri için en az impactful güncelleştirme mekanizması seçer. Bu sıfır olmayan etkisi bakım çoğu sanal makine için 10 saniye duraklatma daha az neden olur. Bazı durumlarda, 30 saniyeye kadar sanal Makineyi duraklatır ve RAM belleği korur bellek koruyucu bakım mekanizması kullanılmaz. Ardından VM sürdürülür ve kendi saati otomatik olarak eşitlenir. Bakımı koruma bellek dışında G, M, N ve H serisi Azure VM'ler için birden fazla %90 çalışır. Azure giderek daha fazla dinamik geçiş teknolojilerini kullanarak ve duraklatma süresi azaltmak için bakım mekanizması koruma bellek artırma.  

Hata etki alanı tarafından uygulanan hata etki alanı bu rebootful olmayan bakım işlemleridir ve hiçbir uyarı sistem durumu sinyali alınırsa ilerleme durduruldu. 

Bazı uygulamalar, bu türden güncelleştirmeler etkilenebilir. VM'yi farklı bir ana bilgisayara geçirilmesine Canlı olması durumunda, önemli bazı iş yükleri için VM duraklatma sonlanan birkaç dakika içinde küçük bir performans düşüşü görebilirsiniz. Bu tür uygulamalar için zamanlanmış olaylar yararlanabileceğiyle [Windows](../articles/virtual-machines/windows/scheduled-events.md) veya [Linux](../articles/virtual-machines/linux/scheduled-events.md) VM bakım için hazırlanmanıza ve Azure bakım sırasında hiçbir etkiye sahip. Azure, ayrıca bakım denetim özellikleri gibi son derece hassas uygulamalar için çalışmaktadır. 

### <a name="live-migration"></a>Dinamik geçiş

Dinamik geçiş VM için bellek koruyan bir rebootful olmayan bir işlemdir ve sonuçları sınırlı bir duraklatma veya dondurma, genellikle en fazla 5 saniye sona erecek. Bugün, bir hizmet (Iaas) sanal makineler, G, M, N ve H serisi, ayrı olarak tüm altyapı Canlı geçiş için uygun. Bu % 90 Azure Fleet için dağıtılan Iaas Vm'leri karşılık gelir. 

Dinamik geçiş, aşağıdaki senaryolarda Azure Fabric tarafından başlatılır:
- Planlı Bakım
- Donanım arızası
- Ayırma en iyi duruma getirme

Dinamik geçiş bazı planlı bakım senaryolarda yararlanılarak ve zamanlanmış olaylar, bilmek kullanılabilir önceden, Canlı geçiş işlemlerini Başlat.

Dinamik geçiş, sanal makineleri ile Machine Learning algoritmalarınızı algılandığında bir tahmin edilen arızayı donanım dışına taşımak ve sanal makine ayırma iyileştirmek için de kullanılır. Bizim Tahmine dayalı algılayan düşürülmüş donanım örneklerini modelleme hakkında daha fazla bilgi için lütfen başlıklı blog gönderimizi bkz [Tahmine dayalı ML ve dinamik geçiş ile Azure sanal makine iyileştirme esnekliği](https://azure.microsoft.com/blog/improving-azure-virtual-machine-resiliency-with-predictive-ml-and-live-migration/?WT.mc_id=thomasmaurer-blog-thmaure). Her zaman müşterilere bir dinamik geçiş bildirimi İzleyicisi'nde, Azure portalında / hizmet durumu günlüğe kaydeder, zamanlanmış bu kullanılıyorsa olaylar gibi aracılığıyla iyi.

## <a name="maintenance-requiring-a-reboot"></a>Yeniden başlatma gerektiren bakım

Vm'leri planlı bakım için yeniden başlatılması gerektiğinde nadir durumlarda, önceden bildirilir. Planlı bakım için iki aşaması vardır: Self Servis ve zamanlanmış bakım penceresinde.

**Self Servis penceresi** Vm'lerinizde bakım başlatmanıza olanak tanır. Genellikle dört hafta bu süre boyunca, kendi durumunu görmek ve Bakım isteğiniz sonucunu denetlemek için her bir VM sorgulayabilirsiniz.

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
