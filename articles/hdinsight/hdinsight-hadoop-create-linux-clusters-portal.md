---
title: Bir web tarayıcısı - Azure HDInsight'ı kullanarak Apache Hadoop kümeleri oluşturma
description: Linux'ta bir web tarayıcısını ve Azure Önizleme portalını kullanarak HDInsight için Apache Hadoop, Apache HBase, Apache Storm veya Apache Spark küme oluşturma konusunda bilgi edinin.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 12/28/2018
ms.author: hrasheed
ms.openlocfilehash: ca8b398a796284e34ce4e0b577c335552ca75a98
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2019
ms.locfileid: "53974276"
---
# <a name="create-linux-based-clusters-in-hdinsight-using-the-azure-portal"></a>HDInsight'ı Azure portalını kullanarak Linux tabanlı kümeler oluşturma
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Azure portalında, hizmetler ve kaynaklar Microsoft Azure bulutunda barındırılan bir web tabanlı yönetim aracıdır. Bu makalede, portal kullanarak Linux tabanlı HDInsight kümeleri oluşturma işlemini öğrenin.

## <a name="prerequisites"></a>Önkoşullar
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Modern bir web tarayıcısı**. Azure portalı, HTML5 ve Javascript kullanır ve eski web tarayıcılarında düzgün çalışmayabilir.

## <a name="create-clusters"></a>Küme oluşturma
Azure portalı küme özelliklerin çoğu kullanıma sunar. Azure Resource Manager şablonlarını kullanarak, birçok ayrıntıyı gizleyebilirsiniz. Daha fazla bilgi için [oluşturma Linux tabanlı Apache Hadoop Azure Resource Manager şablonlarını kullanarak HDInsight kümelerinde](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]


1. [Azure portalı](https://portal.azure.com)’nda oturum açın.

1. Sol menüden **+ kaynak Oluştur**.

1.  Altında **Azure Marketi**seçin **Analytics**.

1.  Altında **öne çıkan**seçin **HDInsight**.
   
    ![Azure portalında yeni bir küme oluşturma](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster.png "Azure portalında yeni bir küme oluşturma")

1. Gelen **HDInsight** sayfasında **özel (boyut, ayarları, uygulamalar)**.

1. Seçin **1 temel**ve ardından aşağıdaki bilgileri girin:

    ![Azure portalında yeni bir küme oluşturma](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-basics.png "Azure portalında yeni bir küme oluşturma")

    * Girin **küme adı**: Bu adın küresel olarak benzersiz olması gerekir.

    * Gelen **abonelik** açılan listesinde, küme için kullanılan Azure aboneliğini seçin.

    * Seçin **küme türü**ve ardından türünü seçin (Hadoop, Spark vb.) küme oluşturmak istiyorsunuz. **İşletim sistemi** olacaktır **Linux**. Ardından bir küme türü sürümü seçin. Neyi seçeceğinizi bilmiyorsanız varsayılan sürümü kullanın. Daha fazla bilgi için bkz. [HDInsight küme sürümleri](hdinsight-component-versioning.md).
     
        > [!IMPORTANT]  
        > HDInsight kümeleri gelen iş yükü veya küme için ayarlanan teknoloji karşılık gelen türleri çeşitli. Bir küme üzerinde Storm ve HBase gibi birden birleştiren bir küme oluşturmak için desteklenen bir yöntem yoktur.
        
    * İçin **küme oturum açma kullanıcı** ve **küme oturum açma parolası**, yönetici kullanıcı için kullanıcı adı ve parola sağlayın.

    * Girin bir **SSH kullanıcı adı** ve SSH parolası aynı daha önce belirttiğiniz yönetici parolasını, select istiyorsanız **kümede oturum açarken kullanılan parolayı kullan** onay kutusu. Aksi takdirde, ya da sağlar. bir **parola** veya **ortak anahtar**, hangi SSH kullanıcısı kimlik doğrulaması için kullanılacak. Ortak anahtar kullanılması önerilen yaklaşımdır. Alt kısımdaki **Seç**’e tıklayarak kimlik bilgileri yapılandırmasını kaydedin.
   
        Bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

    * **Kaynak grubu** için yeni bir kaynak grubu oluşturmayı veya mevcut bir kaynak grubunu kullanmayı seçin.

    * Bir veri merkezini seçebilir **konumu** kümenin oluşturulduğu.

    * Seçin **sonraki** sonraki sayfaya taşınır.

4. Gelen **2 güvenlik + ağ**, kümenizi sağlanan açılır menüyü kullanarak bir sanal ağa bağlanabilir. Bir sanal ağ kümesine yerleştirmek istiyorsanız bir Azure sanal ağı ve alt ağ seçin. Bir sanal ağıyla sanal ağ için belirli yapılandırma gereksinimlerini de dahil olmak üzere, HDInsight'ı kullanma hakkında bilgi için bkz. [kullanarak bir Azure sanal ağ genişletme HDInsight özellikleri](hdinsight-extend-hadoop-virtual-network.md). Kullanmak istiyorsanız **Kurumsal güvenlik paketi**, ayrıca Buradaki yönergeleri izleyebilirsiniz: [Azure Active Directory Domain Services'ı kullanarak bir HDInsight kümesi ile Kurumsal güvenlik paketi yapılandırma](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-configure-using-azure-adds).

    Seçin **sonraki** sonraki sayfaya taşınır.


