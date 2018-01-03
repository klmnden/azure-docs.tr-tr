---
title: "Azure kapsayıcı örnekleri Öğreticisi - app dağıtma"
description: "Azure kapsayıcı örnekleri Öğreticisi bölüm 3 / 3 - uygulamayı dağıtmak"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 471caa1b24dc7017c70782c072b2068f9635244b
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2018
---
# <a name="deploy-a-container-to-azure-container-instances"></a>Azure kapsayıcı örnekleri bir kapsayıcı dağıtma

Son üç bölümlük öğreticide budur. Serideki önceki [bir kapsayıcı görüntüsü oluşturuldu](container-instances-tutorial-prepare-app.md) ve [bir Azure kapsayıcı kayıt defterine gönderilen](container-instances-tutorial-prepare-acr.md). Bu makalede, Azure kapsayıcı örnekleri kapsayıcı dağıtarak öğretici serisi tamamlar.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Azure CLI kullanarak Azure kapsayıcı kayıt defterinden kapsayıcı dağıtma
> * Uygulamayı tarayıcıda görüntüleme
> * Kapsayıcı günlükleri görüntüle

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğretici, Azure CLI Sürüm 2.0.23 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Gerekirse yükleyin veya yükseltin, bakın [Azure CLI 2.0 yükleme][azure-cli-install].

Bu öğreticiyi tamamlamak için yerel olarak yüklenmiş bir Docker geliştirme ortamı gerekir. Docker sağlar kolayca Docker herhangi yapılandırdığınız paketler [Mac][docker-mac], [Windows][docker-windows], veya [Linux] [ docker-linux] sistem.

Azure bulut Kabuk her adımı tamamlamak için gereken Docker bileşenleri Bu öğretici içermez. Bu öğreticiyi tamamlamak için yerel bilgisayarınızda Azure CLI ve Docker geliştirme ortamı yüklemeniz gerekir.

## <a name="deploy-the-container-using-the-azure-cli"></a>Azure CLI kullanarak kapsayıcı dağıtma

Azure CLI Azure kapsayıcı örnekleri için bir kapsayıcı tek bir komut içinde dağıtımını sağlar. Kapsayıcı görüntü özel Azure kapsayıcı kayıt defterinde barındırılan beri erişmek için gerekli kimlik bilgilerini eklemeniz gerekir. Aşağıdaki Azure CLI komutları kimlik bilgilerini edinin.

Kapsayıcı kayıt defteri oturum açma sunucusu (kayıt defteri adınızı güncelleştirmesiyle):

```azurecli
az acr show --name <acrName> --query loginServer
```

Kapsayıcı kayıt defteri parola:

```azurecli
az acr credential show --name <acrName> --query "passwords[0].value"
```

Kapsayıcı görüntünüzü 1 CPU çekirdek kaynak isteği ve 1 GB bellek ile kapsayıcı kayıt defterinden dağıtmak için aşağıdaki komutu çalıştırın. Değiştir `<acrLoginServer>` ve `<acrPassword>` önceki iki komutlarından elde edilen değerleri ile.

```azurecli
az container create --resource-group myResourceGroup --name aci-tutorial-app --image <acrLoginServer>/aci-tutorial-app:v1 --cpu 1 --memory 1 --registry-password <acrPassword> --ip-address public --ports 80
```

Birkaç saniye içinde Azure Kaynak Yöneticisi'nden bir ilk yanıt almanız gerekir. Dağıtım durumunu görüntülemek için kullanın [az kapsayıcı Göster][az-container-show]:

```azurecli
az container show --resource-group myResourceGroup --name aci-tutorial-app --query instanceView.state
```

Yineleme [az kapsayıcı Göster] [ az-container-show] durumu değişiklikleri kadar komut *bekleyen* için *çalıştıran*, altında bir dakika almalıdır. Kapsayıcı olduğunda *çalıştıran*, sonraki adıma geçebilirsiniz.

## <a name="view-the-application-and-container-logs"></a>Uygulama ve kapsayıcı günlükleri görüntüleme

Dağıtım başarılı olduktan sonra kapsayıcının ortak IP adresiyle görüntülemek [az kapsayıcı Göster] [ az-container-show] komutu:

```bash
az container show --resource-group myResourceGroup --name aci-tutorial-app --query ipAddress.ip
```

Örnek çıktı:`"13.88.176.27"`

Çalışan uygulama görmek için en sevdiğiniz tarayıcınızda ortak IP adresine gidin.

![Hello world uygulamayı tarayıcıda][aci-app-browser]

Ayrıca, kapsayıcı günlük çıktısı görüntüleyebilirsiniz:

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

Bu öğretici serisinde oluşturduğunuz kaynaklardan herhangi birini artık ihtiyacınız varsa, yürütebilir [az grubu Sil] [ az-group-delete] kaynak grubu ve içerdiği tüm kaynaklar kaldırmak için komutu. Bu komut, oluşturduğunuz kapsayıcı kayıt defteri yanı sıra çalışan kapsayıcısı ve tüm ilişkili kaynakları siler.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure kapsayıcı örnekleri, kapsayıcıları dağıtma işlemi tamamlandı. Aşağıdaki adımlar tamamlandı:

> [!div class="checklist"]
> * Azure CLI kullanarak Azure kapsayıcı kayıt defterinden kapsayıcı dağıtılan
> * Uygulamayı tarayıcıda görüntülenen
> * Kapsayıcı günlükleri görüntülenebilir

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