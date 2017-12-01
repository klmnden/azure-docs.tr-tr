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
ms.openlocfilehash: bf4b0f67a4c3667fb0c0cb826a822d6090c36375
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
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
> * [Bir küme veya ölçeklendirme](service-fabric-tutorial-scale-cluster.md)
> * [Yükseltme küme çalışma zamanı](service-fabric-tutorial-upgrade-cluster.md)
> * [API Management Service Fabric ile dağıtma](service-fabric-tutorial-deploy-api-management.md)

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce:
- Bir Azure aboneliğiniz yoksa, oluşturma bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Yükleme [doku CLI hizmeti](service-fabric-cli.md)
- Yükleme [Azure CLI 2.0](/cli/azure/install-azure-cli)

Aşağıdaki yordamlar, beş düğümlü Service Fabric kümesi oluşturun. Service Fabric kümesi Azure kullanımda çalıştırarak ücrete maliyeti hesaplamak için [Azure fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/).

## <a name="introduction"></a>Giriş
Bu öğretici, bir tek düğümlü türdeki beş düğümden oluşan bir küme Azure sanal ağında içine dağıtır.

[Service Fabric kümesi](service-fabric-deploy-anywhere.md), mikro hizmetlerin dağıtılıp yönetildiği, ağa bağlı bir sanal veya fiziksel makine kümesidir. Kümeleri makineler binlerce ölçeklendirebilirsiniz. Bir makine ya da bir kümenin parçasıysa VM düğüm denir. Her düğüme bir düğüm adı (dize) atanır. Düğümleri yerleşim özellikleri gibi özelliklere sahiptir.

Düğüm türü küme boyutu, sayı ve sanal makineler kümesinin özelliklerini tanımlar. Her tanımlanan düğüm türü olarak ayarlanan bir [sanal makine ölçek kümesi](/azure/virtual-machine-scale-sets/), Azure işlem kaynağı dağıtmak ve sanal makinelerin bir koleksiyon kümesi olarak yönetmek için kullanın. Her düğüm türü sonra ölçeklendirilebilir veya Aşağı bağımsız olarak, farklı bağlantı noktalarının açık olması ve farklı kapasite ölçümlerini olabilir. Düğüm türleri, küme düğümleri, "ön uç" veya "arka uç" gibi bir dizi için rolleri tanımlamak için kullanılır.  Kümenizi birden fazla düğüm türü olabilir, ancak en az beş VM'ler üretim kümeleri (veya test kümeleri için en az üç sanal makineleri) için birincil düğüm türü olmalıdır.  [Service Fabric Sistem Hizmetleri](service-fabric-technical-overview.md#system-services) birincil düğüm türü düğümlerinde yerleştirilir.

## <a name="cluster-capacity-planning"></a>Küme kapasitesi planlama
Bu öğretici, bir tek düğümlü türdeki beş düğümden oluşan bir küme dağıtır.  Tüm üretim Küme dağıtımı için kapasite planlamasının önemli bir adımdır. Bu işlemin bir parçası olarak dikkate alınması gereken bazı şeyleri burada bulabilirsiniz.

