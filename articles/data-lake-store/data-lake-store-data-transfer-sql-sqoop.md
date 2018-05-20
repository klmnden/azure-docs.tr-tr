---
title: Data Lake Store ile Sqoop kullanarak Azure SQL veritabanı arasında veri kopyalama | Microsoft Docs
description: Azure SQL veritabanı ve Data Lake Store arasında veri kopyalamak için Sqoop kullanın
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 3f914b2a-83cc-4950-b3f7-69c921851683
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 01/09/2018
ms.author: nitinme
ms.openlocfilehash: 255ba1468f4dfbf83a88390fff7355fa2403cac5
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="copy-data-between-data-lake-store-and-azure-sql-database-using-sqoop"></a>Data Lake Store ile Sqoop kullanarak Azure SQL veritabanı arasında veri kopyalama
Apache Sqoop içeri ve dışarı aktarma Azure SQL veritabanı ve Data Lake Store arasında veri için nasıl kullanılacağını öğrenin.

## <a name="what-is-sqoop"></a>Sqoop nedir?
Büyük veri uygulamaları günlükleri ve dosyaları gibi yapılandırılmamış ve yarı yapılandırılmış veri işleme için doğal bir seçimdir. Ancak, aynı zamanda olabilir ilişkisel veritabanlarında depolanan yapılandırılmış verileri işlemek için gerek.

[Apache Sqoop](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html) ilişkisel veritabanları ve Data Lake Store gibi bir büyük veri deposu arasında veri aktarımı için tasarlanmış bir araçtır. Azure SQL veritabanı gibi bir ilişkisel veritabanı yönetim sistemine (RDBMS) verilerini Data Lake Store aktarmak için kullanabilirsiniz. Ardından dönüştürme ve büyük veri iş yüklerini kullanarak verileri analiz ve ardından verileri bir RDBMS dışarı aktarabilirsiniz. Bu öğreticide, bir Azure SQL veritabanı ilişkisel veritabanı olarak gelen içeri/dışarı aktarma için kullanın.

## <a name="prerequisites"></a>Önkoşullar
Bu makaleye başlamadan önce aşağıdakilere sahip olmanız ve aşağıdaki işlemleri yapmış olmanız gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Azure Data Lake Store hesabı**. Bir oluşturma hakkında yönergeler için bkz: [Azure Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-portal.md)
* **Azure Hdınsight kümesi** bir Data Lake Store hesabına erişim. Bkz: [Data Lake Store ile bir Hdınsight kümesi oluşturmayı](data-lake-store-hdinsight-hadoop-use-portal.md). Bu makalede, Data Lake Store erişimi olan bir Hdınsight Linux kümesi olduğunu varsayar.
* **Azure SQL Veritabanı**. Bir oluşturma hakkında yönergeler için bkz: [bir Azure SQL veritabanı oluşturma](../sql-database/sql-database-get-started.md)

