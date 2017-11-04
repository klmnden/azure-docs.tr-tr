---
title: "C++'tan MySQL için Azure Veritabanı'na bağlanma | Microsoft Docs"
description: "Bu hızlı başlangıçta, MySQL için Azure Veritabanı'na bağlanmak ve buradan veri sorgulamak için kullanabileceğiniz bir C++ kod örneği sağlanmıştır."
services: mysql
author: seanli1988
ms.author: seal
manager: janders
editor: jasonwhowell
ms.service: mysql
ms.custom: mvc
ms.devlang: C++
ms.topic: quickstart
ms.date: 09/22/2017
ms.openlocfilehash: 92620c8081b1f0f5c96cc3ae09465b3526e74042
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-database-for-mysql-use-connectorc-to-connect-and-query-data"></a>MySQL için Azure Veritabanı: Connector/C++ kullanarak bağlanma ve veri sorgulama
Bu Hızlı Başlangıç, C++ uygulaması kullanarak MySQL için bir Azure veritabanına bağlanmak gösterilmiştir. Ayrıca veritabanında veri sorgulamak, eklemek, güncelleştirmek ve silmek için SQL deyimlerini nasıl kullanacağınız da gösterilmiştir. Bu konu, C++ kullanarak geliştirme ile tanıdık ve MySQL için Azure veritabanı ile çalışmaya yeni varsayar.

## <a name="prerequisites"></a>Ön koşullar
Bu hızlı başlangıç aşağıdaki kılavuzlara birini başlangıç noktası olarak oluşturulan kaynakları kullanır:
- [Azure portalını kullanarak MySQL için Azure Veritabanı sunucusu oluşturma](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Azure CLI kullanarak MySQL için Azure Veritabanı sunucusu oluşturma](./quickstart-create-mysql-server-database-using-azure-cli.md)

