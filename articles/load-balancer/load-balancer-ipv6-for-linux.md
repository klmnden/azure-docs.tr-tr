---
title: "DHCPv6 Linux VM'ler için yapılandırma | Microsoft Docs"
description: "DHCPv6 Linux VM'ler için yapılandırılır."
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: 
keywords: "IPv6, azure yük dengeleyici, çift yığın, genel IP, yerel IPv6, mobil, IOT"
ms.assetid: b32719b6-00e8-4cd0-ba7f-e60e8146084b
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 84558cb6e3a5524969f590eb0272a64ad8839ab5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configuring-dhcpv6-for-linux-vms"></a>Linux VM’leri için DHCPv6’yı yapılandırma

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Azure Market Linux sanal makine görüntüleri bazıları varsayılan olarak yapılandırılan DHCPv6 sahip değilsiniz. IPv6 desteği için DHCPv6, kullandığınız Linux işletim sistemi dağıtım içinde yapılandırılmış olması gerekir. Farklı Linux dağıtımları farklı paketler kullandığından, DHCPv6 yapılandırma farklı yolu vardır.

> [!NOTE]
> Azure Market son SUSE Linux ve CoreOS görüntüleri DHCPv6 ile önceden yapılandırılmış olmuştur. Bu görüntüleri kullanırken, ek değişiklik gerekmez.

Bu belge, Linux sanal makine bir IPv6 adresi alacağı DHCPv6 etkinleştirmeyi açıklar.

> [!WARNING]
> Ağ yapılandırma dosyalarını yanlış düzenlenmesi, VM ağ erişimi kaybetmenize neden olabilir. Üretim dışı sistemlere yapılandırma değişikliklerinizi test önerilir. Bu makaledeki yönergeleri Azure Marketi Linux görüntülerinde son sürümlerinde test edilmiştir. Sürümünüzün Linux daha ayrıntılı yönergeler için belgelere bakın.

## <a name="ubuntu"></a>Ubuntu

1. Dosyayı düzenleme `/etc/dhcp/dhclient6.conf` ve aşağıdaki satırı ekleyin:

        timeout 10;

2. Eth0 arabirimi için ağ yapılandırması aşağıdaki yapılandırma ile düzenleyin:

   * Üzerinde **Ubuntu 12.04 ve 14.04**, dosyayı düzenleyin.`/etc/network/interfaces.d/eth0.cfg`
   * Üzerinde **Ubuntu 16.04**, dosyayı düzenleyin.`/etc/network/interfaces.d/50-cloud-init.cfg`

         iface eth0 inet6 auto
             up sleep 5
             up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. IPv6 adresi yenileme:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="debian"></a>Debian

1. Dosyayı düzenleme `/etc/dhcp/dhclient6.conf` ve aşağıdaki satırı ekleyin:

        timeout 10;

2. Dosyayı düzenleme `/etc/network/interfaces` ve aşağıdaki yapılandırma ekleyin:

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. IPv6 adresi yenileme:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel--centos--oracle-linux"></a>RHEL / CentOS / Oracle Linux

1. Dosyayı düzenleme `/etc/sysconfig/network` ve aşağıdaki parametresini ekleyin:

        NETWORKING_IPV6=yes

2. Dosyayı düzenleme `/etc/sysconfig/network-scripts/ifcfg-eth0` ve aşağıdaki iki parametreyi ekleyin:

        IPV6INIT=yes
        DHCPV6C=yes

3. IPv6 adresi yenileme:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11--opensuse-13"></a>SLES 11 & openSUSE 13

Azure'nün en son SLES ve openSUSE görüntülerinde DHCPv6 ile önceden yapılandırılmış olmuştur. Bu görüntüleri kullanırken, ek değişiklik gerekmez. Bir özel veya eski SUSE görüntüyü temel alarak bir VM'niz varsa, aşağıdaki adımları kullanın:

1. Yükleme `dhcp-client` gerekiyorsa, paket:

    ```bash
    sudo zypper install dhcp-client
    ```

2. Dosyayı düzenleme `/etc/sysconfig/network/ifcfg-eth0` ve aşağıdaki parametresini ekleyin:

        DHCLIENT6_MODE='managed'

3. IPv6 adresi yenileme:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a>SLES 12 ve openSUSE artık

Azure'nün en son SLES ve openSUSE görüntülerinde DHCPv6 ile önceden yapılandırılmış olmuştur. Bu görüntüleri kullanırken, ek değişiklik gerekmez. Bir özel veya eski SUSE görüntüyü temel alarak bir VM'niz varsa, aşağıdaki adımları kullanın:

1. Dosyayı düzenleme `/etc/sysconfig/network/ifcfg-eth0` ve bu parametre değiştirme

        #BOOTPROTO='dhcp4'

    aşağıdaki değerle:

        BOOTPROTO='dhcp'

2. Aşağıdaki parametre ekleme `/etc/sysconfig/network/ifcfg-eth0`:

        DHCLIENT6_MODE='managed'

3. IPv6 adresi yenileme:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a>CoreOS

Azure son CoreOS görüntülerinde DHCPv6 ile önceden yapılandırılmış olmuştur. Bu görüntüleri kullanırken, ek değişiklik gerekmez. Bir özel veya eski CoreOS görüntüyü temel alarak bir VM'niz varsa, aşağıdaki adımları kullanın:

1. Dosyayı düzenleyin.`/etc/systemd/network/10_dhcp.network`

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. IPv6 adresi yenileme:

    ```bash
    sudo systemctl restart systemd-networkd
    ```
