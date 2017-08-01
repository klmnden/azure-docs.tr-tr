---
title: "İlk Azure Container Instances kapsayıcınızı oluşturma | Azure Docs"
description: "Azure Container Instances’ı dağıtma ve kullanmaya başlama"
services: container-service
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: 
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: seanmck
ms.custom: 
ms.translationtype: HT
ms.sourcegitcommit: 3b15d6645b988f69f1f05b27aff6f726f34786fc
ms.openlocfilehash: 933299ce5a5d6f5b2262d40ae768019ccaf8796a
ms.contentlocale: tr-tr
ms.lasthandoff: 07/26/2017

---

# <a name="create-your-first-container-in-azure-container-instances"></a>Azure Container Instances’da ilk kapsayıcınızı oluşturma

Azure Container Instances, Azure’da kapsayıcı oluşturmayı ve yönetmeyi kolaylaştırır. Bu hızlı başlangıç içeriğinde, bir kapsayıcı oluşturacak ve bu kapsayıcıyı genel IP adresi ile İnternet üzerinden kullanıma sunacaksınız. Bu işlem tek bir komutla tamamlanır. Yalnızca birkaç saniye içinde tarayıcınızda şunu görürsünüz:

![Azure Container Instances kullanılarak dağıtılmış uygulama tarayıcıda görüntüleniyor][aci-app-browser]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure Container Instances CLI komutları şu anda yalnızca Azure Cloud Shell'de kullanılabilir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure Container Instances örnekleri Azure kaynaklarıdır ve Azure kaynaklarının dağıtıldığı ve yönetildiği mantıksal bir koleksiyon olan Azure kaynak gruplarına yerleştirilmelidir.

[az group create](/cli/azure/group#create) komutuyla bir kaynak grubu oluşturun. 

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Bir ad, Docker görüntüsü ve bir Azure kaynak grubu sağlayarak kapsayıcı oluşturabilirsiniz. İsteğe bağlı olarak, kapsayıcıyı genel IP adresi ile İnternet üzerinden kullanıma sunabilirsiniz. Bu durumda, [Node.js](http://nodejs.org) ile yazılan, çok basit bir web uygulamasını barındıran bir kapsayıcı kullanacağız.

Azure Container Instances CLI komutları şu anda yalnızca Azure Cloud Shell'de kullanılabilir.

```azurecli-interactive
az container create --name mycontainer --image microsoft/aci-helloworld --resource-group myResourceGroup --ip-address public 
```

Birkaç saniye içinde isteğinize yanıt almanız gerekir. Kapsayıcı başlangıçta **oluşturma** durumunda olacaktır ancak birkaç saniye içinde başlar. Durumu `show` komutunu kullanarak denetleyebilirsiniz:

```azurecli-interactive
az container show --name mycontainer --resource-group myResourceGroup
```

Çıktının alt kısmında kapsayıcının sağlama durumunu ve IP adresini görürsünüz:

```json
...
"ipAddress": {
      "ip": "13.88.8.148",
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

Kapsayıcı **başarılı** durumuna geçtiğinde, sağlanan IP adresini kullanarak tarayıcınızdan kapsayıcıya ulaşabilirsiniz. 

![Azure Container Instances kullanılarak dağıtılmış uygulama tarayıcıda görüntüleniyor][aci-app-browser]

## <a name="pull-the-container-logs"></a>Kapsayıcı günlüklerini çekme

Oluşturduğunuz kapsayıcı için günlükleri `logs` komutunu kullanarak çekebilirsiniz:

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

Kapsayıcıyla işiniz bittiğinde `delete` komutunu kullanarak kapsayıcıyı kaldırabilirsiniz:

```azurecli-interactive
az container delete --name mycontainer --resource-group myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç bölümünde kullanılan kapsayıcısı için tüm kodlar [GitHub'da][app-github-repo], ilgili Dockerfile ile birlikte bulunur. Kapsayıcıyı kendiniz oluşturup Azure Container Registry’yi kullanarak Azure Container Instances’a dağıtmayı denemek istiyorsanız Azure Container Instances öğreticisine geçin.

> [!div class="nextstepaction"]
> [Azure Container Instances öğreticileri](./container-instances-tutorial-prepare-app.md)


<!-- LINKS -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png
