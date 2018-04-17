---
title: Dağıtma ve Hdınsight üzerinde Apache Storm topolojilerini yönetme | Microsoft Docs
description: Dağıtma, izleme ve Hdınsight üzerinde Storm panosunu kullanarak Apache Storm topolojilerini yönetme öğrenin. Visual Studio Hadoop araçlarını kullanın.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 5e542072-f014-42aa-82d6-2694a76df520
ms.service: hdinsight
ms.devlang: java
ms.topic: conceptual
ms.date: 03/01/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 55c92e6408522b8a96a37dbedd99d929af1e49fb
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="deploy-and-manage-apache-storm-topologies-on-windows-based-hdinsight"></a>Dağıtma ve Windows tabanlı Hdınsight üzerinde Apache Storm topolojilerini yönetme

Storm panosunu kolayca dağıtmak ve web tarayıcınızı kullanarak Hdınsight kümenize Apache Storm topolojileri çalıştırmak sağlar. Pano, çalışan topolojileri izleyip yönetmek ve izlemek için de kullanabilirsiniz. Visual Studio kullanırsanız, Visual Studio için Hdınsight araçları Visual Studio benzer özellikleri sağlar.

Storm panosunu ve Hdınsight araçları Storm özelliklerinde kendi izleme oluşturmak için kullanılan Storm REST API ve yönetim çözümleri kullanır.

> [!IMPORTANT]
> Bu belgede yer alan adımlar işletim sistemi olarak Windows kullanan Hdınsight kümesi üzerinde Storm gerektirir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> Dağıtma ve Storm topolojileri Linux kullanan bir Hdınsight kümesi ile yönetme hakkında daha fazla bilgi için bkz: [dağıtma ve Linux tabanlı Hdınsight üzerinde Apache Storm topolojilerini yönetme](apache-storm-deploy-monitor-topology-linux.md)

## <a name="prerequisites"></a>Önkoşullar

* **Hdınsight üzerinde Apache Storm** -bkz [Hdınsight üzerinde Apache Storm ile çalışmaya başlama](apache-storm-tutorial-get-started-linux.md) küme oluşturma adımları için.

* İçin **Storm Panosu**: HTML5 destekleyen modern bir web tarayıcısı.

* İçin **Visual Studio** -Azure SDK'sı 2.5.1 ya da daha yeni ve Visual Studio için Hdınsight araçları. Bkz: [Visual Studio için Hdınsight araçları kullanarak çalışmaya başlama](../hadoop/apache-hadoop-visual-studio-tools-get-started.md) yükleyin ve Visual Studio için Hdınsight araçları yapılandırmak için.

    Visual Studio aşağıdaki sürümlerinden biri:

  * Visual Studio 2012 güncelleştirme 4 ile

  * Visual Studio 2013 güncelleştirme 4 veya Visual Studio 2013 Community

  * Visual Studio 2015 (herhangi bir sürümünü)

  * Visual Studio 2017 (herhangi bir sürümünü)

## <a name="storm-dashboard"></a>Storm Panosu

Storm panosunu Storm kümenizde kullanılabilir bir web sayfasıdır. URL **https://&lt;clustername >.azurehdinsight.net/**, burada **clustername** Hdınsight kümesi üzerinde Storm'a adıdır.

Storm panosunu üstten seçin **gönderme topoloji**. Örnek topoloji çalıştırmak veya karşıya yüklemek ve oluşturduğunuz bir topoloji çalıştırmak için sayfadaki yönergeleri izleyin.

![Gönderme topoloji sayfası][storm-dashboard-submit]

### <a name="storm-ui"></a>Storm kullanıcı Arabirimi

Storm panodan seçin **Storm kullanıcı Arabirimi** bağlantı. Bu, tüm çalışan topolojileri ek olarak küme hakkında bilgileri görüntüler.

![storm kullanıcı arabirimi][storm-dashboard-ui]

> [!NOTE]
> Internet Explorer'ın bazı sürümleriyle ilk ziyaret ettikten sonra Storm kullanıcı Arabirimi yenilemez fark edebilirsiniz. Örneğin, bunu yeni topolojileri gönderdiğiniz ya da daha önce etkinliği, bir topoloji etkin olarak gösterebilir göstermeyebilir. Microsoft bu sorunun farkındadır ve bir çözüm üzerinde çalışmaktadır.

#### <a name="main-page"></a>Ana sayfa

Storm kullanıcı Arabirimi ana sayfasında aşağıdaki bilgileri sağlar:

* **Küme Özet**: Storm kümesi hakkında temel bilgiler.

