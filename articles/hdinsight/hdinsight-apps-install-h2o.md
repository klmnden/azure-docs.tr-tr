---
title: Yükleme yayımlanan uygulama - H2O Sparkling su - Azure Hdınsight | Microsoft Docs
description: Yükleyin ve H2O Sparkling su üçüncü taraf Hadoop uygulama kullanın.
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
ms.openlocfilehash: 9a03588b3327c3ab231f5c2cae17488f4d63bde7
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31402116"
---
# <a name="install-published-application---h2o-sparkling-water"></a>Yayımlanan uygulama - H2O Sparkling su yükleyin

Bu makalede yüklemek ve çalıştırmak nasıl [H20 Sparkling su](http://www.h2o.ai/) Azure hdınsight'ta Hadoop uygulama yayımlanır. Hdınsight uygulaması platformu genel bir bakış ve bir liste, kullanılabilen bağımsız yazılım satıcısı (ISV) için yayımlanan uygulamalar bkz [üçüncü taraf Hadoop uygulamaları yükleme](hdinsight-apps-install-applications.md). Kendi uygulamanızı yükleme yönergeleri için bkz. [Özel HDInsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md).

## <a name="about-h2o-sparkling-water"></a>H2O Sparkling su hakkında

H2O Sparkling su bir açık, tam olarak dağıtılmış bellek içi machine learning doğrusal ölçeklenebilirlik platformuyla kaynağıdır. H2O Sparkling su, hızlı ve ölçeklenebilir makine öğrenimi Spark yeteneklerini H2O algoritmalarıyla birleştirmek olanak tanır. Sparkling su ile kullanıcılar hesaplama Scala, R ve Python akış H2O UI kullanarak sürücü.

H2O Sparkling su sağlar:

* **Kullanımı kolay WebUI ve tanıdık arabirimleri** – ayarlanmış yukarı ve hızlı bir şekilde ya da H2O's sezgisel web tabanlı akış GUI'yi kullanarak veya ortamlar R, Python, Java, Scala, JSON ve H2O API'leri gibi programlama başlayın.
* **Tüm ortak veritabanı ve dosya türleri için veri belirsiz Destek** – kolayca keşfedin ve Microsoft Excel, R Studio, Tableau ve daha fazla içindeki büyük verileri model. HDFS, S3, SQL ve NoSQL veri kaynaklarından verilere bağlanın.
* **Yüksek düzeyde ölçeklenebilir büyük veri munging ve analiz** – H2O büyük birleştirmeler 7 x R data.table işlemleri daha hızlı bir şekilde gerçekleştirmek ve 10 milyon satır birleştirmeler x 10 milyon için doğrusal olarak ölçeklendirin.
* **Gerçek zamanlı veri Puanlama** – hızlı bir şekilde modelleri düz eski Java nesnelerini (POJO'ya) modeli iyileştirilmiş Java nesnelerini (MOJO) kullanarak üretime dağıtabilirsiniz veya H2O REST API.

### <a name="resource-links"></a>Kaynak bağlantıları

* [H2O.ai mühendislik yol haritası](https://jira.h2o.ai/)
* [H2O.ai giriş](http://www.h2o.ai/)
* [H2O.ai belgeleri](http://docs.h2o.ai/)
* [H2O.ai desteği](https://support.h2o.ai/)
* [H2O.ai açık kaynak Codebase](https://github.com/h2oai/)

## <a name="prerequisites"></a>Önkoşullar

Yeni bir Hdınsight kümesi veya varolan bir kümenin bu uygulamayı yüklemek için aşağıdaki yapılandırmaya sahip olmalısınız:

* Küme tier(s): standart veya Premium
* Küme türü: Spark
* Küme sürüm(ler): 3.5 veya 3.6

## <a name="install-the-h2o-sparkling-water-published-application"></a>Uygulama yükleme H2O Sparkling su yayımlanır

Bu ve diğer kullanılabilir ISV uygulamaları yükleme hakkında adım adım yönergeler için okuma [üçüncü taraf Hadoop uygulamaları yükleme](hdinsight-apps-install-applications.md).

## <a name="launch-h2o-sparkling-water"></a>H2O Sparkling su başlatma

1. Yükleme sonrasında, H2O Sparkling su (h2o-sparklingwater) kullanarak Azure portalında, kümeden Jupyter not defterleri açarak başlatabilirsiniz (`https://<ClusterName>.azurehdinsight.net/jupyter`). Seçerek Jupyter için alabilirsiniz **küme Panosu** Portalı'nda, küme bölmesinden seçilerek **Jupyter not defteri**. Kimlik bilgilerinizi girmeniz istenir. Küme oluşturma sırasında belirtilen kümenin Hadoop kimlik bilgilerini girin.

2. Jupyter'de, üç klasörleri görürsünüz: H2O PySparkling örnekler, PySpark örnekler ve Scala örnekler. Seçin **H2O PySparkling örnekler** klasör.

    ![Jupyter not defterleri giriş](./media/hdinsight-apps-install-h2o/jupyter-home.png)

3. Spark ortamını yapılandırmak için yeni bir not defteri oluştururken ilk adım olacaktır. Bu bilgiler dahil edilir **Sentiment_analysis_with_Sparkling_Water** örnek. Spark ortamını yapılandırırken, ilk hücrenin çıktı tarafından sağlanan IP adresi belirtin ve doğru jar kullanın emin olun.

    ![Jupyter not defterleri giriş](./media/hdinsight-apps-install-h2o/spark-config.png)

4. H2O küme başlatın.

    ![Küme Başlat](./media/hdinsight-apps-install-h2o/start-cluster.png)

5. H2O küme hazır ve çalışır sonra H2O akış giderek açmak **`https://<ClusterName>-h2o.apps.azurehdinsight.net:443`**.

    > [!NOTE]
    > H2O akış açın, tarayıcı önbelleği temizledikten deneyin. Devam ediyorsanız, ulaşmak için büyük olasılıkla yeterli kaynak kümenizde yok. Altında çalışan düğümü sayısını artırmayı deneyin **ölçekte küme** küme bölmesinde seçeneği.

    ![Pano H2O akış](./media/hdinsight-apps-install-h2o/h2o-flow.png)

6. Seçin **Million_Songs.flow** sağ taraftaki menüden örnek. Bir uyarı ile istendiğinde tıklatın **yük dizüstü**. Bu demo gerçek verileri kullanarak birkaç dakika içinde çalışmak üzere tasarlanmıştır. Hedeftir şarkıyı önce veya sonra 2004 yayımlanmış olan verilerden tahmin etmek için ikili sınıflandırma kullanma.

    ![Million_Songs.Flow seçin](./media/hdinsight-apps-install-h2o/million-songs.png)

7. Yolu içeren Bul **milsongs cls train.csv.gz**ve yerine tüm yolu **https://h2o-public-test-data.s3.amazonaws.com/bigdata/laptop/milsongs/milsongs-cls-train.csv.gz**.

8. Yolu içeren Bul **milsongs cls test.csv.gz** ve bunların yerine **https://h2o-public-test-data.s3.amazonaws.com/bigdata/laptop/milsongs/milsongs-cls-test.csv.gz**.

9. Not Defteri hücrelerdeki tüm deyimlerini yürütmek için seçin **tümünü Çalıştır** araç çubuğunda.

    ![Tümünü Çalıştır](./media/hdinsight-apps-install-h2o/run-all.png)

10. Birkaç dakika sonra aşağıdakine benzer bir çıktı görmeniz gerekir.

    ![Çıktı](./media/hdinsight-apps-install-h2o/output.png)

İşte bu kadar! Birkaç dakika içinde Spark yapay zeka harnessed. Machine learning algoritmaları farklı türlerde gösteren daha fazla örneklerde H2O akış şimdi keşfedebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [H2O belgeleri](http://docs.h2o.ai/h2o/latest-stable/h2o-docs/index.html)
* [Özel Hdınsight uygulamaları yükleme](hdinsight-apps-install-custom-applications.md): yayımlanmamış bir Hdınsight uygulamasının Hdınsight'a nasıl yükleneceğini öğrenin.
* [HDInsight uygulamalarını yayımlama](hdinsight-apps-publish-applications.md): Özel HDInsight uygulamalarınızı Azure Marketi’nde nasıl yayımlayacağınızı öğrenin.
* [MSDN: HDInsight uygulaması yükleme](https://msdn.microsoft.com/library/mt706515.aspx): HDInsight uygulamalarını nasıl tanımlayacağınızı öğrenin.
* [Betik eylemi kullanarak Linux tabanlı Hdınsight kümelerini özelleştirme](hdinsight-hadoop-customize-cluster-linux.md): ek uygulamalar yüklemek için betik eylemi kullanmayı öğrenin.
* [Hdınsight'ta boş kenar düğümünü kullanmak](hdinsight-apps-use-edge-node.md): Hdınsight kümeleri erişmek ve test etmek ve Hdınsight uygulamalarını barındırmak için bir boş kenar düğümüne kullanmayı öğrenin.
