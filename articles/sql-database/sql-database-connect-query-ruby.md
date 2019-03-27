---
title: Azure SQL Veritabanı sorgulamak için Ruby kullanma | Microsoft Docs
description: Bu konu başlığı altında, Ruby kullanarak Azure SQL Veritabanına bağlanan ve Transact-SQL deyimleriyle veritabanını sorgulayan bir program oluşturma işlemi gösterilir.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ruby
ms.topic: quickstart
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 03/25/2019
ms.openlocfilehash: 05f3213383c526944a8a1cf51fb92d5186ac7434
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58449020"
---
# <a name="quickstart-use-ruby-to-query-an-azure-sql-database"></a>Hızlı Başlangıç: Ruby kullanarak Azure SQL veritabanı sorgulama

Bu hızlı başlangıçta nasıl kullanılacağını gösterir [Ruby](https://www.ruby-lang.org) Transact-SQL deyimleri ile bir Azure SQL veritabanı ve sorgu verilerine bağlanmak için.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için aşağıdaki önkoşullar gerekir:

- Bir Azure SQL veritabanı. Şu hızlı başlangıçlardan biriyle oluşturmak ve ardından bir veritabanını Azure SQL veritabanı'nda yapılandırmak için kullanabilirsiniz:

  || Tek veritabanı | Yönetilen örnek |
  |:--- |:--- |:---|
  | Oluştur| [Portal](sql-database-single-database-get-started.md) | [Portal](sql-database-managed-instance-get-started.md) |
  || [CLI](scripts/sql-database-create-and-configure-database-cli.md) | [CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) |
  || [PowerShell](scripts/sql-database-create-and-configure-database-powershell.md) | [PowerShell](scripts/sql-database-create-configure-managed-instance-powershell.md) |
  | Yapılandırma | [sunucu düzeyinde IP güvenlik duvarı kuralı](sql-database-server-level-firewall-rule.md)| [Bir VM bağlantısı](sql-database-managed-instance-configure-vm.md)|
  |||[Şirket içi bağlantısı](sql-database-managed-instance-configure-p2s.md)
  |Veri yükleme|Adventure Works hızlı başlangıç yüklendi|[Wide World Importers geri yükleme](sql-database-managed-instance-get-started-restore.md)
  |||Geri yükleme ya da Adventure Works'den içe [BACPAC](sql-database-import.md) dosya [GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works)|
  |||

  > [!IMPORTANT]
  > Komut bu makalede, Adventure Works veritabanı kullanmak için yazılır. Yönetilen örnek sayesinde, Adventure Works veritabanı bir örneği veritabanına aktarmak veya betiklerde Wide World Importers veritabanını kullanmak için bu makaleyi değiştirin.
  
- İşletim sisteminiz için Ruby ve ilgili yazılım:
  
  - **macOS**: Homebrew, rbenv ve ruby-Build'i, Ruby'yi, FreeTDS ve TinyTDS yükleyin. 1.2, 1.3, 1.4, 1.5 ve 2.1 adımları görmek [macOS üzerinde SQL Server'ı kullanarak oluşturmak Ruby uygulamaları](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/).
  
  - **Ubuntu**: Ruby, rbenv ve ruby-Build'i, Ruby'yi, FreeTDS ve TinyTDS önkoşullarını yükleyin. 1.2, 1.3, 1.4, 1.5 ve 2.1 adımları görmek [Ubuntu'da SQL Server'ı kullanarak oluşturma Ruby uygulamaları](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu/).
  
  - **Windows**: Ruby ve Ruby Devkit TinyTDS yükleyin. Bkz: [Ruby geliştirmeye yönelik yapılandırma geliştirme ortamı](/sql/connect/ruby/step-1-configure-development-environment-for-ruby-development).

## <a name="get-sql-server-connection-information"></a>SQL server bağlantı bilgilerini alma

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Yaklaşan yordamlar için tam sunucu adını veya ana bilgisayar adı, veritabanı adını ve oturum açma bilgileri gerekir.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. Gidin **SQL veritabanları** veya **SQL yönetilen örnekler** sayfası.

3. Üzerinde **genel bakış** sayfasında, tam sunucu adını gözden **sunucu adı** tek bir veritabanı veya tam sunucu adı yanındaki **konak** yönetilen bir örneği. Sunucu adı veya ana bilgisayar adı kopyalamak için üzerine gelin ve seçin **kopyalama** simgesi. 

## <a name="create-code-to-query-your-sql-database"></a>SQL veritabanınızı sorgulamak için kod oluşturun

1. Bir metin veya Kod Düzenleyicisi'nde adlı yeni bir dosya oluşturun *sqltest.rb*.
   
1. Aşağıdaki kodu ekleyin. Azure SQL veritabanınız için değerler yerine `<server>`, `<database>`, `<username>`, ve `<password>`.
   
   >[!IMPORTANT]
   >Bu örnekteki kod kaynağı olarak veritabanınızı oluştururken seçebilirsiniz AdventureWorksLT örnek verileri kullanır. Farklı veri veritabanınız varsa kendi veritabanından tablolar SELECT sorgusunda kullanın. 
   
   ```ruby
   require 'tiny_tds'
   server = '<server>.database.windows.net'
   database = '<database>'
   username = '<username>'
   password = '<password>'
   client = TinyTds::Client.new username: username, password: password, 
       host: server, port: 1433, database: database, azure: true
   
   puts "Reading data from table"
   tsql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
           FROM [SalesLT].[ProductCategory] pc
           JOIN [SalesLT].[Product] p
           ON pc.productcategoryid = p.productcategoryid"
   result = client.execute(tsql)
   result.each do |row|
       puts row
   end
   ```

## <a name="run-the-code"></a>Kodu çalıştırma

1. Bir komut isteminde aşağıdaki komutu çalıştırın:

   ```bash
   ruby sqltest.rb
   ```
   
1. Veritabanınızdan en üst 20 kategori/ürün satırın döndürüldüğünü doğrulayın. 

## <a name="next-steps"></a>Sonraki adımlar
- [İlk Azure SQL veritabanınızı tasarlamayı](sql-database-design-first-database.md).
- [TinyTDS için GitHub deposu](https://github.com/rails-sqlserver/tiny_tds).
- [TinyTDS hakkında sorular sormak veya sorunları rapor](https://github.com/rails-sqlserver/tiny_tds/issues).
- [SQL Server için Ruby sürücüsü](https://docs.microsoft.com/sql/connect/ruby/ruby-driver-for-sql-server/).
