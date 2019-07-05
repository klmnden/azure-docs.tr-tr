---
title: Azure Veri Gezgini veri Görselleştirme
description: Azure Veri Gezgini verilerinizi görselleştirmek farklı yollar hakkında bilgi edinin
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/30/2019
ms.openlocfilehash: d1c73d8eb65ed5d67d5250b4a3bca3b80450001e
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67536731"
---
# <a name="data-visualization-with-azure-data-explorer"></a>Azure Veri Gezgini ile veri Görselleştirme 

Azure Veri Gezgini, karmaşık analiz çözümleri için çok büyük miktarda veri oluşturmak için kullanılan günlük ve telemetri verilerini için hızlı ve yüksek oranda ölçeklenebilir bir veri araştırma hizmetidir. Azure Veri Gezgini, verilerinizi görselleştirin ve sonuçları kuruluşunuz içinde paylaşmak için çeşitli görselleştirme araçlarıyla tümleşir. Bu veriler, iş etkisi yapmak için eyleme dönüştürülebilir içgörüler haline dönüştürülebilir.

Veri Görselleştirme ve raporlama veri analiz işleminin kritik bir adım olduğunu. Senaryonuzu ve bütçe en uygun bir kullanabilmeniz için azure Veri Gezgini birçok BI Hizmetleri destekler.

## <a name="kusto-query-language-visualizations"></a>Kusto sorgu dili görselleştirmeler

Kusto sorgu dili [ `render operator` ](/azure/kusto/query/renderoperator) tablolar, pasta grafiklerini ve çubuk grafikleri, sorgu sonuçları göstermek için gibi çeşitli görselleştirmeler sunar. Sorgu görselleştirmeler, anomali algılama ve tahmin, makine öğrenimi ve daha fazla yararlıdır.

## <a name="power-bi"></a>Power BI

Azure Veri Gezgini bağlanma olanağı sağlar [Power BI](https://powerbi.microsoft.com) çeşitli yöntemler kullanarak: 

  * [Yerleşik yerel Power BI Bağlayıcısı](/azure/data-explorer/power-bi-connector)

  * [Azure Veri Gezgini Power bı'a sorgu Al](/azure/data-explorer/power-bi-imported-query)
 
  * [SQL sorgusu](/azure/data-explorer/power-bi-sql-query)

## <a name="microsoft-excel"></a>Microsoft Excel

Azure Veri Gezgini bağlanma olanağı sağlar [Microsoft Excel](https://products.office.com/excel) yerleşik yerel kullanarak Excel Bağlayıcısı veya bir sorgu Azure veri Gezgini'nde Excel'e aktarmak.

## <a name="grafana"></a>Grafana

[Grafana](https://grafana.com) Azure veri Gezgini'nde verileri görselleştirmenize olanak sağlayan bir Azure Veri Gezgini eklentisini sağlar. [Azure Veri Gezgini, Grafana için veri kaynağı olarak ayarlayın ve ardından verileri görselleştirin](/azure/data-explorer/grafana). 

## <a name="odbc-connector"></a>ODBC bağlayıcısı

Azure Veri Gezgini sağlar bir [açık veritabanı bağlantısı (ODBC) bağlayıcı](connect-odbc.md) ODBC destekleyen herhangi bir uygulama, Azure veri Gezgini'ne bağlanabilmesi için.

## <a name="tableau"></a>Tableau

Azure Veri Gezgini bağlanma olanağı sağlar [Tableau](https://www.tableau.com) kullanarak [ODBC Bağlayıcısı](/azure/data-explorer/connect-odbc) ardından [Tableau verileri görselleştirin](tableau.md).

## <a name="qlik"></a>Qlik

Azure Veri Gezgini bağlanma olanağı sağlar [Qlik](https://www.qlik.com) kullanarak [ODBC Bağlayıcısı](/azure/data-explorer/connect-odbc) Qlik Sense panolar oluşturabilir ve verileri görselleştirin. Aşağıdaki videoda kullanarak, Azure Veri Gezgini ile Qlik görselleştirmek bilgi edinebilirsiniz. 

> [!VIDEO https://www.youtube.com/embed/nhWIiBwxjjU]  

## <a name="sisense"></a>Sisense

Azure Veri Gezgini bağlanma olanağı sağlar [Sisense](https://www.sisense.com) JDBC Bağlayıcısı'nı kullanarak. [Azure Veri Gezgini'ni Sisense için veri kaynağı olarak ayarlayın ve ardından verileri görselleştirin](/azure/data-explorer/sisense).