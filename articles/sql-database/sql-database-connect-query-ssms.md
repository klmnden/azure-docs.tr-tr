---
title: "SQL Veritabanı&quot;na bağlanma - SQL Server Management Studio | Microsoft Belgeleri"
description: "SQL Server Management Studio (SSMS) kullanarak Azure&quot;da SQL Database&quot;e nasıl bağlanılacağını öğrenin. Ardından, Transact-SQL (T-SQL) kullanarak bir örnek sorgu çalıştırın."
metacanonical: 
keywords: "sql veritabanına bağlanma,sql server management studio"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 7cd2a114-c13c-4ace-9088-97bd9d68de12
ms.service: sql-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/17/2016
ms.author: sstein;carlrab
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 0eb25eb76c6c6c2446ac0b2b07c65975c3719db0


---
# <a name="connect-to-sql-database-with-sql-server-management-studio-and-execute-a-sample-tsql-query"></a>SQL Server Management Studio ile SQL Database'e bağlanma ve örnek T-SQL sorgusu yürütme
> [!div class="op_single_selector"]
> * [Visual Studio](sql-database-connect-query.md)
> * [SSMS](sql-database-connect-query-ssms.md)
> * [Excel](sql-database-connect-excel.md)
> 
> 

Bu makalede, SQL Server Management Studio'yu (SSMS) kullanarak bir Azure SQL veritabanına nasıl bağlanacağınız gösterilmektedir. Bağlantı başarıyla kurulduktan sonra, veritabanı bağlantısını doğrulamak için basit bir Transact-SQL (T-SQL) sorgusu çalıştırırız.

[!INCLUDE [SSMS Install](../../includes/sql-server-management-studio-install.md)]

[!INCLUDE [SSMS Connect](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]

## <a name="run-sample-queries"></a>Örnek sorgu çalıştırma
Sunucunuza bağlandıktan sonra bir veritabanına bağlanabilir ve örnek sorgu çalıştırabilirsiniz. Sorgu yazmaya yeni başladıysanız bkz. [Transact-SQL Deyimi Yazma](https://msdn.microsoft.com/library/ms365303.aspx).

1. **Nesne Gezgini**'nden, sunucu üzerinde **AdventureWorks** örnek veritabanı gibi bir veritabanına gidin.
2. Veritabanına sağ tıklayın ve **Yeni Sorgu**'yu seçin:
   
    ![Yeni sorgu. SQL Database sunucusuna bağlanma: SQL Server Management Studio](./media/sql-database-connect-query-ssms/4-run-query.png)
3. Sorgu penceresinde, şu kodu kopyalayıp yapıştırın:
   
        SELECT
        CustomerId
        ,Title
        ,FirstName
        ,LastName
        ,CompanyName
        FROM SalesLT.Customer;
4. **Yürüt** düğmesine tıklayın:
   
    ![Başarılı. SQL Database sunucusuna bağlanma: SQL Server Management Studio](./media/sql-database-connect-query-ssms/5-success.png)

## <a name="next-steps"></a>Sonraki adımlar
SQL Server'dakine benzer şekilde, T-SQL deyimlerini kullanarak Azure'da veritabanı oluşturup yönetebilirsiniz. Daha önce SQL Server ile T-SQL kullandıysanız farklılıklara ilişkin özet niteliğinde bir açıklama için bkz. [Azure SQL Database Transact-SQL bilgileri](sql-database-transact-sql-information.md).

T-SQL'i ilk kez kullanıyorsanız bkz. [Öğretici: Transact-SQL Deyimi Yazma](https://msdn.microsoft.com/library/ms365303.aspx) ve [Transact-SQL Başvuru (Veritabanı Altyapısı)](https://msdn.microsoft.com/library/bb510741.aspx).

Veritabanı kullanıcıları ve veritabanı kullanıcı yöneticileri oluşturmaya başlamak için bkz. [Azure SQL Database Güvenliğine Başlama](sql-database-get-started-security.md).

SSMS hakkında daha fazla bilgi için bkz. [SQL Server Management Studio'yu Kullanma](https://msdn.microsoft.com/library/ms174173.aspx).




<!--HONumber=Nov16_HO2-->


