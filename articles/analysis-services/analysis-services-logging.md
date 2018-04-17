---
title: Azure Analysis Services için Diganostic günlüğü | Microsoft Docs
description: Azure Analysis Services için tanılama günlük kaydını ayarlama hakkında bilgi edinin.
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 12f745958130e931bc3c8e81a0a61f3c3f4c4e3c
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="setup-diagnostic-logging"></a>Tanılama Günlüğü Kurulumu

Sunucularınızın nasıl performans gösterdiğini herhangi bir Analysis Services çözümü önemli bir kısmını izlemektedir. İle [Azure kaynak tanılama günlüklerini](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md), izlemek ve günlükleri şuraya gönderin: [Azure Storage](https://azure.microsoft.com/services/storage/), bunları akış [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)ve bunları dışarı aktarma [günlük Analytics](https://azure.microsoft.com/services/log-analytics/), bir hizmet olarak [Azure](https://www.microsoft.com/cloud-platform/operations-management-suite). 

![Depolama, olay hub'ları veya günlük analizi için tanılama günlükleri](./media/analysis-services-logging/aas-logging-overview.png)


## <a name="whats-logged"></a>Ne kaydedilir?

Seçebileceğiniz **altyapısı**, **hizmet**, ve **ölçümleri** kategoriler.

### <a name="engine"></a>Altyapısı

Seçme **altyapısı** tüm günlükleri [Xevent'ler](https://docs.microsoft.com/sql/analysis-services/instances/monitor-analysis-services-with-sql-server-extended-events). Olayları tek tek seçemezsiniz. 

|XEvent kategorileri |Olay adı  |
|---------|---------|
|Güvenlik Denetimi    |   Denetim oturum açma      |
|Güvenlik Denetimi    |   Denetim kapatma      |
|Güvenlik Denetimi    |   Sunucu başlatıldığında ve durdurulduğunda denetim      |
|İlerleme durumu raporları     |   İlerleme raporu başla      |
|İlerleme durumu raporları     |   İlerleme raporu bitiş      |
|İlerleme durumu raporları     |   İlerleme raporu geçerli      |
|Sorgular     |  Başlangıç sorgu       |
|Sorgular     |   Sorgu son      |
|Komutlar     |  Komut başlangıç       |
|Komutlar     |  Komut bitiş       |
|Hatalar ve uyarılar     |   Hata      |
|Keşif     |   Bulma bitiş olayı      |
|Bildirim     |    Bildirim     |
|Oturum     |  Oturum başlatma       |
|Kilitler    |  Kilitlenme       |
|Sorgu işleme     |   VertiPaq SE sorgusu başla      |
|Sorgu işleme     |   VertiPaq SE sorgusu bitiş      |
|Sorgu işleme     |   VertiPaq SE sorgusu önbellek eşleşmiyor      |
|Sorgu işleme     |   Doğrudan sorgu başla      |
|Sorgu işleme     |  Doğrudan sorgu bitiş       |

### <a name="service"></a>Hizmet

|İşlem adı  |Oluşur olduğunda  |
|---------|---------|
|CreateGateway     |   Kullanıcı bir ağ geçidi sunucusunda yapılandırır.      |
|ResumeServer     |    Bir sunucu Sürdür     |
|SuspendServer    |   Bir sunucu duraklatma      |
|DeleteServer     |    Bir sunucu silme     |
|RestartServer    |     Kullanıcı SSMS veya PowerShell ile bir sunucuyu yeniden başlatır    |
|GetServerLogFiles    |    Sunucu günlüğü PowerShell aracılığıyla kullanıcı verir     |
|ExportModel     |   Visual Studio'da Aç kullanarak Kullanıcı Portalı'nda bir model dışarı aktarmalar     |

### <a name="all-metrics"></a>Tüm ölçümleri

Ölçümleri kategori aynı oturum [sunucusu ölçümlerini](analysis-services-monitor.md#server-metrics) ölçümlerini görüntülenir.

## <a name="setup-diagnostics-logging"></a>Tanılama Günlüğü Kurulumu

### <a name="azure-portal"></a>Azure portalına

1. İçinde [Azure portal](https://portal.azure.com) > sunucu, tıklatın **tanılama günlükleri** sol gezinti ve ardından **tanılamayı açın**.

    ![Azure portalında Azure Cosmos DB için tanılama günlük kaydını etkinleştirmek](./media/analysis-services-logging/aas-logging-turn-on-diagnostics.png)

2. İçinde **tanılama ayarlarını**, aşağıdaki seçenekleri belirtin: 

    * **Ad**. Günlükleri oluşturmak için bir ad girin.

    * **Arşiv depolama hesabı**. Bu seçeneği kullanmak için bağlanmak için var olan bir depolama hesabı gerekir. Bkz: [depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md). Kaynak Yöneticisi, genel amaçlı hesabı oluşturmak için yönergeleri izleyin sonra bu sayfaya portalında döndürerek depolama hesabınızı seçin. Yeni oluşturulan depolama hesapları açılır menüde görünmesi birkaç dakika sürebilir.
    * **Bir olay hub'ına akış**. Bu seçeneği kullanmak için bağlanmak için bir var olan olay hub'ı ad alanı ve olay hub'ı gerekir. Daha fazla bilgi için bkz: [bir olay hub'ları ad alanı oluşturup Azure portalını kullanarak bir event hub](../event-hubs/event-hubs-create.md). Olay hub'ı ad alanı ve ilke adı seçmek için portal bu sayfaya dönün.
    * **Günlük analizi için Gönder**. Bu seçeneği kullanmak için varolan bir çalışma alanını kullanın ya da yeni bir günlük analizi çalışma alanı için adımları izleyerek oluşturun [yeni bir çalışma alanı oluşturma](../log-analytics/log-analytics-quick-collect-azurevm.md#create-a-workspace) Portalı'nda. Günlük analizi, günlükleri görüntüleme hakkında daha fazla bilgi için bkz: [görünüm günlüklerini günlük analizi](#view-in-loganalytics).

    * **Altyapısı**. Xevent'ler yazmak için bu seçeneği belirleyin. Bir depolama hesabına arşivleme, tanılama günlüklerini saklama süresi seçebilirsiniz. Bekletme süresi dolduktan sonra günlüklerin autodeleted.
    * **Hizmet**. Hizmet düzeyi olayları günlüğe kaydetmek için bu seçeneği belirleyin. Bir depolama hesabına arşivleme, tanılama günlüklerini saklama süresi seçebilirsiniz. Bekletme süresi dolduktan sonra günlüklerin autodeleted.
    * **Ölçümleri**. Ayrıntılı verileri depolamak için bu seçeneği [ölçümleri](analysis-services-monitor.md#server-metrics). Bir depolama hesabına arşivleme, tanılama günlüklerini saklama süresi seçebilirsiniz. Bekletme süresi dolduktan sonra günlüklerin autodeleted.

3. **Kaydet**’e tıklayın.

    Bildiren bir hata alırsanız, "için tanılama güncelleştirilemedi \<çalışma alanı adı >. Abonelik \<abonelik kimliği > Microsoft.ınsights kullanmak için kayıtlı değil. " izleyin [sorun giderme Azure tanılama](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage) hesabını kaydetmek için yönergeler, bu yordamı yeniden deneyin.

    Tanılama günlüklerinize herhangi bir noktada gelecekte kaydedilme değiştirmek istiyorsanız, ayarlarını değiştirmek için bu sayfaya geri dönebilirsiniz.

### <a name="powershell"></a>PowerShell

Alabileceğiniz temel komutlar şunlardır. PowerShell kullanarak bir depolama hesabı için günlük ayarlamayla ilgili adım adım yönergeler istiyorsanız, bu makalenin sonraki bölümlerinde öğretici bakın.

Ölçümleri ve PowerShell kullanarak günlüğü Tanılama'yı etkinleştirmek için aşağıdaki komutları kullanın:

- Tanılama günlüklerinin bir depolama hesabındaki depolama etkinleştirmek için bu komutu kullanın:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
   ```

   Depolama hesabı depolama hesabı için kaynak kimliği günlükleri göndermek istediğiniz kimliğidir.

- Tanılama günlüklerini bir olay hub'ına akışını etkinleştirmek için bu komutu kullanın:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
   ```

   Azure Service Bus kural kimliği bu biçiminde bir dize şöyledir:

   ```powershell
   {service bus resource ID}/authorizationrules/{key name}
   ``` 

- Günlük analizi çalışma alanı için gönderen tanılama günlüklerini etkinleştirmek için bu komutu kullanın:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
   ```

- Günlük analizi çalışma alanınız kaynak Kimliğini aşağıdaki komutu kullanarak elde edebilirsiniz:

   ```powershell
   (Get-AzureRmOperationalInsightsWorkspace).ResourceId
   ```

Birden çok çıktı seçenekleri etkinleştirmek için bu parametreler birleştirebilirsiniz.

### <a name="rest-api"></a>REST API

Bilgi edinmek için nasıl [Azure İzleyici REST API'sini kullanarak tanılama ayarlarını değiştirme](https://msdn.microsoft.com/library/azure/dn931931.aspx). 

### <a name="resource-manager-template"></a>Resource Manager şablonu

Bilgi edinmek için nasıl [Resource Manager şablonu kullanarak kaynak oluşturmada tanılama ayarları etkinleştirmek](../monitoring-and-diagnostics/monitoring-enable-diagnostic-logs-using-template.md). 

## <a name="manage-your-logs"></a>Günlüklerinizi yönetmek

Günlükleri günlüğünü ayarlama, birkaç saat içinde genellikle kullanılabilir. Depolama hesabınızdaki günlüklerinizi yönetmek size bağlıdır:

* Günlüklerinize erişebilecek kişileri kısıtlayarak güvenliklerini sağlamak için standart Azure erişim denetimi yöntemlerini kullanın.
* Artık depolama hesabınızda tutmak istemediğiniz günlükleri silin.
* Bir bekletme dönemi için eski günlükleri depolama hesabınızdan silindi şekilde ayarladığınızdan emin olun.

## <a name="view-logs-in-log-analytics"></a>Günlük analizi içinde günlüklerini görüntüle

Ölçümleri ve sunucu olaylarını yan yana çözümleme için günlük analizi, Xevent'ler ile tümleştirilir. Günlük analizi Mimarinizi arasında tanılama günlük verilerini bütünsel bir görünümünü sağlayarak diğer Azure hizmetleriyle olayları alacak şekilde de yapılandırılabilir.

Günlük analizi tanılama verilerini görüntülemek için günlük arama sayfası soldaki menüden veya Yönetim alanında, aşağıda gösterildiği gibi açın.

![Azure portalında oturum arama seçenekleri](./media/analysis-services-logging/aas-logging-open-log-search.png)

İçinde veri toplama etkinleştirdikten göre **günlük arama**, tıklatın **tüm toplanan verileri**.

İçinde **türü**, tıklatın **AzureDiagnostics**ve ardından **Uygula**. AzureDiagnostics altyapısı ve hizmet olaylarını içerir. Günlük analizi sorgu üzerinde-çalışma sırasında oluşturulan dikkat edin. EventClass\_s alan günlüğü şirket için Xevent'ler kullandıysanız tanıdık görünebilir xEvent adlarını içerir.

Tıklatın **EventClass\_s** veya bir olay adları ve günlük analizi devam bir sorgu oluşturma. Daha sonra yeniden kullanmak için sorgularınızı kaydettiğinizden emin olun.

Bir Web sitesi Gelişmiş sorgu, dashboarding ve toplanan verileri uyarı verme yetenekleri sağlar günlük analizi görmek emin olun.

### <a name="queries"></a>Sorgular

Kullanabileceğiniz sorgular yüzlerce vardır. Başlamanıza yardımcı olmak için birkaç şunlardır.
Yeni günlük arama sorgu dilini kullanma hakkında daha fazla bilgi için bkz: [anlama günlük arar günlük analizi](../log-analytics/log-analytics-log-search-new.md). 

* Sorgu döndürme sorguları Azure Analysis Services için beş dakika (300.000 milisaniye) tamamlamak için harcanan gönderildi.

    ```
    search * | where ( Type == "AzureDiagnostics" ) | where ( EventClass_s == "QUERY_END" ) | where toint(Duration_s) > 300000
    ```

* Çoğaltmaları genişletme tanımlayın.

    ```
    search * | summarize count() by ServerName_s
    ```
    Genişleme kullanırken, çünkü salt okunur çoğaltmaları tanımlayabilirsiniz ServerName\_s alan değerleri olan adına eklenen çoğaltma örneği sayısı. Kaynak alan kullanıcıların sunucu adı ile eşleşen Azure kaynak adı içerir. IsQueryScaleoutReadonlyInstance_s alan çoğaltmaları true eşittir.



> [!TIP]
> Paylaşmak istediğiniz harika bir günlük analizi sorgu var mı? GitHub hesabı varsa, bu makalede ekleyebilirsiniz. Tıklatmanız **Düzenle** bu sayfanın üst sağdaki.


## <a name="tutorial---turn-on-logging-by-using-powershell"></a>Öğretici - PowerShell kullanarak günlük kaydını etkinleştirin
Hızlı Bu öğreticide, Analysis Services sunucusu olarak bir depolama hesabı aynı abonelikte ve kaynak grubu oluşturun. Günlüğe kaydetme, çıkış yeni depolama hesabı gönderme tanılama etkinleştirmek için Set-AzureRmDiagnosticSetting kullanın.

### <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi tamamlamak için aşağıdaki kaynaklara sahip olmalısınız:

* Var olan bir Azure Analysis Services sunucusu. Bir sunucu kaynağı oluşturma ile ilgili yönergeler için bkz: [Azure portalında bir sunucu oluşturmak](analysis-services-create-server.md), veya [PowerShell kullanarak bir Azure Analysis Services sunucusuna oluşturma](analysis-services-create-powershell.md).

### <a name="aconnect-to-your-subscriptions"></a></a>Aboneliklerinize bağlanma

Bir Azure PowerShell oturumu başlatın ve aşağıdaki komutla Azure hesabınızda oturum açın:  

```powershell
Login-AzureRmAccount
```

Açılır tarayıcı penceresinde Azure hesabı kullanıcı adınızı ve parolanızı girin. Azure PowerShell bu hesapla ilişkili tüm abonelikleri alır ve varsayılan olarak birinciyi kullanır.

Birden çok aboneliğiniz varsa Azure Anahtar Kasanızı oluşturmak için kullanılan belirli bir tanesini belirtmeniz gerekebilir. Hesabınız için abonelikleri görmek üzere aşağıdakini yazın:

```powershell
Get-AzureRmSubscription
```

Ardından, oturum açtığınız Azure Analysis Services hesabıyla ilişkili aboneliği belirtmek için şunu yazın:

```powershell
Set-AzureRmContext -SubscriptionId <subscription ID>
```

> [!NOTE]
> Hesabınızla ilişkili birden çok aboneliğiniz varsa, aboneliği belirtmek önemlidir.
>
>

### <a name="create-a-new-storage-account-for-your-logs"></a>Günlükleriniz için yeni bir depolama hesabı oluşturma

Sunucunuz ile aynı abonelikte olması koşuluyla günlükleriniz için var olan depolama hesabını kullanabilirsiniz. Bu öğretici için yeni bir depolama hesabı Analysis Services günlükleri için ayrılmış da oluşturun. Kolaylaştırmak için depolama hesabı ayrıntıları adlı bir değişkende saklamak **sa**.

Ayrıca, Analysis Services sunucusu içeren bir kaynak grubunu kullanın. Yedek için değerleri `awsales_resgroup`, `awsaleslogs`, ve `West Central US` kendi değerlerinizi ile:

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName awsales_resgroup `
-Name awsaleslogs -Type Standard_LRS -Location 'West Central US'
```

### <a name="identify-the-server-account-for-your-logs"></a>Günlükleriniz için server hesabı belirle

Hesap adı adlı bir değişken ayarlamak **hesap**, burada KaynakAdı hesabının adıdır.

```powershell
$account = Get-AzureRmResource -ResourceGroupName awsales_resgroup `
-ResourceName awsales -ResourceType "Microsoft.AnalysisServices/servers"
```

### <a name="enable-logging"></a>Günlü kaydını etkinleştir

Günlük kaydını etkinleştirmek için yeni depolama hesabı, server hesabı ve kategori için değişkenleri birlikte Set-AzureRmDiagnosticSetting cmdlet'ini kullanın. Ayarlama aşağıdaki komutu çalıştırın **-etkin** bayrağını **$true**:

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

Bu depolama hesabı bilgileri kaydedilirken sunucusu için günlüğü şimdi etkin olduğunu doğrular.

Eski günlükleri otomatik olarak silinir şekilde günlükleriniz için bekletme ilkesi de ayarlayabilirsiniz. Örneğin, bekletme ilkesi kullanılarak ayarlanan **- RetentionEnabled** bayrağını **$true**ve **- RetentionInDays** parametresi **90**. 90 günden daha eski günlükleri otomatik olarak silinir.

```powershell
Set-AzureRmDiagnosticSetting -ResourceId $account.ResourceId`
 -StorageAccountId $sa.Id -Enabled $true -Categories Engine`
  -RetentionEnabled $true -RetentionInDays 90
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek [Azure kaynak tanılama günlük](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).

Bkz: [Set-AzureRmDiagnosticSetting](https://docs.microsoft.com/powershell/module/azurerm.insights/Set-AzureRmDiagnosticSetting) PowerShell Yardımı'nda.