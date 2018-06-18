---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 03/09/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: e484dac645ff2e5867d2e652c389a9950e8bac12
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "30196449"
---
Azure güvenilirliği, performansı ve sanal makineler için konak altyapısının güvenliğini artırmak için güncelleştirmeleri düzenli olarak gerçekleştirir. Bu güncelleştirmeleri aralığı (örneğin, işletim sistemi, hiper yönetici ve konakta dağıtılan çeşitli aracılar), bir barındırma ortamında yazılım bileşenleri düzeltme eki uygulama gelen donanım yetkisini için ağ bileşenleri yükseltiliyor. Bu güncelleştirmeler çoğunluğu barındırılan sanal makineler için herhangi bir etkisi olmadan gerçekleştirilir. Ancak, güncelleştirmeler bir etkiye sahip olduğu durumlar vardır:

- Bir yeniden başlatma daha az güncelleştirmesi olası ise, Azure VM konak güncelleştirilmiş ya da VM zaten güncelleştirilmiş bir ana bilgisayara tamamen taşınır duraklatmak için bakım koruma bellek kullanır.

- Bakım bir yeniden başlatma gerektirirse, ne zaman bunu planlı bakım, bir bildirim alın. Bu durumlarda, ayrıca bakım kendiniz, uygun bir zaman başlayabileceğiniz bir zaman penceresi verilir.

Bu sayfa Microsoft Azure bakım her iki tür nasıl gerçekleştireceğini açıklar. Planlanmamış olaylar (kesintileri) hakkında daha fazla bilgi için sanal makinelerin kullanılabilirliğini yönetme [Windows] bakın (../articles/virtual-machines/windows/manage-availability.md) veya [Linux](../articles/virtual-machines/linux/manage-availability.md).

Bir sanal makinede çalışan uygulamalar için Azure meta veri hizmeti kullanarak gelecek güncelleştirmeleri hakkında bilgi toplayabilir [Windows](../articles/virtual-machines/windows/instance-metadata-service.md) veya [Linux] (../articles/virtual-machines/linux/instance-metadata-service.md).

Planlı bakım yönetme "nasıl yapılır" için "İşleme planlı bakım bildirimleri" için bilgi [Linux](../articles/virtual-machines/linux/maintenance-notifications.md) veya [Windows](../articles/virtual-machines/windows/maintenance-notifications.md).

## <a name="memory-preserving-maintenance"></a>Bakım koruma bellek

Güncelleştirmeleri tam bir yeniden başlatma gerektirmez, bellek koruma bakım mekanizmaları sanal makine etkisini sınırlamak için kullanılır. Sanal makine en fazla 30 barındırma ortamı gerekli güncelleştirmeleri ve düzeltme eklerini geçerlidir ya da VM zaten güncelleştirilmiş bir ana bilgisayara taşır RAM bellek koruma saniye duraklatıldı. Sanal makine sonra sürdürülür ve sanal makine saati otomatik olarak eşitlenir. 

Kullanılabilirlik kümelerinde VM'ler için güncelleştirilmiş birer birer güncelleştirme etki alanlarıdır. Bir güncelleştirme etki alanında (UD) tüm sanal makineleri duraklatıldı, güncelleştirilmiş ve planlı bakım için sonraki UD taşıyan önce sonra sürdürüldü.

Bazı uygulamalar, bu tür güncelleştirmeler tarafından etkilenebilir. Gerçek zamanlı Olay işleme, akış veya kod veya yüksek verimlilik senaryoları, ağ gibi gerçekleştiren uygulamaları 30 saniyelik duraklamalar tolerans için tasarlanmamış olabilir. <!-- sooooo, what should they do? --> VM taşımak için farklı bir konak olması durumunda, bazı önemli iş yükleri için sanal makine duraklatma baştaki birkaç dakika içinde küçük bir performans düşüşü görebilirsiniz. 


## <a name="maintenance-requiring-a-reboot"></a>Yeniden başlatma gerektiren bakım

Sanal makineleri planlı bakım için yeniden başlatılması gerektiğinde, önceden bildirilir. Planlı bakım iki aşaması vardır: Self Servis ve zamanlanmış bakım penceresinde.

