---
title: "Azure olay kılavuz ve Event Hubs ile tümleştirme"
description: "Bir SQL Data Warehouse'a veri taşımak için Azure olay kılavuz ve olay hub'ları kullanmayı açıklar"
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/19/2018
ms.author: tomfitz
ms.openlocfilehash: b315bd77a47a6f106c5768da56828a5169de5fe9
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2018
---
# <a name="stream-big-data-into-a-data-warehouse"></a>Bir veri ambarında büyük veri akışı

Azure [olay kılavuz](overview.md) uygulamaları ve Hizmetleri için bildirimler tepki olanak tanıyan bir akıllı olay yönlendirme hizmetidir. [Olay hub'ları yakalama ve olay kılavuz örnek](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) Azure olay hub'ları yakalama Azure olay kılavuz ile sorunsuz bir şekilde verileri SQL Data Warehouse için bir olay hub'ından geçirmek için nasıl kullanılacağını gösterir.

![Uygulama genel bakış](media/event-grid-event-hubs-integration/overview.png)

Olay hub'ına gönderilen veri olarak yakalama akıştan veri çeker ve Avro biçiminde verilerle depolama BLOB oluşturur. Yakalama blob oluşturduğunda, bir olay tetikler. Olay kılavuz abonelere olayla ilgili veri dağıtır. Bu durumda, olay verileri Azure işlevleri uç noktasına gönderilir. Olay verileri oluşturulan BLOB yolu içerir. İşlevi dosyasını alın ve veri ambarına veri göndermek için bu URL'yi kullanır.

Bu makalede:

* Aşağıdaki altyapısını dağıtın:
  * Olay hub'ı etkin yakalama ile
  * Yakalama dosyaları için depolama hesabı
  * Azure uygulama hizmeti planı işlev uygulaması barındırma
  * Olay işleme için işlev uygulaması
  * SQL Server'ın veri ambarına barındırma için
  * Geçirilen verileri depolamak için SQL veri ambarı
* Veri ambarında bir tablo oluştur
* İşlev uygulaması için kod ekleme
* Olaya abone olma
* Verileri event hub'ına gönderir uygulamayı çalıştırma
* Veri ambarında geçirilen verileri görüntüleme

## <a name="about-the-event-data"></a>Olay verileri hakkında

Olay kılavuz olay verilerini abonelere dağıtır. Aşağıdaki örnek, bir yakalama dosyasını oluşturmak için olay verilerini gösterir. Özellikle, fark `fileUrl` özelliğinde `data` nesnesi. İşlev uygulaması, bu değeri alır ve yakalama dosyasını almak için kullanır.

```json
[
    {
        "topic": "/subscriptions/<guid>/resourcegroups/rgDataMigrationSample/providers/Microsoft.EventHub/namespaces/tfdatamigratens",
        "subject": "eventhubs/hubdatamigration",
        "eventType": "Microsoft.EventHub.CaptureFileCreated",
        "eventTime": "2017-08-31T19:12:46.0498024Z",
        "id": "14e87d03-6fbf-4bb2-9a21-92bd1281f247",
        "data": {
            "fileUrl": "https://tf0831datamigrate.blob.core.windows.net/windturbinecapture/tfdatamigratens/hubdatamigration/1/2017/08/31/19/11/45.avro",
            "fileType": "AzureBlockBlob",
            "partitionId": "1",
            "sizeInBytes": 249168,
            "eventCount": 1500,
            "firstSequenceNumber": 2400,
            "lastSequenceNumber": 3899,
            "firstEnqueueTime": "2017-08-31T19:12:14.674Z",
            "lastEnqueueTime": "2017-08-31T19:12:44.309Z"
        }
    }
]
```

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için şunlara sahip olmalısınız:

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
* [Visual studio 2017 sürüm 15.3.2 veya daha büyük](https://www.visualstudio.com/vs/) iş yükleri için ile: .NET masaüstü geliştirme, Azure geliştirme, ASP.NET ve web geliştirme, Node.js geliştirme ve Python geliştirme.
* [EventHubsCaptureEventGridDemo örnek proje](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) bilgisayarınıza indirilmeden.

## <a name="deploy-the-infrastructure"></a>Altyapıyı

Bu makalede basitleştirmek için gerekli altyapıyı Resource Manager şablonu ile dağıtın. Dağıtılan kaynakları görmek için görüntüleme [şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/event-grid/EventHubsDataMigration.json). Önizleme sürümü için olay kılavuz destekleyen **westus2** ve **westcentralus** bölgeleri. Bu bölgeler için kaynak grubu konumu kullanın.

Azure CLI için şunu kullanın:

```azurecli-interactive
az group create -l westcentralus -n rgDataMigrationSample

az group deployment create \
  --resource-group rgDataMigrationSample \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/EventHubsDataMigration.json \
  --parameters eventHubNamespaceName=<event-hub-namespace> eventHubName=hubdatamigration sqlServerName=<sql-server-name> sqlServerUserName=<user-name> sqlServerPassword=<password> sqlServerDatabaseName=<database-name> storageName=<unique-storage-name> functionAppName=<app-name>
```

PowerShell için şunu kullanın:

```powershell
New-AzureRmResourceGroup -Name rgDataMigration -Location westcentralus

New-AzureRmResourceGroupDeployment -ResourceGroupName rgDataMigration -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/EventHubsDataMigration.json -eventHubNamespaceName <event-hub-namespace> -eventHubName hubdatamigration -sqlServerName <sql-server-name> -sqlServerUserName <user-name> -sqlServerDatabaseName <database-name> -storageName <unique-storage-name> -functionAppName <app-name>
```

Sorulduğunda bir parola değeri sağlayın.

## <a name="create-a-table-in-sql-data-warehouse"></a>SQL Data Warehouse'da tablo oluşturma

Tablo verilerinizi veri ambarına çalıştırarak ekleme [CreateDataWarehouseTable.sql](https://github.com/Azure/azure-event-hubs/blob/master/samples/e2e/EventHubsCaptureEventGridDemo/scripts/CreateDataWarehouseTable.sql) komut dosyası. Komut dosyasını çalıştırmak için portalda Visual Studio'ya veya sorgu Düzenleyicisi'ni kullanın.

Komut dosyasının çalışmasını şöyledir:

```sql
CREATE TABLE [dbo].[Fact_WindTurbineMetrics] (
    [DeviceId] nvarchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL, 
    [MeasureTime] datetime NULL, 
    [GeneratedPower] float NULL, 
    [WindSpeed] float NULL, 
    [TurbineSpeed] float NULL
)
WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
```

## <a name="publish-the-azure-functions-app"></a>Azure işlevleri uygulama yayımlama

1. Açık [EventHubsCaptureEventGridDemo örnek proje](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) Visual Studio 2017'ın (15.3.2 veya daha büyük).

2. Çözüm Gezgini'nde sağ **FunctionDWDumper**seçip **Yayımla**.

   ![İşlev uygulaması yayımlama](media/event-grid-event-hubs-integration/publish-function-app.png)

3. Seçin **Azure işlev uygulaması** ve **Varolanı seç**. **Tamam**’ı seçin.

   ![Hedef işlev uygulaması](media/event-grid-event-hubs-integration/pick-target.png)

4. Şablonu aracılığıyla dağıtılan işlev uygulaması seçin. **Tamam**’ı seçin.

   ![İşlev uygulaması seçin](media/event-grid-event-hubs-integration/select-function-app.png)

5. Visual Studio profil şekilde yapılandırdığında seçin **Yayımla**.

   ![Select yayımlama](media/event-grid-event-hubs-integration/select-publish.png)

6. İşlev yayımlandıktan sonra Git [Azure portal](https://portal.azure.com/). Kaynak grubu ve işlev uygulamanızı seçin.

   ![İşlev uygulamayı görüntüle](media/event-grid-event-hubs-integration/view-function-app.png)

7. İşlevi seçin.

   ![SELECT işlevi](media/event-grid-event-hubs-integration/select-function.png)

8. URL işlevi için alın. Bu URL, olay abonelik oluştururken gerekir.

   ![İşlev URL'sini al](media/event-grid-event-hubs-integration/get-function-url.png)

9. Değerini kopyalayın.

   ![URL'yi kopyalayın](media/event-grid-event-hubs-integration/copy-url.png)

## <a name="subscribe-to-the-event"></a>Olaya abone olma

Olaya abone olmak için Azure CLI veya Portalı'nı kullanabilirsiniz. Bu makalede her iki yaklaşımın gösterilmektedir.

### <a name="portal"></a>Portal

1. Olay hub'ları ad alanından seçin **olay kılavuz** soldaki.

   ![Olay kılavuz seçin](media/event-grid-event-hubs-integration/select-event-grid.png)

2. Bir olay aboneliği ekleyin.

   ![Olay aboneliği Ekle](media/event-grid-event-hubs-integration/add-event-subscription.png)

3. Olay aboneliği için değerler sağlayın. Kopyaladığınız Azure işlevleri URL kullanın. **Oluştur**’u seçin.

   ![Abonelik değerleri sağlayın](media/event-grid-event-hubs-integration/provide-values.png)

### <a name="azure-cli"></a>Azure CLI

Olaya abone olmak için aşağıdaki komutları çalıştırın (sürüm 2.0.24 gerektiren veya Azure CLI sonraki):

```azurecli-interactive
namespaceid=$(az resource show --namespace Microsoft.EventHub --resource-type namespaces --name <your-EventHubs-namespace> --resource-group rgDataMigrationSample --query id --output tsv)
az eventgrid event-subscription create \
  --resource-id $namespaceid \
  --name captureEventSub \
  --endpoint <your-function-endpoint>
```

## <a name="run-the-app-to-generate-data"></a>Verileri oluşturmak için uygulama çalıştırma

Olay hub'ı, SQL data warehouse, Azure işlev uygulaması ve olay aboneliği ayarlama tamamladınız. Olay hub'ından veri ambarına veri geçirmeye hazır çözümüdür. Olay hub'ına yönelik veri oluşturan bir uygulamayı çalıştırmadan önce birkaç değerlerini yapılandırmanız gerekir.

1. Portalda, olay hub'ı ad alanınızı seçin. Seçin **bağlantı dizeleri**.

   ![Bağlantı dizelerini seçin](media/event-grid-event-hubs-integration/event-hub-connection.png)

2. Seçin **RootManageSharedAccessKey**

   ![Anahtarı seçin](media/event-grid-event-hubs-integration/show-root-key.png)

3. Kopya **bağlantı dizesi - birincil anahtar**

   ![Anahtarı kopyalayın](media/event-grid-event-hubs-integration/copy-key.png)

4. Visual Studio projenizi geri dönün. WindTurbineDataGenerator projeyi açın **program.cs**.

5. İki sabit değerleri değiştirin. Kopyalanan değeri kullanmak **EventHubConnectionString**. Olay hub adını kullanmak **EventHubName**.

   ```cs
   private const string EventHubConnectionString = "Endpoint=sb://tfdatamigratens.servicebus.windows.net/...";
   private const string EventHubName = "hubdatamigration";
   ```

6. Çözümü oluşturun. WindTurbineGenerator.exe uygulamayı çalıştırın. Birkaç dakika sonra geçirilen veriler için veri ambarınız tabloda sorgu.

## <a name="next-steps"></a>Sonraki adımlar

* Olay kılavuz giriş için bkz: [hakkında olay kılavuz](overview.md).
* Hub olayı yakalamak için bir giriş için bkz: [etkinleştirmek olay hub'ları Azure portalını kullanarak yakalama](../event-hubs/event-hubs-capture-enable-through-portal.md).
* Ayarlama ve örnek çalıştırma hakkında daha fazla bilgi için bkz: [olay hub'ları yakalama ve olay kılavuz örnek](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo).