---
title: "Azure'da OpenShift kaynak dağıtma | Microsoft Docs"
description: "Azure'da OpenShift kaynak dağıtın."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: haroldw
ms.openlocfilehash: 1a40c4cc064b32aced7e976f40f6ed6a57e62204
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="deploy-openshift-origin-in-azure"></a>Azure'da OpenShift kaynak dağıtma

OpenShift kaynak azure'da dağıtmak için birden çok yolu vardır. El ile tüm gerekli Azure altyapı bileşenlerini dağıtmak ve OpenShift kaynağını izleyin [belgelerine](https://docs.openshift.org/3.6/welcome/index.html).
OpenShift kaynak küme dağıtımını basitleştirir var olan bir Resource Manager şablonu de kullanabilirsiniz. Bu tür şablon bulunduktan sonra [burada](https://github.com/Microsoft/openshift-origin).

## <a name="deploy-using-the-openshift-origin-template"></a>OpenShift kaynak şablonunu kullanarak dağıtın

Kullanım `appId` daha önce oluşturduğunuz için hizmet sorumlusu değerinden `aadClientId` parametresi.

Aşağıdaki örnek adlı bir parametre dosyası oluşturur **azuredeploy.parameters.json** gerekli tüm girişleri ile.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "masterVmSize": {
            "value": "Standard_E2s_v3"
        },
        "infraVmSize": {
            "value": "Standard_E2s_v3"
        },
        "nodeVmSize": {
            "value": "Standard_E2s_v3"
        },
        "openshiftClusterPrefix": {
            "value": "mycluster"
        },
        "masterInstanceCount": {
            "value": 3
        },
        "infraInstanceCount": {
            "value": 2
        },
        "nodeInstanceCount": {
            "value": 2
        },
        "dataDiskSize": {
            "value": 128
        },
        "adminUsername": {
            "value": "clusteradmin"
        },
        "openshiftPassword": {
            "value": "{Strong Password}"
        },
        "sshPublicKey": {
            "value": "{SSH Public Key}"
        },
        "keyVaultResourceGroup": {
            "value": "keyvaultrg"
        },
        "keyVaultName": {
            "value": "keyvault"
        },
        "keyVaultSecret": {
            "value": "keysecret"
        },
        "aadClientId": {
            "value": "11111111-abcd-1234-efgh-111111111111"
        },
        "aadClientSecret": {
            "value": "{Strong Password}"
        },
        "defaultSubDomainType": {
            "value": "nipio"
        }
    }
}
```

### <a name="deploy-using-azure-cli"></a>Azure CLI kullanarak dağıtın


> [!NOTE] 
> Aşağıdaki komutu Azure CLI 2.0.8 gerektirir veya sonraki bir sürümü. CLI az doğrulayabilirsiniz sürümüyle `az --version` komutu. CLI Sürüm güncelleştirmek için bkz: [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

Aşağıdaki örnek, bir myOpenShiftCluster dağıtım adla myResourceGroup adlı bir kaynak grubuna OpenShift küme ve tüm ilgili kaynaklar dağıtır. Şablon doğrudan github deposuna başvurulan ve bir yerel parametre dosyası adlı **azuredeploy.parameters.json** dosyası kullanılır.

```azurecli 
az group deployment create -g myResourceGroup --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-origin/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

Dağıtım düğümleri dağıtılan toplam sayısına bağlı olarak tamamlamak için en az 25 dakika sürer. Dağıtım tamamlandığında OpenShift asıl DNS adını ve OpenShift konsol URL'sini terminale yazdırılır.

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200"
}
```

## <a name="connect-to-the-openshift-cluster"></a>OpenShift kümeye bağlanın

Dağıtım tamamlandığında, tarayıcı kullanarak OpenShift Konsolu'na bağlanmak `OpenShift Console Uri`. Alternatif olarak, aşağıdaki komutu kullanarak OpenShift ana bağlanabilir:

```bash
$ ssh -p 2200 clusteradmin@myopenshiftmaster.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olduğunda, kullanabileceğiniz [az grubu Sil](/cli/azure/group#delete) OpenShift küme, kaynak grubu kaldırmak için komut ve ilişkili tüm kaynakları.

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

- [POST dağıtım görevleri](./openshift-post-deployment.md)
- [OpenShift dağıtım sorunlarını giderme](./openshift-troubleshooting.md)
- [OpenShift kaynak ile çalışmaya başlama](https://docs.openshift.org/latest/getting_started/index.html)
