---
title: Uyarılar için Azure günlük Analytcs genişletme
description: Bu makalede olarak, uyarıları günlük Analytics'ten Azure uyarıları genişletebilirsiniz API ve araçları açıklanmaktadır.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/04/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 21ba95a7b3efff177afe63d22da3f6ba9848ded2
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35301040"
---
# <a name="extend-alerts-from-log-analytics-into-azure-alerts"></a>Uyarıları günlük Analytics'ten Azure Uyarıları ' genişletme
Azure günlük analizi uyarılar özelliğinde Azure uyarıları ile değiştirilir. Bu geçişin bir parçası olarak Azure günlük analizi başlangıçta yapılandırılmış uyarıları genişletilir. Bunları otomatik olarak Azure taşınmasını beklemek istemiyorsanız, işlem başlatabilirsiniz:

- El ile Operations Management Suite portalından. 
- Program aracılığıyla AlertsVersion API'sini kullanarak.  

> [!NOTE]
> 14 Mayıs 2018 üzerinde tamamlanana kadar yinelenen serisinde başlangıç Microsoft otomatik olarak Azure uyarılar için günlük analizi oluşturulan uyarıların genişletir. Microsoft zamanlamaları, uyarıları Azure, bu geçiş sırasında geçirmek için hem Operations Management Suite portalına hem de Azure portalında uyarılar yönetilebilir. Bu işlem, yıkıcı veya interruptive değil.  

## <a name="option-1-initiate-from-the-operations-management-suite-portal"></a>Seçenek 1: Operations Management Suite portalından Başlat
Aşağıdaki adımlar nasıl çalışma alanı için uyarıları Operations Management Suite Portalı'ndan genişletileceğini açıklar.  

