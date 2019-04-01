---
title: Azure için Açık Hizmet Aracısı (OSBA) kullanarak Azure tarafından yönetilen hizmetlerle tümleştirme
description: Azure için Açık Hizmet Aracısı (OSBA) kullanarak Azure tarafından yönetilen hizmetlerle tümleştirme
services: container-service
author: zr-msft
manager: jeconnoc
ms.service: container-service
ms.topic: overview
ms.date: 12/05/2017
ms.author: zarhoads
ms.openlocfilehash: 7a887905bcb4b09c1b4ae179116b3f08c75caabd
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58758280"
---
# <a name="integrate-with-azure-managed-services-using-open-service-broker-for-azure-osba"></a>Azure için Açık Hizmet Aracısı (OSBA) kullanarak Azure tarafından yönetilen hizmetlerle tümleştirme

[Kubernetes Hizmet Kataloğu][kubernetes-service-catalog] ile birlikte Azure için Açık Hizmet Aracısı (OSBA), geliştiricilerin Kubernetes'te Azure tarafından yönetilen hizmetleri kullanmasına izin verir. Bu kılavuz; Kubernetes Hizmet Kataloğu, Azure için Açık Hizmet Aracısı (OSBA) ve Azure tarafından yönetilen hizmetleri kullanan uygulamaları dağıtmaya odaklanır.

## <a name="prerequisites"></a>Önkoşullar
* Bir Azure aboneliği

* Azure CLI: [Yerel olarak yükleyebilir][azure-cli-install] veya [Azure Cloud Shell][azure-cloud-shell]'de kullanabilirsiniz.

* Helm CLI 2.7+: [Yerel olarak yükleyebilir][helm-cli-install] veya [Azure Cloud Shell][azure-cloud-shell]'de kullanabilirsiniz.

* Azure aboneliğinizde Katkıda Bulunan rolü ile bir hizmet sorumlusu oluşturma izinleri

* Mevcut bir Azure Kubernetes Hizmeti (AKS) kümesi. Bir AKS kümesi gerekirse [AKS kümesi oluşturma][create-aks-cluster] hızlı başlangıcını izleyin.

## <a name="install-service-catalog"></a>Hizmet Kataloğu yükleme

Birinci adım, bir Helm grafiği kullanarak Kubernetes kümenize Hizmet Kataloğu yüklemektir. Kümenizdeki Tiller (Helm sunucusu) yüklemesini şununla yükseltin:

```azurecli-interactive
helm init --upgrade
```

Şimdi, Hizmet Kataloğu grafiğini Helm deposuna ekleyin:

```azurecli-interactive
helm repo add svc-cat https://svc-catalog-charts.storage.googleapis.com
```

Son olarak, Hizmet Kataloğunu Helm grafiğiyle yükleyin. Kümenizde RBAC etkinse bu komutu çalıştırın.

```azurecli-interactive
helm install svc-cat/catalog --name catalog --namespace catalog --set apiserver.storage.etcd.persistence.enabled=true --set apiserver.healthcheck.enabled=false --set controllerManager.healthcheck.enabled=false --set apiserver.verbosity=2 --set controllerManager.verbosity=2
```

Kümenizde RBAC etkin değilse bu komutu çalıştırın.

```azurecli-interactive
helm install svc-cat/catalog --name catalog --namespace catalog --set rbacEnable=false --set apiserver.storage.etcd.persistence.enabled=true --set apiserver.healthcheck.enabled=false --set controllerManager.healthcheck.enabled=false --set apiserver.verbosity=2 --set controllerManager.verbosity=2
```

Helm grafik çalıştırıldıktan sonra aşağıdaki komutun çıktısında `servicecatalog` ifadesinin göründüğünü doğrulayın:

```azurecli-interactive
kubectl get apiservice
```

Örneğin, aşağıdakine benzer bir çıktı görmeniz gerekir (burada kesilmiş olarak gösterilmiştir):

```
NAME                                 AGE
v1.                                  10m
v1.authentication.k8s.io             10m
...
v1beta1.servicecatalog.k8s.io        34s
v1beta1.storage.k8s.io               10
```

## <a name="install-open-service-broker-for-azure"></a>Azure için Açık Hizmet Aracısı yükleme

Sonraki adımda, Azure tarafından yönetilen hizmetler için kataloğu içeren [Azure için Açık Hizmet Aracısı][open-service-broker-azure]'nı yükleyin. Kullanılabilir Azure hizmetleri arasında PostgreSQL için Azure Veritabanı, MySQL için Azure Veritabanı ve Azure SQL Veritabanı bulunur.

Azure Helm deposu için açık hizmet Aracısı ekleyerek başlayın:

```azurecli-interactive
helm repo add azure https://kubernetescharts.blob.core.windows.net/azure
```

Aşağıdaki Azure CLI komutuyla bir [Hizmet Sorumlusu][create-service-principal] oluşturun:

```azurecli-interactive
az ad sp create-for-rbac
```

Çıktının aşağıdakine benzer olması gerekir. Sonraki adımda kullanacağınız `appId`, `password` ve `tenant` değerlerini not edin.

```JSON
{
  "appId": "7248f250-0000-0000-0000-dbdeb8400d85",
  "displayName": "azure-cli-2017-10-15-02-20-15",
  "name": "http://azure-cli-2017-10-15-02-20-15",
  "password": "77851d2c-0000-0000-0000-cb3ebc97975a",
  "tenant": "72f988bf-0000-0000-0000-2d7cd011db47"
}
```

