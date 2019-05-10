---
title: Pod'ların ağ ilkeleri Azure Kubernetes Service (AKS) ile güvenli hale getirme
description: Azure Kubernetes Service (AKS) Kubernetes ağ ilkeleri kullanarak, pod'ların içine ve dışına akan trafiği güvenli hale getirme hakkında bilgi edinin
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 05/06/2019
ms.author: iainfou
ms.openlocfilehash: a0512806ec797f43fc54d8a28a7cbadf86faf1d9
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65229998"
---
# <a name="secure-traffic-between-pods-using-network-policies-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) ağ ilkeleri kullanılarak pod'ları arasındaki trafiğin güvenliğini sağlama

Kubernetes'te modern, mikro hizmet tabanlı uygulamaları çalıştırdığınızda, genellikle hangi bileşenlerin birbirleriyle iletişim kurabilir denetlemek istersiniz. En düşük öncelik ilkesini nasıl trafiği Azure Kubernetes Service (AKS) kümesini pod'ların arasında akış için uygulanmalıdır. Büyük olasılıkla, trafiği doğrudan arka uç uygulamaları engellemek istiyorsunuz diyelim. *Ağ İlkesi* özellik kubernetes pod'ların bir küme arasında giriş ve çıkış trafiği için kuralları tanımlamanıza olanak sağlar.

Bu makalede, ağ ilke Altyapısı'nı ve AKS pod'ların arasındaki trafik akışını denetlemek için Kubernetes ağ ilkeleri oluşturmak nasıl gösterir. Ağ İlkesi, yalnızca Linux tabanlı düğümler ve pod'ların AKS için kullanılmalıdır.

## <a name="before-you-begin"></a>Başlamadan önce

Azure CLI Sürüm 2.0.61 gerekir veya daha sonra yüklü ve yapılandırılmış. Çalıştırma `az --version` sürümü bulmak için. Gerekirse yüklemek veya yükseltmek bkz [Azure CLI yükleme][install-azure-cli].

