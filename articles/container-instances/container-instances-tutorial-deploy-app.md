---
title: "Azure Container Instances öğreticisi - Uygulamayı dağıtma"
description: "Azure Container Instances öğreticisi bölüm 3/3 - Uygulamayı dağıtma"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 02/20/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 250f74b1a05959b93000452c4d5f025311f379d8
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2018
---
# <a name="deploy-a-container-to-azure-container-instances"></a>Azure Container Instances‘a kapsayıcı dağıtma

Bu, üç kısımdan oluşan serinin son öğreticisidir. Serinin önceki kısımlarında [bir kapsayıcı görüntüsü oluşturuldu](container-instances-tutorial-prepare-app.md) ve [Azure Container Registry’ye gönderildi](container-instances-tutorial-prepare-acr.md). Bu makalede, kapsayıcı Azure Container Instances’a dağıtılarak öğretici serisi tamamlanır.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Azure CLI’yi kullanarak Azure Container Registry’den kapsayıcıyı dağıtma
> * Uygulamayı tarayıcıda görüntüleme
> * Kapsayıcı günlüklerini görüntüleme

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğretici için Azure CLI 2.0.23 veya sonraki bir sürümü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme][azure-cli-install].

Bu öğreticiyi tamamlamak için yerel olarak yüklü bir Docker geliştirme ortamı gerekir. Docker [Mac][docker-mac], [Windows][docker-windows] veya [Linux][docker-linux]'ta Docker'ı kolayca yapılandırmanızı sağlayan paketler sağlar.

Azure Cloud Shell, bu öğreticideki her adımı tamamlamak için gerekli olan Docker bileşenlerini içermez. Bu öğreticiyi tamamlamak için, yerel bilgisayarınıza Azure CLI ve Docker geliştirme ortamını yüklemeniz gerekir.

## <a name="deploy-the-container-using-the-azure-cli"></a>Azure CLI’yi kullanarak kapsayıcıyı dağıtma

Azure CLI, tek bir komutta bir kapsayıcının Azure Container Instances’a dağıtımını sağlar. Kapsayıcı görüntüsü özel Azure Container Registry’de barındırıldığından, ona erişmek için gerekli kimlik bilgilerini dahil etmeniz gerekir. Aşağıdaki Azure CLI komutlarıyla kimlik bilgilerini edinin.

Kapsayıcı kayıt defteri oturum açma sunucusu (kayıt defteri adınızla güncelleştirin):

```azurecli
az acr show --name <acrName> --query loginServer
```

Kapsayıcı kayıt defteri parolası:

```azurecli
az acr credential show --name <acrName> --query "passwords[0].value"
```

1 CPU çekirdeği ve 1 GB bellek için kaynak isteğiyle kapsayıcı kayıt defterinden kapsayıcı görüntünüzü dağıtmak için aşağıdaki komutu çalıştırın. `<acrLoginServer>` ve `<acrPassword>` değerlerini, önceki iki komuttan aldığınız değerlerle değiştirin. `<acrName>` komutunu, kapsayıcı kayıt defterinizin adıyla değiştirin.

```azurecli
az container create --resource-group myResourceGroup --name aci-tutorial-app --image <acrLoginServer>/aci-tutorial-app:v1 --cpu 1 --memory 1 --registry-username <acrName> --registry-password <acrPassword> --dns-name-label aci-demo --ports 80
```

Birkaç saniye içinde Azure Resource Manager’dan bir ilk yanıt almanız gerekir. `--dns-name-label` değeri, kapsayıcı örneğini oluşturduğunuz Azure bölgesi içinde benzersiz olmalıdır. Komutu yürütürken bir **DNS ad etiketi** hata iletisi alırsanız önceki örnekte yer alan değeri güncelleştirin.

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

Örnek çıktı: `"aci-demo.eastus.azurecontainer.io"`

Çalışan uygulamayı görmek için, sık kullandığınız tarayıcıda görüntülenen DNS adına gidin:

![Tarayıcıdaki Merhaba Dünya uygulaması][aci-app-browser]

Kapsayıcının günlük çıktısını da görüntüleyebilirsiniz:

```azurecli
az container logs --resource-group myResourceGroup --name aci-tutorial-app
```

Çıktı:

```bash
listening on port 80
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://13.88.176.27/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık bu öğretici serisinde oluşturduğunuz kaynakların herhangi birine ihtiyacınız yoksa, kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için [az group delete][az-group-delete] komutunu yürütebilirsiniz. Bu komut, oluşturduğunuz kapsayıcı kayıt defterinin yanı sıra çalışan kapsayıcıyı ve tüm ilişkili kaynakları siler.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Container Instances’a kapsayıcıları dağıtma işlemini tamamladınız. Aşağıdaki adımlar tamamlandı:

> [!div class="checklist"]
> * Azure CLI kullanılarak Azure Container Registry’den kapsayıcı dağıtıldı
> * Tarayıcıda uygulama görüntülendi
> * Kapsayıcı günlükleri görüntülendi

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
[az-container-show]: /cli/azure/container#az_container_show
[az-group-delete]: /cli/azure/group#az_group_delete
[azure-cli-install]: /cli/azure/install-azure-cli
[prepare-app]: ./container-instances-tutorial-prepare-app.md
