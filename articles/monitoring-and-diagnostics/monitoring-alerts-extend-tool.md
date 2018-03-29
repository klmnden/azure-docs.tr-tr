---
title: Azure'da OMS uyarılar genişletme başlatma | Microsoft Docs
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
ms.date: 03/16/2018
ms.author: vinagara
ms.openlocfilehash: 76b7481223566f16a5da8c08d9d76f2bdb6b542a
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="initiate-extending-alerts-from-oms-into-azure"></a>Azure'da OMS genişletme uyarıları başlatır
Başlangıç **23 Nisan 2018**, yapılandırılan uyarıları aracılığıyla tüm müşterilere [Microsoft Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md), Azure'da uzatılır. Azure için genişletilmiş uyarıları OMS aynı şekilde davranır. İzleme yeteneklerini değişmeden kalır. Azure için OMS oluşturulan uyarıların genişletme birçok avantaj sağlar. Avantajları ve Uyarılar için Azure OMS genişletme işlemi hakkında daha fazla bilgi için bkz: [genişletmek uyarıları OMS Azure'a](monitoring-alerts-extend.md).

Azure'a OMS uyarılarını hemen taşımak isteyen müşteriler yapabilirsiniz belirtildiği seçeneklerden birini kullanarak.

## <a name="option-1---using-oms-portal"></a>1. seçenek - OMS portalı kullanma
Uyarıları OMS Azure'da genişletme gönüllü başlatmak için aşağıda listelenen adımları izleyin.

1. Operations Management Suite (OMS) genel bakış sayfasında, ayarları ve sonra uyarıları bölümüne gidin. "Genişletmek içine Azure", vurgulanmış çizimde etiketli düğmesine tıklayın.

    ![Genişletme seçeneği OMS uyarı ayarları sayfası](./media/monitor-alerts-extend/ExtendInto.png)

