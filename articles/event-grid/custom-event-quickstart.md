---
title: "Azure Event Grid için Özel Olaylar | Microsoft Docs"
description: "Azure Event Grid’i kullanarak bir konu yayımlayın ve o olaya abone olun."
services: event-grid
keywords: 
author: djrosanova
ms.author: darosa
ms.date: 08/15/2017
ms.topic: hero-article
ms.service: event-grid
ms.translationtype: HT
ms.sourcegitcommit: 5b6c261c3439e33f4d16750e73618c72db4bcd7d
ms.openlocfilehash: 0290836bebadb20085a3ce84dddc088c3af385da
ms.contentlocale: tr-tr
ms.lasthandoff: 08/28/2017

---

# <a name="create-and-route-custom-events-with-azure-event-grid"></a>Azure Event Grid ile özel olaylar oluşturma ve yönlendirme

Azure Event Grid, bulut için bir olay oluşturma hizmetidir. Bu makalede, Azure CLI aracını kullanarak özel bir konu oluşturur, konuya abone olur ve sonucu görüntülemek için olayı tetiklersiniz. Normalde olayları olaya yanıt veren bir uç noktaya (web kancası veya Azure İşlevi gibi) gönderirsiniz. Ancak, bu makaleyi basitleştirmek amacıyla olayları yalnızca iletileri toplayan bir URL’ye gönderirsiniz. Bu URL’yi [RequestBin](https://requestb.in/) adlı açık kaynak, üçüncü taraf bir aracı kullanarak oluşturursunuz.

>[!NOTE]
>**RequestBin** ağır iş yüklerinde kullanım için tasarlanmamış açık kaynaklı bir araçtır. Burada araç tamamen gösterim amaçlı kullanılmıştır. Aynı anda birden fazla olay gönderirseniz araçta tüm olaylarınızı göremeyebilirsiniz.

İşiniz bittiğinde, olay verilerinin bir uç noktaya gönderildiğini görürsünüz.

![Olay verileri](./media/custom-event-quickstart/request-result.png)

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI’nın en son sürümünü (2.0.14 veya sonraki) kullanıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Event Grid konuları Azure kaynaklarıdır ve bir Azure kaynak grubuna yerleştirilmelidir. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal koleksiyondur.

[az group create](/cli/azure/group#create) komutuyla bir kaynak grubu oluşturun. 

Aşağıdaki örnek *westus2* konumunda *gridResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name gridResourceGroup --location westus2
```

## <a name="create-a-custom-topic"></a>Özel konu oluşturma

Konu, olaylarınızı gönderdiğiniz kullanıcı tanımlı bir uç nokta sağlar. Aşağıdaki örnekte, konu kaynak grubunuzda oluşturulur. `<topic_name>` değerini konunuz için benzersiz bir adla değiştirin. Konu adı bir DNS girdisi ile temsil edildiğinden benzersiz olmalıdır. Önizleme sürümü için Event Grid tarafından **westus2** ve **westcentralus** konumları desteklenir.

```azurecli-interactive
az eventgrid topic create --name <topic_name> -l westus2 -g gridResourceGroup
```

## <a name="create-a-message-endpoint"></a>İleti uç noktası oluşturma

Konuya abone olmadan önce olay iletisi için uç noktayı oluşturalım. Konuya yanıt vermek için kod yazmak yerine, görüntüleyebilmeniz için iletileri toplayan bir uç nokta oluşturalım. RequestBin, bir uç nokta oluşturmanıza ve buna gönderilen istekleri görüntülemenize imkan tanıyan açık kaynak, üçüncü taraf bir araçtır. [RequestBin](https://requestb.in/)’e gidip **Create a RequestBin** (RequestBin oluştur) seçeneğine tıklayın.  Daha sonra konuya abone olurken gerekecek kutu URL’sini kopyalayın.

## <a name="subscribe-to-a-topic"></a>Bir konuya abone olma

Event Grid’e hangi olayları izlemek istediğinizi bildirmek için bir konuya abone olursunuz. Aşağıdaki örnek, oluşturduğunuz konuya abone olur ve RequestBin URL’sini olay bildirimi için uç nokta olarak geçirir. `<event_subscription_name>` değerini aboneliğiniz için benzersiz bir adla, `<URL_from_RequestBin>` değerini ise önceki bölümden bir değerle değiştirin. Abone olurken bir uç nokta belirttiğinizde, Event Grid olayların bu uç noktaya yönlendirilmesini sağlar. `<topic_name>` için daha önce oluşturduğunuz değeri kullanın. 

```azurecli-interactive
az eventgrid topic event-subscription create --name <event_subscription_name> \
  --endpoint <URL_from_RequestBin> \
  -g gridResourceGroup \
  --topic-name <topic_name>
```

## <a name="send-an-event-to-your-topic"></a>Konunuza olay gönderme

Şimdi, Event Grid’in iletiyi uç noktanıza nasıl dağıttığını görmek için bir olay tetikleyelim. İlk olarak konunun URL’sini ve anahtarını alalım. Tekrar belirtmek gerekirse, `<topic_name>` için kendi konu adınızı kullanın.

```azurecli-interactive
endpoint=$(az eventgrid topic show --name <topic_name> -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name <topic_name> -g gridResourceGroup --query "key1" --output tsv)
```

Bu makaleyi basitleştirmek için konuya gönderilmek üzere örnek olay verileri ayarladık. Normalde olay verilerini bir uygulama veya Azure hizmeti gönderir. Aşağıdaki örnek, olay verilerini alır:

```azurecli-interactive
body=$(eval echo "'$(curl https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/customevent.json)'")
```

`echo "$body"` durumunda olayın tamamını görebilirsiniz. JSON’un `data` öğesi, olayınızın yüküdür. Bu alana doğru oluşturulmuş herhangi bir JSON gelebilir. Ayrıca, gelişmiş yönlendirme ve filtreleme için konu alanını da kullanabilirsiniz.

CURL, HTTP istekleri gerçekleştiren bir yardımcı programdır. Bu makalede, olayı konumuza göndermek için CURL kullanıyoruz. 

```azurecli-interactive
curl -X POST -H "aeg-sas-key: $key" -d "$body" $endpoint
```

Olayı tetiklediniz ve Event Grid iletiyi abone olurken yapılandırdığınız uç noktaya gönderdi. Daha önce oluşturduğunuz RequestBin URL’sine gidin. Veya açık olan RequestBin tarayıcınızda Yenile’ye tıklayın. Az önce gönderdiğiniz olayı görürsünüz. 

```json
[{
  "id": "1807",
  "eventType": "recordInserted",
  "subject": "myapp/vehicles/motorcycles",
  "eventTime": "2017-08-10T21:03:07+00:00",
  "data": {
    "make": "Ducati",
    "model": "Monster"
  },
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/topics/{topic}"
}]
```

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu olayla çalışmaya devam etmeyi planlıyorsanız bu makalede oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, aşağıdaki komutu kullanarak bu makalede oluşturduğunuz kaynakları silin.

```azurecli-interactive
az group delete --name gridResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Artık konu oluşturma ve olay aboneliklerini öğrendiğinize göre, Event Grid’in size nasıl yardımcı olabileceği konusunda daha fazla bilgi edinebilirsiniz:

- [Event Grid Hakkında](overview.md)
- [Azure Event Grid ve Logic Apps ile sanal makine değişikliklerini izleme](monitor-virtual-machine-changes-event-grid-logic-app.md)

