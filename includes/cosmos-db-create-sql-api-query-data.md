---
title: include dosyası
description: include dosyası
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 04/05/2019
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: 9971b16da42cdf1de0464857291c74a947535735
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59287094"
---
Almak ve verilerinizi filtrelemek için veri Gezgini'ndeki sorguları kullanabilirsiniz.

1. Üst kısmındaki **belgeleri** sekmesinde veri Gezgini'nde, varsayılan sorguyu inceleyin `SELECT * FROM c`. Bu sorguyu alır ve kimliği sırayla koleksiyondaki tüm belgeleri görüntüler. 
   
   ![Veri Gezgini’ndeki varsayılan sorgu: `SELECT * FROM c`](./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-query.png)
   
1. Sorguyu değiştirmek için seçin **filtreyi Düzenle**, varsayılan sorguyla değiştirin `ORDER BY c._ts DESC`ve ardından **Filtre Uygula**.
   
   ![ORDER BY c._ts DESC ekleyerek ve Filtre Uygula’ya tıklayarak varsayılan sorguyu değiştirin](./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-edit-query.png)

   Şimdi ikinci belgeniz, zaman damgası göre azalan sırayla düzenleyin belgeleri ilk listelenen değiştirilmiş sorguyu görüntüler. 
   
   ![ORDER BY c._ts DESC sorgu ve Filtre Uygula'i tıklatarak değiştirildi](./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-edited-query.png)

Tanıdık SQL söz dizimi, desteklenen herhangi biri girebilirsiniz [SQL sorguları](../articles/cosmos-db/sql-api-sql-query.md) sorgu koşulu kutusuna içinde. Veri Gezgini, saklı yordamlar, UDF'ler ve Tetikleyiciler için sunucu tarafı iş mantığı oluşturma için de kullanabilirsiniz. 

Veri Gezgini, apı'lerdeki tüm yerleşik programlı veri erişim özelliklere için kolayca Azure portal erişmenizi sağlar. Portal ayrıca aktarım hızı ölçeklendirme, anahtarları ve bağlantı dizelerini almak ve ölçümleri ve SLA'ları, Azure Cosmos DB hesabınız için gözden geçirmek için kullanın. 

