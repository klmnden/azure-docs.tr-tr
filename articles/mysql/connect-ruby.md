---
title: "Ruby'yi kullanarak MySQL için Azure Veritabanı'na bağlanma | Microsoft Docs"
description: "Bu hızlı başlangıçta, MySQL için Azure Veritabanı'na bağlanmak ve buradan veri sorgulamak için kullanabileceğiniz birkaç Ruby kod örneği sağlanmıştır."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql
ms.custom: mvc
ms.devlang: ruby
ms.topic: quickstart
ms.date: 09/22/2017
ms.openlocfilehash: 10f774262015cb19e158a687138b4618ce50063b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-database-for-mysql-use-ruby-to-connect-and-query-data"></a>MySQL için Azure Veritabanı: Ruby'yi kullanarak bağlanma ve veri sorgulama
Bu hızlı başlangıçta, Windows, Ubuntu Linux ve Mac platformlarından bir [Ruby](https://www.ruby-lang.org) uygulaması ve [mysql2](https://rubygems.org/gems/mysql2) gem kullanılarak MySQL için Azure Veritabanı’na nasıl bağlanılacağı gösterilmiştir. Ayrıca veritabanında veri sorgulamak, eklemek, güncelleştirmek ve silmek için SQL deyimlerini nasıl kullanacağınız da gösterilmiştir. Bu konu, Ruby kullanarak geliştirme ile tanıdık ve MySQL için Azure veritabanı ile çalışmaya yeni olduğunu varsayar.

## <a name="prerequisites"></a>Ön koşullar
Bu hızlı başlangıçta, başlangıç noktası olarak şu kılavuzlardan birinde oluşturulan kaynaklar kullanılmaktadır:
- [Azure portalını kullanarak MySQL için Azure Veritabanı sunucusu oluşturma](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Azure CLI kullanarak MySQL için Azure Veritabanı sunucusu oluşturma](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-ruby"></a>Ruby’yi yükleme
Ruby, Gem ve MySQL2 kitaplık kendi bilgisayara yükleyin. 

### <a name="windows"></a>Windows
1. [Ruby](http://rubyinstaller.org/downloads/)'nin 2.3 sürümünü indirip yükleyin.
2. Başlat menüsünden yeni bir komut istemi (cmd) başlatın.
3. Dizini değiştirip, Ruby’nin 2.3 sürümü dizinine geçin. `cd c:\Ruby23-x64\bin`
4. Yüklenen sürümü görmek için `ruby -v` komutunu çalıştırarak Ruby yüklemesini test edin.
5. Yüklenen sürümü görmek için `gem -v` komutunu çalıştırarak Gem yüklemesini test edin.
6. `gem install mysql2` komutunu çalıştırarak, Ruby için Mysql2 modülünü Gem kullanarak derleyin.

### <a name="macos"></a>macOS
1. `brew install ruby` komutunu çalıştırarak Homebrew kullanan Ruby’yi yükleyin. Daha fazla yükleme seçeneği için Ruby [yükleme belgelerine](https://www.ruby-lang.org/en/documentation/installation/#homebrew) bakın.
2. Yüklenen sürümü görmek için `ruby -v` komutunu çalıştırarak Ruby yüklemesini test edin.
3. Yüklenen sürümü görmek için `gem -v` komutunu çalıştırarak Gem yüklemesini test edin.
4. `gem install mysql2` komutunu çalıştırarak, Ruby için Mysql2 modülünü Gem kullanarak derleyin.

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
1. `sudo apt-get install ruby-full` komutunu çalıştırarak Ruby’yi yükleyin. Daha fazla yükleme seçeneği için Ruby [yükleme belgelerine](https://www.ruby-lang.org/en/documentation/installation/) bakın.
2. Yüklenen sürümü görmek için `ruby -v` komutunu çalıştırarak Ruby yüklemesini test edin.
3. `sudo gem update --system` komutunu çalıştırarak Gem için en son güncelleştirmeleri yükleyin.
4. Yüklenen sürümü görmek için `gem -v` komutunu çalıştırarak Gem yüklemesini test edin.
5. `sudo apt-get install build-essential` komutunu çalıştırarak gcc, make ve diğer derleme araçlarını yükleyin.
6. `sudo apt-get install libmysqlclient-dev` komutunu çalıştırarak MySQL istemci geliştirme kitaplığını yükleyin.
7. `sudo gem install mysql2` komutunu çalıştırarak, Ruby için mysql2 modülünü Gem kullanarak derleyin.

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma
MySQL için Azure Veritabanı'na bağlanmak üzere gereken bağlantı bilgilerini alın. Tam sunucu adına ve oturum açma kimlik bilgilerine ihtiyacınız vardır.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Azure portalında sol taraftaki menüden **tüm kaynakları**ve ardından creased sunucusu için arama (gibi **myserver4demo**).
3. **myserver4demo** sunucu adına tıklayın.
4. Sunucunun seçin **özellikleri** sayfasında ve sonra Not **sunucu adı** ve **sunucu yönetici oturum açma adı**.
 ![MySQL için Azure Veritabanı - Sunucu Yöneticisi Oturum Açma](./media/connect-ruby/1_server-properties-name-login.png)
5. Sunucu oturum açma bilgilerinizi unutursanız gidin **genel bakış** sunucu yönetici oturum açma adı görüntülemek için sayfa ve gerekirse, parola sıfırlama.

## <a name="run-ruby-code"></a>Ruby kodunu çalıştırma 
1. Aşağıdaki bölümlerde Söyleniş koddan metin dosyasına yapıştırın ve proje klasörüne dosya uzantısı .rb ile dosyaları kaydetme (gibi `C:\rubymysql\createtable.rb` veya `/home/username/rubymysql/createtable.rb`).
2. Kodu çalıştırmak için bir komut istemi başlatın veya kabuk Bash. Dizini değiştirerek, proje klasörünüze geçin (`cd rubymysql`)
3. Ardından dosya adı, gibi Söyleniş komutu yazın `ruby createtable.rb` uygulamayı çalıştırın.
4. Ruby uygulaması path ortam değişkeni içinde değilse, Windows işletim sisteminde, düğüm uygulama gibi başlatmak için tam yolu kullanın gerekebilir`"c:\Ruby23-x64\bin\ruby.exe" createtable.rb`

## <a name="connect-and-create-a-table"></a>Bağlanma ve tablo oluşturma
Bağlanmak ve kullanarak bir tablo oluşturmak için aşağıdaki kodu kullanın **CREATE TABLE** SQL deyimi, arkasından **INSERT INTO** tabloya satır eklemek için SQL deyimleri.

Kod, [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) sınıfı .new() yöntemini kullanarak MySQL için Azure Veritabanıyla bağlantı kurar. Ardından DROP, CREATE TABLE ve INSERT INTO komutlarını çalıştırmak için birkaç kez [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) yöntemini çağırır. Ardından bağlantıyı sonlandırılmadan önce kapatmak için [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) yöntemini çağırır.

`host`, `database`, `username` ve `password` dizelerini kendi değerlerinizle değiştirin. 
```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('myserver4demo.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@myserver4demo')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection to database.'

    # Drop previous table of same name if one exists
    client.query('DROP TABLE IF EXISTS inventory;')
    puts 'Finished dropping table (if existed).'

    # Drop previous table of same name if one exists.
    client.query('CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);')
    puts 'Finished creating table.'

    # Insert some data into table.
    client.query("INSERT INTO inventory VALUES(1, 'banana', 150)")
    client.query("INSERT INTO inventory VALUES(2, 'orange', 154)")
    client.query("INSERT INTO inventory VALUES(3, 'apple', 100)")
    puts 'Inserted 3 rows of data.'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```

## <a name="read-data"></a>Verileri okuma
Aşağıdaki kodu kullanın ve kullanarak verileri okuyun bir **seçin** SQL deyimi. 

Kod kullanan bir [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) MySQL için Azure veritabanına bağlanmak için class.new() yöntemi. Ardından SELECT komutlarını çalıştırmak için [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) yöntemini çağırır. Ardından bağlantıyı sonlandırılmadan önce kapatmak için [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) yöntemini çağırır.

`host`, `database`, `username` ve `password` dizelerini kendi değerlerinizle değiştirin. 

```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('myserver4demo.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@myserver4demo')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection to database.'

    # Read data
    resultSet = client.query('SELECT * from inventory;')
    resultSet.each do |row|
        puts 'Data row = (%s, %s, %s)' % [row['id'], row['name'], row['quantity']]
    end
    puts 'Read ' + resultSet.count.to_s + ' row(s).'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```

## <a name="update-data"></a>Verileri güncelleştirme
Bağlanıp kullanarak verileri güncelleştirmek için aşağıdaki kodu kullanın bir **güncelleştirme** SQL deyimi.

Kod, [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) sınıfı .new() yöntemini kullanarak MySQL için Azure Veritabanıyla bağlantı kurar. Ardından UPDATE komutlarını çalıştırmak için [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) yöntemini çağırır. Ardından bağlantıyı sonlandırılmadan önce kapatmak için [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) yöntemini çağırır.

`host`, `database`, `username` ve `password` dizelerini kendi değerlerinizle değiştirin. 

```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('myserver4demo.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@myserver4demo')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection to database.'

    # Update data
   client.query('UPDATE inventory SET quantity = %d WHERE name = %s;' % [200, '\'banana\''])
   puts 'Updated 1 row of data.'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```


## <a name="delete-data"></a>Verileri silme
Aşağıdaki kodu kullanın ve kullanarak verileri okuyun bir **silmek** SQL deyimi. 

Kod, [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) sınıfı .new() yöntemini kullanarak MySQL için Azure Veritabanıyla bağlantı kurar. Ardından DELETE komutlarını çalıştırmak için [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) yöntemini çağırır. Ardından bağlantıyı sonlandırılmadan önce kapatmak için [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) yöntemini çağırır.

`host`, `database`, `username` ve `password` dizelerini kendi değerlerinizle değiştirin. 

```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('myserver4demo.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@myserver4demo')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection to database.'

    # Delete data
    resultSet = client.query('DELETE FROM inventory WHERE name = %s;' % ['\'orange\''])
    puts 'Deleted 1 row.'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Dışarı Aktarma ve İçeri Aktarma seçeneğini kullanarak veritabanınızı geçirme](./concepts-migrate-import-export.md)
