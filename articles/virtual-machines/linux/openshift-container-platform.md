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
ms.openlocfilehash: c91b7232b2f87e0b4b5e659126b96a6ef8b4202c
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="deploy-openshift-container-platform-in-azure"></a>Azure'da OpenShift kapsayıcı Platform dağıtma

OpenShift kapsayıcı Platform azure'da dağıtmak için birden çok yolu vardır. El ile tüm gerekli Azure altyapı bileşenlerini dağıtmak ve OpenShift kapsayıcı Platform izleyin [belgelerine](https://docs.openshift.com/container-platform/3.6/welcome/index.html).
Ayrıca OpenShift kapsayıcı Platform Küme dağıtımı basitleştirir var olan bir Resource Manager şablonu kullanabilirsiniz. Bu tür şablon bulunduktan sonra [burada](https://github.com/Microsoft/openshift-container-platform/).

Başka bir seçenek kullanmaktır [Azure Marketi teklifi](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/redhat.openshift-container-platform?tab=Overview).

Her iki seçenek için bir Red Hat aboneliği gereklidir. Dağıtım sırasında RHEL örnek Red Hat abonelik için kaydedilmiş ve OpenShift kapsayıcı Platform için yetkilendirmeleri içeren havuz Kimliğini bağlı.
Geçerli bir Red Hat Abonelik Yöneticisi kullanıcı adı, parola ve havuzu kimliği (RHSM adı, RHSM parola ve havuzu kimliği) olduğundan emin olun. Https://access.redhat.com açarak bilgilerini doğrulayın.

## <a name="deploy-using-the-openshift-container-platform-resource-manager-template"></a>OpenShift kapsayıcı Platform Resource Manager şablonunu kullanarak dağıtın

Resource Manager şablonu kullanarak dağıtmak için bir parametre dosyası tüm giriş parametreleri sağlamak için kullanılır. Giriş parametreleri, çatalı github depo kullanarak kapsanmayan dağıtım öğeleri özelleştirmek ve uygun öğeleri değiştirmek istiyorsanız.

(Ancak bunlarla sınırlı değil) bazı yaygın özelleştirme seçenekleri şunlardır:

- VNet CIDR [azuredeploy.json değişkeninde]
- Savunma VM boyutu [azuredeploy.json değişkeninde]
- Adlandırma kuralları [azuredeploy.json değişkenlerde]
- Hosts dosyasını [deployOpenShift.sh] değiştirilmiş OpenShift küme özellikleri-

### <a name="configure-parameters-file"></a>Parametre dosyasını yapılandırma

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

{} İle ilgili bilgilerinizi içine öğeleri değiştirin.

### <a name="deploy-using-azure-cli"></a>Azure CLI kullanarak dağıtın

> [!NOTE] 
> Aşağıdaki komutu Azure CLI 2.0.8 gerektirir veya sonraki bir sürümü. CLI az doğrulayabilirsiniz sürümüyle `az --version` komutu. CLI Sürüm güncelleştirmek için bkz: [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

Aşağıdaki örnek, bir myOpenShiftCluster dağıtım adla myResourceGroup adlı bir kaynak grubuna OpenShift küme ve tüm ilgili kaynaklar dağıtır. Şablon doğrudan github deposuna başvurulan ve bir yerel parametre dosyası adlı **azuredeploy.parameters.json** dosyası kullanılır.

```azurecli 
az group deployment create -g myResourceGroup --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-container-platform/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

Dağıtım düğümleri dağıtılan toplam sayısına bağlı olarak tamamlamak için en az 30 dakika sürer. Dağıtım tamamlandığında OpenShift asıl DNS adını ve OpenShift konsol URL'sini terminale yazdırılır.

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200"
}
```

## <a name="deploy-using-openshift-container-platform-marketplace-offer"></a>OpenShift kapsayıcı Platform Market teklifi kullanarak dağıtma

OpenShift kapsayıcı Platform Azure'a dağıtmak için en basit yolu kullanmaktır [Azure Marketi teklifi](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/redhat.openshift-container-platform?tab=Overview).

Bu seçenek, basit bir ancak özelleştirme özellikleri de sınırlıdır. Teklif üç yapılandırma seçenekleri içerir:

- Küçük: olmayan bir dağıtır-HA bir ana düğüm, bir altyapı düğümü, iki uygulama düğümleri ve bir savunma düğümü ile küme. Tüm düğümleri standart DS2v2 VM boyutlarıdır. Bu küme 10 toplam çekirdek gerektirir ve küçük ölçekli sınamak için idealdir.
- Orta: üç ana düğüm, iki altyapı düğüm, dört uygulama düğüm ve bir savunma düğümü HA kümeyle dağıtır. Savunma dışındaki tüm düğümleri standart DS3v2 VM boyutlarıdır. Standart DS2v2 savunma düğümdür. Bu küme 38 çekirdek gerektirir.
- Büyük: bir HA kümeyle üç ana düğüm, iki altyapı düğümleri, altı uygulama düğümleri ve bir savunma düğümü dağıtır. Ana ve altyapı düğümleri standart DS3v2 VM boyutları, uygulama düğümleri standart DS4v2 VM boyutları olan ve standart DS2v2 savunma düğümdür. Bu küme 70 çekirdek gerektirir.

Azure bulut sağlayıcısının yapılandırmasını Orta ve büyük küme boyutlarını isteğe bağlıdır. Küçük küme boyutu Azure bulut sağlayıcısı yapılandırma seçeneği sağlamaz.

## <a name="connect-to-the-openshift-cluster"></a>OpenShift kümeye bağlanın

Dağıtım tamamlandığında, tarayıcı kullanarak OpenShift Konsolu'na bağlanmak `OpenShift Console Uri`. Alternatif olarak, aşağıdaki komutu kullanarak OpenShift ana bağlanabilir:

```bash
$ ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olduğunda, kullanabileceğiniz [az grubu Sil](/cli/azure/group#delete) OpenShift küme, kaynak grubu kaldırmak için komut ve ilişkili tüm kaynakları.

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

- [POST dağıtım görevleri](./openshift-post-deployment.md)
- [OpenShift dağıtım sorunlarını giderme](./openshift-troubleshooting.md)
- [OpenShift kapsayıcı Platform ile çalışmaya başlama](https://docs.openshift.com/container-platform/3.6/getting_started/index.html)
