<properties
    pageTitle="Apache Storm öğreticisi: HDInsight’ta Linux tabanlı Storm kullanmaya başlama | Microsoft Azure"
    description="Linux tabanlı HDInsight’ta Apache Storm ve Storm Starter örneklerini kullanarak büyük veri analizini kullanmaya başlayın. Verileri gerçek zamanlı olarak işlemek için Storm’u nasıl kullanacağınızı öğrenin."
    keywords="apache storm,apache storm öğreticisi,büyük veri analizi,storm başlatıcısı"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="paulettm"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/16/2016"
   ms.author="larryfr"/>


# Apache Storm öğreticisi: HDInsight’ta büyük veri analizi için Storm Starter örneklerini kullanmaya başlama

Apache Storm, veri akışlarını işlemeye yönelik ölçeklenebilir, hataya dayanıklı, dağıtılmış ve gerçek zamanlı bir işlem sistemidir. Azure HDInsight’ta Storm ile büyük veri analizini gerçek zamanlı olarak gerçekleştiren bulut tabanlı bir Storm kümesi oluşturabilirsiniz.

> [AZURE.NOTE] Bu makaledeki adımlar Linux tabanlı bir HDInsight kümesi oluşturur. HDInsight kümesinde Windows tabanlı bir Storm oluşturma adımları için bkz. [Apache Storm öğreticisi: HDInsight’ta veri analizi kullanarak Storm Starter örneği kullanmaya başlama](hdinsight-apache-storm-tutorial-get-started.md)

## Başlamadan önce

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Bu Apache Storm öğreticisini başarıyla tamamlamak için şunlara sahip olmanız gerekir:

- **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **SSH ve SCP hakkında bilgi**. HDInsight ile SSH ve SCP kullanma hakkında daha fazla bilgi için aşağıdakilere bakın:

    - **Linux, Unix veya OS X istemcileri**: Bkz. [Linux, OS X veya Unix işletim sistemlerinden HDInsight’ta Linux tabanlı Hadoop ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md)

    - **Windows istemcileri**: [Windows’dan HDInsight’ta Linux tabanlı Hadoop ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-windows.md)

## Storm kümesi oluşturma

Bu bölümde Azure Resource Manager şablonunu kullanarak bir HDInsight sürüm 3.2 kümesi (Storm 0.9.3 sürümü) oluşturursunuz. HDInsight sürümleri ve SLA’ları hakkında bilgi için bkz. [HDInsight bileşen sürümü oluşturma](hdinsight-component-versioning.md). Diğer küme oluşturma yöntemleri için bkz. [HDInsight kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md).

1. Azure Portal'da bir şablonu açmak için aşağıdaki görüntüye tıklayın.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-storm-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    Şablon bir ortak blob kapsayıcısında, *https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-storm-cluster-in-hdinsight.json* adresinde bulunur. 
   
2. Parametreler dikey penceresinde aşağıdakileri girin:

    - **ClusterName**: Oluşturacağınız Hadoop kümesi için bir ad girin.
    - **Küme oturum açma adı ve parolası**: Varsayılan oturum açma adı admin şeklindedir.
    - **SSH kullanıcı adı ve parola**.
    
    Lütfen bu değerleri yazın.  Öğreticide daha sonra bunlara ihtiyacınız olacaktır.

    > [AZURE.NOTE] SSH bir komut satırı kullanarak HDInsight’a uzaktan erişmek için kullanılır. Burada kullandığınız kullanıcı adı ve parola, kümeye SSH üzerinden bağlanırken kullanılır. Ayrıca, SSH kullanıcı adı tüm HDInsight küme düğümlerinde bir kullanıcı hesabı oluşturduğundan benzersiz olmalıdır. Kümedeki hizmetlerin kullanımı için ayrılmış olup SSH kullanıcı adı olarak kullanılamayan hesap adlarının bazıları aşağıda verilmiştir:
    >
    > root, hdiuser, storm, hbase, ubuntu, zookeeper, hdfs, yarn, mapred, hbase, hive, oozie, falcon, sqoop, admin, tez, hcat, hdinsight-zookeeper.

    > HDInsight ile SSH kullanma hakkında daha fazla bilgi için aşağıdaki makalelerden birine bakın:

    > * [Linux, Unix ya da OS X’te HDInsight’ta Linux tabanlı Hadoop ile SSH’yi kullanma](hdinsight-hadoop-linux-use-ssh-unix.md)
    > * [Windows’da HDInsight’ta Linux tabanlı Hadoop ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-windows.md)

    
3. Parametreleri kaydetmek için **Tamam**’a tıklayın.

4. **Özel dağıtım** dikey penceresinde **Kaynak grubu** açılır kutusuna ve ardından **Yeni**’ye tıklayarak yeni bir kaynak grubu oluşturun. Kaynak grubu; küme, bağımlı depolama hesabını ve diğer bağlı kaynağı gruplandıran bir kapsayıcıdır.

5. **Yasal koşullar**’a ve ardından **Oluştur**’a tıklayın.

6. **Oluştur**’a tıklayın. Şablon dağıtımı için dağıtım gönderme başlıklı yeni bir kutucuk görürsünüz. Kümenin ve SQL Database’in oluşturulması yaklaşık 20 dakika sürer.


##HDInsight’ta bir Storm Starter örneği çalıştırma

