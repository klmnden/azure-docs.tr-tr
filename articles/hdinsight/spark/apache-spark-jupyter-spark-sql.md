---
title: Azure HDInsight’ta Apache Spark kümesi oluşturma | Microsoft Docs
description: HDInsight’ta bir Apache Spark kümesi oluşturmaya yönelik HDInsight Spark hızlı başlangıcı.
keywords: spark hızlı başlangıç,etkileşimli spark,etkileşimli sorgu,hdinsight spark,azure spark
services: hdinsight
documentationcenter: ''
author: mumian
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.assetid: 91f41e6a-d463-4eb4-83ef-7bbb1f4556cc
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: conceptual
ms.date: 03/01/2018
ms.author: jgao
ms.openlocfilehash: 1fdae7d6e47ff70e83e8153cdc22c0b2881d2743
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="create-an-apache-spark-cluster-in-azure-hdinsight"></a>Azure HDInsight’ta Apache Spark kümesi oluşturma

Azure HDInsight'ta Apache Spark kümesinin nasıl oluşturulacağını ve Spark SQL sorgularının Hive tablolarına karşı nasıl çalıştırılacağını öğrenin. HDInsight’ta Spark hakkında daha fazla bilgi için bkz. [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md).

## <a name="prerequisites"></a>Önkoşullar

