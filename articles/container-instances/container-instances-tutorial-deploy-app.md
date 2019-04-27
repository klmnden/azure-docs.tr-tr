---
title: Öğretici - Azure Container Instances'a kapsayıcı uygulaması dağıtma
description: Azure Container Instances Öğreticisi bölüm 3 / 3 - Azure Container ınstances'a kapsayıcı uygulaması dağıtma
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: tutorial
ms.date: 03/21/2018
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: 210254a4404a5280e326bf40057331a784ff6148
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60684225"
---
# <a name="tutorial-deploy-a-container-application-to-azure-container-instances"></a>Öğretici: Bir Azure Container ınstances'a kapsayıcı uygulaması dağıtma

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

Bir özel kapsayıcı kayıt defterinde oluşturulana benzer barındırılan bir görüntüyü dağıtırken [ikinci öğreticide](container-instances-tutorial-prepare-acr.md), kayıt defterine erişim için kimlik bilgilerini sağlamanız gerekir. Gösterildiği [Azure Container Registry'den Azure Container Instances ile kimlik doğrulama](../container-registry/container-registry-auth-aci.md), birçok senaryo için en iyi uygulama oluşturma ve bir Azure Active Directory Hizmet sorumlusu ile yapılandırmaktır *çekme*kayıt izinleri. Bu makalede gerekli izinlere sahip bir hizmet sorumlusu oluşturmak için örnek betikler için bkz. Hizmet sorumlusu kimliği ve hizmet sorumlusu parolası not alın. Kapsayıcının dağıtımını gerçekleştirdiğinizde bu kimlik bilgilerini kullanın.

Ayrıca kapsayıcı kayıt defteri oturum açma sunucusunun tam adı gerekir (Değiştir `<acrName>` kayıt defterinizin adıyla):

```azurecli
az acr show --name <acrName> --query loginServer
```

### <a name="deploy-container"></a>Kapsayıcıyı dağıtma

Şimdi kapsayıcıyı dağıtmak için [az container create][az-container-create] komutunu kullanın. Değiştirin `<acrLoginServer>` önceki komutta aldığınız değerine sahip. Değiştirin `<service-principal-ID>` ve `<service-principal-password>` hizmet sorumlusu kimliği ve kayıt defterine erişim için oluşturduğunuz parola ile. Değiştirin `<aciDnsLabel>` istenen bir DNS adına sahip.

```azurecli
az container create --resource-group myResourceGroup --name aci-tutorial-app --image <acrLoginServer>/aci-tutorial-app:v1 --cpu 1 --memory 1 --registry-login-server <acrLoginServer> --registry-username <service-principal-ID> --registry-password <service-principal-password> --dns-name-label <aciDnsLabel> --ports 80
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

Örneğin:
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
[az-container-create]: /cli/azure/container#az-container-create
[az-container-show]: /cli/azure/container#az-container-show
[az-group-delete]: /cli/azure/group#az-group-delete
[azure-cli-install]: /cli/azure/install-azure-cli
[prepare-app]: ./container-instances-tutorial-prepare-app.md