[storm-starter](https://github.com/apache/storm/tree/master/examples/storm-starter) örnekleri HDInsight kümesine dahil edilir. Aşağıdaki adımlarda WordCount örneği çalıştırılacaktır.

1. SSH kullanarak HDInsight kümesine bağlanma:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
        
    SSH kullanıcı hesabınızın güvenliğini sağlamak için parola kullandıysanız parolayı girmeniz istenir. Bir ortak anahtar kullandıysanız eşleşen özel anahtarı belirtmek için `-i` parametresini kullanmanız gerekebilir. Örneğin, `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.
        
    Linux tabanlı HDInsight ile SSH kullanma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:
    
    * [Linux, Unix ya da OS X’te HDInsight’ta Linux tabanlı Hadoop ile SSH’yi kullanma](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Windows’da HDInsight’ta Linux tabanlı Hadoop ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-windows)

2. Örnek bir topoloji başlatmak için aşağıdaki komutu kullanın:

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-0.10.0.2.4.2.4-5.jar storm.starter.WordCountTopology wordcount
        
    > [AZURE.NOTE] HDInsight daha yeni Storm sürümleriyle güncelleştirildikçe dosya adının `0.10.0.2.4.2.4-5` kısmı değişebilir.

    Bu durum kümede 'wordcount' kolay adı ile örnek WordCount topolojisini başlatır. Rastgele cümleler oluşturur ve cümlelerde her bir sözcüğün kaç kez geçtiğini sayar.

    > [AZURE.NOTE] Kümeye topoloji gönderirken `storm` komutunu kullanmadan önce kümeyi içeren jar dosyasını kopyalamanız gerekir. Bu işlem, dosyanın mevcut olduğu istemciden `scp` komutu kullanılarak gerçekleştirilebilir. Örneğin, `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
    >
    > WordCount örneği ve diğer storm starter örnekleri `/usr/hdp/current/storm-client/contrib/storm-starter/` konumunda kümenize zaten dahil edilmiştir.

##Topolojiyi izleme

Storm Kullanıcı Arabirimi çalışan topolojilerle çalışmaya yönelik bir web arabirimi sağlar ve HDInsight kümenize dahil edilir.

Storm Kullanıcı Arabirimini kullanarak topolojiyi izlemek için aşağıdaki adımları kullanın:

1. https://CLUSTERNAME.azurehdinsight.net/stormui adresini bir web tarayıcısında açın; burada __CLUSTERNAME__, kümenizin adıdır. Bu işlem Storm Kullanıcı Arabirimini açar.

    > [AZURE.NOTE] Bir kullanıcı adı ve parola girmeniz istenirse kümeyi oluştururken kullandığınız küme yöneticisi (yönetici) ve parolayı girin.

2. **Topoloji özeti** altında **Ad** sütunundaki **wordcount** girişini seçin. Bunun yapılması topoloji hakkında daha fazla bilgi görüntüler.

    ![Storm Starter WordCount topoloji bilgilerini içeren Storm Panosu.](./media/hdinsight-apache-storm-tutorial-get-started-linux/topology-summary.png)

    Bu sayfa aşağıdaki bilgileri sağlar:

    * **Topoloji istatistikleri** - Topoloji performansı hakkında zaman pencereleri halinde düzenlenmiş temel bilgiler.

        > [AZURE.NOTE] Belirli bir zaman penceresinin seçilmesi sayfanın diğer bölümlerinde gösterilen bilgiler için zaman penceresini değiştirir.

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

        > [AZURE.NOTE] Belirli bir zaman penceresinin seçilmesi sayfanın diğer bölümlerinde gösterilen bilgiler için zaman penceresini değiştirir.

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

##Topolojiyi durdurma

Word-count topolojisi için **Topoloji özeti** sayfasına geri dönün ve ardından **Topoloji eylemleri** bölümünden **Sonlandır** düğmesini seçin. İstendiğinde, topolojiyi durdurmadan önce beklenecek saniye sayısı için 10 girin. Zaman aşımı süresinden sonra panonun **Storm Kullanıcı Arabirimi** bölümünü ziyaret ettiğinizde topoloji bir daha görünmez.

##Küme silme

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

##<a id="next"></a>Sonraki adımlar

Bu Apache Storm öğreticisinde HDInsight kümesinde bir Storm oluşturmak ve Storm topolojilerini dağıtmak, izlemek ve yönetmek üzere Storm Panosunu kullanmak için Storm Starter’ı kullandınız. Ardından, [Maven kullanarak Java tabanlı topolojiler geliştirme](hdinsight-storm-develop-java-topology.md) hakkında bilgi edindiniz.

Java tabanlı topolojiler geliştirme hakkında zaten bilgi sahibiyseniz ve mevcut bir topolojiyi HDInsight’a dağıtmak istiyorsanız bkz. [HDInsight’ta Apache Storm topolojilerini dağıtma ve yönetme](hdinsight-storm-deploy-monitor-topology-linux.md).

[apachestorm]: https://storm.incubator.apache.org
[stormdocs]: http://storm.incubator.apache.org/documentation/Documentation.html
[stormstarter]: https://github.com/apache/storm/tree/master/examples/storm-starter
[stormjavadocs]: https://storm.incubator.apache.org/apidocs/
[azureportal]: https://manage.windowsazure.com/
[hdinsight-provision]: hdinsight-provision-clusters.md
[preview-portal]: https://portal.azure.com/


<!--HONumber=Aug16_HO4-->


