---
title: Azure CLI kullanarak birden çok IP adresine sahip VM
titlesuffix: Azure Virtual Network
description: Azure komut satırı arabirimi (CLI) kullanarak bir sanal makineye birden çok IP adresi atama hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: jimdial
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/17/2016
ms.author: jimdial
ms.openlocfilehash: b693500e785d41b2ad3339e26dd9fd3505891bc0
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58648342"
---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-the-azure-cli"></a>Azure CLI kullanarak sanal makineler için birden çok IP adresi atama

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

Bu makalede, Azure CLI kullanarak Azure Resource Manager dağıtım modeli sanal makine (VM) oluşturma açıklanmaktadır. Birden çok IP adresi Klasik dağıtım modeliyle oluşturulan kaynaklara atanamaz. Azure dağıtım modelleri hakkında daha fazla bilgi edinmek için [dağıtım modellerini anlama](../resource-manager-deployment-model.md) makalesi.

[!INCLUDE [virtual-network-multiple-ip-addresses-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name = "create"></a>Birden çok IP adresi ile VM oluşturma

Aşağıdaki adımları senaryoda açıklanan şekilde birden çok IP adresi ile bir örnek sanal makine oluşturma açıklanmaktadır. Değişken değerleri Değiştir "" ve uygulamanız için gereken IP adresi türleri. 

1. Yükleme [Azure CLI](/cli/azure/install-azure-cli) , zaten yüklü değilse.
2. İçindeki adımları tamamlayarak Linux VM'ler için SSH ortak ve özel anahtar çifti oluşturma [Linux VM'ler için SSH ortak ve özel anahtar çifti oluşturma](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
3. Oturum açma komutu ile bir komut kabuğu'ndan `az login` ve kullanmakta olduğunuz aboneliği seçin.
4. VM, bir Linux veya Mac bilgisayarda aşağıdaki komut dosyasını çalıştırarak oluşturun. Betik bağlı iki NIC içeren bir kaynak grubu, bir sanal ağ (VNet), bir NIC üç IP yapılandırmaları olan ve bir VM oluşturur. NIC, ortak IP adresi, sanal ağ ve VM kaynaklarının tümü aynı konum ve abonelikte bulunması gerekir. Kaynakların tümü aynı kaynak grubunda olması gerekmez ancak bunlar aşağıdaki betikte yapın.

```bash
    
#!/bin/sh
    
RgName="myResourceGroup"
Location="westcentralus"
az group create --name $RgName --location $Location
    
# Create a public IP address resource with a static IP address using the `--allocation-method Static` option. If you
# do not specify this option, the address is allocated dynamically. The address is assigned to the resource from a pool
# of IP addresses unique to each Azure region. Download and view the file from
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

az network nic ip-config create \
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
# https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-sizes article. The script fails if the VM size
# is not supported in the location you select. Run the `azure vm sizes --location eastcentralus` command to get a full list
# of VMs in US West Central, for example.

VmSize="Standard_DS1"

# Replace the value for the OsImage variable value with a value for *urn* from the output returned by entering the
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

- Varsayılan olarak tek bir premium yönetilen disk, ancak diğer seçenekler için oluşturabileceğiniz disk türü vardır. Okuma [Azure CLI kullanarak bir Linux VM oluşturma](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) makale Ayrıntılar için.
- Bir sanal ağ ile bir alt ağ ve iki genel IP adresi. Alternatif olarak, *mevcut* sanal ağ, alt ağ, NIC veya genel IP adresi kaynakları. Ek kaynak oluşturmaya yerine var olan ağ kaynaklarını kullanmayı öğrenmek için girin `az vm create -h`.

Genel IP adreslerinin nominal bir ücreti vardır. IP adresi fiyatlandırması hakkında daha fazla bilgi edinmek için [IP adresi fiyatlandırması](https://azure.microsoft.com/pricing/details/ip-addresses) sayfası. Bir abonelikte kullanılan genel IP adresleri sayısına bir sınır yoktur. Sınırlar hakkında daha fazla bilgi için [Azure limitleri](../azure-subscription-service-limits.md#networking-limits) makalesini okuyun.

VM oluşturulduktan sonra girin `az network nic show --name MyNic1 --resource-group myResourceGroup` NIC yapılandırmasını görüntülemek için komutu. Girin `az network nic ip-config list --nic-name MyNic1 --resource-group myResourceGroup --output table` ilişkili NIC IP yapılandırmaları bir listesini görüntülemek için

İşletim sisteminiz için adımları tamamlayarak VM işletim sistemine özel IP adresleri ekleme [ekleme IP adresleri için bir VM işletim sistemi](#os-config) bu makalenin.

## <a name="add"></a>Bir VM'ye IP adresleri ekleme

Aşağıdaki adımları izleyerek, ek özel ve genel IP adresleri için mevcut bir Azure ağ arabirimi ekleyebilirsiniz. Örnekler sınayabilmesi [senaryo](#scenario) bu makalede açıklanan.

1. Bir komut kabuğunu açın ve içinde tek bir oturumda bu bölümün kalan adımları tamamlayın. Azure CLI'yı yüklü ve yapılandırılmış yoksa bölümünde bulunan adımları tamamladığınızdan [Azure CLI yükleme](/cli/azure/install-az-cli2?toc=%2fazure%2fvirtual-network%2ftoc.json) makale ve Azure oturum açma hesabı ile `az-login` komutu.

2. Gereksinimlerinize göre aşağıdaki bölümlerden birindeki adımları tamamlayın:

    **Özel bir IP adresi Ekle**
    
    Özel bir IP adresi bir NIC'ye eklemek için aşağıdaki komutu kullanarak IP yapılandırması oluşturmanız gerekir. Statik IP adresi alt ağı için kullanılmayan bir adresi olmalıdır.

    ```bash
    az network nic ip-config create \
    --resource-group myResourceGroup \
    --nic-name myNic1 \
    --private-ip-address 10.0.0.7 \
    --name IPConfig-4
    ```
    
    Benzersiz yapılandırma adları ve özel IP adresleri (statik IP adresi ile yapılandırmaları) kullanarak gereksinim duyduğunuz kadar çok yapılandırmalarını oluşturun.

    **Genel IP adresi ekleme**
    
    Genel bir IP adresi, yeni bir IP yapılandırması veya mevcut bir IP yapılandırması için ilişkilendirerek eklenir. Gereksinim duyduğunuz kadar aşağıdaki bölümlerde birindeki adımları tamamlayın.

    Genel IP adreslerinin nominal bir ücreti vardır. IP adresi fiyatlandırması hakkında daha fazla bilgi edinmek için [IP adresi fiyatlandırması](https://azure.microsoft.com/pricing/details/ip-addresses) sayfası. Bir abonelikte kullanılan genel IP adresleri sayısına bir sınır yoktur. Sınırlar hakkında daha fazla bilgi için [Azure limitleri](../azure-subscription-service-limits.md#networking-limits) makalesini okuyun.

    - **Yeni bir IP yapılandırması kaynağı ilişkilendirin**
    
        Yeni bir IP yapılandırmasında genel IP adresi eklediğinizde, tüm IP yapılandırmaları özel bir IP adresi olmalıdır çünkü özel bir IP adresi de eklemeniz gerekir. Var olan bir genel IP adresi kaynağı ekleyin veya yeni bir tane oluşturun. Yeni bir tane oluşturmak için aşağıdaki komutu girin:
    
        ```bash
        az network public-ip create \
        --resource-group myResourceGroup \
        --location westcentralus \
        --name myPublicIP3 \
        --dns-name mypublicdns3
        ```

        Statik özel IP adresi ve ilişkili ile yeni bir IP yapılandırması oluşturmak için *myPublicIP3* genel IP adresi kaynağı, aşağıdaki komutu girin:

        ```bash
        az network nic ip-config create \
        --resource-group myResourceGroup \
        --nic-name myNic1 \
        --name IPConfig-5 \
        --private-ip-address 10.0.0.8
        --public-ip-address myPublicIP3
        ```

    - **Mevcut bir IP yapılandırmasına kaynak ilişkilendirmeniz** yalnızca bir genel IP adresi kaynağı zaten ilişkili olmayan bir IP yapılandırması için de ilişkilendirilebilir. Aşağıdaki komutu girerek bir IP yapılandırması ilişkili bir genel IP adresi olup olmadığını belirleyebilirsiniz:

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

        Bu yana **Publicıpaddressıd** sütunu için *IpConfig 3* olan çıkış boş, hiçbir genel IP adresi kaynağı şu anda ilişkili olduğundan. Var olan bir genel IP adresi kaynağı IpConfig-3'e ekleyebilir veya oluşturmak için aşağıdaki komutu girin:

        ```bash
        az network public-ip create \
        --resource-group  myResourceGroup
        --location westcentralus \
        --name myPublicIP3 \
        --dns-name mypublicdns3 \
        --allocation-method Static
        ```
    
        Genel IP adresi kaynağı adı mevcut IP yapılandırmasına ilişkilendirmek için aşağıdaki komutu girin *IPConfig 3*:
    
        ```bash
        az network nic ip-config update \
        --resource-group myResourceGroup \
        --nic-name myNic1 \
        --name IPConfig-3 \
        --public-ip myPublicIP3
        ```

3. Genel IP adresi kaynağı NIC için aşağıdaki komutu girerek atanan kimlikleri ve özel IP adreslerini görüntüleyin:

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
    

4. Eklediğiniz VM işletim sisteminde NIC'ndaki yönergeleri takip ederek özel IP adresleri ekleme [ekleme IP adresleri için bir VM işletim sistemi](#os-config) bu makalenin. Genel IP adreslerine, işletim sistemine eklemeyin.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
