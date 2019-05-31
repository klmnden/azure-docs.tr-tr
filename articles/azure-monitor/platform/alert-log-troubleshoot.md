---
title: Azure İzleyici'de günlük uyarıları gidermek | Microsoft Docs
description: Genel sorunları, sorunlar ve çözümleri Azure günlük uyarı kuralları için.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: 8b1a9b3dee999a35950559a049230f7fdbbc47b6
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66399179"
---
# <a name="troubleshoot-log-alerts-in-azure-monitor"></a>Azure İzleyici'de günlük uyarıları giderme  

Bu makalede, Azure İzleyici'de günlük uyarıları ayarlama, olabileceği yaygın sorunları çözme işlemini göstermektedir. Ayrıca, işlev veya günlük uyarıları yapılandırma ile sık karşılaşılan sorunlara çözümler sağlar. 

Terim *günlük uyarıları* yangın günlük sorguda bağlı uyarılar açıklar bir [Azure Log Analytics çalışma alanı](../learn/tutorial-viewdata.md) veya [Azure Application Insights](../../azure-monitor/app/analytics.md). Daha fazla ilgili işlevler, terminolojisi ve türlerinde öğrenin [Azure İzleyici'de günlük uyarıları](../platform/alerts-unified-log.md).

> [!NOTE]
> Bu makalede, burada Azure portalında tetiklenen bir uyarı kuralı gösterir ve bir bildirim, bir ilişkili eylem grubu tarafından gerçekleştirilmeyen durumları dikkate almaz. Böyle durumlarda, ayrıntılara bakın [oluştur ve Eylem grupları Azure portalında yönetmek](../platform/action-groups.md).

## <a name="log-alert-didnt-fire"></a>Günlük uyarı yangın gelmedi

İşte bazı yaygın nedenler neden durum için yapılandırılmış bir [Azure İzleyici'de günlük uyarı kuralı](../platform/alerts-log.md) göstermez [olarak *harekete* beklendiğinde](../platform/alerts-managing-alert-states.md). 

### <a name="data-ingestion-time-for-logs"></a>Günlükler için veri alım zamanı

Günlük uyarısı sorgunuzu göre düzenli aralıklarla çalışan [Log Analytics](../learn/tutorial-viewdata.md) veya [Application Insights](../../azure-monitor/app/analytics.md). Azure İzleyici, binlerce müşteri çeşitli kaynaklardan gelen verileri terabayta kadar dünya genelindeki işlediğinden, hizmet değişen zaman gecikmeleri saldırılara açıktır. Daha fazla bilgi için [veri alımı zaman Azure İzleyici günlüklerine](../platform/data-ingestion-time.md).

Gecikmelerini azaltmak için sistem bekler ve uyarı sorgusu, gerekli verileri değil henüz alınır bulursa, birden çok kez yeniden dener. Sistem ayarlamak üssel olarak artan bir bekleme süresi vardır. Günlük uyarı gecikmesi nedeniyle yavaş alımı günlük verisi olabilir, böylece verileri yalnızca kullanılabilir olduktan sonra tetiklenir. 

### <a name="incorrect-time-period-configured"></a>Süre yanlış yapılandırılmış

Makalesinde açıklandığı [günlük uyarıları için terimler](../platform/alerts-unified-log.md#log-search-alert-rule---definition-and-types), yapılandırmada belirtilen süre sorgu için zaman aralığını belirtir. Sorgu yalnızca bu aralığı içinde oluşturulmuş olan kayıtları döndürür. 

Kötüye kullanımı önlemek günlük sorgusu için alınan verileri zaman aralığını kısıtlar ve hiçbir zaman komut bozar (gibi **önce**) bir günlük sorguda kullanılan. Örneğin, zaman aralığı 60 dakika olarak ayarlanmıştır ve sorgu 13: 15'te çalıştırılan 12:15 PM arasında 13: 15'te oluşturulan kayıtları için günlük sorgusu kullanılır. Günlük sorgu süresi komutu gibi kullanıyorsa **önce (1d)** , bu aralık için zaman dilimini ayarlandığından sorgu hala yalnızca 13: 15'te 12:15 PM arasında verileri kullanır.

Yapılandırma dönemde sorgunuzu eşleşip eşleşmediğini denetleyin. Daha önce gösterilen örnek için günlük sorgusu kullanıyorsa **önce (1d)** yeşil işaretleyiciyle zaman aralığı 24 saat veya 1,440 dakika (kırmızı renkte gösterilir) olarak ayarlanmalıdır. Bu ayar, sorgu beklendiği gibi çalışmasını sağlar.

![Süre](media/alert-log-troubleshoot/LogAlertTimePeriod.png)

### <a name="suppress-alerts-option-is-set"></a>Seçenek kümesi uyarıları bastır

Makalenin 8 adımda açıklandığı [Azure portalında günlük uyarı kuralı oluşturma](../platform/alerts-log.md#managing-log-alerts-from-the-azure-portal), günlük uyarıları sağlayan bir **uyarıları bastır** seçeneği için yapılandırılmış bir süre tetikleme ve bildirim eylemlerini gizlemek için zamanı. Sonuç olarak, bir uyarı yangın yaramadı düşünebilirsiniz. Aslında, harekete ancak engellendi.  

![Uyarıları bastır](media/alert-log-troubleshoot/LogAlertSuppress.png)

### <a name="metric-measurement-alert-rule-is-incorrect"></a>Ölçüm ölçüsü uyarı kuralı yanlış

*Ölçüm ölçüsü günlük uyarıları* özel özellikleri ve kısıtlı uyarı sorgusu söz dizimi olan günlük uyarı alt olan. Ölçüm ölçüsü günlük uyarısı için bir kural, bir ölçüm zaman serisi olmasını çıkış sorgu gerektirir. Diğer bir deyişle, çıkış ayrı, eşit boyutlu zaman dilimlerine karşılık gelen toplam değerler ile birlikte içeren bir tablodur. 

Tabloda yanı sıra bir ek değişkeni sahip olmadığınıza **AggregatedValue**. Tabloyu sıralamak için bu değişkenleri kullanılabilir. 

Örneğin, bir ölçüm ölçüsü günlüğü uyarısı için bir kural olarak yapılandırılmışsa varsayalım:

- Sorgu `search *| summarize AggregatedValue = count() by $table, bin(timestamp, 1h)`  
- 6 saatlik süre
- 50 eşiği
- üç ardışık ihlaller uyarı mantığı
- **Toplam bağlı** olarak seçilen **$table**

Komut içerdiğinden **özetlemek... tarafından** ve iki değişken sağlar (**zaman damgası** ve **$table**), sistem seçer **$table** için **üzerinde toplama** . Sistem göre sonuç tabloyu sıralar **$table** , aşağıdaki ekran görüntüsünde gösterildiği gibi alan. Çok sınayıp **AggregatedValue** örnekleri her tablo türü için (gibi **availabilityResults**) üç veya daha fazla ardışık ihlaller olup olmadığını görmek için.

![Ölçüm ölçüsü sorgu yürütme ile birden çok değer](media/alert-log-troubleshoot/LogMMQuery.png)

Çünkü **üzerinde toplama** tanımlanan **$table**, veri çubuğunda sıralanmış bir **$table** sütun (kırmızı renkte gösterilir). Grup ve türleri için konum **üzerinde toplama** alan. 

Örneğin, **$table**, değerleri **availabilityResults** bir çizim/varlık (turuncu renkte gösterilir) olarak kabul edilir. Bu değeri Çizdirmek/varlıkta, uyarı hizmetinin (yeşil renkte gösterilir) üç ardışık ihlaller denetler. İhlallerini Tablo değeri için bir uyarı tetiklemek **availabilityResults**. 

Benzer şekilde, üç ardışık ihlaller herhangi diğer değerini seçerseniz **$table**, başka bir uyarı bildirimi aynı şeyi tetiklenir. Uyarı hizmetinin, bir çizim/varlık (turuncu renkte gösterilir) değerleri otomatik olarak zamanına göre sıralar.

Ölçüm ölçüsü günlük uyarı kuralı değiştirildiği ve sorgu şimdi varsayalım `search *| summarize AggregatedValue = count() by bin(timestamp, 1h)`. Üç ardışık ihlaller için uyarı mantığı eklemeden önce yapılandırmayı geri kalanı aynı kaldı. **Üzerinde toplama** seçeneği bu durumda, **zaman damgası** varsayılan olarak. Yalnızca bir değer için sorgu sağlanır **özetlemek... tarafından** (diğer bir deyişle, **zaman damgası**). Önceki örnekteki gibi çıkış yürütme sonuna aşağıdaki şekilde gösterildiği gibi olacaktır.

   ![Ölçüm ölçüsü tekil değerle sorgu yürütme](media/alert-log-troubleshoot/LogMMtimestamp.png)

Çünkü **üzerinde toplama** tanımlanan **zaman damgası**, veriler üzerinde sıralanır **zaman damgası** sütun (kırmızı renkte gösterilir). Biz gruplandırma ölçütü sonra **zaman damgası**. Örneğin, değerleri `2018-10-17T06:00:00Z` bir çizim/varlık (turuncu renkte gösterilir) olarak kabul edilir. Bu değeri Çizdirmek/varlıkta, uyarı hizmetinin hiçbir ardışık ihlaller bulabilirsiniz (çünkü her **zaman damgası** değerine sahip bir girdi). Bu nedenle uyarı asla tetiklenmez. Böyle bir durumda, kullanıcı aşağıdakilerden birini yapmalısınız:

- İşlevsiz bir değişken veya mevcut bir değişken ekleyin (gibi **$table**) kullanarak doğru şekilde sıralanması **üzerinde toplama** alan.
- Temel uyarı mantığı kullanmak için uyarı kuralı yeniden **toplam ihlal** yerine.

## <a name="log-alert-fired-unnecessarily"></a>Günlük uyarı gereksiz yere tetiklendi

Yapılandırılmış bir [Azure İzleyici'de günlük uyarı kuralı](../platform/alerts-log.md) beklenmedik bir şekilde içinde görüntülediğinizde tetiklenebilir [Azure uyarıları](../platform/alerts-managing-alert-states.md). Aşağıdaki bölümlerde, bazı yaygın nedenleri açıklanmaktadır.

### <a name="alert-triggered-by-partial-data"></a>Kısmi veriler tarafından tetiklenen uyarı

Log Analytics ve Application Insights alım gecikmeleri ve işleme için uygulanır. Günlük uyarı sorgusu çalıştırdığınızda hiçbir veri kullanılamıyor bulabilirsiniz veya yalnızca bazı veriler kullanılabilir. Daha fazla bilgi için [Azure İzleyici'de veri alma süresini oturum](../platform/data-ingestion-time.md).

Varsa veri veya kısmi veri günlüklerinde, uyarı yürütme sırasında uyarı kuralını nasıl yapılandırdığınıza bağlı olarak, misfiring gerçekleşebilir. Bu gibi durumlarda, biz, uyarı sorgusu veya yapılandırma değişiklik önerin. 

Örneğin, bir analytics sorgusunun sonuçlarından sayısı 5'ten az olduğunda tetiklenmesi için günlük uyarı kuralı yapılandırırsanız, hiçbir veri (sıfır kaydı) veya kısmi sonuçlar (bir kayıt) olduğunda uyarı tetiklenir. Ancak, veri alımı gecikme sonrasında tam veri ile aynı sorgu sonucu 10 kayıt sağlayabilir.

### <a name="alert-query-output-is-misunderstood"></a>Uyarı sorgusu çıkış yanlış

Log analytics sorgu uyarılara ilişkin mantığı sağlar. Analytics sorgusu, çeşitli büyük veri ve matematik işlevleri kullanabilirsiniz. Uyarı hizmetinin sorgunuzu verilerle belirli bir süre için belirtilen aralıklarla çalışır. Uyarı hizmetinin uyarı türüne göre sorgu görünümünde hafif değişiklikler yapar. Bu değişikliğe görüntüleyebileceğiniz **yürütülecek sorgu** bölümünde **sinyal mantığını yapılandırma** ekran:

![Yürütülecek sorgu](media/alert-log-troubleshoot/LogAlertPreview.png)

**Yürütülecek sorgu** kutusudur günlük uyarı hizmetinin çalıştırılanlar. Uyarı sorgusu olabilir bir uyarı oluşturmadan önce ne çıkış anlamak istiyorsanız, belirtilen sorgu ve zaman aralığı aracılığıyla çalıştırabilirsiniz [analiz portalı](../log-query/portals.md) veya [analizi API'si](https://docs.microsoft.com/rest/api/loganalytics/).

## <a name="log-alert-was-disabled"></a>Günlük uyarı devre dışı bırakıldı

Aşağıdaki bölümlerde neden Azure İzleyici devre dışı bazı nedenler listelenmiştir [günlük uyarı kuralı](../platform/alerts-log.md).

### <a name="resource-where-the-alert-was-created-no-longer-exists"></a>Uyarı artık oluşturulduğu kaynak var.

Azure İzleyici'de oluşturulan günlük uyarı kuralları bir Azure Log Analytics çalışma alanı, Azure Application ınsights'ı uygulama ve bir Azure kaynağı gibi belirli bir kaynağa hedefleyin. Günlük uyarı hizmetinin kuralda belirtilen hedef için sunulan bir analytics sorgusunu çalıştırılır. Ancak, kural oluşturulduktan sonra kullanıcılar genellikle Azure'dan--silin veya Azure--günlük uyarı kuralı hedefinin içinde taşımak için gidin. Uyarı kuralı hedefinin artık geçerli olmadığı için kuralın yürütülmesi başarısız oluyor.

Böyle durumlarda, Azure izleyici günlüğü uyarısı devre dışı bırakır ve kural (örneğin, bir hafta) hacimle dönem için sürekli olarak çalıştırılamaz, gereksiz yere faturalandırılırsınız değil, sağlar. Azure İzleyici aracılığıyla günlüğü Uyarısı ne zaman devre dışı kesin zaman bulabilirsiniz [Azure etkinlik günlüğü](../../azure-resource-manager/resource-group-audit.md). Azure İzleyici günlük uyarı kuralı devre dışı bıraktığında Azure etkinlik günlüğüne bir olay eklenir.

Aşağıdaki örnek Azure etkinlik günlüğü olayında, sürekli bir hata nedeniyle devre dışı bırakılmış bir uyarı kuralı içindir.

```json
{
    "caller": "Microsoft.Insights/ScheduledQueryRules",
    "channels": "Operation",
    "claims": {
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/ScheduledQueryRules"
    },
    "correlationId": "abcdefg-4d12-1234-4256-21233554aff",
    "description": "Alert: test-bad-alerts is disabled by the System due to : Alert has been failing consistently with the same exception for the past week",
    "eventDataId": "f123e07-bf45-1234-4565-123a123455b",
    "eventName": {
        "value": "",
        "localizedValue": ""
    },
    "category": {
        "value": "Administrative",
        "localizedValue": "Administrative"
    },
    "eventTimestamp": "2019-03-22T04:18:22.8569543Z",
    "id": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<ResourceGroup>/PROVIDERS/MICROSOFT.INSIGHTS/SCHEDULEDQUERYRULES/TEST-BAD-ALERTS",
    "level": "Informational",
    "operationId": "",
    "operationName": {
        "value": "Microsoft.Insights/ScheduledQueryRules/disable/action",
        "localizedValue": "Microsoft.Insights/ScheduledQueryRules/disable/action"
    },
    "resourceGroupName": "<Resource Group>",
    "resourceProviderName": {
        "value": "MICROSOFT.INSIGHTS",
        "localizedValue": "Microsoft Insights"
    },
    "resourceType": {
        "value": "MICROSOFT.INSIGHTS/scheduledqueryrules",
        "localizedValue": "MICROSOFT.INSIGHTS/scheduledqueryrules"
    },
    "resourceId": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<ResourceGroup>/PROVIDERS/MICROSOFT.INSIGHTS/SCHEDULEDQUERYRULES/TEST-BAD-ALERTS",
    "status": {
        "value": "Succeeded",
        "localizedValue": "Succeeded"
    },
    "subStatus": {
        "value": "",
        "localizedValue": ""
    },
    "submissionTimestamp": "2019-03-22T04:18:22.8569543Z",
    "subscriptionId": "<SubscriptionId>",
    "properties": {
        "resourceId": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<ResourceGroup>/PROVIDERS/MICROSOFT.INSIGHTS/SCHEDULEDQUERYRULES/TEST-BAD-ALERTS",
        "subscriptionId": "<SubscriptionId>",
        "resourceGroup": "<ResourceGroup>",
        "eventDataId": "12e12345-12dd-1234-8e3e-12345b7a1234",
        "eventTimeStamp": "03/22/2019 04:18:22",
        "issueStartTime": "03/22/2019 04:18:22",
        "operationName": "Microsoft.Insights/ScheduledQueryRules/disable/action",
        "status": "Succeeded",
        "reason": "Alert has been failing consistently with the same exception for the past week"
    },
    "relatedEvents": []
}
```

### <a name="query-used-in-a-log-alert-is-not-valid"></a>Günlük uyarıda kullanılan sorgu geçerli değil

Her günlük uyarı kuralı yapılandırmasına bir parçası olarak Azure İzleyici'de oluşturulan uyarı hizmetinin düzenli aralıklarla çalışacak bir analytics sorgusunun belirtmeniz gerekir. Analiz sorgusu doğru sözdizimi kuralı oluşturma veya güncelleştirme zaman olabilir. Ancak bazı durumlarda, bir süre, günlük uyarı kuralı içinde sağlanan sorgu söz dizimi sorunu geliştirebilir ve kural yürütme başarısız olmasına neden olur. Günlük uyarı kuralı sunulan bir analytics sorgusunu hataları neden geliştirebilirsiniz bazı yaygın nedenleri şunlardır:

- Sorgu yazılan [birden çok kaynaklarınız](../log-query/cross-workspace-query.md). Ve bir veya daha fazla belirtilen kaynakları artık yok.
- [Ölçüm ölçüsü türü günlüğü Uyarısı](../../azure-monitor/platform/alerts-unified-log.md#metric-measurement-alert-rules) yapılandırılmış bir uyarı sahip sorgu söz dizimi normları ile uyumlu değil
- Hiçbir veri akışını analiz platformu için oluştu. [Sorgu yürütme hata verir](https://dev.loganalytics.io/documentation/Using-the-API/Errors) olmadığı için sağlanan sorgu için veri yok.
- Değişiklikleri [sorgu dilini](https://docs.microsoft.com/azure/kusto/query/) komutlar ve İşlevler için gözden geçirilmiş bir biçim içerir. Bu nedenle daha önce bir uyarı kuralı içinde sağlanan sorgusu artık geçerli değil.

[Azure Danışmanı](../../advisor/advisor-overview.md) , bu davranışı hakkında sizi uyarır. Azure Danışmanı hakkında belirli günlük uyarı kuralı yüksek kullanılabilirliğe sahip, Orta etki kategorisini ve Açıklama "izleme sağlamak için onarım günlük uyarı kuralı." altında bir öneri eklendi Azure Danışmanı tarafından sağlanan bir öneri yedi gün sonra günlük uyarı kuralı bir uyarı sorgusu düzeltilebilir değil, Azure izleyici günlüğü uyarısı devre dışı bırakın ve kural sürekli olarak çalıştırılamaz, hacimle bir dönem (için gereksiz yere faturalandırılırsınız değil emin olun bir hafta gibi).

Azure İzleyici devre dışı bırakıldığında günlük uyarı kuralı bir olay için bakarak kesin zaman bulabilirsiniz [Azure etkinlik günlüğü](../../azure-resource-manager/resource-group-audit.md).

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [günlük uyarıları Azure'da](../platform/alerts-unified-log.md).
- Daha fazla bilgi edinin [Application Insights](../../azure-monitor/app/analytics.md).
- Daha fazla bilgi edinin [oturum sorguları](../log-query/log-query-overview.md).
