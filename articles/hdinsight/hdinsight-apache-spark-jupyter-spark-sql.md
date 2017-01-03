---
title: "HDInsight Linux üzerinde bir Spark kümesi oluşturma ve etkileşimli analiz için Jupyter tarafından sağlanan Spark SQL kullanma | Microsoft Belgeleri"
description: "HDInsight’ta bir Apache Spark kümesini hızlıca oluşturmaya ve ardından etkileşimli sorgular gerçekleştirmek üzere Jupyter not defterlerinden Spark SQL’i kullanmaya ilişkin adım adım yönergeler."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 91f41e6a-d463-4eb4-83ef-7bbb1f4556cc
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/28/2016
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 8056e7ece1942c9090a7c36447a96829febaf1a4
ms.openlocfilehash: a6d632ed43580b9773c0e416e22a743f8bc60834


---
# <a name="get-started-create-apache-spark-cluster-on-hdinsight-linux-and-run-interactive-queries-using-spark-sql"></a>Başlangıç: HDInsight Linux üzerinde Apache Spark kümesi oluşturma ve Spark SQL kullanarak etkileşimli sorgular gerçekleştirme
HDInsight’ta bir Apache Spark kümesi oluşturmayı ve ardından [Jupyter](https://jupyter.org) not defteri kullanarak Spark kümesi üzerinde Spark SQL etkileşimli sorguları gerçekleştirmeyi öğrenin.

   ![HDInsight'ta Apache Spark'ı kullanmaya başlayın](./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.getstartedflow.png "HDInsight'ta Apache Spark'ı kullanmaya başlama öğreticisi. Gösterilen adımlar: depolama hesabı oluşturma; küme oluşturma; Spark SQL deyimi çalıştırma")

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Ön koşullar
* **Bir Azure aboneliği**. Bu öğreticiye başlamadan önce bir Azure aboneliğinizin olması gerekir. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Güvenli Kabuk (SSH) istemcisi**: Linux, Unix ve OS X sistemleri `ssh` komutu ile bir SSH istemcisi sağlar. Windows sistemleri için [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) önerilir.
* **Secure Shell (SSH) anahtarları (isteğe bağlı)**: Bir parola veya ortak anahtar kullanarak, kümeye bağlanmak için kullanılan SSH hesabını güvenli hale getirebilirsiniz. Parola kullanmak hızlıca başlamanızı sağlar ve bir kümeyi hızlıca oluşturup bazı test işlemleri gerçekleştirmek isterseniz bu seçeneği kullanmanız gerekir. Anahtar kullanmak daha güvenlidir, ancak ek kurulum gerektirir. Bir üretim kümesi oluştururken bu yaklaşımı kullanmak isteyebilirsiniz. Bu makalede parola yaklaşımı kullanılmaktadır. HDInsight ile SSH anahtarları oluşturma ve kullanma hakkında yönergeler için aşağıdaki makalelere bakın:

  * Bir Linux bilgisayardan - [SSH’yi Linux, Unix veya OS X işletim sistemlerinde Linux tabanlı HDInsight (Hadoop) ile birlikte kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).
  * Bir Windows bilgisayardan - [SSH’yi Windows işletim sisteminde Linux tabanlı HDInsight (Hadoop) ile birlikte kullanma](hdinsight-hadoop-linux-use-ssh-windows.md).

> [!NOTE]
> Bu makalede [küme depolama birimi olarak Azure Depolama Blobları](hdinsight-hadoop-use-blob-storage.md) kullanan bir Spark kümesi oluşturmak üzere Azure Resource Manager şablonu kullanılmaktadır. Ayrıca, varsayılan depolama birimi olan Azure Depolama Bloblarına ek olarak [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) kullanan bir Spark kümesi oluşturabilirsiniz. Yönergeler için bkz. [Data Lake Store ile HDInsight kümesi oluşturma](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).
>
>

### <a name="access-control-requirements"></a>Erişim denetimi gereksinimleri
[!INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-spark-cluster"></a>Spark kümesi oluşturma
Bu bölümde Azure Resource Manager şablonunu kullanarak bir HDInsight sürüm 3.4 kümesi (Spark 1.6.1 sürümü) oluşturursunuz. HDInsight sürümleri ve SLA’ları hakkında bilgi için bkz. [HDInsight bileşen sürümü oluşturma](hdinsight-component-versioning.md). Diğer küme oluşturma yöntemleri için bkz. [HDInsight kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md).

1. Azure Portal'da bir şablonu açmak için aşağıdaki görüntüye tıklayın.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-spark-cluster-in-hdinsight.json" target="_blank"><img src="./media/hdinsight-apache-spark-jupyter-spark-sql/deploy-to-azure.png" alt="Deploy to Azure"></a>

    Şablon bir ortak blob kapsayıcısında, *https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-spark-cluster-in-hdinsight.json* bulunur.
2. Parametreler dikey penceresinde aşağıdakileri girin:

   * **ClusterName**: Oluşturacağınız Hadoop kümesi için bir ad girin.
   * **Küme oturum açma adı ve parolası**: Varsayılan oturum açma adı admin şeklindedir.
   * **SSH kullanıcı adı ve parola**.

     Lütfen bu değerleri yazın.  Öğreticide daha sonra bunlara ihtiyacınız olacaktır.

     > [!NOTE]
     > SSH bir komut satırı kullanarak HDInsight’a uzaktan erişmek için kullanılır. Burada kullandığınız kullanıcı adı ve parola, kümeye SSH üzerinden bağlanırken kullanılır. Ayrıca, SSH kullanıcı adı tüm HDInsight küme düğümlerinde bir kullanıcı hesabı oluşturduğundan benzersiz olmalıdır. Kümedeki hizmetlerin kullanımı için ayrılmış olup SSH kullanıcı adı olarak kullanılamayan hesap adlarının bazıları aşağıda verilmiştir:
     >
     > root, hdiuser, storm, hbase, ubuntu, zookeeper, hdfs, yarn, mapred, hbase, hive, oozie, falcon, sqoop, admin, tez, hcat, hdinsight-zookeeper.
     >
     > HDInsight ile SSH kullanma hakkında daha fazla bilgi için aşağıdaki makalelerden birine bakın:
     >
     > * [Linux, Unix ya da OS X’te HDInsight’ta Linux tabanlı Hadoop ile SSH’yi kullanma](hdinsight-hadoop-linux-use-ssh-unix.md)
     > * [Windows’da HDInsight’ta Linux tabanlı Hadoop ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-windows.md)
     >
     >

3. Parametreleri kaydetmek için **Tamam**’a tıklayın.

4. **Özel dağıtım** dikey penceresinde **Kaynak grubu** açılır kutusuna ve ardından **Yeni**’ye tıklayarak yeni bir kaynak grubu oluşturun. Kaynak grubu; küme, bağımlı depolama hesabını ve diğer bağlı kaynağı gruplandıran bir kapsayıcıdır.

5. **Yasal koşullar**’a ve ardından **Oluştur**’a tıklayın.

6. **Oluştur**’a tıklayın. Şablon dağıtımı için Dağıtım gönderme başlıklı yeni bir kutucuk görürsünüz. Kümenin ve SQL veritabanının oluşturulması yaklaşık 20 dakika sürer.

## <a name="run-spark-sql-queries-using-a-jupyter-notebook"></a>Jupyter not defteri kullanarak Spark SQL sorguları çalıştırma
Bu bölümde, Spark kümesine yönelik Spark SQL sorguları çalıştırmak için Jupyter not defteri kullanırsınız. HDInsight Spark kümeleri Jupyter not defteri ile kullanabileceğiniz iki çekirdek sağlar. Bunlar:

* **PySpark** (Python içinde yazılmış uygulamalar için)
* **Spark** (Scala içinde yazılmış uygulamalar için)

Bu makalede PySpark çekirdeği kullanılacaktır. [Spark HDInsight kümeleri ile Jupyter not defterlerinde kullanılabilen çekirdekler](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-pyspark-or-spark-kernels) makalesinde PySpark çekirdeğini kullanmanın yararları hakkında ayrıntılı bilgi bulabilirsiniz. Ancak, PySpark çekirdeği kullanmanın birkaç avantajı şunlardır:

* Spark ve Hive bağlamlarını ayarlamanız gerekmez. Bunlar sizin için otomatik olarak ayarlanır.
* SQL veya Hive sorgularınızı önceki kod parçacıkları olmadan doğrudan çalıştırmak için `%%sql` gibi hücre işlevlerini kullanabilirsiniz.
* SQL veya Hive sorgularının çıktıları otomatik olarak gösterilir.

### <a name="create-jupyter-notebook-with-pyspark-kernel"></a>PySpark çekirdeği ile Jupyter not defteri oluşturma
1. [Azure portalındaki](https://portal.azure.com/) başlangıç panosunda Spark kümenizin kutucuğuna tıklayın (başlangıç panosuna sabitlediyseniz). Ayrıca **Browse All (Tümüne Gözat)** > **HDInsight Clusters (HDInsight Kümeleri)** altından kümenize gidebilirsiniz.   
2. Spark kümesi dikey penceresinden **Küme Panosu**’na ve ardından **Jupyter Notebook**’a tıklayın. İstenirse, küme için yönetici kimlik bilgilerini girin.

   > [!NOTE]
   > Aşağıdaki URL’yi tarayıcınızda açarak da Jupyter Notebook’a ulaşabilirsiniz. **CLUSTERNAME** değerini kümenizin adıyla değiştirin:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Yeni bir not defteri oluşturun. **Yeni** ve ardından **PySpark** seçeneğine tıklayın.

    ![Yeni bir Jupyter not defteri oluşturma](./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.createnotebook.png "Yeni bir Jupyter not defteri oluşturma")
4. Yeni bir not defteri oluşturulur ve Untitled.pynb adı ile açılır. Üstteki not defteri adına tıklayın ve kolay bir ad girin.

    ![Not defteri adını belirtme](./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.notebook.name.png "Not defteri adını belirtme")
5. PySpark çekirdeği kullanarak bir not defteri oluşturduğunuz için açıkça bir bağlam oluşturmanız gerekmez. Birinci kod hücresini çalıştırdığınızda Spark ve Hive bağlamları sizin için otomatik olarak oluşturulur. Bu senaryo için gereken türleri içeri aktararak işleme başlayabilirsiniz. Bunu yapmak için aşağıdaki kod parçacığını bir hücreye yapıştırın ve **SHIFT + ENTER** tuşuna basın.

        from pyspark.sql.types import *

    Jupyter’de bir işi her çalıştırdığınızda web tarayıcınızın pencere başlığında not defteri başlığı ile birlikte **(Meşgul)** durumu gösterilir. Ayrıca sağ üst köşedeki **PySpark** metninin yanında içi dolu bir daire görürsünüz. İş tamamlandıktan sonra bu simge boş bir daireye dönüşür.

     ![Jupyter not defteri işinin durumu](./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.jupyter.job.status.png "Jupyter not defteri işinin durumu")
6. Örnek verilerini geçici bir tabloya yükleyin. HDInsight’ta bir Spark kümesi oluşturduğunuzda **hvac.csv** örnek veri dosyası, **\HdiSamples\HdiSamples\SensorSampleData\hvac** altındaki ilişkili depolama hesabına kopyalanır.

    Boş bir hücreye aşağıdaki kod örneğini yapıştırın ve **SHIFT + ENTER** tuşlarına basın. Bu kod örneği, verileri **hvac** adlı geçici bir tabloya kaydeder.

        # Load the data
        hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        # Create the schema
        hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])

        # Parse the data in hvacText
        hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))

        # Create a data frame
        hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)

        # Register the data fram as a table to run queries against
        hvacdf.registerTempTable("hvac")
