---
title: Power BI'da Azure HDInsight ile etkileşimli sorgu Hive verilerini Görselleştirme
description: Azure HDInsight etkileşimli sorgu Hive verilerini görselleştirmek için Microsoft Power BI'ı kullanma
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/25/2018
ms.openlocfilehash: fb4e16c8be5344c5b9947758b6a09845b470196d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65801004"
---
# <a name="visualize-interactive-query-apache-hive-data-with-microsoft-power-bi-using-direct-query-in-azure-hdinsight"></a>Doğrudan sorgu kullanarak Azure HDInsight Microsoft Power BI ile etkileşimli sorgu Apache Hive verileri Görselleştirme

Bu makalede, Microsoft Power BI, Azure HDInsight etkileşimli sorgu kümelerine bağlanmak ve doğrudan sorgu kullanarak Apache Hive verileri görselleştirme açıklar. Sağlanan örnek verileri yükleyen bir `hivesampletable` Power bı'a Hive tablosu. `hivesampletable` Hive tablosu bazı cep telefonu kullanım verilerini içerir. Ardından bir dünya Haritası kullanım verileri Çiz:

![HDInsight Power BI harita raporu](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-visualization.png)

Yararlanabileceğiniz [Apache Hive ODBC sürücüsünü](../hadoop/apache-hadoop-connect-hive-power-bi.md) Power BI Desktop'ta genel ODBC bağlayıcısı aracılığıyla içe aktarmak için. Ancak etkileşimli Hive sorgu altyapısı göz önünde bulundurulduğunda BI iş yükleri için önerilmez. [HDInsight Interactive Query Bağlayıcısı](./apache-hadoop-connect-hive-power-bi-directquery.md) ve [HDInsight Apache Spark Bağlayıcısı](https://docs.microsoft.com/power-bi/spark-on-hdinsight-with-direct-connect) performanslarını için daha iyi Seçenekler.

## <a name="prerequisites"></a>Önkoşullar
Bu makalede geçmeden önce aşağıdaki öğelere sahip olmanız gerekir:

* **HDInsight küme**. Küme, Apache Hive ile bir HDInsight kümesi ya da yeni yayımlanmış bir etkileşimli sorgu kümesi olabilir. Kümeleri oluşturmak için bkz: [küme oluştur](../hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster).
* **[Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/)** . Bir kopyasından indirebileceğiniz [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=45331).

## <a name="load-data-from-hdinsight"></a>HDInsight yük verileri

`hivesampletable` Hive tablosu tüm HDInsight kümeleri ile birlikte gelir.

1. Power BI Desktop'ı başlatın.

2. Menü çubuğundan gidin **giriş** > **Veri Al** > **daha...** .

    ![Açık veri HDInsight Power BI](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-open-odbc.png)

3. Gelen **Veri Al** penceresinde girin **hdınsight** arama kutusuna.  

4. Arama sonuçlarından seçin **HDInsight etkileşimli sorgu**ve ardından **Connect**.  Görmüyorsanız **HDInsight etkileşimli sorgu**, Power BI Desktop, en son sürüme güncelleştirmeniz gerekir.

5. Seçin **devam** kapatmak için **bir üçüncü taraf hizmetine** iletişim.

6. İçinde **HDInsight etkileşimli sorgu** penceresinde aşağıdaki bilgileri girin ve ardından **Tamam**:

    |Özellik | Değer |
    |---|---|
    |Sunucusu |Küme adını girin, örneğin *myiqcluster.azurehdinsight.net*.|
    |Database |Girin **varsayılan** Bu makale için.|
    |Veri bağlantısı modu |Seçin **DirectQuery** Bu makale için.|

    ![HDInsight etkileşimli sorgu Power BI DirectQuery bağlanma](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-interactive-query-power-bi-connect.png)

7. HTTP kimlik bilgilerini girin ve ardından **Connect**. Varsayılan kullanıcı adı **yönetici**.

8. Gelen **Gezgin** penceresinin sol bölmesinde, select **hivesampletale**.

9. Seçin **yük** ana penceresinde.

    ![HDInsight etkileşimli sorgu Power BI hivesampletable](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-interactive-query-power-bi-hivesampletable.png)

## <a name="visualize-data-on-a-map"></a>Harita üzerinde verileri Görselleştirme

Son yordama devam edin.

1. Görsel öğeler bölmesinde seçin **harita**, dünyanın dört bir yanındaki simge. Genel eşleme sonra ana penceresinde görünür.

    ![HDInsight Power BI rapor özelleştirir](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-customize.png)

2. Alanlar bölmesinden seçin **Ülke** ve **devicemake**. Veri noktalarıyla bir dünya Haritası birkaç dakika sonra ana penceresinde görünür.

3. Harita genişletin.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Microsoft Power BI'ı kullanarak HDInsight verilerini görselleştirmek öğrendiniz.  Veri görselleştirme hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [ODBC kullanarak Azure HDInsight Microsoft Power BI ile Apache Hive verileri görselleştirme](../hadoop/apache-hadoop-connect-hive-power-bi.md). 
* [Azure HDInsight Apache Hive sorguları çalıştırmak için Apache Zeppelin kullanma](../interactive-query/hdinsight-connect-hive-zeppelin.md).
* [Excel'i Microsoft Hive ODBC sürücüsü ile HDInsight bağlama](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Excel'i Power Query kullanarak Apache Hadoop'a bağlama](../hadoop/apache-hadoop-connect-excel-power-query.md).
* [Azure HDInsight için bağlanın ve Visual Studio için Data Lake Araçları'nı kullanarak Apache Hive sorguları çalıştırma](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).
* [Visual Studio Code için Azure HDInsight aracını](../hdinsight-for-vscode.md).
* [HDInsight için verileri karşıya](./../hdinsight-upload-data.md).
