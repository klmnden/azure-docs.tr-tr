---
title: Azure SQL Veritabanı sorgulamak için Ruby kullanma | Microsoft Docs
description: Bu konu başlığı altında, Ruby kullanarak Azure SQL Veritabanına bağlanan ve Transact-SQL deyimleriyle veritabanını sorgulayan bir program oluşturma işlemi gösterilir.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: mvc,develop apps
ms.devlang: ruby
ms.topic: quickstart
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: f86c53465636f82cf36d5099699fc9e6efeb55a5
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38704666"
---
# <a name="use-ruby-to-query-an-azure-sql-database"></a>Ruby kullanarak Azure SQL veritabanı sorgulama

Bu hızlı başlangıçta, [Ruby](https://www.ruby-lang.org) kullanarak Azure SQL veritabanına bağlanan ve Transact-SQL deyimleriyle veri sorgulayan bir program oluşturma işleminin nasıl yapılacağı açıklanır.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için aşağıdaki önkoşulların karşılandığından emin olun:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- Bu hızlı başlangıçta kullanacağınız bilgisayarın genel IP adresi için [sunucu düzeyinde bir güvenlik duvarı kuralı](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).

- İşletim sisteminiz için Ruby ve ilgili yazılımları yüklediniz:
    - **MacOS**: Homebrew'i, rbenv ve ruby-build'i, Ruby'yi ve sonra da FreeTDS'yi yükleyin. Bkz: [1.2, 1.3, 1.4 ve 1.5 adımları](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/).
    - **Ubuntu**: Ruby'nin önkoşullarını yükleyin, rbenv ve ruby-build'i, Ruby'yi ve sonra da FreeTDS'yi yükleyin. Bkz: [1.2, 1.3, 1.4 ve 1.5 adımları](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu/).

## <a name="sql-server-connection-information"></a>SQL Server bağlantı bilgileri

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

> [!IMPORTANT]
> Bu hızlı başlangıç öğreticisinde kullanacağınız bilgisayarın genel IP adresi için bir güvenlik duvarı kuralınız olmalıdır. Farklı bir bilgisayar kullanıyorsanız veya farklı bir genel IP adresiniz varsa [Azure portal kullanarak bir sunucu düzeyi güvenlik duvarı kuralı](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) oluşturun. 

## <a name="insert-code-to-query-sql-database"></a>SQL veritabanını sorgulamak için kod girme

1. Sık kullandığınız metin düzenleyicisinde **sqltest.rb** adında yeni bir dosya oluşturun

2. İçeriğini aşağıdaki kod ile değiştirin ve sunucunuz, veritabanınız, kullanıcınız ve parolanız için uygun değerleri ekleyin.

```ruby
require 'tiny_tds'
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
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

1. Komut isteminde aşağıdaki komutları çalıştırın:

   ```bash
   ruby sqltest.rb
   ```

2. En üst 20 satırın döndürüldüğünü doğrulayın ve sonra uygulama penceresini kapatın.


## <a name="next-steps"></a>Sonraki Adımlar
- [İlk Azure SQL veritabanınızı tasarlama](sql-database-design-first-database.md)
- [TinyTDS için GitHub deposu](https://github.com/rails-sqlserver/tiny_tds)
- [TinyTDS hakkında sorun bildirin veya soru sorun](https://github.com/rails-sqlserver/tiny_tds/issues)
- [SQL Server için Ruby Sürücüleri](https://docs.microsoft.com/sql/connect/ruby/ruby-driver-for-sql-server/)
