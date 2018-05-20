---
title: (Kopya) genişletmek uyarıları OMS Portalı'ndan Azure'da | Microsoft Docs
description: Araçlar ve olarak uyarıları OMS Azure Uyarıları ' genişletme yapılabilir müşteriler tarafından gönüllü API.
author: msvijayn
manager: kmadnani1
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/14/2018
ms.author: vinagara
ms.openlocfilehash: 241ac027a0606f901f51d6a20b9a48a2cf7a9fcf
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="how-to-extend-copy-alerts-from-oms-into-azure"></a>Azure'a OMS (kopya) uyarıları genişletme
Başlangıç **14 Mayıs 2018**, yapılandırılan uyarıları aracılığıyla tüm müşterilere [Microsoft Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md), Azure'da uzatılır. Azure için genişletilmiş uyarıları OMS aynı şekilde davranır. İzleme yeteneklerini değişmeden kalır. Azure için OMS oluşturulan uyarıların genişletme birçok avantaj sağlar. Avantajları ve Uyarılar için Azure OMS genişletme işlemi hakkında daha fazla bilgi için bkz: [genişletmek uyarıları OMS Azure'a](monitoring-alerts-extend.md).

> [!NOTE]
> 14 Mayıs 2018 - başlangıç Microsoft Azure için uyarıları otomatik olarak genişletme işlemi başlayacak. Tüm çalışma alanları ve Uyarıları bu günde uzatılır; Bunun yerine Microsoft tranches otomatik olarak uyarıları gelecek haftalarda genişletmek başlar. Bu nedenle uyarılarınızı OMS portalında otomatik-Azure'da hemen 14 Mayıs 2018 üzerinde uzatır değil ve kullanıcının aşağıdaki seçenekleri ayrıntıları kullanarak uyarılarını yine de el ile genişletebilirsiniz.

Azure'a OMS uyarılarını hemen taşımak isteyen müşteriler yapabilirsiniz belirtildiği seçeneklerden birini kullanarak.

## <a name="option-1---using-oms-portal"></a>1. seçenek - OMS portalı kullanma
Uyarıları OMS Portalı'ndan Azure'da genişletme gönüllü başlatmak için aşağıda listelenen adımları izleyin.

1. OMS portalı genel bakış sayfasında, ayarları ve sonra uyarıları bölümüne gidin. "Genişletmek içine Azure", vurgulanmış çizimde etiketli düğmesine tıklayın.

    ![OMS portalı uyarı ayarlarını sayfası genişletme seçeneği](./media/monitor-alerts-extend/ExtendInto.png)

2. Düğme tıklatıldığında 3 adım Sihirbazı işleminin ayrıntıları sağlayan ilk adımı ile gösterilir. Devam etmek için İleri düğmesine basın.

    ![Uyarıları OMS Portalı'ndan Azure'da - 1. adım genişletir.](./media/monitor-alerts-extend/ExtendStep1.png)

3. İkinci adımda sistem listeleme uygun önerilen değişikliği özetini gösterir [Eylem grupları](monitoring-action-groups.md), OMS portalında uyarılar için. Benzer eylemler arasında birden fazla uyarısı - görülüyorsa sistem bunların tümünün ile tek eylem grubu ilişkilendirilecek önerebilir.  Önerilen eylem grubu izleyin adlandırma kuralı: *WorkspaceName_AG_ #Number*. Olması devam etmek için İleri'yi tıklatın.
Aşağıdaki örnek ekrana.

    ![Uyarıları OMS Portalı'ndan Azure'da - adım 2 genişletir.](./media/monitor-alerts-extend/ExtendStep2.png)


4. Sihirbazın son adımda yeni eylem grupları oluşturarak ve bunları önceki ekran görüntüsünde gösterildiği gibi uyarıları ile ilişkilendirme Azure'da - tüm uyarılarınızı genişletme zamanlamak için OMS portalı sorabilirsiniz. Devam etmek için seçin Son'u tıklatın ve işlemini başlatmak için komut isteminde onaylayın. İsteğe bağlı olarak, müşteriler işleme bitiş üzerinde bir rapor göndermek için OMS portalı istedikleri e-posta adresleri de sağlayabilirsiniz.

    ![Uyarıları OMS Portalı'ndan Azure'da - 3. adım genişletir.](./media/monitor-alerts-extend/ExtendStep3.png)

5. Sihirbaz tamamlandıktan sonra Denetim uyarı ayarları sayfasına döndürür ve "Azure içine genişletme" seçeneği kaldırılır. Arka planda, Azure'a genişletilmesi için günlük analizi uyarılarını OMS portalı zamanlar; Bu işlem biraz zaman alabilir ve OMS portalında kısa bir dönem uyarılar için işlemi başladığında, değiştirilmek üzere kullanılabilir olmaz. Geçerli durumu başlık gösterilir ve 4. adımda sonra bunlar olacaktır sağlanan e-posta where adresleri varsa, arka plan işlemi başarıyla tüm uyarıları Azure'da genişletir bilgisi. 

