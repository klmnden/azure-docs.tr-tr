---
title: Azure HDInsight’a üçüncü taraf Hadoop uygulamaları yükleme | Microsoft Docs
description: Azure HDInsight'a üçüncü taraf Hadoop uygulamaları yükleme hakkında bilgi alın.
services: hdinsight
documentationcenter: ''
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: eaf5904d-41e2-4a5f-8bec-9dde069039c2
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: jgao
ms.openlocfilehash: 3ad112544a703a9b6ec37fa07cbd6df6976d5e26
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="install-third-party-hadoop-applications-on-azure-hdinsight"></a>Azure HDInsight'a üçüncü taraf Hadoop uygulamaları yükleme

Bir üçüncü taraf Hadoop uygulamanın Azure Hdınsight üzerinde nasıl yükleneceğini öğrenin. Kendi uygulamanızı yükleme yönergeleri için bkz. [Özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md).

Hdınsight uygulaması kullanıcıların bir Hdınsight kümesine yükleyebileceği bir uygulamadır. Bu uygulamalar Microsoft veya bağımsız yazılım satıcıları (ISV) tarafından ya da sizin tarafınızdan geliştirilebilir.  

Aşağıdaki listede, yayımlanan uygulamalar gösterilmektedir:

* **AtScale Intelligence Platform** Hdınsight kümenize genişleme OLAP Sunucusu'na kapatır. Uygulama, sorgu milyarlarca satır etkileşimli olarak Microsoft Excel, Powerbı, Tableau yazılım QlikView BI araçları kullanarak verilerin olanak tanır.
* **Hdınsight için CDAP cask** % 80'üretime veri uygulamaları ve verileri Göller süresini keser büyük veri için ilk Birleşik tümleştirme platformu sağlar. Bu uygulama yalnızca Standart HBase 3.4 kümelerini destekler.
* **Hdınsight üzerinde DATAIKU DDS** prototip, veri uzmanlarına derleme ve etkili iş tahminleri ham verileri dönüştürme yüksek oranda belirli hizmetleri dağıtma sağlar.
* **Hdınsight (Beta) için H2O yapay zeka** H2O Sparkling su aşağıdaki dağıtılmış algoritmalarını destekler: GLM, Naïve Bayes, dağıtılmış rasgele orman, gradyan artırmanın makine, derin sinir ağları, derin, K-ortalamaları, PCA, öğrenme Genelleştirilmiş düşük sıra modelleri, Anomali algılama ve Autoencoders.
* **Kyligence analiz platformu** Kyligence analizi Platformu (KAP) Apache Kylin ve Apache Hadoop tarafından desteklenen bir kurumsal kullanıma hazır veri ambarı; alt ikinci sağlar sorgu büyük ölçekli veri kümesi üzerinde gecikme süresi ve veri analizi için basitleştirir işletme kullanıcılarının ve analistleri. 
* **Paxata Self Servis veri hazırlama**
* **KNIME Spark Yürütücü için Spark iş sunucusu** KNIME Spark Yürütücü için Spark iş sunucusu KNIME analiz platformu Hdınsight kümelerine bağlanmak için kullanılır.
* **HDnsight için Akış Kümeleri Veri Toplayıcısı**, tam özellikli bir tümleşik geliştirme ortamı (IDE) sağlar. Bu ortam, akış ve toplu işlem verileri arasında ağ oluşturan “herhangi birinden herhangi birine” alma işlem hatlarını tasarlamanıza, test etmenize, dağıtmanıza ve yönetmenize olanak tanır. Üstelik hiçbiri için özel kod yazmanız gerekmez. 
* **[Trifacta](http://www.trifacta.com/)**  veri mühendisleri ve analistleri daha verimli bir şekilde keşfedin ve makine devrim niteliğinde bir kullanıcı deneyimi, iş akışı ve mimari sağlamak öğrenme yararlanarak bugün çeşitli veri hazırlamak etkinleştirir.
* **WANdisco Fusion HDI uygulama** bulunduğu yerde değiştiğinde veri sürekli tutarlı bağlantı sağlar. Bu, verilerinize dilediğiniz zaman erişim ile herhangi bir yere kapalı kalma süresi ve hiçbir kesintisi sağlar.

Bu makalede verilen yönergeler Azure portalı kullanmaktadır. Ayrıca portaldan Azure Resource Manager şablonunu dışarı aktarabilir veya satıcılardan Resource Manager şablonunun bir kopyasını edinebilir ve Azure PowerShell ile Azure CLI kullanarak şablonu dağıtabilirsiniz.  Bkz: [oluşturmak Hadoop kümeleri Resource Manager şablonları kullanarak Hdınsight'ta](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

## <a name="prerequisites"></a>Önkoşullar
HDInsight uygulamalarını mevcut bir HDInsight kümesine yüklemek istiyorsanız bir HDInsight kümesine sahip olmanız gerekir. Küme oluşturmak için bkz. [Küme oluşturma](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster). HDInsight uygulamalarını ayrıca bir HDInsight kümesi oluştururken yükleyebilirsiniz.

## <a name="install-applications-to-existing-clusters"></a>Var olan kümelere uygulama yükleme
Aşağıdaki yordamda var olan bir HDInsight kümesine HDInsight uygulamalarının nasıl yükleneceği gösterilmektedir.

**Hdınsight uygulaması yükleme**

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Soldaki menüde **HDInsight Kümeleri**’ne tıklayın.
3. Bir HDInsight kümesine tıklayın.  Henüz yoksa öncelikle bir tane oluşturmanız gerekir.  bkz. [Küme oluşturma](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster).
4. **Yapılandırmalar** kategorisinden **Uygulamalar**‘a tıklayın. Yüklü uygulamaların bir listesini görebilirsiniz. Uygulamalar seçeneğini bulamıyorsanız bu, HDInsight kümesinin bu sürümü için bir uygulama olmadığı anlamına gelir.
   
    ![HDInsight uygulamaları portal menüsü](./media/hdinsight-apps-install-applications/hdinsight-apps-portal-menu.png)
5. Tıklatın **Ekle** menüsünde. Mevcut HDInsight uygulamalarının listesini görebilirsiniz.
   
    ![HDInsight uygulamaları kullanılabilir uygulamalar](./media/hdinsight-apps-install-applications/hdinsight-apps-list.png)
6. Kullanılabilir uygulamaları birini tıklatın ve ardından yasal koşulları kabul etmek için yönergeleri izleyin.

Yükleme durumunu portal bildirimlerinden görebilirsiniz (portalın üst kısmındaki zil simgesine tıklayın). Uygulama yüklendikten sonra uygulama üzerinde yüklü uygulamalar listesinde görüntülenir.

## <a name="install-applications-during-cluster-creation"></a>Küme oluşturma sırasında uygulama yükleme
Bir küme oluştururken HDInsight uygulamaları yükleme seçeneğine sahipsiniz. İşlem sırasında, küme oluşturulup çalışır duruma geldikten sonra HDInsight uygulamaları yüklenir. Azure Portalı'nı kullanarak küme oluşturma sırasında uygulamaları yüklemek için--özel--seçeneğini yerine varsayılan--hızlı kullandığınız oluşturma--seçeneği.

## <a name="list-installed-hdinsight-apps-and-properties"></a>Yüklü HDInsight uygulamalarını ve özelliklerini listeleme
Portal bir küme için yüklü HDInsight uygulamalarının listesini ve yüklü olan her bir uygulamanın özelliklerini gösterir.

**Hdınsight uygulamasını listelemek ve özellikleri görüntüle**

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Soldaki menüde **HDInsight Kümeleri**’ne tıklayın. 
3. Bir HDInsight kümesine tıklayın.
4. Gelen **ayarları**, tıklatın **uygulamaları** altında **yapılandırma** kategorisi. Sağ tarafta yüklü uygulamalar listelenir. 
   
    ![HDInsight uygulamaları yüklü uygulamalar](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps-with-apps.png)
5. Özelliği görüntülemek için yüklü uygulamalardan birine tıklayın. Özellik listeler:
   
   * Uygulama adı: uygulamanın adı.
   * Durum: uygulamanın durumu. 
   * Web sayfası: Kenar düğümüne dağıttığınız web uygulamasının URL'si. Kimlik bilgisi, küme için yapılandırdığınız HTTP kullanıcısı kimlik bilgileri ile aynıdır.
   * HTTP uç noktası: Kimlik bilgisi, küme için yapılandırdığınız HTTP kullanıcısı kimlik bilgileri ile aynıdır. 
   * SSH uç noktası: kenar düğümüne bağlanmak için SSH kullanabilirsiniz. SSH kimlik bilgileri, küme için yapılandırdığınız SSH kullanıcısı kimlik bilgileriyle aynıdır. Bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).
6. Bir uygulamayı silmek için uygulamaya sağ tıklayın ve ardından **silmek** ve bağlam menüsünden.

## <a name="connect-to-the-edge-node"></a>Kenar düğümüne bağlanma
HTTP ve SSH kullanarak kenar düğümüne bağlanabilirsiniz. Uç nokta bilgileri [portalda](#list-installed-hdinsight-apps-and-properties) bulunabilir. Bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

HTTP uç noktası kimlik bilgileri, HDInsight kümesi için yapılandırdığınız HTTP kullanıcı kimlik bilgileridir; SSH uç noktası kimlik bilgileri, HDInsight kümesi için yapılandırdığınız SSH kimlik bilgileridir.

## <a name="troubleshoot"></a>Sorun gider
Bkz. [Yükleme sorunlarını giderme](hdinsight-apps-install-custom-applications.md#troubleshoot-the-installation).

## <a name="next-steps"></a>Sonraki adımlar
* [Özel Hdınsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md): yayımlanmamış bir Hdınsight uygulamasının Hdınsight'a nasıl yükleneceğini öğrenin.
* [HDInsight uygulamalarını yayımlama](hdinsight-apps-publish-applications.md): Özel HDInsight uygulamalarınızı Azure Marketi’nde nasıl yayımlayacağınızı öğrenin.
* [MSDN: HDInsight uygulaması yükleme](https://msdn.microsoft.com/library/mt706515.aspx): HDInsight uygulamalarını nasıl tanımlayacağınızı öğrenin.
* [Betik Eylemi kullanarak Linux tabanlı HDInsight kümelerini özelleştirme](hdinsight-hadoop-customize-cluster-linux.md): ek uygulamalar yüklemek için Betik Eyleminin nasıl kullanılacağını öğrenin.
* [Resource Manager şablonları kullanarak HDInsight’ta Linux tabanlı Hadoop kümeleri oluşturma](hdinsight-hadoop-create-linux-clusters-arm-templates.md): HDInsight kümeleri oluşturmak için Resource Manager şablonlarının nasıl çağrılacağını öğrenin.
* [HDInsight’ta boş kenar düğümleri kullanma](hdinsight-apps-use-edge-node.md): HDInsight kümesine erişmek, HDInsight uygulamalarını test etmek ve HDInsight uygulamalarını barındırmak için boş bir kenar düğümünü kullanmayı öğrenin.

