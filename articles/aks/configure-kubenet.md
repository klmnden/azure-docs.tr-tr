---
title: Kubernetes Azure Kubernetes Service (AKS) ağ yapılandırma
description: Azure Kubernetes Service (AKS kümesi bir var olan sanal ağı ve alt ağ dağıtmak için AKS) kubernetes (Temel) ağ yapılandırmayı öğrenin.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 06/03/2019
ms.author: iainfou
ms.reviewer: nieberts, jomore
ms.openlocfilehash: f57c1af4c497b51f5289559737fad5ce4cf2e85b
ms.sourcegitcommit: a7ea412ca4411fc28431cbe7d2cc399900267585
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2019
ms.locfileid: "67358040"
---
# <a name="use-kubenet-networking-with-your-own-ip-address-ranges-in-azure-kubernetes-service-aks"></a>Kubernetes kendi IP adresi aralıklarını Azure Kubernetes Service (AKS) ile ağ kullanma

Varsayılan olarak, AKS kümesi kullanım [kubernetes][kubenet], ve sizin için bir Azure sanal ağı ve alt ağ oluşturulur. İle *kubernetes*, düğümler, Azure sanal ağ alt ağından bir IP adresi alın. Pod'ların, düğümler Azure sanal ağ alt ağına mantıksal olarak farklı adres alanından bir IP adresi alır. Ağ adresi çevirisi (NAT), ardından pod'ları Azure sanal ağındaki kaynaklara erişebilmesi için yapılandırılır. Trafiği kaynak IP adresini, NAT düğümün birincil IP başvuracağını ' dir. Bu yaklaşım ağ alanınıza kullanmak pod'ları ayırmanız gerekir IP adresi sayısını önemli ölçüde azaltır.

İle [Azure kapsayıcı ağ arabirimi (CNI)][cni-networking], her pod alt ağdan bir IP adresi alır ve doğrudan erişilebilir. Bu IP adresleri, ağ alanı benzersiz olmalıdır ve önceden hazırlıklı olmak gerekir. Her düğümü destekliyorsa pod'ların sayısı için bir yapılandırma parametresi vardır. Düğüm başına IP adreslerinin sayısı, bu düğüm için önden ayrılmıştır. Bu yaklaşım, daha fazla planlama gerektirir ve genellikle IP adresi tükenmesi veya uygulama arttıkça daha büyük bir alt ağ kümelerini yeniden gereksinimini doğurur.

Bu makalede nasıl kullanılacağını gösterir *kubernetes* oluşturmak ve bir AKS kümesi için bir sanal ağ alt ağı kullanmak için ağ. Ağ seçenekleri ve konuları hakkında daha fazla bilgi için bkz. [ağ kavramları Kubernetes ve AKS için][aks-network-concepts].

