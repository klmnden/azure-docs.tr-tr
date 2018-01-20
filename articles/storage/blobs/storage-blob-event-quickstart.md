---
title: "Azure Blob depolama olaylarını bir özel web uç noktasına (önizleme) yönlendirme | Microsoft Docs"
description: "Blob depolama olaylarına abone olmak için Azure Event Grid’i kullanın."
services: storage,event-grid
keywords: 
author: cbrooksmsft
ms.author: cbrooks
ms.date: 01/19/2018
ms.topic: article
ms.service: storage
ms.openlocfilehash: 50a6126f065b1b4d851f53b5cb3096c130314450
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2018
---
# <a name="route-blob-storage-events-to-a-custom-web-endpoint-preview"></a>Blob depolama olaylarını bir özel web uç noktasına (önizleme) yönlendirme

Azure Event Grid, bulut için bir olay oluşturma hizmetidir. Bu makalede, Azure CLI aracını kullanarak Blob depolama olaylarına abone olur ve sonucu görüntülemek için olayı tetiklersiniz. 

Normalde olayları olaya yanıt veren bir uç noktaya (web kancası veya Azure İşlevi gibi) gönderirsiniz. Bu makalede yer alan örneği basitleştirmek amacıyla olayları yalnızca iletileri toplayan bir URL’ye göndereceğiz. Bu URL olarak adlandırılan bir açık kaynak, üçüncü taraf aracını kullanarak oluşturduğunuz [RequestBin](https://requestb.in/).

> [!NOTE]
> **RequestBin** yüksek verimlilik kullanım için tasarlanmamıştır bir açık kaynak aracıdır. Burada araç tamamen gösterim amaçlı kullanılmıştır. Aynı anda birden fazla olay gönderirseniz araçta tüm olaylarınızı göremeyebilirsiniz.

Bu makalede açıklanan adımları tamamladıktan sonra olay verilerinin bir uç noktaya gönderildiğini görürsünüz.

![Olay verileri](./media/storage-blob-event-quickstart/request-result.png)

[!INCLUDE [quickstarts-free-trial-note.md](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Yükleyip CLI yerel olarak kullanmak seçerseniz, bu makale Azure CLI en son sürümünü kullandığınızı gerektirir (2.0.24 veya üstü). Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli).

Cloud Shell kullanmıyorsanız önce `az login` kullanarak oturum açmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Event Grid konuları Azure kaynaklarıdır ve bir Azure kaynak grubuna yerleştirilmelidir. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal koleksiyondur.

[az group create](/cli/azure/group#create) komutuyla bir kaynak grubu oluşturun. 

Aşağıdaki örnek *westcentralus* konumunda `<resource_group_name>` adlı bir kaynak grubu oluşturur.  `<resource_group_name>` değerini kaynak grubunuz için benzersiz bir adla değiştirin.

```azurecli-interactive
az group create --name <resource_group_name> --location westcentralus
```

## <a name="create-a-blob-storage-account"></a>Blob depolama hesabı oluşturma

Azure Depolama kullanmak için bir depolama hesabınız olması gerekir.  Blob depolama olayları şimdilik yalnızca Blob depolama hesaplarında kullanılabilir.

Blob Storage hesabı, yapılandırılmamış verilerinizi bloblar (nesneler) olarak Azure Storage’da depolamanıza yönelik özel depolama hesabıdır. Blob Storage hesapları, varolan genel amaçlı depolama hesaplarınıza benzer ve blok blobları ve ilave blobları için %100 API tutarlığı dahil günümüzde kullandığınız tüm harika dayanıklılık, kullanılabilirlik, ölçeklenebilirlik ve performans özelliklerini paylaşır. Yalnızca blok veya engelleme blobunun gerektiği uygulamalar için Blob Storage hesaplarının kullanılmasını öneririz.

> [!NOTE]
> Olay kılavuz şu anda Önizleme ve yalnızca depolama hesaplarında kullanılabilir **westcentralus** ve **westus2** bölgeleri.

`<storage_account_name>` değerini depolama hesabınız için benzersiz bir ad ile ve `<resource_group_name>` değerini daha önce oluşturduğunuz kaynak grubu ile değiştirin.

```azurecli-interactive
az storage account create \
  --name <storage_account_name> \
  --location westcentralus \
  --resource-group <resource_group_name> \
  --sku Standard_LRS \
  --kind BlobStorage \
  --access-tier Hot
```

## <a name="create-a-message-endpoint"></a>İleti uç noktası oluşturma

Blob depolama hesabındaki olaylara abone olmadan önce olay iletisi için uç noktayı oluşturalım. Konuya yanıt vermek için kod yazmak yerine, görüntüleyebilmeniz için iletileri toplayan bir uç nokta oluşturacağız. RequestBin bir uç nokta oluşturma ve kendisine gönderilen istekleri görüntüleme olanak tanıyan bir açık kaynak, üçüncü taraf aracıdır. [RequestBin](https://requestb.in/)’e gidip **Create a RequestBin** (RequestBin oluştur) seçeneğine tıklayın.  Daha sonra konuya abone olurken gerekecek kutu URL’sini kopyalayın.

## <a name="subscribe-to-your-blob-storage-account"></a>Blob depolama hesabınıza abone olma

Event Grid’e hangi olayları izlemek istediğinizi bildirmek için bir konuya abone olursunuz. Aşağıdaki örnek, oluşturduğunuz Blob depolama hesabına abone olur ve RequestBin URL’sini olay bildirimi için uç nokta olarak geçirir. `<event_subscription_name>` değerini olay aboneliğiniz için benzersiz bir adla, `<URL_from_RequestBin>` değerini ise önceki bölümden bir değerle değiştirin. Abone olurken bir uç nokta belirttiğinizde, Event Grid olayların bu uç noktaya yönlendirilmesini sağlar. `<resource_group_name>` ve `<storage_account_name>` için daha önce oluşturduğunuz değerleri kullanın. 

```azurecli-interactive
storageid=$(az storage account show --name <storage_account_name> --resource-group <resource_group_name> --query id --output tsv)

az eventgrid event-subscription create \
  --resource-id $storageid \
  --name <event_subscription_name> \
  --endpoint <URL_from_RequestBin>
```

## <a name="trigger-an-event-from-blob-storage"></a>Blob depolama biriminden bir olay tetikler

Şimdi, Event Grid’in iletiyi uç noktanıza nasıl dağıttığını görmek için bir olay tetikleyelim. İlk olarak, şimdi biz bir kapsayıcı oluşturmak sonra oluşturacak ve bir dosyayı karşıya yüklemeyi daha sonra depolama hesabı için anahtarı ve adını yapılandırın. `<storage_account_name>` ve `<resource_group_name>` için yine daha önce oluşturduğunuz değerleri kullanın.

```azurecli-interactive
export AZURE_STORAGE_ACCOUNT=<storage_account_name>
export AZURE_STORAGE_ACCESS_KEY="$(az storage account keys list --account-name <storage_account_name> --resource-group <resource_group_name> --query "[0].value" --output tsv)"

az storage container create --name testcontainer

touch testfile.txt
az storage blob upload --file testfile.txt --container-name testcontainer --name testfile.txt
```

Olayı tetiklediniz ve Event Grid iletiyi abone olurken yapılandırdığınız uç noktaya gönderdi. Daha önce oluşturduğunuz RequestBin URL’sine gidin. Veya açık olan RequestBin tarayıcınızda Yenile’ye tıklayın. Az önce gönderdiğiniz olayı görürsünüz. 

```json
[{
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myrg/providers/Microsoft.Storage/storageAccounts/myblobstorageaccount",
  "subject": "/blobServices/default/containers/testcontainer/blobs/testfile.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-08-16T20:33:51.0595757Z",
  "id": "4d96b1d4-0001-00b3-58ce-16568c064fab",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "d65ca2e2-a168-4155-b7a4-2c925c18902f",
    "requestId": "4d96b1d4-0001-00b3-58ce-16568c000000",
    "eTag": "0x8D4E4E61AE038AD",
    "contentType": "text/plain",
    "contentLength": 0,
    "blobType": "BlockBlob",
    "url": "https://myblobstorageaccount.blob.core.windows.net/testcontainer/testblob1.txt",
    "sequencer": "00000000000000EB0000000000046199",
    "storageDiagnostics": {
      "batchId": "dffea416-b46e-4613-ac19-0371c0c5e352"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]

```

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu depolama hesabı ve olay aboneliğiyle çalışmaya devam etmeyi planlıyorsanız bu makalede oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, aşağıdaki komutu kullanarak bu makalede oluşturduğunuz kaynakları silin.

`<resource_group_name>` değerini yukarıda oluşturduğunuz kaynak grubuyla değiştirin.

```azurecli-interactive
az group delete --name <resource_group_name>
```

## <a name="next-steps"></a>Sonraki adımlar

Artık konu oluşturma ve olay aboneliklerini öğrendiğinize göre, Blob depolama Olayları ve Event Grid’in size nasıl yardımcı olabileceği konusunda daha fazla bilgi edinebilirsiniz:

- [Blob depolama olaylarına yanıt verme](storage-blob-event-overview.md)
- [Event Grid Hakkında](../../event-grid/overview.md)
