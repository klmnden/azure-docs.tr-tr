---
title: SQL veri ambarı ile tümleştirilmiş çözümler derleme | Microsoft Docs
description: 'Araçlar ve iş ortakları ile SQL Data Warehouse ile tümleştirme çözümleri. '
services: sql-data-warehouse
author: kavithaj
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: consume
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: f198a99fc03a079be77c7f8167580bb7b758579e
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="integrate-other-services-with-sql-data-warehouse"></a>SQL veri ambarı ile diğer Hizmetleri Tümleştirme
Çekirdek işlevselliğini ek olarak, SQL veri ambarı kullanıcıların çoğu Azure diğer hizmetler ile tümleştirmenize olanak tanır. Bu hizmetlerden bazıları şunlardır:

* Power BI
* Azure Data Factory
* Azure Machine Learning
* Azure Stream Analytics

SQL veri ambarı devam Azure ve daha fazlasını arasında daha fazla hizmetleriyle tümleştirmeye yönelik [tümleştirme ortakları](sql-data-warehouse-partner-data-integration.md).

## <a name="power-bi"></a>Power BI
Power BI tümleştirme, dinamik raporlama ve görsel olarak Power BI ile SQL Data Warehouse işlem gücünü birleştirmenize olanak sağlar. Power BI tümleştirmesi şu anda içerir:

* **Connect doğrudan**: daha gelişmiş SQL veri ambarına karşı mantıksal aşağı itme bağlantıyla. Aşağı İtme daha büyük bir ölçekte daha hızlı analizini sağlar.
* **Power bı'da Aç**: 'Power bı'da Aç' düğmesini örneği bilgileri bağlama simplifed yolu için Power BI geçirir.

Daha fazla bilgi için bkz: [Power BI ile tümleştirme](sql-data-warehouse-get-started-visualize-with-power-bi.md), veya [Power BI belgelerine](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx).

## <a name="azure-data-factory"></a>Azure Data Factory
Azure Data Factory, kullanıcıların karmaşık extract oluşturmak ve ardışık düzen yüklemek için bir yönetilen platform sağlar. Azure Data Factory ile SQL Data Warehouse'un tümleştirmesi içerir:

* **Saklı yordamlar**: SQL Data Warehouse saklı yordamları yürütülmesi düzenlemek.
* **Kopya**: ADF SQL Data Warehouse'a veri taşımak için kullanın. Bu işlem perde arkasında ADF'nin standart veri taşıma mekanizması veya PolyBase kullanabilirsiniz. 

Daha fazla bilgi için bkz: [Azure Data Factory ile tümleştirme](sql-data-warehouse-get-started-visualize-with-power-bi.md).

## <a name="azure-machine-learning"></a>Azure Machine Learning
Azure Machine Learning çok sayıda Tahmine dayalı araçları kullanarak karmaşık modelleri oluşturmanıza olanak tanıyan tam olarak yönetilen analiz hizmetidir. SQL veri ambarı varsayılan olarak, şu işlevlerle hem kaynak hem de bu modeller için hedef olarak desteklenir:

* **Veri okuma:** SQL veri ambarına karşı T-SQL kullanarak ölçekte modelleri sürücü.
* **Veri yaz:** tamamlama geri SQL Data Warehouse için herhangi bir modelden değiştirir.

Daha fazla bilgi için bkz: [Azure Machine Learning ile tümleştirme](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md).

## <a name="azure-stream-analytics"></a>Azure Stream Analytics
Azure Stream Analytics, işleme ve Azure olay Hub'ından oluşturulan olay verilerini tüketen tam olarak yönetilen, karmaşık bir altyapıdır.  Akış etkili bir şekilde işlenir ve derin, daha gelişmiş analiz etkinleştirme ilişkisel veri depolanan verileri SQL Data Warehouse ile tümleştirme sağlar.  

* **İş çıktısı:** Gönder çıkış akış analizi işleri doğrudan SQL Data Warehouse için.

Daha fazla bilgi için bkz: [Azure akış Analizi ile tümleştirme](sql-data-warehouse-integrate-azure-stream-analytics.md).

## <a name="next-steps"></a>Sonraki adımlar
Azure SQL veritabanı ile tümleştirmek için bkz: [SQL veritabanını yapılandırma esnek sorgu](tutorial-elastic-query-with-sql-datababase-and-sql-data-warehouse.md)

