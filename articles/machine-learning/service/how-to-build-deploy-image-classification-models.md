---
title: Derleme ve Azure Machine Learning paket için bilgisayar Vizyon kullanarak bir görüntü sınıflandırma modeli dağıtın.
description: Oluşturmak, eğitmek, test ve Azure Machine Learning paket için bilgisayar Vizyon kullanarak bir bilgisayarı görme görüntü sınıflandırma modeli dağıtma hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: netahw
author: nhaiby
ms.date: 04/23/2018
ms.openlocfilehash: bd9f01e76c68fa41616818251b5b54553059cbcc
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="build-and-deploy-image-classification-models-with-azure-machine-learning"></a>Derleme ve Azure Machine Learning ile görüntü sınıflandırma modelleri dağıtma

Bu makalede, nasıl kullanılacağını öğrenirsiniz **bilgisayar görme için Azure Machine Learning paketi** eğitmek, test ve bir görüntü sınıflandırma modeli dağıtın (AMLPCV). 

Görüntü sınıflandırmasını kullanan çok sayıda bilgisayar görme etki alanındaki sorunları çözülebilir. Bu sorunlar gibi soruları yanıtlamak model oluşturmaya içerir:
+ _Bir nesne görüntüde var mı? Örneğin, "köpek", "araba", "sevk" vb._
+ _Hangi sınıfın göz Hastalık önem derecesine göre bu hasta'nın retinal tarama evinced?_

Oluşturma ve dağıtma AMLPCV Bu modelde olduğunda, aşağıdaki adımlarda size gidin:
1. Veri kümesi oluşturma
2. Görüntü Görselleştirme ve ek açıklaması
3. Görüntü augmentation'a
4. Derin sinir ağı (DNN) modeli tanımı
5. Sınıflandırıcı eğitim
6. Değerlendirme ve görselleştirme
7. Web hizmeti dağıtımı
8. Web hizmeti yük test etme

