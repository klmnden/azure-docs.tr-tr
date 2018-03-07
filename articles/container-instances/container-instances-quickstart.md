---
title: "Hızlı Başlangıç - İlk Azure Container Instances kapsayıcınızı oluşturma"
description: "Azure Container Instances’ı dağıtma ve kullanmaya başlama"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: quickstart
ms.date: 02/22/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: f8fe53f834e4fcf7f16174222cb51d89e40305ec
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="create-your-first-container-in-azure-container-instances"></a>Azure Container Instances’da ilk kapsayıcınızı oluşturma
Azure Container Instances, sanal makine sağlamak veya daha yüksek düzey bir hizmet benimsemek zorunda kalmadan Azure’da Docker kapsayıcıları oluşturmayı ve yönetmeyi kolaylaştırır. Bu hızlı başlangıç içeriğinde, bir kapsayıcı oluşturacak ve bu kapsayıcıyı tam etki alanı adı (FQDN) ile İnternet üzerinden kullanıma sunacaksınız. Bu işlem tek bir komutla tamamlanır. Yalnızca birkaç saniye içinde tarayıcınızda şunu görürsünüz:

![Azure Container Instances kullanılarak dağıtılmış uygulama tarayıcıda görüntüleniyor][aci-app-browser]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap][azure-account] oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bu hızlı başlangıcı tamamlamak için Azure Cloud Shell veya yerel bir Azure CLI yüklemesi kullanabilirsiniz. CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.27 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme][azure-cli-install].

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure Container Instances örnekleri Azure kaynaklarıdır ve Azure kaynaklarının dağıtıldığı ve yönetildiği mantıksal bir koleksiyon olan Azure kaynak gruplarına yerleştirilmelidir.

[az group create][az-group-create] komutuyla bir kaynak grubu oluşturun.

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

[az container create][az-container-create] komutuna bir ad, Docker görüntüsü ve bir Azure kaynak grubu sağlayarak kapsayıcı oluşturabilirsiniz. İsteğe bağlı olarak, bir DNS ad etiketi belirterek kapsayıcıyı internet üzerinden kullanıma sunabilirsiniz. Bu hızlı başlangıçta, [Node.js][node-js] içinde yazılan küçük bir web uygulamasını barındıran bir kapsayıcı dağıtırsınız.

Bir kapsayıcı örneği başlatmak için aşağıdaki komutu yürütün. `--dns-name-label` değeri, örneği oluşturduğunuz Azure bölgesi içinde benzersiz olmalıdır, bu yüzden benzersizliği sağlamak için bu değeri değiştirmeniz gerekebilir.

```azurecli-interactive
az container create --resource-group myResourceGroup --name mycontainer --image microsoft/aci-helloworld --dns-name-label aci-demo --ports 80
```

Birkaç saniye içinde isteğinize yanıt almanız gerekir. Kapsayıcı başlangıçta **Oluşturuluyor** durumunda olacaktır ancak birkaç saniye içinde başlar. Durumu [az container show][az-container-show] komutunu kullanarak denetleyebilirsiniz:

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" --out table
```

Komutu çalıştırdığınızda, kapsayıcının tam etki alanı adı (FQDN) ve sağlama durumu görüntülenir:

```console
$ az container show --resource-group myResourceGroup --name mycontainer --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" --out table
FQDN                               ProvisioningState
---------------------------------  -------------------
aci-demo.eastus.azurecontainer.io  Succeeded
```

Kapsayıcı **Başarılı** durumuna geçtiğinde, FQDN’sine giderek tarayıcınızdan kapsayıcıya ulaşabilirsiniz:

![Bir Azure kapsayıcı örneğinde çalışan uygulamayı gösteren tarayıcı ekran görüntüsü][aci-app-browser]

## <a name="pull-the-container-logs"></a>Kapsayıcı günlüklerini çekme

Oluşturduğunuz kapsayıcı için günlükleri [az container logs][az-container-logs] komutunu kullanarak çekebilirsiniz:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer
```

Çıktı:

```bash
listening on port 80
::ffff:10.240.255.107 - - [29/Nov/2017:20:48:50 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/62.0.3202.94 Safari/537.36"
::ffff:10.240.255.107 - - [29/Nov/2017:20:48:50 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://52.224.178.107/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/62.0.3202.94 Safari/537.36"
```

## <a name="delete-the-container"></a>Kapsayıcıyı silme

Kapsayıcıyla işiniz bittiğinde [az container delete][az-container-delete] komutunu kullanarak kapsayıcıyı kaldırabilirsiniz:

```azurecli-interactive
az container delete --resource-group myResourceGroup --name mycontainer
```

Kapsayıcının silindiğini doğrulamak için, [az container list](/cli/azure/container#az_container_list) komutunu yürütün:

```azurecli-interactive
az container list --resource-group myResourceGroup --output table
```

**mycontainer** kapsayıcısı komut çıkışında görünmemelidir. Kaynak grubunda başka kapsayıcınız yoksa, çıkış görüntülenmez.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç bölümünde kullanılan kapsayıcısı için tüm kodlar [GitHub'da][app-github-repo], ilgili Dockerfile ile birlikte bulunur. Kapsayıcıyı kendiniz oluşturup Azure Container Registry’yi kullanarak Azure Container Instances’a dağıtmayı denemek istiyorsanız Azure Container Instances öğreticisine geçin.

> [!div class="nextstepaction"]
> [Azure Container Instances öğreticileri](./container-instances-tutorial-prepare-app.md)

Kapsayıcıları Azure’da bir düzenleme sistemi içinde çalıştırma seçenekleri için, [Service Fabric][service-fabric] veya [Azure Container Service (AKS)][container-service] hızlı başlangıçlarına bakın.

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png

<!-- LINKS - External -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git
[azure-account]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F
[node-js]: http://nodejs.org

<!-- LINKS - Internal -->
[az-group-create]: /cli/azure/group?view=azure-cli-latest#az_group_create
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az_container_create
[az-container-delete]: /cli/azure/container?view=azure-cli-latest#az_container_delete
[az-container-list]: /cli/azure/container?view=azure-cli-latest#az_container_list
[az-container-logs]: /cli/azure/container?view=azure-cli-latest#az_container_logs
[az-container-show]: /cli/azure/container?view=azure-cli-latest#az_container_show
[azure-cli-install]: /cli/azure/install-azure-cli
[container-service]: ../aks/kubernetes-walkthrough.md
[service-fabric]: ../service-fabric/service-fabric-quickstart-containers.md
