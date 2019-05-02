---
title: Oluşturun, görüntüleyin ve yönetin günlük uyarıları kullanarak Azure İzleyici | Microsoft Docs
description: Azure İzleyici, yazar, görüntüleyin ve azure'da günlük uyarı kuralları yönetmek için kullanın.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/15/2018
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: d893fb1023188498260813642678397a39bb2442
ms.sourcegitcommit: 8a681ba0aaba07965a2adba84a8407282b5762b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64872379"
---
# <a name="create-view-and-manage-log-alerts-using-azure-monitor"></a>Oluşturun, görüntüleyin ve Azure İzleyicisi'ni kullanarak günlük uyarıları yönetme

## <a name="overview"></a>Genel Bakış
Bu makalede, Azure portalı içinden uyarıları arabirimini kullanarak günlük uyarıları ayarlama işlemini göstermektedir. Bir uyarı kuralı tanımı üç bölümlerinde verilmiştir:
- Hedef: İzlenmesi gereken belirli Azure kaynak
- Ölçütleri: Belirli bir koşulu veya mantıksal, sinyalin görülen, tetikleyici
- Eylem: Bir bildirim - bir alıcıya belirli çağrı gönderilen, SMS, Web kancası vb. e-posta.

Terim **günlük uyarıları** günlük sorguda olduğu sinyal uyarılarını açıklamak için bir [Log Analytics çalışma alanı](../learn/tutorial-viewdata.md) veya [Application Insights](../app/analytics.md). Daha fazla ilgili işlevler, terminolojisi ve türlerden öğrenin [günlük uyarıları - genel bakış](alerts-unified-log.md).

> [!NOTE]
> Popüler günlük verilerini [bir Log Analytics çalışma alanı](../../azure-monitor/learn/tutorial-viewdata.md) şimdi de Azure İzleyici ölçüm platformda kullanılabilir. Ayrıntılar görünümü için [günlükleri için ölçüm Uyarısı](alerts-metric-logs.md)

## <a name="managing-log-alerts-from-the-azure-portal"></a>Azure portalından günlük uyarıları yönetme

Ayrıntılı sonraki Azure portal arabirimi kullanarak günlük uyarıları kullanarak adım adım kılavuzdur.