5. Gelen **3 depolama**, varsayılan depolama alanı olarak Azure Storage (WASB) veya Data Lake Storage istediğinizi belirtin. Daha fazla bilgi için aşağıdaki tabloya bakın.

     ![Azure portalında yeni bir küme oluşturma](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-storage.png "Azure portalında yeni bir küme oluşturma")

     | Depolama                                      | Açıklama |
     |----------------------------------------------|-------------|
     | **Varsayılan depolama alanı olarak Azure depolama Blobları**   | <ul><li>İçin **birincil depolama türü**seçin **Azure depolama**. Bundan sonra için **seçme yöntemi**, seçebileceğiniz **Aboneliklerim** Azure aboneliğinizin bir parçası olan bir depolama hesabı belirtin ve ardından depolama hesabını seçmek istiyorsanız. ' A tıklayıp **erişim anahtarı** ve dışında Azure aboneliğinizi seçmek istediğiniz depolama hesabı için bilgileri sağlayın.</li><li>İçin **varsayılan kapsayıcı**, portal tarafından önerilen varsayılan kapsayıcı adı giderek veya kendi koşulunuzu belirtmek seçebilirsiniz.</li><li>(İsteğe bağlı) varsayılan depolama alanı olarak WASB kullanıyorsanız, tıklayabilirsiniz **ek depolama hesapları** kümesi ile ilişkilendirmek üzere ek depolama hesapları belirtmek için. İçin **Azure depolama anahtarları**, tıklayın **depolama anahtarı Ekle**, ve ardından, bir depolama hesabı, Azure aboneliklerinizle veya diğer Aboneliklerdeki (depolama hesabı erişim anahtarı sağlayarak) sağlayabilirsiniz.</li><li>(İsteğe bağlı) varsayılan depolama alanı olarak WASB kullanıyorsanız, tıklayabilirsiniz **Data Lake Storage erişim** ek depolama alanı olarak Azure Data Lake Storage belirtmek için. Daha fazla bilgi için [hızlı başlangıç: HDInsight kümelerinde ayarlama](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).</li></ul> |
     | **Varsayılan depolama alanı olarak Azure Data Lake depolama** | İçin **birincil depolama türü**seçin **Azure Data Lake depolama Gen1** veya **Azure Data Lake depolama Gen2** ve ardından makalesine bakabilirsiniz [hızlı başlangıç: HDInsight kümelerinde ayarlama](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md) yönergeler için. |
     | **Dış meta depolar**                      | İsteğe bağlı olarak, kümeyle ilişkili Hive ve Oozie meta verilerini kaydetmek için bir SQL veritabanını belirtebilirsiniz. İçin **Hive için bir SQL veritabanı seçin** bir SQL veritabanını seçin ve ardından veritabanı kullanıcı adı/parola sağlayın. Oozie meta verileri için bu adımları yineleyin.<br><br>Azure SQL veritabanı için meta depolar kullanırken bazı noktalar. <ul><li>Meta veri deposu için kullanılan Azure SQL veritabanını Azure HDInsight gibi diğer Azure hizmetlerine bağlanmaya izin vermelidir. Azure SQL veritabanı Panoda işlecin sağ tarafındaki sunucu adına tıklayın. Bu, SQL veritabanı örneği üzerinde çalıştığı sunucudur. Sunucu görünümünde açıldığında, tıklayın **yapılandırma**ve ardından **Azure Hizmetleri**, tıklayın **Evet**ve ardından **Kaydet**.</li><li>Bu küme oluşturma işlemi başarısız olmasına neden bir meta veri deposu oluştururken, kısa çizgi veya tire içeren bir veritabanı adı kullanmayın.</li></ul> |

     > [!WARNING]  
     > HDInsight kümesinden farklı bir konumda ek depolama hesabının kullanılması desteklenmez.

     Seçin **sonraki** sonraki sayfaya taşınır.


