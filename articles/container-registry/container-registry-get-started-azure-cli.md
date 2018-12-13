---
title: Hızlı Başlangıç - Azure - Azure CLI özel Docker kayıt defteri oluşturma
description: Azure CLI ile hızlıca özel bir Docker kapsayıcısı kayıt defteri oluşturmayı öğrenin.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: quickstart
ms.date: 03/03/2018
ms.author: danlep
ms.custom: seodec18, H1Hack27Feb2017, mvc
ms.openlocfilehash: e75a2d126680c71542aa04bae5a30ea7c376cea1
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2018
ms.locfileid: "53255933"
---
# <a name="quickstart-create-a-private-container-registry-using-the-azure-cli"></a>Hızlı Başlangıç: Azure CLI kullanarak bir özel kapsayıcı kayıt defteri oluşturma

Azure Container Registry, özel Docker kapsayıcı görüntülerini depolamak için kullanılan bir yönetilen Docker kapsayıcı kayıt defteridir. Bu kılavuzda Azure CLI kullanarak bir Azure Container Registry örneği oluşturma, kayıt defterine bir kapsayıcı görüntüsü gönderme ve son olarak kapsayıcıyı kayıt defterinizden Azure Container Instances’a (ACI) dağıtma hakkında ayrıntılı bilgi verilmektedir.

Bu hızlı başlangıç için Azure CLI 2.0.27 veya sonraki bir sürümü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli].

Ayrıca sisteminizde yerel olarak Docker yüklü olması gerekir. Docker [macOS][docker-mac], [Windows][docker-windows] veya [Linux][docker-linux]'ta Docker'ı kolayca yapılandırmanızı sağlayan paketler sağlar.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create][az-group-create] komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

