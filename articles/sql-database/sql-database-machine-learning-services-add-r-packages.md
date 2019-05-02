---
title: Azure SQL veritabanı Machine Learning hizmetlerini (Önizleme) bir R paketi ekleme
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Bu makalede, Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) zaten yüklü bir R paketi yüklemek açıklanmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: r
ms.topic: tutorial
author: garyericson
ms.author: garye
ms.reviewer: davidph
manager: cgronlun
ms.date: 04/29/2019
ms.openlocfilehash: 4e7145570cbc906ea540c9d8f95f6c3cbde1c610
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64927103"
---
# <a name="add-an-r-package-to-azure-sql-database-machine-learning-services-preview"></a>Azure SQL veritabanı Machine Learning hizmetlerini (Önizleme) bir R paketi ekleme

Bu makalede, Azure SQL veritabanı Machine Learning hizmetlerini (Önizleme) bir R paketi ekleme açıklanmaktadır.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Önkoşullar

- Yükleme [R](https://www.r-project.org) ve [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/) yerel bilgisayarınızda. R; Windows, MacOS ve Linux platformlarında kullanılabilir. Bu makalede, Windows kullanmakta olduğunuz varsayılır.

- Bu makale, kullanmaya bir örnek içerir [Azure veri Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is) veya [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS) bir R çalıştırmak için komut dosyası, Azure SQL veritabanı'nda. Diğer veritabanı yönetim veya sorgu Araçları'nı kullanarak R betikleri çalıştırabilir ancak bu örnek, Azure veri Studio ya da SSMS varsayar.
   
> [!NOTE]
> Bir paketi kullanarak bir R betiği çalıştırarak yükleyemezsiniz **sp_execute_external_script** Azure veri Studio ya da SSMS. Yalnızca, yükleyin ve bu makalede açıklandığı gibi komut satırı R ve Rstudio'yu kullanarak paketleri kaldırın. Paket yüklendikten sonra paket işlevleri bir R betiği kullanarak erişebileceğiniz **sp_execute_external_script**.

## <a name="list-r-packages"></a>R paketlerinin listesi

Microsoft Machine Learning Services ile Azure SQL veritabanı'nda önceden yüklü R paketlerinin sayısını sağlar.
Azure veri Studio veya SSMS içinde aşağıdaki komutu çalıştırarak yüklü R paketlerinin bir listesini görebilirsiniz.

1. Azure veri Studio veya SSMS'yi açın ve Azure SQL veritabanınıza bağlanın.

1. Şu komutu çalıştırın:

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
OutputDataSet <- data.frame(installed.packages()[,c("Package", "Version", "Depends", "License")]);'
WITH RESULT SETS((
            Package NVARCHAR(255)
            , Version NVARCHAR(100)
            , Depends NVARCHAR(4000)
            , License NVARCHAR(1000)
            ));
```

Çıktı aşağıdakine benzer görünmelidir.

**Sonuçlar**

![R içindeki yüklü paketler](./media/sql-database-machine-learning-services-add-r-packages/r-installed-packages.png)

## <a name="add-a-package-with-sqlmlutils"></a>Paket sqlmlutils ile ekleme

Azure SQL veritabanı'nda zaten yüklü olmayan bir paket kullanmanız gerekiyorsa, bunu kullanarak yükleyebilirsiniz [sqlmlutils](https://github.com/Microsoft/sqlmlutils). **sqlmlutils** SQL veritabanları (SQL Server ve Azure SQL veritabanı) ile etkileşime geçmek ve R veya Python kodu bir R veya Python istemciden SQL yürütme kullanıcılara yardımcı olmak için tasarlanmış bir paket. Şu anda yalnızca R sürümü **sqlmlutils** Azure SQL veritabanı'nda desteklenir.

Aşağıdaki örnekte, yüklersiniz **[Birleştirici](https://cran.r-project.org/web/packages/glue/)** biçimlendirebilir ve dizeleri enterpolasyon paketi. Yükleme adımları **sqlmlutils** ve **RODBCext** (ön **sqlmlutils**) ve ekleme **Birleştirici** paket.

### <a name="install-sqlmlutils"></a>Yükleme **sqlmlutils**

1. En son sürümünü indirin **sqlmlutils** zip dosyasından https://github.com/Microsoft/sqlmlutils/tree/master/R/dist yerel bilgisayarınıza. Dosyanın sıkıştırmasını açın gerek yoktur.

1. Açık bir **komut istemi** yüklemek için aşağıdaki komutları çalıştırıp **RODBCext** ve **sqlmlutils** yerel bilgisayarınızda. Tam yol yerine **sqlmlutils** zip dosyasını, indirilen (örneğin dosyayı Belgeler klasörünüzde olduğunu varsayar).
    
    ```console
    R -e "install.packages('RODBCext', repos='https://cran.microsoft.com')"
    R CMD INSTALL %UserProfile%\Documents\sqlmlutils_0.5.1.zip
    ```

    Gördüğünüz çıktısı aşağıdakine benzer olmalıdır.

    ```text
    In R CMD INSTALL
    * installing to library 'C:/Users/<username>/Documents/R/win-library/3.5'
    package sqlmlutils successfully unpacked and MD5 sums checked
    ```

    > [!TIP]
    > Hata alırsanız, "'R' tanınmayan bir iç ya da dış komut, çalıştırılabilir program veya toplu iş dosyası", büyük olasılıkla R.exe yolu içinde bulunmaz geldiğini, **yolu** Windows ortam değişkeni. Yol ortam değişkenine ekleyin veya komut isteminde klasöre gidin (örneğin `cd C:\Program Files\R\R-3.5.3\bin`) ve sonra komutu tekrar deneyin.

### <a name="add-the-package"></a>Paket Ekle

1. RStudio açın ve yeni bir **R betiği** dosya. 

1. Yüklemek için aşağıdaki R kodu kullanan **Birleştirici** kullanarak paket **sqlmlutils**. Kendi Azure SQL veritabanı bağlantısı bilgilerini değiştirin.

    ```R
    library(sqlmlutils)
    connection <- connectionInfo(
    server= "yourserver.database.windows.net",
    database = "yourdatabase",
    uid = "yoursqluser",
    pwd = "yoursqlpassword")
    
    sql_install.packages(connectionString = connection, pkgs = "glue", verbose = TRUE, scope = "PUBLIC")
    ```

    > [!TIP]
    > **Kapsam** olabilir **genel** veya **özel**. Genel kapsam, veritabanı yöneticisinin tüm kullanıcıların kullanabileceği paketleri yüklemesi açısından yararlıdır. Özel kapsamı paket yalnızca onu yükleyen kullanıcı için kullanılabilir hale getirir. Kapsam değeri belirtmezseniz **PRIVATE** varsayılan kapsamı kullanılır.

### <a name="verify-the-package"></a>Paket doğrulayın

Doğrulayın **Birleştirici** RStudio içinde aşağıdaki R betiğini çalıştırarak paketi yüklendi. Aynı **bağlantı** önceki adımda tanımladığınız.

```R
r<-sql_installed.packages(connectionString = connection, fields=c("Package", "Version", "Depends", "License"))
View(r)
```

**Sonuçlar**

![RTestData tablosunun içeriği](./media/sql-database-machine-learning-services-add-r-packages/r-verify-package-install.png)

### <a name="use-the-package"></a>Paket kullanma

Paket yüklendikten sonra bunu bir R betiği aracılığıyla kullanabileceğiniz **sp_execute_external_script**.

1. Azure veri Studio veya SSMS'yi açın ve Azure SQL veritabanınıza bağlanın.

1. Şu komutu çalıştırın:

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    library(glue)
    
    name <- "Fred"
    age <- 50
    anniversary <- as.Date("2020-06-14")
    text <- glue(''My name is {name}, '',
    ''my age next year is {age + 1}, '',
    ''my anniversary is {format(anniversary, "%A, %B %d, %Y")}.'')
    
    print(text)
    ';
    ```

    Aşağıdaki sonucu göreceğiniz **iletileri** sekmesi.

    **Sonuçlar**

    ```text
    My name is Fred, my age next year is 51, my anniversary is Sunday, June 14, 2020.
    ```

### <a name="remove-the-package"></a>Paketi Kaldır

Paketini kaldırmak istiyorsanız, RStudio içinde aşağıdaki R betiği çalıştırın. Aynı **bağlantı** önceden tanımlanmış.

```R
sql_remove.packages(connectionString = connection, pkgs = "glue", scope = "PUBLIC")
```

> [!TIP]
> Bir bayt akışı kullanarak R paketini karşıya yüklemek için Azure SQL veritabanınıza bir R paketi yüklemek için başka bir yolu olan **CREATE EXTERNAL LIBRARY** T-SQL deyimi. Bkz: [öğesinden bir bayt akışı kitaplığı oluşturma](/sql/t-sql/statements/create-external-library-transact-sql#c-create-a-library-from-a-byte-stream) içinde [CREATE EXTERNAL LIBRARY](https://docs.microsoft.com/sql/t-sql/statements/create-external-library-transact-sql) başvuru belgeleri.

## <a name="next-steps"></a>Sonraki adımlar

R (Önizleme) ile Azure SQL veritabanı Machine Learning hizmetleri hakkında daha fazla bilgi için aşağıdaki makalelere bakın.

- [Azure SQL veritabanı Machine Learning Hizmetleri ile R (Önizleme)](sql-database-machine-learning-services-overview.md)
- [Machine Learning Hizmetleri (Önizleme) kullanarak Azure SQL veritabanı içinde Gelişmiş R işlevler yazma](sql-database-machine-learning-services-functions.md)
- [R ve SQL Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) verileri ile çalışma](sql-database-machine-learning-services-data-issues.md)