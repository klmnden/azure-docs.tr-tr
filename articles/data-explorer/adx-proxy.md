---
title: Azure İzleyici'de Azure Veri Gezgini (Önizleme) kullanarak verileri Sorgulama
description: Application Insights ve Log Analytics ile bir Azure Veri Gezgini Ara sunucusunu çapraz ürün sorguları için oluşturarak bu konu başlığında, Azure İzleyici'de veri sorgulama
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: f363e59e6faa6b115eb40a2a5d35432f02299d52
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67800217"
---
# <a name="query-data-in-azure-monitor-using-azure-data-explorer-preview"></a>Azure İzleyici'de Azure Veri Gezgini (Önizleme) kullanarak verileri Sorgulama

Azure Veri Gezgini Ara sunucu kümesi (ADX Proxy), Azure Veri Gezgini arasında çapraz ürün sorguları gerçekleştirmeyi sağlayan bir varlıktır [Application Insights (AI)](/azure/azure-monitor/app/app-insights-overview), ve [Log Analytics (on)](/azure/azure-monitor/platform/data-platform-logs) içinde [Azure İzleyici](/azure/azure-monitor/) hizmeti. Azure İzleyici Log Analytics çalışma alanları veya Application Insights uygulamaları proxy küme olarak eşleyebilirsiniz. Ardından, Azure Veri Gezgini araçlarını kullanarak proxy küme sorgulama ve çapraz küme sorguda başvurduğu. Bu makalede, bir proxy kümeye bağlanmak, Azure Veri Gezgini Web kullanıcı Arabirimi için bir proxy küme ekleyin ve Azure veri Gezgini'nde, yapay ZEKA uygulamaları veya LA çalışma alanları karşı sorguları çalıştırmak gösterilmektedir.

Azure Veri Gezgini proxy akış: 

![ADX proxy akış](media/adx-proxy/adx-proxy-flow.png)

## <a name="prerequisites"></a>Önkoşullar

> [!NOTE]
> ADX Proxy Önizleme modundadır. Bu özelliği etkinleştirmek için kişi [ADXProxy](mailto:adxproxy@microsoft.com) takım.

## <a name="connect-to-the-proxy"></a>Proxy sunucusuna bağlanmak

1. Azure Veri Gezgini yerel kümenizi doğrulayın (gibi *yardımcı* kümesi), Log Analytics veya Application Insights kümeye bağlamadan önce sol taraftaki menüde görünür.

    ![Yerel küme ADX](media/adx-proxy/web-ui-help-cluster.png)

