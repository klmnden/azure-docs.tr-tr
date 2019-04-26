---
title: Azure İzleyici PowerShell hızlı başlangıç örnekleri
description: Otomatik ölçeklendirme, uyarılar, Web kancaları ve etkinlik günlüklerini arama gibi Azure İzleyici özelliklerine erişmek için PowerShell'i kullanın.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 2/14/2018
ms.author: robb
ms.subservice: ''
ms.openlocfilehash: 59cb14c86963d956b0bd63f65b10776dff4aa97f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60452730"
---
# <a name="azure-monitor-powershell-quick-start-samples"></a>Azure İzleyici PowerShell hızlı başlangıç örnekleri
Bu makale, Azure İzleyici özellikleri erişmenize yardımcı olması için PowerShell komutlarını örnek gösterir.

> [!NOTE]
> Azure İzleyici "Azure Insights" olarak adlandırılıyordu yeni 25 Eylül 2016'ya kadar adıdır. Ancak, ad alanları ve bu nedenle aşağıdaki komutları hala "ınsights." sözcüğü içerir

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="set-up-powershell"></a>PowerShell'i ayarlayın
Henüz yapmadıysanız, bilgisayarınızda çalıştırmak için PowerShell'i ayarlayın. Daha fazla bilgi için [yükleme ve yapılandırma PowerShell](/powershell/azure/overview).

