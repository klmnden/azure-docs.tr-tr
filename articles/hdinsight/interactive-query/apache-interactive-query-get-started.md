---
title: "Azure Hdınsight ile etkileşimli sorgu kullanma | Microsoft Docs"
description: "Hdınsight ile etkileşimli sorgu (Hive LLAP) kullanmayı öğrenin."
keywords: 
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 0957643c-4936-48a3-84a3-5dc83db4ab1a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2017
ms.author: jgao
ms.openlocfilehash: 9d9d9556c37cfa5a1a740569b4c7fd4fd07a467a
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/16/2017
---
# <a name="use-interactive-query-with-hdinsight"></a>Hdınsight ile etkileşimli sorgu kullanma
Etkileşimli sorgu (Hive LLAP olarak da bilinir veya [Canlı uzun ve işlem](https://cwiki.apache.org/confluence/display/Hive/LLAP)) bir Azure Hdınsight olan [küme türü](../hdinsight-hadoop-provision-linux-clusters.md#cluster-types). Etkileşimli sorgu bellek içi önbelleğe alma, hangi Hive sorguları daha hızlı ve daha fazla etkileşimli yapar destekler.

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)] 

Bir Hadoop kümesinden farklı bir etkileşimli sorgu kümedir. Yalnızca Hive hizmeti içerir. 

> [!NOTE]
> Ambari Hive görünümü, Beeline ve Microsoft Hive açık veritabanı bağlantısı sürücüsü (Hive ODBC) aracılığıyla yalnızca etkileşimli sorgu kümesindeki Hive hizmet erişebilir. Hive konsol, Templeton, Azure komut satırı aracı (Azure CLI) veya Azure PowerShell erişilemiyor. 
> 
> 

## <a name="create-an-interactive-query-cluster"></a>Etkileşimli sorgu kümesi oluşturma
Bir Hdınsight kümesi oluşturma hakkında daha fazla bilgi için bkz: [Hdınsight'ta oluşturmak Hadoop kümeleri](../hdinsight-hadoop-provision-linux-clusters.md). Etkileşimli sorgu küme türü seçin.

## <a name="execute-hive-queries-from-interactive-query"></a>Etkileşimli sorgudan Hive sorguları yürütme
Hive sorgularını yürütmek için aşağıdaki seçenekleriniz vardır:

* Power BI kullanın

    Bkz: [Azure hdınsight'ta Power BI ile büyük veri görselleştirme](../hadoop/apache-hadoop-connect-hive-power-bi.md).

* Zeppelin kullanma

    Bkz: [kullanım Azure Hdınsight'ta Hive sorguları çalıştırmak için Zeppelin ](../hdinsight-connect-hive-zeppelin.md).

* Visual Studio'yu kullanma

    Bkz: [bağlanın Azure Hdınsight ve Visual Studio için Data Lake Araçları'nı kullanarak çalışma Hive sorguları](../hadoop/apache-hadoop-visual-studio-tools-get-started.md#run-interactive-hive-queries).

* Visual Studio kodu kullanın

    Bkz: [kullanım Visual Studio Code Hive, LLAP veya pySpark](../hdinsight-for-vscode.md).
* Hive Ambari Hive görünümünü kullanarak çalıştırın.
  
    Bkz: [Azure hdınsight'ta Hadoop ile Hive görünümünü kullanın](../hadoop/apache-hadoop-use-hive-ambari-view.md).
* Hive Beeline kullanarak çalıştırın.
  
    Bkz: [Beeline ile hdınsight'ta Hadoop ile Hive kullanma](../hadoop/apache-hadoop-use-hive-beeline.md).
  
    Beeline baş düğümünden veya boş kenar düğümünü kullanabilirsiniz. Bir boş kenar düğümden Beeline kullanmanızı öneririz. Bir boş kenar düğümünü kullanarak bir Hdınsight kümesi oluşturma hakkında daha fazla bilgi için bkz: [Hdınsight'ta boş kenar düğümünü kullanmak](../hdinsight-apps-use-edge-node.md).
* Hive ODBC Hive kullanarak çalıştırın.
  
    Bkz: [bağlanmak Excel için Microsoft Hive ODBC sürücüsü ile hadoop'a](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).

Java veritabanı bağlantısı (JDBC) bağlantı dizesini bulmak için:

1. Ambari için aşağıdaki URL'yi kullanarak oturum açın: https://\<küme adı\>. AzureHDInsight.net.
2. Soldaki menüde seçin **Hive**.
3. URL'yi kopyalamak için Pano simgesini seçin:
   
   ![Hdınsight Hadoop etkileşimli sorgu LLAP JDBC](./media/apache-interactive-query-get-started/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [Hdınsight'ta etkileşimli sorgu kümeleri oluşturma](../hdinsight-hadoop-provision-linux-clusters.md).
* Bilgi edinmek için nasıl [Azure hdınsight'ta Power BI ile büyük veri görselleştirme](../hadoop/apache-hadoop-connect-hive-power-bi.md).
* Bilgi edinmek için nasıl [Zeppelin Azure Hdınsight'ta Hive sorguları çalıştırmak için kullandığınız ](../hdinsight-connect-hive-zeppelin.md).
* Bilgi edinmek için nasıl [Visual Studio için Data Lake Araçları'nı kullanarak Hive sorguları çalıştırma](../hadoop/apache-hadoop-visual-studio-tools-get-started.md#run-interactive-hive-queries).
* Bilgi nasıl [Hdınsight araçları kullanmak için Visual Studio Code](../hdinsight-for-vscode.md).
* Bilgi edinmek için nasıl [hdınsight'ta Hadoop ile Hive görünümünü kullanın](../hadoop/apache-hadoop-use-hive-ambari-view.md)
* Bilgi edinmek için nasıl [hdınsight'ta Hive sorguları göndermek için Beeline kullanın](../hadoop/apache-hadoop-use-hive-beeline.md).
* Bilgi edinmek için nasıl [Excel'i Microsoft Hive ODBC sürücüsü ile Hadoop için bağlama](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).

