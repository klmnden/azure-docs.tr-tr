---
title: "Hızlı Başlangıç - Azure portalı ile özel bir Docker kayıt defteri oluşturma"
description: "Azure portalı ile özel bir Docker kapsayıcısı kayıt defteri oluşturmak hızlı bir şekilde öğrenin."
services: container-registry
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
tags: 
keywords: 
ms.assetid: 53a3b3cb-ab4b-4560-bc00-366e2759f1a1
ms.service: container-registry
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/31/2017
ms.author: marsma
ms.custom: 
ms.openlocfilehash: 514fa3490e480647f0923c99bd9606a3726d4d30
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="create-a-container-registry-using-the-azure-portal"></a>Azure portalını kullanarak kapsayıcı kayıt defteri oluşturma

Azure kapsayıcı kayıt defteri sakladığınız ve özel Docker kapsayıcısı görüntülerinizi yönetmek Azure bir özel Docker kayıt defteri ' dir. Bu hızlı başlangıç Azure portalıyla bir kapsayıcı kayıt defteri oluşturun.

Bu hızlı başlangıç tamamlamak için Docker yerel olarak yüklü olması gerekir. Docker, [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) veya [Linux](https://docs.docker.com/engine/installation/#supported-platforms)’ta Docker’ı kolayca yapılandırmanızı sağlayan paketler sağlar.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Https://portal.azure.com Azure portalında oturum açın.

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

Seçin **yeni** > **kapsayıcıları** > **Azure kapsayıcı kayıt defteri**.

![Azure portalında bir kapsayıcı kayıt defteri oluşturma][qs-portal-01]

İçin değerler girin **kayıt defteri adı** ve **kaynak grubu**. Kayıt defteri adı Azure içinde benzersiz olması ve 5-50 alfasayısal karakterler içermelidir. Adlı yeni bir kaynak grubu oluştur `myResourceGroup`ve **SKU**, 'Temel' seçin. Seçin **oluşturma** ACR örneği dağıtmak için.

![Azure portalında bir kapsayıcı kayıt defteri oluşturma][qs-portal-03]

Bu hızlı başlangıç oluşturuyoruz bir *temel* kayıt defteri. Kısaca aşağıdaki tabloda açıklanan birkaç farklı SKU'ları, Azure kapsayıcı kayıt defteri mevcuttur. Her genişletilmiş Ayrıntılar için bkz [kapsayıcı kayıt defteri SKU'ları](container-registry-skus.md).

[!INCLUDE [container-registry-sku-matrix](../../includes/container-registry-sku-matrix.md)]

Zaman **dağıtım başarılı** iletisi görüntülenirse, kapsayıcı kayıt Portalı'nda seçin ve ardından seçin **erişim anahtarları**.

![Azure portalında bir kapsayıcı kayıt defteri oluşturma][qs-portal-05]

Altında **yönetici kullanıcı**seçin **etkinleştirmek**. Aşağıdaki değerleri not edin:

* Oturum açma sunucusu
* Kullanıcı adı
* password

Aşağıdaki adımlarda, kayıt defteri Docker CLI ile birlikte çalışırken bu değerleri kullanın.

![Azure portalında bir kapsayıcı kayıt defteri oluşturma][qs-portal-06]

## <a name="log-in-to-acr"></a>ACR için oturum açın

