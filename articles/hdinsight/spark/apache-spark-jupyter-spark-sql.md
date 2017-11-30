---
title: "Azure HDInsight’ta Apache Spark kümesi oluşturma | Microsoft Docs"
description: "HDInsight’ta bir Apache Spark kümesi oluşturmaya yönelik HDInsight Spark hızlı başlangıcı."
keywords: "spark hızlı başlangıç,etkileşimli spark,etkileşimli sorgu,hdinsight spark,azure spark"
services: hdinsight
documentationcenter: 
author: mumian
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.assetid: 91f41e6a-d463-4eb4-83ef-7bbb1f4556cc
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/07/2017
ms.author: jgao
ms.openlocfilehash: 20952702ee9dbe9880c80dddc0d75f39e53b6659
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="create-an-apache-spark-cluster-in-azure-hdinsight"></a>Azure HDInsight’ta Apache Spark kümesi oluşturma

Bu makalede, Azure HDInsight'ta Apache Spark kümesi oluşturmayı öğrenecek ve ardından Hive tablosunda bir Spark SQL sorgusu çalıştıracaksınız. HDInsight’ta Spark hakkında daha fazla bilgi için bkz. [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md).

   ![Azure HDInsight’ta Apache Spark kümesi oluşturma adımlarını açıklayan hızlı başlangıç diyagramı](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-quickstart-interactive-spark-query-flow.png "Spark hızlı başlangıcı - HDInsight’ta Apache Spark kullanma. Gösterilen adımlar: küme oluşturma; etkileşimli Spark sorgusu çalıştırma")

## <a name="prerequisites"></a>Ön koşullar

