---
title: "SQL veri ambarı ile tümleştirilmiş çözümler derleme | Microsoft Docs"
description: "Araçlar ve iş ortakları ile SQL Data Warehouse ile tümleştirme çözümleri. "
services: sql-data-warehouse
documentationcenter: NA
author: mlee3gsd
manager: jhubbard
editor: 
ms.assetid: e2dc8f3f-10e3-4589-a4e2-50c67dfcf67f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: martinle;barbkess
ms.openlocfilehash: d407c29f99fd7537590ec787febd84a9e3f4f353
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="leverage-other-services-with-sql-data-warehouse"></a>SQL veri ambarı diğer hizmetlerle yararlanın
Çekirdek işlevselliğini ek olarak, SQL Data Warehouse kullanıcıların diğer hizmetler Azure bunu yanında çoğunu yararlanan olanak tanır.  Özellikle, biz derine aşağıdaki ile tümleştirme adımları şu anda attıktan:

* Power BI
* Azure Data Factory
* Azure Machine Learning
* Azure Stream Analytics

Daha fazla Hizmetleri ile Azure ekosistemi bağlanmak için çalışıyoruz.

## <a name="power-bi"></a>Power BI
Power BI tümleştirme, dinamik Raporlama ile SQL Data Warehouse işlem gücü ve Power BI görselleştirme yararlanmanızı sağlar. Power BI tümleştirmesi şu anda içerir:

* **Connect doğrudan**: daha gelişmiş SQL veri ambarına karşı mantıksal aşağı itme bağlantıyla.  Bu, daha büyük bir ölçekte daha hızlı analizini sağlar.
* **Power bı'da Aç**: Power BI, örnek bilgileri daha sorunsuz bir bağlantı için izin verme 'Power bı'da Aç' düğmesini geçirir.

Bkz: [Power BI ile tümleştirme](sql-data-warehouse-integrate-power-bi.md) veya [Power BI belgelerine](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx) daha fazla bilgi için.

## <a name="azure-data-factory"></a>Azure Data Factory
Azure Data Factory, kullanıcıların karmaşık Extract yük işlem hatlarını oluşturmak için yönetilen bir platform sağlar.  Azure Data Factory ile SQL Data Warehouse'un tümleştirme aşağıdakileri içerir:

* **Saklı yordamlar**: SQL Data Warehouse saklı yordamları yürütülmesi düzenlemek.
* **Kopya**: ADF SQL Data Warehouse'a veri taşımak için kullanın.  Bu işlem perde arkasında ADF'nin standart veri taşıma mekanizması veya PolyBase kullanabilirsiniz. 

Bkz: [Azure Data Factory ile tümleştirme](sql-data-warehouse-integrate-azure-data-factory.md) veya [Azure Data Factory belgelerine](https://azure.microsoft.com/documentation/services/data-factory/) daha fazla bilgi için.

## <a name="azure-machine-learning"></a>Azure Machine Learning
Azure Machine Learning Tahmine dayalı araçları büyük bir dizi yararlanarak karmaşık modelleri oluşturma olanağı tanıyan tam olarak yönetilen analiz hizmetidir.  SQL veri ambarı varsayılan olarak, şu işlevlerle hem kaynak hem de bu modeller için hedef olarak desteklenir:

* **Veri okuma:** SQL veri ambarına karşı T-SQL kullanarak ölçekte modelleri sürücü.
* **Veri yaz:** tamamlama geri SQL Data Warehouse için herhangi bir modelden değiştirir.

Bkz: [Azure Machine Learning ile tümleştirme](sql-data-warehouse-integrate-azure-machine-learning.md) veya [Azure Machine Learning belge](https://azure.microsoft.com/services/machine-learning/) daha fazla bilgi için.

## <a name="azure-stream-analytics"></a>Azure Stream Analytics
Azure Stream Analytics, işleme ve Azure olay Hub'ından oluşturulan olay verilerini tüketen tam olarak yönetilen, karmaşık bir altyapıdır.  Akış etkili bir şekilde işlenir ve derin, daha gelişmiş analiz etkinleştirme ilişkisel veri depolanan verileri SQL Data Warehouse ile tümleştirme sağlar.  

* **İş çıktısı:** Gönder çıkış akış analizi işleri doğrudan SQL Data Warehouse için.

Bkz: [Azure akış Analizi ile tümleştirme](sql-data-warehouse-integrate-azure-stream-analytics.md) veya [Azure akış analizi belgeleri](https://azure.microsoft.com/documentation/services/stream-analytics/) daha fazla bilgi için.

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop/

[Azure Data Factory]: sql-data-warehouse-integrate-azure-data-factory.md
[Azure Machine Learning]: sql-data-warehouse-integrate-azure-machine-learning.md
[Azure Stream Analytics]: sql-data-warehouse-integrate-azure-stream-analytics.md
[Power BI]: sql-data-warehouse-integrate-power-bi.md
[Partners]: sql-data-warehouse-partner-business-intelligence.md

<!--MSDN references-->

<!--Other Web references-->
