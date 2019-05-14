---
title: Event Hubs verilerini veri ambarı - Event Grid Gönder
description: Bir SQL Data Warehouse'a veri taşımak için Azure Event Grid ve olay hub'ları kullanmayı açıklar. Yakalama dosyası almak için bir Azure işlevi kullanır.
services: event-grid
author: spelluru
manager: timlt
ms.service: event-grid
ms.topic: tutorial
ms.date: 01/13/2019
ms.author: spelluru
ms.openlocfilehash: 1ae7a18660d2a7324bc5897d6b3952da42b6c4b2
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65603448"
---
# <a name="tutorial-stream-big-data-into-a-data-warehouse"></a>Öğretici: Büyük verileri bir veri ambarına akışla aktarma
Azure [Event Grid](overview.md) uygulama ve hizmetlerden (olayları) bildirimler için react olanak tanıyan bir akıllı bir olay yönlendirme hizmetidir. Örneğin, bir Azure Blob Depolama veya Azure Data Lake Storage yakalanan Event Hubs verilerini işlemek için bir Azure işlevi tetikleyin ve verileri diğer veri depolarına geçirin. Bu [Event Hubs ve Event Grid tümleştirmesi örnek](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) Event Hubs Event Grid ile sorunsuz bir şekilde yakalanan Event Hubs verilerini blob depolamadan SQL veri ambarı'na geçirmek için nasıl kullanılacağını gösterir.

![Uygulamaya genel bakış](media/event-grid-event-hubs-integration/overview.png)

Bu diyagramda Bu öğreticide oluşturduğunuz çözüm, iş akışı gösterilmektedir: 

1. Azure olay hub'ına gönderilen veriler Azure blob depolama içinde yakalanır.
2. Veri yakalama işlemi tamamlandığında, bir olay oluşturulur ve bir Azure event grid için gönderdi. 
3. Bir Azure işlev uygulaması bu olay verilerini event grid iletir.
4. İşlev uygulaması, blob depolamadan almak için olay verileri blob URL'sini kullanır. 
5. İşlev uygulaması, bir Azure SQL veri ambarı'na blob verileri geçirir. 

Bu makalede, aşağıdaki adımları uygulayın:

