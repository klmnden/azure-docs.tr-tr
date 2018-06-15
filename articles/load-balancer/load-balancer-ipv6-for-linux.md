---
title: DHCPv6 Linux VM'ler için yapılandırma | Microsoft Docs
description: DHCPv6 Linux VM'ler için yapılandırılır.
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: ''
keywords: IPv6, azure yük dengeleyici, çift yığın, genel IP, yerel IPv6, mobil, IOT
ms.assetid: b32719b6-00e8-4cd0-ba7f-e60e8146084b
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 6248ed2f55fb5bbcc2061af6ce1dedf2bd31ccad
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
ms.locfileid: "30261856"
---
# <a name="configure-dhcpv6-for-linux-vms"></a>DHCPv6 Linux VM'ler için yapılandırma


Azure Market Linux sanal makine görüntüleri bazıları, Dinamik Ana Bilgisayar Yapılandırma Protokolü sürüm 6 (DHCPv6) varsayılan olarak yapılandırılan gerekmez. IPv6 desteği için kullandığınız Linux işletim sistemi dağıtımlarında DHCPv6 yapılandırılması gerekir. Farklı paketler kullandığından çeşitli Linux dağıtımları DHCPv6 çeşitli şekillerde yapılandırın.

> [!NOTE]
> Azure Market son SUSE Linux ve CoreOS görüntüleri DHCPv6 ile önceden yapılandırılmış olmuştur. Bu görüntüleri kullandığınızda, ek değişiklik gerekmez.

Bu belge, Linux sanal makine bir IPv6 adresi alacağı DHCPv6 etkinleştirmeyi açıklar.

> [!WARNING]
> Ağ yapılandırma dosyalarını yanlış bir şekilde düzenleyerek VM'nize ağ erişimi kaybedebilirsiniz. Üretim dışı sistemlere yapılandırma değişikliklerinizi test önerilir. Bu makaledeki yönergeleri Azure Marketi Linux görüntülerinde son sürümlerinde test edilmiştir. Daha ayrıntılı yönergeler için kendi Linux sürümü için belgelere bakın.

## <a name="ubuntu"></a>Ubuntu

1. Düzen */etc/dhcp/dhclient6.conf* dosyasını bulun ve aşağıdaki satırı ekleyin:

        timeout 10;

2. Eth0 arabirimi için ağ yapılandırması aşağıdaki yapılandırma ile düzenleyin:

   * Üzerinde **Ubuntu 12.04 ve 14.04**, düzenleme */etc/network/interfaces.d/eth0.cfg* dosya. 
   * Üzerinde **Ubuntu 16.04**, düzenleme */etc/network/interfaces.d/50-cloud-init.cfg* dosya.

         iface eth0 inet6 auto
             up sleep 5
             up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. IPv6 adresi yenileme:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="debian"></a>Debian

1. Düzen */etc/dhcp/dhclient6.conf* dosyasını bulun ve aşağıdaki satırı ekleyin:

        timeout 10;

2. Düzen */etc/network/interfaces* dosya ve aşağıdaki yapılandırma ekleyin:

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. IPv6 adresi yenileme:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel-centos-and-oracle-linux"></a>RHEL, CentOS ve Oracle Linux

1. Düzen */etc/sysconfig/network* dosyasını bulun ve aşağıdaki parametreyi ekleyin:

        NETWORKING_IPV6=yes

2. Düzen */etc/sysconfig/network-scripts/ifcfg-eth0* dosya ve aşağıdaki iki parametreyi ekleyin:

        IPV6INIT=yes
        DHCPV6C=yes

3. IPv6 adresi yenileme:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11-and-opensuse-13"></a>SLES 11 ve openSUSE 13

Son SUSE Linux Enterprise Server (SLES) ve Azure openSUSE görüntüler DHCPv6 ile önceden yapılandırılmış olmuştur. Bu görüntüleri kullandığınızda, ek değişiklik gerekmez. Bir özel veya eski SUSE görüntüyü temel alarak bir VM'niz varsa, aşağıdakileri yapın:

1. Yükleme `dhcp-client` gerekiyorsa, paket:

    ```bash
    sudo zypper install dhcp-client
    ```

2. Düzen */etc/sysconfig/network/ifcfg-eth0* dosyasını bulun ve aşağıdaki parametreyi ekleyin:

        DHCLIENT6_MODE='managed'

3. IPv6 adresi yenileme:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a>SLES 12 ve openSUSE artık

Azure'nün en son SLES ve openSUSE görüntülerinde DHCPv6 ile önceden yapılandırılmış olmuştur. Bu görüntüleri kullandığınızda, ek değişiklik gerekmez. Bir özel veya eski SUSE görüntüyü temel alarak bir VM'niz varsa, aşağıdakileri yapın:

1. Düzenleme */etc/sysconfig/network/ifcfg-eth0* dosya ve değiştirme `#BOOTPROTO='dhcp4'` parametresi şu değere sahip:

        BOOTPROTO='dhcp'

2. İçin */etc/sysconfig/network/ifcfg-eth0* dosya, aşağıdaki parametreyi ekleyin:

        DHCLIENT6_MODE='managed'

3. IPv6 adresi yenileme:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a>CoreOS

Azure son CoreOS görüntülerinde DHCPv6 ile önceden yapılandırılmış olmuştur. Bu görüntüleri kullandığınızda, ek değişiklik gerekmez. Bir özel veya eski CoreOS görüntüyü temel alarak bir VM'niz varsa, aşağıdakileri yapın:

1. Düzen */etc/systemd/network/10_dhcp.network* dosyası:

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. IPv6 adresi yenileme:

    ```bash
    sudo systemctl restart systemd-networkd
    ```
