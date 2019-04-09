---
title: 'Öğretici: Verileri yüklemek ve Azure HDInsight, Apache Spark kümesinde sorguları çalıştırma '
description: Azure HDInsight içindeki Spark kümelerinde veri yüklemeyi ve etkileşimli sorgular çalıştırmayı öğrenin.
services: azure-hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,mvc
ms.topic: tutorial
ms.author: hrasheed
ms.date: 04/03/2019
ms.openlocfilehash: 18f5d34e50a4ed4ed82a3ceb4740d594ce4bd78d
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59274017"
---
# <a name="tutorial-load-data-and-run-queries-on-an-apache-spark-cluster-in-azure-hdinsight"></a>Öğretici: Verileri yüklemek ve Azure HDInsight, Apache Spark kümesinde sorguları çalıştırma

Bu öğreticide, bir csv dosyasından bir dataframe oluşturun ve karşı etkileşimli Spark SQL sorgularının nasıl çalıştırılacağını öğrenin bir [Apache Spark](https://spark.apache.org/) Azure HDInsight kümesinde. Spark’ta dataframe, adlandırılmış sütunlar halinde düzenlenmiş, dağıtılmış bir veri koleksiyonudur. Dataframe kavramsal olarak, ilişkisel bir veritabanındaki tabloya veya R/Python’daki veri çerçevesine eşdeğerdir.
 
Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Bir csv dosyasından dataframe oluşturma
> * Dataframe üzerinde sorgular çalıştırma

## <a name="prerequisites"></a>Önkoşullar

* [Azure HDInsight’ta Apache Spark kümesi oluşturma](apache-spark-jupyter-spark-sql.md) bölümünü tamamlayın.

## <a name="create-a-dataframe-from-a-csv-file"></a>Bir csv dosyasından dataframe oluşturma

Uygulamalar dataframe'leri doğrudan Azure Depolama veya Azure Data Lake Storage’daki dosya veya klasörlerden; Hive tablosundan; ya da Spark tarafından desteklenen Cosmos DB, Azure SQL DB ve DW gibi diğer veri kaynaklarından oluşturabilir. Aşağıdaki ekran görüntüsünde, bu öğreticide kullanılan HVAC.csv dosyasının bir anlık görüntü gösterilmektedir. Csv dosyası, tüm HDInsight Spark kümeleriyle birlikte gelir. Veriler, bazı binaların sıcaklık varyasyonlarını yakalar.
    
![Etkileşimli Spark SQL sorgusu için verilerin anlık görüntüsü](./media/apache-spark-load-data-run-query/hdinsight-spark-sample-data-interactive-spark-sql-query.png "Etkileşimli Spark SQL sorgusu için verilerin anlık görüntüsü")


1. Önkoşullar bölümünde oluşturduğunuz Jupyter not defterini açın ve PySpark ile yeni bir not defteri oluşturun.

    > [!NOTE]  
    > PySpark çekirdeği kullanılarak not defteri oluşturmak için, ilk kod hücresini çalıştırdığınızda sizin için otomatik olarak `spark` oturumu oluşturulur. Belirtik şekilde bir oturum oluşturmanız gerekmez.

2. Aşağıdaki kodu, not defterindeki boş bir koda yapıştırın ve sonra kodu çalıştırmak için **SHIFT + ENTER** tuşlarına basın. Kod, bu senaryo için gerekli olan türleri içeri aktarır:

    ```python
    from pyspark.sql import *
    from pyspark.sql.types import *
    ```

    Jupyter’de etkileşimli bir sorgu çalıştırılırken web tarayıcısı penceresinde veya sekme açıklamalı alt yazısında not defteri başlığıyla birlikte **(Meşgul)** durumu gösterilir. Ayrıca sağ üst köşedeki **PySpark** metninin yanında içi dolu bir daire görürsünüz. İş tamamlandıktan sonra bu simge boş bir daireye dönüşür.

    ![Etkileşimli Spark SQL sorgusunun durumu](./media/apache-spark-load-data-run-query/hdinsight-spark-interactive-spark-query-status.png "Status of interactive Spark SQL query")

3. Aşağıdaki kodu çalıştırarak bir dataframe ve geçici bir tablo (**hvac**) oluşturun. 

    ```python
    # Create a dataframe and table from sample data
    csvFile = spark.read.csv('/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv', header=True, inferSchema=True)
    csvFile.write.saveAsTable("hvac")
    ```

## <a name="run-queries-on-the-dataframe"></a>Dataframe üzerinde sorgular çalıştırma

Tablo oluşturulduktan sonra veriler üzerinde etkileşimli bir sorgu çalıştırabilirsiniz.

1. Not defterinin boş bir hücresinde aşağıdaki kodu çalıştırın:

    ```sql
    %%sql
    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```

   Aşağıdaki tablo çıktısı görüntülenir.

     ![Etkileşimli Spark sorgu sonucunun tablo çıktısı](./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result.png "Table output of interactive Spark query result")

2. Sonuçları diğer görselleştirmelerde de görebilirsiniz. Aynı çıktı için bir alan grafiği görmek için **Alan**’ı seçin ve sonra gösterildiği gibi diğer değerleri ayarlayın.

    ![Etkileşimli Spark sorgu sonucunun alan grafiği](./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result-area-chart.png "Area graph of interactive Spark query result")

3. Not Defteri menü çubuğundan gidin **dosya** > **kaydetme ve denetim noktası**.

4. [Sonraki öğreticiyi](apache-spark-use-bi-tools.md) şimdi başlatıyorsanız, not defterini açık bırakın. Aksi takdirde, küme kaynaklarını serbest bırakmak için Not defterini kapatmanız: Not Defteri menü çubuğundan gidin **dosya** >  **Kapat ve Durdur**.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kullanımda olmadığında bir kümeyi güvenle silebilirsiniz HDInsight ile verileri ve Jupyter not defterleri Azure depolama veya Azure Data Lake Storage, depolanır. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır. Sonraki öğretici üzerinde hemen çalışmayı planlıyorsanız, kümeyi tutmak isteyebilirsiniz.

Azure portalında kümeyi açıp **Sil**’i seçin.

![HDInsight kümesini silme](./media/apache-spark-load-data-run-query/hdinsight-azure-portal-delete-cluster.png "HDInsight kümesini silme")

Kaynak grubu adını seçerek de kaynak grubu sayfasını açabilir ve sonra **Kaynak grubunu sil**’i seçebilirsiniz. Kaynak grubunu silerek hem HDInsight Spark kümesini hem de varsayılan depolama hesabını silersiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:
> [!div class="checklist"]
> * Bir Apache Spark dataframe oluşturun.
> * Dataframe’e karşı Spark SQL’i çalıştırma.

Power BI gibi bir BI analiz aracı Apache Spark, kayıtlı verileri nasıl çekilebilir görmek için sonraki makaleye ilerleyin. 
> [!div class="nextstepaction"]
> [BI araçlarını kullanarak verileri analiz etme](apache-spark-use-bi-tools.md)
