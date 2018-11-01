---
title: Azure, Avere vFXT sisteminizi - planı
description: Azure için Avere vFXT dağıtmadan önce yapmak planlama açıklanır
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: f0e5523565dc561ed457dbc340835ad1889cb876
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50634482"
---
# <a name="plan-your-avere-vfxt-system"></a>Avere vFXT sisteminizi planlama

Bu makalede, Azure kümesine oluşturduğunuz küme konumlandırılır ve ihtiyaçlarınıza uygun şekilde boyutlandırıldığından emin olmak için yeni bir Avere vFXT planlama açıklanmaktadır. 

Azure Marketi'ne gidip veya herhangi bir VM oluşturmak önce kümenin diğer öğeleri Azure ile nasıl etkileşime gireceğini göz önünde bulundurun. Burada küme kaynaklarını özel ağ ve alt ağ içinde yer alır ve arka uç depolama alanınızı nerede olacağına karar vermeniz planlayın. Oluşturduğunuz küme düğümleri, iş akışını desteklemek üzere yeteri kadar güçlü olduğundan emin olun. 

Daha fazla bilgi için okumaya devam edin.

## <a name="resource-group-and-network-infrastructure"></a>Kaynak grubu ve ağ altyapısı

Azure dağıtım için Avere vFXT öğelerini olduğu göz önünde bulundurun. Aşağıdaki diyagramda Azure bileşenleri için Avere vFXT için olası bir düzenleme gösterilmektedir:

![Küme içindeki bir alt ağ denetleyicisi ve küme Vm'leri gösteren diyagram. Alt ağ bir sanal ağ sınırı sınırıdır. Depolama hizmet uç noktasını temsil eden bir Altıgene vnet'inizde olur; sanal ağ dışındaki bir Blob depolama alanına kesikli bir ok ile bağlandı.](media/avere-vfxt-components-option.png)

Avere vFXT sistemin ağ altyapısı planlama yaparken aşağıdaki yönergeleri izleyin:

* Tüm öğeleri Avere vFXT dağıtımı için oluşturulan yeni bir abonelik ile yönetilmelidir. Bu strateji, maliyet izlemesi ve temizleme basitleştirir ve bölüm kaynak kotaları da yardımcı olur. Avere vFXT çok sayıda istemci ile kullanıldığından, istemciler ve küme tek bir abonelikte yalıtma istemci sağlama sırasında azaltma olası kaynaktan diğer kritik iş yüklerini korur.

* İstemci işlem sistemlerinizi vFXT küme yakın bulun. Arka uç depolama daha uzak olabilir.  

* Kolaylık olması için aynı sanal ağdaki (vnet) ve aynı kaynak grubunda vFXT küme ve küme denetleyicisi VM'SİNİN bulun. Ayrıca aynı depolama hesabını kullanmanız gerekir. 

* Küme IP adresi çakışmaları istemcilerle veya işlem kaynakları için kendi alt ağda bulunması gerekir. 

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

## <a name="vfxt-node-sizes"></a>vFXT düğümü boyutları 

Küme düğümleri önbelleğinizin istek aktarım hızı ve depolama kapasitesini belirlemek hizmet VM'ler. Farklı bellek, işlemci ve yerel depolama özelliklerini içeren iki örnek türleri arasından seçim yapabilirsiniz. 

Her vFXT düğüm aynı olacaktır. Diğer bir deyişle, üç düğümlü bir küme oluşturursanız, aynı türde ve size üç VM gerekir. 

| Örnek türü | vCPU sayısı | Bellek  | Yerel SSD depolama  | Maksimum veri diskleri | Önbelleğe alınmamış disk aktarım hızı | NIC (sayı) |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_D16s_v3 | 16  | 64 giB  | 128 GiB  | 32 | 25.600 IOPS <br/> 384 MB/sn | 8000 MB/sn (8) |
| Standard_E32s_v3 | 32  | 256 giB | 512 GiB  | 32 | 51.200 IOPS <br/> 768 MB/sn | 16.000 MB/sn (8)  |

Düğüm başına disk önbellek yapılandırılabilir ve 1000 GB ile 8000 GB rage. Düğüm başına 1 TB olan Standard_D16s_v3 düğümler için önerilen önbellek boyutunu ve düğüm başına 4 TB Standard_E32s_v3 düğümleri için önerilir.

Bu sanal makineler hakkında ek bilgi için aşağıdaki Microsoft Azure belgeleri okuyun:

* [Genel amaçlı sanal makine boyutları](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general)
* [Bellek için iyileştirilmiş sanal makine boyutları](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory)

## <a name="account-quota"></a>Hesap kotası

Aboneliğinizi Avere vFXT küme yanı sıra kullanılan bilgisayar ya da istemci sistemlerini çalıştırma kapasitesine sahip olduğundan emin olun. Okuma [vFXT küme için kota](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster) Ayrıntılar için.

## <a name="back-end-data-storage"></a>Arka uç veri depolama

Önbellekte olmadığı durumlarda, yeni bir Blob kapsayıcısında veya var olan bir bulut ya da donanım depolama sistemi çalışma kümesi depolanacak?

Azure Blob Depolama arka ucu için kullanmak istiyorsanız, vFXT küme oluşturma işleminin parçası olarak yeni bir kapsayıcı oluşturmanız gerekir. Kullanım ``create-cloud-backed-container`` dağıtım betiği ve depolama kaynağı hesap için yeni Blob kapsayıcı. Bu seçenek, oluşturur ve yeni bir kapsayıcı küme hazır olduktan hemen sonra kullanıma hazır şekilde yapılandırır. Okuma [kümesi düğümleri oluşturmalı ve yapılandırmalısınız](avere-vfxt-deploy.md#create-nodes-and-configure-the-cluster) Ayrıntılar için.

> [!NOTE]
> Yalnızca boş Blob Depolama kapsayıcıları çekirdek filtrelerin Avere vFXT sistemi için kullanılabilir. VFXT mevcut verilerinizi korumak gerek kalmadan kendi nesne deposu yönetebilmek gerekir. 
>
> Okuma [vFXT kümeye veri taşıma](avere-vfxt-data-ingest.md) istemci makineleri ve Avere vFXT önbellek kullanarak verimli bir şekilde verileri kümenin yeni kapsayıcıya kopyalama hakkında bilgi edinmek için.

Mevcut bir şirket içi depolama sistemi kullanmak istiyorsanız, oluşturulduktan sonra onu vFXT kümeye eklemeniz gerekir. ``create-minimal-cluster`` Dağıtım betiği ile hiçbir arka uç depolama vFXT küme oluşturur. Okuma [depolamayı yapılandırma](avere-vfxt-add-storage.md) mevcut bir depolama sistemi Avere vFXT kümeye ekleme hakkında ayrıntılı yönergeler için. 

## <a name="next-step-understand-the-deployment-process"></a>Sonraki adım: dağıtım sürecini anlama

[Dağıtıma genel bakış](avere-vfxt-deploy-overview.md) büyük resmi tüm Azure sistem için bir Avere vFXT oluşturmak ve verileri sunmak hazırlanma için gerekli olan adımları sağlar.  