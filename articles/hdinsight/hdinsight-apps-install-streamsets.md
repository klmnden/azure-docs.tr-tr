---
title: Yükleme yayımlanan uygulama - StreamSets veri toplayıcı - Azure Hdınsight | Microsoft Docs
description: Yükleyin ve StreamSets veri toplayıcı üçüncü taraf Hadoop uygulama kullanın.
services: hdinsight
documentationcenter: ''
author: ashishthaps
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: ashish
ms.openlocfilehash: e433de82576f8b943988881ed0b6673c0dccd77e
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="install-published-application---streamsets-data-collector"></a>Yayımlanan uygulama - StreamSets veri toplayıcı yükleme

Bu makalede yüklemek ve çalıştırmak nasıl [StreamSets veri toplayıcı Hdınsight için](https://streamsets.com/) Azure hdınsight'ta Hadoop uygulama yayımlanır. Hdınsight uygulaması platformu genel bir bakış ve bir liste, kullanılabilen bağımsız yazılım satıcısı (ISV) için yayımlanan uygulamalar bkz [üçüncü taraf Hadoop uygulamaları yükleme](hdinsight-apps-install-applications.md). Kendi uygulamanızı yükleme yönergeleri için bkz. [Özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md).

## <a name="about-streamsets-data-collector"></a>StreamSets veri toplayıcı hakkında

StreamSets veri toplayıcı üzerinde Azure Hdınsight uygulaması dağıtır. StreamSets veri toplayıcı tam özellikli tümleşik geliştirme ortamı (IDE, tasarlamak, test, dağıtma ve any herhangi yönetmenizi ardışık düzen alma) sağlar. Bu ardışık düzen akış ve toplu veri kafes ve tüm özel kod yazmak zorunda kalmadan akış dönüşümleri, çeşitli içerir.

StreamSets veri toplayıcı HDFS, Kafka, Solr, Hive, HBASE ve Kudu gibi çok sayıda büyük veri bileşenleri'ni kullanarak yapı veri akışları sağlar. StreamSets veri toplayıcı bir uç sunucusunda veya Hadoop kümenizdeki çalışmaya başladıktan sonra verileri daha fazla bilgi ve veriler için akış işlemlerini izleme gerçek zamanlı alın. Bu izleme, eşik tabanlı uyarı, anomali algılama ve hata kayıtların otomatik düzeltme içerir.

StreamSets veri toplayıcı yeni işlemciler ve bağlayıcıları kodlama olmadan ve en düşük kapalı kalma bırakarak yeni iş gereksinimlerini karşılayabilen şekilde mantıksal olarak bir ardışık düzende her aşama yalıtmak için tasarlanmıştır.

### <a name="streamsets-resource-links"></a>StreamSets kaynak bağlantıları

* [Belgeleri](https://streamsets.com/documentation/datacollector/latest/help/#Getting_Started/GettingStarted_Title.html)
* [Blog](https://streamsets.com/blog/)
* [Öğreticiler](https://github.com/streamsets/tutorials)
* [Geliştirici Destek Forumu](https://groups.google.com/a/streamsets.com/forum/#!forum/sdc-user)
* [Kayma ortak StreamSets kanal](https://streamsetters.slack.com/)
* [Kaynak kodu](https://github.com/streamsets)

## <a name="prerequisites"></a>Önkoşullar

Yeni bir Hdınsight kümesi veya varolan bir kümenin bu uygulamayı yüklemek için aşağıdaki yapılandırmaya sahip olmalısınız:

* Küme tier(s): standart veya Premium
* Küme sürüm(ler): 3.5 ve üstü

## <a name="install-the-streamsets-data-collector-published-application"></a>Uygulama yükleme StreamSets veri toplayıcı yayımlanır

Bu ve diğer kullanılabilir ISV uygulamaları yükleme hakkında adım adım yönergeler için okuma [üçüncü taraf Hadoop uygulamaları yükleme](hdinsight-apps-install-applications.md).

## <a name="launch-streamsets-data-collector"></a>StreamSets veri toplayıcı başlatma

1. Yükleme sonrasında, StreamSets Azure portalında, kümeden giderek başlatabilirsiniz **ayarları** bölmesinde, ardından seçerek **uygulamaları** altında **genel** Kategori. Yüklü uygulamalar bölmesinde yüklü uygulamalar listelenir.

    ![Yüklü StreamSets uygulama](./media/hdinsight-apps-install-streamsets/streamsets.png)

2. StreamSets veri toplayıcı seçtiğinizde, bir web sayfası ve SSH uç noktası yolu bağlantısına bakın. Web sayfası bağlantıyı seçin.

3. Oturum açma iletişim kutusunda, oturum açmak için şu kimlik bilgilerini kullanın: `admin` ve `admin`.

4. Başlarken sayfasında, tıklatın **yeni işlem hattı oluşturmak**.

    ![Yeni işlem hattı oluşturma](./media/hdinsight-apps-install-streamsets/get-started.png)

5. Yeni işlem hattı penceresinde, ardışık düzen ("Hello World") için bir ad girin, isteğe bağlı olarak bir açıklama girin ve seçin **kaydetmek**.

6. Veri Toplayıcı konsol gösterilir. Özellikleri paneli ardışık düzen özelliklerini görüntüler.
 
    ![Veri Toplayıcı konsol](./media/hdinsight-apps-install-streamsets/pipeline-canvas.png)

7. İzlemek artık hazırsınız [StreamSets Öğreticisi](https://streamsets.com/documentation/datacollector/latest/help/#Tutorial/Tutorial-title.html). Öğreticinin ilk işlem hattınızı oluşturma için adım adım yönergeler sağlar.

## <a name="next-steps"></a>Sonraki adımlar

* [StreamSets veri toplayıcı belgelerine](https://streamsets.com/documentation/datacollector/latest/help/#Getting_Started/GettingStarted_Title.html#concept_htw_ghg_jq).
* [Özel Hdınsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md): yayımlanmamış bir Hdınsight uygulamasının Hdınsight'a nasıl yükleneceğini öğrenin.
* [HDInsight uygulamalarını yayımlama](hdinsight-apps-publish-applications.md): Özel HDInsight uygulamalarınızı Azure Marketi’nde nasıl yayımlayacağınızı öğrenin.
* [MSDN: HDInsight uygulaması yükleme](https://msdn.microsoft.com/library/mt706515.aspx): HDInsight uygulamalarını nasıl tanımlayacağınızı öğrenin.
* [Betik eylemi kullanarak Linux tabanlı Hdınsight kümelerini özelleştirme](hdinsight-hadoop-customize-cluster-linux.md): ek uygulamalar yüklemek için betik eylemi kullanmayı öğrenin.
* [Hdınsight'ta boş kenar düğümünü kullanmak](hdinsight-apps-use-edge-node.md): Hdınsight kümeleri erişmek ve test etmek ve Hdınsight uygulamalarını barındırmak için bir boş kenar düğümüne kullanmayı öğrenin.
