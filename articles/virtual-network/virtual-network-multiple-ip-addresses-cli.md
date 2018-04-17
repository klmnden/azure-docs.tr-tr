---
title: VM ile birden çok IP adreslerini Azure CLI kullanarak | Microsoft Docs
description: Azure komut satırı arabirimi (CLI) kullanarak bir sanal makine için birden çok IP adresi atama hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/17/2016
ms.author: jimdial
ms.openlocfilehash: a1e78f7fa892586385e1dbd186125630ee1fb307
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-the-azure-cli"></a>Azure CLI kullanarak sanal makineleri için birden çok IP adresi atayın

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

Bu makalede, Azure CLI kullanarak Azure Resource Manager dağıtım modeli sanal makine (VM) oluşturma açıklanmaktadır. Birden çok IP adresi Klasik dağıtım modeli aracılığıyla oluşturulan kaynakları atanamaz. Azure dağıtım modelleri hakkında daha fazla bilgi için okuma [dağıtım modellerini anlama](../resource-manager-deployment-model.md) makalesi.

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name = "create"></a>Birden çok IP adresiyle bir VM oluşturma

Adımları birden çok IP adresiyle bir örnek sanal makine oluşturmak senaryosunda açıklandığı şekilde açıklanmaktadır. Değişken değerleri değiştirmek "" ve gerektiğinde, uygulamanız için IP adresi türleri. 

