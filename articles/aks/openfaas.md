---
title: "Azure kapsayıcı hizmeti (AKS) OpenFaaS kullanın"
description: "Dağıtma ve OpenFaaS Azure kapsayıcı hizmeti (AKS) ile kullanma"
services: container-service
author: justindavies
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 03/05/2018
ms.author: juda
ms.custom: mvc
ms.openlocfilehash: 06706450d8af6f571f002789815290f75da9623d
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="using-openfaas-on-aks"></a>Üzerinde AKS OpenFaaS kullanma

[OpenFaaS] [ open-faas] kapsayıcıları üstünde sunucusuz işlevleri oluşturmaya yönelik bir çerçevedir. Açık kaynaklı proje, büyük ölçekli benimseme topluluk içinde kazanmıştır. Bu belge yükleme ve bir Azure kapsayıcı hizmeti (AKS) kümede OpenFaas kullanılarak ayrıntıları.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede içindeki adımları tamamlamak için aşağıdakiler gerekir.

* Kubernetes ilgili temel bilgilere.
* Bir Azure kapsayıcı hizmeti (AKS) küme ve geliştirme sisteminizde yapılandırılmış AKS kimlik bilgileri.
* Azure CLI geliştirme sisteminizde yüklü.
* Git komut satırı araçlarını, sisteminizde yüklü.

## <a name="get-openfaas"></a>OpenFaaS Al

Geliştirme sisteminizde OpenFaaS proje depoyu kopyalayın.

```azurecli-interactive
git clone https://github.com/openfaas/faas-netes
```

Kopyalanan deposu dizinine değiştirin.

```azurecli-interactive
cd faas-netes 
```

## <a name="deploy-openfaas"></a>OpenFaaS dağıtma

İyi bir uygulama, OpenFaaS ve OpenFaaS olarak kendi Kubernetes ad alanındaki işlevler depolanmalıdır.

OpenFaaS sistem için bir ad alanı oluşturun.

```azurecli-interactive
kubectl create namespace openfaas
```

OpenFaaS işlevler için ikinci bir ad oluşturun.

```azurecli-interactive 
kubectl create namespace openfaas-fn
```

OpenFaaS Helm grafiğinde kopyalanan depoya dahil edilir. Bu grafik OpenFaaS AKS kümesine dağıtmak için kullanın.

```azurecli-interactive
helm install --namespace openfaas -n openfaas \
  --set functionNamespace=openfaas-fn, \
  --set serviceType=LoadBalancer, \
  --set rbac=false chart/openfaas/ 
```

Çıktı:

```
NAME:   openfaas
LAST DEPLOYED: Wed Feb 28 08:26:11 2018
NAMESPACE: openfaas
STATUS: DEPLOYED

RESOURCES:
==> v1/ConfigMap
NAME                 DATA  AGE
prometheus-config    2     20s
alertmanager-config  1     20s

{snip}

NOTES:
To verify that openfaas has started, run:

  kubectl --namespace=openfaas get deployments -l "release=openfaas, app=openfaas"
```

Bir ortak IP adresi OpenFaaS ağ geçidi erişmek için oluşturulur. Bu IP adresi almak için kullanın [kubectl alma hizmeti] [ kubectl-get] komutu. Hizmete atanan IP adresi için bir dakika sürebilir.

```console
kubectl get service -l component=gateway --namespace openfaas
```

Çıktı. 

```console
NAME               TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)          AGE
gateway            ClusterIP      10.0.156.194   <none>         8080/TCP         7m
gateway-external   LoadBalancer   10.0.28.18     52.186.64.52   8080:30800/TCP   7m
```

OpenFaaS sistem sınamak için Gözat bağlantı noktası 8080 dış IP adresine `http://52.186.64.52:8080` Bu örnekte.

![OpenFaaS UI](media/container-service-serverless/openfaas.png)

Son olarak, OpenFaaS CLI yükleyin. Bu exmaple brew kullanılan bkz [OpenFaaS CLI belgelerine] [ open-faas-cli] daha fazla seçenek için.

```console
brew install faas-cli
```

## <a name="create-first-function"></a>İlk işlevi oluşturma

OpenFaaS çalıştığından, OpenFaas Portalı'nı kullanarak bir işlev oluşturun.

Tıklayın **dağıtmak yeni işlev** arayın ve **Figlet**. Figlet işlevi seçin ve tıklayın **dağıtma**.

![Figlet](media/container-service-serverless/figlet.png)

Curl işlevi çağırmak için kullanın. Aşağıdaki örnekte IP adresi, OpenFaas ağ geçidiniz ile değiştirin.

```azurecli-interactive
curl -X POST http://52.186.64.52:8080/function/figlet -d "Hello Azure"
```

Çıktı:

