---
title: "Azure Kubernetes küme web kullanıcı Arabirimi ile yönetme"
description: "İçinde AKS Kubernetes panosunu kullanma"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 11/15/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: ca828dab7bdb47e41596be2717598cfe828953ca
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="kubernetes-dashboard-with-azure-container-service-aks"></a>Kubernetes Pano Azure kapsayıcı hizmeti (AKS)

Azure CLI Kubernetes Pano başlatmak için kullanılabilir. Bu belge Kubernetes Pano Azure CLI ile başlayan aracılığıyla anlatılmaktadır ve ayrıca bazı temel Pano işlemleri aracılığıyla anlatılmaktadır. Kubernetes Pano bakın, daha fazla bilgi için [Kubernetes Web kullanıcı Arabirimi Panosu][kubernetes-dashboard].

## <a name="before-you-begin"></a>Başlamadan önce

Bu belgedeki adımlarda bir AKS kümesi oluşturduğunuz ve kümeyle bir kubectl bağlantısı kurduğunuz kabul edilmektedir. Bu öğeler gereksinim duyarsanız, bkz: [AKS quickstart][aks-quickstart].

Ayrıca Azure CLI sürüm 2.0.21 veya üzerini yüklemiş ve yapılandırmış olmanız gerekir. Sürümü bulmak için az --version komutunu çalıştırın. Gerekirse yükleyin veya yükseltin, bakın [Azure CLI yükleme][install-azure-cli].

## <a name="start-kubernetes-dashboard"></a>Başlangıç Kubernetes Panosu

Kullanım `az aks browse` Kubernetes Pano başlatmak için komutu. Bu komutu çalıştırdığınızda kaynak grubu ve küme adını değiştirin.

```azurecli
az aks browse --resource-group myResourceGroup --name myK8SCluster
```

Bu komut, geliştirme sisteminizde Kubernetes API arasındaki bir proxy oluşturur ve bir web tarayıcısı Kubernetes panoya açar.

## <a name="run-an-application"></a>Bir uygulamayı çalıştırın

Kubernetes panosunda tıklatın **oluşturma** üst sağ penceresinde düğmesini. Dağıtım adını verin `nginx` ve girin `nginx:latest` görüntüleri adı. Altında **hizmet**seçin **dış** ve girin `80` bağlantı noktası ve hedef bağlantı noktası.

Hazır olduğunuzda tıklatın **dağıtma** dağıtımı oluşturun.

![Kubernetes hizmeti oluşturma iletişim kutusu](./media/container-service-kubernetes-ui/create-deployment.png)

## <a name="view-the-application"></a>Uygulama görüntüleyin

Uygulama hakkında durum Kubernetes Panoda görülebilir. Uygulama çalışmaya başladıktan sonra her bileşen yanında yeşil bir onay kutusu sahiptir.

![Kubernetes pod'ları](./media/container-service-kubernetes-ui/complete-deployment.png)

Uygulama pod'ları hakkında daha fazla bilgi görmek için tıklayın **pod'ları** sol taraftaki menüyü ve ardından **NGINX** pod. Burada kaynak tüketimi gibi pod özgü bilgileri görebilirsiniz.

![Kubernetes kaynakları](./media/container-service-kubernetes-ui/running-pods.png)

Uygulamanın IP adresini bulmak için tıklayın **Hizmetleri** sol taraftaki menüyü ve ardından **NGINX** hizmet.

![nginx görünümü](./media/container-service-kubernetes-ui/nginx-service.png)

## <a name="edit-the-application"></a>Uygulamasını Düzenle

Oluşturma ve görüntüleme uygulamaları ek olarak, Kubernetes panoyu düzenleyebilir ve güncelleştirme uygulama dağıtımları için kullanılabilir.

Bir dağıtım düzenlemek için tıklatın **dağıtımları** sol taraftaki menüyü ve ardından **NGINX** dağıtım. Son olarak, seçin **Düzenle** üst taraftaki gezinti çubuğunda.

![Kubernetes Düzenle](./media/container-service-kubernetes-ui/view-deployment.png)

Bulun `spec.replica` 1 olmalıdır, değeri 3 olarak bu değerle değiştirin. Bunu yaparken, NGINX dağıtım yineleme sayısı 1'den 3'e artar.

Seçin **güncelleştirme** zaman hazır.

![Kubernetes Düzenle](./media/container-service-kubernetes-ui/edit-deployment.png)

## <a name="next-steps"></a>Sonraki adımlar

Kubernetes Pano hakkında daha fazla bilgi için Kubernetes belgelerine bakın.

> [!div class="nextstepaction"]
> [Kubernetes Web kullanıcı Arabirimi Panosu][kubernetes-dashboard]

<!-- LINKS - external -->
[kubernetes-dashboard]: https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[install-azure-cli]: /cli/azure/install-azure-cli