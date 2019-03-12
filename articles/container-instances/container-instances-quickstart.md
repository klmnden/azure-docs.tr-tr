---
title: Hızlı Başlangıç - Azure Container Instances'a - CLI Docker kapsayıcısı dağıtma
description: Bu hızlı başlangıçta, bir yalıtılmış bir Azure kapsayıcı örneğinde çalışan kapsayıcılı web uygulaması hızlı bir şekilde dağıtmak için Azure CLI'yı kullanın
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: quickstart
ms.date: 10/02/2018
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: 7252636287d634927979d70954f48cab5aecde5d
ms.sourcegitcommit: 1902adaa68c660bdaac46878ce2dec5473d29275
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2019
ms.locfileid: "57732281"
---
# <a name="quickstart-deploy-a-container-instance-in-azure-using-the-azure-cli"></a>Hızlı Başlangıç: Azure CLI kullanarak azure'da bir kapsayıcı örneği dağıtma

Azure Container Instances, kolay ve hızlı ile Azure'da sunucusuz Docker kapsayıcıları çalıştırmak için kullanın. Azure Kubernetes hizmeti gibi bir tam kapsayıcı düzenleme platformunu ihtiyacınız kalmadığında bir kapsayıcı örneği isteğe bağlı bir uygulamayı dağıtın.

Bu hızlı başlangıçta, yalıtılmış bir Docker kapsayıcısı dağıtma ve uygulamayı bir tam etki alanı adı (FQDN) ile kullanılabilir hale getirmek için Azure CLI'yı kullanın. Bir tek dağıtım komutu yürüttükten sonra birkaç saniye kapsayıcıda çalışan uygulamaya göz atabilirsiniz:

![Azure Container Instances hizmetine dağıtılmış uygulamanın tarayıcıdaki görüntüsü][aci-app-browser]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap][azure-account] oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bu hızlı başlangıcı tamamlamak için Azure Cloud Shell veya yerel bir Azure CLI yüklemesi kullanabilirsiniz. Yerel olarak 2.0.55 sürümü kullanmak istiyorsanız veya üzeri önerilir olur. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Tüm Azure kaynakları gibi Azure kapsayıcı örneklerinin de bir kaynak grubuna dağıtılması gerekir. Kaynak grupları, ilgili Azure kaynaklarını düzenlemenizi ve yönetmenizi sağlar.

İlk olarak aşağıdaki [az group create][az-group-create] komutunu kullanarak *eastus* bölgesinde *myResourceGroup* adlı bir kaynak grubu oluşturun:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Artık bir kaynak grubuna sahip olduğunuza göre Azure'da kapsayıcı çalıştırabilirsiniz. Azure CLI ile kapsayıcı örneği oluşturmak için [az container create][az-container-create] komutunda bir kaynak grubu adı, kapsayıcı örneği adı ve Docker kapsayıcı görüntüsü belirtin. Bu hızlı başlangıçta, genel kullandığınız `microsoft/aci-helloworld` görüntü. Bu görüntü, statik bir HTML sayfası görevi görür node.js'de yazılmış küçük bir web uygulamasını paketler.

Açılacak bir veya daha fazla bağlantı noktası, DNS ad etiketi ya da ikisini birden belirterek kapsayıcılarınızı internete açabilirsiniz. Bu hızlı başlangıçta, web uygulaması genel olarak erişilebilir olması bir DNS ad etiketi içeren bir kapsayıcıya dağıtın.

Bir kapsayıcı örneği başlatmak için aşağıdaki komutu yürütün. Ayarlanmış bir `--dns-name-label` örneği oluşturduğunuz Azure bölgesi içinde benzersiz olan değer. "DNS ad etiketi kullanılamıyor" hata iletisiyle karşılaşırsanız farklı bir DNS ad etiketi deneyin.

```azurecli-interactive
az container create --resource-group myResourceGroup --name mycontainer --image microsoft/aci-helloworld --dns-name-label aci-demo --ports 80
```

Birkaç saniye içinde Azure CLI'den dağıtımın tamamlandığını belirten bir yanıt almanız gerekir. Durumunu [az container show][az-container-show] komutuyla denetleyebilirsiniz:

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" --out table
```

Komutu çalıştırdığınızda, kapsayıcının tam etki alanı adı (FQDN) ve sağlama durumu görüntülenir.

```console
$ az container show --resource-group myResourceGroup --name mycontainer --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" --out table
FQDN                               ProvisioningState
---------------------------------  -------------------
aci-demo.eastus.azurecontainer.io  Succeeded
```

Kapsayıcının `ProvisioningState` bilgisi **Başarılı** olduğunda tarayıcınızdan FQDN adresine gidin. Aşağıdakine benzer bir web sayfası görüyorsanız kendinizi tebrik edebilirsiniz! Docker kapsayıcısında çalışan bir uygulamayı başarıyla Azure'a dağıttınız.

![Bir Azure kapsayıcı örneğinde çalışan uygulamayı gösteren tarayıcı ekran görüntüsü][aci-app-browser]

İlk seferde uygulama görüntülenmezse DNS kayıtlarının yayılması için birkaç saniye bekleyip tarayıcınızı yenilemeyi deneyebilirsiniz.

## <a name="pull-the-container-logs"></a>Kapsayıcı günlüklerini çekme

Kapsayıcıdaki veya üzerinde çalışan uygulamalardaki sorunları gidermek (veya yalnızca çıkışını görmek) istediğinizde kapsayıcı örneğinin günlüklerinden başlayın.

[az container logs][az-container-logs] komutu ile kapsayıcı örneğinin günlüklerini çekin:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer
```

