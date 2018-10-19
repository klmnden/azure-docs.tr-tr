---
title: Azure Analysis Services için Diganostic günlüğü | Microsoft Docs
description: Azure Analysis Services için tanılama günlüğüne kaydetmeyi ayarlama bilgi edinin.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: a8d6080b573cbad1004166f28a3e6596560241be
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49426524"
---
# <a name="setup-diagnostic-logging"></a>Tanılama günlüğüne kaydetmeyi ayarlama

Herhangi bir Analysis Services çözümü önemli bir parçası sunucularınızı performansını izler. İle [Azure kaynak tanılama günlükleri](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md), izleyebilir ve günlükleri Gönder [Azure depolama](https://azure.microsoft.com/services/storage/), kendisine akış [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)ve bunları dışarı aktarma [günlüğü Analytics](https://azure.microsoft.com/services/log-analytics/), hizmetinde [Azure](https://www.microsoft.com/cloud-platform/operations-management-suite). 

![Depolama, Event Hubs veya Log Analytics için tanılama günlüğüne kaydetme](./media/analysis-services-logging/aas-logging-overview.png)


## <a name="whats-logged"></a>Günlüğe kaydedilenler?

Seçebileceğiniz **altyapısı**, **hizmet**, ve **ölçümleri** kategorileri.

### <a name="engine"></a>Altyapısı

Seçme **altyapısı** tüm günlükleri [Xevent'ler](https://docs.microsoft.com/sql/analysis-services/instances/monitor-analysis-services-with-sql-server-extended-events). Olayları tek tek seçemezsiniz. 

|XEvent kategorileri |Olay adı  |
|---------|---------|
|Güvenlik Denetimi    |   Denetim oturum açma      |
|Güvenlik Denetimi    |   Denetim oturumunu kapatma      |
|Güvenlik Denetimi    |   Denetim sunucusu başlatır ve durdurur      |
|İlerleme raporları     |   İlerleme raporu başlangıç      |
|İlerleme raporları     |   İlerleme raporu bitiş olayı      |
|İlerleme raporları     |   İlerleme raporu geçerli      |
|Sorgular     |  Sorgu başlangıç       |
|Sorgular     |   Sorgu bitiş olayı      |
|Komutlar     |  Komut başlangıç       |
|Komutlar     |  Komut bitiş olayı       |
|Hatalar ve uyarılar     |   Hata      |
|Keşif     |   Son keşfedin      |
|Bildirim     |    Bildirim     |
|Oturum     |  Oturum başlatma       |
|Kilitler    |  Çıkmaz       |
|Sorgu işleme     |   VertiPaq SE sorgusu başla      |
|Sorgu işleme     |   VertiPaq SE sorgusu bitiş      |
|Sorgu işleme     |   VertiPaq SE sorgusu önbellek eşleştir      |
|Sorgu işleme     |   Doğrudan sorgu başlangıç      |
|Sorgu işleme     |  Doğrudan sorgu bitiş olayı       |

### <a name="service"></a>Hizmet

|İşlem adı  |Gerçekleşir,  |
|---------|---------|
|ResumeServer     |    Bir sunucuyu devam ettirin     |
|SuspendServer    |   Bir sunucu pause      |
|DeleteServer     |    Bir sunucu silme     |
|RestartServer    |     Kullanıcı SSMS veya PowerShell aracılığıyla bir sunucu yeniden başlatılır.    |
|GetServerLogFiles    |    Sunucu günlüğü PowerShell aracılığıyla kullanıcı verir     |
|ExportModel     |   Visual Studio'da Aç kullanarak Kullanıcı Portalı'nda bir modeli dışarı aktarır     |

### <a name="all-metrics"></a>Tüm ölçümleri

Ölçüm kategorisi aynı oturum [Sunucu ölçümlerini](analysis-services-monitor.md#server-metrics) ölçümlerde görüntülenir.

## <a name="setup-diagnostics-logging"></a>Tanılama günlüğüne kaydetmeyi ayarlama

### <a name="azure-portal"></a>Azure portal

1. İçinde [Azure portalında](https://portal.azure.com) > sunucu, tıklayın **tanılama günlükleri** 'a tıklayın ve sol gezinti **tanılamayı Aç**.

    ![Azure portalında Azure Cosmos DB için tanılama günlük kaydını açma](./media/analysis-services-logging/aas-logging-turn-on-diagnostics.png)

2. İçinde **tanılama ayarları**, aşağıdaki seçenekleri belirtin: 

    * **Ad**. Oluşturmak günlükleri için bir ad girin.

    * **Bir depolama hesabında arşivle**. Bu seçeneği kullanmak için bağlanmak için mevcut bir depolama hesabı gerekir. Bkz: [depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md). Bir kaynak yöneticisi, genel amaçlı hesap oluşturmak için yönergeleri izleyin, sonra Portalı'nda bu sayfaya dönerek, depolama hesabınızı seçin. Bu, yeni oluşturulan depolama hesapları, aşağı açılan menüsünün görünmesi birkaç dakika sürebilir.
    * **Olay hub'ına Stream**. Bu seçeneği kullanmak için bağlanmak için mevcut bir olay hub'ı ad alanı ve olay hub'gerekir. Daha fazla bilgi için bkz. [bir Event Hubs ad alanı ve Azure portalını kullanarak bir olay hub'ı oluşturma](../event-hubs/event-hubs-create.md). Ardından olay hub'ı ad alanı ve ilke adını seçmek için Portalı'nda bu sayfaya dönün.
    * **Log Analytics'e gönderme**. Bu seçeneği kullanmak için mevcut bir çalışma kullanabilir veya yeni bir Log Analytics çalışma alanı için adımları izleyerek oluşturabilirsiniz [yeni bir çalışma alanı oluşturma](../log-analytics/log-analytics-quick-collect-azurevm.md#create-a-workspace) portalında. Log Analytics'te, günlükleri görüntüleme hakkında daha fazla bilgi için bkz. [görünümü Log Analytics'te oturum](#view-in-loganalytics).

    * **Altyapısı**. Xevent'ler yazmak için bu seçeneği seçin. Bir depolama hesabına arşivleme tanılama günlükleri için saklama süresi seçebilirsiniz. Bekletme süresi dolduktan sonra autodeleted günlüklerdir.
    * **Hizmet**. Hizmet düzeyi olayları günlüğe kaydetmek için bu seçeneği belirleyin. Bir depolama hesabına arşivleme, tanılama günlükleri için saklama süresi seçebilirsiniz. Bekletme süresi dolduktan sonra autodeleted günlüklerdir.
    * **Ölçümleri**. Ayrıntılı verileri depolamak için bu seçeneği [ölçümleri](analysis-services-monitor.md#server-metrics). Bir depolama hesabına arşivleme, tanılama günlükleri için saklama süresi seçebilirsiniz. Bekletme süresi dolduktan sonra autodeleted günlüklerdir.

3. **Kaydet**’e tıklayın.

    Bildiren bir hata alırsanız, "tanılama için güncelleştirilemedi \<çalışma alanı adı >. Abonelik \<abonelik kimliği > microsoft.insights kullanmak için kayıtlı değil. " izleyin [Azure tanılama sorunlarını giderme](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage) yönergeleri hesabını kaydetmek için bu yordamı sonra yeniden deneyin.

    Nasıl Tanılama günlüklerinizi herhangi bir noktada gelecekte kaydedilir değiştirmek istiyorsanız, ayarları değiştirmek için bu sayfaya geri dönebilirsiniz.

### <a name="powershell"></a>PowerShell

Koyulmanız için gerekli olan temel komutlar aşağıdadır. PowerShell kullanarak bir depolama hesabına günlük ayarlamaya ilişkin adım adım yardım istiyorsanız, bu makalenin ilerleyen bölümlerinde bkz.

Ölçümleri ve tanılama PowerShell kullanarak etkinleştirmek için aşağıdaki komutları kullanın:

- Tanılama günlükleri bir depolama hesabında depolama etkinleştirmek için bu komutu kullanın:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
   ```

   Depolama hesabı kimliği kaynak depolama hesabı için günlükleri göndermek istediğiniz kimliğidir.

- Tanılama günlükleri Olay hub'ına akışını etkinleştirmek için bu komutu kullanın:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
   ```

   Azure Service Bus kural kimliği şu biçime sahip bir dizedir:

   ```powershell
   {service bus resource ID}/authorizationrules/{key name}
   ``` 

- Log Analytics çalışma alanına gönderme tanılama günlüklerini etkinleştirmek için bu komutu kullanın:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
   ```

- Log Analytics çalışma alanınızın kaynak Kimliğini aşağıdaki komutu kullanarak elde edebilirsiniz:

   ```powershell
   (Get-AzureRmOperationalInsightsWorkspace).ResourceId
   ```

Birden çok çıkış seçeneği etkinleştirmek için şu parametreleri birleştirebilirsiniz.

### <a name="rest-api"></a>REST API

Bilgi edinmek için nasıl [Azure İzleyici REST API'sini kullanarak tanılama ayarlarını değiştirme](https://msdn.microsoft.com/library/azure/dn931931.aspx). 

### <a name="resource-manager-template"></a>Resource Manager şablonu

Bilgi edinmek için nasıl [Resource Manager şablonu kullanarak kaynak oluşturma sırasında tanılama ayarlarını etkinleştirme](../monitoring-and-diagnostics/monitoring-enable-diagnostic-logs-using-template.md). 

## <a name="manage-your-logs"></a>Günlüklerinizi yönetme

Günlükleri günlüğünü ayarlama, birkaç saat içinde genellikle kullanılabilir. Depolama hesabınızdaki günlüklerinizi yönetmek size bağlıdır:

* Günlüklerinize erişebilecek kişileri kısıtlayarak güvenliklerini sağlamak için standart Azure erişim denetimi yöntemlerini kullanın.
* Artık depolama hesabınızda tutmak istemediğiniz günlükleri silin.
* Bir bekletme dönemi için eski günlükleri depolama hesabınızdan silinir şekilde ayarladığınızdan emin olun.

## <a name="view-logs-in-log-analytics"></a>Log Analytics’te günlükleri görüntüleme

Ölçümler ve sunucu olaylarının, Log analytics'te Xevent'ler yan yana analiz ile tümleştirilir. Log Analytics, diğer Azure Hizmetleri Mimarinizi tanılama günlük verilerini bütünsel bir görünümünü sağlayarak olayları alacak şekilde de yapılandırılabilir.

Log Analytics'te tanılama verilerinizi görüntülemek için günlük araması sayfasını sol menü ya da Yönetim alanında, aşağıda gösterildiği gibi açın.

![Azure portalında günlük arama seçenekleri](./media/analysis-services-logging/aas-logging-open-log-search.png)

İçinde veri toplama etkinleştirdikten göre **günlük araması**, tıklayın **toplanan tüm verileri**.

İçinde **türü**, tıklayın **AzureDiagnostics**ve ardından **Uygula**. AzureDiagnostics altyapısı ve hizmet olaylarını içerir. Log Analytics sorgusu üzerinde anında oluşturulan dikkat edin. EventClass\_s alanında xEvent adları, günlüğe kaydetme için şirket içi Xevent'ler kullandıysanız tanıdık gelmiş.

Tıklayın **EventClass\_s** veya Log Analytics ve olay adları birini devam bir sorgu oluşturma. Sorgularınızı daha sonra kullanmak üzere kaydettiğinizden emin olun.

Gelişmiş sorgu, yönelik Kompozit ve toplanan veriler uyarı verme özellikleri sağlayan bir Web sitesi log Analytics, mutlaka inceleyin.

### <a name="queries"></a>Sorgular

Sorguları kullanabileceğiniz yüzlerce vardır. Başlamanıza yardımcı olmak için birkaç aşağıdadır.
Yeni günlük araması sorgu dili kullanma hakkında daha fazla bilgi edinmek için [günlük aramalarını anlama Log Analytics'te](../log-analytics/log-analytics-log-search-new.md). 

* Sorgu döndürme sorgular Azure Analysis Services için beş dakika (300.000 milisaniye cinsinden) tamamlanması için geçen gönderildi.

    ```
    search * | where ( Type == "AzureDiagnostics" ) | where ( EventClass_s == "QUERY_END" ) | where toint(Duration_s) > 300000
    ```

* Ölçek genişletme yinelemeler tanımlayın.

    ```
    search * | summarize count() by ServerName_s
    ```
    Ölçek genişletme kullanırken, çünkü salt okunur çoğaltmalar tanımlayabilirsiniz ServerName\_s alan değerleri olan adına çoğaltma örneği sayısı. Kullanıcıların gördüğü sunucu adı ile eşleşen Azure kaynağı adı içeren kaynak alan. Yinelemeler için doğru IsQueryScaleoutReadonlyInstance_s alanın eşittir.



> [!TIP]
> Paylaşmak istediğiniz harika bir Log Analytics sorgusu var mı? Bir GitHub hesabı varsa, bu makalede ekleyebilirsiniz. Tıklamanız yeterli **Düzenle** bu sayfanın üst sağ.


## <a name="tutorial---turn-on-logging-by-using-powershell"></a>Öğretici: PowerShell kullanarak günlük kaydını etkinleştirin
Bu hızlı öğreticiyi analiz sunucunuz olarak bir depolama hesabı aynı abonelikte ve kaynak grubu oluşturun. Günlüğe kaydetme, çıkış göndermek için yeni depolama hesabı tanılamayı etkinleştirmek için Set-AzureRmDiagnosticSetting kullanın.

### <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi tamamlamak için aşağıdaki kaynaklara sahip olmalıdır:

* Mevcut bir Azure Analysis Services sunucusu. Sunucu kaynağı oluşturma ile ilgili yönergeler için bkz: [Azure portalında bir sunucu oluşturma](analysis-services-create-server.md), veya [PowerShell kullanarak bir Azure Analysis Services sunucusu oluşturma](analysis-services-create-powershell.md).

### <a name="aconnect-to-your-subscriptions"></a></a>Aboneliklerinize bağlanma

Bir Azure PowerShell oturumu başlatın ve aşağıdaki komutla Azure hesabınızda oturum açın:  

```powershell
Connect-AzureRmAccount
```

Açılır tarayıcı penceresinde Azure hesabı kullanıcı adınızı ve parolanızı girin. Azure PowerShell bu hesapla ilişkili tüm abonelikleri alır ve varsayılan olarak birinciyi kullanır.

Birden çok aboneliğiniz varsa Azure Anahtar Kasanızı oluşturmak için kullanılan belirli bir tanesini belirtmeniz gerekebilir. Hesabınız için abonelikleri görmek üzere aşağıdakini yazın:

```powershell
Get-AzureRmSubscription
```

Sonra oturum açtığınız Azure Analysis Services hesabıyla ilişkili aboneliği belirtmek için şunu yazın:

```powershell
Set-AzureRmContext -SubscriptionId <subscription ID>
```

> [!NOTE]
> Hesabınızla ilişkili birden fazla aboneliğiniz varsa, aboneliği belirtmek önemlidir.
>
>

### <a name="create-a-new-storage-account-for-your-logs"></a>Günlükleriniz için yeni bir depolama hesabı oluşturma

Sunucunuz ile aynı abonelikte olması koşuluyla günlükleriniz için var olan bir depolama hesabı kullanabilirsiniz. Bu öğretici için yeni bir depolama hesabı günlükleri Analysis Services için ayrılmış da oluşturun. Bunu kolaylaştırmak için depolama hesabı ayrıntıları adlı bir değişkende depoladığınız **sa**.

Ayrıca Analysis Services sunucunuz içeren kümeyle aynı kaynak grubunu kullanın. İçin değerler yerine `awsales_resgroup`, `awsaleslogs`, ve `West Central US` kendi değerlerinizle:

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName awsales_resgroup `
-Name awsaleslogs -Type Standard_LRS -Location 'West Central US'
```

### <a name="identify-the-server-account-for-your-logs"></a>Günlükleriniz için sunucu hesabı belirle

Hesap adı adlı bir değişken ayarlamak **hesabı**, burada ResourceName hesabının adıdır.

```powershell
$account = Get-AzureRmResource -ResourceGroupName awsales_resgroup `
-ResourceName awsales -ResourceType "Microsoft.AnalysisServices/servers"
```

### <a name="enable-logging"></a>Günlü kaydını etkinleştir

Günlüğe kaydetmeyi etkinleştirmek için yeni depolama hesabı, sunucu hesabı ve kategori için değişkenleri birlikte Set-AzureRmDiagnosticSetting cmdlet'ini kullanın. Ayar aşağıdaki komutu çalıştırın **-etkin** bayrak **$true**:

```powershell
Set-AzureRmDiagnosticSetting  -ResourceId $account.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories Engine
```

Çıktı aşağıdakine benzer görünmelidir:

```powershell
StorageAccountId            : 
/subscriptions/a23279b5-xxxx-xxxx-xxxx-47b7c6d423ea/resourceGroups/awsales_resgroup/providers/Microsoft.Storage/storageAccounts/awsaleslogs
ServiceBusRuleId            :
EventHubAuthorizationRuleId :
Metrics                    
    TimeGrain       : PT1M
    Enabled         : False
    RetentionPolicy
    Enabled : False
    Days    : 0


Logs                       
    Category        : Engine
    Enabled         : True
    RetentionPolicy
    Enabled : False
    Days    : 0


    Category        : Service
    Enabled         : False
    RetentionPolicy
    Enabled : False
    Days    : 0


WorkspaceId                 :
Id                          : /subscriptions/a23279b5-xxxx-xxxx-xxxx-47b7c6d423ea/resourcegroups/awsales_resgroup/providers/microsoft.analysisservic
es/servers/awsales/providers/microsoft.insights/diagnosticSettings/service
Name                        : service
Type                        :
Location                    :
Tags                        :
```

Bu günlük için depolama hesabı bilgileri kaydedilirken bu sunucu için artık etkinleştirildiğini doğrular.

Eski günlüklerin otomatik olarak silinir şekilde günlükleriniz için bekletme ilkesi de ayarlayabilirsiniz. Örneğin, Bekletme İlkesi'ni kullanarak ayarlayın **- RetentionEnabled** bayrak **$true**ve **- Retentionındays** parametresi **90**. 90 günden eski olan günlükler otomatik olarak silinir.

```powershell
Set-AzureRmDiagnosticSetting -ResourceId $account.ResourceId`
 -StorageAccountId $sa.Id -Enabled $true -Categories Engine`
  -RetentionEnabled $true -RetentionInDays 90
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [Azure kaynak tanılama günlüğüne kaydetme](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).

Bkz: [Set-AzureRmDiagnosticSetting](https://docs.microsoft.com/powershell/module/azurerm.insights/Set-AzureRmDiagnosticSetting) PowerShell Yardım.
