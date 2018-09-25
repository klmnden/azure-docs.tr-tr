---
title: OKD azure'da dağıtma | Microsoft Docs
description: OKD azure'da dağıtın.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: ''
ms.author: haroldw
ms.openlocfilehash: 75a02e61adf3e5477b9945afc778e867d5d9c88c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46958144"
---
# <a name="deploy-okd-in-azure"></a>OKD azure'da dağıtma

Azure'da OKD (eski adıyla OpenShift Origin) dağıtmak için iki şekilde kullanabilirsiniz:

- El ile tüm gerekli Azure altyapı bileşenlerini dağıtmak ve OKD izleyin [belgeleri](https://docs.okd.io/3.10/welcome/index.html).
- Mevcut bir kullanabilirsiniz [Resource Manager şablonu](https://github.com/Microsoft/openshift-origin) , OKD küme dağıtımını basitleştirir.

## <a name="deploy-by-using-the-okd-template"></a>OKD şablonu kullanarak dağıtın

Kullanım `appId` için daha önce oluşturduğunuz hizmet sorumlusunun değerinden `aadClientId` parametresi.

Aşağıdaki örnek, tüm gerekli girişleri ile azuredeploy.parameters.json adlı bir parametre dosyası oluşturur.

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

### <a name="deploy-by-using-azure-cli"></a>Azure CLI kullanarak dağıtma


> [!NOTE] 
> Aşağıdaki komutu Azure CLI.8 gerektirir veya üzeri. CLI sürümü ile doğrulayabilirsiniz `az --version` komutu. CLI sürümünü güncelleştirmek için bkz: [Azure CLI yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

Aşağıdaki örnek, OKD kümeyi ve tüm ilgili kaynakları myOpenShiftCluster ile bir dağıtım adı myResourceGroup, adlı bir kaynak grubuna dağıtır. Şablon, doğrudan GitHub deposundan azuredeploy.parameters.json adlı bir yerel parametreleri dosyası kullanılarak başvurulur.

```azurecli 
az group deployment create -g myResourceGroup --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-origin/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

Dağıtımın tamamlanması toplam dağıtılan düğüm sayısına bağlı olarak en az 25 dakika sürer. OKD konsol OpenShift ana yazdırır dağıtım tamamlandığında terminal için DNS adı ve URL.

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200"
}
```

## <a name="connect-to-the-okd-cluster"></a>OKD kümeye bağlanma

Dağıtım tamamlandığında, tarayıcıyı OKD konsoluyla kullanarak bağlanmak `OpenShift Console Uri`. Alternatif olarak, aşağıdaki komutu kullanarak OKD ana bağlanabilir:

```bash
$ ssh -p 2200 clusteradmin@myopenshiftmaster.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kullanım [az grubu Sil](/cli/azure/group#az_group_delete) komutunu kullanarak kaynak grubunu, OpenShift küme kaldırmak için ve artık gerekmediğinde tüm ilgili kaynakları.

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

- [Dağıtım sonrası görevler](./openshift-post-deployment.md)
- [OpenShift dağıtım sorunlarını giderme](./openshift-troubleshooting.md)
- [OKD ile çalışmaya başlama](https://docs.okd.io/latest/getting_started/index.html)
