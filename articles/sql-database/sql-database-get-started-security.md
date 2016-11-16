---
title: "SQL Database öğreticisi: Güvenliğe Başlarken"
description: "Bir veritabanına erişmek ve veritabanını yönetmek üzere kullanıcı hesabı oluşturmayı öğrenin"
keywords: 
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 67797b09-f5c3-4ec2-8494-fe18883edf7f
ms.service: sql-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/17/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: a3bd8ac5466ae67df0a1865509c8fb897d1374ec


---
# <a name="sql-database-tutorial-create-sql-database-user-accounts-to-access-and-manage-a-database"></a>SQL Veritabanı öğreticisi: Bir veritabanına erişmek ve onu yönetmek için SQL veritabanı kullanıcı hesabı oluşturma
> [!div class="op_single_selector"]
> * [Başlangıç eğitmeni](sql-database-get-started-security.md)
> * [Erişim verme](sql-database-manage-logins.md)
> 
> 

Bu öğreticide, SQL Server Management Studio'yu (SSMS) kullanarak şu işlemleri gerçekleştirmeyi öğreneceksiniz:

* Sunucu düzeyinde asıl oturum açma işlemini kullanarak SQL Veritabanı'nda oturum açma.
* SQL Veritabanı kullanıcı hesabı oluşturma.
* Bir SQL Veritabanı kullanıcısına [db_owner izinleri](https://msdn.microsoft.com/library/ms189121.aspx#Anchor_0) verme.
* Sunucu düzeyinde asıl hesap olmayan bir kullanıcı hesabıyla SQL veritabanına bağlanma.

[!INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]

[!INCLUDE [Create SQL Database logical server](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]

[!INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-database-user.md)]

[!INCLUDE [Create SQL Database database](../../includes/sql-database-grant-database-user-dbo-permissions.md)]

[!INCLUDE [Create SQL Database database](../../includes/sql-database-sql-server-management-studio-connect-user.md)]

## <a name="next-steps"></a>Sonraki adımlar
Artık SQL Database öğreticisini tamamladığınıza, bir kullanıcı hesabı oluşturduğunuza ve kullanıcı hesabına dbo izinlerini atadığınıza göre [SQL Database güvenliği](sql-database-manage-logins.md) hakkında daha fazla bilgi edinmeye hazırsınız.




<!--HONumber=Nov16_HO2-->


