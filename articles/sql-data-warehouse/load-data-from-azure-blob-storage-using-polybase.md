---
title: "Polybase veri yükleme - Azure Storage Blobuna Azure SQL Data Warehouse | Microsoft Docs"
description: "New York Taxicab verileri Azure blob depolama alanından Azure SQL Data Warehouse'a yüklemek için Azure portalı ve SQL Server Management Studio kullanan bir öğretici."
services: sql-data-warehouse
documentationcenter: 
author: ckarst
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-data-warehouse
ms.custom: mvc,develop data warehouses
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: Active
ms.date: 11/17/2017
ms.author: cakarst
ms.reviewer: barbkess
ms.openlocfilehash: fe3ea6c22fafad0d0dcf611ceb365a2ebca80011
ms.sourcegitcommit: 4ea06f52af0a8799561125497f2c2d28db7818e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2017
---
# <a name="use-polybase-to-load-data-from-azure-blob-storage-to-azure-sql-data-warehouse"></a>Verileri Azure blob depolama alanından Azure SQL Data Warehouse'a yüklemek için Polybase'i kullanın

PolyBase verileri SQL Data Warehouse'a almak için teknoloji yüklenirken standardıdır. Bu öğreticide, New York Taxicab verileri Azure blob depolama alanından Azure SQL Data Warehouse'a yüklemek için Polybase'i kullanın. Öğretici kullanır [Azure portal](https://portal.azure.com) ve [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms.md) (SSMS) için: 

> [!div class="checklist"]
> * Azure portalında bir data warehouse oluşturma
> * Azure portalında bir sunucu düzeyinde güvenlik duvarı kuralı ayarlayın
> * Veri ambarı SSMS ile bağlanma
> * Verileri yüklemek için atanmış bir kullanıcı oluşturun
> * Azure blob storage'da verileri için dış tabloları oluşturma
> * Verileri veri ambarınıza yüklemek için CTAS T-SQL deyimini kullanın
> * Bu yükleme gibi veri ilerlemesini görüntülemek
> * Yeni yüklenen verilere ilişkin istatistikler oluşturma

Bir Azure aboneliğiniz yoksa [ücretsiz bir hesap oluşturma](https://azure.microsoft.com/free/) başlamadan önce.

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğreticiye başlamadan önce yükleyip en yeni sürümünü [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms.md) (SSMS).


## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

[Azure Portal](https://portal.azure.com/)’da oturum açın.

## <a name="create-a-blank-sql-data-warehouse"></a>Boş bir SQL data warehouse oluşturma

Bir Azure SQL veri ambarı tanımlanan bir dizi ile oluşturulan [işlem kaynaklarını](performance-tiers.md). Veritabanı içinde oluşturulan bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) ve bir [Azure SQL mantıksal sunucusuna](../sql-database/sql-database-features.md). 

Boş SQL data warehouse oluşturmak için aşağıdaki adımları izleyin. 

1. Tıklatın **yeni** Azure portalının sol üst köşesindeki düğmesi.

2. Seçin **veritabanları** gelen **yeni** sayfasında ve seçin **SQL Data Warehouse** altında **öne çıkan** üzerinde **yeni**sayfası.

    ![Data warehouse oluşturma](media/load-data-from-azure-blob-storage-using-polybase/create-empty-data-warehouse.png)

3. Aşağıdaki bilgiler ile SQL Data Warehouse formu doldurun:   

   | Ayar | Önerilen değer | Açıklama | 
   | ------- | --------------- | ----------- | 
   | **Veritabanı adı** | mySampleDataWarehouse | Geçerli veritabanı adları için bkz. [Veritabanı Tanımlayıcıları](/sql/relational-databases/databases/database-identifiers). | 
   | **Abonelik** | Aboneliğiniz  | Abonelikleriniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.windowsazure.com/Subscriptions). |
   | **Kaynak grubu** | myResourceGroup | Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). |
   | **Kaynak seçin** | Boş veritabanı | Boş bir veritabanı oluşturmak için belirtir. Not, bir veri ambarı veritabanı türüdür.|

    ![Data warehouse oluşturma](media/load-data-from-azure-blob-storage-using-polybase/create-data-warehouse.png)

