---
title: Azure kaynak grafik sorgu dilini anlama
description: Azure kaynak grafik sorgu dilini nasıl çalıştığı açıklanır.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 10/22/2018
ms.topic: conceptual
ms.service: resource-graph
manager: carmonm
ms.openlocfilehash: 09bcedc5250755f06ba23b84a0ae90b4d43a23db
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50086174"
---
# <a name="understanding-the-azure-resource-graph-query-language"></a>Azure kaynak grafik sorgu dilini anlama

Azure kaynak grafik sorgu dilini birkaç işleç ve işlevlerini destekler. Her iş ve temel alınarak işletmek [Azure Veri Gezgini](../../../data-explorer/data-explorer-overview.md).

Azure Veri Gezgini için belgelere başlatmak için kaynak graf tarafından kullanılan bir sorgu dili hakkında bilgi edinmek için en iyi yolu olan [sorgu dili](/azure/kusto/query/index). Dil nasıl yapılandırıldığını ve çeşitli işleçler nasıl desteklediği hakkında bir anlayış sağlar ve işlevleri birlikte çalışır.

## <a name="supported-tabular-operators"></a>Desteklenen tablo işleçleri

Kaynak Graph'te desteklenen tablosal işleçlerin listesi aşağıda verilmiştir:

- [count](/azure/kusto/query/countoperator)
- [Farklı](/azure/kusto/query/distinctoperator)
- [Genişletme](/azure/kusto/query/extendoperator)
- [Sınırı](/azure/kusto/query/limitoperator)
- [Sıralama ölçütü](/azure/kusto/query/orderoperator)
- [Proje](/azure/kusto/query/projectoperator)
- [Proje koyma](/azure/kusto/query/projectawayoperator)
- [Örnek](/azure/kusto/query/sampleoperator)
- [örnek farklı](/azure/kusto/query/sampledistinctoperator)
- [Sıralama ölçütü](/azure/kusto/query/sortoperator)
- [Özetleme](/azure/kusto/query/summarizeoperator)
- [sınav zamanı](/azure/kusto/query/takeoperator)
- [Sayfanın Üstü](/azure/kusto/query/topoperator)
- [üst iç içe geçmiş](/azure/kusto/query/topnestedoperator)
- [Top-hitters](/azure/kusto/query/tophittersoperator)
- [Burada](/azure/kusto/query/whereoperator)

## <a name="supported-functions"></a>Desteklenen işlevleri

Kaynak Graph'te desteklenen işlevler listesi aşağıda verilmiştir:

- [ago()](/azure/kusto/query/agofunction)
- [buildschema()](/azure/kusto/query/buildschema-aggfunction)
- [strcat()](/azure/kusto/query/strcatfunction)
- [isnotempty()](/azure/kusto/query/isnotemptyfunction)
- [ToString()](/azure/kusto/query/tostringfunction)

## <a name="next-steps"></a>Sonraki adımlar

- Kullanımda dili bakın [başlangıç sorguları](../samples/starter.md)
- Bkz: Gelişmiş kullanır [Gelişmiş sorgular](../samples/advanced.md)
- [Kaynakları keşfetmeyi](explore-resources.md) öğrenin
