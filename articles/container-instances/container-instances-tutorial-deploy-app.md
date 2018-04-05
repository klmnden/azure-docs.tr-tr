---
title: Azure Container Instances öğreticisi - Uygulamayı dağıtma
description: Azure Container Instances öğreticisi bölüm 3/3 - Uygulamayı dağıtma
services: container-instances
author: mmacy
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 03/21/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 29d7114f288f7387d0c7cd5c6afe2eaaa7a8c560
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="tutorial-deploy-a-container-to-azure-container-instances"></a>Öğretici: Azure Container Instances’a kapsayıcı dağıtma

Bu, üç kısımdan oluşan serinin son öğreticisidir. Serinin önceki kısımlarında [bir kapsayıcı görüntüsü oluşturuldu](container-instances-tutorial-prepare-app.md) ve [Azure Container Registry’ye gönderildi](container-instances-tutorial-prepare-acr.md). Bu makalede, Azure Container Instances’a kapsayıcı dağıtılarak seri tamamlanır.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Azure Container Registry’den Azure Container Instances’a kapsayıcıyı dağıtma
> * Tarayıcıda çalıştırılan uygulamayı görüntüleme
> * Kapsayıcının günlüklerini görüntüleme

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [container-instances-tutorial-prerequisites](../../includes/container-instances-tutorial-prerequisites.md)]

## <a name="deploy-the-container-using-the-azure-cli"></a>Azure CLI’yi kullanarak kapsayıcıyı dağıtma

Bu bölümde, [birinci öğreticide](container-instances-tutorial-prepare-app.md) derlenen ve [ikinci öğreticide](container-instances-tutorial-prepare-acr.md) Azure Container Registry’ye gönderilen görüntüyü dağıtmak için Azure CLI’yi kullanırsınız. Devam etmeden önce bu öğreticileri tamamladığınızdan emin olun.

### <a name="get-registry-credentials"></a>Kayıt defteri kimlik bilgilerini alma

[İkinci öğreticide](container-instances-tutorial-prepare-acr.md) oluşturulana benzer bir özel kapsayıcı kayıt defterinde barındırılan bir görüntüyü dağıtırken kayıt defterinin kimlik bilgilerini sağlamanız gerekir.

İlk olarak, kapsayıcı kayıt defteri oturum açma sunucusunun tam adını alın (`<acrName>` değerini, kayıt defterinizin adıyla değiştirin):

```azurecli
az acr show --name <acrName> --query loginServer
```

Sonra kapsayıcı kayıt defteri parolasını alın:

```azurecli
az acr credential show --name <acrName> --query "passwords[0].value"
```

### <a name="deploy-container"></a>Kapsayıcıyı dağıtma

Şimdi kapsayıcıyı dağıtmak için [az container create][az-container-create] komutunu kullanın. `<acrLoginServer>` ve `<acrPassword>` değerlerini, önceki iki komuttan aldığınız değerlerle değiştirin. `<acrName>` komutunu, kapsayıcı kayıt defterinizin adıyla değiştirin.

```azurecli
az container create --resource-group myResourceGroup --name aci-tutorial-app --image <acrLoginServer>/aci-tutorial-app:v1 --cpu 1 --memory 1 --registry-username <acrName> --registry-password <acrPassword> --dns-name-label aci-demo --ports 80
```

Birkaç saniye içinde Azure’dan bir ilk yanıt almanız gerekir. `--dns-name-label` değeri, kapsayıcı örneğini oluşturduğunuz Azure bölgesi içinde benzersiz olmalıdır. Komutu yürütürken bir **DNS ad etiketi** hata iletisi alırsanız önceki komutta yer alan değeri değiştirin.

### <a name="verify-deployment-progress"></a>Dağıtım ilerleme durumunu doğrulama

Dağıtımın durumunu görüntülemek için [az container show][az-container-show] komutunu kullanın:

```azurecli
az container show --resource-group myResourceGroup --name aci-tutorial-app --query instanceView.state
```

*Beklemede* olan durum *Çalışıyor* olarak değişinceye kadar [az container show][az-container-show] komutunu yineleyin; bu bir dakikadan kısa sürecektir. Kapsayıcı *Çalışıyor* durumunda olduğunda sonraki adıma ilerleyin.

## <a name="view-the-application-and-container-logs"></a>Uygulama ve kapsayıcı günlüklerini görüntüleme

Dağıtım başarılı olduktan sonra, [az container show][az-container-show] komutuyla kapsayıcının tam etki alanı adını görüntüleyin:

```bash
az container show --resource-group myResourceGroup --name aci-tutorial-app --query ipAddress.fqdn
```

Örnek:
```console
$ az container show --resource-group myResourceGroup --name aci-tutorial-app --query ipAddress.fqdn
"aci-demo.eastus.azurecontainer.io"
```

Çalışan uygulamayı görmek için, sık kullandığınız tarayıcıda görüntülenen DNS adına gidin:

![Tarayıcıdaki Merhaba Dünya uygulaması][aci-app-browser]

Kapsayıcının günlük çıktısını da görüntüleyebilirsiniz:

```azurecli
az container logs --resource-group myResourceGroup --name aci-tutorial-app
```

Örnek çıktı:

```bash
$ az container logs --resource-group myResourceGroup --name aci-tutorial-app
listening on port 80
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://aci-demo.eastus.azurecontainer.io/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık bu öğretici serisinde oluşturduğunuz kaynakların herhangi birine ihtiyacınız yoksa, kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için [az group delete][az-group-delete] komutunu yürütebilirsiniz. Bu komut, oluşturduğunuz kapsayıcı kayıt defterinin yanı sıra çalışan kapsayıcıyı ve tüm ilişkili kaynakları siler.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Container Instances’a kapsayıcınızı dağıtma işlemini tamamladınız. Aşağıdaki adımlar tamamlandı:

> [!div class="checklist"]
> * Azure CLI kullanılarak Azure Container Registry’den kapsayıcı dağıtıldı
> * Tarayıcıda uygulama görüntülendi
> * Kapsayıcı günlükleri görüntülendi

Temel bilgileri edindiğinize göre şimdi kapsayıcı gruplarının nasıl çalıştığı gibi, Azure Container Instances hakkında daha fazla bilgi edinmeye geçebilirsiniz:

> [!div class="nextstepaction"]
> [Azure Container Instances’taki kapsayıcı grupları](container-instances-container-groups.md)

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[az-container-create]: /cli/azure/container#az_container_create
[az-container-show]: /cli/azure/container#az_container_show
[az-group-delete]: /cli/azure/group#az_group_delete
[azure-cli-install]: /cli/azure/install-azure-cli
[prepare-app]: ./container-instances-tutorial-prepare-app.md