6. Gelen **4 uygulamalar (isteğe bağlı)** seçin tüm uygulamaları istenen.  Bu uygulamalar Microsoft veya bağımsız yazılım satıcıları (ISV) tarafından ya da sizin tarafınızdan geliştirilebilir. Daha fazla bilgi için [yükleme HDInsight uygulamalarını](hdinsight-apps-install-applications.md#install-applications-during-cluster-creation).

    Seçin **sonraki** sonraki sayfaya taşınır.


6. **Küme boyutu 5** bu küme için kullanılan düğümleri hakkında bilgileri görüntüler. Küme için gereksinim duyduğunuz çalışan düğümleri sayısını ayarlayın. Küme çalıştırmanın tahmini maliyeti de gösterilir.
   
    ![Düğüm fiyatlandırma katmanları](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-nodes.png "küme düğümleri sayısı belirtin")
   
   > [!IMPORTANT]  
   > 32'den fazla çalışan düğümleri, küme oluşturma sırasında veya Küme oluşturulduktan sonra ölçeklendirme planlıyorsanız bir baş düğüm boyutu en az 8 çekirdek ve 14 GB RAM ile seçmeniz gerekir.
   > 
   > Düğüm boyutları ve ilişkili maliyetler hakkında daha fazla bilgi için bkz. [HDInsight fiyatlandırması](https://azure.microsoft.com/pricing/details/hdinsight/).
   
    Seçin **sonraki** sonraki sayfaya taşınır.

8. Gelen **6 betik eylemleri**, özel bileşenleri yüklemek için bir küme özelleştirebilirsiniz.  Küme oluşturulurken bir küme özelleştirmek için özel bir betik kullanmak istiyorsanız bu seçeneği kullanın. Betik eylemleri hakkında daha fazla bilgi için bkz: [özelleştirme HDInsight kümelerini betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md).

   Seçin **sonraki** sonraki sayfaya taşınır.

9. Gelen **7 özeti**, daha önce girdiğiniz bilgileri doğrulayın ve ardından **Oluştur**.

     ![Düğüm fiyatlandırma katmanları](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-summary.png "küme düğümleri sayısı belirtin")
    
    > [!NOTE]  
    > Kümenin oluşturulması genellikle yaklaşık 20 dakika sürer. İzleyici **bildirimleri** sağlama işlemini denetlemek için.

10. Oluşturma işlemi tamamlandıktan sonra seçin **kaynağa Git** gelen **dağıtım başarılı** bildirim. Küme penceresi, aşağıdaki bilgileri sağlar.
    
    ![Küme arabirimi](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-completed.png "küme özellikleri")
    
    Üst simgeleri anlamak için aşağıdakileri kullanın.
    
    * **Genel Bakış** sekmesi, küme adı, ancak ait olduğu kaynak grubu, konum, işletim sistemi, URL küme Panosu, vb. gibi tüm gerekli bilgileri sağlar.
    * **Pano** kümeyle ilişkili Ambari portalına yönlendirir.
    * **Güvenli Kabuk**: SSH kullanarak kümeye erişmek için gereken bilgileri.
    * **Ölçek kümesi** kümeyle ilişkili çalışan düğümlerinin sayısını artırmak sağlar.
      * **Silme**: HDInsight kümesini siler.
    

## <a name="customize-clusters"></a>Kümeleri özelleştirme
* Bkz: [özelleştirme HDInsight kümeleri Bootstrap ile](hdinsight-hadoop-customize-cluster-bootstrap.md).
* Bkz: [özelleştirme HDInsight kümelerini betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="delete-the-cluster"></a>Küme silme
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Sorun giderme

HDInsight kümeleri oluştururken sorun yaşarsanız bkz. [erişim denetimi gereksinimleri](hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="next-steps"></a>Sonraki adımlar
Bir HDInsight kümesi başarıyla oluşturuldu, kümenizi ile çalışma hakkında bilgi almak için aşağıdakileri kullanın:

### <a name="apache-hadoop-clusters"></a>Apache Hadoop kümelerini
* [Apache Hive, HDInsight ile kullanma](hadoop/hdinsight-use-hive.md)
* [HDInsight ile Pig UseApache](hadoop/hdinsight-use-pig.md)
* [HDInsight ile MapReduce kullanma](hadoop/hdinsight-use-mapreduce.md)

### <a name="apache-hbase-clusters"></a>Apache HBase kümeleri
* [HDInsight üzerinde Apache HBase kullanmaya başlama](hbase/apache-hbase-tutorial-get-started-linux.md)
* [HDInsight üzerinde Apache HBase için Java uygulamaları geliştirin](hbase/apache-hbase-build-java-maven-linux.md)

### <a name="apache-storm-clusters"></a>Apache Storm kümeleri
* [HDInsight üzerinde Apache Storm için Java topolojileri geliştirme](storm/apache-storm-develop-java-topology.md)
* [HDInsight üzerinde Apache Storm, Python bileşenlerini kullanma](storm/apache-storm-develop-python-topology.md)
* [HDInsight üzerinde Apache Storm topolojileri dağıtma ve izleme](storm/apache-storm-deploy-monitor-topology-linux.md)

### <a name="apache-spark-clusters"></a>Apache Spark kümeleri
* [Scala kullanarak tek başına uygulama oluşturma](spark/apache-spark-create-standalone-application.md)
* [Apache Livy kullanarak bir Apache Spark kümesinde işleri uzaktan çalıştırma](spark/apache-spark-livy-rest-interface.md)
* [Apache Spark ile BI: BI araçları ile HDInsight Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](spark/apache-spark-use-bi-tools.md)
* [Apache Spark Machine Learning ile: Gıda denetimi sonuçlarını tahmin etmek için HDInsight içindeki Spark kullanma](spark/apache-spark-machine-learning-mllib-ipython.md)