4. Yeni veritabanınız için yeni bir sunucu oluşturup yapılandırmak üzere **Sunucu**’ya tıklayın. Doldurmak **yeni sunucu form** aşağıdaki bilgilerle: 

    | Ayar | Önerilen değer | Açıklama | 
    | ------- | --------------- | ----------- |
    | **Sunucu adı** | Genel olarak benzersiz bir ad | Geçerli sunucu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). | 
    | **Sunucu yöneticisi oturum açma bilgileri** | Geçerli bir ad | Geçerli oturum açma adları için bkz. [Veritabanı Tanımlayıcıları](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers).|
    | **Parola** | Geçerli bir parola | Parolanız en az sekiz karakter olmalıdır ve aşağıdaki kategoriden üçünden karakterler içermelidir: büyük harf karakterler, küçük harfler, sayılar ve alfasayısal olmayan karakter. |
    | **Konum** | Geçerli bir konum | Bölgeler hakkında bilgi için bkz. [Azure Bölgeleri](https://azure.microsoft.com/regions/). |

    ![Veritabanı sunucusu oluşturma](media/load-data-from-azure-blob-storage-using-polybase/create-database-server.png)

5. **Seç**'e tıklayın.

6. Tıklatın **performans katmanı** veri ambarı esneklik veya işlem için optimize edilmiştir ve sayısı, veri ambarı birimlerini olup olmadığını belirtmek için. 

7. Bu öğretici için seçin **esneklik için en iyi duruma getirilmiş** hizmet katmanı. Varsayılan olarak, kaydırıcıyı kümesine **DW400**.  Yukarı ve aşağı nasıl çalıştığını görmek için taşımayı deneyin. 

    ![Performans yapılandırın](media/load-data-from-azure-blob-storage-using-polybase/configure-performance.png)

8. **Uygula**'ya tıklayın.
9. SQL veri ambarı sayfasında seçin bir **harmanlama** boş veritabanı için. Bu öğretici için varsayılan değeri kullanın. Harmanlamaları hakkında daha fazla bilgi için bkz: [harmanlamaları](/sql/t-sql/statements/collations.md)

11. SQL Veritabanı formunu tamamladıktan sonra veritabanını sağlamak için **Oluştur**’a tıklayın. Sağlama birkaç dakika sürer. 

    ![Oluştur'u tıklatın](media/load-data-from-azure-blob-storage-using-polybase/click-create.png)

12. Araç çubuğunda **Bildirimler**’e tıklayarak dağıtım işlemini izleyin.
    
     ![bildirim](media/load-data-from-azure-blob-storage-using-polybase/notification.png)

## <a name="create-a-server-level-firewall-rule"></a>Sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma

SQL veri ambarı hizmeti, sunucu düzeyinde-dış uygulamaları ve araçları sunucu veya sunucu üzerindeki herhangi bir veritabanına bağlanma engelleyen bir güvenlik duvarı oluşturur. Bağlantıyı etkinleştirmek için belirli IP adresleri için bağlantıyı etkinleştirmek güvenlik duvarı kuralı ekleyebilirsiniz.  Oluşturmak için bu adımları bir [sunucu düzeyinde güvenlik duvarı kuralı](../sql-database/sql-database-firewall-configure.md) , istemcinin IP adresi için. 

> [!NOTE]
> SQL veri ambarı 1433 numaralı bağlantı noktası üzerinden iletişim kurar. Şirket ağı içinde bağlanması çalışıyorsanız, bağlantı noktası 1433 üzerinden giden trafik, ağınızın güvenlik duvarı tarafından izin verilmeyebilir. Bu durumda BT departmanınız 1433 numaralı bağlantı noktasını açmadığı sürece Azure SQL Veritabanı sunucunuza bağlanamazsınız.
>

1. Dağıtım tamamlandıktan sonra, soldaki menüden **SQL veritabanları**'na ve ardından **SQL veritabanları** sayfasında **mySampleDatabase** öğesine tıklayın. Veritabanınız için genel bakış sayfası açılır ve tam sunucu adını gösteren (gibi **mynewserver 20171113.database.windows.net**) ve diğer yapılandırmalar için seçenekler sağlar. 

2. Sonraki hızlı başlangıçlarda sunucunuza ve veritabanlarına bağlanmak için bu tam sunucu adını kopyalayın. Ardından sunucu ayarları'nı açmak için sunucu adına tıklayın.

    ![sunucu adını bulma](media/load-data-from-azure-blob-storage-using-polybase/find-server-name.png) 

3. Sunucu Ayarları'nı açmak için sunucu adına tıklayın.

    ![Sunucu ayarları](media/load-data-from-azure-blob-storage-using-polybase/server-settings.png) 

5. Tıklatın **güvenlik duvarı ayarlarını göster**. SQL Veritabanı sunucusu için **Güvenlik duvarı ayarları** sayfası açılır. 

    ![sunucu güvenlik duvarı kuralı](media/load-data-from-azure-blob-storage-using-polybase/server-firewall-rule.png) 

4. Geçerli IP adresinizi yeni bir güvenlik duvarı kuralına eklemek için araç çubuğunda **İstemci IP’si Ekle** öğesine tıklayın. Güvenlik duvarı kuralı, 1433 numaralı bağlantı noktasını tek bir IP adresi veya bir IP adresi aralığı için açabilir.

5. **Kaydet** düğmesine tıklayın. Geçerli IP adresiniz için mantıksal sunucuda 1433 numaralı bağlantı noktası açılarak sunucu düzeyinde güvenlik duvarı kuralı oluşturulur.

6. **Tamam**’a tıklayın ve sonra **Güvenlik duvarı ayarları** sayfasını kapatın.

Şimdi, SQL server ve bu IP adresini kullanarak kendi veri ambarlarında bağlanabilirsiniz. Bağlantı, SQL Server Management Studio veya tercih ettiğiniz başka bir aracı çalışır. Bağlandığınızda, daha önce oluşturduğunuz ServerAdmin hesabı kullanın.  

> [!IMPORTANT]
> Varsayılan olarak, SQL Veritabanı güvenlik duvarı üzerinden erişim tüm Azure hizmetleri için etkindir. Tıklatın **OFF** bu sayfa ve ardından **kaydetmek** tüm Azure Hizmetleri için Güvenlik Duvarı'nı devre dışı bırakmak için.

## <a name="get-the-fully-qualified-server-name"></a>Tam sunucu adını Al

Tam sunucu adını, SQL server için Azure portalında alın. Daha sonra tam adı sunucuya bağlanırken kullanacaksınız.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Soldaki menüden **SQL Veritabanları**’nı seçin ve **SQL veritabanları** sayfasında veritabanınıza tıklayın. 
3. Veritabanınızın Azure portal sayfasındaki **Temel Bilgiler** bölmesinde, **Sunucu adını** bulup kopyalayın. Bu örnekte, tam mynewserver 20171113.database.windows.net addır. 

    ![bağlantı bilgileri](media/load-data-from-azure-blob-storage-using-polybase/find-server-name.png)  

## <a name="connect-to-the-server-as-server-admin"></a>Sunucu Yöneticisi olarak sunucuya bağlanın

Bu bölümde kullanan [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms.md) Azure SQL sunucunuza bir bağlantı kurmak için (SSMS).

1. SQL Server Management Studio’yu açın.

2. **Sunucuya Bağlan** iletişim kutusuna şu bilgileri girin:

    | Ayar      | Önerilen değer | Açıklama | 
    | ------------ | --------------- | ----------- | 
    | Sunucu türü | Veritabanı altyapısı | Bu değer gereklidir |
    | Sunucu adı | Tam sunucu adı | Adı, şunun gibi olmalıdır: **mynewserver 20171113.database.windows.net**. |
    | Kimlik Doğrulaması | SQL Server Kimlik Doğrulaması | Bu öğreticide yapılandırdığımız tek kimlik doğrulaması türü SQL Kimlik Doğrulamasıdır. |
    | Oturum Aç | Sunucu yöneticisi hesabı | Bu, sunucuyu oluştururken belirttiğiniz hesaptır. |
    | Parola | Sunucu yöneticisi hesabınızın parolası | Bu, sunucuyu oluştururken belirttiğiniz paroladır. |

    ![sunucuya bağlan](media/load-data-from-azure-blob-storage-using-polybase/connect-to-server.png)

4. **Bağlan**'a tıklayın. SSMS’te Nesne Gezgini penceresi açılır. 

5. Nesne Gezgini'nde genişletin **veritabanları**. Ardından **sistem veritabanları** ve **ana** ana veritabanında nesneleri görüntülemek için.  Genişletme **mySampleDatabase** , yeni veritabanı nesneleri görüntülemek için.

    ![Veritabanı nesneleri](media/load-data-from-azure-blob-storage-using-polybase/connected.png) 

## <a name="create-a-user-for-loading-data"></a>Verileri yüklemek için bir kullanıcı oluşturun

Sunucu yönetici hesabı yönetim işlemlerini gerçekleştirmek için tasarlanmıştır ve kullanıcı verilerini sorguları çalıştırmak için uygun değildir. Genellikle veri yükleme miktarda bellek gerektirir. [Bellek üst sınırlar](performance-tiers.md#memory-maximums) göre tanımlanan [performans katmanı](performance-tiers.md), ve [kaynak sınıfı](resource-classes-for-workload-management.md). 

Bir oturum açma ve verileri yüklemek için ayrılmış kullanıcı oluşturmak en iyisidir. Yükleme kullanıcı eklemek bir [kaynak sınıfı](resource-classes-for-workload-management.md) uygun en fazla bellek ayırma sağlar.

Şu anda sunucu yöneticisi olarak bağlandıktan sonra oturumlar ve kullanıcılar oluşturabilirsiniz. Bir oturum açma ve kullanıcı olarak adlandırılır ve oluşturmak için aşağıdaki adımları kullanın **LoaderRC20**. Kullanıcıya atamak **staticrc20** kaynak sınıfı. 

1.  SSMS, sağ **ana** açılır menü göstermek ve seçmek için **yeni sorgu**. Yeni bir sorgu penceresi açılır.

    ![Ana yeni sorgu](media/load-data-from-azure-blob-storage-using-polybase/create-loader-login.png)

2. Sorgu penceresinde, bir oturum açma ve LoaderRC20, adlı kullanıcı 'a123STRONGpassword!' için kendi parolanızı değiştirerek oluşturmak için bu T-SQL komutlarıyla girin. 

    ```sql
    CREATE LOGIN LoaderRC20 WITH PASSWORD = 'a123STRONGpassword!';
    CREATE USER LoaderRC20 FOR LOGIN LoaderRC20;
    ```

3. **Yürüt**’e tıklayın.

4. Sağ **mySampleDataWarehouse**ve seçin **yeni sorgu**. Yeni bir sorgu penceresi açılır.  

    ![Örnek veri ambarı üzerinde yeni bir sorgu](media/load-data-from-azure-blob-storage-using-polybase/create-loading-user.png)
 
5. LoaderRC20 oturum açma için LoaderRC20 adlı bir veritabanı kullanıcısı oluşturmak için aşağıdaki T-SQL komutlarıyla girin. İkinci satır yeni kullanıcı yeni veri ambarı denetim izinleri verir.  Bu izinler, veritabanının sahibi kullanıcı yapmaya benzerdir. Üçüncü satır staticrc20 bir üyesi olarak yeni kullanıcı ekler [kaynak sınıfı](resource-classes-for-workload-management.md).

    ```sql
    CREATE USER LoaderRC20 FOR LOGIN LoaderRC20;
    GRANT CONTROL ON DATABASE::[mySampleDataWarehouse] to LoaderRC20;
    EXEC sp_addrolemember 'staticrc20', 'LoaderRC20';
    ```

6. **Yürüt**’e tıklayın.

## <a name="connect-to-the-server-as-the-loading-user"></a>Sunucuya yükleyen kullanıcı olarak bağlanma

Veri yükleme doğru ilk adımı LoaderRC20 oturum açma.  

1. Nesne Gezgini'nde tıklatarak **Bağlan** menü ve select aşağı bırakma **veritabanı altyapısı**. **Sunucuya Bağlan** iletişim kutusu görüntülenir.

    ![Yeni oturum açma ile bağlanma](media/load-data-from-azure-blob-storage-using-polybase/connect-as-loading-user.png)

2. Tam sunucu adını girin, ancak bu kez girmeniz **LoaderRC20** olarak oturum açın.  LoaderRC20 için parolanızı girin.

3. **Bağlan**'a tıklayın.

4. Bağlantınızı hazır olduğunda, nesne Gezgini'nde iki sunucu bağlantılarını görürsünüz. Bir bağlantı ServerAdmin ve MedRCLogin olarak tek bir bağlantı olarak.

    ![Bağlantı başarılı olur](media/load-data-from-azure-blob-storage-using-polybase/connected-as-new-login.png)

## <a name="create-external-tables-for-the-sample-data"></a>Örnek veriler için dış tabloları oluşturma

Yeni veri ambarına veri yükleme işlemini başlatmaya hazır. Bu öğretici nasıl kullanılacağını gösterir [Polybase](/sql/relational-databases/polybase/polybase-guide.md) bir Azure storage blobundan New York şehrinde ücreti cab verileri yüklenemiyor. [Yüklemeye genel bakış](sql-data-warehouse-overview-load.md) bölümünde, verilerinizi Azure blob depolama alanına alma veya doğrudan kaynağınızdan SQL Veri Ambarı’na yükleme konusunda ileride işinize yarayacak bilgiler edinebilirsiniz.

Aşağıdaki SQL Çalıştır komut dosyalarını yüklemek istediğiniz verileri hakkında bilgiler belirtin. Verilerin bulunduğu bu bilgileri içeren veri ve veriler için tablo tanımı içeriğini biçimi. 

1. Önceki bölümde LoaderRC20 veri ambarınıza günlüğe. SSMS, LoaderRC20 bağlantınızı sağ tıklatın ve seçin **yeni sorgu**.  Yeni bir sorgu penceresi görünür. 

    ![Yeni sorgu penceresi yükleniyor](media/load-data-from-azure-blob-storage-using-polybase/new-loading-query.png)

2. Önceki görüntüde, sorgu penceresine karşılaştırın.  Yeni Sorgu penceresinde LoaderRC20 çalışan ve MySampleDataWarehouse veritabanınızda sorgular gerçekleştirme doğrulayın. Tüm yükleme adımları gerçekleştirmek için bu sorgu penceresi kullanın.

3. MySampleDataWarehouse veritabanı için bir ana anahtar oluşturun. Veritabanı başına yalnızca bir kez ana anahtar oluşturmanız gerekir. 

    ```sql
    CREATE MASTER KEY;
    ```

4. Aşağıdaki komutu çalıştırarak [dış veri kaynağı oluştur](/sql/t-sql/statements/create-external-data-source-transact-sql.md) Azure blob'u konumunu tanımlamak için deyimi. Dış ücreti cab veri konumudur.  Sorgu penceresine eklenen bir komut çalıştırmak için önce çalıştırmak istediğiniz komutları vurgulayın **yürütme**.

    ```sql
    CREATE EXTERNAL DATA SOURCE NYTPublic
    WITH
    (
        TYPE = Hadoop,
        LOCATION = 'wasbs://2013@nytaxiblob.blob.core.windows.net/'
    );
    ```

5. Aşağıdaki komutu çalıştırarak [oluşturmak dış dosya BİÇİMİNİ](/sql/t-sql/statements/create-external-file-format-transact-sql.md) biçimlendirme özellikleri ve dış veri dosyası için seçeneklerini belirtmek için T-SQL ifadesi. Bu deyim dış veriler metin olarak depolanır ve değerleri dikey çubukla ayrılan belirtir ('| ') karakteri. Dış dosya ile Gzip sıkıştırılmış. 

    ```sql
    CREATE EXTERNAL FILE FORMAT uncompressedcsv
    WITH (
        FORMAT_TYPE = DELIMITEDTEXT,
        FORMAT_OPTIONS ( 
            FIELD_TERMINATOR = ',',
            STRING_DELIMITER = '',
            DATE_FORMAT = '',
            USE_TYPE_DEFAULT = False
        )
    );
    CREATE EXTERNAL FILE FORMAT compressedcsv
    WITH ( 
        FORMAT_TYPE = DELIMITEDTEXT,
        FORMAT_OPTIONS ( FIELD_TERMINATOR = '|',
            STRING_DELIMITER = '',
        DATE_FORMAT = '',
            USE_TYPE_DEFAULT = False
        ),
        DATA_COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'
    );
    ```

6.  Aşağıdaki komutu çalıştırarak [CREATE SCHEMA](/sql/t-sql/statements/create-schema-transact-sql.md) deyimi, dış dosya biçimini için bir şema oluşturun. Şema oluşturmak üzere olduğunuz dış tablolara düzenlemek için bir yol sağlar.

    ```sql
    CREATE SCHEMA ext;
    ```

7. Harici tabloları oluşturun. Tablo tanımları SQL veri ambarı'nda depolanır, ancak tabloları Azure blob storage'da depolanan verileri başvuru. Aşağıdaki T-SQL komutlarını çalıştırarak, tamamı dış veri kaynağımızda daha önce tanımladığımız Azure blobunu işaret eden dış tablolar oluşturun.

    ```sql
    CREATE EXTERNAL TABLE [ext].[Date] 
    (
        [DateID] int NOT NULL,
        [Date] datetime NULL,
        [DateBKey] char(10) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfMonth] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DaySuffix] varchar(4) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeek] char(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeekInMonth] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeekInYear] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfQuarter] varchar(3) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfYear] varchar(3) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfMonth] varchar(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfQuarter] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfYear] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Month] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthOfQuarter] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Quarter] char(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [QuarterName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Year] char(4) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [YearName] char(7) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthYear] char(10) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MMYYYY] char(6) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [FirstDayOfMonth] date NULL,
        [LastDayOfMonth] date NULL,
        [FirstDayOfQuarter] date NULL,
        [LastDayOfQuarter] date NULL,
        [FirstDayOfYear] date NULL,
        [LastDayOfYear] date NULL,
        [IsHolidayUSA] bit NULL,
        [IsWeekday] bit NULL,
        [HolidayUSA] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'Date',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    ); 
    CREATE EXTERNAL TABLE [ext].[Geography]
    (
        [GeographyID] int NOT NULL,
        [ZipCodeBKey] varchar(10) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [County] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [City] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [State] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Country] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [ZipCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'Geography',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0 
    );      
    CREATE EXTERNAL TABLE [ext].[HackneyLicense]
    (
        [HackneyLicenseID] int NOT NULL,
        [HackneyLicenseBKey] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [HackneyLicenseCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'HackneyLicense',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    );
    CREATE EXTERNAL TABLE [ext].[Medallion]
    (
        [MedallionID] int NOT NULL,
        [MedallionBKey] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [MedallionCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'Medallion',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;  
    CREATE EXTERNAL TABLE [ext].[Time]
    (
        [TimeID] int NOT NULL,
        [TimeBKey] varchar(8) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [HourNumber] tinyint NOT NULL,
        [MinuteNumber] tinyint NOT NULL,
        [SecondNumber] tinyint NOT NULL,
        [TimeInSecond] int NOT NULL,
        [HourlyBucket] varchar(15) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [DayTimeBucketGroupKey] int NOT NULL,
        [DayTimeBucket] varchar(100) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL
    )
    WITH
    (
        LOCATION = 'Time',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    );
    CREATE EXTERNAL TABLE [ext].[Trip]
    (
        [DateID] int NOT NULL,
        [MedallionID] int NOT NULL,
        [HackneyLicenseID] int NOT NULL,
        [PickupTimeID] int NOT NULL,
        [DropoffTimeID] int NOT NULL,
        [PickupGeographyID] int NULL,
        [DropoffGeographyID] int NULL,
        [PickupLatitude] float NULL,
        [PickupLongitude] float NULL,
        [PickupLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DropoffLatitude] float NULL,
        [DropoffLongitude] float NULL,
        [DropoffLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [PassengerCount] int NULL,
        [TripDurationSeconds] int NULL,
        [TripDistanceMiles] float NULL,
        [PaymentType] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [FareAmount] money NULL,
        [SurchargeAmount] money NULL,
        [TaxAmount] money NULL,
        [TipAmount] money NULL,
        [TollsAmount] money NULL,
        [TotalAmount] money NULL
    )
    WITH
    (
        LOCATION = 'Trip2013',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = compressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    );
    CREATE EXTERNAL TABLE [ext].[Weather]
    (
        [DateID] int NOT NULL,
        [GeographyID] int NOT NULL,
        [PrecipitationInches] float NOT NULL,
        [AvgTemperatureFahrenheit] float NOT NULL
    )
    WITH
    (
        LOCATION = 'Weather',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
    ```

8. Nesne Gezgini'nde dış tablolara oluşturduğunuz listesini görmek için mySampleDataWarehouse genişletin.

    ![Görünüm dış tablolar](media/load-data-from-azure-blob-storage-using-polybase/view-external-tables.png)

## <a name="load-the-data-into-your-data-warehouse"></a>Veri ambarına veri yükleme

Bu bölüm yalnızca örnek verileri Azure Storage Blobundan SQL Data Warehouse'a yüklemek için tanımlanan dış tabloları kullanır.  

Komut dosyası kullanan [oluşturma tablo AS seçin (CTAS)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md) verileri Azure Storage Blobundan, veri ambarındaki yeni tablolara yüklemek için T-SQL ifadesi. CTAS bir select deyimi sonuçlarına dayalı yeni bir tablo oluşturur. Yeni tablo, select deyiminin sonuçları ile aynı sütunlara ve veri türlerine sahiptir. Select deyimi bir dış tablosundan seçtiğinde, SQL veri ambarı verileri veri ambarındaki ilişkisel bir tabloya alır. 

1. Verileri veri ambarındaki yeni tablolara yüklemek için aşağıdaki betiği çalıştırın.

    ```sql
    CREATE TABLE [dbo].[Date]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Date]
    OPTION (LABEL = 'CTAS : Load [dbo].[Date]')
    ;
    CREATE TABLE [dbo].[Geography]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT * FROM [ext].[Geography]
    OPTION (LABEL = 'CTAS : Load [dbo].[Geography]')
    ;
    CREATE TABLE [dbo].[HackneyLicense]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[HackneyLicense]
    OPTION (LABEL = 'CTAS : Load [dbo].[HackneyLicense]')
    ;
    CREATE TABLE [dbo].[Medallion]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Medallion]
    OPTION (LABEL = 'CTAS : Load [dbo].[Medallion]')
    ;
    CREATE TABLE [dbo].[Time]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Time]
    OPTION (LABEL = 'CTAS : Load [dbo].[Time]')
    ;
    CREATE TABLE [dbo].[Weather]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Weather]
    OPTION (LABEL = 'CTAS : Load [dbo].[Weather]')
    ;
    CREATE TABLE [dbo].[Trip]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Trip]
    OPTION (LABEL = 'CTAS : Load [dbo].[Trip]')
    ;
    ```

2. Verilerinizi yüklenirken görüntüleyin. Birkaç GB veri yüklüyorsunuz ve yüksek performanslı kümelenmiş columnstore dizinlerine sıkıştırıyorsunuz. Yüklemenin durumunu göstermek için, dinamik yönetim görünümleri (DMV’ler) kullanan aşağıdaki sorguyu çalıştırın. Sorguyu başlattıktan sonra, SQL Veri Ambarı ağır yükü kaldırırken siz bir kahve alıp arkanıza yaslanın.

    ```sql
    SELECT
        r.command,
        s.request_id,
        r.status,
        count(distinct input_name) as nbr_files,
        sum(s.bytes_processed)/1024/1024/1024 as gb_processed
    FROM 
        sys.dm_pdw_exec_requests r
        INNER JOIN sys.dm_pdw_dms_external_work s
        ON r.request_id = s.request_id
    WHERE
        r.[label] = 'CTAS : Load [dbo].[Date]' OR
        r.[label] = 'CTAS : Load [dbo].[Geography]' OR
        r.[label] = 'CTAS : Load [dbo].[HackneyLicense]' OR
        r.[label] = 'CTAS : Load [dbo].[Medallion]' OR
        r.[label] = 'CTAS : Load [dbo].[Time]' OR
        r.[label] = 'CTAS : Load [dbo].[Weather]' OR
        r.[label] = 'CTAS : Load [dbo].[Trip]'
    GROUP BY
        r.command,
        s.request_id,
        r.status
    ORDER BY
        nbr_files desc, 
        gb_processed desc;
    ```

3. Tüm sistem sorgularını görüntüleyin.

    ```sql
    SELECT * FROM sys.dm_pdw_exec_requests;
    ```

4. Verilerinizi veri ambarına sorunsuz şekilde yüklenen görmesini keyfini çıkarın.

    ![Yüklenen tablolarını görüntüleme](media/load-data-from-azure-blob-storage-using-polybase/view-loaded-tables.png)

## <a name="create-statistics-on-newly-loaded-data"></a>Yeni yüklenen verilere ilişkin istatistikler oluşturma

SQL Data Warehouse, istatistikleri otomatik olarak oluşturup güncelleştirmez. Bu nedenle yüksek sorgu performansı elde etmek için ilk yükleme işleminden sonra her tablonun her sütunu için istatistik oluşturulması önemlidir. Ayrıca veriler üzerinde önemli değişiklikler yapıldıktan sonra istatistiklerin güncelleştirilmesi de önemlidir.

1. Birleşimlerde kullanılma olasılığı olan sütunlarda istatistikler oluşturmak için şu komutları çalıştırın.

    ```sql
    CREATE STATISTICS [dbo.Date DateID stats] ON dbo.Date (DateID);
    CREATE STATISTICS [dbo.Trip DateID stats] ON dbo.Trip (DateID);
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

İşlem kaynakları ve veri ambarına yüklenen veriler için ücretlendirilirsiniz. Bunlar ayrı ayrı faturalandırılır. 

- Veri deposunda tutmak istiyorsanız, veri ambarı kullanmadığınızda işlem duraklatabilirsiniz. Göre işlem duraklatma ücret veri depolama için yalnızca olacaktır ve verilerle çalışmak hazır olduğunda işlem devam edebilirsiniz.
- Gelecekteki ücretleri kaldırmak istiyorsanız, veri ambarı silebilirsiniz. 

İstediğiniz kaynakları temizlemek için aşağıdaki adımları izleyin.

1. Oturum [Azure portal](https://portal.azure.com), veri Ambarınızı'ı tıklatın.

    ![Kaynakları temizleme](media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

2. İşlem duraklatmak için tıklatın **duraklatma** düğmesi. Veri ambarı duraklatıldığında göreceğiniz bir **Başlat** düğmesi.  İşlem devam etmek için tıklatın **Başlat**.

3. Veri ambarı, işlem ya da depolama için sizden ücret olmaz şekilde kaldırmak için tıklatın **silmek**.

4. Oluşturduğunuz SQL server kaldırmak için tıklatın **mynewserver 20171113.database.windows.net** önceki görüntü ve ardından **silmek**.  Bu dikkatli olun sunucuyu silmek sunucuya atanmış tüm veritabanlarını silecek.

5. Kaynak grubu kaldırmak için tıklatın **myResourceGroup**ve ardından **kaynak grubu Sil**.

## <a name="next-steps"></a>Sonraki adımlar 
Bu öğreticide, bir veri ambarı ve veri yüklemek için bir kullanıcı oluşturun öğrendiniz. Azure depolama Blob içinde depolanan verilerin yapısını tanımlamak için dış tabloları oluşturduğunuz ve veri ambarına veri yüklemek için PolyBase CREATE TABLE AS SELECT deyimi kullanılır. 

Bu işlemleri yapar:
> [!div class="checklist"]
> * Azure portalında bir veri ambarı oluşturuldu
> * Azure portalında bir sunucu düzeyinde güvenlik duvarı kuralı ayarlayın
> * SSMS veri ambarına bağlı
> * Verileri yüklemek için atanmış bir kullanıcı oluşturuldu
> * Azure depolama Blob verileri için oluşturulan dış tablolar
> * CTAS T-SQL deyimi, veri ambarına veri yüklemek için kullanılan
> * Bu yükleme gibi veri ilerlemesini görüntülenebilir
> * Yeni yüklenen veriler üzerinde oluşturulan istatistikleri

Varolan bir veritabanını SQL veri ambarına geçirme hakkında bilgi edinmek için geçişine genel bakış için ilerleyin.

> [!div class="nextstepaction"]
>[Varolan bir veritabanını SQL veri ambarına geçirme öğrenin](sql-data-warehouse-overview-migrate.md)
