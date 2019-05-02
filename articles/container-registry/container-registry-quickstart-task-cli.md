---
title: Hızlı Başlangıç - derleme ve çalıştırma bir kapsayıcı görüntüsü Azure Container Registry'de
description: Hızlı Görevler oluşturup bir kapsayıcı görüntüsü isteğe bağlı olarak bulutta çalıştırmak için Azure Container Registry ile çalışır.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: quickstart
ms.date: 04/02/2019
ms.author: danlep
ms.custom: ''
ms.openlocfilehash: be120ea8ae588da486c9a5acd4eb7bfdb4e45dee
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64701560"
---
# <a name="quickstart-build-and-run-a-container-image-using-azure-container-registry-tasks"></a>Hızlı Başlangıç: Derleme ve kapsayıcı görüntüsü Azure Container kayıt defteri görevleri kullanarak çalıştırma

Bu hızlı başlangıçta, Azure kapsayıcı kayıt defteri görevler komutları hızlıca oluşturun, gönderin ve bir Docker kapsayıcı görüntüsü yerel olarak "İç döngü" geliştirme döngünüzün buluta yük boşaltma gösteren Azure içinde çalıştırmak için kullanın. [ACR görevleri] [ container-registry-tasks-overview] yönetmek ve kapsayıcı görüntüleri kapsayıcı yaşam döngüsünün tamamında değiştirmenize yardımcı olmak için Azure Container Registry özeliklerin paketidir. 

Bu hızlı başlangıçta sonra ACR görevleri daha gelişmiş özelliklerini keşfedin. ACR görevler, kod tamamlama veya temel görüntü güncelleştirmeleri temel görüntü oluşturmayı otomatikleştirme veya diğer senaryolar arasında paralel olarak birden çok kapsayıcı test edin. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap][azure-account] oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bu hızlı başlangıcı tamamlamak için Azure Cloud Shell veya yerel bir Azure CLI yüklemesi kullanabilirsiniz. Yerel olarak 2.0.58 sürümü kullanmak istiyorsanız veya üzeri önerilir olur. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kapsayıcı kayıt defteri zaten yoksa, önce bir kaynak grubu oluşturun [az grubu oluşturma] [ az-group-create] komutu. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

Kullanarak bir kapsayıcı kayıt defteri oluşturma [az ACT create] [ az-acr-create] komutu. Kaynak defteri adı Azure’da benzersiz olmalı ve 5-50 arası alfasayısal karakter içermelidir. Aşağıdaki örnekte, *myContainerRegistry008* kullanılır. Bunu benzersiz bir değerle güncelleştirin.

```azurecli-interactive
az acr create --resource-group myResourceGroup --name myContainerRegistry008 --sku Basic
```

