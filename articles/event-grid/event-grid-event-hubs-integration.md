---
title: Event Hubs verilerini veri ambarı - Event Grid Gönder
description: Bir SQL Data Warehouse'a veri taşımak için Azure Event Grid ve olay hub'ları kullanmayı açıklar. Yakalama dosyası almak için bir Azure işlevi kullanır.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: tutorial
ms.date: 08/22/2018
ms.author: tomfitz
ms.openlocfilehash: 0b77d0cc32464fe8b7ac28f491f2cb23b0790ba7
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53097616"
---
# <a name="tutorial-stream-big-data-into-a-data-warehouse"></a>Öğretici: bir veri ambarına büyük veri Stream

Azure [Event Grid](overview.md), uygulama ve hizmetlerden bildirimlere yanıt vermenize olanak tanıyan akıllı bir olay yönlendirme hizmetidir. Örneğin bir Azure İşlevini tetikleyerek Azure Blob depolama alanına veya Data Lake Store'a alınan Event Hubs verilerinin işlenmesini ve verilerin farklı veri depolarına geçirilmesini sağlayabilir. [Event Hubs Capture ve Event Grid örneği](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo), blob depolama alanındaki Event Hubs verilerini SQL Veri Ambarına sorunsuzca geçirmek üzere Event Hubs Capture’ı Event Grid ile birlikte kullanmayı gösterir.

![Uygulamaya genel bakış](media/event-grid-event-hubs-integration/overview.png)

Veriler olay hub’ına gönderildikçe Capture akıştan verileri çeker ve Avro biçiminde verilerle depolama blobları oluşturur. Capture blob’u oluşturduğunda bir olay tetikler. Event Grid, abonelere olay hakkında veriler dağıtır. Bu durumda, olay verileri Azure İşlevleri uç noktasına gönderilir. Olay verileri, oluşturulan blobun yolunu içerir. İşlev bu URL’yi kullanarak dosyayı alır ve veri ambarına gönderir.

Bu makalede şunları yapacaksınız:

* Aşağıdaki altyapıyı dağıtma:
  * Capture etkinken Olay hub'ı
  * Capture dosyaları için depolama hesabı
  * İşlev uygulamasını barındırmak için Azure app service planı
  * Olayı işlemek için işlev uygulaması
  * Veri ambarını barındırmak için SQL Server
  * Geçirilen verileri depolamak için SQL Veri Ambarı
* Veri ambarında tablo oluşturma
* İşlev uygulamasına kod ekleme
* Olaya abone olma
* Olay hub'ına veri gönderen uygulamayı çalıştırma
* Veri ambarındaki geçirilmiş verileri görüntüleme

## <a name="about-the-event-data"></a>Olay verileri hakkında

Event Grid, olay verilerini abonelere dağıtır. Aşağıdaki örnekte bir Capture dosyası oluşturmaya ilişkin olay verileri gösterilmiştir. Özellikle `data` nesnesindeki `fileUrl` özelliğine dikkat edin. İşlev uygulaması bu değeri alır ve Capture dosyasını almak için kullanır.

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