> [!TIP]
> Önizleme sırasında ağ ilkesi özelliğini kullandıysanız, öneririz, [yeni küme oluşturma](#create-an-aks-cluster-and-enable-network-policy).
> 
> Ağ İlkesi Önizleme sırasında kullanılan mevcut test kümelerini kullanmaya devam etmek istiyorsanız, en son GA sürümü için yeni bir Kubernetes sürümleri kümenizi yükseltmek ve ardından kilitlenen ölçümleri sunucu ve Kubernetes düzeltmek için aşağıdaki YAML bildirimi dağıtın Pano. Bu düzeltmedir yalnızca Calico ağ ilke altyapısı kullanılan kümeleri için gereklidir.
>
> Güvenlik açısından en iyisi [bu YAML bildirimin içeriğini gözden geçirin] [ calico-aks-cleanup] AKS kümeye dağıtılan anlamak için.
>
> `kubectl delete -f https://raw.githubusercontent.com/Azure/aks-engine/master/docs/topics/calico-3.3.1-cleanup-after-upgrade.yaml`

## <a name="overview-of-network-policy"></a>Ağ İlkesi'ne genel bakış

Bir AKS kümesindeki tüm pod'ların gönderebilir ve varsayılan olarak, kısıtlama olmadan trafik alır. Güvenliği artırmak için trafik akışını denetleyen kuralları tanımlayabilirsiniz. Arka uç uygulamaları için gerekli ön uç Hizmetleri, genellikle yalnızca örneğin sunulur. Veya, veritabanı bileşenlerini yalnızca bunlara uygulama katmanları tarafından erişilebilir.

Ağ İlkesi pod'ları arasındaki iletişim için erişim ilkeleri tanımlayan bir Kubernetes belirtimdir. Ağ ilkeleri kullanarak, göndermek ve trafiği almak ve bunları bir koleksiyona bir veya daha fazla etiket seçici eşleşen pod'ların uygulamak için kurallar kümesini tanımlarsınız.

Bu ağ ilke kuralları YAML bildirimleri olarak tanımlanır. Ağ ilkeleri ayrıca dağıtım veya hizmeti oluşturan daha geniş bir bildirim bir parçası olarak dahil edilebilir.

### <a name="network-policy-options-in-aks"></a>AKS içindeki Ağ İlkesi seçenekleri

Azure Ağ İlkesi'ni uygulamak için iki yol sunar. Bir AKS kümesi oluşturduğunuzda bir ağ ilkesi seçeneği belirleyin. İlke seçeneği, Küme oluşturulduktan sonra değiştirilemez:

* Adlı, Azure'un kendi uygulama *Azure ağ ilkeleri*.
* *Calico ağ ilkeleri*, bir açık kaynak ağı ve ağ güvenlik çözümü tarafından kurulan [Tigera][tigera].

Her iki uygulamalarını Linux kullanın *IPTables* belirtilen ilkelerini zorlamak için. İlkeler, izin verilen ve verilmeyen IP çiftleri kümeleri halinde çevrilir. Bu çiftler, ardından IPTable filtre kuralları programlanmıştır.

Ağ İlkesi yalnızca Azure CNI (Gelişmiş) seçeneği ile çalışır. Uygulama için iki seçenek farklıdır:

* *Azure ağ ilkeleri* -düğüm içi ağ için VM konağı alandaki bir köprünün Azure CNI ayarlar. Paketler köprü geçirdiğinizde filtreleme kurallarını uygulanır.
* *Calico ağ ilkeleri* -düğüm içi trafik için Yerel Çekirdek rotaları Azure CNI ayarlar. İlkeler, pod'ın ağ arabiriminde uygulanır.

### <a name="differences-between-azure-and-calico-policies-and-their-capabilities"></a>Azure ve Calico ilke ve yeteneklerini arasındaki farklar

| Özellik                               | Azure                      | Calico                      |
|------------------------------------------|----------------------------|-----------------------------|
| Desteklenen platformlar                      | Linux                      | Linux                       |
| Desteklenen ağ seçenekleri             | Azure CNI                  | Azure CNI                   |
| Kubernetes belirtimi ile uyumluluk | Desteklenen tüm ilke türleri |  Desteklenen tüm ilke türleri |
| Ek özellikler                      | None                       | Genel ağ ilkesi, genel ağ ayarlayın ve ana bilgisayar bitiş noktası oluşan ilke modeli genişletilmiş. Kullanma hakkında daha fazla bilgi için `calicoctl` bu özellikler, genişletilmiş yönetmek için CLI bkz [calicoctl kullanıcı başvurusu][calicoctl]. |
| Destek                                  | Azure destek ve mühendislik ekibi tarafından desteklenir | Calico topluluk desteği. Ek Ücretli destek hakkında daha fazla bilgi için bkz. [proje Calico destek seçenekleri][calico-support]. |
| Günlüğe kaydetme                                  | Eklenen / silindi IPTables kurallarını her konak altında oturum */var/log/azure-npm.log* | Daha fazla bilgi için [Calico bileşeni günlükleri][calico-logs] |

## <a name="create-an-aks-cluster-and-enable-network-policy"></a>AKS kümesi oluşturma ve Ağ İlkesi'ni etkinleştirin

Şimdi ağ ilkeleri, uygulamada görmek için oluşturun ve ardından trafik akışını tanımlayan bir ilkeyi açın:

* Pod için tüm trafiğe izin verme.
* Pod etiketlerine bağlı trafiğine izin verin.
* Ad alanını temel alan trafiğine izin verin.

İlk olarak, Ağ İlkesi'ni destekleyen bir AKS kümesi oluşturalım. Ağ İlkesi özelliği, yalnızca küme oluşturulduğunda etkinleştirilebilir. Ağ İlkesi var olan bir AKS kümesi üzerinde etkinleştirilemiyor.

Ağ İlkesi ile bir AKS kümesi kullanmak için kullanmalısınız [Azure CNI eklenti] [ azure-cni] ve kendi sanal ağ ve alt ağları tanımlayın. Ayrıntılı gerekli bir alt ağ aralıklarını planlama hakkında daha fazla bilgi için bkz. [Gelişmiş Ağ][use-advanced-networking].

Aşağıdaki örnek betik:

* Bir sanal ağ ve alt ağ oluşturur.
* Bir Azure Active Directory (Azure AD) ile bir AKS kümesi kullanmak için hizmet sorumlusu oluşturur.
* Atar *katkıda bulunan* izinlerini AKS Küme hizmet sorumlusu sanal ağ.
* Bir AKS kümesi içinde tanımlı sanal ağ oluşturur ve ağ ilkesi sağlar.
    * *Azure* Ağ İlkesi seçeneği kullanılır. Calico bunun yerine ağ ilke seçeneği olarak kullanmak için `--network-policy calico` parametresi.

Kendi güvenli sağlamak *SP_PASSWORD*. Değiştirebilirsiniz *RESOURCE_GROUP_NAME* ve *küme_adı* değişkenleri:

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
SP_ID=$(az ad sp create-for-rbac --password $SP_PASSWORD --skip-assignment --query [appId] -o tsv)

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
# Enable network policy by using the `--network-policy` parameter
az aks create \
    --resource-group $RESOURCE_GROUP_NAME \
    --name $CLUSTER_NAME \
    --node-count 1 \
    --generate-ssh-keys \
    --network-plugin azure \
    --service-cidr 10.0.0.0/16 \
    --dns-service-ip 10.0.0.10 \
    --docker-bridge-address 172.17.0.1/16 \
    --vnet-subnet-id $SUBNET_ID \
    --service-principal $SP_ID \
    --client-secret $SP_PASSWORD \
    --network-policy azure
```

Kümenin oluşturulması birkaç dakika sürer. Küme hazır olduğunda, yapılandırma `kubectl` kullanarak Kubernetes kümenize bağlanmak için [az aks get-credentials] [ az-aks-get-credentials] komutu. Bu komut, kimlik bilgilerini indirir ve Kubernetes CLI'yi bunları kullanacak şekilde yapılandırır:

```azurecli-interactive
az aks get-credentials --resource-group $RESOURCE_GROUP_NAME --name $CLUSTER_NAME
```

## <a name="deny-all-inbound-traffic-to-a-pod"></a>Bir pod tüm gelen trafiği engelle

Belirli ağ trafiğine izin vermek için kuralları tanımlamadan önce ilk tüm trafiği reddetmeye yönelik bir ağ ilkesi oluşturun. Bu ilke, yalnızca istenen trafiğe izin verilenler listesine başlamak için bir başlangıç noktası sunar. Ayrıca, Ağ İlkesi uygulandığında, trafik bırakıldı ayrıca NET bir şekilde görebilirsiniz.

Örnek uygulama ortamı ve trafik kuralları için ilk olarak adlandırılan bir ad alanı oluşturalım *geliştirme* örnek pod'ları çalıştırmak için:

```console
kubectl create namespace development
kubectl label namespace/development purpose=development
```

NGINX çalıştıran bir örnek arka uç pod oluşturun. Bu arka uç pod örnek arka uç web tabanlı bir uygulama benzetimini yapmak için kullanılabilir. Bu pod içinde oluşturma *geliştirme* ad ve bağlantı noktasını Aç *80* web trafiğini hizmet. Etiket ile pod *uygulama Web uygulamasında = rol arka uç =* böylece biz sonraki bölümde Ağ İlkesi'yle hedefleyebilir:

```console
kubectl run backend --image=nginx --labels app=webapp,role=backend --namespace development --expose --port 80 --generator=run-pod/v1
```

Başka bir pod oluşturabilir ve varsayılan NGINX Web sayfasının başarıyla ulaşabilirsiniz test etmek için bir terminal oturumu ekleyin:

```console
kubectl run --rm -it --image=alpine network-policy --namespace development --generator=run-pod/v1
```

Kabuk isteminde kullanmak `wget` varsayılan NGINX Web sayfası erişebildiğinizi onaylamak için:

```console
wget -qO- http://backend
```

Aşağıdaki örnek çıktıda, varsayılan NGINX Web sayfası döndürdüğünü gösterir:

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
[...]
```

Ekli terminal oturumu dışında çıkın. Test pod otomatik olarak silinir.

```console
exit
```

### <a name="create-and-apply-a-network-policy"></a>Oluşturma ve bir ağ ilkesi uygulama

Örnek arka uç pod temel bir NGINX Web sayfasını kullanabileceğiniz onayladıktan sonra tüm trafiği reddetmeye yönelik bir ağ ilkesi oluşturun. Adlı bir dosya oluşturun `backend-policy.yaml` aşağıdaki YAML bildirimi yapıştırın. Bu bildirimi kullanan bir *podSelector* sahip pod'ların ilkesi eklemek için *app:webapp, rol: arka uç* örnek NGINX pod gibi bir etiket. Altında hiçbir kural tanımlanmadı *giriş*, pod tüm gelen trafik engellenir:

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

Kullanarak ağ ilkesi uygulamak [kubectl uygulamak] [ kubectl-apply] komut ve YAML bildiriminizi adını belirtin:

```azurecli-interactive
kubectl apply -f backend-policy.yaml
```

### <a name="test-the-network-policy"></a>Ağ İlkesi test etme


Arka uç pod NGINX Web sayfasını yeniden kullanabilirsiniz, görelim. Başka bir test pod oluşturun ve terminal oturumu ekleyin:

```console
kubectl run --rm -it --image=alpine network-policy --namespace development --generator=run-pod/v1
```

Kabuk isteminde kullanmak `wget` varsayılan NGINX Web sayfasına erişip görmek için. Bu kez, ayarlanmış bir zaman aşımı değeri *2* saniye. Sayfa yüklenemiyor şekilde ağ ilkesi aşağıdaki örnekte gösterildiği gibi artık tüm gelen trafiği engeller:

```console
$ wget -qO- --timeout=2 http://backend

wget: download timed out
```

Ekli terminal oturumu dışında çıkın. Test pod otomatik olarak silinir.

```console
exit
```

## <a name="allow-inbound-traffic-based-on-a-pod-label"></a>Bir pod etikete göre gelen trafiğe izin vermek

Önceki bölümde, bir arka uç NGINX pod zamanlandı ve tüm trafiği reddetmeye yönelik bir ağ ilkesi oluşturuldu. Şimdi ön uç bir pod oluşturun ve ön uç pod'ların gelen trafiğe izin vermek için Ağ İlkesi güncelleştirin.

Pod'ların etiketlerle gelen trafiğe izin vermek için Ağ İlkesi güncelleştirme *app:webapp, rol: ön uç* ve herhangi bir ad alanı. Önceki Düzen *arka uç policy.yaml* dosya ve ekleme *matchLabels* giriş kuralları ve böylece bildiriminizi aşağıdaki örnekteki gibi görünür:

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

> [!NOTE]
> Bu ağ ilkeyi kullanan bir *namespaceSelector* ve *podSelector* giriş kuralı için öğesi. YAML söz dizimi giriş kurallarının olması önemlidir eklenebilir. Bu örnekte, iki öğeyi uygulanması için giriş kuralın eşleşmesi gerekir. Kubernetes sürümlerini öncesinde *1.12* değil Bu öğeleri doğru şekilde yorumlayabilir ve beklediğiniz gibi ağ trafiğini kısıtlayın. Bu davranışı hakkında daha fazla bilgi için bkz. [Seçici gelen ve giden davranışını][policy-rules].

Güncelleştirilmiş Ağ İlkesi kullanarak uygulama [kubectl uygulamak] [ kubectl-apply] komut ve YAML bildiriminizi adını belirtin:

```azurecli-interactive
kubectl apply -f backend-policy.yaml
```

Zamanlama olarak etiketlenmiş bir pod *uygulama Web uygulamasında = rol ön uç =* ve terminal oturumu ekleyin:

```console
kubectl run --rm -it frontend --image=alpine --labels app=webapp,role=frontend --namespace development --generator=run-pod/v1
```

Kabuk isteminde kullanmak `wget` varsayılan NGINX Web sayfasına erişip görmek için:

```console
wget -qO- http://backend
```

Trafik etiketlere sahip pod'ları ile giriş kural izin verdiğinden *uygulama: webapp, rol: ön uç*, ön uç pod gelen trafiğe izin verilir. Aşağıdaki örnek çıktıda, döndürülen varsayılan NGINX Web sayfası gösterilmektedir:

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
[...]
```

Ekli terminal oturumu dışında çıkın. Pod otomatik olarak silinir.

```console
exit
```

### <a name="test-a-pod-without-a-matching-label"></a>Eşleşen bir etiket olmadan bir pod test

Ağ İlkesi etiketli pod'ların gelen trafiğe izin verir *uygulama: webapp, rol: ön uç*, ancak diğer tüm trafiği reddetmeye. Şimdi bu etiket olmadan başka bir pod arka uç NGINX pod erişip erişemeyeceğini görmek için test edin. Başka bir test pod oluşturun ve terminal oturumu ekleyin:

```console
kubectl run --rm -it --image=alpine network-policy --namespace development --generator=run-pod/v1
```

Kabuk isteminde kullanmak `wget` varsayılan NGINX Web sayfasına erişip görmek için. Sayfa yüklenemiyor için ağ ilkesi aşağıdaki örnekte gösterildiği gibi gelen trafiği engeller:

```console
$ wget -qO- --timeout=2 http://backend

wget: download timed out
```

Ekli terminal oturumu dışında çıkın. Test pod otomatik olarak silinir.

```console
exit
```

## <a name="allow-traffic-only-from-within-a-defined-namespace"></a>İçinde tanımlanan bir ad alanı yalnızca gelen trafiğe izin ver

Önceki örneklerde, tüm trafiği reddedildi ve belirli bir etikete sahip pod'ların gelen trafiğe izin vermek ilkesi daha sonra güncelleştirilen bir ağ ilkesi oluşturdunuz. Yalnızca belirtilen bir ad alanı içinde trafiği sınırlamak için başka bir ortak gerekli değildir. Önceki örneklerde trafiği için olsaydı bir *geliştirme* ad alanı gibi başka bir ad alanı, gelen trafiği engelleyen bir ağ ilkesi oluşturma *üretim*, pod'ların ulaşmasını.

İlk olarak, bir üretim ad alanı benzetimini yapmak için yeni bir ad alanı oluşturun:

```console
kubectl create namespace production
kubectl label namespace/production purpose=production
```

Bir test pod içinde zamanlamak *üretim* olarak etiketli bir ad alanı *app = webapp, rol ön uç =*. Terminal oturumu ekleyin:

```console
kubectl run --rm -it frontend --image=alpine --labels app=webapp,role=frontend --namespace production --generator=run-pod/v1
```

Kabuk isteminde kullanmak `wget` varsayılan NGINX Web sayfası erişebildiğinizi onaylamak için:

```console
wget -qO- http://backend.development
```

Ağ İlkesi'nde şu anda izin pod etiketleri aynı gerektiğinden, trafiğe izin verilir. Ağ İlkesi isim uzaylarının, yalnızca pod etiketler gibi görünmüyor. Aşağıdaki örnek çıktıda, döndürülen varsayılan NGINX Web sayfası gösterilmektedir:

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
[...]
```

Ekli terminal oturumu dışında çıkın. Test pod otomatik olarak silinir.

```console
exit
```

### <a name="update-the-network-policy"></a>Ağ İlkesi güncelleştirme

Giriş kuralını güncelleştirelim *namespaceSelector* bölümünde yalnızca içinde gelen trafiğe izin vermek için *geliştirme* ad alanı. Düzen *arka uç policy.yaml* aşağıdaki örnekte gösterildiği gibi bildirim dosyası:

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

Daha karmaşık bir örnekte birden çok giriş kuralları gibi tanımlayabilirsiniz bir *namespaceSelector* ve ardından bir *podSelector*.

Güncelleştirilmiş Ağ İlkesi kullanarak uygulama [kubectl uygulamak] [ kubectl-apply] komut ve YAML bildiriminizi adını belirtin:

```azurecli-interactive
kubectl apply -f backend-policy.yaml
```

### <a name="test-the-updated-network-policy"></a>Güncelleştirilmiş Ağ İlkesi test etme

Başka bir pod içinde zamanlama *üretim* ad alanı ve terminal oturumu ekleyin:

```console
kubectl run --rm -it frontend --image=alpine --labels app=webapp,role=frontend --namespace production --generator=run-pod/v1
```

Kabuk isteminde kullanmak `wget` Ağ İlkesi artık trafiği engelleyen görmek için:

```console
$ wget -qO- --timeout=2 http://backend.development

wget: download timed out
```

Test pod dışında çıkış:

```console
exit
```

Engellenen gelen trafik ile *üretim* ad, zamanlama test pod yeniden *geliştirme* ad alanı ve terminal oturumu ekleyin:

```console
kubectl run --rm -it frontend --image=alpine --labels app=webapp,role=frontend --namespace development --generator=run-pod/v1
```

Kabuk isteminde kullanmak `wget` Ağ İlkesi trafiğe izin verdiğini görmek için:

```console
wget -qO- http://backend
```

Eşleşme ne ağ ilkesinde izin verilir ad alanında programlanmadığından pod trafiğe izin verilir. Aşağıdaki örnek çıktıda, döndürülen varsayılan NGINX Web sayfası gösterilmektedir:

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
[...]
```

Ekli terminal oturumu dışında çıkın. Test pod otomatik olarak silinir.

```console
exit
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makalede oluşturulan iki ad alanları ve ağ ilkesi uygulanır. Bu kaynakları temizlemek için kullanın [kubectl Sil] [ kubectl-delete] komut ve kaynak adlarını belirtin:

```console
kubectl delete namespace production
kubectl delete namespace development
```

## <a name="next-steps"></a>Sonraki adımlar

Ağ kaynakları hakkında daha fazla bilgi için bkz. [kavramları Azure Kubernetes Service (AKS) uygulamaları için ağ][concepts-network].

İlkeleri hakkında daha fazla bilgi edinmek için [Kubernetes ağ ilkelerini][kubernetes-network-policies].

<!-- LINKS - external -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubernetes-network-policies]: https://kubernetes.io/docs/concepts/services-networking/network-policies/
[azure-cni]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[policy-rules]: https://kubernetes.io/docs/concepts/services-networking/network-policies/#behavior-of-to-and-from-selectors
[aks-github]: https://github.com/azure/aks/issues
[tigera]: https://www.tigera.io/
[calicoctl]: https://docs.projectcalico.org/v3.6/reference/calicoctl/
[calico-support]: https://www.projectcalico.org/support
[calico-logs]: https://docs.projectcalico.org/v3.6/maintenance/component-logs
[calico-aks-cleanup]: https://github.com/Azure/aks-engine/blob/master/docs/topics/calico-3.3.1-cleanup-after-upgrade.yaml

<!-- LINKS - internal -->
[install-azure-cli]: /cli/azure/install-azure-cli
[use-advanced-networking]: configure-advanced-networking.md
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[concepts-network]: concepts-network.md
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
