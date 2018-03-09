---
title: "Azure İzleyici PowerShell hızlı başlangıç örnekleri. | Microsoft Docs"
description: "Otomatik ölçeklendirme, uyarılar, Web kancalarını ve etkinlik günlükleri arama gibi Azure İzleyicisi özelliklerine erişmek için PowerShell kullanın."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: c0761814-7148-4ab5-8c27-a2c9fa4cfef5
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/14/2018
ms.author: robb
ms.openlocfilehash: 5a08fd7d20dc78512315ab5d154ba95bd8e8494b
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="azure-monitor-powershell-quick-start-samples"></a>Azure İzleyici PowerShell hızlı başlangıç örnekleri
Bu makale Azure İzleyicisi özelliklerine erişmenize yardımcı olması için PowerShell komutlarını örnek gösterir.

> [!NOTE]
> Azure İzleyicisi "Azure Öngörüler" olarak adlandırılmıştı için yeni 25 Eylül 2016'ya kadar adıdır. Ancak, ad alanları ve bu nedenle aşağıdaki komutları yine "Öngörüler." sözcüğünü içeren
> 
> 

## <a name="set-up-powershell"></a>PowerShell ayarlayın
Henüz yapmadıysanız, bilgisayarınızda çalıştırmak için PowerShell ayarlayın. Daha fazla bilgi için bkz: [yükleme ve yapılandırma PowerShell](/powershell/azure/overview).

