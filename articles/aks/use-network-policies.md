---
title: Pod'ların ağ ilkeleri Azure Kubernetes Service (AKS) ile güvenli hale getirme
description: Kubernetes ağ ilkelerini Azure Kubernetes Service (AKS) kullanarak pod'ların içine ve dışına akan trafiği güvenli hale getirme hakkında bilgi edinin
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 02/12/2019
ms.author: iainfou
ms.openlocfilehash: ade5a39273aa807f6c69f76342a0f715c7a96309
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/16/2019
ms.locfileid: "56327177"
---
# <a name="secure-traffic-between-pods-using-network-policies-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) ağ ilkeleri kullanılarak pod'ları arasındaki trafiğin güvenliğini sağlama

Kubernetes'te modern, mikro hizmet tabanlı uygulamaları çalıştırdığınızda, genellikle hangi bileşenlerin birbirleriyle iletişim kurabilir denetlemek istersiniz. En düşük öncelik ilkesini nasıl trafiği pod'ların bir AKS kümesi arasında akış için uygulanmalıdır. Örneğin, büyük olasılıkla doğrudan arka uç uygulamaları için trafiği engellemek istiyorsunuz. Kubernetes, *Ağ İlkesi* özellik pod'ların bir küme arasında giriş ve çıkış trafiği için kuralları tanımlamanıza olanak sağlar.

Bu makalede aks'deki pod'ları arasındaki trafik akışını denetlemek için ağ ilkeleri kullanmayı gösterir.

> [!IMPORTANT]
> Bu özellik şu anda önizleme sürümündedir. Önizlemeler, [ek kullanım koşullarını][terms-of-use] kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

## <a name="before-you-begin"></a>Başlamadan önce

Azure CLI Sürüm 2.0.56 gerekir veya daha sonra yüklü ve yapılandırılmış. Çalıştırma `az --version` sürümü bulmak için. Gerekirse yüklemek veya yükseltmek bkz [Azure CLI yükleme][install-azure-cli].

## <a name="overview-of-network-policy"></a>Ağ İlkesi'ne genel bakış

Varsayılan olarak, bir AKS kümesindeki tüm pod'ların gönderebilir ve trafiği kısıtlama olmadan alabilirsiniz. Güvenliği artırmak için trafik akışını denetleyen kuralları tanımlayabilirsiniz. Örneğin, arka uç uygulamaları için gerekli ön uç Hizmetleri yalnızca sunulur veya veritabanı bileşenlerini yalnızca bunlara uygulama katmanları tarafından erişilebilir.

Ağ ilkeleri pod'ları arasındaki trafik akışını denetlemenize olanak tanıyan Kubernetes kaynaklardır. İzin verme veya reddetme ayarları gibi atanan etiketleri, ad alanı veya trafiği, bağlantı noktası trafiğini seçebilirsiniz. Bir YAML bildirimleri gibi ağ ilkeleri tanımlanır ve ayrıca bir dağıtım veya hizmeti oluşturan daha geniş bir bildirim bir parçası olarak dahil edilebilir.

Şimdi ağ ilkeleri, uygulamada görmek için oluşturun ve ardından aşağıdaki gibi trafik akışını tanımlayan bir ilkeyi açın:

* Pod için tüm trafiğe izin verme.
* Pod etiketlerine bağlı trafiğine izin verin.
* Ad alanını temel alan trafiğine izin verin.

## <a name="create-an-aks-cluster-and-enable-network-policy"></a>AKS kümesi oluşturma ve Ağ İlkesi'ni etkinleştirin

Ağ İlkesi, yalnızca küme oluşturulduğunda etkinleştirilebilir. Ağ İlkesi var olan bir AKS kümesi üzerinde etkinleştirilemiyor. Bir AKS ile ağ ilkesi oluşturmak için önce aboneliğinizde özellik bayrağı etkinleştirin. Kaydedilecek *EnableNetworkPolicy* özellik bayrağı, kullanın [az özelliği kayıt] [ az-feature-register] komutu aşağıdaki örnekte gösterildiği gibi:

```azurecli-interactive
az feature register --name EnableNetworkPolicy --namespace Microsoft.ContainerService
```

Gösterilecek durum için birkaç dakika sürer *kayıtlı*. Kayıt kullanarak durumu denetleyebilirsiniz [az özellik listesi] [ az-feature-list] komutu:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/EnableNetworkPolicy')].{Name:name,State:properties.state}"
```

Hazır olduğunuzda, kayıt yenileme *Microsoft.ContainerService* kullanarak kaynak sağlayıcısını [az provider register] [ az-provider-register] komutu:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

Ağ İlkesi ile bir AKS kümesi kullanmak için kullanmalısınız [Azure CNI eklentisi] [ azure-cni] ve kendi sanal ağ ve alt ağları tanımlayın. Ayrıntılı gerekli bir alt ağ aralıklarını planlama hakkında daha fazla bilgi için bkz. [Gelişmiş Ağ][use-advanced-networking]. Aşağıdaki örnek betik:

* Bir sanal ağ ve alt ağ oluşturur.
* Bir Azure Active Directory (AD) ile bir AKS kümesi kullanmak için hizmet sorumlusu oluşturur.
* Atar *katkıda bulunan* izinlerini AKS Küme hizmet sorumlusu sanal ağ.
* Bir AKS kümesi içinde tanımlı sanal ağ oluşturur ve ağ ilkesi sağlar.

Kendi güvenli sağlamak *SP_PASSWORD*. İsterseniz değiştirme *RESOURCE_GROUP_NAME* ve *küme_adı* değişkenleri:

```azurecli-interactive
SP_PASSWORD=mySecurePassword
RESOURCE_GROUP_NAME=myResourceGroup-NP
CLUSTER_NAME=myAKSCluster
LOCATION=canadaeast

# Create a resource group
az group create --name $RESOURCE_GROUP_NAME --location $LOCATION

# Create a virtual network and subnet
az network vnet create \
    --resource-group $RESOURCE_GROUP_NAME \
    --name myVnet \
    --address-prefixes 10.0.0.0/8 \
    --subnet-name myAKSSubnet \
    --subnet-prefix 10.240.0.0/16

# Create a service principal and read in the application ID
read SP_ID <<< $(az ad sp create-for-rbac --password $SP_PASSWORD --skip-assignment --query [appId] -o tsv)

# Wait 15 seconds to make sure that service principal has propagated
echo "Waiting for service principal to propagate..."
sleep 15

# Get the virtual network resource ID
VNET_ID=$(az network vnet show --resource-group $RESOURCE_GROUP_NAME --name myVnet --query id -o tsv)

# Assign the service principal Contributor permissions to the virtual network resource
az role assignment create --assignee $SP_ID --scope $VNET_ID --role Contributor

# Get the virtual network subnet resource ID
SUBNET_ID=$(az network vnet subnet show --resource-group $RESOURCE_GROUP_NAME --vnet-name myVnet --name myAKSSubnet --query id -o tsv)

# Create the AKS cluster and specify the virtual network and service principal information
# Enable network policy using the `--network-policy` parameter
az aks create \
    --resource-group $RESOURCE_GROUP_NAME \
    --name $CLUSTER_NAME \
    --node-count 1 \
    --kubernetes-version 1.12.4 \
    --generate-ssh-keys \
    --network-plugin azure \
    --service-cidr 10.0.0.0/16 \
    --dns-service-ip 10.0.0.10 \
    --docker-bridge-address 172.17.0.1/16 \
    --vnet-subnet-id $SUBNET_ID \
    --service-principal $SP_ID \
    --client-secret $SP_PASSWORD \
    --network-policy calico