### <a name="create-a-log-alert-rule-with-the-azure-portal"></a>Azure portal ile günlük uyarı kuralı oluşturma
1. İçinde [portalı](https://portal.azure.com/)seçin **İzleyici** ve izleme bölümü altında - **uyarılar**.

    ![İzleme](media/alerts-log/AlertsPreviewMenu.png)

1. Seçin **yeni uyarı kuralı** Azure'da yeni bir uyarı oluşturmak için.

    ![Uyarı Ekle](media/alerts-log/AlertsPreviewOption.png)

1. Uyarı oluşturma bölümünde, üç bölümden oluşan ile gösterilir: *Uyarı koşulunu tanımlama*, *uyarı ayrıntılarını tanımlama*, ve *tanımla eylem grubu*.

    ![Kural oluşturma](media/alerts-log/AlertsPreviewAdd.png)

1. Kullanarak uyarı koşulunu tanımlama **seçin kaynak** bağlantı ve hedef bir kaynak seçerek belirtme. Seçeneğini belirleyerek Filtre _abonelik_, _kaynak türü_ve gerekli _kaynak_.

   > [!NOTE]
   > 
   > Günlük oluşturmak için uyarı - doğrulama **günlük** devam etmeden önce sinyali seçili kaynak için kullanılabilir.
   >  ![Kaynak seçin](media/alerts-log/Alert-SelectResourceLog.png)

1. *Günlük uyarıları*: Olun **kaynak türü** gibi bir analytics kaynak *Log Analytics* veya *Application Insights* ve sinyal türü olarak **günlük**, sonra bir kez uygun **kaynak** olduğundan seçilen, tıklayın *Bitti*. Ardından **Ölçüt Ekle** sinyal sinyal listeden ve kaynak için kullanılabilir seçenekler listesini görüntüleyin düğmesine **özel günlük araması** seçilen gibi hizmetini izleme günlüğü için seçenek *günlük Analytics* veya *Application Insights*.

   ![Kaynak - özel bir günlük araması'nı seçin](media/alerts-log/AlertsPreviewResourceSelectionLog.png)

   > [!NOTE]
   > 
   > Liste sinyal türü - analytics sorgusuna alma uyarılar **günlük (kayıtlı sorgu)**, çizimde görüldüğü gibi. Böylece kullanıcılar Analytics sorgunuzda mükemmel ve gelecekte kullanılmak üzere uyarılar - kaydetmek daha fazla ayrıntı bulunabilir sorgu kaydetme kullanarak [Azure İzleyici'de günlük sorgusu kullanarak](../log-query/log-query-overview.md) veya [application ınsights analytics paylaşılan sorgu ](../log-query/log-query-overview.md).

1. *Günlük uyarıları*: İçinde bu onay kutusu seçildiğinde, uyarı için sorgu belirtilebilir **arama sorgusu** sorgu söz dizimi yanlışsa alanda hata kırmızı renkte görüntülenir; alan. Sorgu Sözdizimi doğruysa - başvuru için belirtilen sorgu geçmiş veri son altı saat zaman penceresinden geçen hafta için ince seçeneğiyle bir grafik olarak gösterilir.

    ![Uyarı kuralını yapılandırın](media/alerts-log/AlertsPreviewAlertLog.png)

   > [!NOTE]
   > 
   > Geçmiş verileri görselleştirme, sorgu sonuçları saati ayrıntıları varsa yalnızca gösterilebilir. Özetlenmiş veriler veya belirli bir sütun değerleri -, sorgu sonuçları aynı tekil bir çizim gösterilir.
   > Ölçüm ölçüsü türü Application Insights'ı kullanarak günlük uyarıları için veya [yeni API'ye yönelik geçiş](alerts-log-api-switch.md), kullanarak verileri gruplandırmak için hangi belirli bir değişken belirtebilirsiniz **bulunan** ; gösterilen şekilde seçeneği Aşağıda:
   > 
   > ![toplama seçeneği](media/alerts-log/aggregate-on.png)

1. *Günlük uyarıları*: Bir yerde görselleştirme ile **Alert Logic** koşulu, toplama ve son olarak eşiği gösterilen seçeneklerden seçilebilir. Son olarak mantığında belirtin belirtilen koşulun değerlendirme süresi kullanarak **süresi** seçeneği. Uyarı seçerek ne sıklıkta çalıştırılacağını birlikte **sıklığı**. **Günlük uyarıları** temel alabilir:
    - [Kayıt sayısı](../../azure-monitor/platform/alerts-unified-log.md#number-of-results-alert-rules): Sorgu tarafından döndürülen kayıt sayısını büyüktür veya belirtilen değerden daha az ise bir uyarı oluşturulur.
    - [Ölçüm ölçüsü](../../azure-monitor/platform/alerts-unified-log.md#metric-measurement-alert-rules): Her değilse bir uyarı oluşturulur *toplam değer* sonuçlarda sağlanan eşik değerini aşıyor ve bu *göre gruplandırılmış* değer seçildi. Seçili zaman aralığındaki eşiğini kaç kez uyarısının ihlallerini sayısıdır. Sonuç kümesi veya ihlallerini ardışık örnekler içinde gerçekleşmelidir gerektirecek şekilde ardışık ihlaller genelinde toplam ihlal sayısı için herhangi bir birleşimini ihlallerini belirtebilirsiniz.


1. İkinci bir adım olarak, bir ad, uyarının tanımlayın **uyarı kuralı adı** alanı ile birlikte bir **açıklama** uyarı özellikleri gerçekleşen ve **önem derecesi** değerini Sağlanan seçenekler. Bu ayrıntılar, tüm uyarı e-postaları, bildirimleri veya Azure İzleyici tarafından gerçekleştirilen anında iletme tanımlarlar. Kullanıcı uygun şekilde değiştirerek uyarı kuralı oluşturma hemen etkinleştirmeye ek olarak, seçebilir **oluşturulduktan sonra kuralı etkinleştir** seçeneği.

    İçin **günlük uyarıları** yalnızca, bazı ilave işlevler uyarı ayrıntıları kullanılabilir:

    - **Uyarıları bastır**: Uyarı kuralı için gizleme etkinleştirdiğinizde, Eylemler kural için yeni bir uyarı oluşturduktan sonra tanımlı bir süre için devre dışı bırakıldı. Kural hala çalışıyor ve ölçütler karşılanıyorsa sağlanan uyarı kayıtları oluşturur. Yinelenen eylemler çalıştırmadan sorunu düzeltmek için izin verme size zaman.

        ![Günlük uyarıları için uyarıları bastır](media/alerts-log/AlertsPreviewSuppress.png)

        > [!TIP]
        > Bildirimleri çakışma durdurulduğundan emin olmak için uyarı sıklığını büyüktür bastır bir uyarı değerini belirtin

1. Üçüncü ve son adım olarak belirtmek varsa **eylem grubu** uyarı kuralı için Uyarı koşulu karşılandığında tetiklenen gerekir. Uyarı ile mevcut bir eylem grubu seçin veya yeni bir eylem grubu oluşturun. Şunlara göre bir uyarı tetikleyicisi Azure olur olduğunda bir eylem grubu seçili: email(s) göndermek, SMS(s) göndermek, Web kancası çağrısı, Azure runbook'ları, anında iletme ITSM aracına, vb. kullanarak düzeltin. Daha fazla bilgi edinin [Eylem grupları](action-groups.md).

    > [!NOTE]
    > Başvurmak [Azure abonelik hizmeti limitleri](../../azure-subscription-service-limits.md) yönelik limitlerle ilgili Azure Eylem grupları günlük uyarıları için tetiklenen Runbook'u yükler

    İçin **günlük uyarıları** bazı ilave işlevler şu varsayılan eylemleri geçersiz kılmak kullanılabilir:

    - **E-posta bildirimi**: Geçersiz kılmalar *e-posta konusu* eylem grubu aracılığıyla; söz konusu eylem grubuna bir veya daha fazla e-posta eylemleri varsa gönderilen e-postadaki. Postanın gövdesini değiştiremezsiniz ve bu alan **değil** e-posta adresi.
    - **Özel Json yükü dahil**: Web kancası Eylem grupları tarafından kullanılan JSON geçersiz kılar; bir veya daha fazla Web kancası eylemleri söz konusu eylem grubuna varsa. Kullanıcı, ilişkili eylem grubunda yapılandırılmış tüm Web kancaları için kullanılmak üzere JSON biçimi belirtebilirsiniz; Web kancası biçimleri hakkında daha fazla bilgi için bkz. [günlük uyarıları için Web kancası eylemi](../../azure-monitor/platform/alerts-log-webhook.md). Örnek JSON verileri kullanarak biçimini denetlemek için Görünümü Web kancası seçeneği sağlanır.

        ![Günlük uyarıları için eylem geçersiz kılmaları](media/alerts-log/AlertsPreviewOverrideLog.png)


1. Tüm alanları geçerliyse ve yeşil onay **uyarı kuralı oluşturma** düğmesini ve Azure İzleyici - uyarılar bir uyarı oluşturulur. Tüm uyarıları Pano uyarılardan görüntülenebilir.

     ![Kural oluşturma](media/alerts-log/AlertsPreviewCreate.png)

     Birkaç dakika içinde uyarı etkin ve daha önce açıklandığı gibi tetikler.

Kullanıcılar ayrıca kendi analytics sorgunuzda kesin [günlük analizi](../log-query/portals.md) ve Ayarla'uyarı ' düğmesiyle - bir uyarı oluşturmak için anında iletme sonra adım 6'dan başlayarak yukarıdaki öğreticide talimatları.

 ![Log Analytics - uyarı ayarlama](media/alerts-log/AlertsAnalyticsCreate.png)

### <a name="view--manage-log-alerts-in-azure-portal"></a>Azure portalında günlük uyarıları yönetme & Görüntüle

1. İçinde [portalı](https://portal.azure.com/)seçin **İzleyici** ve izleme bölümü altında - **uyarılar**.

1. **Uyarılar Panosu** görüntülenen - gibi tüm Azure Uyarıları'nı (günlük uyarılar dahil) bir tekil panosunda görüntülenir; her örneği, günlük uyarı kuralı ne zaman dahil tetiklendi. Daha fazla bilgi için bkz. [uyarı Yönetimi](https://aka.ms/managealertinstances).
    > [!NOTE]
    > Günlük uyarı kuralı oluşturan kullanıcılar tarafından sağlanan özel sorgu tabanlı mantığı ve bu nedenle çözümlenmiş duruma olmadan. Günlük uyarı kuralında belirtilen koşullar karşılandığında her zaman, nedeniyle harekete geçirilir.

1. Seçin **yönetme kuralları** oluşturulan uyarı kurallarının tümünü burada listelenen kural Yönetim bölümüne - gitmek için üst taraftaki çubukta, düğmesine; devre dışı bırakıldı uyarıları da dahil olmak üzere.
    ![ Uyarı kurallarını yönet](media/alerts-log/manage-alert-rules.png)

## <a name="managing-log-alerts-using-azure-resource-template"></a>Azure kaynak şablonu kullanarak günlük uyarıları yönetme

Azure İzleyici'de günlüğü uyarılarına kaynak türüyle ilişkili `Microsoft.Insights/scheduledQueryRules/`. Bu kaynak türü hakkında daha fazla bilgi için bkz. [Azure İzleyici - zamanlanmış sorgu kuralları API Başvurusu](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/). Application Insights veya Log Analytics için günlük uyarıları, kullanılarak oluşturulan [zamanlanmış sorgu kuralları API'si](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/).

> [!NOTE]
> Log Analytics için günlük uyarıları da eski kullanılarak yönetilebilir [Log Analytics uyarı API](api-alerts.md) ve eski şablonları [Log Analytics kayıtlı aramaları ve Uyarıları](../insights/solutions-resources-searches-alerts.md) de. Varsayılan olarak burada ayrıntıları yeni ScheduledQueryRules API'sini kullanarak daha fazla bilgi için bkz. [geçiş yapmak için yeni bir API için Log Analytics uyarılarını](alerts-log-api-switch.md).


### <a name="sample-log-alert-creation-using-azure-resource-template"></a>Azure kaynak şablonu kullanarak örnek günlük uyarısı oluşturma

Bir yapıdır aşağıdaki [zamanlanmış sorgu kuralı oluşturma](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/createorupdate) temel, standart günlük arama sorgusu kullanarak kaynak şablonu [sonuçları türü günlük uyarı sayısı](alerts-unified-log.md#number-of-results-alert-rules), değişkenleri olarak örnek veri kümesiyle.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
    },
    "variables": {
        "alertLocation": "southcentralus",
        "alertName": "samplelogalert",
        "alertTag": "hidden-link:/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/components/sampleAIapplication",
        "alertDescription": "Sample log search alert",
        "alertStatus": "true",
        "alertSource":{
            "Query":"requests",
            "SourceId": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/components/sampleAIapplication",
            "Type":"ResultCount"
        },
        "alertSchedule":{
            "Frequency": 15,
            "Time": 60
        },
        "alertActions":{
            "SeverityLevel": "4"
        },
        "alertTrigger":{
            "Operator":"GreaterThan",
            "Threshold":"1"
        },
        "actionGrp":{
            "ActionGroup": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/actiongroups/sampleAG",
            "Subject": "Customized Email Header",
            "Webhook": "{ \"alertname\":\"#alertrulename\", \"IncludeSearchResults\":true }"
        }
    },
    "resources":[ {
        "name":"[variables('alertName')]",
        "type":"Microsoft.Insights/scheduledQueryRules",
        "apiVersion": "2018-04-16",
        "location": "[variables('alertLocation')]",
        "tags":{"[variables('alertTag')]": "Resource"},
        "properties":{
            "description": "[variables('alertDescription')]",
            "enabled": "[variables('alertStatus')]",
            "source": {
                "query": "[variables('alertSource').Query]",
                "dataSourceId": "[variables('alertSource').SourceId]",
                "queryType":"[variables('alertSource').Type]"
            },
            "schedule":{
                "frequencyInMinutes": "[variables('alertSchedule').Frequency]",
                "timeWindowInMinutes": "[variables('alertSchedule').Time]"
            },
            "action":{
                "odata.type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.Microsoft.AppInsights.Nexus.DataContracts.Resources.ScheduledQueryRules.AlertingAction",
                "severity":"[variables('alertActions').SeverityLevel]",
                "aznsAction":{
                    "actionGroup":"[array(variables('actionGrp').ActionGroup)]",
                    "emailSubject":"[variables('actionGrp').Subject]",
                    "customWebhookPayload":"[variables('actionGrp').Webhook]"
                },
                "trigger":{
                    "thresholdOperator":"[variables('alertTrigger').Operator]",
                    "threshold":"[variables('alertTrigger').Threshold]"
                }
            }
        }
    } ]
}

```

> [!IMPORTANT]
> Hedef kaynak için gizli-bağlantı etiketi alanıyla kullanımını zorunlu [zamanlanmış sorgu kuralları](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/) API çağrısı veya kaynak şablonu.

Yukarıdaki örnek json (örneğin) sampleScheduledQueryRule.json amacıyla bu kılavuzda olarak kaydedilebilir ve kullanılarak dağıtılabilir [Azure portalında Azure Resource Manager](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template).


### <a name="log-alert-with-cross-resource-query-using-azure-resource-template"></a>Azure kaynak şablonu kullanarak kaynaklar arası sorgu günlüğü Uyarısı

Aşağıdakiler için yapısıdır [zamanlanmış sorgu kuralı oluşturma](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/createorupdate) tabanlı kaynak şablonu kullanarak [kaynaklar arası günlük arama sorgusu](../../azure-monitor/log-query/cross-workspace-query.md) , [ölçüm ölçüsü türü günlüğü Uyarısı](../../azure-monitor/platform/alerts-unified-log.md#metric-measurement-alert-rules), örnek veri kümesiyle değişkenleri olarak.

```json

{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
    },
    "variables": {
        "alertLocation": "Region Name for your Application Insights App or Log Analytics Workspace",
        "alertName": "sample log alert",
        "alertDescr": "Sample log search alert",
        "alertStatus": "true",
        "alertTag": "hidden-link:/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.OperationalInsights/workspaces/servicews",
        "alertSource":{
            "Query":"union workspace(\"servicews\").Update, app('serviceapp').requests | summarize AggregatedValue = count() by bin(TimeGenerated,1h), Classification",
            "Resource1": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.OperationalInsights/workspaces/servicews",
            "Resource2": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.insights/components/serviceapp",
            "SourceId": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.OperationalInsights/workspaces/servicews",
            "Type":"ResultCount"
        },
        "alertSchedule":{
            "Frequency": 15,
            "Time": 60
        },
        "alertActions":{
            "SeverityLevel": "4",
            "SuppressTimeinMin": 20
        },
        "alertTrigger":{
            "Operator":"GreaterThan",
            "Threshold":"1"
        },
        "metricMeasurement": {
            "thresholdOperator": "Equal",
            "threshold": "1",
            "metricTriggerType": "Consecutive",
            "metricColumn": "Classification"
        },
        "actionGrp":{
            "ActionGroup": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.insights/actiongroups/sampleAG",
            "Subject": "Customized Email Header",
            "Webhook": "{ \"alertname\":\"#alertrulename\", \"IncludeSearchResults\":true }"
        }
    },
    "resources":[ {
        "name":"[variables('alertName')]",
        "type":"Microsoft.Insights/scheduledQueryRules",
        "apiVersion": "2018-04-16",
        "location": "[variables('alertLocation')]",
        "tags":{"[variables('alertTag')]": "Resource"},
        "properties":{
            "description": "[variables('alertDescr')]",
            "enabled": "[variables('alertStatus')]",
            "source": {
                "query": "[variables('alertSource').Query]",
                "authorizedResources": "[concat(array(variables('alertSource').Resource1), array(variables('alertSource').Resource2))]",
                "dataSourceId": "[variables('alertSource').SourceId]",
                "queryType":"[variables('alertSource').Type]"
            },
            "schedule":{
                "frequencyInMinutes": "[variables('alertSchedule').Frequency]",
                "timeWindowInMinutes": "[variables('alertSchedule').Time]"
            },
            "action":{
                "odata.type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.Microsoft.AppInsights.Nexus.DataContracts.Resources.ScheduledQueryRules.AlertingAction",
                "severity":"[variables('alertActions').SeverityLevel]",
                "throttlingInMin": "[variables('alertActions').SuppressTimeinMin]",
                "aznsAction":{
                    "actionGroup": "[array(variables('actionGrp').ActionGroup)]",
                    "emailSubject":"[variables('actionGrp').Subject]",
                    "customWebhookPayload":"[variables('actionGrp').Webhook]"
                },
                "trigger":{
                    "thresholdOperator":"[variables('alertTrigger').Operator]",
                    "threshold":"[variables('alertTrigger').Threshold]",
                    "metricTrigger":{
                        "thresholdOperator": "[variables('metricMeasurement').thresholdOperator]",
                        "threshold": "[variables('metricMeasurement').threshold]",
                        "metricColumn": "[variables('metricMeasurement').metricColumn]",
                        "metricTriggerType": "[variables('metricMeasurement').metricTriggerType]"
                    }
                }
            }
        }
    } ]
}

```

> [!IMPORTANT]
> Hedef kaynak için gizli-bağlantı etiketi alanıyla kullanımını zorunlu [zamanlanmış sorgu kuralları](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/) API çağrısı veya kaynak şablonu. Kaynaklar arası sorgu günlüğüne kullanırken uyarı, kullanımını [authorizedResources](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/createorupdate#source) zorunludur ve kullanıcı, belirtilen kaynak listesine erişime sahip olmalıdır

Yukarıdaki örnek json (örneğin) sampleScheduledQueryRule.json amacıyla bu kılavuzda olarak kaydedilebilir ve kullanılarak dağıtılabilir [Azure portalında Azure Resource Manager](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template).

## <a name="managing-log-alerts-using-powershell-cli-or-api"></a>PowerShell, CLI veya API kullanarak günlük uyarıları yönetme

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure İzleyici - [zamanlanmış sorgu kuralları API'si](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/) REST API ve Azure Resource Manager REST API'si ile tamamen uyumlu. Bu nedenle Azure CLI yanı sıra, Resource Manager cmdlet'ini kullanarak Powershell kullanılabilir.


> [!NOTE]
> Log Analytics için günlük uyarıları da eski kullanılarak yönetilebilir [Log Analytics uyarı API](api-alerts.md) ve eski şablonları [Log Analytics kayıtlı aramaları ve Uyarıları](../insights/solutions-resources-searches-alerts.md) de. Varsayılan olarak burada ayrıntıları yeni ScheduledQueryRules API'sini kullanarak daha fazla bilgi için bkz. [geçiş yapmak için yeni bir API için Log Analytics uyarılarını](alerts-log-api-switch.md).

Günlük uyarıları şu anda adanmış PowerShell veya CLI komutları şu anda yoktur; ancak aşağıda gösterildiği gibi Azure Resource Manager PowerShell cmdlet'i kaynak şablonu (sampleScheduledQueryRule.json) daha önce gösterilen örnek için kaynak şablonu bölümünde kullanılabilir:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName "contosoRG" -TemplateFile "D:\Azure\Templates\sampleScheduledQueryRule.json"
```

Aşağıda Azure Resource Manager kaynak şablonu (sampleScheduledQueryRule.json) daha önce gösterilen örnek için Azure CLI komutu aracılığıyla kullanım kaynak şablonu bölümünde gösterilmektedir:

```azurecli
az group deployment create --resource-group contosoRG --template-file sampleScheduledQueryRule.json
```

Başarılı bir işlem 201 durum yeni uyarı kuralı oluşturma döndürülecek veya mevcut bir uyarı kuralı değiştirilmişse, 200 döndürülür.

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [oturum uyarılar Azure uyarıları](../../azure-monitor/platform/alerts-unified-log.md)
* Anlamak [günlük uyarıları için Web kancası eylemleri](../../azure-monitor/platform/alerts-log-webhook.md)
* Daha fazla bilgi edinin [Application Insights](../../azure-monitor/app/analytics.md)
* Daha fazla bilgi edinin [oturum sorguları](../log-query/log-query-overview.md).