## <a name="examples-in-this-article"></a>Bu makaledeki örnekler
Makaledeki örneklerde Azure İzleyici cmdlet'lerini nasıl kullanabileceğinizi göstermektedir. Azure İzleyici PowerShell cmdlet'leri tüm listesini gözden geçirebilirsiniz [Azure İzleyicisi'ni (Öngörüler) cmdlet'leri](https://msdn.microsoft.com/library/azure/mt282452#40v=azure.200#41.aspx).

## <a name="sign-in-and-use-subscriptions"></a>Oturum açma ve abonelikleri kullanın
İlk olarak, Azure aboneliğinizde oturum açın.

```PowerShell
Login-AzureRmAccount
```

Bir oturum açma ekranı görürsünüz. Hesabınızda Tenantıd, bir kez oturum açın ve varsayılan abonelik kimliği görüntülenir. Tüm Azure cmdlet'lerini varsayılan aboneliğinizin bağlamında çalışır. Erişiminiz Aboneliklerin listesini görüntülemek için aşağıdaki komutu kullanın:

```PowerShell
Get-AzureRmSubscription
```

Farklı bir aboneliğe çalışma içeriğiniz değiştirmek için aşağıdaki komutu kullanın:

```PowerShell
Set-AzureRmContext -SubscriptionId <subscriptionid>
```


## <a name="retrieve-activity-log-for-a-subscription"></a>Bir abonelik için etkinlik günlüğü Al
Kullanım `Get-AzureRmLog` cmdlet'i.  Sık karşılaşılan bazı örnekleri aşağıda verilmiştir.

Günlük girişlerini bu zaman/sunmak için başlangıç tarihi alın:

```PowerShell
Get-AzureRmLog -StartTime 2016-03-01T10:30
```

Günlük girdilerini arasında bir saat/tarih aralığı alın:

```PowerShell
Get-AzureRmLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Günlük girişlerini belirli kaynak grubundan alın:

```PowerShell
Get-AzureRmLog -ResourceGroup 'myrg1'
```

Bir saat/tarih aralığı arasında belirli bir kaynak Sağlayıcısı'ndan günlük girişlerini alın:

```PowerShell
Get-AzureRmLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Tüm günlük girişleri belirli çağıran ile alın:

```PowerShell
Get-AzureRmLog -Caller 'myname@company.com'
```

Aşağıdaki komut etkinlik günlüğünden son 1000 olayları alır:

```PowerShell
Get-AzureRmLog -MaxEvents 1000
```

`Get-AzureRmLog` pek çok parametrelerden destekler. Bkz: `Get-AzureRmLog` daha fazla bilgi için başvuru.

> [!NOTE]
> `Get-AzureRmLog` yalnızca 15 gün geçmişi sağlar. Kullanarak **- MaxEvents** parametresi, 15 gün ötesinde son N olayları sorgu olanak verir. Erişim için 15 gün sayısından önceki olayların, REST API veya SDK'sı (SDK'sını kullanarak C# örnek) kullanın. Dahil etmezseniz **StartTime**, varsayılan değer ise **EndTime** bir saat eksi. Dahil etmezseniz **EndTime**, sonra da geçerli saati varsayılan değerdir. Tüm saatler UTC biçimindedir.
> 
> 

## <a name="retrieve-alerts-history"></a>Uyarı geçmişini alma
Tüm uyarı olaylarını görüntülemek için aşağıdaki örnekleri kullanarak Azure Resource Manager günlüklerini sorgulayabilirsiniz.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/alertRules" -DetailedOutput -StartTime 2015-03-01
```

Belirli bir uyarı kuralı geçmişini görüntülemek için kullanabileceğiniz `Get-AzureRmAlertHistory` cmdlet, uyarı kuralı kaynak Kimliğini geçirme.

```PowerShell
Get-AzureRmAlertHistory -ResourceId /subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/myalert -StartTime 2016-03-1 -Status Activated
```

`Get-AzureRmAlertHistory` Cmdlet çeşitli parametreleri destekler. Daha fazla bilgi için bkz: [Get-AlertHistory](https://msdn.microsoft.com/library/mt282453.aspx).

## <a name="retrieve-information-on-alert-rules"></a>Uyarı kuralları hakkında bilgi alma
Aşağıdaki komutların tümü "montest" adlı bir kaynak grubu üzerinde davranır.

Uyarı kuralı tüm özelliklerini görüntüleyin:

```PowerShell
Get-AzureRmAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
```

Bir kaynak grubu üzerinde tüm uyarıları Al:

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest
```

Tüm uyarı kuralı için bir hedef kaynak kümesini alır. Örneğin, bir VM üzerinde tüm uyarı kurallarını ayarlayın.

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig
```

`Get-AzureRmAlertRule` diğer parametreleri destekler. Bkz: [Get-AlertRule](https://msdn.microsoft.com/library/mt282459.aspx) daha fazla bilgi için.

## <a name="create-metric-alerts"></a>Ölçüm uyarı oluşturma
Kullanabileceğiniz `Add-AlertRule` cmdlet'ini oluşturmak, güncelleştirmek ya da bir uyarı kuralı devre dışı bırakın.

Kullanarak e-posta ve Web kancası özellikleri oluşturabilirsiniz `New-AzureRmAlertRuleEmail` ve `New-AzureRmAlertRuleWebhook`sırasıyla. Uyarı kuralı cmdlet eylemleri olarak bu özellikleri atayın **Eylemler** uyarı kuralı özelliği.

Aşağıdaki tabloda, parametreler ve bir ölçüm kullanarak bir uyarı oluşturmak için kullanılan değerleri açıklanmaktadır.

| parametre | değer |
| --- | --- |
| Ad |simpletestdiskwrite |
| Bu uyarı kuralı konumu |Doğu ABD |
| ResourceGroup |montest |
| TargetResourceId |/subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig |
| Oluşturulan uyarının MetricName |\PhysicalDisk (_Total) \Disk Yazma/sn. Bkz: `Get-MetricDefinitions` cmdlet tam ölçüm adları alma hakkında |
| işleci |GreaterThan |
| Eşik değeri (sayısı/sn olarak bu ölçüm için) |1 |
| Pencereboyutu (ss: dd: biçimi) |00:05:00 |
| Toplayıcı (istatistiği ölçüsünün ortalama sayısı, bu durumda kullanır) |Ortalama |
| özel e-postalar (dize dizisi) |'foo@example.com','bar@example.com' |
| sahipleri, Katkıda Bulunanlar ve okuyucular için e-posta Gönder |-SendToServiceOwners |

Bir e-posta eylem oluşturun

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

Bir Web kancası eylem oluşturun

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

Klasik bir VM'de CPU % ölçüm uyarı kuralı oluşturma

```PowerShell
Add-AzureRmMetricAlertRule -Name vmcpu_gt_1 -Location "East US" -ResourceGroup myrg1 -TargetResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.ClassicCompute/virtualMachines/my_vm1 -MetricName "Percentage CPU" -Operator GreaterThan -Threshold 1 -WindowSize 00:05:00 -TimeAggregationOperator Average -Actions $actionEmail, $actionWebhook -Description "alert on CPU > 1%"
```

Uyarı kuralı alma

```PowerShell
Get-AzureRmAlertRule -Name vmcpu_gt_1 -ResourceGroup myrg1 -DetailedOutput
```

Belirli özellikler için bir uyarı kuralı zaten varsa Ekle uyarı cmdlet de kuralı güncelleştirir. Bir uyarı kuralı devre dışı bırakmak için parametresi dahil **- DisableRule**.

## <a name="get-a-list-of-available-metrics-for-alerts"></a>Uyarılar için kullanılabilir ölçümler listesini al
Kullanabileceğiniz `Get-AzureRmMetricDefinition` belirli bir kaynak için tüm ölçümleri listesini görmek için cmdlet.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id>
```

Aşağıdaki örnek, ölçüm adı içeren bir tablo ve onun için birim oluşturur.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Tam listesi için kullanılabilir seçenekleri `Get-AzureRmMetricDefinition` şu adresten edinilebilir [Get-MetricDefinitions](https://msdn.microsoft.com/library/mt282458.aspx).

## <a name="create-and-manage-activity-log-alerts"></a>Etkinlik günlüğü uyarıları oluşturma ve yönetme
Kullanabileceğiniz `Set-AzureRmActivityLogAlert` bir etkinlik günlüğü uyarı ayarlamak için cmdlet. Bir etkinlik günlüğü uyarı, ilk koşullarınızı koşullar bir sözlük olarak tanımlamak ve sonra bu koşullara kullanan bir uyarı oluşturmak, gerektirir.

```PowerShell

$condition1 = New-AzureRmActivityLogAlertCondition -Field 'category' -Equals 'Administrative'
$condition2 = New-AzureRmActivityLogAlertCondition -Field 'operationName' -Equals 'Microsoft.Compute/virtualMachines/write'
$additionalWebhookProperties = New-Object "System.Collections.Generic.Dictionary``2[System.String,System.String]"
$additionalWebhookProperties.Add('customProperty', 'someValue')
$actionGrp1 = New-AzureRmActionGroup -ActionGroupId 'actiongr1' -WebhookProperties $dict
Set-AzureRmActivityLogAlert -Location 'Global' -Name 'alert on VM create' -ResourceGroupName 'myResourceGroup' -Scope '/' -Action $actionGrp1 -Condition $condition1, $condition2

```

Ek Web kancası özellikleri isteğe bağlıdır. Bir etkinlik günlüğü uyarı kullanarak içindekiler geri alabilirsiniz `Get-AzureRmActivityLogAlert`.

## <a name="create-and-manage-autoscale-settings"></a>Otomatik ölçeklendirme ayarlarını oluşturun ve yönetin
Bir kaynak (bir Web uygulaması, VM, bulut hizmeti veya sanal makine ölçek kümesi) yalnızca bir otomatik ölçeklendirme ayarı için yapılandırılmış olabilir.
Bununla birlikte, her otomatik ölçeklendirme ayarında birden çok profil olabilir. Örneğin, bir performans tabanlı ölçek profili ve ikinci bir zamanlama tabanlı profil için. Her profil yapılandırılmış birden çok kural bulunabilir. Otomatik ölçeklendirme hakkında daha fazla bilgi için bkz: [otomatik ölçeklendirme bir uygulamanın nasıl](../cloud-services/cloud-services-how-to-scale-portal.md).

Kullanmak için adımlar şunlardır:

1. Kuralları oluşturun.
2. Profillere daha önce oluşturduğunuz kurallar eşleme profillerini oluşturun.
3. İsteğe bağlı: Web kancası ve e-posta özellikleri yapılandırarak otomatik ölçeklendirme bildirimleri oluşturun.
4. Profilleri ve önceki adımlarda oluşturduğunuz bildirimler eşleyerek hedef kaynak üzerindeki bir ada sahip bir otomatik ölçeklendirme ayarı oluşturun.

Aşağıdaki örnekler, bir sanal makine ölçek kümesi CPU kullanımı ölçümü kullanarak temel Windows işletim sistemi için otomatik ölçeklendirme ayarının nasıl oluşturabileceğinizi gösterir.

İlk olarak, örnek sayısı artışı ile genişletilecek bir kural oluşturun.

```PowerShell
$rule1 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 60 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Increase -ScaleActionValue 1
```        

Ardından, bir örnek sayısı azaltma ile ölçeklendirmek için bir kural oluşturun.

```PowerShell
$rule2 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 30 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Decrease -ScaleActionValue 1
```

Ardından, kural için bir profil oluşturun.

```PowerShell
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "My_Profile"
```

Bir Web kancası özellik oluşturun.

```PowerShell
$webhook_scale = New-AzureRmAutoscaleWebhook -ServiceUri "https://example.com?mytoken=mytokenvalue"
```

Bildirim özelliği otomatik ölçeklendirme ayarının, e-posta ve daha önce oluşturduğunuz Web kancası için oluşturun.

```PowerShell
$notification1= New-AzureRmAutoscaleNotification -CustomEmails ashwink@microsoft.com -SendEmailToSubscriptionAdministrators SendEmailToSubscriptionCoAdministrators -Webhooks $webhook_scale
```

Son olarak, daha önce oluşturduğunuz profili eklemek için otomatik ölçeklendirme ayarı oluşturun. 

```PowerShell
Add-AzureRmAutoscaleSetting -Location "East US" -Name "MyScaleVMSSSetting" -ResourceGroup big2 -TargetResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -AutoscaleProfiles $profile1 -Notifications $notification1
```

Otomatik ölçeklendirme ayarlarını yönetme hakkında daha fazla bilgi için bkz: [Get-AutoscaleSetting](https://msdn.microsoft.com/library/mt282461.aspx).

## <a name="autoscale-history"></a>Otomatik ölçeklendirme geçmişi
Aşağıdaki örnek, son otomatik ölçeklendirme ve uyarı olayları nasıl görüntüleyebileceğiniz gösterir. Otomatik ölçeklendirme geçmişini görüntülemek için etkinlik günlüğü aramayı kullanın.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/autoscaleSettings" -DetailedOutput -StartTime 2015-03-01
```

Kullanabileceğiniz `Get-AzureRmAutoScaleHistory` otomatik ölçeklendirme geçmişi almak üzere.

```PowerShell
Get-AzureRmAutoScaleHistory -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/microsoft.insights/autoscalesettings/myScaleSetting -StartTime 2016-03-15 -DetailedOutput
```

Daha fazla bilgi için bkz: [Get-AutoscaleHistory](https://msdn.microsoft.com/library/mt282464.aspx).

### <a name="view-details-for-an-autoscale-setting"></a>Otomatik ölçeklendirme ayarı için ayrıntıları görüntüle
Kullanabileceğiniz `Get-Autoscalesetting` otomatik ölçeklendirme ayarı hakkında daha fazla bilgi almak üzere.

Aşağıdaki örnekte kaynak grubu 'myrg1' tüm otomatik ölçeklendirme ayarları hakkında ayrıntıları gösterir.

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -DetailedOutput
```

Aşağıdaki örnekte kaynak grubu 'myrg1' tüm otomatik ölçeklendirme ayarlarını ve özellikle 'MyScaleVMSSSetting' adlı otomatik ölçeklendirme ayarı hakkında ayrıntıları gösterir.

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting -DetailedOutput
```

### <a name="remove-an-autoscale-setting"></a>Otomatik ölçeklendirme ayarı kaldırın
Kullanabileceğiniz `Remove-Autoscalesetting` cmdlet'ini bir otomatik ölçeklendirme ayarı silin.

```PowerShell
Remove-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting
```

## <a name="manage-log-profiles-for-activity-log"></a>Etkinlik günlüğü için günlük profilleri Yönet
Oluşturabileceğiniz bir *oturum profili* ve etkinlik günlüğü verileri dışa bir depolama hesabı ve veri saklama için yapılandırabilirsiniz. İsteğe bağlı olarak, olay Hub'ınıza ayrıca veri akışını sağlayabilirsiniz. Bu özellik şu anda önizlemede değil ve yalnızca abonelik başına bir günlük profili oluşturabilirsiniz. Günlük profilleri oluşturmak ve yönetmek için geçerli aboneliğiniz ile aşağıdaki cmdlet'leri kullanın. Belirli bir abonelik de seçebilirsiniz. Geçerli abonelik PowerShell Varsayılanları olsalar bile, her zaman bu kullanarak değiştirebilirsiniz `Set-AzureRmContext`. Etkinlik günlüğü herhangi bir depolama hesabı veya olay hub'ın bu abonelik içindeki rota verileri için yapılandırabilirsiniz. Veriler JSON biçiminde blob dosyaları olarak yazılır.

### <a name="get-a-log-profile"></a>Günlük profilini Al
Var olan günlük profillerinizi getirmek için kullanmak `Get-AzureRmLogProfile` cmdlet'i.

### <a name="add-a-log-profile-without-data-retention"></a>Veri saklama olmayan günlük profili Ekle
```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Günlük profilini Kaldır
```PowerShell
Remove-AzureRmLogProfile -name my_log_profile_s1
```

### <a name="add-a-log-profile-with-data-retention"></a>Veri saklama günlük profiliyle ekleme
Belirleyebileceğiniz **- RetentionInDays** özelliği ile verileri nerede tutulur, pozitif bir tamsayı olarak gün sayısı.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

### <a name="add-log-profile-with-retention-and-eventhub"></a>Günlük profiliyle saklama ve EventHub ekleyin
Depolama hesabına verilerinizi yönlendirmenin yanı sıra, ayrıca, bir olay Hub'ına akışını sağlayabilirsiniz. Bu önizleme sürümünde depolama hesabı yapılandırması zorunludur ancak olay hub'ı yapılandırma isteğe bağlıdır.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

## <a name="configure-diagnostics-logs"></a>Tanılama günlüklerini yapılandırma
Çoğu Azure hizmeti ek günlükleri ve aşağıdakilerden birini veya birkaçını yapabilirsiniz telemetri sağlar: 
 - Azure depolama hesabınızdaki verileri kaydetmek için yapılandırılması
 - Event Hubs'a gönderilen
 - bir OMS günlük analizi çalışma alanına gönderilir. 

İşlemi yalnızca bir kaynak düzeyinde gerçekleştirilebilir. Depolama hesabı veya olay hub'ı tanılama ayarını yapılandırıldığı aynı bölgede hedef kaynak olarak mevcut olmalıdır.

### <a name="get-diagnostic-setting"></a>Tanılama ayarını alma
```PowerShell
Get-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp
```

Tanılama ayarını devre dışı bırak

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $false
```

Bekletme olmadan tanılama ayarını etkinleştirin

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true
```

Bekletme olan tanılama ayarını etkinleştirin

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

Bekletme belirli günlük kategori için tanılama ayarıyla etkinleştir

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/sakteststorage -Categories NetworkSecurityGroupEvent -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

Olay hub'ları için tanılama ayarını etkinleştirin

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Enable $true
```

Tanılama ayarını için günlük analizi (OMS) etkinleştir

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -WorkspaceId /subscriptions/s1/resourceGroups/insights-integration/providers/providers/microsoft.operationalinsights/workspaces/myWorkspace -Enabled $true

```

Workspaceıd özelliği alır Not *kaynak kimliği* çalışma alanının. Aşağıdaki komutu kullanarak, günlük analizi çalışma alanı kaynak Kimliğini elde edebilirsiniz:

```PowerShell
(Get-AzureRmOperationalInsightsWorkspace).ResourceId

```

Bu komutlar, birden çok hedefe veri göndermek için birleştirilebilir.
