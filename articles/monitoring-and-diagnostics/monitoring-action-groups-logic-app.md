---
title: Azure izleme uyarıları ve eylem gruplarla karmaşık eylemleri tetiklemek nasıl
description: Azure izleme uyarıları işlemek için bir mantıksal uygulama eylemi oluşturmayı öğrenin.
author: dkamstra
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/30/2018
ms.author: dukek
ms.component: alerts
ms.openlocfilehash: 14e562234152d2f1f2f2d2b57b34cd5724d3dd14
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36753102"
---
# <a name="create-a-logic-app-action"></a>Bir mantıksal uygulama eylem oluşturun

Bu makalede ayarlama ve bir uyarı oluşturulduğunda bir konuşma Microsoft Teams oluşturmak için bir mantıksal uygulama tetiklemek gösterilmektedir.

## <a name="overview"></a>Genel Bakış
Azure İzleyici uyarıyı tetikleyen, çağıran bir [eylem grubu](monitoring-action-groups.md). Eylem gruplar başkaları hakkında uyarı bildirim ve ayrıca düzeltmek için bir veya daha fazla eylemleri tetiklemek sağlar.

Genel süreç şöyledir:

-   İlgili uyarı türü için mantıksal uygulama oluşturma.

-   İlgili uyarı türüne ilişkin şema mantıksal uygulama alın.

-   Mantıksal uygulama davranışını tanımlayın.

-   Mantıksal uygulama HTTP uç noktası bir Azure eylem grubuna kopyalayın.

İşlemi farklı bir eylem gerçekleştirmek için mantıksal uygulama istiyorsanız benzer.

## <a name="create-an-activity-log-alert-administrative"></a>Bir etkinlik günlüğü uyarısı oluştur: Yönetim

1.  Azure portalında seçin **kaynak oluşturma** sol üst köşedeki.

2.  Aramak ve seçmek **mantıksal uygulama**seçeneğini belirleyip **oluşturma**.

3.  Mantıksal uygulamanızı vermek bir **adı**, seçin bir **kaynak grubu**ve benzeri.

    ![Mantıksal uygulama oluşturma](media/monitoring-action-groups/create-logic-app-dialog.png "mantıksal uygulama oluşturma")

4.  Seçin **oluşturma** mantıksal uygulama oluşturmak için. Mantıksal uygulama oluşturduğunuz bir açılır ileti gösterir. Seçin **başlatma kaynak** açmak için **Logic Apps Tasarımcısı**.

5.  Tetikleyici seçin: **zaman bir HTTP isteği alındığında**.

    ![Mantığını uygulaması Tetikleyicileri](media/monitoring-action-groups/logic-app-triggers.png "mantığını uygulaması Tetikleyicileri")

6.  Seçin **Düzenle** HTTP isteği tetikleyici değiştirmek için.

    ![HTTP isteği Tetikleyicileri](media/monitoring-action-groups/http-request-trigger-shape.png "HTTP isteği Tetikleyicileri")

7.  **Şema oluşturmak için örnek yük kullanma** öğesini seçin.

    ![Bir örnek yükü kullanmak](media/monitoring-action-groups/use-sample-payload-button.png "bir örnek yükü kullanın")

8.  Kopyalayın ve aşağıdaki örnek şeması iletişim kutusuna yapıştırın:

    ```json
        {
            "schemaId": "Microsoft.Insights/activityLogs",
            "data": {
                "status": "Activated",
                "context": {
                "activityLog": {
                    "authorization": {
                    "action": "microsoft.insights/activityLogAlerts/write",
                    "scope": "/subscriptions/…"
                    },
                    "channels": "Operation",
                    "claims": "…",
                    "caller": "logicappdemo@contoso.com",
                    "correlationId": "91ad2bac-1afa-4932-a2ce-2f8efd6765a3",
                    "description": "",
                    "eventSource": "Administrative",
                    "eventTimestamp": "2018-04-03T22:33:11.762469+00:00",
                    "eventDataId": "ec74c4a2-d7ae-48c3-a4d0-2684a1611ca0",
                    "level": "Informational",
                    "operationName": "microsoft.insights/activityLogAlerts/write",
                    "operationId": "61f59fc8-1442-4c74-9f5f-937392a9723c",
                    "resourceId": "/subscriptions/…",
                    "resourceGroupName": "LOGICAPP-DEMO",
                    "resourceProviderName": "microsoft.insights",
                    "status": "Succeeded",
                    "subStatus": "",
                    "subscriptionId": "…",
                    "submissionTimestamp": "2018-04-03T22:33:36.1068742+00:00",
                    "resourceType": "microsoft.insights/activityLogAlerts"
                }
                },
                "properties": {}
            }
        }
    ```

