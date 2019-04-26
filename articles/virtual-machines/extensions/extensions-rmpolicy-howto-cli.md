---
title: VM uzantısı yükleme kısıtlamak için Azure İlkesi'ni kullanın | Microsoft Docs
description: VM uzantısı dağıtımları kısıtlamak için Azure İlkesi'ni kullanın.
services: virtual-machines-linux
documentationcenter: ''
author: roiyz-msft
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/23/2018
ms.author: roiyz;cynthn
ms.openlocfilehash: 1f71276c25e3ec1e5791d9b35f89aa95190c6afd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60543266"
---
# <a name="use-azure-policy-to-restrict-extensions-installation-on-linux-vms"></a>Uzantıları Yükleme Linux vm'lerinde kısıtlamak için Azure İlkesi'ni kullanın

Kullanımından ya da Linux Vm'lerinizi belirli uzantılarını yüklenmesini engellemek istiyorsanız, uzantıları VM'ler için bir kaynak grubu içinde kısıtlamak için CLI'yı kullanarak Azure ilkesi oluşturabilirsiniz. 

Bu öğreticide, en son sürüme sürekli olarak güncelleştirilen Azure Cloud Shell içinde CLI kullanılır. Azure CLI'yı yerel olarak çalıştırmak istiyorsanız, 2.0.26 sürümü yüklemeniz gerekir ya da daha sonra. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme]( /cli/azure/install-azure-cli). 

## <a name="create-a-rules-file"></a>Kurallar dosyası oluşturma

Hangi uzantıların yüklü kısıtlamak için ihtiyacınız bir [kural](../../governance/policy/concepts/definition-structure.md#policy-rule) uzantısı tanımlamak için mantığını sağlamak için.

Bu örnekte Azure Cloud Shell'de kurallar dosyası oluşturarak 'Microsoft.OSTCExtensions' tarafından yayımlanan uzantıları yükleme reddetmeyi gösterilmektedir, ancak CLI'yi yerel olarak çalışıyorsanız, ayrıca bir yerel dosya oluşturun ve yolun (~/clouddrive) yoluyla değiştirin makinenizde yerel dosya.

İçinde bir [Cloud Shell bash](https://shell.azure.com/bash), türü:

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

İşiniz bittiğinde, isabet **Esc** Anahtar'a tıklayın ve anahtar **: wq** kaydedin ve dosyayı kapatın.


## <a name="create-a-parameters-file"></a>Bir parametre dosyası oluşturma

Ayrıca gerekir bir [parametreleri](../../governance/policy/concepts/definition-structure.md#parameters) engellemek için uzantıları listesinde geçirme için kullanabilmeniz için bir yapı oluşturur dosya. 

Bu örnekte Cloud shell'de Linux VM'ler için bir parametre dosyası oluşturma işlemi gösterilmektedir, ancak CLI'yi yerel olarak çalışıyorsanız, ayrıca bir yerel dosya oluşturun ve yolun (~/clouddrive) makinenizde yerel dosyanın yoluyla değiştirin.

İçinde [Cloud Shell bash](https://shell.azure.com/bash), türü:

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

İşiniz bittiğinde, isabet **Esc** Anahtar'a tıklayın ve anahtar **: wq** kaydedin ve dosyayı kapatın.

## <a name="create-the-policy"></a>İlke oluşturma

Bir ilke tanımı, kullanmak istediğiniz yapılandırmayı depolamak için kullanılan bir nesnedir. İlke tanımı, ilke tanımlamak için kuralları ve parametre dosyalarını kullanır. Kullanarak ilke tanımı oluşturma [az ilke tanımını oluşturma](/cli/azure/role/assignment?view=azure-cli-latest).

Bu örnekte, kuralları ve parametreleri, oluşturduğunuz ve .json dosyaları halinde, cloud shell'de depolanan dosyalarıdır.

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

Bu örnekte kullanarak bir kaynak grubu İlkesi atar [az ilke ataması oluşturma](/cli/azure/policy/assignment). Herhangi bir VM oluşturduğunuz **myResourceGroup** kaynak grubu Linux için Linux VM erişimi ya da özel betik uzantıları yükleme mümkün olmayacak. İlkeyi atadığınız önce kaynak grubunun mevcut olması gerekir.

Kullanma [az hesabı listesi](/cli/azure/account?view=azure-cli-latest) yerine bir örnek kullanmak için abonelik Kimliğinizi almak için.


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

## <a name="test-the-policy"></a>Test İlkesi

Yeni VM oluşturma ve yeni bir kullanıcı eklemeye çalışırken İlkesi sınayın.


```azurecli-interactive
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --generate-ssh-keys
```

Adlı yeni bir kullanıcı oluşturmaya çalışın **myNewUser** VM erişimi uzantısını kullanarak.

```azurecli-interactive
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser \
  --password 'mynewuserpwd123!'
```



## <a name="remove-the-assignment"></a>Atamasını Kaldır

```azurecli-interactive
az policy assignment delete --name 'not-allowed-vmextension-linux' --resource-group myResourceGroup
```
## <a name="remove-the-policy"></a>İlke Kaldır

```azurecli-interactive
az policy definition delete --name 'not-allowed-vmextension-linux'
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için [Azure İlkesi](../../governance/policy/overview.md).