---
title: Durumu denetleme, günlük kaydını ayarlama ve Uyarıları - Azure Logic Apps alın | Microsoft Docs
description: Durum İzleme, tanılama verilerini günlüğe kaydetmek ve Azure Logic Apps için uyarılar ayarlayın
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: 5c1b1e15-3b6c-49dc-98a6-bdbe7cb75339
ms.date: 07/21/2017
ms.openlocfilehash: 80776f9284752e8554486cb458096ccc9319949e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61325291"
---
# <a name="monitor-status-set-up-diagnostics-logging-and-turn-on-alerts-for-azure-logic-apps"></a>Azure Logic Apps için uyarılarını Aç durumunu izleme ve tanılama günlük kaydını ayarlama

Çalıştırdıktan sonra [oluşturma ve bir mantıksal uygulama çalıştırma](../logic-apps/quickstart-create-first-logic-app-workflow.md), çalıştırma geçmişi, tetikleyici geçmişi, durumunu ve performansını denetleyebilirsiniz. Gerçek zamanlı Olay izleme ve daha zengin hata ayıklama için ayarlanmış [tanılama günlüğünü](#azure-diagnostics) mantıksal uygulamanız için. Bu şekilde, aşağıdakileri yapabilirsiniz [bulma ve görüntüleme olayları](#find-events)olayları tetiklemeyi, çalışma olayları ve eylem olayları gibi. Bu da kullanabilirsiniz [diğer hizmetlerle veri tanılama](#extend-diagnostic-data), Azure depolama ve Azure Event Hubs gibi. 

Hataları veya diğer olası sorunlar hakkında bildirim almak için ayarlama [uyarılar](#add-azure-alerts). Örneğin, "bir saat içinde beşten fazla çalıştırma başarısız olduğunda." algılarsa bir uyarı oluşturabilirsiniz. İzleme, izleme ve program aracılığıyla kullanarak günlüğe kaydetme ayarlayabilirsiniz [Azure Tanılama Olay ayarları ve özellikleri](#diagnostic-event-properties).

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="view-runs-and-trigger-history-for-your-logic-app"></a>Çalıştırmalarını görüntüle ve mantıksal uygulamanızın tetikleyici geçmişi

1. Mantıksal uygulamanızda bulmak için [Azure portalında](https://portal.azure.com), ana Azure menüsünde **tüm hizmetleri**. Arama kutusuna "logic apps" ve seçin **Logic apps**.

   ![Mantıksal uygulamanızı bulun](./media/logic-apps-monitor-your-logic-apps/find-your-logic-app.png)

   Azure portal, Azure aboneliğinizle ilişkili olan tüm mantıksal uygulamalar gösterir. 

2. Mantıksal uygulamanızı seçin ve ardından **genel bakış**.

   Azure portal, çalıştırma geçmişi ve tetikleyici geçmişi mantıksal uygulamanızın gösterir. Örneğin:

   ![Mantıksal uygulama çalıştırma geçmişi ve tetikleyici geçmişi](media/logic-apps-monitor-your-logic-apps/overview.png)

   * **Çalıştırma geçmişi** mantıksal uygulamanız için tüm çalıştırmalar gösterir. 
   * **Tetikleme geçmişi** mantıksal uygulamanız için tüm tetikleyici etkinliğini gösterir.

   Durum açıklamaları için bkz. [mantıksal uygulamanızla ilgili sorun giderme](../logic-apps/logic-apps-diagnosing-failures.md).

   > [!TIP]
   > Araç çubuğunda beklediğiniz verileri bulamazsanız seçin **Yenile**.

3. Belirli bir çalıştırma adımları altında görüntülemek için **çalıştırma geçmişi**, çalıştırılan seçin. 

   İzleme görünümü her adım, çalışan gösterir. Örneğin:

   ![Belirli bir çalıştırma için Eylemler](media/logic-apps-monitor-your-logic-apps/monitor-view-updated.png)

4. Çalıştırma hakkında daha fazla ayrıntı almak için seçtiğiniz **çalıştırma ayrıntıları**. Adımlar, durumu, giriş ve çıkışları çalıştırma için bu bilgileri özetler. 

   ![Seçin "Çalıştırma Ayrıntıları"](media/logic-apps-monitor-your-logic-apps/run-details.png)

   Örneğin, çalıştırmanın alabilirsiniz **bağıntı kimliği**, kullandığınızda, ihtiyacınız olabilecek [Logic Apps için REST API](https://docs.microsoft.com/rest/api/logic).

5. Belirli bir adım hakkında daha fazla bilgi edinmek için bu adımı seçin. Artık girişler, çıkışlar ve bu adım için gerçekleşen hataları gibi ayrıntılarını da gözden geçirebilirsiniz. Örneğin:

   ![Adımı ayrıntıları](media/logic-apps-monitor-your-logic-apps/monitor-view-details.png)
   
   > [!NOTE]
   > Tüm çalışma zamanı ayrıntılarını ve olayları, Logic Apps hizmetinde şifrelenir. Yalnızca bir kullanıcı verileri görüntülemek için istediğinde şifresi çözülür. Ayrıca bu olaylar ile erişimi denetleyebilirsiniz [Azure rol tabanlı Access Control (RBAC)](../role-based-access-control/overview.md).

6. Belirli bir olayın hakkında bilgi almak için geri gidin **genel bakış** bölmesi. Altında **tetikleyicisi geçmişi**, tetikleyici olayı seçin. Ayrıntılar gibi girişler ve çıkışlar, örneğin inceleyebilirsiniz:

   ![Tetikleyici olay çıkış ayrıntıları](media/logic-apps-monitor-your-logic-apps/trigger-details.png)

<a name="azure-diagnostics"></a>

## <a name="turn-on-diagnostics-logging-for-your-logic-app"></a>Tanılama mantıksal uygulamanız için oturum açın

Daha zengin çalışma zamanı ayrıntılarını ve olayları ile hata ayıklama için ile günlüğe kaydetme tanılama ayarlayabilirsiniz [Azure İzleyicisi](../log-analytics/log-analytics-overview.md). Azure İzleyici, bulut izler ve şirket içi Ortamlarınızdaki kullanılabilirliği ve performansı sürdürmek amacıyla bir Azure hizmetidir. 

Başlamadan önce Log Analytics çalışma alanına sahip olmanız gerekir. Bilgi [bir Log Analytics çalışma alanı oluşturma](../azure-monitor/learn/quick-create-workspace.md).

1. İçinde [Azure portalında](https://portal.azure.com)bulup mantıksal uygulamanızı seçin. 

2. Mantıksal uygulama dikey penceresinde menüsünde altında **izleme**, seçin **tanılama** > **tanılama ayarları**.

   ![İzleme, yönetim tanı, tanılama ayarları için Git](media/logic-apps-monitor-your-logic-apps/logic-app-diagnostics.png)

3. Altında **tanılama ayarları**, seçin **üzerinde**.

   ![Tanılama günlüklerini açın](media/logic-apps-monitor-your-logic-apps/turn-on-diagnostics-logic-app.png)

4. Artık gösterildiği günlüğe kaydetme için Log Analytics çalışma alanı ve olay kategorisi seçin:

   1. Seçin **Log Analytics'e gönderme**. 
   2. Altında **Log Analytics**, seçin **yapılandırma**. 
   3. Altında **OMS çalışma alanları**, günlüğe kaydetme için kullanılacak çalışma alanını seçin.
      > [!NOTE]
      > OMS çalışma alanları artık Log Analytics çalışma alanları olarak adlandırılır.
   4. Altında **günlük**seçin **WorkflowRuntime** kategorisi.
   5. Ölçüm aralığını seçin.
   6. İşiniz bittiğinde **Kaydet**’i seçin.

   ![Log Analytics çalışma alanı ve veri günlüğü için seçin](media/logic-apps-monitor-your-logic-apps/send-diagnostics-data-log-analytics-workspace.png)

Artık, olayları ve eylem olayları çalıştırma tetikleyicisi olaylar için olayları ve diğer verileri bulabilirsiniz.

<a name="find-events"></a>

## <a name="find-events-and-data-for-your-logic-app"></a>Mantıksal uygulamanız için olayları ve verileri bulma

Bulma ve mantıksal uygulamanızda olayları görüntüleme gibi olaylar, olaylar, çalıştırmak ve eylem olaylar tetikleyin, aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com), seçin **tüm hizmetleri**. "Log analytics için" için arama yapın ve ardından **Log Analytics** burada gösterildiği gibi:

   !["Log Analytics" seçin](media/logic-apps-monitor-your-logic-apps/browseloganalytics.png)

2. Altında **Log Analytics**bulup Log Analytics çalışma alanınızı seçin. 

   ![Log Analytics çalışma alanınızı seçin](media/logic-apps-monitor-your-logic-apps/selectla.png)

3. Altında **Yönetim**, seçin **günlük araması**.

   !["Ara" öğesini seçin](media/logic-apps-monitor-your-logic-apps/log-search.png)

4. Arama kutusuna bulmak ve basın istediğiniz bir alanı belirtmek **Enter**. Yazmaya başladığınızda, olası eşleşmeler ve kullanabileceğiniz işlemleri görürsünüz. 

   Örneğin, gerçekleşen ilk 10 olayları bulmak için girin ve bu arama sorgusu seçin: **kategori arama "WorkflowRuntime" == | 10 sınırla**

   ![Arama dizesi girin](media/logic-apps-monitor-your-logic-apps/oms-start-query.png)

   Daha fazla bilgi edinin [Azure İzleyici günlüklerine veri bulma](../log-analytics/log-analytics-log-searches.md).

5. Sonuçları sayfasında sol çubuğunda, görüntülemek istediğiniz zaman çerçevesini seçin.
Bir filtre ekleyerek sorgunuzu iyileştirmek için seçin **+ Ekle**.

   ![Sorgu sonuçları için zaman çerçevesini seçin](media/logic-apps-monitor-your-logic-apps/query-results.png)

6. Altında **ekleme filtreleri**, istediğiniz filtreyi bulabilmesi için filtre adı girin. Filtre seçin ve **+ Ekle**.

   Bu örnek "Durum" sözcüğü altında başarısız olayları bulmak için kullanır. **AzureDiagnostics**.
   Burada filtresi için **status_s** zaten seçildi.

   ![Filtre seçin](media/logic-apps-monitor-your-logic-apps/log-search-add-filter.png)

7. Sol çubuğunda, seçin ve istediğiniz filtre değeri **Uygula**.

   ![Filtre değeri'ı seçin, "Uygula"](media/logic-apps-monitor-your-logic-apps/log-search-apply-filter.png)

8. Artık oluşturduğunuz sorgu için döndürür. Sorgunuz, seçilen filtre ve değer güncelleştirilir. Önceki sonuçlarınızı şimdi çok filtrelenir.

   ![Sorgunuzu filtrelenmiş sonuçları ile geri dönün](media/logic-apps-monitor-your-logic-apps/log-search-query-filtered-results.png)

9. Sorgunuz gelecekte kullanım için kaydetmeyi seçebilirsiniz **Kaydet**. Bilgi [sorgunuzu kaydetme](../logic-apps/logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md#save-oms-query).

<a name="extend-diagnostic-data"></a>

## <a name="extend-how-and-where-you-use-diagnostic-data-with-other-services"></a>Nasıl ve tanılama verilerini diğer hizmetleri ile kullandığınız genişletin

Azure İzleyici günlüklerine yanı sıra, mantıksal uygulamanızın tanılama verilerini diğer Azure hizmetleriyle örneğin kullanma genişletebilirsiniz: 

* [Azure depolama alanında Azure tanılama günlüklerini arşivleme](../azure-monitor/platform/archive-diagnostic-logs.md)
* [Azure Event hubs'a Stream Azure tanılama günlükleri](../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md) 

İzleme telemetri ve diğer hizmetlerden analytics kullanarak gerçek zamanlı Get ister sonra yapabilecekleriniz [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) ve [Power BI](../azure-monitor/platform/powerbi.md). Örneğin:

* [Event Hubs verilerini Stream Stream analytics'e](../stream-analytics/stream-analytics-define-inputs.md)
* [Stream Analytics ile akış verilerini analiz etme ve Power BI'da gerçek zamanlı analiz Pano oluşturma](../stream-analytics/stream-analytics-power-bi-dashboard.md)

Ayarlamak istediğiniz seçenekleri bağlı olarak, emin olun, ilk [bir Azure depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md) veya [bir Azure olay hub'ı oluşturma](../event-hubs/event-hubs-create.md). Ardından, Tanılama verileri göndermek istediğiniz seçenekleri seçin:

![Azure depolama hesabına veya olay hub'ına veri Gönder](./media/logic-apps-monitor-your-logic-apps/storage-account-event-hubs.png)

> [!NOTE]
> Yalnızca bir depolama hesabı kullanmayı seçtiğinizde bekletme dönemleri uygulamak.

<a name="add-azure-alerts"></a>

## <a name="set-up-alerts-for-your-logic-app"></a>Mantıksal uygulamanız için uyarıları ayarlama

Belirli ölçümleri veya mantıksal uygulamanızın aşıldı eşikleri izlemek üzere ayarlanan sahte [azure'daki uyarıları](../azure-monitor/platform/alerts-overview.md). Hakkında bilgi edinin [ölçümleri azure'da](../monitoring-and-diagnostics/monitoring-overview-metrics.md). 

Uyarılar olmadan ayarlamak için [Azure İzleyici günlükleri](../log-analytics/log-analytics-overview.md), şu adımları izleyin. Daha gelişmiş uyarı ölçütleri ve eylemleri için [Azure İzleyici günlüklerini ayarlamak](#azure-diagnostics) çok.

1. Mantıksal uygulama dikey penceresinde menüsünde altında **izleme**, seçin **tanılama** > **uyarı kuralları** > **uyarısı Ekle**burada gösterildiği gibi:

   ![Mantıksal uygulamanız için bir uyarı Ekle](media/logic-apps-monitor-your-logic-apps/set-up-alerts.png)

2. Üzerinde **bir uyarı kuralı Ekle** dikey Uyarınız gösterildiği gibi oluşturun:

   1. Altında **kaynak**, mantıksal uygulamanızı seçin, seçili değilse. 
   2. Bir ad ve açıklama için uyarı verir.
   3. Seçin bir **ölçüm** veya izlemek istediğiniz olay.
   4. Seçin bir **koşul**, belirtin bir **eşiği** kabuş edin ve seçin için **süresi** Bu ölçüm izleme.
   5. Uyarı için posta göndermek bu seçeneği seçin. 
   6. Diğer tüm e-posta adresleri uyarı göndermek için belirtin. 
   Uyarı göndermek istediğiniz bir Web kancası URL'si de belirtebilirsiniz.

   Örneğin, bu kural beş olduğunda bir uyarı gönderir veya bir saat içinde çalıştırma daha başarısız:

   ![Ölçüm uyarı kuralı oluşturma](media/logic-apps-monitor-your-logic-apps/create-alert-rule.png)

> [!TIP]
> Bir uyarıdan bir mantık uygulaması çalıştırmak için dahil edebileceğiniz [istek tetikleyicisi](../connectors/connectors-native-reqres.md) iş akışınızda olanak sağlayan görevleri gibi bu örnekler:
> 
> * [POST Slack için](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
> * [Bir metin iletisi gönderin](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
> * [Kuyruğa bir ileti ekleyin](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)

<a name="diagnostic-event-properties"></a>

## <a name="azure-diagnostics-event-settings-and-details"></a>Azure Tanılama Olay ayarları ve ayrıntıları

Her mantıksal uygulamanızı ve bu olay, örneğin durumu hakkında ayrıntılı tanılama olayında, başlangıç saati, bitiş saati ve benzeri. İzleme, izleme ve günlüğe kaydetme program aracılığıyla ayarlamak için bu ayrıntılarla kullanabilirsiniz [Azure Logic Apps için REST API](https://docs.microsoft.com/rest/api/logic) ve [REST API'si için Azure tanılama](../azure-monitor/platform/metrics-supported.md#microsoftlogicworkflows).

Örneğin, `ActionCompleted` olayının `clientTrackingId` ve `trackedProperties` izleme ve izleme için kullanabileceğiniz özellikler:

``` json
{
    "time": "2016-07-09T17:09:54.4773148Z",
    "workflowId": "/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP",
    "resourceId": "/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP/RUNS/08587361146922712057/ACTIONS/HTTP",
    "category": "WorkflowRuntime",
    "level": "Information",
    "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
    "properties": {
        "$schema": "2016-06-01",
        "startTime": "2016-07-09T17:09:53.4336305Z",
        "endTime": "2016-07-09T17:09:53.5430281Z",
        "status": "Succeeded",
        "code": "OK",
        "resource": {
            "subscriptionId": "<subscription-ID>",
            "resourceGroupName": "MyResourceGroup",
            "workflowId": "cff00d5458f944d5a766f2f9ad142553",
            "workflowName": "MyLogicApp",
            "runId": "08587361146922712057",
            "location": "westus",
            "actionName": "Http"
        },
        "correlation": {
            "actionTrackingId": "e1931543-906d-4d1d-baed-dee72ddf1047",
            "clientTrackingId": "<my-custom-tracking-ID>"
        },
        "trackedProperties": {
            "myTrackedProperty": "<value>"
        }
    }
}
```

* `clientTrackingId`: Aksi durumda tüm iç içe geçmiş iş akışları dahil olmak üzere, Azure otomatik olarak bu kimliği oluşturur ve olayları karşılık gelen bir mantıksal uygulama çalıştırması, sağlanan mantıksal uygulamadan denir. Bu kimliği bir tetikleyici geçirerek el ile belirtebilirsiniz bir `x-ms-client-tracking-id` üstbilgi tetikleyici isteğinde, özel kimlik değerine sahip. Bir istek tetikleyicisi, HTTP tetikleyicisi veya Web kancasını tetikleyici olarak kullanabilirsiniz.

* `trackedProperties`: Giriş veya çıkış tanılama veri izlemek için mantıksal uygulamanızın JSON tanımında eylemler için izlenen özellikler ekleyebilirsiniz. İzlenen özellikler yalnızca tek bir eylemin girişler ve çıkışlar izleyebilirsiniz, ancak kullanabileceğiniz `correlation` çalıştırmada eylemler arasında ilişkilendirmek için olay özellikleri.

  Bir veya daha fazla özellik izlemek için Ekle `trackedProperties` bölümü ve eylem tanımına istediğiniz özellikleri. Örneğin, bir "sipariş kimliği" telemetrinizi gibi verileri izlemek istediğinizi varsayalım:

  ``` json
  "myAction": {
    "type": "http",
    "inputs": {
        "uri": "http://uri",
        "headers": {
            "Content-Type": "application/json"
        },
        "body": "@triggerBody()"
    },
    "trackedProperties": {
        "myActionHTTPStatusCode": "@action()['outputs']['statusCode']",
        "myActionHTTPValue": "@action()['outputs']['body']['<content>']",
        "transactionId": "@action()['inputs']['body']['<content>']"
    }
  }
  ```

## <a name="next-steps"></a>Sonraki adımlar

* [Mantıksal uygulama dağıtımı için şablonlar oluşturma ve release management](../logic-apps/logic-apps-create-deploy-template.md)
* [Enterprise Integration Pack ile B2B senaryoları](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [B2B iletilerini izleme](../logic-apps/logic-apps-monitor-b2b-message.md)
