---
title: 'Linux öğreticisi: Hadoop ve Hive kullanmaya başlama | Microsoft Docs'
description: HDInsight’ta Hadoop kullanmaya başlamak için bu Linux öğreticisini izleyin. Linux kümeleri hazırlamayı ve Hive ile veri sorgulamayı öğrenin.
services: hdinsight
documentationcenter: ''
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal

ms.service: hdinsight
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/14/2016
ms.author: jgao

---
# Hadoop öğreticisi: HDInsight’ta Linux tabanlı Hadoop kullanmaya başlama
> [!div class="op_single_selector"]
> * [Linux tabanlı](hdinsight-hadoop-linux-tutorial-get-started.md)
> * [Windows tabanlı](hdinsight-hadoop-tutorial-get-started-windows.md)
> 
> 

HDInsight’ta Linux tabanlı [Hadoop](http://hadoop.apache.org/) kümeleri oluşturmayı ve HDInsight’ta Hive işleri çalıştırmayı öğrenin. [Apache Hive](https://hive.apache.org/) Hadoop ekosistemindeki en popüler bileşendir. HDInsight şu anda 4 farklı küme türü ile gelir: [Hadoop](hdinsight-hadoop-introduction.md), [Spark](hdinsight-apache-spark-overview.md), [HBase](hdinsight-hbase-overview.md) ve [Storm](hdinsight-storm-overview.md).  Her küme türü farklı bir bileşen kümesini destekler. 4 küme türü de Hive’ı destekler. HDInsight’ta desteklenen bileşenlerin listesi için bkz. [HDInsight tarafından sağlanan Hadoop küme sürümlerindeki yenilikler nelerdir?](hdinsight-component-versioning.md)  

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## Ön koşullar
Bu öğreticiye başlamadan önce

* **Azure aboneliği** gereklidir. Bir aylık ücretsiz deneme hesabı oluşturmak için [azure.microsoft.com/free](https://azure.microsoft.com/free) adresine gidin.

### Erişim denetimi gereksinimleri
[!INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## Küme oluşturma
Hadoop işlerinin çoğu toplu işlemdir. Bir küme oluşturur, bazı işleri çalıştırır ve kümeyi silersiniz. Bu bölümde, [Azure Resource Manager şablonu](../resource-group-template-deploy.md) kullanarak HDInsight’ta Linux tabanlı bir Hadoop kümesi oluşturacaksınız. Resource Manager şablonu tamamen özelleştirilebilir. HDInsight gibi Azure kaynaklarını oluşturmayı kolaylaştırır. Bu öğreticiyi kullanmak için Resource Manager şablonuyla deneyim sahibi olmak gerekli değildir. Diğer küme oluşturma yöntemleri ve bu öğreticide kullanılan özellikler hakkında bilgi edinmek için bkz. [HDInsight kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md). Bu öğreticide kullanılan Resource Manager şablonu bir ortak blob kapsayıcısında bulunur: [https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hadoop-cluster-in-hdinsight.json](https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hadoop-cluster-in-hdinsight.json). 

1. Aşağıdaki resme tıklayarak Azure'da oturum açın ve Azure Portal'da Resource Manager şablonunu açın. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hadoop-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. **Parametreler** dikey penceresinde aşağıdakileri girin:
   
    ![HDInsight Linux kullanmaya başlama portalda Resource Manager şablonu](./media/hdinsight-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-arm-template-on-portal.png).
   
   * **ClusterName**: Oluşturacağınız Hadoop kümesi için bir ad girin.
   * **Küme oturum açma adı ve parolası**: Varsayılan oturum açma adı **admin** şeklindedir.
   * **SSH kullanıcı adı ve parolası**: Varsayılan kullanıcı adı **sshuser** şeklindedir.  Bunu yeniden adlandırabilirsiniz. 
     
     Bu öğreticiyi izlemek için diğer parametreler isteğe bağlıdır. Bunları olduğu gibi bırakabilirsiniz. 
     
     Her kümenin bir Azure Blob Storage hesabı bağımlılığı vardır. Bu genellikle varsayılan depolama hesabı olarak ifade edilir. HDInsight kümesi ve kümenin varsayılan depolama hesabının aynı Azure bölgesinde bulunması gerekir. Kümeleri silmek depolama hesabını silmez. Şablonda, varsayılan depolama hesabı adı, "depo" ifadesi eklenmiş küme adı olarak tanımlanır. 
3. Parametreleri kaydetmek için **Tamam**’a tıklayın.
4. **Özel dağıtım** dikey penceresinde, **Yeni kaynak grubu adı**’nı girerek yeni kaynak grubu oluşturun.  Kaynak grubu; küme, bağımlı depolama hesabı ve diğer depolama hesaplarını gruplandıran bir kapsayıcıdır. Kaynak grubu konumu küme konumundan farklı olabilir.
5. **Yasal koşullar**’a ve ardından **Oluştur**’a tıklayın.
6. **Panoya sabitle** onay kutusunun seçili olduğundan emin olun ve ardından **Oluştur**’a tıklayın. **Şablon Dağıtımı’nı dağıtma** başlıklı yeni bir kutucuk görürsünüz. Bir küme oluşturmak yaklaşık 20 dakika sürer. 
7. Küme oluşturulduktan sonra, kutucuğun açıklamalı alt yazısı belirttiğiniz kaynak grubu adıyla değişir. Ve portal otomatik olarak küme ve küme ayarlarını içeren iki dikey pencere açar. 
   
   ![HDInsight Linux kullanmaya başlama küme ayarları](./media/hdinsight-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-cluster-settings.png).
   
   Küme ve varsayılan depolama hesabı olmak üzere iki kaynak listelenir.

## Hive sorguları çalıştırma
[Apache Hive](hdinsight-use-hive.md) HDInsight’ta kullanılan en popüler bileşendir. HDInsight’ta Hive işleri çalıştırmanın birçok yolu vardır. Bu öğreticide, bazı Hive işlerini çalıştırmak için portalda Ambari Hive görünümünü kullanırsınız. Hive işlerini göndermenin diğer yöntemleri için bkz. [HDInsight’ta Hive kullanma](hdinsight-use-hive.md).

1. **https://&lt;ClusterName>.azurehdinsight.net** adresine gidin. Burada &lt;ClusterName>, önceki bölümde Ambari’yi açmak için oluşturduğunuz kümedir.
2. Önceki bölümde belirttiğiniz Hadoop kullanıcı adını ve parolayı girin. Varsayılan kullanıcı adı **admin** şeklindedir.
3. Aşağıdaki ekran görüntüsünde gösterildiği gibi **Hive Görünümü**’nü açın:
   
    ![Ambari görünümleri seçme](./media/hdinsight-hadoop-linux-tutorial-get-started/selecthiveview.png).
4. Sayfadaki **Sorgu Düzenleyicisi** bölümünde, aşağıdaki HiveQL ifadelerini çalışma sayfasına yapıştırın:
   
        SHOW TABLES;
   
   > [!NOTE]
   > Hive noktalı virgül gerektirmektedir.       
   > 
   > 
5. **Yürüt**’e tıklayın. Sorgu Düzenleyicisi’nin altında **Sorgu İşleminin Sonuçları** bölümü görüntülenmeli ve iş hakkındaki bilgileri görüntülemelidir. 
   
    Sorgu tamamladığında, **Sorgu İşleminin Sonuçları** bölümünde işlemin sonuçlarını görüntülenir. **hivesampletable** adlı bir tablo görürsünüz. Bu örnek Hive tablosu tüm HDInsight kümeleri ile birlikte gelir.
   
    ![HDInsight Hive görünümleri](./media/hdinsight-hadoop-linux-tutorial-get-started/hiveview.png).
6. Aşağıdaki sorguyu çalıştırmak için 4. ve 5. adımı yineleyin:
   
        SELECT * FROM hivesampletable;
   
   > [!TIP]
   > **Sorgu İşleminin Sonuçları** bölümünün sol üst kısmındaki **Sonuçları Kaydet** açılır kutusuna dikkat edin; bunu sonuçları indirmek ya da CSV dosyası olarak HDInsight depolamaya kaydetmek için kullanabilirsiniz.
   > 
   > 
7. İşlerin bir listesini almak için **Geçmiş**’e tıklayın.

Hive işini tamamladıktan sonra, [sonuçları Azure SQL Database’e veya SQL Server veritabanına aktarabilirsiniz](hdinsight-use-sqoop-mac-linux.md), ayrıca [Excel kullanarak sonuçları görselleştirebilirsiniz](hdinsight-connect-excel-power-query.md). HDInsight’ta Hive kullanma hakkında daha fazla bilgi için bkz. [Örnek Apache log4j dosyasını çözümlemek amacıyla HDInsight’ta Hadoop ile Hive ve HiveQL kullanma](hdinsight-use-hive.md).

## Öğreticiyi silme
Öğreticiyi tamamladıktan sonra kümeyi silmek isteyebilirsiniz. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır. 

> [!NOTE]
> [Azure Data Factory ](hdinsight-hadoop-create-linux-clusters-adf.md)’yi kullanarak, istek üzerine HDInsight kümeleri oluşturabilir ve kümeleri otomatik olarak silmek için TimeToLive ayarını yapılandırabilirsiniz. 
> 
> 

**Küme ve/veya varsayılan depolama hesabını silmek için**

1. [Azure Portal](https://portal.azure.com)’da oturum açın.
2. Portal panosunda, bir küme oluştururken kullandığınız kaynak grubu adını içeren kutucuğa tıklayın.
3. Kümeyi ve varsayılan depolama hesabını içeren kaynak grubunu silmek için kaynak dikey penceresinden **Sil**’e tıklayın; ya da **Kaynaklar** kutucuğunda küme adına tıklayın ve küme dikey penceresinde **Sil**’e tıklayın. Lütfen kaynak grubunun silinmesinin depolama hesabını sileceğini unutmayın. Depolama hesabını tutmak istiyorsanız, yalnızca küme silmeyi seçin.

## Sonraki adımlar
Bu öğreticide, Resource Manager şablonu kullanarak Linux tabanlı bir HDInsight kümesi oluşturmayı ve temel Hive sorguları gerçekleştirmeyi öğrendiniz.

HDInsight ile verileri çözümleme hakkında daha fazla bilgi için aşağıdakilere bakın:

* Visual Studio'da Hive sorguları gerçekleştirme dahil, HDInsight ile Hive kullanma hakkında daha fazla bilgi için bkz. [HDInsight ile Hive kullanma][hdinsight-use-hive].
* Verileri dönüştürmek için kullanılan bir dil olan Pig hakkında bilgi için bkz. [HDInsight ile Pig kullanma][hdinsight-use-pig].
* Hadoop’ta verileri işleyen programları yazmanın bir yöntemi olan MapReduce hakkında bilgi edinmek için bkz. [HDInsight ile MapReduce kullanma][hdinsight-use-mapreduce].
* HDInsight’taki verileri çözümlemek amacıyla Visual Studio için HDInsight Araçları kullanma hakkında bilgi edinmek için bkz. [HDInsight için Visual Studio Hadoop araçlarını kullanmaya başlama](hdinsight-hadoop-visual-studio-tools-get-started.md).

Kendi verilerinizle çalışmaya başlamaya hazırsanız ve HDInsight’ın verileri nasıl depoladı veya verileri HDInsight’a alma hakkında daha fazla bilgi edinmek istiyorsanız, aşağıdakilere bakın:

* HDInsight Azure Blob Storage’ı nasıl kullandığı hakkında daha fazla bilgi için bkz. [HDInsight ile Azure Blob Storage’ı kullanma](hdinsight-hadoop-use-blob-storage.md).
* HDInsight’a verileri yükleme hakkında daha fazla bilgi için bkz. [Verileri HDInsight’a yükleme][hdinsight-upload-data].

HDInsight kümesi oluşturma ve yönetme hakkında daha fazla bilgi edinmek istiyorsanız, aşağıdakilere bakın:

* Linux tabanlı HDInsight kümenizi yönetme hakkında bilgi edinmek için bkz. [Ambari kullanarak HDInsight kümelerini yönetme](hdinsight-hadoop-manage-ambari.md).
* HDInsight kümesi oluştururken tercih edebileceğiniz seçenekler hakkında daha fazla bilgi için bkz. [Özel seçenekleri kullanarak Linux’ta HDInsight oluşturma](hdinsight-hadoop-provision-linux-clusters.md).
* Linux ve Hadoop hakkında bilgi sahibiyseniz, ancak HDInsight’ta Hadoop’a ilişkin teknik özellikleri öğrenmek istiyorsanız, bkz: [Linux’ta HDInsight ile çalışma](hdinsight-hadoop-linux-information.md). Bu aşağıdakiler gibi bilgiler sağlar:
  
  * Ambari ve WebHCat gibi küme üzerinde barındırılan hizmetlerin URL'leri
  * Yerel dosya sisteminde Hadoop dosyalarının ve örneklerin konumu
  * Varsayılan veri depolama olarak HDFS yerine Azure Storage (WASB) kullanımı

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[powershell-download]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[powershell-install-configure]: powershell-install-configure.md
[powershell-open]: powershell-install-configure.md#Install

[img-hdi-dashboard]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.png
[img-hdi-dashboard-query-select]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.select.png
[img-hdi-dashboard-query-select-result]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.select.result.png
[img-hdi-dashboard-query-select-result-output]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.select.result.output.png
[img-hdi-dashboard-query-browse-output]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.browse.output.png
[image-hdi-clusterstatus]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.ClusterStatus.png
[image-hdi-gettingstarted-powerquery-importdata]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.GettingStarted.PowerQuery.ImportData.png
[image-hdi-gettingstarted-powerquery-importdata2]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.GettingStarted.PowerQuery.ImportData2.png



<!--HONumber=Sep16_HO5-->