9. **Mantığı Uygulama Tasarımcısı** mantığı uygulamaya gönderilen istek ayarlamalısınız anımsatmak için bir açılır pencere görüntüler **Content-Type** başlığına **uygulama/json**. Açılır penceresini kapatın. Azure İzleyicisi uyarısı üstbilgisini ayarlar.

    ![Content-Type üstbilgisi kümesi](media/monitoring-action-groups/content-type-header.png "Content-Type üstbilgisi ayarlayın")

10. Seçin **+** **yeni adım** ve ardından **Eylem Ekle**.

    ![Eylem Ekle](media/monitoring-action-groups/add-action.png "Eylem Ekle")

11. Arayın ve Microsoft Teams Bağlayıcısı'nı seçin. Seçin **Microsoft Teams – posta iletisi** eylem.

    ![Microsoft Teams Eylemler](media/monitoring-action-groups/microsoft-teams-actions.png "Microsoft Teams Eylemler")

12. Microsoft Teams eylemi yapılandırın. **Logic Apps Tasarımcısı** Office 365 hesabınıza kimlik doğrulaması yapmanızı ister. Seçin **Takım Kimliği** ve **kanal kimliği** ileti göndermek için.

13. Statik metin ve başvurular bir bileşimini kullanarak ileti yapılandırma \<alanları\> dinamik içerik. Aşağıdaki metni kopyalayıp **ileti** alan:

    ```text
      Activity Log Alert: <eventSource>
      operationName: <operationName>
      status: <status>
      resourceId: <resourceId>
    ```

    İçin arama ve değiştirme \<alanları\> dinamik içerik etiketleri, aynı ada sahip.

    > [!NOTE]
    > Adlandırılmış iki dinamik alan **durum**. Bu alanların her ikisi de iletiye ekleyin. İçinde alanı kullanmak **activityLog** özellik paketi ve delete diğer alan. İmlecinizi üzerine gelerek **durum** tam alan başvurusu, aşağıdaki ekran görüntüsünde gösterildiği gibi görmek için:

    ![Microsoft Teams eylem: bir ileti gönderme](media/monitoring-action-groups/teams-action-post-message.png "Microsoft Teams eylem: bir ileti gönderme")

14. Üstündeki **Logic Apps Tasarımcısı**seçin **kaydetmek** mantıksal uygulamanızı kaydetmek için.

15. Varolan eylem grubunuzun açın ve mantıksal uygulama başvurmak için bir eylem ekleyin. Varolan bir eylem grubu yoksa bkz [oluşturma ve Eylem grupları Azure portalında yönetmek](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-action-groups) oluşturmak için. Değişikliklerinizi kaydetmek unutmayın.

    ![Eylem grubunu güncelleştirme](media/monitoring-action-groups/update-action-group.png "eylem grubu güncelleştir")

Bir uyarı eylem grubunuzun çağırır sonraki açışınızda, mantıksal uygulamanızı adı verilir.

## <a name="create-a-service-health-alert"></a>Bir hizmet Sistem Durumu Uyarısı oluştur

