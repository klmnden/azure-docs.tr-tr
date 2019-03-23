---
title: Bir şablondan Azure'da bir Linux VM oluşturma | Microsoft Docs
description: Resource Manager şablonundan bir Linux VM oluşturmak için Azure CLI kullanma
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 721b8378-9e47-411e-842c-ec3276d3256a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 03/22/2019
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 334f69390e4506c6db76c1814f8ec8f1e4417ee9
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58372344"
---
# <a name="how-to-create-a-linux-virtual-machine-with-azure-resource-manager-templates"></a>Azure Resource Manager şablonları ile Linux sanal makinesi oluşturma

Bir Azure Resource Manager şablonu ve Azure Cloud shell'den Azure CLI kullanarak bir Linux sanal makinesini (VM) oluşturmayı öğrenin. Bir Windows sanal makine oluşturmak için bkz: [bir Resource Manager şablonundan bir Windows sanal makine oluşturma](../windows/ps-template.md).

## <a name="templates-overview"></a>Şablonlara genel bakış

Azure Resource Manager şablonları altyapı ve Azure çözümünüzü yapılandırmasını tanımlayan JSON dosyalarıdır. Bir şablon kullanarak çözümünü yaşam döngüsü boyunca defalarca dağıtabilir ve kaynaklarınızın tutarlı bir durumda dağıtıldığından emin olabilirsiniz. Biçimi şablon ve nasıl oluşturulacağı hakkında daha fazla bilgi için bkz: [hızlı başlangıç: Oluşturma ve Azure portalını kullanarak Azure Resource Manager şablonlarını dağıtma](../../azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal.md). Kaynak türleri için JSON söz dizimini görüntülemek üzere bkz. [Azure Resource Manager şablonlarında kaynak tanımlama](/azure/templates/microsoft.compute/allversions).

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Bir Azure sanal makinesi oluşturma genellikle iki adımları içerir:

1. Bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Bir sanal makineden önce bir kaynak grubu oluşturulmalıdır.
1. Sanal makine oluşturur.

Aşağıdaki örnek, bir VM oluşturur. bir [Azure Hızlı Başlangıç şablonu](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json). Yalnızca SSH kimlik doğrulaması, bu dağıtım için izin verilir. İstendiğinde içeriğini gibi kendi SSH ortak anahtarı değerini girin *~/.ssh/id_rsa.pub*. SSH anahtar çifti oluşturmak için ihtiyacınız varsa bkz [oluşturmak ve azure'da Linux VM'ler için SSH anahtar çifti kullanma](mac-create-ssh-keys.md). Şablonun bir kopyasını şu şekildedir:

[!code-json[create-linux-vm](~/quickstart-templates/101-vm-sshkey/azuredeploy.json)]

CLI betiği çalıştırmak için seçin **deneyin** Azure Cloud Shell'i açmak için. Betik yapıştırmak için kabuk sağ tıklayın ve ardından **yapıştırın**:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the location (i.e. centralus):" &&
read location &&
echo "Enter the project name (used for generating resource names):" &&
read projectName &&
echo "Enter the administrator username:" &&
read username &&
echo "Enter the SSH public key:" &&
read key &&
az group create --name $resourceGroupName --location "$location" &&
az group deployment create --resource-group $resourceGroupName --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json --parameters projectName=$projectName adminUsername=$username adminPublicKey='$key' &&
az vm show --resource-group $resourceGroupName --name "$projectName-vm" --show-details --query publicIps --output tsv
```

Son Azure CLI komutu yeni oluşturulan VM'nin genel IP adresini gösterir. Sanal makineye bağlanmak için genel IP adresi gereklidir. Bu makalenin sonraki bölüme bakın.

Önceki örnekte, Github'da depolanmış bir şablon belirtildi. Ayrıca indirin veya bir şablon oluşturmak ve ile yerel bir yol belirtin `--template-file` parametresi.

Bazı ek kaynaklar aşağıda verilmiştir:

- Resource Manager şablonları geliştirme hakkında bilgi edinmek için [Azure Resource Manager belgelerini](/azure/azure-resource-manager/).
- Azure sanal makine şemaları görmek için bkz: [Azure şablon başvurusu](/azure/templates/microsoft.compute/allversions).
- Daha fazla sanal makine şablonu örnekleri görmek için bkz: [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Compute&pageNumber=1&sort=Popular).

## <a name="connect-to-virtual-machine"></a>Sanal makineye bağlanma

Normal olarak sanal makinenize yönelik SSH kullanabilirsiniz. Önceki komutta kendi genel IP adresi sağlayın:

```bash
ssh <adminUsername>@<ipAddress>
```

## <a name="next-steps"></a>Sonraki adımlar

Bu örnekte, temel bir Linux VM oluşturdunuz. Daha karmaşık ortamları oluşturun ya da uygulama çerçeveleri içeren daha fazla için Resource Manager şablonları, Gözat [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Compute&pageNumber=1&sort=Popular).

Şablonları oluşturma hakkında daha fazla bilgi için JSON söz dizimi ve dağıttığınız kaynak türleri için özellikleri görüntüleyin:

- [Microsoft.Network/networkSecurityGroups](/azure/templates/microsoft.network/networksecuritygroups)
- [Microsoft.Network/publicIPAddresses](/azure/templates/microsoft.network/publicipaddresses)
- [Microsoft.Network/virtualNetworks](/azure/templates/microsoft.network/virtualnetworks)
- [Microsoft.Network/networkInterfaces](/azure/templates/microsoft.network/networkinterfaces)
- [Microsoft.Compute/virtualMachines](/azure/templates/microsoft.compute/virtualmachines)
