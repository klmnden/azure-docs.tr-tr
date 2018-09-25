---
title: Azure kaynak grafik sorgu dilini anlama
description: Azure kaynak grafik sorgu dilini nasıl çalıştığı açıklanır.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: resource-graph
manager: carmonm
ms.openlocfilehash: 3600fc47a0fb318a49c1b37722cb7fffa51ec6f2
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46951959"
---
# <a name="understanding-the-azure-resource-graph-query-language"></a>Azure kaynak grafik sorgu dilini anlama

Azure kaynak grafik sorgu dilini birkaç işleç ve işlevlerini destekler. Kusto sorgu dili (KQL) için benzer çalışır ve her çalışır. Ancak, Kaynak grafiğin sorgu dili benzer olmakla birlikte, anlaşılması önemlidir [KQL](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators), aynı değildir.

> [!NOTE]
> KQL belgelere bağlantılar, kimlik doğrulaması gerektirebilir.

KQL dil nasıl yapılandırıldığını ve çeşitli işleçler nasıl desteklediği hakkında anlamak için belgelere başlatmak için kaynak graf tarafından kullanılan bir sorgu dili hakkında bilgi edinmek için en iyi yolu olduğundan ve işlevleri birlikte çalışır.

## <a name="supported-tabular-operators"></a>Desteklenen tablo işleçleri

Kaynak Graph'te desteklenen tablosal işleçlerin listesi aşağıda verilmiştir:

- [Farklı](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/distinct-operator)
- [Genişletme](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/extend-operator)
- [Sınırı](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/limit-operator)
- [Sıralama ölçütü](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/order-operator)
- [Proje](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/project-operator)
- [Proje koyma](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/project-away-operator)
- [Örnek](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/sample-operator)
- [örnek farklı](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/sample-distinct-operator)
- [Sıralama ölçütü](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/sort-operator)
- [Özetleme](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator)
- [sınav zamanı](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/take-operator)
- [Sayfanın Üstü](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/top-operator)
- [üst iç içe geçmiş](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/top-nested-operator)
- [Top-hitters](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/top-hitters-operator)
- [Burada](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/where-operator)

## <a name="supported-functions"></a>Desteklenen işlevleri

Kaynak Graph'te desteklenen işlevler listesi aşağıda verilmiştir:

- [ago()](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/ago%28%29)
- [buldschema()](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/buildschema%28%29)
- [Count() işlevi](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/count%28%29)
- [strcat()](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/strcat%28%29)
- [isnotempty()](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/isnotempty%28%29_-notempty%28%29)
- [ToString()](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/tostring%28%29)

## <a name="next-steps"></a>Sonraki adımlar

- Kullanımda dili bakın [başlangıç sorguları](../samples/starter.md)
- Bkz: Gelişmiş kullanır [Gelişmiş sorgular](../samples/advanced.md)
- Öğrenme [kaynakları keşfedin](explore-resources.md)