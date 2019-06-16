---
title: Azure İzleyici uyarılarıyla karmaşık eylemleri tetiklemek nasıl
description: Azure İzleyici uyarı işlemek için bir mantıksal uygulama eylemi oluşturmayı öğrenin.
author: dkamstra
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: dukek
ms.subservice: alerts
ms.openlocfilehash: a33c6f6621e7fc7944bc116b27e5f26de88f77d9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66389569"
---
# <a name="how-to-trigger-complex-actions-with-azure-monitor-alerts"></a>Azure İzleyici uyarılarıyla karmaşık eylemleri tetiklemek nasıl

Bu makalede ayarlama ve bir uyarı tetiklendiğinde bir konuşma Microsoft Teams içinde oluşturmak için bir mantıksal uygulama tetikleyicisi gösterilmektedir.

## <a name="overview"></a>Genel Bakış
Azure İzleyici bir uyarı tetiklendiğinde çağırdığı bir [eylem grubu](../../azure-monitor/platform/action-groups.md). Eylem grupları, diğerleri hakkında bir uyarı bildirir ve ayrıca düzeltmek için bir veya daha fazla eylemleri tetiklemek izin verin.

Genel süreç şöyledir:

-   İlgili uyarı türü için mantıksal uygulama oluşturun.

-   İlgili uyarı türü için bir örnek yük mantıksal uygulamaya aktarma.

-   Mantıksal uygulama davranışını tanımlayın.

-   Mantıksal uygulamanın HTTP uç noktası, bir Azure eylem grubu kopyalayın.

İşlemi farklı bir eylem gerçekleştirmek için mantıksal uygulama istiyorsanız benzer.

## <a name="create-an-activity-log-alert-administrative"></a>Bir etkinlik günlüğü uyarısı oluştur: Yönetim

1.  Azure portalında **kaynak Oluştur** sol üst köşedeki.

2.  Arayın ve seçin **mantıksal uygulama**, ardından **Oluştur**.

3.  Mantıksal uygulamanızı vermek bir **adı**, seçin bir **kaynak grubu**ve benzeri.

    ![Mantıksal uygulama oluşturma](media/action-groups-logic-app/create-logic-app-dialog.png "mantıksal uygulama oluşturma")

4.  Seçin **Oluştur** mantıksal uygulamanızı oluşturmak için. Bir açılır ileti, mantıksal uygulama oluşturulduğunu gösterir. Seçin **başlatma kaynak** açmak için **Logic Apps Tasarımcısı'nda**.

5.  Tetikleyiciyi seçin: **Bir HTTP isteği alındığında**.

    ![Mantıksal uygulama Tetikleyicileri](media/action-groups-logic-app/logic-app-triggers.png "mantıksal uygulama Tetikleyicileri")

6.  Seçin **Düzenle** HTTP isteği tetikleyicisi değiştirmek için.

    ![HTTP isteği Tetikleyicileri](media/action-groups-logic-app/http-request-trigger-shape.png "HTTP isteği Tetikleyicileri")

7.  **Şema oluşturmak için örnek yük kullanma** öğesini seçin.

    ![Bir örnek yük kullanma](media/action-groups-logic-app/use-sample-payload-button.png "örnek yük kullanma")

8.  Kopyalayıp aşağıdaki örnek yük iletişim kutusuna yapıştırın:

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

9. **Mantıksal Uygulama Tasarımcısı** mantıksal uygulamaya gönderilen istek ayarlamalısınız hatırlatan bir açılır pencere görüntüler **Content-Type** başlığına **application/json**. Açılır penceresini kapatın. Azure İzleyici uyarı üstbilgisini ayarlar.

    ![Content-Type üst bilgisini ayarlayın](media/action-groups-logic-app/content-type-header.png "Content-Type üst bilgisini ayarlayın")

10. Seçin **+** **yeni adım** seçip **Eylem Ekle**.

    ![Eylem Ekle](media/action-groups-logic-app/add-action.png "Eylem Ekle")

11. İçin arama yapın ve Microsoft Teams Bağlayıcısı'nı seçin. Seçin **Microsoft Teams – ileti gönder** eylem.

    ![Microsoft Teams eylemleri](media/action-groups-logic-app/microsoft-teams-actions.png "Microsoft Teams eylemleri")

