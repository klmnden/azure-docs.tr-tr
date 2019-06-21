---
title: Model yorumlanabilirliği
titleSuffix: Azure Machine Learning service
description: Neden modelinizin Azure Machine Learning SDK'sını kullanarak tahminlerde açıklayacağınızı öğrenin. Bu eğitim ve çıkarım sırasında nasıl modelinizi tahminlerde anlamak için kullanılabilir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mesameki
author: mesameki
ms.reviewer: larryfr
ms.date: 05/30/2019
ms.openlocfilehash: b2e3b22672351b7e34c9ccccb37f0303b53a770f
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67292828"
---
# <a name="model-interpretability-with-azure-machine-learning-service"></a>Azure Machine Learning hizmeti ile model interpretability

Bu makalede, neden modelinizi tahmin yapılan açıklayacağınızı öğrenin Azure Machine Learning Python SDK'sı çeşitli interpretability paketlerle vermedi.

Sınıflar ve yöntemler SDK'yı kullanarak alabilirsiniz:
+ Önem derecesi değerlerini ham ve mühendislik uygulanan özellikleri için özellik
+ Eğitim ve çıkarım sırasında interpretability uygun ölçekte, gerçek veri kümeleri üzerinde.
+ Eğitim zaman bulma verileri ve açıklamalar desenlerinin yardımcı olmak için etkileşimli görselleştirmeler

Geliştirme döngüsü eğitim aşamasında modeli tasarımcılar ve değerlendiricilerini hipotezi doğrulayın ve Paydaşlar ile güven oluşturmak için bir modelin interpretability çıkış kullanabilirsiniz.  Ayrıca modelin Öngörüler hata ayıklama için kullandıkları, model davranışı doğrulama eşleşen kendi hedefleri sapması denetlemenizi sağlar.

Machine learning'de **özellikleri** bir hedef veri noktası tahmin etmek için kullanılan veri alanlardır. Örneğin, kredi riskini tahmin etmeniz yaş, hesabı boyut ve hesap yaşı veri alanlarında kullanılabilir. Bu durumda, yaş, hesabı boyut ve hesap geçerlilik süresi olan **özellikleri**. Özellik önem her veri alanı modeline ait tahminlerin nasıl etkilendiğini bildirir. Örneğin, hesabı boyut ve yaş tahmin doğruluğunu önemli ölçüde etkilemez ancak yaş tahmine yoğun olarak kullanılıyor olabilir. Bu işlem, böylece hissedarlar hangi veri noktalarını modelde en önemli içine görünürlük elde edilen tahminlere, açıklamak veri bilimcilerine sağlar.

Bu araçları kullanarak makine öğrenimi modelleri açıklayabilir **genel olarak tüm veriler üzerinde**, veya **yerel olarak belirli veri noktalarında** kullanımı kolay ve ölçeklenebilir bir şekilde-ürünü teknolojilerini kullanarak.

