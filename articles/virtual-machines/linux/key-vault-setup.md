---
title: Linux VM'ler için Azure anahtar kasası ayarlama | Microsoft Docs
description: Anahtar kasası CLI 2.0 ile bir Azure Resource Manager sanal makine ile kullanmak için nasıl kurulur.
services: virtual-machines-linux
documentationcenter: ''
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: bccdd5ab-5ccf-4760-9039-92c6eafb15bd
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 02/24/2017
ms.author: singhkay
ms.openlocfilehash: 6bd039225062ac6010d432b930f601fe4678ed2c
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30907160"
---
# <a name="how-to-set-up-key-vault-for-virtual-machines-with-the-azure-cli-20"></a>Azure CLI 2.0 ile sanal makineler için anahtar kasasını ayarlama

Gizli/sertifikalar, anahtar kasası tarafından sağlanan kaynaklar olarak Azure Kaynak Yöneticisi yığınında modellenir. Azure anahtar kasası hakkında daha fazla bilgi için bkz: [Azure anahtar kasası nedir?](../../key-vault/key-vault-whatis.md) Azure Resource Manager VM'ler ile kullanılmak üzere anahtar kasası için sırayla *EnabledForDeployment* anahtar kasası özelliğinin ayarlanması true. Bu makalede Azure Azure CLI 2.0 kullanarak sanal makineleri (VM'ler) ile kullanmak için anahtar kasasını oluşturup gösterilmiştir. Bu adımları [Azure CLI 1.0](key-vault-setup-cli-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ile de gerçekleştirebilirsiniz.

Bu adımları gerçekleştirmek için en son gerekir [Azure CLI 2.0](/cli/azure/install-az-cli2) yüklü ve bir Azure hesabı kullanarak oturum açmış [az oturum açma](/cli/azure/reference-index#az_login).

## <a name="create-a-key-vault"></a>Anahtar kasası oluşturma
Bir anahtar kasası oluşturun ve dağıtım ilkesiyle atayın [az keyvault oluşturma](/cli/azure/keyvault#az_keyvault_create). Aşağıdaki örnek adlı bir anahtar kasası oluşturur `myKeyVault` içinde `myResourceGroup` kaynak grubu:

```azurecli
az keyvault create -l westus -n myKeyVault -g myResourceGroup --enabled-for-deployment true
```

## <a name="update-a-key-vault-for-use-with-vms"></a>Sanal makineler ile kullanmak için bir anahtar kasası güncelleştirme
Varolan bir anahtar dağıtım ilkesindeki kasa ile kümesi [az keyvault güncelleştirme](/cli/azure/keyvault#az_keyvault_update). Adlı anahtar kasası aşağıdaki güncelleştirmeleri `myKeyVault` içinde `myResourceGroup` kaynak grubu:

```azurecli
az keyvault update -n myKeyVault -g myResourceGroup --set properties.enabledForDeployment=true
```

## <a name="use-templates-to-set-up-key-vault"></a>Anahtar kasası ayarlamak için şablonlarını kullanma
Bir şablonu kullandığınızda ayarlamanız gerekir `enabledForDeployment` özelliğine `true` gibi kaynak için anahtar Kasası:

```json
{
    "type": "Microsoft.KeyVault/vaults",
    "name": "ContosoKeyVault",
    "apiVersion": "2015-06-01",
    "location": "<location-of-key-vault>",
    "properties": {
    "enabledForDeployment": "true",
    ....
    ....
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
Şablonları kullanarak bir anahtar kasası oluşturduğunuzda, sizin yapılandırabileceğiniz diğer seçenekleri için bkz: [bir anahtar kasası oluşturma](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).
