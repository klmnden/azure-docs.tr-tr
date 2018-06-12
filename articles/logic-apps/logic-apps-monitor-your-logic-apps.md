---
title: Durumunu denetlemek için günlüğe kaydetmeyi ayarlayın ve Uyarıları - Azure mantıksal uygulamaları alma | Microsoft Docs
description: Durum ve logic apps için performans izleme, tanılama verilerini günlüğe ve uyarılarını ayarlama
author: jeffhollan
manager: jeconnoc
editor: ''
services: logic-apps
documentationcenter: ''
ms.assetid: 5c1b1e15-3b6c-49dc-98a6-bdbe7cb75339
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/21/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 061269050ad598e1877c3b7bc6745d4095816020
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35301227"
---
# <a name="monitor-status-set-up-diagnostics-logging-and-turn-on-alerts-for-azure-logic-apps"></a>Durum İzleme, tanılama günlük ayarlama ve Azure Logic Apps için uyarılarını Aç

Çalıştırdıktan sonra [oluşturma ve bir mantıksal uygulama çalıştırma](../logic-apps/quickstart-create-first-logic-app-workflow.md), çalışır geçmişi, tetikleyici geçmişi, durumunu ve performansını kontrol edebilirsiniz. Gerçek zamanlı Olay izleme ve daha zengin hata ayıklama için ayarlanmış [tanılama günlükleri](#azure-diagnostics) mantığı uygulamanız için. Bu şekilde yapabilecekleriniz [bulma ve görüntüleme olayları](#find-events)tetikleyici olayları, çalışma olayları ve eylem olayları gibi. Bu da kullanabilirsiniz [tanılama verilerini diğer hizmetlerle](#extend-diagnostic-data)Azure Storage ve Azure Event Hubs gibi. 

Hataları veya diğer olası sorunları hakkında bildirim almak için ayarladığınız [uyarıları](#add-azure-alerts). Örneğin, "beşten fazla çalıştığında bir saat içinde başarısız olduğunda." algıladığında bir uyarı oluşturabilir. Ayrıca izleme, izleme ve program aracılığıyla kullanarak oturum ayarlayabilirsiniz [Azure tanılama olay ayarlarını ve özelliklerini](#diagnostic-event-properties).

## <a name="view-runs-and-trigger-history-for-your-logic-app"></a>Görünüm çalıştırır ve mantıksal uygulamanız için tetikleyici geçmişi

1. Mantıksal uygulamanızı bulmak için [Azure portal](https://portal.azure.com), ana Azure menüsünde, **tüm hizmetleri**. Arama kutusuna "logic apps" yazın ve seçin **Logic apps**.

   ![Mantıksal uygulamanızı Bul](./media/logic-apps-monitor-your-logic-apps/find-your-logic-app.png)

   Azure portalı, Azure aboneliğinizle ilişkili tüm mantığı uygulamalar gösterilir. 

2. Mantıksal uygulamanızı seçin, sonra seçin **genel bakış**.

   Azure Portalı'nı çalıştırır geçmişi ve mantıksal uygulamanızı için tetikleyici geçmişini gösterir. Örneğin:

   ![Mantıksal uygulama geçmişi ve tetikleyici geçmişi çalıştırır](media/logic-apps-monitor-your-logic-apps/overview.png)

   * **Geçmiş çalıştıran** mantığı uygulamanız için tüm metinler gösterir. 
   * **Tetiklemek geçmişi** mantığı uygulamanız için tüm tetikleyici etkinliğini gösterir.

   Durum açıklamaları için bkz: [mantıksal uygulamanızı sorun giderme](../logic-apps/logic-apps-diagnosing-failures.md).

   > [!TIP]
   > Araç çubuğunda, beklediğiniz veri bulamazsanız seçin **yenileme**.

3. Belirli bir çalıştırma adımları altında görüntülemek için **çalıştıran geçmişi**, çalıştırılan seçin. 

   İzleme görünümü her adım, çalışan gösterir. Örneğin:

   ![Belirli bir çalıştırma için Eylemler](media/logic-apps-monitor-your-logic-apps/monitor-view-updated.png)

4. Çalıştırma hakkında daha fazla bilgi almak için tercih **çalıştırma ayrıntıları**. Adımlar, durumu, girişleri ve çıkışları çalıştırma için bu bilgileri özetler. 

   ![Seçin "Ayrıntıları Çalıştır"](media/logic-apps-monitor-your-logic-apps/run-details.png)

   Örneğin, çalışmanın elde edebilirsiniz **bağıntı kimliği**, kullandığınızda, gereksinim duyabileceğiniz [Logic Apps için REST API](https://docs.microsoft.com/rest/api/logic).

5. Belirli bir adıma hakkında bilgi almak için bu adım seçin. Şimdi girişleri ve çıkışları Bu adım için gerçekleşen hataları gibi ayrıntıları gözden geçirebilirsiniz. Örneğin:

   ![Adım ayrıntıları](media/logic-apps-monitor-your-logic-apps/monitor-view-details.png)
   
   > [!NOTE]
   > Tüm çalışma zamanı ayrıntılarını ve olayları Logic Apps Hizmeti'nde şifrelenir. Yalnızca bir kullanıcı bu verileri görüntülemek için istediğinde şifresi çözülür. Ayrıca bu olaylar ile erişimi denetleyebilirsiniz [Azure rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/overview.md).

6. Belirli tetikleyici olay hakkında bilgi almak için geri dönüp **genel bakış** bölmesi. Altında **tetiklemek geçmişi**, tetikleyici olayı seçin. Şimdi girişleri ve çıkışları, gibi ayrıntıları örneğin gözden geçirebilirsiniz:

   ![Tetikleyici olay çıkış ayrıntıları](media/logic-apps-monitor-your-logic-apps/trigger-details.png)

<a name="azure-diagnostics"></a>

## <a name="turn-on-diagnostics-logging-for-your-logic-app"></a>Tanılama mantığı uygulamanız için oturum açma

Daha zengin çalışma zamanı ayrıntılarını ve olayları ile hata ayıklama, tanılama ile oturum ayarlayabilirsiniz [Azure günlük analizi](../log-analytics/log-analytics-overview.md). Günlük analizi, bulut izler ve şirket içi ortamları, performans ve kullanılabilirlik tutmanıza yardımcı olmak için azure'da bir hizmettir. 

Başlamadan önce bir günlük analizi çalışma alanınızın olması gerekir. Bilgi [günlük analizi çalışma alanı oluşturmak nasıl](../log-analytics/log-analytics-quick-create-workspace.md).

1. İçinde [Azure portal](https://portal.azure.com), bulma ve mantıksal uygulamanızı seçin. 

2. Mantıksal uygulama dikey menüsünde altında **izleme**, seçin **tanılama** > **tanılama ayarlarını**.

   ![, Tanılama, Tanılama izleme ayarları için Git](media/logic-apps-monitor-your-logic-apps/logic-app-diagnostics.png)

3. Altında **tanılama ayarları**, seçin **üzerinde**.

   ![Tanılama günlüklerini Aç](media/logic-apps-monitor-your-logic-apps/turn-on-diagnostics-logic-app.png)

4. Şimdi gösterildiği gibi günlüğü için günlük analizi çalışma alanı ve olay kategorisi seçin:

   1. Seçin **için günlük analizi Gönder**. 
   2. Altında **günlük analizi**, seçin **yapılandırma**. 
   3. Altında **OMS çalışma alanları**, günlük için kullanılacak günlük analizi çalışma alanını seçin.
   4. Altında **günlük**seçin **WorkflowRuntime** kategorisi.
   5. Ölçüm aralığını seçin.
   6. İşiniz bittiğinde **Kaydet**’i seçin.

   ![Günlük analizi çalışma alanı ve günlük verilerini seçin](media/logic-apps-monitor-your-logic-apps/send-diagnostics-data-log-analytics-workspace.png)

Şimdi, olaylar ve eylem olaylar çalıştırmak tetikleyici olaylar için olayları ve diğer verileri bulabilirsiniz.

<a name="find-events"></a>

## <a name="find-events-and-data-for-your-logic-app"></a>Mantıksal uygulamanız için olaylar ve veri Bul

Bulma ve mantıksal uygulamanızı olaylarını görüntüleme gibi için olaylar, olaylar, çalıştırmak ve eylem olaylar tetikleyin, şu adımları izleyin.

1. İçinde [Azure portal](https://portal.azure.com), seçin **tüm hizmetleri**. "İçin günlük analizi" arayın, ardından seçin **günlük analizi** aşağıda gösterildiği gibi:

   !["Günlük analizi" seçin](media/logic-apps-monitor-your-logic-apps/browseloganalytics.png)

2. Altında **günlük analizi**, bulma ve günlük analizi çalışma alanınızı seçin. 

   ![Günlük analizi çalışma alanınızı seçin](media/logic-apps-monitor-your-logic-apps/selectla.png)

3. Altında **Yönetim**, seçin **OMS portalı**.

   !["OMS portalı" seçin](media/logic-apps-monitor-your-logic-apps/omsportalpage.png)

4. Giriş sayfanızda seçin **günlük arama**.

   ![Giriş sayfanızda "Günlük arama" seçin](media/logic-apps-monitor-your-logic-apps/logsearch.png)

   -veya-

   ![Menüsünde "Günlük arama" öğesini seçin](media/logic-apps-monitor-your-logic-apps/logsearch-2.png)

5. Arama kutusuna, bulmak ve basın istediğiniz bir alanı belirtmek **Enter**. Yazmaya başladığınızda, olası eşleşmeler ve kullanabileceğiniz işlemleri bakın. 

   Örneğin, ilk 10 olayları bulmak için girin ve bu arama sorgusuna seçin: **kategori arama "WorkflowRuntime" == | 10 sınırla**

   ![Arama dizesini girin](media/logic-apps-monitor-your-logic-apps/oms-start-query.png)

   Daha fazla bilgi edinmek [günlük analizi verileri bulmak üzere nasıl](../log-analytics/log-analytics-log-searches.md).

6. Sonuçları sayfasında sol çubuğunda görüntülemek istediğiniz zaman dilimini seçin.
Bir filtre ekleyerek sorgunuzu daraltın tercih **+ Ekle**.

   ![Sorgu sonuçları ilişkin zaman çerçevesini seçin](media/logic-apps-monitor-your-logic-apps/query-results.png)

7. Altında **filtreler Ekle**, istediğiniz filtreyi bulabilmek için filtre adı girin. Filtreyi seçin ve **+ Ekle**.

   Bu örnekte "Durum" word altında başarısız olayları bulmak için kullanır. **AzureDiagnostics**.
   Burada filtresi için **status_s** zaten seçilidir.

   ![Filtreyi seçin](media/logic-apps-monitor-your-logic-apps/log-search-add-filter.png)

8. Sol çubuğunda ve seçin istediğiniz filtre değeri seçin **Uygula**.

   ![Filtre değeri seçin, "Uygula"](media/logic-apps-monitor-your-logic-apps/log-search-apply-filter.png)

9. Şimdi oluşturmakta olduğunuz sorgu döndür. Sorgunuz seçili filtresi ve değer ile güncelleştirilir. Önceki sonuçlarınızı artık çok filtrelenir.

   ![Sorgunuz filtrelenmiş sonuçlar döndürür](media/logic-apps-monitor-your-logic-apps/log-search-query-filtered-results.png)

10. Sorgunuz gelecekte kullanım için kaydetmek üzere seçim yapın **kaydetmek**. Bilgi [sorgunuzu kaydetmek nasıl](../logic-apps/logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md#save-oms-query).

<a name="extend-diagnostic-data"></a>

## <a name="extend-how-and-where-you-use-diagnostic-data-with-other-services"></a>Nasıl ve tanılama verilerini diğer hizmetleri ile kullandığınız genişletme

Azure günlük analizi ile birlikte nasıl mantığı uygulamanızın tanılama verilerini diğer Azure hizmetleriyle örneğin kullandığınız genişletebilirsiniz: 

* [Azure depolama alanında Azure tanılama günlüklerini arşiv](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md)
* [Azure Event hubs'a akış Azure tanılama günlükleri](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md) 

Telemetri ve diğer hizmetlerden analizi kullanarak izleme Get gerçek zamanlı ister sonra şunları yapabilirsiniz [Azure akış analizi](../stream-analytics/stream-analytics-introduction.md) ve [Power BI](../log-analytics/log-analytics-powerbi.md). Örneğin:

* [Event Hubs akış verilerini akış analizi](../stream-analytics/stream-analytics-define-inputs.md)
* [Akış Analizi ile akış verilerini çözümleme ve gerçek zamanlı analiz Pano Power BI'da oluşturma](../stream-analytics/stream-analytics-power-bi-dashboard.md)

Ayarlamak istediğiniz seçenekleri bağlı olarak, olduğundan emin olun, ilk [bir Azure depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md) veya [Azure olay hub'ı oluşturma](../event-hubs/event-hubs-create.md). Ardından Tanılama verileri gönder istediğiniz seçenekleri seçin:

![Verileri Azure depolama hesabı veya olay hub'ına Gönder](./media/logic-apps-monitor-your-logic-apps/storage-account-event-hubs.png)

> [!NOTE]
> Yalnızca bir depolama hesabı kullanmayı seçtiğinizde bekletme dönemleri uygulamak.

<a name="add-azure-alerts"></a>

## <a name="set-up-alerts-for-your-logic-app"></a>Mantıksal uygulamanız için uyarıları ayarlama

Belirli ölçümleri veya aşıldı eşikler mantıksal uygulamanızı izlemek için ayarladığınız [Azure içindeki uyarıları](../monitoring-and-diagnostics/monitoring-overview-alerts.md). Hakkında bilgi edinin [Azure ölçümlerini](../monitoring-and-diagnostics/monitoring-overview-metrics.md). 

Uyarıları ayarlamak için [Azure günlük analizi](../log-analytics/log-analytics-overview.md), şu adımları izleyin. Uyarı ölçütleri ve Eylemler, daha gelişmiş [günlük analizi ayarlamak](#azure-diagnostics) çok.

1. Mantıksal uygulama dikey menüsünde altında **izleme**, seçin **tanılama** > **uyarı kuralları** > **UyarıEkle**aşağıda gösterildiği gibi:

   ![Mantıksal uygulamanız için uyarı ekleme](media/logic-apps-monitor-your-logic-apps/set-up-alerts.png)

2. Üzerinde **uyarı kuralı eklemek** dikey penceresinde, uyarı gösterildiği gibi oluşturun:

   1. Altında **kaynak**, mantıksal uygulamanızı seçin, seçili değilse. 
   2. Bir ad ve açıklama için uyarı verir.
   3. Seçin bir **ölçüm** veya izlemek istediğiniz olay.
   4. Seçin bir **koşulu**, belirtin bir **eşik** ölçüm ve select **süresi** Bu ölçüm izleme.
   5. Uyarı için posta göndermek isteyip istemediğinizi belirtin. 
   6. Diğer tüm e-posta adreslerini uyarı göndermek için belirtin. 
   Uyarı göndermek istediğiniz bir Web kancası URL'si de belirtebilirsiniz.

   Örneğin, bu kural beş olduğunda bir uyarı gönderir veya daha fazla çalıştığında bir saat içinde başarısız:

   ![Ölçüm uyarı kuralı oluştur](media/logic-apps-monitor-your-logic-apps/create-alert-rule.png)

> [!TIP]
> Bir mantıksal uygulama bir uyarıdan çalıştırmak için dahil edebileceğiniz [isteği tetikleyici](../connectors/connectors-native-reqres.md) , iş akışınızı olanak sağlayan bu örnekler gibi görevleri gerçekleştirmenize:
> 
> * [POST boşluk için](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
> * [Bir metin Gönder](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
> * [Kuyruğa bir ileti Ekle](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)

<a name="diagnostic-event-properties"></a>

## <a name="azure-diagnostics-event-settings-and-details"></a>Azure Tanılama Olay ayarları ve ayrıntıları

Mantıksal uygulamanızı ve bu olay, örneğin, durum ayrıntılarını her Tanılama Olay sahipse, başlangıç saati, bitiş saati ve benzeri. İzleme, izleme ve günlüğe kaydetme programlı olarak ayarlamak için bu ayrıntılarla kullanabilirsiniz [Azure Logic Apps için REST API](https://docs.microsoft.com/rest/api/logic) ve [Azure tanılama için REST API](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftlogicworkflows).

Örneğin, `ActionCompleted` olayında `clientTrackingId` ve `trackedProperties` izleme ve izleme için kullanabileceğiniz özellikler:

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

* `clientTrackingId`: Değilse tüm iç içe geçmiş iş akışları dahil olmak üzere Azure otomatik olarak bu kimliği oluşturur ve olayları çalıştırmak bir mantıksal uygulama arasında karşılık gelen sağlanan mantığı uygulamadan çağrılan. Bu kodu bir tetikleyiciden geçirerek el ile belirtebilirsiniz bir `x-ms-client-tracking-id` , özel tetikleyici istek kimliği değeri ile üstbilgisi. Bir istek tetikleyici, HTTP tetikleyicisini ya da Web kancası tetikleyici kullanabilirsiniz.

* `trackedProperties`: Girişleri veya çıkışları tanılama veri izlemek için mantığı uygulamanızın JSON tanımında Eylemler izlenen özellikleri ekleyebilirsiniz. İzlenen özellikleri yalnızca tek bir eylemin girişleri ve çıkışları izleyebilirsiniz, ancak kullanabileceğiniz `correlation` çalıştırmada eylemler arasında ilişkilendirilmesi olayları özelliklerini.

  Bir veya daha fazla özellikleri izlemek için ekleme `trackedProperties` bölümü ve eylem tanımına istediğiniz özellikleri. Örneğin, bir "sipariş Kimliğinde" telemetrinizi gibi verileri izlemek istediğinizi varsayalım:

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

* [Mantıksal uygulama dağıtımı için şablonlar oluşturma ve yayın Yönetimi](../logic-apps/logic-apps-create-deploy-template.md)
* [B2B senaryolarını Kurumsal tümleştirme paketi](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [B2B iletilerini izleme](../logic-apps/logic-apps-monitor-b2b-message.md)
