---
title: Azure SQL Veritabanı sorgulamak için Python kullanma | Microsoft Docs
description: Bu konu Python kullanarak Azure SQL veritabanına bağlanan bir program oluşturma ve Transact-SQL deyimlerini kullanarak nasıl kullanılacağını gösterir.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: python
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 03/25/2019
ms.openlocfilehash: 7fbb9265ac12126fb13a26650fbb5d65f3d39260
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65792074"
---
# <a name="quickstart-use-python-to-query-an-azure-sql-database"></a>Hızlı Başlangıç: Python kullanarak Azure SQL veritabanı sorgulama

 Bu hızlı başlangıçta [Python](https://python.org) kullanarak bir Azure SQL veritabanına bağlanma ve Transact-SQL deyimleriyle veri sorgulama işlemleri gösterilir. Daha fazla SDK ayrıntıları için kullanıma sunduğumuz [başvuru](https://docs.microsoft.com/python/api/overview/azure/sql) belgeleri, [pyodbc GitHub deposu](https://github.com/mkleehammer/pyodbc/wiki/)ve [pyodbc örnek](https://github.com/mkleehammer/pyodbc/wiki/Getting-started).

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için aşağıdakilere sahip olduğunuzdan emin olun:

- Bir Azure SQL veritabanı. Şu hızlı başlangıçlardan biriyle oluşturmak ve ardından bir veritabanını Azure SQL veritabanı'nda yapılandırmak için kullanabilirsiniz:

  || Tek veritabanı | Yönetilen örnek |
  |:--- |:--- |:---|
  | Oluştur| [Portal](sql-database-single-database-get-started.md) | [Portal](sql-database-managed-instance-get-started.md) |
  || [CLI](scripts/sql-database-create-and-configure-database-cli.md) | [CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) |
  || [PowerShell](scripts/sql-database-create-and-configure-database-powershell.md) | [PowerShell](scripts/sql-database-create-configure-managed-instance-powershell.md) |
  | Yapılandır | [sunucu düzeyinde IP güvenlik duvarı kuralı](sql-database-server-level-firewall-rule.md)| [Bir VM bağlantısı](sql-database-managed-instance-configure-vm.md)|
  |||[Şirket içi bağlantısı](sql-database-managed-instance-configure-p2s.md)
  |Verileri yükleyin|Adventure Works hızlı başlangıç yüklendi|[Wide World Importers geri yükleme](sql-database-managed-instance-get-started-restore.md)
  |||Geri yükleme ya da Adventure Works'den içe [BACPAC](sql-database-import.md) dosya [GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works)|
  |||

  > [!IMPORTANT]
  > Komut bu makalede, Adventure Works veritabanı kullanmak için yazılır. Yönetilen örnek sayesinde, Adventure Works veritabanı bir örneği veritabanına aktarmak veya betiklerde Wide World Importers veritabanını kullanmak için bu makaleyi değiştirin.
  
- Python ve ilgili yazılımları işletim sisteminiz için:
  
  - **macOS**: Homebrew ve Python yükleyin, ODBC sürücüsünü ve SQLCMD yükleyin ve ardından SQL Server için Python sürücüsü yükleyin. 1.2, 1.3 ve 2.1 adımları görmek [macOS üzerinde SQL Server'ı kullanarak oluşturmak Python uygulamaları](https://www.microsoft.com/sql-server/developer-get-started/python/mac/). Daha fazla bilgi için [Linux ve macOS Microsoft ODBC sürücüsünü yükleme](https://docs.microsoft.com/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server).

  - **Ubuntu**: Python ve diğer gerekli paketleri yükleyin `sudo apt-get install python python-pip gcc g++ build-essential`. İndirin ve SQL Server için Python sürücüsü ODBC sürücüsünü ve SQLCMD yükleyin. Yönergeler için [pyodbc Python geliştirme için bir geliştirme ortamı yapılandırma](/sql/connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development#linux).

  - **Windows**: SQL Server için Python, ODBC sürücüsünü ve SQLCMD ve Python sürücüsü yükleyin. Yönergeler için [pyodbc Python geliştirme için bir geliştirme ortamı yapılandırma](/sql/connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development#windows).

## <a name="get-sql-server-connection-information"></a>SQL server bağlantı bilgilerini alma

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Yaklaşan yordamlar için tam sunucu adını veya ana bilgisayar adı, veritabanı adını ve oturum açma bilgileri gerekir.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. Gidin **SQL veritabanları** veya **SQL yönetilen örnekler** sayfası.

3. Üzerinde **genel bakış** sayfasında, tam sunucu adını gözden **sunucu adı** tek bir veritabanı veya tam sunucu adı yanındaki **konak** yönetilen bir örneği. Sunucu adı veya ana bilgisayar adı kopyalamak için üzerine gelin ve seçin **kopyalama** simgesi.

## <a name="create-code-to-query-your-sql-database"></a>SQL veritabanınızı sorgulamak için kod oluşturun 

1. Bir metin düzenleyicisinde adlı yeni bir dosya oluşturmak *sqltest.py*.  
   
1. Aşağıdaki kodu ekleyin. İçin kendi değerlerinizi yerleştirin \<server >, \<veritabanı >, \<kullanıcıadı >, ve \<parola >.
   
   >[!IMPORTANT]
   >Bu örnekteki kod kaynağı olarak veritabanınızı oluştururken seçebilirsiniz AdventureWorksLT örnek verileri kullanır. Farklı veri veritabanınız varsa kendi veritabanından tablolar SELECT sorgusunda kullanın. 
   
   ```python
   import pyodbc
   server = '<server>.database.windows.net'
   database = '<database>'
   username = '<username>'
   password = '<password>'
   driver= '{ODBC Driver 17 for SQL Server}'
   cnxn = pyodbc.connect('DRIVER='+driver+';SERVER='+server+';PORT=1433;DATABASE='+database+';UID='+username+';PWD='+ password)
   cursor = cnxn.cursor()
   cursor.execute("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid")
   row = cursor.fetchone()
   while row:
       print (str(row[0]) + " " + str(row[1]))
       row = cursor.fetchone()
   ```
   

## <a name="run-the-code"></a>Kodu çalıştırma

1. Bir komut isteminde aşağıdaki komutu çalıştırın:

   ```cmd
   python sqltest.py
   ```

1. Üst 20 kategori/ürün satırlar döndürülür ve ardından komut penceresini kapatana doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- [İlk Azure SQL veritabanınızı tasarlama](sql-database-design-first-database.md)
- [SQL Server için Microsoft Python sürücüleri](https://docs.microsoft.com/sql/connect/python/python-driver-for-sql-server/)
- [Python Geliştirici Merkezi](https://azure.microsoft.com/develop/python/?v=17.23h)

