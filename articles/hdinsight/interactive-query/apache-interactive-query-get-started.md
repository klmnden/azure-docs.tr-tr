---
title: Azure HDInsight ile etkileşimli Sorgu'yu kullanma
description: HDInsight ile etkileşimli sorgu (LLAP Hive'ı) kullanmayı öğrenin.
services: hdinsight
ms.service: hdinsight
author: jasonwhowell
ms.author: jasonh
editor: jasonwhowell
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/22/2018
ms.openlocfilehash: 73f7e523ed0abc7d0453096cf783761dd6a884ba
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39628673"
---
# <a name="use-interactive-query-with-hdinsight"></a>HDInsight ile etkileşimli Sorgu'yu kullanma
Etkileşimli sorgu (Hive, LLAP olarak da adlandırılan veya [düşük gecikme süresi analitik işleme](https://cwiki.apache.org/confluence/display/Hive/LLAP)) olan Azure HDInsight [küme türü](../hdinsight-hadoop-provision-linux-clusters.md#cluster-types). Etkileşimli sorgu bellek içi önbelleğe alma, daha hızlı ve daha etkileşimli Hive sorguları getiren destekler.

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)] 

Etkileşimli sorgu kümesi, bir Hadoop kümesi ' farklıdır. Bu, yalnızca Hive hizmeti içerir. 

> [!NOTE]
> Etkileşimli sorgu kümesi Ambari Hive görünümünü, Beeline ve Microsoft Hive açık veritabanı bağlantısı sürücü (Hive ODBC) aracılığıyla yalnızca Hive hizmete erişebilir. Hive konsolunu, templeton da, Azure komut satırı aracı (Azure CLI) veya Azure PowerShell erişemez. 
> 
> 

## <a name="create-an-interactive-query-cluster"></a>Etkileşimli sorgu kümesi oluşturma
Bir HDInsight kümesi oluşturma hakkında daha fazla bilgi için bkz: [Hadoop kümeleri oluşturma HDInsight](../hdinsight-hadoop-provision-linux-clusters.md). Etkileşimli sorgu kümesi türünü seçin.

## <a name="execute-hive-queries-from-interactive-query"></a>Etkileşimli sorgu Hive sorguları yürütme
Hive sorguları çalıştırmak için aşağıdaki seçenekleriniz vardır:

* Power BI kullanma

    Bkz: [görselleştirme etkileşimli sorgu Hive verilerini Power BI'da Azure HDInsight ile](./apache-hadoop-connect-hive-power-bi-directquery.md) bkz [Power BI'da Azure HDInsight ile büyük verileri görselleştirme](../hadoop/apache-hadoop-connect-hive-power-bi.md).
 
* Zeppelin kullanma

    Bkz: [kullanın Azure HDInsight Hive sorguları çalıştırmak için Zeppelin'i ](../hdinsight-connect-hive-zeppelin.md).

* Visual Studio'yu kullanma

    Bkz: [bağlanın Azure HDInsight ve Visual Studio için Data Lake Araçları'nı kullanarak Hive sorgularını çalıştırmak](../hadoop/apache-hadoop-visual-studio-tools-get-started.md#run-interactive-hive-queries).

* Visual Studio Code'u kullanma

    Bkz: [kullanım Visual Studio Code için Hive, LLAP veya pySpark](../hdinsight-for-vscode.md).
* Ambari Hive görünümünü kullanarak Hive çalıştırın.
  
    Bkz: [Azure HDInsight, Hadoop ile Hive görünümünü kullanma](../hadoop/apache-hadoop-use-hive-ambari-view.md).
* Beeline'ı kullanarak Hive çalıştırın.
  
    Bkz: [Beeline ile HDInsight Hadoop ile Hive kullanma](../hadoop/apache-hadoop-use-hive-beeline.md).
  
    Beeline baş düğümünden veya boş bir kenar düğümünü kullanabilirsiniz. Bir boş kenar düğümünü Beeline kullanmanızı öneririz. Boş bir kenar düğümünü kullanarak bir HDInsight kümesi oluşturma hakkında daha fazla bilgi için bkz: [HDInsight içinde boş kenar düğümlerini kullanma](../hdinsight-apps-use-edge-node.md).
* Hive, Hive ODBC kullanarak çalıştırın.
  
    Bkz: [Microsoft Hive ODBC sürücüsü ile hadoop'a bağlama Excel](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).

Java veritabanı bağlantısı (JDBC) bağlantı dizesi bulmak için:

1. Ambari için aşağıdaki URL'yi kullanarak oturum açın: https://\<küme adı\>. AzureHDInsight.net.
2. Sol menüde **Hive**.
3. URL'yi kopyalamak için Pano simgesini seçin:
   
   ![HDInsight Hadoop etkileşimli sorgu LLAP JDBC](./media/apache-interactive-query-get-started/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi nasıl [içinde HDInsight etkileşimli sorgu kümelerine oluşturma](../hdinsight-hadoop-provision-linux-clusters.md).
* Bilgi edinmek için nasıl [Power BI'da Azure HDInsight ile büyük verileri görselleştirme](../hadoop/apache-hadoop-connect-hive-power-bi.md).
* Bilgi edinmek için nasıl [Azure HDInsight Hive sorguları çalıştırmak için Zeppelin'i kullanma ](../hdinsight-connect-hive-zeppelin.md).
* Bilgi edinmek için nasıl [Visual Studio için Data Lake Araçları'nı kullanarak Hive sorguları çalıştırma](../hadoop/apache-hadoop-visual-studio-tools-get-started.md#run-interactive-hive-queries).
* Bilgi edinmek için nasıl [Visual Studio Code için HDInsight araçlarını kullanma](../hdinsight-for-vscode.md).
* Bilgi edinmek için nasıl [HDInsight, Hadoop ile Hive görünümünü kullanma](../hadoop/apache-hadoop-use-hive-ambari-view.md)
* Bilgi edinmek için nasıl [HDInsight Hive sorguları göndermek için Beeline kullanma](../hadoop/apache-hadoop-use-hive-beeline.md).
* Bilgi edinmek için nasıl [Excel'i Microsoft Hive ODBC sürücüsü ile Hadoop'a bağlama](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).