```

Kümenin oluşturulması birkaç dakika sürer. İşiniz bittiğinde yapılandırma `kubectl` kullanarak, Kubernetes kümesine bağlanmak için [az aks get-credentials] [ az-aks-get-credentials] komutu. Bu komut, kimlik bilgilerini indirir ve Kubernetes CLI'yi bunları kullanacak şekilde yapılandırır:

```azurecli-interactive
az aks get-credentials --resource-group $RESOURCE_GROUP_NAME --name $CLUSTER_NAME
```

## <a name="deny-all-inbound-traffic-to-a-pod"></a>Bir pod tüm gelen trafiği engelle

Belirli ağ trafiğine izin vermek için kuralları tanımlamadan önce ilk tüm trafiği reddetmeye yönelik bir ağ ilkesi oluşturun. Bu ilke, yalnızca istenen trafiğe izin verilenler listesine başlamak için bir başlangıç noktası sunar. Ayrıca, Ağ İlkesi uygulandığında, trafik bırakıldı ayrıca NET bir şekilde görebilirsiniz.

Bizim örnek uygulama ortamı ve trafik kuralları için ilk olarak adlandırılan bir ad alanı oluşturalım *geliştirme* bizim örnek pod'ları çalıştırmak için:

```console
kubectl create namespace development
kubectl label namespace/development purpose=development
```

Şimdi NGINX çalıştıran bir örnek arka uç pod oluşturun. Bu arka uç pod örnek arka uç web tabanlı bir uygulama benzetimini yapmak için kullanılabilir. Bu pod içinde oluşturma *geliştirme* ad ve bağlantı noktasını Aç *80* web trafiğini hizmet. Etiket ile pod *uygulama Web uygulamasında = rol arka uç =* böylece biz sonraki bölümde Ağ İlkesi'yle hedefleyebilir:

```console
kubectl run backend --image=nginx --labels app=webapp,role=backend --namespace development --expose --port 80 --generator=run-pod/v1
```

Varsayılan NGINX web sayfası başarıyla ulaşabilirsiniz test etmek için başka bir pod oluşturun ve terminal oturumu ekleyin:

```console
kubectl run --rm -it --image=alpine network-policy --namespace development --generator=run-pod/v1
```

Bir kez shell isteminde kullanın `wget` varsayılan NGINX web sayfası erişebilirsiniz onaylamak için:

```console
wget -qO- http://backend
```

Aşağıdaki örnek çıktıda, varsayılan NGINX web sayfası döndürdüğünü gösterir:

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
[...]
```

Ekli terminal oturumu dışında çıkın. Test pod otomatik olarak silinir:

```console
exit
```

### <a name="create-and-apply-a-network-policy"></a>Oluşturma ve bir ağ ilkesi uygulama

Size örnek arka uç pod temel NGINX web sayfasında erişim onayladıktan sonra tüm trafiği reddetmeye yönelik bir ağ ilkesi oluşturun. Adlı bir dosya oluşturun `backend-policy.yaml` aşağıdaki YAML bildirimi yapıştırın. Bu bildirimi kullanan bir *podSelector* sahip pod'ların ilkesi eklemek için *app:webapp, rol: arka uç* örnek NGINX pod gibi bir etiket. Altında hiçbir kural tanımlanmadı *giriş*, pod tüm gelen trafik engellenir:

```yaml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: backend-policy
  namespace: development
spec:
  podSelector:
    matchLabels:
      app: webapp
      role: backend
  ingress: []
```

Ağ İlkesi kullanarak uygulama [kubectl uygulamak] [ kubectl-apply] komut ve YAML bildiriminizi adını belirtin:

```azurecli-interactive
kubectl apply -f backend-policy.yaml
```

### <a name="test-the-network-policy"></a>Ağ İlkesi test etme

Arka uç pod NGINX Web sayfasına yeniden erişip görelim. Başka bir test pod oluşturun ve terminal oturumu ekleyin:

```console
kubectl run --rm -it --image=alpine network-policy --namespace development --generator=run-pod/v1
```

Bir kez shell isteminde kullanın `wget` varsayılan NGINX web sayfası erişip görmek için. Bu kez, ayarlanmış bir zaman aşımı değeri *2* saniye. Sayfa yüklenemiyor şekilde ağ ilkesi aşağıdaki örnekte gösterildiği gibi artık tüm gelen trafiği engeller:

```console
$ wget -qO- --timeout=2 http://backend

wget: download timed out
```

Ekli terminal oturumu dışında çıkın. Test pod otomatik olarak silinir:

```console
exit
```

## <a name="allow-inbound-traffic-based-on-a-pod-label"></a>Bir pod etikete göre gelen trafiğe izin vermek

