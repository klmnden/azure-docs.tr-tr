---
title: Tetikleyici karmaşık eylemleri Azure izleme uyarıları ile ve Eylemler grupları
description: Azure uyarıları izleme işlemi için bir mantıksal uygulama eylemi oluşturmayı öğrenin.
author: dkamstra
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/30/2018
ms.author: dukek
ms.component: alerts
ms.openlocfilehash: eafb2bcf0175190748c9dd020051cbebfcaee1fd
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35263893"
---
# <a name="create-a-logic-app-action"></a>Bir mantıksal uygulama eylem oluşturun
## <a name="overview"></a>Genel Bakış ##
Azure İzleyici uyarıyı tetikleyen, çağıran bir [eylem grubu](monitoring-action-groups.md). Eylem gruplar uyarının kişilere bildirmek ve hatta düzeltmek için bir veya daha fazla eylemleri tetiklemek sağlar.

Bu makalede, ayarlama ve bir uyarı oluşturulduğunda bir konuşma Microsoft Teams oluşturmak için bir mantıksal uygulama tetiklemek gösterilmektedir.

Genel süreç şöyledir:

-   İlgili uyarı türü için mantıksal uygulama oluşturma

-   İlgili uyarı türüne ilişkin şema mantığı uygulamaya alma

-   Mantıksal uygulama davranışını tanımlayın

-   Bir Azure eylem grubuna mantığı uygulamanın HTTP uç noktası kopyalayın

İşlemi farklı bir eylem gerçekleştirmek için bu mantıksal uygulama istiyorsanız benzer.

## <a name="create-an-activity-log-alert--administrative"></a>Yönetim etkinlik günlüğü uyarı – oluşturma

1.  Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** düğmesine tıklayın.

2.  **Mantıksal Uygulama**'yı arayıp bulun ve seçin. **Oluştur** düğmesine tıklayın.

3.  Mantıksal uygulamanızı adlandırın, vb. bir kaynak grubu seçin.

    ![Oluşturma mantığı uygulama iletişim](media/monitoring-action-groups/create-logic-app-dialog.png "oluşturma mantığı uygulama iletişim")

4.  Mantıksal uygulama oluşturmak için Oluştur düğmesine tıklayın. Mantıksal uygulama oluştururken açılan pencere görüntülenir. Logic Apps Tasarımcısı'nı açmak için Başlat kaynak düğmesini tıklatın.

5.  Tetikleyici seçin **zaman bir HTTP isteği alındığında**.

    ![Mantığını uygulaması Tetikleyicileri](media/monitoring-action-groups/logic-app-triggers.png "mantığını uygulaması Tetikleyicileri")

6.  Seçin **Düzenle** HTTP isteği tetikleyici değiştirmek için

    ![HTTP isteği tetikleyici şekli](media/monitoring-action-groups/http-request-trigger-shape.png "HTTP isteği tetikleyici şekli")

7.  **Şema oluşturmak için örnek yük kullanma** öğesini seçin.

    ![Kullanım örneği yükü düğmesi](media/monitoring-action-groups/use-sample-payload-button.png "kullanım örnek yükü düğmesi")

8.  Kopyalayın ve aşağıdaki örnek şeması iletişim kutusuna yapıştırın.

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

9. Mantıksal Uygulama Tasarımcısı'nı mantığı uygulamaya gönderilen bu isteği Content-Type üstbilgisi application/json değerine ayarlamanız gerekir anımsatma not görüntüler. Devam etmek ve iletişim kutusunu kapatın. Azure İzleyicisi uyarısı doğru bunu.

    ![Content-Type üstbilgisi](media/monitoring-action-groups/content-type-header.png "Content-Type üstbilgisi")

10. Seçin **+ yeni adım** ve ardından **Eylem Ekle**.

    ![Eylem Ekle](media/monitoring-action-groups/add-action.png "Eylem Ekle")

