---
title: "Apache Sqoop Hadoop - Azure Hdınsight ile | Microsoft Docs"
description: "Apache Sqoop hdınsight'ta Hadoop ve bir Azure SQL veritabanı arasında almak ve vermek için nasıl kullanılacağını öğrenin."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
author: Blackmist
tags: azure-portal
keywords: hadoop sqoop, sqoop
ms.assetid: 303649a5-4be5-4933-bf1d-4b232083c354
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: larryfr
ms.openlocfilehash: 35dcbb91e6af1480685c9fd5b829c54277c1c605
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="use-apache-sqoop-to-import-and-export-data-between-hadoop-on-hdinsight-and-sql-database"></a>İçeri ve dışarı aktarma hdınsight'ta Hadoop ile SQL veritabanı arasında veri için Apache Sqoop'u kullanın

[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Apache Sqoop Azure hdınsight'ta Hadoop kümesi ve Azure SQL Database veya Microsoft SQL Server veritabanı arasında almak ve vermek için nasıl kullanılacağını öğrenin. Bu adımlarda belge kullanımı `sqoop` Hadoop kümesi headnode doğrudan komutu. SSH baş düğümüne bağlanmak ve bu belgede komutları çalıştırmak için kullanın.

> [!IMPORTANT]
> Bu belgede yer alan adımlar, yalnızca Linux kullanmak Hdınsight kümeleri ile çalışır. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="install-freetds"></a>Ücretsiz yükleyin

1. SSH Hdınsight kümesine bağlanın. Örneğin, aşağıdaki komutu adlı bir küme için birincil headnode bağlayan `mycluster`:

    ```bash
    ssh CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Ücretsiz yüklemek için aşağıdaki komutu kullanın:

    ```bash
    sudo apt --assume-yes install freetds-dev freetds-bin
    ```

    Ücretsiz birkaç adımda SQL veritabanına bağlanmak için kullanılır.

## <a name="create-the-table-in-sql-database"></a>SQL veritabanı tablosu oluşturma

> [!IMPORTANT]
> Hdınsight kümesi kullanıyorsanız ve SQL veritabanı oluşturuldu [küme ve SQL veritabanı oluşturma](hdinsight-use-sqoop.md), bu bölümdeki adımları atlayın. Veritabanı ve tablo adımlarda bir parçası olarak oluşturulan [küme ve SQL veritabanı oluşturma](hdinsight-use-sqoop.md) belge.

1. SSH oturumundan SQL veritabanı sunucusuna bağlanmak için aşağıdaki komutu kullanın.

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Aşağıdakine benzer bir çıktı alırsınız:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

2. Konumundaki `1>` isteminde, aşağıdaki sorguyu girin:

    ```sql
    CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder] [bigint])
    GO
    CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)
    GO
    ```

    Zaman `GO` deyimi girilir, önceki deyimleri değerlendirilir. İlk olarak, **mobiledata** tablosu oluşturulur ve kümelenmiş bir dizin (SQL veritabanı tarafından gereklidir.) kendisine eklenir

    Tablo oluşturulduğunu doğrulamak için aşağıdaki sorguyu kullanın:

    ```sql
    SELECT * FROM information_schema.tables
    GO
    ```

    Aşağıdakine benzer bir çıktı görürsünüz:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        sqooptest       dbo     mobiledata      BASE TABLE

3. Girin `exit` adresindeki `1>` tsql yardımcı programı'ndan çıkmak komut istemi.

## <a name="sqoop-export"></a>Sqoop dışarı aktarma

1. Kümeye SSH bağlantısı Sqoop SQL veritabanınız görebildiğini doğrulamak için aşağıdaki komutu kullanın:

    ```bash
    sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> -P
    ```
    İstendiğinde, SQL veritabanı oturum açma için parola girin.

    Bu komut veritabanları dahil olmak üzere, bir listesini döndürür **sqooptest** daha önce oluşturduğunuz veritabanı.

2. Verileri dışarı aktarmak için **hivesampletable** için **mobiledata** tablo, aşağıdaki komutu kullanın:

    ```bash
    sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> -P --table 'mobiledata' --export-dir 'wasb:///hive/warehouse/hivesampletable' --fields-terminated-by '\t' -m 1
    ```

    Bu komut bağlanmak için Sqoop bildirir **sqooptest** veritabanı. Sqoop sonra verileri dışa aktarır **wasb: / / / hive/ambarı/hivesampletable** için **mobiledata** tablo.

    > [!IMPORTANT]
    > Kullanım `wasb:///` kümeniz için varsayılan depolama bir Azure depolama hesabı ise. Kullanım `adl:///` bir Azure Data Lake Store ise.

