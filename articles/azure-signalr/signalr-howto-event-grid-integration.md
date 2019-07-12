---
title: Azure SignalR hizmeti olayları Event Grid'e göndermek nasıl
description: Event Grid olaylarını SignalR hizmetiniz için etkinleştirme işlemini göstermek için bir kılavuz ardından gönderin istemci bağlantısı bağlı/bağlantısız olayları için örnek bir uygulama.
services: azure-signalr
author: chenyl
ms.service: azure-signalr
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: chenyl
ms.openlocfilehash: 2d782306938136ce6d21a331185f591316f58a29
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67789182"
---
# <a name="how-to-send-events-from-azure-signalr-service-to-event-grid"></a>Event Grid için Azure SignalR hizmeti olayları göndermek nasıl

Azure Event Grid bir yayımlama-abonelik modeli kullanarak tek düzen olay tüketimine sağlayan tam olarak yönetilen olay yönlendirme hizmetidir. Bu kılavuzda, Azure SignalR hizmeti oluşturma, bağlantı olaylarına abone olma ve olaylarını almak için örnek bir web uygulamasına dağıtmak için Azure CLI'yı kullanın. Son olarak, bağlanabilir ve bağlantıyı kes ve örnek uygulamayı olay yükünde bakın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap][azure-account] oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bu makalede Azure CLI komutları için biçimlendirilmiş **Bash** Kabuğu. PowerShell veya komut istemi gibi farklı bir kabuk kullanıyorsanız, satır devamlılığı karakteri veya değişken ataması satırları uygun şekilde ayarlamanız gerekebilir. Bu makalede, gerekli komut düzenleme miktarını en aza indirmek için değişkenleri kullanır.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bir Azure kaynak grubu, dağıtma ve Azure kaynaklarınızı yönetmek mantıksal bir kapsayıcıdır. Aşağıdaki [az grubu oluşturma][az-group-create] komut adlı bir kaynak grubu oluşturur *myResourceGroup* içinde *eastus* bölge. Kaynak grubunuz için farklı bir ad kullanmak istiyorsanız, `RESOURCE_GROUP_NAME` için farklı bir değer.

```azurecli-interactive
RESOURCE_GROUP_NAME=myResourceGroup

az group create --name $RESOURCE_GROUP_NAME --location eastus
```

## <a name="create-a-signalr-service"></a>SignalR Hizmeti oluşturma

Ardından, aşağıdaki komutlarla kaynak grubuna bir Azure Signalr hizmeti dağıtın.
```azurecli-interactive
SIGNALR_NAME=SignalRTestSvc

az signalr create --resource-group $RESOURCE_GROUP_NAME --name $SIGNALR_NAME --sku Free_F1
```

SignalR hizmeti oluşturulduktan sonra Azure CLI çıktı aşağıdakine benzer döndürür:

```json
{
  "externalIp": "13.76.156.152",
  "hostName": "clitest.servicedev.signalr.net",
  "hostNamePrefix": "clitest",
  "id": "/subscriptions/28cf13e2-c598-4aa9-b8c8-098441f0827a/resourceGroups/clitest1/providers/Microsoft.SignalRService/SignalR/clitest",
  "location": "southeastasia",
  "name": "clitest",
  "provisioningState": "Succeeded",
  "publicPort": 443,
  "resourceGroup": "clitest1",
  "serverPort": 443,
  "sku": {
    "capacity": 1,
    "family": null,
    "name": "Free_F1",
    "size": "F1",
    "tier": "Free"
  },
  "tags": null,
  "type": "Microsoft.SignalRService/SignalR",
  "version": "1.0"
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

Dağıtım başarılı olduktan sonra (birkaç dakika sürebilir), bir tarayıcı açın ve emin olmak için web uygulamanıza gidin çalışıyor:

`http://<your-site-name>.azurewebsites.net`

[!INCLUDE [event-grid-register-provider-cli.md](../../includes/event-grid-register-provider-cli.md)]

## <a name="subscribe-to-registry-events"></a>Kayıt defteri olaylarına abone olma