11. Arayın ve Microsoft Teams Bağlayıcısı'nı seçin. Seçin **Microsoft Teams – posta iletisi** eylem.

    ![Microsoft Teams Eylemler](media/monitoring-action-groups/microsoft-teams-actions.png "Microsoft Teams Eylemler")

12. Microsoft Teams eylemi yapılandırın. Logic Apps Tasarımcısı Office365 hesabınızın kimlik doğrulaması yapmanızı ister. Seçin **Takım Kimliği** ve **kanal kimliği** ileti göndermek için.

13. Yapılandırma **ileti** statik metin ve başvuru birleşimini kullanarak \<alanları\> dinamik içerik. Kesme ve mesaj alanına aşağıdaki metni yapıştırın.

    ```text
      Activity Log Alert: <eventSource>
      operationName: <operationName>
      status: <status>
      resourceId: <resourceId>
    ```

    İçin arama ve değiştirme \<alanları\> dinamik içerik etiketleri, aynı ada sahip.

    **[NOT!]**  Adlı iki dinamik alan **durum**. Her iki durum alanları iletiye ekleyin. ActivityLog özellik paketi birinde kullanın ve diğer silin. Üzerinden farenizi getirirseniz **durum** alan ekran görüntüsünde gösterildiği gibi tam alan başvurusu görürsünüz

    ![Eylem - posta iletisi ekipleri](media/monitoring-action-groups/teams-action-post-message.png "takımlar eylemi - posta iletisi")

14. Tasarımcı üstündeki Kaydet düğmesine tıklayarak mantıksal uygulamanızı kaydetme

15. Genişletmek için neden HTTP isteği şekli tıklatın. HTTP POST URL'sini kopyalayın.

    ![HTTP POST URL](media/monitoring-action-groups/http-post-url.png "HTTP POST URL'Sİ")

16. Varolan eylem grubunuzun açın ve mantıksal uygulama başvurmak için bir eylem ekleyin. Var olan bir eylem grubunu sahip değilseniz, bkz: <https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-action-groups> oluşturmak için. Değişikliklerinizi kaydetmek unutmayın.

    ![Güncelleştirme eylem grubu](media/monitoring-action-groups/update-action-group.png "güncelleştirme eylem grubu")

Bir uyarı eylem grubunuzun çağırır sonraki açışınızda, mantıksal uygulamanızı adı verilir.

## <a name="create-a-service-health-alert"></a>Bir hizmet Sistem Durumu Uyarısı oluştur

İşlemi aşağıdaki değişikliklerle benzer şekilde hizmet sistem durumu girişleri etkinlik günlüğü parçasıdır

1.  1 ile 7 arasındaki adımları aynıdır.
2.  Aşağıdaki örnek şeması 8. adımda HTTP tetikleyicisi için kullanın.

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

3.  Adım 9-10 aynı değildir.
4.  Adım 11'aşağıdaki işlemi kullanın.
5.  Tıklayın **+ yeni adım** düğmesini tıklatın ve seçin **bir koşul eklemek**. Giriş verisi bu değerleri eşleştiğinde mantıksal uygulama yalnızca çalıştığından emin olmak için aşağıdaki koşulları ayarlayın
    - schemaId Microsoft.Insights/activityLogs ==
    - eventSource ServiceHealth ==
    - Sürüm 0.1.1 ==

    !["Hizmet durumu yükü koşul"](media/monitoring-action-groups/service-health-payload-condition.png "hizmet sistem durumu yükü koşulu")

6. İçinde **true ise** koşul, önceki örnekten 11-13 arası adımları kullanarak Microsoft Teams eylemi ekleyin.