* **Topoloji özeti**: topolojileri çalışan bir listesi. Belirli topolojileri hakkında daha fazla bilgi görüntülemek için bu bölümdeki bağlantıları kullanın.

* **Yönetici Özeti**: Storm İdarecisi hakkında bilgi.

* **Nimbus yapılandırma**: kümenin Nimbus yapılandırması.

#### <a name="topology-summary"></a>Topoloji özeti

Bir bağlantıdan seçme **topoloji özeti** bölüm topolojisi hakkında aşağıdaki bilgileri görüntüler:

* **Topoloji özeti**: topolojisi hakkında temel bilgiler.

* **Topoloji eylemleri**: topolojisini gerçekleştirdiğiniz yönetim işlemleri.

  * **Etkinleştirme**: devre dışı bırakılan bir topolojiyi işlemeyi sürdürür.

  * **Devre dışı**: çalışan topolojiyi duraklatır.

  * **Yeniden dengelemeniz**: topolojisinin paralelliğini ayarlar. Kümedeki düğüm sayısını değiştirdikten sonra çalışan topolojileri yeniden dengelemeniz gerekir. Bu kümedeki düğümlere artan veya azalan sayısını dengelemek üzere paralelliği ayarlamaya imkan tanır.

      Daha fazla bilgi için bkz: [bir Storm topolojisinin paralelliğini anlama (http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html)](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).

  * **KILL**: belirtilen zaman aşımından sonra Storm topolojisini sonlandırır.

* **Topoloji istatistikleri**: topoloji hakkındaki istatistiklerdir. Bağlantıları kullanın **penceresi** sütun sayfasında diğer girdiler için zaman dilimini ayarlayın.

* **Spout'lar**: topolojisi tarafından kullanılan spout'lar. Belirli spout'lar hakkında daha fazla bilgi görüntülemek için bu bölümdeki bağlantıları kullanın.

* **Cıvatalar**: topolojisi tarafından kullanılan Cıvatalar. Belirli Cıvatalar hakkında daha fazla bilgi görüntülemek için bu bölümdeki bağlantıları kullanın.

* **Topoloji Yapılandırması**: Seçili topoloji yapılandırması.

#### <a name="spout-and-bolt-summary"></a>Spout ve Cıvata özeti

Spout gelen seçme **Spout'lar** veya **Cıvatalar** bölümleri seçili öğe hakkında aşağıdaki bilgileri görüntüler:

* **Bileşen özeti**: spout veya Cıvata hakkında temel bilgiler.

* **Spout/Cıvata istatistikleri**: spout veya Cıvata hakkındaki istatistiklerdir. Bağlantıları kullanın **penceresi** sütun sayfasında diğer girdiler için zaman dilimini ayarlayın.

* **Giriş İstatistikleri** (yalnızca Cıvata): Cıvata tarafından kullanılan giriş akışları hakkında bilgi.

* **Çıkış istatistikleri**: spout veya Cıvata bu tarafından gösterilen akışları hakkında bilgi.

* **Yürütücüler**: spout veya Cıvata örnekleri hakkında bilgi. Seçin **bağlantı noktası** tanılama bilgilerinin günlüğünü görüntülemek belirli bir yürütücü için giriş üretilen Bu örneği için.

* **Hataları**: Bu hata bilgilerini spout veya Cıvata.

## <a name="hdinsight-tools-for-visual-studio"></a>Visual Studio için HDInsight Araçları

Hdınsight araçları C# veya karma topolojiler Storm kümenize göndermek için kullanılabilir. Örnek bir uygulamayı aşağıdaki adımları kullanın. Hdınsight araçları kullanarak kendi topolojileri oluşturma hakkında daha fazla bilgi için bkz: [Visual Studio için Hdınsight araçları kullanarak C# topolojileri geliştirme](apache-storm-develop-csharp-visual-studio-topology.md).

Hdınsight kümesi üzerinde storm'a bir örnek dağıtmak sonra görüntülemek ve topolojisini yönetmek için aşağıdaki adımları kullanın.

1. Visual Studio için Hdınsight araçları en son sürümünü yüklemediyseniz, bkz: [Visual Studio için Hdınsight araçları kullanarak çalışmaya başlama](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).

2. Visual Studio'ni açın, **dosya** > **yeni** > **proje**.

3. İçinde **yeni proje** iletişim kutusunda, genişletin **yüklü** > **şablonları**ve ardından **Hdınsight**. Şablonları listesinden seçin **Storm örnek**. İletişim kutusunun altında uygulama için bir ad yazın.

    ![görüntü](./media/apache-storm-deploy-monitor-topology/sample.png)

4. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **Hdınsight üzerinde Storm Gönder**.

   > [!NOTE]
   > İstenirse, Azure aboneliğiniz için oturum açma kimlik bilgilerini girin. Birden fazla aboneliğiniz varsa, Hdınsight kümesi üzerinde Storm'a içeren oturum açın.

5. Hdınsight küme üzerinde Storm'a seçin **Storm kümesi** aşağı açılan listeyi ve ardından **gönderme**. Gönderim kullanarak başarılı olup olmadığını izleyebilirsiniz **çıkış** penceresi.

6. Topoloji başarıyla gönderildi, **Storm topolojilerini** küme görünür. Topoloji çalışan topolojisi ile ilgili bilgileri görüntülemek için listeden seçin.

    ![Visual studio İzleyicisi](./media/apache-storm-deploy-monitor-topology/vsmonitor.png)

   > [!NOTE]
   > De görüntüleyebilirsiniz **Storm topolojilerini** gelen **Sunucu Gezgini** genişleterek **Azure** > **Hdınsight**ve Hdınsight kümesinde bir Storm sağ tıklayıp seçerek **görünüm Storm topolojilerini**.

    Şekil spout'lar veya Cıvatalar bu bileşenler hakkındaki bilgileri görüntülemek için seçin. Yeni bir pencere için seçilen her öğenin açar.

   > [!NOTE]
   > Sınıf adı topolojisinin topoloji adıdır (Bu durumda, `HelloWord`,) eklenmiş bir zaman damgasına sahip.

7. Gelen **topoloji özeti** görünümü, select **KILL** topoloji durdurmak için.

   > [!NOTE]
   > Storm topolojilerini durdurulmuş veya küme silinene kadar çalıştırmaya devam edin.


## <a name="rest-api"></a>REST API

Benzer yönetim ve izleme işlevselliği REST API'sini kullanarak gerçekleştirebilmek için Storm kullanıcı Arabirimi REST API üzerinde oluşturulmuştur. REST API, yönetmek ve Storm topolojilerini izlemek için özel araçlar oluşturmak için kullanabilirsiniz.

Daha fazla bilgi için bkz: [Storm kullanıcı Arabirimi REST API](https://github.com/apache/storm/blob/0.9.3-branch/STORM-UI-REST-API.md). Aşağıdaki bilgiler, REST API kullanarak hdınsight'ta Apache Storm özeldir.

### <a name="base-uri"></a>Taban URI

Hdınsight kümelerinde REST API için ana Uri **https://&lt;clustername >.azurehdinsight.net/stormui/api/v1/**, burada **clustername** Hdınsight kümesi üzerinde Storm'a adıdır.

### <a name="authentication"></a>Kimlik Doğrulaması

REST API istekleri kullanmalıdır **temel kimlik doğrulaması**, Hdınsight Küme Yönetici adı ve parola kullanın.

> [!NOTE]
> Temel kimlik doğrulaması düz metin kullanarak gönderildiğinden, aşağıdakileri yapmalısınız **her zaman** küme ile iletişimin güvenliğini sağlamak için HTTPS kullanın.

### <a name="return-values"></a>Dönüş değerleri

REST API öğesinden geri döndürülen bilgiler, yalnızca küme ya da sanal makineleri aynı Azure sanal ağ üzerinde küme içinde kullanılabilir olabilir. Zookeeper sunucuları olmaması için Internet'ten erişilebilir döndürülen Örneğin, tam etki alanı adı (FQDN).

## <a name="next-steps"></a>Sonraki Adımlar

Artık nasıl dağıtmak ve Storm panosunu kullanarak topolojileri izlemek, öğrenme öğrendiğinize göre nasıl yapılır:

* [Visual Studio için Hdınsight araçları kullanarak C# topolojileri geliştirme](apache-storm-develop-csharp-visual-studio-topology.md)

* [Maven kullanarak Java tabanlı topolojiler geliştirme](apache-storm-develop-java-topology.md)

Daha fazla örnek topolojileri listesi için bkz: [Hdınsight üzerinde Storm için örnek topolojiler](apache-storm-example-topology.md).

[hdinsight-dashboard]: ./media/apache-storm-deploy-monitor-topology/dashboard-link.png
[storm-dashboard-submit]: ./media/apache-storm-deploy-monitor-topology/submit.png
[storm-dashboard-ui]: ./media/apache-storm-deploy-monitor-topology/storm-ui-summary.png
