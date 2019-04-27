---
title: Azure Data Lake depolama Gen1 ve Sqoop kullanarak Azure SQL veritabanı arasında veri kopyalama | Microsoft Docs
description: Azure SQL veritabanı ve Azure Data Lake depolama Gen1 arasında veri kopyalamak için Sqoop kullanma
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: 3f914b2a-83cc-4950-b3f7-69c921851683
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 7d3283b03d15278d1f7fd42a72b154dab1a442b4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60878775"
---
# <a name="copy-data-between-azure-data-lake-storage-gen1-and-azure-sql-database-using-sqoop"></a>Azure Data Lake depolama Gen1 ve Sqoop kullanarak Azure SQL veritabanı arasında veri kopyalama
Apache Sqoop alma ve Azure SQL veritabanı ve Azure Data Lake depolama Gen1 arasında verileri dışarı aktarmak için kullanmayı öğrenin.

## <a name="what-is-sqoop"></a>Sqoop nedir?
Büyük veri uygulamaları, günlükler ve dosyaları gibi yapılandırılmamış ve yarı yapılandırılmış verileri işlemek için doğal bir seçimdir. Ancak, aynı zamanda olabilir ilişkisel veritabanlarının depolanan yapılandırılmış verileri işlemek için bir gereksinim.

[Apache Sqoop](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html) ilişkisel veritabanı ve Data Lake depolama Gen1 gibi bir büyük veri deposu arasında veri aktarmak için tasarlanmış bir araçtır. Verileri Azure SQL veritabanı gibi bir ilişkisel veritabanı yönetim sistemi (RDBMS) Data Lake depolama Gen1 almak için kullanabilirsiniz. Daha sonra dönüştürme ve büyük veri iş yükleri kullanarak verileri analiz edin ve ardından verileri bir RDBMS'de geri dışarı aktarabilirsiniz. Bu öğreticide, Azure SQL veritabanı, ilişkisel veritabanı olarak öğesinden içeri/dışarı aktarma için kullanın.

## <a name="prerequisites"></a>Önkoşullar
Bu makaleye başlamadan önce aşağıdakilere sahip olmanız ve aşağıdaki işlemleri yapmış olmanız gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Azure Data Lake depolama Gen1 hesap**. Bir oluşturma hakkında yönergeler için bkz: [Azure Data Lake depolama Gen1 ile çalışmaya başlama](data-lake-store-get-started-portal.md)
* **Azure HDInsight kümesinde** bir Data Lake depolama Gen1 hesabına erişim. Bkz: [ile Data Lake depolama Gen1 bir HDInsight kümesi oluşturma](data-lake-store-hdinsight-hadoop-use-portal.md). Bu makale, Data Lake depolama Gen1 erişimi olan bir HDInsight Linux kümesi olduğunu varsayar.
* **Azure SQL Veritabanı**. Bir oluşturma hakkında yönergeler için bkz: [bir Azure SQL veritabanı oluşturma](../sql-database/sql-database-get-started.md)

