---
title: Azure sanal makine ağ aktarım hızı | Microsoft Docs
description: Azure sanal makine ağ aktarım hızı hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/13/2017
ms.author: kumud
ms.openlocfilehash: 182b3b7dad828e67d006391e00986406729c959d
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64689245"
---
# <a name="virtual-machine-network-bandwidth"></a>Sanal makine ağ bant genişliği

Azure, çeşitli VM boyutları ve türleri, her performans özelliklerini farklı bir karışımını sunar. Ağ aktarım hızı (veya bant genişliği) megabit (Mbps) cinsinden bunun bir özelliktir. Paylaşılan donanımda barındırılan sanal makineler için ağ kapasitesi oldukça aynı donanımı paylaşımı sanal makineler arasında paylaşılması gerekir. Büyük sanal makinelerin daha küçük sanal makineler görece daha fazla bant genişliği ayrılır.
 
Her sanal makineye ayrılan ağ bant genişliği, çıkış (giden) trafiğine, sanal makineden ölçülür. Tüm ağ trafiğini sanal makineyi bırakmak doğru ayrılmış sınır, hedef bağımsız olarak sayılır. Örneğin, bir sanal makine bir 1000 MB/sn sınırı varsa, aynı sanal ağda ya da Azure dışındaki başka bir sanal makineye giden trafiği hedeflenen olup bu sınırı uygular.
 
Giriş yok ölçülen veya doğrudan sınırlı. Ancak, gelen verileri işlemek için bir sanal makinenin becerisini etkileyebilir, CPU ve depolama limitleri gibi diğer faktörlere de vardır.

Hızlandırılmış ağ gecikme süresi, aktarım hızı ve CPU kullanımı gibi ağ performansını iyileştirmek için tasarlanmış bir özelliktir. Hızlandırılmış ağ, sanal makinenin aktarım hızını artırabilir, ancak sanal makinenin en fazla bant genişliği ayrılan yalnızca, bunu yapabilirsiniz. Hızlandırılmış ağ hakkında daha fazla bilgi için hızlandırılmış ağ sayfalarını bkz [Windows](create-vm-accelerated-networking-powershell.md) veya [Linux](create-vm-accelerated-networking-cli.md) sanal makineler.
 
Azure sanal makineleri, Mayıs ancak birkaç, ağ arabirimleri bağlı olması gerekir. Bir sanal makineye ayrılan bant genişliği giden tüm trafiği bir sanal makineye bağlı tüm ağ arabirimleri arasında toplamıdır. Diğer bir deyişle, kaç ağ arabirimleri için sanal makineye bağlı bağımsız olarak sanal makine başına ayrılan bant olur. Farklı Azure VM boyutları destek kaç ağ arabirimleri bilgi edinmek için Azure bkz [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM boyutları. 

## <a name="expected-network-throughput"></a>Beklenen ağ aktarım hızı

Giden beklenen aktarım hızıyla ve her sanal makine boyutu tarafından desteklenen ağ arabirimlerinin sayısını Azure'da ayrıntılı [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM boyutları. Genel amaçlı gibi bir türü ardından boyutu serisi Dv2 serisi gibi elde edilen sayfasında seçin. Her bir seri olan bir tablo, belirtimleri başlıklı son sütunda ağ ile **Maks NIC / beklenen ağ performansı (Mbps)**. 

Aktarım hızı sınırı, sanal makine için geçerlidir. Aktarım hızı, aşağıdaki faktörlerden etkilenmez:
- **Ağ arabirimleri sayısı**: Bant genişliği sınırı, sanal makineden giden tüm trafik toplu.
- **Hızlandırılmış**: Özellik yayımlanan sınırını elde etmeye yardımcı olabilir ancak sınırı değiştirmez.
- **Trafiği hedef**: Tüm hedeflere giden sınırında sayılır.
- **Protokol**: Tüm protokoller üzerinden giden tüm trafiği, sınırında sayılır.

## <a name="next-steps"></a>Sonraki adımlar

- [Sanal makine işletim sistemi için ağ aktarım hızını iyileştirme](virtual-network-optimize-network-bandwidth.md)
- [Test ağ aktarım hızı](virtual-network-bandwidth-testing.md) bir sanal makine için.
