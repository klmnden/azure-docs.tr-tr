---
title: Azure Cosmos DB’de graf verilerini sorgulama | Microsoft Docs
description: Azure Cosmos DB’de graf verilerini sorgulamayı öğrenin
services: cosmos-db
author: luisbosquez
manager: kfile
editor: ''
tags: ''
ms.service: cosmos-db
ms.component: cosmosdb-graph
ms.devlang: na
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: lbosq
ms.custom: mvc
ms.openlocfilehash: dfaf3fe1427c9069939f5421c6a87efb584ebe48
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763213"
---
# <a name="tutorial-query-azure-cosmos-db-graph-api-by-using-gremlin"></a>Öğretici: Gremlin kullanarak Azure Cosmos DB Graph API’yi sorgulama

Azure Cosmos DB [Graph API](graph-introduction.md), [Gremlin](https://github.com/tinkerpop/gremlin/wiki) sorgularını destekler. Bu makalede, başlamanıza yardımcı olmak için örnek belgeler ve sorgular sunulmaktadır. [Gremlin desteği](gremlin-support.md) makalesinde ayrıntılı bir Gremlin başvurusu sağlanmıştır.

Bu makale aşağıdaki görevleri kapsar: 

> [!div class="checklist"]
> * Gremlin ile verileri sorgulama

## <a name="prerequisites"></a>Ön koşullar

Bu sorguların çalışması için bir Azure Cosmos DB hesabınız ve kapsayıcıda graf verileriniz olmalıdır. Bunlardan biri yok mu? Bir hesap oluşturmak ve veritabanınızı doldurmak için [5 dakikalık hızlı başlangıç](create-graph-dotnet.md) veya [geliştirici öğreticisini](tutorial-query-graph.md) tamamlayın. [Azure Cosmos DB .NET graf kitaplığını](graph-sdk-dotnet.md), [Gremlin konsolunu](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console) veya sık kullandığınız Gremlin sürücüsünü kullanarak aşağıdaki sorguları çalıştırabilirsiniz.

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

Artık verilerinizi genel olarak nasıl dağıtacağınızı öğrenmek için sonraki öğreticiye ilerleyebilirsiniz.

> [!div class="nextstepaction"]
> [Verilerinizi genel olarak dağıtma](tutorial-global-distribution-graph.md)
