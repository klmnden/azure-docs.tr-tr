---
title: Hue kümelerindeki - Azure HDInsight Linux tabanlı Hadoop ile
description: HDInsight kümelerinde Hue yüklemek ve istekleri için Hue yönlendirmek için tüneli kullanma hakkında bilgi edinin. Hue, depolamaya Gözat ve Hive veya Pig çalıştırmak için kullanın.
keywords: Hue hadoop
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 12/11/2017
ms.author: hrasheed
ms.openlocfilehash: b0354803a117e8e2c2382ae888bde94a502f24c6
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63760622"
---
# <a name="install-and-use-hue-on-hdinsight-hadoop-clusters"></a>Yükleme ve HDInsight Hadoop kümeler üzerinde Hue kullanma

HDInsight kümelerinde Hue yüklemek ve istekleri için Hue yönlendirmek için tüneli kullanma hakkında bilgi edinin.

> [!IMPORTANT]  
> Bu belgedeki adımlar, Linux kullanan bir HDInsight kümesi gerektirir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="what-is-hue"></a>Hue nedir?
Hue Web uygulamalarının bir Apache Hadoop kümesi ile etkileşim kurmak için kullanılan bir kümesidir. Bir Hadoop kümesi (HDInsight kümeleri söz konusu olduğunda, WASB) ile ilişkili depolama göz atmak için Hue kullanma, Hive işleri ve Pig betikleri çalıştırın ve benzeri. Aşağıdaki bileşenler Hue yüklemelerinde bir HDInsight Hadoop kümesi ile kullanılabilir.

* Beeswax Hive Editor
* Apache Pig
* Meta veri deposu Yöneticisi
* Apache Oozie
* (Bu WASB varsayılan kapsayıcıya konuşuyor) FileBrowser
* İş Tarayıcısı

