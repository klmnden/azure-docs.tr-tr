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
ms.openlocfilehash: f403e060859df6d1de96a3c0d478d57df2677eee
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "31531064"
---
Sabitlenmelidir ayırma isteği neden ortak ayırma senaryolar aşağıda verilmiştir. Her senaryo bu makalenin sonraki bölümlerinde içine dalın.

- Bir VM'yi yeniden boyutlandırın veya sanal makineleri veya rol örnekleri olan bir bulut hizmetini ekleme
- Kısmen durduruldu (serbest bırakıldığında) sanal makineleri yeniden başlatın
- Tam olarak durduruldu (serbest bırakıldığında) sanal makineleri yeniden başlatın
- Hazırlama ve üretim dağıtımları (yalnızca bir hizmet olarak platform)
- Benzeşim grubu (VM veya hizmet yakınlık)
- Benzeşim – grup tabanlı sanal ağ

Ayırma hatası aldığınızda, listelenen senaryoların herhangi biri, hata için geçerli olup olmadığını denetleyin. Karşılık gelen senaryo tanımlamak için Azure platformu tarafından döndürülen ayırma hatası kullanın. İsteğiniz sabitlenmiş, daha fazla kümeler, böylece ayırma Başarı şansı artırma isteğinizi açmak için sabitleme kısıtlamaları bazılarını kaldırın.
Genel olarak, hata "istenen VM boyutu desteklenmediğini" durum, daha sonra her zaman yeniden deneyebilirsiniz. Bu durum, yeterli kaynak isteğiniz uyum sağlayacak şekilde kümede serbest bırakılmış olabilir çünkü. İstenen VM boyutu desteklenmiyor sorunsa, farklı bir VM boyutu deneyin. Aksi durumda, tek seçenek sabitleme kısıtlaması kaldırmaktır.

İki ortak hatası senaryoları için benzeşim grupları ilişkilidir. Geçmişte, VM'ler ve hizmet örneği olarak yakınında sağlamak için kullanılan bir benzeşim grubu veya bir sanal ağ oluşturulmasını sağlamak üzere kullanıldı. Bölgesel sanal ağlar başlanmasıyla, benzeşim grupları artık bir sanal ağ oluşturmak için gerekli değildir. Azure altyapı içindeki ağ gecikme süresi azaltma ile sanal makineleri veya hizmet yakınlık benzeşim grupları kullanmak üzere öneri değişti.

Aşağıdaki diyagramda (sabitlenmiş) ayırma senaryoları sınıflandırma gösterir. 

![Sabitlenmiş ayırma sınıflandırma](./media/virtual-machines-common-allocation-failure/Allocation3.png)

## <a name="resize-a-vm-or-add-vms-or-role-instances-to-an-existing-cloud-service"></a>Bir VM'yi yeniden boyutlandırın veya sanal makineleri veya rol örnekleri olan bir bulut hizmetini ekleme
**Hata**

Upgrade_VMSizeNotSupported veya GeneralError

**Küme sabitleme nedeni**

Mevcut bulut hizmetini barındıran özgün kümesine denenmesi bir VM'yi yeniden boyutlandırın veya bir VM veya rol örneği var olan bir bulut hizmetine eklemek için bir istek aldı. Yeni bir bulut hizmeti oluşturulması, istenen VM boyutu destekler veya kaynakları serbest bırakmak sahip başka bir küme bulmak Azure platformu sağlar.

**Geçici çözüm**

Hata Upgrade_VMSizeNotSupported * varsa, farklı bir VM boyutu deneyin. Farklı bir VM boyutu kullanarak bir seçenek değilse, ancak farklı bir sanal IP adresi (VIP) kullanmak için kabul edilebilir ise, yeni VM barındırmak ve var olan VM'ler çalıştırdığı bölgesel sanal ağ için yeni bulut hizmeti eklemek için yeni bir bulut hizmeti oluşturun. Mevcut bulut hizmetiniz bölgesel bir sanal ağ kullanmıyorsa, hala yeni bulut hizmeti için yeni bir sanal ağ oluşturduğunuzda ve ardından bağlanmak, [yeni bir sanal ağ mevcut sanal ağa](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Daha fazla gördükleri hakkında [bölgesel sanal ağlar](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

Hata GeneralError * varsa, (örneğin, belirli bir VM boyutu) kaynak türü küme tarafından desteklenir, ancak küme şu anda kaynakları serbest bırakmak yok olasıdır. Yukarıdaki senaryosu benzer yeni bir bulut hizmeti (yeni bulut hizmeti farklı bir VIP kullanmak olduğunu unutmayın) oluşturma aracılığıyla istenen işlem kaynak ekleyin ve bulut hizmetlerinizi bağlanmak için bir bölgesel sanal ağ kullanın.

## <a name="restart-partially-stopped-deallocated-vms"></a>Kısmen durduruldu (serbest bırakıldığında) sanal makineleri yeniden başlatın
**Hata**

GeneralError *

**Küme sabitleme nedeni**

