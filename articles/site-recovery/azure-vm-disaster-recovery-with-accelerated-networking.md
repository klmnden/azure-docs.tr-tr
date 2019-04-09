---
title: Azure sanal makine olağanüstü durum kurtarma ile accelerated Networking | Microsoft Docs
description: Azure sanal makine olağanüstü durum kurtarma için hızlandırılmış ağ ile Azure Site Recovery etkinleştirmeyi açıklar
services: site-recovery
documentationcenter: ''
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: mayg
ms.openlocfilehash: c7edc7979636ced8697aa5ad724f9c6600d840bb
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59283367"
---
# <a name="accelerated-networking-with-azure-virtual-machine-disaster-recovery"></a>Azure sanal makine olağanüstü durum kurtarma ile hızlandırılmış ağ

Hızlandırılmış ağ, ağ performansını önemli ölçüde geliştirmek, bir VM için tek köklü g/ç Sanallaştırması (SR-IOV) sağlar. Bu yüksek performanslı yolu, gecikme süresi, değişim ve CPU kullanımı, En zorlu ağ iş yükleri desteklenen VM türleri ile kullanmak için azaltma datapath konaktan atlar. Aşağıdaki resimde, hızlandırılmış ağ bağlantısı olmadan ve iki sanal makineler arasındaki iletişimi gösterir:

![Karşılaştırma](./media/azure-vm-disaster-recovery-with-accelerated-networking/accelerated-networking-benefit.png)

Azure Site Recovery, hızlandırılmış ağ, avantajlarını Azure sanal makineleri farklı bir Azure bölgesine yük devretme için kullanmasına olanak tanır. Bu makalede nasıl, hızlandırılmış ağ ile Azure Site Recovery çoğaltılan Azure sanal makineleri için olanak sağlayabileceğiniz açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce anladığınızdan emin olun:
-   Azure sanal makine [çoğaltma mimarisi](azure-to-azure-architecture.md)
-   [Çoğaltma ayarlandıktan](azure-to-azure-tutorial-enable-replication.md) Azure sanal makineleri için
-   [Yük devretmeyi](azure-to-azure-tutorial-failover-failback.md) Azure sanal makineleri

## <a name="accelerated-networking-with-windows-vms"></a>Hızlandırılmış ağ ile Windows Vm'leri

