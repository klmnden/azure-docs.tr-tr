---
title: Tasarım konuları için Azure sanal makine ölçek kümeleri | Microsoft Docs
description: Azure sanal makine ölçek kümeleriniz için tasarım konuları hakkında bilgi edinin
keywords: Linux sanal makinesi, sanal makine ölçek kümeleri
services: virtual-machine-scale-sets
documentationcenter: ''
author: mayanknayar
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: c27c6a59-a0ab-4117-a01b-42b049464ca1
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: manayar
ms.openlocfilehash: 67bbad7e73f33d73d4c3f1d4f7e5599d2ef914e3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60618481"
---
# <a name="design-considerations-for-scale-sets"></a>Ölçek kümeleri için tasarım konuları
Bu makalede, sanal makine ölçek kümeleri için tasarım konuları açıklanmaktadır. Sanal makine ölçek kümeleri nelerdir hakkında daha fazla bilgi için bkz [sanal makine ölçek kümelerine genel bakış](virtual-machine-scale-sets-overview.md).

## <a name="when-to-use-scale-sets-instead-of-virtual-machines"></a>Zaman ölçeğini kullanmak, sanal makineler yerine ayarlar?
Genel olarak, Ölçek kümeleri bir makine kümesi benzer yapılandırmasına sahip olduğu yüksek oranda kullanılabilir altyapısını dağıtmak için kullanışlıdır. Diğer özellikler yalnızca VM'ler kullanılabilir ancak bununla birlikte, bazı özellikler yalnızca ölçek kümelerinde kullanılabilir. Ne zaman her bir teknolojiyi kullanılacağı konusunda bilinçli bir karar için ilk ölçek kümeleri, ancak olmayan VM'ler kullanılabilir olan yaygın olarak kullanılan özelliklerin bazılarına göz atın:

### <a name="scale-set-specific-features"></a>Ölçek kümesi özgü özellikler

