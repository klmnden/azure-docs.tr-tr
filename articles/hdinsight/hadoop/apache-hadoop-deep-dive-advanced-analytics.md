---
title: Derin Dalış - Analytics - Azure HDInsight Gelişmiş
description: Gelişmiş analizin nasıl bilgi büyük verileri işleme algoritmalarını kullanır.
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/14/2017
ms.author: ashishth
ms.openlocfilehash: ac0edf2de4337154b665b8f3898134a7c2fd1f4c
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62122073"
---
# <a name="deep-dive---advanced-analytics"></a>Derin Dalış - Gelişmiş analiz

## <a name="what-is-advanced-analytics-for-hdinsight"></a>Ne analizi için HDInsight Gelişmiş?

HDInsight, yapılandırılmış, yapılandırılmamış büyük miktarlardaki değerli Öngörüler ve hızlı ilerleyen veri alma olanağı sağlar. Gelişmiş analiz kullanımı yüksek oranda ölçeklenebilir mimarileri, istatistiksel ve makine öğrenimi modelleri ve akıllı Panolar ile anlamlı bilgiler sağlamak için ' dir. Machine learning, veya *Tahmine dayalı analiz*, tanımlamak ve tahminlerde bulunabilir ve kararlarınızı kılavuzu için veri ilişkileri öğrenmek algoritmaları kullanır.

## <a name="advanced-analytics-process"></a>Gelişmiş analiz işlem

![İşlem](./media/apache-hadoop-deep-dive-advanced-analytics/process.png)

İşinizdeki sorunu tespit ettik ve toplama başlatıldı, veri işleme, soru temsil eden bir model oluşturmak ihtiyacınız ve sonra tahmin etmek istiyor. Modelinizi, iş gereksinimlerinize en uygun tahmin türünü yapmak için bir veya daha fazla makine öğrenimi algoritmaları kullanır.  Verilerinizi çoğu, test etmek veya onu değerlendirmek için kullanılan rest ile modeli eğitmek için kullanılmalıdır. 

Oluşturma, yükleme, test ve modelinizi değerlendirmek sonra sonraki adım, sorularınıza yanıt sağlamaya başlar, modelinizi sağlamaktır. Son adım, modelinizin performansını izlemek ve gerektiği gibi ince oluşturmaktır. 

## <a name="common-types-of-algorithms"></a>Genel türleri algoritmaları

Gelişmiş analiz çözümleri, makine öğrenimi algoritmaları kümesi sağlar. Kategorilerini algoritmaları ve ilişkili ortak iş kullanımı örnekleri bir özeti aşağıda verilmiştir.

![Machine Learning kullanım örnekleri](./media/apache-hadoop-deep-dive-advanced-analytics/ml-use-cases.png)

En sığdırma algoritmalarından seçerek yanı sıra eğitim için veri sağlamanıza gerek olup olmadığını göz önünde bulundurmanız gerekir. Makine öğrenimi algoritmaları gibi kategorilere ayrılır:

* Denetimli - algoritması sonuçları sağlamadan önce etiketli veri kümesini temel düşünürler gerektiriyor
* Denetimli yarı - algoritması Genişletilebilir eğitim ilk aşaması sırasında kullanılamayan ek hedefler etkileşimli sorgu bir eğitmen tarafından aracılığıyla tarafından
* Denetimsiz - algoritması eğitim verileri gerektirmez 
* (Genellikle robotlara ilişkin kullanılır) belirli bir bağlam içinde ideal davranışını belirlemek için yazılımları aracıları pekiştirmeye - algoritması kullanır


| Algoritma kategorisi| Kullanım | Öğrenme türü | Algoritmalar |
| --- | --- | --- | -- |
| Sınıflandırma | Kişi veya öğeleri gruplar halinde sınıflandırma | Denetimli | Karar ağaçları, lojistik regresyon, karmaşık sinir ağları |
| Kümeleme | Örnekleri bir dizi homojen gruplar halinde bölme | Denetimsiz | K-ortalamaları kümeleme |
| Desen algılama | Verilerde sık ilişkileri tanımlama | Denetimsiz | İlişkilendirme kuralları |
| Regresyon | Sayısal sonuçlarını tahmin edin | Denetimli | Doğrusal regresyon, karmaşık sinir ağları |
| Pekiştirmeye | Robotlar için en iyi davranışı belirle | Pekiştirmeye | Monte Carlo Simülasyonları, DeepMind |

## <a name="machine-learning-on-hdinsight"></a>Machine learning üzerinde HDInsight

HDInsight öğrenme seçenekleri bir Gelişmiş analiz iş akışı için birden fazla makine vardır:

* Machine Learning ve Apache Spark
* R ve ML Hizmetleri
* Azure Machine Learning ve Apache Hive
* Apache Spark ve derin öğrenme

### <a name="machine-learning-and-apache-spark"></a>Machine Learning ve Apache Spark


