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
ms.openlocfilehash: 1860ede19202566947b68b715e6bd354f64c1085
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2017
---
# <a name="deploy-openshift-origin-in-azure"></a>Azure'da OpenShift kaynak dağıtma

OpenShift kaynak azure'da dağıtmak için iki yoldan biriyle kullanabilirsiniz:

- El ile tüm gerekli Azure altyapı bileşenlerini dağıtmak ve OpenShift kaynağını izleyin [belgelerine](https://docs.openshift.org/3.6/welcome/index.html).
- Var olan de kullanabilirsiniz [Resource Manager şablonu](https://github.com/Microsoft/openshift-origin) OpenShift kaynak Küme dağıtımı basitleştirir.

## <a name="deploy-by-using-the-openshift-origin-template"></a>OpenShift kaynak şablonu kullanarak dağıtın

Kullanım `appId` için daha önce oluşturduğunuz hizmet asıl değerinden `aadClientId` parametresi.

Aşağıdaki örnek, gerekli tüm girişleri ile azuredeploy.parameters.json adlı bir parametre dosyası oluşturur.

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
> Aşağıdaki komutu Azure CLI 2.0.8 gerektirir veya sonraki bir sürümü. CLI sürümüyle doğrulayabilirsiniz `az --version` komutu. CLI Sürüm güncelleştirmek için bkz: [Azure CLI 2.0 yükleme](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest).

Aşağıdaki örnek, myResourceGroup, myOpenShiftCluster dağıtım adı ile adlı bir kaynak grubuna OpenShift küme ve tüm ilgili kaynaklar dağıtır. Şablon azuredeploy.parameters.json adlı bir yerel parametre dosyası kullanarak doğrudan GitHub deposuna başvuruluyor.

```azurecli 
az group deployment create -g myResourceGroup --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-origin/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

Dağıtım, dağıtılan düğümleri toplam sayısına bağlı olarak tamamlamak için en az 25 dakika sürer. OpenShift konsolu ve dağıtım sona erdiğinde terminale OpenShift ana baskı siparişi DNS adını URL'si.

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200"
}
```

## <a name="connect-to-the-openshift-cluster"></a>OpenShift kümeye bağlanın

Dağıtım tamamlandığında tarayıcınızla OpenShift Konsolu'na kullanarak bağlanmak `OpenShift Console Uri`. Alternatif olarak, aşağıdaki komutu kullanarak OpenShift asıl bağlanabilir:

```bash
$ ssh -p 2200 clusteradmin@myopenshiftmaster.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kullanım [az grubu Sil](/cli/azure/group#delete) OpenShift küme, kaynak grubu kaldırmak için komut ve artık gerekmediğinde tüm ilgili kaynaklar.

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

- [Dağıtım sonrası görevler](./openshift-post-deployment.md)
- [OpenShift dağıtım sorunlarını gider](./openshift-troubleshooting.md)
- [OpenShift Origin kullanmaya başlama](https://docs.openshift.org/latest/getting_started/index.html)
