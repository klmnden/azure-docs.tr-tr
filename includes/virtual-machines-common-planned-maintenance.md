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
ms.openlocfilehash: c2931fa410cf92755a5df5b7129dcf93de900930
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66155923"
---
Azure platformu güvenilirlik, performans ve sanal makineler için konak altyapısının güvenliğini iyileştirmek için düzenli olarak güncelleştirir. Bu güncelleştirmeler amacı, yazılım bileşenlerini yükseltme bileşenleri ağ veya donanım yetkisini alma barındırma ortamında düzeltme eki uygulama aralıkları. 

Güncelleştirmeleri nadiren barındırılan sanal makineleri etkiler. Güncelleştirmeleri bir etkisi yoktur, Azure güncelleştirmeleri için en az etkili yöntem seçer:

- Güncelleştirmenin yeniden başlatma gerektirmez, VM konak güncelleştirildiğinde ya da Canlı-geçişi zaten güncelleştirilmiş bir konağa sanal makine duraklatıldı.

- Bakım yeniden başlatma gerektirirse planlı bakım bildirim alırsınız. Azure Ayrıca, bir zaman penceresi içinde bakım kendiniz için en uygun zamanda başlatabilirsiniz sağlar. Bakım Acil olmadığı sürece kendine kendine bakım penceresi genellikle 30 gündür. Azure platformu planlı bakım VM'lerin yeniden başlatılmasını gerektiren bir servis talebi sayısını azaltmak için teknolojileri yatırım yapıyor. 

