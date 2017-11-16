---
title: "Azure hizmet durumu uyarıları ServiceNow yapılandırma | Microsoft Docs"
description: "ServiceNow Örneğiniz için hizmet sistem durumu olayları hakkında Kişiselleştirilmiş bildirimler alın."
author: shawntabrizi
services: service-health
documentationcenter: service-health
ms.assetid: 
ms.service: service-health
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: shtabriz
ms.openlocfilehash: 625718ab82443c897d1b15c2eac51dea3d0dfeb4
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="configure-service-health-alerts-with-servicenow"></a>ServiceNow hizmet sistem durumu uyarılarını yapılandırma

Bu makalede bir Web kancası kullanarak Azure hizmet durumu uyarıları ServiceNow ile tümleştirmek nasıl gösterir. Azure hizmet sorunlar etkiler açtığınızda ServiceNow örneği ile Web kancası tümleştirme kurduktan sonra uyarılar, mevcut bir bildirim altyapınız üzerinden olursunuz. Bir Azure hizmet durumu uyarı harekete her zaman, ServiceNow'ın komut dosyasıyla REST API'si aracılığıyla bir Web kancası çağırır.

## <a name="creating-a-scripted-rest-api-in-servicenow"></a>ServiceNow içinde komut dosyalı bir REST API'si oluşturma
1.  Kaydolup ve içine imzalı olduğundan emin olun, [ServiceNow](https://www.servicenow.com/) hesabı.

2.  Gidin **sistem Web Hizmetleri** ServiceNow ve select bölümüne **Script REST API'leri**.

    ![Servicenow'ı "Web hizmeti komut dosyası" bölümünde](./media/webhook-alerts/servicenow-sws-section.png)

3.  Seçin **yeni** yeni bir komut dosyasıyla REST hizmeti oluşturmak için.
 
    ![ServiceNow içindeki "Yeni Script REST API" düğmesi](./media/webhook-alerts/servicenow-new-button.png)

4.  Ekleme bir **adı** REST API ve kümesi **API kimliği** için `azureservicehealth`.

5.  Seçin **gönderme**.

    !["REST API ayarları" ServiceNow](./media/webhook-alerts/servicenow-restapi-settings.png)

6.  Oluşturduğunuz, REST API seçin ve altında **kaynakları** sekmesini seçin **yeni**.

    !["Kaynak sekmesinde" ServiceNow](./media/webhook-alerts/servicenow-resources-tab.png)

7.  **Ad** yeni kaynak `event` değiştirip **HTTP yöntemini** için `POST`.

8.  İçinde **betik** bölümünde, aşağıdaki JavaScript kodu ekleyin:

    >[!NOTE]
    >Güncelleştirmek gereken `<secret>`,`<group>`, ve `<email>` komut değeri.
    >* `<secret>`bir GUID gibi rastgele bir dize olmalıdır
    >* `<group>`olaya atamak istediğiniz ServiceNow grubu olmalıdır
    >* `<email>`(isteğe bağlı) olaya atamak istediğiniz belirli kişi olması gerekir
    >

    ```javascript
    (function process( /*RESTAPIRequest*/ request, /*RESTAPIResponse*/ response) {
        var apiKey = request.queryParams['apiKey'];
        var secret = '<secret>';
        if (apiKey == secret) {
            var event = request.body.data;
            var responseBody = {};
            if (event.data.context.activityLog.operationName == 'Microsoft.ServiceHealth/incident/action') {
                var inc = new GlideRecord('incident');
                var incidentExists = false;
                inc.addQuery('number', event.data.context.activityLog.properties.trackingId);
                inc.query();
                if (inc.hasNext()) {
                    incidentExists = true;
                    inc.next();
                } else {
                    inc.initialize();
                }
                var short_description = "Azure Service Health";
                if (event.data.context.activityLog.properties.incidentType == "Incident") {
                    short_description += " - Service Issue - ";
                } else if (event.data.context.activityLog.properties.incidentType == "Maintenance") {
                    short_description += " - Planned Maintenance - ";
                } else if (event.data.context.activityLog.properties.incidentType == "Information" || event.data.context.activityLog.properties.incidentType == "ActionRequired") {
                    short_description += " - Health Advisory - ";
                }
                short_description += event.data.context.activityLog.properties.title;
                inc.short_description = short_description;
                inc.description = event.data.context.activityLog.properties.communication;
                inc.work_notes = "Impacted subscription: " + event.data.context.activityLog.subscriptionId;
                if (incidentExists) {
                    if (event.data.context.activityLog.properties.stage == 'Active') {
                        inc.state = 2;
                    } else if (event.data.context.activityLog.properties.stage == 'Resolved') {
                        inc.state = 6;
                    } else if (event.data.context.activityLog.properties.stage == 'Closed') {
                        inc.state = 7;
                    }
                    inc.update();
                    responseBody.message = "Incident updated.";
                } else {
                    inc.number = event.data.context.activityLog.properties.trackingId;
                    inc.state = 1;
                    inc.impact = 2;
                    inc.urgency = 2;
                    inc.priority = 2;
                    inc.assigned_to = '<email>';
                    inc.assignment_group.setDisplayValue('<group>');
                    var subscriptionId = event.data.context.activityLog.subscriptionId;
                    var comments = "Azure portal Link: https://app.azure.com/h";
                    comments += "/" + event.data.context.activityLog.properties.trackingId;
                    comments += "/" + subscriptionId.substring(0, 3) + subscriptionId.slice(-3);
                    var impactedServices = JSON.parse(event.data.context.activityLog.properties.impactedServices);
                    var impactedServicesFormatted = "";
                    for (var i = 0; i < impactedServices.length; i++) {
                        impactedServicesFormatted += impactedServices[i].ServiceName + ": ";
                        for (var j = 0; j < impactedServices[i].ImpactedRegions.length; j++) {
                            if (j != 0) {
                                impactedServicesFormatted += ", ";
                            }
                            impactedServicesFormatted += impactedServices[i].ImpactedRegions[j].RegionName;
                        }

                        impactedServicesFormatted += "\n";

                    }
                    comments += "\n\nImpacted Services:\n" + impactedServicesFormatted;
                    inc.comments = comments;
                    inc.insert();
                    responseBody.message = "Incident created.";
                }
            } else {
                responseBody.message = "Hello from the other side!";
            }
            response.setBody(responseBody);
        } else {
            var unauthorized = new sn_ws_err.ServiceError();
            unauthorized.setStatus(401);
            unauthorized.setMessage('Invalid apiKey');
            response.setError(unauthorized);
        }
    })(request, response);
    ```

9.  Güvenlik sekmesinde işaretini **kimlik doğrulaması gerektiren** seçip **gönderme**. `<secret>` , Kümesi bu API yerine korur.

    ![ServiceNow "Kimlik doğrulaması gerektirir" onay kutusu](./media/webhook-alerts/servicenow-resource-settings.png)

10.  Komut dosyası REST API'leri kısmına geri bulmanız gerekir **temel API yolu** yeni REST API için:

     !["Temel API yolu" içinde ServiceNow](./media/webhook-alerts/servicenow-base-api-path.png)

11.  Tam tümleştirme URL'nizi şuna benzer:
        
         https://<yourInstanceName>.service-now.com/<baseApiPath>?apiKey=<secret>


## <a name="create-an-alert-using-servicenow-in-the-azure-portal"></a>Servicenow'ı kullanarak Azure portalında bir uyarı oluşturabilir.
### <a name="for-a-new-action-group"></a>İçin yeni bir eylem grubu:
1. 1 ile 8 arasındaki adımları [bu makalede](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md) bir uyarı ile yeni bir eylem grubu oluşturmak için.

2. Listesinde tanımlamak **Eylemler**:

    a. **Eylem türü:** *Web kancası*

    b. **Ayrıntılar:** ServiceNow **tümleştirme URL** , daha önce kaydedildi.

    c. **Ad:** Web kancası'nın adı, diğer ad veya tanımlayıcı.

3. Seçin **kaydetmek** uyarı oluşturmak için yapıldığında.

### <a name="for-an-existing-action-group"></a>Var olan bir eylem grubu için:
1. İçinde [Azure portal](https://portal.azure.com/)seçin **İzleyici**.

2. İçinde **ayarları** bölümünde, select **Eylem grupları**.

3. Bulmak ve düzenlemek istediğiniz eylem grubunu seçin.

4. Listesine ekleme **Eylemler**:

    a. **Eylem türü:** *Web kancası*

    b. **Ayrıntılar:** ServiceNow **tümleştirme URL** , daha önce kaydedildi.

    c. **Ad:** Web kancası'nın adı, diğer ad veya tanımlayıcı.

5. Seçin **kaydetmek** eylem grubunu güncelleştirmek için yapıldığında.

## <a name="testing-your-webhook-integration-via-an-http-post-request"></a>Bir HTTP POST isteği üzerinden, Web kancası tümleştirme sınaması
1. Göndermek istediğiniz hizmet durumu yükü oluşturun. Bir örnek hizmet durumu Web kancası yükü sırasında bulduğunuz [Azure etkinlik için Web kancası oturum uyarıları](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md).

2. Bir HTTP POST isteği gibi oluşturun:

    ```
    POST        https://<yourInstanceName>.service-now.com/<baseApiPath>?apiKey=<secret>

    HEADERS     Content-Type: application/json

    BODY        <Service Health payload>
    ```
3. Alması gereken bir `200 OK` yanıt iletisi "olay oluşturuldu."

4. Git [ServiceNow](https://www.servicenow.com/) tümleştirmenize başarıyla ayarlanmıştır onaylamak için.

## <a name="next-steps"></a>Sonraki adımlar
- Bilgi edinmek için nasıl [varolan sorunu yönetim sistemleri için Web kancası bildirimleri yapılandırmak](service-health-alert-webhook-guide.md).
- Gözden geçirme [etkinlik günlüğü uyarı Web kancası şeması](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md). 
- Hakkında bilgi edinin [hizmet durumu bildirimlerine](../monitoring-and-diagnostics/monitoring-service-notifications.md).
- Daha fazla bilgi edinmek [Eylem grupları](../monitoring-and-diagnostics/monitoring-action-groups.md).