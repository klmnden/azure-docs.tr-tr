---
title: Azure SQL Veri Ambarı'na Bağlanma sqlcmd | Microsoft Belgeleri
description: Bağlanmak ve bir Azure SQL Data Warehouse'u sorgulamak için SQLCMD komut satırı yardımcı programını kullanın.
services: sql-data-warehouse
author: XiaoyuL-Preview
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 72760c5123703a664695c1be4d286a38e96ecc3f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65873312"
---
# <a name="connect-to-sql-data-warehouse-with-sqlcmd"></a>sqlcmd ile SQL Data Warehouse'a bağlanma
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Bir Azure SQL Veri Ambarı’na bağlanmak ve sorgu göndermek için [sqlcmd][sqlcmd] komut satırı yardımcı programını kullanın.  

## <a name="1-connect"></a>1. Bağlan
**Sqlcmd** kullanmaya başlamadan önce komut istemini açın ve [sqlcmd][sqlcmd] öğesinden sonra SQL Veri Ambarı veritabanınızın bağlantı dizesini girin. Bağlantı dizesi için aşağıdaki parametreler gereklidir:

* **Server (-S):** Sunucu biçiminde `<`sunucu adı`>`. database.windows.net
* **Veritabanı (-d):** Veritabanı adı.
* **Enable Quoted tanımlayıcıları (-ı):** Bir SQL Data Warehouse örneğine bağlanmak için tırnak işaretli tanımlayıcıların etkinleştirilmesi gerekir.

SQL Server Kimlik Doğrulamasını kullanmak için kullanıcı adı/parola parametrelerini eklemeniz gerekir:

* **Kullanıcı (-U):** Biçimindeki sunucu kullanıcısı `<`kullanıcı`>`
* **Parola (-P):** Kullanıcıyla ilişkili parola.

Örneğin, bağlantı dizeniz aşağıdaki gibi görünebilir:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

Azure Active Directory Tümleşik kimlik doğrulamasını kullanmak için Azure Active Directory parametrelerini eklemeniz gerekir:

* **Azure Active Directory Kimlik Doğrulaması (-G):** Kimlik doğrulaması için Azure Active Directory kullanın

Örneğin, bağlantı dizeniz aşağıdaki gibi görünebilir:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -G -I
```

> [!NOTE]
> Active Directory kullanarak kimlik doğrulaması yapmak için [Azure Active Directory Kimlik Doğrulamasını etkinleştirmeniz](sql-data-warehouse-authentication.md) gerekir.
> 
> 

## <a name="2-query"></a>2. Sorgu
Bağlantının ardından desteklenen herhangi bir Transact-SQL deyimini örnekte yayımlayabilirsiniz.  Bu örnekte sorgular etkileşimli modda gönderilir.

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

Bu sonraki örnekler -Q seçeneği kullanarak veya SQL’i sqlcmd öğesine ekleyerek sorgularınızı toplu iş modunda nasıl çalıştırabileceğinizi gösterir.

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

## <a name="next-steps"></a>Sonraki adımlar
Sqlcmd’de kullanılabilen seçenekler hakkında daha fazla bilgi için bkz. [sqlcmd belgeleri][sqlcmd].

<!--Image references-->

<!--Article references-->

<!--MSDN references--> 
[sqlcmd]: https://msdn.microsoft.com/library/ms162773.aspx
[Azure portal]: https://portal.azure.com

<!--Other Web references-->
