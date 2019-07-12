---
title: Web panosu ile bir Azure Kubernetes Service kümesini yönetme
description: Azure Kubernetes Service (AKS) kümesini yönetmek için yerleşik Kubernetes web kullanıcı Arabirimi Panosu kullanmayı öğrenin
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 10/08/2018
ms.author: mlearned
ms.openlocfilehash: 0de2f285b5eca88a098a2d7cfe1608ad2f0db71b
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67615232"
---
# <a name="access-the-kubernetes-web-dashboard-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) Kubernetes web panosuna erişme

Kubernetes temel yönetim işlemlerini için kullanılabilecek bir web Pano içerir. Bu pano, temel sistem durumu ve uygulamalarınız için ölçümleri görüntüleme, oluşturma ve Hizmetleri dağıtın ve mevcut uygulamaları düzenlemek olanak tanır. Bu makalede, Azure CLI kullanarak Kubernetes panosuna erişme işlemi gösterilir ve ardından, bazı temel Pano işlemleri aracılığıyla size yol gösterir.

Kubernetes panosunu hakkında daha fazla bilgi için bkz. [Kubernetes Web kullanıcı Arabirimi Panosu][kubernetes-dashboard].

## <a name="before-you-begin"></a>Başlamadan önce

Bu belgedeki adımlarda bir AKS kümesi oluşturduğunuz ve belirledik varsayılır bir `kubectl` kümeyle bağlantı. Bir AKS kümesi oluşturmak için ihtiyacınız varsa bkz [AKS hızlı başlangıçları][aks-quickstart].

Ayrıca Azure CLI sürüm 2.0.46 veya üzerini yüklemiş ve yapılandırmış olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Gerekirse yüklemek veya yükseltmek bkz [Azure CLI yükleme][install-azure-cli].

## <a name="start-the-kubernetes-dashboard"></a>Kubernetes panosunu başlatmak

Kubernetes panosunu başlatmak için [az aks Gözat][az-aks-browse] komutu. Aşağıdaki örnekte adlı Küme için Pano açılır *myAKSCluster* adlı kaynak grubunda *myResourceGroup*:

```azurecli
az aks browse --resource-group myResourceGroup --name myAKSCluster
```

Bu komut, Kubernetes API ile geliştirme sisteminizde arasındaki bir proxy oluşturur ve bir web tarayıcı Kubernetes panosunu açar. Bir web tarayıcı Kubernetes panosunu açık değilse, Azure CLI, genellikle belirtilen URL adresini kopyalayıp yapıştırın `http://127.0.0.1:8001`.

![Kubernetes web panonun genel bakış sayfası](./media/kubernetes-dashboard/dashboard-overview.png)

### <a name="for-rbac-enabled-clusters"></a>Kümeler için RBAC etkin

AKS kümenizi RBAC, kullanıyorsa bir *ClusterRoleBinding* Pano doğru bir şekilde erişebilmeniz için önce oluşturulması gerekir. Varsayılan olarak, Kubernetes panosunu en az okuma erişimi ile dağıtılır ve RBAC erişim hataları görüntüler. Kubernetes panosuna erişim düzeyini belirlemek için kullanıcı tarafından sağlanan kimlik bilgileri şu anda desteklemiyor, bunun yerine hizmet hesabına verilen rolleri kullanır. Ek erişim izni vermek bir Küme Yöneticisi seçebilirsiniz *kubernetes panosunu* hizmet hesabı, ancak bu ayrıcalık yükseltme için vektör olabilir. Daha ayrıntılı bir düzeyde erişim sağlamak için Azure Active Directory kimlik doğrulaması tümleştirebilirler.

Bağlama oluşturmak için kullanın [kubectl oluşturma clusterrolebinding][kubectl-create-clusterrolebinding] komutu aşağıdaki örnekte gösterildiği gibi. 

> [!WARNING]
> Bu örnek bağlama herhangi bir ek kimlik doğrulama bileşeni geçerli değildir ve güvensiz kullanımına neden olabilir. Kubernetes panosunu herkese açık erişim URL'si. Kubernetes panosunu genel olarak açığa çıkarmayın.
>
> Kubernetes Panosu wiki görmek için farklı kimlik doğrulama yöntemlerini kullanarak daha fazla bilgi için [erişim denetimleri][dashboard-authentication].

```console
kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
```

Kubernetes panosunu RBAC özellikli kümenizde artık erişebilirsiniz. Kubernetes panosunu başlatmak için [az aks Gözat][az-aks-browse] önceki adımda açıklandığı komutu.

