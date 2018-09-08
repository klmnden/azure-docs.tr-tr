---
title: SQL veri ambarı önerileri - kavramları | Microsoft Docs
description: SQL veri ambarı öneriler ve nasıl oluşturulacağını öğrenin
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 07/27/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 48d64873f0a8c3754ac5c3ecda2294c0f337b9d5
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44094613"
---
# <a name="sql-data-warehouse-recommendations"></a>SQL veri ambarı önerileri

Bu makalede, Azure Danışmanı üzerinden SQL veri ambarı tarafından sunulan önerileri açıklanmaktadır.  

SQL veri ambarı veri Ambarınızı emin olmak için öneriler tutarlı performans için iyileştirilmiş sağlar. Veri ambarı önerileri ile sıkı bir şekilde tümleştirilir [Azure Danışmanı](https://docs.microsoft.com/azure/advisor/advisor-performance-recommendations) içinde doğrudan en iyi yöntemler sağlamak üzere [Azure portalında](https://aka.ms/Azureadvisor). SQL veri ambarı, veri ambarının geçerli durumunu analiz eder, telemetri ve yüzeyleri öneriler etkin iş yükünüz için günlük bir tempoyla toplar. Desteklenen veri ambarı öneri senaryoları, önerilen eylemleri uygulamak nasıl birlikte aşağıda özetlenmiştir.

SQL veri ambarı Advisor hakkında geri bildirimde bulunmak veya herhangi bir sorunla karşılaşırsanız, ulaşın [ sqldwadvisor@service.microsoft.com ](mailto:sqldwadvisor@service.microsoft.com).   

Tıklayın [burada](https://aka.ms/Azureadvisor) önerilerinizi bugün denetlemek için! Şu anda bu özellik yalnızca 2. nesil veri ambarları için geçerlidir. 

## <a name="data-skew"></a>Veri dengesizliği

Veri dengesizliği yükünüz çalıştırırken ek veri hareketi veya kaynak darboğazları neden olabilir. Aşağıdaki belgeler, veri dengesizliği tanımlamak ve uygun dağıtım anahtar seçerek oluşmasını önlemek için Göster açıklar.

- [Belirlenmesi ve kaldırılmasına eğme](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute#how-to-tell-if-your-distribution-column-is-a-good-choice) 

## <a name="no-or-outdated-statistics"></a>Hayır veya güncel olmayan istatistikleri

Yetersiz sorgu planlarına oluşturmak SQL veri ambarı sorgu iyileştiricisi neden olabileceği yetersiz istatistik sorgu performansı önemli ölçüde etkileyebilir. Aşağıdaki belgeler, oluşturma ve istatistikleri güncelleştirmeyi en iyi uygulamalar açıklanmaktadır:

- [Tablo istatistikleri oluşturma ve güncelleştirme](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistic)

Bu öneri tarafından etkilenen tabloların listesini görmek için aşağıdaki komutu çalıştırın. [T-SQL betiği](https://github.com/Microsoft/sql-data-warehouse-samples/blob/master/samples/sqlops/MonitoringScripts/ImpactedTables). Danışman, sürekli olarak bu önerileri oluşturmak için aynı T-SQL betiği çalıştırır.
