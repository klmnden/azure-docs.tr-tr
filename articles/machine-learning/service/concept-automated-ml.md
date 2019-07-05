---
title: ML nedir otomatik / automl
titleSuffix: Azure Machine Learning service
description: Nasıl Azure Machine Learning hizmeti otomatik olarak sizin için bir algoritma seçme ve ondan size zaman sağlamak için ölçütleri ve parametreleri kullanarak kaydetmek için bir model oluşturmak öğrenin modeliniz için en iyi algoritmayı seçin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: nacharya1
ms.author: nilesha
ms.date: 06/20/2019
ms.custom: seodec18
ms.openlocfilehash: 137ef1ad78548053f3c5b8f30b7d83f2370f62da
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67442429"
---
# <a name="what-is-automated-machine-learning"></a>Nedir, makine öğrenimi otomatik?

Otomatik machine Learning, autoML da bilinir, makine öğrenimi modeli geliştirme zaman alıcı ve yinelemeli görevleri otomatikleştirme işlemidir. Veri uzmanları, analistler ve geliştiricilerin tüm model kalitesi dayanıklılık geçtikten çalışırken ile yüksek ölçek, verimliliğini ve üretkenliğini ML modelleri oluşturup izin verir.

Kaynak Kullanımı Yoğun, önemli bir etki alanı bilgisini ve üretmek ve modelleri onlarca karşılaştırmak için zaman gerektiren geleneksel makine öğrenme modeli geliştirme. Eğitme ve modeli sizin için belirttiğiniz hedef ölçüm kullanarak ayarlamak için Azure Machine Learning istediğinizde otomatik ML uygulayın. Hizmet ardından özellik seçimleri, burada her yinelemenin bir eğitim puan modeliyle üretir ile eşleştirilmiş ML algoritmaları aracılığıyla yinelenir. Daha yüksek puanı, daha iyi modeli "verilerinizi uygun" kabul edilir.

Otomatik machine learning ile üretime hazır ML model harika kolaylık ve verimlilik elde süresini hızlandırın.

## <a name="when-to-use-automated-ml"></a>ML otomatik ne zaman kullanılır?

Otomatik ML, makine öğrenimi modeli geliştirme sürecine ıot'yi herkesin kullanımına sunan ve herhangi bir sorun için bir uçtan uca makine öğrenimi işlem hattı tanımlamak için kullanıcılar, veri bilimi uzmanlıklarını ne olursa olsun güçlendirir.

Veri uzmanları ve analistleri sektörlerden geliştiriciler için otomatik ML kullanabilirsiniz:

+ Kapsamlı bir programlama bilgisi olmadan makine öğrenimi çözümleri uygulayın
+ Zamandan ve kaynaklardan tasarruf
+ Veri bilimi en iyi yararlanın
+ Çevik sorun giderme sağlayın

## <a name="how-automated-ml-works"></a>Nasıl otomatik ML çalışır

Kullanarak **Azure Machine Learning hizmeti**, tasarım ve otomatik ML eğitim denemelerinizi ile aşağıdaki adımları çalıştırın:

1. **ML tanımlamaya** çözülecek: Sınıflandırma, tahmin veya regresyon

1. **Kaynak ve etiketli bir eğitim veri biçimi belirtmek**: Numpy diziler veya Pandas dataframe

1. **Model eğitimi için işlem hedefini yapılandırın**, gibi [yerel bilgisayar, Azure Machine Learning hesaplar, uzaktan sanal makineleri veya Azure Databricks](how-to-set-up-training-targets.md).  Otomatik eğitim hakkında bilgi edinin [uzak kaynak](how-to-auto-train-remote.md).

