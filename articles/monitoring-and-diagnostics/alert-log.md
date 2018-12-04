---
title: Oluşturun, görüntüleyin ve yönetin günlük uyarıları Azure İzleyicisi'ni kullanma
description: Azure İzleyici, yazar, görüntüleyin ve azure'da günlük uyarı kuralları yönetmek için kullanın.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/15/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: cca15447587fcec7253b449d93fc2f644fe6c249
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52851052"
---
# <a name="create-view-and-manage-log-alerts-using-azure-monitor"></a>Oluşturun, görüntüleyin ve Azure İzleyicisi'ni kullanarak günlük uyarıları yönetme  

## <a name="overview"></a>Genel Bakış
Bu makalede, Azure portalı içinden uyarıları arabirimini kullanarak günlük uyarıları ayarlama işlemini göstermektedir. Bir uyarı kuralı tanımı üç bölümlerinde verilmiştir:
- Hedef: izlenecek belirli Azure kaynağı
- Ölçüt: Belirli bir koşulu veya mantıksal, sinyalin görülen, tetikleyici
- Eylem: Bir - bildirim alıcısı için belirli çağrı gönderilen, SMS, Web kancası vb. e-posta.

Terim **günlük uyarıları** dayalı özel sorgu olduğu sinyal uyarılarını açıklamak için [Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) veya [Application Insights](../application-insights/app-insights-analytics.md). Daha fazla ilgili işlevler, terminolojisi ve türlerden öğrenin [günlük uyarıları - genel bakış](monitor-alerts-unified-log.md).

> [!NOTE]
> Popüler günlük verilerini [Azure Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) şimdi de Azure İzleyici ölçüm platformda kullanılabilir. Ayrıntılar görünümü için [günlükleri için ölçüm Uyarısı](monitoring-metric-alerts-logs.md)

## <a name="managing-log-alerts-from-the-azure-portal"></a>Azure portalından günlük uyarıları yönetme

Ayrıntılı sonraki Azure portal arabirimi kullanarak günlük uyarıları kullanarak adım adım kılavuzdur.

