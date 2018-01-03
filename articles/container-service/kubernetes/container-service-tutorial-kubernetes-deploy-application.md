---
title: "Azure kapsayıcı hizmeti Öğreticisi - uygulama dağıtma"
description: "Azure kapsayıcı hizmeti Öğreticisi - uygulama dağıtma"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 09/14/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 397bb22918d5b181692a42d0f4c2d87be086c534
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2017
---
# <a name="run-applications-in-kubernetes"></a>Kubernetes çalışma uygulamaları

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

Bu öğreticide yedi dördünü kısım, örnek bir uygulama Kubernetes kümesine dağıtılır. Tamamlanan adımları içerir:

> [!div class="checklist"]
> * Güncelleştirme Kubernetes bildirim dosyaları
> * Kubernetes'te uygulama çalıştırma
> * Uygulamayı test etme

Sonraki öğreticilerde, bu uygulama, güncelleştirilmiş, ölçeklenir ve Kubernetes küme izlemek için Operations Management Suite yapılandırılır.

Bu öğretici Kubernetes kavramlar, Kubernetes bakın hakkında ayrıntılı bilgi için temel bir anlayış varsayar [Kubernetes belgelerine](https://kubernetes.io/docs/home/).

## <a name="before-you-begin"></a>Başlamadan önce

Önceki öğreticileri, bir uygulama bir kapsayıcı görüntüsüne paketlenmiş ve bu görüntüyü Azure kapsayıcı kayıt defterine karşıya Kubernetes küme oluşturuldu. 

Bu öğreticiyi tamamlamak için önceden oluşturulmuş gerekir `azure-vote-all-in-one-redis.yml` Kubernetes bildirim dosyası. Bu dosya uygulama kaynak koduna bir önceki öğreticide yüklendi. Depoyu kopyalanmış ve kopyalanan deposu dizinleri değiştirilmediğini doğrulayın.

Bu adımları yapmadıysanız ve izlemek istediğiniz, geri dönüp [Öğreticisi 1 – Oluştur kapsayıcı görüntüleri](./container-service-tutorial-kubernetes-prepare-app.md). 

## <a name="update-manifest-file"></a>Güncelleştirme bildirim dosyası

Bu öğreticide, Azure kapsayıcı kayıt defteri (ACR) bir kapsayıcı görüntüsü depolamak için kullanılmış. Uygulamayı çalıştırmadan önce ACR oturum açma sunucusu adı Kubernetes bildirim dosyasında güncelleştirilmesi gerekiyor.

ACR oturum açma sunucu adıyla alma [az acr listesi](/cli/azure/acr#list) komutu.

```azurecli-interactive
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Bildirim dosyası bir oturum açma sunucusu adı ile önceden oluşturulmuş `microsoft`. Dosyayı herhangi bir metin düzenleyicisinde açın. Bu örnekte, dosya içeren açıldıktan `vi`.

```bash
vi azure-vote-all-in-one-redis.yml
```

Değiştir `microsoft` ACR oturum açma sunucu adına sahip. Bu değer satırına bulundu **47** bildirim dosyasının.

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:redis-v1
```

Dosyasını kaydedin ve kapatın.

## <a name="deploy-application"></a>Uygulama dağıtma

Uygulamayı çalıştırmak için [kubectl create](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#create) komutunu kullanın. Bu komut, bildirim dosyası ayrıştırır ve tanımlanmış Kubernetes nesneleri oluşturma.

```azurecli-interactive
kubectl create -f azure-vote-all-in-one-redis.yml
```

Çıktı:

```bash
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-application"></a>Uygulamayı test etme

A [Kubernetes hizmet](https://kubernetes.io/docs/concepts/services-networking/service/) hangi uygulamanın Internet'e gösterir oluşturulur. Bu işlem birkaç dakika sürebilir. 

İlerleme durumunu izlemek için [kubectl get service](https://review.docs.microsoft.com/en-us/azure/container-service/container-service-kubernetes-walkthrough?branch=pr-en-us-17681) komutunu `--watch` bağımsız değişkeniyle birlikte kullanın.

```azurecli-interactive
kubectl get service azure-vote-front --watch
```

Başlangıçta, **dış IP** için `azure-vote-front` hizmeti görünür olarak `pending`. DIŞ IP adresi değiştiğinden sonra `pending` için bir `IP address`, kullanın `CTRL-C` kubectl izleme işlemi durdurmak için.

```bash
NAME               CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   10.0.42.158   <pending>     80:31873/TCP   1m
azure-vote-front   10.0.42.158   52.179.23.131 80:31873/TCP   2m
```

Uygulama görmek için dış IP adresine göz atın.

![Azure’da Kubernetes kümesinin görüntüsü](media/container-service-kubernetes-tutorials/azure-vote.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure oy uygulama için bir Azure kapsayıcı hizmeti Kubernetes kümesi dağıtıldı. Tamamlanan görevler aşağıdakileri içerir:  

> [!div class="checklist"]
> * Kubernetes bildirim dosyaları indirme
> * İçinde Kubernetes uygulamayı çalıştırın
> * Uygulamayı test

Kubernetes uygulama ve Kubernetes altyapının ölçeklendirme hakkında bilgi edinmek için sonraki öğretici ilerleyin. 

> [!div class="nextstepaction"]
> [Ölçek Kubernetes uygulama ve altyapı](./container-service-tutorial-kubernetes-scale.md)