Aşağıdaki ortam değişkenlerini yukarıdaki değerlerle ayarlayın:

```azurecli-interactive
AZURE_CLIENT_ID=<appId>
AZURE_CLIENT_SECRET=<password>
AZURE_TENANT_ID=<tenant>
```

Şimdi Azure abonelik kimliğinizi alın:

```azurecli-interactive
az account show --query id --output tsv
```

Yine, aşağıdaki ortam değişkenlerini yukarıdaki değerle ayarlayın:

```azurecli-interactive
AZURE_SUBSCRIPTION_ID=[your Azure subscription ID from above]
```

Bu ortam değişkenlerini doldurduktan sonra, aşağıdaki komutu yürüterek Helm grafiği ile Azure için Açık Hizmet Aracısı’nı yükleyin:

```azurecli-interactive
helm install azure/open-service-broker-azure --name osba --namespace osba \
    --set azure.subscriptionId=$AZURE_SUBSCRIPTION_ID \
    --set azure.tenantId=$AZURE_TENANT_ID \
    --set azure.clientId=$AZURE_CLIENT_ID \
    --set azure.clientSecret=$AZURE_CLIENT_SECRET
```

OSBA dağıtımı tamamlandıktan sonra, hizmet aracılarını, hizmet sınıflarını, hizmet planlarını ve daha fazlasını sorgulamaya yönelik kullanımı kolay bir komut satırı arabirimi olan [Hizmet Kataloğu CLI][service-catalog-cli]'yi yükleyin.

Hizmet Kataloğu CLI ikili dosyasını yüklemek için aşağıdaki komutları çalıştırın:

```azurecli-interactive
curl -sLO https://servicecatalogcli.blob.core.windows.net/cli/latest/$(uname -s)/$(uname -m)/svcat
chmod +x ./svcat
```

Şimdi, yüklü olan hizmet aracılarını listeleyin:

```azurecli-interactive
./svcat get brokers
```

Aşağıdakine benzer bir çıktı görmeniz gerekir:

```
  NAME                               URL                                STATUS
+------+--------------------------------------------------------------+--------+
  osba   http://osba-open-service-broker-azure.osba.svc.cluster.local   Ready
```

Ardından, kullanılabilir hizmet sınıflarını listeleyin. Gösterilen hizmet sınıfları, Azure tarafından yönetilen kullanılabilir hizmetlerdir ve Azure için Açık Hizmet Aracısı ile sağlanabilir.

```azurecli-interactive
./svcat get classes
```

Son olarak, tüm kullanılabilir hizmet planlarını listeleyin. Hizmet planları, Azure tarafından yönetilen hizmetlere yönelik hizmet katmanlarıdır. Örneğin, MySQL için Azure Veritabanı planları, 50 Veritabanı İşlem Birimi (DTU) içeren Temel katmanı için `basic50` ile 800 DTU içeren Standart katmanı için `standard800` arasındadır.

```azurecli-interactive
./svcat get plans
```

## <a name="install-wordpress-from-helm-chart-using-azure-database-for-mysql"></a>MySQL için Azure Veritabanı kullanarak Helm grafiğinden WordPress yükleme

Bu adımda, Helm kullanarak WordPress için güncelleştirilmiş bir Helm grafiği yüklersiniz. Grafik, WordPress’in kullanabileceği bir dış MySQL için Azure Veritabanı sağlar. Bu işlem birkaç dakika sürebilir.

```azurecli-interactive
helm install azure/wordpress --name wordpress --namespace wordpress --set resources.requests.cpu=0 --set replicaCount=1
```

Yüklemenin doğru kaynakları sağladığını onaylamak için, yüklü hizmet örneklerini ve bağlamaları listeleyin:

```azurecli-interactive
./svcat get instances -n wordpress
./svcat get bindings -n wordpress
```

Yüklü gizli dizileri listeleme:

```azurecli-interactive
kubectl get secrets -n wordpress -o yaml
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makaleyi izleyerek, bir Azure Kubernetes Hizmeti (AKS) kümesine Hizmet Kataloğu dağıttınız. Bu örnekte MySQL için Azure Veritabanı olan Azure tarafından yönetilen hizmetleri kullanan bir WordPress yüklemesi dağıtmak üzere Azure için Açık Hizmet Aracısı kullandınız.

Diğer güncelleştirilmiş OSBA tabanlı Helm grafiklerine erişmek için [Azure/helm-charts][helm-charts] deposuna bakın. OSBA ile çalışan kendi grafiklerinizi oluşturmak istiyorsanız bkz. [Yeni Grafik Oluşturma][helm-create-new-chart].

<!-- LINKS - external -->
[helm-charts]: https://github.com/Azure/helm-charts
[helm-cli-install]: https://docs.helm.sh/helm/#helm-install
[helm-create-new-chart]: https://github.com/Azure/helm-charts#creating-a-new-chart
[kubernetes-service-catalog]: https://github.com/kubernetes-incubator/service-catalog
[open-service-broker-azure]: https://github.com/Azure/open-service-broker-azure
[service-catalog-cli]: https://github.com/Azure/service-catalog-cli

<!-- LINKS - internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
[azure-cloud-shell]: ../cloud-shell/overview.md
[create-aks-cluster]: ./kubernetes-walkthrough.md
[create-service-principal]: ./kubernetes-service-principal.md
