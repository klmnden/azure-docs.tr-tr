---
title: "Azure HDInsight’a üçüncü taraf Hadoop uygulamaları yükleme | Microsoft Docs"
description: "Azure HDInsight'a üçüncü taraf Hadoop uygulamaları yükleme hakkında bilgi alın."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: eaf5904d-41e2-4a5f-8bec-9dde069039c2
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/16/2017
ms.author: jgao
ms.openlocfilehash: b3b9d55f2d1e11156c21bc4ec5652e6d7c421db2
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="install-third-party-hadoop-applications-on-azure-hdinsight"></a>Azure HDInsight'a üçüncü taraf Hadoop uygulamaları yükleme

Bu makalede, Azure Hdınsight üzerinde zaten yayımlanmış üçüncü taraf Hadoop uygulama yüklemek öğrenin. Kendi uygulamanızı yükleme yönergeleri için bkz. [Özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md).

HDInsight uygulaması kullanıcıların Linux tabanlı HDInsight kümesine yükleyebileceği bir uygulamadır. Bu uygulamalar Microsoft veya bağımsız yazılım satıcıları (ISV) tarafından ya da sizin tarafınızdan geliştirilebilir.  

Şu anda, yayımlanmış dört uygulama vardır:

* **HDInsight üzerinde DATAIKU DDS**: Dataiku DSS (Data Science Studio), veri uzmanlarının (veri araştırmacıları, iş analizi uzmanları, geliştiriciler...), ham verileri etkili iş tahminlerine dönüştüren yüksek oranda ayrıntılı hizmetler için prototip oluşturma, derleme ve dağıtma işlemlerini gerçekleştirmesine olanak tanıyan bir yazılımdır.
* **Datameer**: [Datameer](http://www.datameer.com/documentation/display/DAS50/Home?ls=Partners&lsd=Microsoft&c=Partners&cd=Microsoft), analiz uzmanlarına Büyük Veri üzerinde sonuçları bulma, çözümleme ve görselleştirme için etkileşimli bir yol sunar. Yeni ilişkileri keşfetmek ve ihtiyaç duyduğunuz yanıtları almak için ek veri kaynaklarını kolayca alın.
* **HDnsight için Akış Kümeleri Veri Toplayıcısı**, tam özellikli bir tümleşik geliştirme ortamı (IDE) sağlar. Bu ortam, akış ve toplu işlem verileri arasında ağ oluşturan “herhangi birinden herhangi birine” alma işlem hatlarını tasarlamanıza, test etmenize, dağıtmanıza ve yönetmenize olanak tanır. Üstelik hiçbiri için özel kod yazmanız gerekmez. 
* **Hdınsight için CDAP cask** % 80'üretime veri uygulamaları ve verileri Göller süresini keser büyük veri için ilk Birleşik tümleştirme platformu sağlar. Bu uygulama yalnızca Standart HBase 3.4 kümelerini destekler.
* **Hdınsight (Beta) için H2O yapay zeka** H2O Sparkling su aşağıdaki dağıtılmış algoritmalarını destekler: GLM, Naïve Bayes, dağıtılmış rasgele orman, gradyan artırmanın makine, derin öğrenme derin sinir ağları, K-ortalamaları, PCA, genelleştirilmiş düşük derece modelleri, Anomali algılama ve Autoencoders.
* **Kyligence analiz platformu** Kyligence analizi Platformu (KAP) Apache Kylin ve Apache Hadoop tarafından desteklenen bir kurumsal kullanıma hazır veri ambarı; büyük ölçekli veri kümesi üzerinde saniyeden sorgu gecikmesi güçlendirir ve veri analizi için işletme kullanıcılarının ve analistleri basitleştirir. 
* **SnapLogic Hadooplex** SnapLogic Hadooplex Hdınsight üzerinde çalışan Self Servis veri alımı ve neredeyse tüm kaynak hazırlık Microsoft Azure bulut platformu sağlayarak iş bilgileri daha hızlı elde etmek müşteriler sağlar.
* **KNIME Spark Yürütücü için Spark iş sunucusu** KNIME Spark Yürütücü için Spark iş sunucusu KNIME analiz platformu Hdınsight kümelerine bağlanmak için kullanılır.

Bu makalede verilen yönergeler Azure portalı kullanmaktadır. Ayrıca portaldan Azure Resource Manager şablonunu dışarı aktarabilir veya satıcılardan Resource Manager şablonunun bir kopyasını edinebilir ve Azure PowerShell ile Azure CLI kullanarak şablonu dağıtabilirsiniz.  Bkz. [HDInsight’ta Resource Manager şablonları kullanarak Linux tabanlı Hadoop kümeleri oluşturma](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

## <a name="prerequisites"></a>Ön koşullar
HDInsight uygulamalarını mevcut bir HDInsight kümesine yüklemek istiyorsanız bir HDInsight kümesine sahip olmanız gerekir. Küme oluşturmak için bkz. [Küme oluşturma](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster). HDInsight uygulamalarını ayrıca bir HDInsight kümesi oluştururken yükleyebilirsiniz.

## <a name="install-applications-to-existing-clusters"></a>Var olan kümelere uygulama yükleme
Aşağıdaki yordamda var olan bir HDInsight kümesine HDInsight uygulamalarının nasıl yükleneceği gösterilmektedir.

**Bir HDInsight uygulaması yüklemek için**

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. Soldaki menüde **HDInsight Kümeleri**’ne tıklayın.  Bu seçeneği görmüyorsanız **Diğer Hizmetler** ve ardından **HDInsight Kümeleri**’ne tıklayın.
3. Bir HDInsight kümesine tıklayın.  Henüz yoksa öncelikle bir tane oluşturmanız gerekir.  bkz. [Küme oluşturma](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster).
4. **Yapılandırmalar** kategorisinden **Uygulamalar**‘a tıklayın. Yüklü uygulamaların bir listesini görebilirsiniz. Uygulamalar seçeneğini bulamıyorsanız bu, HDInsight kümesinin bu sürümü için bir uygulama olmadığı anlamına gelir.
   
    ![HDInsight uygulamaları portal menüsü](./media/hdinsight-apps-install-applications/hdinsight-apps-portal-menu.png)
5. Dikey pencere menüsünden **Ekle**’ye tıklayın. 
   
    ![HDInsight uygulamaları yüklü uygulamalar](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps.png)
   
    Mevcut HDInsight uygulamalarının listesini görebilirsiniz.
   
    ![HDInsight uygulamaları kullanılabilir uygulamalar](./media/hdinsight-apps-install-applications/hdinsight-apps-list.png)
6. Uygulamalardan birine tıklayın, yasal koşulları kabul edin ve ardından **Seç**’e tıklayın.

Yükleme durumunu portal bildirimlerinden görebilirsiniz (portalın üst kısmındaki zil simgesine tıklayın). Uygulama yüklendikten sonra Yüklü Uygulamalar dikey penceresinde görünür.

## <a name="install-applications-during-cluster-creation"></a>Küme oluşturma sırasında uygulama yükleme
Bir küme oluştururken HDInsight uygulamaları yükleme seçeneğine sahipsiniz. İşlem sırasında, küme oluşturulup çalışır duruma geldikten sonra HDInsight uygulamaları yüklenir. Aşağıdaki yordamda bir küme oluştururken HDInsight uygulamalarının nasıl yükleneceği gösterilmektedir.

**Bir HDInsight uygulaması yüklemek için**

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. **YENİ** öğesine, **Veri + Analiz** öğesine ve ardından **HDInsight**'a tıklayın.
3. **Küme Adı** girin: Bu ad genel olarak benzersiz olmalıdır.
4. Tıklatın **abonelik** kümesi için kullanılan Azure aboneliğini seçin.
5. **Küme Türü Seçin**’e tıklayın ve ardından şunu seçin:
   
   * **Küme Türü**: Neyi seçeceğinizi bilmiyorsanız **Hadoop** seçeneğini belirleyin. En popüler küme türü budur.
   * **İşletim Sistemi**: **Linux** seçeneğini belirleyin.
   * **Sürüm**: Neyi seçeceğinizi bilmiyorsanız varsayılan sürümü kullanın. Daha fazla bilgi için bkz. [HDInsight küme sürümleri](hdinsight-component-versioning.md).
   * **Küme Katmanı**: Azure HDInsight iki kategoride büyük veri bulutu teklifleri sunar: Standart katmanı ve Premium katmanı. Daha fazla bilgi için bkz. [Küme katmanları](hdinsight-hadoop-provision-linux-clusters.md#cluster-tiers).
6. **Uygulamalar**’a tıklayın, yayımlanmış uygulamalardan birine ve ardından **Seç**’e tıklayın.
7. **Kimlik Bilgileri**’ne tıklayın ve ardından yönetici kullanıcı için bir parola girin. Girmeniz gereken bir **SSH kullanıcı adı** ve ya da bir **parola** veya **ortak anahtar**, SSH kullanıcı kimlik doğrulaması için kullanılır. Ortak anahtar kullanılması önerilen yaklaşımdır. Alt kısımdaki **Seç**’e tıklayarak kimlik bilgileri yapılandırmasını kaydedin.
8. Tıklatın **veri kaynağı**, var olan depolama hesaplarından birini seçin veya kümenin varsayılan depolama hesabı olarak kullanılacak yeni bir depolama hesabı oluşturun.
9. Var olan bir kaynak grubunu seçmek için **Kaynak Grubu**’na tıklayın ya da **Yeni**’ye tıklayarak yeni bir kaynak grubu oluşturun
10. **Yeni HDInsight Kümesi** dikey penceresinde **Başlangıç Panosuna Sabitle** seçiminin yapıldığından emin olun ve ardından **Oluştur**’a tıklayın. 

## <a name="list-installed-hdinsight-apps-and-properties"></a>Yüklü HDInsight uygulamalarını ve özelliklerini listeleme
Portal bir küme için yüklü HDInsight uygulamalarının listesini ve yüklü olan her bir uygulamanın özelliklerini gösterir.

**HDInsight uygulamasını listelemek ve özellikleri görüntülemek için**

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. Soldaki menüde **HDInsight Kümeleri**’ne tıklayın.  Bu seçeneği görmüyorsanız **Gözat**’a ve ardından **HDInsight Kümeleri**’ne tıklayın.
3. Bir HDInsight kümesine tıklayın.
4. **Ayarlar** dikey penceresinde **Genel** kategorisi altındaki **Uygulamalar**’a tıklayın. Yüklü Uygulamalar dikey penceresinde tüm yüklü uygulamalar listelenir. 
   
    ![HDInsight uygulamaları yüklü uygulamalar](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps-with-apps.png)
5. Özelliği görüntülemek için yüklü uygulamalardan birine tıklayın. Özellik dikey penceresinde şunlar listelenir:
   
   * Uygulama adı: uygulamanın adı.
   * Durum: uygulamanın durumu. 
   * Web sayfası: Kenar düğümüne dağıttığınız web uygulamasının URL'si. Kimlik bilgisi, küme için yapılandırdığınız HTTP kullanıcısı kimlik bilgileri ile aynıdır.
   * HTTP uç noktası: Kimlik bilgisi, küme için yapılandırdığınız HTTP kullanıcısı kimlik bilgileri ile aynıdır. 
   * SSH uç noktası: kenar düğümüne bağlanmak için SSH kullanabilirsiniz. SSH kimlik bilgileri, küme için yapılandırdığınız SSH kullanıcısı kimlik bilgileriyle aynıdır. Bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).
6. Bir uygulamayı silmek için uygulamaya sağ tıklayın ve ardından **silmek** ve bağlam menüsünden.

## <a name="connect-to-the-edge-node"></a>Kenar düğümüne bağlanma
HTTP ve SSH kullanarak kenar düğümüne bağlanabilirsiniz. Uç nokta bilgileri [portalda](#list-installed-hdinsight-apps-and-properties) bulunabilir. Bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

HTTP uç noktası kimlik bilgileri, HDInsight kümesi için yapılandırdığınız HTTP kullanıcı kimlik bilgileridir; SSH uç noktası kimlik bilgileri, HDInsight kümesi için yapılandırdığınız SSH kimlik bilgileridir.

## <a name="troubleshoot"></a>Sorun giderme
Bkz. [Yükleme sorunlarını giderme](hdinsight-apps-install-custom-applications.md#troubleshoot-the-installation).

## <a name="next-steps"></a>Sonraki adımlar
* [Özel Hdınsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md): yayımlanmamış bir Hdınsight uygulamasının Hdınsight'a nasıl yükleneceğini öğrenin.
* [HDInsight uygulamalarını yayımlama](hdinsight-apps-publish-applications.md): Özel HDInsight uygulamalarınızı Azure Marketi’nde nasıl yayımlayacağınızı öğrenin.
* [MSDN: HDInsight uygulaması yükleme](https://msdn.microsoft.com/library/mt706515.aspx): HDInsight uygulamalarını nasıl tanımlayacağınızı öğrenin.
* [Betik Eylemi kullanarak Linux tabanlı HDInsight kümelerini özelleştirme](hdinsight-hadoop-customize-cluster-linux.md): ek uygulamalar yüklemek için Betik Eyleminin nasıl kullanılacağını öğrenin.
* [Resource Manager şablonları kullanarak HDInsight’ta Linux tabanlı Hadoop kümeleri oluşturma](hdinsight-hadoop-create-linux-clusters-arm-templates.md): HDInsight kümeleri oluşturmak için Resource Manager şablonlarının nasıl çağrılacağını öğrenin.
* [HDInsight’ta boş kenar düğümleri kullanma](hdinsight-apps-use-edge-node.md): HDInsight kümesine erişmek, HDInsight uygulamalarını test etmek ve HDInsight uygulamalarını barındırmak için boş bir kenar düğümünü kullanmayı öğrenin.

