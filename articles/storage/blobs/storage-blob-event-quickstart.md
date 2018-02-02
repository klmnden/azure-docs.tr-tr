---
title: "Azure Blob Depolama olayları özel web uç noktasına rota | Microsoft Docs"
description: "Blob depolama olaylarına abone olmak için Azure Event Grid’i kullanın."
services: storage,event-grid
keywords: 
author: cbrooksmsft
ms.author: cbrooks
ms.date: 01/30/2018
ms.topic: article
ms.service: storage
ms.openlocfilehash: 4f10d9b26cb75bee8103d986b7fa1197168c692f
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="route-blob-storage-events-to-a-custom-web-endpoint-with-azure-cli"></a>Azure CLI ile bir özel web uç noktası için rota Blob Depolama olayları

Azure Event Grid, bulut için bir olay oluşturma hizmetidir. Bu makalede, Azure CLI aracını kullanarak Blob depolama olaylarına abone olur ve sonucu görüntülemek için olayı tetiklersiniz. 

Normalde olayları olaya yanıt veren bir uç noktaya (web kancası veya Azure İşlevi gibi) gönderirsiniz. Bu makalede yer alan örneği basitleştirmek amacıyla olayları yalnızca iletileri toplayan bir URL’ye göndereceğiz. Bu URL ya da üçüncü taraf araçları kullanarak oluşturduğunuz [RequestBin](https://requestb.in/) veya [Hookbin](https://hookbin.com/).

> [!NOTE]
> **RequestBin** ve **Hookbin** yüksek verimlilik kullanım için tasarlanmamıştır. Bu araçların tamamen demonstrative kullanılır. Aynı anda birden fazla olay gönderirseniz araçta tüm olaylarınızı göremeyebilirsiniz.

Bu makalede açıklanan adımları tamamladıktan sonra olay verilerinin bir uç noktaya gönderildiğini görürsünüz.

![Olay verileri](./media/storage-blob-event-quickstart/request-result.png)

[!INCLUDE [quickstarts-free-trial-note.md](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI’nın en son sürümünü (2.0.24 veya sonraki) kullanıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli).

Cloud Shell kullanmıyorsanız önce `az login` kullanarak oturum açmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Event Grid konuları Azure kaynaklarıdır ve bir Azure kaynak grubuna yerleştirilmelidir. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal koleksiyondur.

[az group create](/cli/azure/group#az_group_create) komutuyla bir kaynak grubu oluşturun. 

Aşağıdaki örnek *westcentralus* konumunda `<resource_group_name>` adlı bir kaynak grubu oluşturur.  `<resource_group_name>` değerini kaynak grubunuz için benzersiz bir adla değiştirin.

```azurecli-interactive
az group create --name <resource_group_name> --location westcentralus
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

BLOB Depolama olayları kullanmak için ya da gereken bir [Blob storage hesabı](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#blob-storage-accounts) veya [genel amaçlı v2 depolama hesabı](../common/storage-account-options.md#general-purpose-v2). **Genel amaçlı v2 (GPv2)** BLOB'lar, dosyalar, kuyruklar ve tablolar dahil olmak üzere tüm depolama hizmetleri için tüm özellikleri destekleyen depolama hesaplarıdır. A **Blob storage hesabı** , yapılandırılmamış verilerinizi bloblar (nesneler) olarak Azure Storage depolamak için bir özel depolama hesabı. BLOB storage hesapları, genel amaçlı depolama hesapları gibi ve % 100 API tutarlığı dahil günümüzde blok bloblar için kullanılır ve ilave blobları tüm harika dayanıklılık, kullanılabilirlik, ölçeklenebilirlik ve performans özelliklerini paylaşır. Yalnızca blok veya engelleme blobunun gerektiği uygulamalar için Blob Storage hesaplarının kullanılmasını öneririz. 

> [!NOTE]
> Kullanılabilirlik depolama olayları için olay kılavuza bağlı [kullanılabilirlik](../../event-grid/overview.md) ve olay kılavuz yaptığı gibi diğer bölgelerde kullanılabilir hale gelecektir.

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

Konuya abone olmadan önce olay iletisi için uç noktayı oluşturalım. Konuya yanıt vermek için kod yazmak yerine, görüntüleyebilmeniz için iletileri toplayan bir uç nokta oluşturalım. RequestBin ve Hookbin bir uç nokta oluşturmanıza olanak sağlayan ve kendisine gönderilen istekleri görüntüleme üçüncü taraf araçlardır. Gidin [RequestBin](https://requestb.in/), tıklatıp **bir RequestBin oluşturma**, veya gitmek [Hookbin](https://hookbin.com/) tıklatıp **yeni uç nokta oluşturma**.  Daha sonra konuya abone olurken gerekecek kutu URL’sini kopyalayın.

## <a name="subscribe-to-your-storage-account"></a>Depolama hesabınıza abone olma

Event Grid’e hangi olayları izlemek istediğinizi bildirmek için bir konuya abone olursunuz. Aşağıdaki örnek, depolama hesabı oluşturulur ve URL uç nokta için olay bildirimi olarak RequestBin veya Hookbin geçirir abone olur. `<event_subscription_name>` değerini olay aboneliğiniz için benzersiz bir adla, `<endpoint_URL>` değerini ise önceki bölümden bir değerle değiştirin. Abone olurken bir uç nokta belirttiğinizde, Event Grid olayların bu uç noktaya yönlendirilmesini sağlar. `<resource_group_name>` ve `<storage_account_name>` için daha önce oluşturduğunuz değerleri kullanın.  

```azurecli-interactive
storageid=$(az storage account show --name <storage_account_name> --resource-group <resource_group_name> --query id --output tsv)

az eventgrid event-subscription create \
  --resource-id $storageid \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL>
```

## <a name="trigger-an-event-from-blob-storage"></a>Blob depolama biriminden bir olay tetikler

Şimdi, Event Grid’in iletiyi uç noktanıza nasıl dağıttığını görmek için bir olay tetikleyelim. İlk olarak, şimdi biz bir kapsayıcı oluşturur sonra bir dosyayı karşıya yüklemeyi daha sonra depolama hesabı için anahtarı ve adını yapılandırın. `<storage_account_name>` ve `<resource_group_name>` için yine daha önce oluşturduğunuz değerleri kullanın.

```azurecli-interactive
export AZURE_STORAGE_ACCOUNT=<storage_account_name>
export AZURE_STORAGE_ACCESS_KEY="$(az storage account keys list --account-name <storage_account_name> --resource-group <resource_group_name> --query "[0].value" --output tsv)"

az storage container create --name testcontainer

touch testfile.txt
az storage blob upload --file testfile.txt --container-name testcontainer --name testfile.txt
```

Olayı tetiklediniz ve Event Grid iletiyi abone olurken yapılandırdığınız uç noktaya gönderdi. Daha önce oluşturduğunuz uç nokta URL'sine gidin. Veya açık tarayıcınızda Yenile'yi tıklatın. Az önce gönderdiğiniz olayı görürsünüz. 

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
