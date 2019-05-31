---
title: Azure HDInsight web tarayıcısı kullanarak Apache Hadoop kümeleri oluşturma
description: HDInsight için Linux üzerinde web tarayıcısını ve Azure Önizleme portalını kullanarak Apache Hadoop, Apache HBase, Apache Storm veya Apache Spark kümeleri oluşturmayı öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/29/2019
ms.author: hrasheed
ms.openlocfilehash: cf1a6ffa61b41af5abd20dac13b85024001d2ed2
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393991"
---
# <a name="create-linux-based-clusters-in-hdinsight-by-using-the-azure-portal"></a>Azure portalını kullanarak HDInsight Linux tabanlı kümeler oluşturma
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Azure portalında, hizmetler ve kaynaklar Microsoft Azure bulutunda barındırılan bir web tabanlı yönetim aracıdır. Bu makalede, portalı kullanarak Azure HDInsight Linux tabanlı kümeler oluşturma işlemini öğrenin.

## <a name="prerequisites"></a>Önkoşullar
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **Bir Azure aboneliği**. Bkz: [HDInsight, Hadoop sınama için Azure ücretsiz deneme nasıl](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Modern bir web tarayıcısı**. Azure portal, HTML5 ve JavaScript kullanır. Eski web tarayıcılarında düzgün çalışmayabilir.

## <a name="create-clusters"></a>Küme oluşturma
Azure portalı küme özelliklerin çoğu kullanıma sunar. Azure Resource Manager şablonlarını kullanarak, birçok ayrıntıyı gizleyebilirsiniz. Daha fazla bilgi için [Apache Hadoop kümeleri oluşturma HDInsight Resource Manager şablonları kullanarak](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]


1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Sol menüden **+ kaynak Oluştur**.

1.  Altında **Azure Marketi**seçin **Analytics**.

1.  Altında **öne çıkan**seçin **HDInsight**.
   
    ![Azure portalında yeni bir küme oluşturma](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster.png "Azure portalında yeni bir küme oluşturma")

1. Üzerinde **HDInsight** sayfasında **özel (boyut, ayarları, uygulamalar)** .

1. Seçin **1 temel**. Ardından aşağıdaki bilgileri girin.

    ![Temel ayarları yapılandırma](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-basics.png "Azure portalında yeni bir küme oluşturma")

    * Girin **küme adı**. Bu adın küresel olarak benzersiz olması gerekir.

    * Gelen **abonelik** aşağı açılan listesinde, küme için kullanılan Azure aboneliğini seçin.

    * Seçin **küme türü**. Ardından oluşturmak istediğiniz küme türünü seçin. Hadoop ve Apache Spark verilebilir. **İşletim sistemi** olacaktır **Linux**. Ardından, bir küme türü sürümü seçin. Neyi seçeceğinizi bilmiyorsanız varsayılan sürümü kullanın. Daha fazla bilgi için bkz. [HDInsight küme sürümleri](hdinsight-component-versioning.md).
     
        > [!IMPORTANT]  
        > HDInsight kümelerinin çeşitli türleri gelir. Bunlar iş yükü veya küme için ayarlanan teknoloji karşılık gelir. Birden çok birleştiren bir küme oluşturmak için desteklenen bir yöntem yoktur. Storm ve HBase bir kümede verilebilir.
        
    * İçin **küme oturum açma kullanıcı** ve **küme oturum açma parolası**, yönetici kullanıcı için kullanıcı adı ve parola sağlayın.

    * Girin bir **SSH kullanıcı adı**. Daha önce belirttiğiniz yönetici parolasını olarak aynı SSH parolası istiyorsanız belirleyin **kümede oturum açarken kullanılan parolayı kullan** onay kutusu. Aksi takdirde, ya da sağlar. bir **parola** veya **ortak anahtar** SSH kullanıcısı kimlik doğrulaması için. Bir ortak anahtar öneririz yaklaşımdır. Seçin **seçin** altındaki kimlik bilgileri yapılandırmasını kaydedin.
   
        Daha fazla bilgi için [SSH kullanarak HDInsight (Apache Hadoop) bağlanma](hdinsight-hadoop-linux-use-ssh-unix.md).

    * **Kaynak grubu** için yeni bir kaynak grubu oluşturmayı veya mevcut bir kaynak grubunu kullanmayı seçin.

    * Bir veri merkezine belirtin **konumu** kümenin oluşturulduğu.

    * Seçin **sonraki** sonraki sayfaya taşınır.

4. Gelen **2 güvenlik + ağ**, sağlanan açılan menüsünü kullanarak, kümenizin bir sanal ağa bağlanabilir. Bir sanal ağ kümesine yerleştirmek istiyorsanız bir Azure sanal ağı ve alt ağ seçin. Bir sanal ağda HDInsight'ı kullanma hakkında daha fazla bilgi için bkz. [kullanarak bir Azure sanal ağ genişletme HDInsight özellikleri](hdinsight-extend-hadoop-virtual-network.md). Makalede, sanal ağ için belirli yapılandırma gereksinimlerini içerir. 

    Kullanmak istiyorsanız **Kurumsal güvenlik paketi**, bu yönergeleri izleyin: [Azure Active Directory Domain Services'ı kullanarak bir HDInsight kümesi ile Kurumsal güvenlik paketi yapılandırma](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-configure-using-azure-adds).

    Seçin **sonraki** sonraki sayfaya taşınır.


5. Gelen **3 depolama**, varsayılan depolama alanı olarak Azure depolama veya Azure Data Lake Storage istediğinizi belirtin. Daha fazla bilgi için aşağıdaki tabloya bakın.

     ![Depolama ayarlarını](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-storage.png "Azure portalında yeni bir küme oluşturma")

     | Depolama                                      | Açıklama |
     |----------------------------------------------|-------------|
     | **Varsayılan depolama alanı olarak Azure depolama blobları**   | <ul><li>İçin **birincil depolama türü**seçin **Azure depolama**. İçin **seçme yöntemi**, seçin **Aboneliklerim** Azure aboneliğinizin bir parçası olan bir depolama hesabı belirtmek istiyorsanız. Ardından depolama hesabını seçin. Aksi takdirde seçin **erişim anahtarı**. Ardından Azure aboneliğinizde dışında arasından istediğiniz depolama hesabı için bilgileri sağlayın.</li><li>İçin **varsayılan kapsayıcı**, portal tarafından önerilen varsayılan kapsayıcı adı seçin veya kendi belirtin.</li><li>Ayrıca, varsayılan depolama alanı Azure Blob Depolama ise seçebilirsiniz **ek depolama hesapları** kümesi ile ilişkilendirmek üzere ek depolama hesapları belirtmek için. İçin **Azure depolama anahtarları**seçin **depolama anahtarı Ekle**. Ardından, bir depolama hesabı Azure Abonelikleriniz veya diğer Aboneliklerdeki sağlayabilirsiniz. Depolama hesabı erişim anahtarı sağlayın.</li><li>Varsayılan depolama alanınızı BLOB Depolama ise de seçebilirsiniz **Data Lake Storage erişim** ek depolama alanı olarak Azure Data Lake Storage belirtmek için. Daha fazla bilgi için [hızlı başlangıç: HDInsight kümelerinde ayarlama](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).</li></ul> |
     | **Varsayılan depolama alanı olarak Azure Data Lake depolama** | İçin **birincil depolama türü**seçin **Azure Data Lake depolama Gen1** veya **Azure Data Lake depolama Gen2**. Ardından makalesine bakabilirsiniz [hızlı başlangıç: HDInsight kümelerinde ayarlama](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md) yönergeler için. |
     | **Dış meta depolar**                      | Bir seçenek olarak, kümeyle ilişkili Apache Hive ve Apache Oozie meta verileri kaydetmek için bir SQL veritabanını belirtin. İçin **Hive için bir SQL veritabanı seçin**, SQL veritabanı'nı seçin. Ardından veritabanı kullanıcı adı ve parola sağlayın. Oozie meta verileri için bu adımları yineleyin.<br><br>Azure SQL veritabanı için meta depolar kullanma hakkında bazı önemli noktalar aşağıdaki gibidir: <ul><li>Meta veri deposu için kullanılan Azure SQL veritabanı, Azure HDInsight da dahil olmak üzere diğer Azure hizmetlerine izin vermeniz gerekir. Azure SQL veritabanı panonun sağ tarafında, sunucu adını seçin. Bu sunucu, SQL veritabanı örneği üzerinde çalıştığı sunucudur. Sunucu görünümündesiniz sonra seçin **yapılandırma**. Ardından **Azure Hizmetleri**seçin **Evet**. Daha sonra **Kaydet**’e tıklayın.</li><li>Meta veri deposu oluşturduğunuzda, kısa çizgi veya kısa çizgi ile bir veritabanı adı yok. Bu karakterler, küme oluşturma işlemi başarısız olmasına neden olabilir.</li></ul> |

     > [!WARNING]  
     > HDInsight kümesinden farklı bir konumda ek depolama hesabı kullanımı desteklenmiyor.

     Seçin **sonraki** sonraki sayfaya taşınır.


6. Gelen **4 uygulamalar (isteğe bağlı)** , istediğiniz tüm uygulamaları seçin. Microsoft, bağımsız yazılım satıcılarına (ISV) veya bu uygulamaları geliştirebilirsiniz. Daha fazla bilgi için [küme oluşturma sırasında uygulamaları yüklemek](hdinsight-apps-install-applications.md#install-applications-during-cluster-creation).

    Seçin **sonraki** sonraki sayfaya taşınır.


6. **Küme boyutu 5** bu küme için kullanılan düğümleri hakkında bilgileri görüntüler. Küme için gereksinim duyduğunuz çalışan düğümleri sayısını ayarlayın. Küme çalıştırmanın tahmini maliyeti de gösterilir.
   
    ![Belirtin düğüm fiyatlandırma katmanları](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-nodes.png "küme düğümleri sayısı belirtin")
   
   > [!IMPORTANT]  
   > 32'den fazla çalışan düğümlerinde planlıyorsanız, en az sekiz çekirdek ve 14 GB RAM ile bir baş düğüm boyutu seçin. Küme oluşturma sırasında veya Küme oluşturulduktan sonra ölçeklendirme düğümleri planlayın. 
   > 
   > Düğüm boyutlarına ve ilişkili maliyetler hakkında daha fazla bilgi için bkz. [Azure HDInsight fiyatlandırması](https://azure.microsoft.com/pricing/details/hdinsight/).
   
    Seçin **sonraki** sonraki sayfaya taşınır.

8. Gelen **6 betik eylemleri**, özel bileşenleri yüklemek için bir küme özelleştirebilirsiniz. Küme oluşturulurken bir küme özelleştirmek için özel bir betik kullanmak istiyorsanız bu seçeneği çalışır. Betik eylemleri hakkında daha fazla bilgi için bkz: [özelleştirme Linux tabanlı HDInsight kümelerini betik eylemlerini kullanarak](hdinsight-hadoop-customize-cluster-linux.md).

   Seçin **sonraki** sonraki sayfaya taşınır.

9. Gelen **7 özeti**, daha önce girdiğiniz bilgileri doğrulayın. Ardından **Oluştur**’u seçin.

     ![Yapılandırmaları onaylayın](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-summary.png "küme düğümleri sayısı belirtin")
    
    > [!NOTE]  
    > Kümenin oluşturulması genellikle yaklaşık 20 dakika sürer. İzleyici **bildirimleri** sağlama işlemini denetlemek için.

10. Oluşturma işlemi bittikten sonra seçin **kaynağa Git** gelen **dağıtım başarılı** bildirim. Küme penceresi, aşağıdaki bilgileri sağlar.
    
    ![Küme arabirimi](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-completed.png "küme özellikleri")
    
    Simgeleri penceresinde şu şekilde açıklanmıştır:
    
    * **Genel bakış** sekmesi, kümenin tüm önemli bilgileri sağlar. Ad, ait olduğu kaynak grubu, konum, işletim sistemi ve küme Panosu URL'sini verilebilir.
    * **Pano** kümeyle ilişkili Ambari portalına yönlendirir.
    * **Güvenli Kabuk** SSH kullanarak kümeye erişmek için gereken bilgileri sağlar.
    * Kullanarak **ölçek kümesi**, kümeyle ilişkili çalışan düğümlerinin sayısını artırabilirsiniz.
    * **Silme** HDInsight kümesini siler.
    

## <a name="customize-clusters"></a>Kümeleri özelleştirme
* [Önyükleme kullanarak HDInsight kümelerini özelleştirin](hdinsight-hadoop-customize-cluster-bootstrap.md)
* [Betik eylemlerini kullanarak Linux tabanlı HDInsight kümeleri özelleştirme](hdinsight-hadoop-customize-cluster-linux.md)

## <a name="delete-the-cluster"></a>Küme silme
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Sorun giderme

HDInsight kümeleri oluştururken sorun yaşarsanız bkz. [erişim denetimi gereksinimleri](hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="next-steps"></a>Sonraki adımlar
Bir HDInsight kümesi başarıyla oluşturdunuz. Artık kümenizle çalışmayı öğrenin.

### <a name="apache-hadoop-clusters"></a>Apache Hadoop kümelerini
* [Apache Hive, HDInsight ile kullanma](hadoop/hdinsight-use-hive.md)
* [Apache Pig, HDInsight ile kullanma](hadoop/hdinsight-use-pig.md)
* [HDInsight ile MapReduce kullanma](hadoop/hdinsight-use-mapreduce.md)

### <a name="apache-hbase-clusters"></a>Apache HBase kümeleri
* [HDInsight üzerinde Apache HBase kullanmaya başlama](hbase/apache-hbase-tutorial-get-started-linux.md)
* [HDInsight üzerinde Apache HBase için Java uygulamaları geliştirin](hbase/apache-hbase-build-java-maven-linux.md)

### <a name="apache-storm-clusters"></a>Apache Storm kümeleri
* [HDInsight üzerinde Apache Storm için Java topolojileri geliştirme](storm/apache-storm-develop-java-topology.md)
* [HDInsight üzerinde Apache Storm, Python bileşenlerini kullanma](storm/apache-storm-develop-python-topology.md)
* [HDInsight üzerinde Apache Storm topolojileri dağıtma ve izleme](storm/apache-storm-deploy-monitor-topology-linux.md)

### <a name="apache-spark-clusters"></a>Apache Spark kümeleri
* [Scala kullanarak bağımsız uygulama oluşturma](spark/apache-spark-create-standalone-application.md)
* [İşleri uzaktan bir Apache Spark kümesi üzerinde Apache Livy kullanarak çalıştırma](spark/apache-spark-livy-rest-interface.md)
* [Apache Spark ile BI: BI araçları ile HDInsight Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](spark/apache-spark-use-bi-tools.md)
* [Apache Spark Machine Learning ile: Gıda denetimi sonuçlarını tahmin etmek için HDInsight içindeki Spark kullanma](spark/apache-spark-machine-learning-mllib-ipython.md)

