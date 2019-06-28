---
title: API sunucusu yetkili IP aralıkları Azure Kubernetes Service (AKS)
description: Azure Kubernetes Service (AKS) API sunucusuna erişim için bir IP adresi aralığı kullanarak kümenizin güvenliğini sağlama hakkında bilgi edinin
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 05/06/2019
ms.author: iainfou
ms.openlocfilehash: 9ec48c8ed924293a5ffea903fe03a9830dcd1184
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67329425"
---
# <a name="preview---secure-access-to-the-api-server-using-authorized-ip-address-ranges-in-azure-kubernetes-service-aks"></a>Önizleme - IP adresi aralıklarını Azure Kubernetes Service (AKS) kullanarak API sunucusu için güvenli erişim yetkili

Kubernetes API sunucusu kaynakları oluşturmak veya düğüm sayısını ölçeklendirmek için kümedeki gibi eylemleri gerçekleştirmek için isteklerini alır. API sunucusu ile etkileşimde bulunmak ve bir kümeyi yönetmek için merkezi bir yoludur. Küme güvenliği artırmak ve saldırıları en aza indirmek için API sunucusu yalnızca sınırlı bir IP adresi aralıklarını kümesinden erişilebilir olmalıdır.

Bu makalede denetim düzlemi isteklerine sınırlamak için API yetkili sunucusu IP adresi aralıklarını kullanmayı gösterir. Bu özellik şu anda önizleme sürümündedir.

> [!IMPORTANT]
> AKS Önizleme özellikleri, Self Servis, kabul etme. Görüş ve hata topluluğumuza toplamak için sağlanır. Önizleme'de, bu özelliklerin üretim kullanılmak üzere geliştirilmiş değildir. Genel Önizleme Özellikleri 'en yüksek çaba' destek kapsamında ayrılır. İş saatleri Pasifik Saat dilimi sırasında (Pasifik Saati) yalnızca AKS teknik destek ekipleri Yardım kullanılabilir. Ek bilgi için lütfen aşağıdaki destek makaleleri bakın:
>
> * [AKS destek ilkeleri][aks-support-policies]
> * [Azure desteği SSS][aks-faq]

## <a name="before-you-begin"></a>Başlamadan önce

API sunucusu iş oluşturduğunuz yeni AKS kümeleri için yalnızca yetkili IP aralıkları. Bu makalede Azure CLI kullanarak bir AKS kümesi oluşturma işlemini gösterir.

Azure CLI Sürüm 2.0.61 gerekir veya daha sonra yüklü ve yapılandırılmış. Çalıştırma `az --version` sürümü bulmak için. Gerekirse yüklemek veya yükseltmek bkz [Azure CLI yükleme][install-azure-cli].

### <a name="install-aks-preview-cli-extension"></a>Aks önizlemesini CLI uzantısını yükleme

API sunucusu yetkili IP aralıkları yapılandırmak için ihtiyacınız *aks önizlemesini* CLI 0.4.1 uzantı sürümü veya üzeri. Yükleme *aks önizlemesini* uzantısını Azure CLI kullanarak [az uzantısı ekleme][az-extension-add] command, then check for any available updates using the [az extension update][az-extension-update] komutu:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="register-feature-flag-for-your-subscription"></a>Aboneliğiniz için özellik bayrağı Kaydet

Sunucu IP aralıklarını yetkili API'yi kullanmak için önce aboneliğinizde özellik bayrağı etkinleştirin. Kaydedilecek *APIServerSecurityPreview* özellik bayrağı, kullanın [az özelliği kayıt][az-feature-register] komutu aşağıdaki örnekte gösterildiği gibi:

> [!CAUTION]
> Bir Abonelikteki bir özellik kaydettiğinizde, bu özellik şu anda kaydını yapamazsınız. Bazı Önizleme özellikleri etkinleştirdikten sonra varsayılan ardından aboneliği için oluşturulan tüm AKS kümeleri için kullanılabilir. Önizleme özellikleri üretim Aboneliklerde etkinleştirmeyin. Önizleme özellikleri test ve geri bildirim toplamak için ayrı bir abonelik kullanın.

```azurecli-interactive
az feature register --name APIServerSecurityPreview --namespace Microsoft.ContainerService
```

