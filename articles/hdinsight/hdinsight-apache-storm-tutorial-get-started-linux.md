---
title: "Apache Storm öğreticisi: HDInsight’ta Linux tabanlı Storm kullanmaya başlama | Microsoft Belgeleri"
description: "Linux tabanlı HDInsight’ta Apache Storm ve Storm Starter örneklerini kullanarak büyük veri analizini kullanmaya başlayın. Verileri gerçek zamanlı olarak işlemek için Storm’u nasıl kullanacağınızı öğrenin."
keywords: "apache storm,apache storm öğreticisi,büyük veri analizi,storm başlatıcısı"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: d710dcac-35d1-4c27-a8d6-acaf8146b485
ms.service: hdinsight
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/18/2016
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: b9fda8b5f4ffa6679cc8ca9696a4c51084c80645
ms.openlocfilehash: 7c3d73ca6f4f567247ec9796199e68f764a52808


---
# <a name="apache-storm-tutorial-get-started-with-the-storm-starter-samples-for-big-data-analytics-on-hdinsight"></a>Apache Storm öğreticisi: HDInsight’ta büyük veri analizi için Storm Starter örneklerini kullanmaya başlama

Apache Storm, veri akışlarını işlemeye yönelik ölçeklenebilir, hataya dayanıklı, dağıtılmış ve gerçek zamanlı bir işlem sistemidir. Azure HDInsight’ta Storm ile büyük veri analizini gerçek zamanlı olarak gerçekleştiren bulut tabanlı bir Storm kümesi oluşturabilirsiniz.

> [!NOTE]
> Bu makaledeki adımlar Linux tabanlı bir HDInsight kümesi oluşturur. HDInsight kümesinde Windows tabanlı bir Storm oluşturma adımları için bkz. [Apache Storm öğreticisi: HDInsight’ta veri analizi kullanarak Storm Starter örneği kullanmaya başlama](hdinsight-apache-storm-tutorial-get-started.md)

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Bu Apache Storm öğreticisini başarıyla tamamlamak için şunlara sahip olmanız gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* **SSH ve SCP hakkında bilgi**. HDInsight ile SSH ve SCP kullanma hakkında daha fazla bilgi için aşağıdakilere bakın:
  
    * **Linux, Unix veya OS X istemcileri**: Bkz. [HDInsight’ta Linux tabanlı Hadoop ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md)
    
    * **Windows istemcileri**: [Windows’dan HDInsight’ta Linux tabanlı Hadoop ile SSH (PuTTY) kullanma](hdinsight-hadoop-linux-use-ssh-windows.md)

### <a name="access-control-requirements"></a>Erişim denetimi gereksinimleri

