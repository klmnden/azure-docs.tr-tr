---
title: Apache Sqoop Hadoop - Azure Hdınsight ile | Microsoft Docs
description: Apache Sqoop hdınsight'ta Hadoop ve bir Azure SQL veritabanı arasında almak ve vermek için nasıl kullanılacağını öğrenin.
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: ''
author: Blackmist
tags: azure-portal
keywords: hadoop sqoop, sqoop
ms.assetid: 303649a5-4be5-4933-bf1d-4b232083c354
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: larryfr
ms.openlocfilehash: 38b1c57b9ac666bc908df69b29f72fbd5aea0495
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="use-apache-sqoop-to-import-and-export-data-between-hadoop-on-hdinsight-and-sql-database"></a>İçeri ve dışarı aktarma hdınsight'ta Hadoop ile SQL veritabanı arasında veri için Apache Sqoop'u kullanın

[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Apache Sqoop Azure hdınsight'ta Hadoop kümesi ve Azure SQL Database veya Microsoft SQL Server veritabanı arasında almak ve vermek için nasıl kullanılacağını öğrenin. Bu adımlarda belge kullanımı `sqoop` Hadoop kümesi headnode doğrudan komutu. SSH baş düğümüne bağlanmak ve bu belgede komutları çalıştırmak için kullanın.

> [!IMPORTANT]
> Bu belgede yer alan adımlar, yalnızca Linux kullanmak Hdınsight kümeleri ile çalışır. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

> [!WARNING]
> Bu belgede yer alan adımlar adlı bir Azure SQL veritabanı zaten oluşturduğunuzu varsayalım `sqooptest`.
>
> Bu belge oluşturmak ve SQL veritabanındaki bir tabloyu sorgulamak için kullanılan T-SQL deyimlerini sağlar. SQL veritabanı ile bu deyimleri kullanabileceğiniz birçok istemciler vardır. Aşağıdaki istemciler öneririz:
>
> * [SQL Server Management Studio](../../sql-database/sql-database-connect-query-ssms.md)
> * [Visual Studio Code](../../sql-database/sql-database-connect-query-vscode.md)
> * [Sqlcmd](https://docs.microsoft.com/sql/tools/sqlcmd-utility) yardımcı programı

## <a name="create-the-table-in-sql-database"></a>SQL veritabanı tablosu oluşturma

> [!IMPORTANT]
> Hdınsight kümesi kullanıyorsanız ve SQL veritabanı oluşturuldu [küme ve SQL veritabanı oluşturma](hdinsight-use-sqoop.md), bu bölümdeki adımları atlayın. Veritabanı ve tablo adımlarda bir parçası olarak oluşturulan [küme ve SQL veritabanı oluşturma](hdinsight-use-sqoop.md) belge.

SQL istemci bağlanmak için kullandığı `sqooptest` SQL veritabanınız veritabanında. Adlı bir tablo oluşturmak için aşağıdaki T-SQL kullanın `mobiledata`:

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

## <a name="sqoop-export"></a>Sqoop dışarı aktarma

1. SSH Hdınsight kümesine bağlanın. Örneğin, aşağıdaki komutu adlı bir küme için birincil headnode bağlayan `mycluster`:

    ```bash
    ssh mycluster-ssh.azurehdinsight.net
    ```

    Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Sqoop SQL veritabanınız görebildiğini doğrulamak için aşağıdaki komutu kullanın:

    ```bash
    sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> -P
    ```
    İstendiğinde, SQL veritabanı oturum açma için parola girin.

    Bu komut veritabanları dahil olmak üzere, bir listesini döndürür **sqooptest** daha önce kullanılan veritabanı.

3. Veri kovanından dışarı aktarmak için **hivesampletable** tablosu **mobiledata** tablo SQL veritabanı'nda, aşağıdaki komutu kullanın:

    ```bash
    sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> -P -table 'mobiledata' --hcatalog-table hivesampletable
    ```

4. Verileri dışarı aktarılan doğrulamak için dışarı aktarılan verileri görüntülemek için aşağıdaki sorguyu SQL istemcisinden kullanın:

    ```sql
    SET ROWCOUNT 50;
    SELECT * FROM mobiledata;
    ```

    Bu komut tabloya aktarılan 50 satırları listeler.

## <a name="sqoop-import"></a>Sqoop alma

1. Verileri içe aktarmak için aşağıdaki komutu kullanın **mobiledata** SQL veritabanında çok tablo **wasb: / / / öğreticileri/usesqoop/importeddata** hdınsight'ta dizin:

    ```bash
    sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> -P --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

    Veri alanları bir sekme karakteriyle ayrılır ve satırları yeni satır karakteri tarafından sonlandırılır.

    > [!IMPORTANT]
    > `wasb:///` Yolu, varsayılan küme depolama alanı olarak Azure depolama kullanan bir küme ile çalışır. Azure Data Lake Store kullanma kümeleri kullanan `adl:///` yerine.

2. Alma işlemi tamamlandıktan sonra yeni bir dizinde liste veri çıkışı için aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -text /tutorials/usesqoop/importeddata/part-m-00000
    ```

## <a name="using-sql-server"></a>SQL Server kullanma

Sqoop, alabilir ve verileri SQL Server'dan dışarı aktarmak için de kullanabilirsiniz. SQL Database ve SQL Server kullanımı arasındaki farklılıklar şunlardır:

* Hdınsight ve SQL Server aynı Azure sanal ağ üzerinde olmalıdır.

    Bir örnek için bkz: [şirket içi ağınıza bağlanmak Hdınsight](./../connect-on-premises-network.md) belge.

    Hdınsight Azure sanal ağı ile kullanma hakkında daha fazla bilgi için bkz: [genişletmek Hdınsight Azure sanal ağ ile](../hdinsight-extend-hadoop-virtual-network.md) belge. Azure sanal ağ ile ilgili daha fazla bilgi için bkz: [Virtual Network'e genel bakış](../../virtual-network/virtual-networks-overview.md) belge.

* SQL Server, SQL kimlik doğrulaması izin verecek şekilde yapılandırılmalıdır. Daha fazla bilgi için bkz: [bir kimlik doğrulama modu seçme](https://msdn.microsoft.com/ms144284.aspx) belge.

* SQL Server'ın uzak bağlantıları kabul edecek şekilde yapılandırmanız gerekebilir. Daha fazla bilgi için bkz: [SQL Server veritabanı altyapısına bağlanma ile ilgili sorunları giderme](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) belge.

* Oluşturmak için aşağıdaki Transact-SQL deyimi kullanın **mobiledata** tablosu:

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
    sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> -P -table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

## <a name="limitations"></a>Sınırlamalar

* Toplu export - ile Linux tabanlı Hdınsight, Microsoft SQL Server veya Azure SQL veritabanı için verileri dışa aktarmak için kullanılan Sqoop bağlayıcı toplu eklemeler desteklemez.

* -Kullanırken, Linux tabanlı Hdınsight ile toplu işleme `-batch` eklemeleri gerçekleştirirken geçiş, Sqoop yapar INSERT işlemlerine toplu işleme yerine birden çok ekler.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi Sqoop kullanma öğrendiniz. Daha fazla bilgi için bkz:

* [Hdınsight ile Oozie kullanma](../hdinsight-use-oozie.md): Oozie iş akışında kullanmak Sqoop eylem.
* [Hdınsight kullanarak uçuş gecikme verileri analiz](../hdinsight-analyze-flight-delay-data.md): uçuş çözümlemek için kullanmak Hive gecikme veri ve bir Azure SQL veritabanına veri vermek için Sqoop kullanın.
* [Verileri Hdınsight'a yükleme](../hdinsight-upload-data.md): Hdınsight/Azure Blob depolama alanına veri yüklemek için diğer yöntemler bulun.

[hdinsight-versions]:  ../hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]:apache-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[sqldatabase-get-started]: ../sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database-create-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