> [!WARNING]
> Windows Server düğüm havuzları (şu anda önizlemede aks'deki) kullanmak için Azure CNI kullanmanız gerekir. Windows Server kapsayıcıları için kubernetes kullanımını ağ modeli olarak kullanılamaz.

## <a name="before-you-begin"></a>Başlamadan önce

Azure CLI Sürüm 2.0.65 gerekir veya daha sonra yüklü ve yapılandırılmış. Çalıştırma `az --version` sürümü bulmak için. Gerekirse yüklemek veya yükseltmek bkz [Azure CLI yükleme][install-azure-cli].

## <a name="overview-of-kubenet-networking-with-your-own-subnet"></a>Ağ kendi alt ağ ile kubernetes genel bakış

Birçok ortamlarda, sanal ağlar ve alt ağa sahip ayrılmış IP adresi aralıkları tanımladınız. Bu sanal ağ kaynakları, birden çok hizmet ve uygulamaları desteklemek için kullanılır. Ağ bağlantısı sağlamak için AKS kümeleri kullanabilirsiniz *kubernetes* (temel ağ iletişimi) veya Azure CNI (*ağ Gelişmiş*).

İle *kubernetes*yalnızca düğümler, sanal ağ alt ağında bir IP adresi alır. Pod'ların birbirleriyle doğrudan iletişim kuramaz. Bunun yerine, kullanıcı tanımlı yönlendirme (UDR) ve IP iletme düğümlerde pod'ları arasında bağlantı kurmak için kullanılır. Pod'ların atanan bir IP adresi alan bir hizmeti arkasında da dağıtabilirsiniz ve uygulama için trafik yükü dengeler. Aşağıdaki diyagramda, nasıl AKS düğümleri sanal ağ alt ağı, ancak pod olmayan bir IP adresi alır gösterilmektedir:

![Kubernetes AKS kümesi ile ağ modeli](media/use-kubenet/kubenet-overview.png)

Azure, en fazla 400 yolların bir UDR'de destekler, böylece bir AKS kümesi 400 düğümlerinden daha büyük olamaz. AKS özellikleri gibi [sanal düğümü][virtual-nodes] veya ağ ilkeleri ile desteklenmeyen *kubernetes*.

İle *Azure CNI*, her pod IP alt ağda bir IP adresi alır ve diğer pod'ların ve Hizmetleri ile doğrudan iletişim kurabilir. Kümeleri, belirttiğiniz IP adresi aralığı büyük olabilir. Ancak IP adresi aralığı önceden planlanmalıdır ve tüm IP adreslerini desteklemek pod'ları, maksimum sayısına göre AKS düğümleri tarafından kullanılır. Ağ özelliklerini ve senaryoları gibi gelişmiş [sanal düğümü][virtual-nodes] veya ağ ilkeleri ile desteklenen *Azure CNI*.

### <a name="ip-address-availability-and-exhaustion"></a>IP adresi kullanılabilirlik ve tükendi

İle *Azure CNI*, yaygın bir sorun atanan IP adresi aralığı ölçeğini veya bir küme yükseltmesi sırasında ek düğümler ardından eklemek için çok küçük olduğu. Ağ ekibi Ayrıca, beklenen uygulama taleplerini desteklemek için yeterince büyük bir IP adresi aralığı vermek mümkün olmayabilir.

Bir güvenlik ihlali kullanan bir AKS kümesi oluşturabileceğiniz *kubernetes* ve var olan bir sanal ağ alt ağına bağlayın. Bu yaklaşım, çok sayıda IP adresleri Önden, kümede çalışabilecek olası pod'ların tüm ayırmak zorunda kalmadan tanımlanan IP adreslerini almak düğümleri sağlar.

İle *kubernetes*, bir çok daha küçük IP adresi aralığı kullanın ve büyük kümeleri ve uygulama taleplerini destekleyebilir. Örneğin, hatta ile bir */27* IP adresi aralığı, 20-25 düğümlü bir küme ölçeklendirme veya yükseltmek için yeterli alan ile çalıştırılabilir. Bu küme boyutu en fazla destekleyecektir *2.200 2750* pod'ların (ile 110 pod'ların düğüm başına varsayılan en fazla). Pod'ları ile yapılandırabileceğiniz düğüm başına en fazla sayısını *kubernetes* AKS 250'dir.

Aşağıdaki temel hesaplamaları ağ modellerini farkı Karşılaştır:

- **kubernetes** -basit */24* IP adresi aralığı destekleyebilir *251* (her Azure sanal ağ alt ağı, yönetim işlemleri için ilk üç IP adresini ayırır) kümedeki düğümler
  - Bu düğüm sayısı kadar destekleyebilir *27,610* pod'ların (varsayılan en fazla 110 pod'ları ile düğüm başına *kubernetes*)
- **Azure CNI** -o aynı temel */24* alt ağ aralığı en fazla yalnızca Destek *8* kümedeki düğümler
  - Bu düğüm sayısı kadar yalnızca destekleyebilir *240* pod'ların (varsayılan 30 pod'ları ile düğüm başına en fazla *Azure CNI*)

> [!NOTE]
> Bu sınırları hesabı yükseltmesi alması veya ölçeklendirme işlemlerini kullanmayın. Uygulamada en fazla alt ağ IP adresi aralığı destekleyen düğüm sayısını çalıştıramazsınız. Yükseltme işlemleri sırasında ölçeği kullanım için bazı IP adreslerini bırakmanız gerekir.

### <a name="virtual-network-peering-and-expressroute-connections"></a>Sanal Ağ eşlemesi ve ExpressRoute bağlantıları

Şirket içi bağlantı sağlamak için her ikisi de *kubernetes* ve *Azure CNI* ağ yaklaşımları kullanabilirsiniz [Azure sanal ağ eşlemesi][vnet-peering] or [ExpressRoute connections][express-route]. Çakışma ve yanlış trafiği yönlendirmeyi dikkatli bir şekilde önlemek için IP adresi aralıklarınızı planlama. Örneğin, birçok şirket içi ağları kullanın. bir *10.0.0.0/8* adres ExpressRoute bağlantısı üzerinden tanıtılan aralığı. Bu adres aralığının dışında Azure sanal ağ alt ağları, AKS kümeye gibi oluşturmak için önerilen *172.16.0.0/16*.

### <a name="choose-a-network-model-to-use"></a>Kullanılacak ağ modeli seçin

Seçim, AKS için kullanmak için hangi ağ eklentisi genellikle esneklik ve Gelişmiş Yapılandırma gereksinimleriniz arasında bir denge kümedir. Her ağ modeli en uygun olduğunda aşağıdaki maddeler anahat yardımcı olur.

Kullanım *kubernetes* olduğunda:

- IP adres alanı sınırlıdır.
- Çoğu pod iletişim küme içinde olur.
- Sanal düğümler ve ağ ilkesi gibi gelişmiş özellikleri gerekmez.

