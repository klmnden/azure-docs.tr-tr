---
title: include dosyası
description: include dosyası
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 04/13/2018
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: e6e70da1f939547cdb589ae7bcb6ed1f6148f22e
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38733786"
---
Artık, verilerinizi almak ve filtrelemek için Veri Gezgini'ndeki sorguları kullanabilirsiniz.

1. Sorgunun, varsayılan olarak `SELECT * FROM c` şeklinde ayarlandığına dikkat edin. Bu varsayılan sorgu, koleksiyondaki tüm belgeleri alır ve görüntüler. 

    ![Veri Gezgini’ndeki varsayılan sorgu: `SELECT * FROM c`](./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-query.png)

2. **Documents** sekmesinde kalın ve **Filtreyi düzenle** düğmesine tıklayıp sorgu koşulu kutusuna `ORDER BY c._ts DESC` ekledikten sonra **Filtre Uygula** seçeneğine tıklayarak sorguyu değiştirin.

    ![ORDER BY c._ts DESC ekleyerek ve Filtre Uygula’ya tıklayarak varsayılan sorguyu değiştirin](./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-edit-query.png)

Düzenlenen sorguda belgeler zaman damgalarına göre azalan sırada listelendiğinden, şimdi ikinci belgeniz ilk sırada görüntülenir. SQL söz dizimini biliyorsanız desteklenen [SQL sorgularını](../articles/cosmos-db/sql-api-sql-query.md) bu kutuya girebilirsiniz. 

Bu işlemle Veri Gezgini üzerindeki çalışmalarımız tamamlanmış olur. Kodlarla çalışmaya başlamadan önce, Veri Gezgini'ni kullanarak ayrıca saklı yordamlar, UDF'ler ve tetikleyiciler oluşturabileceğinizi, bu sayede sunucu tarafı iş mantığını gerçekleştirebileceğinizi ve aktarım hızını ölçeklendirebileceğinizi göz önünde bulundurun. Veri Gezgini, API'lerdeki tüm yerleşik programlı veri erişimini açığa çıkarır ancak Azure portalındaki verilerinize kolayca erişmenizi sağlar.