Kapsayıcı görüntülerini gönderip çekmeden önce ACR örneğinde oturum açmalısınız. Bunu yapmak için kullanın [docker oturum açma](https://docs.docker.com/engine/reference/commandline/login/) komutu. Değiştir *kullanıcıadı*, *parola*, ve *oturum açma sunucusu* olanlar önceki adımda not ettiğiniz değerlerle.

```bash
docker login --username <username> --password <password> <login server>
```

Komut döndürür `Login Succeeded` tamamlandıktan sonra. Ayrıca, kullanımını öneren bir güvenlik uyarısı görebilirsiniz `--password-stdin` parametresi. Kullanımı, bu makalenin kapsamı dışında olsa da, bu en iyi yöntem öneririz. Bkz: [docker oturum açma](https://docs.docker.com/engine/reference/commandline/login/) daha fazla bilgi için başvuru komutu.

## <a name="push-image-to-acr"></a>ACR itme görüntüye

Görüntü, Azure kapsayıcı kayıt defterine göndermek için ilk görüntüsü olması gerekir. Gerekirse, varolan bir görüntü Docker hub'dan çıkarmak için aşağıdaki komutu çalıştırın.

```bash
docker pull microsoft/aci-helloworld
```

Görüntü, kayıt defterine sokmadan önce ACR oturum açma sunucusu ada sahip bir görüntü etiketlemeniz gerekir. Kullanarak resim etiketi [docker etiketi](https://docs.docker.com/engine/reference/commandline/tag/) komutu. Değiştir *oturum açma sunucusu* daha önce kaydettiğiniz oturum açma sunucu adına sahip.

```
docker tag microsoft/aci-helloworld <login server>/aci-helloworld:v1
```

Son olarak, [docker itme](https://docs.docker.com/engine/reference/commandline/push/) ACR örneğine görüntü göndermek için. Değiştir *oturum açma sunucusu* ACR örneğinizi oturum açma sunucusu adı.

```
docker push <login server>/aci-helloworld:v1
```

Başarılı bir çıktı `docker push` komutu benzer:

```
The push refers to a repository [uniqueregistryname.azurecr.io/aci-helloworld]
7c701b1aeecd: Pushed
c4332f071aa2: Pushed
0607e25cc175: Pushed
d8fbd47558a8: Pushed
44ab46125c35: Pushed
5bef08742407: Pushed
v1: digest: sha256:f2867748615cc327d31c68b1172cc03c0544432717c4d2ba2c1c2d34b18c62ba size: 1577
```

## <a name="list-container-images"></a>Kapsayıcı görüntülerini listeleme

ACR örneğinizi görüntülerinde listelemek için kayıt defterinde seçin ve portal gidin **depoları**, oluşturduğunuz havuzu seçin `docker push`.

Bu örnekte, biz seçin **aci helloworld** depo ve biz görebilir `v1`-altında Etiketli Resim **ETİKETLERİ**.

![Azure portalında bir kapsayıcı kayıt defteri oluşturma][qs-portal-09]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerektiğinde silme **myResourceGroup** kaynak grubu. Bunun yapılması, kaynak grubu, ACR örneği ve tüm kapsayıcı görüntüleri siler.

![Azure portalında bir kapsayıcı kayıt defteri oluşturma][qs-portal-08]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç Azure CLI ile Azure kapsayıcı kayıt defteri oluşturuldu. Azure kapsayıcı kayıt defteri Azure kapsayıcı örnekleri ile kullanmak istiyorsanız, Azure kapsayıcı örnekleri Öğreticisine devam edin.

> [!div class="nextstepaction"]
> [Azure Container Instances öğreticileri](../container-instances/container-instances-tutorial-prepare-app.md)

<!-- IMAGES -->
[qs-portal-01]: ./media/container-registry-get-started-portal/qs-portal-01.png
[qs-portal-02]: ./media/container-registry-get-started-portal/qs-portal-02.png
[qs-portal-03]: ./media/container-registry-get-started-portal/qs-portal-03.png
[qs-portal-04]: ./media/container-registry-get-started-portal/qs-portal-04.png
[qs-portal-05]: ./media/container-registry-get-started-portal/qs-portal-05.png
[qs-portal-06]: ./media/container-registry-get-started-portal/qs-portal-06.png
[qs-portal-07]: ./media/container-registry-get-started-portal/qs-portal-07.png
[qs-portal-08]: ./media/container-registry-get-started-portal/qs-portal-08.png
[qs-portal-09]: ./media/container-registry-get-started-portal/qs-portal-09.png