---
title: Azure'da OpenShift kapsayıcı platformu dağıtma | Microsoft Docs
description: Azure'da OpenShift kapsayıcı platformu dağıtın.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: mdotson
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/18/2019
ms.author: haroldw
ms.openlocfilehash: 664099322bef3ac85d980fbe5e43dcc49cba862b
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65411555"
---
# <a name="deploy-openshift-container-platform-in-azure"></a>Azure'da OpenShift kapsayıcı platformu dağıtma

Azure'da OpenShift kapsayıcı platformu dağıtmak için birkaç yöntemden birini kullanabilirsiniz:

- Gerekli Azure altyapı bileşenlerini el ile dağıtın ve izleyin [OpenShift kapsayıcı platformu belgeleri](https://docs.openshift.com/container-platform).
- Mevcut bir kullanabilirsiniz [Resource Manager şablonu](https://github.com/Microsoft/openshift-container-platform/) , OpenShift kapsayıcı platformu küme dağıtımını basitleştirir.
- Başka bir seçenek kullanmaktır [Azure Marketi'nde teklif](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.openshift-container-platform?tab=Overview).

Red Hat aboneliklerini tüm seçenekler için gereklidir. Dağıtım sırasında Red Hat Enterprise Linux örneği Red Hat aboneliğe kayıtlı ve OpenShift kapsayıcı platformu için yetkilendirmeleri içeren havuzu kimliğine bağlı.
Bir geçerli Red Hat Abonelik Yöneticisi (RHSM) kullanıcı adı, parola ve havuzu kimliği olduğundan emin olun Bir etkinleştirme anahtarı, kuruluş kimliği ve Havuz kimliği kullanabilirsiniz Oturum açarak bu bilgileri doğrulayabilirsiniz https://access.redhat.com.


## <a name="deploy-using-the-openshift-container-platform-resource-manager-template"></a>OpenShift kapsayıcı platformu Resource Manager şablonu kullanarak dağıtma

### <a name="private-clusters"></a>Özel kümeleri

Özel OpenShift kümelerini dağıtma gerektirir (web Konsolu) ana yük dengeleyiciye veya için ilişkili bir genel IP sahip birden fazla değil ınfra yük dengeleyici (yönlendirici).  Özel bir küme genellikle özel bir DNS sunucusu (varsayılan Azure DNS), özel etki alanı adı (contoso.com gibi) ve önceden tanımlanmış bir sanal ağ kullanır.  Özel kümeler için tüm uygun alt ağları ve DNS sunucusu ayarlarını ile sanal ağınızda önceden yapılandırmanız gerekir.  Ardından **existingMasterSubnetReference**, **existingInfraSubnetReference**, **existingCnsSubnetReference**, ve  **existingNodeSubnetReference** kullanmak için mevcut alt ağı küme tarafından belirtmek için.

Özel ana seçtiyseniz (**masterClusterType**özel =), bir statik özel IP için belirtilmesi gerekiyor **masterPrivateClusterIp**.  Bu IP ana yük dengeleyicinin ön ucu atanır.  IP kullanımda ve ana alt ağ için CIDR içinde olmalıdır.  **masterClusterDnsType** "özel" ve asıl için DNS adı sağlanmalıdır ayarlanmalıdır **masterClusterDns**.  DNS adı ve ana düğüm üzerinde konsoluna erişmek için kullanılan statik özel IP harita gerekir.

Özel yönlendirici seçtiyseniz (**routerClusterType**özel =), bir statik özel IP için belirtilmesi gerekiyor **routerPrivateClusterIp**.  Bu IP ön ucunu atanacak ınfra yük dengeleyici.  IP CIDR için içinde olmalıdır ınfra alt ağ ve kullanımda.  **routingSubDomainType** yönlendirme için sağlanmalıdır için "özel" ve joker DNS adına ayarlanmalıdır **routingSubDomain**.  

Özel asıl ve özel yönlendirici seçtiyseniz, özel etki alanı adı da için girilmelidir **domainName**

Başarılı dağıtımdan sonra savunma düğümü yapabileceğiniz ssh içine bir genel IP ile yalnızca düğümüdür.  Ana düğüm ortak erişimi için yapılandırılmış olsa bile, bunlar için ssh kullanıma sunulmaz erişim.

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
        "_artifactsLocation": {
            "value": "https://raw.githubusercontent.com/Microsoft/openshift-container-platform/master"
        },
        "location": {
            "value": "eastus"
        },
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
            "value": "changeme"
        },
        "openshiftClusterPrefix": {
            "value": "changeme"
        },
        "minorVersion": {
            "value": "69"
        },
        "masterInstanceCount": {
            "value": 3
        },
        "infraInstanceCount": {
            "value": 3
        },
        "nodeInstanceCount": {
            "value": 3
        },
        "cnsInstanceCount": {
            "value": 3
        },
        "osDiskSize": {
            "value": 64
        },
        "dataDiskSize": {
            "value": 64
        },
        "cnsGlusterDiskSize": {
            "value": 128
        },
        "adminUsername": {
            "value": "changeme"
        },
        "enableMetrics": {
            "value": "false"
        },
        "enableLogging": {
            "value": "false"
        },
        "enableCNS": {
            "value": "false"
        },
        "rhsmUsernameOrOrgId": {
            "value": "changeme"
        },
        "rhsmPoolId": {
            "value": "changeme"
        },
        "rhsmBrokerPoolId": {
            "value": "changeme"
        },
        "sshPublicKey": {
            "value": "GEN-SSH-PUB-KEY"
        },
        "keyVaultSubscriptionId": {
            "value": "255a325e-8276-4ada-af8f-33af5658eb34"
        },
        "keyVaultResourceGroup": {
            "value": "changeme"
        },
        "keyVaultName": {
            "value": "changeme"
        },
        "enableAzure": {
            "value": "true"
        },
        "aadClientId": {
            "value": "changeme"
        },
        "domainName": {
            "value": "contoso.com"
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
            "value": "apps.contoso.com"
        },
        "virtualNetworkNewOrExisting": {
            "value": "new"
        },
        "virtualNetworkName": {
            "value": "changeme"
        },
        "addressPrefixes": {
            "value": "10.0.0.0/14"
        },
        "masterSubnetName": {
            "value": "changeme"
        },
        "masterSubnetPrefix": {
            "value": "10.1.0.0/16"
        },
        "infraSubnetName": {
            "value": "changeme"
        },
        "infraSubnetPrefix": {
            "value": "10.2.0.0/16"
        },
        "nodeSubnetName": {
            "value": "changeme"
        },
        "nodeSubnetPrefix": {
            "value": "10.3.0.0/16"
        },
        "existingMasterSubnetReference": {
            "value": "/subscriptions/abc686f6-963b-4e64-bff4-99dc369ab1cd/resourceGroups/vnetresourcegroup/providers/Microsoft.Network/virtualNetworks/openshiftvnet/subnets/mastersubnet"
        },
        "existingInfraSubnetReference": {
            "value": "/subscriptions/abc686f6-963b-4e64-bff4-99dc369ab1cd/resourceGroups/vnetresourcegroup/providers/Microsoft.Network/virtualNetworks/openshiftvnet/subnets/infrasubnet"
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
            "value": "10.2.0.200"
        },
        "routingCertType": {
            "value": "selfsigned"
        },
        "masterCertType": {
            "value": "selfsigned"
        }
    }
}
```

Parametreleri özel bilgileri ile değiştirin.

Farklı sürümleri, bu nedenle kullandığınız dal için gerekli parametreleri doğrulayın farklı parametreler olabilir.

### <a name="azuredeployparametersjson-file-explained"></a>azuredeploy. Parameters.json dosyasının açıklaması

| Özellik | Açıklama | Geçerli seçenekler şunlardır: | Varsayılan Değer |
|----------|-------------|---------------|---------------|
| `_artifactsLocation`  | Yapıtları (json, betikleri, vb.) için URL |  |  https:\//raw.githubusercontent.com/Microsoft/openshift-container-platform/master  |
| `location` | Kaynakların dağıtılacağı azure bölgesi |  |  |
| `masterVmSize` | Ana VM'nin boyutu. Azuredeploy.json dosyasında listelenen izin verilen VM boyutları arasından seçin |  | Standard_E2s_v3 |
| `infraVmSize` | Boyutu Infra VM. Azuredeploy.json dosyasında listelenen izin verilen VM boyutları arasından seçin |  | Standard_D4s_v3 |
| `nodeVmSize` | Uygulama düğümüne VM boyutu. Azuredeploy.json dosyasında listelenen izin verilen VM boyutları arasından seçin |  | Standard_D4s_v3 |
| `cnsVmSize` | Kapsayıcı yerel depolama (CNS) düğümü VM boyutu. Azuredeploy.json dosyasında listelenen izin verilen VM boyutları arasından seçin |  | Standard_E4s_v3 |
| `osImageType` | Kullanılacak RHEL görüntüsü. defaultgallery: İsteğe bağlı; Market: üçüncü taraf görüntüsü | defaultgallery <br> market | defaultgallery |
| `marketplaceOsImage` | Varsa `osImageType` Market ve ardından 'Yayımcı', 'teklif', 'sku', 'sürümü' Market teklifi için uygun değerleri girin. Bu parametre bir nesne türü. |  |  |
| `storageKind` | Kullanılacak depolama türü  | Yönetilen<br> Yönetilmeyen | Yönetilen |
| `openshiftClusterPrefix` | Tüm düğümleri için konak adları yapılandırmak için kullanılan önek küme.  1 ila 20 karakter |  | mycluster |
| `minoVersion` | OpenShift kapsayıcı platformu 3.11 dağıtmak için ikincil sürümü |  | 69 |
| `masterInstanceCount` | Dağıtmak için ana düğüm sayısı | 1, 3, 5 | 3 |
| `infraInstanceCount` | Sayısı altyapısı dağıtmak için düğümleri | 1, 2, 3 | 3 |
| `nodeInstanceCount` | Dağıtılacak düğüm sayısı | 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30 | 2 |
| `cnsInstanceCount` | Dağıtılacak CNS düğüm sayısı | 3, 4 | 3 |
| `osDiskSize` | İşletim sistemi diski (GB cinsinden) sanal makine için boyutu | 64, 128, 256, 512, 1024, 2048 | 64 |
| `dataDiskSize` | Docker birimi (GB cinsinden) için düğümler eklemek için veri diski boyutu | 32, 64, 128, 256, 512, 1024, 2048 | 64 |
| `cnsGlusterDiskSize` | (GB cinsinden glusterfs tarafından kullanılmak CNS düğümleri eklemek için veri diski boyutu | 32, 64, 128, 256, 512, 1024, 2048 | 128 |
| `adminUsername` | Hem işletim sistemi (VM) oturum açma ve ilk OpenShift kullanıcı için yönetici kullanıcı adı |  | ocpadmin |
| `enableMetrics` | Ölçümleri sağlar. Daha fazla kaynak altyapısı VM için doğru boyutu şekilde seçin. ölçüm gerektirir | true <br> false | false |
| `enableLogging` | Günlük kaydını etkinleştirin. elasticsearch pod gerektirir 8 GB RAM şekilde Infra VM için uygun bir boyut seçin | true <br> false | false |
| `enableCNS` | Kapsayıcı yerel depolamayı etkinleştir | true <br> false | false |
| `rhsmUsernameOrOrgId` | Red Hat Abonelik Yöneticisi kullanıcı adı veya kuruluş kimliği |  |  |
| `rhsmPoolId` | OpenShift yetkilendirmelere işlem düğümlerinin içeren Red Hat Abonelik Yöneticisi Havuz kimliği |  |  |
| `rhsmBrokerPoolId` | OpenShift yetkilendirmelere ve altyapısı yöneticileri için düğümleri içeren Red Hat Abonelik Yöneticisi Havuz kimliği. Farklı bir havuz kimliği yoksa, 'rhsmPoolId' olarak aynı Havuz kimliği girin |  |
| `sshPublicKey` | SSH ortak anahtarınızı buraya kopyalayın. |  |  |
| `keyVaultSubscriptionId` | Key Vault içeren aboneliği abonelik kimliği |  |  |
| `keyVaultResourceGroup` | Key Vault içeren kaynak grubunun adı |  |  |
| `keyVaultName` | Oluşturduğunuz anahtar kasasının adı |  |  |
| `enableAzure` | Azure bulut sağlayıcısını etkinleştir | true <br> false | true |
| `aadClientId` | Azure Active Directory istemci da için hizmet sorumlusu uygulama kimliği olarak bilinen kimliği |  |  |
| `domainName` | (Eğer varsa) kullanılacak özel etki alanı adı adı. "None" değilse için dağıtımı tam olarak özel kümeye ayarlayın |  | yok |
| `masterClusterDnsType` | OpenShift web Konsolu için etki alanı türü. 'default' kullanacağı ana DNS etiketi altyapısı genel IP. 'custom', kendi adınızı tanımlamanızı sağlar | varsayılan <br> özel | varsayılan |
| `masterClusterDns` | 'Custom' için seçtiyseniz, OpenShift web konsoluna erişmek için kullanılacak özel DNS adı `masterClusterDnsType` |  | Console.contoso.com |
| `routingSubDomainType` | Varsa 'nipio' ayarlayın `routingSubDomain` nip.io kullanır.  Yönlendirme için kullanmak istediğiniz kendi etki alanınız varsa 'custom' kullanın | nipio <br> özel | nipio |
| `routingSubDomain` | 'Custom' için seçtiyseniz, yönlendirme için kullanmak istediğiniz joker DNS adı `routingSubDomainType` |  | Apps.contoso.com |
| `virtualNetworkNewOrExisting` | Yeni bir sanal ağ oluşturma veya var olan bir sanal ağı kullanmayı seçin | Mevcut <br> yeni | yeni |
| `virtualNetworkResourceGroupName` | 'New' için seçtiyseniz, yeni sanal ağın kaynak grubunun adı `virtualNetworkNewOrExisting` |  | resourceGroup().name |
| `virtualNetworkName` | Yeni sanal ağının 'new' için seçtiyseniz oluşturmak için ad `virtualNetworkNewOrExisting` |  | openshiftvnet |
| `addressPrefixes` | Yeni sanal ağın adres ön eki |  | 10.0.0.0/14 |
| `masterSubnetName` | Ana alt ağın adı |  | mastersubnet |
| `masterSubnetPrefix` | İçin ana alt ağ - olarak kullanılan CIDR addressPrefix bir alt kümesi olması gerekir |  | 10.1.0.0/16 |
| `infraSubnetName` | Adını ınfra alt ağ |  | infrasubnet |
| `infraSubnetPrefix` | İçin kullanılan CIDR ınfra alt ağ - addressPrefix bir alt kümesi olması gerekir |  | 10.2.0.0/16 |
| `nodeSubnetName` | Düğüm alt ağın adı |  | nodesubnet |
| `nodeSubnetPrefix` | Düğümü için alt ağ - olarak kullanılan CIDR addressPrefix bir alt kümesi olması gerekir |  | 10.3.0.0/16 |
| `existingMasterSubnetReference` | Ana düğüm için mevcut alt ağı tam başvuru. Yeni sanal ağ oluşturuyorsanız gerekmeyen / alt ağ |  |  |
| `existingInfraSubnetReference` | Başvuru için mevcut alt düğümleri ınfra tam. Yeni sanal ağ oluşturuyorsanız gerekmeyen / alt ağ |  |  |
| `existingCnsSubnetReference` | Mevcut alt ağı CNS düğümlerin tam başvuru. Yeni sanal ağ oluşturuyorsanız gerekmeyen / alt ağ |  |  |
| `existingNodeSubnetReference` | Mevcut alt ağı işlem düğümleri için tam başvuru. Yeni sanal ağ oluşturuyorsanız gerekmeyen / alt ağ |  |  |
| `masterClusterType` | Küme özel veya genel ana düğüm kullanıp kullanmayacağını belirtin. Özel seçilirse, bir genel IP aracılığıyla İnternet'e ana düğüm sunulmamasını. Bunun yerine, belirtilen özel IP kullanır `masterPrivateClusterIp` | genel <br> özel | genel |
| `masterPrivateClusterIp` | Özel ana düğüm seçilirse, ardından özel bir IP adresi kullanmak için ana düğümleri için iç yük dengeleyici tarafından belirtilmesi gerekir. Bu statik bir IP CIDR bloğu ana alt ağ için ve zaten kullanımda içinde olması gerekir. Genel ana düğüm seçtiyseniz, bu değer kullanılmayacak ancak yine de belirtilmesi gerekir |  | 10.1.0.200 |
| `routerClusterType` | Küme özel veya ortak altyapısı düğümleri kullanıp kullanmayacağını belirtin. Özel seçilirse, bir genel IP aracılığıyla İnternet'e düğümleri ınfra sunulmamasını. Bunun yerine, belirtilen özel IP kullanır `routerPrivateClusterIp` | genel <br> özel | genel |
| `routerPrivateClusterIp` | Özel düğümler ınfra seçilir ve ardından için özel bir IP adresi belirtilmelidir için iç yük dengeleyici tarafından ınfra düğümleri kullanın. Bu statik bir IP CIDR bloğu ana alt ağ için ve zaten kullanımda içinde olması gerekir. Ortak altyapısı Seçili düğüm, bu değer kullanılmayacak ancak yine de belirtilmesi gerekir |  | 10.2.0.200 |
| `routingCertType` | Yönlendirme etki alanı veya varsayılan otomatik olarak imzalanan sertifika için özel sertifika kullanma - yönergeleri **özel sertifikaları** bölümü | selfsigned <br> özel | selfsigned |
| `masterCertType` | Ana etki alanı veya varsayılan otomatik olarak imzalanan sertifika için özel sertifika kullanma - yönergeleri **özel sertifikaları** bölümü | selfsigned <br> özel | selfsigned |

<br>

### <a name="deploy-using-azure-cli"></a>Azure CLI kullanarak dağıtma

> [!NOTE] 
> Aşağıdaki komut, Azure CLI'yı 2.0.8 gerektirir veya üzeri. CLI sürümü ile doğrulayabilirsiniz `az --version` komutu. CLI sürümünü güncelleştirmek için bkz: [Azure CLI yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latesti).

Aşağıdaki örnek, OpenShift kümeyi ve tüm ilgili kaynakları myOpenShiftCluster ile bir dağıtım adı openshiftrg, adlı bir kaynak grubuna dağıtır. Şablon, doğrudan GitHub deposunu ve adlı azuredeploy.parameters.json dosyasının dosya kullanılan yerel bir parametre olarak başvuruluyor.

```azurecli 
az group deployment create -g openshiftrg --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-container-platform/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

