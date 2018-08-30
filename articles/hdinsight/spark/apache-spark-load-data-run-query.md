---
title: 'Öğretici: Azure HDInsight içindeki bir Apache Spark kümesinde veri yükleme ve sorgular çalıştırma '
description: Azure HDInsight içindeki Spark kümelerinde veri yüklemeyi ve etkileşimli sorgular çalıştırmayı öğrenin.
services: azure-hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,mvc
ms.topic: tutorial
ms.author: jasonh
ms.date: 05/17/2018
ms.openlocfilehash: d59f04c5dde522f3d193f345ac59147ece9d86f0
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43047566"
---
# <a name="tutorial-load-data-and-run-queries-on-an-apache-spark-cluster-in-azure-hdinsight"></a>Öğretici: Azure HDInsight içindeki bir Apache Spark kümesinde veri yükleme ve sorgular çalıştırma

Bu öğreticide, bir csv dosyasından nasıl dataframe oluşturulacağını ve Azure HDInsight içindeki bir Apache Spark kümesine karşı etkileşimli Spark SQL sorgularının nasıl çalıştırılacağını öğrenirsiniz. Spark’ta dataframe, adlandırılmış sütunlar halinde düzenlenmiş, dağıtılmış bir veri koleksiyonudur. Dataframe kavramsal olarak, ilişkisel bir veritabanındaki tabloya veya R/Python’daki veri çerçevesine eşdeğerdir.
 
Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Bir csv dosyasından dataframe oluşturma
> * Dataframe üzerinde sorgular çalıştırma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

* [Azure HDInsight’ta Apache Spark kümesi oluşturma](apache-spark-jupyter-spark-sql.md) bölümünü tamamlayın.

## <a name="create-a-dataframe-from-a-csv-file"></a>Bir csv dosyasından dataframe oluşturma

Uygulamalar, mevcut bir Esnek Dağıtımlı Veri Kümesinden (RDD), bir Hive tablosundan veya SQLContext nesnesini kullanan veri kaynaklarından dataframe’ler oluşturabilir. Aşağıdaki ekran görüntüsünde, bu öğreticide kullanılan HVAC.csv dosyasının bir anlık görüntü gösterilmektedir. Csv dosyası, tüm HDInsight Spark kümeleriyle birlikte gelir. Veriler, bazı binaların sıcaklık varyasyonlarını yakalar.
    
![Etkileşimli Spark SQL sorgusu için verilerin anlık görüntüsü](./media/apache-spark-load-data-run-query/hdinsight-spark-sample-data-interactive-spark-sql-query.png "Etkileşimli Spark SQL sorgusu için verilerin anlık görüntüsü")


1. Ön koşullar bölümünde oluşturduğunuz Jupyter not defterini açın.
2. Aşağıdaki kodu, not defterindeki boş bir koda yapıştırın ve sonra kodu çalıştırmak için **SHIFT + ENTER** tuşlarına basın. Kod, bu senaryo için gerekli olan türleri içeri aktarır:

    ```PySpark
    from pyspark.sql import *
    from pyspark.sql.types import *
    ```

    Jupyter’de etkileşimli bir sorgu çalıştırılırken web tarayıcısı penceresinde veya sekme açıklamalı alt yazısında not defteri başlığıyla birlikte **(Meşgul)** durumu gösterilir. Ayrıca sağ üst köşedeki **PySpark** metninin yanında içi dolu bir daire görürsünüz. İş tamamlandıktan sonra bu simge boş bir daireye dönüşür.

    ![Etkileşimli Spark SQL sorgusunun durumu](./media/apache-spark-load-data-run-query/hdinsight-spark-interactive-spark-query-status.png "Status of interactive Spark SQL query")

3. Aşağıdaki kodu çalıştırarak bir dataframe ve geçici bir tablo (**hvac**) oluşturun. 

    ```PySpark
    # Create an RDD from sample data
    csvFile = spark.read.csv('wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv', header=True, inferSchema=True)
    csvFile.write.saveAsTable("hvac")
    ```

    > [!NOTE]
    > Not defteri oluşturmak için PySpark çekirdeği kullanılarak, ilk kod hücresini çalıştırdığınızda sizin için otomatik olarak SQL bağlamları oluşturulur. Belirtik şekilde bir bağlam oluşturmanız gerekmez.


## <a name="run-queries-on-the-dataframe"></a>Dataframe üzerinde sorgular çalıştırma

Tablo oluşturulduktan sonra veriler üzerinde etkileşimli bir sorgu çalıştırabilirsiniz.

1. Not defterinin boş bir hücresinde aşağıdaki kodu çalıştırın:

    ```PySpark
    %%sql
    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```

   Not defterinde PySpark çekirdeği kullanıldığından, artık doğrudan **hvac** geçici tablosunda etkileşimli bir SQL sorgusu çalıştırabilirsiniz.

   Aşağıdaki tablo çıktısı görüntülenir.

     ![Etkileşimli Spark sorgu sonucunun tablo çıktısı](./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result.png "Table output of interactive Spark query result")

3. Sonuçları diğer görselleştirmelerde de görebilirsiniz. Aynı çıktı için bir alan grafiği görmek için **Alan**’ı seçin ve sonra gösterildiği gibi diğer değerleri ayarlayın.

    ![Etkileşimli Spark sorgu sonucunun alan grafiği](./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result-area-chart.png "Area graph of interactive Spark query result")

10. Not defterindeki **Dosya** menüsünden **Kaydet ve Denetim Noktası**’nı seçin. 

11. [Sonraki öğreticiyi](apache-spark-use-bi-tools.md) şimdi başlatıyorsanız, not defterini açık bırakın. Aksi takdirde, not defterini kapatarak küme kaynaklarını yayınlayın: Not defterindeki **Dosya** menüsünden **Kaydet ve Durdur**’u seçin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

HDInsight ile verileriniz, Azure Depolama’da veya Azure Data Lake Store’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır. Sonraki öğretici üzerinde hemen çalışmayı planlıyorsanız, kümeyi tutmak isteyebilirsiniz.

Azure portalında kümeyi açıp **Sil**’i seçin.

![HDInsight kümesini silme](./media/apache-spark-load-data-run-query/hdinsight-azure-portal-delete-cluster.png "HDInsight kümesini silme")

Kaynak grubu adını seçerek de kaynak grubu sayfasını açabilir ve sonra **Kaynak grubunu sil**’i seçebilirsiniz. Kaynak grubunu silerek hem HDInsight Spark kümesini hem de varsayılan depolama hesabını silersiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

* Spark dataframe oluşturma.
* Dataframe’e karşı Spark SQL’i çalıştırma.

Spark’a kaydettiğiniz verilerin Power BI gibi bir BI analiz aracına nasıl çekilebileceğini görmek için sonraki makaleye ilerleyin. 
> [!div class="nextstepaction"]
> [BI araçlarını kullanarak verileri analiz etme](apache-spark-use-bi-tools.md)

