---
title: SQL veri ambarı ile tümleşik çözümler oluşturma | Microsoft Docs
description: 'Araçlar ve iş ortakları ile SQL veri ambarı ile tümleştirilen çözümler. '
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: integration
ms.date: 04/17/2018
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: 43a714ae175e0d60f20b5e7ad79e1fa90125b0f8
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65873333"
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

* **Doğrudan bağlantı**: SQL veri ambarına karşı mantıksal itme ile daha gelişmiş bir bağlantı. İtme daha büyük bir ölçekte daha hızlı analizini sağlar.
* **Power bı'da Aç**: "Power bı'da Aç" düğmesi Power bı'a bağlamak basitleştirilmiş bir yolu için örnek bilgisi geçirir.

Daha fazla bilgi için [Power BI ile tümleştirme](sql-data-warehouse-get-started-visualize-with-power-bi.md), veya [Power BI belgeleri](https://powerbi.microsoft.com/blog/exploring-azure-sql-data-warehouse-with-power-bi/).

## <a name="azure-data-factory"></a>Azure Data Factory
Azure Data Factory, kullanıcılara karmaşık ayıklama oluşturun ve işlem hatlarını yüklemek için yönetilen bir platform sağlar. Azure Data Factory ile SQL veri ambarı'nın tümleştirme içerir:

* **Saklı yordamları**: SQL veri ambarı saklı yordamlar yürütülemedi düzenleyin.
* **Kopyalama**: SQL Data Warehouse'a veri taşımak için ADF kullanın. Bu işlem, perde ADF'nin standart veri taşıma mekanizması veya PolyBase kullanabilirsiniz. 

Daha fazla bilgi için [Azure Data Factory ile tümleştir](https://docs.microsoft.com/azure/data-factory/load-azure-sql-data-warehouse?toc=/azure/sql-data-warehouse/toc.json).

## <a name="azure-machine-learning"></a>Azure Machine Learning
Azure Machine Learning çok sayıda Tahmine dayalı araçlarını kullanarak karmaşık modeller oluşturmanızı sağlayan tam olarak yönetilen bir analiz hizmetidir. SQL veri ambarı, şu işlevleri kullanarak hem kaynak hem de bu modeller için hedef olarak desteklenir:

* **Okunan veriler:** T-SQL kullanarak SQL veri ambarına karşı ölçeğinde modeller sürücü.
* **Veri yazma:** Değişiklikleri geri SQL veri ambarı için herhangi bir model uygulayın.

Daha fazla bilgi için [Azure Machine Learning ile tümleştir](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md).

## <a name="azure-stream-analytics"></a>Azure Stream Analytics
Azure Stream Analytics, işleme ve oluşturulan Azure olay Hub'ından olay verilerini kullanan karmaşık, tam olarak yönetilen bir altyapısıdır.  SQL veri ambarı ile tümleştirme, akış verileri etkili bir şekilde işlenen ve depolanan ilişkisel verileri daha ayrıntılı, daha gelişmiş analizi etkinleştirme yanı sıra sağlar.  

* **İş çıktısı:** Çıkış, Stream Analytics işlerinden doğrudan SQL veri ambarı'na gönderin.

Daha fazla bilgi için [Azure Stream Analytics ile tümleştirme](sql-data-warehouse-integrate-azure-stream-analytics.md).