Bu örnekte bir *temel* kayıt defteri, bir Azure Container Registry hakkında öğrenme geliştiriciler için maliyet açısından iyileştirilmiş seçeneği. Kullanılabilir hizmet katmanları hakkında daha fazla bilgi için bkz: [kapsayıcı kayıt defteri SKU'ları][container-registry-skus].

## <a name="build-an-image-from-a-dockerfile"></a>Bir Dockerfile bir görüntü oluşturun

Artık bir görüntüsünü oluşturmak için Azure Container Registry'yi kullanın. İlk olarak, bir çalışma dizini oluşturmak ve adlı bir Dockerfile oluşturup *Dockerfile* aşağıdaki içeriğe sahip. Bu bir Linux kapsayıcı görüntünüzü oluşturmak için basit bir örnektir, ancak standart Dockerfile oluşturun ve diğer platformlar için görüntü oluşturma.

```bash
echo FROM hello-world > Dockerfile
```

Çalıştırma [az acr build] [ az-acr-build] görüntüsünü oluşturmak için komutu. Başarılı bir şekilde yapılandırıldığında, görüntünün kayıt defterinize itilir. Aşağıdaki örnek gönderim `sample/hello-world:v1` görüntü. `.` Komutu sonunda Dockerfile konumunu bu durumda geçerli dizini ayarlar.

```azurecli-interactive
az acr build --image sample/hello-world:v1 --registry myContainerRegistry008 --file Dockerfile . 
```

Başarılı derleme ve anında iletme çıktısı aşağıdakine benzer:

```console
Packing source code into tar to upload...
Uploading archived source code from '/tmp/build_archive_b0bc1e5d361b44f0833xxxx41b78c24e.tar.gz'...
Sending context (1.856 KiB) to registry: mycontainerregistry008...
Queued a build with ID: ca8
Waiting for agent...
2019/03/18 21:56:57 Using acb_vol_4c7ffa31-c862-4be3-xxxx-ab8e615c55c4 as the home volume
2019/03/18 21:56:57 Setting up Docker configuration...
2019/03/18 21:56:58 Successfully set up Docker configuration
2019/03/18 21:56:58 Logging in to registry: mycontainerregistry008.azurecr.io
2019/03/18 21:56:59 Successfully logged into mycontainerregistry008.azurecr.io
2019/03/18 21:56:59 Executing step ID: build. Working directory: '', Network: ''
2019/03/18 21:56:59 Obtaining source code and scanning for dependencies...
2019/03/18 21:57:00 Successfully obtained source code and scanned for dependencies
2019/03/18 21:57:00 Launching container with name: build
Sending build context to Docker daemon  13.82kB
Step 1/1 : FROM hello-world
latest: Pulling from library/hello-world
Digest: sha256:2557e3c07ed1e38f26e389462d03ed943586fxxxx21577a99efb77324b0fe535
Successfully built fce289e99eb9
Successfully tagged mycontainerregistry008.azurecr.io/sample/hello-world:v1
2019/03/18 21:57:01 Successfully executed container: build
2019/03/18 21:57:01 Executing step ID: push. Working directory: '', Network: ''
2019/03/18 21:57:01 Pushing image: mycontainerregistry008.azurecr.io/sample/hello-world:v1, attempt 1
The push refers to repository [mycontainerregistry008.azurecr.io/sample/hello-world]
af0b15c8625b: Preparing
af0b15c8625b: Layer already exists
v1: digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a size: 524
2019/03/18 21:57:03 Successfully pushed image: mycontainerregistry008.azurecr.io/sample/hello-world:v1
2019/03/18 21:57:03 Step ID: build marked as successful (elapsed time in seconds: 2.543040)
2019/03/18 21:57:03 Populating digests for step ID: build...
2019/03/18 21:57:05 Successfully populated digests for step ID: build
2019/03/18 21:57:05 Step ID: push marked as successful (elapsed time in seconds: 1.473581)
2019/03/18 21:57:05 The following dependencies were found:
2019/03/18 21:57:05
- image:
    registry: mycontainerregistry008.azurecr.io
    repository: sample/hello-world
    tag: v1
    digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/hello-world
    tag: v1
    digest: sha256:2557e3c07ed1e38f26e389462d03ed943586f744621577a99efb77324b0fe535
  git: {}

Run ID: ca8 was successful after 10s
```

## <a name="run-the-image"></a>Görüntü çalıştırma

Artık yerleşik ve kayıt defterinize gönderdiniz görüntü hızlıca çalıştırın. Kapsayıcı geliştirme iş akışınızda görüntüsünü dağıtmadan önce bir doğrulama adım bu olabilir.

Bir dosya oluşturun *quickrun.yaml* tek bir adım için aşağıdaki içeriğe sahip bir yerel çalışma dizinindeki. İçin kayıt defterinizin oturum açma sunucusu adını yerine  *\<acrLoginServer\>*. Oturum açma sunucusunun adı şu biçimdedir  *\<kayıt defteri adı\>. azurecr.io* (tamamı küçük harflerle), örneğin, *mycontainerregistry008.azurecr.io*. Bu örnekte oluşturulan ve gönderilen varsayılır `sample/hello-world:v1` görüntü önceki bölümde:

```yml
steps:
  - cmd: <acrLoginServer>/sample/hello-world:v1
```

`cmd` Adım Bu örnekte, varsayılan yapılandırmasında, kapsayıcı çalışır ancak `cmd` desteklediği ek `docker run` parametreleri veya hatta diğer `docker` komutları.

Kapsayıcı ile aşağıdaki komutu çalıştırın:

```azurecli-interactive
az acr run --registry myContainerRegistry008 --file quickrun.yaml .
```

Çıktı aşağıdakine benzer:

```console
Packing source code into tar to upload...
Uploading archived source code from '/tmp/run_archive_ebf74da7fcb04683867b129e2ccad5e1.tar.gz'...
Sending context (1.855 KiB) to registry: mycontainerre...
Queued a run with ID: cab
Waiting for an agent...
2019/03/19 19:01:53 Using acb_vol_60e9a538-b466-475f-9565-80c5b93eaa15 as the home volume
2019/03/19 19:01:53 Creating Docker network: acb_default_network, driver: 'bridge'
2019/03/19 19:01:53 Successfully set up Docker network: acb_default_network
2019/03/19 19:01:53 Setting up Docker configuration...
2019/03/19 19:01:54 Successfully set up Docker configuration
2019/03/19 19:01:54 Logging in to registry: mycontainerregistry008.azurecr.io
2019/03/19 19:01:55 Successfully logged into mycontainerregistry008.azurecr.io
2019/03/19 19:01:55 Executing step ID: acb_step_0. Working directory: '', Network: 'acb_default_network'
2019/03/19 19:01:55 Launching container with name: acb_step_0

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

2019/03/19 19:01:56 Successfully executed container: acb_step_0
2019/03/19 19:01:56 Step ID: acb_step_0 marked as successful (elapsed time in seconds: 0.843801)

Run ID: cab was successful after 6s
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse [az grubu Sil] [ az-group-delete] kaynak grubunu, kapsayıcı kayıt defteri ve burada depolanan kapsayıcı görüntülerini kaldırmak için komutu.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, ACR görevleri özelliklerini hızlıca oluşturun, gönderin ve bir Docker kapsayıcı görüntüsü Azure içinde yerel olarak çalıştırmak için kullanılır. ACR görevleri görüntüsü derleme ve güncelleştirmeleri otomatik hale getirmek için kullanma hakkında bilgi edinmek için Azure Container Registry öğreticileri devam edin.

> [!div class="nextstepaction"]
> [Azure Container Registry öğreticileri][container-registry-tutorial-quick-task]

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-pull]: https://docs.docker.com/engine/reference/commandline/pull/
[docker-rmi]: https://docs.docker.com/engine/reference/commandline/rmi/
[docker-run]: https://docs.docker.com/engine/reference/commandline/run/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/
[azure-account]: https://azure.microsoft.com/free/

<!-- LINKS - internal -->
[az-acr-create]: /cli/azure/acr#az-acr-create
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[azure-cli]: /cli/azure/install-azure-cli
[container-registry-tasks-overview]: container-registry-tasks-overview.md
[container-registry-tutorial-quick-task]: container-registry-tutorial-quick-task.md
[container-registry-skus]: container-registry-skus.md
[azure-cli-install]: /cli/azure/install-azure-cli