* **Bir Azure aboneliği**. Bu öğreticiye başlamadan önce bir Azure aboneliğinizin olması gerekir. Bkz. [Ücretsiz Azure hesabınızı hemen oluşturun](https://azure.microsoft.com/free).

## <a name="create-hdinsight-spark-cluster"></a>HDInsight Spark kümesi oluşturma

Bu bölümde, [Azure Resource Manager şablonu](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/) kullanarak bir HDInsight Spark kümesi oluşturacaksınız. Diğer küme oluşturma yöntemleri için bkz. [HDInsight kümeleri oluşturma](../hdinsight-hadoop-provision-linux-clusters.md).

1. Azure Portal'da bir şablonu açmak için aşağıdaki görüntüye tıklayın.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-spark-linux%2Fazuredeploy.json" target="_blank"><img src="./media/apache-spark-jupyter-spark-sql/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Aşağıdaki değerleri girin:

    ![Azure Resource Manager şablonu kullanarak HDInsight Spark kümesi oluşturma](./media/apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Azure Resource Manager şablonu kullanarak Spark kümesi oluşturma")

    * **Abonelik**: Bu kümeye ait Azure aboneliğinizi seçin.
    * **Kaynak grubu**: Bir kaynak grubu oluşturun veya mevcut bir kaynak grubunu seçin. Kaynak grubu, projelerinize ait Azure kaynaklarını yönetmek için kullanılır.
    * **Konum**: Kaynak grubu için bir konum seçin. Şablon hem kümeyi oluşturmak için hem de varsayılan küme depolaması için bu konumu kullanır.
    * **ClusterName**: Oluşturmak istediğiniz HDInsight kümesi için bir ad girin.
    * **Spark sürümü**: Kümeye yüklemek istediğiniz sürüm olarak **2.0**'ı seçin.
    * **Küme oturum açma adı ve parolası**: Varsayılan oturum açma adı admin şeklindedir.
    * **SSH kullanıcı adı ve parola**.

   Bu değerleri not alın.  Öğreticinin sonraki bölümlerinde bunlara ihtiyacınız olacaktır.

3. **Yukarıdaki hüküm ve koşulları kabul ediyorum**'u ve ardından **Panoya sabitle**'yi seçip **Satın al**'a tıklayın. Şablon dağıtımı için Dağıtım gönderme başlıklı yeni bir kutucuk görürsünüz. Kümenin oluşturulması yaklaşık 20 dakika sürer.

HDInsight kümelerini oluştururken sorunlarla karşılaşırsanız, bunu yapmak için doğru izinlere sahip olmayabilirsiniz. Daha fazla bilgi için bkz. [Erişim denetimi gereksinimleri](../hdinsight-administer-use-portal-linux.md#create-clusters).

> [!NOTE]
> Bu makalede [küme depolama birimi olarak Azure Depolama Blobları](../hdinsight-hadoop-use-blob-storage.md) kullanan bir Spark kümesi oluşturulur. Ayrıca, varsayılan depolama birimi olarak [Azure Data Lake Store](../hdinsight-hadoop-use-data-lake-store.md) kullanan bir Spark kümesi oluşturabilirsiniz. Yönergeler için bkz. [Data Lake Store ile HDInsight kümesi oluşturma](../../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).
>
>

## <a name="run-spark-sql-statements-on-a-hive-table"></a>Bir Hive tablosunda Spark SQL deyimleri çalıştırma

SQL (Yapılandırılmış Sorgu Dili), veri sorgulama ve tanımlama için en çok kullanılan dildir. Spark'ın kurucuları bu bilgiden faydalanmak için çok bilinen veri sorgulama dilini Hadoop Dağıtılmış Dosya Sistemi (HDFS) üzerindeki verilerle çalışmak isteyen analistlerin kullanımına açtı. Bu özellik Spark SQL ile sunulmaktadır. Bilinen SQL söz dizimini kullanan bu özellik, yapısal verileri işleyen bir Apache Spark uzantısı olarak çalışır.

Spark SQL hem SQL hem de HiveQL sorgu dillerini destekler. Python, Scala ve Java ile bağlama gerçekleştirilebilir. Bu işlev sayesinde harici veritabanları, yapısal veri dosyaları (örnek: JSON) ve Hive tabloları gibi farklı konumlarda depolanan verileri sorgulayabilirsiniz.

### <a name="running-spark-sql-on-an-hdinsight-cluster"></a>Spark SQL'i bir HDInsight kümesinde çalıştırma

HDInsight Spark kümeniz için yapılandırılmış bir Jupyter not defteri kullanırken, Spark SQL ile Hive sorguları çalıştırmak için kullanabileceğiniz önceden ayarlanmış bir `sqlContext` alırsınız. Bu bölümde bir Jupyter not defteri başlatıp tüm HDInsight kümelerinde kullanılabilen mevcut Hive tablosunda (**hivesampletable**) temel Spark SQL sorgusu çalıştırmayı öğreneceksiniz.

1. [Azure portalı](https://portal.azure.com/) açın.

2. Kümeyi panoya sabitlemeyi seçtiyseniz, küme dikey penceresini başlatmak için panodan küme kutucuğuna tıklayın.

    Kümeyi panoya sabitlemediyseniz, sol bölmedeki **HDInsight kümeleri** öğesine ve ardından oluşturduğunuz kümeye tıklayın.

3. **Hızlı bağlantılar** bölümünde **Küme panoları**'na ve ardından **Jupyter Notebook**'a tıklayın. İstenirse, küme için yönetici kimlik bilgilerini girin.

   ![Jupyter not defterini açarak etkileşimli Spark SQL sorgusu çalıştırma](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "Open Jupyter notebook to run interactive Spark SQL query")

   > [!NOTE]
   > Aşağıdaki URL’yi tarayıcınızda açarak da Jupyter not defterine erişebilirsiniz. **CLUSTERNAME** değerini kümenizin adıyla değiştirin:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Bir not defteri oluşturun. **Yeni** ve ardından **PySpark** seçeneğine tıklayın.

   ![Jupyter not defterini açarak etkileşimli Spark SQL sorgusu oluşturma](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-spark-sql-query.png "Create a Jupyter notebook to run interactive Spark SQL query")

   Untitled(Untitled.pynb) adıyla yeni bir not defteri oluşturulur ve açılır.

4. Üstteki not defteri adına tıklayın ve isterseniz kolay bir ad girin.

    ![Etkileşimli Spark sorgusunun çalıştırılacağı Jupyter not defteri için ad belirtme](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-jupyter-notebook-name.png "Etkileşimli Spark sorgusunun çalıştırılacağı Jupyter not defteri için ad belirtme")

5.  Aşağıdaki kodu boş bir hücreye yapıştırın ve kodu çalıştırmak için **SHIFT + ENTER** tuşlarına basın. Aşağıdaki kodda `%%sql` (sql sihri olarak adlandırılır), Jupyter not defterine Hive sorgusunu çalıştırmak için önceden ayarlanmış `sqlContext` kullanmasını bildirir. Sorgu, varsayılan olarak tüm HDInsight kümelerinde sağlanan Hive tablosundaki (**hivesampletable**) ilk 10 satırı getirir.

        %%sql
        SELECT * FROM hivesampletable LIMIT 10

    ![HDInsight Spark'ta Hive sorgusu](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query.png "HDInsight Spark'ta Hive sorgusu")

    `%%sql` sihri ve önceden ayarlanmış bağlamlar hakkında daha fazla bilgi için bkz. [HDInsight kümesi için kullanılabilen Jupyter çekirdekleri](apache-spark-jupyter-notebook-kernels.md).

    > [!NOTE]
    > Jupyter’de bir sorguyu her çalıştırdığınızda web tarayıcınızın pencere başlığında not defteri başlığı ile birlikte **(Meşgul)** durumu gösterilir. Ayrıca sağ üst köşedeki **PySpark** metninin yanında içi dolu bir daire görürsünüz. İş tamamlandıktan sonra bu simge boş bir daireye dönüşür.
    >
    >
    
6. Sorgu çıkışının görüntülenmesi için ekranın yenilenmesi gerekir.

    ![HDInsight Spark'ta Hive sorgusu çıkışı](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query-output.png "HDInsight Spark'ta Hive sorgusu çıkışı")

7. Uygulamayı çalıştırmayı tamamladıktan sonra not defterini kapatarak küme kaynaklarını serbest bırakabilirsiniz. Bunu yapmak için not defterindeki **Dosya** menüsünde **Kapat ve Durdur**’a tıklayın.

8. İzleyen adımları daha sonra tamamlamayı planlıyorsanız, bu makalede oluşturduğunuz HDInsight kümesini sildiğinizden emin olun. 

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-step"></a>Sonraki adım 

Bu makalede HDInsight Spark kümesi oluşturmayı ve temel Spark SQL sorgusunu çalıştırmayı öğrendiniz. Örnek veriler üzerinde etkileşimli sorguları çalıştırmak için HDInsight Spark kümesini kullanma hakkında daha fazla bilgiyi bir sonraki makalede bulabilirsiniz.

> [!div class="nextstepaction"]
>[Bir HDInsight Spark kümesinde etkileşimli sorguları çalıştırma](apache-spark-load-data-run-query.md)



