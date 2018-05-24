---
title: Azure'da Linux Service Fabric Kümesi oluşturma | Microsoft Docs
description: Bu öğreticide, Azure CLI kullanarak mevcut bir Azure sanal ağına Linux Service Fabric kümesi dağıtmayı öğrenirsiniz.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/22/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: ff57aec76171b45dbebff928f2898bd5f91ec1c2
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34365551"
---
# <a name="tutorial-deploy-a-service-fabric-linux-cluster-into-an-azure-virtual-network"></a>Öğretici: Azure sanal ağına Service Fabric Linux kümesi dağıtma
Bu öğretici, bir dizinin birinci bölümüdür. Öğreticide, Azure CLI ile bir şablon kullanarak bir [Azure sanal ağına (VNET)](../virtual-network/virtual-networks-overview.md) ve [ağ güvenlik grubuna (NSG) ](../virtual-network/security-overview.md) Linux Service Fabric kümesi dağıtma hakkında bilgi verilir. Öğretici tamamladığınızda, bulutta çalışan ve uygulama dağıtabileceğiniz bir kümeniz olur. PowerShell kullanarak Windows kümesi oluşturmak için bkz. [Azure’da güvenli bir Windows kümesi oluşturma](service-fabric-tutorial-create-vnet-and-windows-cluster.md).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure CLI kullanarak Azure’da VNet oluşturma
> * Azure CLI kullanarak Azure’da güvenli bir Service Fabric kümesi oluşturma
> * X.509 sertifikasıyla kümenin güvenliğini sağlama
> * Service Fabric CLI kullanarak kümeye bağlanma
> * Bir kümeyi kaldırma

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Azure’da güvenli bir küme oluşturma
> * [Bir kümenin ölçeğini daraltma veya genişletme](service-fabric-tutorial-scale-cluster.md)
> * [Bir kümenin çalışma zamanını yükseltme](service-fabric-tutorial-upgrade-cluster.md)
> * [Service Fabric ile API Management dağıtma](service-fabric-tutorial-deploy-api-management.md)

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce:
- Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun
- [Service Fabric CLI](service-fabric-cli.md)'yı yükleyin
- [Azure CLI 2.0](/cli/azure/install-azure-cli)’ı yükleyin

Aşağıdaki yordamlarda beş düğümlü bir Service Fabric kümesi oluşturulur. Azure’da Service Fabric kümesi çalıştırmaktan kaynaklanan maliyetleri hesaplamak için [Azure Fiyatlandırma Hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/)’nı kullanın.

## <a name="key-concepts"></a>Önemli kavramlar
[Service Fabric kümesi](service-fabric-deploy-anywhere.md), mikro hizmetlerin dağıtılıp yönetildiği, ağa bağlı bir sanal veya fiziksel makine kümesidir. Kümeler binlerce makine içerecek şekilde ölçeklendirilebilir. Bir kümenin parçası olan makine veya VM’lere düğüm denir. Her düğüme bir düğüm adı (bir dize) atanır. Düğümlerin yerleşim özellikleri gibi özellikleri vardır.

