---
title: Hızlı Başlangıç - Azure - Azure portalı özel bir Docker kayıt defteri oluşturma
description: Azure portalıyla hızlıca özel bir Docker kapsayıcısı kayıt defteri oluşturmayı öğrenin.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: quickstart
ms.date: 01/22/2019
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: f41d51981c4da9ee089282da8b8d4cc5f37a4aed
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60827631"
---
# <a name="quickstart-create-a-private-container-registry-using-the-azure-portal"></a>Hızlı Başlangıç: Azure portalını kullanarak bir özel kapsayıcı kayıt defteri oluşturma

Azure kapsayıcı kayıt defteri, Azure’da özel Docker kapsayıcısı görüntülerinizi depolayıp yönetebileceğiniz özel bir Docker kayıt defteridir. Bu hızlı başlangıçta, Azure portalıyla bir kapsayıcı kayıt defteri oluşturursunuz. Sonra kayıt defterine bir kapsayıcı görüntüsü göndermeye Docker komutlarını kullanın ve son olarak çekmek ve görüntüyü kayıt defterinizden çalıştırın.

Kapsayıcı görüntüleri ile çalışmak için kayıt defterinde oturum açmak için bu hızlı başlangıçta Azure CLI'yı çalıştıran gerektirir (2.0.55 sürümü veya üzeri önerilir). Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli].

Ayrıca sisteminizde yerel olarak Docker yüklü olması gerekir. Docker [Mac][docker-mac], [Windows][docker-windows] veya [Linux][docker-linux]'ta Docker'ı kolayca yapılandırmanızı sağlayan paketler sağlar.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

**Kaynak oluştur** > **Kapsayıcılar** > **Container Registry**'yi seçin.

![Azure portalında kapsayıcı kayıt defteri oluşturma][qs-portal-01]

**Kaynak Defteri Adı** ve **Kaynak grubu** değerlerini girin. Kaynak defteri adı Azure’da benzersiz olmalı ve 5-50 arası alfasayısal karakter içermelidir. Bu hızlı başlangıçta `West US` konumunda `myResourceGroup` adlı yeni bir kaynak grubu oluşturun ve **SKU** olarak ‘Temel’ seçeneğini belirleyin. **Oluştur**’u seçerek ACR örneğini dağıtın.

![Azure portalında kapsayıcı kayıt defteri oluşturma][qs-portal-03]

Bu hızlı başlangıçta oluşturduğunuz bir *temel* maliyet açısından iyileştirilmiş bir seçenek Azure Container Registry hakkında öğrenme geliştiriciler için kayıt defteri. Kullanılabilir hizmet katmanları hakkında daha fazla bilgi için bkz: [kapsayıcı kayıt defteri SKU'ları][container-registry-skus].

Zaman **dağıtım başarılı** iletisi göründüğünde, portalda kapsayıcı kayıt defteri seçin. 

![Azure portalındaki kapsayıcı kayıt defteri genel bakış][qs-portal-05]

Değerini not **oturum açma sunucusu**. Aşağıdaki adımlarda Azure CLI ve Docker ile kayıt defteriniz üzerinde çalışırken bu değeri kullanın.

## <a name="log-in-to-registry"></a>Kayıt defterinde oturum açma

Kapsayıcı görüntülerini gönderip çekmeden önce ACR örneğinde oturum açmalısınız. İşletim sisteminizde bir komut kabuğunu açın ve kullanmak [az acr oturum açma] [ az-acr-login] Azure CLI komutu.

```azurecli
az acr login --name <acrName>
```

Bu komut tamamlandığında `Login Succeeded` döndürülür. 

[!INCLUDE [container-registry-quickstart-docker-push](../../includes/container-registry-quickstart-docker-push.md)]

## <a name="list-container-images"></a>Kapsayıcı görüntülerini listeleme

Kayıt defterinizde seçin ve portal gidin, kayıt defterindeki görüntüleri listelemek için **depoları**, ardından ile oluşturduğunuz depoyu seçin `docker push`.

Bu örnekte, seçiyoruz **Merhaba-Dünya** depo ve biz görebilirsiniz `v1`-Etiketli Resim altında **ETİKETLERİ**.

![Azure portalında kapsayıcı görüntülerini listeleme][qs-portal-09]

[!INCLUDE [container-registry-quickstart-docker-pull](../../includes/container-registry-quickstart-docker-pull.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynaklarınızı temizlemek için gidin **myResourceGroup** portalında kaynak grubu. Kaynak grubu yüklendikten sonra tıklayarak **kaynak grubunu Sil** kaynak grubunu, kapsayıcı kayıt defteri ve kapsayıcı görüntülerini kaldırmak için depolanan vardır.

![Azure portalında kaynak grubunu silme][qs-portal-08]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure portalı ile bir Azure Container Registry oluşturdunuz, bir kapsayıcı görüntüsü gönderdiniz çekilir ve kayıt defterinden görüntü çalıştı. ACR derin göz atmak için Azure Container Registry öğreticilere geçin.

> [!div class="nextstepaction"]
> [Azure Container Registry öğreticileri][container-registry-tutorial-quick-task]

<!-- IMAGES -->
[qs-portal-01]: ./media/container-registry-get-started-portal/qs-portal-01.png
[qs-portal-02]: ./media/container-registry-get-started-portal/qs-portal-02.png
[qs-portal-03]: ./media/container-registry-get-started-portal/qs-portal-03.png
[qs-portal-05]: ./media/container-registry-get-started-portal/qs-portal-05.png
[qs-portal-08]: ./media/container-registry-get-started-portal/qs-portal-08.png
[qs-portal-09]: ./media/container-registry-get-started-portal/qs-portal-09.png

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-pull]: https://docs.docker.com/engine/reference/commandline/pull/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-rmi]: https://docs.docker.com/engine/reference/commandline/rmi/
[docker-run]: https://docs.docker.com/engine/reference/commandline/run/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[container-registry-tutorial-quick-task]: container-registry-tutorial-quick-task.md
[container-registry-skus]: container-registry-skus.md
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-login]: /cli/azure/acr#az-acr-login
