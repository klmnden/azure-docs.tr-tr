---
title: 'Hızlı Başlangıç: Azure portalıyla Azure’da özel bir Docker kayıt defteri oluşturun'
description: Azure portalıyla hızlıca özel bir Docker kapsayıcısı kayıt defteri oluşturmayı öğrenin.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: quickstart
ms.date: 03/03/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 55da52e0d314c353c669c56ad918c4dd6bef44c3
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32169814"
---
# <a name="quickstart-create-a-container-registry-using-the-azure-portal"></a>Hızlı başlangıç: Azure portalını kullanarak kapsayıcı kayıt defteri oluşturma

Azure kapsayıcı kayıt defteri, Azure’da özel Docker kapsayıcısı görüntülerinizi depolayıp yönetebileceğiniz özel bir Docker kayıt defteridir. Bu hızlı başlangıçta Azure portalını kullanarak bir kapsayıcı kayıt defteri oluşturacak, kayıt defterine bir kapsayıcı görüntüsü gönderecek ve son olarak kapsayıcıyı kayıt defterinizden Azure Container Instances’a (ACI) dağıtacaksınız.

Bu hızlı başlangıcı tamamlayabilmeniz için Docker yerel olarak yüklü olmalıdır. Docker [Mac][docker-mac], [Windows][docker-windows] veya [Linux][docker-linux]'ta Docker'ı kolayca yapılandırmanızı sağlayan paketler sağlar.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

**Kaynak oluştur** > **Kapsayıcılar** > **Azure Container Registry** seçeneklerini belirleyin.

![Azure portalında kapsayıcı kayıt defteri oluşturma][qs-portal-01]

**Kaynak Defteri Adı** ve **Kaynak grubu** değerlerini girin. Kaynak defteri adı Azure’da benzersiz olmalı ve 5-50 arası alfasayısal karakter içermelidir. `myResourceGroup` adlı yeni bir kaynak grubu oluşturun ve **SKU** için 'Temel'i seçin. **Oluştur**’u seçerek ACR örneğini dağıtın.

![Azure portalında kapsayıcı kayıt defteri oluşturma][qs-portal-03]

