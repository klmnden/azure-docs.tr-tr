---
title: "Tasarım konuları Azure sanal makine ölçek kümeleri için | Microsoft Docs"
description: "Azure sanal makine ölçek kümeleri için tasarım konuları hakkında bilgi edinin"
keywords: "Linux sanal makine, sanal makine ölçek ayarlar"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: madhana
editor: tysonn
tags: azure-resource-manager
ms.assetid: c27c6a59-a0ab-4117-a01b-42b049464ca1
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: negat
ms.openlocfilehash: 0b05359938f4da544c4cb2a6fe60cfaf228478e1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="design-considerations-for-scale-sets"></a>Ölçek kümeleri için tasarım konuları
Bu konuda, sanal makine ölçek kümeleri için tasarım konuları açıklanmaktadır. Sanal makine ölçek kümeleri nelerdir hakkında daha fazla bilgi için bkz [sanal makine ölçek kümesi'ne genel bakış](virtual-machine-scale-sets-overview.md).

## <a name="when-to-use-scale-sets-instead-of-virtual-machines"></a>Ne zaman ölçeğini kullanmak yerine sanal makineleri ayarlar?
Genellikle, Ölçek kümeleri makineler kümesi benzer yapılandırmaya sahip olduğu yüksek oranda kullanılabilir altyapısını dağıtmak için kullanışlıdır. Diğer özellikler yalnızca Vm'lerde kullanılabilir, ancak bununla birlikte, bazı özellikler yalnızca ölçek kümeleri içinde kullanılabilir. Her bir teknolojiyi ne zaman kullanacağınızı hakkında bilinçli bir karar yapmak için biz ilk ölçek kümeleri ancak değil VM'ler kullanılabilir yaygın olarak kullanılan özelliklerden bazıları göz atın:

### <a name="scale-set-specific-features"></a>Ölçek kümesi özgü özellikleri

- Ölçek yapılandırma kümesi belirttiğinizde, paralel daha fazla sanal makineleri dağıtmak için "kapasitesi" özelliği yalnızca güncelleştirebilir. Bu paralel birçok ayrı VM dağıtma düzenlemek için bir komut dosyası yazma daha çok daha kolaydır.
- Yapabilecekleriniz [otomatik olarak ölçek kümesini ölçeklendirmek için Azure otomatik ölçeklendirme kullanmak](./virtual-machine-scale-sets-autoscale-overview.md) ancak bireysel VM'ler.
- Yapabilecekleriniz [yeniden görüntü oluşturma ölçek kümesi VM](https://docs.microsoft.com/rest/api/virtualmachinescalesets/manage-a-vm) ancak [bireysel VM'ler](https://docs.microsoft.com/rest/api/compute/virtualmachines).
- Yapabilecekleriniz [overprovision](./virtual-machine-scale-sets-design-overview.md) ölçek kümesi VM'ler daha fazla güvenilirlik ve daha hızlı dağıtım zamanları için. Bunu yapmak için özel kod yazmanıza sürece bu tek tek sanal makineleri ile yapamazsınız.
- Belirleyebileceğiniz bir [yükseltme İlkesi](./virtual-machine-scale-sets-upgrade-scale-set.md) yükseltmeler VM'ler üzerindeki ölçek kümesinde alma kolaylaştırmak için. Tek tek sanal makineleri ile güncelleştirmelerinin kendiniz yönetirler gerekir.

### <a name="vm-specific-features"></a>VM özgü özellikleri

Diğer taraftan, bazı özellikler yalnızca Vm'lerde kullanılabilir (en az şimdilik):

- Belirli tek tek sanal makineleri için veri diski ekleyebilirsiniz, ancak eklenen veri disklerini ölçek kümesindeki tüm VM'ler için yapılandırılır.
- Tek tek sanal makineleri ancak ölçek kümesindeki sanal makineleri için boş olmayan veri diskleri ekleyebilirsiniz.
- Sizin anlık görüntü tekil bir VM ancak VM ölçek kümesindeki değil.
- Tek bir VM'den ancak VM ölçek kümesindeki bir görüntüsünü yakalayabilirsiniz.
- Yönetilen disklere yerel disklerden tekil bir VM geçirebilirsiniz ancak bu VM'ler için bir ölçek kümesinde işlemi yapamazsınız.
- IPv6 ortak IP adresleri tekil VM NIC'lerle atayabilirsiniz ancak VM'ler için bir ölçek kümesinde bunu yapamazsınız. IPv6 ya da tek tek sanal makineleri önünde yük olarak genel IP adresleri atayabilir veya VM ölçek kümesi unutmayın.

## <a name="storage"></a>Depolama

### <a name="scale-sets-with-azure-managed-disks"></a>Azure yönetilen diskleri ölçek kümeleri
Ölçek kümeleri ile oluşturulabilir [Azure yönetilen diskleri](../virtual-machines/windows/managed-disks-overview.md) geleneksel Azure depolama hesapları yerine. Yönetilen diskleri aşağıdaki avantajları sağlar:
- VM ölçek kümesi için Azure depolama hesapları kümesi önceden oluşturmak zorunda değildir.
- Tanımlayabileceğiniz [veri diskleri ekli](virtual-machine-scale-sets-attached-disks.md) , Ölçek VM'ler için ayarlayın.
- Ölçek kümeleri için yapılandırılabilir [kümesindeki en çok 1.000 VM'ler Destek](virtual-machine-scale-sets-placement-groups.md). 

Var olan bir şablonu varsa, şunları da yapabilirsiniz. [yönetilen diskler için kullanılacak şablonu güncelleştirme](virtual-machine-scale-sets-convert-template-to-md.md).

### <a name="user-managed-storage"></a>Kullanıcı tarafından yönetilen depolama
Azure yönetilen disklerle tanımlanmamış bir ölçek kümesini kümesindeki VM'lerin OS disklerini depolamak için kullanıcı tarafından oluşturulan depolama hesapları kullanır. Depolama hesabı başına veya daha az 20 VM'ler oranını en fazla IO elde etmek ve ayrıca yararlanmak için önerilen _işleminin_ (aşağıya bakın). Ayrıca, depolama hesabı adları başlangıç karakterleri alfabe yayılan önerilir. Yardımcı yük farklı iç sistemlerden yayılan şekilde yapılıyor. 


## <a name="overprovisioning"></a>İşleminin
Ölçek şu anda varsayılan "VMs işleminin" olarak ayarlar. Açık işleminin ile ölçek gerçekte dönüş için sorulan olandan daha fazla Vm'leri yedekleme ayarlayın, sonra VM'ler istenen sayıda başarıyla sağlandıktan sonra ek VM'ler siler. İşleminin sağlama başarı oranları artırır ve dağıtım süresini azaltır. VM'ler için fazladan faturalandırılır değil ve doğru kota sınırları sayılmaz.

İşleminin sağlama başarı oranları artırmak sırada görüntülenmesini ve ardından kayboluyor ek VM'ler işlemek üzere tasarlanmamıştır bir uygulama için kafa karıştırıcı davranışlara neden olabilir. İşleminin devre dışı bırakmak için aşağıdaki dizeyi şablonunuzda olduğundan emin olun: `"overprovision": "false"`. Daha fazla ayrıntı bulunabilir [ölçek kümesi REST API belgeleri](/rest/api/virtualmachinescalesets/create-or-update-a-set).

Kullanıcı tarafından yönetilen depolama ölçek kümesini kullanıyorsa ve işleminin devre dışı bırakma, depolama hesabı başına 20'den fazla VMs olabilir, ancak 40 g/ç performansı artırmak için yukarıdaki gitmek için önerilmez. 

## <a name="limits"></a>Sınırlar
Bir Market görüntüsü (platform görüntüsü olarak da bilinir) oluşturulur ve Azure yönetilen diskleri kullanmak üzere yapılandırılmış bir ölçek kümesini en çok 1.000 VM'ler kapasitesini destekler. 100'den fazla sanal makineleri destekleyecek şekilde ayarlayın, Ölçek yapılandırırsanız, tüm senaryoları (örneğin, Yük Dengeleme için) aynı şekilde işler. Daha fazla bilgi için bkz: [büyük sanal makine ölçek kümeleri ile çalışma](virtual-machine-scale-sets-placement-groups.md). 

Ölçeği kullanıcı tarafından yönetilen depolama hesapları ile yapılandırılan Ayarla 100 VM'ler için şu anda sınırlıdır (ve 5 depolama hesapları için bu ölçek önerilir).

Özel görüntü (biri tarafından oluşturulan) üzerinde oluşturulan bir ölçek kümesini Azure yönetilen disklerle yapılandırıldığında kadar 300 VM'ler kapasitesine sahip olabilir. Ölçek kümesini kullanıcı tarafından yönetilen depolama hesapları ile yapılandırılmışsa, bir depolama hesabı içindeki tüm işletim sistemi diski VHD'leri oluşturmanız gerekir. Sonuç olarak, en fazla özel bir görüntüde yerleşik ölçek kümesindeki VM'lerin sayısını ve önerilen kullanıcı tarafından yönetilen depolama 20'dir. İşleminin devre dışı bıraktığınızda, 40 gidebilirsiniz.

Daha fazla VM'ler için bu sınırları izin verdiğinden gösterildiği gibi birden fazla ölçek kümeleri dağıtımı yapmanız [bu şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/301-custom-images-at-scale).

