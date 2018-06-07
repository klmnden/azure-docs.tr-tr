---
title: Azure Cosmos DB tanılama günlük | Microsoft Docs
description: Azure Cosmos DB ile başlamanıza yardımcı olması için bu öğreticiyi kullanın günlüğü.
services: cosmos-db
author: SnehaGunda
manager: kfile
tags: azure-resource-manager
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/07/2018
ms.author: sngun
ms.openlocfilehash: 66ee0856851a301a6849b71b64cb904c925ad18d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34612223"
---
# <a name="azure-cosmos-db-diagnostic-logging"></a>Azure Cosmos DB Tanılama Günlüğü

Bir veya daha fazla Azure Cosmos DB veritabanı kullanmak başlattıktan sonra izlemek isteyebilir nasıl ve ne zaman veritabanlarınızı erişilir. Bu makalede Azure platformunda bulunan günlüklerini genel bir bakış sağlar. İzleme günlükleri göndermek amacıyla tanılama günlük kaydını etkinleştirmeyi öğrenin [Azure Storage](https://azure.microsoft.com/services/storage/), günlüklere akışını nasıl [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)ve günlükleri dışarı aktarmak nasıl [Azure günlük analizi ](https://azure.microsoft.com/services/log-analytics/).

## <a name="logs-available-in-azure"></a>Azure'da kullanılabilir günlük

Şimdi biz Azure Cosmos DB hesabınızı izleme hakkında konuşun önce günlüğe kaydetme ve izleme hakkında birkaç şey açıklayın. Azure platformunda günlükleri farklı tür vardır. Vardır [Azure etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs), [Azure tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs), [Azure ölçümleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics), olaylar, sinyal izleme, işlem günlükleri ve benzeri. Günlükleri sayısız yoktur. Günlüklerde tam listesini görebilir [Azure günlük analizi](https://azure.microsoft.com/services/log-analytics/) Azure portalında. 

Aşağıdaki görüntüde bulunan Azure günlüklerini farklı türde gösterir:

![Farklı türde Azure günlükleri](./media/logging/azurelogging.png)

Görüntüde **işlem kaynaklarını** kendisi için erişebileceğiniz Microsoft konuk işletim sistemi Azure kaynaklarını temsil eder. Örneğin, Azure sanal makineler, sanal makine ölçekleme kümeleri, Azure kapsayıcı hizmeti ve dikkate alınan işlem kaynakları vb. Etkinlik günlükleri, tanılama günlüklerini ve uygulama günlüklerini kaynaklar Oluştur işlem. Daha fazla bilgi edinmek için bkz [Azure Monitoring: işlem kaynaklarını](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md#azure-monitor-sources---compute-subset) makalesi.

**Olmayan işlem kaynakları** içinde edemez temel işletim sistemi erişmek ve iş doğrudan kaynakla kaynaklardır. Örneğin, ağ güvenlik grupları, Logic Apps ve benzeri. Azure Cosmos DB bir işlem dışı kaynaktır. Etkinlik günlüğünde işlem dışı kaynakları günlüklerini görüntüleyin ya da Portalı'nda tanılama günlüklerini seçeneğini etkinleştirin. Daha fazla bilgi edinmek için bkz [Azure Monitoring: işlem dışı kaynaklar](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md#azure-monitor-sources---everything-else) makalesi.

Etkinlik günlüğü Azure Cosmos DB işlemlerinde abonelik düzeyinde kaydeder. ListKeys, yazma DatabaseAccounts ve daha fazlasını gibi işlemleri günlüğe kaydedilir. Tanılama günlükleri daha ayrıntılı günlük kaydını sağlar ve oturum DataPlaneRequests (oluşturma, okuma, sorgu vb.) ve MongoRequests olanak sağlar.


Bu makalede, Azure etkinlik günlüğü, Azure tanılama günlüklerini ve Azure ölçümleri odaklanın. Bu üç günlükler arasındaki fark nedir? 

### <a name="azure-activity-log"></a>Azure etkinlik günlüğü

Azure etkinlik günlüğü, Azure'da oluşan abonelik düzeyinde olaylar hakkında bilgi sağlayan bir abonelik günlüktür. Etkinlik günlüğü denetim düzlemi olayları aboneliklerinizi yönetim kategorisi altında için raporlar. Etkinlik günlüğü belirlemek için kullanabileceğiniz "ne, kimin, ne zaman ve" aboneliğinizdeki herhangi bir yazma işlemi (PUT, POST, DELETE) kaynaklar için. İşleminin durumunu ve ilgili diğer özellikleri de anlayabilirsiniz. 

Etkinlik günlüğü tanılama günlükleri farklıdır. Bir dış kaynaktan işlemlerde ilgili verileri etkinlik günlüğü sağlar ( _denetim düzlemi_). Azure Cosmos DB bağlamı denetim düzlemi işlemleri içeren koleksiyonu, liste anahtarları, delete anahtarları, liste veritabanı ve benzeri oluşturun. Tanılama günlüklerini bir kaynak tarafından gösterilen ve bu kaynağın işlemiyle ilgili bilgi sağlayın ( _veri düzlemi_). Bazı tanılama günlük verilerini düzlemi işlemlerinde silebilir, Ekle ve ReadFeed gösterilebilir.

Etkinlik günlükleri (Denetim düzlemi işlemleri) doğası gereği daha zengin ve arayanın, çağıran IP adresi, kaynak adı, işlem adı, Tenantıd ve daha fazla tam e-posta adresini içerebilir. Etkinlik günlüğü birkaç içeren [kategorileri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-activity-log-schema) veri. Bu kategoriler şemalara hakkında tam bilgi için bkz: [Azure etkinlik günlüğü olay şema](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-activity-log-schema). Ancak, kişisel verileri genellikle bu günlükleri yapılandırıldıktan gibi tanılama günlüklerini doğası gereği kısıtlayıcı olabilir. Arayanın IP adresine sahip, ancak son octant kaldırılır.

### <a name="azure-metrics"></a>Azure ölçümleri

[Azure ölçümleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) Azure telemetri verilerini en önemli türüne sahip (olarak da bilinir _performans sayaçları_) çoğu Azure kaynaklar tarafından gösterilen. Ölçümleri verimlilik, depolama, tutarlılık, kullanılabilirlik ve gecikme Azure Cosmos DB kaynaklarınızın hakkında bilgi görüntülemenize olanak sağlar. Daha fazla bilgi için bkz: [izleme ve ölçümleri Azure Cosmos veritabanı ile hata ayıklama](use-metrics.md).

### <a name="azure-diagnostic-logs"></a>Azure tanılama günlükleri

Azure tanılama günlüklerini bir kaynak tarafından gösterilen ve bu kaynağın işlemi hakkında zengin, sık veriler sağlar. Bu günlükler içeriğini kaynak türüne göre değişir. Kaynak düzeyi tanılama günlüklerini de konuk işletim sistemi düzeyinde tanılama günlükleri farklılık gösterir. Konuk işletim sistemi tanılama günlüklerini bir sanal makine veya diğer desteklenen içinde çalışan bir aracı tarafından toplanan kaynak türü. Kaynak düzeyi tanılama günlüklerini Azure platformu hiçbir aracı ve yakalama kaynak özgü veri gerektirir. Konuk işletim sistemi düzeyinde tanılama günlükleri, işletim sistemi ve bir sanal makinede çalışan uygulamalardan veri yakalayın.

![Depolama, olay hub'ları veya günlük analizi için tanılama günlükleri](./media/logging/azure-cosmos-db-logging-overview.png)

### <a name="what-is-logged-by-azure-diagnostic-logs"></a>Azure tanılama günlükleri tarafından günlüğe kaydedilenler?

* Erişim izinleri, sistem hataları veya hatalı istekler sonucunda başarısız olan istekleri dahil olmak üzere tüm kimliği doğrulanmış arka uç istekleri (TCP/REST) tüm API'leri üzerinden kaydedilir. Kullanıcı tarafından başlatılan grafik, Cassandra ve tablo API isteklerini desteği şu anda kullanılabilir değil.
* CRUD işlemleri tüm belgeleri, kapsayıcıları ve veritabanları dahil işlemler veritabanında kendisi.
* Oluşturma, değiştirme veya anahtarları silme dahil hesabı anahtarları üzerinde işlemler.
* Bir 401 yanıtına neden olan kimliği doğrulanmamış istekler. Örneğin, bir taşıyıcı belirteci yok veya hatalı biçimlendirilmiş ya da süresi dolmuş veya geçersiz bir belirtece sahip istekleri.

<a id="#turn-on"></a>
## <a name="turn-on-logging-in-the-azure-portal"></a>Azure portalında oturum aç

Tanılama günlük kaydını etkinleştirmek için aşağıdaki kaynaklara sahip olmalısınız:

* Bir var olan Azure Cosmos DB hesap, veritabanı ve kapsayıcı. Bu kaynaklar oluşturma ile ilgili yönergeler için bkz: [Azure portalını kullanarak bir veritabanı hesabı oluşturma](create-sql-api-dotnet.md#create-a-database-account), [Azure CLI örnekleri](cli-samples.md), veya [PowerShell örnekleri](powershell-samples.md).

Azure portalında tanılama günlük kaydını etkinleştirmek için aşağıdaki adımları uygulayın:

1. İçinde [Azure portal](https://portal.azure.com), Azure Cosmos DB hesap, seçin **tanılama günlükleri** sol gezinti ve ardından **tanılamayı açın**.

    ![Azure portalında Azure Cosmos DB için tanılama günlük kaydını etkinleştirmek](./media/logging/turn-on-portal-logging.png)

2. İçinde **tanılama ayarlarını** sayfasında, aşağıdaki adımları uygulayın: 

    * **Ad**: oluşturmak günlükleri için bir ad girin.

    * **Arşiv depolama hesabı**: Bu seçeneği kullanmak için bağlanmak için var olan bir depolama hesabı gerekir. Portalda yeni bir depolama hesabı oluşturmak için bkz: [depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md) ve bir Azure Kaynak Yöneticisi, genel amaçlı hesabı oluşturmak için yönergeleri izleyin. Ardından, depolama hesabınızı seçin için portalda bu sayfaya dönün. Yeni oluşturulan depolama hesapları açılır menüde görünmesi birkaç dakika sürebilir.
    * **Bir olay hub'ına akış**: Bu seçeneği kullanmak için bağlanmak için bir var olan olay hub'ları ad alanı ve olay hub'ı gerekir. Bir olay hub'ları ad alanı oluşturmak için bkz: [Azure portalı kullanarak bir olay hub'ları ad alanı ve bir event hub oluşturma](../event-hubs/event-hubs-create.md). Ardından, portaldaki Event Hubs ad alanı ve ilke adı seçmek için bu sayfayı geri dönün.
    * **Günlük analizi için Gönder**: Bu seçeneği kullanmak için varolan bir çalışma alanını kullanın ya da yeni bir günlük analizi çalışma alanı için adımları izleyerek oluşturun [yeni bir çalışma alanı oluşturma](../log-analytics/log-analytics-quick-collect-azurevm.md#create-a-workspace) Portalı'nda. Günlük analizi, günlükleri görüntüleme hakkında daha fazla bilgi için bkz: [görünüm günlüklerini günlük analizi](#view-in-loganalytics).
    * **Oturum DataPlaneRequests**: arka uç isteklerini temel alınan Azure Cosmos DB dağıtılmış platformu SQL, grafik, MongoDB, Cassandra ve tablo API hesapları için günlüğe kaydetmek için bu seçeneği belirleyin. Bir depolama hesabına arşivleme, tanılama günlüklerini saklama süresi seçebilirsiniz. Bekletme süresi dolduktan sonra günlükleri otomatik olarak silinir.
    * **Oturum MongoRequests**: kullanıcı tarafından başlatılan istekleri MongoDB API hesapları hizmet veren için Azure Cosmos DB ön uç oturumu için bu seçeneği belirleyin. Bir depolama hesabına arşivleme, tanılama günlüklerini saklama süresi seçebilirsiniz. Bekletme süresi dolduktan sonra günlükleri otomatik olarak silinir.
    * **Ölçüm istekleri**: ayrıntılı verileri depolamak için bu seçeneği [Azure ölçümleri](../monitoring-and-diagnostics/monitoring-supported-metrics.md). Bir depolama hesabına arşivleme, tanılama günlüklerini saklama süresi seçebilirsiniz. Bekletme süresi dolduktan sonra günlükleri otomatik olarak silinir.

3. **Kaydet**’i seçin.

    Bildiren bir hata alırsanız, "için tanılama güncelleştirilemedi \<çalışma alanı adı >. Abonelik \<abonelik kimliği > Microsoft.ınsights, kullanmak için kayıtlı değil "izleyin [sorun giderme Azure tanılama](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage) yönergeleri hesabını kaydetmek ve bu yordamı yeniden deneyin.

    Nasıl Tanılama günlüklerinize herhangi bir noktada gelecekte kaydedilir değiştirmek isterseniz, hesabınızı tanılama günlük ayarlarını değiştirmek için bu sayfaya dönün.

## <a name="turn-on-logging-by-using-azure-cli"></a>Azure CLI kullanarak günlüğünü etkinleştirme

Azure CLI kullanarak ölçümleri ve tanılama günlük kaydını etkinleştirmek için aşağıdaki komutları kullanın:

- Tanılama günlüklerini depolama bir depolama hesabında etkinleştirmek için bu komutu kullanın:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
   ```

   `resourceId` Azure Cosmos DB hesabının adıdır. `storageId` Günlükleri göndermek istediğiniz depolama hesabının adıdır.

- Tanılama günlüklerini Olay hub'ına akışını etkinleştirmek için bu komutu kullanın:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
   ```

   `resourceId` Azure Cosmos DB hesabının adıdır. `serviceBusRuleId` Bu biçiminde bir dize:

   ```azurecli-interactive
   {service bus resource ID}/authorizationrules/{key name}
   ```

- Günlük analizi çalışma alanı için gönderen tanılama günlüklerini etkinleştirmek için bu komutu kullanın:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of the log analytics workspace> --enabled true
   ```

Birden çok çıktı seçenekleri etkinleştirmek için bu parametreler birleştirebilirsiniz.

## <a name="turn-on-logging-by-using-powershell"></a>PowerShell kullanarak günlüğünü etkinleştirme

PowerShell kullanarak tanılama günlük özelliğini açmak için en az 1.0.1 sürümü ile Azure Powershell gerekir.

Azure PowerShell'i yüklemek ve Azure aboneliğinizle ilişkilendirmek için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).

Azure PowerShell'i zaten yüklemiş olduğunuz ve sürümünü PowerShell konsol türünden bilmediğiniz `(Get-Module azure -ListAvailable).Version`.  

### <a id="connect"></a>Aboneliklerinize bağlanma
Bir Azure PowerShell oturumu başlatın ve aşağıdaki komutla Azure hesabınızda oturum açın:  

```powershell
Connect-AzureRmAccount
```

Açılır tarayıcı penceresinde Azure hesabı kullanıcı adınızı ve parolanızı girin. Azure PowerShell Bu Hesapla ve varsayılan olarak ilişkilendirilmiş, birinciyi kullanır abonelikleri tümünün alır.

Birden fazla aboneliğiniz varsa Azure anahtar kasanızı oluşturmak için kullanılan belirli aboneliği belirtmeniz gerekebilir. Hesabınız için abonelikleri görmek için aşağıdaki komutu yazın:

```powershell
Get-AzureRmSubscription
```

Sonra oturum açtığınızdan Azure Cosmos DB hesabıyla ilişkili aboneliği belirtmek için aşağıdaki komutu yazın:

```powershell
Set-AzureRmContext -SubscriptionId <subscription ID>
```

> [!NOTE]
> Hesabınızla ilişkili birden fazla aboneliğiniz varsa, kullanmak istediğiniz aboneliği belirtmek önemlidir.
>
>

Azure PowerShell yapılandırma hakkında daha fazla bilgi için bkz: [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview).

### <a id="storage"></a>Günlükleriniz için yeni bir depolama hesabı oluşturma
Bu öğreticide günlükleriniz için var olan depolama hesabını kullanabilirsiniz, ancak Azure Cosmos DB günlüklerine özgü yeni bir depolama hesabı oluşturuyoruz. Kolaylık olması için depolama hesabı ayrıntıları adlı bir değişkende depolarız **sa**.

Bu öğreticide yönetim ek kolaylığı için aynı kaynak grubunun Azure Cosmos DB veritabanı içeren bir kullanırız. Kendi değerlerinizi yerleştirin **ContosoResourceGroup**, **contosocosmosdblogs**, ve **Kuzey Orta ABD** geçerli Parametreler:

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName ContosoResourceGroup `
-Name contosocosmosdblogs -Type Standard_LRS -Location 'North Central US'
```

> [!NOTE]
> Mevcut bir depolama hesabını kullanmaya karar verirseniz, hesabı Azure Cosmos DB aboneliğinizi aynı aboneliği kullanmanız gerekir. Hesap da Klasik dağıtım modeli yerine Resource Manager dağıtım modeli kullanmanız gerekir.
>
>

### <a id="identify"></a>Günlükleriniz için Azure Cosmos DB hesabını tanımlama
Adlı bir değişkene Azure Cosmos DB hesap adını ayarlayın **hesap**, burada **ResourceName** Azure Cosmos DB hesabının adıdır.

```powershell
$account = Get-AzureRmResource -ResourceGroupName ContosoResourceGroup `
-ResourceName contosocosmosdb -ResourceType "Microsoft.DocumentDb/databaseAccounts"
```

### <a id="enable"></a>Günlüğe kaydetmeyi etkinleştirme
Azure Cosmos DB için günlük kaydını etkinleştirmek için `Set-AzureRmDiagnosticSetting` yeni depolama hesabı, Azure Cosmos DB hesabı ve günlüğe kaydetmeyi etkinleştirmek için kategori için değişkenleri cmdlet'iyle. Aşağıdaki komutu çalıştırın ve ayarlayın **-etkin** bayrağını **$true**:

```powershell
Set-AzureRmDiagnosticSetting  -ResourceId $account.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories DataPlaneRequests
```

Komutu için çıktı aşağıdaki örneğe benzemelidir:

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

Komut çıktısı günlüğü veritabanınızı artık etkin ve bilgi depolama hesabınız için kaydedilen onaylar.

İsteğe bağlı olarak, eski günlükleri otomatik olarak silinir şekilde günlükleriniz için bekletme ilkesi de ayarlayabilirsiniz. Örneğin, ile Bekletme İlkesi ayarlamak **- RetentionEnabled** bayrağı ayarlanmış **$true**. Ayarlama **- RetentionInDays** parametresi **90** böylece 90 günden daha eski günlükleri otomatik olarak silinir.

```powershell
Set-AzureRmDiagnosticSetting -ResourceId $account.ResourceId`
 -StorageAccountId $sa.Id -Enabled $true -Categories DataPlaneRequests`
  -RetentionEnabled $true -RetentionInDays 90
```

### <a id="access"></a>Günlüklerinize erişme
Azure Cosmos DB günlüklerde **DataPlaneRequests** kategori depolanır **Öngörüler günlükleri-data-düzlemi-istekleri** sağladığınız depolama hesabındaki kapsayıcı. 

İlk olarak, kapsayıcı adı için bir değişken oluşturun. Değişken kılavuz kullanılır.

```powershell
    $container = 'insights-logs-dataplanerequests'
```

Tüm bu kapsayıcıdaki blobları listelemek için şunu yazın:

```powershell
Get-AzureStorageBlob -Container $container -Context $sa.Context
```

Komutu için çıktı aşağıdaki örneğe benzemelidir:

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

Bu Çıkışta gördüğünüz gibi bloblar bir adlandırma kuralı izleyin: `resourceId=/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<Database Account Name>/y=<year>/m=<month>/d=<day of month>/h=<hour>/m=<minute>/filename.json`

Tarih ve saat değerleri UTC'yi kullanır.

Aynı depolama hesabı birden fazla kaynak için günlükleri toplamak için kullanılabilir olmadığından erişmek ve gereken belirli BLOB'ları indirmek için blob adı tam Kaynak Kimliği'ni kullanabilirsiniz. Biz, yapmadan önce tüm BLOB indirmek nasıl ele.

İlk olarak, blobları yüklemek için bir klasör oluşturun. Örneğin:

```powershell
New-Item -Path 'C:\Users\username\ContosoCosmosDBLogs'`
 -ItemType Directory -Force
```

Ardından tüm blobların listesini alın:  

```powershell
$blobs = Get-AzureStorageBlob -Container $container -Context $sa.Context
```

Bu liste üzerinden kanal `Get-AzureStorageBlobContent` blobları hedef klasöre yüklemek için komut:

```powershell
$blobs | Get-AzureStorageBlobContent `
 -Destination 'C:\Users\username\ContosoCosmosDBLogs'
```

Bu ikinci komutu çalıştırdığınızda **/** blob adlarındaki sınırlayıcısı hedef klasörün altında tam klasör yapısı oluşturur. Bu klasör yapısı karşıdan yüklemek ve blobları dosya olarak depolamak için kullanılır.

Blobları seçmeli olarak indirmek için jokerleri kullanın. Örneğin:

* Birden çok veritabanı varsa ve adlı yeni bir veritabanı için günlükleri indirmek istediğiniz **CONTOSOCOSMOSDB3**, şu komutu kullanın:

    ```powershell
    Get-AzureStorageBlob -Container $container `
     -Context $sa.Context -Blob '*/DATABASEACCOUNTS/CONTOSOCOSMOSDB3
    ```

* Birden çok kaynak grupları ve yalnızca bir kaynak grubu için günlükleri indirmek isterseniz varsa, komutunu `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:

    ```powershell
    Get-AzureStorageBlob -Container $container `
    -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'
    ```
* Tüm günlükler Temmuz 2017 ay için karşıdan yüklemek isterseniz, komutunu `-Blob '*/year=2017/m=07/*'`:

    ```powershell
    Get-AzureStorageBlob -Container $container `
     -Context $sa.Context -Blob '*/year=2017/m=07/*'
    ```

Ayrıca, aşağıdaki komutları çalıştırabilirsiniz:

* Veritabanı kaynağınızın tanılama ayarlarının durumunu sorgulamak için komutunu `Get-AzureRmDiagnosticSetting -ResourceId $account.ResourceId`.
* Günlüğünü devre dışı bırakmak için **DataPlaneRequests** veritabanı hesabı kaynağınız için kategori komutunu kullanın `Set-AzureRmDiagnosticSetting -ResourceId $account.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories DataPlaneRequests`.


Her bu sorgular döndürülen BLOB'ları metin olarak depolanır ve aşağıdaki kodda gösterildiği gibi bir JSON blobu olarak biçimlendirilip:

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

## <a name="manage-your-logs"></a>Günlüklerinizi yönetmek

Azure Cosmos DB işlemi yapıldığı zamandan itibaren iki saat için tanılama günlükleri, hesabınızdaki kullanılabilir hale getirilir. Depolama hesabınızdaki günlüklerinizi yönetmek size bağlıdır:

* Bunları erişebilen günlüklerinizi güvenli ve kısıtlamak için standart Azure erişim denetimi yöntemlerini kullanın.
* Artık depolama hesabınızda tutmak istemediğiniz günlükleri silin.
* Bir depolama hesabına arşivlenen verileri düzlemi istekleri için saklama dönemi Portalı'nda yapılandırılmış zaman **günlük DataPlaneRequests** ayarı seçilidir. Bu ayarı değiştirmek için bkz: [Azure portalında oturum aç](#turn-on-logging-in-the-azure-portal).


<a id="#view-in-loganalytics"></a>
## <a name="view-logs-in-log-analytics"></a>Günlük analizi içinde günlüklerini görüntüle

Seçtiyseniz, **için günlük analizi Gönder** seçeneğini tanılama günlük özelliğini tanılama açık koleksiyonunuzu verilerden günlük analizi için iki saat içinde iletilir. Günlük özelliğini açtıktan hemen sonra günlük analizi baktığınızda, herhangi bir veri görmezsiniz. Yalnızca iki saat bekleyin ve yeniden deneyin. 

Günlüklerinizi görüntüleyebilmek denetleyin ve günlük analizi çalışma alanınız yeni günlük analizi sorgu dili kullanmak için yükseltilmiş varsa bkz. Denetlemek için açık [Azure portal](https://portal.azure.com)seçin **günlük analizi** uzak üzerinde sol, ardından çalışma alanı adı sonraki görüntüde gösterildiği gibi seçin. **OMS çalışma** sayfası görüntülenir:

![Azure portalında günlük analizi](./media/logging/azure-portal.png)

Aşağıdaki iletiyi görürseniz **OMS çalışma** sayfasında, çalışma alanınızda yeni dil kullanmak üzere yükseltildi kurmadı. Yeni sorgu dili yükseltme hakkında daha fazla bilgi için bkz: [Azure günlük analizi çalışma alanınız için yeni günlük arama yükseltme](../log-analytics/log-analytics-log-search-upgrade.md). 

![Günlük analizi message yükseltme](./media/logging/upgrade-notification.png)

Günlük analizi tanılama verilerini görüntülemek için açın **günlük arama** soldaki menüden sayfasından veya **Yönetim** aşağıdaki görüntüde gösterildiği gibi sayfasının alanı:

![Azure portalında oturum arama seçenekleri](./media/logging/log-analytics-open-log-search.png)

Veri toplama etkinleştirdikten, 10 en son günlükleri görmek için yeni sorgu dili kullanarak aşağıdaki günlük arama örneği çalıştırmak `AzureDiagnostics | take 10`.

![Örnek günlük 10 en son günlükleri arayın](./media/logging/log-analytics-query.png)

<a id="#queries"></a>
### <a name="queries"></a>Sorgular

İçine girebilirsiniz bazı ek sorgular şunlardır **günlük arama** Azure Cosmos DB kapsayıcılarınızı izlemenize yardımcı olması için kutusu. Bu sorguları çalışmak [yeni dil](../log-analytics/log-analytics-log-search-upgrade.md). 

Her günlük araması tarafından döndürülen veri anlamını öğrenmek için bkz: [Azure Cosmos DB günlüklerinizi yorumlama](#interpret).

* Tüm Azure Cosmos DB'den tanılama günlükleri için belirli bir süre için sorgulamak için:

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests"
    ```

* 10 en son sorgulamak için olayları günlüğe:

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | take 10
    ```

* İşlem türüne göre gruplandırılmış tüm işlemleri için sorgulamak için:

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by OperationName
    ```

* Sorguya göre gruplandırılmış tüm işlemleri için **kaynak**:

    ```
    AzureActivity | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by Resource
    ```

* Kaynağa göre gruplandırılmış tüm kullanıcı etkinliğini sorgulamak için:

    ```
    AzureActivity | where Caller == "test@company.com" and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by Resource
    ```
    > [!NOTE]
    > Bu komut bir tanılama günlük bir etkinlik günlüğü için geçerlidir.

* İşlemleri 3 milisaniye uzun sürer sorgulamak için:

    ```
    AzureDiagnostics | where toint(duration_s) > 30000 and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by clientIpAddress_s, TimeGenerated
    ```

* Hangi aracı için işlemleri çalıştıran sorgulamak için:

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by OperationName, userAgent_s
    ```

* Uzun süre çalışan işlemleri gerçekleştirildiğinde sorgulamak için:

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | project TimeGenerated , toint(duration_s)/1000 | render timechart
    ```

Yeni günlük arama dili kullanma hakkında daha fazla bilgi için bkz: [anlayın günlük günlük analizi aramalarda](../log-analytics/log-analytics-log-search-new.md). 

## <a id="interpret"></a>Günlüklerinizi yorumlama

Azure Storage ve günlük analizi depolanan Tanılama verileri benzer bir şema kullanır. 

Aşağıdaki tabloda her günlük girişinin içeriğini açıklar.

| Azure depolama alanı veya özelliği | Günlük analizi özelliği | Açıklama |
| --- | --- | --- |
| **Saat** | **TimeGenerated** | Tarih ve saat (UTC) işlemi oluştuğunda. |
| **resourceId** | **Kaynak** | Günlükleri etkin Azure Cosmos DB hesabı.|
| **Kategori** | **Kategori** | Azure Cosmos DB günlükleri için **DataPlaneRequests** yalnızca kullanılabilen değerdir. |
| **OperationName** | **OperationName** | İşlemin adı. Bu değer aşağıdaki işlemlerden birini olabilir: oluşturma, güncelleştirme, okuma, ReadFeed, Sil, Değiştir, yürütme, SqlQuery, sorgu, JSQuery, Head, HeadFeed veya Upsert.   |
| **özellikleri** | yok | Bu alanın içeriğini izleyen satırları açıklanmaktadır. |
| **activityId** | **activityId_g** | Oturum işlemi için benzersiz bir GUID. |
| **UserAgent** | **userAgent_s** | İsteği gerçekleştiren istemcinin kullanıcı aracısı belirten bir dize. Biçimi {kullanıcı aracısı adıdır} / {version}.|
| **resourceType** | **Kaynak türü** | Erişilen kaynak türü. Bu değer aşağıdaki kaynak türlerinden herhangi birinde olabilir: veritabanı, koleksiyon, belge, ek, kullanıcı, izin, StoredProcedure, tetikleyici, UserDefinedFunction veya teklif. |
| **statusCode** | **statusCode_s** | İşlem yanıt durumu. |
| **requestResourceId** | **ResourceId** | İsteği ilgilidir ResourceId. Değer databaseRid, collectionRid veya documentRid yapılan işleme bağlı olarak işaret edebilir.|
| **clientIpAddress** | **clientIpAddress_s** | İstemcinin IP adresi. |
| **requestCharge** | **requestCharge_s** | İşlem tarafından kullanılan RUs sayısı |
| **collectionRid** | **collectionId_s** | Koleksiyon için benzersiz kimlik.|
| **Süre** | **duration_s** | İşlemde çizgilerine süresi. |
| **requestLength** | **requestLength_s** | İstek bayt cinsinden uzunluğu. |
| **responseLength** | **responseLength_s** | Yanıtın bayt cinsinden uzunluğu.|
| **resourceTokenUserRid** | **resourceTokenUserRid_s** | Bu değer boş olduğunda [kaynak belirteçleri](https://docs.microsoft.com/azure/cosmos-db/secure-access-to-data#resource-tokens) kimlik doğrulaması için kullanılır. Değerin kullanıcının kaynak Kimliğini gösterir. |

## <a name="next-steps"></a>Sonraki adımlar

- Günlüğe kaydetme ve çeşitli Azure Hizmetleri tarafından desteklenen ölçümleri ve günlük kategorileri etkinleştirme anlamak için hem de okuma [Microsoft Azure ölçümlerini genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md) ve [genel bakış, Azure tanılama günlükleri ](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) makaleler.
- Olay hub'ları hakkında bilgi edinmek için bu makaleler okuyun:
   - [Azure Event Hubs nedir?](../event-hubs/event-hubs-what-is-event-hubs.md)
   - [Event Hubs kullanmaya başlayın](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
- Okuma [Azure depolama biriminden ölçümleri ve tanılama günlüklerini indirin](../storage/blobs/storage-quickstart-blobs-dotnet.md#download-blobs).
- Okuma [anlayın günlük günlük analizi aramalarda](../log-analytics/log-analytics-log-search-new.md).
