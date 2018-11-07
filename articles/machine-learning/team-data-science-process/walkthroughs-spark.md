---
title: PySpark ve Scala Azure'da kullanarak HDInsight Spark izlenecek yollar | Microsoft Docs
description: PySpark ve Scala kullanarak bir Azure HDInsight Tahmine dayalı analiz gerçekleştirmede Spark yol Team Data Science Process örnekleri.
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2017
ms.author: deguhath
ms.openlocfilehash: 3d39e752e874b6b0c6fbdf678d6eddda0b0d9404
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51226557"
---
# <a name="hdinsight-spark-data-science-walkthroughs-using-pyspark-and-scala-on-azure"></a>PySpark ve Scala Azure'da kullanarak HDInsight Spark veri bilimi kılavuzları

Bu izlenecek yollar PySpark ve Scala, Tahmine dayalı analiz yapmak için bir Azure Spark kümesinde kullanın. Bunlar, Team Data Science Process içinde verilen adımları izleyin. Team Data Science Process genel bakış için bkz: [Data Science Process](overview.md). HDInsight üzerinde Spark genel bakış için bkz. [HDInsight üzerinde Spark giriş](../../hdinsight/spark/apache-spark-overview.md).

Team Data Science Process yürütme ek veri bilimi kılavuzları tarafından gruplanır **platform** kullandıkları. Bkz: [Team Data Science Process yürütme izlenecek yollar](walkthroughs.md) Bu örneklerde bir döküm için.

## <a name="predict-taxi-tips-using-pyspark-on-azure-spark"></a>PySpark Azure Spark kullanarak taksi bahşişlerini tahmin etme

[Azure HDInsight üzerinde Spark kullanma](spark-overview.md) izlenecek yol, bir ipucu Ücretli ve ödenecek tutarlar aralığını beklenen tahmin etmek için New York taksiler verileri kullanır. Team Data Science Process kullanarak bir senaryo kullanan bir [Azure HDInsight Spark kümesi](https://azure.microsoft.com/services/hdinsight/) depolamak için keşfetmek, özelliği genel kullanıma açık NYC taksi seyahat mühendisi verileri ve veri kümesi masrafları. Bu genel bakış konusunda sizi bir HDInsight Spark kümesinde Jupyter PySpark ile not defterlerini gözden geçirme geri kalanında kullanılan ayarlar. Bu not defterlerini nasıl oluşturarak verilerinizi araştırmanıza ve nasıl oluşturulup tüketim modelleri görüntüleyin. Gelişmiş Veri keşfi ve modelleme Not Defteri, çapraz doğrulama, hyper-Süpürme saldırısı yapılabilir, parametre eklemek ve değerlendirme modeli gösterilmektedir.

### <a name="data-exploration-and-modeling-with-spark"></a>Spark ile veri keşfi ve modelleme 
Veri kümesini araştırmak ve oluşturma, Puanlama ve makine öğrenimi modellerini aracılığıyla değerlendirmek [Spark MLlib araç seti ile veriler için ikili sınıflandırma ve regresyon modellerini oluşturma](spark-data-exploration-modeling.md) konu.

### <a name="model-consumption"></a>Model tüketim
Bu konu başlığında oluşturduğunuz sınıflandırma ve regresyon modellerini puanlamanıza öğrenmek için bkz. [puanı ve Spark'a yerleşik machine learning modellerini değerlendirme](spark-model-consumption.md).

### <a name="cross-validation-and-hyperparameter-sweeping"></a>Çapraz doğrulama ve hiper parametre Süpürme
Bkz: [Gelişmiş Veri keşfi ve modelleme Spark ile](spark-advanced-data-exploration-modeling.md) modelleri nasıl olabileceğini üzerinde çapraz doğrulama ve hiper parametreli Süpürme kullanarak eğitim.


## <a name="predict-taxi-tips-using-scala-on-azure-spark"></a>Azure üzerinde Spark Scala kullanarak taksi bahşişlerini tahmin etme

[Kullanım Scala ile Spark azure'da](scala-walkthrough.md) izlenecek yol, bir ipucu Ücretli ve ödenecek tutarlar aralığını beklenen tahmin etmek için New York taksiler verileri kullanır. Bu, Scala ile Spark makine öğrenimi kitaplığı (MLlib) ve bir Azure HDInsight Spark kümesi üzerinde SparkML paketleri görevleri öğrenme denetimli makine kullanmak nasıl gösterir. Oluşturan görevlerinde size yol gösterir [Data Science Process](https://aka.ms/datascienceprocess): veri alımı ve keşfi, görselleştirme, özellik Mühendisliği, modelleme ve model tüketim. Mantıksal ve doğrusal regresyon, rastgele ormanları ve gradyan artırmalı ağaçları modellerinin içerir.


## <a name="next-steps"></a>Sonraki adımlar

Team Data Science Process oluşturan anahtar bileşenleri bir tartışma için bkz [Team Data Science Process genel bakış](overview.md).

Veri bilimi projelerinizi yapısı için kullanabileceğiniz Team Data Science Process yaşam döngüsü için bkz [Team Data Science Process yaşam döngüsü](lifecycle.md). Yaşam döngüsü, başlangıçtan bitişe kadar bunlar yürütüldüğünde projeleri genellikle izlemeniz adımlarını özetler. 

