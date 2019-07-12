---
title: 'Hızlı Başlangıç: -Azure HDInsight Azure portalını kullanarak Apache Hive ve Apache Hadoop kullanmaya başlama'
description: Bu hızlı başlangıçta, bir HDInsight Hadoop kümesi oluşturmak için Azure portalını kullanın
keywords: hadoop kullanmaya başlama,hadoop linux,hadoop hızlı başlangıç,hive kullanmaya başlama,hive hızlı başlangıç
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017,mvc,seodec18
ms.topic: quickstart
ms.date: 07/02/2019
ms.author: hrasheed
ms.openlocfilehash: f92cb247afb25562a96373c28534549a2f16d8c9
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67805581"
---
# <a name="quickstart-create-apache-hadoop-cluster-in-azure-hdinsight-using-azure-portal"></a>Hızlı Başlangıç: Azure portalını kullanarak Azure HDInsight Apache Hadoop kümesi oluşturma

Bu makalede, nasıl oluşturulacağını öğreneceksiniz [Apache Hadoop](https://hadoop.apache.org/) Azure portalını kullanarak ve ardından HDInsight Apache Hive işlerini çalıştırın HDInsight kümeleri. Hadoop işlerinin çoğu toplu işlemdir. Bir küme oluşturur, bazı işleri çalıştırır ve kümeyi silersiniz. Bu makalede, üç görevi de gerçekleştirirsiniz.

Bu hızlı başlangıçta, HDInsight Hadoop kümesi oluşturmak için Azure portalını kullanırsınız. [Azure Resource Manager şablonunu](apache-hadoop-linux-tutorial-get-started.md) kullanarak da küme oluşturabilirsiniz.

Şu anda HDInsight [yedi farklı küme türüyle](./apache-hadoop-introduction.md#cluster-types-in-hdinsight) ile birlikte gelir. Her küme türü farklı bir bileşen kümesini destekler. Tüm küme türleri Hive'ı destekler. HDInsight desteklenen bileşenlerin listesi için bkz. [HDInsight tarafından sağlanan Apache Hadoop küme sürümlerindeki yenilikler nelerdir?](../hdinsight-component-versioning.md)  

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="create-an-apache-hadoop-cluster"></a>Bir Apache Hadoop kümesi oluşturma

Bu bölümde, Azure portalını kullanarak HDInsight’ta Hadoop kümesi oluşturursunuz.

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. Azure portalından gidin **kaynak Oluştur** > **Analytics** > **HDInsight**.

    ![Azure portalında Databricks](./media/apache-hadoop-linux-create-cluster-get-started-portal/create-hdinsight.png "Databricks on Azure portal")

1. Altında **HDInsight** > **hızlı oluşturma** > **Temelleri**aşağıdaki değerleri seçin veya girin:

    |Özellik  |Açıklama  |
    |---------|---------|
    |Küme adı   | Hadoop kümesi için bir ad girin. HDInsight’taki tüm kümeler aynı DNS ad alanını paylaştığından, bu adın benzersiz olması gerekir. Ad harf, rakam ve kısa çizgi de dahil olmak üzere en fazla 59 karakterlerinden oluşabilir. Adın ilk ve son karakterleri, kısa çizgi olamaz. |
    |Subscription    |  Azure aboneliğinizi seçin. |
    |Küme türü     | Bunu şimdilik atlayın. Bu yordamın sonraki adımında bu girdiyi sağlayacaksınız.|
    |Küme oturum açma kullanıcı adı ve parola    | Varsayılan oturum açma adı **admin**’dir. Parola en az 10 karakter uzunluğunda olmalıdır ve en az bir rakam, bir büyük harf, bir küçük harf, bir alfasayısal olmayan karakter (' " ` karakterleri hariç\) içermelidir. "Pass@word1" gibi genel parolalar **sağlamadığınızdan** emin olun.|
    |Secure Shell (SSH) kullanıcı adı | Varsayılan kullanıcı adı **sshuser** şeklindedir.  SSH kullanıcı adı için başka bir ad sağlayabilirsiniz. |
    |Küme oturum açma parolası için SSH kullanın.| SSH kullanıcısı için, küme oturum açma kullanıcısı için sağlananla aynı parolayı kullanmak için bu onay kutusunu seçin.|
    |Resource group     | Bir kaynak grubu oluşturun veya mevcut bir kaynak grubunu seçin.  Kaynak grubu, Azure bileşenleri için bir kapsayıcıdır.  Bu durumda, kaynak grubu HDInsight kümesini ve bağımlı Azure Depolama hesabını içermektedir. |
    |Location    | Kümenizi oluşturmak istediğiniz bir Azure konumunu seçin.  Daha iyi performans için kendinize yakın bir konum seçin. |

    ![HDInsight Linux - Küme temel değerleri sağlamaya başlama](./media/apache-hadoop-linux-create-cluster-get-started-portal/quick-create-basics.png "HDInsight kümesi oluşturmak için temel değerler sağlama")

1. Seçin **küme türü** açmak için **küme yapılandırması** sayfasında ve ardından aşağıdaki değerleri sağlayın:

    |Özellik  |Açıklama  |
    |---------|---------|
    |Küme türü     | **Hadoop**’u seçin |
    |Version     | **Hadoop 2.7.3 (HDI 3.6)** seçeneğini belirleyin|

    ![HDInsight Linux - Küme temel değerleri sağlamaya başlama](./media/apache-hadoop-linux-create-cluster-get-started-portal/cluster-configuration-hadoop.png "HDInsight kümesi oluşturmak için temel değerler sağlama")

    Seçin **seçin** seçip **sonraki** depolama ayarları ilerlemek için.

1. Gelen **depolama** sekmesinde, aşağıdaki değerleri sağlayın:

    |Özellik  |Açıklama  |
    |---------|---------|
    |Birincil depolama türü    | Bu makale için Azure depolama blobu varsayılan depolama hesabı olarak kullanmak için Azure storage'ı seçin. Varsayılan depolama alanı olarak Azure Data Lake Storage’ı da kullanabilirsiniz. |
    |Seçim yöntemi     |  Bu makale için, **Aboneliklerim**’i seçerek Azure aboneliğinizdeki bir depolama hesabını kullanın. Diğer aboneliklerdeki depolama hesabını kullanmak için, **Erişim anahtarı**’nı seçin ve sonra o hesaba ilişkin erişim anahtarını sağlayın. |
    |Bir depolama hesabı seçin   | Seçin **bir depolama hesabı seçin** mevcut bir depolama hesabını seçin veya **Yeni Oluştur**. Yeni bir hesap oluşturursanız, adı 3 ila 24 karakter uzunluğunda olmalıdır ve yalnızca sayılar ve küçük harfler içerebilir.|

    Diğer tüm varsayılan değerleri kabul edin ve ardından **sonraki** Özet sayfaya ilerlemek için.

    ![HDInsight Linux - Küme depolama değerleri sağlamaya başlama](./media/apache-hadoop-linux-create-cluster-get-started-portal/quick-create-storage.png "HDInsight kümesi oluşturmak için depolama değerleri sağlama")

1. Gelen **özeti** sekmesinde, önceki adımda seçtiğiniz değerleri doğrulayın.

    ![HDInsight Linux - Küme özetini kullanmaya başlama](./media/apache-hadoop-linux-create-cluster-get-started-portal/quick-create-summary.png "HDInsight Linux - Küme özetini kullanmaya başlama")

1. **Oluştur**’u seçin. Bir küme oluşturmak yaklaşık 20 dakika sürer.

1. Küme oluşturulduktan sonra, Azure portalında kümeye genel bakış sayfasını görürsünüz.

    ![HDInsight - Linux - başlarken - küme ayarları](./media/apache-hadoop-linux-create-cluster-get-started-portal/cluster-overview.png "HDInsight küme özellikleri")    

    Her kümenin bir [Azure Depolama hesabı](../hdinsight-hadoop-use-blob-storage.md) veya [Azure Data Lake hesabı](../hdinsight-hadoop-use-data-lake-store.md) bağımlılığı vardır. Bu genellikle varsayılan depolama hesabı olarak ifade edilir. HDInsight kümesi ve kümenin varsayılan depolama hesabının aynı Azure bölgesinde birlikte gerekir. Kümeleri silmek depolama hesabını silmez.

    > [!NOTE]  
    > Diğer küme oluşturma yöntemleri ve bu hızlı başlangıçta kullanılan özellikler hakkında bilgi edinmek bkz [oluşturma HDInsight kümeleri](../hdinsight-hadoop-provision-linux-clusters.md).

## <a name="run-apache-hive-queries"></a>Apache Hive sorguları çalıştırma

[Apache Hive](hdinsight-use-hive.md) HDInsight’ta kullanılan en popüler bileşendir. HDInsight’ta Hive işleri çalıştırmanın birçok yolu vardır. Bu hızlı başlangıçta, portalda Ambari Hive görünümünü kullanırsınız. Hive işlerini göndermenin diğer yöntemleri için bkz. [HDInsight’ta Hive kullanma](hdinsight-use-hive.md).

1. Ambari’yi açmak için, önceki ekran görüntüsünden **Küme Panosu**’nu seçin.  De göz atabilirsiniz `https://ClusterName.azurehdinsight.net`burada `ClusterName` önceki bölümde oluşturduğunuz kümedir.

    ![HDInsight Linux - Küme panosunu kullanmaya başlama](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-open-cluster-dashboard.png "HDInsight Linux - Küme panosunu kullanmaya başlama")

2. Kümeyi oluştururken belirlediğiniz Hadoop kullanıcı adını ve parolasını girin. Varsayılan kullanıcı adı **admin** şeklindedir.

3. Aşağıdaki ekran görüntüsünde gösterildiği gibi **Hive Görünümü**’nü açın:

    ![Ambari görünümlerini seçme](./media/apache-hadoop-linux-tutorial-get-started/selecthiveview.png "HDInsight Hive Viewer menüsü")

4. Sayfadaki **SORGU** sekmesinde, aşağıdaki HiveQL ifadelerini çalışma sayfasına yapıştırın:

    ```sql
    SHOW TABLES;
    ```

    ![HDInsight Hive görünümleri](./media/apache-hadoop-linux-tutorial-get-started/hiveview-1.png "HDInsight Hive Görünümü Sorgu Düzenleyicisi")

5. **Yürüt**’ü seçin. **SORGU** sekmesinin altında bir **SONUÇLAR** sekmesi görünür. Bu sekmede işle ilgili bilgiler görüntülenir. 

    Sorgu tamamladığında, **sorgu** sekmesinde işlemin sonuçları görüntülenir. **hivesampletable** adlı bir tablo görürsünüz. Bu örnek Hive tablosu tüm HDInsight kümeleri ile birlikte gelir.

    ![HDInsight Hive görünümleri](./media/apache-hadoop-linux-tutorial-get-started/hiveview.png "HDInsight Hive Görünümü Sorgu Düzenleyicisi")

6. Aşağıdaki sorguyu çalıştırmak için 4. ve 5. adımı yineleyin:

    ```sql
    SELECT * FROM hivesampletable;
    ```

7. Ayrıca sorgunun sonuçlarını da kaydedebilirsiniz. Sağdaki menü düğmesini seçtikten sonra, sonuçları CSV dosyası olarak indirme veya kümeyle ilişkili depolama hesabında depolama seçeneklerinden birini belirleyin.

    ![Hive sorgusunun sonucunu kaydetme](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-linux-hive-view-save-results.png "Hive sorgusunun sonucunu kaydetme")

Hive işini tamamladıktan sonra, [sonuçları Azure SQL Veritabanı’na veya SQL Server veritabanına aktarabilirsiniz](apache-hadoop-use-sqoop-mac-linux.md), ayrıca [Excel kullanarak sonuçları görselleştirebilirsiniz](apache-hadoop-connect-excel-power-query.md). HDInsight Hive kullanma hakkında daha fazla bilgi için bkz. [Apache Hive ve HiveQL kullanma HDInsight, Apache Hadoop ile bir örnek Apache log4j dosyasını çözümlemek için](hdinsight-use-hive.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Hızlı başlangıcı tamamladıktan sonra kümeyi silmek isteyebilirsiniz. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır.

> [!NOTE]  
> Eğer *hemen* HDInsight üzerinde Hadoop kullanarak ETL işlemleri çalıştırma hakkında bilgi edinmek için sonraki makaleye geçmeden, çalıştıran kümeye tutmak isteyebilirsiniz. Bir Hadoop kümesini yeniden oluşturmak zorunda öğreticide olmasıdır. Ancak, sonraki makalede hemen kullanmayacaksanız kümeyi artık silmeniz gerekir.

### <a name="to-delete-the-cluster-andor-the-default-storage-account"></a>Küme ve/veya varsayılan depolama hesabını silmek için

1. Azure portalın bulunduğu tarayıcı sekmesine dönün. Kümeye genel bakış sayfasında olmalısınız. Yalnızca kümeyi silmek, ancak varsayılan depolama hesabını korumak istiyorsanız **Sil**’i seçin.

    ![HDInsight küme silme](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-delete-cluster.png "HDInsight kümesini silme")

2. Kümeyi ve varsayılan depolama hesabını silmek istiyorsanız, kaynak grubu sayfasını açmak için kaynak grubu adını (önceki ekran görüntüsünde vurgulanan) seçin.

3. **Kaynak grubunu sil**’i seçerek, kümeyi ve varsayılan depolama hesabını içeren kaynak grubunu silin. Kaynak grubu silindiğinde depolama hesabının da silindiğini unutmayın. Depolama hesabını tutmak istiyorsanız, yalnızca küme silmeyi seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Resource Manager şablonu kullanarak Linux tabanlı HDInsight kümesi oluşturma ve temel Hive sorguları gerçekleştirmeyi öğrendiniz. Sonraki makalede, HDInsight üzerinde Hadoop kullanarak ayıklama, dönüştürme ve yükleme (ETL) işlemi gerçekleştirmeyi öğreneceksiniz.

> [!div class="nextstepaction"]
>[Ayıklama, dönüştürme ve yükleme verilerini kullanarak HDInsight üzerinde etkileşimli sorgu](../interactive-query/interactive-query-tutorial-analyze-flight-data.md)
