---
title: "Anında iletme Docker görüntüsüne özel Azure kayıt defteri"
description: "Docker CLI’yı kullanarak Azure’da özel bir kapsayıcı kayıt defterine Docker görüntüleri itme ve kapsayıcıdan görüntü çekme"
services: container-registry
author: stevelas
manager: balans
editor: mmacy
ms.service: container-registry
ms.topic: article
ms.date: 11/29/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 42877b0aeecf08602d31ba21dccc814e542fd174
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="push-your-first-image-to-a-private-docker-container-registry-using-the-docker-cli"></a>Docker CLI’yı kullanarak özel bir Dockler kapsayıcı kayıt defterine ilk görüntünüzü itme

Azure kapsayıcısı kayıt defteri, [Docker Hub](https://hub.docker.com/)’ın genel Docker görüntülerini depolama yöntemine benzer şekilde özel [Docker](http://hub.docker.com) kapsayıcı görüntülerini depolar ve yönetir. Kullanabileceğiniz [Docker komut satırı arabirimi](https://docs.docker.com/engine/reference/commandline/cli/) (Docker CLI) için [oturum açma](https://docs.docker.com/engine/reference/commandline/login/), [itme](https://docs.docker.com/engine/reference/commandline/push/), [çekme](https://docs.docker.com/engine/reference/commandline/pull/)ve, kapsayıcı üzerinde başka işlemler kayıt defteri.

Aşağıdaki adımlarda resmi karşıdan [Nginx görüntü](https://store.docker.com/images/nginx) ortak Docker hub'a kayıt defterinden özel Azure kapsayıcı kaydınız etiketi, kayıt defterine itme ve kayıt defterinden çekme.

## <a name="prerequisites"></a>Ön koşullar

* **Azure kapsayıcısı kayıt defteri** -Azure aboneliğinizde bir kapsayıcı kayıt defteri oluşturun. Örneğin, [Azure portalını](container-registry-get-started-portal.md) veya [Azure CLI 2.0](container-registry-get-started-azure-cli.md)’ı kullanın.
* **Docker CLI** - yerel bilgisayarınıza Docker ana bilgisayar olarak ayarlayın ve Docker CLI komutlara erişmek için yükleme [Docker](https://docs.docker.com/engine/installation/).

## <a name="log-in-to-a-registry"></a>Kayıt defterinde oturum açma

Vardır [kimliğini doğrulamak için çeşitli yollar](container-registry-authentication.md) özel kapsayıcı kayıt. Komut satırında çalışırken önerilen yöntem ile Azure CLI komuttur [az acr oturum açma](/cli/azure/acr?view=azure-cli-latest#az_acr_login). Örneğin, bir kayıt defterine oturum açmak için adlı *myregistry*:

```azurecli
az acr login --name myregistry
```

İle oturum açabilir [docker oturum açma](https://docs.docker.com/engine/reference/commandline/login/). Aşağıdaki örnekte, bir Azure Active Directory [hizmet sorumlusunun](../active-directory/active-directory-application-objects.md) kimliği ve parolası geçirilmiştir. Örneğin, olabilir [bir hizmet sorumlusu atanan](container-registry-authentication.md#service-principal) bir Otomasyon senaryosu için kayıt defterine.

```Bash
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Her iki komutlar döner `Login Succeeded` tamamlandıktan sonra. Kullanırsanız `docker login`, ayrıca, kullanımını öneren bir güvenlik uyarısı görebilirsiniz `--password-stdin` parametresi. Kullanımı, bu makalenin kapsamı dışında olsa da, bu en iyi yöntem öneririz. Daha fazla bilgi için bkz: [docker oturum açma](https://docs.docker.com/engine/reference/commandline/login/) komut başvurusu.

> [!TIP]
> Her zaman tam kayıt adı (tümü küçük harf) belirtmeniz kullandığınızda `docker login` ve etiket zaman, kayıt defterine gönderilmesi için görüntüler. Bu makaledeki örneklerde, tam ad olduğundan *myregistry.azurecr.io*.

## <a name="pull-the-official-nginx-image"></a>Resmi Nginx görüntü isteme

İlk olarak, yerel bilgisayarınıza ortak Nginx görüntü çeker.

```Bash
docker pull nginx
```

## <a name="run-the-container-locally"></a>Kapsayıcıyı yerel olarak çalıştırma

Yürütme aşağıdaki [çalıştırmak docker](https://docs.docker.com/engine/reference/run/) Nginx kapsayıcısı yerel bir örneğini etkileşimli olarak başlatmak için komut (`-it`) bağlantı noktası 8080 üzerinde. `--rm` Bağımsız değişkeni işlemini durdurduğunuzda kapsayıcı kaldırılması gerektiğini belirtir.

```Bash
docker run -it --rm -p 8080:80 nginx
```

Gözat [http://localhost: 8080](http://localhost:8080) çalışan kapsayıcısında Nginx tarafından sunulan varsayılan web sayfasını görüntülemek için. Aşağıdakine benzer bir sayfa görmeniz gerekir:

![Yerel bilgisayarda Nginx](./media/container-registry-get-started-docker-cli/nginx.png)

Kapsayıcıyla etkileşimli olarak başlatılan çünkü `-it`, Nginx çıkış sunucu komut satırında için tarayıcınızda gezinme sonra görebilirsiniz.

Durdurun ve kapsayıcı kaldırmak için basın `Control` + `C`.

## <a name="create-an-alias-of-the-image"></a>Görüntünün bir diğer ad oluşturma

Kullanım [docker etiketi](https://docs.docker.com/engine/reference/commandline/tag/) kaydınız tam yoluna sahip görüntünün bir diğer ad oluşturmak için. Bu örnek, kayıt defterinin kökünde dağınıklığı önlemek için `samples` ad alanını belirtir.

```Bash
docker tag nginx myregistry.azurecr.io/samples/nginx
```

Ad alanları ile etiketleme hakkında daha fazla bilgi için bkz: [deposu ad alanları](container-registry-best-practices.md#repository-namespaces) bölümünü [en iyi uygulamalar için Azure kapsayıcı kayıt defteri](container-registry-best-practices.md).

## <a name="push-the-image-to-your-registry"></a>Görüntü, kayıt defterine bildirme

Özel kayıt defterine tam yolunu görüntüsüyle etiketlediğiniz, bu kayıt defteri ile gönderebilir [docker itme](https://docs.docker.com/engine/reference/commandline/push/):

```Bash
docker push myregistry.azurecr.io/samples/nginx
```

## <a name="pull-the-image-from-your-registry"></a>Kayıt defterinden görüntü isteme

Kullanım [docker çekme](https://docs.docker.com/engine/reference/commandline/pull/) kayıt defterinden görüntü çekmesini komutu:

```Bash
docker pull myregistry.azurecr.io/samples/nginx
```

## <a name="start-the-nginx-container"></a>Nginx kapsayıcısı Başlat

Kullanım [çalıştırmak docker](https://docs.docker.com/engine/reference/run/) kayıt defterinden çekilen görüntü çalıştırılacak komutu:

```Bash
docker run -it --rm -p 8080:80 myregistry.azurecr.io/samples/nginx
```

Çalışmakta olan kapsayıcıyı görmek için [http://localhost:8080](http://localhost:8080) konumuna gidin.

Durdurun ve kapsayıcı kaldırmak için basın `Control` + `C`.

## <a name="remove-the-image-optional"></a>(İsteğe bağlı) resim Kaldır

Nginx görüntü artık ihtiyacınız varsa, yerel olarak ile silebilirsiniz [docker RMI](https://docs.docker.com/engine/reference/commandline/rmi/) komutu.

```Bash
docker rmi myregistry.azurecr.io/samples/nginx
```

Azure kapsayıcı kayıt defterinden görüntüleri kaldırmak için Azure CLI komutunu kullanabilirsiniz [az acr deposu delete](/cli/azure/acr/repository#az_acr_repository_delete). Örneğin, aşağıdaki komutu bir etiketi, herhangi bir ilişkili katman veri ve bildirim başvuran tüm etiketleri tarafından başvurulan bildirimi siler.

```azurecli
az acr repository delete --name myregistry --repository samples/nginx --tag latest --manifest
```

## <a name="next-steps"></a>Sonraki adımlar

Temel bilgileri artık bildiğinize göre kayıt defteri kullanmaya başlamak hazırsınız! Örneğin, kayıt defterine kapsayıcı görüntüleri dağıtmak bir [Azure kapsayıcı hizmeti (AKS)](../aks/tutorial-kubernetes-prepare-app.md) küme.
