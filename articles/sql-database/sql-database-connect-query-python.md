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
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: hero-article
ms.date: 07/11/2017
ms.author: carlrab
ms.translationtype: HT
ms.sourcegitcommit: 54454e98a2c37736407bdac953fdfe74e9e24d37
ms.openlocfilehash: b7c217be41b979f8a7246109cc95a01341dadf3d
ms.contentlocale: tr-tr
ms.lasthandoff: 07/13/2017

---
# <a name="use-python-to-query-an-azure-sql-database"></a>Python kullanarak Azure SQL veritabanı sorgulama

 Bu hızlı başlangıçta [Python](https://python.org) kullanarak bir Azure SQL veritabanına bağlanma ve Transact-SQL deyimleriyle veri sorgulama işlemleri gösterilir.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç öğreticisini tamamlamak için aşağıdakilere sahip olduğunuzdan emin olun:

- Bir Azure SQL veritabanı. Bu hızlı başlangıçta, aşağıdaki hızlı başlangıçlardan birinde oluşturulan kaynaklar kullanılır: 

   - [DB Oluşturma - Portal](sql-database-get-started-portal.md)
   - [DB oluşturma - CLI](sql-database-get-started-cli.md)
   - [DB Oluşturma - PowerShell](sql-database-get-started-powershell.md)

- Bu hızlı başlangıç öğreticisinde kullanacağınız bilgisayarın genel IP adresi için [sunucu düzeyinde bir güvenlik duvarı kuralı](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).

- İşletim sisteminiz için Python ve ilgili yazılımları yüklediniz.

    - **MacOS**: Homebrew ve Python yükleyin, ODBC sürücüsünü ve SQLCMD yükleyin, ardından SQL Server için Python Sürücüsü yükleyin. Bkz: [1.2, 1.3 ve 2.1 adımları](https://www.microsoft.com/sql-server/developer-get-started/Python/mac/).
    - **Ubuntu**: Python ve diğer gerekli paketleri yükleyin ve ardından SQL Server için Python Sürücüsü yükleyin. Bkz: [1.2 ve 2.1 adımları](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu/).
    - **Windows**: Python’un en yeni sürümünü (ortam değişkeni artık sizin için yapılandırılmıştır) yükleyin, ODBC sürücüsünü ve SQLCMD’yi yükleyin ve SQL Server için Python Sürücüsü yükleyin. Bkz: [1.2, 1.3 ve 2.1 adımları](https://www.microsoft.com/sql-server/developer-get-started/node/windows/). 

## <a name="sql-server-connection-information"></a>SQL Server bağlantı bilgileri

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Sonraki yordamlarda tam sunucu adına, veritabanı adına ve oturum açma bilgilerine ihtiyacınız olacaktır.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Soldaki menüden **SQL Veritabanları**’nı seçin ve **SQL veritabanları** sayfasında veritabanınıza tıklayın. 
3. Veritabanınızın **Genel Bakış** sayfasında, aşağıdaki görüntüde gösterildiği gibi tam sunucu adını gözden geçirin. Sunucu adının üzerine gelerek **Kopyalamak için tıklayın** seçeneğini ortaya çıkarabilirsiniz.  

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Sunucunuzun oturum açma bilgilerini unuttuysanız SQL Veritabanı sunucusu sayfasına giderek sunucu yöneticisi adını görüntüleyin ve gerekirse parolayı sıfırlayın.     
    
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


