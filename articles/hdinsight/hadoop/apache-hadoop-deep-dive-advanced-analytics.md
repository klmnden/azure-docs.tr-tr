---
title: Derin Dalış - Analytics'i - Azure Hdınsight Gelişmiş | Microsoft Docs
description: Nasıl gelişmiş analizler öğrenin büyük verileri işlemek için algoritmalarını kullanır.
services: hdinsight
documentationcenter: ''
author: ashishthaps
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: ashishth
ms.openlocfilehash: ea6ffa9d07be719c43ca33cfca76876c161d69bc
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37048480"
---
# <a name="deep-dive---advanced-analytics"></a>Derin Dalış - Analytics'i Gelişmiş

## <a name="what-is-advanced-analytics-for-hdinsight"></a>Ne analytics Hdınsight için Gelişmiş?

Hdınsight yapılandırılmış ve yapılandırılmamış büyük miktarda gelen çok değerli bilgiler ve hızlı taşıma verileri elde etme yeteneği sağlar. Gelişmiş analizler yüksek düzeyde ölçeklenebilir mimari, istatistiksel kullanımını ve makine öğrenimi modellerini ve ile anlamlı bilgiler sağlamak için akıllı panolar adıdır. Machine learning, veya *Tahmine dayalı analiz*, tanımlamak ve verilerinizi tahminlerde ve kararlarınızı kılavuzuna ilişkilerde öğrenmek algoritmaları kullanır.

## <a name="advanced-analytics-process"></a>Gelişmiş analizler işlemi

![İşlem](./media/apache-hadoop-deep-dive-advanced-analytics/process.png)

İş sorununu tanımladıktan ve toplama başlamış olması ve verilerinizi işleme, soru temsil eden bir model oluşturmak ihtiyacınız sonra tahmin etmek istiyor. Modelinizi, iş gereksinimlerinize en uygun tahmin türü yapmak için bir veya daha fazla makine öğrenimi algoritmalarını kullanır.  Verilerinizi çoğunluğu test etmek veya değerlendirmek için kullanılan rest ile modeli eğitmek için kullanılmalıdır. 

Oluşturmak, yükleme, test ve modelinizi değerlendirmek sonra sonraki adıma böylece sorularınızın yanıtlarını sağlamaya başlar modelinizi dağıtmaktır. Modelinizin performansını izlemek ve gerektiği şekilde ayarlamak için son adım olacaktır. 

## <a name="common-types-of-algorithms"></a>Algoritma genel türleri

Gelişmiş analiz çözümleri machine learning algoritmaları kümesi sağlar. Algoritmaları ve ilişkili ortak iş kullanımı durumlarını kategorilerini bir özeti aşağıda verilmiştir.

![Machine Learning kullanım örnekleri](./media/apache-hadoop-deep-dive-advanced-analytics/ml-use-cases.png)

En iyi sığdırma algoritmalarından seçerek yanı sıra, verileri eğitim gerek olmadığını dikkate almanız gerekir. Machine learning algoritmaları aşağıdaki gibi kategorilere ayrılır:

* Denetimli - sonuçları sağlamadan önce etiketli veri kümesini temel Eğitilecek algoritması gerektiriyor
* Denetimli yarı - algoritması Genişletilebilir eğitim ilk aşaması sırasında kullanılamayan ek hedefleri etkileşimli sorgu bir eğitmen ile tarafından
* Denetimsiz - algoritması verileri eğitim gerektirmez 
* (Genellikle robotics içinde kullanılan) belirli bir bağlam içinde ideal davranışını belirlemek için yazılım aracılarının öğrenmeyi - algoritması kullanır


