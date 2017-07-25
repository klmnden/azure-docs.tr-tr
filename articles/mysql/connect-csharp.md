---
title: "C#'den MySQL için Azure Veritabanı'na bağlanma | Microsoft Docs"
description: "Bu hızlı başlangıçta, MySQL için Azure Veritabanı'na bağlanmak ve buradan veri sorgulamak için kullanabileceğiniz bir C# (.Net) kod örneği sağlanmıştır."
services: MySQL
author: seanli1988
ms.author: seal
manager: janders
editor: jasonwhowell
ms.service: MySQL-database
ms.custom: mvc
ms.devlang: csharp
ms.topic: hero-article
ms.date: 07/10/2017
ms.translationtype: HT
ms.sourcegitcommit: 9afd12380926d4e16b7384ff07d229735ca94aaa
ms.openlocfilehash: ffe3ae320a61031cf314cc1d70e0c093b033f85c
ms.contentlocale: tr-tr
ms.lasthandoff: 07/15/2017

---

# <a name="azure-database-for-mysql-use-net-c-to-connect-and-query-data"></a>MySQL için Azure Veritabanı: .NET (C#) kullanarak bağlanma ve veri sorgulama
Bu hızlı başlangıçta C# uygulaması kullanarak MySQL için Azure Veritabanı'na nasıl bağlanacağınız gösterilmiştir. Ayrıca veritabanında veri sorgulamak, eklemek, güncelleştirmek ve silmek için SQL deyimlerini nasıl kullanacağınız da gösterilmiştir. Bu makaledeki adımlarda, C# kullanarak geliştirmeyle ilgili bilgi sahibi olduğunuz ve MySQL için Azure Veritabanı ile çalışmaya yeni başladığınız varsayılır.

## <a name="prerequisites"></a>Ön koşullar
Bu hızlı başlangıçta, başlangıç noktası olarak şu kılavuzlardan birinde oluşturulan kaynaklar kullanılmaktadır:
- [Azure portalını kullanarak MySQL için Azure Veritabanı sunucusu oluşturma](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Azure CLI kullanarak MySQL için Azure Veritabanı sunucusu oluşturma](./quickstart-create-mysql-server-database-using-azure-cli.md)

