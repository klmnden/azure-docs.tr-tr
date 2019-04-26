---
title: Hızlı Başlangıç - Azure Container Registry'ye gönderme olayları Event Grid'e
description: Bu hızlı başlangıçta, nginx kapsayıcısı kayıt defteriniz için Event Grid olaylarını etkinleştirmek sonra kapsayıcı görüntüsü gönderme Gönder ve örnek bir uygulama olayları Sil.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 08/23/2018
ms.author: danlep
ms.custom: seodec18
ms.openlocfilehash: f5c075942a29968ea57c684cd817e578df951989
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60427700"
---
# <a name="quickstart-send-events-from-private-container-registry-to-event-grid"></a>Hızlı Başlangıç: Event Grid için özel kapsayıcı kayıt defterinden olayları gönderme

Azure Event Grid; Yayımla kullanarak tek düzen olay tüketimine sağlayan tam olarak yönetilen olay yönlendirme hizmeti-abonelik modeli. Bu hızlı başlangıçta, bir kapsayıcı kayıt defteri oluşturma, kayıt defteri olaylarına abone olma ve olaylarını almak için örnek bir web uygulamasına dağıtmak için Azure CLI'yı kullanın. Son olarak, kapsayıcı görüntüsü tetiklenen `push` ve `delete` olayları ve olay yükü örnek uygulamada görüntüle.

Bu makaledeki adımları tamamladıktan sonra olaylar Event Grid, kapsayıcı kayıt defterinden gönderilen örnek web uygulaması görünür:

![Web tarayıcısı ile üç alınan olayların örnek web uygulaması oluşturma][sample-app-01]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap][azure-account] oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bu makalede Azure CLI komutları için biçimlendirilmiş **Bash** Kabuğu. PowerShell veya komut istemi gibi farklı bir kabuk kullanıyorsanız, satır devamlılığı karakteri veya değişken ataması satırları uygun şekilde ayarlamanız gerekebilir. Bu makalede, gerekli komut düzenleme miktarını en aza indirmek için değişkenleri kullanır.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bir Azure kaynak grubu, dağıtma ve Azure kaynaklarınızı yönetmek mantıksal bir kapsayıcıdır. Aşağıdaki [az grubu oluşturma] [ az-group-create] komut adlı bir kaynak grubu oluşturur *myResourceGroup* içinde *eastus* bölge. Kaynak grubunuz için farklı bir ad kullanmak istiyorsanız, `RESOURCE_GROUP_NAME` için farklı bir değer.

```azurecli-interactive
RESOURCE_GROUP_NAME=myResourceGroup

az group create --name $RESOURCE_GROUP_NAME --location eastus
```

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

Ardından, bir kapsayıcı kayıt defteri aşağıdaki komutlarla kaynak grubuna dağıtın. Çalıştırmadan önce [az ACT create] [ az-acr-create] komutu, ayarlama `ACR_NAME` kayıt defteriniz için bir ad. Ad, Azure içinde benzersiz olmalıdır ve 5-50 arası alfasayısal karakter ile sınırlıdır.

```azurecli-interactive
ACR_NAME=<acrName>

az acr create --resource-group $RESOURCE_GROUP_NAME --name $ACR_NAME --sku Basic
```

Kayıt defteri oluşturulduktan sonra Azure CLI çıktı aşağıdakine benzer döndürür:

```json
{
  "adminUserEnabled": false,
  "creationDate": "2018-08-16T20:02:46.569509+00:00",
  "id": "/subscriptions/<Subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myregistry",
  "location": "eastus",
  "loginServer": "myregistry.azurecr.io",
  "name": "myregistry",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}

```

## <a name="create-an-event-endpoint"></a>Bir olay uç noktası oluşturma

Bu bölümde, Azure App Service için önceden oluşturulmuş bir örnek bir web uygulamasına dağıtmak için GitHub deposunda bulunan bir Resource Manager şablonu kullanın. Daha sonra kayıt defterinin Event Grid olaylarına abone olma ve bu uygulama olayları için gönderildiği uç noktası olarak belirtin.

Örnek uygulamayı dağıtmak için ayarlanmış `SITE_NAME` web uygulamanız için benzersiz bir ad ve aşağıdaki komutları yürütün. Site adı, tam etki alanı adı (FQDN) web uygulamasının parçası oluşturur çünkü Azure içinde benzersiz olmalıdır. Bir sonraki bölümde, bir web tarayıcısında için uygulamanın FQDN, kayıt defterinin olayları görüntülemek için gidin.

```azurecli-interactive
SITE_NAME=<your-site-name>

az group deployment create \
    --resource-group $RESOURCE_GROUP_NAME \
    --template-uri "https://raw.githubusercontent.com/Azure-Samples/azure-event-grid-viewer/master/azuredeploy.json" \
    --parameters siteName=$SITE_NAME hostingPlanName=$SITE_NAME-plan
```

