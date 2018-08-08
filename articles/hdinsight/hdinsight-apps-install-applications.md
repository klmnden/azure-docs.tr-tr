---
title: Azure HDInsight'a üçüncü taraf Hadoop uygulamaları yükleme
description: Azure HDInsight'a üçüncü taraf Hadoop uygulamaları yükleme hakkında bilgi alın.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: jasonh
ms.openlocfilehash: c4d8f6fb1804ff48899ebb96d4c4248f337b56ad
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39591984"
---
# <a name="install-third-party-hadoop-applications-on-azure-hdinsight"></a>Azure HDInsight'a üçüncü taraf Hadoop uygulamaları yükleme

Azure HDInsight üzerinde üçüncü taraf Hadoop uygulamasını yüklemeyi öğrenin. Kendi uygulamanızı yükleme yönergeleri için bkz. [Özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md).

Bir HDInsight uygulaması kullanıcıların bir HDInsight kümesine yükleyebileceği bir uygulamadır. Bu uygulamalar Microsoft veya bağımsız yazılım satıcıları (ISV) tarafından ya da sizin tarafınızdan geliştirilebilir.  

Aşağıdaki liste, yayımlanan uygulamalara gösterir:

* **AtScale zeka platformu** genişleme OLAP sunucusunun, HDInsight kümenizle kapatır. Uygulama sorgu etkileşimli olarak QlikView Microsoft Excel, Power BI, Tableau Software BI araçları kullanarak veri satırı milyarlarca olanak tanır.
* **Cask CDAP HDInsight için** % 80 için veri uygulamaları ve veri göllerinin üretim süresini kısalttı büyük veriler için ilk Birleşik tümleştirme platformunu sağlar. Bu uygulama yalnızca Standart HBase 3.4 kümelerini destekler.
* **HDInsight üzerinde DATAIKU DDS** için prototip oluşturma, veri uzmanları oluşturmanızı ve ham verileri etkili iş tahminlerine dönüştüren yüksek oranda ayrıntılı hizmetler dağıtmanızı sağlar.
* **HDInsight (Beta) için yapay zeka H2O** H2O Sparkling Water aşağıdaki dağıtılmış algoritmaları destekler: GLM, Naïve Bayes, dağıtılmış rasgele orman, gradyan geliştirme makinesi, derin sinir ağları, derin öğrenme, K-ortalamaları PCA, Genelleştirilmiş düşük derecelendirme modelleri, Anomali algılama ve Autoencoders.
* **Kyligence Analytics Platform** Kyligence Analytics Platform (KAP) Apache Kylin ve Apache Hadoop tarafından desteklenen bir kurumsal kullanıma hazır veri ambarı; saniyenin sağladığı gecikme süresi çok büyük ölçekli veri kümesinde sorgu ve veri analizi için basitleştirir İş kullanıcıları ve analistleri. 
* **Paxata Self Servis veri hazırlama**
* **Spark iş sunucusu KNIME Spark Yürütücü için** KNIME Spark Yürütücü için Spark iş sunucusu KNIME analiz platformu HDInsight kümelerine bağlanmak için kullanılır.
* **HDnsight için Akış Kümeleri Veri Toplayıcısı**, tam özellikli bir tümleşik geliştirme ortamı (IDE) sağlar. Bu ortam, akış ve toplu işlem verileri arasında ağ oluşturan “herhangi birinden herhangi birine” alma işlem hatlarını tasarlamanıza, test etmenize, dağıtmanıza ve yönetmenize olanak tanır. Üstelik hiçbiri için özel kod yazmanız gerekmez. 
* **[Trifacta](http://www.trifacta.com/)**  daha verimli bir şekilde keşfedin ve çığır açan bir kullanıcı deneyimi, iş akışı ve mimari sağlamak için makine öğrenimini kullanarak çeşitli veri günümüzün hazırlamak, veri mühendisleri ve analistleri sağlar.
* **WANdisco Fusion HDI uygulama** bulunduğu her yerde veriler değiştikçe verileri tutarlı sürekli bağlantı sağlar. Size erişim verilerinize dilediğiniz zaman ve herhangi bir kapalı kalma süresi olmadan ve hiçbir kesinti ile sağlar.

Bu makalede verilen yönergeler Azure portalı kullanmaktadır. Ayrıca portaldan Azure Resource Manager şablonunu dışarı aktarabilir veya satıcılardan Resource Manager şablonunun bir kopyasını edinebilir ve Azure PowerShell ile Azure CLI kullanarak şablonu dağıtabilirsiniz.  Bkz: [Hadoop kümeleri oluşturma Resource Manager şablonlarını kullanarak HDInsight üzerinde](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

## <a name="prerequisites"></a>Önkoşullar
HDInsight uygulamalarını mevcut bir HDInsight kümesine yüklemek istiyorsanız bir HDInsight kümesine sahip olmanız gerekir. Küme oluşturmak için bkz. [Küme oluşturma](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster). HDInsight uygulamalarını ayrıca bir HDInsight kümesi oluştururken yükleyebilirsiniz.

## <a name="install-applications-to-existing-clusters"></a>Var olan kümelere uygulama yükleme
Aşağıdaki yordamda var olan bir HDInsight kümesine HDInsight uygulamalarının nasıl yükleneceği gösterilmektedir.

**Bir HDInsight uygulaması yükleme**

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Soldaki menüde **HDInsight Kümeleri**’ne tıklayın.
3. Bir HDInsight kümesine tıklayın.  Henüz yoksa öncelikle bir tane oluşturmanız gerekir.  bkz. [Küme oluşturma](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster).
4. **Yapılandırmalar** kategorisinden **Uygulamalar**‘a tıklayın. Yüklü uygulamaların bir listesini görebilirsiniz. Uygulamalar seçeneğini bulamıyorsanız bu, HDInsight kümesinin bu sürümü için bir uygulama olmadığı anlamına gelir.
   
    ![HDInsight uygulamaları portal menüsü](./media/hdinsight-apps-install-applications/hdinsight-apps-portal-menu.png)
5. Tıklayın **Ekle** menüsünde. Mevcut HDInsight uygulamalarının listesini görebilirsiniz.
   
    ![HDInsight uygulamaları kullanılabilir uygulamalar](./media/hdinsight-apps-install-applications/hdinsight-apps-list.png)
6. Kullanılabilir uygulamaları birine tıklayın ve ardından yasal koşulları kabul etmek için yönergeleri izleyin.

Yükleme durumunu portal bildirimlerinden görebilirsiniz (portalın üst kısmındaki zil simgesine tıklayın). Uygulama yüklendikten sonra uygulama yüklü uygulamalar listesinde görüntülenir.

## <a name="install-applications-during-cluster-creation"></a>Küme oluşturma sırasında uygulama yükleme
Bir küme oluştururken HDInsight uygulamaları yükleme seçeneğine sahipsiniz. İşlem sırasında, küme oluşturulup çalışır duruma geldikten sonra HDInsight uygulamaları yüklenir. Azure portalını kullanarak küme oluşturma sırasında uygulamaları yüklemek için--özel--seçeneği yerine varsayılan--hızlı kullandığınız seçeneği--oluşturun.

## <a name="list-installed-hdinsight-apps-and-properties"></a>Yüklü HDInsight uygulamalarını ve özelliklerini listeleme
Portal bir küme için yüklü HDInsight uygulamalarının listesini ve yüklü olan her bir uygulamanın özelliklerini gösterir.

**HDInsight uygulamasını listelemek ve özellikleri görüntülemek**

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Soldaki menüde **HDInsight Kümeleri**’ne tıklayın. 
3. Bir HDInsight kümesine tıklayın.
4. Gelen **ayarları**, tıklayın **uygulamaları** altında **yapılandırma** kategorisi. Sağ tarafta yüklü uygulamalar listelenir. 
   
    ![HDInsight uygulamaları yüklü uygulamalar](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps-with-apps.png)
5. Özelliği görüntülemek için yüklü uygulamalardan birine tıklayın. Özellik listeleri:
   
   * Uygulama adı: uygulamanın adı.
   * Durum: uygulamanın durumu. 
   * Web sayfası: Kenar düğümüne dağıttığınız web uygulamasının URL'si. Kimlik bilgisi, küme için yapılandırdığınız HTTP kullanıcısı kimlik bilgileri ile aynıdır.
   * HTTP uç noktası: Kimlik bilgisi, küme için yapılandırdığınız HTTP kullanıcısı kimlik bilgileri ile aynıdır. 
   * SSH uç noktası: kenar düğümüne bağlanmak için SSH kullanabilirsiniz. SSH kimlik bilgileri, küme için yapılandırdığınız SSH kullanıcısı kimlik bilgileriyle aynıdır. Bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).
6. Bir uygulamayı silmek için uygulamaya sağ tıklayın ve ardından **Sil** bağlam menüsünden.

## <a name="connect-to-the-edge-node"></a>Kenar düğümüne bağlanma
HTTP ve SSH kullanarak kenar düğümüne bağlanabilirsiniz. Uç nokta bilgileri [portalda](#list-installed-hdinsight-apps-and-properties) bulunabilir. Bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

HTTP uç noktası kimlik bilgileri, HDInsight kümesi için yapılandırdığınız HTTP kullanıcı kimlik bilgileridir; SSH uç noktası kimlik bilgileri, HDInsight kümesi için yapılandırdığınız SSH kimlik bilgileridir.

## <a name="troubleshoot"></a>Sorun giderme
Bkz. [Yükleme sorunlarını giderme](hdinsight-apps-install-custom-applications.md#troubleshoot-the-installation).

## <a name="next-steps"></a>Sonraki adımlar
* [Özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md): HDInsight için yayımlanmamış bir HDInsight uygulamasının nasıl dağıtılacağını öğrenin.
* [HDInsight uygulamalarını yayımlama](hdinsight-apps-publish-applications.md): Özel HDInsight uygulamalarınızı Azure Marketi’nde nasıl yayımlayacağınızı öğrenin.
* [MSDN: HDInsight uygulaması yükleme](https://msdn.microsoft.com/library/mt706515.aspx): HDInsight uygulamalarını nasıl tanımlayacağınızı öğrenin.
* [Betik Eylemi kullanarak Linux tabanlı HDInsight kümelerini özelleştirme](hdinsight-hadoop-customize-cluster-linux.md): ek uygulamalar yüklemek için Betik Eyleminin nasıl kullanılacağını öğrenin.
* [Resource Manager şablonları kullanarak HDInsight’ta Linux tabanlı Hadoop kümeleri oluşturma](hdinsight-hadoop-create-linux-clusters-arm-templates.md): HDInsight kümeleri oluşturmak için Resource Manager şablonlarının nasıl çağrılacağını öğrenin.
* [HDInsight’ta boş kenar düğümleri kullanma](hdinsight-apps-use-edge-node.md): HDInsight kümesine erişmek, HDInsight uygulamalarını test etmek ve HDInsight uygulamalarını barındırmak için boş bir kenar düğümünü kullanmayı öğrenin.

