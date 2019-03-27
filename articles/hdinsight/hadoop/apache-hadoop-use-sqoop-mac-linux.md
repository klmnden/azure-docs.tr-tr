---
title: Apache Sqoop Apache Hadoop - Azure HDInsight ile
description: HDInsight üzerinde Apache Hadoop ve Azure SQL veritabanı arasında alma ve verme için Apache Sqoop'u kullanma konusunda bilgi edinin.
keywords: hadoop sqoop, sqoop
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 02/15/2019
ms.openlocfilehash: 0f96ee3a24a811d7e3ab7e65ba4ff14050541638
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58443371"
---
# <a name="use-apache-sqoop-to-import-and-export-data-between-apache-hadoop-on-hdinsight-and-sql-database"></a>HDInsight üzerinde Apache Hadoop ile SQL veritabanı arasında verileri dışarı aktarma ve içeri aktarmak için Apache Sqoop'u kullanma

[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Azure HDInsight, Apache Hadoop kümesi ve Azure SQL veritabanı ya da Microsoft SQL Server veritabanı arasında alma ve verme için Apache Sqoop'u kullanma konusunda bilgi edinin. Bu adımları belge kullanım `sqoop` doğrudan Hadoop küme baş düğümüne komutu. Baş düğüme bağlanmak ve bu belgede komutları çalıştırmak için SSH kullanın.

> [!IMPORTANT]  
> Bu belgede yer alan adımlar, yalnızca Linux kullanan HDInsight kümeleri ile çalışır. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

> [!WARNING]  
> Bu belgede yer alan adımlar, adlı bir Azure SQL veritabanı zaten oluşturduğunuzu varsayalım `sqooptest`.
>
> Bu belge, oluşturmak ve SQL veritabanı'nda bir tabloyu sorgulamak için kullanılan bir T-SQL bildirimleri sağlar. SQL veritabanı ile bu deyimler kullanabileceğiniz birçok istemciler vardır. Aşağıdaki istemciler öneririz:
>
> * [SQL Server Management Studio](../../sql-database/sql-database-connect-query-ssms.md)
> * [Visual Studio Code](../../sql-database/sql-database-connect-query-vscode.md)
> * [Sqlcmd](https://docs.microsoft.com/sql/tools/sqlcmd-utility) yardımcı programı

## <a name="create-the-table-in-sql-database"></a>SQL veritabanı'nda Tablo oluşturma

> [!IMPORTANT]  
> HDInsight kümesi kullanırsınız ve oluşturduğunuz SQL veritabanı [küme ve SQL veritabanı oluşturma](hdinsight-use-sqoop.md), bu bölümdeki adımları atlayın. Tablo ve veritabanı'ndaki adımları bir parçası olarak oluşturulan [küme ve SQL veritabanı oluşturma](hdinsight-use-sqoop.md) belge.

Bir SQL istemcisi, bağlanmak için kullandığınız `sqooptest` , SQL veritabanı. Ardından adlı bir tablo oluşturmak için aşağıdaki T-SQL kullanma `mobiledata`:

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

1. HDInsight kümesine bağlanmak için SSH kullanın. Örneğin, aşağıdaki komut birincil adlı bir küme baş düğümüne bağlanır `mycluster`:

    ```bash
    ssh mycluster-ssh.azurehdinsight.net
    ```

    Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. SQL veritabanınız Sqoop görebildiğini doğrulamak için aşağıdaki komutu kullanın:

    ```bash
    sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> -P
    ```
    İstendiğinde, SQL veritabanı oturum açma için parola girin.

    Bu komut dahil olmak üzere, veritabanlarının listesini döndürür **sqooptest** daha önce kullanılan veritabanı.

3. Kovanından verilerini dışarı aktarmaya yönelik **hivesampletable** tablo **mobiledata** tablo SQL veritabanı'nda, aşağıdaki komutu kullanın:

    ```bash
    sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> -P -table 'mobiledata' --hcatalog-table hivesampletable
    ```

4. Verileri dışarı aktarılan doğrulamak için dışarı aktarılan verileri görüntülemek için SQL istemcinizden aşağıdaki sorguyu kullanın:

    ```sql
    SET ROWCOUNT 50;
    SELECT * FROM mobiledata;
    ```

    Bu komut tablosu içine alınan 50 satır listeler.

## <a name="sqoop-import"></a>Sqoop alma

1. Verileri içe aktarmak için aşağıdaki komutu kullanın **mobiledata** için SQL veritabanı'nda Tablo **wasb: / / / öğreticiler/usesqoop/importeddata** HDInsight üzerinde dizin:

    ```bash
    sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> -P --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

    Veri alanları bir sekme karakteriyle ayrılır ve satırların bir yeni satır karakteri tarafından sonlandırılır.

    > [!IMPORTANT]  
    > `wasb:///` Yolu varsayılan küme depolama alanı olarak Azure depolama kullanan kümeler ile birlikte çalışır. Azure Data Lake depolama Gen2 kullanan kümeler için kullanma `abfs:///` yerine. Azure Data Lake depolama Gen1 kullanan kümeler için kullanma `adl:///` yerine.

2. İçeri aktarma tamamlandıktan sonra yeni dizine veri listesini aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -text /tutorials/usesqoop/importeddata/part-m-00000
    ```

## <a name="using-sql-server"></a>SQL Server kullanma

Sqoop, verileri SQL Server'dan dışarı ve içeri aktarmak için de kullanabilirsiniz. SQL veritabanı ve SQL Server'ın kullanımı arasındaki farklılıklar şunlardır:

* HDInsight hem de SQL Server aynı Azure sanal ağ üzerinde olmalıdır.

    Bir örnek için bkz. [HDInsight'ı şirket içi ağınıza bağlama](./../connect-on-premises-network.md) belge.

    HDInsight ile Azure sanal ağı kullanma hakkında daha fazla bilgi için bkz. [HDInsight'ı Azure sanal ağ ile genişletme](../hdinsight-extend-hadoop-virtual-network.md) belge. Azure sanal ağ hakkında daha fazla bilgi için bkz. [sanal ağa genel bakış](../../virtual-network/virtual-networks-overview.md) belge.

* SQL Server, SQL kimlik doğrulaması izin verecek şekilde yapılandırılmalıdır. Daha fazla bilgi için [bir kimlik doğrulama modu seçme](https://msdn.microsoft.com/ms144284.aspx) belge.

* SQL Server'ın uzak bağlantıları kabul edecek şekilde yapılandırmanız gerekebilir. Daha fazla bilgi için [SQL Server veritabanı altyapısına bağlanma sorunlarını giderme](https://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) belge.

* Oluşturmak için aşağıdaki Transact-SQL deyimlerini kullanın **mobiledata** tablosu:

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

* HDInsight SQL Server'a bağlanma, SQL Server'ın IP adresi kullanmak zorunda kalabilirsiniz. Örneğin:

    ```bash
    sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> -P -table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

## <a name="limitations"></a>Sınırlamalar

* Toplu ekleme toplu Dışarı Aktar - ile Linux tabanlı HDInsight, Microsoft SQL Server veya Azure SQL veritabanı için verileri dışarı aktarmak için kullanılan Sqoop bağlayıcısının desteklemez.

* -Kullanırken, Linux tabanlı HDInsight ile toplu işleme `-batch` ekler gerçekleştirirken geçiş, Sqoop toplu INSERT işlemler yerine birden çok ekleme yapar.

## <a name="next-steps"></a>Sonraki adımlar

Artık Sqoop kullanmayı öğrendiniz. Daha fazla bilgi için bkz:

* [HDInsight ile Apache Oozie kullanma](../hdinsight-use-oozie-linux-mac.md): İçinde bir Oozie iş akışının Sqoop eylemini kullanın.
* [HDInsight'ı kullanarak uçuş gecikme verilerini çözümleme](../hdinsight-analyze-flight-delay-data-linux.md): Uçuş gecikme verilerini çözümleme için Apache Hive'ı kullanın ve ardından bir Azure SQL veritabanına veri dışarı aktarmak için Sqoop kullanma.
* [HDInsight için verileri karşıya](../hdinsight-upload-data.md): HDInsight/Azure Blob depolama alanına veri yüklemek için diğer yöntemler bulun.

[hdinsight-versions]:  ../hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]:apache-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[sqldatabase-get-started]: ../sql-database-get-started.md

[powershell-start]: https://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: https://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