Dağıtım başarılı (Bu işlem birkaç dakika sürebilir), bir tarayıcı açın ve emin olmak için web uygulamanıza gidin çalışıyor:

`http://<your-site-name>.azurewebsites.net`

Hiçbir olay ileti görüntülendi işlenen örnek uygulamasını görmeniz gerekir:

![Web tarayıcı gösteren örnek web uygulaması ile görüntülenen olay yok][sample-app-02]

[!INCLUDE [event-grid-register-provider-cli.md](../../includes/event-grid-register-provider-cli.md)]

## <a name="subscribe-to-registry-events"></a>Kayıt defteri olaylarına abone olma

Event Grid, abone olduğunuz bir *konu* hangi olayları izlemek istediğinizi ve bunları gönderileceği söylemek için. Aşağıdaki [az eventgrid olay aboneliği oluşturma] [ az-eventgrid-event-subscription-create] komut abone container registry'ye oluşturulan ve uç noktası için göndermeden olaylar olarak web uygulamanızın URL'sini belirtir. Düzenleme yok, gerekli olacak şekilde, önceki bölümlerde doldurulmuş ortam değişkenleri burada yeniden kullanılır.

```azurecli-interactive
ACR_REGISTRY_ID=$(az acr show --name $ACR_NAME --query id --output tsv)
APP_ENDPOINT=https://$SITE_NAME.azurewebsites.net/api/updates

az eventgrid event-subscription create \
    --name event-sub-acr \
    --source-resource-id $ACR_REGISTRY_ID \
    --endpoint $APP_ENDPOINT
```

Abonelik tamamlandığında, aşağıdakine benzer bir çıktı görmeniz gerekir:

```JSON
{
  "destination": {
    "endpointBaseUrl": "https://eventgridviewer.azurewebsites.net/api/updates",
    "endpointType": "WebHook",
    "endpointUrl": null
  },
  "filter": {
    "includedEventTypes": [
      "All"
    ],
    "isSubjectCaseSensitive": null,
    "subjectBeginsWith": "",
    "subjectEndsWith": ""
  },
  "id": "/subscriptions/<Subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myregistry/providers/Microsoft.EventGrid/eventSubscriptions/event-sub-acr",
  "labels": null,
  "name": "event-sub-acr",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "topic": "/subscriptions/<Subscription ID>/resourceGroups/myresourcegroup/providers/microsoft.containerregistry/registries/myregistry",
  "type": "Microsoft.EventGrid/eventSubscriptions"
}
```

## <a name="trigger-registry-events"></a>Kayıt defteri olayları Tetikle

Örnek uygulamayı açık ve çalışıyor ve Event Grid ile kayıt defteriniz için abone oldunuz göre bazı olaylar oluşturmak hazırsınız. Bu bölümde, oluşturup bir kapsayıcı görüntüsünü kayıt defterinize gönderme ACR görevleri kullanın. ACR görevleri Azure Container Registry'nin, yerel makinenizde Docker altyapısı gerek kalmadan bulutta kapsayıcı görüntülerini oluşturmanıza olanak sağlayan bir özelliktir.

### <a name="build-and-push-image"></a>Derleme ve görüntü gönderme

Bir GitHub deposunun içeriğini bir kapsayıcı görüntüsünü oluşturmak için aşağıdaki Azure CLI komutunu yürütün. Varsayılan olarak, ACR görevleri otomatik olarak başarıyla oluşturulmuş bir görüntüyü oluşturur, kayıt defterine gönderim `ImagePushed` olay.

```azurecli-interactive
az acr build --registry $ACR_NAME --image myimage:v1 -f Dockerfile https://github.com/Azure-Samples/acr-build-helloworld-node.git
```

ACR görevler oluşturur ve sonra görüntünüzü gönderim aşağıdakine benzer bir çıktı görmeniz gerekir. Aşağıdaki örnek çıktıda uzatmamak için kısaltıldı.

```console
$ az acr build -r $ACR_NAME --image myimage:v1 -f Dockerfile https://github.com/Azure-Samples/acr-build-helloworld-node.git
Sending build context to ACR...
Queued a build with build ID: aa2
Waiting for build agent...
2018/08/16 22:19:38 Using acb_vol_27a2afa6-27dc-4ae4-9e52-6d6c8b7455b2 as the home volume
2018/08/16 22:19:38 Setting up Docker configuration...
2018/08/16 22:19:39 Successfully set up Docker configuration
2018/08/16 22:19:39 Logging in to registry: myregistry.azurecr.io
2018/08/16 22:19:55 Successfully logged in
Sending build context to Docker daemon  94.72kB
Step 1/5 : FROM node:9-alpine
...
```

