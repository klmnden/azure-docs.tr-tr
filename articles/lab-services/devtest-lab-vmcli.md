---
title: Oluşturma ve Azure CLI ile DevTest Labs'de sanal makineler yönetme | Microsoft Docs
description: Oluşturma ve Azure CLI ile sanal makineleri yönetmek için Azure DevTest Labs'i kullanmayı öğrenin
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
ms.date: 04/02/2019
ms.author: spelluru
ms.openlocfilehash: 48a30ef86cdb10b540ffe1231294542ccff87255
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59795945"
---
# <a name="create-and-manage-virtual-machines-with-devtest-labs-using-the-azure-cli"></a>Oluşturma ve Azure CLI kullanarak DevTest Labs ile sanal makineleri yönetme
Bu Hızlı oluşturma, başlatma, bağlanma, güncelleştirme, bir geliştirme makinesi laboratuvarınızda temizleme size yol gösterecektir. 

Başlamadan önce:

* Bir laboratuvar oluşturulmadı, bu yönergeler bulunabilir [burada](devtest-lab-create-lab.md).

* [Azure CLI yükleme](/cli/azure/install-azure-cli). Başlamak için Azure ile bir bağlantı oluşturmak için az login çalıştırın. 

## <a name="create-and-verify-the-virtual-machine"></a>Oluşturma ve bir sanal makine doğrulayın 
DevTest Labs ilgili komutları çalıştırmadan önce uygun Azure bağlamı kullanarak ayarlamak `az account set` komutu:

```azurecli
az account set --subscription 11111111-1111-1111-1111-111111111111
```

Bir sanal makine oluşturmak için komut: `az lab vm create`. Laboratuvar, laboratuvarı adı ve sanal makine adı için kaynak grubunu tüm gereklidir. Bağımsız değişkenler kalanı, sanal makine türüne bağlı olarak değiştirin.

Aşağıdaki komut, Azure Market yerden Windows tabanlı bir görüntü oluşturur. Azure portalını kullanarak bir sanal makine oluştururken göreceğiniz şekilde görüntünün adını aynıdır. 

```azurecli
az lab vm create --resource-group DtlResourceGroup --lab-name MyLab --name 'MyTestVm' --image "Visual Studio Community 2017 on Windows Server 2016 (x64)" --image-type gallery --size 'Standard_D2s_v3’ --admin-username 'AdminUser' --admin-password 'Password1!'
```

Aşağıdaki komut, laboratuar ortamında kullanılabilir bir özel görüntüye dayalı bir sanal makine oluşturur:

```azurecli
az lab vm create --resource-group DtlResourceGroup --lab-name MyLab --name 'MyTestVm' --image "My Custom Image" --image-type custom --size 'Standard_D2s_v3' --admin-username 'AdminUser' --admin-password 'Password1!'
```

**Görüntü türü** bağımsız değişken değiştiğini **galeri** için **özel**. Görüntü adı, Azure portalında sanal makine oluşturmak için olsaydı gördüğünüz eşleşir.

Aşağıdaki komut bir VM ile bir Market görüntüsünden ssh kimlik doğrulaması oluşturur:

```azurecli
az lab vm create --lab-name sampleLabName --resource-group sampleLabResourceGroup --name sampleVMName --image "Ubuntu Server 16.04 LTS" --image-type gallery --size Standard_DS1_v2 --authentication-type  ssh --generate-ssh-keys --ip-configuration public 
```

Formülleri ayarlayarak temelinde sanal makineler oluşturabilirsiniz **görüntü türü** parametresi **formül**. Sanal makineniz için belirli bir sanal ağ seçmek ihtiyacınız varsa **vnet-ad** ve **alt** parametreleri. Daha fazla bilgi için [az lab vm oluşturma](/cli/azure/lab/vm#az-lab-vm-create).

## <a name="verify-that-the-vm-is-available"></a>Sanal Makinenin kullanılabilir olduğundan emin olun.
Kullanım `az lab vm show` başlatın ve buna bağlanmak için önce sanal Makinenin kullanılabilir durumda olduğunu doğrulamak için komutu. 

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
Aşağıdaki örnek komut, bir VM başlar:

```azurecli
az lab vm start --lab-name sampleLabName --name sampleVMName --resource-group sampleLabResourceGroup
```

Bir VM'ye bağlanma: [SSH](../virtual-machines/linux/mac-create-ssh-keys.md) veya [Uzak Masaüstü](../virtual-machines/windows/connect-logon.md).
```bash
ssh userName@ipAddressOrfqdn 
```

## <a name="update-the-virtual-machine"></a>Sanal makineyi güncelleştir
Aşağıdaki örnek komut, yapıtlar bir VM için geçerlidir:

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
Aşağıdaki örnek komut VM'yi durdurur.

```azurecli
az lab vm stop --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

Bir VM'yi silin.
```azurecli
az lab vm delete --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki içeriğe bakın: [Azure CLI belgeleri için Azure DevTest Labs](/cli/azure/lab?view=azure-cli-latest). 