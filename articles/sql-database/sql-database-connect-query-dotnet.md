---
title: ".NET (C#) kullanarak Azure SQL Veritabanına bağlanma | Microsoft Docs"
description: "Azure SQL Veritabanında C# dilinde ve bulutta güçlü bir ilişkisel veritabanı ile desteklenen modern bir uygulama derlemek için bu hızlı başlangıçtaki örnek kodu kullanın."
services: sql-database
documentationcenter: 
author: ajlam
manager: jhubbard
editor: 
ms.assetid: 7faca033-24b4-4f64-9301-b4de41e73dfd
ms.service: sql-database
ms.custom: quick start
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 03/28/2017
ms.author: andrela;sstein;carlrab
translationtype: Human Translation
ms.sourcegitcommit: 432752c895fca3721e78fb6eb17b5a3e5c4ca495
ms.openlocfilehash: c6c0c218b8d0456d37a4514238675fd8e75faf9d
ms.lasthandoff: 03/30/2017


---
# <a name="azure-sql-database-use-net-c-to-connect-and-query-data"></a>Azure SQL Veritabanı: .NET (C#) kullanarak verileri bağlama ve sorgulama

Bir Azure SQL veritabanına bağlanmak ve veritabanını sorgulamak için [C# ve ADO.NET](https://msdn.microsoft.com/library/kb9s9ks0.aspx) kullanın. Bu kılavuzda bir Azure SQL veritabanına bağlanmak ve ardından query, insert, update ve delete deyimlerini yürütmek üzere #C kullanmayla ilgili ayrıntılar verilmektedir.

Bu hızlı başlangıçta başlangıç noktası olarak bu hızlı başlangıçlardan birinde oluşturulan kaynaklar kullanılır:

- [DB Oluşturma - Portal](sql-database-get-started-portal.md)
- [DB oluşturma - CLI](sql-database-get-started-cli.md)

## <a name="configure-development-environment"></a>Geliştirme ortamını yapılandırma

Aşağıdaki bölümler mevcut Mac OS, Linux(Ubuntu) ve Windows geliştirme ortamlarınızı Azure SQL Veritabanı ile çalışacak şekilde yapılandırmaya ilişkin ayrıntıları vermektedir.

### <a name="mac-os"></a>**Mac OS**
Terminalinizi açın ve .NET Core projenizi oluşturmayı planladığınız bir dizine gidin. **brew**, **OpenSSL** ve **.NET Core** yüklemek için aşağıdaki komutları girin. 

```C#
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

```C#
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
sudo apt-get update
sudo apt-get install dotnet-dev-1.0.1
```

### <a name="windows"></a>**Windows**
Visual Studio 2015 Community Edition ve .NET Framework yükleyin. Makinenizde Visual Studio zaten yüklüyse, sonraki birkaç adımı atlayın.

Visual Studio 2015 Community; Android, iOS, Windows’un yanı sıra web ile veritabanı uygulamaları ve bulut hizmetleri için modern uygulamalar oluşturmaya yönelik tam özellikli, genişletilebilir, ücretsiz bir IDE’dir.

1. [Yükleyiciyi](https://go.microsoft.com/fwlink/?LinkId=691978) indirin. 
2. Yükleyiciyi çalıştırın ve yükleme istemlerini izleyerek yüklemeyi tamamlayın.

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma

Azure portaldan bağlantı dizesini alın. Azure SQL veritabanına bağlanmak için bağlantı dizesini kullanabilirsiniz.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Soldaki menüden **SQL Veritabanları**’nı seçin ve **SQL veritabanları** sayfasında veritabanınıza tıklayın. 
3. Veritabanınızın **Temel Bilgiler** bölmesinde, tam sunucu adını gözden geçirin. 

    <img src="./media/sql-database-connect-query-dotnet/connection-strings.png" alt="connection strings" style="width: 780px;" />

4. **Veritabanı bağlantı dizelerini göster**’e tıklayın.

5. Tam **ADO.NET** bağlantı dizesiniz gözden geçirin.

    <img src="./media/sql-database-connect-query-dotnet/adonet-connection-string.png" alt="ADO.NET connection string" style="width: 780px;" />
    
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

3. Azure SQL veritabanınızdaki verileri sorgulamak için bir [SELECT](https://msdn.microsoft.com/library/ms189499.aspx) Transact-SQL deyimiyle birlikte [SqlCommand.ExecuteReader](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.executereader.aspx) komutunu kullanın. Sunucunuz için uygun değerler ekleme

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

Azure SQL veritabanınıza veri eklemek için bir [INSERT](https://msdn.microsoft.com/library/ms174335.aspx) Transact-SQL deyimiyle birlikte [SqlCommand.ExecuteNonQuery](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.executenonquery.aspx) komutunu kullanın.

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

Azure SQL veritabanınızdaki verileri güncelleştirmek için bir [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx) Transact-SQL deyimiyle birlikte [SqlCommand.ExecuteNonQuery](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.executenonquery.aspx) komutunu kullanın.

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

Azure SQL veritabanınızdaki verileri silmek için bir [DELETE](https://msdn.microsoft.com/library/ms189835.aspx) Transact-SQL deyimiyle birlikte [SqlCommand.ExecuteNonQuery](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.executenonquery.aspx) komutunu kullanın.

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
- Visual Studio Code ile veri sorgulama ve düzenleme hakkında bilgi için bkz. [Visual Studio Code](https://code.visualstudio.com/docs).

