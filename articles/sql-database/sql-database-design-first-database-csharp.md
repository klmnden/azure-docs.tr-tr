---
title: İlk Azure SQL veritabanınızı tasarlama - C# | Microsoft Docs
description: İlk Azure SQL veritabanınızı tasarlamayı ve ADO.NET kullanarak bunu C# programına bağlamayı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.topic: tutorial
author: MightyPen
ms.author: genemi
ms.reviewer: carlrab
manager: craigg-msft
ms.date: 12/10/2018
ms.openlocfilehash: cf180f6e2970ac4435602f1cceeb98a4dd9e8724
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53727176"
---
# <a name="tutorial-design-an-azure-sql-database-and-connect-with-cx23-and-adonet"></a>Öğretici: C&#x23; ve ADO.NET ile bir Azure SQL veritabanı tasarlama ve bağlama

Azure SQL veritabanı, bir ilişkisel veritabanı olarak-hizmet (DBaaS), Microsoft Cloud (Azure) ' dir. Bu öğreticide, Visual Studio ile ADO.NET ve Azure portalını kullanarak şu işlemleri gerçekleştirmeyi öğreneceksiniz:

> [!div class="checklist"]
> * Azure portalında veritabanı oluşturma
> * Azure portalında sunucu düzeyinde güvenlik duvarı kuralı ayarlama
> * ADO.NET ve Visual Studio ile veritabanına bağlanma
> * ADO.NET ile tablo oluşturma
> * ADO.NET ile veri ekleme, güncelleştirme ve silme
> * ADO.NET ile veri sorgulama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Yüklemesini [Visual Studio 2017](https://www.visualstudio.com/downloads/)

<!-- The following included .md, sql-database-tutorial-portal-create-firewall-connection-1.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-tutorial-portal-create-firewall-connection-1](../../includes/sql-database-tutorial-portal-create-firewall-connection-1.md)]

<!-- The following included .md, sql-database-csharp-adonet-create-query-2.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-csharp-adonet-create-query-2](../../includes/sql-database-csharp-adonet-create-query-2.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, temel veritabanı görevlerini gibi veritabanı ve tablo oluşturma hakkında bilgi edindiniz, veritabanına bağlanmak, verileri yüklemek ve sorguları çalıştırın. Şunları öğrendiniz:

> [!div class="checklist"]
> * Veritabanı oluşturma
> * Güvenlik duvarı kuralı ayarlama
> * [Visual Studio ve C#](sql-database-connect-query-dotnet-visual-studio.md) ile veritabanına bağlanma
> * Tablo oluşturma
> * Ekleme, güncelleştirme, silme ve verileri Sorgulama

Veri geçişi hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [DMS kullanarak SQL Server'ı çevrimdışı Azure SQL Veritabanına geçirme](../dms/tutorial-sql-server-to-azure-sql.md)
