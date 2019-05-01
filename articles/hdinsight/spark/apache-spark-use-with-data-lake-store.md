---
title: Azure Data Lake depolama Gen1 verileri çözümlemek için Apache Spark'ı kullanın
description: Azure Data Lake depolama Gen1 depolanan verileri analiz için Spark işleri çalıştırma
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/21/2018
ms.openlocfilehash: 688b87fcc0ec18e0bf5924470d521c44229ae421
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64682880"
---
# <a name="use-hdinsight-spark-cluster-to-analyze-data-in-data-lake-storage-gen1"></a>Data Lake depolama Gen1 verilerini çözümlemek için HDInsight Spark kümesi kullanın

Bu öğreticide kullandığınız [Jupyter not defteri](https://jupyter.org/) bir Data Lake Store hesabından veri okuyan bir iş çalıştırmak için HDInsight Spark kümeleri ile kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar

* Azure Data Lake depolama Gen1 hesabı. Konumundaki yönergeleri [Azure Data Lake depolama Gen1 ile çalışmaya başlama Azure portalını kullanarak](../../data-lake-store/data-lake-store-get-started-portal.md).

* Azure HDInsight Spark kümesi ile depolama alanı olarak Data Lake depolama Gen1. Konumundaki yönergeleri [hızlı başlangıç: HDInsight kümelerinde ayarlama](../../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).

    
## <a name="prepare-the-data"></a>Verileri hazırlama

> [!NOTE]  
> Varsayılan depolama alanı olarak Data Lake Store ile HDInsight küme oluşturduysanız bu adımı gerçekleştirmeniz gerekmez. Küme oluşturma işlemi bazı örnek veriler, kümeyi oluştururken belirttiğiniz Data Lake Store hesabına ekler. Data Lake Storage bölüm kullan HDInsight Spark kümesiyle bölümüne atlayın.

Ek depolama alanı ve varsayılan depolama alanı olarak Azure depolama blobu olarak Data Lake Store ile HDInsight kümesi oluşturduysanız, Data Lake Storage hesabına önce bazı örnek veriler üzerinde kopyalamanız gerekir. Azure depolama Blob verileri HDInsight kümesi ile ilişkili örnek kullanabilirsiniz. Kullanabileceğiniz [ADLCopy aracı](https://aka.ms/downloadadlcopy) Bunu yapmak için. İndirin ve aracı bağlantıdan yükleyin.

1. Bir komut istemi açın ve AdlCopy yüklü olduğu, genellikle dizine gidin `%HOMEPATH%\Documents\adlcopy`.

2. Belirli bir blobu kaynak kapsayıcısından Data Lake depolamaya kopyalamak için aşağıdaki komutu çalıştırın:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    Kopyalama **HVAC.csv** örnek veri dosyası **/HdiSamples/HdiSamples/SensorSampleData/hvac/** Azure Data Lake Storage hesabına. Kod parçacığı gibi görünmelidir:

        AdlCopy /Source https://mydatastore.blob.core.windows.net/mysparkcluster/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv /dest swebhdfs://mydatalakestore.azuredatalakestore.net/hvac/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

   > [!WARNING]  
   > Dosya ve yol adlarını uygun büyük/küçük harf kullandığınızdan emin olun.

3. Data Lake Store hesabınızın altında sahip Azure aboneliği için kimlik bilgilerini girmeniz istenir. Aşağıdaki kod parçacığına benzer bir çıkış görürsünüz:

        Initializing Copy.
        Copy Started.
        100% data copied.
        Copy Completed. 1 file copied.

    Veri dosyası (**HVAC.csv**) altında bir klasöre kopyalanacak **/hvac** Data Lake Storage hesabında.

## <a name="use-an-hdinsight-spark-cluster-with-data-lake-storage-gen1"></a>Bir HDInsight Spark kümesi ile Data Lake depolama Gen1 kullanın

1. Gelen [Azure portalı](https://portal.azure.com/), (Bu başlangıç panosuna sabitlediğiniz varsa) başlangıç Panosu, Apache Spark kümenizin kutucuğuna tıklayın. Ayrıca **Browse All (Tümüne Gözat)** > **HDInsight Clusters (HDInsight Kümeleri)** altından kümenize gidebilirsiniz.

2. Spark kümesi dikey penceresinden **Hızlı Bağlantılar**’a ve sonra **Küme Panosu** dikey penceresinden **Jupyter Not Defteri**’ne tıklayın. İstenirse, küme için yönetici kimlik bilgilerini girin.

   > [!NOTE]  
   > Aşağıdaki URL’yi tarayıcınızda açarak da Jupyter Notebook’a ulaşabilirsiniz. **CLUSTERNAME** değerini kümenizin adıyla değiştirin:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

3. Yeni bir not defteri oluşturun. **Yeni** ve ardından **PySpark** seçeneğine tıklayın.

    ![Yeni bir Jupyter not defteri oluşturma](./media/apache-spark-use-with-data-lake-store/hdinsight-create-jupyter-notebook.png "Yeni bir Jupyter not defteri oluşturma")

4. PySpark çekirdeği kullanarak bir not defteri oluşturduğunuz için açıkça bir bağlam oluşturmanız gerekmez. Birinci kod hücresini çalıştırdığınızda Spark ve Hive bağlamları sizin için otomatik olarak oluşturulur. Bu senaryo için gereken türleri içeri aktararak işleme başlayabilirsiniz. Bunu yapmak için aşağıdaki kod parçacığını bir hücreye yapıştırın ve **SHIFT + ENTER** tuşuna basın.

        from pyspark.sql.types import *

    Jupyter’de bir işi her çalıştırdığınızda web tarayıcınızın pencere başlığında not defteri başlığı ile birlikte **(Meşgul)** durumu gösterilir. Ayrıca sağ üst köşedeki **PySpark** metninin yanında içi dolu bir daire görürsünüz. İş tamamlandıktan sonra bu simge boş bir daireye dönüşür.

     ![Jupyter not defteri işinin durumu](./media/apache-spark-use-with-data-lake-store/hdinsight-jupyter-job-status.png "Jupyter not defteri işinin durumu")

5. Örnek verileri kullanarak bir geçici tablosuna yükleme **HVAC.csv** dosya Data Lake depolama Gen1 hesabına kopyalanır. Aşağıdaki URL deseni kullanarak Data Lake Store hesabına verilerine erişebilir.

   * Varsayılan depolama alanı olarak Data Lake depolama Gen1 varsa HVAC.csv yolda aşağıdaki URL'ye benzer olacaktır:

           adl://<data_lake_store_name>.azuredatalakestore.net/<cluster_root>/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv

       Ya da kısaltılmış bir biçimi aşağıdaki gibi kullanabilirsiniz:

           adl:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv

   * Ek depolama alanı olarak Data Lake Storage varsa, gibi kopyaladığınız konuma HVAC.csv olacaktır:

           adl://<data_lake_store_name>.azuredatalakestore.net/<path_to_file>

     Boş bir hücreye aşağıdaki kod örneği yapıştırın, yerine **MYDATALAKESTORE** Data Lake Store hesap adını ve basın **SHIFT + ENTER**. Bu kod örneği, verileri **hvac** adlı geçici bir tabloya kaydeder.

           # Load the data. The path below assumes Data Lake Storage is default storage for the Spark cluster
           hvacText = sc.textFile("adl://MYDATALAKESTORE.azuredatalakestore.net/cluster/mysparkcluster/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

           # Create the schema
           hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])

           # Parse the data in hvacText
           hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))

           # Create a data frame
           hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)

           # Register the data fram as a table to run queries against
           hvacdf.registerTempTable("hvac")