Bu sayfada Azure bakım her iki türdeki nasıl gerçekleştireceğini açıklar. Planlanmamış olaylar (kesinti) hakkında daha fazla bilgi için bkz. [Vm'leri için Windows kullanılabilirliğini yönetme](../articles/virtual-machines/windows/manage-availability.md) veya ilgili makaleyi [Linux](../articles/virtual-machines/linux/manage-availability.md).

Bir VM içinde tarafından yaklaşan bakımlar hakkında bildirim almak [Windows için zamanlanmış olaylar kullanarak](../articles/virtual-machines/windows/scheduled-events.md) veya [Linux](../articles/virtual-machines/linux/scheduled-events.md).

Planlı bakım yönetme ile ilgili yönergeler için bkz: [planlı bakım bildirimlerini Linux için işleme](../articles/virtual-machines/linux/maintenance-notifications.md) veya ilgili makaleyi [Windows](../articles/virtual-machines/windows/maintenance-notifications.md).

## <a name="maintenance-that-doesnt-require-a-reboot"></a>Yeniden başlatma gerektirmez bakım

Daha önce belirtildiği gibi birçok platform güncelleştirmelerinin müşteri Vm'leri etkilemez. Etkisiz güncelleştirme mümkün olmadığında, Azure müşteri Vm'leri için en az impactful güncelleştirme mekanizması seçer. 

Çoğu etkisi sıfır bakım 10 saniyeden kısa bir süre için sanal Makineyi duraklatır. Bazı durumlarda, Azure, bellek tasarruflu bakım mekanizmalarını kullanır. Bu mekanizmalar, 30 saniyeye kadar sanal Makineyi duraklatmak ve RAM belleği koruyabilirsiniz. Ardından VM sürdürülür ve kendi saati otomatik olarak eşitlenir. 

Bellek tasarruflu bakım fazla yüzde 90'dan için Azure sanal makineleri çalışır. Bu, G, M, N ve H serisi için çalışmaz. Azure, giderek daha fazla dinamik geçiş teknolojilerini kullanır ve duraklama sürelerini azaltmak için bakım mekanizmaları bellek tasarruflu artırır.  

Yeniden başlatma gerektirmeyen bu bakım uygulanan bir hata etki alanı aynı anda işlemlerdir. Herhangi bir uyarı sistem durumu sinyal aldıkları durumunda durdurulur. 

Bu türden güncelleştirmeler, bazı uygulamaları etkileyebilir. VM'yi farklı bir konağa dinamik-geçişi olduğunda, önemli bazı iş yükleri için VM duraklatma sonlanan birkaç dakika içinde küçük bir performans düşüşü gösterebilir. Sanal makine bakım için hazırlanmanıza ve Azure bakım sırasında etkisini azaltmak için deneyin [Windows için zamanlanmış olaylar kullanarak](../articles/virtual-machines/windows/scheduled-events.md) veya [Linux](../articles/virtual-machines/linux/scheduled-events.md) gibi uygulamalar için. Azure, bakım denetim özellikleri hassas uygulamalar için çalışmaktadır. 

### <a name="live-migration"></a>Dinamik geçiş

Dinamik geçiş, yeniden başlatma gerektirmez ve sanal makine için bellek koruyan bir işlemdir. Duraklatma veya dondurma, genellikle en fazla 5 saniye çözülebilecek neden olur. G dışında M, N ve H serisi, bir hizmet (Iaas) Vm'leri olarak tüm altyapı, dinamik geçiş için uygundur. Uygun sanal makineleri için Azure fleet dağıtılan Iaas Vm'leri yüzde 90'den fazla temsil eder. 

Azure platformu, aşağıdaki senaryolarda dinamik geçiş başlar:
- Planlı bakım
- Donanım arızası
- Ayırma en iyi duruma getirme

Bazı planlı bakım senaryolarını dinamik geçiş kullanan ve zamanlanmış olaylar önceden Canlı öğrenmek için kullanabileceğiniz geçiş işlemlerini başlayacak.

Dinamik geçiş, Azure Machine Learning algoritmaları yaklaşan bir donanım hatası veya VM ayırma iyileştirmek istediğinizde tahmin, sanal makineleri taşımak için de kullanılabilir. Algılayan düşürülmüş donanım örneklerini Tahmine dayalı modelleme hakkında daha fazla bilgi için bkz: [Tahmine dayalı makine öğrenimi ve dinamik geçiş ile geliştirme Azure VM dayanıklılığını](https://azure.microsoft.com/blog/improving-azure-virtual-machine-resiliency-with-predictive-ml-and-live-migration/?WT.mc_id=thomasmaurer-blog-thmaure). Bu hizmetler kullanırsanız, dinamik geçiş bildirimleri İzleyici ve hizmetin sistem durumunu günlükleri de zamanlanmış olaylar olduğu gibi Azure portalında görünür.

## <a name="maintenance-that-requires-a-reboot"></a>Yeniden başlatma gerektiren bir bakım

VM'ler için planlı bakım başlatılması gereken yere nadir durumlarda, önceden bildirilir. Planlı bakım için iki aşaması vardır: Self Servis aşaması ve zamanlanan bakım aşaması.

Sırasında *Self Servis aşaması*, genellikle sürer dört haftada, Vm'lerinizde Bakımı Başlat. Self Servis bir parçası olarak, her VM durumunu ve Bakım isteğiniz sonucunu görmek için sorgulayabilirsiniz.

Self Servis bakım başlattığınızda, sanal makinenizin zaten güncelleştirilmiş bir düğüme yeniden. VM yeniden geçici disk kaybolur ve dinamik IP adresleri sanal ağ arabirimiyle ilişkili güncelleştirilir.

Self Servis bakım sırasında işlemi durdurur, bir hata oluşursa VM güncelleştirilmez ve Self Servis bakım yeniden dene seçeneğine sahip olursunuz. 

Self Servis aşaması sona erdiğinde *zamanlanan bakım aşaması* başlar. Bu aşamasında, yine de bakım aşaması için sorgu yapabilirsiniz, ancak kendiniz bakım başlatılamıyor.

Yeniden başlatma gerektiren bir bakım yönetme ile ilgili daha fazla bilgi için bkz: [planlı bakım bildirimlerini Linux için işleme](../articles/virtual-machines/linux/maintenance-notifications.md) veya ilgili makaleyi [Windows](../articles/virtual-machines/windows/maintenance-notifications.md). 

### <a name="availability-considerations-during-scheduled-maintenance"></a>Zamanlanan bakım sırasında kullanılabilirlik konuları 

Zamanlanan bakım aşaması kadar beklenecek karar verirseniz, sanal makinelerinizin yüksek kullanılabilirliği sürdürmek için dikkate almanız gereken birkaç nokta vardır. 

#### <a name="paired-regions"></a>Eşleştirilmiş bölgeler

Her Azure bölgesi aynı coğrafi yakın çevre içinde başka bir bölgeyle eşleştirilir. Bu arada, bölge çiftine olun. Zamanlanan bakım aşaması sırasında Azure Vm'leri yalnızca tek bir bölgede bir bölge çiftine güncelleştirir. Örneğin, Orta Kuzey ABD VM'yi güncelleştirilirken Azure Orta Güney ABD'deki tüm VM'ler aynı anda güncelleştirmez. Ancak, Kuzey Avrupa gibi diğer bölgelerde, Doğu ABD ile aynı zamanda bakım yapılabilir. Bölge çiftlerinin nasıl yararlanabileceğiniz understanding bölgeler arasında daha iyi Vm'lerinizi dağıtın. Daha fazla bilgi için [Azure bölge çiftleri](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

#### <a name="availability-sets-and-scale-sets"></a>Kullanılabilirlik kümeleri ve ölçek kümeleri

Bir iş yükü Azure sanal makinelerinde dağıtırken, VM içinde oluşturabileceğiniz bir *kullanılabilirlik kümesi* uygulamanıza yüksek kullanılabilirlik sağlamak için. Kullanılabilirlik kümeleri kullanarak, en az bir VM yeniden başlatma gerektiren bir kesinti veya bakım olayları sırasında kullanılabilir olduğundan emin olabilirsiniz.

Bir kullanılabilirlik kümesi içinde tek tek sanal makineleri, (UD) en fazla 20 güncelleştirme etki alanlarına dağıtılır. Planlanan bakım sırasında belirli bir zamanda yalnızca bir UD güncelleştirilir. UD mutlaka sıralı olarak güncelleştirilmez. 

Sanal makine *ölçek kümeleri* olan bir Azure işlem, özdeş Vm'lerden oluşan bir kümesini tek bir kaynak olarak dağıtıp yönetmek için kullanabileceğiniz kaynak. Ölçek kümesi, bir kullanılabilirlik kümesindeki VM'ler gibi Ud'ler arasında otomatik olarak dağıtılır. Ölçek kümeleri kullanırken olarak kullanılabilirlik kümeleri ile yalnızca bir UD planlanan bakım sırasında herhangi bir belirli zamanda güncelleştirilir.

Vm'lerinizi yüksek kullanılabilirlik için ayarlama hakkında daha fazla bilgi için bkz. [, Vm'leri için Windows kullanılabilirliğini yönetme](../articles/virtual-machines/windows/manage-availability.md) veya ilgili makaleyi [Linux](../articles/virtual-machines/linux/manage-availability.md).
