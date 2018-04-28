---
title: Azure Data Lake Store'da verileri çözümlemek için Apache Spark kullanın | Microsoft Docs
description: Azure Data Lake Store içinde depolanan verileri çözümlemek için Spark işleri çalıştırma
services: hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 1f174323-c17b-428c-903d-04f0e272784c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: nitinme
ms.openlocfilehash: fa6f93231cba46e29206ec312fb82ad120ed45f6
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="use-hdinsight-spark-cluster-to-analyze-data-in-data-lake-store"></a>Data Lake Store'da verileri çözümlemek üzere Hdınsight Spark kümesi kullanın

Bu öğreticide, Jupyter not defteri kullanılabilir Hdınsight Spark kümeleri ile bir Data Lake Store hesabından veri okuyan bir işi çalıştırmak için kullanın.

## <a name="prerequisites"></a>Önkoşullar

* Azure Data Lake Store hesabı. [Azure portalını kullanarak Azure Data Lake Store ile çalışmaya başlama](../../data-lake-store/data-lake-store-get-started-portal.md) bölümündeki yönergeleri uygulayın.

* Azure Hdınsight Spark, depolama alanı olarak Data Lake Store ile küme. Bölümündeki yönergeleri izleyin [Azure portalını kullanarak Data Lake Store ile bir Hdınsight kümesi oluşturmayı](../../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

    
## <a name="prepare-the-data"></a>Verileri hazırlama

> [!NOTE]
> Varsayılan depolama olarak Data Lake Store ile Hdınsight küme oluşturduysanız bu adımı gerçekleştirmeniz gerekmez. Küme oluşturma işlemi bazı örnek veri kümesi oluşturulurken belirtin Data Lake Store hesabındaki ekler. Atla bölümüne [Data Lake Store ile kullanımı Hdınsight Spark kümesi](#use-an-hdinsight-spark-cluster-with-data-lake-store).
>
>

Ek depolama alanı ve varsayılan depolama alanı olarak Azure Storage Blobuna olarak Data Lake Store ile Hdınsight kümesi oluşturduysanız, Data Lake Store hesabına önce bazı örnek veriler üzerinde kopyalamanız gerekir. Azure depolama Blob verileri Hdınsight kümesi ile ilişkili örneği kullanabilirsiniz. Kullanabileceğiniz [ADLCopy aracı](http://aka.ms/downloadadlcopy) Bunu yapmak için. Karşıdan yükleme ve aracı bağlantıdan yükleyin.

1. Bir komut istemi açın ve AdlCopy yüklü olduğu, genellikle dizinine gidin `%HOMEPATH%\Documents\adlcopy`.

2. Belirli bir blobu bir Data Lake Store'a kaynak kapsayıcıdan kopyalamak için aşağıdaki komutu çalıştırın:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    Kopya **HVAC.csv** örnek veri dosyası konumunda **/HdiSamples/HdiSamples/SensorSampleData/hvac/** Azure Data Lake Store hesabı. Kod parçacığı gibi görünmelidir:

        AdlCopy /Source https://mydatastore.blob.core.windows.net/mysparkcluster/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv /dest swebhdfs://mydatalakestore.azuredatalakestore.net/hvac/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

   > [!WARNING]
   > Dosya ve yol adlarının doğru durumda olduğundan emin olun.
   >
   >
3. Data Lake Store hesabınızın altında elinizde Azure aboneliği için kimlik bilgilerini girmeniz istenir. Aşağıdaki kod parçacığına benzer bir çıkış görürsünüz:

        Initializing Copy.
        Copy Started.
        100% data copied.
        Copy Completed. 1 file copied.

    Veri dosyası (**HVAC.csv**) altında bir klasöre kopyalanacak **/hvac** Data Lake Store hesabındaki.

## <a name="use-an-hdinsight-spark-cluster-with-data-lake-store"></a>Hdınsight Spark kümesinde Data Lake Store ile kullanma

1. [Azure Portal](https://portal.azure.com/)’daki başlangıç panosunda Spark kümenizin kutucuğuna tıklayın (başlangıç panosuna sabitlediyseniz). Ayrıca **Tüm** > **HDInsight Kümelerine Gözat** altından kümenize gidebilirsiniz.

2. Spark kümesi dikey penceresinden **Hızlı Bağlantılar**’a ve sonra **Küme Panosu** dikey penceresinden **Jupyter Not Defteri**’ne tıklayın. İstenirse, küme için yönetici kimlik bilgilerini girin.

   > [!NOTE]
   > Aşağıdaki URL’yi tarayıcınızda açarak da Jupyter Notebook’a ulaşabilirsiniz. **CLUSTERNAME** değerini kümenizin adıyla değiştirin:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >

3. Yeni bir not defteri oluşturun. **Yeni** ve ardından **PySpark** seçeneğine tıklayın.

    ![Yeni bir Jupyter not defteri oluşturma](./media/apache-spark-use-with-data-lake-store/hdinsight-create-jupyter-notebook.png "Yeni bir Jupyter not defteri oluşturma")

4. PySpark çekirdeği kullanarak bir not defteri oluşturduğunuz için açıkça bir bağlam oluşturmanız gerekmez. Birinci kod hücresini çalıştırdığınızda Spark ve Hive bağlamları sizin için otomatik olarak oluşturulur. Bu senaryo için gereken türleri içeri aktararak işleme başlayabilirsiniz. Bunu yapmak için aşağıdaki kod parçacığını bir hücreye yapıştırın ve **SHIFT + ENTER** tuşuna basın.

        from pyspark.sql.types import *

    Jupyter’de bir işi her çalıştırdığınızda web tarayıcınızın pencere başlığında not defteri başlığı ile birlikte **(Meşgul)** durumu gösterilir. Ayrıca sağ üst köşedeki **PySpark** metninin yanında içi dolu bir daire görürsünüz. İş tamamlandıktan sonra bu simge boş bir daireye dönüşür.

     ![Jupyter not defteri işinin durumu](./media/apache-spark-use-with-data-lake-store/hdinsight-jupyter-job-status.png "Jupyter not defteri işinin durumu")

5. Bir geçici tablo kullanarak örnek verileri yükleme **HVAC.csv** dosya Data Lake Store hesabına kopyalanır. Aşağıdaki URL deseni kullanarak Data Lake Store hesabındaki verilere erişebilir.

    * Data Lake Store varsayılan depolama varsa, HVAC.csv aşağıdaki URL'ye benzer yolundaki olacaktır:

            adl://<data_lake_store_name>.azuredatalakestore.net/<cluster_root>/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv

        Ya da aşağıdaki gibi kısaltılmış bir biçimi de kullanabilirsiniz:

            adl:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv

    * Data Lake Store ek depolama alanı olarak varsa, HVAC.csv, gibi kopyaladığınız konumda olacaktır:

            adl://<data_lake_store_name>.azuredatalakestore.net/<path_to_file>

     Aşağıdaki kod örneğinde boş bir hücreye yapıştırın, yerine **MYDATALAKESTORE** Data Lake Store hesap adı ve basın **SHIFT + ENTER**. Bu kod örneği, verileri **hvac** adlı geçici bir tabloya kaydeder.

            # Load the data. The path below assumes Data Lake Store is default storage for the Spark cluster
            hvacText = sc.textFile("adl://MYDATALAKESTORE.azuredatalakestore.net/cluster/mysparkcluster/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

            # Create the schema
            hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])

            # Parse the data in hvacText
            hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))

            # Create a data frame
            hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)

            # Register the data fram as a table to run queries against
            hvacdf.registerTempTable("hvac")

