---
title: include dosyası
description: include dosyası
services: virtual-machines-windows, azure-resource-manager
author: genlin
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 04/14/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: 8afaeaea3c8ceb0de33147e70e00f3f57c857ca7
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58883165"
---
Sanal makine (VM) oluşturma, durduruldu (serbest bırakıldı) Vm'leri yeniden başlatma veya VM'yi yeniden boyutlandırma, Microsoft Azure aboneliğinize işlem kaynakları ayırır. Biz sürekli olarak ek altyapı ve her zaman müşteri talebini desteklemek mevcut tüm VM türleri sahibiz emin olmak için Özellikler'de yatırım yapıyor. Ancak, belirli bölgelerde Azure Hizmetleri için talepte eşi görülmemiş büyüme nedeniyle bazen kaynak ayırma hatalarıyla karşılaşabilirsiniz. Bu sorun, oluşturma veya aşağıdaki hata kodu ve şu iletiyle Vm'leri görüntüleme sırada bir bölgede VM'lerin başlatma çalıştığınızda oluşabilir:

**Hata kodu**: AllocationFailed veya ZonalAllocationFailed

**Hata iletisi**: "Ayırma başarısız oldu. Bu bölgede biz istenen VM boyutu için yeterli kapasite yoktur. Başarılı ayırma olasılığını artırma hakkında daha fazla okuma https://aka.ms/allocation-guidance"

Bu makalede, ortak bir ayırma hatalarının bazı nedenleri açıklanır ve olası çözümler önerir.

