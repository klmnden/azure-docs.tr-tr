---
title: İlk Azure SQL veritabanınızı tasarlama - C# | Microsoft Docs
description: İlk Azure SQL veritabanınızı tasarlamayı ve ADO.NET kullanarak bunu C# programına bağlamayı öğrenin.
services: sql-database
author: MightyPen
manager: craigg-msft
ms.reviewer: CarlRabeler
ms.service: sql-database
ms.custom: develop databases, mvc, devcenter
ms.topic: tutorial
ms.date: 06/07/2018
ms.openlocfilehash: 4e2238968d29bc0081a472c9c05662cb7813d866
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38697722"
---
# <a name="design-an-azure-sql-database-and-connect-with-cx23-and-adonet"></a>C&#x23; ve ADO.NET ile bir Azure SQL veritabanı tasarlama ve bağlama

Azure SQL Veritabanı, Microsoft Bulut’ta (Azure) ilişkisel bir hizmet olarak veritabanıdır (DBaaS). Bu öğreticide, Visual Studio ile ADO.NET ve Azure portalını kullanarak şu işlemleri gerçekleştirmeyi öğreneceksiniz: 

> [!div class="checklist"]
> * Azure portalında veritabanı oluşturma
> * Azure portalında sunucu düzeyinde güvenlik duvarı kuralı ayarlama
> * ADO.NET ve Visual Studio ile veritabanına bağlanma
> * ADO.NET ile tablo oluşturma
> * ADO.NET ile veri ekleme, güncelleştirme ve silme 
> * ADO.NET ile veri sorgulama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

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

Bu öğreticide, veritabanı ve tablo oluşturma, veri yükleme ve sorgulama ve veritabanını önceki bir zaman noktasına geri yükleme gibi temel veritabanı görevlerini öğrendiniz. Şunları öğrendiniz:
> [!div class="checklist"]
> * Veritabanı oluşturma
> * Güvenlik duvarı kuralı ayarlama
> * [Visual Studio ve C#](sql-database-connect-query-dotnet-visual-studio.md) ile veritabanına bağlanma
> * Tablo oluşturma
> * Veri ekleme, güncelleştirme ve silme
> * Verileri sorgulama

Verilerinizi geçirme hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [SQL Server veritabanınızı Azure SQL Veritabanına geçirme](sql-database-migrate-your-sql-server-database.md)