- Ölçek kümesi yapılandırması belirttiğinizde, güncelleştirebilirsiniz *kapasite* paralel daha fazla sanal makine dağıtmak için özellik. Bu işlem, paralel birçok tek tek sanal makineleri dağıtma düzenlemek için bir komut dosyası yazma daha iyidir.
- Yapabilecekleriniz [bir ölçek kümesini otomatik olarak ölçeklendirmek için Azure otomatik ölçeklendirme kullanın](./virtual-machine-scale-sets-autoscale-overview.md) ancak değil tek VM'ler.
- Yapabilecekleriniz [reimage ölçek kümesinin Vm'leri](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/reimage) ancak [değil tek VM'ler](https://docs.microsoft.com/rest/api/compute/virtualmachines).
- Yapabilecekleriniz [overprovision](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview#overprovisioning) ölçek kümesinin Vm'leri daha fazla güvenilirlik ve daha hızlı dağıtım süreleri için. Bu eylemi gerçekleştirmek için özel bir kod yazmadığınız sürece tek VM'ler için fazladan olamaz.
- Belirtebileceğiniz bir [yükseltme ilkesini](./virtual-machine-scale-sets-upgrade-scale-set.md) ölçek kümenizdeki sanal makinelerde yükseltme konusunu alınacağı kolaylaştırır. Tek tek Vm'lerle, güncelleştirmeleri kendiniz düzenleyin gerekir.

### <a name="vm-specific-features"></a>Sanal Makineye özgü özellikler

Bazı özellikler şu anda yalnızca VM'ler kullanılabilir:

- Tek bir VM'den değiştirebilir, ancak bir ölçek kümesindeki bir sanal makine görüntü yakalamak için.
- Tek bir VM yerel disklerden yönetilen disklere geçirebilirsiniz, ancak bir ölçek kümesindeki sanal makine örnekleri geçiremezsiniz.
- Tek VM sanal ağ arabirimi kartlarıyla (NIC) IPv6 genel IP adresleri atayabilirsiniz ancak bir ölçek kümesindeki sanal makine örnekleri için bunu yapamazsınız. Yük Dengeleyiciler önüne ya da tek tek sanal makineleri için IPv6 genel IP adresleri atayabilirim miyim veya VM ölçek kümesi.

## <a name="storage"></a>Depolama

### <a name="scale-sets-with-azure-managed-disks"></a>Azure yönetilen diskler içeren ölçek kümeleri
Ölçek kümeleri oluşturulabilir [Azure yönetilen diskler](../virtual-machines/windows/managed-disks-overview.md) geleneksel Azure depolama hesapları yerine. Yönetilen diskler, aşağıdaki avantajları sağlar:
- VM ölçek kümesi için Azure depolama hesapları bir dizi önceden oluştur gerekmez.
- Tanımlayabileceğiniz [bağlı veri diskleri](virtual-machine-scale-sets-attached-disks.md) ölçek kümenizdeki VM'lerin ayarlayın.
- Ölçek kümeleri için yapılandırılabilir [kümesindeki 1.000 VM'ye kadar destek](virtual-machine-scale-sets-placement-groups.md). 

Mevcut bir şablonu varsa, ayrıca [şablonunu yönetilen diskler kullanacak şekilde güncelleştirmek](virtual-machine-scale-sets-convert-template-to-md.md).

### <a name="user-managed-storage"></a>Kullanıcı tarafından yönetilen depolama
Kullanıcı tarafından oluşturulan depolama hesapları, kümedeki sanal makinelerin işletim sistemi diskleri depolamak için Azure yönetilen diskler ile tanımlanmamış ölçek kümesi kullanır. Depolama hesabı başına ya da daha az 20 VM oranını en yüksek g/ç ulaşmasını sağlamak ve ayrıca yararlanmak önerilir _açıdan_ (aşağıya bakın). Depolama hesabı adları başındaki karakterleri arasında alfabetik yayılan önerilir. Yardımcı yük iç farklı sistemlerden yayılır. Bu nedenle yapılıyor. 


## <a name="overprovisioning"></a>Fazla sağlama
Ölçek, şu anda "Vm'leri fazla sağlama" için varsayılan ayarlar. Açık açıdan ile ölçek gerçekten dönüş için sorulan çok daha fazla sanal makine yedekleme ayarlayın, ardından istenen VM sayısını başarıyla kaynak sağlandı sonra fazla Vm'leri siler. Fazla sağlama sağlama başarı oranlarını artırır ve dağıtım süresini azaltır. VM'ler için fazladan faturalandırılmaz ve doğru kota sınırları sayılmaz.

Açıdan sağlama başarı oranlarını artırmalarına, görünen ve ardından kaybolmasını ek VM'ler işlemek üzere tasarlanmamıştır bir uygulama için karmaşık davranışlara neden olabilir. Açıdan devre dışı bırakmak için aşağıdaki dizeyi şablonunuzda olduğundan emin olun: `"overprovision": "false"`. Daha fazla ayrıntı bulunabilir [ölçek kümesi REST API'si belgeleri](/rest/api/virtualmachinescalesets/create-or-update-a-set).

Kullanıcı tarafından yönetilen depolama ölçek kümeniz kullanır ve, açıdan devre dışı bırakmanız, depolama hesabı başına 20'den fazla Vm'niz olabilir, ancak 40 GÇ performansı artırmak için yukarıda gitmek için önerilmez. 

## <a name="limits"></a>Limits
Bir Market görüntüsü (platform görüntüsü olarak da bilinir) üzerinde oluşturulmuş ve Azure yönetilen diskleri kullanacak şekilde yapılandırılmış bir ölçek kümesi kapasitesi 1.000 VM'ye kadar destekler. Ölçek kümenizi 100'den fazla Vm'leri destekleyecek şekilde yapılandırırsanız, tüm senaryolarda (için örnek Yük Dengeleme) aynı şekilde işler. Daha fazla bilgi için [büyük sanal makine ölçek kümeleri ile çalışma](virtual-machine-scale-sets-placement-groups.md). 

Bir ölçek kümesi kullanıcı tarafından yönetilen depolama hesapları ile yapılandırılan 100 VM ile şu anda sınırlı olmasına (ve 5 depolama hesapları, bu ölçek birimi için önerilir).

Özel bir görüntü (birisi sizin tarafınızdan oluşturulmuş) temel alan bir ölçek kümesi, Azure yönetilen diskler ile yapılandırıldığında, en fazla 600 VM kapasitesine sahip olabilir. Ölçek kümesi, kullanıcı tarafından yönetilen depolama hesapları ile yapılandırılmışsa, bir depolama hesabı içindeki tüm işletim sistemi diski VHD oluşturmanız gerekir. Sonuç olarak, özel bir görüntü yerleşik bir ölçek kümesi sanal makine sayısı üst sınırı önerilen ve kullanıcı tarafından yönetilen depolama 20'dir. Açıdan kapalı kapatırsanız, 40'a kadar gidebilirsiniz.

Daha fazla sanal makine için limitler izin verdiğinden, gösterildiği gibi birden çok ölçek kümelerini dağıtmak ihtiyacınız [Bu şablon](https://github.com/Azure/azure-quickstart-templates/tree/master/301-custom-images-at-scale).

