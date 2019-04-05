---
title: Model yorumlanabilirliği
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning Interpretability SDK'sı neden modelinizi tahminlerde açıklamak için kullanmayı öğrenin. Bu eğitim ve çıkarım sırasında nasıl modelinizi tahminlerde anlamak için kullanılabilir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mesameki
author: mesameki
ms.reviewer: larryfr
ms.date: 04/04/2019
ms.openlocfilehash: f72923b80751f16ece128ced209679bbc325226c
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59051810"
---
# <a name="azure-machine-learning-interpretability-sdk"></a>Azure Machine Learning Interpretability SDK'sı

Bu makalede, neden modelinizi tahmin yapılan açıklamak nasıl öğreneceksiniz, Azure Machine Learning Interpretability SDK'sını kullanarak yapılır. Modelinizi açıklamak için aşağıdaki nedenlerle önemlidir:

* Müşteriler ve proje katılımcılarının bilmek isteyeceğiniz **Öngörüler güveniyorsanız modelinizi yapar**.
* Öğrenmek istediğiniz bir veri bilimi insanı **ilgili öngörüleri bulmak için bir modeli sorgulama**. Araçlar hakkında bilgiye dayalı kararlar da ihtiyacınız **modelinizi nasıl**.
* Bir şirket anlamanız gerekir. **değişen ile model davranışını giriş dağıtımları** ve **nasıl, model davranacağını özel giriş analiz edilirken**.

Machine learning interpretability makine öğrenimi geliştirme döngüsü iki aşamada önemlidir: **eğitim** zaman ve **çıkarım** zaman:

* Sırasında **eğitim**: Bir modelin çıkış güven oluşturmak için proje katılımcılarına açıklamak için interpretability araçları, model tasarımcılar ve değerlendiricilerini gerektirir. Bunlar ayrıca modelde hata ayıklama ve olup kendi hedefleri davranışı eşleşen hakkında kararlar model Öngörüler gerekir. Son olarak, modeli olmayan ağırlıklı emin olmak gerekir.
* Sırasında **çıkarım**: Öngörüler modelinizi kullanan kişilere explainable olması gerekir. Örneğin, neden modeli bir kredisinin Reddet veya tahmin yatırım Portföy daha yüksek risk taşır?

Azure Machine Learning Interpretability SDK'sı teknolojileri içerir. Microsoft tarafından geliştirilen ve üçüncü taraf kitaplıklar (örneğin, Şekil ve KÜF) kanıtlanmış. SDK'sı arasında tümleşik kitaplıkları ortak bir API oluşturur ve Azure Machine Learning hizmetlerini birleştirir. Bu SDK'sını kullanarak, makine öğrenimi modelleri açıklayabilir **genel olarak tüm veriler üzerinde**, veya **yerel olarak belirli bir veri noktasında** kullanımı kolay ve ölçeklenebilir bir şekilde-ürünü teknolojilerini kullanarak.

## <a name="how-does-it-work"></a>Nasıl çalışır?

Azure Machine Learning Interpretability modelin genel davranış veya belirli bir tahmin anlamak için uygulanabilir. Genel bir açıklama eski çağrılır ve yerel açıklama ikinci çağrılır.

Azure Machine Learning Interpretability yöntemleri de yöntemi modeli belirsiz veya belirli model olduğuna göre kategorilere ayrılabilir. Bazı yöntemler, belirli türde modelleri hedefleyin. Örneğin, Şekil'ın ağaç açıklama, yalnızca ağaç tabanlı modelleri için geçerlidir. Bazı yöntemler, model mimic açıklama veya Şekil'ın çekirdek açıklama gibi bir siyah kutu olarak kabul eder. Azure Machine Learning Interpretability SDK, veri kümeleri, model türleri ve kullanım örneklerine dayalı bu farklı yaklaşımların yararlanır.

Azure Machine Learning Interpretability bir model, tahmin nasıl kolaylaştırdığını bilgi kümesini döndürür. Bilgileri gibi öğeleri içerir:

* Genel/yerel göreli özellik önem derecesi
* Genel/yerel özellik ve tahmin ilişkisi
* Öngörüler gösteren etkileşimli görselleştirmeler özellik ve tahmin ilişki ve göreli önemi değerleri küresel ve yerel özelliği

## <a name="architecture"></a>Mimari

Azure Machine Learning Interpretability SDK'sı, iki Python paketlerine yapılandırılır:

* [azureml.Explain.model](https://docs.microsoft.com/python/api/azureml-explain-model/?view=azure-ml-py) -Microsoft tarafından desteklenen işlevler içeren ana paket.
* `azureml.contrib.explain.model` -Önizleme ve deneyebileceğiniz Deneysel işlevler.

    > [!IMPORTANT]
    > Contrib şeyler tam olarak desteklenmiyor. Deneysel işlevlerini olgun haline geldiğinden, ana paket için kademeli olarak taşınır.

### <a name="explainers"></a>Explainers

Azure Machine Learning Interpretability SDK'sı explainers iki kümesini sunar: Doğrudan Explainers ve Meta Explainers.

__Explainers doğrudan__ tümleşik kitaplıklarından gelir. Bir ortak API ve çıkış biçimini açıklamak SDK explainers sarmalar. SDK'da bulunan doğrudan explainers bir listesi verilmiştir:

> [!TIP]
> Bu explainers kullanarak doğrudan daha rahat kullanıyorsanız, doğrudan bunları ortak API ve çıkış biçimini kullanmak yerine çağırabilirsiniz.

* **Ağaç açıklama**: Polinom zaman hızlı şekil değer tahmin algoritması ağaçları ve grupları ağaçları belirli odaklanır Şekil'ın ağaç açıklama.
* **Ayrıntılı açıklama**: "Bir bağlantıda şekil NIPS açıklanan DeepLIFT oluşturan bir yüksek hızlı yaklaştırma ayrıntılı öğrenme modelleri değerlerde şekil için algoritmasıdır. açıklama şekil, ayrıntılı açıklama dayalı TensorFlow modelleri ve Keras modelleri TensorFlow arka uç kullanarak desteklenir (de mevcuttur PyTorch için ön destekten) ".
* **Çekirdek açıklama**: Şekil'ın çekirdek açıklama özel ağırlıklı yerel doğrusal regresyon şekil değerleri herhangi bir model için tahmin etmek için kullanır.
* **Açıklama taklit**: Mimic açıklama fikrini Genel temsilci modeli temel alır. Genel temsilci modeli, bir siyah kutu modelinin Öngörüler olabildiğince doğru bir şekilde yaklaşık olarak belirlemenizi sağlayan eğitildi doğası gereği yorumlanabilen bir modelidir. Veri uzmanı siyah kutu modeli hakkında bir sonuca çizmek için temsilci modeli yorumlayabilir.
* **KÜF açıklama**: KÜF açıklama üzerinde KÜF bağlı olarak, yerel vekil modeller oluşturmak için resim durumu yerel yorumlanabilirinde modeli belirsiz açıklamaları (sarı) algoritması kullanır. Genel temsilci modelleri, tek tek Öngörüler açıklamak için yerel vekil modelleri eğitme konusunda KÜF odaklanır.
* **Metin açıklama HAN**: HAN metin açıklama hiyerarşik dikkat ağ modeli açıklamalar için belirli bir siyah kutu metin modeli metin verileri almak için kullanır. Size verilen Öğretmen modelinin tahmin edilen çıkış HAN vekil model eğitin. Genel metin topluluğunuza arasında daha fazla eğitim sonra açıklamaları doğruluğunu artırmak için yapabileceğiniz ayarlamalar adım belirli bir belge için ekledik. HAN çift yönlü RNN cümle ve word dikkat iki dikkat katmanlarla kullanır. DNN Öğretmen model üzerinde geliştirilen ve belirli bir belge üzerinde ince ayar sonra word importances dikkat katmanlardan ayıklayabilirsiniz. Zaman de eğitim daha kesin KÜF veya Şekil metni veri ancak daha yüksek maliyetli içinde koşullarını olmasını HAN bulduk. Eğitim süresini geliştirmeleri hala yavaş olmasına rağmen ağ Eldiven word Gömmeleri ile başlatma seçeneği kullanıcı vererek ancak gerçekleştirdik. Uzak bir Azure GPU VM HAN çalıştırarak eğitim süresini önemli ölçüde geliştirilebilir. 'Dikkat ağları hiyerarşik olarak sınıflandırma (Yang et al., 2016) için' HAN uygulamasını açıklanan ([https://www.cs.cmu.edu/~diyiy/docs/naacl16.pdf](https://www.cs.cmu.edu/~diyiy/docs/naacl16.pdf)).

__Meta explainers__ otomatik olarak uygun bir doğrudan açıklama seçin ve verilen bir modeli ve veri kümelerine göre en iyi açıklama bilgisi oluştur. Meta explainers biz tümleşik geliştirilen veya tüm kitaplıkları (Şekil, açık yeşil, GA2M, benzetme, vb.) yararlanın. SDK'da bulunan meta explainers şunlardır:

* **Tablo açıklama**: Tablosal veri kümeleriyle birlikte kullanılır.
* **Metin açıklama**: Metin veri kümeleri ile kullanılır.
* **Görüntü açıklama** görüntü veri kümeleriyle birlikte kullanılır.

Ayrıca çok meta seçme, doğrudan explainers, meta explainers temel alınan kitaplıkları üzerine ek özellikler geliştirmek ve hız ve ölçeklenebilirlik üzerinde doğrudan explainers geliştirin.

Şu anda `TabularExplainer` doğrudan Explainers çağırmak için aşağıdaki mantığı kullanır:

1. Ağaç tabanlı bir modeli ise, uygulama `TreeExplainer`, başka
2. DNN modeli ise, uygulama `DeepExplainer`, başka
3. Bir kara kutu modeli gör ve uygulama `KernelExplainer`

Yerleşik zeka `TabularExplainer` ek olarak başka kitaplıklar SDK'sı ile tümleşiktir ve biz Artıları ve eksileri açıklama her biri hakkında bilgi edinin gibi daha karmaşık hale gelir.

`TabularExplainer` Ayrıca önemli özellik ve performans iyileştirmeleri doğrudan Explainers yapılmıştır:

* **Özetleme başlatma kümesinin**. Açıklama hızını en önemli olduğu durumlarda başlatma veri kümesini özetleyin ve hem genel hem de yerel açıklama hızlandırır küçük bir temsili örnekleri kümesi oluşturur.
* **Değerlendirme veri kümesi örnekleme**. Kullanıcı çok sayıda değerlendirme örnekleri geçirir, ancak uyumluluğunun değerlendirilebilmesi için bunların tümünün gerçekten gerekli değil, örnekleme parametresi Genel Açıklama hızlandırmak için true olarak ayarlanabilir.
* **KNN hızlı açıklama**. Açıklama tek Puanlama/tahmin hızlı olması gereken yere durumda KNN yöntemi kullanılabilir. Genel bir açıklama sırasında karşılık gelen top k özelliklerine ve başlatma örnekleri korunur. Her değerlendirme örnek için bir açıklama oluşturmak için KNN yöntemi en benzer örnekten başlatma örneklerini bulmak için kullanılır ve en çok benzer örneğe ait üst-k özellikleri değerlendirme örnek için ilk-k özellikler olarak döndürülür.

Aşağıdaki diyagramda iki doğrudan kümesi meta explainers arasındaki ilişkiyi gösterir.

[![Machine & öğrenme Interpretability mimari](./media/machine-learning-interpretability-explainability/interpretability-architecture.png)](./media/machine-learning-interpretability-explainability/interpretability-architecture.png#lightbox)

### <a name="models-supported"></a>Desteklenen modeller

Python'da veri kümelerinde eğitim görmüş olan herhangi bir model `numpy.array`, `pandas.DataFrame`, `iml.datatypes.DenseData`, veya `scipy.sparse.csr_matrix` biçimi, Machine Learning Interpretability SDK tarafından desteklenir.

Açıklama işlevleri modelleri hem de işlem hatları girdi olarak kabul edin. Bir model sağlanırsa, modelin tahmin işlevi uygulamak zorundadır `predict` veya `predict_proba` Scikit kuralını onaylar. Bir işlem hattı (işlem hattı betiğin adı) sağladıysanız, çalışan işlem hattı betiğin tahmin döndüren açıklama işlevi varsayar.

### <a name="local-and-remote-compute-target"></a>Yerel ve uzak işlem hedefi

Machine Learning Interpretability SDK'sı, hem yerel ve uzak işlem hedefleri ile çalışacak şekilde tasarlanmıştır. SDK işlevleri yerel olarak çalıştırırsanız, tüm Azure Hizmetleri iletişim kurmaz. Azure Machine Learning çalıştırma geçmişi Hizmetleri Açıklama bilgisi oturum ve Azure Machine Learning işlem açıklaması uzaktan çalıştırabilirsiniz. Bu bilgiler açtıktan sonra raporlar ve görselleştirmeler Açıklama'dan Azure Machine Learning çalışma alanı Portalı'nda kullanıcı analizi için kullanıma hazırdır.

## <a name="train-and-explain-locally"></a>Eğitim ve yerel olarak açıklayın

1. Yerel Jupyter not defterini modelinizi eğitin. 

    ``` python
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

2. Açıklama çağırın: Açıklama nesneyi başlatmak için model, eğitim verileri, ilgi alanı (isteğe bağlı) ve çıkış sınıf adları özelliklerini geçirmek gereken (varsa sınıflandırma) için açıklama. Şöyle bir açıklama nesnesini kullanarak örneği oluşturmak [TabularExplainer](https://docs.microsoft.com/python/api/azureml-explain-model/azureml.explain.model.tabularexplainer?view=azure-ml-py), [MimicExplainer](https://docs.microsoft.com/python/api/azureml-explain-model/azureml.explain.model.mimic.mimicexplainer?view=azure-ml-py), ve `LimeExplainer` yerel olarak. `TabularExplainer` altında üç explainers birini çağırma (`TreeExplainer`, `DeepExplainer`, veya `KernelExplainer`) ve kullanım durumunuz için en uygun olanına otomatik olarak seçme. Bununla birlikte, her biri kendi üç temel explainers doğrudan çağırabilir.

    ```python
    from azureml.explain.model.tabular_explainer import TabularExplainer
    explainer = TabularExplainer(model, x_train, features=breast_cancer_data.feature_names, classes=classes)
    or
    from azureml.explain.model.mimic.mimic_explainer import MimicExplainer
    from azureml.explain.model.mimic.models.lightgbm_model import LGBMExplainableModel
    explainer = MimicExplainer(model, x_train, LGBMExplainableModel, features=breast_cancer_data.feature_names, classes=classes)
    or
    from azureml.contrib.explain.model.lime.lime_explainer import LIMEExplainer
    explainer = LIMEExplainer(model, x_train, features=breast_cancer_data.feature_names, classes=classes)
    ```

3. Genel özellik önem değerleri alır.

    ```python
    # You can use the training data or the test data here. 
    global_explanation = explainer.explain_global(x_train)
    # Sorted feature importance values and feature names
    sorted_global_importance_values = global_explanation.get_ranked_global_values()
    sorted_global_importance_names = global_explanation.get_ranked_global_names()
    dict(zip(sorted_global_importance_names, sorted_global_importance_values))
    ```

4. Yerel özellik önem değerleri: aşağıdaki işlev çağrıları tek tek bir örneği veya bir grup örnekleri açıklamak için kullanın.

    ```python
    # explain the first data point in the test set
    local_explanation = explainer.explain_local(x_test[0,:])
    or
    # explain the first five data points in the test set
    local_explanation = explainer.explain_local(x_test[0:4,:])
    # sorted feature importance values and feature names
    sorted_local_importance_names = local_explanation.get_ranked_local_names()
    sorted_local_importance_values = local_explanation.get_ranked_local_values()
    ```

## <a name="train-and-explain-remotely"></a>Eğitim ve Uzaktan açıklayın

Azure Machine Learning hizmeti tarafından desteklenen çeşitli işlem hedefleri hakkında eğitebilirsiniz karşın, bu bölümdeki örnek AMLCompute kullanarak bunun nasıl yapılacağını gösterir.

1. Yerel Jupyter notebook (örneğin, run_explainer.py) içinde bir eğitim betiği oluşturun.

    ``` python  
    run = Run.get_context()
    client = ExplanationClient.from_run(run)
    
    breast_cancer_data = load_breast_cancer()
    X_train, X_test, y_train, y_test = train_test_split(breast_cancer_data.data, breast_cancer_data.target, test_size = 0.2, random_state = 0)
    data = {
        "train":{"X": X_train, "y": y_train},        
        "test":{"X": X_test, "y": y_test}
    }
    clf = svm.SVC(gamma=0.001, C=100., probability=True)
    model = clf.fit(data['train']['X'], data['train']['y'])
    joblib.dump(value = clf, filename = 'model.pkl')
    # explain predictions on your local machine    
    explainer = TabularExplainer(model, x_train, features=breast_cancer_data.feature_names, classes=classes)
    # explain overall model predictions (global explanation)
    global_explanation = explainer.explain_global(data["test"]["X"])
    # explain local data points (individual instances)
    local_explanation = explainer.explain_local(data["test"]["X"][0,:])
    # upload global and local explanation objects to Run History
    upload_model_explanation(run, local_explanation, top_k=2, comment='local explanation: top 2 features')
    # Uploading global model explanation data for storage or visualization in webUX
    # The explanation can then be downloaded on any compute
    # Multiple explanations can be uploaded
    client.upload_model_explanation(global_explanation, comment='global explanation: all features')
    # Or you can only upload the explanation object with the top k feature info
    #client.upload_model_explanation(global_explanation, top_k=2, comment='global explanation: Only top 2 features')
    ```

2. Yönergeleri takip edin [işlem hedeflerine yönelik model eğitiminin ayarlama](how-to-set-up-training-targets.md#amlcompute) bir Azure Machine Learning işlem, işlem hedefi olarak ayarlayın ve eğitim çalıştırmanız gönderme hakkında bilgi edinmek için.

3. Yerel Jupyter not defterine açıklama indirin. 

    ``` python
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

## <a name="next-steps"></a>Sonraki adımlar

Yukarıdaki yönergeleri gösteren bir Jupyter not defterleri koleksiyonunu görmek için [Azure Machine Learning Interpretability örnek not defterleri](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/explain-model).