> [!WARNING]  
> HDInsight kümesi ile sağlanan bileşenler tam olarak desteklenir ve Microsoft Support yalıtmak ve bu bileşenler için ilgili sorunları gidermek için yardımcı olur.
>
> Özel bileşenler daha fazla sorun giderme konusunda yardımcı olması için ticari açıdan makul destek alırsınız. Bu sorunu çözümlemek ya da bu teknoloji için derin bir uzmanlık bulunduğu açık kaynak teknolojileri için kullanılabilir kanalları etkileşim kurmak isteyen neden olabilir. Örneğin, gibi kullanılan birçok topluluk siteleri vardır: [HDInsight için MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [ https://stackoverflow.com ](https://stackoverflow.com). Apache projeleri proje siteleri de [ https://apache.org ](https://apache.org), örneğin: [Hadoop](https://hadoop.apache.org/).
>
>

## <a name="install-hue-using-script-actions"></a>Betik eylemlerini kullanarak Hue yükleme

Bir Linux tabanlı HDInsight kümesine Hue yüklemek için betik kullanılabilir https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh. Bu betik, Azure depolama BLOB'ları (WASB) veya varsayılan depolama alanı olarak Azure Data Lake Storage kümelerinde Hue yüklemek için kullanabilirsiniz.

Bu bölümde, Azure portalını kullanarak küme sağlanırken betiği kullanmak hakkında yönergeler sağlar.

> [!NOTE]  
> Azure PowerShell, Azure Klasik CLI, HDInsight .NET SDK veya Azure Resource Manager şablonları betik eylemleri uygulamak için de kullanılabilir. Betik eylemleri zaten kümelerini çalıştırmak için de uygulayabilirsiniz. Daha fazla bilgi için [özelleştirme HDInsight kümeleri ile betik eylemleri](hdinsight-hadoop-customize-cluster-linux.md).
>
>

1. Adımları kullanarak bir kümeyi sağlamaya başlayın [sağlama HDInsight kümelerinde Linux üzerinde](hdinsight-hadoop-provision-linux-clusters.md), ancak sağlama tamamlamayın.

   > [!NOTE]  
   > HDInsight kümelerinde Hue yüklemek için önerilen bir baş düğüm boyutu en az şu A4 (8 çekirdek, 14 GB bellek).
   >
   >
2. Üzerinde **isteğe bağlı yapılandırma** dikey penceresinde **betik eylemleri**ve aşağıda gösterildiği gibi bilgileri sağlayın:

    ![Sağlamak betik eylem parametreleri için Hue](./media/hdinsight-hadoop-hue-linux/hue-script-action.png "sağlamak betik eylem parametrelerini Hue için")

   * **AD**: Betik eylemi için bir kolay ad girin.
   * **BETİK URI'Sİ**: https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
   * **HEAD**: Bu seçeneği işaretleyin.
   * **ÇALIŞAN**: Bunu boş bırakın.
   * **ZOOKEEPER**: Bunu boş bırakın.
   * **PARAMETRELERİ**: Bunu boş bırakın.
3. Sayfanın alt kısmında **betik eylemleri**, kullanın **seçin** yapılandırmayı kaydetmek için düğme. Son olarak, **seçin** düğme alttaki **isteğe bağlı yapılandırma** isteğe bağlı yapılandırma bilgileri kaydetmek için dikey pencere.
4. Açıklandığı gibi küme sağlama devam [sağlama HDInsight kümelerinde Linux üzerinde](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="use-hue-with-hdinsight-clusters"></a>Hue HDInsight kümeleri ile kullanma

SSH tünel çalışmaya başladıktan sonra küme üzerinde Hue erişmek için tek yoludur. SSH üzerinden tünel, Hue çalıştığı doğrudan küme baş düğümüne gitmek trafiğe izin verir. Küme Sağlama tamamlandıktan sonra bir HDInsight Linux kümesine Hue kullanmak için aşağıdaki adımları kullanın.

> [!NOTE]  
> Aşağıdaki yönergeleri izlemek için Firefox tarayıcısının kullanmanızı öneririz.
>
>

1. Bilgileri [kullanım Apache Ambari web kullanıcı Arabirimi, ResourceManager, JobHistory, NameNode, Oozie ve diğer web kullanıcı arabirimlerine erişim için SSH tünel](hdinsight-linux-ambari-ssh-tunnel.md) HDInsight kümesine istemci sisteminizden SSH tüneli oluşturma ve sonra yapılandırmak için SSH tüneli bir proxy olarak kullanmak için Web tarayıcınızı.

2. SSH tüneli oluşturup tarayıcınıza proxy trafiği bu altyapıdan yapılandırılmış sonra ana bilgisayar adı birincil baş düğüme bulmanız gerekir. Bağlantı noktası 22 SSH kullanarak kümeye bağlanarak bunu yapabilirsiniz. Örneğin, `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net` burada **kullanıcıadı** SSH kullanıcı adıdır ve **CLUSTERNAME** kümenizin adıdır.

    Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

3. Bağlantı kurulduktan sonra birincil baş tam etki alanı adını almak için aşağıdaki komutu kullanın:

        hostname -f

    Bu ad aşağıdakine benzer döndürür:

        hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net

    Hue Web sitesi bulunduğu ana bilgisayar adını, birincil baş budur.
4. Http adresindeki Hue portalını açmak için tarayıcıyı kullanın:\//HOSTNAME:8888. Ana bilgisayar adı, önceki adımda elde ettiğiniz adıyla değiştirin.

   > [!NOTE]  
   > İlk kez oturum açtığınızda Hue portalda oturum açmak için bir hesap oluşturmak için istenir. Burada belirttiğiniz kimlik bilgilerini Portalı'na sınırlı olacaktır ve yönetici veya küme sağlama sırasında belirtilen SSH kullanıcısı kimlik bilgilerini ilişkili değildir.
   >
   >

    ![Hue portalını oturumuna](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-login.png "Hue portalını için kimlik bilgilerini belirtin")

### <a name="run-a-hive-query"></a>Hive sorgusu çalıştırma
1. Hue Portalı'ndan tıklayın **sorgu düzenleyicileri**ve ardından **Hive** Hive Düzenleyicisi'ni açın.

    ![Hive kullanma](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-use-hive.png "Hive kullanma")
2. Üzerinde **yardımcı** sekmesindeki **veritabanı**, görmelisiniz **hivesampletable**. Tüm HDInsight Hadoop kümeleri ile birlikte gelen bir örnek tablo budur. Sağ bölmede bir örnek sorgu girin ve çıktıyı görmek **sonuçları** sekmesinde bölmesinde aşağıdaki ekran görüntüsünde gösterildiği gibi.

    ![Hive sorgusu çalıştırarak](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-hive-query.png "çalıştırma Hive sorgusu")

    Ayrıca **grafik** sonucu görsel bir temsilini görmek için sekmesinde.

### <a name="browse-the-cluster-storage"></a>Küme depolamaya Gözat
1. Hue Portalı'ndan tıklayın **dosya tarayıcı** menü çubuğunun sağ üst köşedeki.
2. Varsayılan olarak, dosya tarayıcı açılır **/kullanıcı/kullanicim** dizin. Eğik çizgi yolu kümeyle ilişkili Azure depolama kapsayıcısı köküne Git kullanıcı dizininde hemen önce tıklayın.

    ![Kullanım dosya tarayıcı](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-file-browser.png "kullanımı dosya tarayıcısı")
3. Bir dosya veya klasörün kullanılabilir tüm işlemleri görmek için sağ tıklayın. Kullanım **karşıya** geçerli dizine dosya yüklemek için sağ üst köşedeki düğmesi. Kullanım **yeni** yeni dosyaları veya dizinleri oluşturma düğmesi.

> [!NOTE]  
> Hue dosya tarayıcı yalnızca HDInsight kümesi ile ilişkili varsayılan kapsayıcı içeriğini gösterebilirsiniz. Tüm ek depolama hesapları/kümeyle ilişkili kapsayıcılar dosya tarayıcısı kullanılarak erişilebilen olmayacaktır. Ancak, kümeyle ilişkili ek kapsayıcıları her zaman Hive işleri için erişilebilir olacaktır. Örneğin, bir komut girin `dfs -ls wasb://newcontainer@mystore.blob.core.windows.net` Hive Düzenleyicisi'nde diğer kapsayıcıları içeriğini görebilirsiniz. Bu komutta **newcontainer** değil bir kümeyle ilişkilendirilmiş varsayılan kapsayıcı.
>
>

## <a name="important-considerations"></a>Önemli noktalar
1. Hue yüklemek için kullandığınız betik kümenin yalnızca birincil baş düğüme yükler.

2. Yükleme sırasında birden çok Hadoop hizmeti (HDFS, YARN, MR2, Oozie) yapılandırmasını güncelleştirmek için yeniden başlatılır. Hue yükleme komut dosyası tamamlandıktan sonra diğer Hadoop hizmetlerinin başlatılması biraz zaman alabilir. Başlangıçta bu Hue'nın performansını etkileyebilir. Tüm hizmetleri başlatma sonra Hue tamamen işlevsel olacaktır.
3. Hue Apache Tez işlerinde Hive için geçerli varsayılan anlamıyor. Hive yürütme altyapısı MapReduce kullanmak istiyorsanız, aşağıdaki komutu betiğinizde kullanılacak betiği dosyasını güncelleştirin:

        set hive.execution.engine=mr;

4. Linux kümeleri ile Resource Manager ikincil gerçekleştirdiğinizde, hizmetlerinizi birincil baş düğüme çalıştığı bir senaryo olabilir. Böyle bir senaryo (aşağıda gösterilen) hataları Hue kümede çalışan işlerinin ayrıntılarını görüntülemek için kullanırken neden olabilir. Ancak, iş tamamlandığında iş ayrıntılarını görüntüleyebilirsiniz.

   ![Hue portal hata](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-error.png "Hue portal hata")

   Bilinen bir sorun nedeniyle budur. Etkin Kaynak Yöneticisi birincil baş düğüme de çalışır. böylece Ambari geçici bir çözüm olarak değiştirin.
5. Azure depolama kullanarak HDInsight kümelerini kullanırken Hue anlayan WebHDFS `wasb://`. Bu nedenle, betik eylemi ile kullanılan özel betik için WASB konuşmak için WebHDFS ile uyumlu hizmet olan WebWasb yükler. Bu nedenle, HDFS yerde Hue portalını diyor olsa bile (fareyi hareket ettirdiğinizde gibi **dosya tarayıcı**), WASB yorumlanıp.

## <a name="next-steps"></a>Sonraki adımlar
* [HDInsight kümeleri üzerinde Apache Giraph yükleme](hdinsight-hadoop-giraph-install-linux.md). Küme özelleştirmesi, HDInsight Hadoop kümelerinde Giraph'ı yüklemek için kullanın. Giraph grafik işlemeyi Hadoop kullanarak yapmanıza olanak tanır ve Azure HDInsight ile kullanılabilir.
* [HDInsight kümelerinde R yükleme](hdinsight-hadoop-r-scripts-linux.md). Küme özelleştirmesi, HDInsight Hadoop kümelerinde R yüklemek için kullanın. R, bir açık kaynaklı dil ve istatistiksel bilgi işlem ortamı kapsayıcısıdır. Yüzlerce yerleşik istatistiksel işlevler ve işlevsel ve nesne yönelimli programlama yönlerini bir araya getiren kendi programlama dili sağlar. Ayrıca, kapsamlı grafik yetenekleri sağlar.

[powershell-install-configure]: install-configure-powershell-linux.md
[hdinsight-provision]: hdinsight-provision-clusters-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
