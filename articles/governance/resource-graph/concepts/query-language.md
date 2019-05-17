---
title: Sorgu dil anlama
description: Kullanılabilir Kusto işleçler ve Azure kaynak grafiği ile kullanılabilir işlevler açıklanmaktadır.
author: DCtheGeek
ms.author: dacoulte
ms.date: 04/22/2019
ms.topic: conceptual
ms.service: resource-graph
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: dcb21a6aedf16b034fad4f0822e22758dda03c33
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65800503"
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
- [tostring()](/azure/kusto/query/tostringfunction)
- [zip()](/azure/kusto/query/zipfunction)

## <a name="escape-characters"></a>Kaçış karakterleri

İçeren olanlar gibi bazı özellik adlarını bir `.` veya `$`, sarmalanmış kaçış karakterleri veya sorgu ya da özellik adı yanlış yorumlanır ve beklenen sonuçları sağlamaz.

- `.` -Özellik adı şekilde kaydır: `['propertyname.withaperiod']`
  
  Özellik saran bir örnek sorgu _odata.type_:

  ```kusto
  where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.['odata.type']
  ```

- `$` -Özellik adı kaçış karakteri. Kullanılan kaçış karakteri kaynak grafiği çalıştırıldığı Kabuk bağlıdır.

  - **Bash** - `\`

    Özellik çıkışları örnek sorgu  _\$türü_ bash:

    ```kusto
    where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.\$type
    ```

  - **cmd** -çıkış yok `$` karakter.

  - **PowerShell** - ``` ` ```

    Özellik çıkışları örnek sorgu  _\$türü_ PowerShell'de:

    ```kusto
    where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.`$type
    ```

## <a name="next-steps"></a>Sonraki adımlar

- Kullanımda dili bakın [başlangıç sorguları](../samples/starter.md)
- Bkz: Gelişmiş kullanır [Gelişmiş sorgular](../samples/advanced.md)
- [Kaynakları keşfetmeyi](explore-resources.md) öğrenin