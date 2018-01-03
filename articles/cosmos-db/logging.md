---
title: "Azure Cosmos DB tanılama günlük | Microsoft Docs"
description: "Azure Cosmos DB ile başlamanıza yardımcı olması için bu öğreticiyi kullanın günlüğü."
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
tags: azure-resource-manager
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/12/2017
ms.author: mimig
ms.openlocfilehash: c5e85ac6eec1b8b0a5a78f552b190ce3f3c55c38
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/20/2017
---
# <a name="azure-cosmos-db-diagnostic-logging"></a>Azure Cosmos DB Tanılama Günlüğü

Bir veya daha fazla Azure Cosmos DB veritabanı kullanmaya başladıktan sonra izlemek isteyebilir nasıl ve ne zaman veritabanlarınızı erişilir. Azure Cosmos DB'de tanılama günlük kaydını, bu izleme gerçekleştirmenizi sağlar. Tanılama günlüğe kaydetmeyi etkinleştirerek günlüklerini gönderebilir [Azure Storage](https://azure.microsoft.com/services/storage/), bunları akış [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), ve/veya bunları dışarı [günlük analizi](https://azure.microsoft.com/services/log-analytics/), birparçasıolduğu[ Operations Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite).

![Tanılama günlük depolama, olay hub'ları veya günlük analizi aracılığıyla Operations Management Suite](./media/logging/azure-cosmos-db-logging-overview.png)

Azure Cosmos Azure portal, CLI veya PowerShell oturum DB ile çalışmaya başlamak için bu öğreticiyi kullanın.

## <a name="what-is-logged"></a>Ne kaydedilir?

* Erişim izinleri, sistem hataları veya hatalı istekler sonucunda başarısız olan istekleri içeren tüm kimliği doğrulanmış REST SQL API istekleri günlüğe kaydedilir. MongoDB, grafik ve tablo API desteği şu anda kullanılamıyor.
* Tüm belgeler, kapsayıcıları ve veritabanları üzerinde CRUD işlemleri içeren veritabanının kendisi, üzerinde işlemler.
* Oluşturma, değiştirme veya bu anahtarları silme dahil hesabı anahtarları üzerinde işlemler.
* Bir 401 yanıtına neden olan kimliği doğrulanmamış istekler. Örneğin, bir taşıyıcı belirtecine sahip olmayan veya hatalı biçimlendirilmiş ya da süresi dolmuş veya geçersiz bir belirtece sahip olan istekler.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi tamamlamak için aşağıdaki kaynaklara sahip olmalısınız:

* Bir var olan Azure Cosmos DB hesap, veritabanı ve kapsayıcı. Bu kaynaklar oluşturma ile ilgili yönergeler için bkz: [Azure portalını kullanarak bir veritabanı hesabı oluşturma](create-sql-api-dotnet.md#create-a-database-account), [CLI örnekleri](cli-samples.md), veya [PowerShell örnekleri](powershell-samples.md).

<a id="#turn-on"></a>
## <a name="turn-on-logging-in-the-azure-portal"></a>Azure portalında oturum aç

1. İçinde [Azure portal](https://portal.azure.com), Azure Cosmos DB hesap, tıklatın **tanılama günlükleri** sol gezinti ve ardından **tanılamayı açın**.

    ![Azure portalında Azure Cosmos DB için tanılama günlük kaydını etkinleştirmek](./media/logging/turn-on-portal-logging.png)

2. İçinde **tanılama ayarlarını** sayfasında, aşağıdakileri yapın: 

    * **Ad**. Günlükleri oluşturmak için bir ad girin.

    * **Arşiv depolama hesabı**. Bu seçeneği kullanmak için bağlanmak için var olan bir depolama hesabı gerekir. Portalda yeni bir depolama hesabı oluşturmak için bkz: [depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md) ve Kaynak Yöneticisi, genel amaçlı hesabı oluşturmak için yönergeleri izleyin. Depolama hesabınız seçmek için portal bu sayfaya dönün. Yeni oluşturulan depolama hesapları açılır menüde görünmesi birkaç dakika sürebilir.
    * **Bir olay hub'ına akış**. Bu seçeneği kullanmak için bağlanmak için bir var olan olay hub'ı ad alanı ve olay hub'ı gerekir. Bir olay hub'ları ad alanı oluşturmak için bkz: [bir olay hub'ları ad alanı oluşturup Azure portalını kullanarak bir event hub](../event-hubs/event-hubs-create.md). Olay hub'ı ad alanı ve ilke adı seçmek için portal bu sayfaya dönün.
    * **Günlük analizi için Gönder**.     Bu seçeneği kullanmak için varolan bir çalışma alanını kullanın ya da yeni bir günlük analizi çalışma alanı için adımları izleyerek oluşturun [yeni bir çalışma alanı oluşturma](../log-analytics/log-analytics-quick-collect-azurevm.md#create-a-workspace) Portalı'nda. Günlük analizi, günlükleri görüntüleme hakkında daha fazla bilgi için bkz: [görünüm günlüklerini günlük analizi](#view-in-loganalytics).
    * **Oturum DataPlaneRequests**. SQL, grafik ve tablo API hesapları için tanılama günlük için bu seçeneği belirleyin. Bir depolama hesabına arşivleme, tanılama günlüklerini saklama süresi seçebilirsiniz. Bekletme süresi dolduktan sonra günlüklerin autodeleted.
    * **Oturum MongoRequests**. MongoDB API hesapları için tanılama günlük için bu seçeneği belirleyin. Bir depolama hesabına arşivleme, tanılama günlüklerini saklama süresi seçebilirsiniz. Bekletme süresi dolduktan sonra günlüklerin autodeleted.
    * **Ölçüm istekleri**. Ayrıntılı verileri depolamak için bu seçeneği [Azure ölçümleri](../monitoring-and-diagnostics/monitoring-supported-metrics.md). Bir depolama hesabına arşivleme, tanılama günlüklerini saklama süresi seçebilirsiniz. Bekletme süresi dolduktan sonra günlüklerin autodeleted.

3. **Kaydet**’e tıklayın.

    Bildiren bir hata alırsanız, "için tanılama güncelleştirilemedi \<çalışma alanı adı >. Abonelik \<abonelik kimliği > Microsoft.ınsights kullanmak için kayıtlı değil. " izleyin [sorun giderme Azure tanılama](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage) hesabını kaydetmek için yönergeler, bu yordamı yeniden deneyin.

    Nasıl Tanılama günlüklerinize herhangi bir noktada gelecekte kaydedilir değiştirmek istiyorsanız, bu sayfanın dilediğiniz zaman hesabınızı tanılama günlük ayarlarını değiştirmek için geri dönebilirsiniz.

## <a name="turn-on-logging-using-cli"></a>CLI kullanarak oturum aç

Ölçümleri ve Azure CLI kullanarak tanılama günlük kaydını etkinleştirmek için aşağıdaki komutları kullanın:

- Tanılama günlüklerinin bir depolama hesabındaki depolama etkinleştirmek için bu komutu kullanın:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
   ```

   `resourceId` Azure Cosmos DB hesabının adıdır. `storageId` Günlükleri göndermek istediğiniz depolama hesabının adıdır.

- Bir olay Hub'ına tanılama günlüklerini akışını etkinleştirmek için bu komutu kullanın:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
   ```

   `resourceId` Azure Cosmos DB hesabının adıdır. `serviceBusRuleId` Bu biçiminde bir dize:

   ```azurecli-interactive
   {service bus resource ID}/authorizationrules/{key name}
   ```

- Günlük analizi çalışma alanı için tanılama günlüklerini gönderilmesine izin vermek için bu komutu kullanın:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of the log analytics workspace> --enabled true
   ```

Birden çok çıktı seçenekleri etkinleştirmek için bu parametreler birleştirebilirsiniz.

## <a name="turn-on-logging-using-powershell"></a>PowerShell kullanarak oturum aç

PowerShell kullanarak oturum açmak için en az 1.0.1 sürümü ile Azure Powershell gerekir.

Azure PowerShell'i yüklemek ve Azure aboneliğinizle ilişkilendirmek için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).

Azure PowerShell'i zaten yüklediyseniz ve sürümünü PowerShell konsolundan bilmiyorsanız, çalışamazsa `(Get-Module azure -ListAvailable).Version`.  

### <a id="connect"></a>Aboneliklerinize bağlanma
Bir Azure PowerShell oturumu başlatın ve aşağıdaki komutla Azure hesabınızda oturum açın:  

```powershell
Login-AzureRmAccount
```

Açılır tarayıcı penceresinde Azure hesabı kullanıcı adınızı ve parolanızı girin. Azure PowerShell bu hesapla ilişkili tüm abonelikleri alır ve varsayılan olarak birinciyi kullanır.

Birden çok aboneliğiniz varsa Azure Anahtar Kasanızı oluşturmak için kullanılan belirli bir tanesini belirtmeniz gerekebilir. Hesabınız için abonelikleri görmek üzere aşağıdakini yazın:

```powershell
Get-AzureRmSubscription
```

Ardından, oturum açtığınız Azure Cosmos DB hesabıyla ilişkili aboneliği belirtmek için şunu yazın:

```powershell
Set-AzureRmContext -SubscriptionId <subscription ID>
```

> [!NOTE]
> Hesabınızla ilişkili birden çok aboneliğiniz varsa aboneliği belirtmek önemlidir.
>
>

Azure Power Shell'i yapılandırma hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).

### <a id="storage"></a>Günlükleriniz için yeni bir depolama hesabı oluşturma
Bu öğreticide günlükleriniz için var olan depolama hesabını kullanabilirsiniz, ancak Azure Cosmos DB günlüklerine özgü yeni bir depolama hesabı oluşturuyoruz. Kolaylık olması için biz depolama hesabı ayrıntıları adlı bir değişkende depoluyorsanız **sa**.

Ek yönetim kolaylığı için Bu öğreticide aynı kaynak grubunun Azure Cosmos DB Veritabanımıza içeren bir kullanırız. Değerleri ContosoResourceGroup, contosocosmosdblogs ve 'Kuzey Orta ABD' kendi değerlerinizi için uygun şekilde değiştirin:

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName ContosoResourceGroup `
-Name contosocosmosdblogs -Type Standard_LRS -Location 'North Central US'
```

> [!NOTE]
> Mevcut bir depolama hesabını kullanmaya karar verirseniz, aynı abonelik Azure Cosmos DB aboneliğinizi kullanmanız gerekir ve klasik dağıtım modeli yerine Resource Manager dağıtım modeli kullanmanız gerekir.
>
>

### <a id="identify"></a>Günlükleriniz için Azure Cosmos DB hesabını tanımlama
Adlı bir değişkene Azure Cosmos DB hesap adını ayarlayın **hesap**, burada KaynakAdı Azure Cosmos DB hesabının adıdır.

```powershell
$account = Get-AzureRmResource -ResourceGroupName ContosoResourceGroup `
-ResourceName contosocosmosdb -ResourceType "Microsoft.DocumentDb/databaseAccounts"
```

### <a id="enable"></a>Günlüğe kaydetmeyi etkinleştirme
Azure Cosmos DB için günlük kaydını etkinleştirmek için yeni depolama hesabı, Azure Cosmos DB hesabı ve günlükleri etkinleştirmek istediğiniz kategori değişkenleri birlikte Set-AzureRmDiagnosticSetting cmdlet'ini kullanın. Ayarlama aşağıdaki komutu çalıştırın **-etkin** bayrağını **$true**:

```powershell
Set-AzureRmDiagnosticSetting  -ResourceId $account.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories DataPlaneRequests
```

Komutu için çıktı aşağıdakine benzemelidir:

```powershell
    StorageAccountId            : /subscriptions/<subscription-ID>/resourceGroups/ContosoResourceGroup/providers`
    /Microsoft.Storage/storageAccounts/contosocosmosdblogs
    ServiceBusRuleId            :
    EventHubAuthorizationRuleId :
    Metrics
        TimeGrain       : PT1M
        Enabled         : False
        RetentionPolicy
        Enabled : False
        Days    : 0
    
    Logs
        Category        : DataPlaneRequests
        Enabled         : True
        RetentionPolicy
        Enabled : False
        Days    : 0
    
    WorkspaceId                 :
    Id                          : /subscriptions/<subscription-ID>/resourcegroups/ContosoResourceGroup/providers`
    /microsoft.documentdb/databaseaccounts/contosocosmosdb/providers/microsoft.insights/diagnosticSettings/service
    Name                        : service
    Type                        :
    Location                    :
    Tags                        :
```

Bu, depolama hesabınıza bilgi kaydetme veritabanınız için günlüğe kaydetme şimdi etkin olduğunu doğrular.

İsteğe bağlı olarak günlükleriniz için eski günlüklerin otomatik olarak silinmesi gibi bir bekletme ilkesi de ayarlayabilirsiniz. Örneğin, **-RetentionEnabled** bayrağını kullanarak bekletme ilkesini **$true** olarak ayarlayın ve **-RetentionInDays** parametresini **90**’a ayarlayarak 90 günden eski günlüklerin otomatik olarak silinmesini sağlayın.

```powershell
Set-AzureRmDiagnosticSetting -ResourceId $account.ResourceId`
 -StorageAccountId $sa.Id -Enabled $true -Categories DataPlaneRequests`
  -RetentionEnabled $true -RetentionInDays 90
```

### <a id="access"></a>Günlüklerinize erişme
Azure Cosmos DB günlüklerde **DataPlaneRequests** kategori depolanır **Öngörüler günlükleri-data-düzlemi-istekleri** sağladığınız depolama hesabındaki kapsayıcı. 

İlk olarak, kapsayıcı adı için bir değişken oluşturun. Bu kılavuz rest kullanılır.

```powershell
    $container = 'insights-logs-dataplanerequests'
```

Bu kapsayıcıdaki tüm blobları listelemek için şunu yazın:

```powershell
Get-AzureStorageBlob -Container $container -Context $sa.Context
```

Çıkış buna benzer şekilde görünür:

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 10510193
ContentType       : application/octet-stream
LastModified      : 9/28/2017 7:49:04 PM +00:00
SnapshotTime      :
ContinuationToken:
Context           : Microsoft.WindowsAzure.Commands.Common.Storage.`
                    LazyAzureStorageContext
Name              : resourceId=/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS`
/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/CONTOSOCOSMOSDB/y=2017/m=09/d=28/h=19/m=00/PT1H.json
```

Bu Çıkışta gördüğünüz gibi bloblar bir adlandırma kuralı izleyin:`resourceId=/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<Database Account Name>/y=<year>/m=<month>/d=<day of month>/h=<hour>/m=<minute>/filename.json`

Tarih ve saat değerleri UTC'yi kullanır.

Aynı depolama hesabı birden fazla kaynak için günlükleri toplamak için kullanılabileceğinden blob adındaki tam kaynak kimliği erişmek veya yalnızca gereksinim duyduğunuz blobları indirmek çok kullanışlıdır. Ancak bunu yapmadan önce tüm blobların nasıl indirileceğini ele alacağız.

İlk olarak, blobları yüklemek için bir klasör oluşturun. Örneğin:

```powershell
New-Item -Path 'C:\Users\username\ContosoCosmosDBLogs'`
 -ItemType Directory -Force
```

Ardından tüm blobların listesini alın:  

```powershell
$blobs = Get-AzureStorageBlob -Container $container -Context $sa.Context
```

Blobları hedef klasörümüze indirmek için bu listeye 'Get-AzureStorageBlobContent' aracılığıyla kanal oluşturun:

```powershell
$blobs | Get-AzureStorageBlobContent `
 -Destination 'C:\Users\username\ContosoCosmosDBLogs'
```

Bu ikinci komutu çalıştırdığınızda  **/**  blob adlarındaki sınırlayıcısı hedef klasörün altında tam klasör yapısı oluşturur. Bu klasör yapısı karşıdan yüklemek ve blobları dosya olarak depolamak için kullanılır.

Blobları seçmeli olarak indirmek için jokerleri kullanın. Örneğin:

* Birden çok veritabanı varsa ve yalnızca bir veritabanı için günlükleri indirmek istediğiniz CONTOSOCOSMOSDB3 adlı:

    ```powershell
    Get-AzureStorageBlob -Container $container `
     -Context $sa.Context -Blob '*/DATABASEACCOUNTS/CONTOSOCOSMOSDB3
    ```

* Birden çok kaynak grubunuz varsa ve yalnızca bir kaynak grubu için günlük indirmek isterseniz `-Blob '*/RESOURCEGROUPS/<resource group name>/*'` kullanın:

    ```powershell
    Get-AzureStorageBlob -Container $container `
    -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'
    ```
* Temmuz 2017 ayı için tüm günlükleri indirmek isterseniz kullanın `-Blob '*/year=2017/m=07/*'`:

    ```powershell
    Get-AzureStorageBlob -Container $container `
     -Context $sa.Context -Blob '*/year=2017/m=07/*'
    ```

Buna ek olarak:

* Veritabanı kaynağınızın tanılama ayarlarının durumunu sorgulamak için:`Get-AzureRmDiagnosticSetting -ResourceId $account.ResourceId`
* Günlüğünü devre dışı bırakmak için **DataPlaneRequests** veritabanı hesabı kaynağınız için kategori:`Set-AzureRmDiagnosticSetting -ResourceId $account.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories DataPlaneRequests`


Her bu sorgular döndürülen BLOB'ları, aşağıdaki kodda gösterildiği gibi bir JSON blobu olarak biçimlendirilip metin olarak depolanır. 

```json
{
    "records":
    [
        {
           "time": "Fri, 23 Jun 2017 19:29:50.266 GMT",
           "resourceId": "contosocosmosdb",
           "category": "DataPlaneRequests",
           "operationName": "Query",
           "resourceType": "Database",
           "properties": {"activityId": "05fcf607-6f64-48fe-81a5-f13ac13dd1eb",`
           "userAgent": "documentdb-dotnet-sdk/1.12.0 Host/64-bit MicrosoftWindowsNT/6.2.9200.0 AzureSearchIndexer/1.0.0",`
           "resourceType": "Database","statusCode": "200","documentResourceId": "",`
           "clientIpAddress": "13.92.241.0","requestCharge": "2.260","collectionRid": "",`
           "duration": "9250","requestLength": "72","responseLength": "209", "resourceTokenUserRid": ""}
        }
    ]
}
```

Her bir JSON blob verileri hakkında bilgi edinmek için [Azure Cosmos DB günlüklerinizi yorumlama](#interpret).

## <a name="managing-your-logs"></a>Günlüklerinizi yönetme

Günlükleri Azure Cosmos DB işlemi yapıldığı zamanından itibaren iki saatte hesabınızda kullanılabilir hale getirilir. Depolama hesabınızdaki günlüklerinizi yönetmek size bağlıdır:

* Günlüklerinize erişebilecek kişileri kısıtlayarak güvenliklerini sağlamak için standart Azure erişim denetimi yöntemlerini kullanın.
* Artık depolama hesabınızda tutmak istemediğiniz günlükleri silin.
* Bir depolama hesabına arşivlenen verileri düzlemi istekleri için saklama dönemi Portalı'nda yapılandırılmış zaman **günlük DataPlaneRequests** seçilir. Bu ayarı değiştirmek için bkz: [Azure portalında oturum aç](#turn-on-logging-in-the-azure-portal).


<a id="#view-in-loganalytics"></a>
## <a name="view-logs-in-log-analytics"></a>Günlük analizi içinde günlüklerini görüntüle

Seçtiyseniz **için günlük analizi Gönder** seçeneği, günlük kaydı etkinleştirildiğinde, koleksiyonunuzu Tanılama verileri iletilmesi için günlük analizi iki saat içinde. Bu günlük özelliğini açtıktan hemen sonra günlük analizi bakarsanız, herhangi bir veri görmezsiniz anlamına gelir. Yalnızca iki saat bekleyin ve yeniden deneyin. 

Günlüklerinizi görüntülemeden önce kontrol edin ve günlük analizi çalışma alanınız yeni günlük analizi sorgu dili kullanmak için yükseltilmiş varsa bkz istersiniz. Bunu denetlemek için açık [Azure portal](https://portal.azure.com), tıklatın **günlük analizi** kadar sol tarafta, ardından aşağıdaki görüntüde gösterildiği gibi çalışma alanı adı seçin. **OMS çalışma** sayfası aşağıdaki görüntüde gösterildiği gibi görüntülenir.

![Azure portalında günlük analizi](./media/logging/azure-portal.png)

Aşağıdaki iletiyi görürseniz **OMS çalışma** sayfasında, çalışma alanınızı değil yükseltildi yeni dil kullanmak üzere. Yeni sorgu dili yükseltme hakkında daha fazla bilgi için bkz: [Azure günlük analizi çalışma alanınız için yeni günlük arama yükseltme](../log-analytics/log-analytics-log-search-upgrade.md). 

![Günlük analizi yükseltme bildirimi](./media/logging/upgrade-notification.png)

Günlük analizi tanılama verilerini görüntülemek için günlük arama sayfası soldaki menüden ya da sayfa yönetim alanı aşağıdaki görüntüde gösterildiği gibi açın.

![Azure portalında oturum arama seçenekleri](./media/logging/log-analytics-open-log-search.png)

Veri koleksiyonu, aşağıdaki günlük arama örneği çalıştırmak etkinleştirdiğinize göre on en son günlükleri görmek için yeni sorgu dili kullanarak `AzureDiagnostics | take 10`.

![Örnek 10 günlük arama alın](./media/logging/log-analytics-query.png)

<a id="#queries"></a>
### <a name="queries"></a>Sorgular

İçine girin bazı ek sorgular şunlardır **günlük arama** Azure Cosmos DB kapsayıcılarınızı izlemenize yardımcı olması için kutusu. Bu sorguları çalışmak [yeni dil](../log-analytics/log-analytics-log-search-upgrade.md). 

Her günlük araması tarafından döndürülen veri anlamını öğrenmek için bkz: [Azure Cosmos DB günlüklerinizi yorumlama](#interpret).

* Belirtilen süre boyunca tüm tanılama günlüklerini Azure Cosmos DB'den.

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests"
    ```

* On en son olayların günlüğe.

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | take 10
    ```

* İşlem türüne göre gruplandırılmış tüm işlemleri.

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by OperationName
    ```

* Kaynağa göre gruplandırılmış tüm işlemleri.

    ```
    AzureActivity | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by Resource
    ```

* Kaynağa göre gruplandırılmış tüm kullanıcı etkinliği. Bu bir tanılama günlük bir etkinlik günlüğü olduğuna dikkat edin.

    ```
    AzureActivity | where Caller == "test@company.com" and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by Resource
    ```

* Hangi işlemleri 3 milisaniye uzun sürer.

    ```
    AzureDiagnostics | where toint(duration_s) > 3000 and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by clientIpAddress_s, TimeGenerated
    ```

* Hangi aracı operations çalışıyor.

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by OperationName, userAgent_s
    ```

* Uzun süre çalışan işlemleri zaman gerçekleştirilmiştir.

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | project TimeGenerated , toint(duration_s)/1000 | render timechart
    ```

Yeni günlük arama dili kullanma hakkında ek bilgi için bkz: [anlama günlük arar günlük analizi](../log-analytics/log-analytics-log-search-new.md). 

## <a id="interpret"></a>Günlüklerinizi yorumlama

Tanılama verileri Azure Storage ve günlük analizi depolanan çok benzer bir şema kullanın. 

Aşağıdaki tabloda her günlük girişinin içeriğini açıklar.

| Azure depolama alanı veya özelliği | Günlük analizi özelliği | Açıklama |
| --- | --- | --- |
| time | TimeGenerated | Tarih ve saat (UTC) işlemi oluştuğunda. |
| resourceId | Kaynak | Günlükleri etkin Azure Cosmos DB hesabı.|
| category | Kategori | Azure Cosmos DB günlüklerinde DataPlaneRequests yalnızca kullanılabilen değerdir. |
| operationName | OperationName | İşlemin adı. Bu değer aşağıdaki işlemlerden birini olabilir: oluşturma, güncelleştirme, okuma, ReadFeed, Sil, Değiştir, yürütme, SqlQuery, sorgu, JSQuery, Head, HeadFeed veya Upsert.   |
| properties | yok | Bu alanın içeriğini aşağıdaki satırları açıklanmaktadır. |
| Etkinlik Kimliği | activityId_g | Oturum işlemi için benzersiz bir GUID. |
| UserAgent | userAgent_s | İsteği gerçekleştiren istemcinin kullanıcı aracısı belirten bir dize. Biçimi {kullanıcı aracısı adıdır} / {version}.|
| Kaynak türü | ResourceType | Erişilen kaynak türü. Bu değer aşağıdaki kaynak türlerinden herhangi birinde olabilir: veritabanı, koleksiyon, belge, ek, kullanıcı, izin, StoredProcedure, tetikleyici, UserDefinedFunction veya teklif. |
| statusCode |statusCode_s | İşlem yanıt durumu. |
| requestResourceId | ResourceId | İsteğine ilişkin ResourceId databaseRid, collectionRid veya documentRid yapılan işleme bağlı olarak noktası.|
| clientIpAddress | clientIpAddress_s | İstemcinin IP adresi. |
| requestCharge | requestCharge_s | İşlem tarafından kullanılan RUs sayısı |
| collectionRid | collectionId_s | Koleksiyon için benzersiz kimlik.|
| Süre | duration_s | İşlemde çizgilerine süresi. |
| requestLength | requestLength_s | İstek bayt cinsinden uzunluğu. |
| responseLength | responseLength_s | Yanıtın bayt cinsinden uzunluğu.|
| resourceTokenUserRid | resourceTokenUserRid_s | Bu boş olduğunda [kaynak belirteçleri](https://docs.microsoft.com/azure/cosmos-db/secure-access-to-data#resource-tokens) kimlik doğrulama ve kaynak kimliği noktalarına kullanıcının için kullanılır. |

## <a name="next-steps"></a>Sonraki adımlar

- Günlüğe kaydetme, aynı zamanda çeşitli Azure tarafından desteklenen ölçümleri ve günlük kategorileri etkinleştirmek yalnızca nasıl anlamak için hizmetleri hem de okuma [Microsoft Azure ölçümlerini genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md) ve [genel bakış, Azure Tanılama günlüklerini](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) makaleleri.
- Olay hub'ları hakkında bilgi edinmek için bu makaleler okuyun:
   - [Azure Event Hubs nedir?](../event-hubs/event-hubs-what-is-event-hubs.md)
   - [Event Hubs kullanmaya başlayın](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
- Okuma [Azure depolama biriminden ölçümleri ve tanılama günlüklerini indirin](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs)
- Okuma [anlama günlük arar günlük analizi](../log-analytics/log-analytics-log-search-new.md)
