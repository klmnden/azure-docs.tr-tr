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
ms.date: 09/22/2017
ms.author: jgao
ms.openlocfilehash: db8f0056fa3813e95c2c5bea583d7b66ac64260f
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="run-interactive-queries-on-an-hdinsight-spark-cluster"></a>Bir Hdınsight Spark kümesinde etkileşimli sorgular gerçekleştirme

Bu makalede, bir Spark kümesi karşı etkileşimli Spark SQL sorguları çalıştırmak için bir Jupyter not defteri kullanın. Jupyter not defteri Web Konsolu tabanlı etkileşimli deneyiminin genişleten bir tarayıcı tabanlı bir uygulamadır. Daha fazla bilgi için bkz: [Jupyter not defteri](http://jupyter-notebook.readthedocs.io/en/latest/notebook.html).

Bu öğretici için kullandığınız **PySpark** çekirdek etkileşimli bir Spark SQL sorgusunu çalıştırmak Jupyter Not Defteri içinde. Hdınsight kümeleri Jupyter not defterlerini destekleyen da iki diğer çekirdek - **PySpark3** ve **Spark**. Tekrar ve kullanmanın avantajları hakkında daha fazla bilgi için **PySpark**, bkz: [Hdınsight'ta Apache Spark kullanım Jupyter not defterlerinde çekirdekler kümeleri](apache-spark-jupyter-notebook-kernels.md).

## <a name="prerequisites"></a>Ön koşullar

* **Azure Hdınsight Spark kümesinde**. Yönergeler için bkz: [Azure Hdınsight'ta Apache Spark kümesi oluşturma](apache-spark-jupyter-spark-sql.md).

## <a name="create-a-jupyter-notebook-to-run-interactive-queries"></a>Etkileşimli sorgular gerçekleştirmek üzere Jupyter not defteri oluşturma

Sorguları çalıştırmak için varsayılan kümeyle ilişkili depolama alanında kullanılabilir olan örnek veri kullanırız. Ancak, öncelikle bu veri Spark dataframe yüklemeniz gerekir. Dataframe sahip olduğunda, sorguları Jupyter Not Defteri kullanarak çalıştırabilirsiniz. Bu makalede, nasıl bakın:

* Bir örnek veri kümesi Spark dataframe kaydedin.
* Sorgular dataframe üzerinde çalışır.

Haydi başlayalım.

1. [Azure portalı](https://portal.azure.com/) açın. Kümeyi panoya sabitlemeyi seçtiyseniz, küme dikey penceresini başlatmak için panodan küme kutucuğuna tıklayın.

    Kümeyi panoya sabitlemediyseniz, sol bölmedeki **HDInsight kümeleri** öğesine ve ardından oluşturduğunuz kümeye tıklayın.

3. **Hızlı bağlantılar** bölümünde **Küme panoları**'na ve ardından **Jupyter Notebook**'a tıklayın. İstenirse, küme için yönetici kimlik bilgilerini girin.

   ![Jupyter not defterini açarak etkileşimli Spark SQL sorgusu çalıştırma](./media/apache-spark-load-data-run-query/hdinsight-spark-start-jupyter-interactive-spark-sql-query.png "Open Jupyter notebook to run interactive Spark SQL query")

   > [!NOTE]
   > Aşağıdaki URL’yi tarayıcınızda açarak da Jupyter not defterine erişebilirsiniz. **CLUSTERNAME** değerini kümenizin adıyla değiştirin:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Bir not defteri oluşturun. **Yeni** ve ardından **PySpark** seçeneğine tıklayın.

   ![Jupyter not defterini açarak etkileşimli Spark SQL sorgusu oluşturma](./media/apache-spark-load-data-run-query/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "Create a Jupyter notebook to run interactive Spark SQL query")

   Untitled(Untitled.pynb) adıyla yeni bir not defteri oluşturulur ve açılır.

4. Üstteki not defteri adına tıklayın ve isterseniz kolay bir ad girin.

    ![Etkileşimli Spark sorgusunun çalıştırılacağı Jupyter not defteri için ad belirtme](./media/apache-spark-load-data-run-query/hdinsight-spark-jupyter-notebook-name.png "Provide a name for the Jupter notebook to run interactive Spark query from")

5. Aşağıdaki kodu boş bir hücreye yapıştırın ve kodu çalıştırmak için **SHIFT + ENTER** tuşlarına basın. Kod, bu senaryo için gerekli olan türleri içeri aktarır:

        from pyspark.sql import *
        from pyspark.sql.types import *

    PySpark çekirdeği kullanarak bir not defteri oluşturduğunuz için açıkça bir bağlam oluşturmanız gerekmez. Birinci kod hücresini çalıştırdığınızda Spark ve Hive bağlamları sizin için otomatik olarak oluşturulur.

    ![Etkileşimli Spark SQL sorgusunun durumu](./media/apache-spark-load-data-run-query/hdinsight-spark-interactive-spark-query-status.png "Status of interactive Spark SQL query")

    Jupyter’de etkileşimli bir sorguyu her çalıştırdığınızda web tarayıcınızın pencere başlığında not defteri başlığı ile birlikte **(Meşgul)** durumu gösterilir. Ayrıca sağ üst köşedeki **PySpark** metninin yanında içi dolu bir daire görürsünüz. İş tamamlandıktan sonra bu simge boş bir daireye dönüşür.

6. Verileri bir Spark kümesi yüklemek önce bize bir görüntüsünü bakın sağlar. Bu öğreticide kullanılan örnek verileri tüm Hdınsight Spark kümeleri, bir CSV dosyası olarak kullanılabilir **\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv**. Verileri bir yapı sıcaklık varyasyonları yakalar. Burada, verilerin ilk birkaç satırı bulunmaktadır.

    ![Etkileşimli Spark SQL sorgusu için verilerin anlık görüntüsünü](./media/apache-spark-load-data-run-query/hdinsight-spark-sample-data-interactive-spark-sql-query.png "etkileşimli Spark SQL sorgusu için verilerin anlık görüntüsünü")

6. Bir dataframe ve geçici bir tablo oluşturma (**hvac**) aşağıdaki kodu çalıştırarak. Bu öğretici için kullanılabilen tüm sütunları CSV dosyasında oluşturuyoruz değil. 

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

7. Verileri, etkileşimli bir sorguyu çalıştırmak tablo oluşturulduktan sonra aşağıdaki kodu kullanın.

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

   Bir PySpark çekirdeği kullandığınız için `%%sql` işlevini kullanarak, oluşturduğunuz **hvac** geçici tablosunda bundan böyle etkileşimli bir SQL sorgusunu doğrudan çalıştırabilirsiniz. `%%sql` sihrinin yanı sıra PySpark çekirdeği kullanılabilen diğer sihirler hakkında daha fazla bilgi için bkz. [Spark HDInsight kümeleri ile Jupyter not defterlerinde kullanılabilen çekirdekler](apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).

   Varsayılan olarak aşağıdaki tablo çıktısı görüntülenir.

     ![Etkileşimli Spark sorgu sonucunun tablo çıktısı](./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result.png "Table output of interactive Spark query result")

9. Sonuçları diğer görselleştirmelerde de görebilirsiniz. Aynı çıktı için bir alan grafiği görmek için seçin **alanı** sonra diğer değerleri gösterilen şekilde ayarlayın.

    ![Etkileşimli Spark sorgu sonucunun alan grafiği](./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result-area-chart.png "Area graph of interactive Spark query result")

10. Gelen **dosya** dizüstü menüsünde **Kaydet ve denetim noktası**. 

11. Başlatıyorsanız [sonraki öğretici](apache-spark-use-bi-tools.md) şimdi, dizüstü bilgisayar açık bırakın. Aksi takdirde, küme kaynakları serbest bırakmak için Not Defteri kapatın: gelen **dosya** dizüstü menüsünde **Kapat ve Durdur**.

## <a name="next-step"></a>Sonraki adım

Bu makalede Jupyter Not Defteri kullanarak Spark etkileşimli sorgular gerçekleştirme öğrendiniz. Nasıl Spark kayıtlı veri Power BI ve Tableau gibi BI analizi aracını içine alınmasını görmek için sonraki makalede ilerleyin. 

> [!div class="nextstepaction"]
>[Spark Azure Hdınsight ile verileri görselleştirme araçlarını kullanarak BI](apache-spark-use-bi-tools.md)




