---
title: "Azure HDInsight'ta Hadoop ve Hive ile çalışmaya başlama | Microsoft Docs"
description: "HDInsight kümesi oluşturmayı ve Hive ile veri sorgulamayı öğrenin."
keywords: "hadoop kullanmaya başlama,hadoop linux,hadoop hızlı başlangıç,hive kullanmaya başlama,hive hızlı başlangıç"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 6a12ed4c-9d49-4990-abf5-0a79fdfca459
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/17/2018
ms.author: jgao
ms.openlocfilehash: a96612e09e51db8941a7828f8fe9cb7bbced837d
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2018
---
# <a name="hadoop-tutorial-get-started-using-hadoop-in-hdinsight"></a>Hadoop öğreticisi: HDInsight’ta Hadoop kullanmaya başlama

HDInsight’ta [Hadoop](http://hadoop.apache.org/) kümeleri oluşturmayı ve HDInsight’ta Hive işleri çalıştırmayı öğrenin. [Apache Hive](https://hive.apache.org/) Hadoop ekosistemindeki en popüler bileşendir. Şu anda HDInsight [yedi farklı küme türüyle](apache-hadoop-introduction.md#overview) ile birlikte gelir. Her küme türü farklı bir bileşen kümesini destekler. Tüm küme türleri Hive'ı destekler. HDInsight’ta desteklenen bileşenlerin listesi için bkz. [HDInsight tarafından sağlanan Hadoop küme sürümlerindeki yenilikler nelerdir?](../hdinsight-component-versioning.md)  

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce

* **Azure aboneliği** gereklidir. Bir aylık ücretsiz deneme hesabı oluşturmak için [azure.microsoft.com/free](https://azure.microsoft.com/free) adresine gidin.

## <a name="create-cluster"></a>Küme oluşturma

Hadoop işlerinin çoğu toplu işlemdir. Bir küme oluşturur, bazı işleri çalıştırır ve kümeyi silersiniz. Bu bölümde, [Azure Resource Manager şablonu](../../azure-resource-manager/resource-group-template-deploy.md) kullanarak HDInsight'ta Hadoop kümesi oluşturacaksınız. Bu öğreticiyi kullanmak için Resource Manager şablonuyla deneyim sahibi olmak gerekli değildir. Diğer küme oluşturma yöntemleri ve bu öğreticide kullanılan özellikler hakkında bilgi edinmek için bkz. [HDInsight kümeleri oluşturma](../hdinsight-hadoop-provision-linux-clusters.md).

Bu öğreticide kullanılan Resource Manager şablonuna [GitHub](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-password/)'dan ulaşabilirsiniz. 

1. Aşağıdaki resme tıklayarak Azure'da oturum açın ve Azure portalında Resource Manager şablonunu açın. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-ssh-password%2Fazuredeploy.json" target="_blank"><img src="./media/apache-hadoop-linux-tutorial-get-started/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Aşağıdaki değerleri yazın veya seçin:
   
    ![HDInsight - Linux - Başlarken - portalda Resource Manager şablonu kullanmaya başlama](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-arm-template-on-portal.png "Azure portalını ve kaynak grup yöneticisi şablonunu kullanarak HDInsight'ta Hadoop kümesi dağıtma").
   
    * **Abonelik**: Azure aboneliğinizi seçin.
    * **Kaynak grubu**: Kaynak grubu oluşturun veya mevcut bir kaynak grubunu seçin.  Kaynak grubu, Azure bileşenleri için bir kapsayıcıdır.  Bu durumda, kaynak grubu HDInsight kümesini ve bağımlı Azure Depolama hesabını içermektedir. 
    * **Konum**: Kümenizi oluşturmak istediğiniz bir Azure konumu seçin.  Daha iyi performans için kendinize yakın bir konum seçin. 
    * **Küme Türü**: Bu öğretici için **hadoop**'u seçin.
    * **Küme Adı**: Hadoop kümesi için bir ad girin. HDInsight’taki tüm kümeler aynı DNS ad alanını paylaştığından, bu adın benzersiz olması gerekir. Ad, harf, rakam ve kısa çizgilerin dahil olduğu en fazla 59 karakterden oluşabilir. Adın ilk ve son karakterlerinin kısa çizgi olamayacağını unutmayın.
    * **Küme oturum açma adı ve parolası**: Varsayılan oturum açma adı **admin** şeklindedir.
    * **SSH kullanıcı adı ve parolası**: Varsayılan kullanıcı adı **sshuser** şeklindedir.  Bunu yeniden adlandırabilirsiniz. 
     
    Şablondaki bazı özellikler sabit kodlanmış olabilir.  Bu değerleri şablondan yapılandırabilirsiniz.
    
    Her kümenin bir [Azure Depolama hesabı](../hdinsight-hadoop-use-blob-storage.md) veya [Azure Data Lake hesabı](../hdinsight-hadoop-use-data-lake-store.md) bağımlılığı vardır. Bu genellikle varsayılan depolama hesabı olarak ifade edilir. HDInsight kümesi ve kümenin varsayılan depolama hesabının aynı Azure bölgesinde bulunması gerekir. Kümeleri silmek depolama hesabını silmez. 
        
    Bu özellikler hakkında daha fazla açıklama için bkz. [HDInsight'ta Hadoop kümeleri oluşturma](../hdinsight-hadoop-provision-linux-clusters.md).

3. **Yukarıdaki hüküm ve koşulları kabul ediyorum**’u ve **Panoya sabitle**’yi seçip **Satın al**’a tıklayın. Portal panosunda **Şablon Dağıtımı’nı dağıtma** başlıklı yeni bir kutucuk görürsünüz. Bir küme oluşturmak yaklaşık 20 dakika sürer. 

4. Küme oluşturulduktan sonra, kutucuğun açıklamalı alt yazısı belirttiğiniz kaynak grubu adıyla değişir. Hem kümeyi hem de listelenen kümeye yönelik varsayılan depolama alanını görmek için kutucuğa tıklayın.
   
    ![HDInsight Linux yeni başlayanlara yönelik kaynak grubu](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-resource-group.png "Azure HDInsight küme kaynağı grubu")

4. Kümeyi açmak için küme adına tıklayın.

   ![HDInsight - Linux - başlarken - küme ayarları](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-cluster-settings.png "HDInsight küme özellikleri")


## <a name="run-hive-queries"></a>Hive sorguları çalıştırma

[Apache Hive](hdinsight-use-hive.md) HDInsight’ta kullanılan en popüler bileşendir. HDInsight’ta Hive işleri çalıştırmanın birçok yolu vardır. Bu öğreticide, portalda Ambari Hive görünümünü kullanırsınız. Hive işlerini göndermenin diğer yöntemleri için bkz. [HDInsight’ta Hive kullanma](hdinsight-use-hive.md).

1. Ambari’yi açmak için, bir önceki ekran görüntüsünden **Küme Panosu**’na ve ardından **HDInsight Küme Panosu**’na tıklayın.  Ayrıca, **https://&lt;ClusterName>.azurehdinsight.net** adresine de gidebilirsiniz. Burada &lt;ClusterName> değeri, önceki bölümde oluşturduğunuz kümedir.

2. Kümeyi oluştururken belirlediğiniz Hadoop kullanıcı adını ve parolasını girin. Varsayılan kullanıcı adı **admin** şeklindedir.

3. Aşağıdaki ekran görüntüsünde gösterildiği gibi **Hive Görünümü**’nü açın:
   
    ![Ambari görünümlerini seçme](./media/apache-hadoop-linux-tutorial-get-started/selecthiveview.png "HDInsight Hive Viewer menüsü")

4. Sayfadaki **SORGU** sekmesinde, aşağıdaki HiveQL ifadelerini çalışma sayfasına yapıştırın:
   
        SHOW TABLES;

    ![HDInsight Hive görünümleri](./media/apache-hadoop-linux-tutorial-get-started/hiveview-1.png "HDInsight Hive Görünümü Sorgu Düzenleyicisi")
   
   > [!NOTE]
   > Hive noktalı virgül gerektirmektedir.       
   > 
   > 

5. **Yürüt**'e tıklayın. **SORGU** sekmesinin altında bir **SONUÇLAR** sekmesi görünür. Bu sekmede işle ilgili bilgiler görüntülenir. 
   
    Sorgu tamamladığında, **SORGU** sekmesinde işlemin sonuçları görüntülenir. **hivesampletable** adlı bir tablo görürsünüz. Bu örnek Hive tablosu tüm HDInsight kümeleri ile birlikte gelir.
   
    ![HDInsight Hive görünümleri](./media/apache-hadoop-linux-tutorial-get-started/hiveview.png "HDInsight Hive Görünümü Sorgu Düzenleyicisi")

6. Aşağıdaki sorguyu çalıştırmak için 4. ve 5. adımı yineleyin:
   
        SELECT * FROM hivesampletable;
   
7. Ayrıca sorgunun sonuçlarını da kaydedebilirsiniz. Sağdaki menü düğmesine tıkladıktan sonra, sonuçları CSV dosyası olarak indirme veya kümeyle ilişkili depolama hesabında depolama seçeneklerinden birini belirleyin.

    ![Hive sorgusunun sonucunu kaydetme](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-linux-hive-view-save-results.png "Hive sorgusunun sonucunu kaydetme")



Hive işini tamamladıktan sonra, [sonuçları Azure SQL Database’e veya SQL Server veritabanına aktarabilirsiniz](apache-hadoop-use-sqoop-mac-linux.md), ayrıca [Excel kullanarak sonuçları görselleştirebilirsiniz](apache-hadoop-connect-excel-power-query.md). HDInsight’ta Hive kullanma hakkında daha fazla bilgi için bkz. [Örnek Apache log4j dosyasını çözümlemek amacıyla HDInsight’ta Hadoop ile Hive ve HiveQL kullanma](hdinsight-use-hive.md).

## <a name="clean-up-the-tutorial"></a>Öğreticiyi silme
Öğreticiyi tamamladıktan sonra kümeyi silmek isteyebilirsiniz. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır. 

> [!NOTE]
> [Azure Data Factory ](../hdinsight-hadoop-create-linux-clusters-adf.md)’yi kullanarak, istek üzerine HDInsight kümeleri oluşturabilir ve kümeleri otomatik olarak silmek için TimeToLive ayarını yapılandırabilirsiniz. 
> 
> 

**Küme ve/veya varsayılan depolama hesabını silmek için**

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Portal panosunda, bir küme oluştururken kullandığınız kaynak grubu adını içeren kutucuğa tıklayın.
3. Kümeyi ve varsayılan depolama hesabını içeren kaynak grubunu silmek için **Kaynak grubunu sil**'e veya **Kaynaklar** kutucuğunda küme adına ve ardından **Sil**'e tıklayın. Kaynak grubu silindiğinde depolama hesabının da silindiğini unutmayın. Depolama hesabını tutmak istiyorsanız, yalnızca küme silmeyi seçin.

## <a name="troubleshoot"></a>Sorun giderme

HDInsight kümeleri oluştururken sorun yaşarsanız bkz. [erişim denetimi gereksinimleri](../hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Resource Manager şablonu kullanarak Linux tabanlı bir HDInsight kümesi oluşturmayı ve temel Hive sorguları gerçekleştirmeyi öğrendiniz.

HDInsight ile veri çözümleme hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* Visual Studio'da Hive sorguları gerçekleştirme dahil, HDInsight ile Hive kullanma hakkında daha fazla bilgi için bkz. [HDInsight ile Hive kullanma](hdinsight-use-hive.md).
* Verileri dönüştürmek için kullanılan bir dil olan Pig hakkında bilgi için bkz. [HDInsight ile Pig kullanma](hdinsight-use-pig.md).
* Hadoop’ta verileri işleyen programları yazmanın bir yöntemi olan MapReduce hakkında bilgi edinmek için bkz. [HDInsight ile MapReduce kullanma](hdinsight-use-mapreduce.md).
* HDInsight’taki verileri çözümlemek amacıyla Visual Studio için HDInsight Araçları kullanma hakkında bilgi edinmek için bkz. [HDInsight için Visual Studio Hadoop araçlarını kullanmaya başlama](apache-hadoop-visual-studio-tools-get-started.md).

Kendi verilerinizle çalışmaya başlamaya hazırsanız ve HDInsight’ın verileri nasıl depoladı veya verileri HDInsight’a alma hakkında daha fazla bilgi edinmek istiyorsanız, aşağıdaki makalelere bakın:

* HDInsight’ın Azure Depolama’yı nasıl kullandığı hakkında daha fazla bilgi için bkz. [HDInsight ile Azure Depolama kullanma](../hdinsight-hadoop-use-blob-storage.md).
* HDInsight’a veril yükleme hakkında daha fazla bilgi için bkz. [Verileri HDInsight’a yükleme](../hdinsight-upload-data.md).

HDInsight kümesi oluşturma ve yönetme hakkında daha fazla bilgi edinmek istiyorsanız, aşağıdaki makalelere bakın:

* Linux tabanlı HDInsight kümenizi yönetme hakkında bilgi edinmek için bkz. [Ambari kullanarak HDInsight kümelerini yönetme](../hdinsight-hadoop-manage-ambari.md).
* HDInsight kümesi oluştururken tercih edebileceğiniz seçenekler hakkında daha fazla bilgi için bkz. [Özel seçenekleri kullanarak Linux’ta HDInsight oluşturma](../hdinsight-hadoop-provision-linux-clusters.md).
* Linux ve Hadoop hakkında bilgi sahibiyseniz, ancak HDInsight’ta Hadoop’a ilişkin teknik özellikleri öğrenmek istiyorsanız, bkz: [Linux’ta HDInsight ile çalışma](../hdinsight-hadoop-linux-information.md). Bu makale aşağıdaki gibi bilgiler sağlar:
  
  * Ambari ve WebHCat gibi küme üzerinde barındırılan hizmetlerin URL'leri
  * Yerel dosya sisteminde Hadoop dosyalarının ve örneklerin konumu
  * Varsayılan veri depolama olarak HDFS yerine Azure Storage (WASB) kullanımı

[1]: ../HDInsight/apache-hadoop-visual-studio-tools-get-started.md

[hdinsight-provision]: hdinsight-provision-linux-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md


