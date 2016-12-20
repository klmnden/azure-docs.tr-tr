---
title: "HDInsight’a Hadoop uygulamaları yükleme | Microsoft Belgeleri"
description: "HDInsight uygulamalarına HDInsight uygulamalarını nasıl yükleyeceğinizi öğrenin."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: eaf5904d-41e2-4a5f-8bec-9dde069039c2
ms.service: hdinsight
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/23/2016
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: 71aef298af187cd4c370edf9fedc42e75ec10c41
ms.openlocfilehash: 57fbf532c64621ef7d171fc5092708737cd36c33


---
# <a name="install-hdinsight-applications"></a>HDInsight uygulamaları yükleme
HDInsight uygulaması kullanıcıların Linux tabanlı HDInsight kümesine yükleyebileceği bir uygulamadır. Bu uygulamalar Microsoft veya bağımsız yazılım satıcıları (ISV) tarafından ya da sizin tarafınızdan geliştirilebilir. Bu makalede yayımlanmış bir uygulamanın nasıl yükleneceğini öğreneceksiniz. Kendi uygulamanızı yüklemek için bkz. [Özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md). 

Şu anda, yayımlanmış üç uygulama vardır:

* **Datameer**: [Datameer](http://www.datameer.com/documentation/display/DAS50/Home?ls=Partners&lsd=Microsoft&c=Partners&cd=Microsoft), analiz uzmanlarına Büyük Veri üzerinde sonuçları bulma, çözümleme ve görselleştirme için etkileşimli bir yol sunar. Yeni ilişkileri keşfetmek ve ihtiyaç duyduğunuz yanıtları almak için ek veri kaynaklarını kolayca alın.
* **HDnsight için Akış Kümeleri Veri Toplayıcısı**, tam özellikli bir tümleşik geliştirme ortamı (IDE) sağlar. Bu ortam, akış ve toplu işlem verileri arasında ağ oluşturan “herhangi birinden herhangi birine” alma işlem hatlarını tasarlamanıza, test etmenize, dağıtmanıza ve yönetmenize olanak tanır. Üstelik hiçbiri için özel kod yazmanız gerekmez. 
* **HDInsight için Cask CDAP 3.5**, büyük veriler için veri uygulamaları ve veri göllerinin üretim süresini %80 azaltan ilk birleşik tümleştirme platformunu sağlar. Bu uygulama yalnızca Standart HBase 3.4 kümelerini destekler.


Bu makalede verilen yönergeler Azure portalı kullanmaktadır. Ayrıca portaldan Azure Resouce Manager şablonunu dışarı aktarabilir veya satıcılardan Resouce Manager şablonunun bir kopyasını edinebilir ve Azure PowerShell ile Azure CLI kullanarak şablonu dağıtabilirsiniz.  Bkz. [HDInsight’ta Resource Manager şablonları kullanarak Linux tabanlı Hadoop kümeleri oluşturma](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

## <a name="prerequisites"></a>Ön koşullar
HDInsight uygulamalarını mevcut bir HDInsight kümesine yüklemek istiyorsanız bir HDInsight kümesine sahip olmanız gerekir. Küme oluşturmak için bkz. [Küme oluşturma](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster). HDInsight uygulamalarını ayrıca bir HDInsight kümesi oluştururken yükleyebilirsiniz.

## <a name="install-applications-to-existing-clusters"></a>Var olan kümelere uygulama yükleme
Aşağıdaki yordamda var olan bir HDInsight kümesine HDInsight uygulamalarının nasıl yükleneceği gösterilmektedir.

**Bir HDInsight uygulaması yüklemek için**

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Soldaki menüde **HDInsight Kümeleri**’ne tıklayın.  Bu seçeneği görmüyorsanız **Gözat**’a ve ardından **HDInsight Kümeleri**’ne tıklayın.
3. Bir HDInsight kümesine tıklayın.  Henüz yoksa öncelikle bir tane oluşturmanız gerekir.  bkz. [Küme oluşturma](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).
4. **Yapılandırmalar** kategorisinden **Uygulamalar**‘a tıklayın. Yüklü uygulamalar varsa bunların listesini görebilirsiniz.
   
    ![hdinsight uygulamaları portal menüsü](./media/hdinsight-apps-install-applications/hdinsight-apps-portal-menu.png)
5. Dikey pencere menüsünden **Ekle**’ye tıklayın. 
   
    ![hdinsight uygulamaları yüklü uygulamalar](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps.png)
   
    Mevcut HDInsight uygulamalarının listesini görebilirsiniz.
   
    ![hdinsight uygulamaları kullanılabilir uygulamalar](./media/hdinsight-apps-install-applications/hdinsight-apps-list.png)
6. Uygulamalardan birine tıklayın, yasal koşulları kabul edin ve ardından **Seç**’e tıklayın.

Yükleme durumunu portal bildirimlerinden görebilirsiniz (portalın üst kısmındaki zil simgesine tıklayın). Uygulama yüklendikten sonra Yüklü Uygulamalar dikey penceresinde görünür.

## <a name="install-applications-during-cluster-creation"></a>Küme oluşturma sırasında uygulama yükleme
Bir küme oluştururken HDInsight uygulamaları yükleme seçeneğine sahipsiniz. İşlem sırasında, küme oluşturulup çalışır duruma geldikten sonra HDInsight uygulamaları yüklenir. Aşağıdaki yordamda bir küme oluştururken HDInsight uygulamalarının nasıl yükleneceği gösterilmektedir.

**Bir HDInsight uygulaması yüklemek için**

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. **YENİ** öğesine, **Veri + Analiz** öğesine ve ardından **HDInsight**'a tıklayın.
3. **Küme Adı** girin: Bu ad genel olarak benzersiz olmalıdır.
4. **Abonelik** öğesine tıklayarak küme için kullanılacak Azure aboneliğini seçin.
5. **Küme Türü Seçin**’e tıklayın ve ardından şunu seçin:
   
   * **Küme Türü**: Neyi seçeceğinizi bilmiyorsanız **Hadoop** seçeneğini belirleyin. En popüler küme türü budur.
   * **İşletim Sistemi**: **Linux** seçeneğini belirleyin.
   * **Sürüm**: Neyi seçeceğinizi bilmiyorsanız varsayılan sürümü kullanın. Daha fazla bilgi için bkz. [HDInsight küme sürümleri](hdinsight-component-versioning.md).
   * **Küme Katmanı**: Azure HDInsight iki kategoride büyük veri bulutu teklifleri sunar: Standart katmanı ve Premium katmanı. Daha fazla bilgi için bkz. [Küme katmanları](hdinsight-hadoop-provision-linux-clusters.md#cluster-tiers).
6. **Uygulamalar**’a tıklayın, yayımlanmış uygulamalardan birine ve ardından **Seç**’e tıklayın.
7. **Kimlik Bilgileri**’ne tıklayın ve ardından yönetici kullanıcı için bir parola girin. Ayrıca SSH kullanıcısının kimliğini doğrulayacak bir **SSH Kullanıcı Adı** ve bir **PAROLA** ya da **ORTAK ANAHTAR** girmeniz gerekir. Ortak anahtar kullanılması önerilen yaklaşımdır. Alt kısımdaki **Seç**’e tıklayarak kimlik bilgileri yapılandırmasını kaydedin.
8. **Veri Kaynağı**’na tıklayın, var olan depolama hesaplarından birini seçin ya da küme için varsayılan hesap olarak kullanılacak yeni bir depolama hesabı oluşturun.
9. Var olan bir kaynak grubunu seçmek için **Kaynak Grubu**’na tıklayın ya da **Yeni**’ye tıklayarak yeni bir kaynak grubu oluşturun
10. **Yeni HDInsight Kümesi** dikey penceresinde **Başlangıç Panosuna Sabitle** seçiminin yapıldığından emin olun ve ardından **Oluştur**’a tıklayın. 

## <a name="list-installed-hdinsight-apps-and-properties"></a>Yüklü HDInsight uygulamalarını ve özelliklerini listeleme
Portal bir küme için yüklü HDInsight uygulamalarının listesini ve yüklü olan her bir uygulamanın özelliklerini gösterir.

**HDInsight uygulamasını listelemek ve özellikleri görüntülemek için**

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Soldaki menüde **HDInsight Kümeleri**’ne tıklayın.  Bu seçeneği görmüyorsanız **Gözat**’a ve ardından **HDInsight Kümeleri**’ne tıklayın.
3. Bir HDInsight kümesine tıklayın.
4. **Ayarlar** dikey penceresinde **Genel** kategorisi altındaki **Uygulamalar**’a tıklayın. Yüklü Uygulamalar dikey penceresinde tüm yüklü uygulamalar listelenir. 
   
    ![hdinsight uygulamaları yüklü uygulamalar](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps-with-apps.png)
5. Özelliği görüntülemek için yüklü uygulamalardan birine tıklayın. Özellik dikey penceresinde şunlar listelenir:
   
   * Uygulama adı: uygulamanın adı.
   * Durum: uygulamanın durumu. 
   * Web sayfası: Varsa herhangi bir kenar düğümüne dağıttığınız web uygulamasının URL'si. Kimlik bilgisi, küme için yapılandırdığınız HTTP kullanıcısı kimlik bilgileri ile aynıdır.
   * HTTP uç noktası: Kimlik bilgisi, küme için yapılandırdığınız HTTP kullanıcısı kimlik bilgileri ile aynıdır. 
   * SSH uç noktası: Kenar düğümüne bağlanmak için [SSH](hdinsight-hadoop-linux-use-ssh-unix.md) kullanabilirsiniz. SSH kimlik bilgileri, küme için yapılandırdığınız SSH kullanıcısı kimlik bilgileriyle aynıdır.
6. Bir uygulamayı silmek için uygulamaya sağ tıklayın ve ardından kısayol menüsünden **Sil**’e tıklayın.

## <a name="connect-to-the-edge-node"></a>Kenar düğümüne bağlanma
HTTP ve SSH kullanarak kenar düğümüne bağlanabilirsiniz. Uç nokta bilgileri [portalda](#list-installed-hdinsight-apps-and-properties) bulunabilir. SSH kullanma hakkında daha fazla bilgi için bkz. [Linux, Unix veya OS X işletim sistemlerinden HDInsight’ta Linux tabanlı Hadoop ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md). 

HTTP uç noktası kimlik bilgileri, HDInsight kümesi için yapılandırdığınız HTTP kullanıcı kimlik bilgileridir; SSH uç noktası kimlik bilgileri, HDInsight kümesi için yapılandırdığınız SSH kimlik bilgileridir.

## <a name="troubleshoot"></a>Sorun giderme
Bkz. [Yükleme sorunlarını giderme](hdinsight-apps-install-custom-applications.md#troubleshoot-the-installation).

## <a name="next-steps"></a>Sonraki adımlar
* [Özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md): yayımlanmamış bir HDInsight uygulamasının HDInsight’a nasıl yükleneceğini öğrenin.
* [HDInsight uygulamalarını yayımlama](hdinsight-apps-publish-applications.md): Özel HDInsight uygulamalarınızı Azure Marketi’nde nasıl yayımlayacağınızı öğrenin.
* [MSDN: HDInsight uygulaması yükleme](https://msdn.microsoft.com/library/mt706515.aspx): HDInsight uygulamalarını nasıl tanımlayacağınızı öğrenin.
* [Betik Eylemi kullanarak Linux tabanlı HDInsight kümelerini özelleştirme](hdinsight-hadoop-customize-cluster-linux.md): ek uygulamalar yüklemek için Betik Eyleminin nasıl kullanılacağını öğrenin.
* [Resource Manager şablonları kullanarak HDInsight’ta Linux tabanlı Hadoop kümeleri oluşturma](hdinsight-hadoop-create-linux-clusters-arm-templates.md): HDInsight kümeleri oluşturmak için Resource Manager şablonlarının nasıl çağrılacağını öğrenin.
* [HDInsight’ta boş kenar düğümleri kullanma](hdinsight-apps-use-edge-node.md): HDInsight kümesine erişmek, HDInsight uygulamalarını test etmek ve HDInsight uygulamalarını barındırmak için boş bir kenar düğümünü kullanmayı öğrenin.




<!--HONumber=Nov16_HO4-->