Kullanım *Azure CNI* olduğunda:

- Kullanılabilir IP adresi alanı var.
- Çoğu pod iletişim küme dışındaki kaynak etmektir.
- Udr'leri yönetmek istemediğiniz.
- Sanal düğümler ve ağ ilkesi gibi gelişmiş özellikleri ihtiyacınız vardır.

Hangi ağ modeli kullanmaya karar vermenize yardımcı olacak daha fazla bilgi için bkz. [karşılaştırma ağ modelleri ve Destek kapsamlarına][network-comparisons].

> [!NOTE]
> Kuberouter kubernetes kullanırken, ağ ilkesi sağlamak mümkün kılar ve bir AKS kümesindeki bir daemonset olarak yüklenebilir. Lütfen kube-yönlendirici hala beta sürümünde olan ve hiçbir destek proje için Microsoft tarafından sunulan unutmayın.

## <a name="create-a-virtual-network-and-subnet"></a>Sanal ağ ve alt ağ oluşturma

Kullanmaya başlamak için *kubernetes* ve ilk kullanarak bir kaynak grubu oluşturun, kendi sanal ağ alt [az grubu oluşturma][az-group-create] komutu. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Bu ağ kaynaklarını kullanarak bir sanal ağınız ve kullanmak için alt ağ yoksa, oluşturma [az ağ sanal ağ oluşturma][az-network-vnet-create] komutu. Aşağıdaki örnekte, sanal ağ olarak adlandırılır *myVnet* adres ön eki ile *192.168.0.0/16*. Bir alt ağ adında oluşturulur *myAKSSubnet* adres ön eki ile *192.168.1.0/24*.

```azurecli-interactive
az network vnet create \
    --resource-group myResourceGroup \
    --name myAKSVnet \
    --address-prefixes 192.168.0.0/16 \
    --subnet-name myAKSSubnet \
    --subnet-prefix 192.168.1.0/24
```

## <a name="create-a-service-principal-and-assign-permissions"></a>Hizmet sorumlusu oluşturma ve izinleri atama

Bir AKS kümesinin diğer Azure kaynaklarıyla etkileşime geçmesini sağlamak için bir Azure Active Directory hizmet sorumlusu kullanılır. Hizmet sorumlusu AKS düğümleri kullanan bir alt ağ ve sanal ağ'ı yönetmek için izinleri olmalıdır. Bir hizmet sorumlusu oluşturmak için kullanın [az ad sp create-for-rbac][az-ad-sp-create-for-rbac] komutu:

```azurecli-interactive
az ad sp create-for-rbac --skip-assignment
```

Aşağıdaki örnek çıktıda, hizmet sorumlusu uygulama kimliği ve parolası gösterir. Bu değerler, hizmet sorumlusuna bir rol atayın ve ardından AKS kümesi oluşturmak için ek adımları kullanılır:

```console
$ az ad sp create-for-rbac --skip-assignment

{
  "appId": "476b3636-5eda-4c0e-9751-849e70b5cfad",
  "displayName": "azure-cli-2019-01-09-22-29-24",
  "name": "http://azure-cli-2019-01-09-22-29-24",
  "password": "a1024cd7-af7b-469f-8fd7-b293ecbb174e",
  "tenant": "72f998bf-85f1-41cf-92ab-2e7cd014db46"
}
```

Kalan adımlarda doğru temsilcilerden atama [az ağ vnet show][az-network-vnet-show] and [az network vnet subnet show][az-network-vnet-subnet-show] gerekli kaynak kimliklerini almak için komutları. Bu kaynak kimliklerinin değişkenleri olarak depolanır ve kalan adımları, başvurulan:

```azurecli-interactive
VNET_ID=$(az network vnet show --resource-group myResourceGroup --name myAKSVnet --query id -o tsv)
SUBNET_ID=$(az network vnet subnet show --resource-group myResourceGroup --vnet-name myAKSVnet --name myAKSSubnet --query id -o tsv)
```

Hizmet sorumlusu AKS kümenizin atayarak *katkıda bulunan* kullanarak sanal ağ üzerindeki izinleri [az rol ataması oluşturma][az-role-assignment-create] komutu. Kendi sağlamak  *\<AppID >* hizmet sorumlusunu oluşturmak için önceki komutun çıktısında gösterildiği gibi:

```azurecli-interactive
az role assignment create --assignee <appId> --scope $VNET_ID --role Contributor
```

## <a name="create-an-aks-cluster-in-the-virtual-network"></a>Sanal ağda bir AKS kümesi oluşturma

