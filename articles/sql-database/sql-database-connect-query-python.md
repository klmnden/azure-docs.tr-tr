---
title: "Azure SQL Veritabanı sorgulamak için Python kullanma | Microsoft Docs"
description: "Bu konu başlığı altında, Python kullanarak Azure SQL Veritabanına bağlanan ve Transact-SQL deyimleriyle veritabanını sorgulayan bir program oluşturma işlemi gösterilir."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 452ad236-7a15-4f19-8ea7-df528052a3ad
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: On Demand
ms.tgt_pltfrm: n
ms.devlang: python
ms.topic: quickstart
ms.date: 08/09/2017
ms.author: carlrab
ms.openlocfilehash: f7496c7b78449d5ceae861dc0daac607acdb1f84
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="use-python-to-query-an-azure-sql-database"></a>Python kullanarak Azure SQL veritabanı sorgulama

 Bu hızlı başlangıçta [Python](https://python.org) kullanarak bir Azure SQL veritabanına bağlanma ve Transact-SQL deyimleriyle veri sorgulama işlemleri gösterilir.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç öğreticisini tamamlamak için aşağıdakilere sahip olduğunuzdan emin olun:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- Bu hızlı başlangıç öğreticisinde kullanacağınız bilgisayarın genel IP adresi için [sunucu düzeyinde bir güvenlik duvarı kuralı](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).

- İşletim sisteminiz için Python ve ilgili yazılımları yüklediniz:

    - **MacOS**: Homebrew ve Python yükleyin, ODBC sürücüsünü ve SQLCMD yükleyin, ardından SQL Server için Python Sürücüsü yükleyin. Bkz. [Adım 1.2, 1.3 ve 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/mac/).
    - **Ubuntu**: Python ve diğer gerekli paketleri yükleyin ve ardından SQL Server için Python Sürücüsü yükleyin. Bkz. [Adım 1.2, 1.3 ve 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/ubuntu/).
    - **Windows**: Python’un en yeni sürümünü (ortam değişkeni artık sizin için yapılandırılmıştır) yükleyin, ODBC sürücüsünü ve SQLCMD’yi yükleyin ve SQL Server için Python Sürücüsü yükleyin. Bkz: [1.2, 1.3 ve 2.1 adımları](https://www.microsoft.com/sql-server/developer-get-started/python/windows/). 

## <a name="sql-server-connection-information"></a>SQL Server bağlantı bilgileri

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]
    
## <a name="insert-code-to-query-sql-database"></a>SQL veritabanını sorgulamak için kod girme 

1. Sık kullandığınız metin düzenleyicisinde **sqltest.py** adında yeni bir dosya oluşturun.  

2. İçeriğini aşağıdaki kod ile değiştirin ve sunucunuz, veritabanınız, kullanıcınız ve parolanız için uygun değerleri ekleyin.

```Python
import pyodbc
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
cursor.execute("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid")
row = cursor.fetchone()
while row:
    print (str(row[0]) + " " + str(row[1]))
    row = cursor.fetchone()
```

## <a name="run-the-code"></a>Kodu çalıştırma

1. Komut isteminde aşağıdaki komutları çalıştırın:

   ```Python
   python sqltest.py
   ```

2. En üst 20 satırın döndürüldüğünü doğrulayın ve sonra uygulama penceresini kapatın.

## <a name="next-steps"></a>Sonraki adımlar

- [İlk Azure SQL veritabanınızı tasarlama](sql-database-design-first-database.md)
- [SQL Server için Microsoft Python Sürücüleri](https://docs.microsoft.com/sql/connect/python/python-driver-for-sql-server/)
- [Python Geliştirici Merkezi](https://azure.microsoft.com/develop/python/?v=17.23h)