> [!div class="checklist"]
> * Altyapı dağıtmak için bir Azure Resource Manager şablonu kullanın: bir olay hub'ı, bir depolama hesabı, bir işlev uygulaması, SQL veri ambarı.
> * Veri ambarı'nda bir tablo oluşturun.
> * İşlev uygulaması için kod ekleyin.
> * Olaya abone olun. 
> * Olay hub'ına veri gönderen uygulamasını çalıştırın.
> * Geçirilen veriler, veri ambarı'nda görüntüleyin.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu öğreticiyi tamamlamak için aşağıdakiler gereklidir:

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* [Visual studio 2019](https://www.visualstudio.com/vs/) ile iş yükleri için: .NET Masaüstü uygulama geliştirme, Azure geliştirme, ASP.NET ve web geliştirme, Node.js geliştirme ve Python geliştirme.
* İndirme [EventHubsCaptureEventGridDemo kodunuzla](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) bilgisayarınıza.

## <a name="deploy-the-infrastructure"></a>Altyapıyı dağıtma
Bu adımda, gerekli altyapı ile dağıttığınız bir [Resource Manager şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/event-grid/EventHubsDataMigration.json). Şablonu dağıtırken aşağıdaki kaynaklar oluşturulur:

* Olay hub'ı ile yakalama özelliğini etkinleştirebilirsiniz.
* Yakalanan dosyaları için depolama hesabı. 
* İşlev uygulamasını barındırmak için app service planı
* Olayı işlemek için işlev uygulaması
* Veri ambarını barındırmak için SQL Server
* Geçirilen verileri depolamak için SQL Veri Ambarı

### <a name="launch-azure-cloud-shell-in-azure-portal"></a>Azure portalında Azure Cloud Shell'i Başlat

1. [Azure Portal](https://portal.azure.com) oturum açın. 
2. Seçin **Cloud Shell** üstünde düğme.

    ![Azure portal](media/event-grid-event-hubs-integration/azure-portal.png)
3. Cloud Shell tarayıcı alt kısmındaki açılan görürsünüz.

    ![Cloud Shell](media/event-grid-event-hubs-integration/launch-cloud-shell.png) 
4. Cloud shell'de arasından seçmek için bir seçenek görürseniz, **Bash** ve **PowerShell**seçin **Bash**. 
5. Cloud Shell ilk kez kullanıyorsanız, depolama hesabı seçerek oluşturma **depolama oluşturma**. Azure Cloud Shell, bazı dosyaları depolamak için bir Azure depolama hesabı gerektirir. 

    ![Cloud shell için depolama oluşturma](media/event-grid-event-hubs-integration/create-storage-cloud-shell.png)
6. Cloud Shell başlatılana dek bekleyin. 

    ![Cloud shell için depolama oluşturma](media/event-grid-event-hubs-integration/cloud-shell-initialized.png)


### <a name="use-azure-cli"></a>Azure CLI kullanma

1. Aşağıdaki CLI komutunu çalıştırarak Azure kaynak grubu oluşturun: 
    1. Cloud Shell penceresine aşağıdaki komutu yapıştırın

        ```azurecli
        az group create -l eastus -n <Name for the resource group>
        ```
    1. İçin bir ad belirtin **kaynak grubu**
    2. **ENTER**'a basın. 

        Örnek aşağıda verilmiştir:
    
        ```azurecli
        user@Azure:~$ az group create -l eastus -n ehubegridgrp
        {
          "id": "/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups/ehubegridgrp",
          "location": "eastus",
          "managedBy": null,
          "name": "ehubegridgrp",
          "properties": {
            "provisioningState": "Succeeded"
          },
          "tags": null
        }
        ```
2. Aşağıdaki CLI komutunu çalıştırarak (olay hub'ı, depolama hesabı, işlev uygulaması, SQL veri ambarı) önceki bölümde bahsedilen tüm kaynakları dağıtma: 
    1. Kopyalayıp Cloud Shell penceresine komutu yapıştırın. Alternatif olarak, tercih ettiğiniz bir düzenleyiciye kopyala/yapıştır, değerleri ayarlayın ve sonra Cloud Shell için komutu kopyalayın isteyebilirsiniz. 

        ```azurecli
        az group deployment create \
            --resource-group rgDataMigrationSample \
            --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/EventHubsDataMigration.json \
            --parameters eventHubNamespaceName=<event-hub-namespace> eventHubName=hubdatamigration sqlServerName=<sql-server-name> sqlServerUserName=<user-name> sqlServerPassword=<password> sqlServerDatabaseName=<database-name> storageName=<unique-storage-name> functionAppName=<app-name>
        ```
    2. Aşağıdaki varlıklar için değerleri belirtin:
        1. Daha önce oluşturduğunuz kaynak grubunun adı.
        2. Olay hub'ı ad alanı adı. 
        3. Olay hub'ı adı. Değer (hubdatamigration) olduğu gibi bırakabilirsiniz.
        4. SQL server adı.
        5. SQL kullanıcı adı ve parola adı. 
        6. SQL veri ambarı için ad
        7. Depolama hesabının adıdır. 
        8. İşlev uygulamasının adı. 
    3.  Tuşuna **ENTER** komutu çalıştırmak için Cloud Shell penceresinde. Kaynakları bir grup oluşturuyorsanız olduğundan bu işlem biraz zaman alabilir. Komutu sonucu herhangi bir hata olduğunu emin olun. 
    

### <a name="use-azure-powershell"></a>Azure PowerShell kullanma

1. Azure Cloud Shell'de PowerShell moduna geçin. Azure Cloud Shell sol üst köşesindeki ok tuşunu seçip **PowerShell**.

    ![PowerShell'e geçiş](media/event-grid-event-hubs-integration/select-powershell-cloud-shell.png)
2. Aşağıdaki komutu çalıştırarak bir Azure kaynak grubu oluşturun: 
    1. Kopyalayıp Cloud Shell penceresine aşağıdaki komutu yapıştırın.

        ```powershell
        New-AzResourceGroup -Name rgDataMigration -Location westcentralus
        ```
    2. İçin bir ad belirtin **kaynak grubu**.
    3. Press ENTER. 
3. Aşağıdaki komutu çalıştırarak (olay hub'ı, depolama hesabı, işlev uygulaması, SQL veri ambarı) önceki bölümde bahsedilen tüm kaynakları dağıtma:
    1. Kopyalayıp Cloud Shell penceresine komutu yapıştırın. Alternatif olarak, tercih ettiğiniz bir düzenleyiciye kopyala/yapıştır, değerleri ayarlayın ve sonra Cloud Shell için komutu kopyalayın isteyebilirsiniz. 

        ```powershell
        New-AzResourceGroupDeployment -ResourceGroupName rgDataMigration -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/EventHubsDataMigration.json -eventHubNamespaceName <event-hub-namespace> -eventHubName hubdatamigration -sqlServerName <sql-server-name> -sqlServerUserName <user-name> -sqlServerDatabaseName <database-name> -storageName <unique-storage-name> -functionAppName <app-name>
        ```
    2. Aşağıdaki varlıklar için değerleri belirtin:
        1. Daha önce oluşturduğunuz kaynak grubunun adı.
        2. Olay hub'ı ad alanı adı. 
        3. Olay hub'ı adı. Değer (hubdatamigration) olduğu gibi bırakabilirsiniz.
        4. SQL server adı.
        5. SQL kullanıcı adı ve parola adı. 
        6. SQL veri ambarı için ad
        7. Depolama hesabının adıdır. 
        8. İşlev uygulamasının adı. 
    3.  Tuşuna **ENTER** komutu çalıştırmak için Cloud Shell penceresinde. Kaynakları bir grup oluşturuyorsanız olduğundan bu işlem biraz zaman alabilir. Komutu sonucu herhangi bir hata olduğunu emin olun. 

### <a name="close-the-cloud-shell"></a>Cloud Shell kapatın 
Cloud shell seçerek kapatmak **Cloud Shell** Portalı'nda düğme (veya) **X** Cloud Shell penceresinin sağ üst köşesindeki düğme. 

### <a name="verify-that-the-resources-are-created"></a>Kaynakların oluşturulduğunu doğrulayın

1. Azure portalında **kaynak grupları** sol menüsünde. 
2. Arama kutusuna kaynak grubunuzun adını girerek kaynak grupları listesini filtreleyin. 
3. Listeden kaynak grubunuzu seçin.

    ![Kaynak grubunuzu seçin](media/event-grid-event-hubs-integration/select-resource-group.png)
4. Kaynak grubu ortamındaki aşağıdaki kaynaklara gördüğünüzü onaylayın:

    ![Kaynak grubundaki kaynaklar](media/event-grid-event-hubs-integration/resources-in-resource-group.png)

### <a name="create-a-table-in-sql-data-warehouse"></a>SQL Veri Ambarında tablo oluşturma
Çalıştırarak veri ambarı'nda Tablo oluşturma [CreateDataWarehouseTable.sql](https://github.com/Azure/azure-event-hubs/blob/master/samples/e2e/EventHubsCaptureEventGridDemo/scripts/CreateDataWarehouseTable.sql) betiği. Betiği çalıştırmak için portalda Visual Studio veya sorgu Düzenleyicisi'ni kullanabilirsiniz. Aşağıdaki adımlar sorgu Düzenleyicisi'ni kullanmayı gösterir: 

1. Kaynak grubundaki kaynaklar listesinde, SQL veri Ambarınızı seçin. 
2. SQL veri ambarı sayfasında seçin **sorgu Düzenleyicisi (Önizleme)** soldaki menüde. 

    ![SQL veri ambarı sayfası](media/event-grid-event-hubs-integration/sql-data-warehouse-page.png)
2. Adını **kullanıcı** ve **parola** SQL server ve seçin için **Tamam**. 

    ![SQL Server kimlik doğrulaması](media/event-grid-event-hubs-integration/sql-server-authentication.png)
4. Sorgu penceresinde, kopyalayın ve aşağıdaki SQL betiğini çalıştırın: 

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

    ![SQL sorgusu çalıştırma](media/event-grid-event-hubs-integration/run-sql-query.png)
5. Bu sekme veya pencerede veri öğreticinin sonunda oluşturulduğunu doğrulayabilirsiniz. böylece açık tutun. 


## <a name="publish-the-azure-functions-app"></a>Azure İşlevleri uygulamasını yayımlama

1. Visual Studio'yu başlatın.
2. Açık **EventHubsCaptureEventGridDemo.sln** yüklediğiniz çözüm [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) ön koşulların bir parçası olarak.
3. Çözüm Gezgini’nde **FunctionEGDWDumper**’a sağ tıklayın ve **Yayımla**’yı seçin.

   ![İşlev uygulaması yayımlama](media/event-grid-event-hubs-integration/publish-function-app.png)
4. Aşağıdaki ekran görürseniz, seçin **Başlat**. 

   ![Başlangıç Yayımla düğmesi](media/event-grid-event-hubs-integration/start-publish-button.png) 
5. İçinde **yayımlama hedefi seçin** sayfasında **var olanı Seç** seçeneğini belirtin ve **profili oluştur**. 

   ![Yayımlama hedefi seçme](media/event-grid-event-hubs-integration/publish-select-existing.png)
6. App Service sayfasında seçin, **Azure aboneliği**seçin **işlev uygulaması** seçin ve kaynak grubu **Tamam**. 

   ![App Service sayfası](media/event-grid-event-hubs-integration/publish-app-service.png) 
1. Visual Studio profili yapılandırdığında **Yayımla**’yı seçin.

   ![Yayımla’yı seçme](media/event-grid-event-hubs-integration/select-publish.png)

İşlevi yayımladıktan sonra olaya abone olmaya hazır olursunuz.

## <a name="subscribe-to-the-event"></a>Olaya abone olma

1. Yeni bir sekme veya yeni bir web tarayıcısı penceresi gidin [Azure portalında](https://portal.azure.com).
2. Azure portalında **kaynak grupları** sol menüsünde. 
3. Arama kutusuna kaynak grubunuzun adını girerek kaynak grupları listesini filtreleyin. 
4. Listeden kaynak grubunuzu seçin.

    ![Kaynak grubunuzu seçin](media/event-grid-event-hubs-integration/select-resource-group.png)
4. App Service planı listeden seçin. 
5. App Service planı sayfasında seçin **uygulamaları** sol menü ve işlev uygulamasını seçin. 

    ![İşlev uygulamanızı seçin](media/event-grid-event-hubs-integration/select-function-app-app-service-plan.png)
6. İşlev uygulaması'nı genişletin, İşlevler'i genişletin ve ardından işlevinizi seçin. 

    ![Azure işlevinizi seçin](media/event-grid-event-hubs-integration/select-function-add-button.png)
7. Seçin **Event Grid aboneliği Ekle** araç. 
8. İçinde **Event Grid aboneliği oluşturmak** sayfasında, aşağıdaki eylemleri gerçekleştirin: 
    1. İçinde **konu ayrıntıları** bölümünde, aşağıdaki eylemleri gerçekleştirin:
        1. Azure aboneliğinizi seçin.
        2. Azure kaynak grubunu seçin.
        3. Event Hubs ad alanını seçin.
    2. İçinde **olay aboneliği ayrıntıları** sayfasında, abonelik için bir ad girin (örneğin: captureEventSub) seçip **Oluştur**. 

        ![Event Grid aboneliği oluşturun](media/event-grid-event-hubs-integration/create-event-subscription.png)

## <a name="run-the-app-to-generate-data"></a>Veri oluşturmak için uygulama çalıştırma
Olay hub’ı, SQL veri ambarı, Azure işlev uygulaması ve olay aboneliğinizi ayarlamayı tamamladınız. Olay hub’ı için veri oluşturan bir uygulamayı çalıştırmadan önce birkaç değeri yapılandırmanız gerekir.

1. Daha önce yaptığınız gibi Azure Portalı'nda, kaynak grubuna gidin. 
2. Event Hubs ad alanını seçin.
3. İçinde **olay hub'ları Namespace** sayfasında **paylaşılan erişim ilkeleri** sol menüsünde.
4. Seçin **RootManageSharedAccessKey** ilkeleri listesinde. 
5. Kopyala düğmesini işaretleyin **bağlantı dizesi-birincil anahtar** metin kutusu. 

    ![Olay hub'ı ad alanı için bağlantı dizesi](media/event-grid-event-hubs-integration/get-connection-string.png)
1. Visual Studio çözümünüzü geri dönün. 
2. WindTurbineDataGenerator projesinde **program.cs** dosyasını açın.
5. İki sabit değeri değiştirin. **EventHubConnectionString** için kopyalanan değeri kullanın. Olay hub’ının adı için **hubdatamigration** kullanın. Olay hub'ı için farklı bir ad kullandıysanız, bu adı belirtin. 

   ```cs
   private const string EventHubConnectionString = "Endpoint=sb://demomigrationnamespace.servicebus.windows.net/...";
   private const string EventHubName = "hubdatamigration";
   ```

6. Çözümü derleyin. Çalıştırma **WindTurbineGenerator.exe** uygulama. 
7. Birkaç dakika sonra, veri ambarınızdaki tabloda geçirilen verileri sorgulayın.

    ![Sorgu sonuçları](media/event-grid-event-hubs-integration/query-results.png)

### <a name="event-data-generated-by-the-event-hub"></a>Olay hub'ı tarafından oluşturulan olay verilerini
Event Grid, olay verilerini abonelere dağıtır. Aşağıdaki örnek, bir blob'a veri bir olay hub'ı aracılığıyla akış yakalandığında oluşturulan olay verilerini gösterir. Özellikle dikkat edin `fileUrl` özelliğinde `data` nesne depolamada blob işaret eder. İşlev uygulaması, Yakalanan veriler ile blob dosyasını almak için bu URL'yi kullanır.

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


## <a name="next-steps"></a>Sonraki adımlar

* Azure mesajlaşma servislerindeki farklar hakkında bilgi sahibi olmak için, bkz. [iletiler teslim eden Azure hizmetleri arasında seçim yapma](compare-messaging-services.md).
* Event Grid’e giriş için bkz. [Event Grid hakkında](overview.md).
* Event Hubs Capture’a giriş için bkz. [Azure portalını kullanarak Event Hubs Capture’ı etkinleştirme](../event-hubs/event-hubs-capture-enable-through-portal.md).
* Örneği ayarlama ve çalıştırma hakkında daha fazla bilgi için bkz. [Event Hubs Capture ve Event Grid örneği](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo).
