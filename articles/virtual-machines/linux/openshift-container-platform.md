---
title: Azure'da OpenShift kapsayıcı platformu dağıtma | Microsoft Docs
description: Azure'da OpenShift kapsayıcı platformu dağıtın.
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
ms.date: 02/04/2018
ms.author: haroldw
ms.openlocfilehash: 1d869d822cdeb0051836a5fc5f01eb69c523f9e3
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "57995542"
---
# <a name="deploy-openshift-container-platform-in-azure"></a>Azure'da OpenShift kapsayıcı platformu dağıtma

Azure'da OpenShift kapsayıcı platformu dağıtmak için birkaç yöntemden birini kullanabilirsiniz:

- Gerekli Azure altyapı bileşenlerini el ile dağıtın ve izleyin [OpenShift kapsayıcı platformu belgeleri](https://docs.openshift.com/container-platform).
- Mevcut bir kullanabilirsiniz [Resource Manager şablonu](https://github.com/Microsoft/openshift-container-platform/) , OpenShift kapsayıcı platformu küme dağıtımını basitleştirir.
- Başka bir seçenek kullanmaktır [Azure Marketi'nde teklif](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.openshift-container-platform?tab=Overview).

Red Hat aboneliklerini tüm seçenekler için gereklidir. Dağıtım sırasında Red Hat Enterprise Linux örneği Red Hat aboneliğe kayıtlı ve OpenShift kapsayıcı platformu için yetkilendirmeleri içeren havuzu kimliğine bağlı.
Bir geçerli Red Hat Abonelik Yöneticisi (RHSM) kullanıcı adı, parola ve havuzu kimliği olduğundan emin olun Bir etkinleştirme anahtarı, kuruluş kimliği ve Havuz kimliği kullanabilirsiniz Oturum açarak bu bilgileri doğrulayabilirsiniz https://access.redhat.com.

## <a name="deploy-using-the-openshift-container-platform-resource-manager-template"></a>OpenShift kapsayıcı platformu Resource Manager şablonu kullanarak dağıtma

Resource Manager şablonu kullanarak dağıtmak için giriş parametreleri sağlamak için bir parametre dosyası kullanın. Daha fazla dağıtımı özelleştirmek için GitHub depo çatalını oluşturmanız ve uygun öğeleri değiştirin.

Bazı ortak özelleştirme seçenekleri içerir, ancak bunlarla sınırlı değildir:

- Savunma VM boyutu (azuredeploy.json değişkeninde)
- Adlandırma kuralları (azuredeploy.json değişkenlerinde)
- OpenShift küme özellikleri, hosts dosyasını (deployOpenShift.sh) değiştirme

### <a name="configure-the-parameters-file"></a>Parametre dosyasını yapılandırın

[OpenShift kapsayıcı platformu şablon](https://github.com/Microsoft/openshift-container-platform) sahip birden fazla dalı OpenShift kapsayıcı platformu farklı sürümleri için kullanılabilir.  Gereksinimlerinize göre doğrudan deposundan dağıtabilirsiniz veya deponun çatalını oluşturup dağıtmadan önce şablonları veya betik özel değişiklikleri yapın.

Kullanım `appId` değerinden daha önce oluşturduğunuz için hizmet sorumlusu `aadClientId` parametresi.

Aşağıdaki örnek, tüm gerekli girişleri ile azuredeploy.parameters.json adlı bir parametre dosyası gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "masterVmSize": {
            "value": "Standard_E2s_v3"
        },
        "infraVmSize": {
            "value": "Standard_D4s_v3"
        },
        "nodeVmSize": {
            "value": "Standard_D4s_v3"
        },
        "cnsVmSize": {
            "value": "Standard_E4s_v3"
        },
        "osImageType": {
            "value": "defaultgallery"
        },
        "marketplaceOsImage": {
            "value": {
                "publisher": "RedHat",
                "offer": "RHEL",
                "sku": "7-RAW",
                "version": "latest"
            }
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
        "enableMetrics": {
            "value": "true"
        },
        "enableLogging": {
            "value": "false"
        },
        "enableCNS": {
            "value": "false"
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
        "rhsmBrokerPoolId": {
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
        "masterClusterDnsType": {
            "value": "default"
        },
        "masterClusterDns": {
            "value": "console.contoso.com"
        },
        "routingSubDomainType": {
            "value": "nipio"
        },
        "routingSubDomain": {
            "value": "routing.contoso.com"
        },
        "virtualNetworkNewOrExisting": {
            "value": "new"
        },
        "virtualNetworkName": {
            "value": "openshiftvnet"
        },
        "addressPrefixes": {
            "value": "10.0.0.0/14"
        },
        "masterSubnetName": {
            "value": "mastersubnet"
        },
        "masterSubnetPrefix": {
            "value": "10.1.0.0/16"
        },
        "infraSubnetName": {
            "value": "infrasubnet"
        },
        "infraSubnetPrefix": {
            "value": "10.2.0.0/16"
        },
        "nodeSubnetName": {
            "value": "nodesubnet"
        },
        "nodeSubnetPrefix": {
            "value": "10.3.0.0/16"
        },
        "existingMasterSubnetReference": {
            "value": "/subscriptions/abc686f6-963b-4e64-bff4-99dc369ab1cd/resourceGroups/vnetresourcegroup/providers/Microsoft.Network/virtualNetworks/openshiftvnet/subnets/mastersubnet"
        },
        "existingInfraSubnetReference": {
            "value": "/subscriptions/abc686f6-963b-4e64-bff4-99dc369ab1cd/resourceGroups/vnetresourcegroup/providers/Microsoft.Network/virtualNetworks/openshiftvnet/subnets/masterinfrasubnet"
        },
        "existingCnsSubnetReference": {
            "value": "/subscriptions/abc686f6-963b-4e64-bff4-99dc369ab1cd/resourceGroups/vnetresourcegroup/providers/Microsoft.Network/virtualNetworks/openshiftvnet/subnets/cnssubnet"
        },
        "existingNodeSubnetReference": {
            "value": "/subscriptions/abc686f6-963b-4e64-bff4-99dc369ab1cd/resourceGroups/vnetresourcegroup/providers/Microsoft.Network/virtualNetworks/openshiftvnet/subnets/nodesubnet"
        },
        "masterClusterType": {
            "value": "public"
        },
        "masterPrivateClusterIp": {
            "value": "10.1.0.200"
        },
        "routerClusterType": {
            "value": "public"
        },
        "routerPrivateClusterIp": {
            "value": "10.2.0.201"
        },
        "routingCertType": {
            "value": "selfsigned"
        },
        "masterCertType": {
            "value": "selfsigned"
        },
        "proxySettings": {
            "value": "none"
        },
        "httpProxyEntry": {
            "value": "none"
        },
        "httpsProxyEntry": {
            "value": "none"
        },
        "noProxyEntry": {
            "value": "none"
        }
    }
}
```

Parametreleri özel bilgileri ile değiştirin.

Farklı sürümleri, bu nedenle kullandığınız dal için gerekli parametreleri doğrulayın farklı parametreler olabilir.

### <a name="deploy-using-azure-cli"></a>Azure CLI kullanarak dağıtma

> [!NOTE] 
> Aşağıdaki komut, Azure CLI'yı 2.0.8 gerektirir veya üzeri. CLI sürümü ile doğrulayabilirsiniz `az --version` komutu. CLI sürümünü güncelleştirmek için bkz: [Azure CLI yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latesti).

Aşağıdaki örnek, OpenShift kümeyi ve tüm ilgili kaynakları myOpenShiftCluster ile bir dağıtım adı openshiftrg, adlı bir kaynak grubuna dağıtır. Şablon, doğrudan GitHub deposunu ve adlı azuredeploy.parameters.json dosyasının dosya kullanılan yerel bir parametre olarak başvuruluyor.

```azurecli 
az group deployment create -g openshiftrg --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-container-platform/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

Dağıtım dağıtılan düğüm ve yapılandırılan seçeneklerin, toplam sayısına göre tamamlamak için en az 30 dakika sürer. OpenShift Konsolu URL'sini ve savunma DNS FQDN dağıtım tamamlandığında terminale yazdırır.

```json
{
  "Bastion DNS FQDN": "bastiondns4hawllzaavu6g.eastus.cloudapp.azure.com",
  "OpenShift Console URL": "http://openshiftlb.eastus.cloudapp.azure.com/console"
}
```

Dağıtım tamamlamak ekleyin bekleniyor komut satırı bağlamanın istemiyorsanız `--no-wait` grubu dağıtımı için seçenekleri biri olarak. Dağıtım çıkışı, Azure portalında kaynak grubu için Dağıtım bölümündeki alınabilir.
 
## <a name="deploy-using-the-openshift-container-platform-azure-marketplace-offer"></a>OpenShift kapsayıcı platformu Azure Marketi'nde teklif kullanarak dağıtma

OpenShift kapsayıcı platformu Azure'a dağıtmak için en basit yolu kullanmaktır [Azure Marketi'nde teklif](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.openshift-container-platform?tab=Overview).

En basit seçenek budur ancak özelleştirme özellikleri sınırlıdır. Market teklifi, aşağıdaki yapılandırma seçeneklerini içerir:

- **Ana düğümler**: Yapılandırılabilir örneği ile ana düğümler üç (3) yazın.
- **Kızılötesi düğümleri**: Üç (3) altyapısı düğümleri yapılandırılabilir örneği ile yazın.
- **Düğümleri**: Düğüm sayısı (2 ile 9 arasında) yapılandırılabilir örnek türü yanı sıra.
- **Disk türü**: Yönetilen diskler kullanılır.
- **Ağ**: Yeni veya var olan ağ yanı sıra özel CIDR aralığı desteği.
- **CNS**: CNS etkinleştirilebilir.
- **Ölçümleri**: Ölçümleri etkinleştirilebilir.
- **Günlüğe kaydetme**: Günlüğe kaydetme etkinleştirilebilir.
- **Azure bulut sağlayıcısı**: Etkinleştirilebilir.

## <a name="connect-to-the-openshift-cluster"></a>OpenShift kümeye bağlanma

Dağıtım tamamlandığında, bağlantı, dağıtım çıktı bölümünden alın. OpenShift konsoluna tarayıcınızla kullanarak bağlanma `OpenShift Console URL`. Alternatif olarak, savunma ana bilgisayar için SSH kullanabilirsiniz. Yönetici kullanıcı adı clusteradmin ve savunma genel IP DNS FQDN bastiondns4hawllzaavu6g.eastus.cloudapp.azure.com olduğu yerde bir örneği verilmiştir:

```bash
$ ssh clusteradmin@bastiondns4hawllzaavu6g.eastus.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kullanım [az grubu Sil](/cli/azure/group) komutunu kullanarak kaynak grubunu, OpenShift küme kaldırmak için ve artık gerekmediğinde tüm ilgili kaynakları.

```azurecli 
az group delete --name openshiftrg
```

## <a name="next-steps"></a>Sonraki adımlar

- [Dağıtım sonrası görevler](./openshift-post-deployment.md)
- [Azure'da OpenShift dağıtım sorunlarını giderme](./openshift-troubleshooting.md)
- [OpenShift kapsayıcı platformu ile çalışmaya başlama](https://docs.openshift.com/container-platform)

### <a name="documentation-contributors"></a>Belgelere Katkıda Bulunanlar

Vincent Power (vincepower) ve Alfred Sin (asinn826) Katkıları bu belgeleri güncel tutmak için teşekkür ederiz!
