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
ms.date: 05/30/2018
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3e6b431ee55ee73b4f5a69471cca3cc16270198c
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37930248"
---
# <a name="how-to-create-a-linux-virtual-machine-with-azure-resource-manager-templates"></a>Azure Resource Manager şablonları ile Linux sanal makinesi oluşturma
Bu makalede Azure Resource Manager şablonları ve Azure CLI ile Linux sanal makinesi (VM) hızlı bir şekilde dağıtma gösterilmektedir. 


## <a name="templates-overview"></a>Şablonlara genel bakış
Azure Resource Manager şablonları altyapı ve Azure çözümünüzü yapılandırmasını tanımlayan JSON dosyalarıdır. Bir şablon kullanarak çözümünü yaşam döngüsü boyunca defalarca dağıtabilir ve kaynaklarınızın tutarlı bir durumda dağıtıldığından emin olabilirsiniz. Biçimi şablon ve nasıl oluşturulacağı hakkında daha fazla bilgi için bkz: [ilk Azure Resource Manager şablonunuzu oluşturma](../../azure-resource-manager/resource-manager-create-first-template.md). Kaynak türleri için JSON söz dizimini görüntülemek üzere bkz. [Azure Resource Manager şablonlarında kaynak tanımlama](/azure/templates/).


## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Bir sanal makineden önce bir kaynak grubu oluşturulmalıdır. Aşağıdaki örnekte adlı bir kaynak grubu oluşturur *Eastus* içinde *eastus* bölgesi:

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma
Aşağıdaki örnek, bir VM'den oluşturur [bu Azure Resource Manager şablonu](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) ile [az grubu dağıtım oluşturma](/cli/azure/group/deployment#az_group_deployment_create). Yalnızca SSH kimlik doğrulaması izin verilir. İstendiğinde içeriğini gibi kendi SSH ortak anahtarı değerini girin *~/.ssh/id_rsa.pub*. SSH anahtar çifti oluşturmak için ihtiyacınız varsa bkz [oluşturmak ve azure'da Linux VM'ler için SSH anahtar çifti kullanma](mac-create-ssh-keys.md).

```azurecli
az group deployment create \
    --resource-group myResourceGroup \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

Önceki örnekte, Github'da depolanmış bir şablon belirtildi. Ayrıca indirin veya bir şablon oluşturmak ve ile yerel bir yol belirtin `--template-file` parametresi.


## <a name="connect-to-virtual-machine"></a>Sanal makineye bağlanma
Sanal makinenize yönelik SSH için genel IP adresiyle elde [az vm show](/cli/azure/vm#az-vm-show):

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name sshvm \
    --show-details \
    --query publicIps \
    --output tsv
```

Normal olarak sanal makinenize yönelik SSH kullanabilirsiniz. Önceki komutta kendi genel IP adresi sağlayın:

```bash
ssh azureuser@<ipAddress>
```

## <a name="next-steps"></a>Sonraki adımlar
Bu örnekte, temel bir Linux VM oluşturdunuz. Daha karmaşık ortamları oluşturun ya da uygulama çerçeveleri içeren daha fazla için Resource Manager şablonları, Gözat [Azure hızlı başlangıç şablonları Galerisi'ne](https://azure.microsoft.com/documentation/templates/).