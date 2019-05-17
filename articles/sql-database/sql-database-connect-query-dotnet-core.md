---
title: Azure SQL Veritabanı sorgulamak için .NET Core kullanma | Microsoft Docs
description: Bu konuda, .NET Core kullanarak Azure SQL veritabanına bağlanan ve Transact-SQL deyimlerini kullanarak sorgulayan bir program oluşturmak için nasıl kullanılacağını gösterir.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: dotnet
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 03/25/2019
ms.openlocfilehash: 8ca2346de84a97bff370a7d5bacb006130cb5116
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65792466"
---
# <a name="quickstart-use-net-core-c-to-query-an-azure-sql-database"></a>Hızlı Başlangıç: .NET Core (C#) kullanarak Azure SQL veritabanı sorgulama

Bu hızlı başlangıçta kullanacaksınız [.NET Core](https://www.microsoft.com/net/) ve C# bir Azure SQL veritabanına bağlanmak için kod. Ardından, verileri sorgulamak Transact-SQL deyimini çalıştıracaksınız.

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici için şunlar gerekir:

- Bir Azure SQL veritabanı. Şu hızlı başlangıçlardan biriyle oluşturmak ve ardından bir veritabanını Azure SQL veritabanı'nda yapılandırmak için kullanabilirsiniz:

  || Tek veritabanı | Yönetilen örnek |
  |:--- |:--- |:---|
  | Oluştur| [Portal](sql-database-single-database-get-started.md) | [Portal](sql-database-managed-instance-get-started.md) |
  || [CLI](scripts/sql-database-create-and-configure-database-cli.md) | [CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) |
  || [PowerShell](scripts/sql-database-create-and-configure-database-powershell.md) | [PowerShell](scripts/sql-database-create-configure-managed-instance-powershell.md) |
  | Yapılandır | [sunucu düzeyinde IP güvenlik duvarı kuralı](sql-database-server-level-firewall-rule.md)| [Bir VM bağlantısı](sql-database-managed-instance-configure-vm.md)|
  |||[Şirket içi bağlantısı](sql-database-managed-instance-configure-p2s.md)
  |Verileri yükleyin|Adventure Works hızlı başlangıç yüklendi|[Wide World Importers geri yükleme](sql-database-managed-instance-get-started-restore.md)
  |||Geri yükleme ya da Adventure Works'den içe [BACPAC](sql-database-import.md) dosya [GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works)|
  |||

  > [!IMPORTANT]
  > Komut bu makalede, Adventure Works veritabanı kullanmak için yazılır. Yönetilen örnek sayesinde, Adventure Works veritabanı bir örneği veritabanına aktarmak veya betiklerde Wide World Importers veritabanını kullanmak için bu makaleyi değiştirin.

- [İşletim sisteminiz için .NET core](https://www.microsoft.com/net/core) yüklü.

> [!NOTE]
> Bu hızlı başlangıçta kullanılmaktadır *mySampleDatabase* veritabanı. Farklı bir veritabanı kullanmak istiyorsanız, veritabanı başvuruları değiştirin ve değiştirmek ihtiyacınız olacak `SELECT` içinde sorgu C# kod.

## <a name="get-sql-server-connection-information"></a>SQL server bağlantı bilgilerini alma

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Yaklaşan yordamlar için tam sunucu adını veya ana bilgisayar adı, veritabanı adını ve oturum açma bilgileri gerekir.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. Gidin **SQL veritabanları** veya **SQL yönetilen örnekler** sayfası.

3. Üzerinde **genel bakış** sayfasında, tam sunucu adını gözden **sunucu adı** tek bir veritabanı veya tam sunucu adı yanındaki **konak** yönetilen bir örneği. Sunucu adı veya ana bilgisayar adı kopyalamak için üzerine gelin ve seçin **kopyalama** simgesi.

## <a name="get-adonet-connection-information-optional"></a>(İsteğe bağlı) ADO.NET bağlantı bilgilerini alma

1. Gidin **mySampleDatabase** sayfayı ve altındaki **ayarları**seçin **bağlantı dizeleri**.

2. Tam **ADO.NET** bağlantı dizesini gözden geçirin.

    ![ADO.NET bağlantı dizesi](./media/sql-database-connect-query-dotnet/adonet-connection-string2.png)

3. Kopyalama **ADO.NET** kullanmak istiyorsanız, bağlantı dizesi.
  
## <a name="create-a-new-net-core-project"></a>Yeni bir .NET Core projesi oluşturma

1. Komut istemini açın ve **sqltest** adlı bir klasör oluşturun. Bu klasöre gidin ve şu komutu çalıştırın.

    ```cmd
    dotnet new console
    ```
    Bu komut, proje dosyaları, bir ilk dahil olmak üzere yeni bir uygulama oluşturur C# kod dosyası (**Program.cs**), bir XML yapılandırma dosyasını (**sqltest.csproj**) ve ikili dosyaları gerekli.

2. Bir metin düzenleyicisinde açın **sqltest.csproj** arasında aşağıdaki XML Yapıştır `<Project>` etiketler. Bu XML ekler `System.Data.SqlClient` bağımlılık olarak.

    ```xml
    <ItemGroup>
        <PackageReference Include="System.Data.SqlClient" Version="4.6.0" />
    </ItemGroup>
    ```

## <a name="insert-code-to-query-sql-database"></a>SQL veritabanını sorgulamak için kod girme

1. Bir metin düzenleyicisinde açın **Program.cs**.

2. İçeriğini aşağıdaki kodla değiştirin ve sunucu, veritabanı, kullanıcı adı ve parola için uygun değerleri ekleyin.

> [!NOTE]
> Bir ADO.NET bağlantı dizesini kullanmak için sunucu, veritabanı, kullanıcı adı ve parola ile aşağıdaki satırı ayarı kod 4 satırları değiştirin. Dize, kullanıcı adı ve parola ayarlayın.
>
>    `builder.ConnectionString="<your_ado_net_connection_string>";`

```csharp
using System;
using System.Data.SqlClient;
using System.Text;

namespace sqltest
{
    class Program
    {
        static void Main(string[] args)
        {
            try 
            { 
                SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder();

                builder.DataSource = "<your_server.database.windows.net>"; 
                builder.UserID = "<your_username>";            
                builder.Password = "<your_password>";     
                builder.InitialCatalog = "<your_database>";
         
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
            Console.WriteLine("\nDone. Press enter.");
            Console.ReadLine(); 
        }
    }
}
```

## <a name="run-the-code"></a>Kodu çalıştırma

1. İsteminde aşağıdaki komutları çalıştırın.

   ```cmd
   dotnet restore
   dotnet run
   ```

2. En üst 20 satırın döndürüldüğünü doğrulayın.

   ```text
   Query data example:
   =========================================

   Road Frames HL Road Frame - Black, 58
   Road Frames HL Road Frame - Red, 58
   Helmets Sport-100 Helmet, Red
   Helmets Sport-100 Helmet, Black
   Socks Mountain Bike Socks, M
   Socks Mountain Bike Socks, L
   Helmets Sport-100 Helmet, Blue
   Caps AWC Logo Cap
   Jerseys Long-Sleeve Logo Jersey, S
   Jerseys Long-Sleeve Logo Jersey, M
   Jerseys Long-Sleeve Logo Jersey, L
   Jerseys Long-Sleeve Logo Jersey, XL
   Road Frames HL Road Frame - Red, 62
   Road Frames HL Road Frame - Red, 44
   Road Frames HL Road Frame - Red, 48
   Road Frames HL Road Frame - Red, 52
   Road Frames HL Road Frame - Red, 56
   Road Frames LL Road Frame - Black, 58
   Road Frames LL Road Frame - Black, 60
   Road Frames LL Road Frame - Black, 62

   Done. Press enter.
   ```
3. Seçin **Enter** uygulama penceresini kapatın.

## <a name="next-steps"></a>Sonraki adımlar

- [Komut satırını kullanarak Windows/Linus/macOS’ta .NET Core ile çalışmaya başlama](/dotnet/core/tutorials/using-with-xplat-cli).
- Bilgi edinmek için nasıl [bağlanmak ve .NET Framework ve Visual Studio kullanarak Azure SQL veritabanını sorgulamak](sql-database-connect-query-dotnet-visual-studio.md).  
- Bilgi nasıl [SSMS kullanarak ilk Azure SQL veritabanınızı tasarlamayı](sql-database-design-first-database.md) veya [ile bağlanma ve bir Azure SQL veritabanı tasarlama C# ve ADO.NET](sql-database-design-first-database-csharp.md).
- .NET hakkında daha fazla bilgi edinmek için [.NET belgelerine](https://docs.microsoft.com/dotnet/) bakın.