Şunları da yapmanız gerekir:
- [.Net Framework](https://www.microsoft.com/net/download)'ü yükleme
- [Visual Studio](https://www.visualstudio.com/downloads/)'yu yükleme
- [MySQL için ODBC Sürücüsü](https://dev.mysql.com/downloads/connector/odbc/)’nü yükleme 

## <a name="install-visual-studio-and-net"></a>Visual Studio'yu ve .NET'i yükleme
Bu bölümdeki adımlarda .NET kullanarak geliştirmeyle ilgili bilgi sahibi olduğunuz varsayılır.

### <a name="windows-net-framework-and-net-core"></a>**Windows .NET çerçevesi ve .NET çekirdeği**
Visual Studio 2017 Community; Android, iOS ve Windows’un yanı sıra web ve veritabanı uygulamaları ile bulut hizmetleri için modern uygulamalar oluşturmaya yönelik tam özellikli, genişletilebilir, ücretsiz bir IDE’dir. Tam .NET çerçevesini ya da yalnızca .NET çekirdeğini yükleyebilirsiniz. Hızlı başlangıçtaki kod parçacıkları her ikisiyle de çalışır. Makinenizde Visual Studio zaten yüklüyse, sonraki birkaç adımı atlayın.

1. [Visual Studio 2017 yükleyicisi](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15)ni indirin. 
2. Yükleyiciyi çalıştırın ve yükleme istemlerini izleyerek yüklemeyi tamamlayın.

### <a name="mac-os"></a>**Mac OS**
Terminalinizi açın ve .NET Core projenizi oluşturmayı planladığınız bir dizine gidin. **brew**, **OpenSSL** ve **.NET Core** yüklemek için aşağıdaki komutları girin. 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install openssl
mkdir -p /usr/local/lib
ln -s /usr/local/opt/openssl/lib/libcrypto.1.0.0.dylib /usr/local/lib/
ln -s /usr/local/opt/openssl/lib/libssl.1.0.0.dylib /usr/local/lib/
```

macOS işletim sisteminde .NET Core yükleyin. [Resmi yükleyiciyi](https://go.microsoft.com/fwlink/?linkid=843444) indirin. Bu yükleyici, araçları yükler ve Konsoldan .net çalıştırabilmeniz için PATH değişkeninize yerleştirir

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
Terminalinizi açın ve .NET Core projenizi oluşturmayı planladığınız bir dizine gidin. **.NET Core** yüklemek için aşağıdaki komutları girin.

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
sudo apt-get update
sudo apt-get install dotnet-dev-1.0.1
```


## <a name="get-connection-information"></a>Bağlantı bilgilerini alma
MySQL için Azure Veritabanı'na bağlanmak üzere gereken bağlantı bilgilerini alın. Tam sunucu adına ve oturum açma kimlik bilgilerine ihtiyacınız vardır.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Azure portalında sol taraftaki menüden **Tüm kaynaklar**'a tıklayın ve oluşturduğunuz sunucuyu (örneğin, **myserver4demo**) arayın.
3. Sunucunun adına tıklayın.
4. Sunucunun **Özellikler** sayfasını seçin. **Sunucu adını** ve **Sunucu yöneticisi oturum açma adını** not edin.
 ![MySQL için Azure Veritabanı sunucu adı](./media/connect-csharp/1_server-properties-name-login.png)
5. Sunucunuzun oturum açma bilgilerini unuttuysanız **Genel Bakış** sayfasına giderek Sunucu yöneticisi oturum açma adını görüntüleyin ve gerekirse parolayı sıfırlayın.

## <a name="connect-create-table-and-insert-data"></a>Bağlanma, tablo oluşturma ve veri ekleme
Bağlanıp **CREATE TABLE** ve **INSERT INTO** SQL deyimlerini kullanarak verileri yüklemek için aşağıdaki kodu kullanın. Kod, [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) yöntemiyle birlikte ODBC sınıfını kullanarak MySQL ile bağlantı kurar. Ardından kod [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx) yöntemini kullanarak CommandText özelliğini ayarlar ve [ExecuteNonQuery()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executenonquery(v=vs.110).aspx) yöntemini çağırarak veritabanı komutlarını çalıştırır. 

Host, DBName, User ve Password parametrelerini, sunucuyu ve veritabanını oluştururken belirttiğiniz değerlerle değiştirin. 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using MySql.Data;
using System.Data.Odbc;

namespace driver
{
    class MySQLCreate
    {
        static void Main(string[] args)
        {
            var conn = new OdbcConnection("DRIVER={MySQL ODBC 5.3 unicode Driver}; Server=myserver4demo.mysql.database.azure.com; Port=3306;" +
            " Database=quickstartdb; Uid=myadmin@myserver4demo; Pwd=server_admin_password; sslverify=0; Option=3;MULTI_STATEMENTS=1");

            Console.Out.WriteLine("Opening connection");
            conn.Open();

            var command = conn.CreateCommand();
            command.CommandText = "DROP TABLE IF EXISTS inventory;";
            command.ExecuteNonQuery();
            Console.Out.WriteLine("Finished dropping table (if existed)");

            command.CommandText = "CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);";
            command.ExecuteNonQuery();
            Console.Out.WriteLine("Finished creating table");

            command.CommandText =
                String.Format(
                    @"INSERT INTO inventory (name, quantity) VALUES ({0}, {1});
                    INSERT INTO inventory (name, quantity) VALUES ({2}, {3});
                    INSERT INTO inventory (name, quantity) VALUES ({4}, {5});",
                    "\'banana\'", 150,
                    "\'orange\'", 154,
                    "\'apple\'", 100
                    );

            int nRows = command.ExecuteNonQuery();
            Console.Out.WriteLine(String.Format("Number of rows inserted={0}", nRows));

            Console.Out.WriteLine("Closing connection");
            conn.Close();

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }

    }
}

```

## <a name="read-data"></a>Verileri okuma

Bağlanmak ve **SELECT** SQL deyimi kullanarak verileri okumak için aşağıdaki kodu kullanın. Kod, [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) yöntemiyle birlikte ODBC sınıfını kullanarak MySQL ile bağlantı kurar. Ardından kod [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx) ve [ExecuteReader()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executereader(v=vs.110).aspx) yöntemini kullanarak veritabanı komutlarını çalıştırır. Daha sonra kod [Read()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcdatareader.read(v=vs.110).aspx) yöntemini kullanarak sonuçlardaki kayıtlara gider. Ardından kod, GetInt32 ve GetString yöntemini kullanarak kayıttaki değerleri ayrıştırır.

Host, DBName, User ve Password parametrelerini, sunucuyu ve veritabanını oluştururken belirttiğiniz değerlerle değiştirin. 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using MySql.Data;
using System.Data.Odbc;


namespace driver
{
    class MySQLRead
    {

        static void Main(string[] args)
        {
            var conn = new OdbcConnection("DRIVER={MySQL ODBC 5.3 unicode Driver}; Server=myserver4demo.mysql.database.azure.com; Port=3306;" +
            " Database=quickstartdb; Uid=myadmin@myserver4demo; Pwd=server_admin_password; sslverify=0; Option=3;MULTI_STATEMENTS=1");

            Console.Out.WriteLine("Opening connection");
            conn.Open();

            var command = conn.CreateCommand();
            command.CommandText = "SELECT * FROM inventory;";

            var reader = command.ExecuteReader();
            while (reader.Read())
            {
                Console.WriteLine(
                    string.Format(
                        "Reading from table=({0}, {1}, {2})",
                        reader.GetInt32(0).ToString(),
                        reader.GetString(1),
                        reader.GetInt32(2).ToString()
                        )
                    );
            }

            Console.Out.WriteLine("Closing connection");
            conn.Close();

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}


```

## <a name="update-data"></a>Verileri güncelleştirme
Bağlanmak ve **UPDATE** SQL deyimi kullanarak verileri okumak için aşağıdaki kodu kullanın. Kod, [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) yöntemiyle birlikte ODBC sınıfını kullanarak MySQL ile bağlantı kurar. Ardından kod [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx) yöntemini kullanarak CommandText özelliğini ayarlar ve [ExecuteNonQuery()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executenonquery(v=vs.110).aspx) yöntemini çağırarak veritabanı komutlarını çalıştırır.

Host, DBName, User ve Password parametrelerini, sunucuyu ve veritabanını oluştururken belirttiğiniz değerlerle değiştirin. 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Odbc;

namespace driver
{
    class MySQLUpdate
    {
        static void Main(string[] args)
        {
            var conn = new OdbcConnection("DRIVER={MySQL ODBC 5.3 unicode Driver}; Server=myserver4demo.mysql.database.azure.com; Port=3306;" +
            " Database=quickstartdb; Uid=myadmin@myserver4demo; Pwd=server_admin_password; sslverify=0; Option=3;MULTI_STATEMENTS=1");

            Console.Out.WriteLine("Opening connection");
            conn.Open();

            var command = conn.CreateCommand();
            command.CommandText =
            String.Format("UPDATE inventory SET quantity = {0} WHERE name = {1};",
                200,
                "\'banana\'"
                );

            int nRows = command.ExecuteNonQuery();
            Console.Out.WriteLine(String.Format("Number of rows updated={0}", nRows));

            Console.Out.WriteLine("Closing connection");
            conn.Close();

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}



```


## <a name="delete-data"></a>Verileri silme
Bağlanmak ve **DELETE** SQL deyimi kullanarak verileri okumak için aşağıdaki kodu kullanın. 

Kod, [Open()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) yöntemiyle birlikte ODBC sınıfını kullanarak MySQL ile bağlantı kurar. Ardından kod [CreateCommand()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx) yöntemini kullanarak CommandText özelliğini ayarlar ve [ExecuteNonQuery()](https://msdn.microsoft.com/en-us/library/system.data.odbc.odbccommand.executenonquery(v=vs.110).aspx) yöntemini çağırarak veritabanı komutlarını çalıştırır.

Host, DBName, User ve Password parametrelerini, sunucuyu ve veritabanını oluştururken belirttiğiniz değerlerle değiştirin. 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Odbc;

namespace driver
{
    class MySQLDelete
    {
        static void Main(string[] args)
        {
            var conn = new OdbcConnection("DRIVER={MySQL ODBC 5.3 unicode Driver}; Server=myserver4demo.mysql.database.azure.com; Port=3306;" +
            " Database=quickstartdb; Uid=myadmin@myserver4demo; Pwd=server_admin_password; sslverify=0; Option=3;MULTI_STATEMENTS=1");

            Console.Out.WriteLine("Opening connection");
            conn.Open();

            var command = conn.CreateCommand();
            command.CommandText =
                String.Format("DELETE FROM inventory WHERE name = {0};",
                    "\'orange\'");
            int nRows = command.ExecuteNonQuery();
            Console.Out.WriteLine(String.Format("Number of rows deleted={0}", nRows));

            Console.Out.WriteLine("Closing connection");
            conn.Close();

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}

```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Döküm alma ve geri yükleme işlemlerini kullanarak MySQL veritabanınızı MySQL için Azure Veritabanı'na geçirme](concepts-migrate-dump-restore.md)

