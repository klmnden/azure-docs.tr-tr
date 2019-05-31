---
title: Azure Cosmos DB'de tanılama günlüğüne kaydetme
description: Azure Cosmos DB'de depolanan verileri günlüğe kaydetme ve izleme için farklı yollar hakkında bilgi edinin.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: sngun
ms.custom: seodec18
ms.openlocfilehash: 67a6eec938a4a18455e4063925e21e26fe362f76
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66243480"
---
# <a name="diagnostic-logging-in-azure-cosmos-db"></a>Azure Cosmos DB'de tanılama günlüğüne kaydetme 

Bir veya daha fazla Azure Cosmos DB veritabanı kullanmaya başladıktan sonra izlemek isteyebilirsiniz nasıl ve ne zaman veritabanlarınızı erişilir. Bu makalede Azure platformunda kullanılabilir günlükleri genel bir bakış sağlar. İzleme günlüklerini göndermek amacıyla tanılama günlüğü etkinleştirme hakkında bilgi edinin [Azure depolama](https://azure.microsoft.com/services/storage/), akış günlüklerine nasıl [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)ve günlükleri dışarı aktarma [Azure İzleyicigünlüğekaydeder](https://azure.microsoft.com/services/log-analytics/).

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="logs-available-in-azure"></a>Azure'da kullanılabilen günlükleri

Şimdi Azure Cosmos DB hesabınız izlemek nasıl çözdüğünü önce günlüğe kaydetme ve izleme hakkında bazı noktalar açıklığa. Farklı türde Azure platformunda günlükleri vardır. Vardır [Azure etkinlik günlüklerini](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs), [Azure tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs), [Azure ölçümleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics), olaylar, sinyal izleme, işlem günlükleri ve benzeri. Günlükleri deseninizi oluşturmayı yoktur. Günlüklerde tam listesini görebilirsiniz [Azure İzleyici günlükleri](https://azure.microsoft.com/services/log-analytics/) Azure portalında. 

Aşağıdaki görüntüde, farklı türde bir kullanılabilir Azure günlükleri gösterilmektedir:

![Çeşitli Azure günlükleri](./media/logging/azurelogging.png)

Görüntüde **işlem kaynaklarını** kendisi için erişebileceğiniz Microsoft konuk işletim sistemi Azure kaynaklarını temsil eder. Örneğin, Azure sanal makineler, sanal makine ölçek kümeleri, Azure Container Service, ve bu şekilde dikkate alınan bilgi işlem kaynakları. İşlem kaynakları etkinlik günlükleri, tanılama günlükleri ve uygulama günlüklerini oluşturur. Daha fazla bilgi edinmek için bkz [azure'da veri izleme kaynakları](../azure-monitor/platform/data-sources.md) makalesi.

**Olmayan işlem kaynakları** içinde edemez temel işletim sistemi erişmek ve iş doğrudan kaynak kaynaklardır. Örneğin, ağ güvenlik grupları, Logic Apps ve benzeri. Azure Cosmos DB işlem dışı bir kaynaktır. Etkinlik günlüğü'nde olmayan işlem kaynakları için günlükleri görüntüleyebilir veya Portalı'nda tanılama günlükleri seçeneğini etkinleştirin. Daha fazla bilgi edinmek için bkz [Azure İzleyici'de veri kaynaklarını](../azure-monitor/platform/data-sources.md) makalesi.

Etkinlik günlüğü, Azure Cosmos DB için abonelik düzeyinde işlemleri kaydeder. Listkeys'i DatabaseAccounts yazma ve daha fazlası gibi işlemleri günlüğe kaydedilir. Tanılama günlükleri daha ayrıntılı günlük kaydı sağlayın ve DataPlaneRequests (oluşturma, okuma, sorgu vb.) ve MongoRequests oturum olanak sağlar.


Bu makalede, Azure etkinlik günlüğü, Azure tanılama günlükleri ve Azure ölçümleri odaklanın. Bu üç günlük arasındaki fark nedir? 

### <a name="azure-activity-log"></a>Azure etkinlik günlüğü

Azure etkinlik günlüğü oluşan Azure'da abonelik düzeyindeki olayların bir anlayış sağlayan bir abonelik günlüktür. Etkinlik günlüğü yönetim kategorisi altındaki Abonelikleriniz için Denetim düzlemi olayları bildirir. Etkinlik günlüğü belirlemek için kullanabilirsiniz "ne, kim ve ne zaman" herhangi bir aboneliğinizdeki kaynaklar üzerinde işlemi (PUT, POST, DELETE) yazma için. Ayrıca, işlemi ve ilgili diğer özellikleri durumunu anlayabilirsiniz. 

Etkinlik günlüğü tanılama günlüklerinden farklıdır. Etkinlik günlüğü bir dış kaynaktan işlemleri hakkında veri sağlar ( _denetim düzlemi_). Azure Cosmos DB bağlamında, Denetim düzlemi işlemleri içeren kapsayıcı, anahtarlarını Listele, silme anahtarları, liste veritabanı vb. oluşturun. Tanılama günlükleri bir kaynak tarafından gönderilir ve bu kaynağın işlemiyle ilgili bilgi sağlayın ( _veri düzlemi_). Tanılama günlüğüne veri düzlemi işlemleri bazı örnekleri, Delete, INSERT ve ReadFeed şunlardır.

Etkinlik günlüklerini (Denetim düzlemi işlemleri) doğası gereği daha zengin ve çağıran, arayan IP adresine, kaynak adı, işlem adı, Kiracı kimliği ve daha fazla tam e-posta adresini içerebilir. Etkinlik günlüğü birkaç içeren [kategorileri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-activity-log-schema) veri. Bu kategorilerin şemaların hakkında tam Ayrıntılar için bkz: [Azure etkinlik günlüğü olay şeması](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-activity-log-schema). Ancak, tanılama günlükleri kişisel veriler genellikle bu günlüklerinden kesilmiş olarak doğası gereği kısıtlayıcı olabilir. Çağıran IP adresine sahip olabilir, ancak son octant kaldırılır.

### <a name="azure-metrics"></a>Azure ölçümleri

[Azure ölçümleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) en önemli Azure telemetri veri türüne sahip (olarak da adlandırılan _performans sayaçları_) çoğu Azure kaynakları tarafından yayılan. Ölçümleri aktarım hızı, depolama, tutarlılık, kullanılabilirlik ve gecikme süresini, Azure Cosmos DB kaynaklarını bilgilerini görüntülemenize olanak sağlar. Daha fazla bilgi için [izleme ve Azure Cosmos DB'de ölçümlerle hata ayıklama](use-metrics.md).

### <a name="azure-diagnostic-logs"></a>Azure tanılama günlükleri

Azure tanılama günlükleri, kaynak tarafından gönderilir ve bu kaynağın işlemiyle ilgili zengin, sık sık veri sağlar. Bu günlüklerin içeriği kaynak türüne göre değişir. Kaynak tanılama günlükleri de konuk işletim sistemi düzeyinde tanılama günlüklerinden farklıdır. Bir sanal makine veya diğer desteklenen içinde çalışan bir aracısı tarafından toplanan konuk işletim sistemi tanılama günlükleri kaynak türü. Kaynak düzeyinde tanılama günlükleri, Azure platformu hiçbir aracı ve yakalama kaynağa özgü verileri gerektirir. Konuk işletim sistemi düzeyinde tanılama günlükleri, işletim sistemi ve bir sanal makinede çalışan uygulamaların verilerini yakalayın.

![Depolama, Event Hubs veya Azure İzleyici günlüklerine tanılama günlüğüne kaydetme](./media/logging/azure-cosmos-db-logging-overview.png)

### <a name="what-is-logged-by-azure-diagnostic-logs"></a>Azure tanılama günlükleri tarafından günlüğe kaydedilenler?

* Erişim izinleri, sistem hataları veya hatalı istekler sonucunda başarısız istekleri dahil olmak üzere tüm kimliği doğrulanmış bir arka uç (TCP/REST) isteklerinde tüm API'leri günlüğe kaydedilir. Kullanıcı tarafından başlatılan istek Graph, Cassandra ve tablo API'si desteği şu anda kullanılabilir değil.
* Tüm belgeleri, kapsayıcılar ve veritabanları CRUD işlemleri içeren işlemler veritabanında kendisi.
* İşlem oluşturma, değiştirme veya anahtarlarının silinmesi dahil hesabı anahtarları.
* Bir 401 yanıtına neden olan kimliği doğrulanmamış istekler. Örneğin, bir taşıyıcı belirteç yok veya hatalı biçimlendirilmiş ya da süresi dolmuş ya da geçersiz bir belirtece sahip istekler.

<a id="#turn-on"></a>
## <a name="turn-on-logging-in-the-azure-portal"></a>Azure portalında oturum açın

Tanılama günlük kaydını etkinleştirmek için aşağıdaki kaynaklara sahip olmalıdır:

* Bir var olan Azure Cosmos DB hesabı, veritabanı ve kapsayıcı. Bu kaynakları oluşturma ile ilgili yönergeler için bkz: [Azure portalını kullanarak bir veritabanı hesabı oluşturma](create-sql-api-dotnet.md#create-account), [Azure CLI örnekleri](cli-samples.md), veya [PowerShell örnekleri](powershell-samples.md).

Azure portalında tanılama günlük kaydını etkinleştirmek için aşağıdaki adımları uygulayın:

1. İçinde [Azure portalında](https://portal.azure.com), uygulamanızı Azure Cosmos DB hesabı, seçin **tanılama günlükleri** sol gezinti ve ardından **tanılamayı Aç**.

    ![Azure portalında Azure Cosmos DB için tanılama günlük kaydını açma](./media/logging/turn-on-portal-logging.png)

2. İçinde **tanılama ayarları** sayfasında, aşağıdaki adımları uygulayın: 

    * **Ad**: Oluşturmak günlükleri için bir ad girin.

    * **Bir depolama hesabında arşivle**: Bu seçeneği kullanmak için bağlanmak için mevcut bir depolama hesabı gerekir. Portalda yeni bir depolama hesabı oluşturmak için bkz [depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md) ve Azure Resource Manager, genel amaçlı hesap oluşturmak için yönergeleri izleyin. Bu sayfaya portalındaki depolama hesabınızı seçin, ardından döndürür. Bu, yeni oluşturulan depolama hesapları, aşağı açılan menüsünün görünmesi birkaç dakika sürebilir.
    * **Olay hub'ına Stream**: Bu seçeneği kullanmak için bağlanmak için mevcut bir Event Hubs ad alanı ve olay hub'gerekir. Bir Event Hubs ad alanı oluşturmak için bkz [Azure portalını kullanarak bir Event Hubs ad alanı ve olay hub'ı oluşturma](../event-hubs/event-hubs-create.md). Ardından, portaldaki Event Hubs ad alanı ve ilke adı seçmek için bu sayfaya dönün.
    * **Log Analytics'e gönderme**: Bu seçeneği kullanmak için mevcut bir çalışma kullanabilir veya yeni bir Log Analytics çalışma alanı için adımları izleyerek oluşturabilirsiniz [yeni bir çalışma alanı oluşturma](../azure-monitor/learn/quick-collect-azurevm.md#create-a-workspace) portalında. Azure İzleyici günlüklerine, günlükleri görüntüleme hakkında daha fazla bilgi için bkz: görünümü Azure İzleyici günlüklerine kaydeder.
    * **Oturum DataPlaneRequests**: Temel alınan Azure Cosmos DB dağıtılmış platformu SQL, grafik, MongoDB, Cassandra ve tablo API'si hesapları için arka uç isteklerini günlüğe kaydetmek için bu seçeneği belirleyin. Bir depolama hesabına arşivleme tanılama günlükleri için saklama süresi seçebilirsiniz. Bekletme süresi dolduktan sonra günlükleri otomatik olarak silinir.
    * **Oturum MongoRequests**: MongoDB için Azure Cosmos DB API'si ile yapılandırılan Cosmos hesaplar sunulması için Azure Cosmos DB ön uç kullanıcı tarafından başlatılan istek oturumu için bu seçeneği belirleyin. Bir depolama hesabına arşivleme tanılama günlükleri için saklama süresi seçebilirsiniz. Bekletme süresi dolduktan sonra günlükleri otomatik olarak silinir.
    * **Ölçüm istekleri**: Ayrıntılı verileri depolamak için bu seçeneği [Azure ölçümleri](../azure-monitor/platform/metrics-supported.md). Bir depolama hesabına arşivleme tanılama günlükleri için saklama süresi seçebilirsiniz. Bekletme süresi dolduktan sonra günlükleri otomatik olarak silinir.

3. **Kaydet**’i seçin.

    Bildiren bir hata alırsanız, "tanılama için güncelleştirilemedi \<çalışma alanı adı >. Abonelik \<abonelik kimliği >, microsoft.insights kullanmak için kayıtlı değil "izleyin [Azure tanılama sorunlarını giderme](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage) yönergeleri hesabını kaydeder ve bu yordamı yeniden deneyin.

    Nasıl Tanılama günlüklerinizi herhangi bir noktada gelecekte kaydedilir değiştirmek istiyorsanız, hesabınız için tanılama günlük ayarlarını değiştirmek için bu sayfaya dönün.

## <a name="turn-on-logging-by-using-azure-cli"></a>Azure CLI'yı kullanarak günlük özelliğini açar

Azure CLI kullanarak ölçümleri ve tanılama günlüğünü etkinleştirmek için aşağıdaki komutları kullanın:

- Bir depolama hesabında depolama, tanılama günlüğü etkinleştirmek için bu komutu kullanın:

   ```azurecli-interactive
   az monitor diagnostic-settings create --name DiagStorage --resource <resourceId> --storage-account <storageAccountName> --logs '[{"category": "QueryRuntimeStatistics", "enabled": true, "retentionPolicy": {"enabled": true, "days": 0}}]'
   ```

   `resource` Azure Cosmos DB hesabının adıdır. Kaynak şu biçimdedir "/subscriptions/`<subscriptionId>`/resourceGroups/`<resource_group_name>`/providers/Microsoft.DocumentDB/databaseAccounts/ < Azure_Cosmos_account_name >" `storage-account` depolama hesabının adı, Günlükleri göndermek istersiniz. "MongoRequests" veya "DataPlaneRequests" kategorisi parametre değerlerini güncelleştirerek, diğer günlükler oturum açabilirsiniz. 

- Tanılama günlüklerini, olay hub'ına akış etkinleştirmek için bu komutu kullanın:

   ```azurecli-interactive
   az monitor diagnostic-settings create --name cdbdiagsett --resourceId <resourceId> --event-hub-rule <eventHubRuleID> --logs '[{"category":"QueryRuntimeStatistics","enabled":true,"retentionPolicy":{"days":6,"enabled":true}}]'
   ```

   `resource` Azure Cosmos DB hesabının adıdır. `event-hub-rule` Olay hub'ı kural kimliğidir. 

- Log Analytics çalışma alanına gönderme tanılama günlüklerini etkinleştirmek için bu komutu kullanın:

   ```azurecli-interactive
   az monitor diagnostic-settings create --name cdbdiagsett --resourceId <resourceId> --workspace <resource id of the log analytics workspace> --logs '[{"category":"QueryRuntimeStatistics","enabled":true,"retentionPolicy":{"days":6,"enabled":true}}]'
   ```

Birden çok çıkış seçeneği etkinleştirmek için şu parametreleri birleştirebilirsiniz.

## <a name="turn-on-logging-by-using-powershell"></a>PowerShell kullanarak günlük özelliğini açar

PowerShell kullanarak Tanılama Günlüğü etkinleştirmek için bir en az 1.0.1 sürümü ile Azure Powershell olması gerekir.

Azure PowerShell'i yüklemek ve Azure aboneliğinizle ilişkilendirmek için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).

Azure PowerShell'i zaten yüklemiş olduğunuz ve sürümü PowerShell konsolunu türünden bilmiyorum `(Get-Module azure -ListAvailable).Version`.  

### <a id="connect"></a>Aboneliklerinize bağlanma
Bir Azure PowerShell oturumu başlatın ve aşağıdaki komutla Azure hesabınızda oturum açın:  

```powershell
Connect-AzAccount
```

Açılır tarayıcı penceresinde Azure hesabı kullanıcı adınızı ve parolanızı girin. Azure PowerShell tüm birinciyi kullanır, varsayılan olarak ve bu hesapla ilişkili abonelikleri alır.

Birden fazla aboneliğiniz varsa, Azure anahtar kasanızı oluşturmak için kullanılan belirli bir aboneliğe belirtmeniz gerekebilir. Hesabınız için abonelikleri görmek için aşağıdaki komutu yazın:

```powershell
Get-AzSubscription
```

Sonra oturum açtığınızdan Azure Cosmos DB hesabıyla ilişkili aboneliği belirtmek için aşağıdaki komutu yazın:

```powershell
Set-AzContext -SubscriptionId <subscription ID>
```

> [!NOTE]
> Hesabınızla ilişkili birden fazla aboneliğiniz varsa, kullanmak istediğiniz aboneliği belirtmek önemlidir.
>
>

Azure PowerShell yapılandırma hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/overview).

### <a id="storage"></a>Günlükleriniz için yeni bir depolama hesabı oluşturma
Bu öğreticide günlükleriniz için var olan bir depolama hesabı kullanabilirsiniz, ancak Azure Cosmos DB günlüklerine özgü yeni bir depolama hesabı oluşturacağız. Kolaylık olması için depolama hesabı ayrıntıları adlı bir değişkende depolarız **sa**.

Bu öğreticide, yönetim kolaylığı açısından ek aynı kaynak grubunda Azure Cosmos DB veritabanı içeren bir kullanırız. Kendi değerlerinizi yerleştirin **ContosoResourceGroup**, **contosocosmosdblogs**, ve **Orta Kuzey ABD** uygunsa parametreleri:

```powershell
$sa = New-AzStorageAccount -ResourceGroupName ContosoResourceGroup `
-Name contosocosmosdblogs -Type Standard_LRS -Location 'North Central US'
```

> [!NOTE]
> Mevcut bir depolama hesabı kullanmaya karar verirseniz, hesabı Azure Cosmos DB aboneliğiniz aynı aboneliği kullanmanız gerekir. Hesap de klasik dağıtım modeli yerine Resource Manager dağıtım modeli kullanmanız gerekir.
>
>

### <a id="identify"></a>Günlükleriniz için Azure Cosmos DB hesabı belirle
Azure Cosmos DB hesap adınızı adlı bir değişken ayarlamak **hesabı**burada **ResourceName** Azure Cosmos DB hesabının adıdır.

```powershell
$account = Get-AzResource -ResourceGroupName ContosoResourceGroup `
-ResourceName contosocosmosdb -ResourceType "Microsoft.DocumentDb/databaseAccounts"
```

### <a id="enable"></a>Günlüğe kaydetmeyi etkinleştirme
Azure Cosmos DB için günlük kaydını etkinleştirmek için `Set-AzDiagnosticSetting` cmdlet'iyle yeni depolama hesabı ve Azure Cosmos DB hesabı için günlük kaydını etkinleştirmek için bir kategori için değişkenleri. Aşağıdaki komutu çalıştırın ve ayarlama **-etkin** bayrak **$true**:

```powershell
Set-AzDiagnosticSetting  -ResourceId $account.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories DataPlaneRequests
```

Komut çıktısı, aşağıdaki örneğe benzemelidir:

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

Komut çıktısı, günlüğe kaydetme, veritabanı için artık etkin ve depolama hesabınıza kaydedilen bilgileri onaylar.

İsteğe bağlı olarak, eski günlüklerin otomatik olarak silinmesi gerektiğini günlükleriniz için bekletme ilkesi de ayarlayabilirsiniz. Örneğin, bekletme ilkesi ile ayarlamak **- RetentionEnabled** bayrağı ayarlanmış **$true**. Ayarlama **- Retentionındays** parametresi **90** böylece 90 günden eski olan günlükler otomatik olarak silinir.

```powershell
Set-AzDiagnosticSetting -ResourceId $account.ResourceId`
 -StorageAccountId $sa.Id -Enabled $true -Categories DataPlaneRequests`
  -RetentionEnabled $true -RetentionInDays 90
```

### <a id="access"></a>Günlüklerinize erişme
Azure Cosmos DB için günlüklerini **DataPlaneRequests** kategori depolanır **ınsights-günlükleri-data-düzlemi-istekleri** , sağladığınız depolama hesabındaki kapsayıcı. 

İlk olarak, kapsayıcı adı için bir değişken oluşturun. Değişkeni kılavuz kullanılır.

```powershell
    $container = 'insights-logs-dataplanerequests'
```

Tüm bu kapsayıcıdaki blobları listelemek için şunu yazın:

```powershell
Get-AzStorageBlob -Container $container -Context $sa.Context
```

Komut çıktısı, aşağıdaki örneğe benzemelidir:

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

Bu Çıkışta gördüğünüz gibi bloblar bir adlandırma kuralını izler: `resourceId=/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<Database Account Name>/y=<year>/m=<month>/d=<day of month>/h=<hour>/m=<minute>/filename.json`

Tarih ve saat değerleri UTC'yi kullanır.

Aynı depolama hesabında birden fazla kaynak için günlükleri toplamak için kullanılabildiğinden, erişim ve ihtiyaç duyduğunuz belirli blobları indirmek için blob adı, tam Kaynak Kimliği'ni kullanabilirsiniz. Bunu yapmadan önce tüm blobların indirmek nasıl ele.

İlk olarak, blobları yüklemek için bir klasör oluşturun. Örneğin:

```powershell
New-Item -Path 'C:\Users\username\ContosoCosmosDBLogs'`
 -ItemType Directory -Force
```

Ardından tüm blobların listesini alın:  

```powershell
$blobs = Get-AzStorageBlob -Container $container -Context $sa.Context
```

Bu listede kanal `Get-AzStorageBlobContent` blobları hedef klasöre yüklemek için komutu:

```powershell
$blobs | Get-AzStorageBlobContent `
 -Destination 'C:\Users\username\ContosoCosmosDBLogs'
```

Bu ikinci komutu çalıştırdığınızda **/** blob adlarındaki sınırlayıcısı hedef klasörün altında tam klasör yapısı oluşturur. Bu klasör yapısını, indirmek ve blobları dosya olarak depolamak için kullanılır.

Blobları seçmeli olarak indirmek için jokerleri kullanın. Örneğin:

* Birden çok veritabanına sahip ve tek veritabanı adlı günlükleri indirmek istiyorsanız **CONTOSOCOSMOSDB3**, komutu kullanın:

    ```powershell
    Get-AzStorageBlob -Container $container `
     -Context $sa.Context -Blob '*/DATABASEACCOUNTS/CONTOSOCOSMOSDB3
    ```

* Birden çok kaynak grupları ve yalnızca bir kaynak grubu için günlük indirmek isterseniz varsa komutunu `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:

    ```powershell
    Get-AzStorageBlob -Container $container `
    -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'
    ```
* Temmuz 2017'in ay için tüm günlükleri indirmek isterseniz, komutunu `-Blob '*/year=2017/m=07/*'`:

    ```powershell
    Get-AzStorageBlob -Container $container `
     -Context $sa.Context -Blob '*/year=2017/m=07/*'
    ```

Ayrıca, aşağıdaki komutları çalıştırabilirsiniz:

* Veritabanı kaynağınızın tanılama ayarlarının durumunu sorgulamak için komutunu `Get-AzDiagnosticSetting -ResourceId $account.ResourceId`.
* Günlük kaydını devre dışı bırakmak için **DataPlaneRequests** veritabanı hesabı kaynağınızın kategorisini kullanın komutu `Set-AzDiagnosticSetting -ResourceId $account.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories DataPlaneRequests`.


Bu sorguları her döndürülen BLOB'ları metin olarak depolandığını ve aşağıdaki kodda gösterildiği gibi bir JSON blobu olarak biçimlendirilmiş:

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

Her bir JSON blob verileri hakkında bilgi edinmek için bkz. [Azure Cosmos DB günlüklerinizi yorumlama](#interpret).

## <a name="manage-your-logs"></a>Günlüklerinizi yönetme

Azure Cosmos DB işlemi yapıldığını andan itibaren iki saat için tanılama günlükleri, hesabınızda kullanılabilir hale getirilir. Depolama hesabınızdaki günlüklerinizi yönetmek size bağlıdır:

* Bunları erişebilen günlüklerinizi güvenli ve kısıtlamak için standart Azure erişim denetimi yöntemlerini kullanın.
* Artık depolama hesabınızda tutmak istemediğiniz günlükleri silin.
* Portalda bir depolama hesabına arşivlenen veri düzlemi istekleri saklama süresini yapılandırılmış olduğunda **günlük DataPlaneRequests** ayarı seçildiyse. Bu ayarı değiştirmek için bkz: [Azure portalında günlük](#turn-on-logging-in-the-azure-portal).


<a id="#view-in-loganalytics"></a>
## <a name="view-logs-in-azure-monitor-logs"></a>Görünüm Azure İzleyici günlüklerine kaydeder

Seçtiyseniz **Log Analytics'e gönderme** seçeneği tanı, tanılama günlüğü etkinleştirildiğinde iki saat içinde kapsayıcınızı verilerini Azure İzleyici günlüklerine iletilir. Günlüğe kaydetmeyi hemen sonra Azure İzleyici günlüklerine bakın, herhangi bir veri görmezsiniz. Yalnızca iki saat bekleyin ve yeniden deneyin. 

Günlüklerinizi görüntülemek için önce denetleyin ve Log Analytics çalışma alanınızı yeni Kusto sorgu dili kullanmak için yükseltilmiş varsa bkz. Denetlemek için açın [Azure portalı](https://portal.azure.com)seçin **Log Analytics çalışma alanları** üzerinde en solda, ardından çalışma alanı adı sonraki resimde gösterildiği gibi seçin. **Log Analytics çalışma alanı** sayfası görüntülenir:

![Azure portalında Azure izleme günlükleri](./media/logging/azure-portal.png)

>[!NOTE]
>OMS çalışma alanları artık Log Analytics çalışma alanları olarak adlandırılır.  

Aşağıdaki iletiyi görürseniz **Log Analytics çalışma alanı** sayfasında, çalışma alanınız Henüz yükseltme yeni dil kullanmak üzere. Yeni sorgu diline yükseltme hakkında daha fazla bilgi için bkz. [yeni günlük araması için Azure Log Analytics çalışma alanınızı yükseltme](../log-analytics/log-analytics-log-search-upgrade.md). 

![Azure İzleyici günlüklerine message yükseltme](./media/logging/upgrade-notification.png)

Azure İzleyici günlüklerine tanılama verilerinizi görüntülemek için Aç **günlük araması** sayfasında sol menüden veya **Yönetim** sayfasında, aşağıdaki görüntüde gösterildiği gibi alanı:

![Azure portalında günlük arama seçenekleri](./media/logging/log-analytics-open-log-search.png)

10 en son günlükleri görmek için yeni bir sorgu dili kullanarak veri toplama etkinleştirdikten sonra aşağıdaki günlük araması örneği çalıştırmak `AzureDiagnostics | take 10`.

![10 en son günlükler için örnek günlük araması](./media/logging/log-analytics-query.png)

<a id="#queries"></a>
### <a name="queries"></a>Sorgular

İçine girdiğiniz bazı ek sorgular şunlardır **günlük araması** kutusuna Azure Cosmos DB kapsayıcıları izlemenize yardımcı olacak. Bu sorguları çalışmak [yeni dil](../log-analytics/log-analytics-log-search-upgrade.md). 

Her günlük araması tarafından döndürülen verinin anlamı hakkında bilgi edinmek için bkz. [Azure Cosmos DB günlüklerinizi yorumlama](#interpret).

* Belirtilen bir süre için tüm Azure Cosmos DB'den tanılama günlükleri sorgulamak için:

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests"
    ```

* 10 en son sorgulamak için olayları günlüğe kaydedilir:

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | take 10
    ```

* İşlem türüne göre gruplandırılan tüm işlemler için sorgulamak için:

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by OperationName
    ```

* Sorguya göre gruplandırılmış tüm işlemler için **kaynak**:

    ```
    AzureActivity | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by Resource
    ```

* Kaynağa göre gruplandırılmış tüm kullanıcı etkinliklerini sorgulamak için:

    ```
    AzureActivity | where Caller == "test@company.com" and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by Resource
    ```
    > [!NOTE]
    > Bu komut, bir etkinlik günlüğü, bir tanılama günlüğü için geçerlidir.

* Sorgu işlemlerde 3 milisaniyeden uzun almak için:

    ```
    AzureDiagnostics | where toint(duration_s) > 30000 and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by clientIpAddress_s, TimeGenerated
    ```

* İşlemler için hangi aracıyı çalıştıran sorgulamak için:

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by OperationName, userAgent_s
    ```

* Uzun süre çalışan işlemleri gerçekleştirildiğinde sorgulamak için:

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | project TimeGenerated , toint(duration_s)/1000 | render timechart
    ```

Yeni günlük araması dili kullanma hakkında daha fazla bilgi için bkz. [anlayın günlük aramaları Azure İzleyici günlüklerine](../log-analytics/log-analytics-log-search-new.md). 

## <a id="interpret"></a>Günlüklerinizi yorumlama

Azure depolama ve Azure İzleyici günlüklerine depolanan tanılama verilerini benzer bir şema kullanır. 

Aşağıdaki tabloda, her günlük girişinin içeriğini açıklar.

| Azure depolama alanı veya özelliği | Azure İzleyici özelliği günlüğe kaydeder. | Açıklama |
| --- | --- | --- |
| **saat** | **TimeGenerated** | Tarih ve saat (UTC) işlemi oluştuğunda. |
| **resourceId** | **Kaynak** | Azure Cosmos DB hesabı için Günlükleri etkinleştirildi.|
| **Kategori** | **Kategori** | Azure Cosmos DB günlükleri **DataPlaneRequests** yalnızca bir değerdir. |
| **OperationName** | **OperationName** | İşlemin adı. Bu değer, aşağıdaki işlemlerden birini olabilir: Oluşturma, güncelleştirme, okuma, ReadFeed, Sil, Değiştir, yürütün, SqlQuery, sorgu, JSQuery, Head, HeadFeed veya Upsert.   |
| **Özellikleri** | yok | Bu alanın içeriğini izleyen satırları açıklanmaktadır. |
| **activityId** | **activityId_g** | Oturum işlemi için benzersiz GUID. |
| **UserAgent** | **userAgent_s** | İsteği gerçekleştiren istemcinin kullanıcı aracısı belirten bir dize. Biçimdir {kullanıcı aracısı adı} / {version}.|
| **requestResourceType** | **requestResourceType_s** | Erişilen kaynak türü. Bu değer, aşağıdaki kaynak türlerinden herhangi birinde olabilir: Veritabanı, kapsayıcı, belge, ek, kullanıcı, izin, StoredProcedure, tetikleyici, UserDefinedFunction veya teklif. |
| **statusCode** | **statusCode_s** | İşlem yanıt durumu. |
| **requestResourceId** | **ResourceId** | İsteği ilgilidir ResourceId. Değer databaseRid, collectionRid veya documentRid yapılan işleme bağlı olarak işaret edebilir.|
| **clientIpAddress** | **clientIpAddress_s** | İstemcinin IP adresi. |
| **requestCharge** | **requestCharge_s** | İşlem tarafından kullanılan RU sayısı |
| **collectionRid** | **collectionId_s** | Koleksiyon için benzersiz kimliği.|
| **Süresi** | **duration_s** | Saat döngüsü içindeki işlem süresi. |
| **requestLength** | **requestLength_s** | İstek, bayt cinsinden uzunluğu. |
| **responseLength** | **responseLength_s** | Yanıtın bayt cinsinden uzunluğu.|
| **resourceTokenUserRid** | **resourceTokenUserRid_s** | Bu değer boş olduğunda [kaynak belirteçleri](https://docs.microsoft.com/azure/cosmos-db/secure-access-to-data#resource-tokens) kimlik doğrulaması için kullanılır. Değer, kullanıcının kaynak Kimliğini işaret eder. |

## <a name="next-steps"></a>Sonraki adımlar

- Günlüğe kaydetme ve ayrıca çeşitli Azure Hizmetleri tarafından desteklenen Ölçümler ve günlük kategorileri etkinleştirme anlamak için hem de okuma [Microsoft azure'da ölçümlere genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md) ve [genel bakış, Azure tanılama günlükleri ](../azure-monitor/platform/diagnostic-logs-overview.md) makaleler.
- Event hubs hakkında bilgi edinmek için bu makaleleri okuyun:
   - [Azure Event Hubs nedir?](../event-hubs/event-hubs-what-is-event-hubs.md)
   - [Event Hubs kullanmaya başlayın](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
- Okuma [Azure Depolama'dan ölçümleri ve tanılama günlüklerini indirin](../storage/blobs/storage-quickstart-blobs-dotnet.md#download-blobs).
- Okuma [anlayın günlük aramaları Azure İzleyici günlüklerine](../log-analytics/log-analytics-log-search-new.md).
