---
title: Azure sanal makine ağ aktarım hızı | Microsoft Docs
description: Azure sanal makine ağ aktarım hızı hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: steveesp
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 4/26/2019
ms.author: kumud,steveesp, mareat
ms.openlocfilehash: 9d74e53c754367ecfa63642514db93354fcadf25
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65153714"
---
# <a name="virtual-machine-network-bandwidth"></a>Sanal makine ağ bant genişliği

Azure, çeşitli VM boyutları ve türleri, her performans özelliklerini farklı bir karışımını sunar. Ağ aktarım hızı (veya bant genişliği) megabit (Mbps) cinsinden bunun bir özelliktir. Paylaşılan donanımda barındırılan sanal makineler için ağ kapasitesi oldukça aynı donanımı paylaşımı sanal makineler arasında paylaşılması gerekir. Büyük sanal makinelerin daha küçük sanal makineler görece daha fazla bant genişliği ayrılır.
 
Her sanal makineye ayrılan ağ bant genişliği, çıkış (giden) trafiğine, sanal makineden ölçülür. Tüm ağ trafiğini sanal makineyi bırakmak doğru ayrılmış sınır, hedef bağımsız olarak sayılır. Örneğin, bir sanal makine bir 1000 MB/sn sınırı varsa, aynı sanal ağda ya da Azure dışındaki başka bir sanal makineye giden trafiği hedeflenen olup bu sınırı uygular.
 
Giriş yok ölçülen veya doğrudan sınırlı. Ancak, gelen verileri işlemek için bir sanal makinenin becerisini etkileyebilir, CPU ve depolama limitleri gibi diğer faktörlere de vardır.

Hızlandırılmış ağ gecikme süresi, aktarım hızı ve CPU kullanımı gibi ağ performansını iyileştirmek için tasarlanmış bir özelliktir. Hızlandırılmış ağ, sanal makinenin aktarım hızını artırabilir, ancak sanal makinenin en fazla bant genişliği ayrılan yalnızca, bunu yapabilirsiniz. Hızlandırılmış ağ hakkında daha fazla bilgi için hızlandırılmış ağ sayfalarını bkz [Windows](create-vm-accelerated-networking-powershell.md) veya [Linux](create-vm-accelerated-networking-cli.md) sanal makineler.
 
Azure sanal makineleri, Mayıs ancak birkaç, ağ arabirimleri bağlı olması gerekir. Bir sanal makineye ayrılan bant genişliği giden tüm trafiği bir sanal makineye bağlı tüm ağ arabirimleri arasında toplamıdır. Diğer bir deyişle, kaç ağ arabirimleri için sanal makineye bağlı bağımsız olarak sanal makine başına ayrılan bant olur. Farklı Azure VM boyutları destek kaç ağ arabirimleri bilgi edinmek için Azure bkz [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM boyutları. 

## <a name="expected-network-throughput"></a>Beklenen ağ aktarım hızı

Giden beklenen aktarım hızıyla ve her sanal makine boyutu tarafından desteklenen ağ arabirimlerinin sayısını Azure'da ayrıntılı [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM boyutları. Genel amaçlı gibi bir türü ardından boyutu serisi Dv2 serisi gibi elde edilen sayfasında seçin. Her bir seri olan bir tablo, belirtimleri başlıklı son sütunda ağ ile **Maks NIC / beklenen ağ performansı (Mbps)** . 

Aktarım hızı sınırı, sanal makine için geçerlidir. Aktarım hızı, aşağıdaki faktörlerden etkilenmez:
- **Ağ arabirimleri sayısı**: Bant genişliği sınırı, sanal makineden giden tüm trafik toplu.
- **Hızlandırılmış**: Özellik yayımlanan sınırını elde etmeye yardımcı olabilir ancak sınırı değiştirmez.
- **Trafiği hedef**: Tüm hedeflere giden sınırında sayılır.
- **Protokol**: Tüm protokoller üzerinden giden tüm trafiği, sınırında sayılır.

## <a name="network-flow-limits"></a>Ağ akış limitleri

Bant genişliğine ek olarak, belirli bir zamanda bir sanal makinede mevcut ağ bağlantısı sayısı ağ performansını etkileyebilir. Azure ağ yığınının her yönünü 'akışlar' adlı veri yapılarını TCP/UDP bağlantı için durumu korur. Tipik bir TCP/UDP bağlantı oluşturan, bir gelen ve giden yön için başka 2 akışa sahip. 

Uç noktalar arasında veri aktarımı, veri aktarımı gerçekleştirmesi yanı sıra birkaç akışları oluşturulmasını gerektirir. DNS çözümlemesi için oluşturulan akışlar ve yük dengeleyici sistem durumu araştırmaları için oluşturulan akışların bazı örnekler verilmiştir. Ayrıca ağ sanal Gereçleri (Nva) ağ geçitleri, Ara sunucuları, güvenlik duvarları, Not gereç sonlandırıldı ve Gereci tarafından oluşturulan bağlantıları için oluşturulan akışların görürsünüz. 

![TCP konuşma iletme Gereci aracılığıyla akış sayısı](media/virtual-machine-network-throughput/flow-count-through-network-virtual-appliance.png)

## <a name="flow-limits-and-recommendations"></a>Akış limitleri ve öneriler

Bugün Azure ağ yığınını 8 CPU Çekirdeği ve VM'ler için iyi bir performans az 8 CPU çekirdeği ile 100 k toplam akışlar daha büyük sanal makineler için iyi bir performans 250 bin toplam ağ akışlarıyla desteklemektedir. Akışlar, gelen ve 500, 500 K sabit bir sınır 1 milyon adede kadar ek akışlar toplam için bu sınırı ağ performansı düzgün bir şekilde düşürür K hangi ek akışlar bırakılan sonra giden.

||Vm'lerle < 8 CPU Çekirdeği|VM'ler ile 8 + CPU çekirdekleri|
|---|---|---|
|<b>İyi bir performans</b>|100 bin akışlar |250 bin akışlar|
|<b>Performans</b>|100 bin cinsinden akışlar|250 bin akışlar|
|<b>Akış sınırı</b>|1M akışlar|1M akışlar|

Ölçümler kullanılabilir [Azure İzleyici](../azure-monitor/platform/metrics-supported.md#microsoftcomputevirtualmachines) ağ akışlarına ve akış oluşturma oranını sayısını, VM veya VMSS örnekleri izlemek için.

![Azure İzleyici akışı metrics.png](media/virtual-machine-network-throughput/azure-monitor-flow-metrics.png)

Bağlantı kurma ve sonlandırma ücretleri, ayrıca bağlantı kurma ve sonlandırma paylaşımlarıyla CPU işleme rutinleri paket olarak ağ performansını etkileyebilir. Beklenen trafik modelleri ve iş yüklerinin ölçeğini iş yükleri uygun şekilde performans ihtiyaçlarınıza uyum sağlayacak Kıyaslama olmasını öneririz. 

## <a name="next-steps"></a>Sonraki adımlar

- [Sanal makine işletim sistemi için ağ aktarım hızını iyileştirme](virtual-network-optimize-network-bandwidth.md)
- [Test ağ aktarım hızı](virtual-network-bandwidth-testing.md) bir sanal makine için.
