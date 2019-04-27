---
title: Uyarıları Log Analytics'ten Azure kamu buluta genişletin.
description: Bu makalede, uyarıları Log Analytics'ten Azure Uyarıları'na genişletebileceğiniz API ve araçları açıklar.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/04/2018
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: 9d734f74c4e12b369e46c15dcb9d01a8185dddd6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60431173"
---
# <a name="extend-alerts-from-log-analytics-into-azure-alerts"></a>Uyarıları Log Analytics'ten Azure uyarılarına genişletecektir genişletme
OMS portalında uyarıları özelliği, Azure Uyarıları'nda Azure kamu Bulutu ile değiştirilmektedir. Bu geçişin bir parçası olarak, ilk olarak yapılandırdığınız Log Analytics'te uyarıları Azure'a genişletilir. Bunlar otomatik olarak Azure'a taşınması için beklemek istemiyorsanız işlemi başlatabilirsiniz:

- Operations Management Suite portaldaki el ile. 
- Program aracılığıyla AlertsVersion API'si kullanarak.  

> [!NOTE]
> Microsoft, sistematik bir şekilde 1 Mart'ta 2019'üzerinde başlatma Azure Uyarılar ' Log Analytics, Azure kamu OMS portalı durumlarda oluşturulan uyarıların otomatik olarak genişletilir. Oluşturma sorunları varsa [Eylem grupları](../../azure-monitor/platform/action-groups.md), kullanın [düzeltme adımları](alerts-extend-tool.md#troubleshooting) otomatik olarak oluşturulan eylem grupları alınamıyor. 15 Mart 2019'e kadar Azure kamu OMS Portalı'nda aşağıdaki adımları kullanabilirsiniz.

## <a name="option-1-initiate-from-the-operations-management-suite-portal"></a>1. seçenek: Operations Management Suite Portalı'ndan Başlat
Aşağıdaki adımlar, Azure kamu bulutu için Operations Management Suite portalında çalışma alanı için uyarılar genişletmek açıklanmaktadır.  

