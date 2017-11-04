---
title: "Ruby kullanarak PostgreSQL için Azure Veritabanı’na bağlanma | Microsoft Docs"
description: "Bu hızlı başlangıçta, PostgreSQL için Azure Veritabanı’na bağlanmak ve buradan veri sorgulamak için kullanabileceğiniz Ruby kod örneği sağlanmıştır."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: ruby
ms.topic: quickstart
ms.date: 06/30/2017
ms.openlocfilehash: 7ad4a2fd7452f31689a02b144078a9630bfd5259
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2017
---
# <a name="azure-database-for-postgresql-use-ruby-to-connect-and-query-data"></a>PostgreSQL için Azure Veritabanı: Bağlanmak ve veri sorgulamak için Ruby’yi kullanma
Bu hızlı başlangıçta, [Ruby](https://www.ruby-lang.org) uygulaması kullanılarak PostgreSQL için Azure Veritabanı’na nasıl bağlanılacağı gösterilmiştir. Ayrıca veritabanında veri sorgulamak, eklemek, güncelleştirmek ve silmek için SQL deyimlerini nasıl kullanacağınız da gösterilmiştir. Bu makaledeki adımları Ruby kullanarak geliştirme ile tanıdık ve PostgreSQL için Azure veritabanı ile çalışmaya yeni varsayalım.

## <a name="prerequisites"></a>Ön koşullar
Bu hızlı başlangıçta, başlangıç noktası olarak şu kılavuzlardan birinde oluşturulan kaynaklar kullanılmaktadır:
- [DB Oluşturma - Portal](quickstart-create-server-database-portal.md)
- [DB Oluşturma - Azure CLI](quickstart-create-server-database-azure-cli.md)

## <a name="install-ruby"></a>Ruby’yi yükleme
Ruby’yi kendi makinenize yükleyin. 

### <a name="windows"></a>Windows
- [Ruby'nin](http://rubyinstaller.org/downloads/) en son sürümünü indirip yükleyin.
- MSI yükleyicisinin son ekranında, “MSYS2’yi ve geliştirme araç zincirini yüklemek için ‘ridk install’u çalıştır” yazan kutuyu işaretleyin. Ardından sonraki yükleyiciyi başlatmak için **Son**’a tıklayın.
- Windows installer için RubyInstaller2 başlatılır. MSYS2 depo güncelleştirmesini yüklemek için 2 yazın. Tamamlandıktan ve yükleme istemine döndükten sonra, komut penceresini kapatın.
- Başlat menüsünden yeni bir komut istemi (cmd) başlatın.
- Yüklenen sürümü görmek için `ruby -v` Ruby yüklemesini test edin.
- Yüklenen sürümü görmek için `gem -v` Gem yüklemesini test edin.
- `gem install pg` komutunu çalıştırarak Gem kullanan Ruby için PostgreSQL modülünü oluşturun.

### <a name="macos"></a>MacOS
- `brew install ruby` komutunu çalıştırarak Homebrew kullanan Ruby’yi yükleyin. Daha fazla yükleme seçeneği için Ruby [yükleme belgelerine](https://www.ruby-lang.org/en/documentation/installation/#homebrew) bakın
- Yüklenen sürümü görmek için `ruby -v` Ruby yüklemesini test edin.
- Yüklenen sürümü görmek için `gem -v` Gem yüklemesini test edin.
- `gem install pg` komutunu çalıştırarak Gem kullanan Ruby için PostgreSQL modülünü oluşturun.

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
- `sudo apt-get install ruby-full` komutunu çalıştırarak Ruby’yi yükleyin. Daha fazla yükleme seçeneği için Ruby [yükleme belgelerine](https://www.ruby-lang.org/en/documentation/installation/) bakın.
- Yüklenen sürümü görmek için `ruby -v` Ruby yüklemesini test edin.
- `sudo gem update --system` komutunu çalıştırarak Gem için en son güncelleştirmeleri yükleyin.
- Yüklenen sürümü görmek için `gem -v` Gem yüklemesini test edin.
- `sudo apt-get install build-essential` komutunu çalıştırarak gcc, make ve diğer derleme araçlarını yükleyin.
- `sudo apt-get install libpq-dev` komutunu çalıştırarak PostgreSQL kitaplıklarını yükleyin.
- `sudo gem install pg` komutunu çalıştırarak Gem kullanan Ruby pg modülünü oluşturun.

## <a name="run-ruby-code"></a>Ruby kodunu çalıştırma 
- Kodu bir metin dosyasına kaydedin ve dosyayı `C:\rubypostgres\read.rb` veya `/home/username/rubypostgres/read.rb` gibi .rb dosya uzantısıyla bir proje klasörüne yükleyin
- Kodu çalıştırmak için komut istemi veya bash kabuğu başlatın. Dizini `cd rubypostgres` proje klasörünüzle değiştirin, ardından uygulamayı çalıştırmak için `ruby read.rb` komutunu yazın.

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma
PostgreSQL için Azure Veritabanı'na bağlanmak üzere gereken bağlantı bilgilerini alın. Tam sunucu adına ve oturum açma kimlik bilgilerine ihtiyacınız vardır.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Azure portalında sol taraftaki menüden **Tüm kaynaklar**'a tıklayın ve oluşturduğunuz sunucuyu (örneğin, **mypgserver-20170401**) arayın.
3. **mypgserver-20170401** sunucu adına tıklayın.
4. Sunucunun **Genel Bakış** sayfasını seçin. **Sunucu adını** ve **Sunucu yöneticisi oturum açma adını** not edin.
 ![PostgreSQL için Azure Veritabanı - Sunucu Yöneticisi Oturum Açma](./media/connect-ruby/1-connection-string.png)
5. Sunucunuzun oturum açma bilgilerini unuttuysanız **Genel Bakış** sayfasına giderek Sunucu yöneticisi oturum açma adını görüntüleyin. Gerekirse parolayı sıfırlayın.

## <a name="connect-and-create-a-table"></a>Bir tabloyu bağlama ve oluşturma
**CREATE TABLE** SQL deyimini kullanarak bir tabloyu bağlamak ve oluşturmak ve ardından **INSERT INTO** SQL deyimlerini kullanarak tabloya satırlar eklemek için aşağıdaki kodu kullanın.

Kod, PostgreSQL için Azure Veritabanı’na bağlanmak amacıyla [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) oluşturucusuna sahip [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) nesnesini kullanır. Ardından DROP, CREATE TABLE ve INSERT INTO komutlarını çalıştırmak için [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) yöntemini çağırır. Kod, [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) sınıfını kullanarak hataları kontrol eder. Ardından bağlantıyı sonlandırılmadan önce kapatmak için [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) yöntemini çağırır.

`host`, `database`, `user` ve `password` dizelerini kendi değerlerinizle değiştirin. 
```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mypgserver-20170401.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mypgserver-20170401')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection to database'

    # Drop previous table of same name if one exists
    connection.exec('DROP TABLE IF EXISTS inventory;')
    puts 'Finished dropping table (if existed).'

    # Drop previous table of same name if one exists.
    connection.exec('CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);')
    puts 'Finished creating table.'

    # Insert some data into table.
    connection.exec("INSERT INTO inventory VALUES(1, 'banana', 150)")
    connection.exec("INSERT INTO inventory VALUES(2, 'orange', 154)")
    connection.exec("INSERT INTO inventory VALUES(3, 'apple', 100)")
    puts 'Inserted 3 rows of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

## <a name="read-data"></a>Verileri okuma
**SELECT** SQL deyimini kullanarak bağlanmak ve verileri okumak için aşağıdaki kodu kullanın. 

Kod, PostgreSQL için Azure Veritabanı’na bağlanmak amacıyla [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) oluşturucusuna sahip [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) nesnesini kullanır. Ardından sonuçları sonuç kümesinde tutarak SELECT komutunu çalıştırmak için [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) yöntemini çağırır. Sonuç kümesi koleksiyonu, `resultSet.each do` döngüsü kullanılarak ve geçerli satır değerleri `row` değişkeninde tutularak yinelenir. Kod, [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) sınıfını kullanarak hataları kontrol eder. Ardından bağlantıyı sonlandırılmadan önce kapatmak için [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) yöntemini çağırır.

`host`, `database`, `user` ve `password` dizelerini kendi değerlerinizle değiştirin. 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mypgserver-20170401.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mypgserver-20170401')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :database => dbname, :port => '5432', :password => password)
    puts 'Successfully created connection to database.'

    resultSet = connection.exec('SELECT * from inventory;')
    resultSet.each do |row|
        puts 'Data row = (%s, %s, %s)' % [row['id'], row['name'], row['quantity']]
    end

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

## <a name="update-data"></a>Verileri güncelleştirme
**UPDATE** SQL deyimini kullanarak bağlanmak ve verileri güncelleştirmek için aşağıdaki kodu kullanın.

Kod, PostgreSQL için Azure Veritabanı’na bağlanmak amacıyla [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) oluşturucusuna sahip [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) nesnesini kullanır. Ardından UPDATE komutunu çalıştırmak için [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) yöntemini çağırır. Kod, [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) sınıfını kullanarak hataları kontrol eder. Ardından bağlantıyı sonlandırılmadan önce kapatmak için [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) yöntemini çağırır.

`host`, `database`, `user` ve `password` dizelerini kendi değerlerinizle değiştirin. 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mypgserver-20170401.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mypgserver-20170401')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection to database.'

    # Modify some data in table.
    connection.exec('UPDATE inventory SET quantity = %d WHERE name = %s;' % [200, '\'banana\''])
    puts 'Updated 1 row of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```


## <a name="delete-data"></a>Verileri silme
**DELETE** SQL deyimini kullanarak bağlanmak ve verileri okumak için aşağıdaki kodu kullanın. 

Kod, PostgreSQL için Azure Veritabanı’na bağlanmak amacıyla [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) oluşturucusuna sahip [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) nesnesini kullanır. Ardından UPDATE komutunu çalıştırmak için [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) yöntemini çağırır. Kod, [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) sınıfını kullanarak hataları kontrol eder. Ardından bağlantıyı sonlandırılmadan önce kapatmak için [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) yöntemini çağırır.

`host`, `database`, `user` ve `password` dizelerini kendi değerlerinizle değiştirin. 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mypgserver-20170401.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mypgserver-20170401')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection to database.'

    # Modify some data in table.
    connection.exec('DELETE FROM inventory WHERE name = %s;' % ['\'orange\''])
    puts 'Deleted 1 row of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Dışarı Aktarma ve İçeri Aktarma seçeneğini kullanarak veritabanınızı geçirme](./howto-migrate-using-export-and-import.md)