Kısmi ayırmayı kaldırma (serbest bırakıldığında) bir veya daha fazlasını ancak değil tüm VM'lerin bir bulut hizmetinde durduruldu anlamına gelir. Ne zaman durdurup (deallocate) VM, bir ilişkili kaynakları serbest bırakılır. Bu durduruldu (serbest bırakıldığında) VM'yi yeniden başlatırken bu nedenle yeni ayırma isteğidir. VM'ler kısmen deallocated bulut hizmetinde yeniden başlatma var olan bir bulut hizmetini VM'ler ekleme ile eşdeğerdir. Mevcut bulut hizmetini barındıran özgün kümesine denenmesi ayırma isteğini sahiptir. Farklı bir bulut hizmeti oluşturulması, istenen VM boyutu destekler veya ücretsiz kaynağa sahip başka bir küme bulmak Azure platformu sağlar.

**Geçici çözüm**

Farklı bir VIP kullanın, durduruldu (serbest bırakıldığında) sanal makineleri silin (ancak ilişkili diskler tutmak için) kabul edilebilir ve eklerseniz farklı bir bulut hizmeti sanal makineleri yedekleyin. Bulut hizmetlerinizi bağlanmak için bir bölgesel sanal ağ kullanın:

* Mevcut bulut hizmetiniz bölgesel bir sanal ağ kullanıyorsa, yeni bulut hizmeti aynı sanal ağa eklemeniz yeterlidir.
* Mevcut bulut hizmetiniz bölgesel bir sanal ağ kullanmıyorsa, yeni bulut hizmeti için yeni bir sanal ağ oluşturun ve ardından [yeni bir sanal ağa varolan sanal ağınıza bağlamak](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Daha fazla gördükleri hakkında [bölgesel sanal ağlar](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

## <a name="restart-fully-stopped-deallocated-vms"></a>Tam olarak durduruldu (serbest bırakıldığında) sanal makineleri yeniden başlatın
**Hata**

GeneralError *

**Küme sabitleme nedeni**

Durduruldu tam ayırmayı kaldırma anlamına gelir (tüm sanal makineler bir bulut hizmetinden serbest). Bu sanal makineleri yeniden başlatmayı ayırma isteklerini bulut hizmetini barındıran özgün kümesine denenmesi gerekir. Yeni bir bulut hizmeti oluşturulması, istenen VM boyutu destekler veya kaynakları serbest bırakmak sahip başka bir küme bulmak Azure platformu sağlar.

**Geçici çözüm**

Farklı bir VIP kullanın, özgün durduruldu (serbest bırakıldığında) sanal makineleri silin (ancak ilişkili diskler tutmak için) kabul edilebilir ise ve karşılık gelen bulut hizmetini silin ((serbest bırakıldığında) durduğunda ilişkili işlem kaynaklarını zaten yayımlanan VM'ler). Sanal makineleri geri eklemek için yeni bir bulut hizmeti oluşturun.

## <a name="stagingproduction-deployments-platform-as-a-service-only"></a>Hazırlama/üretim dağıtımları (yalnızca bir hizmet olarak platform)
**Hata**

New_General * veya New_VMSizeNotSupported *

**Küme sabitleme nedeni**

Hazırlama dağıtımı ve bir bulut hizmeti Üretim dağıtımı aynı küme içinde barındırılır. İkinci dağıtımı eklediğinizde, ilk dağıtım barındıran aynı küme içinde karşılık gelen ayırma isteğini denenir.

**Geçici çözüm**

İlk dağıtımı silin ve özgün bulut hizmeti ve bulut hizmeti yeniden dağıtın. Bu eylem, her iki dağıtım sığması için ücretsiz yeterli kaynaklara sahip bir küme veya istediğiniz VM boyutları destekleyen bir küme ilk dağıtım güden.

## <a name="affinity-group-vmservice-proximity"></a>Benzeşim grubu (VM/hizmet yakınlık)
**Hata**

New_General * veya New_VMSizeNotSupported *

**Küme sabitleme nedeni**

Herhangi bir benzeşim grubuna atanan kaynak bir kümeye bağlanır işlem. Benzeşim grubu çalıştı varolan kaynakları barındırıldığı kümede, yeni işlem kaynağı ister. Yeni kaynaklar var olan bir bulut hizmetini veya yeni bir bulut hizmeti aracılığıyla oluşturulan bu geçerlidir.

**Geçici çözüm**

Bir benzeşim grubu gerekli değilse, olmayan bir benzeşim grubu kullanın veya işlem kaynaklarınızı birden çok benzeşim gruplar halinde gruplandırabilirsiniz.

## <a name="affinity-group-based-virtual-network"></a>Benzeşim grubu tabanlı sanal ağ
**Hata**

New_General * veya New_VMSizeNotSupported *

**Küme sabitleme nedeni**

Bölgesel sanal ağlar sunulmadan önce bir sanal ağ bir benzeşim grubu ile ilişkilendirmek için gerekli olmuştur. Sonuç olarak, bir benzeşim grubu yerleştirilen kaynakları açıklandığı gibi aynı kısıtlamalar tarafından bağlı işlem "ayırma senaryo: benzeşim grubu (VM/hizmet yakınlık)" Yukarıdaki bölümde. İşlem kaynaklarını kümeye bağlıdır.

**Geçici çözüm**

Bir benzeşim grubu gerekmiyorsa ekleyeceğiniz, yeni kaynaklar için yeni bir bölgesel sanal ağ oluşturun ve ardından [yeni bir sanal ağa varolan sanal ağınıza bağlamak](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Daha fazla gördükleri hakkında [bölgesel sanal ağlar](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

Alternatif olarak, [benzeşim grubu tabanlı sanal ağınızı bölgesel bir sanal ağa geçirmeniz](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/)ve ardından istenen kaynakları yeniden ekleyin.
