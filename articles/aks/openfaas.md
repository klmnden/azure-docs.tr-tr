---
title: Azure Kubernetes Service'i (AKS) ile OpenFaaS kullanma
description: Dağıtma ve Azure Kubernetes Service (AKS) ile OpenFaaS kullanma
services: container-service
author: justindavies
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 03/05/2018
ms.author: juda
ms.custom: mvc
ms.openlocfilehash: 5ed6e0b21b00ede3f78a102fd004e5706ae3cea5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60464905"
---
# <a name="using-openfaas-on-aks"></a>AKS OpenFaaS kullanma

[OpenFaaS] [ open-faas] kapsayıcıları kullanarak sunucusuz işlevler oluşturmaya yönelik bir çerçevedir. Açık kaynaklı proje, bu büyük ölçekli benimseme topluluk içinde kazanmıştır. Bu belge yükleme ve bir Azure Kubernetes Service (AKS) kümesine OpenFaas kullanma ayrıntılı olarak açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları tamamlamak için aşağıdakilere ihtiyacınız vardır.

* Kubernetes temel düzeyde bilinmesini.
* Azure Kubernetes Service (AKS) kümesini ve geliştirme sisteminizde yapılandırılmış AKS kimlik bilgileri.
* Azure CLI geliştirme sisteminizde yüklü.
* Git komut satırı araçları, sisteminizde yüklü.

## <a name="add-the-openfaas-helm-chart-repo"></a>OpenFaaS helm grafiği depo Ekle

En son değişikliklerle güncel tutmak için kendi helm grafikleri OpenFaaS tutar.

```azurecli-interactive
helm repo add openfaas https://openfaas.github.io/faas-netes/
helm repo update
```

## <a name="deploy-openfaas"></a>OpenFaaS dağıtma

İyi uygulama olarak, OpenFaaS ve OpenFaaS işlevleri kendi Kubernetes ad alanında depolanmalıdır.

İşlevler ve OpenFaaS sistem için bir ad alanı oluşturun:

```azurecli-interactive
kubectl apply -f https://raw.githubusercontent.com/openfaas/faas-netes/master/namespaces.yml
```

REST API ve OpenFaaS UI portalı için bir parola oluştur:

```azurecli-interactive
# generate a random password
PASSWORD=$(head -c 12 /dev/urandom | shasum| cut -d' ' -f1)

kubectl -n openfaas create secret generic basic-auth \
--from-literal=basic-auth-user=admin \
--from-literal=basic-auth-password="$PASSWORD"
```

Sahip gizli dizi, değer elde edebileceği `echo $PASSWORD`.

Burada oluşturduğunuz parolayı helm grafiği ile OpenFaaS Bulutu LoadBalancer üzerinden İnternet'e ifşa eden ağ geçidi, temel kimlik doğrulamasını etkinleştirmek için kullanılır.

Kopyalanan depoya bir Helm grafiği OpenFaaS için dahil edilir. AKS kümenizi OpenFaaS dağıtmak için bu tabloyu kullanın.

```azurecli-interactive
helm upgrade openfaas --install openfaas/openfaas \
    --namespace openfaas  \
    --set basic_auth=true \
    --set functionNamespace=openfaas-fn \
    --set serviceType=LoadBalancer
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

Genel bir IP adresi OpenFaaS ağ geçidi erişmek için oluşturulur. Bu IP adresini almak için kullanın [kubectl alma hizmeti] [ kubectl-get] komutu. Bu hizmete atanacak IP adresi için bir dakika sürebilir.

```console
kubectl get service -l component=gateway --namespace openfaas
```

Çıktı.

```console
NAME               TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)          AGE
gateway            ClusterIP      10.0.156.194   <none>         8080/TCP         7m
gateway-external   LoadBalancer   10.0.28.18     52.186.64.52   8080:30800/TCP   7m
```

OpenFaaS sistem test etmek için Gözat 8080 numaralı bağlantı noktasındaki dış IP adresine `http://52.186.64.52:8080` Bu örnekte. Oturum açmak için istenir. Parolanızı getirilecek girin `echo $PASSWORD`.

![OpenFaaS UI](media/container-service-serverless/openfaas.png)

Son olarak, OpenFaaS CLI'yı yükleyin. Bu örnekte kullanılan brew bkz [OpenFaaS CLI belgeleri] [ open-faas-cli] daha fazla seçenek için.

```console
brew install faas-cli
```

Ayarlama `$OPENFAAS_URL` yukarıda bulunan genel IP için.

Azure CLI ile oturum açın:

```azurecli-interactive
export OPENFAAS_URL=http://52.186.64.52:8080
echo -n $PASSWORD | ./faas-cli login -g $OPENFAAS_URL -u admin --password-stdin
```

