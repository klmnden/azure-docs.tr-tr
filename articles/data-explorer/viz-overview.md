---
title: Azure Veri Gezgini veri Görselleştirme
description: Azure Veri Gezgini verilerinizi görselleştirmek farklı yollar hakkında bilgi edinin
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 85c37b6d626fc9942f5df956e738431d2727d282
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66481842"
---
# <a name="data-visualization-with-azure-data-explorer"></a>Azure Veri Gezgini ile veri Görselleştirme 

Azure Veri Gezgini, karmaşık analiz çözümleri için çok büyük miktarda veri oluşturmak için kullanılan günlük ve telemetri verilerini için hızlı ve yüksek oranda ölçeklenebilir bir veri araştırma hizmetidir. Azure Veri Gezgini, verilerinizi görselleştirin ve sonuçları kuruluşunuz içinde paylaşmak için çeşitli görselleştirme araçlarıyla tümleşir. Bu veriler, iş etkisi yapmak için eyleme dönüştürülebilir içgörüler haline dönüştürülebilir.

Veri Görselleştirme ve raporlama veri analiz işleminin kritik bir adım olduğunu. Senaryonuzu ve bütçe en uygun bir kullanabilmeniz için azure Veri Gezgini birçok BI Hizmetleri destekler.

* Azure Veri Gezgini görselleştirmeler: Kusto sorgu dilini kullanarak [ `render operator` ](/azure/kusto/query/renderoperator) sorgu sonuçları göstermek için çeşitli görselleştirme türlerini sunar. Sorgu görselleştirmeler, anomali algılama ve tahmin, makine öğrenimi ve daha fazla yararlıdır.

* [Power BI](https://powerbi.microsoft.com): Azure Veri Gezgini, çeşitli yöntemler kullanarak Power BI'a bağlanma özelliği sağlar: 

  * [Yerleşik yerel Power BI Bağlayıcısı](/azure/data-explorer/power-bi-connector)

  * [Azure Veri Gezgini Power bı'a sorgu Al](/azure/data-explorer/power-bi-imported-query)
 
  * [SQL sorgusu](/azure/data-explorer/power-bi-sql-query).

* [Microsoft Excel](https://products.office.com/excel): Azure Veri Gezgini yerleşik yerel Excel bağlayıcı kullanarak Excel'e bağlanın veya bir sorgu Azure veri Gezgini'nde Excel'e aktarmak yeteneği sağlar.

* [Grafana](https://grafana.com): Grafana, Azure veri Gezgini'nde verileri görselleştirmenize olanak sağlayan bir Azure Veri Gezgini eklentisini sağlar. [Azure Veri Gezgini, Grafana için veri kaynağı olarak ayarlayın ve ardından verileri görselleştirin](/azure/data-explorer/grafana)

* [Sisense](https://www.sisense.com): Azure Veri Gezgini, JDBC Bağlayıcısı'nı kullanarak Sisense için bağlama özelliği sağlar. [Azure Veri Gezgini'ni Sisense için veri kaynağı olarak ayarlayın ve ardından verileri görselleştirin](/azure/data-explorer/sisense).

* [Tableau](https://www.tableau.com): Azure veri Gezgini'nin, Tableau kullanarak bağlanmak için özelliği sağlar [ODBC Bağlayıcısı ve Tableau verileri görselleştirin](/azure/data-explorer/connect-odbc).

* [Qlik](https://www.qlik.com): Azure Veri Gezgini, Qlik kullanarak bağlanmak için özelliği sağlar [ODBC Bağlayıcısı](/azure/data-explorer/connect-odbc).