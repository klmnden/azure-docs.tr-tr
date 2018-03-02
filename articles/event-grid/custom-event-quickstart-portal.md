---
title: "Azure portalıyla Azure Event Grid için özel olaylar | Microsoft Docs"
description: "Azure Event Grid'i ve PowerShell'i kullanarak bir konu yayımlayın ve o olaya abone olun."
services: event-grid
keywords: 
author: tfitzmac
ms.author: tomfitz
ms.date: 01/30/2018
ms.topic: hero-article
ms.service: event-grid
ms.openlocfilehash: f37d496d43bb24c51d6e1c11b77d9ceba48b7b23
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="create-and-route-custom-events-with-the-azure-portal-and-event-grid"></a>Azure portalı ve Event Grid ile özel olaylar oluşturma ve yönlendirme

Azure Event Grid, bulut için bir olay oluşturma hizmetidir. Bu makalede, Azure portalını kullanarak özel bir konu oluşturur, konuya abone olur ve sonucu görüntülemek için olayı tetiklersiniz. Normalde olayları olaya yanıt veren bir uç noktaya (web kancası veya Azure İşlevi gibi) gönderirsiniz. Ancak, bu makaleyi basitleştirmek amacıyla olayları yalnızca iletileri toplayan bir URL’ye gönderirsiniz. Bu URL’yi [RequestBin](https://requestb.in/) veya [Hookbin](https://hookbin.com/)’deki üçüncü taraf araçlarını kullanarak oluşturursunuz.

>[!NOTE]
>**RequestBin** ve **Hookbin** yüksek aktarım hızında kullanıma yönelik tasarlanmamıştır. Bu araçların kullanımını tamamen gösterim amaçlıdır. Aynı anda birden fazla olay gönderirseniz araçta tüm olaylarınızı göremeyebilirsiniz.

İşiniz bittiğinde, olay verilerinin bir uç noktaya gönderildiğini görürsünüz.

![Olay verileri](./media/custom-event-quickstart-portal/request-result.png)

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Event Grid konuları Azure kaynaklarıdır ve bir Azure kaynak grubuna yerleştirilmelidir. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal koleksiyondur.

1. Soldaki menüden **Kaynak Grupları**'nı seçin. Ardından **Ekle**'yi seçin.

   ![Kaynak grubu oluşturma](./media/custom-event-quickstart-portal/create-resource-group.png)

1. Kaynak grubu adını *gridResourceGroup*, konumu da *westus2* yapın. **Oluştur**’u seçin.

   ![Kaynak grubu değerlerini sağlama](./media/custom-event-quickstart-portal/provide-resource-group-values.png)

## <a name="create-a-custom-topic"></a>Özel konu oluşturma

Konu, olaylarınızı gönderdiğiniz kullanıcı tanımlı bir uç nokta sağlar. 

1. Kaynak grubunuzda bir konu oluşturmak için **Tüm hizmetler**'i seçin ve *olay kılavuzu* araması yapın. Açılan seçeneklerden **Olay Kılavuzu Konuları**'nı seçin.

   ![Olay kılavuzu konusu oluşturma](./media/custom-event-quickstart-portal/create-event-grid-topic.png)

1. **Add (Ekle)** seçeneğini belirleyin.

   ![Olay kılavuzu konusu ekleme](./media/custom-event-quickstart-portal/add-topic.png)

1. Konu için bir ad belirtin. Konu adı bir DNS girdisi ile temsil edildiğinden benzersiz olmalıdır. [Desteklenen bölgelerden](overview.md) birini seçin. Önceden oluşturduğunuz kaynak grubunu seçin. **Oluştur**’u seçin.

   ![Olay kılavuzu konu değerlerini sağlama](./media/custom-event-quickstart-portal/provide-topic-values.png)

1. Konu oluşturulduktan sonra görmek için **Yenile**'yi seçin.

   ![Olay kılavuzu konusunu görme](./media/custom-event-quickstart-portal/see-topic.png)

## <a name="create-a-message-endpoint"></a>İleti uç noktası oluşturma

Konuya abone olmadan önce olay iletisi için uç noktayı oluşturalım. Konuya yanıt vermek için kod yazmak yerine, görüntüleyebilmeniz için iletileri toplayan bir uç nokta oluşturalım. RequestBin ve Hookbin, bir uç nokta oluşturmanıza ve buna gönderilen istekleri görüntülemenize imkan tanıyan üçüncü taraf araçlardır. [RequestBin](https://requestb.in/)’e gidip **RequestBin Oluştur**’a tıklayın veya [Hookbin](https://hookbin.com/)’e gidip **Yeni Uç Nokta Oluştur**’a tıklayın.  Daha sonra konuya abone olurken gerekecek kutu URL’sini kopyalayın.

## <a name="subscribe-to-a-topic"></a>Bir konuya abone olma

Event Grid’e hangi olayları izlemek istediğinizi bildirmek için bir konuya abone olursunuz. 

1. Event Grid aboneliği oluşturmak için yine **Tüm Hizmetler**'i seçin ve *olay kılavuzu* araması yapın. Açılan seçeneklerden **Olay Kılavuzu Abonelikleri**'ni seçin.

   ![Olay kılavuzu aboneliği oluşturma](./media/custom-event-quickstart-portal/create-subscription.png)

1. **+ Olay Aboneliği**'ni seçin.

   ![Olay kılavuzu aboneliği ekleme](./media/custom-event-quickstart-portal/add-subscription.png)

1. Olay aboneliğiniz için benzersiz bir ad girin. Konu türü için **Olay Kılavuzu Konuları**'nı seçin. Örneğin oluşturduğunuz özel konuyu seçebilirsiniz. Olay bildirimi uç noktası olarak RequestBin veya Hookbin URL'sini sağlayın. Değerleri girmeyi bitirdiğinizde **Oluştur**'u seçin.

   ![Olay kılavuzu abonelik değeri sağlama](./media/custom-event-quickstart-portal/provide-subscription-values.png)

Şimdi, Event Grid’in iletiyi uç noktanıza nasıl dağıttığını görmek için bir olay tetikleyelim. Bu makaleyi basitleştirmek için Cloud Shell kullanarak konuya örnek olay verilerini gönderin. Normalde olay verilerini bir uygulama veya Azure hizmeti gönderir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="send-an-event-to-your-topic"></a>Konunuza olay gönderme

İlk olarak konunun URL’sini ve anahtarını alalım. `<topic_name>` için kendi konu adınızı kullanın.

```azurecli-interactive
endpoint=$(az eventgrid topic show --name <topic_name> -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name <topic_name> -g gridResourceGroup --query "key1" --output tsv)
```

Aşağıdaki örnek, örnek olay verilerini alır:

```azurecli-interactive
body=$(eval echo "'$(curl https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/customevent.json)'")
```

`echo "$body"` durumunda olayın tamamını görebilirsiniz. JSON’un `data` öğesi, olayınızın yüküdür. Bu alana doğru oluşturulmuş herhangi bir JSON gelebilir. Ayrıca, gelişmiş yönlendirme ve filtreleme için konu alanını da kullanabilirsiniz.

CURL, HTTP istekleri gerçekleştiren bir yardımcı programdır. Bu makalede, konuya bir olay göndermek için CURL kullanın. 

```azurecli-interactive
curl -X POST -H "aeg-sas-key: $key" -d "$body" $endpoint
```

Olayı tetiklediniz ve Event Grid iletiyi abone olurken yapılandırdığınız uç noktaya gönderdi. Daha önce oluşturduğunuz uç nokta URL’sine gidin. Veya açık olan tarayıcınızda Yenile’ye tıklayın. Az önce gönderdiğiniz olayı görürsünüz.

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
  "dataVersion": "1.0",
  "metadataVersion": "1",
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/topics/{topic}"
}]
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu olayla çalışmaya devam etmeyi planlıyorsanız bu makalede oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız bu makalede oluşturduğunuz kaynakları silin.

Kaynak grubunu seçin ve **Kaynak grubunu sil** seçeneğini belirleyin.

## <a name="next-steps"></a>Sonraki adımlar

Artık konu oluşturma ve olay aboneliklerini öğrendiğinize göre, Event Grid’in size nasıl yardımcı olabileceği konusunda daha fazla bilgi edinebilirsiniz:

- [Event Grid Hakkında](overview.md)
- [Blob depolama olaylarını bir özel web uç noktasına yönlendirme](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json)
- [Azure Event Grid ve Logic Apps ile sanal makine değişikliklerini izleme](monitor-virtual-machine-changes-event-grid-logic-app.md)
- [Veri ambarına büyük veri akışı yapma](event-grid-event-hubs-integration.md)
