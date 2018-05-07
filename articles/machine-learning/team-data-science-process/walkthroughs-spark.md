---
title: Azure üzerinde PySpark ve Scala kullanarak Hdınsight Spark izlenecek | Microsoft Docs
description: PySpark ve Scala kullanımı ile bir Azure Hdınsight Tahmine dayalı analiz gerçekleştirmede Spark üzerinde yol örnekleri takım veri bilimi işleminin.
services: machine-learning
documentationcenter: ''
author: deguhath
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2017
ms.author: deguhath
ms.openlocfilehash: 06498f55b38be98441ddceb8f1667c89d257f8fd
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="hdinsight-spark-data-science-walkthroughs-using-pyspark-and-scala-on-azure"></a>Hdınsight Spark veri bilimi Azure üzerinde PySpark ve Scala kullanarak izlenecek yollar

Bu izlenecek yollar Tahmine dayalı analiz gerçekleştirmek için bir Azure Spark kümesinde PySpark ve Scala kullanın. Bunlar takım veri bilimi işleminde açıklanan adımları izleyin. Takım veri bilimi işlemine genel bakış için bkz: [veri bilimi işlemi](overview.md). Hdınsight'ta Spark genel bakış için bkz: [hdınsight'ta Spark giriş](../../hdinsight/spark/apache-spark-overview.md).

Takım veri bilimi işlemi yürütmek ek veri bilimi talimatlara göre gruplandırılır **platform** kullandıkları. Bkz: [takım veri bilimi işlemi yürütülürken izlenecek yollar](walkthroughs.md) bir döküm Bu örnekler için.

## <a name="predict-taxi-tips-using-pyspark-on-azure-spark"></a>PySpark Azure Spark kullanarak ücreti ipuçları tahmin etme

[Azure hdınsight'ta Spark kullanma](spark-overview.md) izlenecek bir ipucu Ücretli ve tutarlar aralığını ödenmesi beklenen tahmin etmek için New York taksiler verileri kullanır. Senaryo kullanarak takım veri bilimi işlemi kullanan bir [Azure Hdınsight Spark küme](https://azure.microsoft.com/services/hdinsight/) depolamak için keşfetmek, özelliği genel kullanıma açık NYC ücreti seyahat mühendislik verileri ve veri kümesi masrafları. Bu genel bakış konusunun, Yukarı Hdınsight Spark kümesinde Jupyter PySpark ile not defterlerini Kılavuzu geri kalanı kullanılan ayarlar. Bu dizüstü bilgisayarlar, verilerinizi keşfedin ve ardından oluşturmak ve modelleri kullanmak gösterir. Gelişmiş Veri keşfi ve modelleme Not Defteri, çapraz doğrulama, hyper-yerleştirmez, parametre içerir ve değerlendirme modeli gösterilmektedir.

### <a name="data-exploration-and-modeling-with-spark"></a>Veri keşfi ve Spark modelleme 
Dataset keşfetmek ve oluşturmak, Puanlama ve modelleri ile çalışarak öğrenme makine değerlendirmek [Spark Mllib'i araç seti ile veri için ikili sınıflandırma ve regresyon model oluşturma](spark-data-exploration-modeling.md) konu.

### <a name="model-consumption"></a>Model tüketim
Bu konuda oluşturulan sınıflandırma ve regresyon modeli Puanlama öğrenmek için bkz: [puanı ve Spark yerleşik makine öğrenimi modellerini değerlendirme](spark-model-consumption.md).

### <a name="cross-validation-and-hyperparameter-sweeping"></a>Çapraz doğrulama ve hyperparameter Süpürme
Bkz: [veri keşfi ve modelleme Spark ile Gelişmiş](spark-advanced-data-exploration-modeling.md) modelleri nasıl olabilir üzerinde çapraz doğrulama ve parametre hyper Süpürme kullanılarak eğitilmiş.


## <a name="predict-taxi-tips-using-scala-on-azure-spark"></a>Azure Spark Scala kullanarak ücreti ipuçları tahmin etme

[Azure üzerinde Spark ile kullanım Scala](scala-walkthrough.md) izlenecek bir ipucu Ücretli ve tutarlar aralığını ödenmesi beklenen tahmin etmek için New York taksiler verileri kullanır. Denetimli makine görevleri kitaplığı (Mllib'i) ve Azure Hdınsight Spark kümesinde SparkML paketleri öğrenme Spark makine öğrenimi için Scala kullanmayı gösterir. Oluşturduğunu görevlerinde anlatılmaktadır [veri bilimi işlemi](http://aka.ms/datascienceprocess): veri alımı ve keşfi, görselleştirme, özellik Mühendisliği, model ve model tüketim. Modellerinin oluşturulması Lojistik ve doğrusal regresyon, rastgele ormanları ve gradyan boosted ağaçları içerir.


## <a name="next-steps"></a>Sonraki adımlar

Takım veri bilimi işlemi oluşturan anahtar bileşenleri bir tartışma için bkz: [takım veri bilimi işlemine genel bakış](overview.md).

Bir veri bilimi projelerinizi yapısı için kullanabileceğiniz takım veri bilimi işlemi yaşam döngüsü tartışma için bkz: [takım veri bilimi işlemi yaşam döngüsü](lifecycle.md). Yaşam döngüsü başlangıçtan bitişe kadar bunlar çalıştırıldığında projeleri genellikle izlemeniz gereken adımları özetler. 