6. Uyarıları bile bunların başarıyla Azure'da genişletilmiş sonra OMS Portalı'nda listelenmesi devam eder.

    ![Azure için OMS portalında uyarılar genişlettikten sonra](./media/monitor-alerts-extend/PostExtendList.png)


## <a name="option-2---using-api"></a>Seçenek 2 - API kullanma
Programlı olarak denetlemek veya, Azure'da OMS portalında uyarılar genişletme işlemini otomatikleştirmek isteyen müşteriler için; Microsoft, günlük analizi altında yeni AlertsVersion API sağlamıştır.

Günlük analizi AlertsVersion API RESTful ve Azure Resource Manager REST API'si erişilebilir. Bu belgede, API kullanarak bir PowerShell komut satırı burada erişilen örnekler bulacaksınız [ARMClient](https://github.com/projectkudu/ARMClient), Azure Kaynak Yöneticisi API'si çağırma basitleştiren bir açık kaynak komut satırı aracı. ARMClient ve PowerShell kullanımını API erişmek için birçok seçenek biridir. API sonuçları JSON biçiminde sonuçları kullanımını birçok farklı yolla program aracılığıyla izin vererek çıktı.

GET API'sini kullanarak, bir sonuçlanır önerilen değişikliği özetini uygun listesi olarak elde edebilirsiniz [Eylem grupları](monitoring-action-groups.md) JSON'de OMS portalında uyarılar için biçimlendirin. Benzer eylemler arasında birden fazla uyarı görülen - sistem oluşturmayı önerecektir, bunların tümünün ile tek eylem grubu ilişkilendirin.  Önerilen eylem grubu izleyin adlandırma kuralı: *WorkspaceName_AG_ #Number*.