Bu hızlı başlangıçta *Temel* kayıt defteri oluşturulur. Azure Container Registry, aşağıdaki tabloda kısaca açıklandığı gibi çeşitli SKU’lar altında sunulur. Her biriyle ilgili ayrıntılı bilgi edinmek için bkz. [Kapsayıcı kayıt defteri SKU'ları][container-registry-skus].

[!INCLUDE [container-registry-sku-matrix](../../includes/container-registry-sku-matrix.md)]

**Dağıtım başarılı** iletisi göründüğünde, portalda kapsayıcı kayıt defterini seçip **Erişim anahtarları**’nı seçin.

![Azure portalında kapsayıcı kayıt defteri oluşturma][qs-portal-05]

**Yönetici kullanıcı** altında **Etkinleştir**’i seçin. Aşağıdaki değerleri not alın:

* Oturum açma sunucusu
* Kullanıcı adı
* password

Bu değerleri aşağıdaki adımlarda, Docker CLI ile kayıt defteriniz üzerinde çalışırken kullanırsınız.

![Azure portalında kapsayıcı kayıt defteri oluşturma][qs-portal-06]

## <a name="log-in-to-acr"></a>ACR üzerinde oturum açın

Kapsayıcı görüntülerini gönderip çekmeden önce ACR örneğinde oturum açmalısınız. Bunu yapmak için [docker login][docker-login] komutunu kullanın. *username*, *password* ve *login server* değerlerini bir önceki adımda not aldığınız değerlerle değiştirin.

```bash
docker login --username <username> --password <password> <login server>
```

Bu komut tamamlandığında `Login Succeeded` döndürülür. `--password-stdin` parametresinin kullanılmasını öneren bir güvenlik uyarısı da görebilirsiniz. Bunun kullanımı bu makalenin kapsamında olmasa da bu en iyi yöntemin izlenmesi önerilir. Daha fazla bilgi edinmek için [docker login][docker-login] komut başvurusuna bakın.

## <a name="push-image-to-acr"></a>Görüntüyü ACR’ye gönderme

Azure Container Registry’nize görüntü gönderebilmeniz için önce bir görüntünüz olmalıdır. Gerekirse aşağıdaki komutu çalıştırarak Docker Hub’dan mevcut bir görüntüyü çekin.

```bash
docker pull microsoft/aci-helloworld
```

Görüntüyü kayıt defterinize göndermeden önce ACR oturum açma sunucusunun adıyla etiketlemeniz gerekir. Görüntüyü [docker tag][docker-tag] komutunu kullanarak etiketleyin. *login server* değerini daha önce kaydettiğiniz oturum açma sunucusu adıyla değiştirin.

```bash
docker tag microsoft/aci-helloworld <login server>/aci-helloworld:v1
```

Son olarak, [docker push][docker-push] komutunu kullanarak görüntüyü ACR örneğine gönderin. *login server* değerini ACR örneğinizin sunucu adıyla değiştirin.

```bash
docker push <login server>/aci-helloworld:v1
```

Başarılı bir `docker push` komutunun çıktısı şuna benzer:

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

ACR örneğinizdeki görüntüleri listelemek için portalda kayıt defterinize gidin ve **Depolar**’ı seçip `docker push` ile oluşturduğunuz depoyu seçin.

Bu örnekte **aci-helloworld** deposunu seçiyoruz ve **TAGS** altında `v1` etiketli görüntüyü görebiliyoruz.

![Azure portalında kapsayıcı kayıt defteri oluşturma][qs-portal-09]

## <a name="deploy-image-to-aci"></a>Görüntüyü ACI’ya dağıtma

Bir örneği kayıt defterinden dağıtmak için depoya (aci-helloworld) gitmemiz ve sonra v1 ifadesinin yanındaki üç noktaya tıklamamız gerekir.

![Portaldan bir Azure Container Örneği başlatma][qs-portal-10]

Açılır menü görüntülendiğinde **Örneği çalıştır**’ı seçin:

![ACI açılır menüsünü başlatma][qs-portal-11]

**Kapsayıcı adı**’nı doldurun, doğru aboneliğin seçildiğinden emin olun, mevcut **Kaynak grubu**: "myResourceGroup" öğesini seçin ve ardından **Tamam**’a tıklayarak Azure Container Örneğini başlatın.

![ACI dağıtım seçeneklerini başlatma][qs-portal-12]

Dağıtım başlatıldığında, dağıtım ilerlemesini gösteren bir kutucuk portal panonuza yerleştirilir. Dağıtım tamamlandığında, kutucuk yeni **mycontainer** kapsayıcı grubunuzu göstermek için güncelleştirilir.

![ACI dağıtım durumu][qs-portal-13]

Kapsayıcı grubu özelliklerini görüntülemek için mycontainer kapsayıcı grubunu seçin. Kapsayıcı grubunun **IP adresi** ve kapsayıcının **STATUS** değerini not edin.

![ACI kapsayıcı ayrıntıları][qs-portal-14]

## <a name="view-the-application"></a>Uygulamayı görüntüleme

Kapsayıcı **Çalışıyor** durumuna geçtikten sonra uygulamayı görüntülemek için sık kullandığınız tarayıcıdan önceki adımda not ettiğiniz IP adresine gidin.

![Tarayıcıdaki Merhaba Dünya uygulaması][qs-portal-15]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynaklarınızı temizlemek için portaldaki **myResourceGroup** kaynak grubuna gidin. Kaynak grubu yüklendikten sonra **Kaynak grubunu sil**’e tıklayarak kaynak grubunu, Azure Container Registry’yi ve tüm Azure Container Örneklerini kaldırın.

![Azure portalında kapsayıcı kayıt defteri oluşturma][qs-portal-08]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure CLI ile bir Azure Container Registry oluşturdunuz ve Azure Container Instances üzerinden bir örneğini başlattınız. ACI’ya daha ayrıntılı bir bakış için Azure Container Instances öğreticisine geçin.

> [!div class="nextstepaction"]
> [Azure Container Instances öğreticileri][container-instances-tutorial-prepare-app]

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
[qs-portal-10]: ./media/container-registry-get-started-portal/qs-portal-10.png
[qs-portal-11]: ./media/container-registry-get-started-portal/qs-portal-11.png
[qs-portal-12]: ./media/container-registry-get-started-portal/qs-portal-12.png
[qs-portal-13]: ./media/container-registry-get-started-portal/qs-portal-13.png
[qs-portal-14]: ./media/container-registry-get-started-portal/qs-portal-14.png
[qs-portal-15]: ./media/container-registry-get-started-portal/qs-portal-15.png

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[container-instances-tutorial-prepare-app]: ../container-instances/container-instances-tutorial-prepare-app.md
[container-registry-skus]: container-registry-skus.md