Çıkış, kapsayıcının günlüklerini görüntüler ve uygulamayı tarayıcınızda görüntülediğinizde oluşturulan HTTP GET isteklerini göstermelidir.

```console
$ az container logs --resource-group myResourceGroup --name mycontainer
listening on port 80
::ffff:10.240.255.105 - - [01/Oct/2018:18:25:51 +0000] "GET / HTTP/1.0" 200 1663 "-" "-"
::ffff:10.240.255.106 - - [01/Oct/2018:18:31:04 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.100 Safari/537.36"
::ffff:10.240.255.106 - - [01/Oct/2018:18:31:04 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://aci-demo.eastus.azurecontainer.io/" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.100 Safari/537.36"
```

## <a name="attach-output-streams"></a>Çıkış akışları ekleme

Günlükleri görüntülemeye ek olarak, yerel standart çıkış ve standart hata akışlarınızı kapsayıcınınkine ekleyebilirsiniz.

Öncelikle şunu yürütün [az kapsayıcı ekleme] [ az-container-attach] kapsayıcının çıkış akışlarına kapsayıcıya yerel Konsolunuzu eklenecek komut:

```azurecli-interactive
az container attach --resource-group myResourceGroup --name mycontainer
```

Bağlandıktan sonra, ek çıkışlar oluşturmak için tarayıcınızı birkaç defa yenileyin. İşlemi tamamladığınızda `Control+C` ile konsolunuzu ayırın. Aşağıdakine benzer bir çıktı görmeniz gerekir:

```console
$ az container attach --resource-group myResourceGroup --name mycontainer
Container 'mycontainer' is in state 'Running'...
(count: 1) (last timestamp: 2018-03-15 21:17:59+00:00) pulling image "microsoft/aci-helloworld"
(count: 1) (last timestamp: 2018-03-15 21:18:05+00:00) Successfully pulled image "microsoft/aci-helloworld"
(count: 1) (last timestamp: 2018-03-15 21:18:05+00:00) Created container with id 3534a1e2ee392d6f47b2c158ce8c1808d1686fc54f17de3a953d356cf5f26a45
(count: 1) (last timestamp: 2018-03-15 21:18:06+00:00) Started container with id 3534a1e2ee392d6f47b2c158ce8c1808d1686fc54f17de3a953d356cf5f26a45

Start streaming logs:
listening on port 80
::ffff:10.240.255.105 - - [15/Mar/2018:21:18:26 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
::ffff:10.240.255.105 - - [15/Mar/2018:21:18:26 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://aci-demo.eastus.azurecontainer.io/" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
::ffff:10.240.255.107 - - [15/Mar/2018:21:18:44 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
::ffff:10.240.255.107 - - [15/Mar/2018:21:18:47 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kapsayıcıyla işiniz bittiğinde [az container delete][az-container-delete] komutunu kullanarak kapsayıcıyı kaldırın:

```azurecli-interactive
az container delete --resource-group myResourceGroup --name mycontainer
```

Kapsayıcının silindiğini doğrulamak için, [az container list](/cli/azure/container#az-container-list) komutunu yürütün:

```azurecli-interactive
az container list --resource-group myResourceGroup --output table
```

**mycontainer** kapsayıcısı komut çıkışında görünmemelidir. Kaynak grubunda başka kapsayıcınız yoksa, çıkış görüntülenmez.

*myResourceGroup* kaynak grubuyla ve içindeki kaynaklarla işiniz bittiyse [az group delete][az-group-delete] komutuyla silin:

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, genel bir Docker Hub kayıt defterindeki bir görüntüyü kullanarak Azure kapsayıcı örneği oluşturdunuz. Kapsayıcı görüntüsünü oluşturup özel bir Azure kapsayıcı kayıt defterinden dağıtmak istiyorsanız Azure Container Instances öğreticisine geçin.

> [!div class="nextstepaction"]
> [Azure Container Instances öğreticisi](./container-instances-tutorial-prepare-app.md)

Kapsayıcıları Azure üzerinde bir düzenleme sistemi içinde çalıştırma seçenekleri denemek için bkz: [Azure Kubernetes Service (AKS)] [ container-service] hızlı başlangıçları.

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png

<!-- LINKS - External -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git
[azure-account]: https://azure.microsoft.com/free/
[node-js]: https://nodejs.org

<!-- LINKS - Internal -->
[az-container-attach]: /cli/azure/container#az-container-attach
[az-container-create]: /cli/azure/container#az-container-create
[az-container-delete]: /cli/azure/container#az-container-delete
[az-container-list]: /cli/azure/container#az-container-list
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[azure-cli-install]: /cli/azure/install-azure-cli
[container-service]: ../aks/kubernetes-walkthrough.md