Dağıtım, dağıtılan düğüm ve yapılandırılan seçeneklerin, toplam sayısına göre tamamlamak için en az 60 dakika sürer. OpenShift Konsolu URL'sini ve savunma DNS FQDN dağıtım tamamlandığında terminale yazdırır.

```json
{
  "Bastion DNS FQDN": "bastiondns4hawllzaavu6g.eastus.cloudapp.azure.com",
  "OpenShift Console URL": "http://openshiftlb.eastus.cloudapp.azure.com/console"
}
```

Dağıtım tamamlamak ekleyin bekleniyor komut satırı bağlamanın istemiyorsanız `--no-wait` grubu dağıtımı için seçenekleri biri olarak. Dağıtım çıkışı, Azure portalında kaynak grubu için Dağıtım bölümündeki alınabilir.

## <a name="connect-to-the-openshift-cluster"></a>OpenShift kümeye bağlanma

Dağıtım tamamlandığında, bağlantı, dağıtım çıktı bölümünden alın. OpenShift konsoluna tarayıcınızla kullanarak bağlanma **OpenShift Konsolu URL'si**. Ayrıca savunma ana bilgisayara SSH kullanabilirsiniz. Yönetici kullanıcı adı clusteradmin ve savunma genel IP DNS FQDN bastiondns4hawllzaavu6g.eastus.cloudapp.azure.com olduğu yerde bir örneği verilmiştir:

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
