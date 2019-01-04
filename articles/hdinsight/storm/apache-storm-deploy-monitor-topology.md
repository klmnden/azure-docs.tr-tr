---
title: Azure HDInsight, Apache Storm topolojilerini dağıtma ve yönetme
description: Dağıtma, izleme ve Storm panosunu kullanarak Windows tabanlı HDInsight üzerinde Apache Storm topolojilerini yönetme hakkında bilgi edinin. Visual Studio için Hadoop araçlarını kullanın.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 03/01/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 322c7164c0ecda550bf1bfe6a55075759bf95735
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53630525"
---
# <a name="deploy-and-manage-apache-storm-topologies-on-windows-based-hdinsight"></a>Windows tabanlı HDInsight üzerinde Apache Storm topolojilerini dağıtma ve yönetme

[Apache Storm](https://storm.apache.org/) çalışma Apache Storm topolojilerini, HDInsight için web tarayıcınızı kullanarak küme ve Pano kolayca dağıtmanızı sağlar. Pano, çalışan topolojileri izleyip yönetmek ve izlemek için de kullanabilirsiniz. Visual Studio kullanıyorsanız, Visual Studio için HDInsight araçları, Visual Studio'da benzer özellikleri sağlar.

Storm panosunu ve Storm özellikleri HDInsight Araçları'nda kendi izleme oluşturmak için kullanılan Storm REST API ve yönetim çözümleri kullanır.

> [!IMPORTANT]  
> Bu belgede yer alan adımlar, Windows işletim sistemi olarak kullanan HDInsight kümesi üzerinde Storm gerektirir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> Storm topolojileri Linux kullanan bir HDInsight kümesi ile dağıtıp hakkında daha fazla bilgi için bkz: [Dağıt ve Linux tabanlı HDInsight üzerinde Apache Storm topolojilerini yönetme](apache-storm-deploy-monitor-topology-linux.md)

## <a name="prerequisites"></a>Önkoşullar

* **HDInsight üzerinde Apache Storm** -bkz [HDInsight üzerinde Apache Storm ile çalışmaya başlama](apache-storm-tutorial-get-started-linux.md) kümesi oluşturma adımları için.

* İçin **Storm Panosu**: HTML5'i destekleyen modern bir web tarayıcısı.

* İçin **Visual Studio** -Azure SDK'sı 2.5.1 veya daha yeni ve Visual Studio için HDInsight araçları. Bkz: [Visual Studio için HDInsight araçlarını kullanmaya başlama](../hadoop/apache-hadoop-visual-studio-tools-get-started.md) yüklemek ve Visual Studio için HDInsight Araçları'nı yapılandırmak için.

    Visual Studio sürümlerinden biri:

  * Visual Studio 2012 güncelleştirme 4

  * Güncelleştirme 4 ile Visual Studio 2013 veya Visual Studio 2013 Community

  * Visual Studio 2015 (herhangi bir sürümü)

  * Visual Studio 2017 (herhangi bir sürümü)

## <a name="storm-dashboard"></a>Storm Panosu

Storm panosunu kullanarak Storm kümenizde kullanılabilir bir web sayfasıdır. URL **https://&lt;clustername >.azurehdinsight.net/** burada **clustername** HDInsight kümesi üzerinde Storm'a adıdır.

Storm panosunu başından seçin **topoloji Gönder**. Bir örnek topoloji çalıştırmak için veya karşıya yükleme ve oluşturduğunuz bir topoloji çalıştırmak için sayfadaki yönergeleri izleyin.

![Gönderme topolojisi sayfası][storm-dashboard-submit]

### <a name="storm-ui"></a>Storm kullanıcı Arabirimi

Storm Panoda **Storm kullanıcı arabirimini** bağlantı. Bu, kümedeki tüm çalışan topolojileri yanı sıra ilgili bilgileri görüntüler.

![storm kullanıcı arabirimi][storm-dashboard-ui]

> [!NOTE]  
> Internet Explorer bazı sürümleri ile ilk kez ziyaret ettikten sonra Storm kullanıcı arabirimini yenilemez keşfedebilir. Örneğin, bu yeni topolojileri gönderdiğiniz veya önceden etkinliği, bir topoloji etkin olarak gösterebilir gösterilmeyebilir. Microsoft, bu sorunun farkındadır ve bir çözüm üzerinde çalışmaktadır.

#### <a name="main-page"></a>Ana sayfa

Storm kullanıcı arabirimini'nın ana sayfasında aşağıdaki bilgileri sağlar:

* **Küme özeti**: Storm kümesi ile ilgili temel bilgileri.

* **Topoloji özeti**: Topolojileri çalışan bir listesi. Bağlantıları bu bölümde belirli topolojiler hakkında daha fazla bilgi görüntülemek için kullanın.

* **Summary İdarecisi**: Storm gözetmen hakkında bilgi sağlar.

* **Nimbus yapılandırma**: Kümenin yapılandırmasını nimbus.

#### <a name="topology-summary"></a>Topoloji özeti

Bir bağlantıdan seçerek **topoloji özeti** bölüm topoloji hakkında aşağıdaki bilgileri görüntüler:

* **Topoloji özeti**: Topoloji hakkında temel bilgiler.

* **Topoloji eylemleri**: Yönetim eylemleri için topoloji gerçekleştirebilirsiniz.

  * **Etkinleştirme**: Devre dışı bırakılan bir topolojiyi işlemeyi sürdürür.

  * **Devre dışı bırakma**: Çalışan topolojiyi duraklatır.

  * **Yeniden Dengeleme**: Topolojinin paralelliğini ayarlar. Kümedeki düğüm sayısını değiştirdikten sonra çalışan topolojileri yeniden dengelemeniz gerekir. Bu kümedeki düğümlere artan veya azalan sayısını dengelemek üzere paralelliği ayarlamaya imkan tanır.

      Daha fazla bilgi için [Apache Storm topolojisinin paralelliğini anlama](https://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).

  * **KILL**: Belirtilen zaman aşımından sonra Storm topolojisini sonlandırır.

* **Topoloji istatistikleri**: Topoloji hakkındaki istatistiklerdir. Bağlantıları kullanın **penceresi** sayfada diğer girdiler için zaman çerçevesini ayarlamak için sütun.

* **Spout'lar**: Topoloji tarafından kullanılan spout. Bağlantıları bu bölümde belirli spout hakkında daha fazla bilgi görüntülemek için kullanın.

* **Cıvatalar**: Boltlar topolojiye göre kullanılır. Bağlantıları bu bölümde belirli Cıvatalar hakkında daha fazla bilgi görüntülemek için kullanın.

* **Topoloji Yapılandırması**: Seçili topoloji yapılandırması.

#### <a name="spout-and-bolt-summary"></a>Spout ve Bolt özeti

Gelen bir spout seçerek **Spout'lar** veya **Cıvatalar** bölümleri seçili öğe hakkında aşağıdaki bilgileri görüntüler:

* **Bileşen özeti**: Spout veya Cıvata hakkındaki temel bilgileri.

* **Spout/Cıvata**: Spout veya Cıvata hakkındaki istatistiklerdir. Bağlantıları kullanın **penceresi** sayfada diğer girdiler için zaman çerçevesini ayarlamak için sütun.

* **Girdi istatistikleri** (yalnızca Cıvata): Cıvata tarafından kullanılan giriş akışları hakkında bilgi sağlar.

* **Çıktı istatistikleri**: Bu spout veya Cıvata tarafından yayılan akışları hakkında bilgi sağlar.

* **Yürütücü**: Spout veya Cıvata örnekleri hakkında bilgi. Seçin **bağlantı noktası** girişi tanılama bilgileri günlüğünü görüntülemek belirli bir yürütücü için bu örneği için üretilen.

* **Hataları**: Bu spout veya Cıvata hata bilgileri.

## <a name="hdinsight-tools-for-visual-studio"></a>Visual Studio için HDInsight Araçları

[HDInsight Araçları](https://azure.microsoft.com/resources/videos/hdinsight-tools-for-visual-studio/) göndermek için kullanılan C# veya karma topolojiler, Storm kümesi için. Örnek uygulama aşağıdaki adımları kullanın. HDInsight Araçları'nı kullanarak kendi topolojileri oluşturma hakkında daha fazla bilgi için bkz: [Visual Studio için HDInsight Araçları'nı kullanarak C# topolojileri geliştirme](apache-storm-develop-csharp-visual-studio-topology.md).

HDInsight kümesi üzerinde storm'a bir örneği dağıtma sonra görüntülemek ve topolojisini yönetmek için aşağıdaki adımları kullanın.

1. Visual Studio için HDInsight Araçları'nı en son sürümünü yüklemediyseniz, bkz. [Visual Studio için HDInsight araçlarını kullanmaya başlama](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).

2. Visual Studio'yu açın, **dosya** > **yeni** > **proje**.

3. İçinde **yeni proje** iletişim kutusunda **yüklü** > **şablonları**ve ardından **HDInsight**. Şablonlar listesinden **Storm örnek**. İletişim kutusunun en altında uygulama için bir ad yazın.

    ![image](./media/apache-storm-deploy-monitor-topology/sample.png)

4. İçinde **Çözüm Gezgini**projeye sağ tıklayıp seçin **HDInsight üzerindeki storm'a Gönder**.

   > [!NOTE]  
   > İstenirse, Azure aboneliğiniz için oturum açma kimlik bilgilerini girin. Birden fazla aboneliğiniz varsa, HDInsight kümesi üzerinde Storm'a içeren oturum açın.

5. HDInsight kümesi üzerinde Storm'a seçin **Storm kümesi** aşağı açılan liste ve ardından **Gönder**. Gönderim kullanarak başarılı olup olmadığını izleyebilirsiniz **çıkış** penceresi.

6. Topoloji başarıyla gönderildi, **Storm topolojilerini** küme görünür. Topoloji, çalışan topolojiyi hakkındaki bilgileri görüntülemek için listeden seçin.

    ![Visual studio İzleyicisi](./media/apache-storm-deploy-monitor-topology/vsmonitor.png)

   > [!NOTE]  
   > Ayrıca görüntüleyebilirsiniz **Storm topolojilerini** gelen **Sunucu Gezgini** genişleterek **Azure** > **HDInsight**ve ardından HDInsight kümesinde Storm sağ tıklayıp seçerek **Storm topolojilerini görüntüle**.

    Spout veya bu bileşenler hakkındaki bilgileri görüntülemek için Cıvatalar şekli seçin. Her seçili öğe için yeni bir pencere açılır.

   > [!NOTE]  
   > Topoloji adını topolojinin sınıf adıdır (Bu durumda, `HelloWord`,) ile eklenmiş bir zaman damgası.

7. Gelen **topoloji özeti** görüntülenecek **KILL** topoloji durdurmak için.

   > [!NOTE]  
   > Storm topolojilerini, durdurulmuş olsa veya küme silinir kadar çalıştırmaya devam edin.


## <a name="rest-api"></a>REST API

Storm kullanıcı arabirimini, benzer yönetim ve işlevsellik REST API kullanarak izleme gerçekleştirebilmesi için REST API temelinde oluşturulmuştur. REST API, yönetmeye ve izlemeye Storm Topolojileri için özel araçlar oluşturmak için kullanabilirsiniz.

Daha fazla bilgi için [Apache Storm kullanıcı Arabirimi REST API](https://github.com/apache/storm/blob/0.9.3-branch/STORM-UI-REST-API.md). Aşağıdaki bilgileri, HDInsight üzerinde Apache Storm ile REST API kullanarak özeldir.

### <a name="base-uri"></a>Taban URI

HDInsight kümelerinde REST API için taban URI **https://&lt;clustername >.azurehdinsight.net/stormui/api/v1/** burada **clustername** HDInsight üzerindeki Storm'a adıdır Küme.

### <a name="authentication"></a>Kimlik Doğrulaması

REST API istekleri kullanmalıdır **temel kimlik doğrulaması**, HDInsight küme yöneticisinin adı ve parola kullanın.

> [!NOTE]  
> Temel kimlik doğrulaması düz metin kullanarak gönderildiğinden, aşağıdakileri yapmalısınız **her zaman** küme ile güvenli iletişim için HTTPS kullanın.

### <a name="return-values"></a>Döndürülen değerler

REST API öğesinden geri döndürülen bilgiler, yalnızca küme veya küme aynı Azure sanal ağ üzerindeki sanal makineler kullanılabilir olabilir. İçin döndürülen gibi tam etki alanı adı (FQDN) [Apache ZooKeeper](https://zookeeper.apache.org/) sunucuları olan değil Internet'ten erişilebilir.

## <a name="next-steps"></a>Sonraki Adımlar

Size nasıl dağıtmak ve Storm panosunu kullanarak topolojileri izleme, öğrenme öğrendiğinize göre nasıl yapılır:

* [Visual Studio için HDInsight Araçları'nı kullanarak C# topolojileri geliştirme](apache-storm-develop-csharp-visual-studio-topology.md)

* [Apache Maven kullanarak Java tabanlı topolojiler geliştirme](apache-storm-develop-java-topology.md)

Daha fazla örnek topolojileri listesi için bkz. [HDInsight üzerinde Apache Storm için örnek topolojiler](apache-storm-example-topology.md).

[hdinsight-dashboard]: ./media/apache-storm-deploy-monitor-topology/dashboard-link.png
[storm-dashboard-submit]: ./media/apache-storm-deploy-monitor-topology/submit.png
[storm-dashboard-ui]: ./media/apache-storm-deploy-monitor-topology/storm-ui-summary.png
