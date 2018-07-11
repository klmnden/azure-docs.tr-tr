---
title: Azure Kubernetes kümesi web kullanıcı Arabirimi ile yönetme
description: Yerleşik Kubernetes web kullanıcı Arabirimi Panosu Azure Kubernetes Service (AKS) kullanmayı öğrenin
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/09/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: e197a251df3f34e5416bafacfd54a3fc7f51d503
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37928225"
---
# <a name="access-the-kubernetes-dashboard-with-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) ile Kubernetes panosuna erişme

Kubernetes temel yönetim işlemlerini için kullanılabilecek bir web Pano içerir. Bu makalede, Azure CLI kullanarak Kubernetes panosuna erişme işlemi gösterilir ve ardından, bazı temel Pano işlemleri aracılığıyla size yol gösterir. Kubernetes panosunu hakkında daha fazla bilgi için bkz. [Kubernetes Web kullanıcı Arabirimi Panosu][kubernetes-dashboard].

## <a name="before-you-begin"></a>Başlamadan önce

Bu belgedeki adımlarda bir AKS kümesi oluşturduğunuz ve belirledik varsayılır bir `kubectl` kümeyle bağlantı. Bir AKS kümesi oluşturmak için ihtiyacınız varsa bkz [AKS hızlı başlangıçları][aks-quickstart].

Ayrıca Azure CLI sürüm 2.0.27 veya üzerini yüklemiş ve yapılandırmış olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][install-azure-cli].

## <a name="start-kubernetes-dashboard"></a>Başlangıç Kubernetes Panosu

Kubernetes panosunu başlatmak için [az aks Gözat] [ az-aks-browse] komutu. Aşağıdaki örnekte adlı Küme için Pano açılır *myAKSCluster* adlı kaynak grubunda *myResourceGroup*:

```azurecli
az aks browse --resource-group myResourceGroup --name myAKSCluster
```

Bu komut, Kubernetes API ile geliştirme sisteminizde arasındaki bir proxy oluşturur ve bir web tarayıcı Kubernetes panosunu açar.

### <a name="for-rbac-enabled-clusters"></a>Kümeler için RBAC etkin

AKS kümenizi RBAC, kullanıyorsa bir *ClusterRoleBinding* Pano erişebilmeniz için önce oluşturulması gerekir. Bir rol bağlama olmadan Azure CLI aşağıdaki örneğe benzer şekilde, bir hatayı döndürür:

```
error: unable to forward port because pod is not running. Current status=Pending
```

Bir bağlamayı oluşturmak için adlı bir dosya oluşturun. *Pano admin.yaml* ve aşağıdaki örnek yapıştırın. Bu örnek bağlama herhangi bir ek kimlik doğrulama bileşeni geçerli değildir. Taşıyıcı belirteçleri veya Pano ve ne erişebilen denetlemek için bir kullanıcı adı/parola gibi mekanizmalar kullanabilirsiniz izinlere sahiptirler. Kimlik doğrulama yöntemleri hakkında daha fazla bilgi için Kubernetes Panosu wiki bakın [erişim denetimleri][dashboard-authentication].

```yaml
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: kubernetes-dashboard
  labels:
    k8s-app: kubernetes-dashboard
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: kubernetes-dashboard
  namespace: kube-system
```

Bağlama ile uygulama [kubectl uygulamak] [ kubectl-apply] ve belirtin, *Pano admin.yaml*aşağıdaki örnekte gösterildiği gibi:

```
$ kubectl apply -f dashboard-admin.yaml

clusterrolebinding.rbac.authorization.k8s.io/kubernetes-dashboard created
```

Kubernetes panosunu RBAC özellikli kümenizde artık erişebilirsiniz. Kubernetes panosunu başlatmak için [az aks Gözat] [ az-aks-browse] önceki adımda açıklandığı komutu.


## <a name="run-an-application"></a>Bir uygulamayı çalıştırma

Kubernetes Pano tıklayın **Oluştur** üst sağ pencerede düğmesi. Dağıtım adını verin `nginx` girin `nginx:latest` kapsayıcı görüntü adı için. Altında **hizmet**seçin **dış** girin `80` bağlantı noktası ve hedef bağlantı noktası.

Hazır olduğunuzda tıklayın **Dağıt** dağıtımı oluşturun.

![Kubernetes hizmeti oluşturma iletişim kutusu](./media/container-service-kubernetes-ui/create-deployment.png)

## <a name="view-the-application"></a>Uygulamayı görüntüleme

Kubernetes Panosu üzerinde uygulama hakkında durum görülebilir. Uygulama çalıştıktan sonra her bileşen yanında yeşil bir onay kutusu sahiptir.

![Kubernetes pod'ları](./media/container-service-kubernetes-ui/complete-deployment.png)

Uygulama pod'ları hakkında daha fazla bilgi için tıklayın **pod'ların** seçin ve soldaki menüden **NGINX** pod. Burada, kaynak tüketimi gibi pod özgü bilgileri görebilirsiniz.

![Kubernetes kaynakları](./media/container-service-kubernetes-ui/running-pods.png)

Uygulamanın IP adresini bulmak için tıklayın **Hizmetleri** seçin ve soldaki menüden **NGINX** hizmeti.

![ngınx görüntüle](./media/container-service-kubernetes-ui/nginx-service.png)

## <a name="edit-the-application"></a>Uygulamayı Düzenle

Oluşturma ve görüntüleme uygulamaları yanı sıra Kubernetes panosunu, düzenlemek ve güncelleştirme uygulama dağıtımları için kullanılabilir.

Bir dağıtım düzenlemek için tıklayın **dağıtımları** seçin ve soldaki menüden **NGINX** dağıtım. Son olarak, seçin **Düzenle** üst sağ gezinti çubuğunda.

![Kubernetes Düzenle](./media/container-service-kubernetes-ui/view-deployment.png)

Bulun `spec.replica` değeri 1 olmalıdır, bu değeri 3'e değiştirin. Bunun yapılması, NGINX dağıtım yineleme sayısı 1'den 3'e yükseltildi.

Seçin **güncelleştirme** ne zaman hazır.

![Kubernetes Düzenle](./media/container-service-kubernetes-ui/edit-deployment.png)

## <a name="next-steps"></a>Sonraki adımlar

Kubernetes Panosu hakkında daha fazla bilgi için Kubernetes belgelerine bakın.

> [!div class="nextstepaction"]
> [Kubernetes Web kullanıcı Arabirimi Panosu][kubernetes-dashboard]

<!-- LINKS - external -->
[kubernetes-dashboard]: https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/
[dashboard-authentication]: https://github.com/kubernetes/dashboard/wiki/Access-control
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[install-azure-cli]: /cli/azure/install-azure-cli
[az-aks-browse]: /cli/azure/aks#az-aks-browse