12. Microsoft Teams eylemi yapılandırın. **Logic Apps Tasarımcısı'nda** da Office 365 hesabınızda kimlik doğrulaması yapmanızı ister. Seçin **Takım Kimliği** ve **kanal kimliği** ileti göndermek için.

13. Statik metin ve başvurular bir bileşimini kullanarak ileti yapılandırma \<alanları\> dinamik içerik. İçine aşağıdaki metni kopyalayıp **ileti** alan:

    ```text
      Activity Log Alert: <eventSource>
      operationName: <operationName>
      status: <status>
      resourceId: <resourceId>
    ```

    Ardından arama ve değiştirme \<alanları\> dinamik içerik etiketlere aynı ada sahip.

    > [!NOTE]
    > Adlandırılmış dinamik iki alan vardır **durumu**. Bu alanların her ikisi de iletiye ekleyin. İçinde alan **activityLog** özellik paketini ve delete diğer alan. İmleci üzerine **durumu** alan aşağıdaki ekran görüntüsünde gösterildiği gibi tam bir alan başvuru görmek için:

    ![Microsoft Teams eylem: Bir ileti gönderin](media/action-groups-logic-app/teams-action-post-message.png "Microsoft Teams eylem: Bir ileti gönderin")

14. Üst kısmındaki **Logic Apps Tasarımcısı'nda**seçin **Kaydet** mantıksal uygulamanızı kaydetmek için.

15. Mevcut eylem grubunuzu açın ve mantıksal uygulamaya başvurmak için eylem ekleme. Var olan bir eylem grubu yoksa bkz [oluştur ve Eylem grupları Azure portalında yönetmek](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups) oluşturmak için. Yaptığınız değişiklikleri kaydetmeyi unutmayın.

    ![Eylem grubu](media/action-groups-logic-app/update-action-group.png "eylem grubu")

Bir uyarı eylem grubunuzu çağıran bir sonraki açışınızda, mantıksal uygulamanızın adı verilir.

## <a name="create-a-service-health-alert"></a>Hizmet durumu uyarısı oluşturma