Bu öğreticiyi tamamlamak için aşağıdakiler gereklidir:

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
* [Visual Studio 2017 Sürüm 15.3.2 veya üzeri](https://www.visualstudio.com/vs/) ile şunlar için iş yükleri: .NET masaüstü geliştirme, Azure geliştirme, ASP.NET ve web geliştirme, Node.js geliştirme ve Python geliştirme.
* Bilgisayarınıza indirilmiş [EventHubsCaptureEventGridDemo örnek projesi](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo).

## <a name="deploy-the-infrastructure"></a>Altyapıyı dağıtma

Bu makaleyi basitleştirmek için gerekli altyapıyı bir Resource Manager şablonuna dağıtın. Dağıtılan kaynakları görmek için [şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/event-grid/EventHubsDataMigration.json) görüntüleyin.

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

Sorulduğunda bir parola değeri belirtin.

## <a name="create-a-table-in-sql-data-warehouse"></a>SQL Veri Ambarında tablo oluşturma

[CreateDataWarehouseTable.sql](https://github.com/Azure/azure-event-hubs/blob/master/samples/e2e/EventHubsCaptureEventGridDemo/scripts/CreateDataWarehouseTable.sql) betiğini çalıştırarak veri ambarınıza bir tablo ekleyin. Betiği çalıştırmak için portalda Visual Studio veya Sorgu Düzenleyicisi'ni kullanın.

Çalıştırılacak betik:

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

## <a name="publish-the-azure-functions-app"></a>Azure İşlevleri uygulamasını yayımlama

1. Visual Studio 2017’de (15.3.2 veya üzeri) [EventHubsCaptureEventGridDemo örnek projesini](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) açın.

1. Çözüm Gezgini’nde **FunctionEGDWDumper**’a sağ tıklayın ve **Yayımla**’yı seçin.

   ![İşlev uygulaması yayımlama](media/event-grid-event-hubs-integration/publish-function-app.png)

1. **Azure İşlev Uygulaması** ve **Var Olanı Seç**’i seçin. **Yayımla**’yı seçin.

   ![Hedef işlev uygulaması](media/event-grid-event-hubs-integration/pick-target.png)

1. Şablon aracılığıyla dağıttığınız işlev uygulamasını seçin. **Tamam**’ı seçin.

   ![İşlev uygulaması seçme](media/event-grid-event-hubs-integration/select-function-app.png)

1. Visual Studio profili yapılandırdığında **Yayımla**’yı seçin.

   ![Yayımla’yı seçme](media/event-grid-event-hubs-integration/select-publish.png)

İşlevi yayımladıktan sonra olaya abone olmaya hazır olursunuz.

## <a name="subscribe-to-the-event"></a>Olaya abone olma

1. [Azure Portal](https://portal.azure.com/) gidin. Kaynak grubunuzu ve işlev uygulamanızı seçin.

   ![İşlev uygulamasını görüntüleme](media/event-grid-event-hubs-integration/view-function-app.png)

1. İşlevi seçin.

   ![İşlev seçme](media/event-grid-event-hubs-integration/select-function.png)

1. **Event Grid aboneliği ekle**’yi seçin.

   ![Abonelik ekleme](media/event-grid-event-hubs-integration/add-event-grid-subscription.png)

9. Event grid aboneliğine bir ad verin. Olay türü olarak **Event Hubs Ad Alanları**’nı kullanın. Event Hubs ad alanı örneğinizi seçmek için değerler sağlayın. Abone uç noktasını sağlanan değer olarak bırakın. **Oluştur**’u seçin.

   ![Abonelik oluşturma](media/event-grid-event-hubs-integration/set-subscription-values.png)

## <a name="run-the-app-to-generate-data"></a>Veri oluşturmak için uygulama çalıştırma

Olay hub’ı, SQL veri ambarı, Azure işlev uygulaması ve olay aboneliğinizi ayarlamayı tamamladınız. Çözüm, olay hub’ından veri ambarına verileri geçirmeye hazırdır. Olay hub’ı için veri oluşturan bir uygulamayı çalıştırmadan önce birkaç değeri yapılandırmanız gerekir.

1. Portalda olay hub'ı ad alanınızı seçin. **Bağlantı Dizeleri**’ni seçin.

   ![Bağlantı dizelerini seçme](media/event-grid-event-hubs-integration/event-hub-connection.png)

2. **RootManageSharedAccessKey** öğesini seçin

   ![Anahtar seçme](media/event-grid-event-hubs-integration/show-root-key.png)

3. **Bağlantı dizesi - birincil anahtar** değerini kopyalayın

   ![Anahtarı kopyalama](media/event-grid-event-hubs-integration/copy-key.png)

4. Visual Studio projenize geri dönün. WindTurbineDataGenerator projesinde **program.cs** dosyasını açın.

5. İki sabit değeri değiştirin. **EventHubConnectionString** için kopyalanan değeri kullanın. Olay hub’ının adı için **hubdatamigration** kullanın.

   ```cs
   private const string EventHubConnectionString = "Endpoint=sb://demomigrationnamespace.servicebus.windows.net/...";
   private const string EventHubName = "hubdatamigration";
   ```

6. Çözümü derleyin. WindTurbineGenerator.exe uygulamasını çalıştırın. Birkaç dakika sonra, veri ambarınızdaki tabloda geçirilen verileri sorgulayın.

## <a name="next-steps"></a>Sonraki adımlar

* Azure mesajlaşma servislerindeki farklar hakkında bilgi sahibi olmak için, bkz. [iletiler teslim eden Azure hizmetleri arasında seçim yapma](compare-messaging-services.md).
* Event Grid’e giriş için bkz. [Event Grid hakkında](overview.md).
* Event Hubs Capture’a giriş için bkz. [Azure portalını kullanarak Event Hubs Capture’ı etkinleştirme](../event-hubs/event-hubs-capture-enable-through-portal.md).
* Örneği ayarlama ve çalıştırma hakkında daha fazla bilgi için bkz. [Event Hubs Capture ve Event Grid örneği](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo).