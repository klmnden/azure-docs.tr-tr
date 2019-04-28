---
title: Uyarı sorguları Azure İzleyicisi'nde oturum açın | Microsoft Docs
description: Azure İzleyici güncelleştirmeleri ve var olan sorguları dönüştürmek için bir işlem de günlük uyarıları için etkili sorgular yazmaya öneriler sağlar.
author: yossi-y
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: bwren
ms.subservice: alerts
ms.openlocfilehash: 429770b7651a93473c03f5e386d8f7b72692c161
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60996020"
---
# <a name="log-alert-queries-in-azure-monitor"></a>Azure İzleyici'de günlük uyarı sorguları
[Uyarı kuralları Azure İzleyici günlüklerine göre](alerts-unified-log.md) düzenli aralıklarla çalıştırın, böylece emin olmanız gerekir, ek yükü ve gecikme süresini en aza indirmek için yazılır. Bu makalede, günlük uyarıları için etkili sorgular ve var olan sorguları dönüştürmek için bir işlem yazmaya öneriler sağlar. 

## <a name="types-of-log-queries"></a>Günlük sorgu türleri
[Azure İzleyicisi'nde sorgular oturum](../log-query/log-query-overview.md) bir tablo ile başlayın veya [arama](/azure/kusto/query/searchoperator) veya [birleşim](/azure/kusto/query/unionoperator) işleci.

Örneğin aşağıdaki sorguyu kapsamı _SecurityEvent_ tablo ve belirli olay kimliğine arar Bu sorgu işlemesi gerekir, yalnızca bir tablodur.

``` Kusto
SecurityEvent | where EventID == 4624 
```

İle başlayan sorguları `search` veya `union` bir tabloda birden fazla sütun veya bile birden çok tablo arasında arama olanak sağlar. Arama terimi birden çok yöntem aşağıdaki örnekler _bellek_:

```Kusto
search "Memory"
search * | where == "Memory"
search ObjectName: "Memory"
search ObjectName == "Memory"
union * | where ObjectName == "Memory"
```

Ancak `search` ve `union` olan veri keşfi, tüm veri modeli üzerinde koşulları arama sırasında bunlar bir tablo arasında birden çok tablo tarama gerekir bu yana kullanmaktan daha az verimli yararlıdır. Uyarı kuralları sorgularda düzenli aralıklarla çalıştırıldığından, gecikme süresi için uyarı ekleme aşırı ek yükten sonuçlanabilir. Bu ek yükü nedeniyle, azure'da günlük uyarısı kuralları için sorgular her zaman sorgu performansı hem sonuçlarının ilgi geliştiren bir Temizle kapsamını tanımlamak için bir tablo ile başlamanız gerekir.

## <a name="unsupported-queries"></a>Desteklenmeyen sorgu
Ocak ayından başlayarak oluştururken veya değiştirirken kullanın günlük uyarı kuralları 11,2019 `search`, veya `union` işleçleri kullanılamaz desteklenen içinde Azure portalı. Bu işleçler bir uyarı kuralı kullanarak bir hata iletisi döndürür. Mevcut uyarı kuralları ve oluşturulup düzenlenebilir günlük analizi API'sini kullanarak uyarı kuralları, bu değişiklikten etkilenmez. Yine de olsa, verimliliği artırmak için bu sorgu türleri kullanan herhangi bir uyarı değiştirme kuralları dikkate almanız gerekir.  

Günlük uyarısı kuralları kullanarak [kaynaklar arası sorgular](../log-query/cross-workspace-query.md) kaynaklar arası sorgular kullandığından bu değişiklikten etkilenmez `union`, belirli kaynaklara sorgu kapsamını sınırlar. Bu, eşdeğer değil `union *` hangi kullanılamaz.  Aşağıdaki örnek, bir günlük uyarı kuralı geçerli olacaktır:

```Kusto
union 
app('Contoso-app1').requests, 
app('Contoso-app2').requests, 
workspace('Contoso-workspace1').Perf 
```

>[!NOTE]
>[Kaynaklar arası sorgu](../log-query/cross-workspace-query.md) günlüğünde uyarı desteklenen yeni [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules). Varsayılan olarak, Azure İzleyici kullanır [eski Log Analytics uyarı API](api-alerts.md) gelen geçiş yapmadığınız sürece, yeni günlük uyarı kuralları Azure portalından oluşturmak için [eski günlük uyarıları API](alerts-log-api-switch.md#process-of-switching-from-legacy-log-alerts-api). Anahtardan sonra yeni API, Azure portalında yeni uyarı kuralları için varsayılan olur ve kaynaklar arası sorgu günlük uyarı kuralları oluşturmanıza olanak tanır. Oluşturabileceğiniz [kaynaklar arası sorgu](../log-query/cross-workspace-query.md) günlük uyarısı kuralları kullanarak geçiş yapmaya gerek olmadan [scheduledQueryRules API için ARM şablonu](alerts-log.md#log-alert-with-cross-resource-query-using-azure-resource-template) – ancak yine de bu uyarı kuralı yönetilebilir [ API scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) ve Azure Portal'da değil.

## <a name="examples"></a>Örnekler
Örnek olarak şunlar gösterilebilir kullanan günlük sorguları `search` ve `union` ve uyarı kuralları ile kullanmak için bu sorguları değiştirmek için kullanabileceğiniz adımlar sağlar.

### <a name="example-1"></a>Örnek 1
Performans bilgileri kullanarak alan aşağıdaki sorguyu kullanarak günlük uyarı kuralı oluşturmak istediğiniz `search`: 

``` Kusto
search * | where Type == 'Perf' and CounterName == '% Free Space' 
| where CounterValue < 30 
| summarize count()
```
  

Bu sorguyu değiştirmek için özellikler ait tablo tanımlamak için aşağıdaki sorguyu kullanarak başlatın:

``` Kusto
search * | where CounterName == '% Free Space'
| summarize by $table
```
 

Bu sorgunun sonucu, gösterebilir _CounterName_ özelliği geldiği _Perf_ tablo. 

Bu sonuç için uyarı kuralı kullanacağınız aşağıdaki sorguyu oluşturmak için kullanabilirsiniz:

``` Kusto
Perf 
| where CounterName == '% Free Space' 
| where CounterValue < 30 
| summarize count()
```


### <a name="example-2"></a>Örnek 2
Performans bilgileri kullanarak alan aşağıdaki sorguyu kullanarak günlük uyarı kuralı oluşturmak istediğiniz `search`: 

``` Kusto
search ObjectName =="Memory" and CounterName=="% Committed Bytes In Use"  
| summarize Avg_Memory_Usage =avg(CounterValue) by Computer 
| where Avg_Memory_Usage between(90 .. 95)  
| count 
```
  

Bu sorguyu değiştirmek için özellikler ait tablo tanımlamak için aşağıdaki sorguyu kullanarak başlatın:

``` Kusto
search ObjectName=="Memory" and CounterName=="% Committed Bytes In Use" 
| summarize by $table 
```
 

Bu sorgunun sonucu, gösterebilir _ObjectName_ ve _CounterName_ özelliği geldiği _Perf_ tablo. 

Bu sonuç için uyarı kuralı kullanacağınız aşağıdaki sorguyu oluşturmak için kullanabilirsiniz:

``` Kusto
Perf 
| where ObjectName =="Memory" and CounterName=="% Committed Bytes In Use" 
| summarize Avg_Memory_Usage=avg(CounterValue) by Computer 
| where Avg_Memory_Usage between(90 .. 95)  
| count 
```
 

### <a name="example-3"></a>Örnek 3

Her ikisi de kullanan aşağıdaki sorguyu kullanarak günlük uyarı kuralı oluşturmak istediğiniz `search` ve `union` performans bilgilerini almak için: 

``` Kusto
search (ObjectName == "Processor" and CounterName == "% Idle Time" and InstanceName == "_Total")  
| where Computer !in ((union * | where CounterName == "% Processor Utility" | summarize by Computer))
| summarize Avg_Idle_Time = avg(CounterValue) by Computer|  count  
```
 

Bu sorguyu değiştirmek için sorgu ilk bölümünü özelliklerinde ait tablosu tanımlamak için aşağıdaki sorguyu kullanarak başlatın: 

``` Kusto
search (ObjectName == "Processor" and CounterName == "% Idle Time" and InstanceName == "_Total")  
| summarize by $table 
```

Bu sorgu sonucunu, tüm bu özellikler geldiğini gösterebilir _Perf_ tablo. 

Artık `union` ile `withsource` hangi kaynak tablosu tanımlamak için komut her satır katkıda bulundu.

``` Kusto
union withsource=table * | where CounterName == "% Processor Utility" 
| summarize by table 
```
 

Bu özellikler ayrıca geldiğini bu sorgunun sonucu gösterebilir _Perf_ tablo. 

Uyarı kuralı için kullanacağınız aşağıdaki sorguyu oluşturmak için bu sonuçları kullanabilirsiniz: 

``` Kusto
Perf 
| where ObjectName == "Processor" and CounterName == "% Idle Time" and InstanceName == "_Total" 
| where Computer !in ( 
    (Perf 
    | where CounterName == "% Processor Utility" 
    | summarize by Computer)) 
| summarize Avg_Idle_Time = avg(CounterValue) by Computer 
| count 
``` 

### <a name="example-4"></a>Örnek 4
İki sonuçları birleştirir aşağıdaki sorguyu kullanarak günlük uyarı kuralı oluşturmak istediğiniz `search` sorgular:

```Kusto
search Type == 'SecurityEvent' and EventID == '4625' 
| summarize by Computer, Hour = bin(TimeGenerated, 1h) 
| join kind = leftouter ( 
    search in (Heartbeat) OSType == 'Windows' 
    | summarize arg_max(TimeGenerated, Computer) by Computer , Hour = bin(TimeGenerated, 1h) 
    | project Hour , Computer  
)  
on Hour 
| count 
```
 

Sorguyu değiştirmek için birleştirmenin sol tarafındaki özellikleri içeren bir tablo tanımlamak için aşağıdaki sorguyu kullanarak başlatın: 

``` Kusto
search Type == 'SecurityEvent' and EventID == '4625' 
| summarize by $table 
```
 

Sonuç birleştirmenin sol tarafındaki özelliklerinde ait olduğunu gösterir. _SecurityEvent_ tablo. 

Artık birleştirme sağ tarafında özellikleri içeren bir tablo tanımlamak için aşağıdaki sorguyu kullanın: 

 
``` Kusto
search in (Heartbeat) OSType == 'Windows' 
| summarize by $table 
```

 
Birleşimin sağ tarafındaki özelliklerinde sinyal tabloya ait sonucu gösterir. 

Uyarı kuralı için kullanacağınız aşağıdaki sorguyu oluşturmak için bu sonuçları kullanabilirsiniz: 


``` Kusto
SecurityEvent
| where EventID == '4625'
| summarize by Computer, Hour = bin(TimeGenerated, 1h)
| join kind = leftouter (
    Heartbeat  
    | where OSType == 'Windows' 
    | summarize arg_max(TimeGenerated, Computer) by Computer , Hour = bin(TimeGenerated, 1h) 
    | project Hour , Computer  
)  
on Hour 
| count 
```

## <a name="next-steps"></a>Sonraki adımlar
- Hakkında bilgi edinin [günlük uyarıları](alerts-log.md) Azure İzleyici'de.
- Hakkında bilgi edinin [oturum sorguları](../log-query/log-query-overview.md).

