---
title: Azure HDInsight Apache Hive sorguları çalıştırmak için Apache Zeppelin kullanma
description: Apache Hive sorguları çalıştırmak için Apache Zeppelin kullanmayı öğrenin.
keywords: hdınsight, hadoop, hive, LLAP etkileşimli sorgu
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,
ms.topic: conceptual
ms.date: 11/05/2018
ms.author: hrasheed
ms.openlocfilehash: 417e9f8ae78889374983bf77900ee00fa7fc6338
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62098769"
---
# <a name="use-apache-zeppelin-to-run-apache-hive-queries-in-azure-hdinsight"></a>Azure HDInsight Apache Hive sorguları çalıştırmak için Apache Zeppelin kullanma 

HDInsight etkileşimli sorgu kümelerine dahil [Apache Zeppelin](https://zeppelin.apache.org/) Hive sorguları, etkileşimli olarak çalıştırmak için kullanabileceğiniz not defterleri. Bu makalede, çalıştırılacak Apache Zeppelin kullanmayı öğrenirsiniz [Apache Hive](https://hive.apache.org/) sorgular, Azure HDInsight. 

## <a name="prerequisites"></a>Önkoşullar
Bu makalede geçmeden önce aşağıdaki öğelere sahip olmanız gerekir:

* **HDInsight etkileşimli sorgu kümesi**. Bkz: [küme oluştur](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster) bir HDInsight kümesi oluşturmak için.  Etkileşimli sorgu türünü seçtiğinizden emin olun. 

## <a name="create-an-apache-zeppelin-note"></a>Bir Apache Zeppelin Not oluşturun

1. Aşağıdaki URL'ye gidin:

        https://CLUSTERNAME.azurehdinsight.net/zeppelin
    **CLUSTERNAME** değerini kümenizin adıyla değiştirin.

2. Hadoop kullanıcı adını ve parolayı girin. Zeppelin sayfasından yeni bir not oluşturun veya var olan Notları'nı açın. HiveSample bazı örnek Hive sorguları içerir.  

    ![HDInsight etkileşimli sorgu zeppelin](./media/hdinsight-connect-hive-zeppelin/hdinsight-hive-zeppelin.png)
3. Tıklayın **yeni not oluşturma**.
4. Aşağıdaki değerleri yazın veya seçin:

    - Notu adı: notu için bir ad girin.
    - Varsayılan yorumlayıcı: seçin **JDBC**.

5. Tıklayın **oluşturmanız**.
6. Aşağıdaki Hive sorgusunu çalıştırın:

        %jdbc(hive)
        show tables

    ![HDInsight etkileşimli sorgu zeppelin sorgusu çalıştırır.](./media/hdinsight-connect-hive-zeppelin/hdinsight-hive-zeppelin-query.png)

    **%Jdbc(hive)** deyiminin ilk satırı Hive JDBC yorumlayıcı kullanmak için Not defterini söyler.

    Sorgu adı verilen bir Hive tablosu döndürme *hivesampletable*.

    Hivesampletable karşı çalıştırabilirsiniz iki daha fazla Hive sorguları aşağıda verilmiştir. 

        %jdbc(hive)
        select * from hivesampletable limit 10

        %jdbc(hive)
        select ${group_name}, count(*) as total_count
        from hivesampletable
        group by ${group_name=market,market|deviceplatform|devicemake}
        limit ${total_count=10}

    Geleneksel Hive için karşılaştırma, sorgu sonuçları gereken daha hızlı dönün.


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, HDInsight'ı kullanarak verileri görselleştirmek öğrendiniz [Microsoft Power BI](https://powerbi.microsoft.com/).  Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure HDInsight, Microsoft Power BI ile Apache Hive verileri görselleştirme](hadoop/apache-hadoop-connect-hive-power-bi.md).
* [Power BI'da Azure HDInsight ile Apache etkileşimli sorgu Hive verilerini görselleştirme](./interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Excel'i Microsoft Hive ODBC sürücüsü ile HDInsight bağlama](hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Excel'i Power Query kullanarak Apache Hadoop'a bağlama](hadoop/apache-hadoop-connect-excel-power-query.md).
* [Azure HDInsight için bağlanın ve Visual Studio için Data Lake Araçları'nı kullanarak Apache Hive sorguları çalıştırma](hadoop/apache-hadoop-visual-studio-tools-get-started.md).
* [Visual Studio Code için Azure HDInsight aracını](hdinsight-for-vscode.md).
* [HDInsight için verileri karşıya](./hdinsight-upload-data.md).
