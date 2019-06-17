---
title: (KULLANIM DIŞI) Azure kubernetes Helm ile kapsayıcıları dağıtın
description: Azure Container Service'te bir Kubernetes kümesinde kapsayıcıları dağıtmak için Helm paketleme Aracı'nı kullanın
services: container-service
author: sauryadas
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 04/10/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 05edbf40e8cd5f8edbdc8b74b540962b1a25c8de
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60712329"
---
# <a name="deprecated-use-helm-to-deploy-containers-on-a-kubernetes-cluster"></a>(KULLANIM DIŞI) Bir Kubernetes kümesinde kapsayıcıları dağıtmak için Helm kullanın

> [!TIP]
> Bu makale, güncelleştirilmiş sürümü kullanan Azure Kubernetes Service için bkz: [Azure Kubernetes Service (AKS) Helm ile uygulamaları yükleme](../../aks/kubernetes-helm.md).

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

[Helm](https://github.com/kubernetes/helm/) yükleyin ve Kubernetes uygulamaların yaşam döngüsünü yönetmenize yardımcı olan bir açık kaynak paketleme aracıdır. Benzer şekilde Apt-get ve Yum gibi Linux paket yöneticileri, Helm Kubernetes grafikleri, önceden yapılandırılmış Kubernetes kaynak paketleri yönetmek için kullanılır. Bu makalede, dağıtılan Azure Container Service'te bir Kubernetes kümesi üzerinde Helm ile çalışacak şekilde nasıl gösterir.

Helm iki bileşenden oluşur: 
* **Helm CLI** makinenizde yerel olarak veya bulutta çalışan bir istemci  

* **Tiller** Kubernetes kümesinde çalışır ve Kubernetes uygulamalarınızın yaşam döngüsünü yöneten bir sunucu 
 
## <a name="prerequisites"></a>Önkoşullar

* [Bir Kubernetes kümesi oluşturma](container-service-kubernetes-walkthrough.md) Azure Container Service'te

* [Yükleme ve yapılandırma `kubectl` ](../container-service-connect.md) yerel bir bilgisayardaki

* [Helm yükleme](https://github.com/kubernetes/helm/blob/master/docs/install.md) yerel bir bilgisayardaki

## <a name="helm-basics"></a>Helm temelleri 

Tiller yüklediğiniz ve uygulamalarınızı dağıtmak, Kubernetes kümesi hakkındaki bilgileri görüntülemek için aşağıdaki komutu yazın:

```bash
kubectl cluster-info 
```
![kubectl küme-info](./media/container-service-kubernetes-helm/clusterinfo.png)
 
Helm yükledikten sonra aşağıdaki komutu yazarak Tiller Kubernetes kümenizde yükleyin:

```bash
helm init --upgrade
```
Başarıyla tamamlandığında, aşağıdakine benzer bir çıktı görürsünüz:

![Tiller yükleme](./media/container-service-kubernetes-helm/tiller-install.png)
 
 
 
 
Depoda kullanılabilir tüm Helm grafikleri görüntülemek için aşağıdaki komutu yazın:

```bash 
helm search 
```

Aşağıdakine benzer bir çıktı görürsünüz:

![Helm arama](./media/container-service-kubernetes-helm/helm-search.png)
 
En son sürümlerini almak için grafikler güncelleştirmek için şunu yazın:

```bash 
helm repo update 
```
## <a name="deploy-an-nginx-ingress-controller-chart"></a>Bir Nginx giriş denetleyicisine grafik dağıtma 
 
Bir Nginx giriş denetleyicisine grafik dağıtmak için tek bir komut yazın:

```bash
helm install stable/nginx-ingress 
```
![Giriş denetleyicisi dağıtma](./media/container-service-kubernetes-helm/nginx-ingress.png)

Yazarsanız `kubectl get svc` küme üzerinde çalışan tüm hizmetleri görüntülemek için bir IP adresi giriş denetleyicisine atandığından emin bakın. (Atama işlemi devam ederken, gördüğünüz `<pending>`. Birkaç dakika sürer.) 

Sonra IP adresi atanır, değeri olarak çalışan Ngınx arka uç görmek için dış IP adresine gidin. 
 
![Giriş IP adresi](./media/container-service-kubernetes-helm/ingress-ip-address.png)


Kümenizde yüklü grafikleri listesini görmek için şunu yazın:

```bash
helm list 
```

Komutu kısaltabilirsiniz `helm ls`.
 
 
 
 
## <a name="deploy-a-mariadb-chart-and-client"></a>MariaDB grafiği ve istemci dağıtma

Şimdi bir MariaDB grafik ve MariaDB veritabanı'na bağlanma istemciye dağıtın.

MariaDB grafik dağıtmak için aşağıdaki komutu yazın:

```bash
helm install --name v1 stable/mariadb
```

Burada `--name` sürümleri için kullanılan bir etikettir.

> [!TIP]
> Dağıtım başarısız olursa çalıştırma `helm repo update` ve yeniden deneyin.
>
 
 
Kümenizde dağıttığınız tüm grafikleri görüntülemek için şunu yazın:

```bash 
helm list
```
 
Kümenizde çalışan tüm dağıtımları görüntülemek için şunu yazın:

```bash
kubectl get deployments 
``` 
 
 
Son olarak, istemci erişimi için bir pod çalıştırmak için şunu yazın:

```bash
kubectl run v1-mariadb-client --rm --tty -i --image bitnami/mariadb --command -- bash  
``` 
 
 
İstemciye bağlanmak için aşağıdaki komutu yazın komutuyla değiştirerek `v1-mariadb` dağıtım adı:

```bash
sudo mysql –h v1-mariadb
```
 
 
Şimdi, veritabanları, tablolar oluşturmak için standart SQL komutlarını da kullanabilirsiniz. Örneğin, `Create DATABASE testdb1;` boş bir veritabanı oluşturur. 
 
 
 
## <a name="next-steps"></a>Sonraki adımlar

* Kubernetes grafikleri yönetme hakkında daha fazla bilgi için bkz. [Helm belgeleri](https://github.com/kubernetes/helm/blob/master/docs/index.md). 

