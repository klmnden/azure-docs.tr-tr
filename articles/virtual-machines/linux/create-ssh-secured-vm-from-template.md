---
title: "Bir şablondan bir Linux VM oluşturma | Microsoft Docs"
description: "Bir Resource Manager şablonu bir Linux VM oluşturmak için Azure CLI 2.0 kullanma"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 721b8378-9e47-411e-842c-ec3276d3256a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 12/18/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2750bed40707872bb120a7cb7130d8be01aabf7d
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="how-to-create-a-linux-virtual-machine-with-azure-resource-manager-templates"></a>Azure Resource Manager şablonları ile Linux sanal makine oluşturma
Bu makalede Azure Resource Manager şablonları ve Azure CLI 2.0 ile Linux sanal makine (VM) hızlı bir şekilde dağıtma gösterilmektedir. Bu adımları [Azure CLI 1.0](create-ssh-secured-vm-from-template-nodejs.md) ile de gerçekleştirebilirsiniz.


## <a name="templates-overview"></a>Şablonlara genel bakış
Azure Resource Manager şablonları altyapısı ve Azure çözümünüzü yapılandırmasını tanımlayan JSON dosyalarıdır. Bir şablon kullanarak çözümünü yaşam döngüsü boyunca defalarca dağıtabilir ve kaynaklarınızın tutarlı bir durumda dağıtıldığından emin olabilirsiniz. Şablon ve oluşturmak nasıl biçimi hakkında daha fazla bilgi için bkz: [, ilk Azure Resource Manager şablonu oluşturma](../../azure-resource-manager/resource-manager-create-first-template.md). Kaynak türleri için JSON söz dizimini görüntülemek üzere bkz. [Azure Resource Manager şablonlarında kaynak tanımlama](/azure/templates/).


## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Bir kaynak grubu bir sanal makine önce oluşturulması gerekir. Aşağıdaki örnek, bir kaynak grubu oluşturur *myResourceGroupVM* içinde *eastus* bölge:

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma
Aşağıdaki örnek, bir VM'den oluşturur [bu Azure Resource Manager şablonu](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) ile [az grup dağıtımı oluşturmak](/cli/azure/group/deployment#az_group_deployment_create). Yalnızca SSH kimlik doğrulaması izin verilir. İstendiğinde, içeriği gibi kendi SSH ortak anahtarı değerini sağlamalısınız *~/.ssh/id_rsa.pub*. SSH anahtar çifti oluşturmanız gerekiyorsa, bkz: [nasıl oluşturulacağı ve Linux VM'ler için Azure'da SSH anahtar çifti kullanılmaya](mac-create-ssh-keys.md).

```azurecli
az group deployment create --resource-group myResourceGroup \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

Önceki örnekte, Github'da depolanan bir şablon belirtilen. Ayrıca indirin veya bir şablon oluşturmak ve yerel yolu belirtin `--template-file` parametresi.


## <a name="connect-to-virtual-machine"></a>Sanal makineye bağlanma
VM için SSH için ortak IP adresi ile elde [az vm Göster](/cli/azure/vm#az_vm_show):

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name sshvm \
    --show-details \
    --query publicIps \
    --output tsv
```

Normal olarak, VM için SSH kullanabilirsiniz. Yukarıdaki komut kendi ortak IP adresini girin:

```bash
ssh azureuser@<ipAddress>
```

## <a name="next-steps"></a>Sonraki adımlar
Bu örnekte, temel bir Linux VM oluşturdunuz. Uygulama çerçeveleri dahil etmek veya daha karmaşık ortamları oluşturma daha fazla Resource Manager şablonları için Gözat [Azure hızlı başlangıç Şablon Galerisi](https://azure.microsoft.com/documentation/templates/).