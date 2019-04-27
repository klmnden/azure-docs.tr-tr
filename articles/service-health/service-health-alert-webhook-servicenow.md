---
title: Azure hizmet durumu uyarıları ServiceNow ile yapılandırma | Microsoft Docs
description: ServiceNow Örneğinize hizmet durumu olayları hakkında Kişiselleştirilmiş bildirimler alın.
author: stephbaron
ms.author: stbaron
ms.topic: article
ms.service: service-health
ms.date: 11/14/2017
ms.openlocfilehash: f17215a5695128bf2ea507efa0c12fdbba9467d2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60620954"
---
# <a name="configure-service-health-alerts-with-servicenow"></a>ServiceNow ile hizmet sistem durumu uyarılarını yapılandırma

Bu makalede, Azure hizmet durumu uyarıları ServiceNow ile tümleştirmek Web kancası kullanarak gösterilmektedir. Azure hizmet sorunlardan etkilendiğiniz durumlar Web kancası Tümleştirmesi ile ServiceNow Örneğinize kurduktan sonra uyarı mevcut bildirim altyapınızın alın. Bir Azure hizmet durumu uyarısı tetikler her seferinde, ServiceNow'ın Script REST API aracılığıyla bir Web kancası çağırır.

## <a name="creating-a-scripted-rest-api-in-servicenow"></a>Servicenow'ı içinde simgeleştirilmiş bir REST API'si oluşturma
1.  Kaydolup ve oturum açmış emin olun, [ServiceNow](https://www.servicenow.com/) hesabı.

1.  Gidin **System Web Hizmetleri** servicenow'ı seçip alt bölümünde **Script REST API'leri**.

    ![Servicenow'ı "Web hizmeti komut dosyası" bölümünde](./media/webhook-alerts/servicenow-sws-section.png)

1.  Seçin **yeni** yeni bir komut dosyası REST hizmeti oluşturursunuz.
 
    ![ServiceNow "Yeni komut dosyası REST API" düğmesi](./media/webhook-alerts/servicenow-new-button.png)

1.  Ekleme bir **adı** REST API ve kümesi **API kimliği** için `azureservicehealth`.

1.  Seçin **gönderme**.

    !["REST API ayarları" ServiceNow](./media/webhook-alerts/servicenow-restapi-settings.png)

1.  Oluşturduğunuz REST API'yi seçin ve altındaki **kaynakları** sekmesinde **yeni**.

    !["Kaynak sekmesinde" ServiceNow](./media/webhook-alerts/servicenow-resources-tab.png)

1.  **Adı** yeni kaynağınızı `event` değiştirip **HTTP yöntemini** için `POST`.

1.  İçinde **betik** bölümünde, aşağıdaki JavaScript kodunu ekleyin:

    >[!NOTE]
    >Güncelleştirmeye gerek duyduğunuz `<secret>`,`<group>`, ve `<email>` aşağıdaki betiği değeri.
    >* `<secret>` gibi bir GUID rastgele bir dize olmalıdır
    >* `<group>` olayla atamak istediğiniz ServiceNow grubu olmalıdır
    >* `<email>` (isteğe bağlı) olayla atamak istediğiniz belirli bir kişi olması gerekir
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
                } else if (event.data.context.activityLog.properties.incidentType == "Informational" || event.data.context.activityLog.properties.incidentType == "ActionRequired") {
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

1.  Güvenlik sekmesinde'seçeneğinin işaretini kaldırın **kimlik doğrulaması gerektiren** seçip **Gönder**. `<secret>` , Kümesi bu API yerine korur.

    ![ServiceNow "Kimlik doğrulaması gerektirir" onay kutusu](./media/webhook-alerts/servicenow-resource-settings.png)

1.  Komut dosyası REST API'leri kısmına geri bulmanız gerekir **temel API yolu** yeni REST API'niz için:

     !["Temel API yol" ServiceNow](./media/webhook-alerts/servicenow-base-api-path.png)

1.  Tam tümleştirme URL'niz şuna benzer:
        
         https://<yourInstanceName>.service-now.com/<baseApiPath>?apiKey=<secret>


## <a name="create-an-alert-using-servicenow-in-the-azure-portal"></a>Servicenow'ı kullanarak Azure portalında uyarı oluşturma
### <a name="for-a-new-action-group"></a>Yeni bir eylem grubu için:
1. 1 ile 8 arasındaki adımları [bu makalede](../azure-monitor/platform/alerts-activity-log-service-notifications.md) bir uyarı ile yeni bir eylem grubu oluşturmak için.

1. Listesinde tanımlamak **Eylemler**:

    a. **Eylem türü:** *Web kancası*

    b. **Ayrıntılar:** Servicenow'ı **tümleştirme URL'sini** , daha önce kaydedildi.

    c. **Adı:** Web kancası'nın adı, diğer adı veya tanımlayıcısı.

1. Seçin **Kaydet** uyarı oluşturma işlemi tamamlandığında.

### <a name="for-an-existing-action-group"></a>Var olan bir eylem grubu için:
1. İçinde [Azure portalında](https://portal.azure.com/)seçin **İzleyici**.

1. İçinde **ayarları** bölümünden **Eylem grupları**.

1. Bulmak ve düzenlemek istediğiniz eylem grubu seçin.

1. Listesine ekleme **Eylemler**:

    a. **Eylem türü:** *Web kancası*

    b. **Ayrıntılar:** Servicenow'ı **tümleştirme URL'sini** , daha önce kaydedildi.

    c. **Adı:** Web kancası'nın adı, diğer adı veya tanımlayıcısı.

1. Seçin **Kaydet** eylem grubunu güncelleştirmeye işiniz bittiğinde.

## <a name="testing-your-webhook-integration-via-an-http-post-request"></a>Bir HTTP POST isteği üzerinden, Web kancası tümleştirme testi
1. Göndermek istediğiniz hizmet sistem durumu yükü oluşturun. Bir örnek hizmet sistem durumu Web kancası yükü konumunda bulabilirsiniz [günlük uyarıları Azure etkinlik için Web kancaları](../azure-monitor/platform/activity-log-alerts-webhook.md).

1. Bir HTTP POST isteği şu şekilde oluşturun:

    ```
    POST        https://<yourInstanceName>.service-now.com/<baseApiPath>?apiKey=<secret>

    HEADERS     Content-Type: application/json

    BODY        <service health payload>
    ```
1. Alması gereken bir `200 OK` yanıt iletisi "olay oluşturuldu."

1. Git [ServiceNow](https://www.servicenow.com/) tümleştirmenizi başarılı bir şekilde ayarlandığını doğrulamak için.

## <a name="next-steps"></a>Sonraki adımlar
- Bilgi edinmek için nasıl [mevcut sorun yönetim sistemleri için Web kancası bildirimleri yapılandırma](service-health-alert-webhook-guide.md).
- Gözden geçirme [etkinlik günlüğü uyarısı Web kancası şeması](../azure-monitor/platform/activity-log-alerts-webhook.md). 
- Hakkında bilgi edinin [hizmet durumu bildirimlerini](../azure-monitor/platform/service-notifications.md).
- Daha fazla bilgi edinin [Eylem grupları](../azure-monitor/platform/action-groups.md).