Önceki bölümde, bir arka uç NGINX pod zamanlandı ve tüm trafiği reddetmeye yönelik bir ağ ilkesi oluşturuldu. Hemen şimdi bir ön uç pod oluşturun ve ön uç pod'ların gelen trafiğe izin vermek için Ağ İlkesi güncelleştirin.

Pod'ların etiketlerle gelen trafiğe izin vermek için Ağ İlkesi güncelleştirme *app:webapp, rol: ön uç* ve herhangi bir ad alanı. Önceki Düzen *arka uç policy.yaml* dosya ve ekleme bir *matchLabels* giriş kuralları ve böylece bildiriminizi aşağıdaki örnekteki gibi görünür:

```yaml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: backend-policy
  namespace: development
spec:
  podSelector:
    matchLabels:
      app: webapp
      role: backend
  ingress:
  - from:
    - namespaceSelector: {}
      podSelector:
        matchLabels:
          app: webapp
          role: frontend
```

Kullanarak güncelleştirilmiş bir ağ ilkesi uygulamak [kubectl uygulamak] [ kubectl-apply] komut ve YAML bildiriminizi adını belirtin:

```azurecli-interactive
kubectl apply -f backend-policy.yaml
```

Artık olarak etiketlenmiş bir pod zamanlama *uygulama Web uygulamasında = rol ön uç =* ve terminal oturumu ekleyin:

```console
kubectl run --rm -it frontend --image=alpine --labels app=webapp,role=frontend --namespace development --generator=run-pod/v1
```

Bir kez shell isteminde kullanın `wget` varsayılan NGINX web sayfası erişip görmek için:

```console
wget -qO- http://backend
```

Giriş kural etiketlere sahip pod'ların trafiğe izin verir. *uygulama: webapp, rol: ön uç*, ön uç pod gelen trafiğe izin verilir. Aşağıdaki örnek çıktıda, döndürülen varsayılan NGINX web sayfası gösterilmektedir:

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
[...]
```

Ekli terminal oturumu dışında çıkın. Pod otomatik olarak silinir:

```console
exit
```

### <a name="test-a-pod-without-a-matching-label"></a>Eşleşen bir etiket olmadan bir pod test

Ağ İlkesi etiketli pod'ların gelen trafiğe izin verir *uygulama: webapp, rol: ön uç*, ancak diğer tüm trafiği reddetmeye. Şimdi bu etiket olmadan başka bir pod arka uç NGINX pod erişemiyor test edin. Başka bir test pod oluşturun ve terminal oturumu ekleyin:

```console
kubectl run --rm -it --image=alpine network-policy --namespace development --generator=run-pod/v1
```

Bir kez shell isteminde kullanın `wget` varsayılan NGINX web sayfası erişip görmek için. Sayfa yüklenemiyor için ağ ilkesi aşağıdaki örnekte gösterildiği gibi gelen trafiği engeller:

```console
$ wget -qO- --timeout=2 http://backend

wget: download timed out
```

Ekli terminal oturumu dışında çıkın. Test pod otomatik olarak silinir:

```console
exit
```

## <a name="allow-traffic-only-from-within-a-defined-namespace"></a>İçinde tanımlanan bir ad alanı yalnızca gelen trafiğe izin ver

Önceki örneklerde, tüm trafiği reddedildi ve ardından ilkeyi belirli bir etikete sahip pod'ların gelen trafiğe izin vermek için güncelleştirilmiş bir ağ ilkesi oluşturdunuz. Bir diğer yaygın gereksinim, trafik yalnızca belirtilen bir ad alanı içinde çalıştırmanız gerekir. Önceki örneklerde trafiği için olsaydı bir *geliştirme* ad alanı gibi başka bir ad alanı, gelen trafiği engelleyen bir ağ ilkesi oluşturup isteyebilirsiniz *üretim*, ulaşmasını pod'ları.

İlk olarak, bir üretim ad alanı benzetimini yapmak için yeni bir ad alanı oluşturun:

```console
kubectl create namespace production
kubectl label namespace/production purpose=production
```

Bir test pod içinde zamanlamak *üretim* olarak etiketli bir ad alanı *app = webapp, rol ön uç =*. Terminal oturumu ekleyin:

```console
kubectl run --rm -it frontend --image=alpine --labels app=webapp,role=frontend --namespace production --generator=run-pod/v1
```

Bir kez shell isteminde kullanın `wget` varsayılan NGINX web sayfası erişebilirsiniz onaylamak için:

```console
wget -qO- http://backend.development
```

Pod etiketlerini eşleşen gibi Ağ İlkesi'nde şu anda izin, trafiğe izin verilir. Ağ İlkesi isim uzaylarının, yalnızca pod etiketler gibi görünmüyor. Aşağıdaki örnek çıktıda, döndürülen varsayılan NGINX web sayfası gösterilmektedir:

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
[...]
```

