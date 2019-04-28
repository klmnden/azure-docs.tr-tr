---
title: Azure SQL veritabanı sorgulamak için R kullanma
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Bu makalede bir R betiği bir Azure SQL veritabanına bağlanan ve Transact-SQL deyimlerini kullanarak sorgulamak için nasıl kullanılacağını gösterir.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: python
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.reviewer: davidph, carlrab
manager: cgronlun
ms.date: 04/11/2019
ms.openlocfilehash: 2b1206e3087b0573736174d4eed502653d76f7a5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61408971"
---
# <a name="quickstart-use-r-to-query-an-azure-sql-database-preview"></a>Hızlı Başlangıç: (Önizleme) Azure SQL veritabanını sorgulamaya R kullanma

 Bu hızlı başlangıçta nasıl kullanılacağını gösterir [R](https://www.r-project.org/) ile Machine Learning Services'ı kullanarak Azure SQL veritabanına bağlanan ve Transact-SQL deyimleriyle veri kullanın. Machine Learning Hizmetleri, Azure SQL veritabanı, veritabanı R betikleri yürütmek için kullanılan bir özelliktir. Daha fazla bilgi için bkz: [R (Önizleme) ile Azure SQL veritabanı Machine Learning Hizmetleri](sql-database-machine-learning-services-overview.md).

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için aşağıdakilere sahip olduğunuzdan emin olun:

- Bir Azure SQL veritabanı. Şu hızlı başlangıçlardan biriyle oluşturmak ve ardından bir veritabanını Azure SQL veritabanı'nda yapılandırmak için kullanabilirsiniz:

  || Tek veritabanı | Yönetilen örnek |
  |:--- |:--- |:---|
  | Oluştur| [Portal](sql-database-single-database-get-started.md) | [Portal](sql-database-managed-instance-get-started.md) |
  || [CLI](scripts/sql-database-create-and-configure-database-cli.md) | [CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) |
  || [PowerShell](scripts/sql-database-create-and-configure-database-powershell.md) | [PowerShell](scripts/sql-database-create-configure-managed-instance-powershell.md) |
  | Yapılandırma | [sunucu düzeyinde IP güvenlik duvarı kuralı](sql-database-server-level-firewall-rule.md)| [Bir VM bağlantısı](sql-database-managed-instance-configure-vm.md)|
  |||[Şirket içi bağlantısı](sql-database-managed-instance-configure-p2s.md)
  |||

- Etkin olan Machine Learning Hizmetleri (R ile). Genel Önizleme sırasında Microsoft tarafından ekleyin ve machine learning, mevcut veya yeni bir veritabanı için etkinleştirin. Bağlantısındaki [Önizleme için kaydolun](sql-database-machine-learning-services-overview.md#signup).

- En son [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS). Diğer veritabanı yönetim veya sorgu Araçları'nı kullanarak R betikleri çalıştırabilir ancak bu hızlı başlangıçta SSMS kullanacaksınız.

## <a name="get-sql-server-connection-information"></a>SQL server bağlantı bilgilerini alma

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Yaklaşan yordamlar için tam sunucu adını veya ana bilgisayar adı, veritabanı adını ve oturum açma bilgileri gerekir.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. Gidin **SQL veritabanları** veya **SQL yönetilen örnekler** sayfası.

3. Üzerinde **genel bakış** sayfasında, tam sunucu adını gözden **sunucu adı** tek bir veritabanı veya tam sunucu adı yanındaki **konak** yönetilen bir örneği. Sunucu adı veya ana bilgisayar adı kopyalamak için üzerine gelin ve seçin **kopyalama** simgesi.

## <a name="create-code-to-query-your-sql-database"></a>SQL veritabanınızı sorgulamak için kod oluşturun

1. **SQL Server Management Studio**’yu açın ve SQL veritabanınıza bağlanın.

   Bağlanma yardıma ihtiyacınız varsa bkz [hızlı başlangıç: Bağlanmak ve bir Azure SQL veritabanı sorgulamak için SQL Server Management Studio'yu kullanın](sql-database-connect-query-ssms.md).

1. Tam R komut dosyasına geçirmek [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) saklı yordamı.

   Komut dosyası aracılığıyla geçirilir `@script` bağımsız değişken. İçindeki her şey `@script` bağımsız değişken, geçerli R kodu olmalıdır.

    ```sql
    EXECUTE sp_execute_external_script
    @language = N'R'
    , @script = N'OutputDataSet <- InputDataSet;'
    , @input_data_1 = N'SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid'
    ```

   > [!NOTE]
   > Hata alırsanız Machine Learning Services (R ile) genel önizleme sürümü SQL veritabanınız için etkinleştirilmemiş olabilir. Bkz: [önkoşulları](#prerequisites) yukarıda.

## <a name="run-the-code"></a>Kodu çalıştırma

1. Yürütme [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) saklı yordamı.

1. İlk 20 kategori/ürün satır içinde döndürüldüğünü doğrulayın **iletileri** penceresi.

## <a name="next-steps"></a>Sonraki adımlar

- [İlk Azure SQL veritabanınızı tasarlama](sql-database-design-first-database.md)
- [Azure SQL veritabanı Machine Learning Hizmetleri (R ile)](sql-database-machine-learning-services-overview.md)
- [Oluşturma ve Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) basit R betikleri çalıştırma](sql-database-quickstart-r-create-script.md)
- [Machine Learning Hizmetleri (Önizleme) kullanarak Azure SQL veritabanı içinde Gelişmiş R işlevler yazma](sql-database-machine-learning-services-functions.md)