7. HTML ve [dinamik içerik] birleşimini kullanarak ileti tanımlayın. Kopyalayıp altındaki içerik mesaj alanına yapıştırın. Değiştirme [incidentType], [Trackingıd], [başlık] ve [iletişim] alanları aynı ada sahip dinamik içerik etiketlerle

    ```html
    <p>
    <b>Alert Type:</b>&nbsp;<strong>[incidentType]</strong>&nbsp;
    <strong>Tracking ID:</strong>&nbsp;[trackingId]&nbsp;
    <strong>Title:</strong>&nbsp;[title]</p>
    <p>
    <a href="https://ms.portal.azure.com/#blade/Microsoft_Azure_Health/AzureHealthBrowseBlade/serviceIssues">For details log into the Azure Service Health dashboard</a>
    </p>
    <p>[communication]</p>
    ```

    !["Hizmet durumu koşulun sonrası eylemi"](media/monitoring-action-groups/service-health-true-condition-post-action.png "hizmet sistem durumu koşulun sonrası eylemi")

8. İçin **false ise** koşul yararlı bir iletiyi sağlayın

    ```html
    <p><strong>Service Health Alert</strong></p>
    <p><b>Unrecognized alert schema</b></p>
    <p><a href="https://ms.portal.azure.com/#blade/Microsoft_Azure_Health/AzureHealthBrowseBlade/serviceIssues">For details log into the Azure Service Health dashboard\</a></p>
    ```

    !["Hizmet durumu yanlış koşul sonrası eylemi"](media/monitoring-action-groups/service-health-false-condition-post-action.png "hizmet durumu yanlış koşul sonrası eylemi")

9.  Mantıksal uygulamanızı kaydetmek ve eylem grubunuzun güncelleştirmek için önceki örnekte 15 – 16 adımlarını izleyin

## <a name="metric-alert"></a>Ölçüm Uyarısı

1.  1 ile 7 arasındaki adımları ilk örnekle aynıdır
2.  Aşağıdaki örnek şeması 8. adımda HTTP tetikleyicisi için kullanın.

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

3.  Adım 9-10 aynı değildir.
4.  Adım 11'aşağıdaki işlemi kullanın.
5.  Tıklayın **+ yeni adım** düğmesini tıklatın ve seçin **bir koşul eklemek**. Giriş verisi bu değerleri eşleştiğinde mantıksal uygulama yalnızca çalıştığından emin olmak için aşağıdaki koşulları ayarlayın

    - schemaId AzureMonitorMetricAlert ==
    - sürüm 2.0 ==

    !["Ölçüm uyarı yükü koşul"](media/monitoring-action-groups/metric-alert-payload-condition.png "ölçüm uyarı yükü koşulu")

6.  İçinde **true ise** eklenecektir koşul bir **her** şekli ve Microsoft Teams eylem ve HTML ve [dinamik içerik] birleşimini kullanarak ileti tanımlayın

    !["Ölçüm uyarı koşulun sonrası eylemi"](media/monitoring-action-groups/metric-alert-true-condition-post-action.png "ölçüm uyarı koşulun sonrası eylemi")

7.  İçinde **false ise** şekil, ölçüm uyarı değil mantığı uygulamanın beklentilerini eşleşen ve JSON yükünü dahil et, iletişim kurar bir Microsoft Teams eylemi tanımlayın. Biz json() ifade triggerBody dinamik içeriği nasıl başvuru unutmayın.

    !["Ölçüm uyarı yanlış koşulu sonrası eylemi"](media/monitoring-action-groups/metric-alert-false-condition-post-action.png "ölçüm uyarı yanlış koşulu sonrası eylemi")

8.  Mantıksal uygulamanızı kaydetmek ve eylem grubunuzun güncelleştirmek için ilk Örneğin 15 – 16 adımları izleyin

## <a name="next-steps"></a>Sonraki adımlar ##
* Alma bir [etkinlik günlüğü uyarıları genel bakış](monitoring-overview-alerts.md)ve uyarıların nasıl alınacağını öğrenin.  
* Bilgi edinmek için nasıl [hizmeti sistem durumu bildirimi gönderilen her uyarıları yapılandırmak](monitoring-activity-log-alerts-on-service-notifications.md).
* Daha fazla bilgi edinmek [Eylem grupları](monitoring-action-groups.md)