* **Bir Azure aboneliği**. Bu öğreticiye başlamadan önce bir Azure aboneliğinizin olması gerekir. Bkz. [Ücretsiz Azure hesabınızı oluşturun](https://azure.microsoft.com/free).

## <a name="create-hdinsight-spark-cluster"></a>HDInsight Spark kümesi oluşturma

[Azure Resource Manager şablonu](../hdinsight-hadoop-create-linux-clusters-arm-templates.md) kullanarak bir HDInsight Spark kümesi oluşturun. Şablon, [github](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/)’da bulunabilir. Diğer küme oluşturma yöntemleri için bkz. [HDInsight kümeleri oluşturma](../hdinsight-hadoop-provision-linux-clusters.md).

1. Azure Portal'da bir şablonu açmak için aşağıdaki görüntüye tıklayın.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-spark-linux%2Fazuredeploy.json" target="_blank"><img src="./media/apache-spark-jupyter-spark-sql/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Aşağıdaki değerleri girin:

    ![Azure Resource Manager şablonu kullanarak HDInsight Spark kümesi oluşturma](./media/apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Azure Resource Manager şablonu kullanarak Spark kümesi oluşturma")

    * **Abonelik**: Bu kümeyi oluşturmak için kullanılan Azure aboneliğinizi seçin.
    * **Kaynak grubu**: Bir kaynak grubu oluşturun veya mevcut bir kaynak grubunu seçin. Kaynak grubu, projelerinize ait Azure kaynaklarını yönetmek için kullanılır.
    * **Konum**: Kaynak grubu için bir konum seçin. Şablon hem kümeyi oluşturmak için hem de varsayılan küme depolaması için bu konumu kullanır.
    * **ClusterName**: Oluşturmak istediğiniz HDInsight kümesi için bir ad girin.
    * **Küme oturum açma adı ve parolası**: Varsayılan oturum açma adı admin şeklindedir. Kümede oturum açmak için bir parola seçin.
    * **SSH kullanıcı adı ve parola**. SSH kullanıcısı için bir parola seçin.

3. **Yukarıdaki hüküm ve koşulları kabul ediyorum**'u ve ardından **Panoya sabitle**'yi seçip **Satın al**'a tıklayın. Artık **Şablon Dağıtımını dağıtma** başlıklı yeni bir kutucuk görebilirsiniz. Kümenin oluşturulması yaklaşık 20 dakika sürer.

HDInsight kümelerini oluştururken sorunlarla karşılaşırsanız, bunu yapmak için doğru izinlere sahip olmayabilirsiniz. Daha fazla bilgi için bkz. [Erişim denetimi gereksinimleri](../hdinsight-administer-use-portal-linux.md#create-clusters).

> [!NOTE]
> Bu makalede [küme depolama birimi olarak Azure Depolama Blobları](../hdinsight-hadoop-use-blob-storage.md) kullanan bir Spark kümesi oluşturulur. Ayrıca, varsayılan depolama birimi olarak [Azure Data Lake Store](../hdinsight-hadoop-use-data-lake-store.md) kullanan bir Spark kümesi oluşturabilirsiniz. Yönergeler için bkz. [Data Lake Store ile HDInsight kümesi oluşturma](../../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).
>
>

## <a name="create-a-jupyter-notebook"></a>Jupyter not defteri oluşturma

[Jupyter Not Defteri](http://jupyter.org), verilerinizle etkileşim kurabilmenizi sağlayan çeşitli programlama dilleri destekleyen, markdown metniyle kod birleştiren ve basit görselleştirmeleri gerçekleştiren etkileşimli bir not defteridir. HDInsight'ta Spark, ayrıca [Zeppelin Not Defteri](apache-spark-zeppelin-notebook.md)’ni içerir. Jupyter Not Defteri, bu öğreticide kullanılır.

**Jupyter not defteri oluşturmak için**

1. [Azure portalı](https://portal.azure.com/) açın.

2. Oluşturduğunuz Spark kümesini açın. Yönergeler için bkz. [Kümeleri listele ve göster](../hdinsight-administer-use-portal-linux.md#list-and-show-clusters).

3. Portalda, **Küme panoları**'na ve ardından **Jupyter Notebook**'a tıklayın. İstenirse, küme için yönetici kimlik bilgilerini girin.

   ![Jupyter not defterini açarak etkileşimli Spark SQL sorgusu çalıştırma](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "Jupyter not defterini açarak etkileşimli Spark SQL sorgusu çalıştırma")

   > [!NOTE]
   > Şu URL’yi tarayıcınızda açarak da Jupyter Not Defteri’ne erişebilirsiniz. **CLUSTERNAME** değerini kümenizin adıyla değiştirin:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Not defteri oluşturmak için **Yeni** ve ardından **PySpark** seçeneğine tıklayın. HDInsight kümelerindeki Jupyter not defterleri üç çekirdek destekliyor - **PySpark**, **PySpark3**, ve **Spark**. **PySpark** çekirdeği bu öğreticide kullanılır. Çekirdekler ve **PySpark** kullanmanın avantajları hakkında daha fazla bilgi için bkz. [HDInsight’ta Apache Spark kümeleri ile Jupyter not defterleri kullanma](apache-spark-jupyter-notebook-kernels.md).

   ![Jupyter Not Defteri’ni oluşturarak etkileşimli Spark SQL sorgusu çalıştırma](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-spark-sql-query.png "Jupyter Not Defteri’ni oluşturarak etkileşimli Spark SQL sorgusu çalıştırma")

   Untitled(Untitled.pynb) adıyla yeni bir not defteri oluşturulur ve açılır.

4. Üstteki not defteri adına tıklayın ve isterseniz kolay bir ad girin.

    ![Etkileşimli Spark sorgusunun çalıştırılacağı Jupyter not defteri için ad sağlama](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-jupyter-notebook-name.png "Etkileşimli Spark sorgusunun çalıştırılacağı Jupyter not defteri için ad sağlama")

## <a name="run-spark-sql-statements-on-a-hive-table"></a>Bir Hive tablosunda Spark SQL deyimleri çalıştırma

SQL (Yapılandırılmış Sorgu Dili), veri sorgulama ve tanımlama için en çok kullanılan dildir. Bilinen SQL söz dizimini kullanan Spark SQL, yapısal verileri işleyen bir Apache Spark uzantısı olarak çalışır.

Spark SQL hem SQL hem de HiveQL sorgu dillerini destekler. Python, Scala ve Java ile bağlama gerçekleştirilebilir. Bu işlev sayesinde harici veritabanları, yapısal veri dosyaları (örnek: JSON) ve Hive tabloları gibi farklı konumlarda depolanan verileri sorgulayabilirsiniz.

Bir Hive tablosu yerine csv dosyasından verileri okuma hakkında bir örnek için bkz. [HDInsight’taki Spark kümeleri üzerinde etkileşimli sorguları çalıştırma](apache-spark-load-data-run-query.md).

**Spark SQL’i çalıştırma**

1. Not defterini ilk kez başlattığınızda, çekirdek arka planda birkaç görev gerçekleştirir. Çekirdeğin hazır olmasını bekleyin. Not defterinde çekirdek adının yanında boş bir daire görmeniz, çekirdeğin hazır olduğu anlamına gelir. Dolu daire, çekirdeğin meşgul olduğunu belirtir.

    ![HDInsight Spark'ta Hive sorgusu](./media/apache-spark-jupyter-spark-sql/jupyter-spark-kernel-status.png "HDInsight Spark'ta Hive sorgusu")

2. Çekirdek hazır olduğunda, aşağıdaki kodu boş bir hücreye yapıştırın ve ardından kodu çalıştırmak için **SHIFT + ENTER** tuşlarına basın. Komut, kümedeki Hive tablolarını listeler:

    ```PySpark
    %%sql
    SHOW TABLES
    ```
    HDInsight Spark kümeniz için yapılandırılmış bir Jupyter not defteri kullanırken, Spark SQL ile Hive sorguları çalıştırmak için kullanabileceğiniz önceden ayarlanmış bir `sqlContext` alırsınız. `%%sql`, Hive sorgusunu çalıştırmak için Jupyter Not Defteri’ne `sqlContext` ön ayarını kullanmasını söyler. Sorgu, varsayılan olarak tüm HDInsight kümelerinde sağlanan Hive tablosundaki (**hivesampletable**) ilk 10 satırı getirir. Sonuçları almak 30 saniye kadar sürer. Çıkış aşağıdakine benzer olacaktır: 

    ![HDInsight Spark'ta Hive sorgusu](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query.png "HDInsight Spark'ta Hive sorgusu")

    `%%sql` sihri ve önceden ayarlanmış bağlamlar hakkında daha fazla bilgi için bkz. [HDInsight kümesi için kullanılabilen Jupyter çekirdekleri](apache-spark-jupyter-notebook-kernels.md).

    Jupyter’de bir sorguyu her çalıştırdığınızda web tarayıcınızın pencere başlığında not defteri başlığı ile birlikte **(Meşgul)** durumu gösterilir. Ayrıca sağ üst köşedeki **PySpark** metninin yanında içi dolu bir daire görürsünüz.
    
2. `hivesampletable` komutundaki verileri görmek için başka bir sorgu çalıştırın.

    ```PySpark
    %%sql
    SELECT * FROM hivesampletable LIMIT 10
    ```
    
    Sorgu çıkışının görüntülenmesi için ekranın yenilenmesi gerekir.

    ![HDInsight Spark'ta Hive sorgusu çıkışı](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query-output.png "HDInsight Spark'ta Hive sorgusu çıkışı")

2. Not defterindeki **Dosya** menüsünde **Kapat ve Durdur**’a tıklayın. Not defterini kapatmak, küme kaynaklarını serbest bırakır.

3. İzleyen adımları daha sonra tamamlamayı planlıyorsanız, bu makalede oluşturduğunuz HDInsight kümesini sildiğinizden emin olun. 

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Sonraki adımlar 

Bu makalede HDInsight Spark kümesi oluşturmayı ve temel Spark SQL sorgusunu çalıştırmayı öğrendiniz. Örnek veriler üzerinde etkileşimli sorguları çalıştırmak için HDInsight Spark kümesini kullanma hakkında daha fazla bilgiyi bir sonraki makalede bulabilirsiniz.

> [!div class="nextstepaction"]
>[Bir HDInsight Spark kümesinde etkileşimli sorguları çalıştırma](apache-spark-load-data-run-query.md)



