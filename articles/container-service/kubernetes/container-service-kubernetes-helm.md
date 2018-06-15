---
title: Azure Kubernetes Helm ile kapsayıcıları dağıtın
description: Azure kapsayıcı hizmeti Kubernetes kümesinde kapsayıcıları dağıtmak için Helm paketleme Aracı'nı kullanın
services: container-service
author: sauryadas
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 04/10/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 882e785968f94473e80c7a14e5a68498add37735
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32163104"
---
# <a name="use-helm-to-deploy-containers-on-a-kubernetes-cluster"></a>Helm kapsayıcıları Kubernetes kümede dağıtmak için kullanın

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

[Helm](https://github.com/kubernetes/helm/) yükleyip Kubernetes uygulamaların yaşam döngüsünü yönetmenize yardımcı olan bir açık kaynak paketleme aracıdır. Benzer şekilde Linux paketi yöneticileri Apt get ve Yum gibi Helm önceden yapılandırılmış Kubernetes kaynakların paketleri Kubernetes grafikler, yönetmek için kullanılır. Bu makalede, Azure kapsayıcı Hizmeti'nde dağıtılan Kubernetes kümede Helm ile çalışmaya nasıl gösterilmektedir.

Helm iki bileşenden oluşur: 
* **Helm CLI** makinenizde yerel olarak veya bulutta çalışan bir istemci  

* **Tiller** Kubernetes kümede çalışan ve Kubernetes uygulamalarınızı yaşam döngüsü yöneten bir sunucu 
 
## <a name="prerequisites"></a>Önkoşullar

* [Kubernetes küme oluşturma](container-service-kubernetes-walkthrough.md) Azure kapsayıcı Hizmeti'nde

* [Yükleme ve yapılandırma `kubectl` ](../container-service-connect.md) yerel bir bilgisayarda

* [Helm yüklemek](https://github.com/kubernetes/helm/blob/master/docs/install.md) yerel bir bilgisayarda

## <a name="helm-basics"></a>Helm temelleri 

Tiller yükleme ve uygulamalarınızı dağıtma olduğundan Kubernetes küme hakkındaki bilgileri görüntülemek için aşağıdaki komutu yazın:

```bash
kubectl cluster-info 
```
![kubectl küme bilgisi](./media/container-service-kubernetes-helm/clusterinfo.png)
 
Tiller Helm yükledikten sonra aşağıdaki komutu yazarak Kubernetes kümenizde yükleyin:

```bash
helm init --upgrade
```
Başarıyla tamamlandıktan sonra aşağıdaki gibi bir çıktı görürsünüz:

![Tiller yükleme](./media/container-service-kubernetes-helm/tiller-install.png)
 
 
 
 
Havuzda kullanılabilir tüm Helm grafikler görüntülemek için aşağıdaki komutu yazın:

```bash 
helm search 
```

Aşağıdaki gibi bir çıktı görürsünüz:

![Helm arama](./media/container-service-kubernetes-helm/helm-search.png)
 
En son sürümlerini almak için grafikler güncelleştirmek için şunu yazın:

```bash 
helm repo update 
```
## <a name="deploy-an-nginx-ingress-controller-chart"></a>Bir Nginx giriş denetleyicisi grafik dağıtma 
 
Bir Nginx giriş denetleyicisi grafik dağıtmak için tek bir komut yazın:

```bash
helm install stable/nginx-ingress 
```
![Giriş denetleyicisi dağıtma](./media/container-service-kubernetes-helm/nginx-ingress.png)

Yazarsanız, `kubectl get svc` küme üzerinde çalışan tüm hizmetleri görüntülemek için bir IP adresi giriş denetleyiciye atanır bakın. (Atama işlemi devam ederken, gördüğünüz `<pending>`. Birkaç dakika sürer.) 

Sonra IP adresi atanır, çalışan Nginx arka uç görmek için harici IP adresi değerine gidin. 
 
![Giriş IP adresi](./media/container-service-kubernetes-helm/ingress-ip-address.png)


Grafikler, kümeye yüklü listesini görmek için şunu yazın:

```bash
helm list 
```

Komuta kısaltma `helm ls`.
 
 
 
 
## <a name="deploy-a-mariadb-chart-and-client"></a>MariaDB grafik ve istemci dağıtma

Şimdi bir MariaDB grafik ve veritabanına bağlanmak için bir MariaDB istemci dağıtın.

MariaDB grafik dağıtmak için aşağıdaki komutu yazın:

```bash
helm install --name v1 stable/mariadb
```

Burada `--name` olan sürümleri için kullanılan bir etiket.

> [!TIP]
> Dağıtım başarısız olursa çalıştırmak `helm repo update` ve yeniden deneyin.
>
 
 
Kümenizde dağıtılan tüm grafikler görüntülemek için şunu yazın:

```bash 
helm list
```
 
Küme üzerinde çalışan tüm dağıtımları görüntülemek için şunu yazın:

```bash
kubectl get deployments 
``` 
 
 
Son olarak, istemci erişimi için bir pod çalıştırmak için şunu yazın:

```bash
kubectl run v1-mariadb-client --rm --tty -i --image bitnami/mariadb --command -- bash  
``` 
 
 
İstemciye bağlanmak için aşağıdaki komutu yazın komutu, değiştirme `v1-mariadb` dağıtımınızı adı:

```bash
sudo mysql –h v1-mariadb
```
 
 
Şimdi, veritabanları, tablolar vb. oluşturmak için standart SQL komutlarını da kullanabilirsiniz. Örneğin, `Create DATABASE testdb1;` boş bir veritabanı oluşturur. 
 
 
 
## <a name="next-steps"></a>Sonraki adımlar

* Kubernetes grafikleri yönetme hakkında daha fazla bilgi için bkz: [Helm belgelerine](https://github.com/kubernetes/helm/blob/master/docs/index.md). 