## <a name="create-first-function"></a>İlk işlevinizi oluşturma

OpenFaaS çalıştığından, OpenFaas portalını kullanarak bir işlev oluşturun.

Tıklayarak **dağıtma yeni işlev** araması **Figlet**. Figlet işlevi seçin ve tıklayın **Dağıt**.

![Figlet](media/container-service-serverless/figlet.png)

İşlevin çalıştırılabilmesi için Curl kullanın. Aşağıdaki örnekte IP adresi, OpenFaas ağ geçidiniz ile değiştirin.

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

## <a name="create-second-function"></a>İkinci bir işlev oluşturma

Şimdi ikinci bir işlev oluşturun. Bu örnekte, OpenFaaS CLI'yı kullanarak dağıtılan ve bir özel kapsayıcı görüntüsüne ve Cosmos DB'den verileri alınırken içerir. Çeşitli öğeler işlev oluşturmadan önce yapılandırılması gerekir.

İlk olarak Cosmos DB için yeni bir kaynak grubu oluşturun.

```azurecli-interactive
az group create --name serverless-backing --location eastus
```

Bir tür CosmosDB örneğini dağıtma `MongoDB`. Örneğin, benzersiz bir ad gerekiyor., güncelleştirme `openfaas-cosmos` ortamınız için benzersiz bir şey.

```azurecli-interactive
az cosmosdb create --resource-group serverless-backing --name openfaas-cosmos --kind MongoDB
```

Cosmos veritabanı bağlantı dizesini alın ve bunu bir değişkende depolayın.

Değerini güncelleştirin `--resource-group` kaynak grubunuzun adı bağımsız değişkeni ve `--name` Cosmos DB adı bağımsız değişkeni.

```azurecli-interactive
COSMOS=$(az cosmosdb list-connection-strings \
  --resource-group serverless-backing \
  --name openfaas-cosmos \
  --query connectionStrings[0].connectionString \
  --output tsv)
```

Şimdi Cosmos DB test verileri ile doldurun. Adlı bir dosya oluşturun `plans.json` aşağıdaki json kopyalayın.

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

Kullanım *veya* CosmosDB örnek verileri yüklemek için aracı.

Gerekli olursa, MongoDB Araçları'nı yükleme. Aşağıdaki örnek brew kullanarak bu araçları yükler, bkz: [MongoDB belgelerine] [ install-mongo] diğer seçenekler için.

```azurecli-interactive
brew install mongodb
```

Verileri veritabanı'na yükler.

```azurecli-interactive
mongoimport --uri=$COSMOS -c plans < plans.json
```

Çıktı:

```console
2018-02-19T14:42:14.313+0000    connected to: localhost
2018-02-19T14:42:14.918+0000    imported 1 document
```

İşlevi profili oluşturmak için aşağıdaki komutu çalıştırın. Değerini güncelleştirin `-g` OpenFaaS ağ geçidi adresiyle bağımsız değişken.

```azurecli-interctive
faas-cli deploy -g http://52.186.64.52:8080 --image=shanepeckham/openfaascosmos --name=cosmos-query --env=NODE_ENV=$COSMOS
```

Dağıtıldıktan sonra yeni oluşturulan OpenFaaS uç noktanız için işlev görmeniz gerekir.

```console
Deployed. 202 Accepted.
URL: http://52.186.64.52:8080/function/cosmos-query
```

Curl kullanarak işlevi test etme. IP adresi ile OpenFaaS ağ geçidi adresini güncelleştirin.

```console
curl -s http://52.186.64.52:8080/function/cosmos-query
```

Çıktı:

```json
[{"ID":"","Name":"two_person","FriendlyName":"","PortionSize":"","MealsPerWeek":"","Price":72,"Description":"Our basic plan, delivering 3 meals per week, which will feed 1-2 people."}]
```

Ayrıca, OpenFaaS UI içinde işlevi test edebilirsiniz.

![alternatif metin](media/container-service-serverless/OpenFaaSUI.png)

## <a name="next-steps"></a>Sonraki Adımlar

Gizli ölçümleri görüntüleme ve otomatik ölçeklendirmeyi kullanma kümesi kapsayan kendi GitHub bot oluşturma gibi konular uygulamalı laboratuvarlar ile OpenFaaS Atölyesi ile öğrenmeye devam edebilirsiniz.

<!-- LINKS - external -->
[install-mongo]: https://docs.mongodb.com/manual/installation/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[open-faas]: https://www.openfaas.com/
[open-faas-cli]: https://github.com/openfaas/faas-cli
[openfaas-workshop]: https://github.com/openfaas/workshop
