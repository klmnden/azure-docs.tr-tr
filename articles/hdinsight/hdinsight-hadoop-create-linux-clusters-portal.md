---
title: "Kullanarak bir web tarayıcısı - Azure Hdınsight Hadoop kümeleri oluşturma | Microsoft Docs"
description: "Bir web tarayıcısı ve Azure Önizleme portalını kullanarak Hdınsight için Linux'ta Hadoop, HBase, Storm ve Spark kümeleri oluşturmayı öğrenin."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 697278cf-0032-4f7c-b9b2-a84c4347659e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: 812b6f323e2ddaee9095a7bdf221d6a8ebd69fd2
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
---
# <a name="create-linux-based-clusters-in-hdinsight-using-the-azure-portal"></a>Azure portalını kullanarak Hdınsight'ta Linux tabanlı kümeleri oluşturma
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Azure portalı, hizmetleri ve Microsoft Azure bulutunda barındırılan kaynaklar için bir web tabanlı yönetim aracıdır. Bu makalede, portal kullanarak Linux tabanlı Hdınsight kümeleri oluşturmak öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Modern bir web tarayıcısı**. Azure portalı, HTML5 ve Javascript kullanır ve eski web tarayıcısında doğru şekilde çalışmayabilir.

## <a name="create-clusters"></a>Küme oluşturma
Azure portalı küme özelliklerinin çoğu kullanıma sunar. Azure Resource Manager şablonu kullanarak, çok sayıda ayrıntıları gizleyebilirsiniz. Daha fazla bilgi için bkz: [oluşturma Linux tabanlı Hadoop kümeleri Azure Resource Manager şablonları kullanarak Hdınsight'ta](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]


1. [Azure portalında](https://portal.azure.com) oturum açın.
2. Tıklatın  **+** , tıklatın **Intelligence + analiz**ve ardından **Hdınsight**.
   
    ![Azure portalında yeni bir küme oluşturma](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster.png "Azure portalında yeni bir küme oluşturma")

3. İçinde **Hdınsight** dikey penceresinde tıklatın **özel (boyutu, ayarları, uygulamalar)**, tıklatın **Temelleri**ve aşağıdaki bilgileri girin.

    ![Azure portalında yeni bir küme oluşturma](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-basics.png "Azure portalında yeni bir küme oluşturma")

    * **Küme Adı** girin: Bu ad genel olarak benzersiz olmalıdır.

    * Gelen **abonelik** açılan listesinde, küme için kullanılacak Azure aboneliğini seçin.

    * Tıklatın **küme türü**ve ardından seçin:
   
        * **Küme Türü**: Neyi seçeceğinizi bilmiyorsanız **Hadoop** seçeneğini belirleyin. En popüler küme türü budur.
     
            > [!IMPORTANT]
            > Hdınsight kümeleri gelen iş yükü veya küme için ayarlanmış teknoloji karşılık türleri çeşitli. Bir küme üzerinde Storm ve HBase gibi birden çok tür birleştiren bir küme oluşturmak için desteklenen yöntem yoktur. 
            > 
            > 
        
        * **İşletim Sistemi**: **Linux** seçeneğini belirleyin.
        
        * **Sürüm**: Neyi seçeceğinizi bilmiyorsanız varsayılan sürümü kullanın. Daha fazla bilgi için bkz. [HDInsight küme sürümleri](hdinsight-component-versioning.md).
        

    * İçin **küme oturum açma kullanıcı** ve **küme oturum açma parolasını**, yönetici kullanıcı için kullanıcı adı ve parola sağlayın.

    * Girin bir **SSH kullanıcı adı** ve daha önce belirtilen yönetici parolasına aynı, select SSH parolası sahip olmak istiyorsanız **küme oturum açma aynı parolayı kullanın** onay kutusu. Değilse, ya da sağlayan bir **parola** veya **ortak anahtar**, SSH kullanıcısının kimliğini doğrulayacak. Ortak anahtar kullanılması önerilen yaklaşımdır. Alt kısımdaki **Seç**’e tıklayarak kimlik bilgileri yapılandırmasını kaydedin.
   
        Bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

    * İçin **kaynak grubu**, yeni bir kaynak grubu oluşturmak veya mevcut bir kullanmak isteyip istemediğinizi belirtin.

    * Bir veri merkezi belirtin **konumu** burada küme oluşturulur.

    * **İleri**’ye tıklayın.

4. Üzerinde **depolama** dikey penceresinde, varsayılan depolama alanı olarak Azure Storage (WASB) veya Data Lake Store istediğinizi belirtin. Daha fazla bilgi için aşağıdaki tabloya bakın.

    ![Azure portalında yeni bir küme oluşturma](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-storage.png "Azure portalında yeni bir küme oluşturma")

    | Depolama                                      | Açıklama |
    |----------------------------------------------|-------------|
    | **Azure Storage Bloblarında varsayılan depolama**   | <ul><li>İçin **birincil depolama türü**seçin **Azure Storage**. Bundan sonra için **seçim yöntemini**, seçebileceğiniz **My abonelikleri** , Azure aboneliğinizin bir parçası olan bir depolama hesabı belirtin ve ardından depolama hesabını seçin istiyorsanız. Aksi takdirde tıklatın **erişim tuşu** ve Azure aboneliğinize dışında seçmek istediğiniz depolama hesabı için bilgileri sağlayın.</li><li>İçin **varsayılan kapsayıcı**, portal tarafından önerilen varsayılan kapsayıcı adıyla gidin veya kendi koşulunuzu belirtmek seçebilirsiniz.</li><li>Varsayılan depolama alanı olarak WASB kullanıyorsanız (isteğe bağlı) tıklayabilirsiniz **ek depolama hesapları** kümesi ile ilişkilendirmek için ek depolama hesapları belirtmek için. İçinde **Azure depolama anahtarları** dikey penceresinde tıklatın **depolama anahtarı eklemek**, ve ardından, bir depolama hesabı, Azure aboneliklerinize veya diğer abonelikler (depolama hesabı erişim sağlayarak sağlayabilir anahtar).</li><li>Varsayılan depolama alanı olarak WASB kullanıyorsanız (isteğe bağlı) tıklayabilirsiniz **Data Lake Store erişim** ek depolama alanı olarak Azure Data Lake Store belirtmek için. Daha fazla bilgi için bkz: [Azure Portal'ı kullanarak Data Lake Store ile bir Hdınsight kümesi oluşturmayı](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</li></ul> |
    | **Azure Data Lake Store varsayılan depolama** | İçin **birincil depolama türü**seçin **Data Lake Store** ve makalesine başvurun [Azure Portal'ı kullanarak Data Lake Store ile bir Hdınsight kümesi oluşturmayı](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md) yönergeler için. |
    | **Dış meta deponuz**                      | İsteğe bağlı olarak, kümeyle ilişkili Hive ve Oozie meta verileri kaydetmek için bir SQL veritabanı belirtebilirsiniz. İçin **bir SQL veritabanı için Hive seçin** bir SQL veritabanını seçin ve ardından veritabanı için kullandığınız kullanıcı adı/parola sağlayın. Oozie meta verileri için bu adımları yineleyin.<br><br>Azure SQL veritabanı için meta deponuz kullanırken bazı noktalar. <ul><li>Meta depo için kullanılan Azure SQL veritabanı Azure Hdınsight gibi diğer Azure hizmetlerine bağlantıyı izin vermeniz gerekir. Azure SQL veritabanı Panoda sağ tarafında sunucu adını tıklatın. Bu, SQL veritabanı örneğinin çalıştığı sunucudur. Sunucu görünümünde olduktan sonra tıklatın **yapılandırma**ve ardından **Azure Hizmetleri**, tıklatın **Evet**ve ardından **kaydetmek**.</li><li>Bu küme oluşturma işleminin başarısız olmasına neden bir meta depo oluştururken, kısa çizgi veya kısa çizgi, içeren bir veritabanı adı kullanmayın.</li></ul>                                                                                                                                                                       |

    **İleri**’ye tıklayın. 

    > [!WARNING]
    > HDInsight kümesinden farklı bir konumda ek depolama hesabının kullanılması desteklenmez.

5. İsteğe bağlı olarak, tıklayın **uygulamaları** Hdınsight kümeleri ile çalışma uygulamaları yüklemek için. Bu uygulamalar Microsoft veya bağımsız yazılım satıcıları (ISV) tarafından ya da sizin tarafınızdan geliştirilebilir. Daha fazla bilgi için bkz: [yükleme Hdınsight uygulamaları](hdinsight-apps-install-applications.md#install-applications-during-cluster-creation).


6. Tıklatın **küme boyutu** bu küme için oluşturulan düğümleri hakkındaki bilgileri görüntülemek için. Küme için gereksinim duyduğunuz çalışan düğümü sayısını ayarlayın. Kümenin tahmini maliyeti, dikey pencerede gösterilir.
   
    ![Düğüm fiyatlandırma katmanları dikey](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-nodes.png "Küme düğüm sayısını belirtin")
   
   > [!IMPORTANT]
   > Düğümlerde 32'den fazla worker, küme oluşturma sırasında ya da Küme oluşturulduktan sonra ölçeklendirme planlıyorsanız bir baş düğüm boyutu en az 8 çekirdek ve 14 GB ram ile seçmeniz gerekir.
   > 
   > Düğümü boyutları ve ilişkili maliyetler hakkında daha fazla bilgi için bkz: [Hdınsight fiyatlandırma](https://azure.microsoft.com/pricing/details/hdinsight/).
   > 
   > 
   
   Tıklatın **sonraki** yapılandırma fiyatlandırma düğümü kaydetmek için.

7. Tıklatın **Gelişmiş ayarları** kullanarak gibi isteğe bağlı diğer ayarları yapılandırmak için **betik eylemleri** özel bileşenler veya birleştirme yüklemek için bir küme özelleştirmek için bir **sanal ağ**. Daha fazla bilgi için aşağıdaki tabloya bakın.

    ![Düğüm fiyatlandırma katmanları dikey](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-advanced.png "Küme düğüm sayısını belirtin")

    | Seçenek | Açıklama |
    |--------|-------------|
    | **Betik eylemleri** | Küme oluşturuldu olarak bir küme özelleştirmek için özel bir komut dosyası kullanmak istiyorsanız bu seçeneği kullanın. Betik eylemleri hakkında daha fazla bilgi için bkz: [özelleştirme Hdınsight kümeleri betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md). |
    | **Sanal Ağ** | Bir sanal ağ kümesine yerleştirmek istiyorsanız bir Azure sanal ağı ve alt ağ seçin. Bir ağla sanal sanal ağ için belirli yapılandırma gereksinimlerini de dahil olmak üzere, Hdınsight kullanma hakkında bilgi için bkz: [bir Azure sanal ağı kullanarak genişletme Hdınsight yetenekleri](hdinsight-extend-hadoop-virtual-network.md). |

    **İleri**’ye tıklayın.

8. Üzerinde **Özet** dikey penceresinde, daha önce girdiğiniz bilgileri doğrulayın ve ardından **oluşturma**.

    ![Düğüm fiyatlandırma katmanları dikey](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-summary.png "Küme düğüm sayısını belirtin")
    
    > [!NOTE]
    > Bu, genellikle yaklaşık 15 dakika oluşturulacak küme için biraz zaman alabilir. Başlangıç Panosu'nu üzerinde döşeme kullanın veya **bildirimleri** sağlama işlemi denetlemek için sayfanın sol giriş.
    > 
    > 
12. Oluşturma işlemi tamamlandıktan sonra küme dikey penceresini başlatmak üzere başlangıç Panosu'nu kümeden kutucuğuna tıklayın. Küme dikey penceresinde aşağıdaki bilgileri sağlar.
    
    ![Küme dikey](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-completed.png "küme özellikleri")
    
    Bu dikey pencerenin üstündeki simgeleri anlamak için aşağıdakileri kullanın.
    
    * **Genel Bakış** sekmesi, küme adı, ait kaynak grubu, konum, işletim sistemi, küme Panosu, vb. için URL gibi ilgili tüm önemli bilgileri sağlar.
    * **Pano** kümeyle ilişkili ambarı portalına yönlendirir.
    * **Kabuk güvenli**: SSH kullanarak kümeye erişmek gerekli bilgiler.
    * **Ölçek kümesi** kümeyle ilişkili alt düğümlerin sayısını artırmak sağlar.
    * **Silme**: Hdınsight kümesi siler.
    

## <a name="customize-clusters"></a>Kümeleri özelleştirme
* Bkz: [önyükleme kullanarak özelleştirme Hdınsight kümelerini](hdinsight-hadoop-customize-cluster-bootstrap.md).
* Bkz: [özelleştirme Hdınsight kümeleri betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="delete-the-cluster"></a>Küme silme
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Sorun giderme

HDInsight kümeleri oluştururken sorun yaşarsanız bkz. [erişim denetimi gereksinimleri](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Sonraki adımlar
Hdınsight kümesi başarıyla oluşturuldu, kümenizi ile çalışmayı öğrenmek için aşağıdakileri kullanın:

### <a name="hadoop-clusters"></a>Hadoop kümeleri
* [HDInsight ile Hive kullanma](hadoop/hdinsight-use-hive.md)
* [HDInsight ile Pig kullanma](hadoop/hdinsight-use-pig.md)
* [Hdınsight ile MapReduce kullanma](hadoop/hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase kümeleri
* [Hdınsight'ta HBase kullanmaya başlama](hbase/apache-hbase-tutorial-get-started-linux.md)
* [Hdınsight'ta HBase için Java uygulamaları geliştirme](hbase/apache-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Storm kümeleri
* [Hdınsight üzerinde Storm için Java topolojisi geliştirme](storm/apache-storm-develop-java-topology.md)
* [Hdınsight üzerinde Storm Python bileşenleri kullanma](storm/apache-storm-develop-python-topology.md)
* [Dağıtma ve hdınsight'ta Storm topolojileri izleme](storm/apache-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a>Spark kümeleri
* [Scala kullanarak tek başına uygulama oluşturma](spark/apache-spark-create-standalone-application.md)
* [Livy kullanarak Spark kümesinde işleri uzaktan çalıştırma](spark/apache-spark-livy-rest-interface.md)
* [BI ile Spark: BI araçlarıyla HDInsight’ta Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](spark/apache-spark-use-bi-tools.md)
* [Machine Learning ile Spark: Yemek inceleme sonuçlarını tahmin etmek için HDInsight’ta Spark kullanma](spark/apache-spark-machine-learning-mllib-ipython.md)
* [Spark Akış: Gerçek zamanlı akış uygulamaları oluşturmak için HDInsight’ta Spark kullanma](spark/apache-spark-eventhub-streaming.md)

