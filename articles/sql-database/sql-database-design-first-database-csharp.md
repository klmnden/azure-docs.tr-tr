---
title: "İlk Azure SQL veritabanınızı - C# tasarlama | Microsoft Docs"
description: "İlk Azure SQL veritabanınızı tasarım ve ADO.NET kullanarak C# programı ile bağlanma hakkında bilgi edinme."
services: sql-database
documentationcenter: 
author: MightyPen
manager: craigg-msft
editor: CarlRabeler
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: develop databases, mvc, devcenter
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 08/25/2017
ms.author: genemi
ms.openlocfilehash: 1e13a9a89873b189e1ea05c0208e7c8646881655
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="design-an-azure-sql-database-and-connect-with-cx23-and-adonet"></a>Bir Azure SQL veritabanı tasarlamak ve C & #x23 bağlanın; ve ADO.NET

Azure SQL veritabanı bir ilişkisel veritabanı-olarak-a (DBaaS) (Azure) Microsoft Cloud hizmetidir. Bu öğreticide, Azure portal ve ADO.NET için Visual Studio ile nasıl kullanılacağını öğrenin: 

> [!div class="checklist"]
> * Azure portalında bir veritabanı oluşturun
> * Azure portalında bir sunucu düzeyinde güvenlik duvarı kuralı ayarlayın
> * ADO.NET ve Visual Studio ile veritabanına bağlan
> * ADO.NET ile tabloları oluşturma
> * Ekle, Güncelleştir ve ADO.NET verilerle silme 
> * ADO.NET veri sorgulama

Bir Azure aboneliğiniz yoksa [ücretsiz bir hesap oluşturma](https://azure.microsoft.com/free/) başlamadan önce.

## <a name="prerequisites"></a>Ön koşullar

[Visual Studio Community 2017, Visual Studio Professional 2017 veya Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/) yüklemesi.

<!-- The following included .md, sql-database-tutorial-portal-create-firewall-connection-1.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-tutorial-portal-create-firewall-connection-1](../../includes/sql-database-tutorial-portal-create-firewall-connection-1.md)]


<!-- The following included .md, sql-database-csharp-adonet-create-query-2.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-csharp-adonet-create-query-2](../../includes/sql-database-csharp-adonet-create-query-2.md)]


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, temel veritabanı görevlerini gibi bir veritabanı ve tablo oluşturma hakkında bilgi edindiniz, yükleme ve verileri sorgulamak ve veritabanını zaman içinde daha önceki bir noktaya geri. Şunları öğrendiniz:
> [!div class="checklist"]
> * Veritabanı oluşturma
> * Bir güvenlik duvarı kuralı ayarlamak
> * Veritabanı ile bağlanmak [Visual Studio ve C#](sql-database-connect-query-dotnet-visual-studio.md)
> * Tabloları oluşturma
> * Ekle, Güncelleştir ve verilerini sil
> * Verileri sorgulama

Verilerinizi geçirme hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
>[SQL Server veritabanını Azure SQL veritabanına geçirme](sql-database-migrate-your-sql-server-database.md)

