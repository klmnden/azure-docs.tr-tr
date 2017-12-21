---
title: "Grafik verileri Azure Cosmos veritabanı nasıl? | Microsoft Belgeleri"
description: "Sorgu grafik verileri Azure Cosmos veritabanı hakkında bilgi edinin"
services: cosmos-db
documentationcenter: 
author: luisbosquez
manager: jhubbard
editor: 
tags: 
ms.assetid: 8bde5c80-581c-4f70-acb4-9578873c92fa
ms.service: cosmos-db
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: lbosq
ms.custom: mvc
ms.openlocfilehash: bf4bb59545ce2d4172cb001d29f5bfc68968d389
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="azure-cosmos-db-how-to-query-with-the-graph-api-preview"></a>Azure Cosmos DB: grafik API'si (Önizleme) ile nasıl?

Azure Cosmos DB [grafik API'si](graph-introduction.md) (Önizleme) destekleyen [Gremlin](https://github.com/tinkerpop/gremlin/wiki) sorgular. Bu makalede örnek belgelerdeki ve sorgulardaki başlamanıza yardımcı olmak için sunulmaktadır. A ayrıntılı başvuru sağlanır Gremlin [Gremlin Destek](gremlin-support.md) makalesi.

Bu makalede aşağıdaki görevleri içerir: 

> [!div class="checklist"]
> * Gremlin verilerle sorgulama

## <a name="prerequisites"></a>Ön koşullar

Bu sorguları çalışmak bir Azure Cosmos DB hesabınız varsa ve grafik verileri kapsayıcısında sahip. Bu yok? Tamamlamak [5 dakikalık quickstart](create-graph-dotnet.md) veya [Geliştirici öğretici](tutorial-query-graph.md) bir hesap oluşturun ve veritabanınızı doldurmak için. Kullanarak aşağıdaki sorguları çalıştırabilirsiniz [Azure Cosmos DB .NET grafik Kitaplığı](graph-sdk-dotnet.md), [Gremlin konsol](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console), veya sık kullanılan Gremlin sürücünüzü.

## <a name="count-vertices-in-the-graph"></a>Grafik tepe sayısı

Aşağıdaki kod parçacığında, grafik tepe sayısı gösterilmektedir:

```
g.V().count()
```

## <a name="filters"></a>Filtreler

Gremlin'ın kullanarak filtreler gerçekleştirebilirsiniz `has` ve `hasLabel` adımları ve bunları birleştirmek kullanarak `and`, `or`, ve `not` daha karmaşık filtreler oluşturmak için. Azure Cosmos DB köşeleri ve derece hızlı sorgular için içindeki tüm özelliklerinin şema belirsiz dizin oluşturma sağlar:

```
g.V().hasLabel('person').has('age', gt(40))
```

## <a name="projection"></a>Yansıtma

Belirli özellikleri kullanarak sorgu sonuçlarındaki proje `values` . adım:

```
g.V().hasLabel('person').values('firstName')
```

## <a name="find-related-edges-and-vertices"></a>İlgili kenarları ve köşeleri Bul

Şu ana kadar yalnızca tüm veritabanındaki iş sorgu işleçleri gördük. İlgili kenarları ve köşeleri gitmek gerektiğinde grafikleri hızlı ve verimli çapraz geçiş işlemleri için. Şimdi tüm arkadaşların Thomas bulun. Biz Gremlin'ın kullanarak bunu `outE` tüm bulmak için adım Thomas silip Gremlin'ın kullanarak bu kenarlarından içinde-köşeleri için çapraz geçiş yapan dışarı kenarları `inV` . adım:

```cs
g.V('thomas').outE('knows').inV().hasLabel('person')
```

Tüm Thomas' "arkadaş arkadaş", çağırarak bulmak için iki atlama sonraki sorgu gerçekleştirir `outE` ve `inV` iki kez. 

```cs
g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')
```

Daha karmaşık sorgular derlemek ve döngü kullanarak gerçekleştirmeden, filtre ifadeleri karıştırma dahil olmak üzere Gremlin kullanan güçlü grafik geçişi mantığı uygulamanıza `loop` adım ve uygulama koşullu Gezinti kullanarak `choose` adım. İle yapabilecekleriniz hakkında daha fazla bilgi [Gremlin Destek](gremlin-support.md)!

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdakileri yaptığınızdan:

> [!div class="checklist"]
> * Grafik kullanarak sorgu öğrendiniz 

Verilerinizi Genel dağıtma konusunda bilgi almak için sonraki öğretici şimdi devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Verilerinizi genel Dağıt](tutorial-global-distribution-sql-api.md)
