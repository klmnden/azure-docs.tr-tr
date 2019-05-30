---
title: Power BI - Azure HDInsight ile Apache Hive verileri Görselleştirme
description: Azure HDInsight tarafından işlenen Hive verileri görselleştirmek için Microsoft Power BI'ı kullanmayı öğrenin.
keywords: hdınsight, hadoop, hive, etkileşimli sorgu, Interactive hive, LLAP, odbc
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: hrasheed
ms.openlocfilehash: 1e0c043e484e4eaf2639f76c9af3fef15ad85047
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66237496"
---
# <a name="visualize-apache-hive-data-with-microsoft-power-bi-using-odbc-in-azure-hdinsight"></a>ODBC kullanarak Azure HDInsight Microsoft Power BI ile Apache Hive verileri Görselleştirme

Apache Hive verileri Görselleştirme ve ODBC kullanarak Azure HDInsight için Microsoft Power BI Desktop bağlanma hakkında bilgi edinin.

>[!IMPORTANT]
> Power BI Desktop'ta genel ODBC bağlayıcısı aracılığıyla içe aktarmak için Hive ODBC sürücüsünü yararlanabilirsiniz. Ancak etkileşimli Hive sorgu altyapısı göz önünde bulundurulduğunda BI iş yükleri için önerilmez. [HDInsight Interactive Query Bağlayıcısı](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md) ve [HDInsight Spark Bağlayıcısı](https://docs.microsoft.com/power-bi/spark-on-hdinsight-with-direct-connect) performanslarını için daha iyi Seçenekler.

Bu öğreticide, verileri yüklediğiniz bir `hivesampletable` Power bı'a Hive tablosu. Hive tablosu bazı cep telefonu kullanım verilerini içerir. Ardından bir dünya Haritası kullanım verileri Çiz:

![HDInsight Power BI harita raporu](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-visualization.png)

Bilgiler de yeni geçerlidir [etkileşimli sorgu](../interactive-query/apache-interactive-query-get-started.md) küme türü. HDInsight etkileşimli doğrudan sorgu kullanarak sorgu için bkz öğrenmek için [doğrudan sorgu kullanarak Azure HDInsight Microsoft Power BI ile görselleştirin etkileşimli sorgu Hive verilerini](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).

## <a name="prerequisites"></a>Önkoşullar

Bu makalede geçmeden önce aşağıdaki öğelere sahip olmanız gerekir:

* **HDInsight küme**. Küme bir Hive ile HDInsight kümesi ya da yeni yayımlanmış bir etkileşimli sorgu kümesi olabilir. Kümeleri oluşturmak için bkz: [küme oluştur](apache-hadoop-linux-tutorial-get-started.md#create-cluster).

* **[Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/)** . Bir kopyasından indirebileceğiniz [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=45331).

## <a name="create-hive-odbc-data-source"></a>Hive ODBC veri kaynağı oluşturma

Bkz: [oluşturma Hive ODBC veri kaynağı](apache-hadoop-connect-excel-hive-odbc-driver.md#create-apache-hive-odbc-data-source).

## <a name="load-data-from-hdinsight"></a>HDInsight yük verileri

Hivesampletable Hive tablosu tüm HDInsight kümeleri ile birlikte gelir.

1. Power BI Desktop'ı başlatın.

2. Üstteki menüden gidin **giriş** > **Veri Al** > **daha...** .

    ![Açık veri HDInsight Power BI](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-open-odbc.png)

3. Gelen **Veri Al** iletişim kutusunda **diğer** soldan seçin **ODBC** sağa seçip **Connect** alt.

4. Gelen **gelen ODBC** veri kaynağı adı aşağı açılan listeden son bölümde oluşturduğunuz ve ardından iletişim kutusunda **Tamam**.

5. Gelen **Gezgin** iletişim kutusunda Genişlet **ODBC > HIVE > Varsayılan**seçin **hivesampletable**ve ardından **yük**.

6. Gelen **ODBC sürücüsü** iletişim kutusunda **varsayılan veya özel**, ardından **Connect**.

## <a name="visualize-data"></a>Verileri görselleştirme

Son yordama devam edin.

1. Görsel öğeler bölmesinde seçin **harita**.  Dünya simgesi var.

    ![HDInsight Power BI rapor özelleştirir](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-customize.png)
2. Gelen **alanları** bölmesinde **Ülke** ve **devicemake**. Harita üzerinde çizilen verileri görebilirsiniz.
3. Harita genişletin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Power BI'ı kullanarak HDInsight verilerini görselleştirmek öğrendiniz.  Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure HDInsight Apache Hive sorguları çalıştırmak için Apache Zeppelin kullanma](../interactive-query/hdinsight-connect-hive-zeppelin.md).
* [Excel'i Microsoft Hive ODBC sürücüsü ile HDInsight bağlama](./apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Excel'i Power Query kullanarak Apache Hadoop'a bağlama](apache-hadoop-connect-excel-power-query.md).
* [Azure HDInsight için bağlanın ve Visual Studio için Data Lake Araçları'nı kullanarak Apache Hive sorguları çalıştırma](apache-hadoop-visual-studio-tools-get-started.md).
* [Visual Studio Code için Azure HDInsight aracını](../hdinsight-for-vscode.md).
* [HDInsight için verileri karşıya](./../hdinsight-upload-data.md).
