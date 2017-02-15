---
title: "Azure HDInsight&quot;ta bir Spark kümesi oluşturma ve etkileşimli analiz için Jupyter tarafından sağlanan Spark SQL kullanma | Microsoft Docs"
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
ms.date: 01/06/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 791b6a5a07bb87302cb382290a355c9a14c63ff0
ms.openlocfilehash: cc1d484d40dce0b1c64f2e8cdebb9377a38705cb


---
# <a name="get-started-create-apache-spark-cluster-in-azure-hdinsight-and-run-interactive-queries-using-spark-sql"></a>Başlangıç: Azure HDInsight'ta Apache Spark kümesi oluşturma ve Spark SQL kullanarak etkileşimli sorgular gerçekleştirme
HDInsight'ta bir [Apache Spark](hdinsight-apache-spark-overview.md) kümesi oluşturmayı ve ardından [Jupyter](https://jupyter.org) not defteri kullanarak Spark kümesi üzerinde Spark SQL etkileşimli sorguları gerçekleştirmeyi öğrenin.

   ![HDInsight'ta Apache Spark'ı kullanmaya başlayın](./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.getstartedflow.png "HDInsight'ta Apache Spark'ı kullanmaya başlama öğreticisi. Gösterilen adımlar: depolama hesabı oluşturma; küme oluşturma; Spark SQL deyimi çalıştırma")

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Ön koşullar
* **Bir Azure aboneliği**. Bu öğreticiye başlamadan önce bir Azure aboneliğinizin olması gerekir. Bkz. [Ücretsiz Azure hesabınızı hemen oluşturun](https://azure.microsoft.com/free).
* **Güvenli Kabuk (SSH) istemcisi**: Linux, Unix ve OS X sistemleri `ssh` komutu ile bir SSH istemcisi sağlar. Windows işletim sistemleri için bkz. [PuTTY ile Windows'dan HDInsight'ta Linux tabanlı Hadoop ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-windows.md); Linux, Unix veya OS X için bkz. [Linux, Unix veya OS X'ten HDInsight'ta Linux tabanlı Hadoop ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md)

> [!NOTE]
> Bu makalede [küme depolama birimi olarak Azure Depolama Blobları](hdinsight-hadoop-use-blob-storage.md) kullanan bir Spark kümesi oluşturmak üzere Azure Resource Manager şablonu kullanılmaktadır. Ayrıca, varsayılan depolama birimi olan Azure Depolama Bloblarına ek olarak [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) kullanan bir Spark kümesi oluşturabilirsiniz. Yönergeler için bkz. [Data Lake Store ile HDInsight kümesi oluşturma](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).
>
>

### <a name="access-control-requirements"></a>Erişim denetimi gereksinimleri
[!INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-spark-cluster"></a>Spark kümesi oluşturma
Bu bölümde, [Azure Resource Manager şablonu](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/) kullanarak HDInsight'ta Spark kümesi oluşturacaksınız. HDInsight sürümleri ve SLA’ları hakkında bilgi için bkz. [HDInsight bileşen sürümü oluşturma](hdinsight-component-versioning.md). Diğer küme oluşturma yöntemleri için bkz. [HDInsight kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md).

1. Azure Portal'da bir şablonu açmak için aşağıdaki görüntüye tıklayın.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-spark-linux%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apache-spark-jupyter-spark-sql/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Aşağıdaki değerleri girin:

    ![Azure Resource Manager şablonu kullanarak HDInsight'ta Spark kümesi oluşturma](./media/hdinsight-apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Azure Resource Manager şablonu kullanarak HDInsight'ta Spark kümesi oluşturma")

   * **Abonelik**: Bu kümeye ait Azure aboneliğinizi seçin.
   * **Kaynak grubu**: Yeni bir kaynak grubu oluşturun veya var olan bir kaynak grubunu seçin. Kaynak grubu, projelerinize ait Azure kaynaklarını yönetmek için kullanılır.
   * **Konum**: Kaynak grubu için bir konum seçin.  Bu konum varsayılan küme depolama alanı ve HDInsight kümesi için de kullanılır.
   * **ClusterName**: Oluşturacağınız Hadoop kümesi için bir ad girin.
   * **Küme oturum açma adı ve parolası**: Varsayılan oturum açma adı admin şeklindedir.
   * **SSH kullanıcı adı ve parola**.

   Lütfen bu değerleri yazın.  Öğreticide daha sonra bunlara ihtiyacınız olacaktır.

3. **Yukarıdaki hüküm ve koşulları kabul ediyorum**'u ve ardından **Panoya sabitle**'yi seçip **Satın al**'a tıklayın. Şablon dağıtımı için Dağıtım gönderme başlıklı yeni bir kutucuk görürsünüz. Kümeyi oluşturmak yaklaşık 20 dakika sürer.

## <a name="run-spark-sql-queries-using-a-jupyter-notebook"></a>Jupyter not defteri kullanarak Spark SQL sorguları çalıştırma
Bu bölümde, Spark kümesine yönelik Spark SQL sorguları çalıştırmak için Jupyter not defteri kullanırsınız. HDInsight Spark kümeleri Jupyter not defteri ile kullanabileceğiniz iki çekirdek sağlar. Bunlar:

* **PySpark** (Python içinde yazılmış uygulamalar için)
* **Spark** (Scala içinde yazılmış uygulamalar için)

Bu makalede PySpark çekirdeği kullanılacaktır. [Spark HDInsight kümeleri ile Jupyter not defterlerinde kullanılabilen çekirdekler](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-pyspark-or-spark-kernels) makalesinde PySpark çekirdeğini kullanmanın yararları hakkında ayrıntılı bilgi bulabilirsiniz. Ancak, PySpark çekirdeği kullanmanın birkaç avantajı şunlardır:

* Spark ve Hive bağlamlarını ayarlamanız gerekmez. Bunlar sizin için otomatik olarak ayarlanır.
* SQL veya Hive sorgularınızı önceki kod parçacıkları olmadan doğrudan çalıştırmak için `%%sql` gibi hücre işlevlerini kullanabilirsiniz.
* SQL veya Hive sorgularının çıktıları otomatik olarak gösterilir.

### <a name="create-jupyter-notebook-with-pyspark-kernel"></a>PySpark çekirdeği ile Jupyter not defteri oluşturma

1. [Azure portalı](https://portal.azure.com/) açın.
2. Sol menüden **Kaynak grupları**'na tıklayın.
3. Son bölümde oluşturduğunuz kaynak grubuna tıklayın. Çok fazla kaynak grubu varsa bu arama işlevini kullanabilirsiniz. Bu grupta HDInsight kümesi ve varsayılan depolama hesabı olmak üzere iki kaynak görürsünüz.
4. Açmak için kümeye tıklayın.
 
2. **Hızlı bağlantılar** bölümünde **Küme panoları**'na ve ardından **Jupyter Notebook**'a tıklayın. İstenirse, küme için yönetici kimlik bilgilerini girin.

   ![HDInsight kümesi panoları](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-azure-portal-cluster-dashboards.png "HDInsight kümesi panoları")

   > [!NOTE]
   > Aşağıdaki URL’yi tarayıcınızda açarak da Jupyter Notebook’a ulaşabilirsiniz. **CLUSTERNAME** değerini kümenizin adıyla değiştirin:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Yeni bir not defteri oluşturun. **Yeni** ve ardından **PySpark** seçeneğine tıklayın.

   ![Yeni bir Jupyter not defteri oluşturma](./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.createnotebook.png "Yeni bir Jupyter not defteri oluşturma")

   Untitled(Untitled.pynb) adıyla yeni bir not defteri oluşturulur ve açılır. 

4. Üstteki not defteri adına tıklayın ve isterseniz kolay bir ad girin.

    ![Not defteri adını belirtme](./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.notebook.name.png "Not defteri adını belirtme")
5. Aşağıdaki kodu boş bir hücreye yapıştırın ve kodu yürütmek için **SHIFT + ENTER** tuşlarına basın. Kod, bu senaryo için gerekli olan türleri içeri aktarır:

        from pyspark.sql.types import *

    PySpark çekirdeği kullanarak bir not defteri oluşturduğunuz için açıkça bir bağlam oluşturmanız gerekmez. Birinci kod hücresini çalıştırdığınızda Spark ve Hive bağlamları sizin için otomatik olarak oluşturulur.

    ![Jupyter not defteri işinin durumu](./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.jupyter.job.status.png "Jupyter not defteri işinin durumu")

    Jupyter’de bir işi her çalıştırdığınızda web tarayıcınızın pencere başlığında not defteri başlığı ile birlikte **(Meşgul)** durumu gösterilir. Ayrıca sağ üst köşedeki **PySpark** metninin yanında içi dolu bir daire görürsünüz. İş tamamlandıktan sonra bu simge boş bir daireye dönüşür.

6. **hvac** adlı geçici bir tabloya örnek veri kaydetmek için aşağıdaki kodu çalıştırın.

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

    HDInsight içindeki Spark kümeleri **\HdiSamples\HdiSamples\SensorSampleData\hvac** dizininde bulunan **hvac.csv** adlı bir örnek veri dosyasıyla gelir.
    
7. Verileri sorgulamak için aşağıdaki kodu çalıştırın:

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

   Bir PySpark çekirdeği kullandığınız için `%%sql` sihrini kullanarak yeni oluşturduğunuz **hvac** geçici tablosunda bundan böyle bir SQL sorgusunu doğrudan çalıştırabilirsiniz. `%%sql` sihrinin yanı sıra PySpark çekirdeği kullanılabilen diğer sihirler hakkında daha fazla bilgi için bkz. [Spark HDInsight kümeleri ile Jupyter not defterlerinde kullanılabilen çekirdekler](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-pyspark-or-spark-kernels).

   Varsayılan olarak aşağıdaki tablo çıktısı görüntülenir.

     ![Sorgu sonucunun tablo çıktısı](./media/hdinsight-apache-spark-jupyter-spark-sql/tabular.output.png "Sorgu sonucunun tablo çıktısı")

    Sonuçları diğer görselleştirmelerde de görebilirsiniz. Örneğin, aynı çıktı için bir alan grafiği aşağıdaki gibi görünür.

    ![Sorgu sonucunun alan grafiği](./media/hdinsight-apache-spark-jupyter-spark-sql/area.output.png "Sorgu sonucunun alan grafiği")

9. Uygulamayı çalıştırmayı tamamladıktan sonra kaynakları serbest bırakmak için not defterini kapatabilirsiniz. Bunu yapmak için not defterindeki **Dosya** menüsünde **Kapat ve Durdur**’a tıklayın. Bunun yapılması not defterini kapatır.

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



<!--HONumber=Jan17_HO2-->