6. Bir PySpark çekirdeği kullandığınız için `%%sql` sihrini kullanarak yeni oluşturduğunuz **hvac** geçici tablosunda bundan böyle bir SQL sorgusunu doğrudan çalıştırabilirsiniz. Hakkında daha fazla bilgi için `%%sql` Sihirli yanı sıra PySpark çekirdeği kullanılabilen diğer sihirler bkz [Apache Spark HDInsight kümeleri ile Jupyter not defterlerinde kullanılabilen çekirdekler](apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

7. İş başarıyla tamamlandıktan sonra varsayılan olarak aşağıdaki tablo çıktısı görüntülenir.

      ![Sorgu sonucunun tablo çıktısı](./media/apache-spark-use-with-data-lake-store/jupyter-tabular-output.png "Sorgu sonucunun tablo çıktısı")

     Sonuçları diğer görselleştirmelerde de görebilirsiniz. Örneğin, aynı çıktı için bir alan grafiği aşağıdaki gibi görünür.

     ![Sorgu sonucunun alan grafiği](./media/apache-spark-use-with-data-lake-store/jupyter-area-output.png "Sorgu sonucunun alan grafiği")

8. Uygulamayı çalıştırmayı tamamladıktan sonra kaynakları serbest bırakmak için not defterini kapatmanız gerekir. Bunu yapmak için not defterindeki **Dosya** menüsünde **Kapat ve Durdur**’a tıklayın. Bunun yapılması not defterini kapatır.


## <a name="next-steps"></a>Sonraki adımlar

* [Tek başına bir Apache Spark kümesi üzerinde bir Scala uygulama oluşturma](apache-spark-create-standalone-application.md)
* [HDInsight Spark Linux kümesi için Apache Spark uygulamaları oluşturmak için Intellij için Azure araç seti, HDInsight araçları kullanma](apache-spark-intellij-tool-plugin.md)
* [HDInsight Spark Linux kümesi için Apache Spark uygulamaları oluşturmak için Eclipse için Azure araç seti, HDInsight araçları kullanma](apache-spark-eclipse-tool-plugin.md)
* [Azure Data Lake depolama Gen2 Azure HDInsight kümeleri ile kullanma](../hdinsight-hadoop-use-data-lake-storage-gen2.md)