## <a name="do-you-learn-fast-with-videos"></a>Videolarla daha hızlı mı öğreniyorsunuz?
[Bu videoyu](https://mix.office.com/watch/1butcdjxmu114) nasıl Azure depolama Blobları ile Data Lake depolama Gen1 arasında veri kopyalamak DistCp kullanma.

## <a name="create-sample-tables-in-the-azure-sql-database"></a>Azure SQL veritabanı'nda örnek tablo oluşturma
1. Çalışmaya başlamak için Azure SQL veritabanı'nda iki örnek tablo oluşturun. Kullanım [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) veya Visual Studio Azure SQL veritabanına bağlanmak ve aşağıdaki sorguları çalıştırın.

    **Tablo1 oluşturma**

        CREATE TABLE [dbo].[Table1](
        [ID] [int] NOT NULL,
        [FName] [nvarchar](50) NOT NULL,
        [LName] [nvarchar](50) NOT NULL,
         CONSTRAINT [PK_Table_1] PRIMARY KEY CLUSTERED
            (
                   [ID] ASC
            )
        ) ON [PRIMARY]
        GO

    **Table2 oluşturma**

        CREATE TABLE [dbo].[Table2](
        [ID] [int] NOT NULL,
        [FName] [nvarchar](50) NOT NULL,
        [LName] [nvarchar](50) NOT NULL,
         CONSTRAINT [PK_Table_2] PRIMARY KEY CLUSTERED
            (
                   [ID] ASC
            )
        ) ON [PRIMARY]
        GO
2. İçinde **Table1**, bazı örnek veriler ekleyin. Bırakın **Table2** boş. Biz verilerin hangi dosyadan içeri **Table1** Data Lake depolama Gen1 içine. Ardından, biz Data Lake depolama Gen1 ' den dışarı aktar **Table2**. Aşağıdaki kod parçacığını çalıştırın.

        INSERT INTO [dbo].[Table1] VALUES (1,'Neal','Kell'), (2,'Lila','Fulton'), (3, 'Erna','Myers'), (4,'Annette','Simpson');


## <a name="use-sqoop-from-an-hdinsight-cluster-with-access-to-data-lake-storage-gen1"></a>Data Lake depolama Gen1 için erişimi olan bir HDInsight kümesinden Sqoop'u kullanma
Sqoop paketleri kullanılabilir bir HDInsight kümesi zaten var. HDInsight kümesi, ek depolama alanı olarak Data Lake depolama Gen1 kullanmak için yapılandırdıysanız, Sqoop (herhangi bir yapılandırma değişikliği) ilişkisel bir veritabanında (Bu örnekte, Azure SQL veritabanı) arasındaki verileri içeri/dışarı aktarma için kullanabileceğiniz ve bir veri Gölü Depolama Gen1 hesabı.

1. Bu öğreticide, kümeye bağlanmak için SSH kullanması gereken şekilde bir Linux kümesi oluşturduğunuz varsayılır. Bkz: [Linux tabanlı HDInsight kümesine bağlanma](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).
2. Kümeden Data Lake depolama Gen1 hesap erişim sağlayıp sağlayamadığınızı doğrulayın. SSH isteminden aşağıdaki komutu çalıştırın:

        hdfs dfs -ls adl://<data_lake_storage_gen1_account>.azuredatalakestore.net/

    Bu Data Lake depolama Gen1 hesabındaki dosyaların/klasörlerin listesini sağlamanız gerekir.

### <a name="import-data-from-azure-sql-database-into-data-lake-storage-gen1"></a>Data Lake depolama Gen1 Azure SQL veritabanından veri içeri aktarın
1. Sqoop paketleri kullanılabilir olduğu dizine gidin. Bu, genellikle olacaktır `/usr/hdp/<version>/sqoop/bin`.
2. Verileri içeri aktarma **Table1** Data Lake depolama Gen1 hesaba. Aşağıdaki sözdizimini kullanın:

        sqoop-import --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table1 --target-dir adl://<data-lake-storage-gen1-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1

    Unutmayın **sql veritabanı sunucu adı** yer tutucusu, Azure SQL veritabanı çalıştığı sunucunun adını temsil eder. **SQL veritabanı adı** yer tutucusu gerçek veritabanı adını temsil eder.

    Örneğin,


        sqoop-import --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=twooley@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table1 --target-dir adl://myadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1

1. Verileri Data Lake depolama Gen1 hesabına aktarıldığını doğrulayın. Şu komutu çalıştırın:

        hdfs dfs -ls adl://hdiadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1/

    Aşağıdaki çıktıyı görmeniz gerekir.


        -rwxrwxrwx   0 sshuser hdfs          0 2016-02-26 21:09 adl://hdiadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1/_SUCCESS
        -rwxrwxrwx   0 sshuser hdfs         12 2016-02-26 21:09 adl://hdiadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00000
        -rwxrwxrwx   0 sshuser hdfs         14 2016-02-26 21:09 adl://hdiadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00001
        -rwxrwxrwx   0 sshuser hdfs         13 2016-02-26 21:09 adl://hdiadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00002
        -rwxrwxrwx   0 sshuser hdfs         18 2016-02-26 21:09 adl://hdiadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00003

    Her **bölümü-m -*** dosya karşılık gelen kaynak tablosunda bir satıra **Table1**. ' % S'bölümü - m - içeriğini görüntüleyebilirsiniz * doğrulamak için dosyaları.


### <a name="export-data-from-data-lake-storage-gen1-into-azure-sql-database"></a>Azure SQL veritabanı'na Data Lake depolama Gen1 verileri dışarı aktarma
1. Verileri Data Lake depolama Gen1 hesabından boş bir tablo için dışarı aktarma **Table2**, Azure SQL veritabanı'nda. Aşağıdaki sözdizimini kullanın.

        sqoop-export --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table2 --export-dir adl://<data-lake-storage-gen1-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

    Örneğin,


        sqoop-export --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=twooley@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table2 --export-dir adl://myadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

1. Verileri SQL veritabanı tablosuna karşıya yüklendiğini doğrulayın. Kullanım [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) veya Visual Studio Azure SQL veritabanına bağlanmak ve aşağıdaki sorguyu çalıştırın.

        SELECT * FROM TABLE2

    Bu, aşağıdaki çıktıyı olması gerekir.

         ID  FName   LName
        ------------------
        1    Neal    Kell
        2    Lila    Fulton
        3    Erna    Myers
        4    Annette    Simpson

## <a name="performance-considerations-while-using-sqoop"></a>Sqoop kullanırken performans konuları

Data Lake depolama Gen1 için veri kopyalamak için Sqoop işinizi ayarlama performans için bkz: [Sqoop performans belge](https://blogs.msdn.microsoft.com/bigdatasupport/2015/02/17/sqoop-job-performance-tuning-in-hdinsight-hadoop/).

## <a name="see-also"></a>Ayrıca bkz.
* [Data Lake depolama Gen1 için Azure depolama Bloblarından veri kopyalama](data-lake-store-copy-data-azure-storage-blob.md)
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics'i Data Lake depolama Gen1 ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight ile Data Lake depolama Gen1 kullanın](data-lake-store-hdinsight-hadoop-use-portal.md)