1. **Yapılandırma parametreleri öğrenme otomatik makine** farklı modelleri, Hiper parametre ayarları üzerinden kaç adet yineleme ön işleme/özellik kazandırma sayesinde Gelişmiş ve en iyi modeli belirlerken bakmak için hangi ölçümleri belirler.  Otomatik eğitim denemesini ayarlarını yapılandırabilirsiniz [Azure portalında](how-to-create-portal-experiments.md) veya [SDK'sı ile](how-to-configure-auto-train.md).

1. **Çalıştırma eğitim gönderin.**

  ![Otomatik makine öğrenimi](./media/how-to-automated-ml/automl-concept-diagram2.png)

Eğitim sırasında Azure Machine Learning hizmeti farklı algoritmaları ve parametrelerle deneyin paralel işlem hatları bir dizi oluşturur. Deneme içinde tanımlanan çıkış ölçütlerini İsabetleri sonra durdurulur.

Günlüğe kaydedilen çalışma bilgileri, ayrıca inceleyebilirsiniz hangi [ölçümleri içeren](how-to-understand-accuracy-metrics.md) çalıştırma sırasında toplanan. Python serileştirilmiş nesne eğitim çalıştırmanın (`.pkl` dosyası) modeli ve veri ön işleme içerir.

Model oluşturmanın otomatik karşın, şunları da yapabilirsiniz. [nasıl önemli veya ilgili özellikleri olan](how-to-configure-auto-train.md#explain) oluşturulmuş modelleri.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2Xc9t]

<a name="preprocess"></a>

## <a name="preprocessing"></a>Ön işleme

Deneme her otomatik makine öğrenimi, verilerinizi varsayılan yöntemleri kullanılarak ve isteğe bağlı olarak Gelişmiş ön işleme önceden işlenmiş.

### <a name="automatic-preprocessing-standard"></a>Otomatik (standart) ön işleme

Deneme her otomatik makine öğrenimi, verilerinizi otomatik olarak ölçeği veya iyi gerçekleştirilip algoritmaları yardımcı olmak için normalleştirilmiş.  Her model için model eğitim sırasında ölçeklendirme veya normalleştirme aşağıdaki tekniklerden birini uygulanır.

|Ölçeklendirme&nbsp;&&nbsp;normalleştirme| Açıklama |
| ------------- | ------------- |
| [StandardScaleWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html)  | Ortalama kaldırarak ve birim varyansı için ölçeklendirme özellikleri standart hale getirin  |
| [MinMaxScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html)  | Her bir özellik bu sütunun minimum ve maksimum tarafından ölçeklendirerek özellikleri dönüştürür  |
| [MaxAbsScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MaxAbsScaler.html#sklearn.preprocessing.MaxAbsScaler) |Her bir özellik tarafından en yüksek mutlak değeri ölçeklendirin |
| [RobustScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.RobustScaler.html) |Bu Scaler tarafından quantile menzil özellikleri |
| [PCA](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html) |Doğrusal boyut düzeyi azaltma için daha düşük bir boyutlu alanı proje için verilerin tekil değer ayrıştırma kullanma |
| [TruncatedSVDWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.TruncatedSVD.html) |Bu dönüştürücü doğrusal boyut düzeyi azaltma kesilmiş tekil değer ayrıştırma (SVD) yoluyla gerçekleştirir. PCA aykırı bu tahmin veri tekil değer ayrıştırma bilgi işlem önce Merkezi değil. Bu scipy.sparse matrisleri ile verimli bir şekilde çalışabilir anlamına gelir. |
| [SparseNormalizer](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.Normalizer.html) | Böylece kendi norm (l1 veya l2) bir eşittir en az bir sıfır olmayan bileşeni ile her örnek (diğer bir deyişle, her satır veri matris) bağımsız olarak diğer örnekleri yeniden Ölçeklendirildi |

### <a name="advanced-preprocessing-optional-featurization"></a>Ön işleme Gelişmiş: isteğe bağlı özellik kazandırma sayesinde

Ayrıca ek Gelişmiş ön işleme ve özellik kazandırma sayesinde değerleri imputation, kodlama ve dönüştürmeler eksik gibi kullanılabilir. [Hangi özellik kazandırma sayesinde dahildir hakkında daha fazla bilgi](how-to-create-portal-experiments.md#preprocess). Bu ayarı etkinleştirin:

+ Azure portalı: Seçme **ön işleme** onay kutusu **Gelişmiş ayarlar** [adımlarıyla](how-to-create-portal-experiments.md).

+ Python SDK: Belirtme `"preprocess": True` için [ `AutoMLConfig` sınıfı](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig?view=azure-ml-py).


## <a name="time-series-forecasting"></a>Zaman serisi tahmin etme
Tahminlerini oluşturmak, gelir, Envanter, satış ve müşteri isteğe bağlı olup olmadığını, herhangi bir iş bütünleyici özelliğidir. Otomatik ML teknikleri ve yaklaşımları birleştirin ve bir önerilen, yüksek kaliteli zaman tahmin serisi almak için kullanabilirsiniz. 

Otomatik bir zaman serisi deneme çok değişkenli regresyon problemi kabul edilir. Son zaman serisi değerleri "diğer adaylarının birlikte üzerine regressor için ek boyutlar olacak özetlenmiş". Klasik zaman serisi yöntemlerinden farklı olarak, bu yaklaşım, birden çok bağlam değişkenleri ve bunların birbirleriyle eğitim sırasında doğal olarak ekleme avantajı vardır. Otomatik ML veri kümesi ve tahmin horizons içindeki tüm öğeler için tek, ancak genellikle dahili olarak dallandırılmış bir model öğrenir. Daha fazla veri modelini parametreleri tahmin etmek bu nedenle kullanılabilir ve Genelleştirme görünmeyen dizilerine mümkün hale gelir. 

Daha fazla bilgi edinin ve bir örneğini görmek [zaman serisi tahmini için makine öğrenimi otomatik](how-to-auto-train-forecast.md).

## <a name="ensemble-models"></a>Topluluğu modelleri

Otomatik machine learning ile kullanarak topluluğu modelleri eğitebilirsiniz [Caruana topluluğu seçimi algoritması ile sıralanmış topluluğu başlatma](http://www.niculescu-mizil.org/papers/shotgun.icml04.revised.rev2.pdf). Topluluğu öğrenme, machine learning sonuçları ve öngörülebilir performans sınıfını tek modelleri kullanarak çok sayıda model tarayan tarafından artırır. Topluluğu yineleme çalıştırmanızın son yineleme görünür.

## <a name="use-with-onnx-in-c-apps"></a>İçinde ONNX kullanımıyla C# uygulamaları

Azure Machine Learning ile bir Python modeli oluşturun ve ONNX biçimine dönüştürülmesini sağlamak için otomatik ML kullanabilirsiniz. ONNX çalışma zamanını destekleyen C#otomatik olarak derlenen modelini kullanabilirsiniz. Bu nedenle, C# değiştirilemeyen için herhangi bir gereksinim ya da herhangi bir REST uç noktalarını tanıtan ağ gecikme olmadan uygulamalar. Bu akış örneği deneyin [bu Jupyter not defterine](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-with-onnx/auto-ml-classification-with-onnx.ipynb).

## <a name="automated-ml-across-microsoft"></a>Microsoft Otomatik ML

Otomatik ML ayrıca gibi diğer Microsoft çözümleri kullanılabilir:

|Tümleştirmeler|Açıklama|
|------------|-----------|
|[ML.NET](https://docs.microsoft.com/dotnet/machine-learning/automl-overview)|Otomatik model seçimi ve eğitim ML.NET ile Visual Studio ve Visual Studio Code kullanarak .NET uygulamaları (Önizleme) ML otomatik.|
|[HDInsight](../../hdinsight/spark/apache-spark-run-machine-learning-automl.md)|Ölçeği genişletme, otomatik ML eğitim işleri HDInsight içindeki Spark üzerinde paralel olarak kümeleri.|
|[PowerBI](https://docs.microsoft.com/power-bi/service-machine-learning-automated)|Makine öğrenimi modellerini doğrudan Power BI (Önizleme) içinde çağırın.|
|[SQL Server](https://cloudblogs.microsoft.com/sqlserver/2019/01/09/how-to-automate-machine-learning-on-sql-server-2019-big-data-clusters/)|Yeni makine öğrenimi modellerini SQL Server 2019 büyük veri kümelerinde verileriniz üzerinde oluşturun.|

## <a name="next-steps"></a>Sonraki adımlar

Örneklere bakın ve otomatik makine öğrenimini kullanarak model oluşturmayı öğrenin:

+ İzleyin [Öğreticisi: Otomatik olarak Azure Machine Learning otomatik ile bir regresyon modeli eğitimi](tutorial-auto-train-models.md)

+ Otomatik eğitim denemesini için ayarları yapılandırın:
  + Azure portal arabiriminde [şu adımları kullanarak](how-to-create-portal-experiments.md).
  + Python SDK'sı ile [şu adımları kullanarak](how-to-configure-auto-train.md).

+ Zaman serisi verilerini kullanarak train otomatik öğrenin [şu adımları kullanarak](how-to-auto-train-forecast.md).

+ Denemenin [Jupyter not defteri için otomatik machine learning örnekleri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/)
