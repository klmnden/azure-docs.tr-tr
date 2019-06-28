---
title: Azure SQL veri ambarı yönetim ve izleme - sorgu etkinliği, kaynak kullanımını | Microsoft Docs
description: Hangi özelliklerin yönetmek ve Azure SQL veri ambarı izlemek kullanılabilir olduğunu öğrenin. Sorgu etkinliği ve veri Ambarınızı kaynak kullanımını anlamak için Azure portalı ve dinamik yönetim görünümlerini (Dmv'ler) kullanın.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 06/20/2019
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 5038ae99a804b456c2cc388f07899278cc0f9a24
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67312872"
---
# <a name="monitoring-resource-utilization-and-query-activity-in-azure-sql-data-warehouse"></a>Azure SQL veri ambarı'nda kaynak kullanımı ve sorgu etkinliğini izleme
Azure SQL veri ambarı, Azure Portal, veri ambarı iş yükü için içgörüleri için zengin bir izleme deneyimi sağlar. Azure portalı, ölçüm ve günlükleri için yapılandırılabilir saklama süreleri, uyarıları, öneriler ve özelleştirilebilir grafikler ve panolar sağladığı gibi veri Ambarınızı izlerken önerilen araç winres.exe'dir. Portal Ayrıca, yalnızca veri ambarınızın aynı zamanda tüm Azure analiz için bütünsel bir izleme deneyimi sağlamak için Operations Management Suite (OMS) ve Azure İzleyici (günlükleri) gibi diğer Azure izleme hizmetleriyle tümleştirmenize olanak sağlar tümleşik bir izleme deneyimi için Platform. Bu belgede, izleme hangi özellikleri en iyi duruma getirmek ve SQL veri ambarı ile analiz platformunuz yönetmek kullanılabilen açıklanmaktadır. 

## <a name="resource-utilization"></a>Kaynak kullanımı 
Aşağıdaki ölçümler, Azure portalında SQL veri ambarı için kullanılabilir. Bu ölçümler üzerinden çıkmış [Azure İzleyici](https://docs.microsoft.com/azure/azure-monitor/platform/data-collection#metrics).

> [!NOTE]
> Şu anda nodu düzeyinde CPU ve g/ç ölçümleri veri ambarı kullanımı düzgün yansıtmıyor. Takım izleme ve sorun giderme deneyimini SQL veri ambarı için geliştikçe Bu ölçümler yakın gelecekte kaldırılacak. 

| Ölçüm Adı             | Açıklama                                                  | Toplama Türü |
| ----------------------- | ------------------------------------------------------------ | ---------------- |
| CPU yüzdesi          | Veri ambarı için tüm düğümlerde CPU kullanımı      | Maksimum          |
| Veri G/Ç yüzdesi      | Veri ambarı için tüm düğümlerde g/ç kullanımı       | Maksimum          |
| Bellek yüzdesi       | Veri ambarı için tüm düğümlerde bellek kullanımı (SQL Server) | Maksimum          |
| Başarılı bağlantılar  | Veriler başarılı bağlantı sayısı                 | Toplam            |
| Başarısız Bağlantılar      | Başarısız veri ambarı bağlantı sayısı           | Toplam            |
| Güvenlik Duvarı tarafından engellendi     | Oturum açma engellendi veri ambarına sayısı     | Toplam            |
| DWU limiti               | Veri ambarı hizmet düzeyi hedefi                | Maksimum          |
| DWU yüzdesi          | En fazla CPU yüzdesi arasındaki veri g/ç yüzdesi        | Maksimum          |
| Kullanılan DWU                | DWU limiti * DWU yüzdesi                                   | Maksimum          |
| Önbellek isabet yüzdesi    | (önbellek isabet / isabetsizlik önbelleğe) * burada yerel SSD önbellekte tüm columnstore segmentleri isabetli okuma sayısının toplam önbellek isabet olduğu ve önbellek isabetsizliği columnstore segmentleri 100 kaçan tüm düğümlerde toplamı yerel SSD önbellekte | Maksimum          |
| Kullanılan önbellek yüzdesi   | (önbellek kullanılan / kapasite önbellek) * burada kullanılan önbellek yerel SSD önbellekte tüm baytların toplamından tüm düğümlerde ve önbellek kapasitesi kadar yerel SSD depolama kapasitesinin toplamı 100 tüm düğümlerde önbelleğe alma | Maksimum          |
| Yerel tempdb yüzdesi | Yerel tempdb kullanımı tüm işlem düğümlerinde - değerler her beş dakikada gönderilir | Maksimum          |

## <a name="query-activity"></a>Sorgu etkinliği
SQL veri ambarı T-SQL aracılığıyla izlerken bir programlama deneyimi için hizmet dinamik yönetim görünümlerini (Dmv'ler) sunmaktadır. Bu görünümler, etkin olarak sorun giderme ve, iş yükü ile performans sorunlarını tanımlamak faydalıdır.

SQL veri ambarı sağlayan Dmv'leri listesini görüntülemek için şuna başvurun [belgeleri](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-system-views#sql-data-warehouse-dynamic-management-views-dmvs). 

## <a name="metrics-and-diagnostics-logging"></a>Ölçümleri ve tanılama günlükleri
Hem ölçüm ve günlükleri dışarı aktarılabilir için Azure İzleyici, özellikle [Azure İzleyici günlükleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) bileşeni ve programlı olarak erişilebilir [oturum sorguları](https://docs.microsoft.com/azure/log-analytics/log-analytics-tutorial-viewdata). SQL veri ambarı için günlük gecikme süresi yaklaşık 10-15 dakika ' dir. Gecikme süresini etkileyen faktörleri hakkında daha fazla bilgi için aşağıdaki belgeleri ziyaret edin.


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki nasıl yapılır kılavuzları, ortak senaryolar açıklanmaktadır ve izleme ve veri Ambarınızı yönetme Kullanım:

- [Veri ambarı yükünüzü Dmv'ler ile izleme](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-manage-monitor)