```console
 _   _      _ _            _                        
| | | | ___| | | ___      / \    _____   _ _ __ ___ 
| |_| |/ _ \ | |/ _ \    / _ \  |_  / | | | '__/ _ \
|  _  |  __/ | | (_) |  / ___ \  / /| |_| | | |  __/
|_| |_|\___|_|_|\___/  /_/   \_\/___|\__,_|_|  \___|

```

## <a name="create-second-function"></a>İkinci bir işlev oluşturun

Şimdi ikinci bir işlev oluşturun. Bu örnek OpenFaaS CLI kullanarak dağıtılır ve bir özel kapsayıcı görüntüsü ve bir Cosmos Veritabanından veri alma içerir. Birden çok öğe işlevi oluşturmadan önce yapılandırılması gerekir. 

İlk olarak, yeni bir kaynak grubu için Cosmos DB oluşturun.

```azurecli-interactive
az group create --name serverless-backing --location eastus
```

Tür CosmosDB örneğini dağıtmak `MongoDB`. Örnek benzersiz bir adı olmalıdır, güncelleştirme `openfaas-cosmos` ortamınız için benzersiz bir şey. 

```azurecli-interactive
az cosmosdb create --resource-group serverless-backing --name openfaas-cosmos --kind MongoDB
```

Cosmos veritabanı bağlantı dizesi alma ve bir değişkende saklayın. 

Değeri güncelleştirme `--resource-group` kaynak grubunuzun adını bağımsız değişken ve `--name` Cosmos DB adı bağımsız değişkeni.

```azurecli-interactive
COSMOS=$(az cosmosdb list-connection-strings \
  --resource-group serverless-backing \
  --name openfaas-cosmos \
  --query connectionStrings[0].connectionString \
  --output tsv)
```

Şimdi Cosmos DB test verileri ile doldurun. Adlı bir dosya oluşturun `plans.json` ve aşağıdaki json kopyalayın.

```json
{
    "name" : "two_person",
    "friendlyName" : "Two Person Plan",
    "portionSize" : "1-2 Person",
    "mealsPerWeek" : "3 Unique meals per week",
    "price" : 72,
    "description" : "Our basic plan, delivering 3 meals per week, which will feed 1-2 people.",
    "__v" : 0
}
```

Kullanım *mongoimport* verilerle CosmosDB örneğini yükleme aracı. 

Gerekiyorsa, MongoDB araçlarını yükleyin. Aşağıdaki örnek brew kullanarak bu araçların yükler, bkz: [MongoDB belgelerine] [ install-mongo] diğer seçenekler için.

```azurecli-interactive
brew install mongodb
```

Verileri veritabanına yükleyin.

```azurecli-interactive
mongoimport --uri=$COSMOS -c plans < plans.json
```

Çıktı:

```console
2018-02-19T14:42:14.313+0000    connected to: localhost
2018-02-19T14:42:14.918+0000    imported 1 document
```

İşlev oluşturmak için aşağıdaki komutu çalıştırın. Değerini güncelleştirmek `-g` bağımsız değişkeniyle OpenFaaS ağ geçidi adresi.

```azurecli-interctive
faas-cli deploy -g http://52.186.64.52:8080 --image=shanepeckham/openfaascosmos --name=cosmos-query --env=NODE_ENV=$COSMOS
```

Uygulama dağıtıldıktan sonra işlevi için yeni oluşturulan bir OpenFaaS uç noktanızı görmeniz gerekir.

```console
Deployed. 202 Accepted.
URL: http://52.186.64.52:8080/function/cosmos-query
```

Curl kullanarak işlevi sınayın. IP adresi OpenFaaS ağ geçidi adresiyle güncelleştirin.

```console
curl -s http://52.186.64.52:8080/function/cosmos-query
```

Çıktı:

```json
[{"ID":"","Name":"two_person","FriendlyName":"","PortionSize":"","MealsPerWeek":"","Price":72,"Description":"Our basic plan, delivering 3 meals per week, which will feed 1-2 people."}]
```

Ayrıca OpenFaaS UI işlevindeki test edebilirsiniz.

![alternatif metin](media/container-service-serverless/OpenFaaSUI.png)

# <a name="next-steps"></a>Sonraki Adımlar

OpenFaas varsayılan dağıtımını OpenFaaS ağ geçidi ve işlevleri için kilitli gerekir. [Alex Ellis Blog Gönderisi](https://blog.alexellis.io/lock-down-openfaas/) güvenli yapılandırma seçenekleri hakkında daha fazla ayrıntı yok. 

<!-- LINKS - external -->
[install-mongo]: https://docs.mongodb.com/manual/installation/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[open-faas]: https://www.openfaas.com/
[open-faas-cli]: https://github.com/openfaas/faas-cli