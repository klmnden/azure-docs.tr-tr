---
title: Visual Studio .NET ile kullanma ve C# Azure SQL veritabanı sorgulamak için | Microsoft Docs
description: Visual Studio oluşturulacağı bir C# bir Azure SQL veritabanına bağlanan ve Transact-SQL deyimleriyle veri sorgulayan bir uygulama.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: dotnet
ms.topic: quickstart
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 03/25/2019
ms.openlocfilehash: 7c444235ed7aa641fe218b4d1667e661ff05d77e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61409753"
---
# <a name="quickstart-use-net-and-c-in-visual-studio-to-connect-to-and-query-an-azure-sql-database"></a>Hızlı Başlangıç: .NET kullanın ve C# bağlanmak ve bir Azure SQL veritabanını sorgulamak için Visual Studio

Bu hızlı başlangıçta nasıl kullanılacağını gösterir [.NET framework](https://www.microsoft.com/net/) ve C# Transact-SQL deyimleri ile bir Azure SQL veritabanı sorgulamak için Visual Studio'da kodu.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için şunlar gerekir:

- Bir Azure SQL veritabanı. Şu hızlı başlangıçlardan biriyle oluşturmak ve ardından bir veritabanını Azure SQL veritabanı'nda yapılandırmak için kullanabilirsiniz:

  || Tek veritabanı | Yönetilen örnek |
  |:--- |:--- |:---|
  | Oluştur| [Portal](sql-database-single-database-get-started.md) | [Portal](sql-database-managed-instance-get-started.md) |
  || [CLI](scripts/sql-database-create-and-configure-database-cli.md) | [CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) |
  || [PowerShell](scripts/sql-database-create-and-configure-database-powershell.md) | [PowerShell](scripts/sql-database-create-configure-managed-instance-powershell.md) |
  | Yapılandırma | [sunucu düzeyinde IP güvenlik duvarı kuralı](sql-database-server-level-firewall-rule.md)| [Bir VM bağlantısı](sql-database-managed-instance-configure-vm.md)|
  |||[Şirket içi bağlantısı](sql-database-managed-instance-configure-p2s.md)
  |Veri yükleme|Adventure Works hızlı başlangıç yüklendi|[Wide World Importers geri yükleme](sql-database-managed-instance-get-started-restore.md)
  |||Geri yükleme ya da Adventure Works'den içe [BACPAC](sql-database-import.md) dosya [GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works)|
  |||

  > [!IMPORTANT]
  > Komut bu makalede, Adventure Works veritabanı kullanmak için yazılır. Yönetilen örnek sayesinde, Adventure Works veritabanı bir örneği veritabanına aktarmak veya betiklerde Wide World Importers veritabanını kullanmak için bu makaleyi değiştirin.

- [Visual Studio 2017](https://www.visualstudio.com/downloads/) Community, Professional veya Enterprise edition.

## <a name="get-sql-server-connection-information"></a>SQL server bağlantı bilgilerini alma

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Yaklaşan yordamlar için tam sunucu adını veya ana bilgisayar adı, veritabanı adını ve oturum açma bilgileri gerekir.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. Gidin **SQL veritabanları** veya **SQL yönetilen örnekler** sayfası.

3. Üzerinde **genel bakış** sayfasında, tam sunucu adını gözden **sunucu adı** tek bir veritabanı veya tam sunucu adı yanındaki **konak** yönetilen bir örneği. Sunucu adı veya ana bilgisayar adı kopyalamak için üzerine gelin ve seçin **kopyalama** simgesi. 

## <a name="create-code-to-query-the-sql-database"></a>SQL veritabanı sorgulamak için kod oluşturun

1. Visual Studio'da **dosya** > **yeni** > **proje**. 
   
1. İçinde **yeni proje** iletişim kutusunda **Visual C#** ve ardından **konsol uygulaması (.NET Framework)**.
   
1. Girin *sqltest* proje adı ve ardından **Tamam**. Yeni Proje oluşturulur. 
   
1. Seçin **proje** > **NuGet paketlerini Yönet**. 
   
1. İçinde **NuGet Paket Yöneticisi**seçin **Gözat** sekmesine, ardından aramak ve seçmek **System.Data.SqlClient**.
   
1. Üzerinde **System.Data.SqlClient** sayfasında **yükleme**. 
   - İstenirse, seçin **Tamam** yüklemeye devam etmek için. 
   - Varsa bir **lisans kabulü** penceresi görüntülenirse, seçin **kabul ediyorum**.
   
1. Yükleme tamamlandığında, kapatabilirsiniz **NuGet Paket Yöneticisi**. 
   
1. Kod Düzenleyicisi'nde değiştirin **Program.cs** içeriğini aşağıdaki kodla. Kendi değerlerinizi yerleştirin `<server>`, `<username>`, `<password>`, ve `<database>`.
   
   >[!IMPORTANT]
   >Bu örnekteki kod kaynağı olarak veritabanınızı oluştururken seçebilirsiniz AdventureWorksLT örnek verileri kullanır. Farklı veri veritabanınız varsa kendi veritabanından tablolar SELECT sorgusunda kullanın. 
   
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
                   builder.DataSource = "<server>.database.windows.net"; 
                   builder.UserID = "<username>";            
                   builder.Password = "<password>";     
                   builder.InitialCatalog = "<database>";
   
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

1. Uygulamayı çalıştırmak için seçin **hata ayıklama** > **hata ayıklamayı Başlat**, ya da seçin **Başlat** basın veya araç **F5**.
1. İlk 20 kategori/ürün satırları veritabanınızdan döndürülür ve sonra uygulama penceresini kapatın doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [bağlanmak ve .NET Core kullanarak Azure SQL veritabanını sorgulamak](sql-database-connect-query-dotnet-core.md) Windows/Linus/macos'ta.  
- [Komut satırını kullanarak Windows/Linus/macOS’ta .NET Core ile çalışmaya başlama](/dotnet/core/tutorials/using-with-xplat-cli) hakkında bilgi edinin.
- [SSMS kullanarak ilk Azure SQL veritabanınızı tasarlamayı](sql-database-design-first-database.md) veya [.NET kullanarak ilk Azure SQL veritabanınızı tasarlamayı](sql-database-design-first-database-csharp.md) öğrenin.
- .NET hakkında daha fazla bilgi edinmek için [.NET belgelerine](https://docs.microsoft.com/dotnet/) bakın.
- Yeniden deneme mantığı örneği: [Dayanıklı ADO.NET ile SQL bağlantısı kurma][step-4-connect-resiliently-to-sql-with-ado-net-a78n].


<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-ado-net-a78n]: https://docs.microsoft.com/sql/connect/ado-net/step-4-connect-resiliently-to-sql-with-ado-net