Gösterilecek durum için birkaç dakika sürer *kayıtlı*. Kayıt kullanarak durumu denetleyebilirsiniz [az özellik listesi][az-feature-list] komutu:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/APIServerSecurityPreview')].{Name:name,State:properties.state}"
```

Hazır olduğunuzda, kayıt yenileme *Microsoft.ContainerService* kullanarak kaynak sağlayıcısını [az provider register][az-provider-register] komutu:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="limitations"></a>Sınırlamalar

API sunucusu yetkili IP aralıkları yapılandırdığınızda aşağıdaki sınırlamalar geçerlidir:

* API sunucusu ile iletişimi de engellendi olarak şu anda Azure geliştirme alanları kullanamazsınız.

## <a name="overview-of-api-server-authorized-ip-ranges"></a>API sunucusu genel bakış yetkili IP aralıkları

Temel alınan Kubernetes API'ları nasıl sunulur Kubernetes API sunucusudur. Bu bileşen etkileşimi için yönetim araçları gibi sağlar `kubectl` veya Kubernetes panosunu. AKS, bir tek kiracılı Küme Yöneticisi, ayrılmış bir API sunucusu sağlar. Varsayılan olarak, API sunucusuna bir genel IP adresi atanır ve rol tabanlı erişim denetimi (RBAC) kullanarak erişimi denetlemesi gerekir.

Aksi durumda ortak olarak erişilebilen AKS denetim düzlemi güvenli erişimi / API sunucusu etkinleştirebilmek ve kullanabilmek IP aralıklarını yetkili. Bu yetkili IP aralıkları yalnızca API sunucusu ile iletişim kurmak için tanımlanan IP adres aralıklarına izin verin. Bu yetkili IP aralıkları parçası olmayan bir IP adresinden API sunucusuna yapılan bir istek engellendi. Kullanıcılar ve istediklerinde eylemleri korunmasına yetki vermek için RBAC kullanmaya devam etmelidir.

API sunucusu ve diğer küme bileşenleri hakkında daha fazla bilgi için bkz. [Kubernetes kavramlarını AKS için çekirdek][concepts-clusters-workloads].

## <a name="create-an-aks-cluster"></a>AKS kümesi oluşturma

Yeni AKS kümeleri için yalnızca iş API sunucusu yetkili IP aralıkları. Kümenin parçası işlemi oluştururken, yetkili IP aralıkları etkinleştiremezsiniz. Etkinleştirmeyi deneyin. işlem yetkili IP aralıkları kümesinin parçası olarak oluşturun, küme düğümleri çıkış IP adresi bu noktada tanımlanmamış gibi dağıtım sırasında API sunucusuna erişemiyor.

İlk olarak kullanarak bir küme oluşturun [az aks oluşturma][az-aks-create] komutu. Aşağıdaki örnekte adlı tek düğümlü bir küme oluşturur *myAKSCluster* adlı kaynak grubunda *myResourceGroup*.

```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location eastus

# Create an AKS cluster
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 1 \
    --generate-ssh-keys

# Get credentials to access the cluster
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

## <a name="create-outbound-gateway-for-firewall-rules"></a>Güvenlik duvarı kuralları için giden ağ geçidi oluşturma

Sonraki bölümde yetkili IP aralıkları etkinleştirdiğinizde, kümedeki düğümler güvenilir bir şekilde API sunucuyla iletişim kurabildiğinden emin olmak için ağ geçidi çıkış olarak kullanmak için Azure bir güvenlik duvarı oluşturun. Azure Güvenlik Duvarı'nın IP adresi, sonra sonraki bölümde yetkilendirilmiş API sunucusu IP adreslerinin listesine eklenir.

> [!WARNING]
> Azure Güvenlik Duvarı'nın kullanımı, bir aylık fatura döngüsü üzerinden önemli ücrete neden. Azure Güvenlik Duvarı'nı kullanma gereksinimi, yalnızca bu ilk önizleme döneminde gerekli olmamalıdır. Daha fazla bilgi ve maliyet planlaması için bkz. [Azure güvenlik duvarı fiyatlandırma][azure-firewall-costs].

İlk olarak, alın *MC_* AKS kümesi ve sanal ağ için kaynak grubu adı. Ardından, kullanarak bir alt ağ oluşturmak [az ağ sanal ağ alt ağı oluşturma][az-network-vnet-subnet-create] komutu. Aşağıdaki örnekte adlı bir alt ağ oluşturulmaktadır *AzureFirewallSubnet* CIDR aralığı ile *10.200.0.0/16*:

