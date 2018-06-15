---
title: VM uzantısı yükleme kısıtlamak için Azure ilke kullanın | Microsoft Docs
description: VM uzantısı dağıtımları kısıtlamak için Azure ilkesini kullanın.
services: virtual-machines-linux
documentationcenter: ''
author: danielsollondon
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/23/2018
ms.author: danis;cynthn
ms.openlocfilehash: 8e65b82730884947633688db9ed50080b96e0b8e
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "33942749"
---
# <a name="use-azure-policy-to-restrict-extensions-installation-on-linux-vms"></a>Uzantıları Yükleme Linux sanal makineleri üzerinde kısıtlamak için Azure İlkesi kullanın

Kullanın ya da Linux sanal makineleri üzerinde belirli Uzantıları yüklemesini önlemek istiyorsanız, bir kaynak grubu içindeki VM'ler için uzantıları kısıtlamak için CLI kullanarak bir Azure ilke oluşturabilirsiniz. 

Bu öğretici, sürekli olarak en son sürümüne güncelleştirildiğinden Azure bulut Kabuk içinde CLI kullanır. Azure CLI yerel olarak çalıştırmak istiyorsanız, sürüm 2.0.26 yüklemeniz gerekir ya da daha sonra. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="create-a-rules-file"></a>Kuralları oluşturma

Hangi uzantıları yüklenebilir kısıtlamak için gerek bir [kuralı](/azure/azure-policy/policy-definition#policy-rule) uzantısı belirlemek için mantığı sağlamak için.

Bu örnek, Azure bulut Kabuğu'nda bir kurallar dosyası oluşturarak 'Microsoft.OSTCExtensions' tarafından yayımlanan uzantıları yükleme Reddet gösterilmektedir, ancak CLI içinde yerel olarak çalışıyorsanız, ayrıca yerel bir dosya oluşturun ve yolunu (~/clouddrive) yol yerine makinenizde yerel dosya.

İçinde bir [bulut Kabuk bash](https://shell.azure.com/bash), türü:

```azurecli-interactive 
vim ~/clouddrive/azurepolicy.rules.json
```

Kopyalayın ve aşağıdaki .json dosyaya yapıştırın.

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "equals": "Microsoft.OSTCExtensions/virtualMachines/extensions"
            },
            {
                "field": "Microsoft.OSTCExtensions/virtualMachines/extensions/publisher",
                "equals": "Microsoft.OSTCExtensions"
            },
            {
                "field": "Microsoft.OSTCExtensions/virtualMachines/extensions/type",
                "in": "[parameters('notAllowedExtensions')]"
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```

İşiniz bittiğinde, isabet **Esc** anahtar ve ardından **: wq** kaydedin ve dosyayı kapatın.


## <a name="create-a-parameters-file"></a>Bir parametre dosyası oluşturma

Ayrıca gerekir bir [parametreleri](/azure/azure-policy/policy-definition#parameters) engellemek için uzantılarının listesini bilgilerinde kullanılmak üzere bir yapısını oluşturur dosya. 

Bu örnek bulut kabuğunda Linux VM'ler için bir parametre dosyası oluşturma gösterir, ancak CLI içinde yerel olarak çalışıyorsanız, ayrıca yerel bir dosya oluşturun ve makinenizde yerel dosya yolu (~/clouddrive) yolu değiştirin.

İçinde [bulut Kabuk bash](https://shell.azure.com/bash), türü:

```azurecli-interactive
vim ~/clouddrive/azurepolicy.parameters.json
```

Kopyalayın ve aşağıdaki .json dosyaya yapıştırın.

```json
{
    "notAllowedExtensions": {
        "type": "Array",
        "metadata": {
            "description": "The list of extensions that will be denied. Example: CustomScriptForLinux, VMAccessForLinux etc.",
            "strongType": "type",
            "displayName": "Denied extension"
        }
    }
}
```

İşiniz bittiğinde, isabet **Esc** anahtar ve ardından **: wq** kaydedin ve dosyayı kapatın.

## <a name="create-the-policy"></a>İlke Oluştur

Bir ilke tanımı, kullanmak istediğiniz yapılandırmayı depolamak için kullanılan bir nesnedir. İlke tanımı kuralları ve parametre dosyalarını ilkesini tanımlamak için kullanır. İlke tanımı kullanılarak oluşturma [az ilke tanımı oluşturun](/cli/azure/role/assignment?view=azure-cli-latest#az_role_assignment_create).

Bu örnekte, kuralları ve parametreleri oluşturulur ve bulut Kabuğu'nda .json dosyaları olarak depolanır dosyalardır.

```azurecli-interactive
az policy definition create \
   --name 'not-allowed-vmextension-linux' \
   --display-name 'Block VM Extensions' \
   --description 'This policy governs which VM extensions that are blocked.' \
   --rules '~/clouddrive/azurepolicy.rules.json' \
   --params '~/clouddrive/azurepolicy.parameters.json' \
   --mode All
```


## <a name="assign-the-policy"></a>İlke atama

Bu örnek ilkeyi kullanarak bir kaynak grubu atar [az ilke ataması oluşturma](/cli/azure/policy/assignment#az_policy_assignment_create). Tüm VM oluşturulan **myResourceGroup** kaynak grubu Linux VM erişimi veya özel komut dosyası uzantılarını için Linux yüklemek mümkün olmayacak. İlke atamadan önce kaynak grubu mevcut olmalıdır.

Kullanmak [az hesabı listesi](/cli/azure/account?view=azure-cli-latest#az_account_list) bir örnekte yerine kullanılacak abonelik Kimliğinizi alma.


```azurecli-interactive
az policy assignment create \
   --name 'not-allowed-vmextension-linux' \
   --scope /subscriptions/<subscription Id>/resourceGroups/myResourceGroup \
   --policy "not-allowed-vmextension-linux" \
   --params '{
        "notAllowedExtensions": {
            "value": [
                "VMAccessForLinux",
                "CustomScriptForLinux"
            ]
        }
    }'
```

## <a name="test-the-policy"></a>İlkeyi test etme

İlkeyi test yeni bir VM oluşturma ve yeni bir kullanıcı eklemeye çalışıyor.


```azurecli-interactive
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --generate-ssh-keys
```

Adlı yeni bir kullanıcı oluşturmayı deneyin **myNewUser** VM erişim uzantısını kullanarak.

```azurecli-interactive
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser \
  --password 'mynewuserpwd123!'
```



## <a name="remove-the-assignment"></a>Atama kaldırma

```azurecli-interactive
az policy assignment delete --name 'not-allowed-vmextension-linux' --resource-group myResourceGroup
```
## <a name="remove-the-policy"></a>İlkeyi kaldırın

```azurecli-interactive
az policy definition delete --name 'not-allowed-vmextension-linux'
```


## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için bkz: [Azure ilke](../../azure-policy/azure-policy-introduction.md).