Artık bir sanal ağ ve alt ağ, oluşturulan ve oluşturduğunuz ve bu ağ kaynakları kullanmak için bir hizmet sorumlusu izinleri atanmış. Artık, sanal ağ ve alt ağ kullanarak AKS kümesi oluşturma [az aks oluşturma][az-aks-create] komutu. Kendi hizmet sorumlusu tanımlama  *\<AppID >* ve  *\<parola >* hizmet sorumlusunu oluşturmak için önceki komutun çıktısında gösterildiği gibi.

Kümenin parçası oluşturma işlemi aşağıdaki IP adresi aralıklarını da olarak tanımlanır:

* *--Hizmet CIDR* AKS kümesi iç Hizmetleri'nde bir IP adresi atamak için kullanılır. Bu IP adresi aralığı, ağ ortamınızda başka bir yerde kullanımda olmayan bir adres alanı olmalıdır. Bu, bağlanmak veya Express Route veya siteden siteye VPN bağlantısı kullanarak Azure sanal ağlarınıza bağlanmayı planlıyorsanız hiçbir şirket içi ağ aralığı içerir.

* *--Dns-hizmet-IP* adresi olmalıdır *.10* hizmeti IP adresi aralığınızı adresidir.

* *--Pod CIDR* , ağ ortamınızda başka bir yerde kullanımda olmayan bir geniş adres alanı olmalıdır. Bu, bağlanmak veya Express Route veya siteden siteye VPN bağlantısı kullanarak Azure sanal ağlarınıza bağlanmayı planlıyorsanız hiçbir şirket içi ağ aralığı içerir.
    * Bu adres aralığı'nın ölçeği beklediğiniz düğüm sayısını tutabilecek kadar büyük olmalıdır. Ek düğümler için daha fazla adres gerekiyorsa, küme dağıtıldıktan sonra bu adres aralığını değiştiremezsiniz.
    * Pod IP adresi aralığı atamak için kullanılan bir */24* adres alanı her küme düğümünde için. Aşağıdaki örnekte, *--pod CIDR* , *10.244.0.0/16* ilk düğümü atar *10.244.0.0/24*, ikinci düğümü *10.244.1.0/24*ve üçüncü düğüm *10.244.2.0/24*.
    * Küme ölçek veya yükseltme olarak, Azure platformu pod IP adresi aralığı her yeni düğüme atamak devam eder.
    
* *--Docker köprü adresi* AKS düğümleri temel alınan yönetim platformu ile iletişim kurmasına olanak tanır. Bu IP adresi kümenizin sanal ağ IP adresi aralığında olmamalıdır ve ağınızda kullanımda başka adres aralıklarıyla çakışıyor olmamalıdır.

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 3 \
    --network-plugin kubenet \
    --service-cidr 10.0.0.0/16 \
    --dns-service-ip 10.0.0.10 \
    --pod-cidr 10.244.0.0/16 \
    --docker-bridge-address 172.17.0.1/16 \
    --vnet-subnet-id $SUBNET_ID \
    --service-principal <appId> \
    --client-secret <password>
```

Bir AKS kümesi oluşturduğunuzda, bir ağ güvenlik grubu ve rota tablosu oluşturulur. Bu ağ kaynaklarına, AKS denetim düzlemi tarafından yönetilir. Ağ güvenlik grubu, düğümlerde sanal NIC ile otomatik olarak ilişkilendirilir. Sanal ağ alt ağ ile rota tablosunu otomatik olarak ilişkilendirilir. Ağ güvenlik grubu kuralları ve yol tablolarını ve olan otomatik olarak oluşturur ve hizmetlerini güncelleştirildi.

## <a name="next-steps"></a>Sonraki adımlar

AKS kümesi, var olan bir sanal ağ alt ağa dağıtılan bir artık küme normal olarak kullanabilirsiniz. Kullanmaya başlama [Azure geliştirme alanları kullanılarak uygulama yazmaya][dev-spaces] or [using Draft][use-draft], veya [Helm kullanarak uygulama dağıtma][kullanım helm].

<!-- LINKS - External -->
[dev-spaces]: https://docs.microsoft.com/azure/dev-spaces/
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet

<!-- LINKS - Internal -->
[install-azure-cli]: /cli/azure/install-azure-cli
[aks-network-concepts]: concepts-network.md
[az-group-create]: /cli/azure/group#az-group-create
[az-network-vnet-create]: /cli/azure/network/vnet#az-network-vnet-create
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-network-vnet-show]: /cli/azure/network/vnet#az-network-vnet-show
[az-network-vnet-subnet-show]: /cli/azure/network/vnet/subnet#az-network-vnet-subnet-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-aks-create]: /cli/azure/aks#az-aks-create
[use-helm]: kubernetes-helm.md
[use-draft]: kubernetes-draft.md
[virtual-nodes]: virtual-nodes-cli.md
[vnet-peering]: ../virtual-network/virtual-network-peering-overview.md
[express-route]: ../expressroute/expressroute-introduction.md
[network-comparisons]: concepts-network.md#compare-network-models
