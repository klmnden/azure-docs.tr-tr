---
title: SR-IOV - Azure sanal makineleri ile InifinBand etkinleştirme | Microsoft Docs
description: Infiniband ile SR-IOV etkinleştirme konusunda bilgi edinin.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 05/15/2019
ms.author: amverma
ms.openlocfilehash: 2e28627359f339a3bf818a15d6a5c8e456fb554a
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67797535"
---
# <a name="enable-infiniband-with-sr-iov"></a>SR-IOV ile InfiniBand etkinleştir

Infiniband (IB) ile özel bir VM görüntüsü yapılandırmak için basit ve önerilen yol, dağıtımınıza InfiniBandDriverLinux veya InfiniBandDriverWindows VM uzantısı eklemektir.
Bu VM uzantıları ile kullanmayı öğrenin [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-hpc#rdma-capable-instances) ve [Windows](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-hpc#rdma-capable-instances)

Özellikli VM'ler (şu anda HB ve HC serisi) InfiniBand SR-IOV el ile yapılandırmak için aşağıdaki adımları izleyin. Bu adımlar yalnızca RHEL/CentOS içindir. Ubuntu (16.04 ve 18.04) ve SLES (12 SP4 ve 15) için gelen sürücüler de çalışır.

## <a name="manually-install-ofed"></a>OFED el ile yükleyin

En son MLNX_OFED sürücüleri ConnectX-5'ten için yükleme [Mellanox](https://www.mellanox.com/page/products_dyn?product_family=26).

RHEL/CentOS için (Örneğin aşağıda 7.6):

```bash
sudo yum install -y kernel-devel python-devel
sudo yum install -y redhat-rpm-config rpm-build gcc-gfortran gcc-c++
sudo yum install -y gtk2 atk cairo tcl tk createrepo
wget --retry-connrefused --tries=3 --waitretry=5 http://content.mellanox.com/ofed/MLNX_OFED-4.5-1.0.1.0/MLNX_OFED_LINUX-4.5-1.0.1.0-rhel7.6-x86_64.tgz
tar zxvf MLNX_OFED_LINUX-4.5-1.0.1.0-rhel7.6-x86_64.tgz
sudo ./MLNX_OFED_LINUX-4.5-1.0.1.0-rhel7.6-x86_64/mlnxofedinstall --add-kernel-support
```

ConnectX-5'ten WinOF 2 sürücüleri yükleyebilir ve Windows için indirin [Mellanox](https://www.mellanox.com/page/products_dyn?product_family=32&menu_section=34)

## <a name="enable-ipoib"></a>IPoIB etkinleştir

```bash
sudo sed -i 's/LOAD_EIPOIB=no/LOAD_EIPOIB=yes/g' /etc/infiniband/openib.conf
sudo /etc/init.d/openibd restart
if [ $? -eq 1 ]
then
  sudo modprobe -rv  ib_isert rpcrdma ib_srpt
  sudo /etc/init.d/openibd restart
fi
```

## <a name="assign-an-ip-address"></a>Bir IP adresi atama

Bir IP adresi kullanarak ib0 arabirimine atayın:

- El ile IP adresi ib0 arabirimine (kök) atayın.

    ```bash
    ifconfig ib0 $(sed '/rdmaIPv4Address=/!d;s/.*rdmaIPv4Address="\([0-9.]*\)".*/\1/' /var/lib/waagent/SharedConfig.xml)/16
    ```

OR

- IP adresi atayın ve kalıcı kolaylaştırmak için WALinuxAgent kullanın.

    ```bash
    yum install -y epel-release
    yum install -y python-pip
    python -m pip install --upgrade pip setuptools wheel
    wget "https://github.com/Azure/WALinuxAgent/archive/release-2.2.36.zip"
    unzip release-2.2.36.zip
    cd WALinuxAgent*
    python setup.py install --register-service --force
    sed -i -e 's/# OS.EnableRDMA=y/OS.EnableRDMA=y/g' /etc/waagent.conf
    sed -i -e 's/# AutoUpdate.Enabled=y/AutoUpdate.Enabled=y/g' /etc/waagent.conf
    systemctl restart waagent
    ```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [HPC](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) azure'da.