Event Grid, abone olduğunuz bir *konu* hangi olayları izlemek istediğinizi ve bunları gönderileceği söylemek için. Aşağıdaki [az eventgrid olay aboneliği oluşturma][az-eventgrid-event-subscription-create] komut aboneliği Azure SignalR hizmeti oluşturuldu ve uç noktası için göndermeden olaylar olarak web uygulamanızın URL'sini belirtir. Düzenleme yok, gerekli olacak şekilde, önceki bölümlerde doldurulmuş ortam değişkenleri burada yeniden kullanılır.

```azurecli-interactive
SIGNALR_SERVICE_ID=$(az signalr show --resource-group $RESOURCE_GROUP_NAME --name $SIGNALR_NAME --query id --output tsv)
APP_ENDPOINT=https://$SITE_NAME.azurewebsites.net/api/updates

az eventgrid event-subscription create \
    --name event-sub-signalr \
    --source-resource-id $SIGNALR_SERVICE_ID \
    --endpoint $APP_ENDPOINT
```

Abonelik tamamlandığında, aşağıdakine benzer bir çıktı görmeniz gerekir:

```JSON
{
  "deadLetterDestination": null,
  "destination": {
    "endpointBaseUrl": "https://$SITE_NAME.azurewebsites.net/api/updates",
    "endpointType": "WebHook",
    "endpointUrl": null
  },
  "filter": {
    "includedEventTypes": [
      "Microsoft.SignalRService.ClientConnectionConnected",
      "Microsoft.SignalRService.ClientConnectionDisconnected"
    ],
    "isSubjectCaseSensitive": null,
    "subjectBeginsWith": "",
    "subjectEndsWith": ""
  },
  "id": "/subscriptions/28cf13e2-c598-4aa9-b8c8-098441f0827a/resourceGroups/myResourceGroup/providers/Microsoft.SignalRService/SignalR/SignalRTestSvc/providers/Microsoft.EventGrid/eventSubscriptions/event-sub-signalr",
  "labels": null,
  "name": "event-sub-signalr",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "retryPolicy": {
    "eventTimeToLiveInMinutes": 1440,
    "maxDeliveryAttempts": 30
  },
  "topic": "/subscriptions/28cf13e2-c598-4aa9-b8c8-098441f0827a/resourceGroups/myResourceGroup/providers/microsoft.signalrservice/signalr/SignalRTestSvc",
  "type": "Microsoft.EventGrid/eventSubscriptions"
}
```

## <a name="trigger-registry-events"></a>Kayıt defteri olayları Tetikle

Hizmet moduna geçin `Serverless Mode` ve SignalR Service istemci bağlantısı kurma. Gerçekleştirebileceğiniz [sunucusuz örnek](https://github.com/aspnet/AzureSignalR-samples/tree/master/samples/Serverless) başvuru olarak.

```bash
git clone git@github.com:aspnet/AzureSignalR-samples.git

cd samples/Management

# Start the negotiation server
# Negotiation server is responsible for generating access token for clients
cd NegotitationServer
dotnet user-secrets set Azure:SignalR:ConnectionString "<Connection String>"
dotnet run

# Use a seperate command line
# Start a client
cd SignalRClient
dotnet run
```

## <a name="view-registry-events"></a>Kayıt defteri olaylarını görüntüle

Artık bir istemci için SignalR hizmeti bağlamış olursunuz. Event Grid Görüntüleyicisi web uygulamanıza gidin ve görmelisiniz bir `ClientConnectionConnected` olay. İstemci sonlandırılması durumunda da göreceksiniz bir `ClientConnectionDisconnected` olay.

<!-- LINKS - External -->
[azure-account]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F
[sample-app]: https://github.com/dbarkol/azure-event-grid-viewer

<!-- LINKS - Internal -->
[az-eventgrid-event-subscription-create]: /cli/azure/eventgrid/event-subscription#az-eventgrid-event-subscription-create
[az-group-create]: /cli/azure/group#az-group-create
