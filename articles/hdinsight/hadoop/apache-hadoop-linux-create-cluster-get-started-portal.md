---
title: 'Hızlı Başlangıç: -Azure HDInsight Azure portalını kullanarak Apache Hive ve Apache Hadoop kullanmaya başlama'
description: Bu hızlı başlangıçta, bir HDInsight Hadoop kümesi oluşturmak için Azure portalını kullanın
keywords: hadoop kullanmaya başlama,hadoop linux,hadoop hızlı başlangıç,hive kullanmaya başlama,hive hızlı başlangıç
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017,mvc,seodec18
ms.topic: quickstart
ms.date: 06/12/2019
ms.author: hrasheed
ms.openlocfilehash: 020d1be0587214f560bcb0cb717ec9166302cf9a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67057579"
---
# <a name="quickstart-create-apache-hadoop-cluster-in-azure-hdinsight-using-azure-portal"></a>Hızlı Başlangıç: Azure portalını kullanarak Azure HDInsight Apache Hadoop kümesi oluşturma

Bu makalede, nasıl oluşturulacağını öğreneceksiniz [Apache Hadoop](https://hadoop.apache.org/) Azure portalını kullanarak ve ardından HDInsight Apache Hive işlerini çalıştırın HDInsight kümeleri. Hadoop işlerinin çoğu toplu işlemdir. Bir küme oluşturur, bazı işleri çalıştırır ve kümeyi silersiniz. Bu makalede, üç görevi de gerçekleştirirsiniz.

Bu hızlı başlangıçta, HDInsight Hadoop kümesi oluşturmak için Azure portalını kullanırsınız. [Azure Resource Manager şablonunu](apache-hadoop-linux-tutorial-get-started.md) kullanarak da küme oluşturabilirsiniz.

Şu anda HDInsight [yedi farklı küme türüyle](./apache-hadoop-introduction.md#cluster-types-in-hdinsight) ile birlikte gelir. Her küme türü farklı bir bileşen kümesini destekler. Tüm küme türleri Hive'ı destekler. HDInsight desteklenen bileşenlerin listesi için bkz. [HDInsight tarafından sağlanan Apache Hadoop küme sürümlerindeki yenilikler nelerdir?](../hdinsight-component-versioning.md)  

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="create-an-apache-hadoop-cluster"></a>Bir Apache Hadoop kümesi oluşturma

Bu bölümde, Azure portalını kullanarak HDInsight’ta Hadoop kümesi oluşturursunuz. 

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. Azure portalında **Kaynak oluşturun** > **Veri ve Analiz** > **HDInsight** seçeneklerini belirleyin. 

    ![Azure portalında Databricks](./media/apache-hadoop-linux-create-cluster-get-started-portal/azure-hdinsight-on-portal.png "Databricks on Azure portal")

2. **HDInsight** > **Hızlı Oluştur** > **Temel Bilgiler** bölümünde, aşağıdaki ekran görüntüsünde önerildiği şekilde değerleri sağlayın:

    ![HDInsight Linux - Küme temel değerleri sağlamaya başlama](./media/apache-hadoop-linux-create-cluster-get-started-portal/hdinsight-linux-get-started-portal-provide-basic-values.png "HDInsight kümesi oluşturmak için temel değerler sağlama")

    Aşağıdaki değerleri yazın veya seçin:
    
    |Özellik  |Açıklama  |
    |---------|---------|
    |**Küme adı**     | Hadoop kümesi için bir ad girin. HDInsight’taki tüm kümeler aynı DNS ad alanını paylaştığından, bu adın benzersiz olması gerekir. Ad, harf, rakam ve kısa çizgilerin dahil olduğu en fazla 59 karakterden oluşabilir. Adın ilk ve son karakterleri, kısa çizgi olamaz. |
    |**Abonelik**     |  Azure aboneliğinizi seçin. |
    |**Küme Türü**     | Bunu şimdilik atlayın. Bu yordamın sonraki adımında bu girdiyi sağlayacaksınız.|
    |**Küme oturum açma kullanıcı adı ve parolası**     | Varsayılan oturum açma adı **admin**’dir. Parola en az 10 karakter uzunluğunda olmalıdır ve en az bir rakam, bir büyük harf, bir küçük harf, bir alfasayısal olmayan karakter (' " ` karakterleri hariç\) içermelidir. "Pass@word1" gibi genel parolalar **sağlamadığınızdan** emin olun.|
    |**SSH kullanıcı adı** | Varsayılan kullanıcı adı **sshuser** şeklindedir.  SSH kullanıcı adı için başka bir ad sağlayabilirsiniz. |
    | **Küme oturum açmak için kullanılan parolayı kullan** | SSH kullanıcısı için, küme oturum açma kullanıcısı için sağlananla aynı parolayı kullanmak için bu onay kutusunu seçin.|
    |**Kaynak grubu**     | Bir kaynak grubu oluşturun veya mevcut bir kaynak grubunu seçin.  Kaynak grubu, Azure bileşenleri için bir kapsayıcıdır.  Bu durumda, kaynak grubu HDInsight kümesini ve bağımlı Azure Depolama hesabını içermektedir. |
    |**Konum**     | Kümenizi oluşturmak istediğiniz bir Azure konumunu seçin.  Daha iyi performans için kendinize yakın bir konum seçin. |
        
3. **Küme türü**’nü seçin ve sonra aşağıdaki ekran görüntüsünde gösterildiği şekilde girdileri sağlayın:

    ![HDInsight Linux - Küme temel değerleri sağlamaya başlama](./media/apache-hadoop-linux-create-cluster-get-started-portal/hdinsight-linux-get-started-portal-provide-cluster-type.png "HDInsight kümesi oluşturmak için temel değerler sağlama")

    Aşağıdaki değerleri seçin:
    
    |Özellik  |Açıklama  |
    |---------|---------|
    |**Küme türü**     | **Hadoop**’u seçin |
    |**İşletim sistemi**     | Seçin **Linux** |
    |**Sürüm**     | **Hadoop 2.7.3 (HDI 3.6)** seçeneğini belirleyin|

    **Seç**’e ve sonra **İleri**’ye tıklayın.

4. **Depolama** sekmesinde, aşağıdaki ekran görüntüsünde gösterildiği şekilde girdileri sağlayın:

    ![HDInsight Linux - Küme depolama değerleri sağlamaya başlama](./media/apache-hadoop-linux-create-cluster-get-started-portal/hdinsight-linux-get-started-portal-select-storage.png "HDInsight kümesi oluşturmak için depolama değerleri sağlama")

    Aşağıdaki değerleri seçin:
    
    |Özellik  |Açıklama  |
    |---------|---------|
    |**Birincil depolama türü**     | Bu makale için, **Azure depolama**’yı seçerek Azure Depolama Blobunu varsayılan depolama hesabı olarak kullanın. Varsayılan depolama alanı olarak Azure Data Lake Storage’ı da kullanabilirsiniz. |
    |**Seçim yöntemi**     |  Bu makale için, **Aboneliklerim**’i seçerek Azure aboneliğinizdeki bir depolama hesabını kullanın. Diğer aboneliklerdeki depolama hesabını kullanmak için, **Erişim anahtarı**’nı seçin ve sonra o hesaba ilişkin erişim anahtarını sağlayın. |
    |**Yeni depolama hesabı oluşturma**     | Depolama hesabına bir ad verin.|

    Diğer tüm varsayılan değerleri kabul edin ve **İleri**’yi seçin.

5. **Özet** sekmesinde, önceki adımlarda seçtiğiniz değerleri doğrulayın.

    ![HDInsight Linux - Küme özetini kullanmaya başlama](./media/apache-hadoop-linux-create-cluster-get-started-portal/hdinsight-linux-get-started-portal-summary.png "HDInsight Linux - Küme özetini kullanmaya başlama")
      
4. **Oluştur**’u seçin. Portal panosunda, **HDInsight için dağıtım gönderme** başlıklı yeni bir kutucuk görürsünüz. Bir küme oluşturmak yaklaşık 20 dakika sürer.

    ![HDInsight Linux yeni başlayanlara yönelik kaynak grubu](./media/apache-hadoop-linux-create-cluster-get-started-portal/deployment-progress-tile.png "Azure HDInsight küme kaynağı grubu")

4. Küme oluşturulduktan sonra, Azure portalında kümeye genel bakış sayfasını görürsünüz.
   
    ![HDInsight - Linux - başlarken - küme ayarları](./media/apache-hadoop-linux-create-cluster-get-started-portal/hdinsight-linux-get-started-cluster-settings.png "HDInsight küme özellikleri")    
    
    Her kümenin bir [Azure Depolama hesabı](../hdinsight-hadoop-use-blob-storage.md) veya [Azure Data Lake hesabı](../hdinsight-hadoop-use-data-lake-store.md) bağımlılığı vardır. Bu genellikle varsayılan depolama hesabı olarak ifade edilir. HDInsight kümesi ve kümenin varsayılan depolama hesabının aynı Azure bölgesinde bulunması gerekir. Kümeleri silmek depolama hesabını silmez.

    > [!NOTE]  
    > Diğer küme oluşturma yöntemleri ve bu öğreticide kullanılan özellikler hakkında bilgi edinmek için bkz. [HDInsight kümeleri oluşturma](../hdinsight-hadoop-provision-linux-clusters.md).       

## <a name="run-apache-hive-queries"></a>Apache Hive sorguları çalıştırma

[Apache Hive](hdinsight-use-hive.md) HDInsight’ta kullanılan en popüler bileşendir. HDInsight’ta Hive işleri çalıştırmanın birçok yolu vardır. Bu öğreticide, portalda Ambari Hive görünümünü kullanırsınız. Hive işlerini göndermenin diğer yöntemleri için bkz. [HDInsight’ta Hive kullanma](hdinsight-use-hive.md).

1. Ambari’yi açmak için, önceki ekran görüntüsünden **Küme Panosu**’nu seçin.  Ayrıca, **https://&lt;ClusterName>.azurehdinsight.net** adresine de gidebilirsiniz. Burada &lt;ClusterName> değeri, önceki bölümde oluşturduğunuz kümedir.

    ![HDInsight Linux - Küme panosunu kullanmaya başlama](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-open-cluster-dashboard.png "HDInsight Linux - Küme panosunu kullanmaya başlama")

2. Kümeyi oluştururken belirlediğiniz Hadoop kullanıcı adını ve parolasını girin. Varsayılan kullanıcı adı **admin** şeklindedir.

3. Aşağıdaki ekran görüntüsünde gösterildiği gibi **Hive Görünümü**’nü açın:
   
    ![Ambari görünümlerini seçme](./media/apache-hadoop-linux-tutorial-get-started/selecthiveview.png "HDInsight Hive Viewer menüsü")

4. Sayfadaki **SORGU** sekmesinde, aşağıdaki HiveQL ifadelerini çalışma sayfasına yapıştırın:
   
        SHOW TABLES;

    ![HDInsight Hive görünümleri](./media/apache-hadoop-linux-tutorial-get-started/hiveview-1.png "HDInsight Hive Görünümü Sorgu Düzenleyicisi")     

5. **Yürüt**’ü seçin. **SORGU** sekmesinin altında bir **SONUÇLAR** sekmesi görünür. Bu sekmede işle ilgili bilgiler görüntülenir. 
   
    Sorgu tamamladığında, **SORGU** sekmesinde işlemin sonuçları görüntülenir. **hivesampletable** adlı bir tablo görürsünüz. Bu örnek Hive tablosu tüm HDInsight kümeleri ile birlikte gelir.
   
    ![HDInsight Hive görünümleri](./media/apache-hadoop-linux-tutorial-get-started/hiveview.png "HDInsight Hive Görünümü Sorgu Düzenleyicisi")

6. Aşağıdaki sorguyu çalıştırmak için 4. ve 5. adımı yineleyin:
   
        SELECT * FROM hivesampletable;
   
7. Ayrıca sorgunun sonuçlarını da kaydedebilirsiniz. Sağdaki menü düğmesini seçtikten sonra, sonuçları CSV dosyası olarak indirme veya kümeyle ilişkili depolama hesabında depolama seçeneklerinden birini belirleyin.

    ![Hive sorgusunun sonucunu kaydetme](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-linux-hive-view-save-results.png "Hive sorgusunun sonucunu kaydetme")

Hive işini tamamladıktan sonra, [sonuçları Azure SQL Veritabanı’na veya SQL Server veritabanına aktarabilirsiniz](apache-hadoop-use-sqoop-mac-linux.md), ayrıca [Excel kullanarak sonuçları görselleştirebilirsiniz](apache-hadoop-connect-excel-power-query.md). HDInsight Hive kullanma hakkında daha fazla bilgi için bkz. [Apache Hive ve HiveQL kullanma HDInsight, Apache Hadoop ile bir örnek Apache log4j dosyasını çözümlemek için](hdinsight-use-hive.md).

## <a name="troubleshoot"></a>Sorun giderme

HDInsight kümeleri oluştururken sorun yaşarsanız bkz. [erişim denetimi gereksinimleri](../hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme
Öğreticiyi tamamladıktan sonra kümeyi silmek isteyebilirsiniz. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır. 

> [!NOTE]  
> HDInsight üzerinde Hadoop kullanarak ETL işlemlerinin nasıl çalıştırılacağını öğrenmek için sonraki öğreticiye *hemen* ilerliyorsanız, kümeyi çalışır durumda tutmak isteyebilirsiniz. Bir Hadoop kümesini yeniden oluşturmak zorunda öğreticide olmasıdır. Ancak sonraki öğreticiye hemen geçmeyecekseniz şimdi kümeyi silmeniz gerekir.
> 
>  

**Küme ve/veya varsayılan depolama hesabını silmek için**

1. Azure portalın bulunduğu tarayıcı sekmesine dönün. Kümeye genel bakış sayfasında olmalısınız. Yalnızca kümeyi silmek, ancak varsayılan depolama hesabını korumak istiyorsanız **Sil**’i seçin.

    ![HDInsight küme silme](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-delete-cluster.png "HDInsight kümesini silme")

2. Kümeyi ve varsayılan depolama hesabını silmek istiyorsanız, kaynak grubu sayfasını açmak için kaynak grubu adını (önceki ekran görüntüsünde vurgulanan) seçin.

3. **Kaynak grubunu sil**’i seçerek, kümeyi ve varsayılan depolama hesabını içeren kaynak grubunu silin. Kaynak grubu silindiğinde depolama hesabının da silindiğini unutmayın. Depolama hesabını tutmak istiyorsanız, yalnızca küme silmeyi seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Kaynak Yöneticisi şablonu kullanarak Linux tabanlı bir HDInsight kümesi oluşturmayı ve temel Hive sorguları gerçekleştirmeyi öğrendiniz. Sonraki makalede, HDInsight üzerinde Hadoop kullanarak ayıklama, dönüştürme ve yükleme (ETL) işlemi gerçekleştirmeyi öğreneceksiniz.

> [!div class="nextstepaction"]
>[Ayıklama, dönüştürme ve HDInsight üzerinde Apache Hive kullanarak verileri yükleme](../hdinsight-analyze-flight-delay-data-linux.md)