### <a name="create-a-log-alert-rule-with-the-azure-portal"></a>Azure portal ile günlük uyarı kuralı oluşturma
1. İçinde [portalı](https://portal.azure.com/)seçin **İzleyici** ve izleme bölümü altında - **uyarılar**.  
    ![İzleme](media/alert-log/AlertsPreviewMenu.png)

1. Seçin **yeni uyarı kuralı** Azure'da yeni bir uyarı oluşturmak için.
    ![Uyarı Ekle](media/alert-log/AlertsPreviewOption.png)

1. Uyarı oluşturma bölümünde, üç bölümden oluşan ile gösterilir: *uyarı koşulunu tanımlama*, *uyarı ayrıntılarını tanımlama*, ve *tanımla eylem grubu*.

    ![Kural oluşturma](media/alert-log/AlertsPreviewAdd.png)

1.  Kullanarak uyarı koşulunu tanımlama **seçin kaynak** bağlantı ve hedef bir kaynak seçerek belirtme. Seçeneğini belirleyerek Filtre _abonelik_, _kaynak türü_ve gerekli _kaynak_. 

    >[!NOTE]

    > Günlük oluşturmak için uyarı - doğrulama **günlük** devam etmeden önce sinyali seçili kaynak için kullanılabilir.
    ![Kaynak seçin](media/alert-log/Alert-SelectResourceLog.png)

 
1. *Günlük uyarıları*: olun **kaynak türü** gibi bir analytics kaynak *Log Analytics* veya *Application Insights* ve sinyal türü olarak **günlüğü** sonra bir kez uygun **kaynak** olduğundan seçilen, tıklayın *Bitti*. Ardından **Ölçüt Ekle** sinyal sinyal listeden ve kaynak için kullanılabilir seçenekler listesini görüntüleyin düğmesine **özel günlük araması** seçilen gibi hizmetini izleme günlüğü için seçenek *günlük Analytics* veya *Application Insights*.

   ![Kaynak - özel bir günlük araması'nı seçin](media/alert-log/AlertsPreviewResourceSelectionLog.png)

   > [!NOTE]

   > Liste sinyal türü - analytics sorgusuna alma uyarılar **günlük (kayıtlı sorgu)**, çizimde görüldüğü gibi. Böylece kullanıcılar Analytics sorgunuzda mükemmel ve gelecekte kullanılmak üzere uyarılar - kaydetmek daha fazla ayrıntı bulunabilir sorgu kaydetme kullanarak [log analytics'te günlük arama özelliğini kullanarak](../azure-monitor/log-query/log-query-overview.md) veya [application ınsights'ta paylaşılan sorgu Analytics](../azure-monitor/log-query/log-query-overview.md). 

1.  *Günlük uyarıları*: içinde seçildiğinde, uyarı için sorgu belirtilebilir **arama sorgusu** sorgu söz dizimi yanlışsa alanda hata kırmızı renkte görüntülenir; alan. Sorgu Sözdizimi doğruysa - başvuru için belirtilen sorgu geçmiş veri son altı saat zaman penceresinden geçen hafta için ince seçeneğiyle bir grafik olarak gösterilir.

 ![Uyarı kuralını yapılandırın](media/alert-log/AlertsPreviewAlertLog.png)

 > [!NOTE]

    > Geçmiş verileri görselleştirme, sorgu sonuçları saati ayrıntıları varsa yalnızca gösterilebilir. Özetlenmiş veriler veya belirli bir sütun değerleri -, sorgu sonuçları aynı tekil bir çizim gösterilir.

    >  Application ınsights'ı kullanarak günlük uyarıları ölçüm ölçüsü türü için kullanarak verileri gruplandırmak için hangi belirli bir değişken belirtebilirsiniz **bulunan** ; aşağıda gösterildiği gibi seçeneği:

    ![toplama seçeneği](media/alert-log/aggregate-on.png)

1.  *Günlük uyarıları*: bir yerde görselleştirme ile **Alert Logic** koşulu, toplama ve son olarak eşiği gösterilen seçeneklerden seçilebilir. Son olarak mantığında belirtin belirtilen koşulun değerlendirme süresi kullanarak **süresi** seçeneği. Uyarı seçerek ne sıklıkta çalıştırılacağını birlikte **sıklığı**.
İçin **günlük uyarıları** uyarılar temelinde:
   - *Kayıt sayısı*: sorgu tarafından döndürülen kayıt sayısını büyüktür veya belirtilen değerden daha az ise bir uyarı oluşturulur.
   - *Ölçüm ölçüsü*: her değilse bir uyarı oluşturulur *toplam değer* sonuçlarda sağlanan eşik değerini aşıyor ve bu *göre gruplandırılmış* değer seçildi. Seçili zaman aralığındaki eşiğini kaç kez uyarısının ihlallerini sayısıdır. Sonuç kümesi veya ihlallerini ardışık örnekler içinde gerçekleşmelidir gerektirecek şekilde ardışık ihlaller genelinde toplam ihlal sayısı için herhangi bir birleşimini ihlallerini belirtebilirsiniz. Daha fazla bilgi edinin [günlük uyarıları ve bunların türlerini](monitor-alerts-unified-log.md).


1. İkinci bir adım olarak, bir ad, uyarının tanımlayın **uyarı kuralı adı** alanı ile birlikte bir **açıklama** uyarı özellikleri gerçekleşen ve **önem derecesi** değerini Sağlanan seçenekler. Bu ayrıntılar, tüm uyarı e-postaları, bildirimleri veya Azure İzleyici tarafından gerçekleştirilen anında iletme tanımlarlar. Kullanıcı uygun şekilde değiştirerek uyarı kuralı oluşturma hemen etkinleştirmeye ek olarak, seçebilir **oluşturulduktan sonra kuralı etkinleştir** seçeneği.

    İçin **günlük uyarıları** yalnızca, bazı ilave işlevler uyarı ayrıntıları kullanılabilir:

    - **Uyarıları bastır**: gizleme için uyarı kuralı etkinleştirdiğinizde, Eylemler kural için yeni bir uyarı oluşturduktan sonra tanımlı bir süre için devre dışı bırakılır. Kural hala çalışıyor ve ölçütler karşılanıyorsa sağlanan uyarı kayıtları oluşturur. Yinelenen eylemler çalıştırmadan sorunu düzeltmek için izin verme size zaman.

        ![Günlük uyarıları için uyarıları bastır](media/alert-log/AlertsPreviewSuppress.png)

        > [!TIP]
        > Bildirimleri çakışma durdurulduğundan emin olmak için uyarı sıklığını büyüktür bastır bir uyarı değerini belirtin

1. Üçüncü ve son adım olarak belirtmek varsa **eylem grubu** uyarı kuralı için Uyarı koşulu karşılandığında tetiklenen gerekir. Uyarı ile mevcut bir eylem grubu seçin veya yeni bir eylem grubu oluşturun. Şunlara göre bir uyarı tetikleyicisi Azure olur olduğunda bir eylem grubu seçili: email(s) göndermek, SMS(s) göndermek, Web kancası çağrısı, Azure runbook'ları, anında iletme ITSM aracına, vb. kullanarak düzeltin. Daha fazla bilgi edinin [Eylem grupları](monitoring-action-groups.md).

    > [!NOTE]
    > Başvurmak [Azure abonelik hizmeti limitleri](../azure-subscription-service-limits.md) yönelik limitlerle ilgili Azure Eylem grupları günlük uyarıları için tetiklenen Runbook'u yükler

    İçin **günlük uyarıları** bazı ilave işlevler şu varsayılan eylemleri geçersiz kılmak kullanılabilir:

    - **E-posta bildirimi**: geçersiz kılan *e-posta konusu* eylem grubu aracılığıyla; söz konusu eylem grubuna bir veya daha fazla e-posta eylemleri varsa gönderilen e-postadaki. Postanın gövdesini değiştiremezsiniz ve bu alan **değil** e-posta adresi.
    - **Özel Json yükü dahil**: söz konusu eylem grubuna bir veya daha fazla Web kancası işlemleri mevcut Web kancası Eylem grupları tarafından; kullanılan JSON geçersiz kılar. Kullanıcı, ilişkili eylem grubunda yapılandırılmış tüm Web kancaları için kullanılmak üzere JSON biçimi belirtebilirsiniz; Web kancası biçimleri hakkında daha fazla bilgi için bkz. [günlük uyarıları için Web kancası eylemi](monitor-alerts-unified-log-webhook.md). Örnek JSON verileri kullanarak biçimini denetlemek için Görünümü Web kancası seçeneği sağlanır.

        ![Günlük uyarıları için eylem geçersiz kılmaları](media/alert-log/AlertsPreviewOverrideLog.png)


1. Tüm alanları geçerliyse ve yeşil onay **uyarı kuralı oluşturma** düğmesini ve Azure İzleyici - uyarılar bir uyarı oluşturulur. Tüm uyarıları Pano uyarılardan görüntülenebilir.

    ![Kural oluşturma](media/alert-log/AlertsPreviewCreate.png)

    Birkaç dakika içinde uyarı etkin ve daha önce açıklandığı gibi tetikler.

Kullanıcılar ayrıca kendi analytics sorgunuzda kesin [Azure portalında günlüklerini analiz sayfası](../log-analytics/log-analytics-log-search-portals.md#log-analytics-page
) ve Ayarla'uyarı ' düğmesiyle - bir uyarı oluşturmak için anında iletme sonra adım 6'dan başlayarak yukarıdaki öğreticide talimatları.

 ![Log Analytics - uyarı ayarlama](media/alert-log/AlertsAnalyticsCreate.png)

### <a name="view--manage-log-alerts-in-azure-portal"></a>Azure portalında günlük uyarıları yönetme & Görüntüle

1. İçinde [portalı](https://portal.azure.com/)seçin **İzleyici** ve izleme bölümü altında - **uyarılar**.  

1. **Uyarılar Panosu** görüntülenen - gibi tüm Azure Uyarıları'nı (günlük uyarılar dahil) bir tekil panosunda görüntülenir; her örneği, günlük uyarı kuralı ne zaman dahil tetiklendi. Daha fazla bilgi için bkz. [uyarı Yönetimi](https://aka.ms/managealertinstances).
    > [!NOTE]
    > Günlük uyarı kuralı oluşturan kullanıcılar tarafından sağlanan özel sorgu tabanlı mantığı ve bu nedenle çözümlenmiş duruma olmadan. Günlük uyarı kuralında belirtilen koşullar karşılandığında her zaman, nedeniyle harekete geçirilir. 


1. Seçin **yönetme kuralları** oluşturulan uyarı kurallarının tümünü burada listelenen kural Yönetim bölümüne - gitmek için üst taraftaki çubukta, düğmesine; devre dışı bırakıldı uyarıları da dahil olmak üzere.
    ![ Uyarı kurallarını yönet](media/alert-log/manage-alert-rules.png)

## <a name="managing-log-alerts-using-azure-resource-template"></a>Azure kaynak şablonu kullanarak günlük uyarıları yönetme
Şu anda günlük uyarı oluşturulabilmesi için iki farklı kaynak şablonlarını kullanarak hangi analytics platformu üzerine uyarı (yani) Log Analytics veya Application Insights temel dayanır.

Bu nedenle aşağıdaki bölümde ayrıntıları sağlayın kaynak şablonu için günlük uyarıları için her bir analiz platformu kullanarak.

### <a name="azure-resource-template-for-log-analytics"></a>Log Analytics için Azure kaynak şablonu
Günlük uyarıları için Log Analytics kayıtlı bir aramayı düzenli aralıklarla çalıştıran uyarı kuralları tarafından oluşturulur. Sorgu eşleşmenin sonuçlarını ölçütleri belirtilirse, bir uyarı kaydı oluşturulur ve bir veya daha fazla Eylemler çalıştırılır. 

Kaynak şablonu Log analytics arama ve Log analytics uyarılarını kayıtlı için Log Analytics belgeleri bölümünde kullanılabilir. Daha fazla bilgi için bkz, [ekleme Log Analytics kayıtlı aramaları ve Uyarıları](../azure-monitor/insights/solutions-resources-searches-alerts.md); yalnızca tanım örnekleri ve bunun yanı sıra şema ayrıntılarını içerir.

### <a name="azure-resource-template-for-application-insights"></a>Application Insights için Azure kaynak şablonu
Application Insights kaynakları için günlük uyarı sahip bir tür `Microsoft.Insights/scheduledQueryRules/`. Bu kaynak türü hakkında daha fazla bilgi için bkz. [Azure İzleyici - zamanlanmış sorgu kuralları API Başvurusu](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/).

Aşağıdakiler için yapısıdır [zamanlanmış sorgu kuralı oluşturma](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/createorupdate) değişkenleri olarak örnek veri kümesiyle kaynak şablonu temel.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0", 
    "parameters": {      
    },   
    "variables": {
    "alertLocation": "southcentralus",
    "alertName": "samplelogalert",
    "alertTag": "hidden-link:/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/components/sampleAIapplication",
    "alertDesription": "Sample log search alert",
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
       "description": "[variables('alertDesription')]",
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
   }
 ]
}
```
> [!IMPORTANT]
> Hedef kaynak için gizli-bağlantı etiketi alanıyla kullanımını zorunlu [zamanlanmış sorgu kuralları ](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/) API çağrısı veya kaynak şablonu. 

Yukarıdaki örnek json sampleScheduledQueryRule.json amacıyla bu izlenecek yolda (örneğin) olarak kaydedilebilir ve kullanılarak dağıtılabilir [Azure portalında Azure Resource Manager](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template).


## <a name="managing-log-alerts-using-powershell-cli-or-api"></a>PowerShell, CLI veya API kullanarak günlük uyarıları yönetme
Şu anda günlük uyarı oluşturulabilmesi için iki farklı Resource Manager uyumlu API'leri kullanarak hangi analytics platformu üzerine uyarı (yani) Log Analytics veya Application Insights temel dayanır.

Bu nedenle aşağıdaki bölümde ayrıntıları sağlayın API, Powershell veya CLI aracılığıyla günlük uyarıları için her bir analiz platformu kullanarak.

### <a name="powershell-cli-or-api-for-log-analytics"></a>PowerShell, CLI veya Log Analytics için API
Log Analytics uyarı REST API, RESTful olduğu ve Azure Resource Manager REST API aracılığıyla erişilebilir. API, böylece bir PowerShell komut satırından erişilebilir ve sonuçları programlı olarak birçok farklı şekilde kullanmanıza olanak sağlayan, arama sonuçları JSON biçiminde çıkarır.

Daha fazla bilgi edinin [oluşturmak ve REST API ile Log analytics'teki uyarı kuralları yönetmek](../azure-monitor/platform/api-alerts.md)Powershell'den API'sine erişim örnekler dahil.

### <a name="powershell-cli-or-api-for-application-insights"></a>PowerShell, CLI veya Application ınsights API
[Azure İzleyici - zamanlanmış sorgu kuralları API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/) REST API ve Azure Resource Manager REST API'si ile tamamen uyumlu. Bu nedenle Azure CLI yanı sıra, Resource Manager cmdlet'ini kullanarak Powershell kullanılabilir.

Kaynak şablonu (sampleScheduledQueryRule.json) daha önce gösterilen örnek için Azure Resource Manager PowerShell cmdlet'i aracılığıyla kullanımı aşağıda gösterilen [kaynak şablon bölümü](#azure-resource-template-for-application-insights) :
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile "D:\Azure\Templates\sampleScheduledQueryRule.json"
```
Azure Resource Manager kaynak şablonu (sampleScheduledQueryRule.json) daha önce gösterilen örnek için Azure CLI komutu aracılığıyla kullanımı aşağıda gösterilen [kaynak şablon bölümü](#azure-resource-template-for-application-insights) :

```azurecli
az group deployment create --resource-group myRG --template-file sampleScheduledQueryRule.json
```
Başarılı bir işlem 201 durum yeni uyarı kuralı oluşturma döndürülecek veya mevcut bir uyarı kuralı değiştirilmişse, 200 döndürülür.


  
## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [oturum uyarılar Azure uyarıları](monitor-alerts-unified-log.md)
* Anlamak [günlük uyarıları için Web kancası eylemleri](monitor-alerts-unified-log-webhook.md)
* Daha fazla bilgi edinin [Application Insights](../application-insights/app-insights-analytics.md)
* Daha fazla bilgi edinin [Log Analytics](../azure-monitor/log-query/log-query-overview.md). 

