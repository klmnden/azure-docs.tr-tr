---
title: "Hdınsight Linux tabanlı kümelerde - Azure Hadoop ile ton | Microsoft Docs"
description: "Hdınsight kümelerinde Hue yüklemek ve istekleri için ton yönlendirmek için tünel kullanmayı öğrenin. Depolama göz atın ve Hive veya Pig çalıştırmak için ton kullanın."
keywords: Hue hadoop
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 9e57fcca-e26c-479d-a745-7b80a9290447
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: nitinme
ms.openlocfilehash: bf68e9af7592d5abe5a4c9aba8c5061819cbc97a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="install-and-use-hue-on-hdinsight-hadoop-clusters"></a>Yükleme ve Hue Hdınsight Hadoop kümeleri kullanma

Hdınsight kümelerinde Hue yüklemek ve istekleri için ton yönlendirmek için tünel kullanmayı öğrenin.

> [!IMPORTANT]
> Bu belgede yer alan adımlar Linux kullanan bir Hdınsight kümesi gerektirir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="what-is-hue"></a>Hue nedir?
Hue Hadoop kümeyle etkileşim kurmak için kullanılan Web uygulamaları kümesidir. Bir Hadoop kümesine (Hdınsight kümeleri söz konusu olduğunda, WASB) ile ilişkili depolama göz atmak için ton kullanın, Hive işleri ve Pig betikleri çalıştırmak ve benzeri. Aşağıdaki bileşenleri bir Hdınsight Hadoop kümesi ton yüklemelerinde ile kullanılabilir.

* Beeswax Hive Düzenleyicisi
* Pig
* Meta depo Yöneticisi
* Oozie
* (Hangi WASB varsayılan kapsayıcı ettiği) FileBrowser
* İş tarayıcı