Bu makalede Azure sorunu ele alınmamışsa ziyaret [MSDN ve Stack Overflow Azure forumları](https://azure.microsoft.com/support/forums/). Bu Forum veya çok sorun gönderebilir @AzureSupport Twitter'da. Ayrıca, Azure destek isteği Get destek seçerek dosya [Azure Destek](https://azure.microsoft.com/support/options/) site.

Dağıtım sorunlarla aşağıdaki tabloda kılavuz geçici bir çözüm olarak dikkate alınması gereken müşteriler, tercih edilen sanal makine türünüzü tercih edilen Bölgenizde kullanılabilir oluncaya kadar biz önerin. 

Talebinize en iyi şekilde eşleşen bir senaryo belirleme ve sonra başarılı ayırma olasılığını artırmak için karşılık gelen önerilen geçici çözüm kullanılarak ayırma isteği yeniden deneyin. Alternatif olarak, her zaman daha sonra yeniden deneyebilir. Yeterli kaynaklar, küme, bölge veya isteğiniz uyum sağlamak için bölge boşaltılmış olmasıdır. 


## <a name="resize-a-vm-or-add-vms-to-an-existing-availability-set"></a>Bir veya birden çok VM'yi mevcut kullanılabilirlik kümesine yeniden boyutlandırma

### <a name="cause"></a>Nedeni

VM'yi yeniden boyutlandırma veya mevcut bir kullanılabilirlik kümesi için bir VM konumundaki mevcut kullanılabilirlik barındıran özgün küme denenecek eklemek için bir istek ayarlayın. İstenen VM boyutu, küme tarafından desteklenir, ancak küme şu anda yeterli kapasiteye sahip olmayabilir. 

### <a name="workaround"></a>Geçici çözüm

VM'yi farklı bir kullanılabilirlik kümesinin parçası olabilir (aynı bölgede) kümesi farklı bir kullanılabilirlik bir VM oluşturun. Bu yeni VM, sonra aynı sanal ağa eklenebilir.

Durdurun (serbest bırakın) tüm VM'lerin aynı kullanılabilirlik kümesi ve ardından her biri yeniden başlatın.
Durdurmak için: Kaynak Gruplar > [kaynak grubunuzun] > kaynak > [kullanılabilirlik kümesi] > sanal makineler > [sanal makinenizi] > Durdur.
Tüm VM'lerin durdurduktan sonra ilk VM seçin ve ardından Başlat'a tıklayın.
Bu adım, yeni bir ayırma girişimi çalıştırılır ve yeni bir küme, yeterli kapasiteye sahip seçilebileceğini emin olur.

## <a name="restart-partially-stopped-deallocated-vms"></a>Kısmen durdurulmuş (serbest bırakılmış) VM'leri yeniden başlatma

### <a name="cause"></a>Nedeni

Kısmi ayırmayı kaldırma (serbest bırakıldı) bir veya daha fazla durduruldu, ancak tüm, Vm'leri bir kullanılabilirlik kümesi anlamına gelir. Bir VM'yi serbest bırakın, ilişkili kaynakları serbest bırakılır. Kısmen serbest kullanılabilirlik kümesindeki Vm'leri yeniden başlatma var olan bir kullanılabilirlik kümesine Vm'leri ekleme aynıdır. Bu nedenle, ayırma isteğinin özgün kümesine mevcut bir kullanılabilirlik kümesi konakları yeterli kapasiteye sahip olmayabilir çalıştı gerekir.

### <a name="workaround"></a>Geçici çözüm

Durdurun (serbest bırakın) tüm VM'lerin aynı kullanılabilirlik kümesi ve ardından her biri yeniden başlatın.
Durdurmak için: Kaynak Gruplar > [kaynak grubunuzun] > kaynak > [kullanılabilirlik kümesi] > sanal makineler > [sanal makinenizi] > Durdur.
Tüm VM'lerin durdurduktan sonra ilk VM seçin ve ardından Başlat'a tıklayın.
Bu yeni bir ayırma girişimi çalıştırılır ve yeni bir küme, yeterli kapasiteye sahip seçilebileceğini emin olmanızı sağlar.

## <a name="restart-fully-stopped-deallocated-vms"></a>Tamamen durdurulmuş (serbest bırakılmış) VM'leri yeniden başlatma

### <a name="cause"></a>Nedeni

Durduruldu tam ayırmayı kaldırma anlamına gelir (bir kullanılabilirlik kümesindeki tüm sanal makineler serbest bırakıldı). Bu Vm'leri yeniden başlatma ayırma isteği bölgeyi veya bölgenin içinde istenen boyut destekleyen tüm kümeleri hedefleyecektir. Bu makaledeki öneriler başına ayırma isteğiniz değiştirin ve başarılı ayırma olasılığını artırmak için isteği yeniden deneyin. 

### <a name="workaround"></a>Geçici çözüm

Eski VM serisi veya boyutları, Dv1, DSv1, Av1, D15v2 veya DS15v2, gibi kullanırsanız yeni sürümlere taşıma göz önünde bulundurun. Bu önerileri belirli VM boyutları için bkz.
Başka bir VM boyutu kullanma seçeneğiniz yoksa, aynı coğrafyadaki başka bir bölgeye dağıtmayı deneyin. Daha fazla bilgi için her bir bölgede kullanılabilen VM boyutları https://aka.ms/azure-regions

Kullanılabilirlik alanları kullanıyorsanız, istenen VM boyutu için mevcut kapasiteyi olabilecek bölgede başka bir bölgeye deneyin.

Ayırma isteğiniz büyükse (500'den fazla çekirdek), kılavuz aşağıdaki bölümlerde daha küçük dağıtımlar isteğe bölmeniz bakın.

## <a name="allocation-failures-for-older-vm-sizes-av1-dv1-dsv1-d15v2-ds15v2-etc"></a>Eski sanal makine boyutları (Av1, Dv1, DSv1, D15v2, DS15v2, vb.) için ayırma hataları

Biz, size Azure altyapı genişlettiğinizde, en son sanal makine türlerini desteklemek için tasarlanan yeni nesil donanımdan dağıtın. Bazı eski serisi VM'ler son nesil altyapımız üzerinde çalıştırmayın. Bu nedenle, müşterilerin bazen bu eski Sku'larda ayırma hatalarıyla karşılaşabilirsiniz. Bu sorunu önlemek için aşağıdaki önerileri başına eşdeğer yeni vm'lere taşıma dikkate alınması gereken eski serisi sanal makineler kullanan müşteriler öneririz: Bu VM'ler, daha iyi fiyat ve performans avantajlarından yararlanmanıza olanak tanıyacak ve en son donanım için iyileştirilmiştir. 

|Eski VM serisi/boyutu|Önerilen yeni VM serisi/boyut|Daha fazla bilgi|
|----------------------|----------------------------|--------------------|
|Av1 serisi|[Av2 serisi](../articles/virtual-machines/windows/sizes-general.md#av2-series)|https://azure.microsoft.com/blog/new-av2-series-vm-sizes/
|Dv1 veya DSv1 serisi (D1 D5 için)|[Dv3 veya Dsv3 serisi](../articles/virtual-machines/windows/sizes-general.md#dsv3-series-1)|https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/
|Dv1 veya DSv1 serisi (D11-D14)|[Ev3 veya Esv3 serisi](../articles/virtual-machines/windows/sizes-memory.md#esv3-series)|
|D15v2 veya DS15v2|Büyük VM boyutları yararlanabilmek theResource Manager dağıtım modelini kullanıyorsanız E16v3/E16sv3 veya E32v3/E32sv3 taşımayı düşünün. Bu, en yeni nesil donanımlarda çalıştırmak için tasarlanmıştır. Sanal makine Örneğinize tek bir müşteriye özel donanımla yalıtılmıştır emin olmak için Resource Manager dağıtım modelini kullanıyorsanız, en yeni nesil donanımlarda çalıştırmak için tasarlanmış yeni yalıtılmış VM boyutları, E64i_v3 veya E64is_v3, geçmeyi göz önünde bulundurabilirsiniz. |https://azure.microsoft.com/blog/new-isolated-vm-sizes-now-available/


## <a name="allocation-failures-for-large-deployments-more-than-500-cores"></a>Ayırma hatalarının büyük dağıtımlarda (500'den fazla çekirdek)

İstenen VM boyutu örnek sayısını azaltın ve ardından dağıtım işlemi yeniden deneyin. Ayrıca, daha büyük dağıtımlar için değerlendirmek istediğiniz [Azure sanal makine ölçek kümeleri](https://docs.microsoft.com/azure/virtual-machine-scale-sets/). Sanal makine örneği sayısını otomatik olarak artırabilir veya azaltabilirsiniz içinde tanımlanmış bir zamanlamaya veya talebe yanıt olarak ve dağıtımlar arasında birden fazla küme yayılıyor olabilir çünkü büyük bir başarılı ayırma olasılığını sahipsiniz. 

## <a name="background-information"></a>Arka plan bilgileri
### <a name="how-allocation-works"></a>Ayırma nasıl çalışır?
Azure veri merkezlerindeki sunucular kümelere bölünmüştür. Normalde, birden fazla kümede bir ayırma isteğinde bulunulur, ancak ayırma isteğindeki belirli kısıtlamalar, Azure platformunu yalnızca bir kümede istekte bulunmaya zorlar. Bu makalede, bu "bir kümeye sabitlenebilir."olarak diyoruz Aşağıdaki diyagramda 1 birden fazla kümede denenir normal bir ayırma durumunu gösterir. Diyagram 2 ayırma durumunu gösterir, varolan bir bulut hizmeti CS_1 veya kullanılabilirlik kümesini barındırıldığı olduğu için kümeye 2'ye sabitlenmiş.
![Ayırma diyagramı](./media/virtual-machines-common-allocation-failure/Allocation1.png)

### <a name="why-allocation-failures-happen"></a>Ayırma hataları neden olması
Ayırma isteği için bir küme sabitlenmiş olduğunda daha yüksek kullanılabilir bir kaynak havuzu daha küçük olduğundan ücretsiz kaynakları bulmak başarısız olan kaybedilebilir. Küme kaynakları serbest bırakmak olsa bile, ayrıca, bir kümeye, ayırma isteğinin sabitlenmiş ancak istediğiniz kaynak türünü, küme tarafından desteklenmiyor, isteğiniz başarısız olur. Aşağıdaki diyagram 3'te yalnızca aday küme kaynakları serbest bırakmak olmadığından burada sabitlenmiş bir ayırma başarısız durum gösterilir. Diyagram 4 yalnızca aday küme istenen VM boyutu desteklemediği için küme ücretsiz kaynaklara sahip olsa da burada bir Sabitlenmiş ayırma başarısız durumda gösterir.

![Sabitlenmiş ayırma hatası](./media/virtual-machines-common-allocation-failure/Allocation2.png)


