---
title: Docker görüntüsünü özel Azure container registry'ye gönderme
description: Docker CLI’yı kullanarak Azure’da özel bir kapsayıcı kayıt defterine Docker görüntüleri itme ve kapsayıcıdan görüntü çekme
services: container-registry
author: dlepow
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 01/23/2019
ms.author: danlep
ms.custom: seodec18, H1Hack27Feb2017
ms.openlocfilehash: 2cb401dfd68075ff0867ae3f89eee3474000b5de
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60828768"
---
# <a name="push-your-first-image-to-a-private-docker-container-registry-using-the-docker-cli"></a>Docker CLI’yı kullanarak özel bir Dockler kapsayıcı kayıt defterine ilk görüntünüzü itme

Azure kapsayıcısı kayıt defteri, [Docker Hub](https://hub.docker.com/)’ın genel Docker görüntülerini depolama yöntemine benzer şekilde özel [Docker](https://hub.docker.com) kapsayıcı görüntülerini depolar ve yönetir. Kullanabileceğiniz [Docker komut satırı arabirimi](https://docs.docker.com/engine/reference/commandline/cli/) (Docker CLI) için [oturum açma](https://docs.docker.com/engine/reference/commandline/login/), [anında iletme](https://docs.docker.com/engine/reference/commandline/push/), [çekme](https://docs.docker.com/engine/reference/commandline/pull/)ve kapsayıcınızı üzerinde başka işlemler kayıt defteri.

Aşağıdaki adımlarda indirdiğiniz resmi [Nginx görüntüsü](https://store.docker.com/images/nginx) genel Docker Hub kayıt defterinden nginx özel Azure kapsayıcısı kayıt defteriniz için etiket, kayıt defterinize gönderin ve ardından kayıt defterinden çekme.

## <a name="prerequisites"></a>Önkoşullar

* **Azure kapsayıcısı kayıt defteri** -Azure aboneliğinizde bir kapsayıcı kayıt defteri oluşturun. Örneğin, [Azure portalında](container-registry-get-started-portal.md) veya [Azure CLI](container-registry-get-started-azure-cli.md).
* **Docker CLI** -ayrıca Docker yerel olarak yüklü olması gerekir. Docker [macOS][docker-mac], [Windows][docker-windows] veya [Linux][docker-linux]'ta Docker'ı kolayca yapılandırmanızı sağlayan paketler sağlar.

## <a name="log-in-to-a-registry"></a>Kayıt defterinde oturum açma

Vardır [kimliğini doğrulamak için birkaç yol](container-registry-authentication.md) özel kapsayıcı kayıt defteriniz için. Azure CLI komutuyla bir komut satırında çalışırken önerilen yöntem olduğu [az acr oturum açma](/cli/azure/acr?view=azure-cli-latest#az-acr-login). Örneğin, bir kayıt defterinde oturum açmak için adlı *myregistry*:

```azurecli
az acr login --name myregistry
```

İle oturum açabilir [docker oturum açma](https://docs.docker.com/engine/reference/commandline/login/). Örneğin, sahip olabileceğiniz [hizmet sorumlusuna atanmış](container-registry-authentication.md#service-principal) kayıt defterinize bir Otomasyon senaryosu için. Etkileşimli olarak aşağıdaki komutu çalıştırdığınızda, hizmet sorumlusu uygulama kimliği (kullanıcı adı) ve istendiğinde parolayı belirtin. Oturum açma kimlik bilgilerini yönetmek en iyi yöntemler için bkz: [docker oturum açma](https://docs.docker.com/engine/reference/commandline/login/) komut başvurusuna:

```
docker login myregistry.azurecr.io
```

Her iki komutları döndürür `Login Succeeded` tamamlandıktan sonra.

> [!TIP]
> Her zaman tam kayıt defteri adını (tamamı küçük harf) belirtin kullandığınızda `docker login` ve görüntüleri kayıt defterinize gönderme için etiket zaman. Bu makaledeki örneklerde, tam adı olduğunu *myregistry.azurecr.io*.

## <a name="pull-the-official-nginx-image"></a>Resmi Nginx görüntüsünü çekme

Önce genel Nginx görüntüsünü yerel bilgisayarınıza çekin.

```
docker pull nginx
```

## <a name="run-the-container-locally"></a>Kapsayıcıyı yerel olarak çalıştırma

Yürütme aşağıdaki [docker run](https://docs.docker.com/engine/reference/run/) yerel bir örneğini Nginx kapsayıcısını etkileşimli olarak başlatmak için komut (`-it`) bağlantı noktası 8080 üzerinde. `--rm` Bağımsız değişkeni, durdurduğunuzda kapsayıcı kaldırılması gerektiğini belirtir.

```
docker run -it --rm -p 8080:80 nginx
```

Gözat `http://localhost:8080` çalışmakta olan kapsayıcıyı içinde Ngınx tarafından sunulan varsayılan web sayfasını görüntülemek için. Aşağıdakine benzer bir sayfa görmeniz gerekir:

![Yerel bilgisayarda Nginx](./media/container-registry-get-started-docker-cli/nginx.png)

Kapsayıcı ile etkileşimli olarak başlatıldığından `-it`, Ngınx sunucusunu çıkış komut satırında için tarayıcınızda ayrıldıktan sonra görebilirsiniz.

Durdur ve kapsayıcı kaldırmak için basın `Control` + `C`.

## <a name="create-an-alias-of-the-image"></a>Görüntünün bir diğer ad oluştur

Kullanım [docker tag](https://docs.docker.com/engine/reference/commandline/tag/) kayıt defterinizin tam yoluyla görüntünün bir diğer ad oluşturmak için. Bu örnek, kayıt defterinin kökünde dağınıklığı önlemek için `samples` ad alanını belirtir.

```
docker tag nginx myregistry.azurecr.io/samples/nginx
```

Ad alanları ile etiketleme hakkında daha fazla bilgi için bkz. [depo ad alanlarından](container-registry-best-practices.md#repository-namespaces) bölümünü [en iyi uygulamalar için Azure Container Registry](container-registry-best-practices.md).

## <a name="push-the-image-to-your-registry"></a>Görüntüyü kayıt defterinize gönderme

Özel kayıt defterinize görüntü tam olarak nitelenmiş yola sahip etiketlediğinize göre bunu kayıt defterine gönderebilir [docker itme](https://docs.docker.com/engine/reference/commandline/push/):

```
docker push myregistry.azurecr.io/samples/nginx
```

## <a name="pull-the-image-from-your-registry"></a>Görüntüyü kayıt defterinizden çekme

Kullanım [docker isteği](https://docs.docker.com/engine/reference/commandline/pull/) komut görüntüyü kayıt defterinizden çekme:

```
docker pull myregistry.azurecr.io/samples/nginx
```

## <a name="start-the-nginx-container"></a>Nginx kapsayıcısını başlatma

Kullanım [docker run](https://docs.docker.com/engine/reference/run/) kayıt defterinizden çekilen görüntü çalıştırmak için komutu:

```
docker run -it --rm -p 8080:80 myregistry.azurecr.io/samples/nginx
```

Gözat `http://localhost:8080` çalışmakta olan kapsayıcıyı görmek için.

Durdur ve kapsayıcı kaldırmak için basın `Control` + `C`.

## <a name="remove-the-image-optional"></a>(İsteğe bağlı) görüntüyü kaldırma

Nginx görüntüsü artık ihtiyacınız kalmadığında, yerel olarak ile silebilirsiniz [docker RMI](https://docs.docker.com/engine/reference/commandline/rmi/) komutu.

```
docker rmi myregistry.azurecr.io/samples/nginx
```

Görüntüleri, Azure container registry'den kaldırmak için Azure CLI komutunu kullanabilirsiniz [az acr depo silme](/cli/azure/acr/repository#az-acr-repository-delete). Örneğin, aşağıdaki komut, bildirim tarafından başvurulan siler `samples/nginx:latest` etiketi, herhangi bir benzersiz katman veri ve bildirim başvuran tüm etiketler.

```azurecli
az acr repository delete --name myregistry --image samples/nginx:latest
```

## <a name="next-steps"></a>Sonraki adımlar

Temel bilgileri artık bildiğinize göre defterinizi kullanmaya başlamaya hazırsınız! Örneğin, kayıt defterine kapsayıcı görüntüleri dağıtın:

* [Azure Kubernetes Service (AKS)](../aks/tutorial-kubernetes-prepare-app.md)
* [Azure Container Instances](../container-instances/container-instances-tutorial-prepare-app.md)
* [Service Fabric](../service-fabric/service-fabric-tutorial-create-container-images.md)

İsteğe bağlı olarak yükleme [Visual Studio Code için Docker uzantısını](https://code.visualstudio.com/docs/azure/docker) ve [Azure hesabı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) uzantısı, Azure kapsayıcısı kayıt defterleri ile çalışma. Çekme ve bir Azure container registry'ye görüntüleri gönderme veya Visual Studio Code içinde tüm ACR görevler çalıştırabilirsiniz.


<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-windows]: https://docs.docker.com/docker-for-windows/