| Algoritma kategorisi| Kullanım | Öğrenme türü | Algoritmalar |
| --- | --- | --- | -- |
| Sınıflandırma | Kişi ya da şeyler gruplar halinde sınıflandırın | Denetimli | Karar ağaçları, lojistik regresyon, sinir ağları |
| Kümeleme | Örnekler kümesini homojen gruplar halinde bölme | Denetimsiz | K-ortalamaları kümeleme |
| Desen algılama | Verilerde sık ilişkileri tanımlayın | Denetimsiz | İlişkilendirme kuralları |
| Regresyon | Sayısal sonuçlar tahmin etme | Denetimli | Doğrusal regresyon, sinir ağları |
| Öğrenmeyi | Robots için en iyi davranışı belirle | Öğrenmeyi | Monte Carlo benzetimleri, DeepMind |

## <a name="machine-learning-on-hdinsight"></a>Machine learning Hdınsight'ta

Hdınsight birkaç makine öğrenme Gelişmiş analytics iş akışı için seçenekleri sahiptir:

* [Machine Learning ve Spark](#machine-learning-and-spark)
* [R ve ML Hizmetleri](#r-and-r-server)
* [Azure Machine Learning ve Hive](#azure-machine-learning-and-hive)
* [Spark ve derin öğrenme](#spark-and-deep-learning)

### <a name="machine-learning-and-spark"></a>Machine Learning ve Spark

[Hdınsight Spark](../spark/apache-spark-overview.md) bir Azure barındırılan sunulması olan [Spark](http://spark.apache.org/), birleştirilmiş, açık kaynaklı, büyük veri analizi artırmak üzere bellek içi işleme kullanan veri bir paralel işleme altyapısıdır. Spark işleme altyapısı hızı, kullanımı kolay, gelişmiş analizler için yerleşik olarak bulunur. Spark'ın bellek içi dağıtılmış hesaplama özellikleri onu kullanılan machine learning ve grafik hesaplamalarında yinelemeli algoritmalar için iyi bir seçimdir haline getirir. 

Bu dağıtılmış ortamına algoritmik modelleme yetenekleri Getir üç ölçeklenebilir makine öğrenme kitaplıkları vardır:

* [**Mllib'i** ](https://spark.apache.org/docs/latest/ml-guide.html) -Mllib'i üzerinde Spark RDDs yerleşik özgün API içerir.
* [**SparkML** ](https://spark.apache.org/docs/1.2.2/ml-guide.html) -SparkML olduğundan üzerinde Spark DataFrames ML işlem hatlarını oluşturmak için yerleşik yüksek düzeyli bir API sağlar daha yeni bir paket.
* [**MMLSpark** ](https://github.com/Azure/mmlspark) - Microsoft makine Apache Spark (MMLSpark) veri bilimcilerine Spark, deneme oranını artırmak ve modern machine learning yararlanacağınızı daha verimli hale getirmek için tasarlanmıştır için kitaplık öğrenme teknikler, çok büyük veri kümeleri üzerinde derin öğrenme dahil olmak üzere. MMLSpark kitaplığı PySpark model oluşturmaya yönelik ortak modelleme görevleri basitleştirir. 

### <a name="r-and-ml-services"></a>R ve ML Hizmetleri

Hdınsight bir parçası olarak, bir Hdınsight kümesini oluşturabilirsiniz [ML Hizmetleri](../r-server/r-server-overview.md) büyük veri kümelerine ve modelleri ile kullanılmak üzere hazır. Bu yeni özellik veri bilimcileri ve istatistikçiler ölçeklendirebilirsiniz bilinen bir R arabirim sağlayan isteğe bağlı Hdınsight, aracılığıyla küme kurulum ve Bakım yükü olmadan.

### <a name="azure-machine-learning-and-hive"></a>Azure Machine Learning ve Hive

[Azure Machine Learning Studio](https://studio.azureml.net/) modeli tahmine dayalı analiz yanı sıra, Tahmine dayalı Modellerinizi tüketen hazır web Hizmetleri olarak dağıtmak için kullanabileceğiniz tam olarak yönetilen bir hizmet için araçlar sağlar. Azure Machine Learning, tamamlanmış Tahmine dayalı analiz çözümleri hızlıca oluşturun, test, faaliyete ve Tahmine dayalı modelleri yönetmek için bulutta oluşturmak için araçlar sağlar. Büyük bir algoritma kitaplığından seçin, bir web tabanlı studio modeli oluşturmak için kullanabilir ve kolayca modelinizi bir web hizmeti olarak dağıtabilirsiniz.

### <a name="spark-and-deep-learning"></a>Spark ve derin öğrenme

[Derin öğrenme](https://www.microsoft.com/research/group/dltc/) kullanan machine learning dalı olan *derin sinir ağları* (DNNs) İnsan beyin Biyolojik işlemler tarafından neden olacak. Birçok araştırmacılarının derin öğrenme taahhüdü bir yaklaşım için yapay zeka olarak bakın. Bazı derin öğrenme konuşulan dile çevirmenler, görüntü tanıma sistemleri ve makine mantığı gösterilebilir. Derin öğrenme kendi işlerinde ilerletmek yardımcı olmak için ücretsiz ve kolay kullanımlı, açık kaynaklı geliştirmiştir [Microsoft Bilişsel Araç Seti](https://www.microsoft.com/cognitive-toolkit/). Araç Seti yaygın çok çeşitli Microsoft ürünleri, şirketler dünya çapında ölçekte derin öğrenme dağıtmak için sahip ve en son algoritmalar ve teknikler ilgilenen Öğrenciler tarafından kullanılıyor. 

## <a name="scenario---score-images-to-identify-patterns-in-urban-development"></a>Senaryo - Kentsel geliştirme düzenleri tanımlamak için puan görüntüleri

Şimdi bir gelişmiş analizler makine Hdınsight kullanarak işlem hattı öğrenme örneği gözden geçirin.

Bu senaryoda, nasıl derin öğrenme Framework'te DNNs üretilen görürsünüz Microsoft Bilişsel Araç Seti (CNTK) kullanıcının, bir Hdınsight Spark kümesinde PySpark kullanarak bir Azure Blob Storage hesabında depolanan büyük görüntü koleksiyonları Puanlama operationalized. Bu yaklaşım, bir ortak DNN kullanım örneği olan havadan görüntü sınıflandırma için uygulanır ve Kentsel geliştirme son düzenleri tanımlamak için kullanılabilir.  Önceden eğitilen görüntü sınıflandırma modeli kullanır. Model üzerinde önceden eğitilen [CIFAR 10 dataset](https://www.cs.toronto.edu/~kriz/cifar.html) ve 10.000 tutulan görüntülere uygulanır.

Bu gelişmiş analizler senaryoda üç önemli görevler şunlardır:

1. Bir Azure Hdınsight Hadoop kümesi ile bir Apache Spark 2.1.0 dağıtım oluşturun. 
2. Microsoft Bilişsel araç seti bir Azure Hdınsight Spark kümesinin tüm düğümlerine yüklemek için özel bir komut dosyasını çalıştırın. 
3. Önceden derlenmiş Jupyter not defteri bir eğitilen Microsoft Bilişsel Araç Seti Spark Python API (PySpark) kullanarak bir Azure Blob Storage hesabı dosyalarında modeline öğrenme derin uygulamak için Hdınsight Spark kümenize karşıya yükleyin. 

Bu örnek derlenmiş ve dağıtılmış Alex Krizhevsky, Vinod Nair ve Geoffrey Hinton tarafından ayarlanan CIFAR 10 görüntünün kullanır. 60.000 32 x 32 CIFAR 10 veri kümesi içerdiğinden renk 10 birbirini dışlayan sınıflarına ait görüntüler:

![Görüntüler](./media/apache-hadoop-deep-dive-advanced-analytics/ml-images.png)

Alex Krizhevsky'nin veri kümesini hakkında daha fazla bilgi için bkz [öğrenme birden çok Katmanlar özelliklerin küçük görüntülerden](https://www.cs.toronto.edu/~kriz/learning-features-2009-TR.pdf).

Veri kümesi, bir sınama kümesi 10.000 görüntülerinin ve 50.000 görüntüleri Eğitim kümesi bölümlenmiş olabilir. İlk küme izleyerek Microsoft Bilişsel Araç Seti kullanarak yirmi katman derin convolutional kalan ağ (ResNet) modeli eğitmek için kullanılan [Bu öğretici](https://github.com/Microsoft/CNTK/tree/master/Examples/Image/Classification/ResNet) Bilişsel Araç Seti Github'da depodan. Kalan 10.000 görüntüleri modelin doğruluğunu test etmek için kullanılmıştır. Dağıtılmış bilgi işlem oyuna nereden geldiğini budur: önceden işlenmesi ve görüntüleri Puanlama yüksek oranda paralelleştirilebilir bir görevdir. Elle içinde kaydedilmiş eğitilen modeli ile kullandık:

* Kümenin çalışan düğümlerine eğitilen model ve görüntüleri dağıtmak için PySpark.
* Hdınsight Spark kümesinin her bir düğümüne görüntülerinde önceden işlemek için Python'ı tıklatın.
* Araç Seti, modeli ve puanı her düğüm üzerinde önceden işlenmiş yansımaları yüklemek için bilişsel.
* PySpark komut dosyasını çalıştırmak için Jupyter Not sonuçları toplama ve kullanma [Matplotlib](https://matplotlib.org/) modeli performans görselleştirmek için.

Tüm ön işleme/Puanlama 10.000 görüntülerinin 4 çalışan düğümleri ile bir kümede bir dakikadan az alır. Model ~ 9,100 (%91) görüntüleri etiketlerinin doğru şekilde tahmin eder. Karışıklığı önlemek için matris en yaygın sınıflandırma hataları gösterilmektedir. Örneğin, matris köpekler kediler olarak ve mislabeling oluştuğunu gösterir. birden çok sık diğer etiket çiftleri için.

![Sonuçlar](./media/apache-hadoop-deep-dive-advanced-analytics/ml-results.png)

### <a name="try-it-out"></a>Deneyin!

İzleyin [Bu öğretici](../spark/apache-spark-microsoft-cognitive-toolkit.md) Bu çözüm uçtan uca uygulanacak: Hdınsight Spark Küme kurulumu için Bilişsel Araç Seti yükleyip 10.000 CIFAR görüntüleri puanlar Jupyter not defteri çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

Hive ve Azure Machine Learning

* [Hive ve Azure Machine Learning uçtan uca](../../machine-learning/team-data-science-process/hive-walkthrough.md)
* [1 TB veri kümesinde Azure Hdınsight Hadoop kümesi kullanma](../../machine-learning/team-data-science-process/hive-criteo-walkthrough.md)

Spark ve Mllib'i

* [Makine hdınsight'ta Spark ile öğrenme](../../machine-learning/team-data-science-process/spark-overview.md)
* [Machine Learning ile Spark: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight’ta Spark kullanma](../spark/apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Spark: Yemek inceleme sonuçlarını tahmin etmek için HDInsight’ta Spark kullanma](../spark/apache-spark-machine-learning-mllib-ipython.md)

Derin öğrenme, Bilişsel Araç Seti ve diğerleri

* [Bilişsel Araç Seti ve TensorFlow Azure Hdınsight Spark kullanarak utandırıcı derecede paralel görüntü sınıflandırma](https://blogs.technet.microsoft.com/machinelearning/2017/04/12/embarrassingly-parallel-image-classification-using-cognitive-toolkit-tensorflow-on-azure-hdinsight-spark/)
* [Veri bilimi Azure sanal makinesi](../../machine-learning/data-science-virtual-machine/overview.md)
* [Azure hdınsight'ta H2O.ai Tanıtımı](https://azure.microsoft.com/blog/introducing-h2o-ai-with-on-azure-hdinsight-to-bring-the-most-robust-ai-platform-for-enterprises/)
