---
title: Özel web uç noktasına - Powershell Azure Blob Depolama olayları rota | Microsoft Docs
description: Blob depolama olaylarına abone olmak için Azure Event Grid’i kullanın.
services: storage,event-grid
keywords: ''
author: david-stanford
ms.author: dastanfo
ms.date: 05/24/2018
ms.topic: article
ms.service: storage
ms.openlocfilehash: b6764ffa0e7cfbc888f11c22af855d48d8160372
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34650511"
---
# <a name="route-blob-storage-events-to-a-custom-web-endpoint-with-powershell"></a>PowerShell ile bir özel web uç noktası için rota Blob Depolama olayları

Azure Event Grid, bulut için bir olay oluşturma hizmetidir. Bu makalede, Blob Depolama olayları, tetikleyici bir olay abone olmak için Azure PowerShell kullanın ve sonucu görüntüleyin. 

Genellikle, olayları olay verilerini işler ve eylemleri gerçekleştirir bir uç nokta için gönderin. Ancak, bu makalede basitleştirmek için olayları toplayan ve iletileri görüntüleyen bir web uygulaması gönderin.

İşlemi tamamladığınızda, olay verilerini web uygulamasına gönderildi bakın.

![Sonuçları görüntüle](./media/storage-blob-event-quickstart-powershell/view-results.png)

## <a name="setup"></a>Kurulum

Bu makale için Azure PowerShell'in en yeni sürümünü kullanmanız gerekir. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/install-azurerm-ps).

## <a name="log-in-to-azure"></a>Azure'da oturum açma

`Connect-AzureRmAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyerek kimlik doğrulaması yapın.

```powershell
Connect-AzureRmAccount
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

LRS çoğaltma kullanarak Blob Depolama hesabı oluşturma [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount), sonra kullanılacak depolama hesabını tanımlayan depolama hesabı bağlamını alır. Depolama hesabında bir işlem gerçekleştirirken, kimlik bilgilerini tekrar tekrar sağlamak yerine bağlama başvurursunuz. Bu örnek adlı bir depolama hesabı oluşturur **gridstorage** yerel olarak yedekli depolama (LRS). 

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

Konuya abone olmadan önce olay iletisi için uç noktayı oluşturalım. Genellikle, uç nokta olay verileri temel alan eylemleri gerçekleştirir. Bu hızlı başlangıç basitleştirmek için dağıttığınız bir [önceden oluşturulmuş web uygulaması](https://github.com/dbarkol/azure-event-grid-viewer) olay iletileri görüntüler. Dağıtılan çözümü, bir uygulama hizmeti planı, bir App Service web uygulaması ve github'dan kaynak kodunu içerir.

Değiştir `<your-site-name>` web uygulamanız için benzersiz bir ada sahip. DNS girişini parçası olduğu için web uygulaması adı benzersiz olmalıdır.

```powershell
$sitename="<your-site-name>"

New-AzureRmResourceGroupDeployment `
  -ResourceGroupName $resourceGroup `
  -TemplateUri "https://raw.githubusercontent.com/dbarkol/azure-event-grid-viewer/master/azuredeploy.json" `
  -siteName $sitename `
  -hostingPlanName viewerhost
```

Dağıtımın tamamlanması birkaç dakika sürebilir. Dağıtım başarılı olduktan sonra emin olmak için web uygulamanızı görüntülemek çalışıyor. Bir web tarayıcısında gidin: `https://<your-site-name>.azurewebsites.net`

Site şu anda görüntülenen hiçbir iletiler görmeniz gerekir.

## <a name="subscribe-to-your-storage-account"></a>Depolama hesabınıza abone olma

Event Grid’e hangi olayları izlemek istediğinizi bildirmek için bir konuya abone olursunuz. Aşağıdaki örnek, depolama hesabı oluşturulur ve olay bildirimi için uç noktaya olarak web uygulamanızdan URL geçirir abone olur. Web uygulamanız için uç noktaya soneki içermelidir `/api/updates/`.

```powershell
$storageId = (Get-AzureRmStorageAccount -ResourceGroupName $resourceGroup -AccountName $storageName).Id
$endpoint="https://$sitename.azurewebsites.net/api/updates"

New-AzureRmEventGridSubscription `
  -EventSubscriptionName gridBlobQuickStart `
  -Endpoint $endpoint `
  -ResourceId $storageId
```

Web uygulamanızı yeniden görüntülemek ve bir abonelik doğrulama olayı için gönderildiğini dikkat edin. Olay verileri genişletmek için göz simgesini seçin. Uç nokta, olay verileri almak istediği doğrulayabilmeniz için olay kılavuz doğrulama olay gönderir. Web uygulamasının abonelik doğrulamak için kodu içerir.

![Abonelik olayı görüntüle](./media/storage-blob-event-quickstart-powershell/view-subscription-event.png)

## <a name="trigger-an-event-from-blob-storage"></a>Blob depolama biriminden bir olay tetikler

Şimdi, Event Grid’in iletiyi uç noktanıza nasıl dağıttığını görmek için bir olay tetikleyelim. İlk olarak, bir kapsayıcı ve nesneyi oluşturalım. Ardından, şimdi nesnesi kapsayıcıya karşıya yükleyin.

```powershell
$containerName = "gridcontainer"
New-AzureStorageContainer -Name $containerName -Context $ctx

echo $null >> gridTestFile.txt

Set-AzureStorageBlobContent -File gridTestFile.txt -Container $containerName -Context $ctx -Blob gridTestFile.txt
```

Olayı tetiklediniz ve Event Grid iletiyi abone olurken yapılandırdığınız uç noktaya gönderdi. Yalnızca gönderilen olay görmek için web uygulamanızın görüntüleyin.

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