## <a name="create-an-application"></a>Uygulama oluşturma

Kubernetes panosunu karmaşıklığını yönetim görevlerinin nasıl azaltabilirsiniz görmek için bir uygulama oluşturalım. Metin girişi, bir YAML dosyası sağlayarak veya Grafik Sihirbazı ile Kubernetes panodan bir uygulama oluşturabilirsiniz.

Bir uygulama oluşturmak için aşağıdaki adımları tamamlayın:

1. Seçin **Oluştur** üst sağ pencerede düğmesi.
1. Grafik Sihirbazı'nı kullanmayı tercih **uygulama oluşturma**.
1. Dağıtım için bir ad sağlayın *nginx*
1. Gibi kullanmanız kapsayıcı görüntüsü için bir ad girin *nginx:1.15.5*
1. Web trafiği için 80 numaralı bağlantı noktasını kullanıma sunmak için bir Kubernetes hizmeti oluşturun. Altında **hizmet**seçin **dış**, enter **80** bağlantı noktası ve hedef bağlantı noktası.
1. Hazır olduğunuzda seçin **Dağıt** uygulamayı oluşturmak için.

![Kubernetes web panosunda uygulama dağıtma](./media/kubernetes-dashboard/create-app.png)

Bir veya iki Kubernetes hizmetine atanan genel bir dış IP adresi için dakika sürer. Sol taraftaki boyutu altında **bulma ve Yük Dengeleme** seçin **Hizmetleri**. Dahil olmak üzere, uygulamanızın hizmet listelenir *dış uç noktalar*, aşağıdaki örnekte gösterildiği gibi:

![Hizmet ve uç nokta listesini görüntüleyin](./media/kubernetes-dashboard/view-services.png)

Uç nokta adresini bir web tarayıcısı penceresinde varsayılan NGINX sayfasını açmak için seçin:

![Dağıtılan uygulamanın varsayılan NGINX sayfasını görüntüle](./media/kubernetes-dashboard/default-nginx.png)

## <a name="view-pod-information"></a>Pod bilgilerini görüntüle

Kubernetes panosunu temel ölçümleri izleme ve sorun giderme günlükleri gibi bilgileri sağlar.

Uygulama pod'ları hakkında daha fazla bilgi için seçin **pod'ların** sol menüdeki. Kullanılabilir pod'ların listesi gösterilir. Seçin, *ngınx* pod kaynak tüketimi gibi bilgileri görüntülemek için:

![Pod bilgilerini görüntüle](./media/kubernetes-dashboard/view-pod-info.png)

## <a name="edit-the-application"></a>Uygulamayı Düzenle

Oluşturma ve görüntüleme uygulamaları yanı sıra Kubernetes panosunu, düzenlemek ve güncelleştirme uygulama dağıtımları için kullanılabilir. Uygulama için ek yedeklilik sağlamak için şimdi NGINX yineleme sayısını artırın.

Bir dağıtım düzenlemek için:

1. Seçin **dağıtımları** soldaki menüden seçin, *ngınx* dağıtım.
1. Seçin **Düzenle** üst sağ gezinti çubuğunda.
1. Bulun `spec.replica` değeri, adresindeki geçici olarak 20 satır. Uygulama için yineleme sayısını artırmak için bu değeri değiştirmek *1* için *3*.
1. Seçin **güncelleştirme** ne zaman hazır.

![Yineleme sayısını güncelleştirmek için dağıtımı düzenleyin](./media/kubernetes-dashboard/edit-deployment.png)

Yeni pod çoğaltma kümesi içinde oluşturulması birkaç dakika sürer. Sol taraftaki menüden seçin **çoğaltma kümeleri**ve ardından, *ngınx* çoğaltma kümesi. Pod'ların listesini artık aşağıdaki örnek çıktıda gösterildiği gibi güncelleştirilmiş yineleme sayısını yansıtır:

![Çoğaltma kümesi hakkındaki bilgileri görüntüleyin](./media/kubernetes-dashboard/view-replica-set.png)

## <a name="next-steps"></a>Sonraki adımlar

Kubernetes Panosu hakkında daha fazla bilgi için bkz. [Kubernetes Web kullanıcı Arabirimi Panosu][kubernetes-dashboard].

<!-- LINKS - external -->
[kubernetes-dashboard]: https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/
[dashboard-authentication]: https://github.com/kubernetes/dashboard/wiki/Access-control
[kubectl-create-clusterrolebinding]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-clusterrolebinding-em-
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[install-azure-cli]: /cli/azure/install-azure-cli
[az-aks-browse]: /cli/azure/aks#az-aks-browse