## <a name="examples-in-this-article"></a>Bu makaledeki örnekleri
Makaledeki örnekler, Azure İzleyici cmdlet'lerini nasıl kullanabileceğinizi gösterir. Ayrıca Azure İzleyici PowerShell cmdlet'leri listesini gözden geçirebilirsiniz [Azure İzleyici (Öngörüler) cmdlet'leri](https://docs.microsoft.com/powershell/module/az.applicationinsights).

## <a name="sign-in-and-use-subscriptions"></a>Oturum açın ve aboneliklerini kullanma
İlk olarak, Azure aboneliğinizde oturum açın.

```powershell
Connect-AzAccount
```

Oturum açma ekranı görürsünüz. Hesabınızdaki Tenantıd, bir kez oturum açmak ve varsayılan abonelik kimliği görüntülenir. Tüm Azure cmdlet'lerini varsayılan aboneliğinizi bağlamında çalışır. Erişiminiz Aboneliklerin listesini görüntülemek için aşağıdaki komutu kullanın:

```powershell
Get-AzSubscription
```

Çalışma Bağlamınızı farklı bir aboneliğe değiştirmek için aşağıdaki komutu kullanın:

```powershell
Set-AzContext -SubscriptionId <subscriptionid>
```


## <a name="retrieve-activity-log-for-a-subscription"></a>Etkinlik günlüğü bir abonelik için Al
Kullanım `Get-AzLog` cmdlet'i.  Bazı genel örnekleri aşağıda verilmiştir.

Bu zaman/sunmak için tarihten günlük girişlerini alın:

```powershell
Get-AzLog -StartTime 2016-03-01T10:30
```

Günlük girişlerini arasında bir saat/tarih aralığı alın:

```powershell
Get-AzLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Günlük girişlerini belirli bir kaynak grubundan alın:

```powershell
Get-AzLog -ResourceGroup 'myrg1'
```

Bir saat/tarih aralığı arasında belirli kaynak sağlayıcısından günlük girişlerini alın:

```powershell
Get-AzLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Belirli bir çağıranın ile tüm günlük girişlerini alın:

```powershell
Get-AzLog -Caller 'myname@company.com'
```

Aşağıdaki komut, etkinlik günlüğünde son 1000 olayları alır:

```powershell
Get-AzLog -MaxEvents 1000
```

`Get-AzLog` diğer birçok parametrelerini destekler. Bkz: `Get-AzLog` daha fazla bilgi için başvuru.

> [!NOTE]
> `Get-AzLog` yalnızca 15 günlük geçmişi sağlar. Kullanarak **- MaxEvents** parametresi, 15 gün dışında bir son N olayları, sorgulamaya olanak sağlar. 15 günden eski erişim olayları için REST API veya SDK'sını (SDK'sını kullanarak C# örneği) kullanın. Dahil etmezseniz **StartTime**, varsayılan değer ise **EndTime** eksi bir saat. Dahil etmezseniz **EndTime**, geçerli zamanı varsayılan değeridir. Tüm saatler UTC biçimindedir.
> 
> 

## <a name="retrieve-alerts-history"></a>Uyarı geçmişini alma
Tüm Uyarı olayları görüntülemek için aşağıdaki örnekleri kullanarak Azure Resource Manager günlükleri sorgulayabilir.

```powershell
Get-AzLog -Caller "Microsoft.Insights/alertRules" -DetailedOutput -StartTime 2015-03-01
```

Belirli bir uyarı kuralı geçmişini görüntülemek için kullanabileceğiniz `Get-AzAlertHistory` cmdlet'i, uyarı kuralı kaynak Kimliğinde geçirme.

```powershell
Get-AzAlertHistory -ResourceId /subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/myalert -StartTime 2016-03-1 -Status Activated
```

`Get-AzAlertHistory` Cmdlet'i çeşitli parametreleri destekler. Daha fazla bilgi için bkz: [Get-AlertHistory](https://msdn.microsoft.com/library/mt282453.aspx).

## <a name="retrieve-information-on-alert-rules"></a>Uyarı kuralları hakkında bilgi alma
Aşağıdaki komutların tümü "montest" adlı bir kaynak grubu üzerinde işlevi görür.

Uyarı kuralı tüm özelliklerini görüntüleyin:

```powershell
Get-AzAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
```

Bir kaynak grubundaki tüm uyarıları Al:

```powershell
Get-AzAlertRule -ResourceGroup montest
```

Tüm uyarı kuralı için bir hedef kaynak kümesini alır. Örneğin, bir VM'de tüm uyarı kuralları ayarlayın.

```powershell
Get-AzAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig
```

`Get-AzAlertRule` diğer parametreleri destekler. Bkz: [Get-AlertRule](https://msdn.microsoft.com/library/mt282459.aspx) daha fazla bilgi için.

## <a name="create-metric-alerts"></a>Ölçüm uyarıları oluşturma
Kullanabileceğiniz `Add-AlertRule` cmdlet'ini oluşturmak, güncelleştirmek veya bir uyarı kuralı devre dışı bırakın.

E-posta ve Web kancası özellikleri kullanarak oluşturabileceğiniz `New-AzAlertRuleEmail` ve `New-AzAlertRuleWebhook`sırasıyla. Uyarı kuralı cmdlet'te, bu özellikler için eylem olarak atayın. **eylemleri** uyarı kuralı bir özelliğidir.

Aşağıdaki tabloda, bir ölçüm kullanarak bir uyarı oluşturmak için kullanılan değerleri ve parametreler açıklanmaktadır.

| parametre | value |
| --- | --- |
| Ad |simpletestdiskwrite |
| Bu uyarı kuralının konumu |Doğu ABD |
| ResourceGroup |montest |
| TargetResourceId |/subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig |
| Oluşturulan uyarının MetricName |\PhysicalDisk (_Total) \Disk Yazma/sn. Bkz: `Get-MetricDefinitions` cmdlet'i tam ölçüm adları almak nasıl hakkında |
| İşleci |GreaterThan |
| Eşik değerini (sayısı/sn, bu ölçümün) |1 |
| Pencereboyutu (ss: dd: biçimi) |00:05:00 |
| Toplayıcı (istatistiği ölçümün ortalama sayısı, bu durumda kullanır) |Ortalama |
| özel e-postaları (dize dizisi) |'foo@example.com','bar@example.com' |
| sahipleri, Katkıda Bulunanlar ve okuyucular için e-posta Gönder |-SendToServiceOwners |

Bir e-posta eylem oluşturun

```powershell
$actionEmail = New-AzAlertRuleEmail -CustomEmail myname@company.com
```

Bir Web kancası Eylem oluştur

```powershell
$actionWebhook = New-AzAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

Klasik bir sanal makine üzerindeki CPU % ölçüm uyarı kuralı oluşturun

```powershell
Add-AzMetricAlertRule -Name vmcpu_gt_1 -Location "East US" -ResourceGroup myrg1 -TargetResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.ClassicCompute/virtualMachines/my_vm1 -MetricName "Percentage CPU" -Operator GreaterThan -Threshold 1 -WindowSize 00:05:00 -TimeAggregationOperator Average -Action $actionEmail, $actionWebhook -Description "alert on CPU > 1%"
```

Uyarı kuralı alma

```powershell
Get-AzAlertRule -Name vmcpu_gt_1 -ResourceGroup myrg1 -DetailedOutput
```

Belirli özellikleri için bir uyarı kuralı zaten varsa Ekle uyarı cmdlet de kuralı güncelleştirir. Bir uyarı kuralı devre dışı bırakmak için parametresini içerecek **- DisableRule**.

## <a name="get-a-list-of-available-metrics-for-alerts"></a>Uyarılar için kullanılabilir ölçümlerin bir listesini alın
Kullanabileceğiniz `Get-AzMetricDefinition` belirli bir kaynak için tüm ölçümleri listesini görüntülemek için cmdlet'i.

```powershell
Get-AzMetricDefinition -ResourceId <resource_id>
```

Aşağıdaki örnek, ölçüm adı içeren bir tablo ve onun için birim oluşturur.

```powershell
Get-AzMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Tam listesi için kullanılabilir seçenekleri `Get-AzMetricDefinition` kullanılabilir [Get-MetricDefinitions](https://msdn.microsoft.com/library/mt282458.aspx).

## <a name="create-and-manage-activity-log-alerts"></a>Etkinlik günlüğü uyarıları oluşturma ve yönetme
Kullanabileceğiniz `Set-AzActivityLogAlert` cmdlet'i bir etkinlik günlüğü uyarısı ayarlamak için. Etkinlik günlüğü Uyarısı, önce koşullarınızı koşulları bir sözlük olarak tanımlayın, ardından bu koşullara kullanan bir uyarı oluşturmak, gerektirir.

```powershell

$condition1 = New-AzActivityLogAlertCondition -Field 'category' -Equal 'Administrative'
$condition2 = New-AzActivityLogAlertCondition -Field 'operationName' -Equal 'Microsoft.Compute/virtualMachines/write'
$additionalWebhookProperties = New-Object "System.Collections.Generic.Dictionary``2[System.String,System.String]"
$additionalWebhookProperties.Add('customProperty', 'someValue')
$actionGrp1 = New-AzActionGroup -ActionGroupId '/subscriptions/<subid>/providers/Microsoft.Insights/actiongr1' -WebhookProperty $additionalWebhookProperties
Set-AzActivityLogAlert -Location 'Global' -Name 'alert on VM create' -ResourceGroupName 'myResourceGroup' -Scope '/subscriptions/<subid>' -Action $actionGrp1 -Condition $condition1, $condition2

```

Ek Web kancası özellikleri isteğe bağlıdır. Bir etkinlik günlüğü uyarısı kullanarak içerikleri geri dönebilirsiniz `Get-AzActivityLogAlert`.

## <a name="create-and-manage-autoscale-settings"></a>Oluşturma ve otomatik ölçeklendirme ayarlarını yönetme
Bir kaynak (bir Web uygulaması, VM, bulut hizmeti veya sanal makine ölçek kümesi), yalnızca bir otomatik ölçeklendirme ayarı için yapılandırılmış olabilir.
Ancak, her otomatik ölçeklendirme ayarı, birden çok profil olabilir. Örneğin, bir performans tabanlı ölçek profili ve ikinci bir zamanlama tabanlı profil için. Her profil yapılandırılmış birden çok kural bulunabilir. Otomatik ölçeklendirme hakkında daha fazla bilgi için bkz: [otomatik ölçeklendirme bir uygulamayı nasıl](../../cloud-services/cloud-services-how-to-scale-portal.md).

Kullanılacak adımlar şunlardır:

1. Kuralları oluşturun.
2. Oluşturduğunuz kurallar, daha önce profillere eşleme profillerini oluşturun.
3. İsteğe bağlı: Otomatik ölçeklendirme bildirimleri için Web kancası ve e-posta özelliklerini yapılandırarak oluşturun.
4. Önceki adımlarda oluşturulan bildirimleri ve profilleri eşleyerek hedef kaynak üzerinde bir ada sahip bir otomatik ölçeklendirme ayarı oluşturun.

Aşağıdaki örnekler, bir sanal makine ölçek kümesi için CPU kullanımı ölçümü kullanarak temel bir Windows işletim sistemi için bir otomatik ölçeklendirme ayarı nasıl oluşturabileceğinizi gösterir.

İlk olarak, örnek sayısı artışı ile ölçek genişletme, bir kural oluşturun.

```powershell
$rule1 = New-AzAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 60 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Increase -ScaleActionValue 1
```        

Ardından, bir örnek sayısını azaltma ile azaltmak için bir kural oluşturun.

```powershell
$rule2 = New-AzAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 30 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Decrease -ScaleActionValue 1
```

Ardından, kural için bir profil oluşturun.

```powershell
$profile1 = New-AzAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "My_Profile"
```

Bir Web kancası özellik oluşturun.

```powershell
$webhook_scale = New-AzAutoscaleWebhook -ServiceUri "https://example.com?mytoken=mytokenvalue"
```

E-posta ve daha önce oluşturduğunuz Web kancası dahil olmak üzere otomatik ölçeklendirme ayarı için bildirim özelliği oluşturun.

```powershell
$notification1= New-AzAutoscaleNotification -CustomEmails ashwink@microsoft.com -SendEmailToSubscriptionAdministrators SendEmailToSubscriptionCoAdministrators -Webhooks $webhook_scale
```

Son olarak, daha önce oluşturduğunuz profili eklemek için otomatik ölçeklendirme ayarı oluşturun. 

```powershell
Add-AzAutoscaleSetting -Location "East US" -Name "MyScaleVMSSSetting" -ResourceGroup big2 -TargetResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -AutoscaleProfiles $profile1 -Notifications $notification1
```

Otomatik ölçeklendirme ayarlarını yönetme hakkında daha fazla bilgi için bkz. [Get-AutoscaleSetting](https://msdn.microsoft.com/library/mt282461.aspx).

## <a name="autoscale-history"></a>Otomatik ölçeklendirme geçmişi
Aşağıdaki örnek nasıl yeni bir otomatik ölçeklendirme ve uyarı olayları görüntüleyebileceğiniz gösterir. Otomatik ölçeklendirme geçmişini görüntülemek için etkinliği günlük araması'nı kullanın.

```powershell
Get-AzLog -Caller "Microsoft.Insights/autoscaleSettings" -DetailedOutput -StartTime 2015-03-01
```

Kullanabileceğiniz `Get-AzAutoScaleHistory` cmdlet'i otomatik ölçeklendirme geçmişi alınamadı.

```powershell
Get-AzAutoScaleHistory -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/microsoft.insights/autoscalesettings/myScaleSetting -StartTime 2016-03-15 -DetailedOutput
```

Daha fazla bilgi için [Get-AutoscaleHistory](https://msdn.microsoft.com/library/mt282464.aspx).

### <a name="view-details-for-an-autoscale-setting"></a>Bir otomatik ölçeklendirme ayarı için ayrıntıları görüntüle
Kullanabileceğiniz `Get-Autoscalesetting` otomatik ölçeklendirme ayarı hakkında daha fazla bilgi almak için cmdlet'i.

Aşağıdaki örnek, kaynak grubu 'myrg1' tüm otomatik ölçeklendirme ayarları hakkında ayrıntılı bilgi gösterir.

```powershell
Get-AzAutoscalesetting -ResourceGroup myrg1 -DetailedOutput
```

Aşağıdaki örnek, tüm otomatik ölçeklendirme ayarlarını 'myrg1' kaynak grubu ve özellikle 'MyScaleVMSSSetting' adlı otomatik ölçeklendirme ayarı hakkındaki ayrıntıları gösterir.

```powershell
Get-AzAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting -DetailedOutput
```

### <a name="remove-an-autoscale-setting"></a>Otomatik ölçeklendirme ayarını kaldırın
Kullanabileceğiniz `Remove-Autoscalesetting` bir otomatik ölçeklendirme ayarı silmeye yönelik cmdlet'i.

```powershell
Remove-AzAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting
```

## <a name="manage-log-profiles-for-activity-log"></a>Etkinlik günlüğü için günlük profillerini yönetme
Oluşturabileceğiniz bir *günlük profili* ve dışarı aktarma, etkinlik günlüğü verileri bir depolama hesabı ve veri saklama için yapılandırabilirsiniz. İsteğe bağlı olarak olay Hub'ınıza da veri akışını yapabilirsiniz. Bu özellik şu anda Önizleme aşamasındadır ve yalnızca abonelik başına bir günlük profilini oluşturabilirsiniz. Günlük profilleri oluşturup yönetmek için geçerli aboneliğiniz ile aşağıdaki cmdlet'leri kullanın. Ayrıca, belirli bir abonelik seçebilirsiniz. PowerShell varsayılan olarak mevcut aboneliğe olsa da, her zaman bu kullanarak değiştirebileceğiniz `Set-AzContext`. Etkinlik günlüğü bu Abonelikteki bir depolama hesabına veya olay hub'ı için rota verileri için yapılandırabilirsiniz. Veri, JSON biçimindeki blob dosyaları olarak yazılır.

### <a name="get-a-log-profile"></a>Günlük profilini Al
Var olan günlük profillerinizi getirilecek kullanın `Get-AzLogProfile` cmdlet'i.

### <a name="add-a-log-profile-without-data-retention"></a>Veri saklama olmayan bir günlük profili Ekle
```powershell
Add-AzLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Günlük profilini Kaldır
```powershell
Remove-AzLogProfile -name my_log_profile_s1
```

### <a name="add-a-log-profile-with-data-retention"></a>Veri saklama ile bir günlük profili Ekle
Belirtebileceğiniz **- Retentionındays** özelliği ile veri nerede tutulur, pozitif bir tamsayı olarak gün sayısı.

```powershell
Add-AzLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

### <a name="add-log-profile-with-retention-and-eventhub"></a>Elde tutma ve EventHub ile günlük profili Ekle
Veri depolama hesabına yönlendirme yanı sıra, ayrıca, bir olay Hub'ına akışını yapabilirsiniz. Bu önizleme sürümünde depolama hesabı yapılandırması zorunludur, ancak olay hub'ı yapılandırma isteğe bağlıdır.

```powershell
Add-AzLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

## <a name="configure-diagnostics-logs"></a>Tanılama günlüklerini Yapılandır
Çoğu Azure hizmeti, ek günlükleri ve aşağıdakilerden birini veya daha fazlasını yapabilirsiniz telemetri sağlar: 
 - Azure depolama hesabınızdaki verileri kaydetmek için yapılandırılması
 - Event Hubs için gönderildi
 - Log Analytics çalışma alanına gönderilir. 

İşlemi yalnızca bir kaynak düzeyinde gerçekleştirilebilir. Depolama hesabına veya olay hub'ı tanılama ayarı yapılandırıldığı aynı bölgede hedef kaynak olarak mevcut olmalıdır.

### <a name="get-diagnostic-setting"></a>Tanılama ayarını Al
```powershell
Get-AzDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp
```

Tanılama ayarını devre dışı bırak

```powershell
Set-AzDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $false
```

Tanılama ayarı bekletme olmadan etkinleştirin

```powershell
Set-AzDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true
```

Tanılama ayarı saklama süresi etkinleştirin

```powershell
Set-AzDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

Tanılama ayarı olan belirli günlük kategorisi için bekletme etkinleştirin

```powershell
Set-AzDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/sakteststorage -Categories NetworkSecurityGroupEvent -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

Event Hubs için tanılama ayarını etkinleştirin

```powershell
Set-AzDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Enable $true
```

Log Analytics için tanılama ayarını etkinleştirin

```powershell
Set-AzDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -WorkspaceId /subscriptions/s1/resourceGroups/insights-integration/providers/providers/microsoft.operationalinsights/workspaces/myWorkspace -Enabled $true

```

Çalışma alanı kimliği özelliği Not *kaynak kimliği* çalışma alanının. Aşağıdaki komutu kullanarak Log Analytics çalışma alanınızın kaynak kimliği elde edebilirsiniz:

```powershell
(Get-AzOperationalInsightsWorkspace).ResourceId

```

Bu komutlar, birden çok hedefe veri göndermek için birleştirilebilir.

