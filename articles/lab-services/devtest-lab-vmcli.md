---
title: Oluşturma ve Azure CLI ile DevTest Labs'de sanal makineler yönetme | Microsoft Docs
description: Oluşturma ve Azure CLI 2.0 ile sanal makineleri yönetmek için Azure DevTest Labs'i kullanmayı öğrenin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 5e50bc3c6804a6f3d3dafd07b2918605c4cbc6ab
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39434688"
---
# <a name="create-and-manage-virtual-machines-with-devtest-labs-using-the-azure-cli"></a>Oluşturma ve Azure CLI kullanarak DevTest Labs ile sanal makineleri yönetme
Bu hızlı başlangıçta oluşturma, başlatma, bağlama, güncelleştirme, bir geliştirme makinesi laboratuvarınızda temizleme size yol gösterecektir. 

Başlamadan önce:

* Bir laboratuvar oluşturulmadı, bu yönergeler bulunabilir [burada](devtest-lab-create-lab.md).

* [CLI 2.0 sürümünü yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli). Başlamak için Azure ile bir bağlantı oluşturmak için az login çalıştırın. 

## <a name="create-and-verify-the-virtual-machine"></a>Oluşturma ve bir sanal makine doğrulayın 
Bir VM ile bir Market görüntüsünden ssh kimlik doğrulaması oluşturun.
```azurecli
az lab vm create --lab-name sampleLabName --resource-group sampleLabResourceGroup --name sampleVMName --image "Ubuntu Server 16.04 LTS" --image-type gallery --size Standard_DS1_v2 --authentication-type  ssh --generate-ssh-keys --ip-configuration public 
```
> [!NOTE]
> PUT **Laboratuvar kaynak grubu** resource-group parametre adı.
>

Bir VM oluşturmak istiyorsanız formül parametresinde bir formülü kullanarak [az lab vm oluşturma](https://docs.microsoft.com/cli/azure/lab/vm#az-lab-vm-create).


Sanal Makinenin kullanılabilir olduğundan emin olun.
```azurecli
az lab vm show --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup --expand 'properties($expand=ComputeVm,NetworkInterface)' --query '{status: computeVm.statuses[0].displayStatus, fqdn: fqdn, ipAddress: networkInterface.publicIpAddress}'
```
```json
{
  "fqdn": "lisalabvm.southcentralus.cloudapp.azure.com",
  "ipAddress": "13.85.228.112",
  "status": "Provisioning succeeded"
}
```

## <a name="start-and-connect-to-the-virtual-machine"></a>Başlatma ve sanal makineye bağlanma
Bir VM'yi başlatın.
```azurecli
az lab vm start --lab-name sampleLabName --name sampleVMName --resource-group sampleLabResourceGroup
```
> [!NOTE]
> PUT **Laboratuvar kaynak grubu** resource-group parametre adı.
>

Bir VM'ye bağlanma: [SSH](../virtual-machines/linux/mac-create-ssh-keys.md) veya [Uzak Masaüstü](../virtual-machines/windows/connect-logon.md).
```bash
ssh userName@ipAddressOrfqdn 
```

## <a name="update-the-virtual-machine"></a>Sanal makineyi güncelleştir
Yapıtlar bir VM için geçerlidir.
```azurecli
az lab vm apply-artifacts --lab-name  sampleLabName --name sampleVMName  --resource-group sampleResourceGroup  --artifacts @/artifacts.json
```

```json
[
  {
    "artifactId": "/artifactSources/public repo/artifacts/linux-java",
    "parameters": []
  },
  {
    "artifactId": "/artifactSources/public repo/artifacts/linux-install-nodejs",
    "parameters": []
  },
  {
    "artifactId": "/artifactSources/public repo/artifacts/linux-apt-package",
    "parameters": [
      {
        "name": "packages",
        "value": "abcd"
      },
      {
        "name": "update",
        "value": "true"
      },
      {
        "name": "options",
        "value": ""
      }
    ]
  } 
]
```

Laboratuar ortamında kullanılabilen yapılara listeleyin.
```azurecli
az lab vm show --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup --expand "properties(\$expand=artifacts)" --query 'artifacts[].{artifactId: artifactId, status: status}'
```
```json
{
  "artifactId": "/subscriptions/abcdeftgh1213123/resourceGroups/lisalab123RG822645/providers/Microsoft.DevTestLab/labs/lisalab123/artifactSources/public repo/artifacts/linux-install-nodejs",
  "status": "Succeeded"
}
```

## <a name="stop-and-delete-the-virtual-machine"></a>Durdurma ve sanal makineyi silme    
Bir sanal Makineyi durdurun.
```azurecli
az lab vm stop --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

Bir VM'yi silin.
```azurecli
az lab vm delete --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]