---
title: Yayımlanan uygulama - yükleme H2O Sparkling Water - Azure HDInsight | Microsoft Docs
description: Yükleyip H2O Sparkling Water üçüncü taraf Hadoop uygulamasını kullanabilirsiniz.
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
ms.openlocfilehash: e3c80fe824d87c15a710b133c8e6cddf4ee0e096
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37856564"
---
# <a name="install-published-application---h2o-sparkling-water"></a>Yayımlanan uygulama - H2O Sparkling Water yükleme

Bu makalede, yüklemek ve çalıştırmak açıklanır [H20 Sparkling Water](http://www.h2o.ai/) Azure HDInsight Hadoop uygulamasında yayımlanan. HDInsight uygulama platformu için genel bir bakış ve bir liste, kullanılabilen bağımsız yazılım satıcısı (ISV) için bkz. yayımlanan uygulamalar [üçüncü taraf Hadoop uygulamaları yükleme](hdinsight-apps-install-applications.md). Kendi uygulamanızı yükleme yönergeleri için bkz. [Özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md).

## <a name="about-h2o-sparkling-water"></a>H2O Sparkling Water hakkında

H2O Sparkling Water, açık kaynaklı, doğrusal ölçeklenebilirlik ile tam olarak dağıtılmış bellek içi makine öğrenimi platformudur ' dir. H2O Sparkling Water, hızlı ve ölçeklenebilir makine öğrenimi algoritmalarını Spark'ın özellikleriyle H2O birleştirmek sağlar. Sparkling Water ile kullanıcılar, Scala, R ve Python H2O Flow kullanıcı arabirimini kullanarak hesaplama yönlendirebilirsiniz.

H2O Sparkling Water sağlar:

* **Kullanımı kolay WebUI ve tanıdık arabirimleri** – belirlenen ayarlama ve hızlı bir şekilde ya da H2O'un sezgisel web tabanlı akış GUI kullanarak veya ortamlar R, Python, Java, Scala, JSON ve H2O API'leri gibi programlama kullanmaya başlayabilirsiniz.
* **Tüm ortak veritabanı ve dosya türleri için veri geçişte sorun yaşamaz Destek** – kolayca keşfedin ve Microsoft Excel, R Studio, Tableau ve daha büyük verilerden model. HDFS, S3, SQL ve NoSQL veri kaynaklarından alınan verilere bağlanın.
* **Büyük ölçüde ölçeklenebilir bozma büyük veri ve analiz** – H2O büyük birleştirmeler 7 x R data.table işlemleri daha hızlı bir şekilde gerçekleştirmek ve doğrusal olarak 10 milyar x 10 milyar satır birleştirmeler Ölçeklendir.
* **Gerçek zamanlı veri Puanlama** – hızlı bir şekilde modelleri düz eski Java nesnelerini (POJO'ya) modeli için iyileştirilmiş Java nesnelerini (MOJO) kullanarak üretime dağıtma veya H2O REST API.

### <a name="resource-links"></a>Kaynak bağlantıları

* [H2O.ai mühendislik yol haritası](http://jira.h2o.ai/)
* [H2O.ai giriş](http://www.h2o.ai/)
* [H2O.ai belgeleri](http://docs.h2o.ai/)
* [H2O.ai desteği](https://support.h2o.ai/)
* [H2O.ai açık kaynak kod temeli](https://github.com/h2oai/)

## <a name="prerequisites"></a>Önkoşullar

Bu uygulamayı yeni bir HDInsight kümesi veya mevcut bir kümeye yüklemek için aşağıdaki yapılandırmaya sahip olmalıdır:

* Küme katmanları: standart veya Premium
* Küme türü: Spark
* Küme sürümleri: 3.5 veya 3.6

## <a name="install-the-h2o-sparkling-water-published-application"></a>Yayımlanan uygulamayı yükleme H2O Sparkling Water

Bu ve diğer kullanılabilir ISV uygulamalarını yükleme hakkında adım adım yönergeler için okuma [üçüncü taraf Hadoop uygulamaları yükleme](hdinsight-apps-install-applications.md).

## <a name="launch-h2o-sparkling-water"></a>H2O Sparkling Water başlatın

1. Yükleme sonrasında, H2O Sparkling Water (sparklingwater h2o) kullanarak Azure portalında kümenizin Jupyter not defterleri açarak başlamak için kullanabileceğiniz (`https://<ClusterName>.azurehdinsight.net/jupyter`). Jupyter için seçerek de sahip olabilirsiniz **küme Panosu** sonra portalda, küme bölmesinden seçerek **Jupyter not defteri**. Kimlik bilgilerinizi girmeniz istenir. Küme oluşturma sırasında belirtilen kümenin Hadoop kimlik bilgilerini girin.

2. Jupyter'de, üç klasör görürsünüz: H2O PySparkling örnekler, örnekler PySpark ve Scala örnekleri. Seçin **H2O PySparkling örnekler** klasör.

    ![Jupyter not defterleri giriş](./media/hdinsight-apps-install-h2o/jupyter-home.png)

3. Yeni bir not defteri oluştururken ilk adım, Spark ortam yapılandırmaktır. Bu bilgiler dahil edilir **Sentiment_analysis_with_Sparkling_Water** örnek. Spark ortamını yapılandırma, doğru jar kullanın ve ilk hücrenin çıktı tarafından sağlanan IP adresini belirtin emin olun.

    ![Jupyter not defterleri giriş](./media/hdinsight-apps-install-h2o/spark-config.png)

4. H2O kümeyi başlatın.

    ![Kümeyi başlatın](./media/hdinsight-apps-install-h2o/start-cluster.png)

5. H2O küme çalışır duruma geldikten sonra H2O akış giderek açın **`https://<ClusterName>-h2o.apps.azurehdinsight.net:443`**.

    > [!NOTE]
    > H2O akış açık tarayıcı önbelleğinizi temizlemeyi deneyin. Yine oluşturulamıyor ulaşmak büyük olasılıkla yeterli kaynakları kümenizde erişiminiz yok. Altında çalışan düğümleri sayısını artırmayı deneyin **ölçek kümesi** kümesi bölmenizi seçeneği.

    ![H2O Flow Panosu](./media/hdinsight-apps-install-h2o/h2o-flow.png)

6. Seçin **Million_Songs.flow** sağdaki menüden örnek. Bir uyarı ile sorulduğunda **yük not defteri**. Bu Tanıtım, gerçek verileri kullanarak birkaç dakika içinde çalışacak şekilde tasarlanmıştır. Hedef şarkıyı önce veya sonra 2004 yayımlanmış olan verilerden tahmin etmektir ikili sınıflandırma kullanma.

    ![Million_Songs.Flow seçin](./media/hdinsight-apps-install-h2o/million-songs.png)

7. Yolu içeren Bul **milsongs cls train.csv.gz**ve tüm yoluyla değiştirin **https://h2o-public-test-data.s3.amazonaws.com/bigdata/laptop/milsongs/milsongs-cls-train.csv.gz**.

8. Yolu içeren Bul **milsongs cls test.csv.gz** değiştirin **https://h2o-public-test-data.s3.amazonaws.com/bigdata/laptop/milsongs/milsongs-cls-test.csv.gz**.

9. Not Defteri hücreleri içinde tüm deyimleri yürütmek için seçin **tümünü Çalıştır** araç çubuğunda.

    ![Tümünü Çalıştır](./media/hdinsight-apps-install-h2o/run-all.png)

10. Birkaç dakika sonra aşağıdakine benzer bir çıktı görmeniz gerekir.

    ![Çıktı](./media/hdinsight-apps-install-h2o/output.png)

İşte bu kadar! Dakikalar içinde yapay zeka Spark harnessed. Şimdi farklı makine öğrenimi algoritması türleri gösteren daha fazla örneklerde H2O akış keşfedebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [H2O belgeleri](http://docs.h2o.ai/h2o/latest-stable/h2o-docs/index.html)
* [Özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md): HDInsight için yayımlanmamış bir HDInsight uygulamasının nasıl dağıtılacağını öğrenin.
* [HDInsight uygulamalarını yayımlama](hdinsight-apps-publish-applications.md): Özel HDInsight uygulamalarınızı Azure Marketi’nde nasıl yayımlayacağınızı öğrenin.
* [MSDN: HDInsight uygulaması yükleme](https://msdn.microsoft.com/library/mt706515.aspx): HDInsight uygulamalarını nasıl tanımlayacağınızı öğrenin.
* [Betik eylemi kullanarak Linux tabanlı HDInsight kümelerini özelleştirin](hdinsight-hadoop-customize-cluster-linux.md): ek uygulamalar yüklemek için betik eylemi kullanmayı öğrenin.
* [HDInsight içinde boş kenar düğümlerini kullanma](hdinsight-apps-use-edge-node.md): HDInsight kümeleri erişmek ve test etmek ve HDInsight uygulamalarını barındırmak için boş bir kenar düğümünü kullanmayı öğrenin.
