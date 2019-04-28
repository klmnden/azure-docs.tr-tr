---
title: HDInsight üzerinde Apache Storm ile ilgili storm-starter örnekleri - Azure
description: HDInsight üzerinde Apache Storm'u ve storm-starter örneklerini kullanarak nasıl büyük veri analizi yapabileceğinizi ve verileri gerçek zamanlı olarak nasıl işleyebileceğinizi öğrenin.
keywords: Storm-starter, Apache Storm örneği
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/27/2018
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.openlocfilehash: 40757c80878ef5a06d3368d4c20f65ebfa11e47b
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62116396"
---
# <a name="get-started-with-apache-storm-on-hdinsight-using-the-storm-starter-examples"></a>Storm-starter örneklerini kullanarak HDInsight üzerinde Apache Storm ile çalışmaya başlama

Nasıl kullanacağınızı öğrenin [Apache Storm](https://storm.apache.org/) storm-starter örneklerini kullanarak HDInsight içinde.

Apache Storm, veri akışlarını işlemeye yönelik ölçeklenebilir, hataya dayanıklı, dağıtılmış ve gerçek zamanlı bir işlem sistemidir. Azure HDInsight’ta Storm ile büyük veri analizini gerçek zamanlı olarak gerçekleştiren bulut tabanlı bir Storm kümesi oluşturabilirsiniz.

> [!IMPORTANT]  
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* **SSH ve SCP hakkında bilgi**. Bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="create-an-apache-storm-cluster"></a>Apache Storm kümesi oluşturma

HDInsight kümesinde Storm oluşturmak için aşağıdaki adımları kullanın:

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Gidin **+ kaynak Oluştur** > **Analytics** > **HDInsight**.

    ![HDInsight kümesi oluşturma](./media/apache-storm-tutorial-get-started-linux/create-hdinsight.png)

2. **Temel bilgiler** bölümünde aşağıdaki bilgileri girin:

    * **Küme adı**: HDInsight kümesinin adı.
    * **Abonelik**: Kullanılacak aboneliği seçin.
    * **Küme oturum açma kullanıcı adı** ve **küme oturum açma parolası**: HTTPS üzerinden kümeye erişirken oturum açın. Ambari Web kullanıcı arabirimi veya REST API gibi hizmetlere erişmek için bu kimlik bilgilerini kullanın.
    * **Güvenli Kabuk (SSH) kullanıcı adı**: SSH üzerinden kümeye erişirken kullanılan oturum açma bilgileri. Varsayılan olarak parola, küme oturum açma parolası ile aynıdır.
    * **Kaynak grubu**: İçinde kümenin oluşturulduğu kaynak grubu.
    * **Konum**: İçinde kümenin oluşturulacağı Azure bölgesi.

   ![Abonelik seçme](./media/apache-storm-tutorial-get-started-linux/hdinsight-basic-configuration.png)

3. **Küme türü**’nü seçin, ardından **Küme yapılandırması** bölümünde aşağıdaki değerleri ayarlayın:

    * **Küme türü**: Storm

    * **İşletim sistemi**: Linux

    * **Sürüm**: Storm 1.1.0 (HDI 3.6)

   Son olarak, **Seç** düğmesini kullanarak ayarları kaydedin.

    ![Küme türü seçme](./media/apache-storm-tutorial-get-started-linux/set-hdinsight-cluster-type.png)

4. Küme türünü seçtikten sonra __Seç__ düğmesini kullanarak küme türünü ayarlayın. Ardından, __İleri__ düğmesini kullanarak temel yapılandırmayı tamamlayın.

5. **Depolama** bölümünden bir depolama hesabı seçin veya oluşturun. Bu belgedeki adımlar için bu bölümdeki diğer alanları varsayılan değerlerinde bırakın. __İleri__ düğmesini kullanarak depolama yapılandırmasını kaydedin. Data Lake depolama Gen2 kullanma hakkında daha fazla bilgi için bkz. [hızlı başlangıç: HDInsight kümelerinde ayarlama](../../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).

    ![HDInsight depolama hesabı ayarlarını belirleme](./media/apache-storm-tutorial-get-started-linux/set-hdinsight-storage-account.png)

6. **Özet** bölümünden kümenin yapılandırmasını gözden geçirin. Yanlış olan ayarları değiştirmek için __Düzenle__ bağlantılarını kullanın. Son olarak, __Oluştur__ düğmesini kullanarak kümeyi oluşturun.

    ![Küme yapılandırma özeti](./media/apache-storm-tutorial-get-started-linux/hdinsight-configuration-summary.png)

    > [!NOTE]
    > Kümenin oluşturulması 20 dakika sürebilir.

## <a name="run-a-storm-starter-sample-on-hdinsight"></a>HDInsight'ta bir storm-starter örneği çalıştırma

1. SSH kullanarak HDInsight kümesine bağlanma:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    > [!TIP]  
    > SSH istemciniz konak bilgisayarın orijinalliğinin sağlanamadığını belirtebilir. Bu durumda devam etmek için `yes` girin.

    > [!NOTE]  
    > SSH kullanıcı hesabınızın güvenliğini sağlamak için parola kullandıysanız parolayı girmeniz istenir. Bir ortak anahtar kullandıysanız eşleşen özel anahtarı belirtmek için `-i` parametresini kullanmanız gerekebilir. Örneğin, `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.

    Bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Örnek bir topoloji başlatmak için aşağıdaki komutu kullanın:

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology wordcount

    Bu komut, kümede örnek WordCount topolojisini başlatır. Bu topoloji, rastgele tümceler oluşturur ve kelimelerin tekrarlama sayısını belirler. Bu topolojinin kolay adı: `wordcount`.

    > [!NOTE]  
    > Kümeye kendi topolojilerinizi gönderirken `storm` komutunu kullanmadan önce kümeyi içeren jar dosyasını kopyalamanız gerekir. Dosyayı kopyalamak için `scp` komutunu kullanın. Örneğin, `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
    >
    > WordCount örneği ve diğer storm-starter örnekleri `/usr/hdp/current/storm-client/contrib/storm-starter/` konumunda kümenize zaten dahil edilmiştir.

Storm-starter örneklerinin kaynağını görüntülemek istiyorsanız, kodu [https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter](https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter) adresinde bulabilirsiniz. Bu bağlantı, HDInsight 3.6 ile birlikte sağlanan Storm 1.1.x içindir. Diğer Storm sürümleri için sayfanın üstündeki __Dal__ düğmesini kullanarak farklı bir Storm sürümü seçin.

## <a name="monitor-the-topology"></a>Topolojiyi izleme

Storm Kullanıcı Arabirimi çalışan topolojilerle çalışmaya yönelik bir web arabirimi sağlar ve HDInsight kümenize dahil edilir.

Storm Kullanıcı Arabirimini kullanarak topolojiyi izlemek için aşağıdaki adımları kullanın:

1. Storm kullanıcı arabirimini görüntülemek için bir web tarayıcısı açarak `https://CLUSTERNAME.azurehdinsight.net/stormui` adresine gidin. **CLUSTERNAME** değerini kümenizin adıyla değiştirin.

    > [!NOTE]  
    > Bir kullanıcı adı ve parola girmeniz istenirse kümeyi oluştururken kullandığınız küme yöneticisi (yönetici) ve parolayı girin.

2. **Topoloji özeti** altında **Ad** sütunundaki **wordcount** girişini seçin. Topoloji hakkında bilgiler görüntülenir.

    ![Storm-starter WordCount topoloji bilgilerini içeren Storm Panosu.](./media/apache-storm-tutorial-get-started-linux/topology-summary.png)

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

   * **Yeniden dengele** - Topolojinin paralelliğini ayarlar. Kümedeki düğüm sayısını değiştirdikten sonra çalışan topolojileri yeniden dengelemeniz gerekir. Yeniden dengeleme, kümede artan/azalan düğüm sayısını dengelemek üzere paralelliği ayarlamaya imkan tanır. Daha fazla bilgi için [Apache Storm topolojisinin paralelliğini anlama](https://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).

   * **Sonlandır** - Belirtilen zaman aşımından sonra Storm topolojisini sonlandırır.

3. Bu sayfada **Spout’lar** veya **Cıvatalar** bölümünden bir giriş seçin. Seçilen bileşen hakkında bilgiler görüntülenir.

    ![Seçili bileşenler hakkında bilgi içeren Storm Panosu.](./media/apache-storm-tutorial-get-started-linux/component-summary.png)

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

    Bu örnekte **seven** kelimesi 1493957 kez geçmiştir. Bu sayı, bu topoloji başlatıldığından beri kelimeyle kaç kez karşılaşıldığını gösterir.

## <a name="stop-the-topology"></a>Topolojiyi durdurma

Word-count topolojisi için **Topoloji özeti** sayfasına geri dönün ve ardından **Topoloji eylemleri** bölümünden **Sonlandır** düğmesini seçin. İstendiğinde, topolojiyi durdurmadan önce beklenecek saniye sayısı için 10 girin. Zaman aşımı süresinden sonra panonun **Storm Kullanıcı Arabirimi** bölümünü ziyaret ettiğinizde topoloji bir daha görünmez.

## <a name="delete-the-cluster"></a>Küme silme

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

HDInsight kümesi oluştururken sorun yaşarsanız bkz. [erişim denetimi gereksinimleri](../hdinsight-hadoop-create-linux-clusters-portal.md).

## <a id="next"></a>Sonraki adımlar

Bu Apache Storm öğreticisinde HDInsight üzerinde Storm ile çalışma hakkındaki temel bilgileri edindiniz. Ardından, bilgi nasıl [Apache Maven kullanarak geliştirme Java tabanlı topolojiler](apache-storm-develop-java-topology.md).

Java tabanlı topolojiler geliştirme hakkında zaten bilgi sahibiyseniz [HDInsight’ta Apache Storm topolojilerini dağıtma ve yönetme](apache-storm-deploy-monitor-topology-linux.md) belgesine göz atın.

.NET geliştiricisiyseniz Visual Studio'yu kullanarak C# veya karma C#/Java topolojileri oluşturabilirsiniz. Daha fazla bilgi için [geliştirme C# Visual Studio için Apache Hadoop araçlarını kullanarak HDInsight üzerinde Apache Storm topolojilerini](apache-storm-develop-csharp-visual-studio-topology.md).

HDInsight üzerinde Storm ile kullanılabilecek örnek topolojiler için şu örneklere bakın:

* [HDInsight üzerinde Apache Storm için örnek topolojiler](apache-storm-example-topology.md)

[apachestorm]: https://storm.incubator.apache.org
[stormdocs]: https://storm.incubator.apache.org/documentation/Documentation.html
[stormstarter]: https://github.com/apache/storm/tree/master/examples/storm-starter
[stormjavadocs]: https://storm.incubator.apache.org/apidocs/
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[preview-portal]: https://portal.azure.com/
