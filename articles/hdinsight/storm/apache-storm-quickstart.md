---
title: 'Hızlı Başlangıç: Oluşturma ve Azure HDInsight için Apache Storm topolojisinde izleme'
description: Bu hızlı başlangıçta, oluşturmayı ve bir Azure HDInsight Apache Storm topolojisinde izlemeyi öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: quickstart
ms.date: 06/14/2019
ms.author: hrasheed
ms.custom: mvc
ms.openlocfilehash: 49da6ad20b29193f0430a66658f1bf80625704b3
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67274134"
---
# <a name="quickstart-create-and-monitor-an-apache-storm-topology-in-azure-hdinsight"></a>Hızlı Başlangıç: Oluşturma ve bir Azure HDInsight Apache Storm topolojisinde izleme

Apache Storm, veri akışlarını işlemeye yönelik ölçeklenebilir, hataya dayanıklı, dağıtılmış ve gerçek zamanlı bir işlem sistemidir. Azure HDInsight’ta Storm ile büyük veri analizini gerçek zamanlı olarak gerçekleştiren bulut tabanlı bir Storm kümesi oluşturabilirsiniz.

Bu hızlı başlangıçta, bir örnekten Apache kullandığınız [storm-starter](https://github.com/apache/storm/tree/v2.0.0/examples/storm-starter) oluşturun ve var olan Apache Storm kümesine bir Apache Storm topolojisi izlemek için proje.

## <a name="prerequisites"></a>Önkoşullar

* HDInsight üzerinde Apache Storm kümesi. Bkz: [Apache Hadoop kümeleri oluşturma Azure portalını kullanarak](../hdinsight-hadoop-create-linux-clusters-portal.md) seçip **Storm** için **küme türü**.

* Bir SSH istemcisi. Daha fazla bilgi için [SSH kullanarak HDInsight (Apache Hadoop) bağlanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="create-the-topology"></a>Topoloji oluşturma

1. Storm kümenize bağlanın. Değiştirerek aşağıdaki komutu düzenleyin `CLUSTERNAME` Storm'a adı ile küme ve ardından komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

2. **WordCount** örnek konumunda HDInsight kümenize dahil `/usr/hdp/current/storm-client/contrib/storm-starter/`. Topoloji, rastgele tümceler oluşturur ve kelimelerin tekrarlama sayısını belirler. Başlamak için aşağıdaki komutu kullanın **wordcount** topoloji küme üzerinde:

    ```bash
    storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology wordcount
    ```

## <a name="monitor-the-topology"></a>Topolojiyi izleme

Storm çalışan topolojilerle çalışmaya yönelik bir web arabirimi sağlar ve HDInsight kümenize dahil edilir.

Storm Kullanıcı Arabirimini kullanarak topolojiyi izlemek için aşağıdaki adımları kullanın:

1. Storm kullanıcı arabirimini görüntülemek için bir web tarayıcısı açarak `https://CLUSTERNAME.azurehdinsight.net/stormui` adresine gidin. `CLUSTERNAME` değerini kümenizin adıyla değiştirin.

2. Altında **topoloji özeti**seçin **wordcount** girişi **adı** sütun. Topoloji hakkında bilgiler görüntülenir.

    ![Storm-starter WordCount topoloji bilgilerini içeren Storm Panosu.](./media/apache-storm-quickstart/topology-summary.png)

    Yeni sayfa aşağıdaki bilgileri sağlar:

    |Özellik | Açıklama |
    |---|---|
    |Topoloji istatistikleri|Topoloji performansı hakkında zaman pencereleri halinde düzenlenmiş temel bilgiler. Belirli bir zaman penceresinin seçilmesi sayfanın diğer bölümlerinde gösterilen bilgiler için zaman penceresini değiştirir.|
    |Spout|Her bir spout'un döndürdüğü son hata dahil olmak üzere spout'lar hakkında temel bilgiler.|
    |Cıvatalar|Cıvatalar hakkında temel bilgiler.|
    |Topoloji yapılandırması|Topoloji yapılandırması hakkında ayrıntılı bilgiler.|
    |Etkinleştir|Devre dışı bırakılan bir topolojiyi işlemeyi sürdürür.|
    |Devre dışı bırak|Çalışan topolojiyi duraklatır.|
    |Yeniden Dengeleme|Topolojinin paralelliğini ayarlar. Kümedeki düğüm sayısını değiştirdikten sonra çalışan topolojileri yeniden dengelemeniz gerekir. Yeniden dengeleme, kümede artan/azalan düğüm sayısını dengelemek üzere paralelliği ayarlamaya imkan tanır. Daha fazla bilgi için [Apache Storm topolojisinin paralelliğini anlama](https://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).|
    |sonlandırma|Belirtilen zaman aşımından sonra Storm topolojisini sonlandırır.|

3. Bu sayfada **Spout’lar** veya **Cıvatalar** bölümünden bir giriş seçin. Seçilen bileşen hakkında bilgiler görüntülenir.

    ![Seçili bileşenler hakkında bilgi içeren Storm Panosu.](./media/apache-storm-quickstart/component-summary.png)

    Yeni sayfa aşağıdaki bilgileri görüntüler:

    |Özellik | Açıklama |
    |---|---|
    |Spout/Cıvata|Bileşen performansı hakkında zaman pencereleri halinde düzenlenmiş temel bilgiler. Belirli bir zaman penceresinin seçilmesi sayfanın diğer bölümlerinde gösterilen bilgiler için zaman penceresini değiştirir.|
    |Giriş İstatistikleri (yalnızca Cıvata)|Cıvata tarafından kullanılan verileri üreten bileşenler hakkında bilgi.|
    |Çıkış istatistikleri|Bu Cıvata tarafından yayılan veriler hakkında bilgiler.|
    |Yürütücü|Bu bileşenin örnekleri hakkında bilgiler.|
    |Hatalar|Bu bileşen tarafından üretilen hataları.|

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

## <a name="clean-up-resources"></a>Kaynakları temizleme

Hızlı başlangıcı tamamladıktan sonra kümeyi silmek isteyebilirsiniz. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır.

Bir kümeyi silmek için bkz: [tarayıcınız, PowerShell veya Azure CLI kullanarak bir HDInsight kümesi silme](../hdinsight-delete-cluster.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir örnekten Apache kullanılan [storm-starter](https://github.com/apache/storm/tree/v2.0.0/examples/storm-starter) oluşturun ve var olan Apache Storm kümesine bir Apache Storm topolojisi izlemek için proje. Yönetme ve Apache Storm topolojilerini izleme temel bilgileri öğrenmek için sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
>[Azure HDInsight üzerinde Apache Storm topolojilerini dağıtma ve yönetme ](./apache-storm-deploy-monitor-topology-linux.md)