1. Azure portalında seçin **tüm hizmetleri**. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**’i seçin.
2. Günlük analizi abonelikleri bölmesinde, bir çalışma alanını seçin ve ardından **OMS portalı** döşeme.
![Günlük analizi ekran abonelik bölmesiyle, vurgulanmış OMS portalı döşeme](./media/monitor-alerts-extend/azure-portal-01.png) 
3. Operations Management Suite portalına yönlendirilirsiniz sonra seçin **ayarları** simgesi.
![Operations Management Suite ekran portalıyla vurgulanmış ayarları simgesi](./media/monitor-alerts-extend/oms-portal-settings-option.png) 
4. Gelen **ayarları** sayfasında, **uyarıları**.  
5. Seçin **Azure'da genişletmek**.
![Operations Management Suite ekran portal uyarı ayarları sayfası, vurgulanmış Azure içine Genişlet](./media/monitor-alerts-extend/ExtendInto.png)
6. Üç adımlık Sihirbazı görünür **uyarıları** bölmesi. Genel bakış bilgileri okuyun ve seçin **sonraki**.
![1. adım Sihirbazı'nın ekran görüntüsü](./media/monitor-alerts-extend/ExtendStep1.png)  
7. İkinci adımda, önerilen değişikliklerin özetini uygun listeleme gördüğünüz [Eylem grupları](monitoring-action-groups.md) uyarılar için. Benzer eylemler arasında birden fazla uyarı görülüyorsa tek eylem grubu bunların tümünün ile ilişkilendirmek sihirbazın önerir.  Adlandırma kuralı aşağıdaki gibidir: *WorkspaceName_AG_ #Number*. Devam etmek için seçmeniz **sonraki**.
![2. adım Sihirbazı'nın ekran görüntüsü](./media/monitor-alerts-extend/ExtendStep2.png)  
8. Sihirbazın son adımda seçin **son**ve işlemini başlatmak için istendiğinde onaylayın. İsteğe bağlı olarak, böylece işlemi tamamlandıktan ve tüm uyarıları Azure uyarıları başarıyla taşındığını bildirilir bir e-posta adresi sağlayabilir.
![Adım 3 Sihirbazı'nın ekran görüntüsü](./media/monitor-alerts-extend/ExtendStep3.png)

Sihirbaz olduğunda üzerinde tamamlanmış **uyarı ayarlarını** sayfa, Azure için uyarıları uzatma seçeneği kaldırılır. Arka planda uyarılarınızı Azure'da taşınır ve bu biraz zaman alabilir. İşlem sırasında uyarıları Operations Management Suite Portalı'ndan değişiklik yapamazsınız. Portal üstündeki başlığından geçerli durumunu görebilirsiniz. Bir e-posta adresi daha önce sağlanan, işlemi başarıyla tamamlandığında bir e-posta alırsınız.  


Uyarıları bile Azure'da başarıyla taşındıktan sonra Operations Management Suite Portalı'nda listelenmesi için devam edin.
![Operations Management Suite ekran portal uyarı ayarları sayfası](./media/monitor-alerts-extend/PostExtendList.png)


## <a name="option-2-use-the-alertsversion-api"></a>Seçenek 2: AlertsVersion API kullanın
Günlük analizi AlertsVersion API uyarıları günlük Analytics'ten Azure Uyarıları ' bir REST API'si çağırabilirsiniz herhangi bir istemciden genişletmek için kullanabilirsiniz. API Powershell'den kullanarak erişebileceğiniz [ARMClient](https://github.com/projectkudu/ARMClient), açık kaynaklı komut satırı aracı. JSON sonuçları çıkarabilirsiniz.  

API kullanmak için önce bir GET isteği oluşturun. Bu hesaplar ve gerçekte bir POST isteği kullanarak Azure'da genişletme girişiminde bulunmadan önce önerilen değişikliklerin özetini döndürür. Sonuçlar, uyarılar ve önerilen listesini listesinde [Eylem grupları](monitoring-action-groups.md), JSON biçiminde. Benzer eylemler arasında birden fazla uyarı görülüyorsa, bunların tümünü tek eylem grubuyla ilişkilendirilecek hizmet önerir. Adlandırma kuralı aşağıdaki gibidir: *WorkspaceName_AG_ #Number*.

```
armclient GET  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

GET isteğini başarılı olursa, bir HTTP durum kodu 200, uyarıların bir listesi ile birlikte döndürülen ve önerilen eylem grupları JSON veri. Bir örnek yanıt verilmiştir:

```json
{
    "version": 1,
    "migrationSummary": {
        "alertsCount": 2,
        "actionGroupsCount": 2,
        "alerts": [
            {
                "alertName": "DemoAlert_1",
                "alertId": " /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/savedSearches/<savedSearchId>/schedules/<scheduleId>/actions/<actionId>",
                "actionGroupName": "<workspaceName>_AG_1"
            },
            {
                "alertName": "DemoAlert_2",
                "alertId": " /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/savedSearches/<savedSearchId>/schedules/<scheduleId>/actions/<actionId>",
                "actionGroupName": "<workspaceName>_AG_2"
            }
        ],
        "actionGroups": [
            {
                "actionGroupName": "<workspaceName>_AG_1",
                "actionGroupResourceId": "/subscriptions/<subscriptionid>/resourceGroups/<resourceGroupName>/providers/microsoft.insights/actionGroups/<workspaceName>_AG_1",
                "actions": {
                    "emailIds": [
                        "JohnDoe@mail.com"
                    ],
                    "webhookActions": [
                        {
                            "name": "Webhook_1",
                            "serviceUri": "http://test.com"
                        }
                    ],
                    "itsmAction": {}
                }
            },
            {
                "actionGroupName": "<workspaceName>_AG_1",
                "actionGroupResourceId": "/subscriptions/<subscriptionid>/resourceGroups/<resourceGroupName>/providers/microsoft.insights/actionGroups/<workspaceName>_AG_1",
                 "actions": {
                    "emailIds": [
                        "test1@mail.com",
                          "test2@mail.com"
                    ],
                    "webhookActions": [],
                    "itsmAction": {
                        "connectionId": "<Guid>",
                        "templateInfo":"{\"PayloadRevision\":0,\"WorkItemType\":\"Incident\",\"UseTemplate\":false,\"WorkItemData\":\"{\\\"contact_type\\\":\\\"email\\\",\\\"impact\\\":\\\"3\\\",\\\"urgency\\\":\\\"2\\\",\\\"category\\\":\\\"request\\\",\\\"subcategory\\\":\\\"password\\\"}\",\"CreateOneWIPerCI\":false}"
                    }
                }
            }
        ]
    }
}

```
Belirtilen çalışma tanımlı hiçbir uyarı kuralı yoksa, JSON verilerini aşağıdaki döndürür:

```json
{
    "version": 1,
    "Message": "No Alerts found in the workspace for migration."
}
```

Belirtilen çalışma alanındaki tüm uyarı kuralları Azure'a zaten genişletilmiştir, GET isteğine yanıt şöyledir:

```json
{
    "version": 2
}
```

Uyarıları Azure'a geçirme başlatmak için bir POST yanıt başlatır. POST yanıt, amacı, yanı sıra kabul Azure Uyarıları'için günlük analizi genişletilmiş uyarılar onaylar. Etkinlik zamanlanmış ve Uyarıları Al yanıt önceki gerçekleştirildiğinde sonuçlarına dayalı belirtildiği şekilde işlenir. İsteğe bağlı olarak, uyarıları geçirme zamanlanmış arka plan işlemi başarıyla tamamlandığında, günlük analizi rapor gönderdiği e-posta adreslerinin listesini sağlayabilirsiniz. Aşağıdaki isteği örnek kullanabilirsiniz:

```
$emailJSON = “{‘Recipients’: [‘a@b.com’, ‘b@a.com’]}”
armclient POST  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview $emailJSON
```

> [!NOTE]
> Sonucu Azure uyarıları içine geçirme uyarıları bağlı olarak değişir GET yanıt tarafından sağlanan özeti. Zamanlanan günlük analizi uyarıları Operations Management Suite portalına değişiklik için geçici olarak kullanılamıyor. Ancak, yeni uyarılar oluşturabilirsiniz. 

POST isteğini başarılı olursa, aşağıdaki yanıtı birlikte bir HTTP 200 Tamam durumu döndürür:

```json
{
    "version": 2
}
```

Bu yanıt uyarıları Azure Uyarıları ' başarılı bir şekilde genişletilmiştir gösterir. Uyarılar için Azure genişletilmiş ve ilişkisi yoktur, yalnızca denetlemek için sürüm özelliğidir [günlük analizi arama API](../log-analytics/log-analytics-api-alerts.md). Uyarıları başarıyla Azure'a genişletilmiş olduğunda ile POST isteği gönderilir sağlanan bir rapor herhangi bir e-posta adresleri. Belirtilen çalışma alanındaki tüm uyarıları zaten genişletilmesi zamanlanan, POST isteğinin yanıtı denemesi (403 durum kodu) yasaklanmış olur. Herhangi bir hata iletisi görüntülemek veya işlem takıldı anlamak için bir GET isteği gönderebilirsiniz. Bir hata iletisi varsa, bu, Özet bilgilerin yanı sıra döndürülür.

```json
{
    "version": 1,
    "message": "OMS was unable to extend your alerts into Azure, Error: The subscription is not registered to use the namespace 'microsoft.insights'. OMS will schedule extending your alerts, once remediation steps illustrated in the troubleshooting guide are done.",
    "recipients": [
       "john.doe@email.com",
       "jane.doe@email.com"
     ],
    "migrationSummary": {
        "alertsCount": 2,
        "actionGroupsCount": 2,
        "alerts": [
            {
                "alertName": "DemoAlert_1",
                "alertId": " /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/savedSearches/<savedSearchId>/schedules/<scheduleId>/actions/<actionId>",
                "actionGroupName": "<workspaceName>_AG_1"
            },
            {
                "alertName": "DemoAlert_2",
                "alertId": " /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/savedSearches/<savedSearchId>/schedules/<scheduleId>/actions/<actionId>",
                "actionGroupName": "<workspaceName>_AG_2"
            }
        ],
        "actionGroups": [
            {
                "actionGroupName": "<workspaceName>_AG_1",
                "actionGroupResourceId": "/subscriptions/<subscriptionid>/resourceGroups/<resourceGroupName>/providers/microsoft.insights/actionGroups/<workspaceName>_AG_1",
                "actions": {
                    "emailIds": [
                        "JohnDoe@mail.com"
                    ],
                    "webhookActions": [
                        {
                            "name": "Webhook_1",
                            "serviceUri": "http://test.com"
                        }
                    ],
                    "itsmAction": {}
                }
            },
            {
                "actionGroupName": "<workspaceName>_AG_1",
                "actionGroupResourceId": "/subscriptions/<subscriptionid>/resourceGroups/<resourceGroupName>/providers/microsoft.insights/actionGroups/<workspaceName>_AG_1",
                 "actions": {
                    "emailIds": [
                        "test1@mail.com",
                          "test2@mail.com"
                    ],
                    "webhookActions": [],
                    "itsmAction": {
                        "connectionId": "<Guid>",
                        "templateInfo":"{\"PayloadRevision\":0,\"WorkItemType\":\"Incident\",\"UseTemplate\":false,\"WorkItemData\":\"{\\\"contact_type\\\":\\\"email\\\",\\\"impact\\\":\\\"3\\\",\\\"urgency\\\":\\\"2\\\",\\\"category\\\":\\\"request\\\",\\\"subcategory\\\":\\\"password\\\"}\",\"CreateOneWIPerCI\":false}"
                    }
                }
            }
        ]
    }
}              

```


## <a name="option-3-use-a-custom-powershell-script"></a>Seçenek 3: özel bir PowerShell komut dosyası kullanın
 Microsoft başarıyla uyarılarınızı Operations Management Suite Portalı'ndan Azure'a genişlettiği değil, 5 Temmuz 2018 kadar el ile yapabilirsiniz. El ile uzantısı için iki seçenek önceki iki bölümlerde ele alınmıştır.

5 Temmuz 2018 sonra tüm uyarıları Operations Management Suite portalından Azure'da genişletilmiştir. Ele kaydetmedi kullanıcılar [önerilen gerekli düzeltme adımları](#troubleshooting) eylemler veya eksikliği nedeniyle bildirimleri tetikleme olmadan çalışan uyarılarını ilişkilendirilmiş olan [Eylem grupları](monitoring-action-groups.md). 

Oluşturmak için [Eylem grupları](monitoring-action-groups.md) uyarılar için el ile günlük analizi, aşağıdaki örnek komut dosyasını kullanın:
```PowerShell
########## Input Parameters Begin ###########


$subscriptionId = ""
$resourceGroup = ""
$workspaceName = "" 


########## Input Parameters End ###########

armclient login

try
{
    $workspace = armclient get /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/"$workspaceName"?api-version=2015-03-20 | ConvertFrom-Json
    $workspaceId = $workspace.properties.customerId
    $resourceLocation = $workspace.location
}
catch
{
    "Please enter valid input parameters i.e. Subscription Id, Resource Group and Workspace Name !!"
    exit
}

# Get Extend Summary of the Alerts
"`nGetting Extend Summary of Alerts for the workspace...`n"
try
{

    $value = armclient get /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/alertsversion?api-version=2017-04-26-preview

    "Extend preview summary"
    "=========================`n"

    $value

    $result = $value | ConvertFrom-Json
}
catch
{

    $ErrorMessage = $_.Exception.Message
    "Error occured while fetching/parsing Extend summary: $ErrorMessage"
    exit 
}

if ($result.version -eq 2)
{
    "`nThe alerts in this workspace have already been extended to Azure."
    exit
}

$in = Read-Host -Prompt "`nDo you want to continue extending the alerts to Azure? (Y/N)"

if ($in.ToLower() -ne "y")
{
    exit
} 


# Check for resource provider registration
try
{
    $val = armclient get subscriptions/$subscriptionId/providers/microsoft.insights/?api-version=2017-05-10 | ConvertFrom-Json
    if ($val.registrationState -eq "NotRegistered")
    {
        $val = armclient post subscriptions/$subscriptionId/providers/microsoft.insights/register/?api-version=2017-05-10
    }
}
catch
{
    "`nThe user does not have required access to register the resource provider. Please try with user having Contributor/Owner role in the subscription"
    exit
}

$actionGroupsMap = @{}
try
{
    "`nCreating new action groups for alerts extension...`n"
    foreach ($actionGroup in $result.migrationSummary.actionGroups)
    {
        $actionGroupName = $actionGroup.actionGroupName
        $actions = $actionGroup.actions
        if ($actionGroupsMap.ContainsKey($actionGroupName))
        {
            continue
        } 
        
        # Create action group payload
        $shortName = $actionGroupName.Substring($actionGroupName.LastIndexOf("AG_"))
        $properties = @{"groupShortName"= $shortName; "enabled" = $true}
        $emailReceivers = New-Object Object[] $actions.emailIds.Count
        $webhookReceivers = New-Object Object[] $actions.webhookActions.Count
        
        $count = 0
        foreach ($email in $actions.emailIds)
        {
            $emailReceivers[$count] = @{"name" = "Email$($count+1)"; "emailAddress" = "$email"}
            $count++
        }

        $count = 0
        foreach ($webhook in $actions.webhookActions)
        {
            $webhookReceivers[$count] = @{"name" = "$($webhook.name)"; "serviceUri" = "$($webhook.serviceUri)"}
            $count++
        }

        $itsmAction = $actions.itsmAction
        if ($itsmAction.connectionId -ne $null)
        {
            $val = @{
            "name" = "ITSM"
            "workspaceId" = "$subscriptionId|$workspaceId"
            "connectionId" = "$($itsmAction.connectionId)"
            "ticketConfiguration" = $itsmAction.templateInfo
            "region" = "$resourceLocation"
            }
            $properties["itsmReceivers"] = @($val)  
        }

        $properties["emailReceivers"] = @($emailReceivers)
        $properties["webhookReceivers"] = @($webhookReceivers)
        $armPayload = @{"properties" = $properties; "location" = "Global"} | ConvertTo-Json -Compress -Depth 4

    
        # Azure Resource Manager call to create action group
        $response = $armPayload | armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.insights/actionGroups/$actionGroupName/?api-version=2017-04-01

        "Created Action Group with name $actionGroupName" 
        $actionGroupsMap[$actionGroupName] = $actionGroup.actionGroupResourceId.ToLower()
        $index++
    }

    "`nSuccessfully created all action groups!!"
}
catch
{
    $ErrorMessage = $_.Exception.Message

    #Delete all action groups in case of failure
    "`nDeleting newly created action groups if any as some error happened..."
    
    foreach ($actionGroup in $actionGroupsMap.Keys)
    {
        $response = armclient delete /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.insights/actionGroups/$actionGroup/?api-version=2017-04-01      
    }

    "`nError: $ErrorMessage"
    "`nExiting..."
    exit
}

# Update all alerts configuration to the new version
"`nExtending OMS alerts to Azure...`n"

try
{
    $index = 1
    foreach ($alert in $result.migrationSummary.alerts)
    {
        $uri = $alert.alertId + "?api-version=2015-03-20"
        $config = armclient get $uri | ConvertFrom-Json
        $aznsNotification = @{
            "GroupIds" = @($actionGroupsMap[$alert.actionGroupName])
        }
        if ($alert.customWebhookPayload)
        {
            $aznsNotification.Add("CustomWebhookPayload", $alert.customWebhookPayload)
        }
        if ($alert.customEmailSubject)
        {
            $aznsNotification.Add("CustomEmailSubject", $alert.customEmailSubject)
        }      

        # Update alert version
        $config.properties.Version = 2

        $config.properties | Add-Member -MemberType NoteProperty -Name "AzNsNotification" -Value $aznsNotification
        $payload = $config | ConvertTo-Json -Depth 4
        $response = $payload | armclient put $uri
    
        "Extended alert with name $($alert.alertName)"
        $index++
    }
}
catch
{
    $ErrorMessage = $_.Exception.Message   
    if ($index -eq 1)
    {
        "`nDeleting all newly created action groups as no alerts got extended..."
        foreach ($actionGroup in $actionGroupsMap.Keys)
        {
            $response = armclient delete /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.insights/actionGroups/$actionGroup/?api-version=2017-04-01      
        }
        "`nDeleted all action groups."  
    }
    
    "`nError: $ErrorMessage"
    "`nPlease resolve the issue and try extending again!!"
    "`nExiting..."
    exit
}

"`nSuccessfully extended all OMS alerts to Azure!!" 

# Update version of workspace to indicate extension
"`nUpdating alert version information in OMS workspace..." 

$response = armclient post "/subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/alertsversion?api-version=2017-04-26-preview&iversion=2"

"`nExtension complete!!"
```


### <a name="about-the-custom-powershell-script"></a>Özel PowerShell komut dosyası hakkında 
Komut dosyası kullanma hakkında önemli bilgiler aşağıdadır:
- Yüklemesini bir önkoşuldur [ARMclient](https://github.com/projectkudu/ARMClient), Azure Kaynak Yöneticisi API'si çağırma basitleştiren bir açık kaynak komut satırı aracı.
- Komut dosyasını çalıştırmak için Azure aboneliğinde katılımcı veya sahibi rolü olmalıdır.
- Aşağıdaki parametreleri belirtmeniz gerekir:
    - $subscriptionId: Operations Management Suite günlük analizi çalışma alanı ile ilişkili Azure abonelik kimliği.
    - $resourceGroup: Operations Management Suite günlük analizi çalışma alanı için Azure kaynak grubu.
    - $workspaceName: Operations Management Suite günlük analizi çalışma alanı adı.

### <a name="output-of-the-custom-powershell-script"></a>Özel bir PowerShell komut dosyası çıktısı
Komut dosyası ayrıntılıdır ve çalışan adımları çıkarır: 
- Çalışma alanında varolan Operations Management Suite günlük analizi uyarılarla ilgili bilgiler içeren Özet görüntüler. Özet, ilişkili eylemler için oluşturulacak Azure Eylem grupları hakkındaki bilgileri de içerir. 
- Uzantısı ile devam edin veya Özet görüntüledikten sonra çıkmak istenir.
- Uzantısı ile devam edin, yeni Azure Eylem grupları oluşturulur ve varolan tüm uyarıları bunlarla ilişkili. 
- "Tam! uzantısı" iletisini görüntüleyerek betik çıkar Tüm ara başarısızlık durumunda komut sonraki hataları görüntüler.

## <a name="troubleshooting"></a>Sorun giderme 
Uyarıları genişletme işlemi sırasında sorunları sistem gerekli oluşturulurken engelleyebilir [Eylem grupları](monitoring-action-groups.md). Böyle durumlarda, bir başlığı bir hata iletisi bkz **uyarı** bölüm Operations Management Suite portalı ya da Get API için Bitti'yi çağırın.

> [!IMPORTANT]
> 5 Temmuz 2018 önce aşağıdaki düzeltme adımları görüntüsünü yok uyarıları Azure üzerinde çalışır, ancak herhangi bir eylem veya bildirim harekete geçmeyecektir. Uyarılar için bildirim almak için el ile düzenleme eklemek ve gerekir [Eylem grupları](monitoring-action-groups.md), veya önceki kullanın [özel bir PowerShell komut dosyası](#option-3---using-custom-powershell-script).

Her hata düzeltme adımları şunlardır:
- **Hata: Kapsam kilit abonelik/kaynak grubu düzeyinde yazma işlemleri için varsa**: ![Operations Management Suite portal sayfasının uyarı ayarlarını, kapsam kilit hata iletisiyle vurgulanan ekran görüntüsü](./media/monitor-alerts-extend/ErrorScopeLock.png)

    Kapsam kilidi etkinleştirilmişse, özellik için günlük analizi (Operations Management Suite) çalışma içeren abonelik veya kaynak grubu içinde herhangi bir yeni değişiklik kısıtlar. Azure'da uyarıları genişletmek ve gerekli Eylem grupları oluşturmak sistem alamıyor.
    
    Çözmek için silme *ReadOnly* çalışma alanını içeren abonelik veya kaynak grubunuz kilit. Azure portalını, PowerShell'i, Azure CLI veya API kullanarak bunu yapabilirsiniz. Daha fazla bilgi için bkz: [kaynak kilit kullanımı](../azure-resource-manager/resource-group-lock-resources.md). 
    
    Makalede gösterilen adımları kullanarak hatanın çözümlediğinizde Operations Management Suite içinde sonraki günün zamanlanmış çalıştırmada Azure'da uyarılarınızı genişletir. Başka herhangi bir eylemde veya her şeyi başlatma gerekmez.

- **Hata: Abonelik/kaynak grubu düzeyinde ilke varsa**: ![Operations Management Suite portal sayfasının uyarı ayarlarını, ilke hata iletisiyle vurgulanan ekran görüntüsü](./media/monitor-alerts-extend/ErrorPolicy.png)

    Zaman [Azure ilke](../azure-policy/azure-policy-introduction.md) olan uygulanan, tüm yeni kaynak grubunda günlük analizi (Operations Management Suite) çalışma alanını içeren bir abonelik veya kaynak kısıtlar. Azure'da uyarıları genişletmek ve gerekli Eylem grupları oluşturmak sistem alamıyor.
    
    Çözmek için neden olan ilkeyi düzenlediğinizde *[RequestDisallowedByPolicy](../azure-resource-manager/resource-manager-policy-requestdisallowedbypolicy-error.md)* çalışma alanı içerir, abonelik veya kaynak grubu üzerinde yeni kaynaklar oluşturulmasını engeller hata. Azure portalını, PowerShell'i, Azure CLI veya API kullanarak bunu yapabilirsiniz. Hataya neden olan uygun ilke bulmak için eylemlerini denetleyebilirsiniz. Daha fazla bilgi için bkz: [Eylemler denetim için etkinlik günlükleri görüntüleme](../azure-resource-manager/resource-group-audit.md). 
    
    Makalede gösterilen adımları kullanarak hatanın çözümlediğinizde Operations Management Suite içinde sonraki günün zamanlanmış çalıştırmada Azure'da uyarılarınızı genişletir. Başka herhangi bir eylemde veya her şeyi başlatma gerekmez.


## <a name="next-steps"></a>Sonraki adımlar

* Yeni hakkında daha fazla bilgi [Azure uyarıları deneyimi](monitoring-overview-unified-alerts.md).
* Hakkında bilgi edinin [uyarıları Azure Uyarıları'nda oturum](monitor-alerts-unified-log.md).