Oluşturulan görüntüyü kayıt defterinizde olduğunu doğrulamak için "myımage" deposunda etiketlerini görüntülemek için aşağıdaki komutu yürütün:

```azurecli-interactive
az acr repository show-tags --name $ACR_NAME --repository myimage
```

Oluşturulan görüntünün "v1" etiketi çıkışında, aşağıdakine benzer görünmelidir:

```console
$ az acr repository show-tags --name $ACR_NAME --repository myimage
[
  "v1"
]
```

### <a name="delete-the-image"></a>Görüntüyü Sil

Artık, bir `ImageDeleted` görüntüyü silerek olay [az acr depo silme] [ az-acr-repository-delete] komutu:

```azurecli-interactive
az acr repository delete --name $ACR_NAME --image myimage:v1
```

Aşağıdakine benzer bir çıktı görmeniz gerekir onay bildirimi ve ilişkili görüntüler silmek isteyen:

```console
$ az acr repository delete --name $ACR_NAME --image myimage:v1
This operation will delete the manifest 'sha256:f15fa9d0a69081ba93eee308b0e475a54fac9c682196721e294b2bc20ab23a1b' and all the following images: 'myimage:v1'.
Are you sure you want to continue? (y/n): y
```

## <a name="view-registry-events"></a>Kayıt defteri olaylarını görüntüle

Görüntüyü kayıt defterinize gönderdiniz artık ve ardından silinir. Event Grid Görüntüleyicisi web uygulamanıza gidin ve her ikisi de görmelisiniz `ImageDeleted` ve `ImagePushed` olayları. Bir abonelik doğrulama olayı komutu yürüterek oluşturulan de görebilirsiniz [abone olmak için kayıt defteri olaylarını](#subscribe-to-registry-events) bölümü.

Örnek uygulamayı üç olaylarla aşağıdaki ekran gösterilir ve `ImageDeleted` olay ayrıntılarını görüntülemek için genişletilir.

![Örnek uygulamayı ImagePushed ve ImageDeleted olaylarla gösteren bir web tarayıcısı][sample-app-03]

Tebrikler! Görürseniz `ImagePushed` ve `ImageDeleted` olayları, kayıt defterinizin olayları Event Grid'e gönderiyor ve Event Grid, web uygulama uç noktasına olayları iletme.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıçta oluşturulan kaynakları ile işiniz bittiğinde, tüm aşağıdaki Azure CLI komutu ile silebilirsiniz. Bir kaynak grubu sildiğinizde, içerdiği tüm kaynakları kalıcı olarak silinir.

**UYARI**: Bu işlem geri alınamaz. Komutu çalıştırmadan önce herhangi bir grup içindeki kaynaklara artık ihtiyacınız emin olun.

```azurecli-interactive
az group delete --name $RESOURCE_GROUP_NAME
```

## <a name="event-grid-event-schema"></a>Event Grid olay şeması

Azure Container Registry olay iletisi şema başvurusu Event Grid belgelerinde bulabilirsiniz:

[Kapsayıcı kayıt defteri için Azure Event Grid olay şeması](../event-grid/event-schema-container-registry.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir kapsayıcı kayıt defteri dağıtılan, görüntüyü ACR görevler ile oluşturulan, silinmiş ve Event grid'den olayların bir örnek uygulaması ile kayıt defterinin tüketilen. Ardından, taşıma hakkında bilgi edinmek için daha fazla bulutta, kapsayıcı görüntüleri oluşturma hakkında da dahil olmak üzere otomatik ACR görevleri öğreticiye üzerinde temel görüntü güncelleştirme derlemeleri:

> [!div class="nextstepaction"]
> [ACR görevler ile bulutta kapsayıcı görüntüleri oluşturma](container-registry-tutorial-quick-task.md)

<!-- IMAGES -->
[sample-app-01]: ./media/container-registry-event-grid-quickstart/sample-app-01.png
[sample-app-02]: ./media/container-registry-event-grid-quickstart/sample-app-02-no-events.png
[sample-app-03]: ./media/container-registry-event-grid-quickstart/sample-app-03-with-events.png

<!-- LINKS - External -->
[azure-account]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F
[sample-app]: https://github.com/dbarkol/azure-event-grid-viewer

<!-- LINKS - Internal -->
[az-acr-create]: /cli/azure/acr/repository
[az-acr-repository-delete]: /cli/azure/acr/repository#az-acr-repository-delete
[az-eventgrid-event-subscription-create]: /cli/azure/eventgrid/event-subscription#az-eventgrid-event-subscription-create
[az-group-create]: /cli/azure/group#az-group-create