[!INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-a-storm-cluster"></a>Storm kümesi oluşturma

Bu bölümde, Azure Resource Manager şablonu kullanarak bir HDInsight sürüm 3.5 kümesi (Storm 1.0.1 sürümü) oluşturacaksınız. HDInsight sürümleri ve SLA’ları hakkında bilgi için bkz. [HDInsight bileşen sürümü oluşturma](hdinsight-component-versioning.md). Diğer küme oluşturma yöntemleri için bkz. [HDInsight kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md).

1. Azure Portal'da bir şablonu açmak için aşağıdaki görüntüye tıklayın.         
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-storm-cluster-in-hdinsight-35.json" target="_blank"><img src="./media/hdinsight-apache-storm-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>
   
    Şablon bir ortak blob kapsayıcısında, *https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-storm-cluster-in-hdinsight.json* adresinde bulunur. 

2. __Özel dağıtım__ dikey penceresine şu değerleri girin:
   
    * __Kaynak grubu__: Kümenin oluşturulduğu kaynak grubu.

    * **Küme Adı**: Hadoop kümesinin adı.

    * __Küme oturum açma adı__ ve __parola__: Varsayılan oturum açma adı admin’dir.
    
    * __SSH kullanıcı adı__ ve __parola__: SSH kullanılarak kümeye bağlanmak için gerekli kullanıcı adı ve parola.

    * __Konum__: Kümenin coğrafi konumu.
     
     Lütfen bu değerleri yazın.  Öğreticide daha sonra bunlara ihtiyacınız olacaktır.
     
     > [!NOTE]
     > SSH bir komut satırı kullanarak HDInsight’a uzaktan erişmek için kullanılır. Burada kullandığınız kullanıcı adı ve parola, kümeye SSH üzerinden bağlanırken kullanılır. Ayrıca, SSH kullanıcı adı tüm HDInsight küme düğümlerinde bir kullanıcı hesabı oluşturduğundan benzersiz olmalıdır. Kümedeki hizmetlerin kullanımı için ayrılmış olup SSH kullanıcı adı olarak kullanılamayan hesap adlarının bazıları aşağıda verilmiştir:
     > 
     > root, hdiuser, storm, hbase, ubuntu, zookeeper, hdfs, yarn, mapred, hbase, hive, oozie, falcon, sqoop, admin, tez, hcat, hdinsight-zookeeper.
     > 
     > HDInsight ile SSH kullanma hakkında daha fazla bilgi için aşağıdaki makalelerden birine bakın:
     > 
     > * [HDInsight’ta Linux tabanlı Hadoop ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md)
     > * [Windows’dan HDInsight’ta Linux tabanlı Hadoop ile SSH (PuTTY) kullanma](hdinsight-hadoop-linux-use-ssh-windows.md)

3. __Yukarıdaki hüküm ve koşulları kabul ediyorum__’u seçin, **Tamam**’a tıklayın ve ardından __Panoya sabitle__’yi seçin.

6. **Satın al**’a tıklayın. Şablon dağıtımı için Dağıtım gönderme başlıklı yeni bir kutucuk görürsünüz. Kümeyi oluşturmak yaklaşık 20 dakika sürer.

## <a name="run-a-storm-starter-sample-on-hdinsight"></a>HDInsight’ta bir Storm Starter örneği çalıştırma

[storm-starter](https://github.com/apache/storm/tree/master/examples/storm-starter) örnekleri HDInsight kümesine dahil edilir. Aşağıdaki adımlarda WordCount örneği çalıştırılacaktır.

1. SSH kullanarak HDInsight kümesine bağlanma:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    SSH kullanıcı hesabınızın güvenliğini sağlamak için parola kullandıysanız parolayı girmeniz istenir. Bir ortak anahtar kullandıysanız eşleşen özel anahtarı belirtmek için `-i` parametresini kullanmanız gerekebilir. Örneğin, `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.
   
    Linux tabanlı HDInsight ile SSH kullanma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:
   
    * [HDInsight’ta Linux tabanlı Hadoop ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Windows’dan HDInsight’ta Linux tabanlı Hadoop ile SSH (PuTTY) kullanma](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Örnek bir topoloji başlatmak için aşağıdaki komutu kullanın:
   
        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar storm jar org.apache.storm.starter.WordCountTopology wordcount
   
    > [!NOTE]
    > HDInsight’ın önceki sürümlerinde, topolojinin sınıf adı `storm.starter.WordCountTopology` yerine `org.apache.storm.starter.WordCountTopology` şeklindedir.
   
    Bu durum kümede 'wordcount' kolay adı ile örnek WordCount topolojisini başlatır. Rastgele cümleler oluşturur ve cümlelerde her bir sözcüğün kaç kez geçtiğini sayar.
   
    > [!NOTE]
    > Kümeye kendi topolojilerinizi gönderirken `storm` komutunu kullanmadan önce kümeyi içeren jar dosyasını kopyalamanız gerekir. Bu işlem, dosyanın mevcut olduğu istemciden `scp` komutu kullanılarak gerçekleştirilebilir. Örneğin, `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
    > 
    > WordCount örneği ve diğer storm starter örnekleri `/usr/hdp/current/storm-client/contrib/storm-starter/` konumunda kümenize zaten dahil edilmiştir.

Storm başlangıç örneklerinin kaynağını görüntülemek istiyorsanız kaynak kodu [https://github.com/apache/storm/tree/1.0.x-branch/examples/storm-starter](https://github.com/apache/storm/tree/1.0.x-branch/examples/storm-starter) adresinde bulabilirsiniz. Bu bağlantı, HDInsight 3.5 ile birlikte sağlanan Storm 1.0.x içindir. Diğer Storm sürümleri için sayfanın üstündeki __Dal__ düğmesini kullanarak farklı bir Storm sürümü seçin.

## <a name="monitor-the-topology"></a>Topolojiyi izleme

Storm Kullanıcı Arabirimi çalışan topolojilerle çalışmaya yönelik bir web arabirimi sağlar ve HDInsight kümenize dahil edilir.

Storm Kullanıcı Arabirimini kullanarak topolojiyi izlemek için aşağıdaki adımları kullanın:

1. https://CLUSTERNAME.azurehdinsight.net/stormui adresini bir web tarayıcısında açın; burada **CLUSTERNAME**, kümenizin adıdır. Bu işlem Storm Kullanıcı Arabirimini açar.
    
    > [!NOTE]
    > Bir kullanıcı adı ve parola girmeniz istenirse kümeyi oluştururken kullandığınız küme yöneticisi (yönetici) ve parolayı girin.

2. **Topoloji özeti** altında **Ad** sütunundaki **wordcount** girişini seçin. Bunun yapılması topoloji hakkında daha fazla bilgi görüntüler.
    
    ![Storm Starter WordCount topoloji bilgilerini içeren Storm Panosu.](./media/hdinsight-apache-storm-tutorial-get-started-linux/topology-summary.png)
    
    Bu sayfa aşağıdaki bilgileri sağlar:
    
    * **Topoloji istatistikleri** - Topoloji performansı hakkında zaman pencereleri halinde düzenlenmiş temel bilgiler.
     
        > [!NOTE]
        > Belirli bir zaman penceresinin seçilmesi sayfanın diğer bölümlerinde gösterilen bilgiler için zaman penceresini değiştirir.

    * **Spout’lar** - Her bir spout’un döndürdüğü son hata dahil olmak üzere spout’lar hakkında temel bilgi.
    
    * **Cıvatalar** - Cıvatalar hakkında temel bilgiler.
    
    * **Topoloji yapılandırması** - Topoloji yapılandırması hakkında ayrıntılı bilgi.
     
    Bu sayfa ayrıca topoloji üzerinde gerçekleştirilebilen eylemleri sağlar:
   
    * **Etkinleştir** - Devre dışı bırakılan bir topolojiyi işlemeyi sürdürür.
    
    * **Devre dışı bırak** - Çalışan topolojiyi duraklatır.
    
    * **Yeniden dengele** - Topolojinin paralelliğini ayarlar. Kümedeki düğüm sayısını değiştirdikten sonra çalışan topolojileri yeniden dengelemeniz gerekir. Bunun yapılması kümede artan/azalan düğüm sayısını dengelemek üzere paralelliği ayarlamaya imkan tanır. Daha fazla bilgi için bkz. [Bir Storm topolojisinin paralelliğini anlama](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).
    
    * **Sonlandır** - Belirtilen zaman aşımından sonra Storm topolojisini sonlandırır.

3. Bu sayfada **Spout’lar** veya **Cıvatalar** bölümünden bir giriş seçin. Bunun yapılması seçilen bileşenle ilgili bilgileri görüntüler.
   
    ![Seçili bileşenler hakkında bilgi içeren Storm Panosu.](./media/hdinsight-apache-storm-tutorial-get-started-linux/component-summary.png)
   
    Bu sayfa aşağıdaki bilgileri gösterir:
   
    * **Spout/Cıvata istatistikleri** - Bileşen performansı hakkında zaman pencereleri halinde düzenlenmiş temel bilgiler.
     
        > [!NOTE]
        > Belirli bir zaman penceresinin seçilmesi sayfanın diğer bölümlerinde gösterilen bilgiler için zaman penceresini değiştirir.
     
    * **Girdi istatistikleri** (yalnızca cıvata) - Cıvata tarafından kullanılan verileri üreten bileşenler hakkında bilgi.
    
    * **Çıktı istatistikleri** - Bu cıvata tarafından yayılan veriler hakkında bilgi.
    
    * **Yürütücüler** - Bu bileşenin örnekleri hakkında bilgi.
    
    * **Hatalar** - Bu bileşenden kaynaklanan hatalar.

4. Spout veya cıvata ayrıntılarını görüntülerken bileşenin belirli bir örneğine ilişkin ayrıntıları görmek için **Yürütücüler** bölümündeki **Bağlantı Noktası** sütunundan bir giriş seçin.
   
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["with"]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["nature"]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [snow]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [snow, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [white]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [white, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [seven]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [seven, 1493957]
   
    Bu verilerde **yedi** sözcüğünün 1493957 kez ortaya çıktığını görebilirsiniz. Bu sayı, bu topoloji başlatıldığından beri kaç kez karşılaşıldığını gösterir.

## <a name="stop-the-topology"></a>Topolojiyi durdurma

Word-count topolojisi için **Topoloji özeti** sayfasına geri dönün ve ardından **Topoloji eylemleri** bölümünden **Sonlandır** düğmesini seçin. İstendiğinde, topolojiyi durdurmadan önce beklenecek saniye sayısı için 10 girin. Zaman aşımı süresinden sonra panonun **Storm Kullanıcı Arabirimi** bölümünü ziyaret ettiğinizde topoloji bir daha görünmez.

## <a name="delete-the-cluster"></a>Küme silme

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="a-idnextanext-steps"></a><a id="next"></a>Sonraki adımlar

Bu Apache Storm öğreticisinde HDInsight kümesinde bir Storm oluşturmak ve Storm topolojilerini dağıtmak, izlemek ve yönetmek üzere Storm Panosunu kullanmak için Storm Starter’ı kullandınız. Ardından, [Maven kullanarak Java tabanlı topolojiler geliştirme](hdinsight-storm-develop-java-topology.md) hakkında bilgi edindiniz.

Java tabanlı topolojiler geliştirme hakkında zaten bilgi sahibiyseniz ve mevcut bir topolojiyi HDInsight’a dağıtmak istiyorsanız bkz. [HDInsight’ta Apache Storm topolojilerini dağıtma ve yönetme](hdinsight-storm-deploy-monitor-topology-linux.md).

.NET geliştiricisiyseniz Visual Studio'yu kullanarak C# veya karma C#/Java topolojileri oluşturabilirsiniz. Daha fazla bilgi edinmek için bkz. [Visual Studio için Hadoop araçlarını kullanarak HDInsight'ta Apache Storm için C# topolojileri geliştirme](hdinsight-storm-develop-csharp-visual-studio-topology.md).

HDInsight üzerinde Storm ile kullanılabilecek örnek topolojiler için şu örneklere bakın:

* [HDInsight üzerinde Storm için örnek topolojiler](hdinsight-storm-example-topology.md)

[apachestorm]: https://storm.incubator.apache.org
[stormdocs]: http://storm.incubator.apache.org/documentation/Documentation.html
[stormstarter]: https://github.com/apache/storm/tree/master/examples/storm-starter
[stormjavadocs]: https://storm.incubator.apache.org/apidocs/
[azureportal]: https://manage.windowsazure.com/
[hdinsight-provision]: hdinsight-provision-clusters.md
[preview-portal]: https://portal.azure.com/



<!--HONumber=Jan17_HO1-->


