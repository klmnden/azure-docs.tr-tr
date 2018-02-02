---
title: "Özel web uç noktasına - Powershell Azure Blob Depolama olayları rota | Microsoft Docs"
description: "Blob depolama olaylarına abone olmak için Azure Event Grid’i kullanın."
services: storage,event-grid
keywords: 
author: david-stanford
ms.author: dastanfo
ms.date: 01/30/2018
ms.topic: article
ms.service: storage
ms.openlocfilehash: 329a79511b810159244b5530a49a5916440d2046
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="route-blob-storage-events-to-a-custom-web-endpoint-with-powershell"></a>PowerShell ile bir özel web uç noktası için rota Blob Depolama olayları

Azure Event Grid, bulut için bir olay oluşturma hizmetidir. Bu makalede, Blob Depolama olayları, tetikleyici bir olay abone olmak için Azure PowerShell kullanın ve sonucu görüntüleyin. 

Normalde olayları olaya yanıt veren bir uç noktaya (web kancası veya Azure İşlevi gibi) gönderirsiniz. Bu makalede gösterilen örneği basitleştirmek için olayları yalnızca iletileri toplayan bir URL'ye gönderilir. Bu URL ya da üçüncü taraf araçları kullanarak oluşturduğunuz [RequestBin](https://requestb.in/) veya [Hookbin](https://hookbin.com/).

> [!NOTE]
> **RequestBin** ve **Hookbin** yüksek verimlilik kullanım için tasarlanmamıştır. Bu araçların tamamen demonstrative kullanılır. Aynı anda birden fazla olay gönderirseniz araçta tüm olaylarınızı göremeyebilirsiniz.

Bu makalede açıklanan adımları tamamladıktan sonra olay verilerinin bir uç noktaya gönderildiğini görürsünüz.

![Olay verileri](./media/storage-blob-event-quickstart/request-result.png)

## <a name="setup"></a>Kurulum

Bu makale için Azure PowerShell'in en yeni sürümünü kullanmanız gerekir. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/install-azurerm-ps).

## <a name="log-in-to-azure"></a>Azure'da oturum açma

`Login-AzureRmAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyerek kimlik doğrulaması yapın.

```powershell
Login-AzureRmAccount
```

> [!NOTE]
> Kullanılabilirlik depolama olayları için olay kılavuza bağlı [kullanılabilirlik](../../event-grid/overview.md) ve olay kılavuz yaptığı gibi diğer bölgelerde kullanılabilir hale gelecektir.

Bu örnekte **westus2** ve seçim boyunca kullanmanız için bir değişken depolar.

```powershell
$location = "westus2"
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Event Grid konuları Azure kaynaklarıdır ve bir Azure kaynak grubuna yerleştirilmelidir. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal koleksiyondur.

[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) komutu ile yeni bir kaynak grubu oluşturun.

Aşağıdaki örnek **westus2** konumunda **gridResourceGroup** adlı bir kaynak grubu oluşturur.  

```powershell
$resourceGroup = "gridResourceGroup"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

BLOB Depolama olayları kullanmak için ya da gereken bir [Blob storage hesabı](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#blob-storage-accounts) veya [genel amaçlı v2 depolama hesabı](../common/storage-account-options.md#general-purpose-v2). **Genel amaçlı v2 (GPv2)** BLOB'lar, dosyalar, kuyruklar ve tablolar dahil olmak üzere tüm depolama hizmetleri için tüm özellikleri destekleyen depolama hesaplarıdır. A **Blob storage hesabı** , yapılandırılmamış verilerinizi bloblar (nesneler) olarak Azure Storage depolamak için bir özel depolama hesabı. BLOB storage hesapları, genel amaçlı depolama hesapları gibi ve % 100 API tutarlığı dahil günümüzde blok bloblar için kullanılır ve ilave blobları tüm harika dayanıklılık, kullanılabilirlik, ölçeklenebilirlik ve performans özelliklerini paylaşır. Yalnızca blok veya engelleme blobunun gerektiği uygulamalar için Blob Storage hesaplarının kullanılmasını öneririz.  

LRS çoğaltma kullanarak Blob Depolama hesabı oluşturma [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount), sonra kullanılacak depolama hesabını tanımlayan depolama hesabı bağlamını alır. Bir depolama hesabı üzerinde hareket ederken, tekrar tekrar kimlik bilgileri sağlama yerine bağlamı başvuru. Bu örnek adlı bir depolama hesabı oluşturur **gridstorage** (varsayılan olarak etkindir) yerel olarak yedekli storage(LRS) ve blob şifrelemesi ile. 

> [!NOTE]
> Depolama hesabı adları bir genel ad alanı olduğundan, bu komut dosyasında belirtilen adı rastgele bazı karakterleri eklemek gerekir.

```powershell
$storageName = "gridstorage"
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageName `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind BlobStorage `
  -AccessTier Hot

