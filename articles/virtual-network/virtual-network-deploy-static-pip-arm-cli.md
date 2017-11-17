---
title: "Bir statik genel IP adresi ile - Azure CLI bir VM oluşturma | Microsoft Docs"
description: "Azure komut satırı arabirimi (CLI) kullanarak bir statik genel IP adresi ile VM oluşturmayı öğrenin."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 55bc21b0-2a45-4943-a5e7-8d785d0d015c
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c50f685745a645b5fbe383a5fe4726faa0e36345
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-the-azure-cli"></a>Azure CLI kullanarak bir statik genel IP adresiyle bir VM oluşturma

> [!div class="op_single_selector"]
> * [Azure portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure CLI](virtual-network-deploy-static-pip-arm-cli.md)
> * [Şablon](virtual-network-deploy-static-pip-arm-template.md)
> * [PowerShell (Klasik)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Bu makalede, Klasik dağıtım modeli yerine en yeni dağıtımlar için Microsoft önerir Resource Manager dağıtım modelini kullanarak yer almaktadır.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name = "create"></a>VM oluşturma

Değerler "" izlediği adımları değişkenlerde senaryodan ayarlarla kaynakları oluşturun. Değerleri, ortamınız için uygun şekilde değiştirin.

1. Yükleme [Azure CLI 2.0](/cli/azure/install-az-cli2) , zaten yüklü değilse.
2. İçindeki adımları tamamlayarak Linux VM'ler için bir SSH ortak ve özel anahtar çifti oluşturma [Linux VM'ler için bir SSH ortak ve özel anahtar çifti oluşturma](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
3. Oturum açma komutunu bir komut kabuğu'ndan `az login`.
4. VM, bir Linux veya Mac bilgisayarda aşağıdaki komut dosyasını çalıştırarak oluşturun. Azure genel IP adresi, sanal ağ, ağ arabirimi ve VM kaynakları tüm aynı konumda bulunmalıdır. Kaynakların tümü aynı kaynak grubunda mevcut gerekmez ancak, aşağıdaki komut dosyasında yaparlar.

```bash
RgName="IaaSStory"
Location="westus"

# Create a resource group.

az group create \
--name $RgName \
--location $Location

# Create a public IP address resource with a static IP address using the --allocation-method Static option.
# If you do not specify this option, the address is allocated dynamically. The address is assigned to the
# resource from a pool of IP adresses unique to each Azure region. The DnsName must be unique within the
# Azure location it's created in. Download and view the file from https://www.microsoft.com/en-us/download/details.aspx?id=41653#
# that lists the ranges for each region.

PipName="PIPWEB1"
DnsName="iaasstoryws1"
az network public-ip create \
--name $PipName \
--resource-group $RgName \
--location $Location \
--allocation-method Static \
--dns-name $DnsName

# Create a virtual network with one subnet

VnetName="TestVNet"
VnetPrefix="192.168.0.0/16"
SubnetName="FrontEnd"
SubnetPrefix="192.168.1.0/24"
az network vnet create \
--name $VnetName \
--resource-group $RgName \
--location $Location \
--address-prefix $VnetPrefix \
--subnet-name $SubnetName \
--subnet-prefix $SubnetPrefix

# Create a network interface connected to the VNet with a static private IP address and associate the public IP address
# resource to the NIC.

NicName="NICWEB1"
PrivateIpAddress="192.168.1.101"
az network nic create \
--name $NicName \
--resource-group $RgName \
--location $Location \
--subnet $SubnetName \
--vnet-name $VnetName \
--private-ip-address $PrivateIpAddress \
--public-ip-address $PipName

# Create a new VM with the NIC

VmName="WEB1"

# Replace the value for the VmSize variable with a value from the
# https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-sizes article.
VmSize="Standard_DS1"

# Replace the value for the OsImage variable with a value for *urn* from the output returned by entering
# the `az vm image list` command. 

OsImage="credativ:Debian:8:latest"
Username='adminuser'

# Replace the following value with the path to your public key file.
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
# If creating a Windows VM, remove the previous line and you'll be prompted for the password you want to configure for the VM.
```

Bir VM oluşturmaya ek olarak, komut dosyası oluşturur:
- Tek bir premium disk varsayılan olarak yönetilen, ancak oluşturabileceğiniz disk türü için diğer seçeneğiniz vardır. Okuma [Azure CLI 2.0 kullanarak bir Linux VM oluşturma](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Ayrıntılar için makale.
- Sanal ağ, alt ağ, NIC ve ortak IP adresi kaynakları. Alternatif olarak, kullanabileceğiniz *varolan* sanal ağ, alt ağ, NIC veya ortak IP adresi kaynakları. Oluşturma ek kaynaklar yerine var olan ağ kaynaklarını kullanmayı öğrenmek için girin `az vm create -h`.

## <a name = "validate"></a>VM oluşturma ve genel IP adresi doğrula

1. Aşağıdaki komutu girin `az resource list --resouce-group IaaSStory --output table` komut dosyası tarafından oluşturulan kaynakların bir listesini görmek için. Döndürülen çıktısında beş kaynakları olmalıdır: ağ arabirimi, disk, genel IP adresi, sanal ağ ve bir sanal makine.
2. Aşağıdaki komutu girin `az network public-ip show --name PIPWEB1 --resource-group IaaSStory --output table`. Döndürülen çıkış değerini not edin **IPADDRESS** ve değerini **Publicıpallocationmethod** olan *statik*.
3. Aşağıdaki komutu yürütmeden önce <> kaldırmak, değiştirmek *kullanıcı adı* için kullanılan adla **kullanıcıadı** komut dosyası ve değiştirme değişkeni *IPADDRESS*ile **IPADDRESS** önceki adımdan. VM'e bağlanmak için aşağıdaki komutu çalıştırın: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`. 

## <a name= "clean-up"></a>VM ve ilişkili kaynakları Kaldır

Üretimde kullanmaz, bu alıştırmada oluşturulan kaynakları silmeniz önerilir. Sağlanan sürece ücretler, VM, genel IP adresi ve disk kaynakları uygulanır. Bu alıştırmada sırasında oluşturulan kaynakları kaldırmak için aşağıdaki adımları tamamlayın:

1. Kaynak grubunda olan kaynakları görüntülemek için Çalıştır `az resource list --resource-group IaaSStory` komutu.
2. Bu makalede betik tarafından oluşturulan kaynakları dışında kaynak grubunda hiç kaynak yok onaylayın. 
3. Bu alıştırmada oluşturulan tüm kaynakları silmek için çalıştırın `az group delete -n IaaSStory` komutu. Komut kaynak grubu ve içerdiği tüm kaynakları siler.

## <a name="next-steps"></a>Sonraki adımlar

Herhangi bir ağ trafiği için ve bu makalede oluşturulan VM akabilir. Bir NSG içinde için ve ağ arabirimi, alt ağ ya da her ikisini de akabilir trafiğini sınırlandırmak gelen ve giden kurallar tanımlayabilirsiniz. Nsg'ler hakkında daha fazla bilgi için okuma [NSG genel bakış](virtual-networks-nsg.md) makalesi.