## <a name="do-you-learn-fast-with-videos"></a>Videolarla daha hızlı mı öğreniyorsunuz?
[Bu videoyu izleyin](https://mix.office.com/watch/1butcdjxmu114) Azure Storage Bloblarında ve Distcp'yi kullanarak Data Lake Store arasında veri kopyalama hakkında.

## <a name="create-sample-tables-in-the-azure-sql-database"></a>Azure SQL veritabanı'nda örnek tabloları oluşturma
1. İle başlamak Azure SQL veritabanı'nda iki örnek tablo oluşturun. Kullanım [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) veya Azure SQL veritabanına bağlanmak ve aşağıdaki sorguları çalıştırmak için Visual Studio.

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

    **Tablo2 oluşturma**

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
2. İçinde **tablo1**, bazı örnek veri ekleyin. Bırakın **tablo2** boş. Verileri içeri aktaracak **tablo1** Data Lake Store içine. Ardından, biz veri Data Lake Store verilecek **tablo2**. Aşağıdaki kod parçacığında çalıştırın.

        INSERT INTO [dbo].[Table1] VALUES (1,'Neal','Kell'), (2,'Lila','Fulton'), (3, 'Erna','Myers'), (4,'Annette','Simpson');


## <a name="use-sqoop-from-an-hdinsight-cluster-with-access-to-data-lake-store"></a>Sqoop erişim bir Hdınsight kümesini Data Lake Store'a kullanın
Hdınsight kümesi kullanılabilir Sqoop paketler zaten var. Hdınsight kümesi Data Lake Store ek depolama alanı kullanmak için yapılandırdıysanız, Sqoop (herhangi bir yapılandırma değişikliği) (Bu örnekte, Azure SQL veritabanı) ilişkisel veritabanı arasında veri içeri/dışarı aktarma için kullanabileceğiniz ve Data Lake Store hesabı.

1. Bu öğretici için SSH kümeye bağlanmak için kullanması gereken şekilde Linux kümesi oluşturulan varsayalım. Bkz: [Linux tabanlı Hdınsight kümesine bağlanma](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).
2. Data Lake Store hesabı kümeden erişip erişemeyeceğini doğrulayın. SSH isteminden aşağıdaki komutu çalıştırın:

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net/

    Bu dosyalar/klasörler Data Lake Store hesabındaki listesini sağlamanız gerekir.

### <a name="import-data-from-azure-sql-database-into-data-lake-store"></a>Data Lake Store Azure SQL veritabanından veri içeri aktarın
1. Sqoop paketlerinin kullanılabildiği dizinine gidin. Genellikle, bu anda olacaktır `/usr/hdp/<version>/sqoop/bin`.
2. Verilerini içeri **tablo1** Data Lake Store hesabında. Aşağıdaki sözdizimini kullanın:

        sqoop-import --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table1 --target-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1

    Unutmayın **sql veritabanı sunucusu adı** yer tutucu Azure SQL veritabanı çalıştığı sunucunun adını temsil eder. **SQL veritabanı adı** yer tutucu gerçek veritabanı adını temsil eder.

    Örneğin,


        sqoop-import --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table1 --target-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1

1. Data Lake Store hesabına veri aktarıldı doğrulayın. Şu komutu çalıştırın:

        hdfs dfs -ls adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/

    Şu çıktı görmeniz gerekir.


        -rwxrwxrwx   0 sshuser hdfs          0 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/_SUCCESS
        -rwxrwxrwx   0 sshuser hdfs         12 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00000
        -rwxrwxrwx   0 sshuser hdfs         14 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00001
        -rwxrwxrwx   0 sshuser hdfs         13 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00002
        -rwxrwxrwx   0 sshuser hdfs         18 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00003

    Her **bölümü-m -*** dosya karşılık gelen kaynak tablodaki bir satıra **tablo1**. -M - bölümünün içeriğini görüntülemek * dosyaları doğrulayın.


### <a name="export-data-from-data-lake-store-into-azure-sql-database"></a>Data Lake Store Azure SQL veritabanına verileri dışa
1. Verileri Data Lake Store hesabından boş tabloya dışarı **tablo2**, Azure SQL veritabanında. Aşağıdaki sözdizimini kullanın.

        sqoop-export --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table2 --export-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

    Örneğin,


        sqoop-export --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table2 --export-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

1. SQL veritabanı tablosuna verileri karşıya yüklenen doğrulayın. Kullanım [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) veya Azure SQL veritabanına bağlanmak ve aşağıdaki sorguyu çalıştırmak için Visual Studio.

        SELECT * FROM TABLE2

    Bu, aşağıdaki çıkış olması gerekir.

         ID  FName   LName
        ------------------
        1    Neal    Kell
        2    Lila    Fulton
        3    Erna    Myers
        4    Annette    Simpson

## <a name="performance-considerations-while-using-sqoop"></a>Sqoop kullanırken performans etkenleri

Data Lake Store'a veri kopyalamak için Sqoop işinizi ayarlama performans için bkz: [Sqoop performans belge](https://blogs.msdn.microsoft.com/bigdatasupport/2015/02/17/sqoop-job-performance-tuning-in-hdinsight-hadoop/).

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Storage Bloblarından Data Lake Store'a veri kopyalama](data-lake-store-copy-data-azure-storage-blob.md)
* [Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics'i Data Lake Store ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight'ı Data Lake Store ile kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)
