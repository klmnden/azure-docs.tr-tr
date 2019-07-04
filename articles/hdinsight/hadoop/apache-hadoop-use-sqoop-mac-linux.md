---
title: Apache Sqoop Apache Hadoop - Azure HDInsight ile
description: HDInsight üzerinde Apache Hadoop ve Azure SQL veritabanı arasında alma ve verme için Apache Sqoop'u kullanma konusunda bilgi edinin.
keywords: hadoop sqoop, sqoop
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 04/15/2019
ms.openlocfilehash: 02b4eb6c367510e8994aa7723fe3fdd3e43af0e6
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67462172"
---
# <a name="use-apache-sqoop-to-import-and-export-data-between-apache-hadoop-on-hdinsight-and-sql-database"></a>HDInsight üzerinde Apache Hadoop ile SQL veritabanı arasında verileri dışarı aktarma ve içeri aktarmak için Apache Sqoop'u kullanma

[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Azure HDInsight, Apache Hadoop kümesi ve Azure SQL veritabanı ya da Microsoft SQL Server veritabanı arasında alma ve verme için Apache Sqoop'u kullanma konusunda bilgi edinin. Bu adımları belge kullanım `sqoop` doğrudan Hadoop küme baş düğümüne komutu. Baş düğüme bağlanmak ve bu belgede komutları çalıştırmak için SSH kullanın. Bu makalede devamı niteliğindedir [HDInsight, Hadoop ile Apache Sqoop'u kullanma](./hdinsight-use-sqoop.md).

## <a name="prerequisites"></a>Önkoşullar

* Tamamlanmasından [test ortamını ayarlama](./hdinsight-use-sqoop.md#create-cluster-and-sql-database) gelen [HDInsight, Hadoop ile Apache Sqoop'u kullanma](./hdinsight-use-sqoop.md).

* Azure SQL veritabanını sorgulamak için bir istemci. Kullanmayı [SQL Server Management Studio](../../sql-database/sql-database-connect-query-ssms.md) veya [Visual Studio Code](../../sql-database/sql-database-connect-query-vscode.md).

* Bir SSH istemcisi. Daha fazla bilgi için [SSH kullanarak HDInsight (Apache Hadoop) bağlanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="sqoop-export"></a>Sqoop dışarı aktarma

SQL Server kovanından.

1. HDInsight kümesine bağlanmak için SSH kullanın. Değiştirin `CLUSTERNAME` değerini kümenizin adıyla, sonra komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

2. Değiştirin `MYSQLSERVER` SQL sunucunuzun adı ile. SQL veritabanınız Sqoop görebildiğini doğrulamak için açık olan SSH bağlantınızı içinde aşağıdaki komutu girin. SQL Server oturumu için istendiğinde parolayı girin. Bu komut, veritabanlarının listesini döndürür.

    ```bash
    sqoop list-databases --connect jdbc:sqlserver://MYSQLSERVER.database.windows.net:1433 --username sqluser -P
    ```

3. Değiştirin `MYSQLSERVER` SQL sunucunuzun adı ile ve `MYDATABASE` SQL veritabanınızın adı ile. Kovanından verilerini dışarı aktarmaya yönelik `hivesampletable` tablo `mobiledata` tablo SQL veritabanı'nda, açık olan SSH bağlantınızı içinde aşağıdaki komutu girin. SQL Server oturumu için istendiğinde parolayı girin

    ```bash
    sqoop export --connect 'jdbc:sqlserver://MYSQLSERVER.database.windows.net:1433;database=MYDATABASE' --username sqluser -P -table 'mobiledata' --hcatalog-table hivesampletable
    ```

4. Verileri dışarı aktarılan doğrulamak için dışarı aktarılan verileri görüntülemek için aşağıdaki sorguları SQL istemcisinden kullanın:

    ```sql
    SELECT COUNT(*) FROM [dbo].[mobiledata] WITH (NOLOCK);
    SELECT TOP(25) * FROM [dbo].[mobiledata] WITH (NOLOCK);
    ```

## <a name="sqoop-import"></a>Sqoop alma

Azure depolama için SQL Server.

1. Değiştirin `MYSQLSERVER` SQL sunucunuzun adı ile ve `MYDATABASE` SQL veritabanınızın adı ile. Açık SSH bağlantınızı verileri içe aktarmak için aşağıdaki komutu girin `mobiledata` için SQL veritabanı'nda Tablo `wasb:///tutorials/usesqoop/importeddata` HDInsight üzerinde dizin. SQL Server oturumu için istendiğinde parolayı girin. Veri alanları bir sekme karakteriyle ayrılır ve satırların bir yeni satır karakteri tarafından sonlandırılır.

    ```bash
    sqoop import --connect 'jdbc:sqlserver://MYSQLSERVER.database.windows.net:1433;database=MYDATABASE' --username sqluser -P --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

2. İçeri aktarma tamamlandıktan sonra aşağıdaki komutu listesine yeni dizine verileri kullanıma açık SSH bağlantınızı girin:

    ```bash
    hdfs dfs -text /tutorials/usesqoop/importeddata/part-m-00000
    ```

## <a name="limitations"></a>Sınırlamalar

* Toplu ekleme toplu Dışarı Aktar - ile Linux tabanlı HDInsight, Microsoft SQL Server veya Azure SQL veritabanı için verileri dışarı aktarmak için kullanılan Sqoop bağlayıcısının desteklemez.

* -Kullanırken, Linux tabanlı HDInsight ile toplu işleme `-batch` ekler gerçekleştirirken geçiş, Sqoop toplu INSERT işlemler yerine birden çok ekleme yapar.

## <a name="important-considerations"></a>Önemli noktalar

* HDInsight hem de SQL Server aynı Azure sanal ağ üzerinde olmalıdır.

    Bir örnek için bkz. [HDInsight'ı şirket içi ağınıza bağlama](./../connect-on-premises-network.md) belge.

    HDInsight ile Azure sanal ağı kullanma hakkında daha fazla bilgi için bkz. [HDInsight'ı Azure sanal ağ ile genişletme](../hdinsight-extend-hadoop-virtual-network.md) belge. Azure sanal ağ hakkında daha fazla bilgi için bkz. [sanal ağa genel bakış](../../virtual-network/virtual-networks-overview.md) belge.

* SQL Server, SQL kimlik doğrulaması izin verecek şekilde yapılandırılmalıdır. Daha fazla bilgi için [bir kimlik doğrulama modu seçme](https://msdn.microsoft.com/ms144284.aspx) belge.

* SQL Server'ın uzak bağlantıları kabul edecek şekilde yapılandırmanız gerekebilir. Daha fazla bilgi için [SQL Server veritabanı altyapısına bağlanma sorunlarını giderme](https://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) belge.

## <a name="next-steps"></a>Sonraki adımlar

Artık Sqoop kullanmayı öğrendiniz. Daha fazla bilgi için bkz:

* [HDInsight ile Apache Oozie kullanma](../hdinsight-use-oozie-linux-mac.md): İçinde bir Oozie iş akışının Sqoop eylemini kullanın.
* [HDInsight'ı kullanarak uçuş gecikme verilerini çözümleme](../interactive-query/interactive-query-tutorial-analyze-flight-data.md): Uçuş gecikme verilerini çözümleme için etkileşimli sorgu kullanın ve ardından bir Azure SQL veritabanına veri dışarı aktarmak için Sqoop kullanma.
* [HDInsight için verileri karşıya](../hdinsight-upload-data.md): HDInsight/Azure Blob depolama alanına veri yüklemek için diğer yöntemler bulun.
