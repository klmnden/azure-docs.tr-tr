---
title: Azure, Avere vFXT sisteminizi - planı
description: Azure için Avere vFXT dağıtmadan önce yapmak planlama açıklanır
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: v-erkell
ms.openlocfilehash: 46978d19a0789bb43e861ca89661aa5b78eb4ec7
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59271076"
---
# <a name="plan-your-avere-vfxt-system"></a>Avere vFXT sisteminizi planlama

Bu makalede yeni bir Avere vFXT konumlandırılır ve ihtiyaçlarınıza uygun şekilde boyutlandırıldığından Azure küme planı açıklanmaktadır. 

Azure Marketi'ne gidip veya herhangi bir VM oluşturmak önce kümenin diğer öğeleri Azure ile nasıl etkileşime gireceğini göz önünde bulundurun. Burada küme kaynaklarını özel ağ ve alt ağ içinde yer alır ve arka uç depolama alanınızı nerede olacağına karar vermeniz planlayın. Oluşturduğunuz küme düğümleri, iş akışını desteklemek üzere yeteri kadar güçlü olduğundan emin olun. 

Daha fazla bilgi için okumaya devam edin.

## <a name="resource-group-and-network-infrastructure"></a>Kaynak grubu ve ağ altyapısı

Azure dağıtım için Avere vFXT öğelerini olduğu göz önünde bulundurun. Aşağıdaki diyagramda Azure bileşenleri için Avere vFXT için olası bir düzenleme gösterilmektedir:

![Küme içindeki bir alt ağ denetleyicisi ve küme Vm'leri gösteren diyagram. Alt ağ bir sanal ağ sınırı sınırıdır. Depolama hizmet uç noktasını temsil eden bir Altıgene vnet'inizde olur; sanal ağ dışındaki bir Blob depolama alanına kesikli bir ok ile bağlandı.](media/avere-vfxt-components-option.png)

Avere vFXT sistemin ağ altyapısı planlama yaparken aşağıdaki yönergeleri izleyin:

* Tüm öğeleri Avere vFXT dağıtımı için oluşturulan yeni bir abonelik ile yönetilmelidir. Faydaları şunlardır: 
  * Daha basit maliyet izleme - görüntüleme ve tüm işlem kaynakları ve altyapı maliyetlerinden döngüleri bir abonelikte denetim.
  * Daha kolay temizleme - projeyle bitirdikten sonra tüm abonelik kaldırabilirsiniz.
  * Kaynak uygun bölümleme kotalar - Avere vFXT istemciler ve küme tek bir abonelikte yalıtarak azaltma olası kaynaktan diğer kritik iş yüklerini koruma. Çok sayıda istemcileri için yüksek performanslı bilgi işlem iş akışı'kurmak dönüştürürken bu çakışmayı önler.

* İstemci işlem sistemlerinizi vFXT küme yakın bulun. Arka uç depolama daha uzak olabilir.  

* VFXT küme ve küme denetleyicisi VM'SİNİN aynı sanal ağda (vnet), aynı kaynak grubunda yer alması ve aynı depolama hesabını kullanırsınız. Otomatik küme oluşturma şablonu için çoğu durumda bu işler.

* Küme IP adresi çakışmaları istemcilerle veya işlem kaynakları için kendi alt ağda bulunması gerekir. 

* Küme oluşturma şablonu kaynak grupları, sanal ağlar, alt ağlar ve depolama hesapları dahil olmak üzere küme için gerekli altyapı kaynakların çoğunu oluşturabilirsiniz. Zaten mevcut olan kaynakları kullanmak istiyorsanız, bunlar bu tablodaki gereksinimleri karşıladığından emin olun. 

  | Kaynak | Var olanı kullan? | Gereksinimler |
  |----------|-----------|----------|
  | Kaynak grubu | Evet, boş ise | Boş olmalıdır| 
  | Depolama hesabı | Küme oluşturulduktan sonra Blob kapsayıcı var olan bir bağlama varsa Evet <br/>  Küme oluşturma sırasında yeni bir Blob kapsayıcısı oluşturma, Hayır | Var olan Blob kapsayıcı boş olmalıdır <br/> &nbsp; |
  | Sanal ağ | Evet | Depolama hizmet uç noktası, yeni bir Azure Blob kapsayıcısı oluşturuyorsanız içermelidir. | 
  | Alt ağ | Evet |   |

## <a name="ip-address-requirements"></a>IP adresi gereksinimleri 

Kümenizin alt kümesini desteklemek için yeterince büyük bir IP adresi aralığı olduğundan emin olun. 

