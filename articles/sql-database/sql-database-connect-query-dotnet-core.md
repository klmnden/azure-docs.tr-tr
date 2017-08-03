---
title: "Azure SQL Veritabanı sorgulamak için .NET Core kullanma | Microsoft Docs"
description: "Bu konu başlığı altında, .NET Core kullanarak Azure SQL Veritabanına bağlanan ve Transact-SQL deyimleriyle veritabanını sorgulayan bir program oluşturma işlemi gösterilir."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/05/2017
ms.author: carlrab
ms.translationtype: HT
ms.sourcegitcommit: 137671152878e6e1ee5ba398dd5267feefc435b7
ms.openlocfilehash: 046322624d3b89bb983acee863534256fee94b60
ms.contentlocale: tr-tr
ms.lasthandoff: 07/28/2017

---
# <a name="use-net-core-c-to-query-an-azure-sql-database"></a>.NET Core (C#) kullanarak Azure SQL veritabanı sorgulama

Bu hızlı başlangıç öğreticisi, Windows/Linux/macOS’ta [.NET Core](https://www.microsoft.com/net/) kullanarak Azure SQL veritabanına bağlanan ve Transact-SQL deyimleriyle veri sorgulayan bir C# programı oluşturmayı gösterir.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç öğreticisini tamamlamak için aşağıdakilere sahip olduğunuzdan emin olun:

- Bir Azure SQL veritabanı. Bu hızlı başlangıçta, aşağıdaki hızlı başlangıçlardan birinde oluşturulan kaynaklar kullanılır: 

   - [DB Oluşturma - Portal](sql-database-get-started-portal.md)
   - [DB oluşturma - CLI](sql-database-get-started-cli.md)
   - [DB Oluşturma - PowerShell](sql-database-get-started-powershell.md)

- Bu hızlı başlangıç öğreticisinde kullanacağınız bilgisayarın genel IP adresi için [sunucu düzeyinde bir güvenlik duvarı kuralı](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).
- [İşletim sisteminiz için .NET Core](https://www.microsoft.com/net/core) yüklediniz. 

## <a name="sql-server-connection-information"></a>SQL Server bağlantı bilgileri

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Sonraki yordamlarda tam sunucu adına, veritabanı adına ve oturum açma bilgilerine ihtiyacınız olacaktır.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Soldaki menüden **SQL Veritabanları**’nı seçin ve **SQL veritabanları** sayfasında veritabanınıza tıklayın. 
3. Veritabanınızın **Genel Bakış** sayfasında, aşağıdaki görüntüde gösterildiği gibi tam sunucu adını gözden geçirin. Sunucu adının üzerine gelerek **Kopyalamak için tıklayın** seçeneğini ortaya çıkarabilirsiniz. 

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Azure SQL Veritabanı sunucunuzun oturum açma bilgilerini unuttuysanız, SQL Veritabanı sunucu sayfasına giderek sunucu yöneticisi adını görüntüleyin. Gerekirse parolayı sıfırlayabilirsiniz.

5. **Veritabanı bağlantı dizelerini göster**’e tıklayın.

6. Tam **ADO.NET** bağlantı dizesini gözden geçirin.

    ![ADO.NET bağlantı dizesi](./media/sql-database-connect-query-dotnet/adonet-connection-string.png)

> [!IMPORTANT]
> Bu hızlı başlangıç öğreticisinde kullanacağınız bilgisayarın genel IP adresi için bir güvenlik duvarı kuralınız olmalıdır. Farklı bir bilgisayar kullanıyorsanız veya farklı bir genel IP adresiniz varsa [Azure portal kullanarak bir sunucu düzeyi güvenlik duvarı kuralı](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) oluşturun. 
>
  
## <a name="create-a-new-net-project"></a>Yeni bir .NET projesi oluşturma

1. Komut istemini açın ve *sqltest* adlı bir klasör oluşturun. Oluşturduğunuz klasöre gidin ve aşağıdaki komutu çalıştırın:

    ```
    dotnet new console
    ```

2. Sık kullandığınız metin düzenleyicisinde ***sqltest.csproj*** dosyasını açın ve aşağıdaki kodu kullanarak System.Data.SqlClient’ı bağımlılık olarak ekleyin:

    ```xml
    <ItemGroup>
        <PackageReference Include="System.Data.SqlClient" Version="4.3.0" />
    </ItemGroup>
    ```

## <a name="insert-code-to-query-sql-database"></a>SQL veritabanını sorgulamak için kod girme

1. Geliştirme ortamınızda veya sık kullandığınız metin düzenleyicisinde **Program.cs** dosyasını açın.

2. İçeriğini aşağıdaki kod ile değiştirin ve sunucunuz, veritabanınız, kullanıcınız ve parolanız için uygun değerleri ekleyin.

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
            Console.ReadLine();
        }
    }
}
```

## <a name="run-the-code"></a>Kodu çalıştırma

1. Komut isteminde aşağıdaki komutları çalıştırın:

   ```csharp
   dotnet restore
   dotnet run
   ```

2. En üst 20 satırın döndürüldüğünü doğrulayın ve sonra uygulama penceresini kapatın.


## <a name="next-steps"></a>Sonraki adımlar

- [Komut satırını kullanarak Windows/Linus/macOS’ta .NET Core ile çalışmaya başlama](/dotnet/core/tutorials/using-with-xplat-cli).
- [.NET framework ve Visual Studio kullanarak Azure SQL veritabanını bağlamayı ve sorgulamayı](sql-database-connect-query-dotnet-visual-studio.md) öğrenin.  
- [SSMS kullanarak ilk Azure SQL veritabanınızı tasarlamayı](sql-database-design-first-database.md) veya [.NET kullanarak ilk Azure SQL veritabanınızı tasarlamayı](sql-database-design-first-database-csharp.md) öğrenin.
- .NET hakkında daha fazla bilgi edinmek için [.NET belgelerine](https://docs.microsoft.com/dotnet/) bakın.

