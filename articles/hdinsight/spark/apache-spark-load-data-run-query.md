---
title: "Bir Azure Hdınsight Spark kümesinde etkileşimli sorgular gerçekleştirme | Microsoft Docs"
description: "HDInsight’ta bir Apache Spark kümesi oluşturmaya yönelik HDInsight Spark hızlı başlangıcı."
keywords: "spark hızlı başlangıç,etkileşimli spark,etkileşimli sorgu,hdinsight spark,azure spark"
services: hdinsight
documentationcenter: 
author: mumian
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: jgao
ms.openlocfilehash: 78ab44a7afa6523e1e9e4082b3f45b1a28affe77
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="run-interactive-queries-on-spark-clusters-in-hdinsight"></a>Hdınsight'ta Spark kümeleri üzerinde etkileşimli sorgular gerçekleştirme

Jupyter not defteri karşı bir Spark kümesi etkileşimli Spark SQL sorguları çalıştırmak için nasıl kullanılacağını öğrenin. 

[Jupyter not defteri](http://jupyter-notebook.readthedocs.io/en/latest/notebook.html) Web Konsolu tabanlı etkileşimli deneyiminin genişleterek tarayıcı tabanlı bir uygulama. Hdınsight'ta Spark de içeren [Zeppelin not defteri](apache-spark-zeppelin-notebook.md). Jupyter Not Defteri, bu öğreticide kullanılır.

Hdınsight kümeleri Jupyter not defterlerini destekleyen üç tekrar - **PySpark**, **PySpark3**, ve **Spark**. **PySpark** çekirdek, bu öğreticide kullanılır. Tekrar ve kullanmanın avantajları hakkında daha fazla bilgi için **PySpark**, bkz: [Hdınsight'ta Apache Spark kullanım Jupyter not defterlerinde çekirdekler kümeleri](apache-spark-jupyter-notebook-kernels.md). Zeppelin not defteri kullanmak için bkz: [Azure Hdınsight'ta Apache Spark kullanmak Zeppelin not defterlerini küme](./apache-spark-zeppelin-notebook.md).

Bu öğreticide, bir csv dosyasındaki verileri sorgulayın. İlk bir dataframe Spark verileri da yüklemeniz gerekir. Ardından, Jupyter Not Defteri kullanarak dataframe üzerinde sorguları çalıştırabilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar

* **Azure Hdınsight Spark kümesinde**. Yönergeler için bkz: [Azure Hdınsight'ta Apache Spark kümesi oluşturma](apache-spark-jupyter-spark-sql.md).
* **PySpark kullanarak bir Jupyter not defteri**. Yönergeler için bkz: [Jupyter not defteri oluşturma](./apache-spark-jupyter-spark-sql.md#create-a-jupyter-notebook).

## <a name="create-a-dataframe-from-a-csv-file"></a>Bir csv dosyasından bir dataframe oluşturma

Bir SQLContext ile varolan bir RDD bir Hive tablosu veya veri kaynaklarının dataframes uygulamalar oluşturabilirsiniz. 

**Bir csv dosyasından bir dataframe oluşturmak için**

1. Yoksa, PySpark kullanarak bir Jupyter not defteri oluşturun. Yönergeler için bkz: [Jupyter not defteri oluşturma](./apache-spark-jupyter-spark-sql.md#create-a-jupyter-notebook).

2. Aşağıdaki kodu Not Defteri boş bir hücreye yapıştırın ve sonra basın **SHIFT + ENTER** kodu çalıştırmak için. Kod, bu senaryo için gerekli olan türleri içeri aktarır:

    ```PySpark
    from pyspark.sql import *
    from pyspark.sql.types import *
    ```
    Birinci kod hücresini çalıştırdığınızda PySpark çekirdeği kullanarak bir not defteri, Spark oluşturun ve Hive bağlamları otomatik olarak sizin için oluşturulur. Açıkça bir bağlam oluşturmanız gerekmez.

    Etkileşimli bir sorgu Jupyter'de çalıştırırken, web tarayıcısı penceresi veya sekmesinde başlık gösterilir bir **(meşgul)** not defteri başlığı ile birlikte durum. Ayrıca sağ üst köşedeki **PySpark** metninin yanında içi dolu bir daire görürsünüz. İş tamamlandıktan sonra bu simge boş bir daireye dönüşür.

    ![Etkileşimli Spark SQL sorgusunun durumu](./media/apache-spark-load-data-run-query/hdinsight-spark-interactive-spark-query-status.png "Status of interactive Spark SQL query")

3. Bir dataframe ve geçici bir tablo oluşturmak için aşağıdaki kodu çalıştırın (**hvac**) aşağıdaki kodu çalıştırarak: kod tüm sütunlarda kullanılabilir CSV dosyasını ayıklamak değil. 

    ```PySpark
    # Create an RDD from sample data
    hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
    
    # Create a schema for our data
    Entry = Row('Date', 'Time', 'TargetTemp', 'ActualTemp', 'BuildingID')
    
    # Parse the data and create a schema
    hvacParts = hvacText.map(lambda s: s.split(',')).filter(lambda s: s[0] != 'Date')
    hvac = hvacParts.map(lambda p: Entry(str(p[0]), str(p[1]), int(p[2]), int(p[3]), int(p[6])))
    
    # Infer the schema and create a table       
    hvacTable = sqlContext.createDataFrame(hvac)
    hvacTable.registerTempTable('hvactemptable')
    dfw = DataFrameWriter(hvacTable)
    dfw.saveAsTable('hvac')
    ```
    Aşağıdaki ekran görüntüsünde bir anlık görüntü HVAC.csv dosyasının gösterir. Csv dosyası tüm HDInsigt Spark kümeleri ile birlikte gelir. Verileri bir yapı sıcaklık varyasyonları yakalar.

    ![Etkileşimli Spark SQL sorgusu için verilerin anlık görüntüsünü](./media/apache-spark-load-data-run-query/hdinsight-spark-sample-data-interactive-spark-sql-query.png "etkileşimli Spark SQL sorgusu için verilerin anlık görüntüsünü")

## <a name="run-queries-on-the-dataframe"></a>Üzerinde dataframe sorguları çalıştırma

Tablo oluşturulduktan sonra verileri etkileşimli bir sorgu çalıştırabilirsiniz.

**Bir sorguyu çalıştırmak için**

1. Aşağıdaki kodu Not Defteri boş bir hücreye çalıştırın:

    ```PySpark
    %%sql
    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```

   PySpark çekirdeği not defterinde kullanıldığından, artık doğrudan etkileşimli bir SQL sorgusu geçici tablosunda çalıştırabilirsiniz **hvac** kullanılarak oluşturulan `%%sql` Sihirli. `%%sql` sihrinin yanı sıra PySpark çekirdeği kullanılabilen diğer sihirler hakkında daha fazla bilgi için bkz. [Spark HDInsight kümeleri ile Jupyter not defterlerinde kullanılabilen çekirdekler](apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).

   Varsayılan olarak aşağıdaki tablo çıktısı görüntülenir.

     ![Etkileşimli Spark sorgu sonucunun tablo çıktısı](./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result.png "Table output of interactive Spark query result")

3. Sonuçları diğer görselleştirmelerde de görebilirsiniz. Aynı çıktı için bir alan grafiği görmek için seçin **alanı** sonra diğer değerleri gösterilen şekilde ayarlayın.

    ![Etkileşimli Spark sorgu sonucunun alan grafiği](./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result-area-chart.png "Area graph of interactive Spark query result")

10. Gelen **dosya** dizüstü menüsünde **Kaydet ve denetim noktası**. 

11. Başlatıyorsanız [sonraki öğretici](apache-spark-use-bi-tools.md) şimdi, dizüstü bilgisayar açık bırakın. Aksi takdirde, küme kaynakları serbest bırakmak için Not Defteri kapatın: gelen **dosya** dizüstü menüsünde **Kapat ve Durdur**.

## <a name="next-step"></a>Sonraki adım

Bu makalede, Jupyter Not Defteri kullanarak Spark etkileşimli sorgular gerçekleştirme öğrendiniz. Nasıl Spark kayıtlı veri Power BI ve Tableau gibi BI analizi aracını içine alınmasını görmek için sonraki makalede ilerleyin. 

> [!div class="nextstepaction"]
>[Spark Azure Hdınsight ile verileri görselleştirme araçlarını kullanarak BI](apache-spark-use-bi-tools.md)




