---
title: OKD azure'da dağıtma | Microsoft Docs
description: OKD azure'da dağıtın.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: joraio
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/02/2019
ms.author: haroldw
ms.openlocfilehash: 7db50007dd32c84a360eaec25bf860709272437b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60542518"
---
# <a name="deploy-okd-in-azure"></a>OKD azure'da dağıtma

Azure'da OKD (eski adıyla OpenShift Origin) dağıtmak için iki şekilde kullanabilirsiniz:

- Tüm gerekli Azure altyapı bileşenlerini el ile dağıtın ve izleyin [OKD belgeleri](https://docs.okd.io).
- Mevcut bir kullanabilirsiniz [Resource Manager şablonu](https://github.com/Microsoft/openshift-origin) , OKD küme dağıtımını basitleştirir.

## <a name="deploy-using-the-okd-template"></a>OKD şablon kullanarak dağıtma

Resource Manager şablonu kullanarak dağıtmak için giriş parametreleri sağlamak için bir parametre dosyası kullanın. Daha fazla dağıtımı özelleştirmek için GitHub depo çatalını oluşturmanız ve uygun öğeleri değiştirin.

Bazı ortak özelleştirme seçenekleri içerir, ancak bunlarla sınırlı değildir:

- Savunma VM boyutu (azuredeploy.json değişkeninde)
- Adlandırma kuralları (azuredeploy.json değişkenlerinde)
- OpenShift küme özellikleri, hosts dosyasını (deployOpenShift.sh) değiştirme

[OKD şablon](https://github.com/Microsoft/openshift-origin) sahip birden fazla dalı OKD farklı sürümleri için kullanılabilir.  Gereksinimlerinize göre doğrudan deposundan dağıtabilirsiniz veya deponun çatalını oluşturup dağıtmadan önce özel değişiklikleri yapın.

Kullanım `appId` için daha önce oluşturduğunuz hizmet sorumlusunun değerinden `aadClientId` parametresi.

Tüm gerekli girişleri ile azuredeploy.parameters.json adlı bir parametre dosyası örneği verilmiştir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
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
        "storageKind": {
            "value": "managed"
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
        "enableMetrics": {
            "value": "true"
        },
        "enableLogging": {
            "value": "false"
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
        "enableAzure": {
            "value": "true"
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

Parametreleri özel bilgileri ile değiştirin.

Farklı sürümleri olabilir farklı parametreler bu nedenle Lütfen kullandığınız dal için gerekli parametreleri doğrulayın.

### <a name="deploy-using-azure-cli"></a>Azure CLI kullanarak dağıtma


> [!NOTE] 
> Aşağıdaki komut, Azure CLI'yı 2.0.8 gerektirir veya üzeri. CLI sürümü ile doğrulayabilirsiniz `az --version` komutu. CLI sürümünü güncelleştirmek için bkz: [Azure CLI yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

Aşağıdaki örnek, OKD kümeyi ve tüm ilgili kaynakları myOpenShiftCluster ile bir dağıtım adı openshiftrg, adlı bir kaynak grubuna dağıtır. Şablon azuredeploy.parameters.json adlı bir yerel parametre dosyasını kullanırken doğrudan GitHub deposundan başvuruluyor.

```azurecli 
az group deployment create -g openshiftrg --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-origin/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

Dağıtım toplam dağıtılan düğüm sayısına göre tamamlamak için en az 30 dakika sürer. OpenShift konsol OpenShift ana yazdırır dağıtım tamamlandığında terminal için DNS adı ve URL. Alternatif olarak, Azure portalından dağıtımı çıktılar bölümünü görüntüleyebilirsiniz.

```json
{
  "OpenShift Console Url": "http://openshiftlb.cloudapp.azure.com/console",
  "OpenShift Master SSH": "ssh -p 2200 clusteradmin@myopenshiftmaster.cloudapp.azure.com"
}
```

Dağıtım tamamlamak ekleyin bekleniyor komut satırı bağlamanın istemiyorsanız `--no-wait` grubu dağıtımı için seçenekleri biri olarak. Dağıtım çıkışı, Azure portalında kaynak grubu için Dağıtım bölümündeki alınabilir.

## <a name="connect-to-the-okd-cluster"></a>OKD kümeye bağlanma

Dağıtım tamamlandığında, tarayıcıyı kullanarak ile OpenShift konsoluna bağlanmak `OpenShift Console Url`. Alternatif olarak, SSH OKD asıl olabilir. Dağıtım çıktısını kullanan bir örnek aşağıda verilmiştir:

```bash
$ ssh -p 2200 clusteradmin@myopenshiftmaster.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kullanım [az grubu Sil](/cli/azure/group) komutunu kullanarak kaynak grubunu, OpenShift küme kaldırmak için ve artık gerekmediğinde tüm ilgili kaynakları.

```azurecli 
az group delete --name openshiftrg
```

## <a name="next-steps"></a>Sonraki adımlar

- [Dağıtım sonrası görevler](./openshift-post-deployment.md)
- [OpenShift dağıtım sorunlarını giderme](./openshift-troubleshooting.md)
- [OKD ile çalışmaya başlama](https://docs.okd.io)
