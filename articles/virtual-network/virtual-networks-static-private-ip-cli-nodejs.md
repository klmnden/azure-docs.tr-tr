---
title: "Özel IP adresleri VM'ler - Azure CLI 1.0 için yapılandırma | Microsoft Docs"
description: "Azure komut satırı arabirimi (CLI) 1.0 kullanarak sanal makineleri için özel IP adresleri yapılandırmayı öğrenin."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 40b03a1a-ea00-454c-b716-7574cea49ac0
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9179319095c31d5eb454860e173ffa7c65fc9f73
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-cli-10"></a>Azure CLI 1.0 kullanarak bir sanal makine için özel IP adreslerini yapılandırın


## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri 

Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz: 

- [Azure CLI 1.0](#how-to-specify-a-static-private-ip-address-when-creating-a-vm) – bizim CLI Klasik ve kaynak yönetimi dağıtım modeline (Bu makalede)
- [Azure CLI 2.0](virtual-networks-static-private-ip-arm-cli.md) -bizim nesil CLI kaynak yönetimi dağıtım modeli için 

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Bu makalede Resource Manager dağıtım modeli anlatılmaktadır. Ayrıca [Klasik dağıtım modelinde statik özel IP adresi yönetmek](virtual-networks-static-private-ip-classic-cli.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Aşağıdaki örnek Azure CLI komutları önceden oluşturulmuş basit bir ortam bekler. Bu belgede gösterildiği komutları çalıştırmak istiyorsanız, önce açıklanan test ortamı oluşturmak [vnet oluşturma](virtual-networks-create-vnet-arm-cli.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Bir VM oluşturulurken özel bir statik IP adresi belirtme
Adlı bir VM oluşturmak için *DNS01* içinde *ön uç* adlı bir sanal ağ alt ağı *TestVNet* özel bir statik IP *192.168.1.101*, aşağıdaki adımları izleyin:

1. Hiç Azure CLI kullanmadıysanız bkz. [Azure CLI’yi Yükleme ve Yapılandırma](../cli-install-nodejs.md); sonra da, Azure hesabınızı ve aboneliğinizi seçtiğiniz noktaya kadar yönergeleri uygulayın.
2. Resource Manager moduna geçmek için **azure config mode** komutunu aşağıda gösterildiği gibi çalıştırın.
   
        azure config mode arm
   
    Beklenen çıktı:
   
        info:    New mode is arm
3. Çalıştırma **azure ağı ve genel IP oluşturun** VM için bir ortak IP oluşturmak için. Çıktıdan sonra gösterilen listede, kullanılan parametreler açıklanmaktadır.
   
        azure network public-ip create -g TestRG -n TestPIP -l centralus
   
    Beklenen çıktı:
   
        info:    Executing command network public-ip create
        + Looking up the public ip "TestPIP"
        + Creating public ip address "TestPIP"
        + Looking up the public ip "TestPIP"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP
        data:    Name                            : TestPIP
        data:    Type                            : Microsoft.Network/publicIPAddresses
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Allocation method               : Dynamic
        data:    Idle timeout                    : 4
        info:    network public-ip create command OK
   
   * **-g (veya --resource-group)**. Genel IP oluşturulduğu kaynak grubunun adı.
   * **-n (veya --name)**. Genel IP adı.
   * **-l (veya --location)**. Genel IP oluşturulacağı azure bölgesi. Bizim senaryomuz için bu *centralus* ’tur.
4. Çalıştırma **azure ağı NIC grubu oluşturmak** statik özel IP ile NIC grubu oluşturmak için komutu. Çıktıdan sonra gösterilen listede, kullanılan parametreler açıklanmaktadır.
   
        azure network nic create -g TestRG -n TestNIC -l centralus -a 192.168.1.101 -m TestVNet -k FrontEnd
   
    Beklenen çıktı:
   
        + Looking up the network interface "TestNIC"
        + Looking up the subnet "FrontEnd"
        + Creating network interface "TestNIC"
        + Looking up the network interface "TestNIC"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
        data:    Name                            : TestNIC
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.1.101
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK
   
   * **-a (veya--özel IP adresi)**. NIC özel statik IP adresi
   * **-m (veya--alt sanal ağ adı)**. NIC oluşturulacağı Vnet'in adı.
   * **-k (veya--alt ağ adı)**. NIC oluşturulacağı alt ağ adı.
5. Çalıştırma **azure vm oluşturma** ortak IP yukarıda oluşturduğunuz NIC ile VM oluşturmak için komutu. Çıktıdan sonra gösterilen listede, kullanılan parametreler açıklanmaktadır.
   
        azure vm create -g TestRG -n DNS01 -l centralus -y Windows -f TestNIC -i TestPIP -F TestVNet -j FrontEnd -o vnetstorage -q bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 -u adminuser -p AdminP@ssw0rd
   
    Beklenen çıktı:
   
        info:    Executing command vm create
        + Looking up the VM "DNS01"
        info:    Using the VM Size "Standard_A1"
        warn:    The image "bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2" will be used for VM
        info:    The [OS, Data] Disk or image configuration requires storage account
        + Looking up the storage account vnetstorage
        + Looking up the NIC "TestNIC"
        info:    Found an existing NIC "TestNIC"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd" in the NIC "TestNIC"
        info:    Found public ip parameters, trying to setup PublicIP profile
        + Looking up the public ip "TestPIP"
        info:    Found an existing PublicIP "TestPIP"
        info:    Configuring identified NIC IP configuration with PublicIP "TestPIP"
        + Updating NIC "TestNIC"
        + Looking up the NIC "TestNIC"
        + Creating VM "DNS01"
        info:    vm create command OK
   
   * **-y (veya--işletim sistemi türü)**. Ya da VM için işletim sistemi türü *Windows* veya *Linux*.
   * **-f (veya--NIC-name)**. NIC VM adını kullanır.
   * **-i (veya--genel IP adı)**. Genel IP VM adını kullanır.
   * **-F (veya--vnet-ad)**. VM oluşturulacağı Vnet'in adı.
   * **-j (veya--vnet alt ağ adı)**. VM oluşturulacağı alt ağ adı.

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Bir VM için özel statik IP adresi bilgilerini alma
Komut dosyası yukarıdaki VM oluşturulan için statik özel IP adresi bilgilerini görüntülemek için Azure CLI ve aşağıdaki komutu çalıştırın değerlerini gözlemlemek *özel IP ayırma yöntemi* ve *özel IP adresi*:

    azure vm show -g TestRG -n DNS01

Beklenen çıktı:

    info:    Executing command vm show
    + Looking up the VM "DNS01"
    + Looking up the NIC "TestNIC"
    + Looking up the public ip "TestPIP
    data:    Id                              :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
    data:    ProvisioningState               :Succeeded
    data:    Name                            :DNS01
    data:    Location                        :centralus
    data:    Type                            :Microsoft.Compute/virtualMachines
    data:
    data:    Hardware Profile:
    data:      Size                          :Standard_A1
    data:
    data:    Storage Profile:
    data:      Source image:
    data:        Id                          :/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/services/images/bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
    data:
    data:      OS Disk:
    data:        OSType                      :Windows
    data:        Name                        :cli08d7bd987a0112a8-os-1441774961355
    data:        Caching                     :ReadWrite
    data:        CreateOption                :FromImage
    data:        Vhd:
    data:          Uri                       :https://vnetstorage2.blob.core.windows.net/vhds/cli08d7bd987a0112a8-os-1441774961355vhd
    data:
    data:    OS Profile:
    data:      Computer Name                 :DNS01
    data:      User Name                     :adminuser
    data:      Windows Configuration:
    data:        Provision VM Agent          :true
    data:        Enable automatic updates    :true
    data:
    data:    Network Profile:
    data:      Network Interfaces:
    data:        Network Interface #1:
    data:          Id                        :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
    data:          Primary                   :true
    data:          MAC Address               :00-0D-3A-90-1A-A8
    data:          Provisioning State        :Succeeded
    data:          Name                      :TestNIC
    data:          Location                  :centralus
    data:            Private IP alloc-method :Static
    data:            Private IP address      :192.168.1.101
    data:            Public IP address       :40.122.213.159
    info:    vm show command OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Özel bir statik IP adresi bir VM'den kaldırma
Özel bir statik IP adresi, Azure CLI için Resource Manager içinde nıc'den kaldıramazsınız. Dinamik IP kullanan yeni bir NIC oluşturun, önceki NIC sanal makineden kaldırın ve ardından yeni NIC VM eklemeniz gerekir. Yukarıdaki komutlarda kullanılan VM int eh NIC değiştirmek için aşağıdaki adımları izleyin.

1. Çalıştırma **azure ağı NIC grubu oluşturmak** dinamik IP ayırma kullanarak yeni bir NIC grubu oluşturmak için komutu. Nasıl IP adresini bu süreyi belirtin gerekmez dikkat edin.
   
        azure network nic create -g TestRG -n TestNIC2 -l centralus -m TestVNet -k FrontEnd
   
    Beklenen çıktı:
   
        info:    Executing command network nic create
        + Looking up the network interface "TestNIC2"
        + Looking up the subnet "FrontEnd"
        + Creating network interface "TestNIC2"
        + Looking up the network interface "TestNIC2"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
        data:    Name                            : TestNIC2
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.1.6
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK
2. Çalıştırma **azure vm kümesi** VM tarafından kullanılan NIC değiştirmek için komutu.
   
        azure vm set -g TestRG -n DNS01 -N TestNIC2
   
    Beklenen çıktı:
   
        info:    Executing command vm set
        + Looking up the VM "DNS01"
        + Looking up the NIC "TestNIC2"
        + Updating VM "DNS01"
        info:    vm set command OK
3. İstediyseniz, çalıştırmak **azure ağı NIC silme** eski NIC silmek için
   
        azure network nic delete -g TestRG -n TestNIC --quiet
   
    Beklenen çıktı:
   
        info:    Executing command network nic delete
        + Looking up the network interface "TestNIC"
        + Deleting network interface "TestNIC"
        info:    network nic delete command OK

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Mevcut bir VM'yi özel bir statik IP adresi ekleme
Yukarıdaki komut dosyası kullanılarak oluşturulan VM tarafından kullanılan NIC özel bir statik IP adresi eklemek için aşağıdaki komutu çalıştırın:

    azure network nic set -g TestRG -n TestNIC2 -a 192.168.1.101

Beklenen çıktı:

    info:    Executing command network nic set
    + Looking up the network interface "TestNIC2"
    + Updating network interface "TestNIC2"
    + Looking up the network interface "TestNIC2"
    data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
    data:    Name                            : TestNIC2
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : centralus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-90-29-25
    data:    Enable IP forwarding            : false
    data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
    data:    IP configurations:
    data:      Name                          : NIC-config
    data:      Provisioning state            : Succeeded
    data:      Private IP address            : 192.168.1.101
    data:      Private IP Allocation Method  : Static
    data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [ayrılmış genel IP](virtual-networks-reserved-public-ip.md) adresleri.
* Hakkında bilgi edinin [örnek düzeyinde ortak IP (ILPIP)](virtual-networks-instance-level-public-ip.md) adresleri.
* Başvurun [ayrılmış IP REST API'leri](https://msdn.microsoft.com/library/azure/dn722420.aspx).

