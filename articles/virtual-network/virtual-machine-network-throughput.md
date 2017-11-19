---
title: "Azure sanal makine ağ verimliliği | Microsoft Docs"
description: "Azure sanal makine ağ verimliliği hakkında bilgi edinin."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/13/2017
ms.author: jdial
ms.openlocfilehash: 536a779d7de51180aa6410911dea2b6c47780c2f
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="virtual-machine-network-throughput"></a>Sanal makine ağ verimliliği

Azure çeşitli sanal makine boyutları ve türleri, her performans özellikleri farklı karışımını içeren sunar. Bu performans özelliklerinin yanı sıra ağ verimliliği (veya bant genişliği) ölçülür megabit (Mbps) içinde. Sanal makineleri paylaşılan donanımda barındırıldığından, ağ kapasitesi oldukça aynı donanımını paylaşan sanal makineler arasında paylaşılması gerekir. Daha büyük sanal makineler daha küçük sanal makinelerden görece daha fazla bant genişliği ayrılır.
 
Her bir sanal makine için ayrılan ağ bant genişliği, sanal makineden çıkış (giden) trafiği üzerinde ölçülen. Sanal makine bırakarak tüm ağ trafiğini doğru hedef bağımsız olarak ayrılmış sınırı sayılır. Örneğin, bir sanal makine 1000 MB/sn sınırı varsa, aynı sanal ağda veya Azure dışında başka bir sanal makine için giden trafik hedefleyen olup olmadığını bu sınırı geçerlidir.
 
Giriş yok ölçülen veya doğrudan sınırlı. Ancak, bir sanal makinenin gelen verileri işleme yeteneğini etkileyebilir, CPU ve depolama sınırları gibi diğer faktörlere de vardır.

[Ağ hızlandırılmış](virtual-network-create-vm-accelerated-networking.md) gecikme süresi, üretilen iş ve CPU kullanımı da dahil olmak üzere ağ performansını iyileştirmek için tasarlanmış bir özelliktir. Hızlandırılmış ağ bir sanal makinenin verimliliği artırabilir, ancak sanal makinenin en fazla bant genişliği ayrılan yalnızca bu bunu yapabilirsiniz.
 
Azure sanal makineler, olabilir ancak birkaç, ağ arabirimleri kendisine bağlı olması gerekir. Bir sanal makineye ayrılan bant genişliği giden tüm trafiği bir sanal makineye bağlı tüm ağ arabirimleri toplamıdır. Diğer bir deyişle, ayrılan bant kaç ağ arabirimleri için sanal makineye bağlı bağımsız olarak sanal makine başına ' dir. Farklı Azure VM boyutlarını destek kaç ağ arabirimleri öğrenmek için Azure bkz [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM boyutları. 

## <a name="expected-network-throughput"></a>Beklenen bir ağ verimliliği

Beklenen giden verimlilik ve her VM boyutu tarafından desteklenen ağ arabirimleri sayısı Azure'da ayrıntılı [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM boyutları. Genel amaçlı gibi bir türü ardından boyutu serisi Dv2 serisi gibi sonuçta elde edilen sayfasında seçin. Her seri başlıklı son sütun belirtimlerini ağ ile bir tablolu **Max NIC'ler / beklenen ağ performansı (Mbps)**. 

Üretilen iş sınırı, sanal makine için geçerlidir. Verimlilik aşağıdaki etkenlere göre etkilenmez:
- **Ağ arabirimleri sayısı**: bant genişliği sınırı, sanal makineden giden tüm trafiği toplu bir pakettir.
- **Ağ hızlandırılmış**: özellik yayımlanan sınırı elde etmeye yardımcı olabilir ancak sınırı değiştirmez.
- **Trafiği hedef**: tüm hedefleri giden sınırında sayılır.
- **Protokol**: tüm protokoller üzerinden giden tüm trafiği sınırında sayılır.

## <a name="next-steps"></a>Sonraki adımlar

- [Bir sanal makine işletim sistemi için ağ verimliliğini en iyi duruma getirme](virtual-network-optimize-network-bandwidth.md)
- [Test ağ verimliliği](virtual-network-bandwidth-testing.md) bir sanal makine için.