3. Komut tamamlandığında, TSQL kullanılarak veritabanına bağlanmak için aşağıdaki komutu kullanın:

    ```bash
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P -p 1433 -D sqooptest
    ```

    Bağlantı kurulduktan sonra verileri dışarı aktarılan doğrulamak için aşağıdaki ifadeleri kullanın **mobiledata** tablosu:

    ```sql
    SET ROWCOUNT 50;
    SELECT * FROM mobiledata
    GO
    ```

    Tablosunda veri listesini görmelisiniz. Tür `exit` tsql yardımcı programı'ndan çıkmak için.

## <a name="sqoop-import"></a>Sqoop alma

1. Verileri içe aktarmak için aşağıdaki komutu kullanın **mobiledata** SQL veritabanında çok tablo **wasb: / / / öğreticileri/usesqoop/importeddata** hdınsight'ta dizin:

    ```bash
    sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

    Veri alanları bir sekme karakteriyle ayrılır ve satırları yeni satır karakteri tarafından sonlandırılır.

2. Alma işlemi tamamlandıktan sonra yeni bir dizinde liste veri çıkışı için aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -text /tutorials/usesqoop/importeddata/part-m-00000
    ```

## <a name="using-sql-server"></a>SQL Server kullanma

Sqoop, veri merkezinizdeki veya Azure üzerinde barındırılan bir sanal makinede SQL Server'dan veri vermek ve almak için de kullanabilirsiniz. SQL Database ve SQL Server kullanımı arasındaki farklılıklar şunlardır:

* Hdınsight ve SQL Server aynı Azure sanal ağ üzerinde olmalıdır.

    Bir örnek için bkz: [şirket içi ağınıza bağlanmak Hdınsight](./connect-on-premises-network.md) belge.

    Hdınsight Azure sanal ağı ile kullanma hakkında daha fazla bilgi için bkz: [genişletmek Hdınsight Azure sanal ağ ile](hdinsight-extend-hadoop-virtual-network.md) belge. Azure sanal ağ ile ilgili daha fazla bilgi için bkz: [Virtual Network'e genel bakış](../virtual-network/virtual-networks-overview.md) belge.

* SQL Server, SQL kimlik doğrulaması izin verecek şekilde yapılandırılmalıdır. Daha fazla bilgi için bkz: [bir kimlik doğrulama modu seçme](https://msdn.microsoft.com/ms144284.aspx) belge.

* SQL Server'ın uzak bağlantıları kabul edecek şekilde yapılandırmanız gerekebilir. Daha fazla bilgi için bkz: [SQL Server veritabanı altyapısına bağlanma ile ilgili sorunları giderme](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) belge.

* Oluşturma **sqooptest** gibi bir yardımcı programını kullanarak SQL Server veritabanında **SQL Server Management Studio** veya **tsql**. Azure CLI kullanarak için adımlar, yalnızca Azure SQL veritabanı için geçerlidir.

    Oluşturmak için aşağıdaki Transact-SQL deyimi kullanın **mobiledata** tablosu:

    ```sql
    CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder] [bigint])
    ```

* SQL Server'a Hdınsight'ta bağlanırken, SQL Server'ın IP adresi kullanmak zorunda kalabilirsiniz. Örneğin:

    ```bash
    sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

## <a name="limitations"></a>Sınırlamalar

* Toplu export - ile Linux tabanlı Hdınsight, Microsoft SQL Server veya Azure SQL veritabanı için verileri dışa aktarmak için kullanılan Sqoop bağlayıcı toplu eklemeler şu anda desteklemiyor.

* -Kullanırken, Linux tabanlı Hdınsight ile toplu işleme `-batch` eklemeleri gerçekleştirirken geçiş, Sqoop yapar INSERT işlemlerine toplu işleme yerine birden çok ekler.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi Sqoop kullanma öğrendiniz. Daha fazla bilgi için bkz:

* [Hdınsight ile Oozie kullanma][hdinsight-use-oozie]: Oozie iş akışında kullanmak Sqoop eylem.
* [Hdınsight kullanarak uçuş gecikme verileri analiz][hdinsight-analyze-flight-data]: uçuş çözümlemek için kullanmak Hive gecikme veri ve bir Azure SQL veritabanına veri vermek için Sqoop kullanın.
* [Verileri Hdınsight'a yükleme][hdinsight-upload-data]: Hdınsight/Azure Blob depolama alanına veri yüklemek için diğer yöntemler bulun.

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database-create-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
