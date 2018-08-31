---
title: Azure'da OpenShift kapsayıcı platformu dağıtma | Microsoft Docs
description: Azure'da OpenShift kapsayıcı platformu dağıtın.
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
ms.openlocfilehash: a275df4567053149688694315ff24ac1ad7f711f
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43186923"
---
# <a name="deploy-openshift-container-platform-in-azure"></a>Azure'da OpenShift kapsayıcı platformu dağıtma

Azure'da OpenShift kapsayıcı platformu dağıtmak için birkaç yöntemden birini kullanabilirsiniz:

- Gerekli Azure altyapı bileşenlerini el ile dağıtın ve ardından OpenShift kapsayıcı platformu izleyin [belgeleri](https://docs.openshift.com/container-platform/3.10/welcome/index.html).
- Mevcut bir kullanabilirsiniz [Resource Manager şablonu](https://github.com/Microsoft/openshift-container-platform/) , OpenShift kapsayıcı platformu küme dağıtımını basitleştirir.
- Başka bir seçenek kullanmaktır [Azure Marketi'nde teklif](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.openshift-container-platform?tab=Overview).

Red Hat aboneliklerini tüm seçenekler için gereklidir. Dağıtım sırasında Red Hat Enterprise Linux örneği Red Hat aboneliğe kayıtlı ve OpenShift kapsayıcı platformu için yetkilendirmeleri içeren havuzu kimliğine bağlı.
Geçerli Red Hat Abonelik Yöneticisi (RHSM) kullanıcı adı, parola ve havuzu kimliği olduğundan emin olun Oturum açarak bu bilgileri doğrulayabilirsiniz https://access.redhat.com.

## <a name="deploy-by-using-the-openshift-container-platform-resource-manager-template"></a>OpenShift kapsayıcı platformu Resource Manager şablonu kullanarak dağıtın

Resource Manager şablonu kullanarak dağıtmak için giriş parametreleri sağlamak için bir parametre dosyası kullanın. Giriş parametreleri kullanarak kapsamında değildir dağıtım öğeleri özelleştirmek için GitHub depo çatalını oluşturmanız ve uygun öğeleri değiştirin.

Bazı ortak özelleştirme seçenekleri dahil ancak bunlarla sınırlı değildir:

- Sanal ağın CIDR (azuredeploy.json değişkeninde)
- Savunma VM boyutu (azuredeploy.json değişkeninde)
- Adlandırma kuralları (azuredeploy.json değişkenlerinde)
- OpenShift küme özellikleri, hosts dosyasını (deployOpenShift.sh) değiştirme

### <a name="configure-the-parameters-file"></a>Parametre dosyasını yapılandırın

Kullanım `appId` değerinden daha önce oluşturduğunuz için hizmet sorumlusu `aadClientId` parametresi. 

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

Özel bilgileri ile köşeli ayraçlar içine öğeleri değiştirin.

### <a name="deploy-by-using-azure-cli"></a>Azure CLI kullanarak dağıtma

> [!NOTE] 
> Aşağıdaki komut, Azure CLI'yı 2.0.8 gerektirir veya üzeri. CLI sürümü ile doğrulayabilirsiniz `az --version` komutu. CLI sürümünü güncelleştirmek için bkz: [Azure CLI 2.0 yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latesti).

Aşağıdaki örnek, OpenShift kümeyi ve tüm ilgili kaynakları myOpenShiftCluster ile bir dağıtım adı myResourceGroup, adlı bir kaynak grubuna dağıtır. Şablon, doğrudan GitHub deposunu ve adlı azuredeploy.parameters.json dosyasının dosya kullanılan yerel bir parametre olarak başvuruluyor.

```azurecli 
az group deployment create -g myResourceGroup --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-container-platform/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

Dağıtımın tamamlanması toplam dağıtılan düğüm sayısına bağlı olarak en az 30 dakika sürer. OpenShift konsol OpenShift ana yazdırır dağıtım tamamlandığında terminal için DNS adı ve URL.

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200"
}
```

## <a name="deploy-by-using-the-openshift-container-platform-azure-marketplace-offer"></a>OpenShift kapsayıcı platformu Azure Marketi'nde teklif kullanarak dağıtma

OpenShift kapsayıcı platformu Azure'a dağıtmak için en basit yolu kullanmaktır [Azure Marketi'nde teklif](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.openshift-container-platform?tab=Overview).

En basit seçenek budur ancak özelleştirme özellikleri sınırlıdır. Bu teklif, üç yapılandırma seçenekleri içerir:

- **Küçük**: bir ana düğüm, altyapı bir düğümü, iki uygulama düğümü ve bir savunma düğümü ile yüksek olmayan kullanılabilirlik (HA) küme dağıtır. Tüm düğümleri olan standart DS2v2 VM boyutları. Bu küme, 10 toplam çekirdek gerektirir ve küçük ölçekli sınamak için idealdir.
- **Orta**: üç ana düğüm, iki altyapı düğüm, dört uygulama düğüm ve bir savunma düğümü ile bir HA kümesi dağıtır. Savunma düğüm dışındaki tüm düğümleri olan standart DS3v2 VM boyutları. Standart DS2v2 savunma düğümüdür. Bu küme 38 çekirdek gerektirir.
- **Büyük**: üç ana düğüm, iki altyapı düğümleri, altı uygulama düğümleri ve bir savunma düğümü ile bir HA kümesi dağıtır. Ana ve altyapı düğümleri olan standart DS3v2 VM boyutları. Uygulama düğümleri olan standart DS4v2 VM boyutları ve standart DS2v2 savunma düğümüdür. Bu küme 70 çekirdek gereklidir.

Azure bulut çözümü sağlayıcısı yapılandırmasını, Orta ve büyük bir küme boyutlarını için isteğe bağlıdır. Küçük bir küme boyutu, Azure bulut çözümü sağlayıcısı yapılandırma seçeneği sağlamaz.

## <a name="connect-to-the-openshift-cluster"></a>OpenShift kümeye bağlanma

Dağıtım tamamlandığında OpenShift konsoluyla birlikte tarayıcınızı kullanarak bağlanmak `OpenShift Console Uri`. Alternatif olarak, aşağıdaki komutu kullanarak OpenShift ana bağlanabilir:

```bash
$ ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kullanım [az grubu Sil](/cli/azure/group#az_group_delete) komutunu kullanarak kaynak grubunu, OpenShift küme kaldırmak için ve artık gerekmediğinde tüm ilgili kaynakları.

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

- [Dağıtım sonrası görevler](./openshift-post-deployment.md)
- [Azure'da OpenShift dağıtım sorunlarını giderme](./openshift-troubleshooting.md)
- [OpenShift kapsayıcı platformu ile çalışmaya başlama](https://docs.openshift.com/container-platform/3.6/getting_started/index.html)
