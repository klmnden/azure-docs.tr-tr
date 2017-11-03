---
title: "Azure'da bir Linux Service Fabric kümesi oluşturun | Microsoft Docs"
description: "Mevcut Azure sanal ağda Azure CLI kullanarak bir Linux Service Fabric kümesi dağıtmayı öğrenin."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/26/2017
ms.author: ryanwi
ms.openlocfilehash: b2542af86be236b8d575fcaf7687222cd74af661
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2017
---
# <a name="deploy-a-service-fabric-linux-cluster-into-an-azure-virtual-network"></a>Azure sanal ağda bir Service Fabric Linux kümesi dağıtma
Bu öğretici bir dizi birini bir parçasıdır. Mevcut bir Azure sanal ağı (VNET) Linux Service Fabric kümesine dağıtmak ve Azure CLI kullanarak alt net öğreneceksiniz. İşlemi tamamladığınızda, uygulamaları dağıtabileceğiniz bulutta çalıştıran bir kümeye sahip. PowerShell kullanarak bir Windows kümesi oluşturmak için bkz: [Azure'da güvenli bir Windows kümesi oluşturma](service-fabric-tutorial-create-vnet-and-windows-cluster.md).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure CLI kullanarak Azure'da bir sanal ağ oluşturma
> * Azure CLI kullanarak Azure'da güvenli bir Service Fabric kümesi oluştur
> * Küme bir X.509 sertifikası ile güvenli
> * Service Fabric CLI kullanarak kümeye bağlanın
> * Bir küme Kaldır

Bu öğretici serisinde öğrenin nasıl yapılır:
> [!div class="checklist"]
> * Azure üzerinde güvenli bir küme oluşturun
> * [API Management Service Fabric ile dağıtma](service-fabric-tutorial-deploy-api-management.md)

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce:
- Bir Azure aboneliğiniz yoksa, oluşturma bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Yükleme [doku CLI hizmeti](service-fabric-cli.md)
- Yükleme [Azure CLI 2.0](/cli/azure/install-azure-cli)

Aşağıdaki yordamlar, beş düğümlü Service Fabric kümesi oluşturun. Service Fabric kümesi Azure kullanımda çalıştırarak ücrete maliyeti hesaplamak için [Azure fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/).

## <a name="sign-in-to-azure-and-select-your-subscription"></a>Azure'da oturum açın ve aboneliğinizi seçin
Bu kılavuz, Azure CLI kullanır. Yeni bir oturum başlattığınızda, Azure hesabınızda oturum açın ve Azure komutları çalıştırmadan önce aboneliğinizi seçin.
 
Azure hesabınızda oturum açmak için aşağıdaki komutu çalıştırın, aboneliğinizi seçin:

```azurecli
az login
az account set --subscription <guid>
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Dağıtımınız için yeni bir kaynak grubu oluşturun ve bir ad ve bir konum girin.

```azurecli
ResourceGroupName="sflinuxclustergroup"
Location="southcentralus"
az group create --name $ResourceGroupName --location $Location
```

## <a name="deploy-the-network-topology"></a>Ağ topolojisi dağıtmak
Ardından, ağ topolojisini için API Management ve Service Fabric kümesi dağıtılacak ayarlayın. [Network.json] [ network-arm] Resource Manager şablonu yapılandırılmış bir sanal ağ (VNET) ve ayrıca bir alt ağ ve ağ güvenlik grubu (NSG) ve bir alt ağ ve NSG Service Fabric için API Management oluşturmak için . Daha fazla hakkında sanal ağlar, alt ağları ve Nsg'ler öğrenin [burada](../virtual-network/virtual-networks-overview.md).

[Network.parameters.json] [ network-parameters-arm] parametreler dosyası alt ağları ve Service Fabric ve API Management dağıtmak Nsg'ler adlarını içerir.  API Management dağıtıldığı [öğretici aşağıdaki](service-fabric-tutorial-deploy-api-management.md). Bu kılavuz, parametre değerlerini değiştirilmesi gerekmez. Service Fabric Resource Manager şablonları bu değerleri kullanın.  Değerler burada değiştirildiyse, onları bu öğreticide kullanılan diğer Resource Manager şablonları olarak değiştirmeniz gerekir ve [dağıtmak API Management Öğreticisi](service-fabric-tutorial-deploy-api-management.md). 

Aşağıdaki Resource Manager şablonu ve parametre dosyasını karşıdan yükleyin:
- [Network.JSON][network-arm]
- [Network.Parameters.JSON][network-parameters-arm]

Ağ Kurulumu Resource Manager şablonu ve parametre dosyaları dağıtmak için aşağıdaki komut dosyasını kullanın:

```azurecli
az group deployment create \
    --name VnetDeployment \
    --resource-group $ResourceGroupName \
    --template-file network.json \
    --parameters @network.parameters.json
```
<a id="createvaultandcert" name="createvaultandcert_anchor"></a>
## <a name="deploy-the-service-fabric-cluster"></a>Service Fabric kümesi dağıtma
Ağ kaynaklarını dağıtma tamamladıktan sonra sonraki adım bir Service Fabric kümesi sanal ağ alt ağındaki ve Service Fabric kümesi için belirlenmiş NSG dağıtmaktır. Var olan bir VNET ve alt ağ (Bu makalede daha önce dağıttığınız) için bir küme dağıtımı Resource Manager şablonu gerektirir.  Daha fazla bilgi için bkz: [Azure Resource Manager kullanarak bir küme oluşturma](service-fabric-cluster-creation-via-arm.md). Bu öğretici seri için şablon sanal ağ, alt ağ ve bir önceki adımda ayarladığınız NSG adları kullanmak üzere önceden yapılandırılmıştır.  Aşağıdaki Resource Manager şablonu ve parametre dosyasını karşıdan yükleyin:
- [linuxcluster.JSON][cluster-arm]
- [linuxcluster.Parameters.JSON][cluster-parameters-arm]

Boş dolgu **clusterName**, **adminUserName**, ve **Admınpassword** parametrelerinde *linuxcluster.parameters.json* dosyası Dağıtımınız için.  Bırakın **certificateThumbprint**, **certificateUrlValue**, ve **sourceVaultValue** parametreler boş otomatik olarak imzalanan sertifika oluşturmak istiyorsanız.  Daha önce bir anahtar Kasası'na yüklenen var olan bir sertifikayı varsa, bu parametre değerlerini doldurun.

Resource Manager şablonu ve parametre dosyalarını kullanarak küme dağıtmak için aşağıdaki komut dosyasını kullanın.  Kendinden imzalı bir sertifika içinde belirtilen anahtar kasası oluşturulur ve küme güvenliğini sağlamak için kullanılır.  Sertifikanın da yerel olarak yüklenir.

```azurecli
Password="q6D7nN%6ck@6"
Subject="aztestcluster.southcentralus.cloudapp.azure.com"
VaultName="linuxclusterkeyvault"
az group create --name $ResourceGroupName --location $Location

az sf cluster create --resource-group $ResourceGroupName --location $Location \
   --certificate-output-folder . --certificate-password $Password --certificate-subject-name $Subject \
   --vault-name $VaultName --vault-resource-group $ResourceGroupName  \
   --template-file linuxcluster.json --parameter-file linuxcluster.parameters.json

```

## <a name="connect-to-the-secure-cluster"></a>Güvenli kümeye bağlanın
Service Fabric CLI kullanarak kümeye bağlanın `sfctl cluster select` anahtarınızı kullanarak komutu.  Unutmayın, yalnızca **--Hayır doğrulayın** otomatik olarak imzalanan bir sertifika seçeneği.

```azurecli
sfctl cluster select --endpoint https://aztestcluster.southcentralus.cloudapp.azure.com:19080 \
--pem ./aztestcluster201709151446.pem --no-verify
```

Bağlı ve küme sağlıklı olduğundan emin olun kullanarak `sfctl cluster health` komutu.

```azurecli
sfctl cluster health
```

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu öğretici serisinde diğer makaleleri oluşturduğunuz küme kullanın. Sonraki makalede açın hemen taşıma değilseniz, ücret oluşmasını önlemek için Kümeyi silmek isteyebilirsiniz. Kümeyi ve kullandığı tüm kaynakları silmenin en basit yolu, kaynak grubunun silinmesidir.

Azure'da oturum açma ve küme kaldırmak istediğiniz abonelik kimliği seçin.  Abonelik Kimliğiniz için oturum açarak bulabileceğiniz [Azure portal](http://portal.azure.com). Kaynak grubu ve kullanarak tüm küme kaynakları silmek [az grubu Sil](/cli/azure/group?view=azure-cli-latest#az_group_delete) komutu.

```azurecli
az group delete --name $ResourceGroupName
```

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure CLI kullanarak Azure'da bir sanal ağ oluşturma
> * Azure CLI kullanarak Azure'da güvenli bir Service Fabric kümesi oluştur
> * Küme bir X.509 sertifikası ile güvenli
> * Service Fabric CLI kullanarak kümeye bağlanın
> * Bir küme Kaldır

Ardından, API Management Service Fabric ile dağıtma hakkında bilgi edinmek için aşağıdaki öğreticiyi ilerleyin.
> [!div class="nextstepaction"]
> [API Yönetimi dağıtma](service-fabric-tutorial-deploy-api-management.md)


[network-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.json
[network-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.parameters.json

[cluster-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/linuxcluster.json
[cluster-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/linuxcluster.parameters.json