> [!WARNING]
> Hdınsight kümesi ile sağlanan bileşenler tam olarak desteklenir ve Microsoft Support yalıtmak ve bu bileşenleri ilgili sorunları gidermek için yardımcı olur.
>
> Özel bileşenler, daha fazla sorun gidermenize yardımcı olması için ticari koşulların elverdiği oranda makul destek alırsınız. Bu sorunu çözmek veya bu teknoloji derin uzmanlık bulunduğu açık kaynak teknolojileri için kullanılabilir kanalları devreye isteyen neden olabilir. Örneğin, olduğu gibi kullanılabilecek birçok topluluk siteleri vardır: [Hdınsight için MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache projeleri proje siteleri de [http://apache.org](http://apache.org), örneğin: [Hadoop](http://hadoop.apache.org/).
>
>

## <a name="install-hue-using-script-actions"></a>Betik eylemleri kullanılarak Hue yüklemek

Bir Linux tabanlı Hdınsight kümesine Hue yüklemek için komut dosyası https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh kullanılabilir. Azure Storage Blobları (WASB) veya varsayılan depolama alanı olarak Azure Data Lake Store ile kümelerde Hue yüklemek için bu komut dosyasını kullanabilirsiniz.

Bu bölümde Azure Portalı'nı kullanarak küme sağlamada komut dosyası kullanma hakkında yönergeler sağlar.

> [!NOTE]
> Azure PowerShell, Azure CLI, Hdınsight .NET SDK veya Azure Resource Manager şablonları betik eylemleri uygulamak için de kullanılabilir. Zaten çalışan kümeler için betik eylemleri de uygulayabilirsiniz. Daha fazla bilgi için bkz: [özelleştirme Hdınsight betik eylemleri ile kümeleri](hdinsight-hadoop-customize-cluster-linux.md).
>
>

1. ' Ndaki adımları kullanarak bir küme hazırlama Başlat [Hdınsight kümeleri hazırlama Linux'ta](hdinsight-hadoop-provision-linux-clusters.md), ancak sağlama tamamlamayın.

   > [!NOTE]
   > Hdınsight kümelerinde Hue yüklemek için önerilen headnode en az boyutudur A4 (8 çekirdek, 14 GB bellek).
   >
   >
2. Üzerinde **isteğe bağlı yapılandırma** dikey penceresinde, select **betik eylemleri**ve aşağıda gösterildiği gibi bilgileri sağlayın:

    ![Betik eylemi parametreler sağlayın ton için](./media/hdinsight-hadoop-hue-linux/hue-script-action.png "sağlamak betik eylem parametrelerini ton için")

   * **AD**: betik eylemi için kolay bir ad girin.
   * **BETİK URI'si**: https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
   * **HEAD**: Bu seçeneği işaretleyin.
   * **ÇALIŞAN**: Bu alanı boş bırakın.
   * **ZOOKEEPER**: Bu alanı boş bırakın.
   * **PARAMETRELERİ**: Bu alanı boş bırakın.
3. Ekranın alt kısmındaki **betik eylemleri**, kullanın **seçin** yapılandırmayı kaydetmek için düğmesi. Son olarak, **seçin** alt kısmındaki düğmesi **isteğe bağlı yapılandırma** isteğe bağlı yapılandırma bilgilerini kaydetmek için dikey penceresini.
4. Bölümünde açıklandığı gibi küme hazırlama devam [Hdınsight kümeleri hazırlama Linux'ta](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="use-hue-with-hdinsight-clusters"></a>Hdınsight kümeleri ile ton kullanın

SSH tünel çalışmaya başladıktan sonra kümede ton erişmek için tek yoludur. SSH yoluyla tünel ton çalıştığı doğrudan küme headnode gitmek trafiğe izin verir. Küme Sağlama tamamlandıktan sonra bir Hdınsight Linux kümede ton kullanmak için aşağıdaki adımları kullanın.

> [!NOTE]
> Aşağıdaki yönergeleri izleyin için Firefox web tarayıcısı kullanmanızı öneririz.
>
>

1. İçinde bulunan bilgileri kullanın [kullanım SSH Ambari web kullanıcı Arabirimi, ResourceManager, kaynak, iş, Oozie ve diğer web kullanıcı arabirimlerine erişim için tünel](hdinsight-linux-ambari-ssh-tunnel.md) Hdınsight kümesine istemci sisteminizden SSH tüneli oluşturma ve ardından Web yapılandırmak için SSH tüneli bir proxy olarak kullanmak için tarayıcı.

2. SSH tüneli oluşturup tarayıcınıza proxy trafiğinin yapılandırdıktan sonra birincil baş düğüm ana bilgisayar adını bulmanız gerekir. Bu bağlantı noktası 22 SSH kullanarak kümeye bağlanarak yapabilirsiniz. Örneğin, `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net` nerede **kullanıcıadı** SSH kullanıcı adınız ve **CLUSTERNAME** kümenizin adıdır.

    Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

3. Bağlantı kurulduktan sonra birincil headnode tam etki alanı adını almak için aşağıdaki komutu kullanın:

        hostname -f

    Bu ad aşağıdakine benzer döndürür:

        hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net

    Bu, ana birincil headnode Hue Web sitesi bulunduğu adıdır.
4. Http://HOSTNAME:8888 adresindeki Hue portalını açmak için tarayıcı kullanın. Ana bilgisayar adı önceki adımda elde ettiğiniz adla değiştirin.

   > [!NOTE]
   > İlk kez oturum açtığınızda ton portalına oturum açmak için bir hesap oluşturmak için istenir. Burada belirttiğiniz kimlik portalına sınırlı olacaktır ve yönetici veya küme hazırlama sırasında belirtilen SSH kullanıcısı kimlik bilgileriyle ilişkili değil.
   >
   >

    ![Hue portalını oturum açma](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-login.png "Hue portalını için kimlik bilgilerini belirtin")

### <a name="run-a-hive-query"></a>Hive sorgusu çalıştırma
1. Hue Portalı'ndan tıklatın **sorgu düzenleyicileri**ve ardından **Hive** Hive Düzenleyicisi'ni açın.

    ![Hive kullanma](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-use-hive.png "Hive kullanma")
2. Üzerinde **yardımcı** sekmesinde, altında **veritabanı**, görmeniz gerekir **hivesampletable**. Hdınsight üzerinde tüm Hadoop kümeleri ile birlikte gelen bir örnek tablo budur. Sağ bölmede bir örnek sorgu girin ve çıkış bakın **sonuçları** sekmesinde bölmesinde aşağıdaki ekran görüntüsünde gösterildiği gibi.

    ![Hive sorgusu çalıştırmak](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-hive-query.png "çalıştırmak Hive sorgusu")

    Aynı zamanda **grafik** görsel bir sonuç görmek için sekmesini.

### <a name="browse-the-cluster-storage"></a>Küme depolama Gözat
1. Hue Portalı'ndan tıklatın **dosya tarayıcısı** menü çubuğunu sağ üst köşesindeki.
2. Varsayılan olarak, dosya tarayıcısı açılır **/kullanıcı/myuser** dizini. Eğik kök kümesi ile ilişkili Azure depolama kapsayıcısının gitmek için yol kullanıcı dizininde önce sağ tıklayın.

    ![Kullanım dosya tarayıcısı](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-file-browser.png "kullanım dosya tarayıcısı")
3. Bir dosya veya klasörün kullanılabilir tüm işlemleri görmek için sağ tıklatın. Kullanım **karşıya** geçerli dizine dosya yüklemek için sağ alt köşesindeki düğmesini. Kullanım **yeni** yeni dosya ve dizinleri oluşturma düğmesi.

> [!NOTE]
> Hue dosya tarayıcı yalnızca Hdınsight kümesi ile ilişkili varsayılan kapsayıcı içeriğini gösterebilir. Tüm ek depolama hesapları/kümeyle ilişkili kapsayıcıları dosya tarayıcısı kullanılarak erişilebilir olmaz. Ancak, kümeyle ilişkili ek kapsayıcıları her zaman Hive işleri için erişilebilir olacaktır. Örneğin, aşağıdaki komutu girin `dfs -ls wasb://newcontainer@mystore.blob.core.windows.net` Hive Düzenleyicisi'nde diğer kapsayıcıları içeriğini görebilir. Bu komutta **newcontainer** değil bir kümeyle ilişkili varsayılan kapsayıcı.
>
>

## <a name="important-considerations"></a>İle ilgili önemli noktalar
1. Hue yüklemek için kullanılan komut dosyası üzerinde kümenin yalnızca birincil headnode yükler.

2. Yükleme sırasında birden çok Hadoop Hizmetleri (HDFS, YARN, MR2, Oozie) yapılandırmasını güncelleştirmek için yeniden başlatılır. Hue yükleme komut dosyası tamamlandıktan sonra diğer Hadoop hizmetlerin başlatılması biraz zaman alabilir. Başlangıçta bu ton'ın performansını etkileyebilir. Tüm hizmetler başlatma sonra ton tamamen işlevsel olacaktır.
3. Hue Tez işlerinde Hive için geçerli varsayılan olduğu anlamıyor. Hive yürütme altyapısı MapReduce kullanmak istiyorsanız, komut dosyanıza aşağıdaki komutu kullanılacak komut dosyasını güncelleştirin:

        set hive.execution.engine=mr;

4. Linux kümeleriyle Resource Manager ikincil gerçekleştirdiğinizde burada hizmetlerinizi birincil headnode üzerinde çalışan bir senaryo olabilir. Böyle bir senaryo (aşağıda gösterilen) hataları ton kümede çalışan işlerinin ayrıntılarını görüntülemek için kullanırken neden olabilir. Ancak, iş tamamlandığında iş ayrıntılarını görüntüleyebilirsiniz.

   ![Hue portal hata](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-error.png "ton portal hata")

   Bilinen bir sorun nedeniyle budur. Etkin Kaynak Yöneticisi'ni de birincil headnode üzerinde çalışır Ambari geçici bir çözüm olarak değiştirin.
5. Hdınsight kümeleri Azure Storage kullanarak kullanırken ton anlar WebHDFS `wasb://`. Bu nedenle, betik eylemi ile kullanılan özel komut dosyası için WASB Konuşmayı için WebHDFS ile uyumlu hizmeti WebWasb yükler. Bunu, HDFS yerlerde Hue portalını diyor olsa bile (fare taşıdığınızda gibi **dosya tarayıcısı**), WASB yorumlanıp.

## <a name="next-steps"></a>Sonraki adımlar
* [Hdınsight kümelerinde Giraph yükleme](hdinsight-hadoop-giraph-install-linux.md). Küme özelleştirme Giraph Hdınsight Hadoop kümelerine yüklemek için kullanın. Giraph Hadoop kullanarak grafik işleme işlemleri yapmanıza olanak tanır ve Azure Hdınsight ile kullanılabilir.
* [Hdınsight kümelerinde Solr yükleme](hdinsight-hadoop-solr-install-linux.md). Küme özelleştirme Solr Hdınsight Hadoop kümelerine yüklemek için kullanın. Solr, depolanan verileri güçlü arama işlemleri olanak sağlar.
* [Hdınsight kümelerinde R yüklemek](hdinsight-hadoop-r-scripts-linux.md). Hdınsight Hadoop kümeleri üzerinde R yüklemek için küme özelleştirme kullanın. R bir açık kaynaklı dil ve istatistiksel bilgi işlem ortamı ' dir. Yüzlerce yerleşik istatistik işlevleri ve işlev ve nesne odaklı programlama yönlerini birleştirir kendi programlama dili sağlar. Yoğun grafik yetenekleri de sağlar.

[powershell-install-configure]: install-configure-powershell-linux.md
[hdinsight-provision]: hdinsight-provision-clusters-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