Ekli terminal oturumu dışında çıkın. Test pod otomatik olarak silinir:

```console
exit
```

### <a name="update-the-network-policy"></a>Ağ İlkesi güncelleştirme

Artık giriş kuralını güncelleştirelim *namespaceSelector* bölümünde yalnızca içinde gelen trafiğe izin vermek için *geliştirme* ad alanı. Düzen *arka uç policy.yaml* aşağıdaki örnekte gösterildiği gibi bildirim dosyası:

```yaml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: backend-policy
  namespace: development
spec:
  podSelector:
    matchLabels:
      app: webapp
      role: backend
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          purpose: development
      podSelector:
        matchLabels:
          app: webapp
          role: frontend
```

Daha karmaşık bir örnekte birden çok giriş kuralları gibi kullanılacak tanımlayabilirsiniz bir *namespaceSelector* ve ardından bir *podSelector*.

Kullanarak güncelleştirilmiş bir ağ ilkesi uygulamak [kubectl uygulamak] [ kubectl-apply] komut ve YAML bildiriminizi adını belirtin:

```azurecli-interactive
kubectl apply -f backend-policy.yaml
```

### <a name="test-the-updated-network-policy"></a>Güncelleştirilmiş Ağ İlkesi test etme

Artık başka bir pod içinde zamanlama *üretim* ad alanı ve terminal oturumu ekleyin:

```console
kubectl run --rm -it frontend --image=alpine --labels app=webapp,role=frontend --namespace production --generator=run-pod/v1
```

Bir kez shell isteminde kullanın `wget` artık trafiği reddetmeye Ağ İlkesi görmek için:

```console
$ wget -qO- --timeout=2 http://backend.development

wget: download timed out
```

Test pod dışında çıkış:

```console
exit
```

Engellenen gelen trafik ile *üretim* ad alanı, şimdi zamanlama test pod yeniden *geliştirme* ad alanı ve terminal oturumu ekleyin:

```console
kubectl run --rm -it frontend --image=alpine --labels app=webapp,role=frontend --namespace development --generator=run-pod/v1
```

Bir kez shell isteminde kullanın `wget` ilke ağ görmek için trafiğe izin ver:

```console
wget -qO- http://backend
```

Pod eşleşen ağ ilkesinde izin verilen ad alanında zamanlanmış olarak trafiğe izin verilir. Aşağıdaki örnek çıktıda, döndürülen varsayılan NGINX web sayfası gösterilmektedir:

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
[...]
```

Ekli terminal oturumu dışında çıkın. Test pod otomatik olarak silinir:

```console
exit
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makalede, iki ad alanları oluşturun ve ağ ilkesi uygulanır. Bu kaynakları temizlemek için kullanın [kubectl Sil] [ kubectl-delete] komut ve kaynak adları aşağıdaki gibi belirtin:

```console
kubectl delete namespace production
kubectl delete namespace development
```

## <a name="next-steps"></a>Sonraki adımlar

Ağ kaynakları hakkında daha fazla bilgi için bkz. [kavramları Azure Kubernetes Service (AKS) uygulamaları için ağ][concepts-network].

İlkeleri kullanma hakkında daha fazla bilgi için bkz. [Kubernetes ağ ilkelerini][kubernetes-network-policies].

<!-- LINKS - external -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubernetes-network-policies]: https://kubernetes.io/docs/concepts/services-networking/network-policies/
[azure-cni]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - internal -->
[install-azure-cli]: /cli/azure/install-azure-cli
[use-advanced-networking]: configure-advanced-networking.md
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[concepts-network]: concepts-network.md
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
