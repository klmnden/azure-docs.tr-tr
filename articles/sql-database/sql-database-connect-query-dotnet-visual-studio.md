---
title: Azure SQL Veritabanı’nı sorgulamak için Visual Studio ve .NET kullanma | Microsoft Docs
description: Bu konu başlığı altında, Visual Studio kullanarak Azure SQL Veritabanına bağlanan ve Transact-SQL deyimleriyle veritabanını sorgulayan bir program oluşturma işlemi gösterilir.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: devcenter
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: d785436f157fb694ccf4447db91f28b25c9f2a3a
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38704708"
---
# <a name="use-net-c-with-visual-studio-to-connect-and-query-an-azure-sql-database"></a>Visual Studio ile .NET (C#) kullanarak Azure SQL veritabanına bağlanma ve veritabanını sorgulama

Bu hızlı başlangıçta, [.NET framework](https://www.microsoft.com/net/) kullanarak Visual Studio ile Azure SQL veritabanına bağlanan ve Transact-SQL deyimleriyle veri sorgulayan bir C# programı oluşturma işleminin nasıl yapılacağı açıklanır.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için aşağıdakilere sahip olduğunuzdan emin olun:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- Bu hızlı başlangıçta kullanacağınız bilgisayarın genel IP adresi için [sunucu düzeyinde bir güvenlik duvarı kuralı](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).

- [Visual Studio Community 2017, Visual Studio Professional 2017 veya Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/) yüklemesi.

## <a name="sql-server-connection-information"></a>SQL Server bağlantı bilgileri

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

#### <a name="for-adonet"></a>ADO.NET için

1. **Veritabanı bağlantı dizelerini göster**’e tıklayarak devam edin.

2. Tam **ADO.NET** bağlantı dizesini gözden geçirin.

    ![ADO.NET bağlantı dizesi](./media/sql-database-connect-query-dotnet/adonet-connection-string.png)

> [!IMPORTANT]
> Bu hızlı başlangıç öğreticisinde kullanacağınız bilgisayarın genel IP adresi için bir güvenlik duvarı kuralınız olmalıdır. Farklı bir bilgisayar kullanıyorsanız veya farklı bir genel IP adresiniz varsa [Azure portal kullanarak bir sunucu düzeyi güvenlik duvarı kuralı](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) oluşturun. 
>
  
## <a name="create-a-new-visual-studio-project"></a>Yeni Visual Studio projesi oluşturma

1. Visual Studio'da **Dosya**, **Yeni**, **Proje**’yi seçin. 
2. **Yeni Proje** iletişim kutusunda **Visual C#** öğesini genişletin.
3. **Konsol Uygulaması**’nı seçin ve projenin adı için *sqltest* girin.
4. Visual Studio’da yeni projeyi oluşturmak ve açmak için **Tamam**’a tıklayın.
4. Çözüm Gezgini'nde, **sqltest**'e sağ tıklayın ve **NuGet Paketlerini Yönet**'e tıklayın. 
5. **Gözat**’ta ```System.Data.SqlClient``` öğesini arayın ve bulduğunuzda seçin.
6. **System.Data.SqlClient** sayfasında **Yükle**’ye tıklayın.
7. Yükleme tamamlandığında değişiklikleri gözden geçirin ve **Önizleme** penceresini kapamak için **Tamam**’a tıklayın. 
8. **Lisans Kabulü** penceresi gösterilirse **Kabul Ediyorum**’a tıklayın.

## <a name="insert-code-to-query-sql-database"></a>SQL veritabanını sorgulamak için kod girme
1. **Program.cs**’ye geçin (veya gerekiyorsa açın)

2. **Program.cs** dosyasının içeriğini aşağıdaki kodla değiştirin ve sunucunuz, veritabanınız, kullanıcınız ve parolanız için uygun değerleri ekleyin.

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

1. Uygulamayı çalıştırmak için **F5**'e basın.
2. En üst 20 satırın döndürüldüğünü doğrulayın ve sonra uygulama penceresini kapatın.

## <a name="next-steps"></a>Sonraki adımlar

- Windows/Linus/macOS’ta [.NET Core kullanarak Azure SQL veritabanını bağlamayı ve sorgulamayı](sql-database-connect-query-dotnet-core.md) öğrenin.  
- [Komut satırını kullanarak Windows/Linus/macOS’ta .NET Core ile çalışmaya başlama](/dotnet/core/tutorials/using-with-xplat-cli) hakkında bilgi edinin.
- [SSMS kullanarak ilk Azure SQL veritabanınızı tasarlamayı](sql-database-design-first-database.md) veya [.NET kullanarak ilk Azure SQL veritabanınızı tasarlamayı](sql-database-design-first-database-csharp.md) öğrenin.
- .NET hakkında daha fazla bilgi edinmek için [.NET belgelerine](https://docs.microsoft.com/dotnet/) bakın.
- [Yeniden deneme mantığı örneği: ADO.NET ile dayanıklı SQL bağlantısı kurma][step-4-connect-resiliently-to-sql-with-ado-net-a78n]


<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-ado-net-a78n]: https://docs.microsoft.com/sql/connect/ado-net/step-4-connect-resiliently-to-sql-with-ado-net

