---
title: Power BI - Azure HDInsight ile Apache Hive verileri Görselleştirme
description: Azure HDInsight tarafından işlenen Hive verileri görselleştirmek için Microsoft Power BI'ı kullanmayı öğrenin.
keywords: hdınsight, hadoop, hive, etkileşimli sorgu, Interactive hive, LLAP, odbc
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: hrasheed
ms.openlocfilehash: ac5ca3d9501718cd5b538f6bb8c1fafee78063b2
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62122107"
---
# <a name="visualize-apache-hive-data-with-microsoft-power-bi-using-odbc-in-azure-hdinsight"></a>ODBC kullanarak Azure HDInsight Microsoft Power BI ile Apache Hive verileri Görselleştirme

Apache Hive verileri Görselleştirme ve ODBC kullanarak Azure HDInsight için Microsoft Power BI'ı bağlama hakkında bilgi edinin.

>[!IMPORTANT]
> Power BI Desktop'ta genel ODBC bağlayıcısı aracılığıyla içe aktarmak için Hive ODBC sürücüsünü yararlanabilirsiniz. Ancak etkileşimli Hive sorgu altyapısı göz önünde bulundurulduğunda BI iş yükleri için önerilmez. [HDInsight Interactive Query Bağlayıcısı](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md) ve [HDInsight Spark Bağlayıcısı](https://docs.microsoft.com/power-bi/spark-on-hdinsight-with-direct-connect) performanslarını için daha iyi Seçenekler.

Bu öğreticide, verileri Power BI'a hivesampletable Hive tablosundaki yükleyin. Hive tablosu bazı cep telefonu kullanım verilerini içerir. Ardından bir dünya Haritası kullanım verileri Çiz:

![HDInsight Power BI harita raporu](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-visualization.png)

Bilgiler de yeni geçerlidir [etkileşimli sorgu](../interactive-query/apache-interactive-query-get-started.md) küme türü. HDInsight etkileşimli doğrudan sorgu kullanarak sorgu için bkz öğrenmek için [doğrudan sorgu kullanarak Azure HDInsight Microsoft Power BI ile görselleştirin etkileşimli sorgu Hive verilerini](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).



## <a name="prerequisites"></a>Önkoşullar
Bu makalede geçmeden önce aşağıdaki öğelere sahip olmanız gerekir:

* **HDInsight küme**. Küme bir Hive ile HDInsight kümesi ya da yeni yayımlanmış bir etkileşimli sorgu kümesi olabilir. Kümeleri oluşturmak için bkz: [küme oluştur](apache-hadoop-linux-tutorial-get-started.md#create-cluster).
* **[Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/)**. Bir kopyasından indirebileceğiniz [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=45331).

## <a name="create-hive-odbc-data-source"></a>Hive ODBC veri kaynağı oluşturma

Bkz: [oluşturma Hive ODBC veri kaynağı](apache-hadoop-connect-excel-hive-odbc-driver.md#create-apache-hive-odbc-data-source).

## <a name="load-data-from-hdinsight"></a>HDInsight yük verileri

Hivesampletable Hive tablosu tüm HDInsight kümeleri ile birlikte gelir.

1. Power BI Desktop için oturum açın.
2. Tıklayın **giriş** sekmesinde **Veri Al** gelen **dış veri** Şerit ve ardından **daha fazla**.

    ![Açık veri HDInsight Power BI](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-open-odbc.png)
3. Gelen **Veri Al** bölmesinde tıklayın **diğer** soldan tıklayın **ODBC** sağ tıkladıktan sonra **Connect** alt.
4. Gelen **gelen ODBC** veri kaynağı adı son bölümde oluşturduğunuz ve ardından bölmesinde **Tamam**.
5. Gelen **Gezgin** bölmesini genişletin **ODBC HIVE -> Varsayılan ->** seçin **hivesampletable**ve ardından **yük**.

## <a name="visualize-data"></a>Verileri görselleştirme

Son yordama devam edin.

1. Görsel öğeler bölmesinde seçin **harita**.  Dünya simgesi var.

    ![HDInsight Power BI rapor özelleştirir](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-customize.png)
2. Alanlar bölmesinden seçin **Ülke** ve **devicemake**. Harita üzerinde çizilen verileri görebilirsiniz.
3. Harita genişletin.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Power BI'ı kullanarak HDInsight verilerini görselleştirmek öğrendiniz.  Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure HDInsight Apache Hive sorguları çalıştırmak için Apache Zeppelin kullanma](./../hdinsight-connect-hive-zeppelin.md).
* [Excel'i Microsoft Hive ODBC sürücüsü ile HDInsight bağlama](./apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Excel'i Power Query kullanarak Apache Hadoop'a bağlama](apache-hadoop-connect-excel-power-query.md).
* [Azure HDInsight için bağlanın ve Visual Studio için Data Lake Araçları'nı kullanarak Apache Hive sorguları çalıştırma](apache-hadoop-visual-studio-tools-get-started.md).
* [Visual Studio Code için Azure HDInsight aracını](../hdinsight-for-vscode.md).
* [HDInsight için verileri karşıya](./../hdinsight-upload-data.md).