1. Azure Veri Gezgini kullanıcı arabiriminde (https://dataexplorer.azure.com/clusters) seçin **küme Ekle**.

1. İçinde **küme Ekle** penceresi:

    * URL, LA veya AI kümeye ekleyin. Örneğin, `https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>`

    * **Add (Ekle)** seçeneğini belirleyin.

    ![Küme ekleme](media/adx-proxy/add-cluster.png)

    Birden fazla proxy kümeye bağlantı eklerseniz, her farklı bir ad verin. Aksi halde bunların tümü aynı adı sol bölmede gerekir.

1. Bağlantı kurulduktan sonra LA veya AI kümenizi yerel ADX kümenizle sol bölmesinde görünür. 

    ![Log Analytics ve Azure Veri Gezgini kümeleri](media/adx-proxy/la-adx-clusters.png)

## <a name="run-queries"></a>Sorgu çalıştırma

Kusto Gezgini, Jupyter Kqlmagic veya REST API'yi ADX web Gezgini Ara sunucu kümeleri sorgulamak için kullanabilirsiniz. 

> [!TIP]
> * Veritabanı adı proxy kümede belirtilen kaynak adıyla aynı olmalıdır. Adları büyük/küçük harfe duyarlıdır.
> * İçinde arası küme sorguları, emin olun [uygulamalar ve çalışma alanlarını adlandırma](#application-insights-app-and-log-analytics-workspace-names) doğrudur.

### <a name="query-against-the-native-azure-data-explorer-cluster"></a>Yerel bir Azure Veri Gezgini kümesine göre sorgulama 

Azure Veri Gezgini kümenizde sorguları çalıştırma (gibi *StormEvents* tablosundaki *yardımcı* kümesi). Sorgu çalıştırırken, yerel Azure Veri Gezgini kümenizi sol bölmede seçili olduğunu doğrulayın.

```kusto
StormEvents | take 10 // Demonstrate query through the native ADX cluster
```

![Sorgu StormEvents tablosu](media/adx-proxy/query-adx.png)

### <a name="query-against-your-la-or-ai-cluster"></a>LA veya AI kümenizi karşı sorgulama

LA veya AL kümenizde sorguları çalıştırdığınızda, LA veya AI kümenizi sol bölmede seçili olduğunu doğrulayın. 

```kusto
Perf | take 10 // Demonstrate query through the proxy on the LA workspace
```

![Sorgu LA çalışma](media/adx-proxy/query-la.png)

### <a name="query-your-la-or-ai-cluster-from-the-adx-proxy"></a>LA veya AI kümenizi ADX Proxy'den gelen sorgu  

Ara sunucuya LA veya AI kümesinde sorguları çalıştırdığınızda, ADX yerel kümenizi sol bölmede seçili doğrulayın. Aşağıdaki örnek, bir sorgu kullanarak yerel ADX kümesi LA çalışma alanının gösterir.

```kusto
cluster('https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>').database('<workspace-name').Perf
| take 10 
```

![Azure Veri Gezgini Ara sunucuya sorgulama](media/adx-proxy/query-adx-proxy.png)

### <a name="cross-query-of-la-or-ai-cluster-and-the-adx-cluster-from-the-adx-proxy"></a>Sorgu LA veya AI kümesi ve ADX kümenin ADX Ara sunucuya arası 

Ara sunucuya çapraz küme sorguları çalıştırdığınızda, ADX yerel kümenizi sol bölmede seçili doğrulayın. Aşağıdaki örnekler ADX birleştirme göstermektedir küme tabloları (kullanarak `union`) LA çalışma alanıyla.

```kusto
union StormEvents, cluster('https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>').database('<workspace-name>').Perf
| take 10 
```

```kusto
let CL1 = 'https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>';
union <ADX table>, cluster(CL1).database(<workspace-name>).<table name>
```

![Azure Veri Gezgini Ara sunucuya sorgu arası](media/adx-proxy/cross-query-adx-proxy.png)

Kullanarak [ `join` işleci](/azure/kusto/query/joinoperator), UNION yerine, Azure Veri Gezgini ile yerel bir küme (ve proxy üzerinde değil) çalıştırmak için bir ipucu gerektirebilir. 

## <a name="additional-syntax-examples"></a>Ek söz dizimi örnekleri

Application Insights (AI) veya Log Analytics (on) kümeleri çağrılırken aşağıdaki söz dizimini seçenekleri kullanılabilir:

|Söz dizimi açıklaması  |Application Insights  |Log Analytics  |
|----------------|---------|---------|
| Bu abonelik yalnızca tanımlı kaynak içeren bir küme içindeki veritabanı (**çapraz küme sorgular için önerilen**) |   Küme (`https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<ai-app-name>').database('<ai-app-name>`) | Küme (`https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>').database('<workspace-name>`)     |
| Tüm uygulamalar/bu abonelikte çalışma alanı içeren kümesi    |     Küme (`https://ade.applicationinsights.io/subscriptions/<subscription-id>`)    |    Küme (`https://ade.loganalytics.io/subscriptions/<subscription-id>`)     |
|Abonelikteki tüm uygulamalar/çalışma içeren ve bu kaynak grubunun üyesi olan bir küme    |   Küme (`https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>`)      |    Küme (`https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>`)      |
|Bu abonelik yalnızca tanımlı kaynak içeren kümesi      |    Küme (`https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<ai-app-name>`)    |  Küme (`https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>`)     |

### <a name="application-insights-app-and-log-analytics-workspace-names"></a>Application Insights uygulaması ve Log Analytics çalışma alanı adları

* Adları özel karakterler içeriyorsa, bunlar URL proxy küme adında kodlama yerine. 
* Adları sağlamayan karakterler içeriyorsa [KQL tanımlayıcı adı kuralları](/azure/kusto/query/schema-entities/entity-names), yok, kısa çizgi yerine **-** karakter.

## <a name="next-steps"></a>Sonraki adımlar

[Sorgu yazma](write-queries.md)