7. Bir PySpark çekirdeği kullandığınız için `%%sql` sihrini kullanarak yeni oluşturduğunuz **hvac** geçici tablosunda bundan böyle bir SQL sorgusunu doğrudan çalıştırabilirsiniz. `%%sql` sihrinin yanı sıra PySpark çekirdeği kullanılabilen diğer sihirler hakkında daha fazla bilgi için bkz. [Spark HDInsight kümeleri ile Jupyter not defterlerinde kullanılabilen çekirdekler](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-pyspark-or-spark-kernels).

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
8. İş başarıyla tamamlandıktan sonra varsayılan olarak aşağıdaki tablo çıktısı görüntülenir.

     ![Sorgu sonucunun tablo çıktısı](./media/hdinsight-apache-spark-jupyter-spark-sql/tabular.output.png "Sorgu sonucunun tablo çıktısı")

    Sonuçları diğer görselleştirmelerde de görebilirsiniz. Örneğin, aynı çıktı için bir alan grafiği aşağıdaki gibi görünür.

    ![Sorgu sonucunun alan grafiği](./media/hdinsight-apache-spark-jupyter-spark-sql/area.output.png "Sorgu sonucunun alan grafiği")
9. Uygulamayı çalıştırmayı tamamladıktan sonra kaynakları serbest bırakmak için not defterini kapatmanız gerekir. Bunu yapmak için not defterindeki **Dosya** menüsünde **Kapat ve Durdur**’a tıklayın. Bunun yapılması not defterini kapatır.