1. Yükleme [Azure CLI 2.0](/cli/azure/install-az-cli2) , zaten yüklü değilse.
2. İçindeki adımları tamamlayarak Linux VM'ler için bir SSH ortak ve özel anahtar çifti oluşturma [Linux VM'ler için bir SSH ortak ve özel anahtar çifti oluşturma](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
3. Oturum açma komutunu bir komut kabuğu'ndan `az login` ve kullanmakta olduğunuz abonelik seçin.
4. VM, bir Linux veya Mac bilgisayarda aşağıdaki komut dosyasını çalıştırarak oluşturun. Komut dosyası, ona iliştirilmiş iki NIC içeren bir kaynak grubu, bir sanal ağ (VNet), bir NIC üç IP yapılandırmaları olan ve bir VM oluşturur. NIC, ortak IP adresi, sanal ağ ve VM kaynakları tüm aynı konumu ve abonelik bulunmalıdır. Kaynakların tümü aynı kaynak grubunda mevcut gerekmez ancak, aşağıdaki komut dosyasında yaparlar.

```bash
    
#!/bin/sh
    
RgName="myResourceGroup"
Location="westcentralus"
az group create --name $RgName --location $Location
    
# Create a public IP address resource with a static IP address using the `--allocation-method Static` option. If you
# do not specify this option, the address is allocated dynamically. The address is assigned to the resource from a pool
# of IP adresses unique to each Azure region. Download and view the file from
# https://www.microsoft.com/en-us/download/details.aspx?id=41653 that lists the ranges for each region.

PipName="myPublicIP"

# This name must be unique within an Azure location.
DnsName="myDNSName"

az network public-ip create \
--name $PipName \
--resource-group $RgName \
--location $Location \
--dns-name $DnsName\
--allocation-method Static

# Create a virtual network with one subnet

VnetName="myVnet"
VnetPrefix="10.0.0.0/16"
VnetSubnetName="mySubnet"
VnetSubnetPrefix="10.0.0.0/24"

az network vnet create \
--name $VnetName \
--resource-group $RgName \
--location $Location \
--address-prefix $VnetPrefix \
--subnet-name $VnetSubnetName \
--subnet-prefix $VnetSubnetPrefix

# Create a network interface connected to the subnet and associate the public IP address to it. Azure will create the
# first IP configuration with a static private IP address and will associate the public IP address resource to it.

NicName="MyNic1"
az network nic create \
--name $NicName \
--resource-group $RgName \
--location $Location \
--subnet $VnetSubnet1Name \
--private-ip-address 10.0.0.4
--vnet-name $VnetName \
--public-ip-address $PipName
    
# Create a second public IP address, a second IP configuration, and associate it to the NIC. This configuration has a
# static public IP address and a static private IP address.

az network public-ip create \
--resource-group $RgName \
--location $Location \
--name myPublicIP2 \
--dns-name mypublicdns2 \
--allocation-method Static

az network nic ip-config create \
--resource-group $RgName \
--nic-name $NicName \
--name IPConfig-2 \
--private-ip-address 10.0.0.5 \
--public-ip-name myPublicIP2

# Create a third IP configuration, and associate it to the NIC. This configuration has  static private IP address and   # no public IP address.

azure network nic ip-config create \
--resource-group $RgName \
--nic-name $NicName \
--private-ip-address 10.0.0.6 \
--name IPConfig-3

# Note: Though this article assigns all IP configurations to a single NIC, you can also assign multiple IP configurations
# to any NIC in a VM. To learn how to create a VM with multiple NICs, read the Create a VM with multiple NICs 
# article: https://docs.microsoft.com/azure/virtual-network/virtual-network-deploy-multinic-arm-cli.

# Create a VM and attach the NIC.

VmName="myVm"

# Replace the value for the following **VmSize** variable with a value from the
# https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-sizes rticle. The script fails if the VM size
# is not supported in the location you select. Run the `azure vm sizes --location estcentralus` command to get a full list
# of VMs in US West Central, for example.

VmSize="Standard_DS1"

# Replace the value for the OsImage variable value with a value for *urn* from the utput returned by entering the
# `az vm image list` command.

OsImage="credativ:Debian:8:latest"

Username="adminuser"

# Replace the following value with the path to your public key file. If you're creating a Windows VM, remove the following
# line and you'll be prompted for the password you want to configure for the VM.

SshKeyValue="~/.ssh/id_rsa.pub"

az vm create \
--name $VmName \
--resource-group $RgName \
--image $OsImage \
--location $Location \
--size $VmSize \
--nics $NicName \
--admin-username $Username \
--ssh-key-value $SshKeyValue
```

3 IP yapılandırmaları olan bir NIC ile VM oluşturmaya ek olarak, komut dosyası oluşturur:

- Tek bir premium disk varsayılan olarak yönetilen, ancak oluşturabileceğiniz disk türü için diğer seçeneğiniz vardır. Okuma [Azure CLI 2.0 kullanarak bir Linux VM oluşturma](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Ayrıntılar için makale.
- Bir alt ağı ve iki genel IP adresine sahip bir sanal ağ. Alternatif olarak, kullanabileceğiniz *varolan* sanal ağ, alt ağ, NIC veya ortak IP adresi kaynakları. Oluşturma ek kaynaklar yerine var olan ağ kaynaklarını kullanmayı öğrenmek için girin `az vm create -h`.

Nominal bir ücret ortak IP adresine sahip. IP adresi fiyatlandırma hakkında daha fazla bilgi için okuma [IP adresi fiyatlandırma](https://azure.microsoft.com/pricing/details/ip-addresses) sayfası. Bir abonelikte kullanılabilir genel IP adresleri sayısına bir sınır yoktur. Sınırlar hakkında daha fazla bilgi için [Azure limitleri](../azure-subscription-service-limits.md#networking-limits) makalesini okuyun.

VM oluşturulduktan sonra girin `az network nic show --name MyNic1 --resource-group myResourceGroup` NIC yapılandırmasını görüntülemek için komutu. Girin `az network nic ip-config list --nic-name MyNic1 --resource-group myResourceGroup --output table` NIC'ye ilişkili IP yapılandırmaları bir listesini görüntülemek için

İşletim sisteminiz için adımları tamamlayarak VM işletim sistemine özel IP adresleri ekleme [eklemek IP adresleri bir VM işletim sistemine](#os-config) bu makalenin.

## <a name="add"></a>Bir VM için IP adreslerini ekleyin

Adımları tamamlayarak var olan bir Azure ağ arabirimine ek özel ve genel IP adresleri ekleyebilirsiniz. Temel örnekler yapı [senaryo](#Scenario) bu makalede açıklanan.

1. Bir komut kabuğu'nu açın ve tek bir oturum içinde bu bölümdeki kalan adımları tamamlayın. Azure CLI yüklenmiş ve yapılandırılmış zaten sahip değilseniz, bölümündeki adımları tamamlamanız [Azure CLI 2.0 yüklemesi](/cli/azure/install-az-cli2?toc=%2fazure%2fvirtual-network%2ftoc.json) makale ve Azure oturum açma hesabı ile `az-login` komutu.

2. Gereksinimlerinize göre aşağıdaki bölümlerden birindeki adımları tamamlayın:

    **Özel bir IP adresi Ekle**
    
    Bir NIC için özel bir IP adresi eklemek için aşağıdaki komutu kullanarak IP yapılandırması oluşturmanız gerekir. Statik IP adresi alt ağ için kullanılmayan bir adres olmalıdır.

    ```bash
    az network nic ip-config create \
    --resource-group myResourceGroup \
    --nic-name myNic1 \
    --private-ip-address 10.0.0.7 \
    --name IPConfig-4
    ```
    
    Benzersiz yapılandırma adları ve özel IP adresleri (yapılandırmaları statik IP adresleriyle) kullanarak gereksinim duyduğunuz kadar çok yapılandırmaları oluşturun.

    **Bir ortak IP adresi Ekle**
    
    Bir ortak IP adresi, yeni bir IP yapılandırması veya var olan IP yapılandırmasını ilişkilendirerek eklenir. Gereksinim duyduğunuz kadar aşağıdaki bölümlerde birindeki adımları tamamlayın.

    Nominal bir ücret ortak IP adresine sahip. IP adresi fiyatlandırma hakkında daha fazla bilgi için okuma [IP adresi fiyatlandırma](https://azure.microsoft.com/pricing/details/ip-addresses) sayfası. Bir abonelikte kullanılabilir genel IP adresleri sayısına bir sınır yoktur. Sınırlar hakkında daha fazla bilgi için [Azure limitleri](../azure-subscription-service-limits.md#networking-limits) makalesini okuyun.

    - **Yeni bir IP yapılandırması için kaynak ilişkilendirme**
    
        Yeni bir IP yapılandırmasında bir ortak IP adresi eklediğinizde, tüm IP yapılandırmalarının özel bir IP adresi olması gerektiği için özel bir IP adresi eklemelisiniz. Varolan bir ortak IP adresi kaynağı ekleyin veya yeni bir tane oluşturun. Yeni bir tane oluşturmak için aşağıdaki komutu girin:
    
        ```bash
        az network public-ip create \
        --resource-group myResourceGroup \
        --location westcentralus \
        --name myPublicIP3 \
        --dns-name mypublicdns3
        ```

        Özel bir statik IP adresi ile ilişkili yeni bir IP yapılandırması oluşturmak için *myPublicIP3* genel IP adresi kaynak, aşağıdaki komutu girin:

        ```bash
        az network nic ip-config create \
        --resource-group myResourceGroup \
        --nic-name myNic1 \
        --name IPConfig-5 \
        --private-ip-address 10.0.0.8
        --public-ip-address myPublicIP3
        ```

    - **Var olan IP yapılandırmasını kaynağa ilişkilendirmek** genel bir IP adresi kaynağı yalnızca zaten ilişkili olmayan bir IP yapılandırmasıyla ilişkilendirilmiş olabilir. Aşağıdaki komutu girerek bir IP yapılandırması ilişkili bir ortak IP adresi olup olmadığını belirleyebilirsiniz:

        ```bash
        az network nic ip-config list \
        --resource-group myResourceGroup \
        --nic-name myNic1 \
        --query "[?provisioningState=='Succeeded'].{ Name: name, PublicIpAddressId: publicIpAddress.id }" --output table
        ```

        Çıkış döndürdü:
    
            Name        PublicIpAddressId
            
            ipconfig1   /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP1
            IPConfig-2  /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP2
            IPConfig-3

        Bu yana **PublicIpAddressId** sütunu için *IpConfig 3* olan çıkışı, hiçbir ortak IP adresi kaynağı ilişkili şu anda boştur. Mevcut bir ortak IP adresi kaynağı IpConfig-3'e ekleyin veya oluşturmak için aşağıdaki komutu girin:

        ```bash
        az network public-ip create \
        --resource-group  myResourceGroup
        --location westcentralus \
        --name myPublicIP3 \
        --dns-name mypublicdns3 \
        --allocation-method Static
        ```
    
        Adlı varolan IP yapılandırması için genel IP adresi kaynağı ilişkilendirmek için aşağıdaki komutu girin *IPConfig 3*:
    
        ```bash
        az network nic ip-config update \
        --resource-group myResourceGroup \
        --nic-name myNic1 \
        --name IPConfig-3 \
        --public-ip myPublicIP3
        ```

3. Özel IP adreslerini ve ortak IP adresi kaynağı aşağıdaki komutu girerek NIC'ye atanan kimlikler görüntüleyin:

    ```bash
    az network nic ip-config list \
    --resource-group myResourceGroup \
    --nic-name myNic1 \
    --query "[?provisioningState=='Succeeded'].{ Name: name, PrivateIpAddress: privateIpAddress, PrivateIpAllocationMethod: privateIpAllocationMethod, PublicIpAddressId: publicIpAddress.id }" --output table
    ```

    Çıkış döndürdü: <br>
    
        Name        PrivateIpAddress    PrivateIpAllocationMethod   PublicIpAddressId
        
        ipconfig1   10.0.0.4            Static                      /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP1
        IPConfig-2  10.0.0.5            Static                      /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP2
        IPConfig-3  10.0.0.6            Static                      /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP3
    

4. Eklediğiniz NIC VM işletim sistemine'ndaki yönergeleri izleyerek özel IP adresleri ekleme [eklemek IP adresleri bir VM işletim sistemine](#os-config) bu makalenin. Genel IP adreslerine işletim sistemine eklemeyin.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