1. Azure portalda **Tüm hizmetler**’i seçin. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**’i seçin.
2. Log Analytics abonelikleri bölmesinde, bir çalışma alanı seçin ve ardından **OMS portalında** Döşe.
![Log Analytics ekran abonelik bölmesinde, vurgulanan OMS portalında kutucuğu](media/alerts-extend-tool/azure-portal-01.png) 
3. Operations Management Suite portalına yönlendirilirsiniz sonra seçin **ayarları** simgesi.
![Operations Management Suite ekran portalıyla vurgulanmış ayarlar simgesi](media/alerts-extend-tool/oms-portal-settings-option.png) 
4. Gelen **ayarları** sayfasında **uyarılar**.  
5. Seçin **Azure'a Genişlet**.
![Operations Management Suite ekran portal uyarı ayarları sayfası, vurgulanan azure'a Genişlet](media/alerts-extend-tool/ExtendInto.png)
6. Üç adımlık Sihirbazı görünür **uyarılar** bölmesi. Genel bakışı okuyun ve seçin **sonraki**.
![1. adım Sihirbazı ekran görüntüsü](media/alerts-extend-tool/ExtendStep1.png)  
7. İkinci adımda, önerilen değişikliklerin özetini listeleme uygun gördüğünüz [Eylem grupları](../../azure-monitor/platform/action-groups.md) uyarılar için. Benzer işlemler arasında birden fazla uyarı görülüyorsa bir tek bir eylem grubu tümünün ile ilişkilendirmek sihirbazın önerir.  Adlandırma kuralı aşağıdaki gibidir: *WorkspaceName_AG_ #Number*. Devam etmek için seçmeniz **sonraki**.
![2. adım Sihirbazı ekran görüntüsü](media/alerts-extend-tool/ExtendStep2.png)  
8. Sihirbazın son adımda seçin **son**ve işlemini başlatmak için sorulduğunda onaylayın. İsteğe bağlı olarak, böylece işlemi tamamlanır ve tüm uyarılar Azure Uyarıları'na başarıyla taşındığını bildirilir bir e-posta adresi sağlayabilir.
![Adım 3 Sihirbazı ekran görüntüsü](media/alerts-extend-tool/ExtendStep3.png)

Sihirbaz olduğunda üzerinde tamamlanmış **uyarı ayarlarını** sayfasında, uyarıları Azure'a genişletme seçeneği kaldırılır. Arka planda uyarılarınızı Azure'a taşınır ve bu işlem biraz zaman alabilir. İşlem sırasında uyarıları Operations Management Suite Portalı'ndan değişiklik yapamazsınız. Portalın üst kısmındaki başlık geçerli durumunu görebilirsiniz. Daha önce sağladığınız bir e-posta adresi, işlem başarıyla tamamlandığında bir e-posta alırsınız.  


Uyarılar bile Azure'a başarıyla taşındıktan sonra Operations Management Suite portalında listelenmeye devam eder.
![Ekran Operations Management Suite portalı uyarı ayarları sayfası](media/alerts-extend-tool/PostExtendList.png)


## <a name="option-2-use-the-alertsversion-api"></a>2. seçenek: AlertsVersion API'si kullanma
Log Analytics AlertsVersion API'si uyarıları Log Analytics'ten Azure uyarılarına genişletecektir REST API'sine çağrı yapmadan herhangi bir istemciden genişletmek için kullanabilirsiniz. API kullanarak Powershell'den erişebilirsiniz [ARMClient](https://github.com/projectkudu/ARMClient), açık kaynak komut satırı aracı. JSON sonuçları çıkış sağlayabilir.  

API'yi kullanmak için öncelikle bir GET isteği oluşturun. Bu, değerlendirir ve aslında bir POST isteği kullanarak Azure'a genişletme girişiminde bulunmadan önce önerilen değişikliklerin özetini döndürür. Uyarıları ve önerilen listesini sonuçları listesinde [Eylem grupları](../../azure-monitor/platform/action-groups.md), JSON biçiminde. Benzer işlemler arasında birden fazla uyarı görülürse, bunların tümünün tek bir eylem grubuyla ilişkilendirilecek hizmet önerir. Adlandırma kuralı aşağıdaki gibidir: *WorkspaceName_AG_ #Number*.

```
armclient GET  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

GET isteği başarılı olursa, bir HTTP 200 durum kodu döndürdü, uyarıların bir listesi ile birlikte ve önerilen eylem gruplarında JSON verilerini. Bir yanıt örneği verilmiştir:

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
                            "serviceUri": "https://test.com"
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
Belirtilen çalışma alanı tanımlı hiçbir uyarı kuralı yoksa, JSON verilerini aşağıdaki döndürür:

```json
{
    "version": 1,
    "Message": "No Alerts found in the workspace for migration."
}
```

Belirtilen çalışma alanında uyarı kurallarının tümünü zaten Azure'a genişletilmiş, GET isteğine yanıt şöyledir:

```json
{
    "version": 2
}
```

Uyarıları Azure'a geçirmeye başlatmak üzere bir GÖNDERİ yanıtı başlatın. GÖNDERİ yanıtı, amacı, hem de kabul uyarıları Log Analytics'ten Azure Uyarıları'için genişletilmiş onaylar. Etkinlik zamanlandı ve uyarılar, daha önce GET yanıt gerçekleştirildiğinde sonuçlarına göre belirtildiği şekilde işlenir. İsteğe bağlı olarak, uyarıları geçirme zamanlanmış arka plan işlemi başarıyla tamamlandığında, Log Analytics bir rapor gönderdiği e-posta adreslerinin listesini sağlayabilirsiniz. Aşağıdaki isteği örnek kullanabilirsiniz:

```
$emailJSON = “{‘Recipients’: [‘a@b.com’, ‘b@a.com’]}”
armclient POST  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview $emailJSON
```

> [!NOTE]
> Sonucu geçirme uyarılar Azure uyarıları içine GET yanıt tarafından sağlanan özeti göre değişiklik gösterebilir. Zamanlandıysa, Log analytics'teki uyarılar Operations Management Suite portalında değişiklik için geçici olarak kullanılamıyor. Ancak, yeni uyarılar oluşturabilirsiniz. 

POST isteği başarılı olursa, şu yanıtı birlikte bir HTTP 200 Tamam durumu döndürür:

```json
{
    "version": 2
}
```

Bu yanıt uyarıları başarılı bir şekilde Azure uyarılarına genişletecektir genişletilmiştir gösterir. Uyarıları Azure'a genişletildi ve bir ilişkisi yoktur, yalnızca denetimi için sürüm özelliğidir [Log Analytics arama API](../../azure-monitor/platform/api-alerts.md). Uyarılar başarıyla Azure'a genişletilmiş, bir rapor ile POST isteği gönderilir sağlanan herhangi e-posta adresleri. Belirtilen çalışma alanı Uyarılardaki tüm genişletilmesi için zamanlanır, POST isteğinize yanıt denemesi (403 durum kodu) yasaklanmış olur. Herhangi bir hata iletisi görüntülemek veya işlem takılıp takılmadığına anlamak için bir GET isteği gönderebilirsiniz. Bir hata iletisi varsa, bu, Özet bilgilerinin yanı sıra döndürülür.

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
                            "serviceUri": "https://test.com"
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

## <a name="troubleshooting"></a>Sorun giderme 
Uyarılar genişletme işlemi sırasında sorunları sistem gerekli oluşturmasını engelleyebilirsiniz [Eylem grupları](../../azure-monitor/platform/action-groups.md). Bu gibi durumlarda, bir hata iletisi bir başlık görürsünüz **uyarı** bölüm Operations Management Suite portalı veya GET API'sine yapılan çağırın.

> [!IMPORTANT]
> Azure kamu Bulutu tabanlı OMS portalı kullanıcıları, 15 Mart 2019'den önce aşağıdaki düzeltme adımları yakalayana, uyarılar, Azure'da çalışır ancak herhangi bir eylem veya bildirim tetiklenmez. Uyarılar için bildirim almak için el ile azure'da kendi uyarı kuralları düzenlemek ve eklemeniz gerekir [Eylem grupları](../../azure-monitor/platform/action-groups.md)

Her hata için düzeltme adımları şunlardır:
- **Hata: Kapsam kilit mevcut abonelik/kaynak grubu düzeyinde yazma işlemleri için**:   ![Sayfasının ekran görüntüsü Operations Management Suite portalı uyarı ayarlarını, vurgulanan kapsam kilit hata iletisi](media/alerts-extend-tool/ErrorScopeLock.png)

    Kapsam kilidi etkinleştirildiğinde, özellik (Operations Management Suite) Log Analytics çalışma alanını içeren abonelik veya kaynak grubunu yeni herhangi bir değişiklik kısıtlar. Sistem uyarıları Azure'a genişletme ve gerekli Eylem grupları oluşturma silemiyor.
    
    Çözümlemek için silme *salt okunur* çalışma alanını içeren, abonelik veya kaynak grubu üzerinde kilit. Azure portalı, PowerShell, Azure CLI veya API kullanarak bunu yapabilirsiniz. Daha fazla bilgi için bkz. [kaynak kilidi kullanımı](../../azure-resource-manager/resource-group-lock-resources.md). 
    
    Makalesinde anlatılan adımları kullanarak Hatayı çözümlediğinizde, Operations Management Suite sonraki günün zamanlanan çalıştırma içinde uyarılarınızı Azure'a genişletir. Herhangi bir şey başlatmak veya başka bir işlem yapması gerekmez.

- **Hata: İlke abonelik/kaynak grubu düzeyinde varsa**:   ![Operations Management Suite portal sayfasının uyarı ayarlarını, ilke hata iletisiyle vurgulandığı ekran görüntüsü](media/alerts-extend-tool/ErrorPolicy.png)

    Zaman [Azure İlkesi](../../governance/policy/overview.md) olduğu uygulanan yeni bir kaynak (Operations Management Suite) Log Analytics çalışma alanı içeren bir abonelik veya kaynak grubu içinde sınırlar. Sistem uyarıları Azure'a genişletme ve gerekli Eylem grupları oluşturma silemiyor.
    
    Çözümlemek için neden olan ilkeyi Düzenle *[RequestDisallowedByPolicy](../../azure-resource-manager/resource-manager-policy-requestdisallowedbypolicy-error.md)* çalışma alanını içeren, abonelik veya kaynak grubundaki yeni kaynaklarının oluşturulmasını önleyen bir hata. Azure portalı, PowerShell, Azure CLI veya API kullanarak bunu yapabilirsiniz. Hataya neden olduğunu uygun ilkeyi bulmak için eylemleri denetleyebilirsiniz. Daha fazla bilgi için bkz. [eylemleri denetlemek için etkinlik günlüklerini görüntüleme](../../azure-resource-manager/resource-group-audit.md). 
    
    Makalesinde anlatılan adımları kullanarak Hatayı çözümlediğinizde, Operations Management Suite sonraki günün zamanlanan çalıştırma içinde uyarılarınızı Azure'a genişletir. Herhangi bir şey başlatmak veya başka bir işlem yapması gerekmez.


## <a name="next-steps"></a>Sonraki adımlar

* Yeni hakkında daha fazla bilgi [Azure Uyarıları'deneyimi](../../azure-monitor/platform/alerts-overview.md).
* Hakkında bilgi edinin [uyarılar Azure Uyarıları'nda oturum](alerts-unified-log.md).