İnterpretability sınıfları birden çok SDK paketleri kullanıma sunulur. Bilgi edinmek için nasıl [SDK paketleri yüklemek için Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

* [`azureml.explain.model`](https://docs.microsoft.com/python/api/azureml-explain-model/?view=azure-ml-py), Microsoft tarafından desteklenen işlevler içeren bir ana paket.

* `azureml.contrib.explain.model`, Önizleme ve deneyebileceğiniz Deneysel işlevler.

* `azureml.train.automl.automlexplainer` Otomatik makine öğrenimi modellerini yorumlanması için paket.

> [!IMPORTANT]
> İçeriği `contrib` ad alanı tam olarak desteklenmiyor. Deneysel işlevlerini olgun haline geldiğinden, ana ad alanına aşamalı olarak taşınır.

## <a name="how-to-interpret-your-model"></a>Modelinizi yorumlama

İnterpretability sınıfları ve modelin genel davranış veya belirli tahminleri anlamak için yöntemleri uygulayabilirsiniz. Genel bir açıklama eski çağrılır ve yerel açıklama ikinci çağrılır.

Yöntemleri de yöntemi modeli belirsiz veya belirli model olduğuna göre kategorilere ayrılabilir. Bazı yöntemler, belirli türde modelleri hedefleyin. Örneğin, Şekil'ın ağaç açıklama, yalnızca ağaç tabanlı modelleri için geçerlidir. Bazı yöntemler, model mimic açıklama veya Şekil'ın çekirdek açıklama gibi bir siyah kutu olarak kabul eder. `explain` Paket veri kümeleri, model türleri ve kullanım örneklerine dayalı bu farklı yaklaşımların yararlanır.

Çıktı bir dizi bilgi nasıl belirli bir model, tahmin gibi yapar.
* Genel/yerel göreli özellik önem derecesi

* Genel/yerel özellik ve tahmin ilişkisi

### <a name="explainers"></a>Explainers

Explainers iki tür vardır: Doğrudan Explainers ve Meta Explainers SDK.

__Explainers doğrudan__ tümleşik kitaplıklarından gelir. Bir ortak API ve çıkış biçimini açıklamak SDK explainers sarmalar. Bu explainers kullanarak doğrudan daha rahat kullanıyorsanız, doğrudan bunları ortak API ve çıkış biçimini kullanmak yerine çağırabilirsiniz. SDK'da bulunan doğrudan explainers bir listesi verilmiştir:

* **Ağaç açıklama**: Polinom zaman hızlı şekil değer tahmin algoritması ağaçları ve grupları ağaçları belirli odaklanır Şekil'ın ağaç açıklama.
* **Ayrıntılı açıklama**: "Bir bağlantıda şekil NIPS açıklanan DeepLIFT oluşturan bir yüksek hızlı yaklaştırma ayrıntılı öğrenme modelleri değerlerde şekil için algoritmasıdır. açıklama şekil, ayrıntılı açıklama dayalı TensorFlow modelleri ve Keras modelleri TensorFlow arka uç kullanarak desteklenir (de mevcuttur PyTorch için ön destekten) ".
* **Çekirdek açıklama**: Şekil'ın çekirdek açıklama özel ağırlıklı yerel doğrusal regresyon şekil değerleri herhangi bir model için tahmin etmek için kullanır.
* **Açıklama taklit**: Mimic açıklama fikrini Genel temsilci modeli temel alır. Genel temsilci modeli, bir siyah kutu modelinin Öngörüler olabildiğince doğru bir şekilde yaklaşık olarak belirlemenizi sağlayan eğitildi doğası gereği yorumlanabilen bir modelidir. Veri uzmanı siyah kutu modeli hakkında bir sonuca çizmek için temsilci modeli yorumlayabilir.
* **KÜF açıklama** (`contrib`): KÜF açıklama üzerinde KÜF bağlı olarak, yerel vekil modeller oluşturmak için resim durumu yerel yorumlanabilirinde modeli belirsiz açıklamaları (sarı) algoritması kullanır. Genel temsilci modelleri, tek tek Öngörüler açıklamak için yerel vekil modelleri eğitme konusunda KÜF odaklanır.
* **Metin açıklama HAN** (`contrib`): HAN metin açıklama hiyerarşik dikkat ağ modeli açıklamalar için belirli bir siyah kutu metin modeli metin verileri almak için kullanır. Size verilen Öğretmen modelinin tahmin edilen çıkış HAN vekil model eğitin. Genel metin topluluğunuza arasında daha fazla eğitim sonra açıklamaları doğruluğunu artırmak için yapabileceğiniz ayarlamalar adım belirli bir belge için ekledik. HAN çift yönlü RNN cümle ve word dikkat iki dikkat katmanlarla kullanır. DNN Öğretmen model üzerinde geliştirilen ve belirli bir belge üzerinde ince ayar sonra word importances dikkat katmanlardan ayıklayabilirsiniz. Zaman de eğitim daha kesin KÜF veya Şekil metni veri ancak daha yüksek maliyetli içinde koşullarını olmasını HAN bulduk. Eğitim süresini geliştirmeleri hala yavaş olmasına rağmen ağ Eldiven word Gömmeleri ile başlatma seçeneği kullanıcı vererek ancak gerçekleştirdik. Uzak bir Azure GPU VM HAN çalıştırarak eğitim süresini önemli ölçüde geliştirilebilir. 'Dikkat ağları hiyerarşik olarak sınıflandırma (Yang et al., 2016) için' HAN uygulamasını açıklanan ([https://www.cs.cmu.edu/~diyiy/docs/naacl16.pdf](https://www.cs.cmu.edu/~diyiy/docs/naacl16.pdf)).

__Meta explainers__ otomatik olarak uygun bir doğrudan açıklama seçin ve verilen bir modeli ve veri kümelerine göre en iyi açıklama bilgisi oluştur. Meta explainers biz tümleşik geliştirilen veya tüm kitaplıkları (Şekil, açık yeşil, benzetme, vb.) yararlanın. SDK'da bulunan meta explainers şunlardır:

* **Tablo açıklama**: Tablosal veri kümeleriyle birlikte kullanılır.
* **Metin açıklama**: Metin veri kümeleri ile kullanılır.

Ayrıca çok meta seçme, doğrudan explainers, meta explainers temel alınan kitaplıkları üzerine ek özellikler geliştirmek ve hız ve ölçeklenebilirlik üzerinde doğrudan explainers geliştirin.

Şu anda `TabularExplainer` doğrudan Explainers çağırmak için aşağıdaki mantığı kullanır:

1. Ağaç tabanlı bir modeli ise, uygulama `TreeExplainer`, başka
2. DNN modeli ise, uygulama `DeepExplainer`, başka
3. Bir kara kutu modeli gör ve uygulama `KernelExplainer`

Yerleşik zeka `TabularExplainer` ek olarak başka kitaplıklar SDK'sı ile tümleşiktir ve biz Artıları ve eksileri açıklama her biri hakkında bilgi edinin gibi daha karmaşık hale gelir.

`TabularExplainer` Ayrıca önemli özellik ve performans iyileştirmeleri doğrudan Explainers yapılmıştır:

* **Özetleme başlatma kümesinin**. Açıklama hızını en önemli olduğu durumlarda başlatma veri kümesini özetleyin ve hem genel hem de yerel açıklama hızlandırır küçük bir temsili örnekleri kümesi oluşturur.
* **Değerlendirme veri kümesi örnekleme**. Kullanıcı çok sayıda değerlendirme örnekleri geçirir, ancak uyumluluğunun değerlendirilebilmesi için bunların tümünün gerçekten gerekli değil, örnekleme parametresi Genel Açıklama hızlandırmak için true olarak ayarlanabilir.

Aşağıdaki diyagramda iki doğrudan kümesi meta explainers arasındaki ilişkiyi gösterir.

[![Machine Learning Interpretability mimarisi](./media/machine-learning-interpretability-explainability/interpretability-architecture.png)](./media/machine-learning-interpretability-explainability/interpretability-architecture.png#lightbox)

### <a name="models-supported"></a>Desteklenen modeller

Python'da veri kümelerinde eğitim görmüş olan herhangi bir model `numpy.array`, `pandas.DataFrame`, `iml.datatypes.DenseData`, veya `scipy.sparse.csr_matrix` biçim interpretability tarafından desteklenen `explain` SDK paketi.

Açıklama işlevleri modelleri hem de işlem hatları girdi olarak kabul edin. Bir model sağlanırsa, modelin tahmin işlevi uygulamak zorundadır `predict` veya `predict_proba` Scikit kurala uyan. Bir işlem hattı (işlem hattı betiğin adı) sağladıysanız, çalışan işlem hattı betiğin tahmin döndüren açıklama işlevi varsayar.

### <a name="local-and-remote-compute-target"></a>Yerel ve uzak işlem hedefi

`explain` Paket, hem yerel ve uzak işlem hedefleri ile çalışacak şekilde tasarlanmıştır. SDK işlevleri yerel olarak çalıştırırsanız, tüm Azure Hizmetleri iletişim kurmaz. Azure Machine Learning çalıştırma geçmişi Hizmetleri Açıklama bilgisi oturum ve Azure Machine Learning işlem açıklaması uzaktan çalıştırabilirsiniz. Bu bilgiler açtıktan sonra raporlar ve görselleştirmeler Açıklama'dan kullanıcı analizi için Azure Machine Learning çalışma alanı portalında kullanıma hazırdır.

## <a name="interpretability-in-training"></a>Eğitim interpretability

### <a name="train-and-explain-locally"></a>Eğitim ve yerel olarak açıklayın

1. Yerel Jupyter not defterini modelinizi eğitin.

    ```python
    # load breast cancer dataset, a well-known small dataset that comes with scikit-learn
    from sklearn.datasets import load_breast_cancer
    from sklearn import svm
    from sklearn.model_selection import train_test_split
    breast_cancer_data = load_breast_cancer()
    classes = breast_cancer_data.target_names.tolist()
    # Split data into train and test
    from sklearn.model_selection import train_test_split
    x_train, x_test, y_train, y_test = train_test_split(breast_cancer_data.data, breast_cancer_data.target, test_size=0.2, random_state=0)
    clf = svm.SVC(gamma=0.001, C=100., probability=True)
    model = clf.fit(x_train, y_train)
    ```

2. Açıklama çağırın: Bir açıklama nesnesini başlatmak için model ve bazı eğitim verilerini Açıklama'nın oluşturucuya geçirilecek gerekir. Ayrıca isteğe bağlı olarak, özellik adları ve açıklamaları ve görselleştirmeler daha bilgilendirici yapmak için kullanılacak (sınıflandırma yapılması durumunda) çıktı sınıf adları da geçirebilirsiniz. Şöyle bir açıklama nesnesini kullanarak örneği oluşturmak [TabularExplainer](https://docs.microsoft.com/python/api/azureml-explain-model/azureml.explain.model.tabularexplainer?view=azure-ml-py) ve [MimicExplainer](https://docs.microsoft.com/python/api/azureml-explain-model/azureml.explain.model.mimic.mimicexplainer?view=azure-ml-py) yerel olarak. `TabularExplainer` altında üç explainers birini çağırma (`TreeExplainer`, `DeepExplainer`, veya `KernelExplainer`) ve kullanım durumunuz için en uygun olanına otomatik olarak seçme. Bununla birlikte, her biri kendi üç temel explainers doğrudan çağırabilir.

    ```python
    from azureml.explain.model.tabular_explainer import TabularExplainer
    # "features" and "classes" fields are optional
    explainer = TabularExplainer(model, x_train, features=breast_cancer_data.feature_names, classes=classes)
    ```

    or

    ```python
    from azureml.explain.model.mimic.mimic_explainer import MimicExplainer
    from azureml.explain.model.mimic.models.lightgbm_model import LGBMExplainableModel

    # "features" and "classes" fields are optional
    explainer = MimicExplainer(model, x_train, LGBMExplainableModel, features=breast_cancer_data.feature_names, classes=classes)
    ```

3. Genel özellik önem değerleri alır.

    ```python
    # You can use the training data or the test data here
    global_explanation = explainer.explain_global(x_train)
    # Sorted feature importance values and feature names
    sorted_global_importance_values = global_explanation.get_ranked_global_values()
    sorted_global_importance_names = global_explanation.get_ranked_global_names()
    dict(zip(sorted_global_importance_names, sorted_global_importance_values))
    ```

4. Yerel özellik önem değerleri: aşağıdaki işlev çağrıları tek tek bir örneği veya bir grup örnekleri açıklamak için kullanın.

    ```python
    # explain the first data point in the test set
    local_explanation = explainer.explain_local(x_test[0])

    # sorted feature importance values and feature names
    sorted_local_importance_names = local_explanation.get_ranked_local_names()
    sorted_local_importance_values = local_explanation.get_ranked_local_values()
    ```

    or

    ```python
    # explain the first five data points in the test set
    local_explanation = explainer.explain_local(x_test[0:4])

    # sorted feature importance values and feature names
    sorted_local_importance_names = local_explanation.get_ranked_local_names()
    sorted_local_importance_values = local_explanation.get_ranked_local_values()
    ```

### <a name="train-and-explain-remotely"></a>Eğitim ve Uzaktan açıklayın

Azure Machine Learning hizmeti tarafından desteklenen çeşitli işlem hedefleri hakkında eğitebilirsiniz karşın, bu bölümdeki örnek bir Azure Machine Learning işlem hedef kullanarak bunu nasıl gösterir.

1. Yerel Jupyter notebook (örneğin, run_explainer.py) içinde bir eğitim betiği oluşturun.

    ```python
    run = Run.get_context()
    client = ExplanationClient.from_run(run)

    # Train your model here

    # explain predictions on your local machine
    # "features" and "classes" fields are optional
    explainer = TabularExplainer(model, x_train, features=breast_cancer_data.feature_names, classes=classes)
    # explain overall model predictions (global explanation)
    global_explanation = explainer.explain_global(x_test)
    # explain local data points (individual instances)
    local_explanation = explainer.explain_local(x_test[0])
    # upload global and local explanation objects to Run History
    client.upload_model_explanation(run, local_explanation, top_k=2, comment='local explanation: top 2 features')
    # Uploading global model explanation data for storage or visualization in webUX
    # The explanation can then be downloaded on any compute
    # Multiple explanations can be uploaded
    client.upload_model_explanation(global_explanation, comment='global explanation: all features')
    # Or you can only upload the explanation object with the top k feature info
    #client.upload_model_explanation(global_explanation, top_k=2, comment='global explanation: Only top 2 features')
    ```

2. Yönergeleri takip edin [işlem hedeflerine yönelik model eğitiminin ayarlama](how-to-set-up-training-targets.md#amlcompute) bir Azure Machine Learning işlem, işlem hedefi olarak ayarlayın ve eğitim çalıştırmanız gönderme hakkında bilgi edinmek için.

3. Yerel Jupyter not defterine açıklama indirin.

    ```python
    from azureml.contrib.explain.model.explanation.explanation_client import ExplanationClient
    # Get model explanation data
    client = ExplanationClient.from_run(run)
    explanation = client.download_model_explanation()
    local_importance_values = explanation.local_importance_values
    expected_values = explanation.expected_values
    # Or you can use the saved run.id to retrive the feature importance values
    client = ExplanationClient.from_run_id(ws, experiment_name, run.id)
    explanation = client.download_model_explanation()
    local_importance_values = explanation.local_importance_values
    expected_values = explanation.expected_values
    # Get the top k (e.g., 4) most important features with their importance values
    explanation = client.download_model_explanation(top_k=4)
    global_importance_values = explanation.get_ranked_global_values()
    global_importance_names = explanation.get_ranked_global_names()
    print('global importance values: {}'.format(global_importance_values))
    print('global importance names: {}'.format(global_importance_names))
    ```

## <a name="visualizations"></a>Görsel öğeler

Anlama ve modelinizi yorumlamak için görselleştirme Panosu kullanın:

### <a name="global-visualizations"></a>Genel görselleştirmeler

Aşağıdaki çizimler, eğitilen model, Öngörüler ve açıklamalar ile birlikte genel bir görünümünü sağlar.

|Çizim|Açıklama|
|----|-----------|
|Veri keşfi| Tahmin değerleri yanı sıra veri kümesi genel bakış.|
|Genel önem derecesi|Genel olarak üst K (yapılandırılabilir K) önemli özellikleri gösterir. Bu grafik, temel alınan modelin genel davranışını anlamak için kullanışlıdır.|
|Açıklama keşfetme|Nasıl bir özellik modelinin tahmin değerleri (veya olasılığını tahmin değerleri) bir değişiklik yapmaktan sorumlu gösterir. |
|Özet| İmzalı yerel özellik önem değerleri tüm veri noktaları her bir özellik etkisini tahmin değerine göre dağılımını göstermek için kullanır.|

[![Görselleştirme Panosu-genel](./media/machine-learning-interpretability-explainability/global-charts.png)](./media/machine-learning-interpretability-explainability/global-charts.png#lightbox)

### <a name="local-visualizations"></a>Yerel görselleştirmeler

Verilen veri noktası için yerel özelliği önem çizim yüklemek için yukarıdaki çizimleri herhangi bir zamanda herhangi bir tek veri noktasında tıklayabilirsiniz.

|Çizim|Açıklama|
|----|-----------|
|Yerel önem derecesi|Genel olarak üst K (yapılandırılabilir K) önemli özellikleri gösterir. Bu grafik, belirli bir veri noktasına ait modelini yerel davranışını anlamak için kullanışlıdır.|

[![Görsel öğe Pano yerel](./media/machine-learning-interpretability-explainability/local-charts.png)](./media/machine-learning-interpretability-explainability/local-charts.png#lightbox)

Not Jupyter çekirdek başlatılmadan önce etkin görsel öğe Pano pencere öğesi uzantıları sahip olması gerekir.

* Jupyter notebooks

    ```shell
    jupyter nbextension install --py --sys-prefix azureml.contrib.explain.model.visualize
    jupyter nbextension enable --py --sys-prefix azureml.contrib.explain.model.visualize
    ```



* Jupyter Laboratuvarları

    ```shell
    jupyter labextension install @jupyter-widgets/jupyterlab-manager
    jupyter labextension install microsoft-mli-widget
    ```
Görselleştirmeyi panoya yüklemek için aşağıdaki kodu kullanın:

```python
from azureml.contrib.explain.model.visualize import ExplanationDashboard

ExplanationDashboard(global_explanation, model, x_test)
```

## <a name="raw-feature-transformations"></a>RAW özelliği dönüşümleri

İsteğe bağlı olarak, ham özellikler açısından açıklamaları dönüştürme (yerine önce mühendislik uygulanan özellikleri) almak için bir açıklama için özellik dönüşüm işlem hattı geçirebilirsiniz. Bu atlarsanız, açıklama, mühendislik uygulanan özellikler açısından açıklamalar sağlar.

Bir açıklandığı gibi desteklenen dönüşümler biçiminin aynı [sklearn pandas](https://github.com/scikit-learn-contrib/sklearn-pandas). Genel olarak, bunlar tek bir sütun üzerinde çalışır ve bu nedenle açıkça tek-çok sürece herhangi bir dönüştürme desteklenir.

```python
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.linear_model import LogisticRegression
from sklearn_pandas import DataFrameMapper

# Assume that we have created two arrays, numerical and categorical, which holds the numerical and categorical feature names

numeric_transformations = [([f], Pipeline(steps=[('imputer', SimpleImputer(strategy='median')), ('scaler', StandardScaler())])) for f in numerical]

categorical_transformations = [([f], OneHotEncoder(handle_unknown='ignore', sparse=False)) for f in categorical]

transformations = numeric_transformations + categorical_transformations

# Append model to preprocessing pipeline.
# Now we have a full prediction pipeline.
clf = Pipeline(steps=[('preprocessor', DataFrameMapper(transformations)),
                    ('classifier', LogisticRegression(solver='lbfgs'))])

# clf.steps[-1][1] returns the trained classification model
# Pass transformation as an input to create the explanation object
# "features" and "classes" fields are optional
tabular_explainer = TabularExplainer(clf.steps[-1][1], initialization_examples=x_train, features=dataset_feature_names, classes=dataset_classes, transformations=transformations)
```

## <a name="interpretability-in-inference"></a>Çıkarım içinde interpretability

Açıklama, özgün modeli ile birlikte dağıtılabilir ve zaman Puanlama yerel açıklama bilgilerini sağlamak için kullanılabilir. Puanlama açıklama dağıtma işlemi, bir model dağıtımına benzer ve aşağıdaki adımları içerir:

1. Bir açıklama nesnesi oluşturun:

   ```python
   from azureml.contrib.explain.model.tabular_explainer import TabularExplainer

   explainer = TabularExplainer(model, x_test)
   ```

1. Açıklama nesnesini kullanarak Puanlama bir açıklama oluşturun:

   ```python
   scoring_explainer = explainer.create_scoring_explainer(x_test)

   # Pickle scoring explainer
   scoring_explainer_path = scoring_explainer.save('scoring_explainer_deploy')
   ```

1. Yapılandırma ve puanlama açıklama modelini kullanan bir görüntüyü kaydedin.

   ```python
   # Register explainer model using the path from ScoringExplainer.save - could be done on remote compute
   run.upload_file('breast_cancer_scoring_explainer.pkl', scoring_explainer_path)
   model = run.register_model(model_name='breast_cancer_scoring_explainer', model_path='breast_cancer_scoring_explainer.pkl')
   print(model.name, model.id, model.version, sep = '\t')
   ```

1. [İsteğe bağlı] Buluttan Puanlama açıklama almak ve açıklamaları test edin

   ```python
   from azureml.contrib.explain.model.scoring.scoring_explainer import ScoringExplainer

   # Retrieve the scoring explainer model from cloud"
   scoring_explainer_model = Model(ws, 'breast_cancer_scoring_explainer')
   scoring_explainer_model_path = scoring_explainer_model.download(target_dir=os.getcwd(), exist_ok=True)

   # Load scoring explainer from disk
   scoring_explainer = ScoringExplainer.load(scoring_explainer_model_path)

   # Test scoring explainer locally
   preds = scoring_explainer.explain(x_test)
   print(preds)
   ```

1. Resim bir işlem hedefine dağıtın:

   1. Puanlama dosyası oluşturun (Bu adımdan önce adımları [dağıtma modeller Azure Machine Learning hizmeti ile](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-and-where) özgün tahmin modelinizi kaydetmek için)

        ```python
        %%writefile score.py
        import json
        import numpy as np
        import os
        import pickle
        from sklearn.externals import joblib
        from sklearn.linear_model import LogisticRegression
        from azureml.core.model import Model

        def init():

            global original_model
            global scoring_model

            # Retrieve the path to the model file using the model name
            # Assume original model is named original_prediction_model
            original_model_path = Model.get_model_path('original_prediction_model')
            scoring_explainer_path = Model.get_model_path('breast_cancer_scoring_explainer')

            original_model = joblib.load(original_model_path)
            scoring_explainer = joblib.load(scoring_explainer_path)

        def run(raw_data):
            # Get predictions and explanations for each data point
            data = np.array(json.loads(raw_data)['data'])
            # Make prediction
            predictions = original_model.predict(data)
            # Retrieve model explanations
            local_importance_values = scoring_explainer.explain(data)
            # You can return any data type as long as it is JSON-serializable
            return {'predictions': predictions.tolist(), 'local_importance_values': local_importance_values}
        ```

   1. (Bu yapılandırma, modelinizi gereksinimlerine bağlıdır. dağıtım yapılandırmasını tanımlayın Aşağıdaki örnek, bir CPU Çekirdeği ve 1 GB bellek kullanan yapılandırması tanımlar)

        ```python
        from azureml.core.webservice import AciWebservice

        aciconfig = AciWebservice.deploy_configuration(cpu_cores=1,
                                                       memory_gb=1,
                                                       tags={"data": "breastcancer",
                                                             "method" : "local_explanation"},
                                                       description='Get local explanations for breast cancer data')
        ```

   1. Ortam bağımlılıkları olan bir dosya oluşturun

        ```python
        from azureml.core.conda_dependencies import CondaDependencies

        # WARNING: to install this, g++ needs to be available on the Docker image and is not by default (look at the next cell)


        myenv = CondaDependencies.create(pip_packages=["azureml-defaults", "azureml-explain-model", "azureml-contrib-explain-model"],
                                        conda_packages=["scikit-learn"])

        with open("myenv.yml","w") as f:
            f.write(myenv.serialize_to_string())

        with open("myenv.yml","r") as f:
            print(f.read())
        ```

   1. Özel bir dockerfile g yüklü ++ ile oluşturma

        ```python
        %%writefile dockerfile
        RUN apt-get update && apt-get install -y g++
        ```

   1. Oluşturulan görüntüyü dağıtmak (tahmini süre: 5 dakika)

        ```python
        from azureml.core.webservice import Webservice
        from azureml.core.image import ContainerImage

        # Use the custom scoring, docker, and conda files we created above
        image_config = ContainerImage.image_configuration(execution_script="score.py",
                                                        docker_file="dockerfile",
                                                        runtime="python",
                                                        conda_file="myenv.yml")

        # Use configs and models generated above
        service = Webservice.deploy_from_model(workspace=ws,
                                            name='model-scoring-service',
                                            deployment_config=aciconfig,
                                            models=[scoring_explainer_model, original_model],
                                            image_config=image_config)

        service.wait_for_deployment(show_output=True)
        ```

1. Dağıtımı test etme

    ```python
    import requests

    # Create data to test service with
    x_list = x_test.tolist()
    examples = x_list[:4]
    input_data = "{\"data\": " + str(examples) + "}"

    headers = {'Content-Type':'application/json'}

    # send request to service
    resp = requests.post(service.scoring_uri, input_data, headers=headers)

    print("POST to url", service.scoring_uri)
    # can covert back to Python objects from json string if desired
    print("prediction:", resp.text)
    ```

1. Temizleme: Dağıtılmış bir web hizmetini silmek için kullanın `service.delete()`.

## <a name="interpretability-in-automated-ml"></a>Otomatik ML interpretability

Otomatik machine learning paketleri otomatik olarak geliştirilen modellerinin özellik önemi yorumlanması için içerir. Ayrıca, sınıflandırma senaryoları sınıf düzeyinde özelliği önem almanızı sağlar. Otomatik makine öğrenimi içinde bu davranışı etkinleştirmek için iki yöntem vardır:

* Eğitilen topluluğu modeli için özellik önem etkinleştirmek için [ `explain_model()` ](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlexplainer?view=azure-ml-py) işlevi.

    ```python
    from azureml.train.automl.automlexplainer import explain_model

    shap_values, expected_values, overall_summary, overall_imp, \
        per_class_summary, per_class_imp = explain_model(fitted_model, X_train, X_test)
    ```

* Eğitim önce tek tek her çalıştırma için özellik önem etkinleştirmek için `model_explainability` parametresi `True` içinde `AutoMLConfig` birlikte doğrulama verileri sağlayan nesne. Ardından [ `retrieve_model_explanation()` ](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlexplainer?view=azure-ml-py) işlevi.

    ```python
    from azureml.train.automl.automlexplainer import retrieve_model_explanation

    shap_values, expected_values, overall_summary, overall_imp, per_class_summary, \
        per_class_imp = retrieve_model_explanation(best_run)
    ```

Daha fazla bilgi için [yapılır](how-to-configure-auto-train.md#explain-the-model-interpretability) otomatik machine learning'de interpretability özellikleri etkinleştirme.

## <a name="next-steps"></a>Sonraki adımlar

Yukarıdaki yönergeleri gösteren bir Jupyter not defterleri koleksiyonunu görmek için [Azure Machine Learning Interpretability örnek not defterleri](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/explain-model).