```azurecli-interactive
# Get the name of the MC_ cluster resource group
MC_RESOURCE_GROUP=$(az aks show \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --query nodeResourceGroup -o tsv)

# Get the name of the virtual network used by the cluster
VNET_NAME=$(az network vnet list \
    --resource-group $MC_RESOURCE_GROUP \
    --query [0].name -o tsv)

# Create a subnet in the virtual network for Azure Firewall
az network vnet subnet create \
    --resource-group $MC_RESOURCE_GROUP \
    --vnet-name $VNET_NAME \
    --name AzureFirewallSubnet \
    --address-prefixes 10.200.0.0/16
```

Bir Azure güvenlik duvarı oluşturmak için yükleme *azure Güvenlik Duvarı* CLI uzantısını kullanarak [az uzantısı ekleme][az-extension-add] command. Then, create a firewall using the [az network firewall create][az-network-firewall-create] komutu. Aşağıdaki örnekte adlı bir Azure güvenlik duvarı oluşturur *myAzureFirewall*:

```azurecli-interactive
# Install the CLI extension for Azure Firewall
az extension add --name azure-firewall

# Create an Azure firewall
az network firewall create \
    --resource-group $MC_RESOURCE_GROUP\
    --name myAzureFirewall
```

Bir Azure güvenlik duvarı aracılığıyla çıkış trafiği akan genel bir IP adresi atanır. Genel Adres kullanarak oluşturma [az network public-IP oluşturma][az-network-public-ip-create] command, then create an IP configuration on the firewall using the [az network firewall ip-config create][az-network-firewall-ip-config-create] , genel IP için geçerlidir:

```azurecli-interactive
# Create a public IP address for the firewall
FIREWALL_PUBLIC_IP=$(az network public-ip create \
    --resource-group $MC_RESOURCE_GROUP \
    --name myAzureFirewallPublicIP \
    --sku Standard \
    --query publicIp.ipAddress -o tsv)

# Associated the firewall with virtual network
az network firewall ip-config create \
    --resource-group $MC_RESOURCE_GROUP \
    --name myAzureFirewallIPConfig \
    --vnet-name $VNET_NAME \
    --firewall-name myAzureFirewall \
    --public-ip-address myAzureFirewallPublicIP
```

Artık Azure ağ güvenlik duvarı için oluşturma *izin* tüm *TCP* kullanarak trafiği [az ağ ağ-güvenlik duvarı oluşturma][az-network-firewall-network-rule-create] komutu. Aşağıdaki örnekte adlı bir ağ kuralı oluşturur *AllowTCPOutbound* trafiği ile herhangi bir kaynak veya hedef adresi için:

```azurecli-interactive
az network firewall network-rule create \
    --resource-group $MC_RESOURCE_GROUP \
    --firewall-name myAzureFirewall \
    --name AllowTCPOutbound \
    --collection-name myAzureFirewallCollection \
    --priority 200 \
    --action Allow \
    --protocols TCP \
    --source-addresses '*' \
    --destination-addresses '*' \
    --destination-ports '*'
```

Azure güvenlik duvarı ağ yolu ile ilişkilendirmek için var olan yol tablosu bilgileri, Azure Güvenlik Duvarı'nın iç IP adresi ve API sunucunun IP adresini alın. Bu IP adresleri, küme iletişimi için trafiği nasıl yönlendirileceğini denetlemek için sonraki bölümde belirtilir.

```azurecli-interactive
# Get the AKS cluster route table
ROUTE_TABLE=$(az network route-table list \
    --resource-group $MC_RESOURCE_GROUP \
    --query "[?contains(name,'agentpool')].name" -o tsv)

# Get internal IP address of the firewall
FIREWALL_INTERNAL_IP=$(az network firewall show \
    --resource-group $MC_RESOURCE_GROUP \
    --name myAzureFirewall \
    --query ipConfigurations[0].privateIpAddress -o tsv)

# Get the IP address of API server endpoint
K8S_ENDPOINT_IP=$(kubectl get endpoints -o=jsonpath='{.items[?(@.metadata.name == "kubernetes")].subsets[].addresses[].ip}')
```

Son olarak, var olan AKS ağ rota tablosunu kullanarak azure'da bir yol oluşturma [az ağ route-table route oluşturma][az-network-route-table-route-create] API sunucu iletişimi için Azure güvenlik duvarı gerecini kullanmanız trafiğe izin veren komutu.

