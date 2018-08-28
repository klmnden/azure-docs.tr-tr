---
title: Azure SQL veri ambarı yönetim ve izleme - sorgu etkinliği, kaynak kullanımını | Microsoft Docs
description: Hangi özelliklerin yönetmek ve Azure SQL veri ambarı izlemek kullanılabilir olduğunu öğrenin. Sorgu etkinliği ve veri Ambarınızı kaynak kullanımını anlamak için Azure portalı ve dinamik yönetim görünümlerini (Dmv'ler) kullanın.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 08/26/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 7ff304fa478942254cca372282a30a1a3f00f354
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43112974"
---
# <a name="monitoring-resource-utilization-and-query-activity-in-azure-sql-data-warehouse"></a>Azure SQL veri ambarı'nda kaynak kullanımı ve sorgu etkinliğini izleme
Azure SQL veri ambarı, Azure Portal, veri ambarı iş yükü için içgörüleri için zengin bir izleme deneyimi sağlar. Azure portalı, ölçüm ve günlükleri için yapılandırılabilir retentions dönemleri, uyarıları, öneriler ve özelleştirilebilir grafikler ve panolar sağladığı gibi veri Ambarınızı izlerken önerilen araç winres.exe'dir. Portal Ayrıca, diğer Azure izleme Hizmetleri gibi Operations Management Suite (OMS) ile tümleştirmenize olanak tanır / Log Analytics ve Azure İzleyici bütünsel bir izleme sağlamak için yalnızca veri ambarınızın aynı zamanda tüm Azure için deneyimi tümleşik bir izleme deneyimi için analiz platformu. Bu belgede, izleme hangi özellikleri en iyi duruma getirmek ve SQL veri ambarı ile analiz platformunuz yönetmek kullanılabilen açıklanmaktadır. 

## <a name="resource-utilization"></a>Kaynak kullanımı 
Aşağıdaki ölçümler, Azure portalında kullanılabilir.

| Ölçüm Adı                           | Açıklama     | Toplama Türü |
| --------------------------------------- | ---------------- | --------------------------------------- |
| CPU yüzdesi                          | Veri ambarı için tüm düğümlerde CPU kullanımı | Maksimum      |
| Veri G/Ç yüzdesi                      | Veri ambarı için tüm düğümlerde g/ç kullanımı | Maksimum   |
| Başarılı bağlantılar                  | Veriler başarılı bağlantı sayısı | Toplam            |
| Başarısız Bağlantılar                      | Başarısız veri ambarı bağlantı sayısı | Toplam            |
| Güvenlik Duvarı tarafından engellendi                     | Oturum açma engellendi veri ambarına sayısı | Toplam            |
| DWU limiti                              | Veri ambarı hizmet düzeyi hedefi | Maksimum   |
| DWU yüzdesi                          | En fazla CPU yüzdesi arasındaki veri g/ç yüzdesi | Maksimum   |
| Kullanılan DWU                                | DWU limiti * DWU yüzdesi | Maksimum   |
| Önbellek isabet yüzdesi | (önbellek isabet / isabetsizlik önbelleğe) * burada yerel SSD önbellekte tüm columnstore segmentleri isabetli okuma sayısının toplam önbellek isabet olduğu ve önbellek isabetsizliği columnstore segmentleri 100 kaçan tüm düğümlerde toplamı yerel SSD önbellekte | Maksimum |
| Kullanılan önbellek yüzdesi | (önbellek kullanılan / kapasite önbellek) * burada kullanılan önbellek yerel SSD önbellekte tüm baytların toplamından tüm düğümlerde ve önbellek kapasitesi kadar yerel SSD depolama kapasitesinin toplamı 100 tüm düğümlerde önbelleğe alma | Maksimum |

## <a name="query-activity"></a>Sorgu Etkinliği
SQL veri ambarı T-SQL aracılığıyla izlerken bir programlama deneyimi için hizmet dinamik yönetim görünümlerini (Dmv'ler) sunmaktadır. Bu görünümler, etkin olarak sorun giderme ve, iş yükü ile performans sorunlarını tanımlamak faydalıdır.

SQL veri ambarı sağlayan Dmv'leri listesini görüntülemek için şuna başvurun [belgeleri](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-system-views#sql-data-warehouse-dynamic-management-views-dmvs). 

## <a name="metrics-and-diagnostics-logging"></a>Ölçümleri ve tanılama günlükleri
Hem ölçüm ve günlükleri dışarı aktarılabilir için [Operations Management Suite](https://azure.microsoft.com/resources/videos/operations-management-suite-oms-overview/) (OMS), özellikle [Log analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) bileşeni ve programlı olarak erişilebilir [Günlükaraması](https://docs.microsoft.com/azure/log-analytics/log-analytics-tutorial-viewdata).


> [!NOTE]
> Ağustos 2018'den itibaren günlükleri şu anda SQL veri ambarı için uygulanır

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki nasıl yapılır kılavuzları, ortak senaryolar açıklanmaktadır ve izleme ve veri Ambarınızı yönetme Kullanım:

- [Veri ambarı yükünüzü Dmv'ler ile izleme](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-manage-monitor)