$ctx = $storageAccount.Context
```

## <a name="create-a-message-endpoint"></a>İleti uç noktası oluşturma

Konuya abone olmadan önce olay iletisi için uç noktayı oluşturalım. Konuya yanıt vermek için kod yazmak yerine, görüntüleyebilmeniz için iletileri toplayan bir uç nokta oluşturalım. RequestBin ve Hookbin bir uç nokta oluşturmanıza olanak sağlayan ve kendisine gönderilen istekleri görüntüleme üçüncü taraf araçlardır. Gidin [RequestBin](https://requestb.in/), tıklatıp **bir RequestBin oluşturma**, veya gitmek [Hookbin](https://hookbin.com/) tıklatıp **yeni uç nokta oluşturma**. Depo URL'yi kopyalayın ve değiştirme `<bin URL>` aşağıdaki komut dosyasında.

```powershell
$binEndPoint = "<bin URL>"
```

## <a name="subscribe-to-your-storage-account"></a>Depolama hesabınıza abone olma

Event Grid’e hangi olayları izlemek istediğinizi bildirmek için bir konuya abone olursunuz. Aşağıdaki örnek, depolama hesabı oluşturulur ve URL uç nokta için olay bildirimi olarak RequestBin veya Hookbin geçirir abone olur. 

```powershell
$storageId = (Get-AzureRmStorageAccount -ResourceGroupName $resourceGroup -AccountName $storageName).Id
New-AzureRmEventGridSubscription `
  -EventSubscriptionName gridBlobQuickStart `
  -Endpoint $binEndPoint `
  -ResourceId $storageId
```

## <a name="trigger-an-event-from-blob-storage"></a>Blob depolama biriminden bir olay tetikler

Şimdi, Event Grid’in iletiyi uç noktanıza nasıl dağıttığını görmek için bir olay tetikleyelim. İlk olarak, bir kapsayıcı ve nesneyi oluşturalım. Ardından, şimdi nesnesi kapsayıcıya karşıya yükleyin.

```powershell
$containerName = "gridcontainer"
New-AzureStorageContainer -Name $containerName -Context $ctx

echo $null >> gridTestFile.txt

Set-AzureStorageBlobContent -File gridTestFile.txt -Container $containerName -Context $ctx -Blob gridTestFile.txt
```

Olayı tetiklediniz ve Event Grid iletiyi abone olurken yapılandırdığınız uç noktaya gönderdi. Daha önce oluşturduğunuz uç nokta URL'sine gidin. Veya açık tarayıcınızda Yenile'yi tıklatın. Az önce gönderdiğiniz olayı görürsünüz. 

```json
[{
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myrg/providers/Microsoft.Storage/storageAccounts/myblobstorageaccount",
  "subject": "/blobServices/default/containers/gridcontainer/blobs/gridTestFile.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-08-16T20:33:51.0595757Z",
  "id": "4d96b1d4-0001-00b3-58ce-16568c064fab",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "Azure-Storage-PowerShell-d65ca2e2-a168-4155-b7a4-2c925c18902f",
    "requestId": "4d96b1d4-0001-00b3-58ce-16568c000000",
    "eTag": "0x8D4E4E61AE038AD",
    "contentType": "application/octet-stream",
    "contentLength": 0,
    "blobType": "BlockBlob",
    "url": "https://myblobstorageaccount.blob.core.windows.net/gridcontainer/gridTestFile.txt",
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

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Artık konu oluşturma ve olay aboneliklerini öğrendiğinize göre, Blob depolama Olayları ve Event Grid’in size nasıl yardımcı olabileceği konusunda daha fazla bilgi edinebilirsiniz:

- [Blob depolama olaylarına yanıt verme](storage-blob-event-overview.md)
- [Event Grid Hakkında](../../event-grid/overview.md)