```azurecli-interactive
az network route-table route create \
    --resource-group $MC_RESOURCE_GROUP \
    --route-table-name $ROUTE_TABLE \
    --name AzureFirewallAPIServer \
    --address-prefix $K8S_ENDPOINT_IP/32 \
    --next-hop-ip-address $FIREWALL_INTERNAL_IP \
    --next-hop-type VirtualAppliance

echo "Public IP address for the Azure Firewall instance that should be added to the list of API server authorized addresses is:" $FIREWALL_PUBLIC_IP
```

Azure güvenlik duvarı aletinizin genel IP adresini not edin. Bu adres, sonraki bölümde API sunucusu yetkili IP aralıkları listesine eklenir.

## <a name="enable-authorized-ip-ranges"></a>Yetkili IP aralıkları etkinleştir

API sunucusu yetkili IP aralıkları etkinleştirmek için yetkili IP adresi aralıkları listesini sağlar. CIDR aralığı belirttiğiniz zaman aralığındaki ilk IP adresi ile başlatın. Örneğin, *137.117.106.90/29* geçerli bir aralık, ancak emin olun, belirttiğiniz aralığındaki ilk IP adresi gibi *137.117.106.88/29*.

Kullanım [az aks güncelleştirme][az-aks-update] belirtin ve komutu *--API-server-yetkili-IP-aralıkları* izin vermek için. Bu IP adresi aralıkları, genellikle şirket içi ağlarınızı tarafından kullanılan adres aralıkları biçimindedir. Kendi Azure güvenlik duvarı gibi önceki adımda edinilen genel IP adresini ekleyin *20.42.25.196/32*.

Aşağıdaki örnek API sunucusu yetkili IP aralıkları adlı kümede etkinleştirir *myAKSCluster* adlı kaynak grubunda *myResourceGroup*. Yetkilendirmek için IP adres aralıkları *20.42.25.196/32* (Azure güvenlik duvarı genel IP adresi), ardından *172.0.0.10/16* ve *168.10.0.10/18*:

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --api-server-authorized-ip-ranges 20.42.25.196/32,172.0.0.10/16,168.10.0.10/18
```

## <a name="update-or-disable-authorized-ip-ranges"></a>Güncelleştirme veya yetkili IP aralıkları devre dışı bırakma

Güncelleştirme veya yetkili IP aralıkları devre dışı bırakmak için tekrar kullanmanız [az aks güncelleştirme][az-aks-update] komutu. İzin vermek veya aşağıdaki örnekte gösterildiği gibi API sunucusu devre dışı bırakmak için boş bir aralığın IP aralıklarının yetkili belirtmek istiyorsanız güncelleştirilmiş CIDR aralığı belirtin:

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --api-server-authorized-ip-ranges ""
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, API sunucu yetkili IP aralıkları etkin. Bu yaklaşım, güvenli bir AKS kümesi nasıl çalıştırabileceğiniz bir parçasıdır.

Daha fazla bilgi için [uygulama ve kümelerin aks'deki için güvenlik kavramları][concepts-security] and [Best practices for cluster security and upgrades in AKS][operator-best-practices-cluster-security].

<!-- LINKS - external -->
[azure-firewall-costs]: https://azure.microsoft.com/pricing/details/azure-firewall/

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[install-azure-cli]: /cli/azure/install-azure-cli
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-aks-update]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-update
[concepts-clusters-workloads]: concepts-clusters-workloads.md
[concepts-security]: concepts-security.md
[operator-best-practices-cluster-security]: operator-best-practices-cluster-security.md
[create-aks-sp]: kubernetes-service-principal.md#manually-create-a-service-principal
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-network-vnet-subnet-create]: /cli/azure/network/vnet/subnet#az-network-vnet-subnet-create
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-network-firewall-create]: /cli/azure/ext/azure-firewall/network/firewall#ext-azure-firewall-az-network-firewall-create
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[az-network-firewall-ip-config-create]: /cli/azure/ext/azure-firewall/network/firewall/ip-config#ext-azure-firewall-az-network-firewall-ip-config-create
[az-network-firewall-network-rule-create]: /cli/azure/ext/azure-firewall/network/firewall/network-rule#ext-azure-firewall-az-network-firewall-network-rule-create
[az-network-route-table-route-create]: /cli/azure/network/route-table/route#az-network-route-table-route-create
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[az-extension-list]: /cli/azure/extension#az-extension-list
[az-extension-update]: /cli/azure/extension#az-extension-update
