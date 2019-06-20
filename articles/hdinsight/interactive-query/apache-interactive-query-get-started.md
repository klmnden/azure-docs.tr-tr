---
title: Azure HDInsight etkileşimli sorgu nedir?
description: Etkileşimli sorgu, Azure HDInsight giriş
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: overview
ms.date: 06/14/2019
ms.openlocfilehash: ea17ddeab21c371f41cc57115df4dd91277c3c42
ms.sourcegitcommit: 6e6813f8e5fa1f6f4661a640a49dc4c864f8a6cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67151190"
---
# <a name="what-is-interactive-query-in-azure-hdinsight"></a>Etkileşimli sorgu, Azure HDInsight nedir

Etkileşimli sorgu (Hive, Apache LLAP olarak da adlandırılan veya [düşük gecikme süresi analitik işleme](https://cwiki.apache.org/confluence/display/Hive/LLAP)) olan Azure HDInsight [küme türü](../hdinsight-hadoop-provision-linux-clusters.md#cluster-types). Etkileşimli sorgu bellek içi önbelleğe alma, Apache Hive sorguları daha hızlı ve daha etkileşimli getiren destekler. Müşteriler, süper hızlı bir şekilde Azure depolama ve Azure Data Lake Store içinde depolanan verileri sorgulamak için etkileşimli sorgu kullanır. Etkileşimli sorgu sevdikleri BI araçlarını kullanarak büyük verilerle çalışmak için geliştiriciler ve veri uzmanı en kolaylaştırır. HDInsight etkileşimli sorgu büyük verilere kolay bir şekilde erişmek için çeşitli araçlar destekler.

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)]

Etkileşimli sorgu kümesi, bir Apache Hadoop kümesinden farklıdır. Bu, yalnızca Hive hizmeti içerir.

Etkileşimli sorgu kümesi Apache Ambari Hive görünümünü, Beeline ve Microsoft Hive açık veritabanı bağlantısı sürücü (Hive ODBC) aracılığıyla yalnızca Hive hizmete erişebilir. Hive konsolunu, templeton da, Klasik Azure CLI veya Azure PowerShell erişemez.

## <a name="create-an-interactive-query-cluster"></a>Etkileşimli sorgu kümesi oluşturma

Bir HDInsight kümesi oluşturma hakkında daha fazla bilgi için bkz: [Apache Hadoop kümeleri oluşturma HDInsight](../hdinsight-hadoop-provision-linux-clusters.md). Etkileşimli sorgu kümesi türünü seçin.

## <a name="execute-apache-hive-queries-from-interactive-query"></a>Apache Hive sorguları, etkileşimli sorgu yürütme

Hive sorguları çalıştırmak için aşağıdaki seçenekleriniz vardır:

* Microsoft Power BI kullanın

    Bkz: [Power BI'da Azure HDInsight ile verileri görselleştirme etkileşimli Apache Hive sorgusu](./apache-hadoop-connect-hive-power-bi-directquery.md) bkz [Power BI'da Azure HDInsight ile büyük verileri görselleştirme](../hadoop/apache-hadoop-connect-hive-power-bi.md).

* Visual Studio'yu kullanma

    Bkz: [bağlanın Azure HDInsight ve Visual Studio için Data Lake Araçları'nı kullanarak çalışma Apache Hive sorguları](../hadoop/apache-hadoop-visual-studio-tools-get-started.md#run-interactive-apache-hive-queries).

* Visual Studio Code'u kullanma

    Bkz: [kullanım Visual Studio Code için Apache Hive, LLAP veya pySpark](../hdinsight-for-vscode.md).
* Apache Hive, Apache Ambari Hive görünümünü kullanarak çalıştırın.
  
    Bkz: [Azure HDInsight, Apache Hadoop ile Apache Hive görünümünü kullanma](../hadoop/apache-hadoop-use-hive-ambari-view.md).

* Apache Hive, Beeline'ı kullanarak çalıştırın.
  
    Bkz: [Beeline ile HDInsight, Apache Hadoop ile Apache Hive kullanma](../hadoop/apache-hadoop-use-hive-beeline.md).
  
    Beeline baş düğümünden veya boş bir kenar düğümünü kullanabilirsiniz. Bir boş kenar düğümünü Beeline kullanmanızı öneririz. Boş bir kenar düğümünü kullanarak bir HDInsight kümesi oluşturma hakkında daha fazla bilgi için bkz: [HDInsight içinde boş kenar düğümlerini kullanma](../hdinsight-apps-use-edge-node.md).
* Apache Hive, Hive ODBC kullanarak çalıştırın.
  
    Bkz: [Microsoft Hive ODBC sürücüsü ile Apache Hadoop Excel'e bağlanma](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).

Java veritabanı bağlantısı (JDBC) bağlantı dizesi bulmak için:

1. Apache Ambari aşağıdaki URL'yi kullanarak oturum açın: `https://<cluster name>.AzureHDInsight.net`.
2. Sol menüde **Hive**.
3. URL'yi kopyalamak için Pano simgesini seçin:

   ![HDInsight Hadoop etkileşimli sorgu LLAP JDBC](./media/apache-interactive-query-get-started/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi nasıl [içinde HDInsight etkileşimli sorgu kümelerine oluşturma](../hdinsight-hadoop-provision-linux-clusters.md).
* Bilgi edinmek için nasıl [Power BI'da Azure HDInsight ile büyük verileri görselleştirme](../hadoop/apache-hadoop-connect-hive-power-bi.md).
* Bilgi edinmek için nasıl [Azure HDInsight Apache Hive sorguları çalıştırmak için Apache Zeppelin kullanma](../interactive-query/hdinsight-connect-hive-zeppelin.md).
