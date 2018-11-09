---
title: SQL veri ambarı ile tümleşik çözümler oluşturma | Microsoft Docs
description: 'Araçlar ve iş ortakları ile SQL veri ambarı ile tümleştirilen çözümler. '
services: sql-data-warehouse
author: kavithaj
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: consume
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: 5b302be582bd22a7b38601c90f5fe475062afb26
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51239680"
---
# <a name="integrate-other-services-with-sql-data-warehouse"></a>SQL veri ambarı ile diğer Hizmetleri Tümleştirme
Çekirdek işlevselliğini ek olarak, SQL veri ambarı birçok diğer Azure Hizmetleri ile tümleştirme olanağı sağlar. Bu hizmetlerden bazıları şunlardır:

* Power BI
* Azure Data Factory
* Azure Machine Learning
* Azure Stream Analytics

SQL veri ambarı devam Azure ve daha fazlası konusunda daha fazla hizmetleriyle tümleştirmeye yönelik [tümleştirme iş ortaklarının](sql-data-warehouse-partner-data-integration.md).

## <a name="power-bi"></a>Power BI
Power BI tümleştirmesi, dinamik raporlama ve görselleştirme, Power BI ile SQL veri ambarı işlem gücüyle birleştirerek olanak tanır. Power BI tümleştirmesi şu anda içerir:

* **Doğrudan bağlan**: SQL veri ambarına karşı mantıksal itme ile bağlantı daha gelişmiş. İtme daha büyük bir ölçekte daha hızlı analizini sağlar.
* **Power bı'da Aç**: "Power bı'da Aç" düğmesi simplifed şekilde bağlanmak için Power BI için örnek bilgisi geçirir.

Daha fazla bilgi için [Power BI ile tümleştirme](sql-data-warehouse-get-started-visualize-with-power-bi.md), veya [Power BI belgeleri](https://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx).

## <a name="azure-data-factory"></a>Azure Data Factory
Azure Data Factory, kullanıcılara karmaşık ayıklama oluşturun ve işlem hatlarını yüklemek için yönetilen bir platform sağlar. Azure Data Factory ile SQL veri ambarı'nın tümleştirme içerir:

* **Saklı yordamları**: SQL veri ambarı saklı yordamlar yürütülemedi düzenleyin.
* **Kopyalama**: kullanım verileri SQL Data Warehouse'a veri taşımak için ADF. Bu işlem, perde ADF'nin standart veri taşıma mekanizması veya PolyBase kullanabilirsiniz. 

Daha fazla bilgi için [Azure Data Factory ile tümleştir](sql-data-warehouse-get-started-visualize-with-power-bi.md).

## <a name="azure-machine-learning"></a>Azure Machine Learning
Azure Machine Learning çok sayıda Tahmine dayalı araçlarını kullanarak karmaşık modeller oluşturmanızı sağlayan tam olarak yönetilen bir analiz hizmetidir. SQL veri ambarı, şu işlevleri kullanarak hem kaynak hem de bu modeller için hedef olarak desteklenir:

* **Veri okuma:** uygun ölçekte T-SQL kullanarak SQL veri ambarına karşı modelleri sürücü.
* **Veri yazma:** tamamlama geri SQL veri ambarı için herhangi bir model değiştirir.

Daha fazla bilgi için [Azure Machine Learning ile tümleştir](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md).

## <a name="azure-stream-analytics"></a>Azure Stream Analytics
Azure Stream Analytics, işleme ve oluşturulan Azure olay Hub'ından olay verilerini kullanan karmaşık, tam olarak yönetilen bir altyapısıdır.  SQL veri ambarı ile tümleştirme, akış verileri etkili bir şekilde işlenen ve depolanan ilişkisel verileri daha ayrıntılı, daha gelişmiş analizi etkinleştirme yanı sıra sağlar.  

* **İş çıktısı:** Gönder çıkış Stream Analytics işlerinden doğrudan SQL veri ambarı.

Daha fazla bilgi için [Azure Stream Analytics ile tümleştirme](sql-data-warehouse-integrate-azure-stream-analytics.md).

## <a name="next-steps"></a>Sonraki adımlar
Azure SQL veritabanı ile tümleştirmek için bkz: [yapılandırma SQL veritabanı esnek sorgu](tutorial-elastic-query-with-sql-datababase-and-sql-data-warehouse.md)