Şunları da yapmanız gerekir:
- [.Net Framework](https://www.microsoft.com/net/download)'ü yükleme
- [Visual Studio](https://www.visualstudio.com/downloads/)'yu yükleme
- [MySQL Connector/C++](https://dev.mysql.com/downloads/connector/cpp/) yükleyin 
- [Boost](http://www.boost.org/)’u yükleyin

## <a name="install-visual-studio-and-net"></a>Visual Studio'yu ve .NET'i yükleme
Bu bölümdeki adımlarda .NET kullanarak geliştirmeyle ilgili bilgi sahibi olduğunuz varsayılır.

### <a name="windows"></a>**Windows**
- Tanıtılan, Android, iOS, Windows yanı sıra web için modern uygulamalar ve veritabanı uygulamaları oluşturmak için genişletilebilir, ücretsiz IDE tam olan Visual Studio 2017 Community, yükleyin ve bulut Hizmetleri. Tam .NET Framework veya .NET Core yükleyebilirsiniz: Hızlı Başlangıç, kod parçacıkları ile birlikte çalışır. Bilgisayarınızda yüklü Visual Studio zaten varsa, sonraki iki adımı atlayın.
   1. [Visual Studio 2017 yükleyicisi](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15)ni indirin. 
   2. Yükleyiciyi çalıştırın ve yükleme istemlerini izleyerek yüklemeyi tamamlayın.

### <a name="configure-visual-studio"></a>**Visual Studio'yu yapılandırma**
1. Visual Studio’dan, proje özelliği > yapılandırma özellikleri > C/C++ > bağlayıcı > genel > ek kitaplık dizinleri’ne gidip c++ bağlayıcısının lib\opt dizinini (ör. C:\Program Files (x86)\MySQL\MySQL Connector C++ 1.1.9\lib\opt) ekleyin. 
2. Visual Studio'dan özellik Proje > yapılandırma özellikleri > C/C++ > Genel > ek içeren dizinler:
   - Ekleme dahil / c ++ bağlayıcı dizininde (örn: C:\Program Files (x86) \MySQL\MySQL bağlayıcı C++ 1.1.9\include\).
   - Artırma kitaplığın kök dizin ekleyin (ör: C:\boost_1_64_0\).
3. Visual Studio'dan özellik Proje > yapılandırma özellikleri > C/C++ > bağlayıcı > giriş > ek bağımlılıklar mysqlcppconn.lib metin alanına ekleyin.
4. Adım 3'te C++ bağlayıcı kitaplık klasörü mysqlcppconn.dll uygulama yürütülebilir dosyasının aynı dizine kopyalayın ya da uygulamanızı dosyayı bulması için ortam değişkenine ekleyin.

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma
MySQL için Azure Veritabanı'na bağlanmak üzere gereken bağlantı bilgilerini alın. Tam sunucu adına ve oturum açma kimlik bilgilerine ihtiyacınız vardır.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Azure portalında sol taraftaki menüden **tüm kaynakları**, oluşturduğunuz sunucunun arayın (gibi **myserver4demo**).
3. Sunucunun adına tıklayın.
4. Sunucunun seçin **özellikleri** sayfasında ve sonra Not **sunucu adı** ve **sunucu yönetici oturum açma adı**.
 ![MySQL için Azure Veritabanı sunucu adı](./media/connect-cpp/1_server-properties-name-login.png)
5. Sunucu oturum açma bilgilerinizi unutursanız gidin **genel bakış** sunucu yönetici oturum açma adı görüntülemek için sayfa ve gerekirse, parola sıfırlama.

## <a name="connect-create-table-and-insert-data"></a>Bağlanma, tablo oluşturma ve veri ekleme
Aşağıdaki kodu kullanın ve verileri kullanarak yük **CREATE TABLE** ve **INSERT INTO** SQL deyimlerini. Kod MySQL ile bağlantı kurmak için connect() yöntemiyle sql::Driver sınıfını kullanır. Ardından kod veritabanı komutlarını çalıştırmak için createStatement() ve execute() yöntemlerini kullanır. 

Host, DBName, User ve Password parametrelerini, sunucuyu ve veritabanını oluştururken belirttiğiniz değerlerle değiştirin. 

```c++
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::Statement *stmt;
    sql::PreparedStatement *pstmt;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }

    stmt = con>createStatement();
    stmt>execute("DROP TABLE IF EXISTS inventory");
    cout << "Finished dropping table (if existed)" << endl;
    stmt>execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);");
    cout << "Finished creating table" << endl;
    delete stmt;

    pstmt = con>prepareStatement("INSERT INTO inventory(name, quantity) VALUES(?,?)");
    pstmt>setString(1, "banana");
    pstmt>setInt(2, 150);
    pstmt>execute();
    cout << "One row inserted." << endl;

    pstmt>setString(1, "orange");
    pstmt>setInt(2, 154);
    pstmt>execute();
    cout << "One row inserted." << endl;

    pstmt>setString(1, "apple");
    pstmt>setInt(2, 100);
    pstmt>execute();
    cout << "One row inserted." << endl;
    
    delete pstmt;   
    delete con;
    system("pause");
    return 0;

```

## <a name="read-data"></a>Verileri okuma

Aşağıdaki kodu kullanın ve kullanarak verileri okuyun bir **seçin** SQL deyimi. Kod MySQL ile bağlantı kurmak için connect() yöntemiyle sql::Driver sınıfını kullanır. Ardından kod, seçme komutlarını çalıştırmak için prepareStatement() ve executeQuery() yöntemlerini kullanır. Ardından, kod next() sonuçları kayıtları ilerletmek için kullanır. Son olarak, kodu getInt() ve getString() kayıttaki değerleri ayrıştırmak için kullanır.

Host, DBName, User ve Password parametrelerini, sunucuyu ve veritabanını oluştururken belirttiğiniz değerlerle değiştirin. 

```csharp
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/resultset.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::PreparedStatement *pstmt;
    sql::ResultSet *result;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }   

//  select  
    pstmt = con>prepareStatement("SELECT * FROM inventory;");
    result = pstmt>executeQuery();  
    
    while (result>next())
        printf("Reading from table=(%d, %s, %d)\n", result>getInt(1), result>getString(2).c_str(), result>getInt(3));   
    
    delete result;
    delete pstmt;   
    delete con;
    system("pause");
    return 0;
}
```

## <a name="update-data"></a>Verileri güncelleştirme
Aşağıdaki kodu kullanın ve kullanarak verileri okuyun bir **güncelleştirme** SQL deyimi. Kod MySQL ile bağlantı kurmak için connect() yöntemiyle sql::Driver sınıfını kullanır. Ardından kod, güncelleme komutlarını çalıştırmak için prepareStatement() ve executeQuery() yöntemlerini kullanır. 

Host, DBName, User ve Password parametrelerini, sunucuyu ve veritabanını oluştururken belirttiğiniz değerlerle değiştirin. 

```csharp
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::PreparedStatement *pstmt;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }   

    //update
    pstmt = con>prepareStatement("UPDATE inventory SET quantity = ? WHERE name = ?");
    pstmt>setInt(1, 200);
    pstmt>setString(2, "banana");
    pstmt>executeQuery();
    printf("Row updated\n");
    
    delete con;
    delete pstmt;
    system("pause");
    return 0;
}
```


## <a name="delete-data"></a>Verileri silme
Aşağıdaki kodu kullanın ve kullanarak verileri okuyun bir **silmek** SQL deyimi. Kod MySQL ile bağlantı kurmak için connect() yöntemiyle sql::Driver sınıfını kullanır. Ardından kod, silme komutlarını çalıştırmak için prepareStatement() ve executeQuery() yöntemlerini kullanır.

Host, DBName, User ve Password parametrelerini, sunucuyu ve veritabanını oluştururken belirttiğiniz değerlerle değiştirin. 

```csharp
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/resultset.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::PreparedStatement *pstmt;
    sql::ResultSet *result;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }
        
    //delete
    pstmt = con>prepareStatement("DELETE FROM inventory WHERE name = ?");
    pstmt>setString(1, "orange");
    result = pstmt>executeQuery();
    printf("Row deleted\n");    
    
    delete pstmt;
    delete con;
    delete result;
    system("pause");
    return 0;
}
```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Döküm alma ve geri yükleme işlemlerini kullanarak MySQL veritabanınızı MySQL için Azure Veritabanı'na geçirme](concepts-migrate-dump-restore.md)