Azure hizmet durumu girişleri etkinlik günlüğü bir parçasıdır. Uyarı oluşturma süreci benzer [bir etkinlik günlüğü uyarı oluşturma](#create-an-activity-log-alert-administrative), ancak bazı değişiklikler:

- 1 ile 7 arasındaki adımları aynıdır.
- Adım 8 için aşağıdaki örnek şeması için HTTP isteği tetikleyici kullanın:

    ```json
    {
        "schemaId": "Microsoft.Insights/activityLogs",
        "data": {
            "status": "Activated",
            "context": {
                "activityLog": {
                    "channels": "Admin",
                    "correlationId": "e416ed3c-8874-4ec8-bc6b-54e3c92a24d4",
                    "description": "…",
                    "eventSource": "ServiceHealth",
                    "eventTimestamp": "2018-04-03T22:44:43.7467716+00:00",
                    "eventDataId": "9ce152f5-d435-ee31-2dce-104228486a6d",
                    "level": "Informational",
                    "operationName": "Microsoft.ServiceHealth/incident/action",
                    "operationId": "e416ed3c-8874-4ec8-bc6b-54e3c92a24d4",
                    "properties": {
                        "title": "…",
                        "service": "…",
                        "region": "Global",
                        "communication": "…",
                        "incidentType": "Incident",
                        "trackingId": "…",
                        "impactStartTime": "2018-03-22T21:40:00.0000000Z",
                        "impactMitigationTime": "2018-03-22T21:41:00.0000000Z",
                        "impactedServices": "[{"ImpactedRegions"}]",
                        "defaultLanguageTitle": "…",
                        "defaultLanguageContent": "…",
                        "stage": "Active",
                        "communicationId": "11000001466525",
                        "version": "0.1.1"
                    },
                    "status": "Active",
                    "subscriptionId": "…",
                    "submissionTimestamp": "2018-04-03T22:44:50.8013523+00:00"
                }
            },
            "properties": {}
        }
    }
    ```

-  Adım 9 ve 10 aynıdır.
-  11 ile 14 adımlar için aşağıdaki işlemi kullanın:

   1. Seçin **+** **yeni adım** ve ardından **bir koşul eklemek**. Mantıksal uygulama yalnızca giriş verilerini bu değerleri zaman eşleşen çalıştığından emin olmak için aşağıdaki koşulların ayarlayın:
       - `schemaId == Microsoft.Insights/activityLogs`
       - `eventSource == ServiceHealth`
       - `version == 0.1.1`

      !["Hizmet durumu yükü koşul"](media/monitoring-action-groups/service-health-payload-condition.png "hizmet durumu yükü koşulu")

   1. İçinde **true ise** koşul, 13'te 11 adımlara'ndaki yönergeleri izleyin [etkinlik günlüğü uyarı oluşturma](#create-an-activity-log-alert-administrative) Microsoft Teams eylem eklemek için.

   1. İleti, HTML ve dinamik içerik bir bileşimini kullanarak tanımlayın. İçine aşağıdaki içeriği kopyalama ve yapıştırma **ileti** alan. Değiştir `[incidentType]`, `[trackingID]`, `[title]`, ve `[communication]` aynı ada sahip dinamik içerik etiketlerle alanlar:

       ```html
       <p>
       <b>Alert Type:</b>&nbsp;<strong>[incidentType]</strong>&nbsp;
       <strong>Tracking ID:</strong>&nbsp;[trackingId]&nbsp;
       <strong>Title:</strong>&nbsp;[title]</p>
       <p>
       <a href="https://ms.portal.azure.com/#blade/Microsoft_Azure_Health/AzureHealthBrowseBlade/serviceIssues">For details, log in to the Azure Service Health dashboard.</a>
       </p>
       <p>[communication]</p>
       ```

       !["Hizmet durumu koşulun sonrası eylemi"](media/monitoring-action-groups/service-health-true-condition-post-action.png "hizmet durumu koşulun sonrası eylemi")

   1. İçin **false ise** koşul, kullanışlı bir iletiyi sağlayın:

       ```html
       <p><strong>Service Health Alert</strong></p>
       <p><b>Unrecognized alert schema</b></p>
       <p><a href="https://ms.portal.azure.com/#blade/Microsoft_Azure_Health/AzureHealthBrowseBlade/serviceIssues">For details, log in to the Azure Service Health dashboard.\</a></p>
       ```

       !["Hizmet durumu yanlış koşul sonrası eylemi"](media/monitoring-action-groups/service-health-false-condition-post-action.png "hizmet durumu yanlış koşul sonrası eylemi")

- Adım 15 aynıdır. Mantıksal uygulamanızı kaydetmek ve eylem grubunuzun güncelleştirmek için yönergeleri izleyin.

## <a name="create-a-metric-alert"></a>Bir ölçüm uyarısı oluştur

Ölçüm bir uyarı oluşturmak için işlem benzer [bir etkinlik günlüğü uyarı oluşturma](#create-an-activity-log-alert-administrative), ancak bazı değişiklikler:

- 1 ile 7 arasındaki adımları aynıdır.
- Adım 8 için aşağıdaki örnek şeması için HTTP isteği tetikleyici kullanın:

    ```json
    {
    "schemaId": "AzureMonitorMetricAlert",
    "data": {
        "version": "2.0",
        "status": "Activated",
        "context": {
        "timestamp": "2018-04-09T19:00:07.7461615Z",
        "id": "…",
        "name": "TEST-VM CPU Utilization",
        "description": "",
        "conditionType": "SingleResourceMultipleMetricCriteria",
        "condition": {
            "windowSize": "PT15M",
            "allOf": [
                {
                    "metricName": "Percentage CPU",
                    "dimensions": [
                    {
                        "name": "ResourceId",
                        "value": "d92fc5cb-06cf-4309-8c9a-538eea6a17a6"
                    }
                ],
                "operator": "GreaterThan",
                "threshold": "5",
                "timeAggregation": "PT15M",
                "metricValue": 1.0
            }
            ]
        },
        "subscriptionId": "…",
        "resourceGroupName": "TEST",
        "resourceName": "test-vm",
        "resourceType": "Microsoft.Compute/virtualMachines",
        "resourceId": "…",
        "portalLink": "…"
        },
        "properties": {}
    }
    }
    ```

- Adım 9 ve 10 aynıdır.
- 11 ile 14 adımlar için aşağıdaki işlemi kullanın:

   1. Seçin **+** **yeni adım** ve ardından **bir koşul eklemek**. Mantıksal uygulama yalnızca giriş verilerini bu değerleri zaman eşleşen çalıştığından emin olmak için aşağıdaki koşulların ayarlayın:
       - `schemaId == AzureMonitorMetricAlert`
       - `version == 2.0`

       !["Ölçüm uyarı yükü koşul"](media/monitoring-action-groups/metric-alert-payload-condition.png "ölçüm uyarı yükü koşulu")

   1. İçinde **true ise** koşul, ekleme bir **her** döngü ve Microsoft Teams eylem. İleti, HTML ve dinamik içerik bir bileşimini kullanarak tanımlayın.

       !["Ölçüm uyarı koşulun sonrası eylemi"](media/monitoring-action-groups/metric-alert-true-condition-post-action.png "ölçüm uyarı koşulun sonrası eylemi")

   1. İçinde **false ise** koşul, ölçüm uyarı mantıksal uygulama beklentilerini eşleşmiyor iletişim kurmak için bir Microsoft Teams eylemi tanımlayın. JSON yükünü dahil et. Başvuru nasıl dikkat edin `triggerBody` dinamik içeriği `json()` ifade.

       !["Ölçüm uyarı yanlış koşulu sonrası eylemi"](media/monitoring-action-groups/metric-alert-false-condition-post-action.png "ölçüm uyarı yanlış koşulu sonrası eylemi")

- Adım 15 aynıdır. Mantıksal uygulamanızı kaydetmek ve eylem grubunuzun güncelleştirmek için yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar
* Alma bir [Azure etkinlik günlüğü uyarılar genel bakış](monitoring-overview-alerts.md) ve uyarıların nasıl alınacağını öğrenin.  
* Bilgi edinmek için nasıl [bir Azure hizmet durumu bildirimi gönderildiğinde uyarıları yapılandırmak](monitoring-activity-log-alerts-on-service-notifications.md).
* Daha fazla bilgi edinmek [Eylem grupları](monitoring-action-groups.md).