6. Bir PySpark çekirdeği kullandığınız için `%%sql` sihrini kullanarak yeni oluşturduğunuz **hvac** geçici tablosunda bundan böyle bir SQL sorgusunu doğrudan çalıştırabilirsiniz. `%%sql` sihrinin yanı sıra PySpark çekirdeği kullanılabilen diğer sihirler hakkında daha fazla bilgi için bkz. [Spark HDInsight kümeleri ile Jupyter not defterlerinde kullanılabilen çekirdekler](apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

7. İş başarıyla tamamlandıktan sonra varsayılan olarak aşağıdaki tablo çıktısı görüntülenir.

      ![Sorgu sonucunun tablo çıktısı](./media/apache-spark-use-with-data-lake-store/jupyter-tabular-output.png "Sorgu sonucunun tablo çıktısı")

     Sonuçları diğer görselleştirmelerde de görebilirsiniz. Örneğin, aynı çıktı için bir alan grafiği aşağıdaki gibi görünür.

     ![Sorgu sonucunun alan grafiği](./media/apache-spark-use-with-data-lake-store/jupyter-area-output.png "Sorgu sonucunun alan grafiği")

8. Uygulamayı çalıştırmayı tamamladıktan sonra kaynakları serbest bırakmak için not defterini kapatmanız gerekir. Bunu yapmak için not defterindeki **Dosya** menüsünde **Kapat ve Durdur**’a tıklayın. Bunun yapılması not defterini kapatır.


## <a name="next-steps"></a>Sonraki adımlar

* [Tek başına bir Apache Spark küme üzerinde çalışacak şekilde Scala uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Hdınsight araçları Intellij için Azure Araç Seti Spark Hdınsight Spark Linux kümesi için uygulamalar oluşturmak için kullanın](apache-spark-intellij-tool-plugin.md)
* [Hdınsight araçları Eclipse için Azure Araç Seti Spark Hdınsight Spark Linux kümesi için uygulamalar oluşturmak için kullanın](apache-spark-eclipse-tool-plugin.md)
