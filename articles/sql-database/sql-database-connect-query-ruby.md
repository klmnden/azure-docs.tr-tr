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
ms.date: 12/20/2018
ms.openlocfilehash: 66819cbd65f6f044d0dac68326eb5890476964b6
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2019
ms.locfileid: "53993918"
---
# <a name="quickstart-use-ruby-to-query-an-azure-sql-database"></a>Hızlı Başlangıç: Ruby kullanarak Azure SQL veritabanı sorgulama

Bu hızlı başlangıçta nasıl kullanılacağını gösterir [Ruby](https://www.ruby-lang.org) Transact-SQL deyimleri ile bir Azure SQL veritabanı ve sorgu verilerine bağlanmak için.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için aşağıdaki önkoşullar gerekir:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]
  
- Bu hızlı başlangıçta kullanacağınız bilgisayarın genel IP adresi için [sunucu düzeyinde bir güvenlik duvarı kuralı](sql-database-get-started-portal-firewall.md).
  
- İşletim sisteminiz için Ruby ve ilgili yazılım:
  
  - **macOS**: Homebrew, rbenv ve ruby-Build'i, Ruby'yi, FreeTDS ve TinyTDS yükleyin. 1.2, 1.3, 1.4, 1.5 ve 2.1 adımları görmek [macOS üzerinde SQL Server'ı kullanarak oluşturmak Ruby uygulamaları](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/).
  
  - **Ubuntu**: Ruby, rbenv ve ruby-Build'i, Ruby'yi, FreeTDS ve TinyTDS önkoşullarını yükleyin. 1.2, 1.3, 1.4, 1.5 ve 2.1 adımları görmek [Ubuntu'da SQL Server'ı kullanarak oluşturma Ruby uygulamaları](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu/).
  
  - **Windows**: Ruby ve Ruby Devkit TinyTDS yükleyin. Bkz: [Ruby geliştirmeye yönelik yapılandırma geliştirme ortamı](/sql/connect/ruby/step-1-configure-development-environment-for-ruby-development).

## <a name="get-sql-server-connection-information"></a>SQL server bağlantı bilgilerini alma

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

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
