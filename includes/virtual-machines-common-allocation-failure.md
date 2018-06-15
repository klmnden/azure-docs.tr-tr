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
ms.openlocfilehash: 24d89b617c347bc9443b437c92cb034acb3e05cb
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
ms.locfileid: "33901338"
---
Bir sanal makine (VM) oluşturmak, durduruldu (serbest bırakıldığında) sanal makineleri yeniden başlatın veya bir VM'yi yeniden boyutlandırın, Microsoft Azure aboneliğinize işlem kaynakları ayırır. Biz sürekli olarak ek altyapı ve her zaman müşteri taleplerini desteklemek kullanılabilir tüm VM türler sahibiz emin olmak için özellikler yatırım yapıyor. Ancak, belirli bölgelerdeki Azure Hizmetleri için isteğe bağlı eşi görülmemiş büyüme nedeniyle bazen kaynak ayırma hatalarıyla karşılaşabilirsiniz. Oluşturun veya sanal makineleri şu hata kodu ve iletiyi görüntülemek sırasında sanal makineleri bir bölgede başlatmak çalıştığınızda bu sorun oluşabilir:

**Hata kodu**: AllocationFailed veya ZonalAllocationFailed

**Hata iletisi**: "ayırma başarısız oldu. Biz yeterli kapasitesi istenen VM boyutu bu bölgede gerekmez. Ayırma başarı olasılığını artırma hakkında daha fazla okuma http://aka.ms/allocation-guidance"

Bu makalede, bazı ortak ayırma hatalarının nedenlerini açıklar ve olası çözümler önerir.