```
armclient GET  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

> [!NOTE]
> Alma API çağrısı Azure'da genişletilmiş OMS portalında uyarılar neden. Yalnızca Özet yanıt olarak sağlarız önerilen değişikliklerin. Bir POST uyarıları Azure'da genişletmek için bu değişikliklerin yapılması onaylamak için API için yapılması gereken çağırın.

API GET çağrısı ile birlikte 200 Tamam yanıt başarılı olursa, JSON listesini uyarıları önerilen eylem grupları ile birlikte verilir. Aşağıdaki örnek yanıt:

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
ALMA işlemi için 200 Tamam yanıt birlikte olması durumunda, hiçbir uyarı belirtilen çalışma alanında, JSON olacaktır:

```json
{
    "version": 1,
    "Message": "No Alerts found in the workspace for migration."
}
```

Belirtilen çalışma alanında, tüm uyarıları zaten genişletilmişse Azure - GET çağrısının yanıtı şöyle olur:
```json
{
    "version": 2
}
```

Azure için OMS portalında uyarılar genişletme zamanlama başlatmak için API POST başlatın. Bu çağrı/komutu yapılması kullanıcının hedefi yanı sıra Azure'a genişletilmiş OMS portalında uyarılarını olması ve yanıt GET çağrısı API belirtildiği gibi değişiklikleri yapabilir kabul onaylar. İsteğe bağlı olarak, kullanıcı için Azure OMS portalında uyarılar genişletme zamanlanmış arka plan işlemi başarıyla tamamlandığında, hangi OMS bir rapor portal posta e-posta adreslerinin listesini sağlayabilirsiniz.

```
$emailJSON = “{‘Recipients’: [‘a@b.com’, ‘b@a.com’]}”
armclient POST  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview $emailJSON
```

> [!NOTE]
> OMS genişletme sonucunu Azure'da, portal uyarıları sağlanan tarafından GET - sistemde yapılan herhangi bir değişiklik hesabındaki özeti gelen farklılık gösterebilir. Yeni uyarılar oluşturduğunuz sırada zamanlanmış sonra OMS portalında uyarılar düzenleme/değiştirilmek üzere - geçici olarak kullanılamaz. 

POST başarılı olursa, 200 Tamam yanıt ile birlikte Döndür:
```json
{
    "version": 2
}
```
Uyarılar, Azure'da genişletilmiştir sürüm 2 tarafından belirtildiği şekilde gösteren. Uyarıları Azure'da genişletilmiş ve şifrelemeyle kullanımı ile varsa yalnızca denetlemek için bu sürümüdür [günlük analizi arama API](../log-analytics/log-analytics-api-alerts.md). Uyarıları başarıyla Azure'da genişletilmiş sonra tüm sırasında sağlanan e-posta adreslerini GET yapılan değişiklikleri ayrıntılarını içeren bir rapor gönderilir.

Ve son olarak, belirtilen çalışma alanında, tüm uyarıları zaten planlanmış durumunda Azure - genişletilmesi POST yanıtı 403 Yasak olacaktır. Herhangi bir hata iletisi görüntülemek veya olmadığının anlaşılması için genişletme işlemi takıldı, kullanıcı, bir GET çağrısı ve hata iletisi yapabilir, herhangi bir Özet birlikte döndürülecek durumunda.

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

## <a name="troubleshooting"></a>Sorun giderme 
Azure'da OMS uyarılar genişletme işlemi sırasında olabilir sistem gerekli oluşturmasını engeller nadir [Eylem grupları](monitoring-action-groups.md). Böyle durumlarda, başlık uyarı bölümünde ve API için yapılan GET çağrısı aracılığıyla OMS portalında bir hata iletisi gösterilir.

Aşağıda listelenen her bir hata düzeltme adımları şunlardır:
1. **Hata: Abonelik 'Microsoft.ınsights' ad alanını kullanmak için kayıtlı değil**: ![kayıt hata iletisi OMS portalı uyarı ayarları sayfası](./media/monitor-alerts-extend/ErrorMissingRegistration.png)

    a. OMS çalışma alanınızla - ilişkili abonelik kaydettirilemedi Azure İzleyicisi'ni (Microsoft.ınsights) işlevini kullanmayı; hangi OMS nedeniyle uyarılar Azure İzleyici & uyarılar genişletmek oluşturulamıyor.
    
    b. Çözmek için Powershell, Azure CLI veya Azure portal kullanarak aboneliğinizde Microsoft.ınsights (Azure İzleyici & uyarılar) kullanım kaydedin. Daha fazla bilgi için makaleyi görüntülemek [kaynak Sağlayıcısı kaydı hatalarını giderme](../azure-resource-manager/resource-manager-register-provider-errors.md)
    
    c. Makalede gösterilen adımları göredir çözülmüş sonra OMS Azure'da uyarılarınızı sonraki günün zamanlanmış çalıştırmada içinde Uzat; herhangi bir eylem veya başlatma gerek olmadan.
2. **Hata: Kapsam kilit abonelik/kaynak grubu düzeyinde yazma işlemleri için varsa**: ![ScopeLock hata iletisiyle OMS portalı uyarı ayarları sayfası](./media/monitor-alerts-extend/ErrorScopeLock.png)

    a. Kapsam zaman kilitleme, abonelik veya kaynak grubu için günlük analizi (OMS) çalışma içeren yeni herhangi bir değişikliği kısıtlama etkinleştirilir; Azure'da (kopya) uyarıları genişletmek ve gerekli Eylem grupları oluşturmak sistem alamıyor.
    
    b. Çözmek için silme *ReadOnly* Azure portalını, PowerShell'i, Azure CLI veya API kullanarak; çalışma içeren abonelik veya kaynak grubunuz kilit. Daha fazla bilgi için makaleyi görüntülemek [kaynak kilit kullanımı](../azure-resource-manager/resource-group-lock-resources.md). 
    
    c. Makalede gösterilen adımları göredir çözülmüş sonra OMS Azure'da uyarılarınızı sonraki günün zamanlanmış çalıştırmada içinde Uzat; herhangi bir eylem veya başlatma gerek olmadan.

3. **Hata: Abonelik/kaynak grubu düzeyinde ilke varsa**: ![ilke hata iletisi OMS portalı uyarı ayarları sayfası](./media/monitor-alerts-extend/ErrorPolicy.png)

    a. Zaman [Azure ilke](../azure-policy/azure-policy-introduction.md) uygulanır, abonelik veya kaynak grubu için günlük analizi (OMS) çalışma; içeren yeni bir kaynak kısıtlama sistem Azure'da (kopya) uyarıları genişletmek ve gerekli Eylem grupları oluşturmak alamıyor.
    
    b. İlke neden gidermek için Düzenle *[RequestDisallowedByPolicy](../azure-resource-manager/resource-manager-policy-requestdisallowedbypolicy-error.md)* çalışma içeren, abonelik veya kaynak grubu üzerinde yeni kaynaklar oluşturulmasını engeller hata. Azure portalını, PowerShell'i, Azure CLI veya API kullanarak; hataya neden olan uygun ilke bulmak için eylemlerini denetleyebilirsiniz. Daha fazla bilgi için makaleyi görüntülemek [Eylemler denetim için etkinlik günlükleri görüntüleme](../azure-resource-manager/resource-group-audit.md). 
    
    c. Makalede gösterilen adımları göredir çözülmüş sonra OMS Azure'da uyarılarınızı sonraki günün zamanlanmış çalıştırmada içinde Uzat; herhangi bir eylem veya başlatma gerek olmadan.


## <a name="next-steps"></a>Sonraki adımlar

* Yeni hakkında daha fazla bilgi [Azure uyarıları deneyimi](monitoring-overview-unified-alerts.md).
* Hakkında bilgi edinin [uyarıları Azure Uyarıları'nda oturum](monitor-alerts-unified-log.md).