Düğüm türü, kümedeki bir grup sanal makinenin boyutunu, sayısını ve özelliklerini tanımlar. Tanımlanan her düğüm türü, bir sanal makine koleksiyonunu küme halinde yönetmek için kullandığınız bir Azure işlem kaynağı olan [sanal makine ölçek kümesi](/azure/virtual-machine-scale-sets/) olarak ayarlanır. Daha sonra, her düğüm türünün ölçeği birbirinden bağımsız olarak artırılabilir veya azaltılabilir, her düğüm türünde farklı bağlantı noktası kümeleri açık olabilir ve farklı kapasite ölçümleri yapılabilir. Düğüm türleri, bir düğüm kümesinin "ön uç" veya "arka uç" şeklindeki rolünün tanımlanması için kullanılır.  Kümenizde birden çok düğüm türü olabilir, ancak üretim kümeleri için birincil düğüm türünde en az beş VM (veya test kümeleri için en az üç VM) olmalıdır.  [Service Fabric sistem hizmetleri](service-fabric-technical-overview.md#system-services), birincil düğüm türündeki düğümlere yerleştirilir.

Kümenin güvenliği bir küme sertifikası ile sağlanır. Küme sertifikası, düğümler arası iletişimin güvenliğini sağlamak ve bir yönetim istemcisinde küme yönetimi uç noktalarının kimliğini doğrulamak için kullanılan bir X.509 sertifikasıdır.  Küme sertifikası ayrıca, HTTPS üzerinden HTTPS yönetim API’si ve Service Fabric Explorer için SSL sağlar. Otomatik olarak imzalanan sertifikalar, test kümeleri için kullanışlıdır.  Üretim kümeleri için küme sertifikası olarak bir sertifika yetkilisinden (CA) alınan bir sertifika kullanın.

Küme sertifikası:

- Özel anahtar içermelidir.
- Bir Kişisel Bilgi Değişimi (.pfx) dosyasına aktarılabilen anahtar değişimi için oluşturulmuş olmalıdır.
- Service Fabric kümesine erişmek için kullandığınız etki alanıyla eşleşen bir konu adına sahip olmalıdır. Kümenin HTTPS yönetim uç noktalarına ve Service Fabric Explorer’a yönelik SSL sağlanması için bu eşleşme gereklidir. Bir sertifika yetkilisinden (CA) .cloudapp.azure.com etki alanı için SSL sertifikası edinemezsiniz. Kümeniz için özel bir etki alanı adı edinmeniz gerekir. CA’dan sertifika istediğinizde sertifikanın konu adı, kümeniz için kullandığınız özel etki alanı adıyla eşleşmelidir.

Azure’da Service Fabric kümelerine ait sertifikaları yönetmek için Azure Key Vault kullanılır.  Azure’da bir küme dağıtıldığında, Azure Service Fabric kümeleri oluşturmaktan sorumlu Azure kaynak sağlayıcısı sertifikaları Key Vault’tan çeker ve küme VM’lerine yükler.

Bu öğreticide tek bir küme türündeki beş düğüme sahip bir küme dağıtılır. Bununla birlikte, tüm küme dağıtımları için [kapasite planlaması](service-fabric-cluster-capacity.md) önemli bir adımdır. Bu süreç kapsamında dikkat etmeniz gerekenler şunlardır:

- Kümeniz için gerekli düğüm sayısı ve türleri 
- Her düğüm türünün özellikleri (örneğin boyut, birincil, İnternet’e yönelik ve VM sayısı)
- Kümenin güvenilirlik ve dayanıklılık özellikleri

## <a name="download-and-explore-the-template"></a>Şablonu indirin ve keşfedin
Aşağıdaki Resource Manager şablonu dosyalarını indirin:
- [vnet-linuxcluster.json][template]
- [vnet-linuxcluster.parameters.json][parameters]

[vnet-linuxcluster.json][template] tarafından aşağıdakiler dahil çeşitli kaynaklar dağıtılır.

### <a name="service-fabric-cluster"></a>Service Fabric kümesi
Aşağıdaki özelliklere sahip bir Linux kümesi dağıtılır:
- tek bir düğüm türü 
- birincil düğüm türünde beş düğüm (şablon parametrelerinden yapılandırılabilir)
- İşletim sistemi: Ubuntu 16.04 LTS (şablon parametrelerinde yapılandırılabilir)
- sertifikanın güvenliğinin sağlanması (şablon parametrelerinde yapılandırılabilir)
- [DNS hizmeti](service-fabric-dnsservice.md) etkin
- Bronz [dayanıklılık düzeyi](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) (şablon parametrelerinde yapılandırılabilir)
- Gümüş [güvenilirlik düzeyi](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) (şablon parametrelerinden yapılandırılabilir)
- istemci bağlantısı uç noktası: 19000 (şablon parametrelerinde yapılandırılabilir)
- HTTP ağ geçidi uç noktası: 19080 (şablon parametrelerinde yapılandırılabilir)

### <a name="azure-load-balancer"></a>Azure yük dengeleyici
Bir yük dengeleyici dağıtılır ve aşağıdaki bağlantı noktalarını araştırıp bunların ayarlanmasını yönetir:
- istemci bağlantı uç noktası: 19000
- HTTP ağ geçidi uç noktası: 19080 
- uygulama bağlantı noktası: 80
- uygulama bağlantı noktası: 443

### <a name="virtual-network-subnet-and-network-security-group"></a>Sanal ağ, alt ağ ve ağ güvenlik grubu
Sanal ağ, alt ağ ve ağ güvenlik grubunun adları şablon parametrelerinde bildirilir.  Sanal ağ ve alt ağın adres alanları da şablon parametrelerinde bildirilir:
- sanal ağ adres alanı: 10.0.0.0/16
- Service Fabric alt ağ adres alanı: 10.0.2.0/24

Ağ güvenlik grubunda aşağıdaki gelen trafik kuralları etkindir. Şablon değişkenlerini değiştirerek bağlantı noktası değerlerini değiştirebilirsiniz.
- ClientConnectionEndpoint (TCP): 19000
- HttpGatewayEndpoint (HTTP/TCP): 19080
- SMB : 445
- Internodecommunication - 1025, 1026, 1027
- Kısa ömürlü bağlantı noktası aralığı – 49152-65534 (en az 256 bağlantı noktası gerekir)
- Uygulamanın kullanımına yönelik bağlantı noktaları: 80 ve 443
- Uygulama bağlantı noktası aralığı – 49152-65534 (hizmetler arası iletişim için kullanılır ve diğerlerinden farklı olarak Yük dengeleyicide açılmaz)
- Diğer tüm bağlantı noktalarını engelleyin

Başka bir uygulama bağlantı noktası gerekiyorsa Microsoft.Network/loadBalancers kaynağı ile Microsoft.Network/networkSecurityGroups kaynağını trafiğin geçmesine izin verecek şekilde ayarlamanız gerekir.

## <a name="set-template-parameters"></a>Şablon parametrelerini ayarlama
[vnet-cluster.parameters.json][parameters] parametre dosyası tarafından kümenin ve ilişkili kaynakların dağıtılması için kullanılan birçok değer bildirilir. Dağıtımınız için değiştirmeniz gerekebilecek bazı parametreler:

|Parametre|Örnek değer|Notlar|
|---|---||
|adminUserName|vmadmin| Küme VM’leri için yönetici kullanıcı adı. |
|adminPassword|Password#1234| Küme VM’leri için yönetici parolası.|
|clusterName|mysfcluster123| Kümenin adı. |
|location|southcentralus| Kümenin konumu. |
|certificateThumbprint|| <p>Otomatik olarak imzalanan bir sertifika oluşturuluyor veya sertifika dosyası sağlanıyorsa değer boş olmalıdır.</p><p>Daha önce bir anahtar kasasına yüklenmiş mevcut bir sertifikayı kullanmak için sertifika parmak izi değerini girin. Örneğin: "6190390162C988701DB5676EB81083EA608DCCF3". </p>| 
|certificateUrlValue|| <p>Otomatik olarak imzalanan bir sertifika oluşturuluyor veya sertifika dosyası sağlanıyorsa değer boş olmalıdır.</p><p>Daha önce bir anahtar kasasına yüklenmiş mevcut bir sertifikayı kullanmak için sertifika URL’sini girin. Örneğin, "https://mykeyvault.vault.azure.net:443/secrets/mycertificate/02bea722c9ef4009a76c5052bcbf8346".</p>|
|sourceVaultValue||<p>Otomatik olarak imzalanan bir sertifika oluşturuluyor veya sertifika dosyası sağlanıyorsa değer boş olmalıdır.</p><p>Daha önce bir anahtar kasasına yüklenmiş mevcut bir sertifikayı kullanmak için kaynak kasa değerini girin. Örneğin: "/subscriptions/333cc2c84-12fa-5778-bd71-c71c07bf873f/resourceGroups/MyTestRG/providers/Microsoft.KeyVault/vaults/MYKEYVAULT".</p>|


<a id="createvaultandcert" name="createvaultandcert_anchor"></a>

## <a name="deploy-the-virtual-network-and-cluster"></a>Sanal ağı ve kümeyi dağıtma
Ardından, ağ topolojisini ayarlayın ve Service Fabric kümesini dağıtın. [vnet-linuxcluster.json][template] Resource Manager şablonu bir sanal ağın (VNET) yanı sıra Service Fabric için bir alt ağ ve ağ güvenlik grubu (NSG) da oluşturur. Şablon tarafından sertifika güvenliği etkin bir küme de dağıtılır.  Üretim kümeleri için küme sertifikası olarak bir sertifika yetkilisinden (CA) alınan bir sertifika kullanın. Test kümelerinin güvenliğinin sağlanması için otomatik olarak imzalanan bir sertifika kullanılabilir.

Aşağıdaki betik, mevcut bir sertifikayla güvenliği sağlanmış yeni bir küme dağıtmak için [az sf cluster create](/cli/azure/sf/cluster?view=azure-cli-latest#az_sf_cluster_create) komutunu ve şablonunu kullanır. Ayrıca, komut tarafından Azure’da yeni bir anahtar kasası oluşturulur ve sertifikanız karşıya yüklenir.

```azurecli
ResourceGroupName="sflinuxclustergroup"
Location="southcentralus"  
Password="q6D7nN%6ck@6"
VaultName="linuxclusterkeyvault"
VaultGroupName="linuxclusterkeyvaultgroup"
CertPath="C:\MyCertificates\MyCertificate.pem"

# sign in to your Azure account and select your subscription
az login
az account set --subscription <guid>

# Create a new resource group for your deployment and give it a name and a location.
az group create --name $ResourceGroupName --location $Location

# Create the Service Fabric cluster.
az sf cluster create --resource-group $ResourceGroupName --location $Location \
   --certificate-password $Password --certificate-file $CertPath \
   --vault-name $VaultName --vault-resource-group $ResourceGroupName  \
   --template-file vnet-linuxcluster.json --parameter-file vnet-linuxcluster.parameters.json
```

## <a name="connect-to-the-secure-cluster"></a>Güvenli kümeye bağlanma
Anahtarınızı kullanarak, Service Fabric CLI `sfctl cluster select` komutunu kullanan kümeye bağlanın.  Otomatik olarak imzalanan sertifika için yalnızca **--no-verify** seçeneğinin kullanılması gerektiğini göz önünde bulundurun.

```azurecli
sfctl cluster select --endpoint https://aztestcluster.southcentralus.cloudapp.azure.com:19080 \
--pem ./aztestcluster201709151446.pem --no-verify
```

Bağlı olup olmadığınızı ve kümenin sağlıklı olup olmadığını denetlemek için `sfctl cluster health` komutunu kullanın.

```azurecli
sfctl cluster health
```

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu öğretici serisindeki diğer makalelerde, az önce oluşturduğunuz küme kullanılır. Hemen bir sonraki makaleye geçmeyecekseniz ücretlendirmeden kaçınmak için kümeyi silmek isteyebilirsiniz. Kümeyi ve kullandığı tüm kaynakları silmenin en basit yolu, kaynak grubunun silinmesidir.

Azure’da oturum açın ve kümeyi kaldırmak istediğiniz abonelik kimliğini seçin.  Abonelik kimliğinizi, [Azure portalında](http://portal.azure.com) oturum açarak öğrenebilirsiniz. [az group delete](/cli/azure/group?view=azure-cli-latest#az_group_delete) komutunu kullanarak kaynak grubunu ve tüm küme kaynaklarını silin.

```azurecli
az group delete --name $ResourceGroupName
```

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure CLI kullanarak Azure’da VNet oluşturma
> * Azure CLI kullanarak Azure’da güvenli bir Service Fabric kümesi oluşturma
> * X.509 sertifikasıyla kümenin güvenliğini sağlama
> * Service Fabric CLI kullanarak kümeye bağlanma
> * Bir kümeyi kaldırma

Ardından, kümenizi nasıl ölçeklendirebileceğinizi öğrenmek üzere aşağıdaki öğreticiye geçin.
> [!div class="nextstepaction"]
> [Küme Ölçeklendirme](service-fabric-tutorial-scale-cluster.md)


[template]:https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/cluster-tutorial/vnet-linuxcluster.json
[parameters]:https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/cluster-tutorial/vnet-linuxcluster.parameters.json
