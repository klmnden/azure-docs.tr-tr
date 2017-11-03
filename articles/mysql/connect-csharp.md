---
title: "C#'den MySQL için Azure Veritabanı'na bağlanma | Microsoft Docs"
description: "Bu hızlı başlangıçta, MySQL için Azure Veritabanı'na bağlanmak ve buradan veri sorgulamak için kullanabileceğiniz bir C# (.NET) kod örneği sağlanmıştır."
services: MySQL
author: seanli1988
ms.author: seal
manager: janders
editor: jasonwhowell
ms.service: MySQL
ms.custom: mvc
ms.devlang: csharp
ms.topic: quickstart
ms.date: 09/22/2017
ms.openlocfilehash: f87c3c302ec25f33af4334c3753dfae0084e4393
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-database-for-mysql-use-net-c-to-connect-and-query-data"></a>MySQL için Azure Veritabanı: .NET (C#) kullanarak bağlanma ve veri sorgulama
Bu Hızlı Başlangıç, MySQL için C# uygulamasını kullanarak bir Azure veritabanına bağlanmak gösterilmiştir. Ayrıca veritabanında veri sorgulamak, eklemek, güncelleştirmek ve silmek için SQL deyimlerini nasıl kullanacağınız da gösterilmiştir. Bu konu, C# kullanarak geliştirme ile tanıdık ve MySQL için Azure veritabanı ile çalışmaya yeni varsayar.

## <a name="prerequisites"></a>Ön koşullar
Bu hızlı başlangıçta, başlangıç noktası olarak şu kılavuzlardan birinde oluşturulan kaynaklar kullanılmaktadır:
- [Azure portalını kullanarak MySQL için Azure Veritabanı sunucusu oluşturma](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Azure CLI kullanarak MySQL için Azure Veritabanı sunucusu oluşturma](./quickstart-create-mysql-server-database-using-azure-cli.md)

Şunları da yapmanız gerekir:
- [.NET](https://www.microsoft.com/net/download) yükleyin. NET’i platformunuza (Windows, Ubuntu Linux veya macOS) özel olarak yüklemek için bağlantılı makaledeki adımları izleyin. 
- [Visual Studio](https://www.visualstudio.com/downloads/)’yu yükleyin.
- [MySQL için ODBC Sürücüsü](https://dev.mysql.com/downloads/connector/odbc/)’nü yükleyin.

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma
MySQL için Azure Veritabanı'na bağlanmak üzere gereken bağlantı bilgilerini alın. Tam sunucu adına ve oturum açma kimlik bilgilerine ihtiyacınız vardır.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Azure portalında sol taraftaki menüden **tüm kaynakları**ve ardından oluşturduğunuz sunucu için arama (gibi **myserver4demo**).
3. Sunucunun adına tıklayın.
4. Sunucunun seçin **özellikleri** sayfasında ve sonra Not **sunucu adı** ve **sunucu yönetici oturum açma adı**.
 ![MySQL için Azure Veritabanı sunucu adı](./media/connect-csharp/1_server-properties-name-login.png)
5. Sunucu oturum açma bilgilerinizi unutursanız gidin **genel bakış** sunucu yönetici oturum açma adı görüntülemek için sayfa ve gerekirse, parola sıfırlama.

## <a name="connect-create-table-and-insert-data"></a>Bağlanma, tablo oluşturma ve veri ekleme
Aşağıdaki kodu kullanın ve verileri kullanarak yük **CREATE TABLE** ve **INSERT INTO** SQL deyimlerini. Kod, [Open()](https://msdn.microsoft.com/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) yöntemiyle birlikte ODBC sınıfını kullanarak MySQL ile bağlantı kurar. Ardından kod [CreateCommand()](https://msdn.microsoft.com/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx) yöntemini kullanarak CommandText özelliğini ayarlar ve [ExecuteNonQuery()](https://msdn.microsoft.com/library/system.data.odbc.odbccommand.executenonquery(v=vs.110).aspx) yöntemini çağırarak veritabanı komutlarını çalıştırır. 

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

Aşağıdaki kodu kullanın ve kullanarak verileri okuyun bir **seçin** SQL deyimi. Kod, [Open()](https://msdn.microsoft.com/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) yöntemiyle birlikte ODBC sınıfını kullanarak MySQL ile bağlantı kurar. Ardından kod [CreateCommand()](https://msdn.microsoft.com/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx) ve [ExecuteReader()](https://msdn.microsoft.com/library/system.data.odbc.odbccommand.executereader(v=vs.110).aspx) yöntemini kullanarak veritabanı komutlarını çalıştırır. Daha sonra kod [Read()](https://msdn.microsoft.com/library/system.data.odbc.odbcdatareader.read(v=vs.110).aspx) yöntemini kullanarak sonuçlardaki kayıtlara gider. Ardından kod, GetInt32 ve GetString yöntemini kullanarak kayıttaki değerleri ayrıştırır.

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
Aşağıdaki kodu kullanın ve kullanarak verileri okuyun bir **güncelleştirme** SQL deyimi. Kod, [Open()](https://msdn.microsoft.com/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) yöntemiyle birlikte ODBC sınıfını kullanarak MySQL ile bağlantı kurar. Ardından kod [CreateCommand()](https://msdn.microsoft.com/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx) yöntemini kullanarak CommandText özelliğini ayarlar ve [ExecuteNonQuery()](https://msdn.microsoft.com/library/system.data.odbc.odbccommand.executenonquery(v=vs.110).aspx) yöntemini çağırarak veritabanı komutlarını çalıştırır.

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
Aşağıdaki kodu kullanın ve kullanarak verileri silme bir **silmek** SQL deyimi. 

Kod, [Open()](https://msdn.microsoft.com/library/system.data.odbc.odbcconnection.open(v=vs.110).aspx) yöntemiyle birlikte ODBC sınıfını kullanarak MySQL ile bağlantı kurar. Ardından kod [CreateCommand()](https://msdn.microsoft.com/library/system.data.odbc.odbcconnection.createcommand(v=vs.110).aspx) yöntemini kullanarak CommandText özelliğini ayarlar ve [ExecuteNonQuery()](https://msdn.microsoft.com/library/system.data.odbc.odbccommand.executenonquery(v=vs.110).aspx) yöntemini çağırarak veritabanı komutlarını çalıştırır.

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
