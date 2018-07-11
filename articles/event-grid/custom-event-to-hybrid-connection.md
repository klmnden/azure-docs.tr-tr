---
title: Azure Event Grid için özel olayları karma bağlantıya gönderme | Microsoft Docs
description: Azure Event Grid'i ve Azure CLI'yı kullanarak bir konu yayımlayın ve o olaya abone olun. Uç nokta için karma bir bağlantı kullanılır.
services: event-grid
keywords: ''
author: tfitzmac
ms.author: tomfitz
ms.date: 06/29/2018
ms.topic: tutorial
ms.service: event-grid
ms.openlocfilehash: ee504f805c536ba9a6186514206546c3df1f0f1a
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37127722"
---
# <a name="route-custom-events-to-azure-relay-hybrid-connections-with-azure-cli-and-event-grid"></a>Azure CLI ve Event Grid ile özel olayları Azure Relay Karma Bağlantılar’a yönlendirme

Azure Event Grid, bulut için bir olay oluşturma hizmetidir. Azure Relay Karma Bağlantılar, desteklenen olay işleyicilerinden biridir. Genel uç noktası olmayan uygulamalardan alınan olayları işlemeniz gerektiğinde olay işleyicisi olarak karma bağlantıları kullanırsınız. Bu uygulamalar kurumsal ağınızın içinde olabilir. Bu makalede, Azure CLI aracını kullanarak özel bir konu oluşturur, konuya abone olur ve sonucu görüntülemek için olayı tetiklersiniz. Olayları karma bağlantıya gönderirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu makalede zaten bir karma bağlantınız ve dinleyici uygulamanız olduğu varsayılmıştır. Karma bağlantıları kullanmaya başlamak için bkz. [Relay Karma Bağlantılar’ı kullanmaya başlama - .NET](../service-bus-relay/relay-hybrid-connections-dotnet-get-started.md) veya [Relay Karma Bağlantılar’ı kullanmaya başlama - Düğüm](../service-bus-relay/relay-hybrid-connections-node-get-started.md).

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Event Grid konuları Azure kaynaklarıdır ve bir Azure kaynak grubuna yerleştirilmelidir. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal koleksiyondur.

[az group create](/cli/azure/group#az_group_create) komutuyla bir kaynak grubu oluşturun. 

Aşağıdaki örnek *westus2* konumunda *gridResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name gridResourceGroup --location westus2
```

## <a name="create-a-custom-topic"></a>Özel konu oluşturma

Event grid konusu, olaylarınızı göndereceğiniz kullanıcı tanımlı bir uç nokta sağlar. Aşağıdaki örnekte özel konu, kaynak grubunuzda oluşturulur. `<topic_name>` değerini konunuz için benzersiz bir adla değiştirin. Konu adı bir DNS girdisi ile temsil edildiğinden konu adı benzersiz olmalıdır.

```azurecli-interactive
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid topic create --name <topic_name> -l westus2 -g gridResourceGroup
```

## <a name="subscribe-to-a-topic"></a>Bir konuya abone olma

Event Grid’e hangi olayları izlemek istediğinizi bildirmek için bir konuya abone olursunuz. Aşağıdaki örnek, oluşturduğunuz konuya abone olur ve uç nokta için karma bağlantının kaynak kimliğini geçirir. Karma bağlantı kimliği şu biçimdedir:

`/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Relay/namespaces/<relay-namespace>/hybridConnections/<hybrid-connection-name>`

Aşağıdaki betik, geçiş ad alanının kaynak kimliğini alır. Karma bağlantının kimliğini oluşturur ve bir event grid konusuna abone olur. Betik, uç nokta türünü `hybridconnection` olarak ayarlar ve uç noktanın karma bağlantı kimliğini kullanır.

```azurecli-interactive
relayname=<namespace-name>
relayrg=<resource-group-for-relay>
hybridname=<hybrid-name>

relayid=$(az resource show --name $relayname --resource-group $relayrg --resource-type Microsoft.Relay/namespaces --query id --output tsv)
hybridid="$relayid/hybridConnections/$hybridname"

az eventgrid event-subscription create \
  --topic-name <topic_name> \
  -g gridResourceGroup \
  --name <event_subscription_name> \
  --endpoint-type hybridconnection \
  --endpoint $hybridid
```

## <a name="create-application-to-process-events"></a>Olayları işlemek için uygulama oluşturma

Karma bağlantıdan olayları alabilecek bir uygulamaya ihtiyacınız vardır. [Microsoft Azure Event Grid Hybrid Connection Consumer sample for C#](https://github.com/Azure-Samples/event-grid-dotnet-hybridconnection-destination), bu işlemi gerçekleştirir. Önkoşul adımlarını tamamladınız.

1. Visual Studio 2017 Sürüm 15.5 veya üzerine sahip olduğunuzdan emin olun.

1. Depoyu yerel makinenize kopyalayın.

1. Visual Studio'da HybridConnectionConsumer projesini yükleyin.

1. Program.cs dosyasında `<relayConnectionString>` ve `<hybridConnectionName>` yerine oluşturduğunuz geçiş bağlantısı dizesini ve karma bağlantı adını yazın.

1. Visual Studio'da derleyin ve çalıştırın.

## <a name="send-an-event-to-your-topic"></a>Konunuza olay gönderme

Event Grid’in iletiyi uç noktanıza nasıl dağıttığını görmek için bir olay tetikleyelim. Bu makalede olayı tetiklemek için Azure CLI'yı nasıl kullanacağınız gösterilmektedir. Alternatif olarak [Event Grid yayımcı uygulamasını](https://github.com/Azure-Samples/event-grid-dotnet-publish-consume-events/tree/master/EventGridPublisher) da kullanabilirsiniz.

İlk olarak özel konunun URL’sini ve anahtarını alalım. Tekrar belirtmek gerekirse, `<topic_name>` için kendi konu adınızı kullanın.

```azurecli-interactive
endpoint=$(az eventgrid topic show --name <topic_name> -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name <topic_name> -g gridResourceGroup --query "key1" --output tsv)
```

Bu makaleyi basitleştirmek için konuya gönderilmek üzere örnek olay verilerini kullanın. Normalde olay verilerini bir uygulama veya Azure hizmeti gönderir. CURL, HTTP istekleri gönderen bir yardımcı programdır. Bu makalede, konuya olayı göndermek için CURL kullanın.  Aşağıdaki örnek, event grid konusuna üç olay gönderir:

```azurecli-interactive
body=$(eval echo "'$(curl https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/customevent.json)'")
curl -X POST -H "aeg-sas-key: $key" -d "$body" $endpoint
```

Dinleyici uygulamanız olay iletisini almalıdır.

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu olayla çalışmaya devam etmeyi planlıyorsanız bu makalede oluşturulan kaynakları temizlemeyin. Aksi takdirde, bu makalede oluşturduğunuz kaynakları silmek için aşağıdaki komutu kullanın.

```azurecli-interactive
az group delete --name gridResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Artık konu oluşturma ve olay aboneliklerini öğrendiğinize göre, Event Grid’in size nasıl yardımcı olabileceği konusunda daha fazla bilgi edinebilirsiniz:

- [Event Grid Hakkında](overview.md)
- [Blob depolama olaylarını bir özel web uç noktasına yönlendirme](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json)
- [Azure Event Grid ve Logic Apps ile sanal makine değişikliklerini izleme](monitor-virtual-machine-changes-event-grid-logic-app.md)
- [Veri ambarına büyük veri akışı yapma](event-grid-event-hubs-integration.md)
