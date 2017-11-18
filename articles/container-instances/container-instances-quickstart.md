---
title: "Hızlı Başlangıç - ilk Azure kapsayıcı örnekleri kapsayıcı oluşturma"
description: "Azure Container Instances’ı dağıtma ve kullanmaya başlama"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: mmacy
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/17/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: a6d5690edd9020e777f3d71c41a53856d0a400db
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2017
---
# <a name="create-your-first-container-in-azure-container-instances"></a>Azure Container Instances’da ilk kapsayıcınızı oluşturma
Azure kapsayıcı örnekleri oluşturmak ve sanal makineler sağlamak veya bir üst düzey hizmet benimsemeyi zorunda kalmadan, azure'da Docker kapsayıcıları yönetmek kolay hale getirir. Bu hızlı başlangıç bir kapsayıcı oluşturmak ve genel bir IP adresi ile Internet'e kullanıma. Bu işlem tek bir komutla tamamlanır. Yalnızca birkaç saniye içinde bu tarayıcınızda görürsünüz:

![Azure Container Instances kullanılarak dağıtılmış uygulama tarayıcıda görüntüleniyor][aci-app-browser]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bu hızlı başlangıç tamamlamak için Azure bulut Kabuğu'nu veya Azure CLI yerel yüklemesi'ni kullanabilirsiniz. Azure CLI Sürüm 2.0.20 çalıştırıyorsanız bu hızlı başlangıç yükleyip CLI yerel olarak kullanmak seçerseniz, gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure Container Instances örnekleri Azure kaynaklarıdır ve Azure kaynaklarının dağıtıldığı ve yönetildiği mantıksal bir koleksiyon olan Azure kaynak gruplarına yerleştirilmelidir.

Bir kaynak grubu ile oluşturmak [az grubu oluşturma] [ az-group-create] komutu.

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Bir ad, bir Docker görüntüsü ve bir Azure kaynak grubu sağlayarak bir kapsayıcı oluşturabilirsiniz [az kapsayıcı oluşturmak] [ az-container-create] komutu. İsteğe bağlı olarak, kapsayıcıyı genel IP adresi ile İnternet üzerinden kullanıma sunabilirsiniz. Bu hızlı başlangıç yazılmış küçük bir web uygulamasını barındıran bir kapsayıcıyı dağıtmak [Node.js](http://nodejs.org).

```azurecli-interactive
az container create --name mycontainer --image microsoft/aci-helloworld --resource-group myResourceGroup --ip-address public --ports 80
```

Birkaç saniye içinde isteğinize yanıt almanız gerekir. Başlangıçta, içinde kapsayıcıdır **oluşturma** durumu, ancak bu işlem birkaç saniye içinde başlamalıdır. Durum kullanarak denetleyebilirsiniz [az kapsayıcı Göster] [ az-container-show] komutu:

```azurecli-interactive
az container show --name mycontainer --resource-group myResourceGroup
```

Çıktının alt kısmında kapsayıcının sağlama durumunu ve IP adresini görürsünüz:

```json
...
"ipAddress": {
      "ip": "13.88.176.27",
      "ports": [
        {
          "port": 80,
          "protocol": "TCP"
        }
      ]
    },
    "osType": "Linux",
    "provisioningState": "Succeeded"
...
```

Kapsayıcı taşır sonra **başarılı** durumunda, ulaşana, sağlanan IP adresi kullanarak tarayıcınızda.

![Azure Container Instances kullanılarak dağıtılmış uygulama tarayıcıda görüntüleniyor][aci-app-browser]

## <a name="pull-the-container-logs"></a>Kapsayıcı günlüklerini çekme

Günlükleri kullanarak oluşturduğunuz kapsayıcısı için çekme [az kapsayıcı günlükleri] [ az-container-logs] komutu:

```azurecli-interactive
az container logs --name mycontainer --resource-group myResourceGroup
```

Çıktı:

```bash
listening on port 80
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://104.210.39.122/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="delete-the-container"></a>Kapsayıcıyı silme

Kapsayıcıyla bittiğinde kullanarak kaldırabilirsiniz [az kapsayıcı delete] [ az-container-delete] komutu:

```azurecli-interactive
az container delete --name mycontainer --resource-group myResourceGroup
```

Kapsayıcı silinip silinmediğini doğrulamak için yürütme [az kapsayıcı listesi](/cli/azure/container#az_container_list) komutu:

```azurecli-interactive
az container list --resource-group myResourceGroup -o table
```

**Mycontainer** kapsayıcı değil komutunun çıktısını görünmelidir. Kaynak grubunu başka bir kapsayıcıları varsa, hiçbir çıktısı görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç içinde kullanılan kapsayıcı kodunu tümünün kullanılabilir [github'da][app-github-repo], kendi Dockerfile yanı sıra. Kapsayıcıyı kendiniz oluşturup Azure Container Registry’yi kullanarak Azure Container Instances’a dağıtmayı denemek istiyorsanız Azure Container Instances öğreticisine geçin.

> [!div class="nextstepaction"]
> [Azure Container Instances öğreticileri](./container-instances-tutorial-prepare-app.md)

Azure üzerinde bir orchestration sistemde çalışan kapsayıcılar seçeneklerini denemek için bkz: [Service Fabric] [ service-fabric] veya [Azure kapsayıcı hizmeti (AKS)] [ container-service] quickstarts.

<!-- LINKS -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git
[az-group-create]: /cli/azure/group?view=azure-cli-latest#az_group_create
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az_container_create
[az-container-delete]: /cli/azure/container?view=azure-cli-latest#az_container_delete
[az-container-list]: /cli/azure/container?view=azure-cli-latest#az_container_list
[az-container-logs]: /cli/azure/container?view=azure-cli-latest#az_container_logs
[az-container-show]: /cli/azure/container?view=azure-cli-latest#az_container_show
[service-fabric]: ../service-fabric/service-fabric-quickstart-containers.md
[container-service]: ../aks/kubernetes-walkthrough.md


<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png