Avere vFXT küme, aşağıdaki IP adreslerini kullanır:

* Bir küme yönetim IP adresi. Bu adres, düğümden düğüme kümede geçebilirsiniz ancak Avere Denetim Masası'nı Yapılandırma Aracı'na bağlanabilmesi için her zaman kullanılabilir.
* Her küme düğümü için:
  * En az bir istemciye yönelik IP adresi. (Tüm istemci dönük adresleri küme tarafından yönetilen *vserver*, hangi taşıyabilirsiniz bunları düğümler arasında gerektiği gibi.)
  * Küme iletişimi için bir IP adresi
  * Bir örnek IP adresi (VM'ye atanmış)

Azure Blob Depolama hizmetini kullanıyorsanız, kümenin sanal ağ IP adreslerinden da gerekebilir:  

* Bir Azure Blob Depolama hesabı, en az beş adet IP adresi gerektirir. Blob Depolama kümeniz aynı sanal ağda bulmak, bu gereksinimin göz önünde bulundurun.
* Kümenizin bir sanal ağın dışında olan Azure Blob Depolama hizmetini kullanıyorsanız, sanal ağ içinde depolama hizmet uç noktası oluşturmanız gerekir. Bu uç nokta, bir IP adresi kullanmaz.

Ağ kaynaklarını bulun ve Blob Depolama alanı (kullanılıyorsa) farklı kaynak gruplarında kümeden seçeneği var.

## <a name="vfxt-node-size"></a>vFXT düğüm boyutu

Küme düğümleri önbelleğinizin istek aktarım hızı ve depolama kapasitesini belirlemek hizmet VM'ler. <!-- The instance type offered has been chosen for its memory, processor, and local storage characteristics. You can choose from two instance types, with different memory, processor, and local storage characteristics. -->

Her vFXT düğüm aynı olacaktır. Diğer bir deyişle, üç düğümlü bir küme oluşturursanız, aynı türde ve size üç VM gerekir. 

| Örnek türü | vCPU sayısı | Bellek  | Yerel SSD depolama  | Maksimum veri diskleri | Önbelleğe alınmamış disk aktarım hızı | NIC (sayı) |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_E32s_v3 | 32  | 256 GiB | 512 GiB  | 32 | 51.200 IOPS <br/> 768 MB/sn | 16.000 MB/sn (8)  |

Düğüm başına disk önbellek yapılandırılabilir ve 1000 GB ile 8000 GB rage. Düğüm başına 4 TB Standard_E32s_v3 düğümler için önerilen önbellek boyutudur.

Bu sanal makineler hakkında ek bilgi için Microsoft Azure belgeleri okuyun:

* [Bellek için iyileştirilmiş sanal makine boyutları](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory)

## <a name="account-quota"></a>Hesap kotası

Aboneliğinizi Avere vFXT küme yanı sıra kullanılan bilgisayar ya da istemci sistemlerini çalıştırma kapasitesine sahip olduğundan emin olun. Okuma [vFXT küme için kota](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster) Ayrıntılar için.

## <a name="back-end-data-storage"></a>Arka uç veri depolama

Burada önbellekte olmadığında Avere vFXT küme verilerinizi mı saklamalıyım? Çalışma kümenizin saklı uzun vadeli yeni bir Blob kapsayıcısında veya var olan bir bulut ya da donanım depolama sistemi olup olmayacağını karar verin. 

Azure Blob Depolama arka ucu için kullanmak istiyorsanız, vFXT küme oluşturma işleminin parçası olarak yeni bir kapsayıcı oluşturmanız gerekir. Bu seçenek, oluşturur ve yeni bir kapsayıcı küme hazır olduktan hemen sonra kullanıma hazır şekilde yapılandırır. 

Okuma [Avere vFXT oluşturmak için Azure](avere-vfxt-deploy.md#create-the-avere-vfxt-for-azure) Ayrıntılar için.

> [!NOTE]
> Yalnızca boş Blob Depolama kapsayıcıları çekirdek filtrelerin Avere vFXT sistemi için kullanılabilir. VFXT mevcut verilerinizi korumak gerek kalmadan kendi nesne deposu yönetebilmek gerekir. 
>
> Okuma [vFXT kümeye veri taşıma](avere-vfxt-data-ingest.md) istemci makineleri ve Avere vFXT önbellek kullanarak verimli bir şekilde verileri kümenin yeni kapsayıcıya kopyalama hakkında bilgi edinmek için.

Mevcut bir şirket içi depolama sistemi kullanmak istiyorsanız, oluşturulduktan sonra onu vFXT kümeye eklemeniz gerekir. Okuma [depolamayı yapılandırma](avere-vfxt-add-storage.md) mevcut bir depolama sistemi Avere vFXT kümeye ekleme hakkında ayrıntılı yönergeler için.

## <a name="cluster-access"></a>Kümeye erişim 

Avere vFXT Azure küme için özel bir alt ağda bulunan ve kümenin genel bir IP adresi yok. Some yöntemi özel alt küme yönetimi ve istemci bağlantıları için erişimi olması gerekir. 

Erişim seçenekleri şunlardır:

* Konak bağlantı - özel ağdaki ayrı bir VM'ye bir genel IP adresi atamak ve küme düğümlerine SSL tüneli oluşturmak için bunu kullanın. 

  > [!TIP]
  > Genel bir IP adresi küme denetleyicisinde ayarlarsanız, atlama konağı olarak kullanabilirsiniz. Okuma [konak atlama gibi küme denetleyicisi](#cluster-controller-as-jump-host) daha fazla bilgi için.

* Sanal özel ağ (VPN) - özel ağınızın bir noktadan siteye veya siteden siteye VPN'yi yapılandırın.

* Azure ExpressRoute - bir ExpressRoute iş ortağı aracılığıyla özel bir bağlantı yapılandırın. 

Bu seçenekler hakkında daha fazla ayrıntı için okuma [internet iletişimi hakkında Azure sanal ağ belgeleri](../virtual-network/virtual-networks-overview.md#communicate-with-the-internet).

### <a name="cluster-controller-as-jump-host"></a>Konak atlama gibi küme denetleyicisi

Genel bir IP adresi küme denetleyicisinde ayarlarsanız, bu hızlı konak olarak Avere vFXT kümeden özel alt ağın dışında iletişim için kullanabilirsiniz. Ancak, denetleyici küme düğümleri değiştirmek için erişim ayrıcalıklarına sahip olduğundan, bu küçük bir güvenlik riski oluşturur.  

Dağıtım betiği, bir genel IP adresi ile bir denetleyici için güvenliği artırmak için yalnızca bağlantı 22 için gelen erişimi kısıtlayan ağ güvenlik grubu otomatik olarak oluşturur. Sistem kilitleyerek daha iyi koruyabilirsiniz, IP kaynak adresi aralığı - erişim tuşunu diğer bir deyişle, yalnızca gelen bağlantılara makineler kümeye erişim için kullanmayı düşündüğünüz izin verin.

Kümeyi oluştururken küme denetleyicisinde bir genel IP adresi oluşturma verilip verilmeyeceğini seçebilirsiniz. 

* Yeni bir vnet ya da yeni bir alt ağ oluşturmak, küme denetleyicisi bir genel IP adresi atanır.
* Bir mevcut bir vnet ve alt ağı seçerseniz, küme denetleyicisi özel IP adreslerine sahip olacaktır. 

## <a name="vm-access-roles"></a>VM erişim rolleri 

Azure kullanan [rol tabanlı erişim denetimi](../role-based-access-control/index.yml) küme Vm'leri bazı görevleri gerçekleştirmek üzere yetkilendirmek için (RBAC). Örneğin, küme denetleyicisi oluşturmak ve küme düğümü sanal makineleri yapılandırmak için yetkilendirilmesi gerekiyor. Küme düğümleri atama veya diğer küme düğümleri için IP adresi yeniden atama gerekir.

Azure iki yerleşik role Avere vFXT sanal makineler için kullanılır: 

* Yerleşik rol kümesi denetleyicisi kullanan [Avere katkıda bulunan](../role-based-access-control/built-in-roles.md#avere-contributor). 
* Küme düğümleri kullanan yerleşik rolü [Avere işleci](../role-based-access-control/built-in-roles.md#avere-operator)

Erişim rolleri Avere vFXT bileşenleri için özelleştirmek gerekiyorsa, kendi Rol tanımlamak ve ardından oluşturuldukları sırada Vm'lere atar. Dağıtım şablonu Azure Market'te kullanamazsınız. Microsoft Müşteri Hizmetleri ve desteği açıklandığı gibi Azure portalında bir bileti açarak başvurun [sisteminizle Yardım Al](avere-vfxt-open-ticket.md). 

## <a name="next-step-understand-the-deployment-process"></a>Sonraki adım: Dağıtım sürecini anlama

[Dağıtıma genel bakış](avere-vfxt-deploy-overview.md) büyük resmi tüm Azure sistem için bir Avere vFXT oluşturmak ve verileri sunmak hazırlanma için gerekli olan adımları sağlar.  