Bu hızlı başlangıçta bir *Temel* kayıt defteri oluşturursunuz. Azure Container Registry, aşağıdaki tabloda kısaca açıklandığı gibi çeşitli SKU’lar altında sunulur. Her biriyle ilgili ayrıntılı bilgi edinmek için bkz. [Kapsayıcı kayıt defteri SKU'ları][container-registry-skus].

[!INCLUDE [container-registry-sku-matrix](../../includes/container-registry-sku-matrix.md)]

[az act create][az-acr-create] komutunu kullanarak bir ACR örneği oluşturun. Kaynak defteri adı Azure’da benzersiz olmalı ve 5-50 arası alfasayısal karakter içermelidir. Aşağıdaki örnekte *myContainerRegistry007* komutu kullanılmıştır. Bunu benzersiz bir değerle güncelleştirin.

```azurecli
az acr create --resource-group myResourceGroup --name myContainerRegistry007 --sku Basic
```

Kayıt defteri oluşturulduğunda çıkış aşağıdakilere benzer:

```json
{
  "adminUserEnabled": false,
  "creationDate": "2017-09-08T22:32:13.175925+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry007",
  "location": "eastus",
  "loginServer": "myContainerRegistry007.azurecr.io",
  "name": "myContainerRegistry007",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

Bu hızlı başlangıcın geri kalanında `<acrName>`, kapsayıcı kayıt defteri adı için bir yer tutucudur.

## <a name="log-in-to-acr"></a>ACR üzerinde oturum açın

Kapsayıcı görüntülerini gönderip çekmeden önce ACR örneğinde oturum açmalısınız. Bunu yapmak için [az acr login][az-acr-login] komutunu kullanın.

```azurecli
az acr login --name <acrName>
```

Bu komut tamamlandığında `Login Succeeded` iletisi döndürülür.

## <a name="push-image-to-acr"></a>Görüntüyü ACR’ye gönderme

Azure Container kayıt defterine görüntü gönderebilmeniz için önce bir görüntünüz olmalıdır. Henüz yerel kapsayıcı görüntünüz yoksa Docker Hub’dan mevcut bir görüntüyü çekmek için aşağıdaki komutu çalıştırın.

```bash
docker pull microsoft/aci-helloworld
```

Kayıt defterinize görüntü gönderebilmeniz için, ACR oturum açma sunucunuzun tam adını etiketlemeniz gerekir. ACR örneğinizin tam oturum açma sunucusu adını öğrenmek için aşağıdaki komutu çalıştırın.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Görüntüyü [docker tag][docker-tag] komutunu kullanarak etiketleyin. `<acrLoginServer>` değerini, ACR örneğinizin sunucu adıyla değiştirin.

```bash
docker tag microsoft/aci-helloworld <acrLoginServer>/aci-helloworld:v1
```

Son olarak, [docker push][docker-push] komutunu kullanarak görüntüyü ACR örneğine gönderin. `<acrLoginServer>` değerini, ACR örneğinizin sunucu adıyla değiştirin.

```bash
docker push <acrLoginServer>/aci-helloworld:v1
```

## <a name="list-container-images"></a>Kapsayıcı görüntülerini listeleme

Aşağıdaki örnekte, bir kayıt defterinde yer alan depolar listelenmektedir:

```azurecli
az acr repository list --name <acrName> --output table
```

Çıkış:

```bash
Result
----------------
aci-helloworld
```

Aşağıdaki örnekte, **aci-helloworld** deposunda yer alan etiketlerin listesi verilmiştir.

```azurecli
az acr repository show-tags --name <acrName> --repository aci-helloworld --output table
```

Çıkış:

```bash
Result
--------
v1
```

## <a name="deploy-image-to-aci"></a>Görüntüyü ACI’ya dağıtma

Oluşturduğunuz kayıt defterinden bir kapsayıcı örneği dağıtmak için, dağıtım sırasında kayıt defteri kimlik bilgilerini sağlamanız gerekir. Üretim senaryolarında kapsayıcı kayıt defteri erişimi için bir [hizmet sorumlusu][container-registry-auth-aci] kullanmanız gerekir; ancak bu hızlı başlangıcı kısa tutmak amacıyla aşağıdaki komutu kullanarak kayıt defterinizde yönetici kullanıcıyı etkinleştirin:

```azurecli
az acr update --name <acrName> --admin-enabled true
```

Yönetici etkinleştirildikten sonra kullanıcı adı, kayıt defterinizin adı ile aynıdır ve parola şu komutla alınabilir:

```azurecli
az acr credential show --name <acrName> --query "passwords[0].value"
```

Kapsayıcı görüntünüzü 1 CPU çekirdeği ve 1 GB bellekle dağıtmak için aşağıdaki komutu çalıştırın. `<acrName>`, `<acrLoginServer>` ve `<acrPassword>` değerlerini, önceki komutlardan aldığınız değerlerle değiştirin.

```azurecli
az container create --resource-group myResourceGroup --name acr-quickstart --image <acrLoginServer>/aci-helloworld:v1 --cpu 1 --memory 1 --registry-username <acrName> --registry-password <acrPassword> --dns-name-label aci-demo --ports 80
```

Azure Resource Manager’dan kapsayıcınızın ayrıntılarını içeren ilk yanıtı almanız gerekir. Kapsayıcınızın durumunu izlemek ve ne zaman çalıştığını denetleyip görmek için [az container show][az-container-show] komutunu tekrarlayın. Bu işlemin bir dakikadan kısa sürmesi gerekir.

```azurecli
az container show --resource-group myResourceGroup --name acr-quickstart --query instanceView.state
```

## <a name="view-the-application"></a>Uygulamayı görüntüleme

ACI’ya dağıtım başarılı olduktan sonra kapsayıcının FQDN’sini [az container show][az-container-show] komutuyla alın:

```azurecli
az container show --resource-group myResourceGroup --name acr-quickstart --query ipAddress.fqdn
```

Örnek çıktı: `"aci-demo.eastus.azurecontainer.io"`

Çalışan uygulamayı görmek için, sık kullandığınız tarayıcıda genel IP adresine gidin.

![Tarayıcıdaki Merhaba Dünya uygulaması][aci-app-browser]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse [az group delete][az-group-delete] komutunu kullanarak kaynak grubunu, ACR örneğini ve tüm kapsayıcı görüntülerini kaldırabilirsiniz.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure CLI ile bir Azure Container Registry oluşturdunuz, kayıt defterine bir kapsayıcı görüntüsü gönderdiniz ve Azure Container Instances üzerinden bir örneğini başlattınız. ACI’ya daha ayrıntılı bir bakış için Azure Container Instances öğreticisine geçin.

> [!div class="nextstepaction"]
> [Azure Container Instances öğreticisi][container-instances-tutorial-prepare-app]

<!-- IMAGES> -->
[aci-app-browser]: ../container-instances/media/container-instances-quickstart/aci-app-browser.png


<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[az-acr-create]: /cli/azure/acr#az-acr-create
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[azure-cli]: /cli/azure/install-azure-cli
[az-container-show]: /cli/azure/container#az-container-show
[container-instances-tutorial-prepare-app]: ../container-instances/container-instances-tutorial-prepare-app.md
[container-registry-skus]: container-registry-skus.md
[container-registry-auth-aci]: container-registry-auth-aci.md