Bu makalede Azure sorunu ele alınmamışsa ziyaret [MSDN ve yığın taşması Azure forumları](https://azure.microsoft.com/support/forums/). Bu forumları veya çok sorununuzu nakledebilirsiniz @AzureSupport Twitter'da. Ayrıca, size bir Azure destek isteği üzerinde Get destek seçerek dosya [Azure Destek](https://azure.microsoft.com/support/options/) site.

Tercih edilen VM türünüz tercih edilen Bölgenizde kullanılabilir oluncaya kadar biz aşağıdaki tabloda yer alan yönergeleri geçici bir çözüm olarak dikkate alınması gereken dağıtım sorunlarla müşteriler öneriyoruz. 

Durumunuza en iyi şekilde eşleşen senaryo tanımlamak ve ayırma başarı olasılığını artırmak için karşılık gelen önerilen geçici çözüm kullanılarak ayırma isteği yeniden deneyin. Alternatif olarak, her zaman daha sonra yeniden deneyebilirsiniz. Yeterli kaynaklar, küme, bölge veya isteğiniz uyum sağlayacak şekilde bölge boşaltılmış olmasıdır. 


## <a name="resize-a-vm-or-add-vms-to-an-existing-availability-set"></a>Bir VM'yi yeniden boyutlandırın veya VM'ler var olan bir kullanılabilirlik kümesine ekleme

### <a name="cause"></a>Nedeni

Bir VM'yi yeniden boyutlandırın veya mevcut bir kullanılabilirlik kümesi için bir VM varolan kullanılabilirlik barındıran özgün kümesine denenecek eklemek için bir istek ayarlayın. İstenen VM boyutu küme tarafından desteklenir, ancak küme şu anda yeterli kapasiteye sahip olmayabilir. 

### <a name="workaround"></a>Geçici çözüm

VM farklı bir kullanılabilirlik kümesinin bir parçası olabilir, farklı bir kullanılabilirlik (aynı bölgede) kümesindeki bir VM oluşturun. Bu yeni VM sonra aynı sanal ağa eklenebilir.

Durdur (deallocate) tüm sanal makineleri aynı kullanılabilirlik ayarlayın, sonra her birini yeniden başlatın.
Durdurmak için: tıklatın Kaynak grupları > [kaynak grubunuz] > Kaynakları > [kullanılabilirlik kümesi] > sanal makineleri > [sanal makinenize] > durdurun.
Tüm sanal makineleri durdurduktan sonra ilk VM seçin ve ardından Başlat'ı tıklatın.
Bu adım, yeni bir ayırma girişimi çalıştırılır ve yeni bir küme yeterli kapasiteye sahip seçilebilir emin emin olur.

## <a name="restart-partially-stopped-deallocated-vms"></a>Kısmen durduruldu (serbest bırakıldığında) sanal makineleri yeniden başlatın

### <a name="cause"></a>Nedeni

Kısmi ayırmayı kaldırma (serbest bırakıldığında) bir veya daha fazla durduruldu ancak tümü, sanal makineleri bir kullanılabilirlik kümesi anlamına gelir. Bir VM serbest bırakma, ilişkili kaynakları serbest bırakılır. Kısmen deallocated kullanılabilirlik kümesindeki sanal makineleri yeniden başlatmayı VM'ler var olan bir kullanılabilirlik kümesine ekleme aynıdır. Bu nedenle, ayırma isteğini özgün kümesine varolan kullanılabilirlik kümesi konakları yeterli kapasiteye sahip olmayabilir çalıştı gerekir.

### <a name="workaround"></a>Geçici çözüm

Durdur (deallocate) tüm sanal makineleri aynı kullanılabilirlik ayarlayın, sonra her birini yeniden başlatın.
Durdurmak için: tıklatın Kaynak grupları > [kaynak grubunuz] > Kaynakları > [kullanılabilirlik kümesi] > sanal makineleri > [sanal makinenize] > durdurun.
Tüm sanal makineleri durdurduktan sonra ilk VM seçin ve ardından Başlat'ı tıklatın.
Bu yeni bir ayırma girişimi çalıştırılır ve yeni bir küme yeterli kapasitesine sahip seçilebilir olduğundan emin olmanızı sağlar.

## <a name="restart-fully-stopped-deallocated-vms"></a>Tam olarak durduruldu (serbest bırakıldığında) sanal makineleri yeniden başlatın

### <a name="cause"></a>Nedeni

Durduruldu tam ayırmayı kaldırma anlamına gelir (tüm sanal makineleri bir kullanılabilirlik kümesinde serbest). Bu sanal makineleri yeniden başlatmayı ayırma isteği bölge ve bölge içinde istenen boyuta destekleyen tüm kümeleri hedefleyecektir. Bu makaledeki öneriler başına ayırma isteğinizi değiştirin ve ayırma başarı olasılığını artırmak için isteği yeniden deneyin. 

### <a name="workaround"></a>Geçici çözüm

Eski VM dizisi ya da Dv1, DSv1, Av1, D15v2 veya DS15v2, gibi boyutları kullanırsanız, daha yeni sürümleri için taşımayı düşünün. Bu önerileri belirli VM boyutları için bkz.
Farklı bir VM boyutu kullanma seçeneğiniz yoksa, aynı coğrafi içinde farklı bir bölgeye dağıtmayı deneyin. Her bir bölge içinde kullanılabilir VM boyutları hakkında daha fazla bilgi için https://aka.ms/azure-regions

Kullanılabilirlik bölgeleri kullanıyorsanız, istenen VM boyutu için kullanılabilir kapasiteye sahip olmayabilir ve bölge içindeki başka bir bölgeye deneyin.

Ayırma isteği büyükse (500'den fazla çekirdek), daha küçük dağıtımlar içine isteği bölmeniz aşağıdaki bölümlerde yer alan yönergeleri bakın.

## <a name="allocation-failures-for-older-vm-sizes-av1-dv1-dsv1-d15v2-ds15v2-etc"></a>Eski VM boyutları (Av1, Dv1, DSv1, D15v2, DS15v2, vb.) için ayırma hataları

Biz Azure altyapı genişletin gibi biz son sanal makine türlerini desteklemek üzere tasarlanmış yeni nesil donanım dağıtın. Bazı eski serisi VM'ler bizim son nesil altyapısı üzerinde çalıştırmayın. Bu nedenle, müşteriler bu eski SKU'ları için ayırma hatalarını bazen karşılaşabilirsiniz. Bu sorunu önlemek için aşağıdaki önerileri başına eşdeğer yeni sanal makineleri taşımak dikkate alınması gereken eski serisi sanal makineler kullanan müşteriler şu önerilir: Bu VM'ler için en son donanım en iyi duruma getirilir ve daha iyi yararlanmak sağlar Fiyatlandırma ve performans. 

|Eski VM-serisi/boyutu|Önerilen yeni VM-serisi/boyutu|Daha fazla bilgi|
|----------------------|----------------------------|--------------------|
|Av1-serisi|[Av2-serisi](../articles/virtual-machines/windows/sizes-general.md#av2-series)|https://azure.microsoft.com/blog/new-av2-series-vm-sizes/
|Dv1 veya DSv1 seri (D1 D5 için)|[Dv3 veya DSv3-serisi](../articles/virtual-machines/windows/sizes-general.md#dsv3-series-sup1sup)|https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/
|Dv1 veya DSv1 seri (D11 D14 için)|[Ev3 veya ESv3-serisi](../articles/virtual-machines/windows/sizes-memory.md#ev3-series)|
|D15v2 veya DS15v2|Daha büyük VM boyutları yararlanmak için theResource Manager dağıtım modeli kullanarak, D16v3/DS16v3 veya D32v3/DS32v3 taşımayı düşünün. Bu son nesil donanımda çalışmak üzere tasarlanmıştır. VM örneği için tek bir müşteriye ayrılmış donanım için ayrılmış olduğundan emin olmak için Resource Manager dağıtım modeli kullanıyorsanız en son nesil donanımda çalıştırmak için tasarlanmış yeni yalıtılmış VM boyutları, E64i_v3 veya E64is_v3, taşımayı düşünün. |https://azure.microsoft.com/blog/new-isolated-vm-sizes-now-available/

## <a name="allocation-failures-for-large-deployments-more-than-500-cores"></a>Ayırma hatalarının büyük dağıtımlar (500'den fazla çekirdek)

İstenen VM boyutu örneklerinin sayısını azaltın ve dağıtım işlemi yeniden deneyin. Ayrıca, büyük ölçekli dağıtımlarda, değerlendirmek istediğiniz [Azure sanal makine ölçek kümeleri](https://docs.microsoft.com/azure/virtual-machine-scale-sets/). VM örneği sayısını otomatik olarak artırabilir veya azaltabilirsiniz isteğe bağlı veya tanımlı bir zamanlamayı yanıt olarak ve dağıtımlar arasında birden fazla küme yayılabilir çünkü ayırma başarı büyük şansına sahip olabilirsiniz. 

## <a name="background-information"></a>Arka plan bilgileri
### <a name="how-allocation-works"></a>Ayırma nasıl çalışır?
Azure veri merkezlerindeki sunucular kümelere bölünmüştür. Normalde, birden fazla kümede bir ayırma isteğinde bulunulur, ancak ayırma isteğindeki belirli kısıtlamalar, Azure platformunu yalnızca bir kümede istekte bulunmaya zorlar. Bu makalede, biz buna "bir kümeye sabitlenmiş gibi." başvuruyor Aşağıda 1 diyagram birden çok kümelerde denenir normal bir ayırma durumunu gösterir. Diyagram 2 bir ayırma durumunun gösterir, varolan bir bulut hizmeti CS_1 veya kullanılabilirlik kümesini barındırıldığı olduğu için küme 2'ye sabitlenmiş.
![Ayırma diyagramı](./media/virtual-machines-common-allocation-failure/Allocation1.png)

### <a name="why-allocation-failures-happen"></a>Ayırma hatalarının neden olması
Ayırma isteği bir kümeye sabitlenmiş, kullanılabilir kaynak havuzu daha küçük olduğundan boş kaynakları bulmak başarısız olan, daha yüksek bir fırsat yok. Küme kaynakları serbest bırakmak olsa bile Ayrıca, bir kümeye ayırma isteği sabitlenmiş ancak, istenen kaynak türü, küme tarafından desteklenmiyor, isteğiniz başarısız olur. Aşağıdaki diyagram 3 yalnızca adayı küme kaynakları serbest bırakmak olmadığından burada Sabitlenmiş ayırma başarısız durumu gösterir. Diyagram 4 yalnızca adayı küme istenen VM boyutu desteklemediği için küme kaynakları serbest bırakmak sahip olsa bile burada Sabitlenmiş ayırma başarısız durumda gösterir.

![Sabitlenmiş ayırma hatası](./media/virtual-machines-common-allocation-failure/Allocation2.png)


