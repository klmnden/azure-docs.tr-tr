---
title: Azure HDInsight ile etkileşimli Sorgu'yu kullanma
description: HDInsight ile etkileşimli sorgu (LLAP Hive'ı) kullanmayı öğrenin.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/07/2019
ms.openlocfilehash: 9636157182e8b40914bde2515c5b295d0480255a
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65510992"
---
# <a name="use-interactive-query-with-hdinsight"></a>HDInsight ile etkileşimli Sorgu'yu kullanma
Etkileşimli sorgu (Hive, Apache LLAP olarak da adlandırılan veya [düşük gecikme süresi analitik işleme](https://cwiki.apache.org/confluence/display/Hive/LLAP)) olan Azure HDInsight [küme türü](../hdinsight-hadoop-provision-linux-clusters.md#cluster-types). Etkileşimli sorgu bellek içi önbelleğe alma, Apache Hive sorguları daha hızlı ve daha etkileşimli getiren destekler.

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)] 

Etkileşimli sorgu kümesi, bir Apache Hadoop kümesinden farklıdır. Bu, yalnızca Hive hizmeti içerir. 

> [!NOTE]  
> Etkileşimli sorgu kümesi Apache Ambari Hive görünümünü, Beeline ve Microsoft Hive açık veritabanı bağlantısı sürücü (Hive ODBC) aracılığıyla yalnızca Hive hizmete erişebilir. Hive konsolunu, templeton da, Klasik Azure CLI veya Azure PowerShell erişemez. 

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
* Bilgi edinmek için nasıl [Azure HDInsight Apache Hive sorguları çalıştırmak için Apache Zeppelin kullanma](../hdinsight-connect-hive-zeppelin.md).
* Bilgi edinmek için nasıl [Visual Studio için Data Lake Araçları'nı kullanarak Apache Hive sorguları çalıştırma](../hadoop/apache-hadoop-visual-studio-tools-get-started.md#run-interactive-apache-hive-queries).
* Bilgi edinmek için nasıl [Visual Studio Code için HDInsight araçlarını kullanma](../hdinsight-for-vscode.md).
* Bilgi edinmek için nasıl [HDInsight, Apache Hadoop ile Apache Hive görünümünü kullanma](../hadoop/apache-hadoop-use-hive-ambari-view.md)
* Bilgi edinmek için nasıl [HDInsight, Apache Hive sorguları göndermek için Beeline kullanma](../hadoop/apache-hadoop-use-hive-beeline.md).
* Bilgi edinmek için nasıl [Excel'i Microsoft Hive ODBC sürücüsü ile Apache Hadoop'a bağlama](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).

