---
title: "Azure'da OpenShift kapsayıcı Platform dağıtma | Microsoft Docs"
description: "Azure'da OpenShift kapsayıcı Platform dağıtın."
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
ms.openlocfilehash: f1ba6a3d3b9e576d513b55beac4e9365102433e9
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="deploy-openshift-container-platform-in-azure"></a>Azure'da OpenShift kapsayıcı Platform dağıtma

OpenShift kapsayıcı Platform azure'da dağıtmak için birkaç yöntemden birini kullanabilirsiniz:

- El ile gerekli Azure altyapı bileşenlerini dağıtmak ve OpenShift kapsayıcı Platform izleyin [belgelerine](https://docs.openshift.com/container-platform/3.6/welcome/index.html).
- Var olan de kullanabilirsiniz [Resource Manager şablonu](https://github.com/Microsoft/openshift-container-platform/) OpenShift kapsayıcı Platform Küme dağıtımı basitleştirir.
- Başka bir seçenek kullanmaktır [Azure Marketi teklifi](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.openshift-container-platform?tab=Overview).

Tüm seçenekleri için bir Red Hat aboneliği gereklidir. Dağıtım sırasında Red Hat Enterprise Linux örnek Red Hat abonelik için kaydedilmiş ve OpenShift kapsayıcı Platform için yetkilendirmeleri içeren havuz Kimliğini eklenir.
Geçerli Red Hat Abonelik Yöneticisi (RHSM) kullanıcı adı, parola ve Havuz kimliği olduğundan emin olun Oturum açarak https://access.redhat.com için bu bilgileri doğrulayabilirsiniz.

## <a name="deploy-by-using-the-openshift-container-platform-resource-manager-template"></a>OpenShift kapsayıcı Platform Resource Manager şablonu kullanarak dağıtın

Resource Manager şablonu kullanarak dağıtmak için giriş parametreleri sağlamak için bir parametre dosyası kullanın. Giriş parametreleri kullanarak kapsanmayan dağıtım öğeleri özelleştirmek için GitHub depo çatallaştırma ve uygun öğeleri değiştirin.

Bazı yaygın özelleştirme seçenekleri dahil ancak bunlarla sınırlı değildir:

- Sanal ağ CIDR (azuredeploy.json değişkeninde)
- Savunma VM boyutu (azuredeploy.json değişkeninde)
- Adlandırma kuralları (azuredeploy.json değişkenler)
- Hosts dosyasını (deployOpenShift.sh) OpenShift küme özelliklerini değiştirdi

### <a name="configure-the-parameters-file"></a>Parametre dosyasını yapılandırma

Kullanım `appId` daha önce oluşturduğunuz için hizmet sorumlusu değerinden `aadClientId` parametresi. 

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
        "enableMetrics": {
            "value": "true"
        },
        "enableLogging": {
            "value": "true"
        },
        "enableCockpit": {
            "value": "false"
        },
        "rhsmUsernamePasswordOrActivationKey": {
            "value": "usernamepassword"
        },
        "rhsmUsernameOrOrgId": {
            "value": "{RHSM Username}"
        },
        "rhsmPasswordOrActivationKey": {
            "value": "{RHSM Password}"
        },
        "rhsmPoolId": {
            "value": "{Pool ID}"
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

Köşeli ayraçlar, belirli bilgilerle içine öğeleri değiştirin.

### <a name="deploy-by-using-azure-cli"></a>Azure CLI kullanarak dağıtma

> [!NOTE] 
> Aşağıdaki komutu Azure CLI 2.0.8 gerektirir veya sonraki bir sürümü. CLI sürümüyle doğrulayabilirsiniz `az --version` komutu. CLI Sürüm güncelleştirmek için bkz: [Azure CLI 2.0 yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latesti).

Aşağıdaki örnek, myResourceGroup, myOpenShiftCluster dağıtım adı ile adlı bir kaynak grubuna OpenShift küme ve tüm ilgili kaynaklar dağıtır. Şablon doğrudan GitHub depo ve azuredeploy.parameters.json dosya adlı dosya kullanılan yerel bir parametreler başvuruluyor.

```azurecli 
az group deployment create -g myResourceGroup --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-container-platform/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

Dağıtım, dağıtılan düğümleri toplam sayısına bağlı olarak tamamlamak için en az 30 dakika sürer. OpenShift konsolu ve dağıtım sona erdiğinde terminale OpenShift ana baskı siparişi DNS adını URL'si.

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200"
}
```

## <a name="deploy-by-using-the-openshift-container-platform-azure-marketplace-offer"></a>OpenShift kapsayıcı Platform Azure Marketi teklifi kullanarak dağıtma

OpenShift kapsayıcı Platform Azure'a dağıtmak için en basit yolu kullanmaktır [Azure Marketi teklifi](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.openshift-container-platform?tab=Overview).

En basit seçenek budur ancak özelleştirme yeteneği sınırlıdır. Teklif üç yapılandırma seçenekleri içerir:

- **Küçük**: bir yönetici düğümü, bir altyapı düğümü, iki uygulama düğümleri ve bir savunma düğüm yüksek olmayan kullanılabilirlik (HA) kümeyle dağıtır. Tüm düğümler, yalnızca standart DS2v2 VM boyutlarıdır. Bu küme 10 toplam çekirdek gerektirir ve küçük ölçekli sınamak için idealdir.
- **Orta**: üç ana düğüm, iki altyapı düğüm, dört uygulama düğümleri ve bir savunma düğüm HA kümeyle dağıtır. Savunma düğüm dışındaki tüm düğümleri standart DS3v2 VM boyutlarıdır. Standart DS2v2 savunma düğümdür. Bu küme 38 çekirdek gerektirir.
- **Büyük**: üç ana düğüm, iki altyapı düğümleri, altı uygulama düğümleri ve bir savunma düğüm HA kümeyle dağıtır. Ana ve altyapı düğümleri standart DS3v2 VM boyutlarıdır. Uygulama düğümleri standart DS4v2 VM boyutları olan ve standart DS2v2 savunma düğümdür. Bu küme 70 çekirdek gerektirir.

Azure bulut çözümü sağlayıcısı yapılandırmasını Orta ve büyük küme boyutlarını isteğe bağlıdır. Küçük küme boyutu Azure bulut çözümü sağlayıcısı yapılandırma seçeneği sağlamaz.

## <a name="connect-to-the-openshift-cluster"></a>OpenShift kümeye bağlanın

Dağıtım tamamlandığında tarayıcınızla OpenShift Konsolu'na kullanarak bağlanmak `OpenShift Console Uri`. Alternatif olarak, aşağıdaki komutu kullanarak OpenShift asıl bağlanabilir:

```bash
$ ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kullanım [az grubu Sil](/cli/azure/group#az_group_delete) OpenShift küme, kaynak grubu kaldırmak için komut ve artık gerekmediğinde tüm ilgili kaynaklar.

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

- [Dağıtım sonrası görevler](./openshift-post-deployment.md)
- [Azure'da OpenShift dağıtım sorunlarını gider](./openshift-troubleshooting.md)
- [OpenShift kapsayıcı Platform ile çalışmaya başlama](https://docs.openshift.com/container-platform/3.6/getting_started/index.html)
