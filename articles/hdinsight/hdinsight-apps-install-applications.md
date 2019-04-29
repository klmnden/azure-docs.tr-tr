---
title: Azure HDInsight üzerinde üçüncü taraf uygulamaları yükleme
description: Azure HDInsight'a üçüncü taraf Hadoop uygulamaları yükleme hakkında bilgi alın.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: hrasheed
ms.openlocfilehash: 7cbcc8ac05e4541406abd08c14006a8abc0c91ca
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62097376"
---
# <a name="install-third-party-apache-hadoop-applications-on-azure-hdinsight"></a>Azure HDInsight üzerinde üçüncü taraf Apache Hadoop uygulamaları yükleme

Bir üçüncü taraf yüklemeyi öğrenin [Apache Hadoop](https://hadoop.apache.org/) Azure HDInsight uygulaması. Kendi uygulamanızı yükleme yönergeleri için bkz. [Özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md).

Bir HDInsight uygulaması kullanıcıların bir HDInsight kümesine yükleyebileceği bir uygulamadır. Bu uygulamalar Microsoft veya bağımsız yazılım satıcıları (ISV) tarafından ya da sizin tarafınızdan geliştirilebilir.  

Aşağıdaki liste, yayımlanan uygulamalara gösterir:

|Uygulama |Küme türleri | Açıklama |
|---|---|---|
|AtScale zeka platformu |Hadoop |AtScale sorgu milyarlarca satır etkileşimli olarak zaten biliyorsanız, sahibi ve QlikView için Microsoft Excel, Power BI, Tableau Software – sevdiğiniz BI araçlarını kullanarak veri izin vererek, HDInsight kümenizle genişleme OLAP sunucusunun kapatır. |
|CDAP 4.2, HDInsight için 4.3 |HBase |CDAP, saat değeri için Self Servis veri sağlamak Hadoop ve BT'nin hızlandırır büyük veriler için ilk Birleşik tümleştirme platformudur. Açık kaynaklı ve Genişletilebilir CDAP yenilik teknolojik avantajların önündeki engelleri kaldırır. Gereksinimler: 4 bölge düğüm, en düşük D3 v2. |
|Datameer |Hadoop |Hazırlama Datameer'ın Self-Servis ölçeklenebilir platformu, karmaşık çok kaynaklı veri değerli çalışma ortamına hazır bilgi, daha hızlı ve daha akıllı içgörüler sunan bir kurumsal ölçekte oturum kapatma keşfetmeye ve analiz için verilerinizi düzenleyen hızlandırır. |
|HDInsight üzerinde Dataiku DSS |Hadoop, Spark |Dataiku DSS, veri uzmanları ve veri analistleri sağlayan bir kurumsal veri bilimi platformu işbirliği tasarlamak ve yeni veri ürünleri ve Hizmetleri daha verimli bir şekilde çalıştırmak için ham verileri etkili tahminlerine kapatma. |
|WANdisco Fusion HDI uygulama - 2.12.3, 2.12.1 2.11.2 |Hadoop, Spark, HBase, Storm, Kafka |Dağıtılmış bir ortamda tutarlı verileri tutmak çok büyük veri işlemleri zordur. WANdisco Fusion, kurumsal düzeyde yazılım platformu, yapılandırılmamış veriler tutarlılık arasında herhangi bir ortam sağlayarak bu sorunu çözer. |
|HDInsight için H2O SparklingWater |Spark |H2O Sparkling Water aşağıdaki dağıtılmış algoritmaları destekler: GLM, Naïve Bayes, dağıtılmış rasgele orman, gradyan geliştirme makinesi, derin öğrenme, K-ortalamaları, PCA, genelleştirilmiş düşük sıra modelleri, Anomali algılama, Autoencoders derin sinir ağları. |
|HDInsight için gerçek zamanlı veri tümleştirmesi Striim |Hadoop,HBase,Storm,Spark,Kafka |Bir-akış veri tümleştirmesi + zeka platformu, sürekli alımı, işleme ve analizi farklı veri akışları etkinleştirme uca Striim (okunur "stream") olur. |
|HDInsight için Jumbune |Hadoop, Spark |Yüksek bir düzeyde Jumbune 1, kuruluşlara yardımcı olur. Tez hızlandırma, MapReduce ve Spark altyapısı yığın, Java, Scala iş yükü performansı temel. 2. İzleme, 3 proaktif Hadoop kümesi. Dağıtılmış dosya sistemi veri kalitesi yönetimini kurma. |
|Kyligence Enterprise |Hadoop,HBase,Spark |Apache Kylin tarafından desteklenen, Kyligence Kurumsal BI büyük verileri sağlar. Hadoop üzerinde bir kurumsal OLAP altyapısı Kyligence Kurumsal BI metodolojisini ve endüstri standardı veri ambarı ile hadoop'ta BI mimari için iş analisti düşürüyor. |
|Spark iş sunucusu KNIME Spark Yürütücü için |Spark |Spark iş sunucusu KNIME Spark Yürütücü için KNIME analiz platformu HDInsight kümelerine bağlanmak için kullanılır. |
|Yıldız Yağmuru Presto üzerindeki Azure HDInsight, Yıldız Yağmuru Presto (v0.213 e) |Hadoop |Bir hızlı ve ölçeklenebilir dağıtılmış SQL sorgu presto altyapısıdır. Depolama ve işlem ayrımı için tasarlanmış, Presto Azure Data Lake Storage, Azure Blob Depolama, SQL ve NoSQL veritabanları ve diğer veri kaynaklarından veri sorgulamak için idealdir. |
|HDInsight bulut için StreamSets Data Collector |Hadoop,HBase,Spark,Kafka |StreamSets Data Collector, gerçek zamanlı veri akışları basit, güçlü bir altyapısıdır. Veri Toplayıcı, veri akışlarını rota ve işlem verileri için kullanın. Bu, 30 günlük deneme sürümü lisansı ile birlikte gelir. |
|[Trifacta Wrangler Enterprise](https://www.trifacta.com/) |Hadoop, Spark, HBase |HDInsight için Trifacta Wrangler kurumsal verilerin herhangi bir ölçekte denetimi Kurumsal çapta veri destekler. Trifacta Azure'da çalıştırmanın maliyeti, Trifacta abonelik maliyetleri ve sanal makineler için Azure altyapı maliyetleri birleşimidir. |
|3.1 Unifi veri platformu |Hadoop,HBase,Storm,Spark |Unifi veri platformu sorunsuz bir şekilde tümleşik bir veri sorunlarını gidermek için iş kullanıcısı bu sürücü gelirlerinizi güç katın, maliyetler veya işletim karmaşıklığını azaltmak üzere tasarlanan bir Self Servis veri araçları paketidir. |
|Unraveldata APM |Spark |HDInsight Spark kümesi için uygulama verileri aydınlatmak. |
|Waterline veri Kataloğu |Spark |Waterline kataloglar, düzenler ve veri iş terimlerini otomatik etiketi verilerle yapay ZEKA kullanarak yönetir. Waterline'nın iş literate Kataloğu, Self Servis analizler, uyumluluk ve idare ve BT yönetimi girişimleri için önemli bir başarı bileşenidir. |

Bu makalede verilen yönergeler Azure portalı kullanmaktadır. Ayrıca portaldan Azure Resource Manager şablonunu dışarı aktarabilir veya satıcılardan Resource Manager şablonunun bir kopyasını edinmek ve şablonu dağıtmak için Azure PowerShell ve klasik Azure CLI'yı kullanın.  Bkz: [Apache Hadoop kümeleri oluşturma Resource Manager şablonlarını kullanarak HDInsight üzerinde](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

## <a name="prerequisites"></a>Önkoşullar
HDInsight uygulamalarını mevcut bir HDInsight kümesine yüklemek istiyorsanız bir HDInsight kümesine sahip olmanız gerekir. Küme oluşturmak için bkz. [Küme oluşturma](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster). HDInsight uygulamalarını ayrıca bir HDInsight kümesi oluştururken yükleyebilirsiniz.

## <a name="install-applications-to-existing-clusters"></a>Var olan kümelere uygulama yükleme
Aşağıdaki yordamda var olan bir HDInsight kümesine HDInsight uygulamalarının nasıl yükleneceği gösterilmektedir.

**Bir HDInsight uygulaması yükleme**

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol menüden gidin **tüm hizmetleri** > **Analytics** > **HDInsight kümeleri**.
3. Bir HDInsight kümesi listeden seçin.  Henüz yoksa öncelikle bir tane oluşturmanız gerekir.  bkz. [Küme oluşturma](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster).
4. Altında **ayarları** kategorisi, select **uygulamaları**. Ana penceresinde yüklü uygulamaların bir listesini görebilirsiniz. 
   
    ![HDInsight uygulamaları portal menüsü](./media/hdinsight-apps-install-applications/hdinsight-apps-portal-menu.png)
5. Seçin **+ Ekle** menüsünde. Kullanılabilir uygulamaların listesini görebilirsiniz.  Varsa **+ Ekle** griyse, HDInsight kümesinin bu sürümü için uygulama olduğu anlamına gelir vardır.
   
    ![HDInsight uygulamaları kullanılabilir uygulamalar](./media/hdinsight-apps-install-applications/hdinsight-apps-list.png)
6. Kullanılabilir uygulamaları birini seçin ve ardından yasal koşulları kabul etmek için yönergeleri izleyin.

Yükleme durumunu portal bildirimlerinden (portalın üst kısmındaki zil simgesini seçin) görebilirsiniz. Uygulama yüklendikten sonra uygulama yüklü uygulamalar listesinde görüntülenir.

## <a name="install-applications-during-cluster-creation"></a>Küme oluşturma sırasında uygulama yükleme
Bir küme oluştururken HDInsight uygulamaları yükleme seçeneğine sahipsiniz. İşlem sırasında, küme oluşturulup çalışır duruma geldikten sonra HDInsight uygulamaları yüklenir. Azure portalını kullanarak küme oluşturma sırasında uygulamaları yüklemek için kullandığınız **özel** seçeneği varsayılan yerine **hızlı oluşturma** seçeneği.

## <a name="list-installed-hdinsight-apps-and-properties"></a>Yüklü HDInsight uygulamalarını ve özelliklerini listeleme
Portal bir küme için yüklü HDInsight uygulamalarının listesini ve yüklü olan her bir uygulamanın özelliklerini gösterir.

**HDInsight uygulamasını listelemek ve özellikleri görüntülemek**

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol menüden gidin **tüm hizmetleri** > **Analytics** > **HDInsight kümeleri**.
3. Bir HDInsight kümesi listeden seçin.
4. Altında **ayarları** kategorisi, select **uygulamaları**. Ana penceresinde yüklü uygulamaların bir listesini görebilirsiniz. 
   
    ![HDInsight uygulamaları yüklü uygulamalar](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps-with-apps.png)
5. Özelliği görüntülemek için yüklü uygulamalardan birini seçin. Özellik listeleri:

    |Özellik | Açıklama |
    |---|---|
    |Uygulama adı |Uygulama adı. |
    |Durum |Uygulama durumu. |
    |Web sayfası |Kenar düğümüne dağıttığınız web uygulamasının URL'si. Kimlik bilgisi, küme için yapılandırdığınız HTTP kullanıcısı kimlik bilgileri ile aynıdır. |
    |SSH uç noktası |Kenar düğümüne bağlanmak için SSH kullanabilirsiniz. SSH kimlik bilgileri, küme için yapılandırdığınız SSH kullanıcısı kimlik bilgileriyle aynıdır. Bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md). |
    |Açıklama | Uygulama açıklaması. |

6. Bir uygulamayı silmek için uygulamaya sağ tıklayın ve ardından **Sil** bağlam menüsünden.

## <a name="connect-to-the-edge-node"></a>Kenar düğümüne bağlanma
HTTP ve SSH kullanarak kenar düğümüne bağlanabilirsiniz. Uç nokta bilgileri [portalda](#list-installed-hdinsight-apps-and-properties) bulunabilir. Bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

HTTP uç noktası kimlik bilgileri, HDInsight kümesi için yapılandırdığınız HTTP kullanıcı kimlik bilgileridir; SSH uç noktası kimlik bilgileri, HDInsight kümesi için yapılandırdığınız SSH kimlik bilgileridir.

## <a name="troubleshoot"></a>Sorun giderme
Bkz. [Yükleme sorunlarını giderme](hdinsight-apps-install-custom-applications.md#troubleshoot-the-installation).

## <a name="next-steps"></a>Sonraki adımlar
* [Özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md): HDInsight için yayımlanmamış bir HDInsight uygulamasının nasıl dağıtılacağını öğrenin.
* [HDInsight uygulamaları yayımlama](hdinsight-apps-publish-applications.md): Özel HDInsight uygulamalarınızı Azure Marketi'nde yayımlama konusunda bilgi edinin.
* [MSDN: Bir HDInsight uygulaması yükleme](https://msdn.microsoft.com/library/mt706515.aspx): HDInsight uygulamalarını nasıl tanımlayacağınızı öğrenin.
* [Betik Eylemi kullanarak Linux tabanlı HDInsight kümelerini özelleştirme](hdinsight-hadoop-customize-cluster-linux.md): ek uygulamalar yüklemek için Betik Eyleminin nasıl kullanılacağını öğrenin.
* [Linux tabanlı Apache Hadoop kümelerini Resource Manager şablonlarını kullanarak HDInsight oluşturma](hdinsight-hadoop-create-linux-clusters-arm-templates.md): HDInsight kümeleri oluşturmak için Resource Manager şablonlarının nasıl çağrılacağını öğrenin.
* [HDInsight’ta boş kenar düğümleri kullanma](hdinsight-apps-use-edge-node.md): HDInsight kümesine erişmek, HDInsight uygulamalarını test etmek ve HDInsight uygulamalarını barındırmak için boş bir kenar düğümünü kullanmayı öğrenin.