[CNTK](https://www.microsoft.com/cognitive-toolkit/) kullanılan derin öğrenme çerçevesi eğitim yerel olarak desteklenen GPU makinede gibi gerçekleştirilir ([derin veri bilimi VM öğrenme](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview)), ve dağıtım Azure ML Operationalization CLI kullanır.

Başvurun [paketini başvuru belgeleri](https://aka.ms/aml-packages/vision) her modül ve sınıfı için ayrıntılı başvuru için.

## <a name="prerequisites"></a>Önkoşullar

1. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

1. Aşağıdaki hesapları ve uygulama kurmalı ve yüklü:
   - Bir Azure Machine Learning Denemesi hesabı 
   - Bir Azure Machine Learning modeli yönetim hesabı
   - Azure Machine Learning Workbench'in yüklü olması

   Bu üç henüz oluşturduğunuz veya yüklü uygulayın [Azure Machine Learning Quickstart ve çalışma ekranı yükleme](../service/quickstart-installation.md) makalesi. 

1. Bilgisayar görme için Azure Machine Learning paketi yüklü olmalıdır. Bilgi edinmek için nasıl [burada bu paketi yükledikten](https://aka.ms/aml-packages/vision).

## <a name="sample-data-and-notebook"></a>Örnek veriler ve not defteri

### <a name="get-the-jupyter-notebook"></a>Jupyter Not Defteri Al

İndirme örneği çalıştırmak için Not Defteri kendiniz burada açıklanmıştır.

> [!div class="nextstepaction"]
> [Jupyter Not Defteri Al](https://aka.ms/aml-packages/vision/notebooks/image_classification)

### <a name="load-the-sample-data"></a>Örnek verileri yükleme

Aşağıdaki örnek, 63 yemek takımı görüntülerini oluşan bir veri kümesi kullanır. Her görüntü (bowl fincanı, Çatal bıçak, kalıbı) dört farklı sınıflardan birine ait olarak etiketlenir. Bu örnekte resimlerinin sayısı küçük, böylelikle Bu örnek hızlı yürütülebilir. Uygulamada sınıfı başına en az 100 görüntüleri sağlanmalıdır. Tüm görüntüleri adresindedir *".. /sample_data/imgs_recycling / "*,"bowl"adlı alt dizinleri,"fincanı","Çatal bıçak"ve"kalıbı".

![Azure Machine Learning veri kümesi](media/how-to-build-deploy-image-classification-models/recycling_examples.jpg)

## <a name="storage-context"></a>Depolama bağlamı

Depolama bağlamı çeşitli genişletilmiş görüntüleri gibi çıktı dosyaları veya DNN model dosyalarının nerede depolanacağını belirlemek için kullanılır. Depolama bağlamları hakkında daha fazla bilgi için bkz: [StorageContext belgelerine](https://review.docs.microsoft.com/en-us/python/api/cvtk.core.context.storagecontext?view=azure-python&branch=smoke-test). 

Normalde, depolama içerik açıkça ayarlanmış olması gerekmez. Ancak, 25 MB sınırına Azure Machine Learning çalışma ekranı tarafından uygulanan proje boyutu olmaması için çıkış dizini için Azure Machine Learning paketi için bilgisayar Vizyon Azure Machine Learning proje dışında bir yere ayarlayın ("... /.. /.. /.. / cvtk_output "). Artık gerekli sonra "cvtk_output" directory kaldırdığınızdan emin olun.


```python
import warnings
warnings.filterwarnings("ignore")
import json, numpy as np, os, timeit 
from azureml.logging import get_azureml_logger
from imgaug import augmenters
from IPython.display import display
from sklearn import svm
from cvtk import ClassificationDataset, CNTKTLModel, Context, Splitter, StorageContext
from cvtk.augmentation import augment_dataset
from cvtk.core.classifier import ScikitClassifier
from cvtk.evaluation import ClassificationEvaluation, graph_roc_curve, graph_pr_curve, graph_confusion_matrix, basic_plot
import matplotlib.pyplot as plt
%matplotlib inline

# Disable printing of logging messages
from azuremltkbase.logging import ToolkitLogger
ToolkitLogger.getInstance().setEnabled(False)

# Set storage context.
out_root_path = "../../../cvtk_output"
Context.create(outputs_path=out_root_path, persistent_path=out_root_path, temp_path=out_root_path)
```




    {
        "storage": {
            "outputs_path": "../../../cvtk_output",
            "persistent_path": "../../../cvtk_output",
            "temp_path": "../../../cvtk_output"
        }
    }



## <a name="create-a-dataset"></a>Veri kümesi oluşturma

Bağımlılıkları alınan ve depolama bağlamını ayarlayın sonra veri kümesi nesnesi oluşturabilirsiniz.

Bu nesne için bilgisayar Vizyon Azure Machine Learning paketiyle oluşturmak için yerel diskte görüntüleri kök dizininin sağlar. Bu dizin, Yemek takımı dataset aynı genel yapısını diğer bir deyişle izleyin, gerçek görüntüleri alt dizinleri içerir:
- kök
    - Etiket 1
    - 2 etiketi
    - ...
    - Etiket n
  
Bir görüntü sınıflandırma model için farklı bir veri kümesi kök yolu değiştirirken kadar kolay eğitim `dataset_location` farklı görüntülere işaret etmek için aşağıdaki kodu.


```python
# Root image directory 
dataset_location = os.path.abspath(os.path.join(os.getcwd(), "../sample_data/imgs_recycling"))

dataset_name = 'recycling'
dataset = ClassificationDataset.create_from_dir(dataset_name, dataset_location)
print("Dataset consists of {} images with {} labels.".format(len(dataset.images), len(dataset.labels)))
print("Select information for image 2: name={}, label={}, unique id={}.".format(
    dataset.images[2].name, dataset.images[2]._labels[0].name, dataset.images[2]._storage_id))
```

    F1 2018-04-23 17:12:57,593 INFO azureml.vision:machine info {"is_dsvm": true, "os_type": "Windows"} 
    F1 2018-04-23 17:12:57,599 INFO azureml.vision:dataset creating dataset for scenario=classification 
    Dataset consists of 63 images with 4 labels.
    Select information for image 2: name=msft-plastic-bowl20170725152154282.jpg, label=bowl, unique id=3.

Veri kümesi nesnesi kullanarak görüntüleri indirmek için işlevsellik sağlar [Bing görüntü arama API](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/). 

Arama sorguları iki tür desteklenir: 
+ Normal metin sorguları
+ Görüntü URL sorguları

Bu sorguları sınıfı etiketi ile birlikte bir JSON olarak kodlanmış metin dosyası içinde sağlanmalıdır. Örneğin:

```json
{
    "bowl": [
                    "plastic bowl",
                    "../imgs_recycling/bowl"
    ],
    "cup": [
                    "plastic cup",
                    "../imgs_recycling/cup",
                    "http://cdnimg2.webstaurantstore.com/images/products/main/123662/268841/dart-solo-ultra-clear-conex-tp12-12-oz-pet-plastic-cold-cup-1000-case.jpg"
    ],
    "cutlery": [
                    "plastic cutlery",
                    "../imgs_recycling/cutlery",
                    "http://img4.foodservicewarehouse.com/Prd/1900SQ/Fineline_2514-BO.jpg"
    ],
    "plate": [
                    "plastic plate",
                    "../imgs_recycling/plate"
    ]
}
```

Ayrıca, açıkça Bing görüntü arama API anahtarını içermesi için bir bağlam nesnesi oluşturmanız gerekir. Bu bir Bing görüntü arama API aboneliği gerektirir.

## <a name="visualize-and-annotate-images"></a>Görselleştirme ve görüntüleri açıklama ekleme

Görüntüleri ve aşağıdaki pencere öğesini kullanarak veri kümesi nesnesi doğru etiketler görselleştirebilirsiniz. 

"Pencere öğesi Javascript algılanmadı" hatayla karşılaşırsanız, bunu çözmek için şu komutu çalıştırın:
<br>`jupyter nbextension enable --py --sys-prefix widgetsnbextension`


```python
from ui_utils.ui_annotation import AnnotationUI
annotation_ui = AnnotationUI(dataset, Context.get_global_context())
display(annotation_ui.ui)
```

![Azure Machine Learning veri kümesi](media/how-to-build-deploy-image-classification-models/image_annotation.png)

## <a name="augment-images"></a>Görüntüleri büyütmek

[ `augmentation` Modülü](https://docs.microsoft.com/en-us/python/api/cvtk.augmentation) açıklanan tüm dönüştürmeleri kullanarak bir veri kümesi nesnesi büyütmek için işlevsellik sağlar [imgaug](https://github.com/aleju/imgaug) kitaplığı. Resim dönüşümleri gruplandırılabilir tek bir ardışık düzende kanaldaki tüm dönüştürmeleri aynı anda uygulanır; Bu durumda her görüntü. 

Farklı augmentation'a adımları ayrı ayrı uygulamak istediğiniz veya farklı şekilde, birden çok ardışık düzen tanımlayın ve onlara geçirin *augment_dataset* işlevi. Daha fazla bilgi ve görüntü augmentation'a örnekleri için bkz: [imgaug belgelerine](https://github.com/aleju/imgaug).

Genişletilmiş görüntüler eğitim kümesine ekleme, küçük veri kümeleri için özellikle yararlıdır. DNN eğitim işlem artan eğitim resimlerinin sayısı nedeniyle daha yavaş olduğundan, deneme augmentation'a olmadan Başlat öneririz.


```python
# Split the dataset into train and test  
train_set_orig, test_set = dataset.split(train_size = 0.66, stratify = "label")
print("Number of training images = {}, test images = {}.".format(train_set_orig.size(), test_set.size()))
```

    F1 2018-04-23 17:13:01,780 INFO azureml.vision:splitter splitting a dataset 
    F1 2018-04-23 17:13:01,805 INFO azureml.vision:dataset creating dataset for scenario=classification 
    F1 2018-04-23 17:13:01,809 INFO azureml.vision:dataset creating dataset for scenario=classification 
    Number of training images = 41, test images = 22.
    


```python
augment_train_set = False

if augment_train_set:
    aug_sequence = augmenters.Sequential([
            augmenters.Fliplr(0.5),             # horizontally flip 50% of all images
            augmenters.Crop(percent=(0, 0.1)),  # crop images by 0-10% of their height/width
        ])
    train_set = augment_dataset(train_set_orig, [aug_sequence])
    print("Number of original training images = {}, with augmented images included = {}.".format(train_set_orig.size(), train_set.size()))
else:
    train_set = train_set_orig  
```

## <a name="define-dnn-models"></a>DNN modelleri tanımlayın

Aşağıdaki pretrained derin sinir ağı modeller bu paketle desteklenir: 
+ AlexNet
+ Resnet 18
+ Resnet 34
+ Resnet 50
+ Resnet 101
+ Resnet 152

Bu DNNs sınıflandırıcı olarak ya da featurizer olarak kullanılabilir. 

Ağlar hakkında daha fazla bilgi bulunabilir [burada](https://github.com/Microsoft/CNTK/blob/master/PretrainedModels/Image.md), ve aktarım öğrenme için temel bir giriş [burada](https://blog.slavv.com/a-gentle-intro-to-transfer-learning-2c0b674375a0).

Varsayılan görüntü sınıflandırma bu paket için 224 x 224 piksel çözünürlüğü ve Resnet 18 DNN parametreleridir. Bu parametreler de çok çeşitli görevler üzerinde çalışmak için seçilmedi. Doğruluk genellikle, örneğin, 500 x 500 piksel resim çözünürlüğü artırma ve/veya derin modeli (Resnet-50) seçme tarafından geliştirilebilir. Parametrelerinin değiştirilmesi, eğitim süresini önemli bir artış, ancak gelebilir. Makaleyi bakın [doğruluğunu artırmak nasıl](https://docs.microsoft.com/azure/machine-learning/service/how-to-improve-accuracy-for-computer-vision-models).


```python
# Default parameters (224 x 224 pixels resolution, Resnet-18)
lr_per_mb = [0.05]*7 + [0.005]*7 +  [0.0005]
mb_size = 32
input_resoluton = 224
base_model_name = "ResNet18_ImageNet_CNTK"

# Suggested parameters for 500 x 500 pixels resolution, Resnet-50
# (see in the Appendix "How to improve accuracy", last row in table)
# lr_per_mb   = [0.01] * 7 + [0.001] * 7 + [0.0001]
# mb_size    = 8
# input_resoluton = 500
# base_model_name = "ResNet50_ImageNet_CNTK"

# Initialize model
dnn_model = CNTKTLModel(train_set.labels,
                       base_model_name=base_model_name,
                       image_dims = (3, input_resoluton, input_resoluton))
```

    Successfully downloaded ResNet18_ImageNet_CNTK
    

## <a name="train-the-classifier"></a>Tren sınıflandırıcı

Önceden eğitilen DNN için aşağıdaki yöntemlerden birini seçebilirsiniz.

  - **DNN iyileştirme**, sınıflandırma doğrudan gerçekleştirmek için DNN eğitir. DNN eğitim yavaş olsa da, tüm ağ ağırlıkları en iyi doğruluğu vermek için eğitim sırasında geliştirilebilir beri genellikle en iyi sonuçlar verecektir.

  - **DNN featurization**, DNN olarak çalıştığı-görüntünün küçük boyutlu bir gösterimini elde edilir (512, 2048 veya 4096 gezinen). Bu gösterim sonra ayrı bir sınıflandırıcı eğitmek için girdi olarak kullanılır. DNN değişmeden tutulur olduğundan, doğruluk kadar iyi değildir ancak bu yaklaşım çok daha hızlı DNN iyileştirme için karşılaştırılır. Yine de, doğrusal SVM gibi harici bir sınıflandırıcı (aşağıdaki kodda gösterildiği gibi) eğitim sağlam bir temel sağlar ve bir sorun uygulanabilirliğini anlamak.
  
TensorBoard eğitim ilerleme görselleştirmek için kullanılabilir. TensorBoard etkinleştirmek için:
1. Parametre ekleme `tensorboard_logdir=PATH` aşağıdaki kodda gösterildiği gibi
1. Komutunu kullanarak TensorBoard istemcisini başlattığınızda `tensorboard --logdir=PATH` yeni bir konsol içinde.
1. Varsayılan olarak localhost:6006 olan TensorBoard tarafından belirtildiği gibi bir web tarayıcısı açın. 


```python
# Train either the DNN or a SVM as classifier 
classifier_name = "dnn"

if classifier_name.lower() == "dnn":  
    dnn_model.train(train_set, lr_per_mb = lr_per_mb, mb_size = mb_size, eval_dataset=test_set) #, tensorboard_logdir=r"tensorboard"
    classifier = dnn_model
elif classifier_name.lower() == "svm":
    learner = svm.LinearSVC(C=1.0, class_weight='balanced', verbose=0)
    classifier = ScikitClassifier(dnn_model, learner = learner)
    classifier.train(train_set)
else:
    raise Exception("Classifier unknown: " + classifier)   
```

    F1 2018-04-23 17:13:28,238 INFO azureml.vision:Fit starting in experiment  1541466320 
    F1 2018-04-23 17:13:28,239 INFO azureml.vision:model starting training for scenario=classification 
    <class 'int'>
    1 worker
    Training transfer learning model for 15 epochs (epoch_size = 41).
    non-distributed mode
    Training 15741700 parameters in 53 parameter tensors.
    Training 15741700 parameters in 53 parameter tensors.
    Learning rate per minibatch: 0.05
    Momentum per minibatch: 0.9
    PROGRESS: 0.00%
    Finished Epoch[1 of 15]: [Training] loss = 2.820586 * 41, metric = 68.29% * 41 5.738s (  7.1 samples/s);
    Evaluation Set Error :: 29.27%
    Finished Epoch[2 of 15]: [Training] loss = 0.286728 * 41, metric = 9.76% * 41 0.752s ( 54.5 samples/s);
    Evaluation Set Error :: 34.15%
    Finished Epoch[3 of 15]: [Training] loss = 0.206938 * 41, metric = 4.88% * 41 0.688s ( 59.6 samples/s);
    Evaluation Set Error :: 41.46%
    Finished Epoch[4 of 15]: [Training] loss = 0.098931 * 41, metric = 2.44% * 41 0.785s ( 52.2 samples/s);
    Evaluation Set Error :: 48.78%
    Finished Epoch[5 of 15]: [Training] loss = 0.046547 * 41, metric = 0.00% * 41 0.724s ( 56.6 samples/s);
    Evaluation Set Error :: 43.90%
    Finished Epoch[6 of 15]: [Training] loss = 0.059709 * 41, metric = 4.88% * 41 0.636s ( 64.5 samples/s);
    Evaluation Set Error :: 34.15%
    Finished Epoch[7 of 15]: [Training] loss = 0.005817 * 41, metric = 0.00% * 41 0.710s ( 57.7 samples/s);
    Evaluation Set Error :: 14.63%
    Learning rate per minibatch: 0.005
    Finished Epoch[8 of 15]: [Training] loss = 0.014917 * 41, metric = 0.00% * 41 0.649s ( 63.2 samples/s);
    Evaluation Set Error :: 9.76%
    Finished Epoch[9 of 15]: [Training] loss = 0.040539 * 41, metric = 2.44% * 41 0.777s ( 52.8 samples/s);
    Evaluation Set Error :: 9.76%
    Finished Epoch[10 of 15]: [Training] loss = 0.024606 * 41, metric = 0.00% * 41 0.626s ( 65.5 samples/s);
    Evaluation Set Error :: 7.32%
    PROGRESS: 0.00%
    Finished Epoch[11 of 15]: [Training] loss = 0.004225 * 41, metric = 0.00% * 41 0.656s ( 62.5 samples/s);
    Evaluation Set Error :: 4.88%
    Finished Epoch[12 of 15]: [Training] loss = 0.004364 * 41, metric = 0.00% * 41 0.702s ( 58.4 samples/s);
    Evaluation Set Error :: 4.88%
    Finished Epoch[13 of 15]: [Training] loss = 0.007974 * 41, metric = 0.00% * 41 0.721s ( 56.9 samples/s);
    Evaluation Set Error :: 4.88%
    Finished Epoch[14 of 15]: [Training] loss = 0.000655 * 41, metric = 0.00% * 41 0.711s ( 57.7 samples/s);
    Evaluation Set Error :: 4.88%
    Learning rate per minibatch: 0.0005
    Finished Epoch[15 of 15]: [Training] loss = 0.024865 * 41, metric = 0.00% * 41 0.688s ( 59.6 samples/s);
    Evaluation Set Error :: 4.88%
    Stored trained model at ../../../cvtk_output\model_trained\ImageClassification.model
    F1 2018-04-23 17:13:45,097 INFO azureml.vision:Fit finished in experiment  1541466320 
    


```python
# Plot how the training and test accuracy increases during gradient descent. 
if classifier_name == "dnn":
    [train_accs, test_accs, epoch_numbers] = classifier.train_eval_accs
    plt.xlabel("Number of training epochs") 
    plt.ylabel("Classification accuracy") 
    train_plot = plt.plot(epoch_numbers, train_accs, 'r-', label = "Training accuracy")
    test_plot = plt.plot(epoch_numbers, test_accs, 'b-.', label = "Test accuracy")
    plt.legend()
```

![PNG](media/how-to-build-deploy-image-classification-models/output_17_0.png)


## <a name="evaluate-and-visualize-model-performance"></a>Değerlendirin ve model performans Görselleştirme

Değerlendirme modülünü kullanarak bir bağımsız sınama veri kümesi üzerinde eğitilen model performansını değerlendirebilirsiniz. Bu hesaplar değerlendirme ölçümleri bazıları şunlardır:
 
+ Doğruluk (varsayılan sınıf ortalaması)
+ PR eğri
+ ROC eğrisi
+ Eğri altında alanı
+ Karışıklığı Matrisi


```python
# Run the classifier on all test set images
ce = ClassificationEvaluation(classifier, test_set, minibatch_size = mb_size)

# Compute Accuracy and the confusion matrix
acc = ce.compute_accuracy()
print("Accuracy = {:2.2f}%".format(100*acc))
cm  = ce.compute_confusion_matrix()
print("Confusion matrix = \n{}".format(cm))

# Show PR curve, ROC curve, and confusion matrix
fig, ((ax1, ax2, ax3)) = plt.subplots(1,3)
fig.set_size_inches(18, 4)
graph_roc_curve(ce, ax=ax1)
graph_pr_curve(ce, ax=ax2)
graph_confusion_matrix(ce, ax=ax3)
plt.show()
```

    F1 2018-04-23 17:14:37,449 INFO azureml.vision:evaluation doing evaluation for scenario=classification 
    F1 2018-04-23 17:14:37,450 INFO azureml.vision:model scoring dataset for scenario=classification 
    Accuracy = 95.45%
    Confusion matrix = 
    [[ 0  1  0  1]
     [ 0  7  0  0]
     [ 0  0  2  0]
     [ 0  0  0 11]]
    


![PNG](media/how-to-build-deploy-image-classification-models/output_20_1.png)



```python
# Results viewer UI
labels = [l.name for l in dataset.labels] 
pred_scores = ce.scores #classification scores for all images and all classes
pred_labels = [labels[i] for i in np.argmax(pred_scores, axis=1)]

from ui_utils.ui_results_viewer import ResultsUI
results_ui = ResultsUI(test_set, Context.get_global_context(), pred_scores, pred_labels)
display(results_ui.ui)
```

![Azure Machine Learning veri kümesi](media/how-to-build-deploy-image-classification-models/Image_Classification_Results.png)


```python
# Precision / recall curve UI
precisions, recalls, thresholds = ce.compute_precision_recall_curve() 
thresholds = list(thresholds)
thresholds.append(thresholds[-1])
from ui_utils.ui_precision_recall import PrecisionRecallUI
pr_ui = PrecisionRecallUI(100*precisions[::-1], 100*recalls[::-1], thresholds[::-1])
display(pr_ui.ui) 
```

![Azure Machine Learning veri kümesi](media/how-to-build-deploy-image-classification-models/image_precision_curve.png)

## <a name="operationalization-deploy-and-consume"></a>Operationalization: dağıtma ve kullanma

Operationalization yayımlama modelleri ve web Hizmetleri olarak kod sürecini ve bu hizmetleri iş sonuçlar tüketiminin ' dir. 

Modelinizi eğitildi sonra tüketim kullanarak bir web hizmeti olarak modelin dağıtabilirsiniz [Azure Machine Learning CLI](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/cli-for-azure-machine-learning). Modellerinizi, LocalMachine ya da Azure kapsayıcı Hizmeti'ni (ACS) küme dağıtılabilir. ACS kullanarak, web hizmetiniz elle ölçeklendirme veya otomatik ölçeklendirmeyi işlevini kullanın.

**Oturum ile Azure CLI**

Kullanarak bir [Azure](https://azure.microsoft.com/) hesap geçerli bir abonelik ile aşağıdaki CLI komutu kullanarak oturum açın:
<br>`az login`

+ Başka bir Azure aboneliğine geçmeniz komutu kullanın:
<br>`az account set --subscription [your subscription name]`

+ Geçerli model yönetim hesabı görmek için komutu kullanın:
  <br>`az ml account modelmanagement show`

**Oluşturun ve dağıtım ortamınızı ayarlayın**

Yalnızca dağıtım ortamınızı bir kez ayarlamanız gerekir. Henüz yoksa, şimdi kullanarak dağıtım ortamınızı ayarlayın [bu yönergeleri](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/deployment-setup-configuration#environment-setup). 

Yerel veya doğru şekilde, gereksinimleri temelinde küme dağıtım kurulum adımlarını izleyin.
+ Yerel dağıtımları, Linux ve Windows 10 için ancak Windows veri bilimi VM veya derin öğrenme VM için desteklenir. 
+ Küme ortamı dağıtımları, Linux ve Windows için desteklenir. 

Etkin dağıtım ortamınızı görmek için aşağıdaki CLI komutu kullanın:
<br>`az ml env show`
   
Oluşturun ve dağıtım ortamı için örnek Azure CLI komutu

```CLI
az provider register -n Microsoft.MachineLearningCompute
az provider register -n Microsoft.ContainerRegistry
az provider register -n Microsoft.ContainerService
az ml env setup --cluster -n [your environment name] -l [Azure region e.g. westcentralus] [-g [resource group]]
az ml env set -n [environment name] -g [resource group]
az ml env cluster
```
    
### <a name="manage-web-services-and-deployments"></a>Web Hizmetleri ve Dağıtımları yönet

Aşağıdaki API, web Hizmetleri olarak modelleri dağıtmak, bu web hizmetleri yönetmek ve dağıtımları yönetmek için kullanılabilir.

|Görev|API|
|----|----|
|Dağıtım nesnesi oluşturun|`deploy_obj = AMLDeployment(deployment_name=deployment_name, associated_DNNModel=dnn_model, aml_env="cluster")`
|Web hizmetini dağıtma|`deploy_obj.deploy()`|
|Puan görüntüsü|`deploy_obj.score_image(local_image_path_or_image_url)`|
|Web hizmeti Sil|`deploy_obj.delete()`|
|Web hizmeti olmadan docker yansıması oluştur|`deploy_obj.build_docker_image()`|
|Var olan dağıtım listesi|`AMLDeployment.list_deployment()`|
|Hizmet dağıtım adıyla varsa silin|`AMLDeployment.delete_if_service_exist(deployment_name)`|

**API belgelerine:** başvurun [paketini başvuru belgeleri](https://aka.ms/aml-packages/vision) her modül ve sınıfı için ayrıntılı başvuru için.

**CLI başvuru:** daha gelişmiş işlemleri, dağıtımla ilgili başvurmak için [model yönetim CLI başvuru](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/model-management-cli-reference).

**Azure portalında dağıtım Yönetimi**: izleme ve dağıtımlarınızda yönetme [Azure portal](https://ms.portal.azure.com/). Azure portalından adını kullanarak, Machine Learning modeli yönetim hesabı sayfasını bulun. Model yönetim hesabı sayfasına gidin > Model Yönetim > hizmetler.


```python
# ##### OPTIONAL###### 
# Interactive CLI setup helper, including model management account and deployment environment.
# If you haven't setup you CLI before or if you want to change you CLI settings, you can use this block to help you interactively.
#
# UNCOMMENT THE FOLLOWING LINES IF YOU HAVE NOT CREATED OR SET THE MODEL MANAGEMENT ACCOUNT AND DEPLOYMENT ENVIRONMENT
#
# from azuremltkbase.deployment import CliSetup
# CliSetup().run()
```


```python
# # Optional. Persist your model on disk and reuse it later for deployment. 
# from cvtk import TFFasterRCNN, Context
# import os
# save_model_path = os.path.join(Context.get_global_context().storage.persistent_path, "saved_classifier.model")
# # Save model to disk
# dnn_model.serialize(save_model_path)
# # Load model from disk
# dnn_model = CNTKTLModel.deserialize(save_model_path)
```


```python
from cvtk.operationalization import AMLDeployment

# set deployment name
deployment_name = "wsdeployment"

# Optional Azure Machine Learning deployment cluster name (environment name) and resource group name
# If you don't provide here. It will use the current deployment environment (you can check with CLI command "az ml env show").
azureml_rscgroup = "<resource group>"
cluster_name = "<cluster name>"

# If you provide the cluster information, it will use the provided cluster to deploy. 
# Example: deploy_obj = AMLDeployment(deployment_name=deployment_name, associated_DNNModel=dnn_model,
#                            aml_env="cluster", cluster_name=cluster_name, resource_group=azureml_rscgroup, replicas=1)

# Create deployment object
deploy_obj = AMLDeployment(deployment_name=deployment_name, aml_env="cluster", associated_DNNModel=dnn_model, replicas=1)

# Check if the deployment name exists, if yes remove it first.
if deploy_obj.is_existing_service():
    AMLDeployment.delete_if_service_exist(deployment_name)
    
# Create the web service
print("Deploying to Azure cluster...")
deploy_obj.deploy()
print("Deployment DONE")
```

### <a name="consume-the-web-service"></a>Web hizmetini kullanma 

Bir web hizmeti olarak modeli dağıttığınızda, bu yöntemlerden birini kullanarak web hizmetiyle görüntüleri puan:

- Dağıtım nesnesi kullanılarak ile doğrudan web hizmeti puan `deploy_obj.score_image(image_path_or_url)`

- Hizmet uç noktası URL'si ve hizmet anahtarını (hiçbiri yerel dağıtım için) kullanın: `AMLDeployment.score_existing_service_with_image(image_path_or_url, service_endpoint_url, service_key=None)`

- Web hizmeti uç noktası doğrudan Puanlama amacıyla, HTTP istekleri oluşturur. Bu seçenek Gelişmiş kullanıcılar için kullanılır.

### <a name="score-with-existing-deployment-object"></a>Var olan dağıtım nesnesiyle puan

```
deploy_obj.score_image(image_path_or_url)
```


```python
# Score with existing deployment object

# Score local image with file path
print("Score local image with file path")
image_path_or_url = test_set.images[0].storage_path
print("Image source:",image_path_or_url)
serialized_result_in_json = deploy_obj.score_image(image_path_or_url, image_resize_dims=[224,224])
print("serialized_result_in_json:", serialized_result_in_json)

# Score image url and remove image resizing
print("Score image url")
image_path_or_url = "https://cvtkdata.blob.core.windows.net/publicimages/microsoft_logo.jpg"
print("Image source:",image_path_or_url)
serialized_result_in_json = deploy_obj.score_image(image_path_or_url)
print("serialized_result_in_json:", serialized_result_in_json)

# Score image url with added paramters. Add softmax to score.
print("Score image url with added paramters. Add softmax to score")
from cvtk.utils.constants import ClassificationRESTApiParamters
image_path_or_url = "https://cvtkdata.blob.core.windows.net/publicimages/microsoft_logo.jpg"
print("Image source:",image_path_or_url)
serialized_result_in_json = deploy_obj.score_image(image_path_or_url, image_resize_dims=[224,224], parameters={ClassificationRESTApiParamters.ADD_SOFTMAX:True})
print("serialized_result_in_json:", serialized_result_in_json)
```


```python
# Time image scoring
import timeit

for img_index, img_obj in enumerate(test_set.images[:10]):
    print("Calling API for image {} of {}: {}...".format(img_index, len(test_set.images), img_obj.name))
    tic = timeit.default_timer()
    return_json = deploy_obj.score_image(img_obj.storage_path, image_resize_dims=[224,224])
    print("   Time for API call: {:.2f} seconds".format(timeit.default_timer() - tic))
    print(return_json)
```

### <a name="score-with-service-endpoint-url-and-service-key"></a>Hizmet uç noktasının URL'sini ve hizmet anahtarı ile puan

`AMLDeployment.score_existing_service_with_image(image_path_or_url, service_endpoint_url, service_key=None)`

```python
# Import related classes and functions
from cvtk.operationalization import AMLDeployment

service_endpoint_url = "" # please replace with your own service url
service_key = "" # please replace with your own service key
# score local image with file path
image_path_or_url = test_set.images[0].storage_path
print("Image source:",image_path_or_url)
serialized_result_in_json = AMLDeployment.score_existing_service_with_image(image_path_or_url,service_endpoint_url, service_key = service_key)
print("serialized_result_in_json:", serialized_result_in_json)

# score image url
image_path_or_url = "https://cvtkdata.blob.core.windows.net/publicimages/microsoft_logo.jpg"
print("Image source:",image_path_or_url)
serialized_result_in_json = AMLDeployment.score_existing_service_with_image(image_path_or_url,service_endpoint_url, service_key = service_key, image_resize_dims=[224,224])
print("serialized_result_in_json:", serialized_result_in_json)
```

### <a name="score-endpoint-with-http-request-directly"></a>Http isteği doğrudan puan uç noktası

Aşağıdaki kod örneği, HTTP isteği doğrudan Python içinde oluşturur. Ancak, diğer programlama dillerinde olarak yapabilirsiniz.


```python
def score_image_list_with_http(images, service_endpoint_url, service_key=None, parameters={}):
    """Score image list with http request

    Args:
        images(list): list of (input image file path, base64 image string, url or buffer)
        service_endpoint_url(str): endpoint url
        service_key(str): service key, None for local deployment.
        parameters(dict): service additional paramters in dictionary


    Returns:
        result (list): list of serialized result 
    """
    import requests
    from io import BytesIO
    import base64
    routing_id = ""

    if service_key is None:
        headers = {'Content-Type': 'application/json',
                   'X-Marathon-App-Id': routing_id}
    else:
        headers = {'Content-Type': 'application/json',
                   "Authorization": ('Bearer ' + service_key), 'X-Marathon-App-Id': routing_id}
    payload = []
    for image in images:
        encoded = None
        # read image
        with open(image,'rb') as f:
            image_buffer = BytesIO(f.read()) ## Getting an image file represented as a BytesIO object
        # convert your image to base64 string
        encoded = base64.b64encode(image_buffer.getvalue())
        image_request = {"image_in_base64": "{0}".format(encoded), "parameters": parameters}
        payload.append(image_request)
    body = json.dumps(payload)
    r = requests.post(service_endpoint_url, data=body, headers=headers)
    try:
        result = json.loads(r.text)
    except:
        raise ValueError("Incorrect output format. Result cant not be parsed: " + r.text)
    return result

# Test with images
images = [test_set.images[0].storage_path, test_set.images[1].storage_path] # A list of local image files
score_image_list_with_http(images, service_endpoint_url, service_key)
```

### <a name="parse-serialized-result-from-web-service"></a>Seri hale getirilmiş sonuç web hizmetinden ayrıştırılamıyor

Web hizmeti çıktısını bir JSON dizesidir. Bu JSON dizesi farklı DNN modeli sınıflarla ayrıştıramıyor.


```python
image_path_or_url = test_set.images[0].storage_path
print("Image source:",image_path_or_url)
serialized_result_in_json = deploy_obj.score_image(image_path_or_url, image_resize_dims=[224,224])
print("serialized_result_in_json:", serialized_result_in_json)
```


```python
# Parse result from json string
import numpy as np
parsed_result = CNTKTLModel.parse_serialized_result(serialized_result_in_json)
print("Parsed result:", parsed_result)
# Map result to image class
class_index = np.argmax(np.array(parsed_result))
print("Class index:", class_index)
dnn_model.class_map
print("Class label:", dnn_model.class_map[class_index])
```


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning paketi hakkında daha fazla bilgi için bilgisayar Vizyon bu makalelerde öğrenin:

+ Bilgi edinmek için nasıl [Bu modelin doğruluğunu artırmak](how-to-improve-accuracy-for-computer-vision-models.md).

+ Okuma [genel bakış paketini ve nasıl yükleneceği öğrenin](https://aka.ms/aml-packages/vision).

+ Araştır [başvuru belgelerini](https://docs.microsoft.com/python/api/overview/azure-machine-learning/computer-vision) bu paket için.

+ Hakkında bilgi edinin [Azure Machine Learning için diğer Python paketlerini](reference-python-package-overview.md).
