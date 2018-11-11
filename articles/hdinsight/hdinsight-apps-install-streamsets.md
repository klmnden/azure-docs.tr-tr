---
title: Yayımlanan uygulama - StreamSets Data Collector - Azure HDInsight yükleme
description: Yükleyip StreamSets Data Collector üçüncü taraf Apache Hadoop uygulamasını kullanabilirsiniz.
services: hdinsight
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: ashish
ms.openlocfilehash: f963ae53e1396b1ef6279f2bd6502e5ab0cd23a1
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2018
ms.locfileid: "51034566"
---
# <a name="install-published-application---streamsets-data-collector"></a>Yayımlanan uygulama - StreamSets Data Collector yükleme

Bu makalede, yüklemek ve çalıştırmak açıklanır [StreamSets Data Collector HDInsight için](https://streamsets.com/) Azure HDInsight üzerinde Apache Hadoop uygulama yayımlanır. HDInsight uygulama platformu için genel bir bakış ve bir liste, kullanılabilen bağımsız yazılım satıcısı (ISV) için bkz. yayımlanan uygulamalar [üçüncü taraf Hadoop uygulamaları yükleme](hdinsight-apps-install-applications.md). Kendi uygulamanızı yükleme yönergeleri için bkz. [Özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md).

## <a name="about-streamsets-data-collector"></a>StreamSets Data Collector hakkında

StreamSets Data Collector, bir Azure HDInsight uygulamasının üzerinde dağıtılır. StreamSets Data Collector, tam özellikli tümleşik geliştirme ortamı (IDE alma işlem hatlarını tasarlamanıza, test edin, dağıtın ve herhangi bir ağdan herhangi yönetmenize olanak tanır) sağlar. Bu işlem hatları, akış ve toplu işlem verileri arasında ağ ve tüm özel kod yazmak zorunda kalmadan akış dönüşümleri çeşitli içerir.

StreamSets Data Collector, HDFS, Kafka, Solr, Hive, HBASE ve Kudu gibi çok sayıda büyük boyutlu veri bileşenlerini kullanarak bir derleme veri akışı sağlar. StreamSets Data Collector, Hadoop kümenizi veya bir uç sunucusu çalıştırıldıktan sonra gerçek zamanlı akış işlem veri anomalileri ve verileri için izleme alın. Bu izleme, eşik tabanlı uyarılar, anomali algılama ve otomatik düzeltme hata kayıtları içerir.

StreamSets Data Collector, işlem hattındaki her bir aşama kodlama içermeyen ve en düşük kapalı kalma süresi ile yeni işlemcilere ve bağlayıcılar bırakarak yeni işletme gereksinimlerinizi karşılayabilir şekilde mantıksal olarak ayırmak için tasarlanmıştır.

### <a name="streamsets-resource-links"></a>StreamSets kaynak bağlantıları

* [Belgeleri](https://streamsets.com/documentation/datacollector/latest/help/#Getting_Started/GettingStarted_Title.html)
* [Blog](https://streamsets.com/blog/)
* [Öğreticiler](https://github.com/streamsets/tutorials)
* [Geliştirici Destek Forumu](https://groups.google.com/a/streamsets.com/forum/#!forum/sdc-user)
* [Slack genel StreamSets kanal](https://streamsetters.slack.com/)
* [Kaynak kodu](https://github.com/streamsets)

## <a name="prerequisites"></a>Önkoşullar

Bu uygulamayı yeni bir HDInsight kümesi veya mevcut bir kümeye yüklemek için aşağıdaki yapılandırmaya sahip olmalıdır:

* Küme katmanları: standart veya Premium
* Küme sürümleri: 3.5 ve üzeri

## <a name="install-the-streamsets-data-collector-published-application"></a>Yayımlanan uygulamayı yükleme StreamSets Data Collector

Bu ve diğer kullanılabilir ISV uygulamalarını yükleme hakkında adım adım yönergeler için okuma [üçüncü taraf Hadoop uygulamaları yükleme](hdinsight-apps-install-applications.md).

## <a name="launch-streamsets-data-collector"></a>StreamSets Data Collector başlatın

1. Yükleme sonrasında, StreamSets Azure portalında kümenizden giderek başlatabilirsiniz **ayarları** seçip bölmesinde **uygulamaları** altında **genel** Kategori. Yüklü uygulamalar bölmesi, yüklü uygulamalar listelenir.

    ![Yüklü StreamSets uygulama](./media/hdinsight-apps-install-streamsets/streamsets.png)

2. StreamSets Data Collector'ı seçtiğinizde, web sayfası ve SSH uç noktası yolu için bir bağlantı görürsünüz. Web sayfası bağlantıyı seçin.

3. Oturum açma iletişim kutusunda, oturum açmak için aşağıdaki kimlik bilgilerini kullan: `admin` ve `admin`.

4. Başlarken sayfasında tıklayın **yeni işlem hattı oluşturma**.

    ![Yeni işlem hattı oluşturma](./media/hdinsight-apps-install-streamsets/get-started.png)

5. Yeni işlem hattı penceresinde, işlem hattı ("Merhaba Dünya") için bir ad girin, isteğe bağlı olarak bir açıklama girin ve seçin **Kaydet**.

6. Veri Toplayıcı konsolda gösterilir. Özellikler panelini ardışık düzen özelliklerini görüntüler.
 
    ![Veri Toplayıcı Konsolu](./media/hdinsight-apps-install-streamsets/pipeline-canvas.png)

7. Şimdi takip etmeye hazır olduğunuz [StreamSets öğretici](https://streamsets.com/documentation/datacollector/latest/help/#Tutorial/Tutorial-title.html). Öğretici, ilk işlem hattınızı oluşturmak için adım adım yönergeler sağlar.

## <a name="next-steps"></a>Sonraki adımlar

* [StreamSets Data Collector belgeleri](https://streamsets.com/documentation/datacollector/latest/help/#Getting_Started/GettingStarted_Title.html#concept_htw_ghg_jq).
* [Özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md): HDInsight için yayımlanmamış bir HDInsight uygulamasının nasıl dağıtılacağını öğrenin.
* [HDInsight uygulamalarını yayımlama](hdinsight-apps-publish-applications.md): Özel HDInsight uygulamalarınızı Azure Marketi’nde nasıl yayımlayacağınızı öğrenin.
* [MSDN: HDInsight uygulaması yükleme](https://msdn.microsoft.com/library/mt706515.aspx): HDInsight uygulamalarını nasıl tanımlayacağınızı öğrenin.
* [Betik eylemi kullanarak Linux tabanlı HDInsight kümelerini özelleştirin](hdinsight-hadoop-customize-cluster-linux.md): ek uygulamalar yüklemek için betik eylemi kullanmayı öğrenin.
* [HDInsight içinde boş kenar düğümlerini kullanma](hdinsight-apps-use-edge-node.md): HDInsight kümeleri erişmek ve test etmek ve HDInsight uygulamalarını barındırmak için boş bir kenar düğümünü kullanmayı öğrenin.
