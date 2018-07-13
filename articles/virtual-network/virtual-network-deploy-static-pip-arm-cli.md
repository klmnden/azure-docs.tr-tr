---
title: Bir statik genel IP adresiyle - Azure CLI VM oluşturma | Microsoft Docs
description: Azure komut satırı arabirimi (CLI) kullanarak statik genel IP adresiyle VM oluşturma konusunda bilgi edinin.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
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
ms.openlocfilehash: bd44971162a79e53b731c5c89316f14e8bb0a1a6
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38651968"
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-the-azure-cli"></a>Azure CLI kullanarak bir statik genel IP adresiyle VM oluşturma

> [!div class="op_single_selector"]
> * [Azure portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure CLI](virtual-network-deploy-static-pip-arm-cli.md)
> * [PowerShell (Klasik)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Bu makale Klasik dağıtım modeli yerine en yeni dağıtımlar için Microsoft'un önerdiği Resource Manager dağıtım modelini incelemektedir.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name = "create"></a>VM oluşturma

Değerler "" adımları değişkenlerinde senaryosundaki ayarlarla kaynakları oluşturun. Ortamınız için uygun değerleri değiştirin.

1. Yükleme [Azure CLI 2.0](/cli/azure/install-az-cli2) , zaten yüklü değilse.
2. İçindeki adımları tamamlayarak Linux VM'ler için SSH ortak ve özel anahtar çifti oluşturma [Linux VM'ler için SSH ortak ve özel anahtar çifti oluşturma](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
3. Oturum açma komutu ile bir komut kabuğu'ndan `az login`.
4. VM, bir Linux veya Mac bilgisayarda aşağıdaki komut dosyasını çalıştırarak oluşturun. Azure genel IP adresi, sanal ağ, ağ arabirimi ve VM kaynaklarının tümünü aynı konumda bulunmalıdır. Kaynakların tümü aynı kaynak grubunda olması gerekmez ancak bunlar aşağıdaki betikte yapın.

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

Bir sanal makine oluşturmaya ek olarak, komut dosyası oluşturur:
- Varsayılan olarak tek bir premium yönetilen disk, ancak diğer seçenekler için oluşturabileceğiniz disk türü vardır. Okuma [Azure CLI 2.0 kullanarak bir Linux VM oluşturma](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) makale Ayrıntılar için.
- Sanal ağ, alt ağ, NIC ve genel IP adresi kaynakları. Alternatif olarak, *mevcut* sanal ağ, alt ağ, NIC veya genel IP adresi kaynakları. Ek kaynak oluşturmaya yerine var olan ağ kaynaklarını kullanmayı öğrenmek için girin `az vm create -h`.

## <a name = "validate"></a>VM oluşturma ve genel IP adresini doğrula

1. Komutu girdikten `az resource list --resouce-group IaaSStory --output table` betiği tarafından oluşturulan kaynakların bir listesini görmek için. Döndürülen çıktısında beş kaynaklar olmalıdır: ağ arabirimi, disk, genel IP adresi, sanal ağ ve sanal makine.
2. Komutu girdikten `az network public-ip show --name PIPWEB1 --resource-group IaaSStory --output table`. Döndürülen çıktısında değerini not edin **IPADDRESS** ve değerini **Publicıpallocationmethod** olduğu *statik*.
3. Aşağıdaki komutu yürütmeden önce çatal karakterini kaldırın ve değiştirin *kullanıcıadı* için kullanılan ad ile **kullanıcı adı** betik ve değiştirme değişkeni *IPADDRESS*ile **IPADDRESS** önceki adımdan. VM'ye bağlanmak için aşağıdaki komutu çalıştırın: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`. 

## <a name= "clean-up"></a>Sanal Makineyi ve ilişkili kaynakları Kaldır

Üretimde kullanmaz, bu alıştırmada oluşturulan kaynakları silmeniz önerilir. Sağlanan sürece VM, genel IP adresi ve disk kaynakları, ücretleri. Bu alıştırmada sırasında oluşturulan kaynakları kaldırmak için aşağıdaki adımları tamamlayın:

1. Kaynak grubundaki kaynakları görüntülemek için çalıştırın `az resource list --resource-group IaaSStory` komutu.
2. Bu makalede betik tarafından oluşturulan kaynakları dışındaki kaynak grubunda kaynak yok onaylayın. 
3. Bu alıştırmada oluşturulan tüm kaynakları silmek için çalıştırın `az group delete -n IaaSStory` komutu. Komut, kaynak grubunu ve içerdiği tüm kaynakları siler.
 
## <a name="set-ip-addresses-within-the-operating-system"></a>İşletim sistemi içinde IP adreslerini ayarlayın

Hiçbir zaman el ile bir Azure sanal makinesi sanal makinenin işletim sistemi içinde atanan genel IP adresi atamanız gerekir. Önerilen, statik olarak bir sanal makinenin işletim sistemi içinde Azure sanal makinesine atanmış özel IP sürece atamayın, gerekli olduğunda gibi [birden çok IP adresleri atama bir Windows VM'ye](virtual-network-multiple-ip-addresses-cli.md). İşletim sistemi içinde özel IP adresini el ile ayarlamanız durumunda Azure atanmış özel IP adresiyle aynı adresi olduğundan emin olun [ağ arabirimi](virtual-network-network-interface-addresses.md#change-ip-address-settings), ya da sanal makineye bağlantı kaybedebilir. Daha fazla bilgi edinin [özel IP adresi](virtual-network-network-interface-addresses.md#private) ayarları.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede oluşturulan VM'ye gelen ve giden ağ trafiğini akabilir. Ağ arabirimi, alt ağ veya her ikisi de gelen ve giden akış trafiğini sınırlandırmak gelen ve giden güvenlik kuralları bir ağ güvenlik grubu içinde tanımlayabilirsiniz. Ağ güvenlik grupları hakkında daha fazla bilgi için bkz. [ağ güvenlik grubu genel bakış](security-overview.md).