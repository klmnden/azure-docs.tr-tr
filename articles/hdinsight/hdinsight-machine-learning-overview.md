---
title: "Machine learning genel bakış - Azure Hdınsight | Microsoft Docs"
description: "Makine öğrenimi hdınsight'ta seçenekleri açıklar."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: nitinme
ms.openlocfilehash: ff99a7a60573cad5e6dd30d4ca48903423e9f87f
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2018
---
# <a name="machine-learning-on-hdinsight"></a>Machine learning Hdınsight'ta

Hdınsight, büyük veri, büyük miktarlarda yapılandırılmış ve yapılandırılmamış, (Petabayt ve hatta eksabayt) değerli bilgiler elde etme yeteneği sağlamak ve hızlı taşıma verilerle machine learning sağlar. Hdınsight'ta machine learning birkaç seçenek vardır: SparkML ve Mllib'i, R, Hive ve Microsoft Bilişsel Araç Seti.

## <a name="sparkml-and-mllib"></a>SparkML ve Mllib'i

[Hdınsight Spark](spark/apache-spark-overview.md) bir Azure barındırılan sunulması olan [Spark](http://spark.apache.org/), birleştirilmiş, açık kaynaklı, büyük veri analizi artırmak üzere bellek içi işlemeyi destekleyen paralel veri işleme altyapısıdır. Spark işleme altyapısı hızı, kullanımı kolay, gelişmiş analizler için yerleşik olarak bulunur. Spark'ın bellek içi dağıtılmış hesaplama özellikleri onu kullanılan machine learning ve grafik hesaplamalarında yinelemeli algoritmalar için iyi bir seçimdir haline getirir. Bu dağıtılmış ortamına algoritmik modelleme yetenekleri Getir iki ölçeklenebilir machine learning kitaplığı: Mllib'i ve SparkML. En üstünde RDDs yerleşik özgün API Mllib'i içerir. SparkML DataFrames üstünde ML işlem hatlarını oluşturmak için yerleşik yüksek düzeyli bir API sağlayan yeni bir pakettir. SparkML tüm Mllib'i özelliklerini henüz olarak desteklemez, ancak Mllib'i Spark'ın standart machine learning kitaplığı değiştiriyor.

Apache Spark için Microsoft Machine Learning Kitaplığı [MMLSpark](https://github.com/Azure/mmlspark). Bu kitaplık Spark veri bilimcilerine daha verimli hale, deneme oranını artırmak ve modern machine learning teknikler, çok büyük veri kümeleri üzerinde derin öğrenme dahil olmak üzere yararlanmak için tasarlanmıştır. MMLSpark dizin dizeler gibi ölçeklenebilir ML modeller oluştururken bir katmanı SparkML'ın alt düzey API üstünde zorlama learning algoritmaları ve özellik vektörlerinin atayarak makine tarafından beklenen bir düzen halinde veri sağlar. Bunlar ve diğer ortak görevler PySpark model oluşturmaya yönelik MMLSpark kitaplığı basitleştirir.

## <a name="r"></a>R

[R](https://www.r-project.org/) şu anda dünyanın en popüler istatistiksel programlama dilinde olduğu. Bu bir açık kaynak veri görselleştirme 2.5 milyondan kullanıcıları ve büyüyen topluluğuyla aracıdır. Temel thriving kullanıcı ve üzerinde 8.000 revizyonlarınız paketleri ile R machine learning isteyen birçok şirket için büyük olasılıkla bir seçimdir. R Server büyük veri kümelerine ve modelleri ile kullanılmaya hazır olan bir Hdınsight kümesi oluşturabilirsiniz. Bu özellik veri bilimcileri ve istatistikçiler ölçeklendirebilirsiniz bilinen bir R arabirimi sağlar-isteğe bağlı Hdınsight, aracılığıyla küme kurulum ve Bakım yükü olmadan.

![R server ile tahmin için eğitim](./media/hdinsight-machine-learning-overview/r-training.png)

Kümenin kenar düğümü kümeye bağlanın ve R komut dosyalarını çalıştırmak için uygun bir yer sağlar.  Ayrıca ScaleR'ın eşlemesi Hadoop azaltmak kullanarak küme düğümleri arasında R betiklerini çalıştırmak seçeneğiniz vardır veya Spark işlem bağlamı.

Spark ile hdınsight'ta R Server ile eğitim küme düğümleri arasında bir Spark işlem bağlamını kullanarak paralel hale. Tüm kullanılabilir çekirdekler paralel olarak gerektiği gibi kullanarak doğrudan kenar düğümde R betiklerini çalıştırabilirsiniz. Alternatif olarak, kümedeki tüm düğümler arasında dağıtılmış işlem dışı kazandırın için sınır düğümünden kodunuzu çalıştırabilirsiniz. Spark ile hdınsight'ta R Server parallelizing işlevleri açık kaynak R paketlerinden, isterseniz de sağlar.

## <a name="azure-machine-learning-and-hive"></a>Azure Machine Learning ve Hive

Azure Machine Learning, Tahmine dayalı Modellerinizi tüketen hazır web Hizmetleri olarak dağıtmak için kullanabileceğiniz tam olarak yönetilen bir hizmet yanı sıra, model Tahmine dayalı analiz araçları sağlar. Azure Machine Learning, tamamlanmış Tahmine dayalı analiz çözümü oluşturmak, sınamak, faaliyete ve Tahmine dayalı modelleri yönetmek için kullanabileceğiniz bulut. Büyük bir algoritma kitaplığından seçin, bir web tabanlı studio modeli oluşturmak için kullanabilir ve kolayca modelinizi bir web hizmeti olarak dağıtabilirsiniz.

![Gelişmiş analizler erişilebilir Microsoft Azure Machine Learning ile hadoop'a yapma](./media/hdinsight-machine-learning-overview/hadoop-azure-ml.png)

Kullanarak bir Hdınsight Hadoop veri kümesi için özellikler oluşturmak [Hive sorguları](../machine-learning/team-data-science-process/create-features-hive.md). *Özellik Mühendisliği* öğrenme sürecini hızlandırmanız ham verilerden özellikler oluşturarak algoritmaları öğrenme Tahmine dayalı güç artırmak çalışır. Azure ML HiveQL sorguları çalıştırmak ve erişim kovanında işlenir ve kullanarak blob storage'da depolanan verileri [veri içeri aktarma modülü](../machine-learning/studio/import-data.md).

## <a name="microsoft-cognitive-toolkit"></a>Microsoft Bilişsel Araç Seti

[Derin öğrenme](https://www.microsoft.com/en-us/research/group/dltc/) dalının sinir ağları, İnsan beyin Biyolojik işlemler tarafından esin kullanan makine öğrenme. Birçok araştırmacılarının derin öğrenme yapay zeka geliştirme için taahhüdü bir yaklaşım olarak bakın. Derin öğrenme konuşulan dile çevirmenler, görüntü tanıma sistemleri ve makine mantığı gösterilebilir.

Derin öğrenme kendi işlerinde ilerletmek yardımcı olmak için ücretsiz ve kolay kullanımlı, açık kaynak Microsoft geliştirilen [Microsoft Bilişsel Araç Seti](https://www.microsoft.com/en-us/cognitive-toolkit/). Bu araç seti, çok çeşitli Microsoft ürünleri, şirketler dünya çapında ölçekte derin öğrenme dağıtmak için sahip ve en son algoritmalar ve teknikler ilgilenen Öğrenciler tarafından kullanılıyor. 

## <a name="see-also"></a>Ayrıca bkz.

### <a name="scenarios"></a>Senaryolar

* [Machine Learning ile Spark: HVAC verilerini kullanarak bina sıcaklığını çözümlemek için HDInsight’ta Spark kullanma](spark/apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Spark: Yemek inceleme sonuçlarını tahmin etmek için HDInsight’ta Spark kullanma](spark/apache-spark-machine-learning-mllib-ipython.md)
* [Mahout ile film önerileri oluşturma](hadoop/apache-hadoop-mahout-linux-mac.md)
* [Hive ve Azure Machine Learning](../machine-learning/team-data-science-process/create-features-hive.md)
* [Hive ve Azure Machine Learning uçtan uca](../machine-learning/team-data-science-process/hive-walkthrough.md)
* [Makine hdınsight'ta Spark ile öğrenme](../machine-learning/team-data-science-process/spark-overview.md)

### <a name="deep-learning-resources"></a>Derin öğrenme kaynakları

* [Spark ile derin öğrenme Araç Seti](https://blogs.technet.microsoft.com/machinelearning/2017/04/25/using-microsofts-deep-learning-toolkit-with-spark-on-azure-hdinsight-clusters/)
* [Bilişsel Araç Seti + Spark üzerinde Tensorflow utandırıcı derecede paralel görüntü sınıflandırma](https://blogs.technet.microsoft.com/machinelearning/2017/04/12/embarrassingly-parallel-image-classification-using-cognitive-toolkit-tensorflow-on-azure-hdinsight-spark/)
