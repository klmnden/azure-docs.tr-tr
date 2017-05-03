---
title: ".NET (C#) kullanarak Azure SQL Veritabanına bağlanma | Microsoft Docs"
description: "Azure SQL Veritabanı’na bağlanmak ve veritabanını sorgulamak için kullanabileceğiniz bir .NET kod örneği sunar"
services: sql-database
documentationcenter: 
author: ajlam
manager: jhubbard
editor: 
ms.assetid: 7faca033-24b4-4f64-9301-b4de41e73dfd
ms.service: sql-database
ms.custom: quick start connect
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 04/17/2017
ms.author: andrela;sstein;carlrab
translationtype: Human Translation
ms.sourcegitcommit: abdbb9a43f6f01303844677d900d11d984150df0
ms.openlocfilehash: 119ffa3ac31e0ea6e76f8232f13b4dd8667f78aa
ms.lasthandoff: 04/20/2017


---
# <a name="azure-sql-database-use-net-c-to-connect-and-query-data"></a>Azure SQL Veritabanı: .NET (C#) kullanarak verileri bağlama ve sorgulama

Bu hızlı başlangıçta [C# ve ADO.NET](https://msdn.microsoft.com/library/kb9s9ks0.aspx) kullanarak bir Azure SQL veritabanına bağlanma ve daha sonra Windows, Mac OS ve Ubuntu Linux platformlarından Transact-SQL deyimlerini kullanarak veritabanındaki verileri sorgulama, ekleme, güncelleştirme ve silme işlemlerinin nasıl yapılacağı açıklanır.

Bu hızlı başlangıçta başlangıç noktası olarak bu hızlı başlangıçlardan birinde oluşturulan kaynaklar kullanılır:

- [DB Oluşturma - Portal](sql-database-get-started-portal.md)
- [DB oluşturma - CLI](sql-database-get-started-cli.md)

## <a name="install-net"></a>.NET yükleme

Bu bölümdeki adımlarda .NET kullanarak geliştirmeyi bildiğiniz ve Azure SQL Veritabanı iye yeni çalışmaya başladığınız varsayılır. .NET ile geliştirmeye yeni başladıysanız, [SQL Server kullanarak uygulama geliştirme](https://www.microsoft.com/en-us/sql-server/developer-get-started/) konusuna gidin, **C#** dilini ve sonra da işletim sisteminizi seçin.

### <a name="windows-net-framework-and-net-core"></a>**Windows .NET çerçevesi ve .NET çekirdeği**

Visual Studio 2017 Community; Android, iOS ve Windows’un yanı sıra web ve veritabanı uygulamaları ile bulut hizmetleri için modern uygulamalar oluşturmaya yönelik tam özellikli, genişletilebilir, ücretsiz bir IDE’dir. Tam .NET çerçevesini ya da yalnızca .NET çekirdeğini yükleyebilirsiniz. Hızlı başlangıçtaki kod parçacıkları her ikisiyle de çalışır. Makinenizde Visual Studio zaten yüklüyse, sonraki birkaç adımı atlayın.

1. [Yükleyiciyi](https://go.microsoft.com/fwlink/?LinkId=691978) indirin. 
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

macOS işletim sisteminde .NET Core yükleyin. [Resmi yükleyiciyi](https://go.microsoft.com/fwlink/?linkid=843444) indirin. Bu yükleyici, araçları yükler ve Konsoldan dotnet çalıştırabilmeniz için PATH değişkeninize yerleştirir

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
Terminalinizi açın ve .NET Core projenizi oluşturmayı planladığınız bir dizine gidin. **.NET Core** yüklemek için aşağıdaki komutları girin.

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
sudo apt-get update
sudo apt-get install dotnet-dev-1.0.1
```

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Sonraki yordamlarda tam sunucu adına, veritabanı adına ve oturum açma bilgilerine ihtiyacınız olacaktır.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Soldaki menüden **SQL Veritabanları**’nı seçin ve **SQL veritabanları** sayfasında veritabanınıza tıklayın. 
3. Veritabanınızın **Genel Bakış** sayfasında, aşağıdaki görüntüde gösterildiği gibi tam sunucu adını gözden geçirin. Sunucu adının üzerine gelerek **Kopyalamak için tıklayın** seçeneğini ortaya çıkarabilirsiniz. 

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Azure SQL Veritabanı sunucunuzun oturum açma bilgilerini unuttuysanız, SQL Veritabanı sunucu sayfasına giderek sunucu yöneticisi adını görüntüleyin ve gerekirse parolayı sıfırlayın.

5. **Veritabanı bağlantı dizelerini göster**’e tıklayın.

6. Tam **ADO.NET** bağlantı dizesiniz gözden geçirin.

    ![ADO.NET bağlantı dizesi](./media/sql-database-connect-query-dotnet/adonet-connection-string.png)
  
## <a name="add-systemdatasqlclient"></a>System.Data.SqlClient ekleme
.NET core kullanırken, projenizin ***csproj*** dosyasına bağımlılık olarak System.Data.SqlClient ekleyin.

```xml
<ItemGroup>
    <PackageReference Include="System.Data.SqlClient" Version="4.3.0" />
</ItemGroup>
```

## <a name="select-data"></a>Verileri seçme

1. Geliştirme ortamınızda boş bir kod dosyası açın.
2. Kod dosyanıza ([System.Data.SqlClient namespace](https://msdn.microsoft.com/library/system.data.sqlclient.aspx)) ```using System.Data.SqlClient``` kodunu ekleyin. 

3. [SqlCommand.ExecuteReader](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.executereader.aspx) komutunu [SELECT](https://msdn.microsoft.com/library/ms189499.aspx) Transact-SQL deyimiyle kullanarak ilk 20 ürünü kategoriye göre sorgulamak için aşağıdaki kodu kullanın. Sunucunuz, veritabanınız, kullanıcınız ve parolanız için uygun değerleri ekleyin.

```csharp
using System;
using System.Data;
using System.Data.SqlClient;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            try 
            { 
                SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder();
                builder.DataSource = "your_server.database.windows.net"; 
                builder.UserID = "your_user";            
                builder.Password = "your_password";     
                builder.InitialCatalog = "your_database";

                using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
                {
                    Console.WriteLine("\nQuery data example:");
                    Console.WriteLine("=========================================\n");
                    
                    connection.Open();       
                    StringBuilder sb = new StringBuilder();
                    sb.Append("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName ");
                    sb.Append("FROM [SalesLT].[ProductCategory] pc ");
                    sb.Append("JOIN [SalesLT].[Product] p ");
                    sb.Append("ON pc.productcategoryid = p.productcategoryid;");
                    String sql = sb.ToString();

                    using (SqlCommand command = new SqlCommand(sql, connection))
                    {
                        using (SqlDataReader reader = command.ExecuteReader())
                        {
                            while (reader.Read())
                            {
                                Console.WriteLine("{0} {1}", reader.GetString(0), reader.GetString(1));
                            }
                        }
                    }                    
                }
            }
            catch (SqlException e)
            {
                Console.WriteLine(e.ToString());
            }
        }
    }
}
```

## <a name="insert-data"></a>Veri ekleme

[SqlCommand.ExecuteNonQuery](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.executenonquery.aspx) komutunu [INSERT](https://msdn.microsoft.com/library/ms174335.aspx) Transact-SQL deyimiyle kullanarak SalesLT.Product tablosuna yeni ürün eklemek için aşağıdaki kodu kullanın. Sunucunuz, veritabanınız, kullanıcınız ve parolanız için uygun değerleri ekleyin.

```csharp
using System;
using System.Data;
using System.Data.SqlClient;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            try 
            { 
                SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder();
                builder.DataSource = "your_server.database.windows.net"; 
                builder.UserID = "your_user";            
                builder.Password = "your_password";     
                builder.InitialCatalog = "your_database";

                using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
                {
                    Console.WriteLine("\nInsert data example:");
                    Console.WriteLine("=========================================\n");

                    connection.Open();       
                    StringBuilder sb = new StringBuilder();
                    sb.Append("INSERT INTO [SalesLT].[Product] ([Name], [ProductNumber], [Color], [StandardCost], [ListPrice], [SellStartDate]) ");
                    sb.Append("VALUES (@Name, @ProductNumber, @Color, @StandardCost, @ListPrice, @SellStartDate);");
                    String sql = sb.ToString();
                    using (SqlCommand command = new SqlCommand(sql, connection))
                    {
                        command.Parameters.AddWithValue("@Name", "BrandNewProduct");
                        command.Parameters.AddWithValue("@ProductNumber", "200989");
                        command.Parameters.AddWithValue("@Color", "Blue");
                        command.Parameters.AddWithValue("@StandardCost", 75);
                        command.Parameters.AddWithValue("@ListPrice", 80);
                        command.Parameters.AddWithValue("@SellStartDate", "7/1/2016");
                        int rowsAffected = command.ExecuteNonQuery();
                        Console.WriteLine(rowsAffected + " row(s) inserted");
                    }         
                }
            }
            catch (SqlException e)
            {
                Console.WriteLine(e.ToString());
            }
        }
    }
}
```

## <a name="update-data"></a>Verileri güncelleştirme

[SqlCommand.ExecuteNonQuery](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.executenonquery.aspx) komutunu [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx) Transact-SQL deyimiyle kullanarak daha önce eklemiş olduğunuz yeni ürünü güncelleştirmek için aşağıdaki kodu kullanın. Sunucunuz, veritabanınız, kullanıcınız ve parolanız için uygun değerleri ekleyin.

```csharp
using System;
using System.Data;
using System.Data.SqlClient;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            try 
            { 
                SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder();
                builder.DataSource = "your_server.database.windows.net"; 
                builder.UserID = "your_user";            
                builder.Password = "your_password";     
                builder.InitialCatalog = "your_database";

                using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
                {
                    Console.WriteLine("\nUpdate data example");
                    Console.WriteLine("=========================================\n");

                    connection.Open();       
                    StringBuilder sb = new StringBuilder();
                    sb.Append("UPDATE [SalesLT].[Product] SET ListPrice = @ListPrice WHERE Name = @Name;");
                    String sql = sb.ToString();
                    using (SqlCommand command = new SqlCommand(sql, connection))
                    {
                        command.Parameters.AddWithValue("@Name", "BrandNewProduct");
                        command.Parameters.AddWithValue("@ListPrice", 500);
                        int rowsAffected = command.ExecuteNonQuery();
                        Console.WriteLine(rowsAffected + " row(s) updated");
                    }         
                }
            }
            catch (SqlException e)
            {
                Console.WriteLine(e.ToString());
            }
        }
    }
}
```

## <a name="delete-data"></a>Verileri silme

[SqlCommand.ExecuteNonQuery](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.executenonquery.aspx) komutunu [DELETE](https://msdn.microsoft.com/library/ms189835.aspx) Transact-SQL deyimiyle kullanarak daha önce eklemiş olduğunuz yeni ürünü silmek için aşağıdaki kodu kullanın. Sunucunuz, veritabanınız, kullanıcınız ve parolanız için uygun değerleri ekleyin.

```csharp
using System;
using System.Data;
using System.Data.SqlClient;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            try 
            { 
                SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder();
                builder.DataSource = "your_server.database.windows.net"; 
                builder.UserID = "your_user";            
                builder.Password = "your_password";     
                builder.InitialCatalog = "your_database";

                using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
                {
                    Console.WriteLine("\nDelete data example");
                    Console.WriteLine("=========================================\n");

                    connection.Open();       
                    StringBuilder sb = new StringBuilder();
                    sb.Append("DELETE FROM SalesLT.Product WHERE Name = @Name;");
                    String sql = sb.ToString();
                    using (SqlCommand command = new SqlCommand(sql, connection))
                    {
                        command.Parameters.AddWithValue("@Name", "BrandNewProduct");
                        int rowsAffected = command.ExecuteNonQuery();
                        Console.WriteLine(rowsAffected + " row(s) deleted");
                    }         
                }
            }
            catch (SqlException e)
            {
                Console.WriteLine(e.ToString());
            }
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

- .NET belgeleri için bkz. [.NET belgeleri](https://docs.microsoft.com/dotnet/).
- SQL Server Management Studio kullanarak bağlanmak ve sorgu yürütmek için, bkz. [SSMS ile bağlanma ve sorgu yürütme](sql-database-connect-query-ssms.md)
- Visual Studio’yu kullanarak bağlanmak ve sorgulamak için bkz. [Visual Studio Code ile bağlanma ve sorgulama](sql-database-connect-query-vscode.md).
- PHP kullanarak bağlanıp sorgulamak için bkz. [PHP ile bağlanma ve sorgulama](sql-database-connect-query-php.md).
- Node.js kullanarak bağlanıp sorgulamak için bkz. [Node.js ile bağlanma ve sorgulama](sql-database-connect-query-nodejs.md).
- Java kullanarak bağlanıp sorgulamak için bkz. [Java ile bağlanma ve sorgulama](sql-database-connect-query-java.md).
- Python kullanarak bağlanıp sorgulamak için bkz. [Python ile bağlanma ve sorgulama](sql-database-connect-query-python.md).
- Ruby kullanarak bağlanıp sorgulamak için bkz. [Ruby ile bağlanma ve sorgulama](sql-database-connect-query-ruby.md).