[HDInsight Spark](../spark/apache-spark-overview.md) , Azure'da barındırılan bir teklifi [Apache Spark](https://spark.apache.org/), birleşik, açık kaynaklı, büyük veri analizi artırmak üzere bellek içi işleme kullanan bir paralel veri işleme çerçevesinden. Spark işleme altyapısı hız, kullanım kolaylığı ve Gelişmiş analiz için oluşturulmuştur. Spark'ın dağıtılmış bellek içi hesaplama özellikleri onu kullanılan makine öğrenimi ve grafik hesaplamalarında yinelemeli algoritmalar için iyi bir seçim haline getirir. 


Bu dağıtılmış ortamı algoritmik modelleme özellikleri getirin üç ölçeklenebilir makine öğrenimi kitaplıkları vardır:

* [**MLlib** ](https://spark.apache.org/docs/latest/ml-guide.html) -MLlib üzerinde Spark Rdd yerleşik özgün API içerir.
* [**SparkML** ](https://spark.apache.org/docs/1.2.2/ml-guide.html) -SparkML olan ML işlem hatları oluşturmak için Spark veri çerçevelerini üzerinde oluşturulan üst düzey bir API sağlayan yeni bir paket.
* [**MMLSpark** ](https://github.com/Azure/mmlspark) - Microsoft Machine Learning kitaplığı için Apache Spark (MMLSpark), veri bilimcileri deneme oranını artırmak için ve son teknoloji ürünü makine öğrenimi yararlanmak için Spark üzerinde daha üretken hale getirmek için tasarlanmıştır teknikleri, çok büyük veri kümeleri üzerinde derin öğrenme dahil. MMLSpark kitaplığı, PySpark modelleri oluşturmaya yönelik yaygın modelleme görevlerinizi basitleştirir. 

### <a name="r-and-ml-services"></a>R ve ML Hizmetleri

HDInsight'ın bir parçası olarak bir HDInsight kümesi ile oluşturabileceğiniz [ML Hizmetleri](../r-server/r-server-overview.md) modelleri ve yüksek hacimli veri kümeleri ile kullanılmaya hazır. Veri bilimcileri ve tanıdık bir R arabirimiyle ölçeklendirilebilir istatistikçiler bu yeni özellik sağlar. isteğe bağlı HDInsight, aracılığıyla küme kurulum ve Bakım yükü olmadan.

### <a name="azure-machine-learning-and-apache-hive"></a>Azure Machine Learning ve Apache Hive

[Azure Machine Learning Studio](https://studio.azureml.net/) Tahmine dayalı analiz modeli yanı sıra, Tahmine dayalı Modellerinizi-hazır web Hizmetleri olarak dağıtmada kullanabileceğiniz tam olarak yönetilen bir hizmet için araçlar sağlar. Azure Machine Learning, hızlı bir şekilde oluşturun, test, kullanıma hazır hale getirme ve Tahmine dayalı modelleri yönetmek için bulutta kapsamlı Tahmine dayalı analiz çözümleri oluşturmak için araçlar sağlar. Büyük bir algoritma kitaplığından tercih modelleri oluşturmak için bir web tabanlı studio kullanın ve modelinizi bir web hizmeti olarak kolayca dağıtın.

### <a name="apache-spark-and-deep-learning"></a>Apache Spark ve derin öğrenme

[Derin öğrenme](https://www.microsoft.com/research/group/dltc/) kullanan makine öğrenimi bir dalı *derin sinir ağı* (üst), İnsan beyninin Biyolojik işlemler tarafından ilham. Birçok Araştırmacıları, derin öğrenme yapay zeka için olası bir yaklaşım olarak bakın. Bazı derin öğrenme konuşulan dili çevirmenler ve görüntü tanıma sistemlerini makine mantık örnekleridir. Derin öğrenme kendi işlerinde ilerletmek için ücretsiz, kullanımı kolay, açık kaynak geliştirmiştir [Microsoft Bilişsel Araç Seti](https://www.microsoft.com/en-us/cognitive-toolkit/). Araç Seti, çok çeşitli Microsoft ürünlerinin, derin öğrenme uygun ölçekte dağıtma gereksinimi ile dünya çapında ve en son algoritmalar ve teknikler isteyen Öğrenciler tarafından yaygın olarak kullanılıyor. 

## <a name="scenario---score-images-to-identify-patterns-in-urban-development"></a>Senaryo - puanı görüntüleri Urban geliştirme desenleri tanımlamak için

HDInsight'ı kullanarak işlem hattı öğrenme bir Gelişmiş analiz makine örneği gözden geçirelim.

Bu senaryoda, derin öğrenme Framework'teki Dnn'leri üretme görürsünüz Microsoft Cognitive Toolkit (CNTK) kullanıcının, PySpark bir HDInsight Spark kümelerinde kullanarak bir Azure Blob Depolama hesabında depolanan büyük görüntü koleksiyonları Puanlama için kullanıma hazır hale getirdiniz. Bu yaklaşım, bir ortak DNN kullanım örneğine havadan görünüm sınıflandırması, uygulanır ve son urban geliştirme desenleri tanımlamak için kullanılabilir.  Önceden eğitilen bir görüntü sınıflandırma modeli kullanır. Model üzerinde önceden eğitilen [CIFAR 10 veri kümesi](https://www.cs.toronto.edu/~kriz/cifar.html) ve 10.000 tutulan görüntülere uygulanır.

Bu gelişmiş analiz senaryoda üç önemli görevler şunlardır:

1. Bir Apache Spark 2.1.0 dağıtımı olan Azure HDInsight Hadoop kümesi oluşturun. 
2. Microsoft Bilişsel araç seti, bir Azure HDInsight Spark kümesinin tüm düğümlerine yüklemek için özel bir betik çalıştırın. 
3. Önceden oluşturulmuş bir Jupyter Not Defteri, HDInsight Spark kümenize bir eğitilen Microsoft Bilişsel Araç Seti derin öğrenme modeli Spark Python API'si (PySpark) kullanarak bir Azure Blob Depolama hesabı içindeki dosyalara uygulamak için karşıya yükleyin. 

Bu örnek, derlenmiş ve dağıtılmış Alex Krizhevsky Vinod Nair ve Geoffrey Hinton tarafından ayarlanan CIFAR 10 görüntünün kullanır. 60.000 32 x 32 CIFAR 10 veri kümesini içeren 10 birbirini dışlayan sınıflarına ait görüntüleri rengi:

![Görüntüler](./media/apache-hadoop-deep-dive-advanced-analytics/ml-images.png)

Alex Krizhevsky'nın veri kümesi hakkında daha fazla bilgi için bkz. [öğrenme birden çok katman özellik çok küçük görüntülerinden](https://www.cs.toronto.edu/~kriz/learning-features-2009-TR.pdf).

Veri kümesi 50.000 görüntü Eğitim kümesi ve 10.000 görüntü test kümesi olarak bölümlenmiş olabilir. İlk kümenin izleyerek Microsoft Bilişsel Araç Seti kullanarak bir yirmi katman derin öğrenme kalan ağ (ResNet) modeli eğitmek için kullanılan [Bu öğreticide](https://github.com/Microsoft/CNTK/tree/master/Examples/Image/Classification/ResNet) Bilişsel Araç Seti GitHub deposundan. Kalan 10.000 görüntüleri modelin doğruluğunu test etmek için kullanıldı. Bu burada dağıtılmış bilgi işlem devreye giriyor: ön işleme ve görüntüleri Puanlama yüksek oranda paralelleştirilebilir görevdir. El ile olarak kaydedilmiş eğitilen modeli ile kullandık:

* Görüntüleri ve eğitilen model kümenin çalışan düğümlerine dağıtmak için PySpark.
* HDInsight Spark kümesinin tüm düğümlerine görüntülerinde önceden işlenecek Python'ı tıklatın.
* Model ve puanlama her düğümde önceden işlenmiş görüntü yüklemek için bilişsel Araç Seti'ı seçin.
* PySpark betiğini çalıştırmak için Jupyter not defterleri sonuçları toplama ve kullanma [Matplotlib](https://matplotlib.org/) model performansını görselleştirmek için.

Tüm ön işleme/Puanlama 10.000 görüntülerin 4 çalışan düğümleri ile bir kümede bir dakikadan az sürer. Model ~ 9,100 (%91) etiketlerin doğru tahmin görüntüler. Karışıklık matrisi, en yaygın sınıflandırma hataları gösterir. Örneğin, matris köpekler kediler olarak ve mislabeling oluştuğunu gösterir. birden çok sık diğer etiket çiftleri için.

![Sonuçlar](./media/apache-hadoop-deep-dive-advanced-analytics/ml-results.png)

### <a name="try-it-out"></a>Deneyin!

İzleyin [Bu öğreticide](../spark/apache-spark-microsoft-cognitive-toolkit.md) Bu çözüm için uçtan uca uygulamak için: bir HDInsight Spark kümesi kurulumu, Bilişsel Araç Seti yükleyin ve 10.000 CIFAR görüntüleri puanlar Jupyter not defteri çalıştırma.

## <a name="next-steps"></a>Sonraki adımlar

Apache Hive ve Azure Machine Learning

* [Apache Hive ve Azure Machine Learning için uçtan uca](../../machine-learning/team-data-science-process/hive-walkthrough.md)
* [1 TB veri kümesinde bir Azure HDInsight Hadoop kümesi kullanarak](../../machine-learning/team-data-science-process/hive-criteo-walkthrough.md)

Apache Spark ve MLLib

* [Machine learning ile HDInsight üzerinde Apache Spark](../../machine-learning/team-data-science-process/spark-overview.md)
* [Apache Spark Machine Learning ile: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight Apache Spark kullanma](../spark/apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark Machine Learning ile: Gıda denetimi sonuçlarını tahmin etmek için HDInsight, Apache Spark kullanma](../spark/apache-spark-machine-learning-mllib-ipython.md)

Derin öğrenme, Bilişsel Araç Seti ve diğerleri

* [Veri bilimi Azure sanal makinesi](../../machine-learning/data-science-virtual-machine/overview.md)
* [Azure HDInsight üzerinde H2O.ai ile tanışın](https://azure.microsoft.com/blog/introducing-h2o-ai-with-on-azure-hdinsight-to-bring-the-most-robust-ai-platform-for-enterprises/)