**Self Servis penceresi** bakım, sanal makineleri üzerinde başlatmak olanak tanır. Bu süre boyunca bakım isteğiniz sonucunu denetleyin ve bunların durumunu görmek için her bir VM sorgulayabilirsiniz.

Self Servis bakım başlattığınızda, VM zaten güncelleştirildi ve geri çalıştırır bir düğüme taşınır. VM yeniden çünkü geçici disk kaybolur ve sanal ağ arabirimiyle ilişkilendirilmiş dinamik IP adreslerini güncelleştirilir.

Self Servis bakım başlatın ve işlemi sırasında bir hata ise, işlem durduruldu, VM güncelleştirilmez ve planlı bakım yinelemeden diğerine de kaldırılır. Daha sonra yeni bir zamanlama ile temas ve olması Self Servis bakım yapmak için yeni bir fırsat sunulan. 

Self Servis penceresi geçtiğinde **zamanlanmış bakım penceresi** başlar. Bu zaman penceresi sırasında hala sorgu için bakım penceresi, ancak artık bakım kendiniz başlatılamaz.

Görmek için yeniden başlatma gerektiren bakım yönetme hakkında daha fazla bilgi için "İşleme planlı bakım bildirimleri" [Linux](../articles/virtual-machines/linux/maintenance-notifications.md) veya [Windows](../articles/virtual-machines/windows/maintenance-notifications.md). 

## <a name="availability-considerations-during-planned-maintenance"></a>Planlı bakım sırasında kullanılabilirlik konuları 

Planlı Bakım penceresini beklemek karar verirseniz, Vm'leriniz için en yüksek kullanılabilirliği sürdürmek için dikkate alınması gereken birkaç nokta vardır. 

### <a name="paired-regions"></a>Eşleştirilmiş bölgeleri

Her Azure bölgesi başka bir bölge içinde aynı coğrafi konum ile eşleştirilmiş, bölgesel çifti birlikte yaptıkları. Planlı bakım sırasında Azure yalnızca tek bir bölgedeki bir bölge çiftinin VM'ler güncelleştirir. Örneğin, Orta Kuzey ABD’de Sanal Makineler güncelleştirilirken Azure aynı anda Orta Güney ABD’deki bir Sanal Makineyi güncelleştirmez. Ancak, Kuzey Avrupa gibi diğer bölgelerde, Doğu ABD ile aynı zamanda bakım yapılabilir. Bölge çiftleri nasıl yardımcı olabilecek anlama bölgeler arasında daha iyi Vm'leriniz dağıtın. Daha fazla bilgi için bkz: [Azure bölgesi çiftleri](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

### <a name="availability-sets-and-scale-sets"></a>Kullanılabilirlik kümeleri ve ölçek kümeleri

Bir iş yükü Azure vm'lerinde dağıtırken, uygulamanız için yüksek kullanılabilirlik sağlamak üzere bir kullanılabilirlik kümesi içindeki sanal makineleri oluşturabilirsiniz. Bu bir kesinti veya bakım olayları sırasında en az bir sanal makinenin kullanılabilir olmasını sağlar.

Bir kullanılabilirlik kümesinde en fazla 20 güncelleştirme etki alanları arasında (UDs) tek tek sanal makineleri yayılır. Planlı bakım sırasında herhangi bir anda yalnızca bir tek güncelleştirme etki alanı etkilenmez. Güncelleme etki alanına etkilenip sırasını mutlaka sıralı olarak gerçekleşmez olduğunu unutmayın. 

Sanal makine ölçek kümeleri dağıtmak ve aynı sanal makineleri bir küme tek bir kaynak olarak yönetmenize olanak sağlayan bir Azure işlem kaynaktır. Ölçek kümesi, bir kullanılabilirlik kümesindeki sanal makineleri gibi güncelleştirme etki alanları arasında otomatik olarak dağıtılır. Gibi kullanılabilirlik kümeleri ile ölçek kümeleri ile yalnızca bir tek güncelleştirme etki alanı herhangi bir zamanda etkilenmez.

Sanal makinelerinizin yüksek kullanılabilirlik için yapılandırma hakkında daha fazla bilgi için bkz, sanal makinelerin kullanılabilirliğini için [Windows](../articles/virtual-machines/windows/manage-availability.md) veya [Linux](../articles/virtual-machines/linux/manage-availability.md).