- Düğüm sayısı kümeniz türleri 
- Her bir düğüm türü (örneğin boyutu, birincil, İnternete ve VM'ler) özellikleri
- Küme güvenilirlik ve dayanıklılık özellikleri

Daha fazla bilgi için bkz: [küme kapasite planlama konuları](service-fabric-cluster-capacity.md).

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
Ağ kaynaklarını dağıtma tamamladıktan sonra sonraki adım bir Service Fabric kümesi sanal ağ alt ağındaki ve Service Fabric kümesi için belirlenmiş NSG dağıtmaktır. Var olan bir VNET ve alt ağ (Bu makalede daha önce dağıttığınız) için bir küme dağıtımı Resource Manager şablonu gerektirir.  Daha fazla bilgi için bkz: [Azure Resource Manager kullanarak bir küme oluşturma](service-fabric-cluster-creation-via-arm.md). Bu öğretici seri için şablon sanal ağ, alt ağ ve bir önceki adımda ayarladığınız NSG adları kullanmak üzere önceden yapılandırılmıştır.  

Aşağıdaki Resource Manager şablonu ve parametre dosyasını karşıdan yükleyin:
- [linuxcluster.JSON][cluster-arm]
- [linuxcluster.Parameters.JSON][cluster-parameters-arm]

Güvenli bir küme oluşturmak için bu şablonu kullanın.  Küme düğümü, düğümü iletişimi güvenli hale getirmek ve bir yönetim istemcisi için küme yönetim uç noktalarının kimliğini doğrulamak için kullanılan bir X.509 sertifikası sertifikasıdır.  Küme sertifika ayrıca bir SSL HTTPS yönetim API'si ve Service Fabric Explorer için HTTPS üzerinden sağlar. Azure anahtar kasası, Azure Service Fabric kümeleri sertifikalarını yönetmek için kullanılır.  Bir küme Azure'da dağıtıldığında, Azure kaynak sağlayıcısı Service Fabric kümeleri oluşturmak için sorumlu sertifikaları anahtar Kasası'nı çeker ve VM'ler kümede yükler. 

Küme sertifikası veya test amacıyla bir sertifika yetkilisinden (CA) bir sertifika kullanmak, otomatik olarak imzalanan bir sertifika oluşturun. Küme sertifika gerekir:

- özel anahtarı içerir.
- bir kişisel bilgi değişimi (.pfx) dosyası verilebilir anahtar değişimi için oluşturulamıyor.
- Service Fabric kümesi erişmek için kullandığınız etki alanı ile eşleşen bir konu adına sahip. Bu eşleştirme, kümenin HTTPS yönetim uç noktaları ve Service Fabric Explorer için SSL sağlamak için gereklidir. İçin bir sertifika yetkilisinden (CA) bir SSL sertifikası alınamıyor. cloudapp.azure.com etki alanı. Özel etki alanı adı, kümeniz için edinmeniz gerekir. Bir CA'dan bir sertifika isteme, sertifikanın konu adı, kümeniz için kullandığınız özel etki alanı adı eşleşmelidir.

Bu boş parametreleri doldurmanız *linuxcluster.parameters.json* dosya dağıtımınız için:

|Parametre|Değer|
|---|---|
|Admınpassword|Parola #1234|
|adminUserName|vmadmin|
|clusterName|mysfcluster|

Bırakın **certificateThumbprint**, **certificateUrlValue**, ve **sourceVaultValue** parametreleri otomatik olarak imzalanan bir sertifika oluşturmak için boş.  Daha önce bir anahtar Kasası'na yüklenen var olan bir sertifikayı kullanmak istiyorsanız, bu parametre değerlerini doldurun.

Aşağıdaki komut dosyası kullanan [az BT küme oluşturmak](/cli/azure/sf/cluster?view=azure-cli-latest#az_sf_cluster_create) komutu ve şablon azure'da yeni bir küme dağıtmak için. Cmdlet ayrıca Azure'da yeni bir anahtar kasası oluşturur, anahtar Kasası'na yeni bir otomatik olarak imzalanan sertifika ekler ve sertifika dosyasını yerel olarak indirir. Diğer parametreleri kullanarak, varolan bir sertifikayı ve/veya anahtar kasası belirtebilirsiniz [az BT küme oluşturmak](/cli/azure/sf/cluster?view=azure-cli-latest#az_sf_cluster_create) komutu.

```azurecli
Password="q6D7nN%6ck@6"
Subject="mysfcluster.southcentralus.cloudapp.azure.com"
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

Ardından, kümenizi ölçeklendirme öğrenmek için aşağıdaki öğretici ilerleyin.
> [!div class="nextstepaction"]
> [Küme ölçeklendirme](service-fabric-tutorial-scale-cluster.md)


[network-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.json
[network-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.parameters.json

[cluster-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/linuxcluster.json
[cluster-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/linuxcluster.parameters.json
