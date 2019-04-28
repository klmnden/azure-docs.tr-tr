---
title: Azure Cosmos DB’de graf verilerini sorgulama
description: Azure Cosmos DB’de graf verilerini sorgulamayı öğrenin
author: luisbosquez
ms.author: lbosq
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: tutorial
ms.date: 01/02/2018
ms.reviewer: sngun
ms.openlocfilehash: 2bc79801864481562967702a7c52a7670950199b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61293121"
---
# <a name="tutorial-query-azure-cosmos-db-gremlin-api-by-using-gremlin"></a>Öğretici: Azure Cosmos DB Gremlin API, Gremlin kullanarak sorgulama

Azure Cosmos DB [Gremlin API](graph-introduction.md), [Gremlin](https://github.com/tinkerpop/gremlin/wiki) sorgularını destekler. Bu makalede, başlamanıza yardımcı olmak için örnek belgeler ve sorgular sunulmaktadır. [Gremlin desteği](gremlin-support.md) makalesinde ayrıntılı bir Gremlin başvurusu sağlanmıştır.

Bu makale aşağıdaki görevleri kapsar: 

> [!div class="checklist"]
> * Gremlin ile verileri sorgulama

## <a name="prerequisites"></a>Önkoşullar

Bu sorguların çalışması için bir Azure Cosmos DB hesabınız ve kapsayıcıda graf verileriniz olmalıdır. Bunlardan biri yok mu? Bir hesap oluşturmak ve veritabanınızı doldurmak için [5 dakikalık hızlı başlangıç](create-graph-dotnet.md) veya [geliştirici öğreticisini](tutorial-query-graph.md) tamamlayın. [Gremlin konsolunu](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console) veya sık kullandığınız Gremlin sürücüsünü kullanarak aşağıdaki sorguları çalıştırabilirsiniz.

## <a name="count-vertices-in-the-graph"></a>Graftaki köşeleri sayma

Aşağıdaki kod parçacığı, graftaki köşe sayısının nasıl hesaplanacağını gösterir:

```
g.V().count()
```

## <a name="filters"></a>Filtreler

Gremlin’in `has` ve `hasLabel` etiketlerini kullanarak filtreler gerçekleştirebilir ve daha karmaşık filtreler derlemek için `and`, `or` ve `not` kullanarak bunları birleştirebilirsiniz. Azure Cosmos DB, hızlı sorgular için köşeleriniz ve dereceleriniz içindeki tüm özelliklerin şemadan bağımsız dizinlemesini sağlar:

```
g.V().hasLabel('person').has('age', gt(40))
```

## <a name="projection"></a>Yansıtma

`values` adımını kullanarak sorgu sonuçlarında belirli özellikleri yansıtabilirsiniz:

```
g.V().hasLabel('person').values('firstName')
```

## <a name="find-related-edges-and-vertices"></a>İlgili kenarları ve köşeleri bulma

Şu ana kadar yalnızca tüm veritabanlarında çalışan sorgu işleçlerini gördük. Graflar, ilgili kenarlara ve köşelere gitmeniz gerektiğinde çapraz geçiş işlemleri için hızlı ve etkilidir. Şimdi Thomas’ın tüm arkadaşlarını bulalım. Thomas’taki tüm dış kenarları bulmak için Gremlin’in `outE` adımını kullanıp sonra Gremlin’in `inV` adımını kullanarak bu kenarlardan iç köşelere çapraz geçiş yaparak bunu gerçekleştiririz:

```cs
g.V('thomas').outE('knows').inV().hasLabel('person')
```

Sonraki sorgu, `outE` ve `inV` öğelerini iki kez çağırarak Thomas’ın tüm “arkadaşlarının arkadaşlarını” bulmak için iki atlama gerçekleştirir. 

```cs
g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')
```

Gremlin kullanarak, filtre ifadelerini karıştırma, `loop` adımını kullanarak döngü gerçekleştirme ve `choose` adımını kullanarak koşullu gezinti uygulama gibi güçlü grafik çapraz geçişi mantığı uygulayabilir ve daha karmaşık sorgular derleyebilirsiniz. [Gremlin desteği](gremlin-support.md) ile neler yapabileceğiniz hakkında daha fazla bilgi edinin!

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdakileri yaptınız:

> [!div class="checklist"]
> * Graf kullanarak sorgulamayı öğrendiniz 

Şimdi Cosmos DB hakkında daha fazla bilgi için Kavramlar bölümüne geçebilirsiniz.

> [!div class="nextstepaction"]
> [Genel dağıtım](distribute-data-globally.md) 