## <a name="delete-the-cluster"></a>Küme silme
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="see-also"></a>Ayrıca bkz.
* [Genel Bakış: Azure HDInsight’ta Apache Spark](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar
* [BI ile Spark: BI araçlarıyla HDInsight’ta Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](hdinsight-apache-spark-use-bi-tools.md)
* [Machine Learning ile Spark: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight’ta Spark kullanma](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Spark: Yemek inceleme sonuçlarını tahmin etmek için HDInsight’ta Spark kullanma](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Akış: Gerçek zamanlı akış uygulamaları oluşturmak için HDInsight’ta Spark kullanma](hdinsight-apache-spark-eventhub-streaming.md)
* [HDInsight’ta Spark kullanarak Web sitesi günlüğü çözümlemesi](hdinsight-apache-spark-custom-library-website-log-analysis.md)
* [HDInsight’ta Spark kullanarak Application Insight telemetri verilerinin analizi](hdinsight-spark-analyze-application-insight-logs.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma
* [Scala kullanarak tek başına uygulama oluşturma](hdinsight-apache-spark-create-standalone-application.md)
* [Livy kullanarak Spark kümesinde işleri uzaktan çalıştırma](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar
* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisi kullanma](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Spark uygulamalarında uzaktan hata ayıklamak amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisi kullanma](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight’ta Spark kümesi ile Zeppelin not defterlerini kullanma](hdinsight-apache-spark-use-zeppelin-notebook.md)
* [HDInsight için Spark kümesinde Jupyter not defteri için kullanılabilir çekirdekler](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](hdinsight-apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md



<!--HONumber=Jan17_HO1-->


