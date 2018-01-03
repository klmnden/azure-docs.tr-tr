---
title: "Azure CLI ile DevTest Labs içinde sanal makineler oluşturun ve yönetin | Microsoft Docs"
description: "Azure DevTest Labs oluşturun ve Azure CLI 2.0 ile sanal makineleri yönetmek için nasıl kullanılacağını öğrenin"
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: v-craic
ms.openlocfilehash: e73ddeba56c779d9fb1be77a50cbae5111de03c4
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2018
---
# <a name="create-and-manage-virtual-machines-with-devtest-labs-using-the-azure-cli"></a>Oluşturma ve sanal makineleri DevTest Labs Azure CLI kullanarak yönetme
Bu hızlı başlangıç oluşturma, başlatma, bağlanma, güncelleştirme ve geliştirme makine laboratuvarınızda temizleniyor size yol gösterecektir. 

Başlamadan önce:

* Bir laboratuvar oluşturulmadığında, yönergeler bulunabilir [burada](devtest-lab-create-lab.md).

* [CLI 2.0 yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli). Başlatmak için Azure ile bir bağlantı oluşturmak için az oturum açma çalıştırın. 

## <a name="create-and-verify-the-virtual-machine"></a>Oluşturun ve sanal makine doğrulayın 
Bir VM ile bir Market görüntüsünden ssh kimlik doğrulaması oluşturun.
```azurecli
az lab vm create --lab-name sampleLabName --resource-group sampleLabResourceGroup --name sampleVMName --image "Ubuntu Server 16.04 LTS" --image-type gallery --size Standard_DS1_v2 --authentication-type  ssh --generate-ssh-keys --ip-configuration public 
```
> [!NOTE]
> PUT **Laboratuvar'ın kaynak grubu** kaynak-grubu parametresinin adı.
>

Bir VM oluşturmak istiyorsanız--formül parametresinde formülü kullanarak [az Laboratuvar vm oluşturma](https://docs.microsoft.com/cli/azure/lab/vm#az_lab_vm_create).


VM kullanılabilir olduğundan emin olun.
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
VM başlatmak.
```azurecli
az lab vm start --lab-name sampleLabName --name sampleVMName --resource-group sampleLabResourceGroup
```
> [!NOTE]
> PUT **Laboratuvar'ın kaynak grubu** kaynak-grubu parametresinin adı.
>

Bir VM'ye bağlanın: [SSH](../virtual-machines/linux/mac-create-ssh-keys.md) veya [Uzak Masaüstü](../virtual-machines/windows/connect-logon.md).
```bash
ssh userName@ipAddressOrfqdn 
```

## <a name="update-the-virtual-machine"></a>Sanal makineyi güncelleştirin
Yapıları bir VM için geçerlidir.
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

Liste yapıları laboratuar ortamında kullanılabilir.
```azurecli
az lab vm show --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup --expand "properties(\$expand=artifacts)" --query 'artifacts[].{artifactId: artifactId, status: status}'
```
```json
{
  "artifactId": "/subscriptions/abcdeftgh1213123/resourceGroups/lisalab123RG822645/providers/Microsoft.DevTestLab/labs/lisalab123/artifactSources/public repo/artifacts/linux-install-nodejs",
  "status": "Succeeded"
}
```

## <a name="stop-and-delete-the-virtual-machine"></a>Durdurun ve sanal makineyi silin    
Bir VM'yi durdurun.
```azurecli
az lab vm stop --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

Bir VM silin.
```azurecli
az lab vm delete --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]