Azure hizmet durumu girişleri etkinlik günlüğü bir parçasıdır. Uyarı oluşturma işlemi benzer [bir etkinlik günlüğü uyarısı oluşturma](#create-an-activity-log-alert-administrative), ancak bazı değişiklikler:

- 1 ile 7 arasındaki adımları aynıdır.
- Adım 8 için aşağıdaki örnek yük için HTTP isteği tetikleyicisi kullanın:

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
-  14 11 adımları için aşağıdaki işlemi kullanın:

   1. Seçin **+** **yeni adım** seçip **koşul Ekle**. Giriş verilerini aşağıdaki değerleri eşleştiğinde mantıksal uygulama yürütür. Bu nedenle aşağıdaki koşulları ayarlayın.  Sürüm değeri metin kutusuna girerken, dize ve bir sayısal tür değil değerlendirilir emin olmak için tırnak içine ("0.1.1") alın.  Sistem sayfasına geri dönün, ancak arka plandaki kod hala dize türü tutar, tırnak işaretleri göstermez.   
       - `schemaId == Microsoft.Insights/activityLogs`
       - `eventSource == ServiceHealth`
       - `version == "0.1.1"`

      !["Hizmet durumu yükü koşul"](media/action-groups-logic-app/service-health-payload-condition.png "hizmet durumu yükü koşulu")

   1. İçinde **true ise** koşulu için 13'te 11 adım'daki yönergeleri [bir etkinlik günlüğü uyarısı oluşturma](#create-an-activity-log-alert-administrative) Microsoft Teams eylem ekleme.

   1. İleti HTML ve dinamik içerik bir bileşimini kullanarak tanımlayın. Aşağıdaki içeriği kopyalayıp **ileti** alan. Değiştirin `[incidentType]`, `[trackingID]`, `[title]`, ve `[communication]` dinamik içerik etiketlere aynı ada sahip alanlar:

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

       !["Hizmet durumu koşulun sonrası eylemi"](media/action-groups-logic-app/service-health-true-condition-post-action.png "hizmet durumu koşulun sonrası eylemi")

   1. İçin **false ise** koşulu için kullanışlı bir iletiyi sağlayın:

       ```html
       <p><strong>Service Health Alert</strong></p>
       <p><b>Unrecognized alert schema</b></p>
       <p><a href="https://ms.portal.azure.com/#blade/Microsoft_Azure_Health/AzureHealthBrowseBlade/serviceIssues">For details, log in to the Azure Service Health dashboard.\</a></p>
       ```

       !["Hizmet durumu yanlış koşul sonrası eylemi"](media/action-groups-logic-app/service-health-false-condition-post-action.png "hizmet durumu yanlış koşul sonrası eylemi")

- 15\. adımına aynıdır. Mantıksal uygulamanızı kaydedin ve kendi eylem grubu için yönergeleri izleyin.

## <a name="create-a-metric-alert"></a>Ölçüm uyarısı oluşturma

Ölçüm uyarısı oluşturma işlemi benzer [bir etkinlik günlüğü uyarısı oluşturma](#create-an-activity-log-alert-administrative), ancak bazı değişiklikler:

- 1 ile 7 arasındaki adımları aynıdır.
- Adım 8 için aşağıdaki örnek yük için HTTP isteği tetikleyicisi kullanın:

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
- 14 11 adımları için aşağıdaki işlemi kullanın:

  1. Seçin **+** **yeni adım** seçip **koşul Ekle**. Mantıksal uygulama yalnızca giriş verileri bu değerleri aşağıdaki eşleştiğinde yürütür, böylece aşağıdaki koşulları ayarlayın. Ne zaman için tırnak ("2.0") içine alın ve metin kutusuna sürüm değeri girme, dize ve bir sayısal tür değil değerlendirilir emin olur.  Sistem sayfasına geri dönün, ancak arka plandaki kod hala dize türü tutar, tırnak işaretleri göstermez. 
     - `schemaId == AzureMonitorMetricAlert`
     - `version == "2.0"`
       
       !["Ölçüm uyarı yükü koşul"](media/action-groups-logic-app/metric-alert-payload-condition.png "ölçüm uyarı yükü koşulu")

  1. İçinde **doğruysa** koşul, ekleme bir **her** döngüsü ve Microsoft Teams eylem. İleti HTML ve dinamik içerik bir bileşimini kullanarak tanımlayın.

      !["Ölçüm uyarı koşulun sonrası eylemi"](media/action-groups-logic-app/metric-alert-true-condition-post-action.png "ölçüm uyarı koşulun sonrası eylemi")

  1. İçinde **false ise** koşulu için ölçüm uyarısı mantıksal uygulamanın beklentileri eşleşmiyor iletişim kurmak için bir Microsoft Teams eylem tanımlayın. JSON yükünü dahil et. Dosyasına nasıl başvurulacağı fark `triggerBody` dinamik içeriği `json()` ifade.

      !["Ölçüm uyarı false koşulu sonrası eylemi"](media/action-groups-logic-app/metric-alert-false-condition-post-action.png "ölçüm uyarı false koşulu sonrası eylemi")

- 15\. adımına aynıdır. Mantıksal uygulamanızı kaydedin ve kendi eylem grubu için yönergeleri izleyin.

## <a name="calling-other-applications-besides-microsoft-teams"></a>Microsoft Teams yanı sıra diğer uygulamalar
Logic Apps tetikleyici eylemlere uygulamaları ve veritabanlarının geniş bir aralıktaki izin farklı bağlayıcıları birçok vardır. Slack, SQL Server, Oracle, Salesforce, yalnızca bazı örnekler verilmiştir. Bağlayıcılar hakkında daha fazla bilgi için bkz. [Logic App bağlayıcıları](../../connectors/apis-list.md).  

## <a name="next-steps"></a>Sonraki adımlar
* Alma bir [Azure etkinlik günlüğü uyarılarına genel bakış](../../azure-monitor/platform/alerts-overview.md) ve uyarıları alma hakkında bilgi edinin.  
* Bilgi edinmek için nasıl [bir Azure hizmet durumu bildirimi gönderildiğinde uyarıları yapılandırma](../../azure-monitor/platform/alerts-activity-log-service-notifications.md).
* Daha fazla bilgi edinin [Eylem grupları](../../azure-monitor/platform/action-groups.md).