Azure Site Recovery, yalnızca kaynak sanal makineyi hızlandırılmış ağ etkin varsa çoğaltılan sanal makineler için hızlandırılmış ağ etkinleştirme destekler. Hızlandırılmış ağ etkin kaynak sanal makineniz yoksa, sanal makineler için ağ Windows hızlandırılmış etkinleştirme konusunda bilgi edinebilirsiniz [burada](../virtual-network/create-vm-accelerated-networking-powershell.md#enable-accelerated-networking-on-existing-vms).

### <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri
Azure Galerisi hazır aşağıdaki dağıtımlar desteklenir:
* **Windows Server 2016 Datacenter**
* **Windows Server 2012 R2 Datacenter**

### <a name="supported-vm-instances"></a>Desteklenen sanal makine örnekleri
Hızlandırılmış ağ en genel amaçlı ve işlem için iyileştirilmiş örnek boyutları Vcpu, 2 veya daha fazla ile desteklenir.  Bu desteklenen serisi şunlardır: D/DSv2 ve F/Fs

Hiper iş parçacığı destek örneklerinde, hızlandırılmış ağ ile Vcpu, 4 veya daha fazla VM örneklerinde desteklenir. Desteklenen serisi şunlardır: D/DSv3, E/ESv3 Fsv2 ve Ms/Mms

Sanal makine örnekleri hakkında daha fazla bilgi için bkz. [Windows VM boyutları](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="accelerated-networking-with-linux-vms"></a>Linux Vm'leri ile hızlandırılmış ağ

Azure Site Recovery, yalnızca kaynak sanal makineyi hızlandırılmış ağ etkin varsa çoğaltılan sanal makineler için hızlandırılmış ağ etkinleştirme destekler. Hızlandırılmış ağ etkin kaynak sanal makineniz yoksa, Linux sanal makineleri için hızlandırılmış Ağ'ı etkinleştirme konusunda bilgi edinebilirsiniz [burada](../virtual-network/create-vm-accelerated-networking-cli.md#enable-accelerated-networking-on-existing-vms).

### <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri
Azure Galerisi hazır aşağıdaki dağıtımlar desteklenir:
* **Ubuntu 16.04**
* **SLES 12 SP3**
* **RHEL 7.4**
* **CentOS 7.4**
* **CoreOS Linux**
* **Debian "backports çekirdekle Uzat"**
* **Oracle Linux 7.4**

### <a name="supported-vm-instances"></a>Desteklenen sanal makine örnekleri
Hızlandırılmış ağ en genel amaçlı ve işlem için iyileştirilmiş örnek boyutları Vcpu, 2 veya daha fazla ile desteklenir.  Bu desteklenen serisi şunlardır: D/DSv2 ve F/Fs

Hiper iş parçacığı destek örneklerinde, hızlandırılmış ağ ile Vcpu, 4 veya daha fazla VM örneklerinde desteklenir. Desteklenen serisi şunlardır: D/DSv3, E/ESv3, Fsv2 ve Ms/Mms.

Sanal makine örnekleri hakkında daha fazla bilgi için bkz. [Linux VM boyutları](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="enabling-accelerated-networking-for-replicated-vms"></a>Çoğaltılan VM'ler için hızlandırılmış Ağ'ı etkinleştirme

Olduğunda, [çoğaltmayı etkinleştirme](azure-to-azure-tutorial-enable-replication.md) Azure sanal makineler için Site Recovery otomatik olarak sanal makine ağ arabirimleri, hızlandırılmış ağ etkin olup olmadığını algılar. Hızlandırılmış ağ zaten etkin değilse, Site Recovery otomatik olarak hızlandırılmış ağ çoğaltılan sanal makinenin ağ arabirimleri üzerinde yapılandırır.

Hızlandırılmış ağ durumunu altında doğrulanabilir **ağ arabirimleri** bölümünü **işlem ve ağ** çoğaltılan sanal makine için ayarları.

![Hızlandırılmış ağ ayarı](./media/azure-vm-disaster-recovery-with-accelerated-networking/compute-network-accelerated-networking.png)

Hızlandırılmış ağ kaynak sanal makinede çoğaltmayı etkinleştirdikten sonra etkinleştirdiyseniz, çoğaltılan sanal makinenin ağ arabirimleri için şu işlem tarafından hızlandırılmış ağ etkinleştirebilirsiniz:
1. Açık **işlem ve ağ** çoğaltılan sanal makine için ayarları
2. Altında ağ arabiriminin adına tıklayın **ağ arabirimleri** bölümü
3. Seçin **etkin** hızlandırılmış ağ açılan listeden **hedef** sütun

![Hızlandırılmış ağ iletişimi etkinleştirin](./media/azure-vm-disaster-recovery-with-accelerated-networking/network-interface-accelerated-networking-enabled.png)

Yukarıdaki işlem, ayrıca var olan çoğaltılmış, daha önce otomatik olarak Site Recovery tarafından etkin hızlandırılmış ağ yoktu makineler için gelmelidir.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [hızlandırılmış ağ avantajlarını](../virtual-network/create-vm-accelerated-networking-powershell.md#benefits).
- Sınırlamalar ve kısıtlamalar için hızlandırılmış ağ hakkında daha fazla bilgi edinin [Windows sanal makineleri](../virtual-network/create-vm-accelerated-networking-powershell.md#limitations-and-constraints) ve [Linux sanal makineleri](../virtual-network/create-vm-accelerated-networking-cli.md#limitations-and-constraints).
- Daha fazla bilgi edinin [kurtarma planları](site-recovery-create-recovery-plans.md) uygulama yük devretme otomatikleştirmek için.
