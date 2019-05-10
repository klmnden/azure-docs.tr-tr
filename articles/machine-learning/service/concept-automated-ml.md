---
title: Otomatik ML algoritması seçme ve ayarlama
titleSuffix: Azure Machine Learning service
description: Nasıl Azure Machine Learning hizmeti otomatik olarak sizin için bir algoritma seçme ve ondan size zaman sağlamak için ölçütleri ve parametreleri kullanarak kaydetmek için bir model oluşturmak öğrenin modeliniz için en iyi algoritmayı seçin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: nacharya1
ms.author: nilesha
ms.date: 05/02/2019
ms.custom: seodec18
ms.openlocfilehash: 136141f5b598fd080edf3254fd01200f2742c763
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65235823"
---
# <a name="what-is-automated-machine-learning"></a>Nedir, makine öğrenimi otomatik?

AutoML da adlandırılan otomatik machine learning, veri uzmanları, analistler ve geliştiricilerin tüm model kalitesi dayanıklılık geçtikten çalışırken ile yüksek ölçek, verimliliğini ve üretkenliğini ML modelleri oluşturup sağlar. 

Otomatik ML, ML modelleri modelleri eğitim için otomatik olarak, akıllı bir şekilde seçerek bir dizi oluşturur ve sonra sizin için en iyisi önerir. Kaynak Kullanımı Yoğun önemli etki alanı bilgisini ve üretmek ve modelleri onlarca karşılaştırmak için zaman gerektiren geleneksel makine öğrenme modeli geliştirme. Otomatik ML ile üretime hazır ML model harika kolaylık ve verimlilik elde süresini hızlandırın.

Arka planda eğitim verilerinizi tanımlanmış hedef özelliğiyle gerçekleştirilen ve akıllı bir şekilde bağlı ML algoritmaları ve özellik seçimleri birleşimlerini yinelenir. Ardından, eğitim puanları temel alarak en iyi Ekrana sığdırılmış modeli tanımlanan ve sizin için önerilen. 

Deneme ve neler olduğunu, konusunda saydamlık denetime çözümlenmedi. Kısıtlamaları tanımlamak ve deneme süresi, doğruluk veya yineleme sayısı örneğin dayalı hedefler. Denemeniz, her yineleme için eğitim akış ve belirli bir model için en etkili özellikleri için oluşturulan her model görebilirsiniz.

## <a name="how-automated-ml-works"></a>Nasıl otomatik ML çalışır

Kullanarak **Azure Machine Learning hizmeti**, tasarım ve otomatik ML eğitim denemelerinizi ile aşağıdaki adımları çalıştırın:

1. **ML tanımlamaya** çözülecek: Sınıflandırma, tahmin veya regresyon
   
1. **Kaynak ve etiketli bir eğitim veri biçimi belirtmek**: Numpy diziler veya Pandas dataframe

1. **Model eğitimi için işlem hedefini yapılandırın**, gibi [yerel bilgisayar, Azure Machine Learning hesaplar, uzaktan sanal makineleri veya Azure Databricks](how-to-set-up-training-targets.md).  Otomatik eğitim hakkında bilgi edinin [uzak kaynak](how-to-auto-train-remote.md).

