---
title: Yüksek performanslı bilgi işlem - Azure sanal makineleri | Microsoft Docs
description: Azure üzerinde yüksek performanslı bilgi işlem hakkında bilgi edinin.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 05/07/2019
ms.author: amverma
ms.openlocfilehash: e8ff4147130dfeff14be41ed292b51ed34966df0
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67537675"
---
# <a name="optimization-for-linux"></a>Linux için iyileştirme

Bu makalede, işletim sistemi görüntüsü iyileştirmek üzere birkaç anahtar teknikleri gösterir. Daha fazla bilgi edinin [InfiniBand etkinleştirme](enable-infiniband.md) ve işletim sistemi görüntüleri en iyileştirme.

## <a name="update-lis"></a>Güncelleştirme LIS

Özel bir görüntü (örneğin, eski işletim sistemi CentOS/RHEL 7.4 veya 7.5 gibi) kullanarak dağıtıyorsanız, VM'ye LIS güncelleştirin.

```bash
wget https://aka.ms/lis
tar xzf lis
pushd LISISO
./upgrade.sh
```

## <a name="reclaim-memory"></a>Bellek boşaltma

Otomatik olarak uzak bellek erişimi önlemek için belleği geri kazanmak ile verimliliği artırın.

```bash
echo 1 >/proc/sys/vm/zone_reclaim_mode
```

Bu VM yeniden başlatıldıktan sonra kalıcı olması için:

```bash
echo "vm.zone_reclaim_mode = 1" >> /etc/sysctl.conf sysctl -p
```

## <a name="disable-firewall-and-selinux"></a>Güvenlik Duvarı ve SELinux devre dışı bırak

Güvenlik Duvarı ve SELinux devre dışı bırakın.

```bash
systemctl stop iptables.service
systemctl disable iptables.service
systemctl mask firewalld
systemctl stop firewalld.service
systemctl disable firewalld.service
iptables -nL
sed -i -e's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
```

## <a name="disable-cpupower"></a>Cpupower devre dışı bırak

Cpupower devre dışı bırakın.

```bash
service cpupower status
if enabled, disable it:
service cpupower stop
sudo systemctl disable cpupower
```

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [InfiniBand etkinleştirme](enable-infiniband.md) ve işletim sistemi görüntüleri en iyileştirme.

* Daha fazla bilgi edinin [HPC](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) azure'da.