2. Düğme tıklatıldığında 3 adım Sihirbazı işleminin ayrıntıları sağlayan ilk adımı ile gösterilir. Devam etmek için İleri düğmesine basın.

    ![Uyarıları OMS Azure'da - 1. adım genişletir.](./media/monitor-alerts-extend/ExtendStep1.png)

3. İkinci adımda sistem listeleme uygun önerilen değişikliği özetini gösterir [Eylem grupları](monitoring-action-groups.md), OMS uyarılarını. Benzer eylemler arasında birden fazla uyarısı - görülüyorsa sistem bunların tümünün ile tek eylem grubu ilişkilendirilecek önerebilir.  Önerilen eylem grubu izleyin adlandırma kuralı: *WorkspaceName_AG_ #Number*. Olması devam etmek için İleri'yi tıklatın.
Aşağıdaki örnek ekrana.

    ![Uyarıları OMS Azure'da - 2. adım genişletir.](./media/monitor-alerts-extend/ExtendStep2.png)


4. Sihirbazın son adımda yeni eylem grupları oluşturarak ve bunları önceki ekran görüntüsünde gösterildiği gibi uyarıları ile ilişkilendirme Azure'da - tüm uyarılarınızı genişletme zamanlamak için OMS sorabilirsiniz. Devam etmek için seçin Son'u tıklatın ve işlemini başlatmak için komut isteminde onaylayın. İsteğe bağlı olarak, müşteriler işleme bitiş üzerinde bir rapor göndermek için OMS istedikleri e-posta adresleri de sağlayabilirsiniz.

    ![Uyarıları OMS Azure'da - 3. adım genişletir.](./media/monitor-alerts-extend/ExtendStep3.png)

5. Sihirbaz tamamlandıktan sonra Denetim uyarı ayarları sayfasına döndürür ve "Azure içine genişletme" seçeneği kaldırılır. Arka planda, Azure'a genişletilmesi OMS uyarılar OMS zamanlar; Bu işlem biraz zaman alabilir ve OMS kısa bir dönem uyarılarını işlemi başladığında, değiştirilmek üzere kullanılabilir olmaz. Geçerli durumu başlık gösterilir ve 4. adımda sonra bunlar olacaktır sağlanan e-posta where adresleri varsa, arka plan işlemi başarıyla tüm uyarıları Azure'da genişletir bilgisi. 

6. Uyarıları bile bunların başarıyla Azure'da genişletilmiş sonra OMS içinde listelenecek devam eder.

    ![Azure için OMS uyarılar genişlettikten sonra](./media/monitor-alerts-extend/PostExtendList.png)


## <a name="option-2---using-api"></a>Seçenek 2 - API kullanma
Programlı olarak denetlemek veya, Azure'da OMS uyarılar genişletme işlemini otomatikleştirmek isteyen müşteriler için; Microsoft, günlük analizi altında yeni AlertsVersion API sağlamıştır.

Günlük analizi AlertsVersion API RESTful ve Azure Resource Manager REST API'si erişilebilir. Bu belgede, API kullanarak bir PowerShell komut satırı burada erişilen örnekler bulacaksınız [ARMClient](https://github.com/projectkudu/ARMClient), Azure Kaynak Yöneticisi API'si çağırma basitleştiren bir açık kaynak komut satırı aracı. ARMClient ve PowerShell kullanımını API erişmek için birçok seçenek biridir. API sonuçları JSON biçiminde sonuçları kullanımını birçok farklı yolla program aracılığıyla izin vererek çıktı.

GET API'sini kullanarak, bir sonuçlanır önerilen değişikliği özetini uygun listesi olarak elde edebilirsiniz [Eylem grupları](monitoring-action-groups.md) JSON'de OMS uyarılar için biçimlendirin. Benzer eylemler arasında birden fazla uyarı görülen - sistem oluşturmayı önerecektir, bunların tümünün ile tek eylem grubu ilişkilendirin.  Önerilen eylem grubu izleyin adlandırma kuralı: *WorkspaceName_AG_ #Number*.

```
armclient GET  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

> [!NOTE]
> Alma API çağrısı Azure'da genişletilmiş OMS uyarılar değil sonuçlanır. Yalnızca Özet yanıt olarak sağlarız önerilen değişikliklerin. Bir POST uyarıları Azure'da genişletmek için bu değişikliklerin yapılması onaylamak için API için yapılması gereken çağırın.

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

Azure için OMS uyarıları genişletme zamanlama başlatmak için API POST başlatın. Bu çağrı/komutu yapılması kullanıcının hedefi yanı sıra Azure'a genişletilmiş OMS uyarılarını var ve yanıt GET çağrısı API belirtildiği gibi değişiklikleri yapabilir için kabul onaylar. İsteğe bağlı olarak, kullanıcı için Azure OMS uyarıları genişletme zamanlanmış arka plan işlemi başarıyla tamamlandığında, bir rapor OMS olduğu posta e-posta adreslerinin listesini sağlayabilirsiniz.

```
$emailJSON = “{‘Recipients’: [‘a@b.com’, ‘b@a.com’]}”
armclient POST  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview $emailJSON
```

> [!NOTE]
> Genişletme sonucunu OMS Azure uyarıları sağlanan tarafından GET - sistemde yapılan herhangi bir değişiklik hesabındaki özeti gelen farklılık gösterebilir. Yeni uyarılar oluşturduğunuz sırada zamanlanmış sonra OMS uyarılar düzenleme/değiştirilmek üzere - geçici olarak kullanılamaz. 

POST başarılı olursa, 200 Tamam yanıt ile birlikte Döndür:
```json
{
    "version": 2
}
```
Uyarılar, Azure'da genişletilmiştir sürüm 2 tarafından belirtildiği şekilde gösteren. Uyarıları Azure'da genişletilmiş ve şifrelemeyle kullanımı ile varsa yalnızca denetlemek için bu sürümüdür [günlük analizi arama API](../log-analytics/log-analytics-api-alerts.md). Çalışma alanındaki Yönetici ve katkıda bulunan rolleriyle ilişkili olan tüm kullanıcıları uyarıları başarıyla Azure'da genişletilmiş sonra yapılan değişiklikleri ayrıntılarını içeren bir e-posta alırsınız.


Ve son olarak, belirtilen çalışma alanında, tüm uyarıları zaten zamanlandı, Azure'da - genişletilmesi POST yanıtı 403 Yasak olacaktır.


## <a name="next-steps"></a>Sonraki adımlar

* Yeni hakkında daha fazla bilgi [Azure uyarıları deneyimi](monitoring-overview-unified-alerts.md).
* Hakkında bilgi edinin [uyarıları Azure Uyarıları'nda oturum](monitor-alerts-unified-log.md).