1. **Yapılandırma parametreleri öğrenme otomatik makine** farklı modelleri, Hiper parametre ayarları üzerinden kaç adet yineleme ön işleme/özellik kazandırma sayesinde Gelişmiş ve en iyi modeli belirlerken bakmak için hangi ölçümleri belirler.  Otomatik eğitim denemesini ayarlarını yapılandırabilirsiniz [Azure portalında](how-to-create-portal-experiments.md) veya [SDK'sı ile](how-to-configure-auto-train.md).

1. **Çalıştırma eğitim gönderin.** 


[![Otomatik makine öğrenimi](./media/how-to-automated-ml/automated-machine-learning.png)](./media/how-to-automated-ml/automated-machine-learning.png#lightbox)

Eğitim sırasında Azure Machine Learning hizmetinde bir dizi farklı algoritmaları ve parametrelerle deneyin işlem hatları oluşturur. Deneme içinde tanımlanan çıkış ölçütlerini İsabetleri sonra durdurulur. 

Ayrıca, çalışma sırasında toplanan ölçümleri içeren günlüğe kaydedilen çalışma bilgileri inceleyebilirsiniz. Python serileştirilmiş nesne eğitim çalıştırmanın (`.pkl` dosyası) modeli ve veri ön işleme içerir.


Model oluşturmanın otomatik karşın, şunları da yapabilirsiniz. [nasıl önemli veya ilgili özellikleri olan](how-to-configure-auto-train.md#explain) oluşturulmuş modelleri. 

> [!VIDEO https://www.youtube.com/embed/l8c-4iDPE0M]

<a name="preprocess"></a>

## <a name="preprocessing"></a>Ön işleme

Deneme her otomatik makine öğrenimi, verilerinizi varsayılan yöntemleri kullanılarak ve isteğe bağlı olarak Gelişmiş ön işleme önceden işlenmiş.

### <a name="automatic-preprocessing-standard"></a>Otomatik (standart) ön işleme
Deneme her otomatik makine öğrenimi, verilerinizi otomatik olarak ölçeği veya iyi gerçekleştirilip algoritmaları yardımcı olmak için normalleştirilmiş.  Her model için model eğitim sırasında ölçeklendirme veya normalleştirme aşağıdaki tekniklerden birini uygulanır.

|Ölçeklendirme&nbsp;&&nbsp;normalleştirme| Açıklama |
| ------------- | ------------- |
| [StandardScaleWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html)  | Ortalama kaldırarak ve birim varyansı için ölçeklendirme özellikleri standart hale getirin  |
| [MinMaxScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html)  | Her bir özellik bu sütunun minimum ve maksimum tarafından ölçeklendirerek özellikleri dönüştürür  |
| [MaxAbsScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MaxAbsScaler.html#sklearn.preprocessing.MaxAbsScaler) |    Her bir özellik tarafından en yüksek mutlak değeri ölçeklendirin |  
| [RobustScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.RobustScaler.html) |   Bu Scaler tarafından quantile menzil özellikleri |
| [PCA](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html) | Doğrusal boyut düzeyi azaltma için daha düşük bir boyutlu alanı proje için verilerin tekil değer ayrıştırma kullanma | 
| [TruncatedSVDWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.TruncatedSVD.html) |    Bu dönüştürücü doğrusal boyut düzeyi azaltma kesilmiş tekil değer ayrıştırma (SVD) yoluyla gerçekleştirir. PCA aykırı bu tahmin veri tekil değer ayrıştırma bilgi işlem önce Merkezi değil. Bu scipy.sparse matrisleri ile verimli bir şekilde çalışabilir anlamına gelir. | 
| [SparseNormalizer](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.Normalizer.html) | Böylece kendi norm (l1 veya l2) bir eşittir (diğer bir deyişle, her satır veri matris)'en az bir sıfır olmayan bileşeni ile her örnek bağımsız olarak diğer örnekleri boyutlandırılan | 

### <a name="advanced-preprocessing-optional-featurization"></a>Ön işleme Gelişmiş: isteğe bağlı özellik kazandırma sayesinde

Ayrıca ek Gelişmiş ön işleme ve özellik kazandırma sayesinde değerleri imputation, kodlama ve dönüştürmeler eksik gibi kullanılabilir. [Hangi özellik kazandırma sayesinde dahildir hakkında daha fazla bilgi](how-to-create-portal-experiments.md#preprocess). Bu ayarı etkinleştirin:
+ Azure portalı: Seçme **ön işleme** onay kutusu **Gelişmiş ayarlar** [adımlarıyla](how-to-create-portal-experiments.md). 
+ Python SDK: Belirtme `"preprocess": True` için [ `AutoMLConfig` sınıfı](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig?view=azure-ml-py).

## <a name="ensemble-models"></a>Topluluğu modelleri

Otomatik machine learning ile kullanarak topluluğu modelleri eğitebilirsiniz [Caruana topluluğu seçimi algoritması ile sıralanmış topluluğu başlatma](http://www.niculescu-mizil.org/papers/shotgun.icml04.revised.rev2.pdf). Topluluğu öğrenme, machine learning sonuçları ve öngörülebilir performans sınıfını tek modelleri kullanarak çok sayıda model tarayan tarafından artırır. Topluluğu yineleme çalıştırmanızın son yineleme görünür.

## <a name="use-with-onnx-in-c-apps"></a>İçinde ONNX kullanımıyla C# uygulamaları

Azure Machine Learning ile bir Python modeli oluşturun ve ONNX biçimine dönüştürülmesini sağlamak için otomatik ML kullanabilirsiniz. ONNX çalışma zamanını destekleyen C#otomatik olarak derlenen modelini kullanabilirsiniz. Bu nedenle, C# değiştirilemeyen için herhangi bir gereksinim ya da herhangi bir REST uç noktalarını tanıtan ağ gecikme olmadan uygulamalar. Bu akış örneği deneyin [bu Jupyter not defterine](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-with-onnx/auto-ml-classification-with-onnx.ipynb).

## <a name="automated-ml-across-microsoft"></a>Microsoft Otomatik ML

Otomatik ML ayrıca gibi diğer Microsoft çözümleri kullanılabilir:
+ Visual Studio ve Visual Studio Code kullanarak .NET uygulamaları [ML.NET](https://docs.microsoft.com/dotnet/machine-learning/automl-overview)
+ [HDInsight üzerinde](../../hdinsight/spark/apache-spark-run-machine-learning-automl.md), otomatik ML eğitim projelerinizdeki Spark HDInsight kümeleri paralel olarak ölçeği genişletme burada. 
+ [Power BI'da](https://docs.microsoft.com/power-bi/service-machine-learning-automated)

## <a name="next-steps"></a>Sonraki adımlar

Örneklere bakın ve otomatik Machine Learning kullanarak modelleri oluşturmayı öğrenin:

+ İzleyin [Öğreticisi: Otomatik olarak bir Azure Machine Learning otomatik sınıflandırma modeli eğitme](tutorial-auto-train-models.md)

+ Otomatik eğitim denemesini için ayarları yapılandırın: 
   + Azure portal arabiriminde [şu adımları kullanarak](how-to-create-portal-experiments.md).
   + Python SDK'sı ile [şu adımları kullanarak](how-to-configure-auto-train.md).
  
 + Zaman serisi verilerini kullanarak train otomatik öğrenin [şu adımları kullanarak](how-to-auto-train-forecast.md).

+ Denemenin [Jupyter not defteri örnekleri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/)
