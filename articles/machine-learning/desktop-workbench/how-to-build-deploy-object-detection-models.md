---
title: Oluşturun ve görüntü işleme için Azure Machine Learning paketi kullanarak bir nesne algılama modeli dağıtın.
description: Oluşturma, eğitme, sınama ve Azure Machine Learning paket için görüntü işleme kullanan bir bilgisayar işleme nesne algılama modeli dağıtma konusunda bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: netahw
author: nhaiby
ms.date: 06/01/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 1451cdc5efcb62cff5c5d3725d5b15eb44be1755
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51613673"
---
# <a name="build-and-deploy-object-detection-models-with-azure-machine-learning"></a>Azure Machine Learning ile nesne algılama modellerini Derleme ve dağıtma

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]


Bu makalede, kullanmayı öğrenin **görüntü işleme için Azure Machine Learning paketi** eğitmek için test etme ve dağıtma bir [daha hızlı R CNN](https://arxiv.org/abs/1506.01497) nesne algılama modeli. 

Çok sayıda bilgisayar işleme etki alanındaki sorunları, nesne algılama kullanarak çözülebilir. Bu sorunlar, nesne bir değişken sayısı üzerindeki resmin Bul modeller oluşturma içerir. 

Oluşturma ve bu modeli Bu paket dağıtma, aşağıdaki adımları izleyerek gidin:
1.  Veri kümesi oluşturma
2.  Derin sinir ağı (DNN) modeli tanımı
3.  Model Eğitimi
4.  Değerlendirme ve görselleştirme
5.  Web hizmeti dağıtımı
6.  Web hizmeti yük testi

Bu örnekte, TensorFlow, derin öğrenme framework kullanılır, eğitim gerçekleştirilir yerel olarak desteklenen GPU makinede gibi [derin öğrenme veri bilimi sanal makinesi](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview), ve dağıtımı, Azure ML kullanıma hazır hale getirme CLI'yı kullanır.

Başvurun [paketini başvuru belgeleri](https://aka.ms/aml-packages/vision) her modülü ve sınıf için ayrıntılı başvuru.

## <a name="prerequisites"></a>Önkoşullar

1. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

1. Aşağıdaki hesapları ve uygulama kurmalı ve yüklü:
   - Bir Azure Machine Learning Denemesi hesabı 
   - Bir Azure Machine Learning Model Yönetimi hesabı
   - Azure Machine Learning Workbench'in yüklü olması

   Bu üç henüz oluşturduysanız veya yüklü izleyin [Azure Machine Learning hızlı ve Workbench'i yükleme](../desktop-workbench/quickstart-installation.md) makalesi. 

1. Görüntü işleme için Azure Machine Learning paketi yüklü olmalıdır. Bilgi edinmek için nasıl [burada bu paketi yüklemek](https://aka.ms/aml-packages/vision).

## <a name="sample-data-and-notebook"></a>Örnek veriler ve not defteri

### <a name="get-the-jupyter-notebook"></a>Jupyter not defterini alma

Örneği çalıştırmak için Not defterini indirme kendiniz burada açıklanmıştır.

> [!div class="nextstepaction"]
> [Jupyter not defterini alma](https://aka.ms/aml-packages/vision/notebooks/object_detection)

### <a name="load-the-sample-data"></a>Örnek verileri yükleme

Bu Tanıtım için 30 görüntüleri ve 8 sınıfları (eggBox, joghurt, ketchup, mantar, kurt, orange, sıkıştırma ve su) oluşan bir veri kümesi içinde refrigerators Market öğelerinin sağlanır. Her jpg resmi için bir ek açıklama xml dosyası benzer ada sahip yoktur. 

Aşağıdaki şekil, önerilen klasör yapısını gösterir. 

![klasör yapısı](media/how-to-build-deploy-object-detection-models/data_directory.JPG)

## <a name="image-annotation"></a>Görüntü ek açıklaması

Ek açıklama nesnesi konumları eğitmek ve bir nesne algılayıcısı değerlendirmek için gereklidir. [LabelImg](https://tzutalin.github.io/labelImg) görüntü ek açıklama eklemek için kullanılan bir açık kaynak ek açıklama aracı. LabelImg, bu paket tarafından okunabilir Pascal VOC biçiminde resim başına bir xml dosyasına yazar. 


```python
import warnings
warnings.filterwarnings("ignore")
import os, time
from cvtk.core import Context, ObjectDetectionDataset, TFFasterRCNN
from cvtk.evaluation import DetectionEvaluation
from cvtk.evaluation.evaluation_utils import graph_error_counts
from cvtk.utils import detection_utils

# Disable printing of logging messages
from azuremltkbase.logging import ToolkitLogger
ToolkitLogger.getInstance().setEnabled(False)

from matplotlib import pyplot as plt
# Display the images
%matplotlib inline
```

## <a name="create-a-dataset"></a>Veri kümesi oluşturma

Kendi ilgili bir sınırlama kutusu Not içeren bir görüntü kümesi içeren bir CVTK veri kümesi oluşturursunuz. Bu örnekte, sağlanan buzdolabınız görüntüleri ".. / foods/sample_data/Eğitim"klasörü kullanılır. Yalnızca JPEG resimleri desteklenir.


```python
image_folder = "detection/sample_data/foods/train"
data_train = ObjectDetectionDataset.create_from_dir(dataset_name='training_dataset', data_dir=image_folder,
                                                    annotations_dir="Annotations", image_subdirectory='JPEGImages')

# Show some statistics of the training image, and also give one example of the ground truth rectangle annotations
data_train.print_info()
_ = data_train.images[2].visualize_bounding_boxes(image_size = (10,10))
```

    F1 2018-05-25 23:12:21,727 INFO azureml.vision:machine info {"is_dsvm": true, "os_type": "Windows"} 
    F1 2018-05-25 23:12:21,733 INFO azureml.vision:dataset creating dataset for scenario=detection 
    Dataset name: training_dataset
    Total classes: 8, total images: 25
    Label-wise object counts:
        Label eggBox: 20 objects
        Label joghurt: 20 objects
        Label ketchup: 20 objects
        Label mushroom: 20 objects
        Label mustard: 20 objects
        Label orange: 20 objects
        Label squash: 40 objects
        Label water: 20 objects
    Bounding box width and height distribution:
        Bounding box widths  0/5/25/50/75/95/100-percentile: 54/61/79/117/133/165/311 pixels
        Bounding box heights 0/5/25/50/75/95/100-percentile: 48/58/75/124/142/170/212 pixels
    


![PNG](media/how-to-build-deploy-object-detection-models/output_6_1.JPG)


## <a name="define-a-model"></a>Bir modeli tanımlamak

Bu örnekte, daha hızlı R CNN modeli kullanılır. Bu model tanımlarken, çeşitli parametrelerin sağlanabilir. (Sonraki bölüme bakın) eğitimi için kullanılan parametreler yanı sıra, bu parametreleri anlamını ya da CVTK'ın API belgeleri veya üzerinde bulunabilir [Tensorflow nesne algılama Web sitesi](https://github.com/tensorflow/models/tree/master/research/object_detection). Daha hızlı R CNN modeli hakkında daha fazla bilgi şu adreste bulunabilir: [bu bağlantıyı](https://docs.microsoft.com/cognitive-toolkit/Object-Detection-using-Faster-R-CNN#technical-details). Hızlı R-CNN üzerinde bu modeli temel alır ve daha fazla bilgi bulunabilir [burada](https://docs.microsoft.com/cognitive-toolkit/Object-Detection-using-Fast-R-CNN#algorithm-details).


```python
score_threshold = 0.0       # Threshold on the detection score, use to discard lower-confidence detections.
max_total_detections = 300  # Maximum number of detections. A high value slows down training but might increase accuracy.
my_detector = TFFasterRCNN(labels=data_train.labels, 
                           score_threshold=score_threshold, 
                           max_total_detections=max_total_detections)
```

## <a name="train-the-model"></a>Modeli eğitme

ResNet50 COCO eğitimli daha hızlı R-CNN modeliyle eğitim için başlangıç noktası olarak kullanılır. 

Algılayıcı eğitmek için kod eğitimi adım sayısı eğitim (~ 5 dakika GPU ile) daha hızlı çalışır. böylece 350'e ayarlanır. Uygulamada, eğitim kümesine görüntü sayısını en az 10 kez ayarlayın.

Bu örnekte, algılayıcısı eğitimi adım sayısı, 350 hızlı eğitim için ayarlanır. Ancak, uygulamada bir iyi adımları için 10 veya daha fazla kez görüntülerinin sayısını Eğitim kümesi için udur.

Eğitim için iki anahtar Parametreler şunlardır:
- Num_seps bağımsız değişkeni tarafından temsil edilen modeli eğitmek için adım sayısı. Her adım, bir toplu iş boyutu bir minibatch modeli eğitir.
- Oranı öğrenme, hangi initial_learning_rate tarafından ayarlanabilir.

```python
print("tensorboard --logdir={}".format(my_detector.train_dir))

# to get good results, use a larger value for num_steps, e.g., 5000.
num_steps = 350
learning_rate = 0.001 # learning rate

start_train = time.time()
my_detector.train(dataset=data_train, num_steps=num_steps, 
                  initial_learning_rate=learning_rate)
end_train = time.time()
print(end_train-start_train)
```

    tensorboard --logdir=C:\Users\lixun\Desktop\AutoDL\CVTK\Src\API\cvtk_output\temp_faster_rcnn_resnet50\models\train
    F1 2018-05-25 23:12:22,764 INFO azureml.vision:Fit starting in experiment  1125722225 
    F1 2018-05-25 23:12:22,767 INFO azureml.vision:model starting trainging for scenario=detection 
    Using existing checkpoint file that's saved at 'C:\Users\lixun\Desktop\AutoDL\CVTK\Src\API\cvtk_output\models\detection\faster_rcnn_resnet50_coco_2018_01_28\model.ckpt.index'.
    TFRecords creation started.
    F1 2018-05-25 23:12:22,773 INFO On image 0 of 25
    TFRecords creation completed.
    Training started.
    Training progressing: step 0 ...
    Training progressing: step 100 ...
    Training progressing: step 200 ...
    Training progressing: step 300 ...
    F1 2018-05-25 23:18:02,730 INFO Graph Rewriter optimizations enabled
    Converted 275 variables to const ops.
    F1 2018-05-25 23:18:10,722 INFO 2953 ops in the final graph.
    F1 2018-05-25 23:18:24,244 INFO azureml.vision:Fit finished in experiment  1125722225 
    Training completed.
    361.604615688324
    

TensorBoard eğitim ilerleme durumunu görselleştirmek için kullanılabilir. TensorBoard olayları model nesnenin train_dir özniteliği tarafından belirtilen klasörde yer alır. TensorBoard görüntülemek için aşağıdaki adımları izleyin:
1. 'Tensorboard--logdir' için bir komut satırı ile başlayan ve BT çıktıyı kopyalayın. 
2. Döndürülen URL'yi komut satırından TensorBoard görüntülemek için bir web tarayıcısı kopyalayın. 

TensorBoard aşağıdaki ekran görüntüsüne benzer görünmelidir. Doldurulacak eğitim klasörü için birkaç dakika sürer. TensorBoard göstermezse kadar yukarı doğru ilk zaman try tekrarlanan 1-2 adımları.  

![tensorboard](media/how-to-build-deploy-object-detection-models/tensorboard.JPG)

## <a name="evaluate-the-model"></a>Modeli değerlendirme

'Değerlendirme' yöntemi, model değerlendirmek için kullanılır. Bu işlev, girdi olarak ObjectDetectionDataset nesneyi gerektirir. Değerlendirme veri kümesi, eğitim veri kümesi için kullanılan aynı işlevi kullanılarak oluşturulabilir. Desteklenen ölçüm için tanımlandığı gibi ortalama kesinlik olduğunu [PASCAL VOC sınama](http://host.robots.ox.ac.uk/pascal/VOC/pubs/everingham10.pdf).  


```python
image_folder = "detection/sample_data/foods/test"
data_val = ObjectDetectionDataset.create_from_dir(dataset_name='val_dataset', data_dir=image_folder)
eval_result = my_detector.evaluate(dataset=data_val)
```

    F1 2018-05-25 23:18:24,253 INFO azureml.vision:dataset creating dataset for scenario=detection 
    F1 2018-05-25 23:18:24,286 INFO On image 0 of 5
    F1 2018-05-25 23:18:29,300 INFO Starting evaluation at 2018-05-26-03:18:29
    F1 2018-05-25 23:18:32,403 INFO Creating detection visualizations.
    F1 2018-05-25 23:18:33,158 INFO Detection visualizations written to summary with tag image-0.
    F1 2018-05-25 23:18:33,518 INFO Creating detection visualizations.
    F1 2018-05-25 23:18:34,342 INFO Detection visualizations written to summary with tag image-1.
    F1 2018-05-25 23:18:34,714 INFO Creating detection visualizations.
    F1 2018-05-25 23:18:35,470 INFO Detection visualizations written to summary with tag image-2.
    F1 2018-05-25 23:18:35,835 INFO Creating detection visualizations.
    F1 2018-05-25 23:18:36,654 INFO Detection visualizations written to summary with tag image-3.
    F1 2018-05-25 23:18:37,010 INFO Creating detection visualizations.
    F1 2018-05-25 23:18:37,798 INFO Detection visualizations written to summary with tag image-4.
    F1 2018-05-25 23:18:37,804 INFO Running eval batches done.
    F1 2018-05-25 23:18:37,805 INFO # success: 5
    F1 2018-05-25 23:18:37,806 INFO # skipped: 0
    F1 2018-05-25 23:18:38,119 INFO Writing metrics to tf summary.
    F1 2018-05-25 23:18:38,121 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/eggBox: 1.000000
    F1 2018-05-25 23:18:38,205 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/joghurt: 0.942857
    F1 2018-05-25 23:18:38,206 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/ketchup: 1.000000
    F1 2018-05-25 23:18:38,207 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/mushroom: 1.000000
    F1 2018-05-25 23:18:38,208 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/mustard: 1.000000
    F1 2018-05-25 23:18:38,209 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/orange: 1.000000
    F1 2018-05-25 23:18:38,210 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/squash: 1.000000
    F1 2018-05-25 23:18:38,211 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/water: 1.000000
    F1 2018-05-25 23:18:38,211 INFO PASCAL/Precision/mAP@0.5IOU: 0.992857
    F1 2018-05-25 23:18:38,253 INFO Metrics written to tf summary.
    F1 2018-05-25 23:18:38,254 INFO Finished evaluation!
    

Değerlendirme sonuçları temiz bir biçimde yazdırılabilir.


```python
# print out the performance metric values
for label_obj in data_train.labels:
    label = label_obj.name
    key = 'PASCAL/PerformanceByCategory/AP@0.5IOU/' + label
    print('{0: <15}: {1: <3}'.format(label, round(eval_result[key], 2)))
print('{0: <15}: {1: <3}'.format("overall:", round(eval_result['PASCAL/Precision/mAP@0.5IOU'], 2))) 
```

    joghurt        : 0.94
    squash         : 1.0
    mushroom       : 1.0
    eggBox         : 1.0
    ketchup        : 1.0
    mustard        : 1.0
    water          : 1.0
    orange         : 1.0
    overall:       : 0.99
    

Benzer şekilde, doğruluğu, model Eğitim kümesi üzerinde işlem. Bunu yaptığınızda bu yardımcı olan eğitim yakınsanmış iyi bir çözüme emin olun. Doğruluk eğitim sonra başarılı eğitim genellikle yakın %100 ayarlayın.

Harita değerleri ve görüntüler öngörülen sınırlama kutusu dahil olmak üzere, TensorBoard değerlendirme sonuçlarını da görüntülenebilir. Çıktının TensorBoard istemcisini başlatmak için komut satırı penceresi aşağıdaki kodu kopyalayın. Burada bir bağlantı noktası değeri 8008 eğitim durumu görüntülemek için kullanarak 6006, varsayılan değeri ile çakışma olmasını önlemek için kullanılır.


```python
print("tensorboard --logdir={} --port=8008".format(my_detector.eval_dir))
```

    tensorboard --logdir=C:\Users\lixun\Desktop\AutoDL\CVTK\Src\API\cvtk_output\temp_faster_rcnn_resnet50\models\eval --port=8008
    

## <a name="score-an-image"></a>Görüntü Puanlama

Eğitilen model performansını memnun olduğunuzda, model nesnenin 'puan' işlevi, yeni görüntüleri puanlamak için kullanılabilir. Döndürülen puanları 'görselleştirebilir' işlevini kullanarak görselleştirilebilir. 


```python
image_path = data_val.images[1].storage_path
detections_dict = my_detector.score(image_path)
path_save = "./scored_images/scored_image_preloaded.jpg"
ax = detection_utils.visualize(image_path, detections_dict, image_size=(8, 12))
path_save_dir = os.path.dirname(os.path.abspath(path_save))
os.makedirs(path_save_dir, exist_ok=True)
ax.get_figure().savefig(path_save)
```

![PNG](media/how-to-build-deploy-object-detection-models/output_20_0.JPG)

##  <a name="save-the-model"></a>Modeli kaydedin

Eğitilen model diske kaydedilebilir ve aşağıdaki kodu örneklerde gösterildiği gibi belleğe geri yüklendi.


```python
save_model_path = "./frozen_model/faster_rcnn.model"
my_detector.save(save_model_path)
```

    F1 2018-05-25 23:18:55,166 INFO Graph Rewriter optimizations enabled
    Converted 275 variables to const ops.
    F1 2018-05-25 23:19:03,867 INFO 2953 ops in the final graph.
    

## <a name="load-the-saved-model-for-scoring"></a>Puanlama için kaydedilen model yüklenemiyor

Kaydedilen modelini kullanmak için 'load' işlevini kullanarak belleğe modeli yükler. Yalnızca bir kez yüklemeniz gerekir. 

```python
my_detector_loaded = TFFasterRCNN.load(save_model_path)
```

Modele yüklendikten sonra görüntü veya görüntülerin listesini puanlamak için kullanılabilir. Tek bir görüntü için bir sözlük anahtarları 'detection_boxes', 'detection_scores' ve 'num_detections' gibi döndürülür. Giriş, görüntülerin listesini ise bir görüntüye karşılık gelen bir sözlük ile bir sözlük listesi döndürülür. 


```python
detections_dict = my_detector_loaded.score(image_path)
```

Puanları 0,5 yukarıda algılanan nesnelerle etiketleri, puanları ve koordinatları dahil yazdırılabilir.


```python
look_up = dict((v,k) for k,v in my_detector.class_map.items())
n_obj = 0
for i in range(detections_dict['num_detections']):
    if detections_dict['detection_scores'][i] > 0.5:
        n_obj += 1
        print("Object {}: label={:11}, score={:.2f}, location=(top: {:.2f}, left: {:.2f}, bottom: {:.2f}, right: {:.2f})".format(
            i, look_up[detections_dict['detection_classes'][i]], 
            detections_dict['detection_scores'][i], 
            detections_dict['detection_boxes'][i][0],
            detections_dict['detection_boxes'][i][1], 
            detections_dict['detection_boxes'][i][2],
            detections_dict['detection_boxes'][i][3]))    
        
print("\nFound {} objects in image {}.".format(n_obj, image_path))           
```

    Object 0: label=squash     , score=0.99, location=(top: 0.74, left: 0.30, bottom: 0.84, right: 0.42)
    Object 1: label=squash     , score=0.98, location=(top: 0.27, left: 0.21, bottom: 0.37, right: 0.33)
    Object 2: label=orange     , score=0.98, location=(top: 0.31, left: 0.39, bottom: 0.37, right: 0.48)
    Object 3: label=joghurt    , score=0.98, location=(top: 0.57, left: 0.29, bottom: 0.67, right: 0.39)
    Object 4: label=eggBox     , score=0.97, location=(top: 0.41, left: 0.53, bottom: 0.49, right: 0.69)
    Object 5: label=water      , score=0.95, location=(top: 0.23, left: 0.51, bottom: 0.37, right: 0.57)
    Object 6: label=mustard    , score=0.88, location=(top: 0.61, left: 0.47, bottom: 0.66, right: 0.57)
    Object 7: label=ketchup    , score=0.80, location=(top: 0.62, left: 0.62, bottom: 0.68, right: 0.72)
    
    Found 8 objects in image ../sample_data/foods/test\JPEGImages\10.jpg.
    

Puanları önce olduğu gibi görselleştirin.


```python
path_save = "./scored_images/scored_image_frozen_graph.jpg"
ax = detection_utils.visualize(image_path, detections_dict, path_save=path_save, image_size=(8, 12))
# ax.get_figure() # use this code extract the returned image
```

![PNG](media/how-to-build-deploy-object-detection-models/output_30_0.JPG)

## <a name="operationalization-deploy-and-consume"></a>Kullanıma hazır hale getirme:, dağıtma ve kullanma

Kullanıma hazır hale getirme işlemini yayımlama modelleri ve web Hizmetleri olarak kod ve iş sonuçlar bu hizmetlerin kullanımını ' dir. 

Modelinizi eğitildi sonra tüketim kullanmak için bir web hizmeti olarak modelin dağıtabilir [Azure Machine Learning CLI](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/cli-for-azure-machine-learning). Modellerinizi, yerel makine veya Azure Container Service (ACS) kümesine dağıtılabilir. ACS kullanarak, web hizmetini el ile ölçeklendirebilir veya otomatik ölçeklendirme işlevini kullanın.

**Oturum Azure CLI ile oturum açın**

Kullanarak bir [Azure](https://azure.microsoft.com/) hesap ile geçerli bir abonelik, aşağıdaki CLI komutunu kullanarak oturum açın:
<br>`az login`

+ Başka bir Azure aboneliğine geçiş yapmak için komutu kullanın:
<br>`az account set --subscription [your subscription name]`

+ Geçerli model Yönetimi hesabı görmek için komutu kullanın:
  <br>`az ml account modelmanagement show`

**Oluşturun ve küme dağıtım ortamınızı ayarlayın**

Yalnızca bir kez dağıtım ortamınızı ayarlamanız gerekir. Henüz yoksa, şimdi kullanarak dağıtım ortamı oluşturmanız [bu yönergeleri](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/deployment-setup-configuration#environment-setup). 

Etkin dağıtım ortamınızı görmek için aşağıdaki CLI komutunu kullanın:
<br>`az ml env show`
   
Örnek Azure CLI komutunu oluşturmak ve dağıtım ortamı ayarlamak için

```CLI
az provider register -n Microsoft.MachineLearningCompute
az provider register -n Microsoft.ContainerRegistry
az provider register -n Microsoft.ContainerService
az ml env setup --cluster -n [your environment name] -l [Azure region e.g. westcentralus] [-g [resource group]]
az ml env set -n [environment name] -g [resource group]
az ml env cluster
```
    
### <a name="manage-web-services-and-deployments"></a>Web Hizmetleri ve dağıtımları yönetin

Aşağıdaki API, Modellerinizi web Hizmetleri olarak dağıtmanıza, bu web hizmetlerini yönetme ve dağıtımları yönetmek için kullanılabilir.

|Görev|API|
|----|----|
|Dağıtım nesnesi oluşturma|`deploy_obj = AMLDeployment(deployment_name=deployment_name, associated_DNNModel=dnn_model, aml_env="cluster")`
|Web hizmetini dağıtma|`deploy_obj.deploy()`|
|Puan görüntüsü|`deploy_obj.score_image(local_image_path_or_image_url)`|
|Web hizmeti silme|`deploy_obj.delete()`|
|Web hizmeti olmadan docker görüntüsü oluşturun|`deploy_obj.build_docker_image()`|
|Mevcut bir dağıtım listesi|`AMLDeployment.list_deployment()`|
|Hizmet dağıtımı adı ile varsa silin|`AMLDeployment.delete_if_service_exist(deployment_name)`|

**API belgeleri:** başvurun [paketini başvuru belgeleri](https://aka.ms/aml-packages/vision) her modülü ve sınıf için ayrıntılı başvuru.

**CLI başvurusu:** daha gelişmiş işlemler, dağıtımla ilgili başvurmak için [model Yönetimi CLI başvurusu](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/model-management-cli-reference).

**Azure portalında dağıtım Yönetimi**: izleme ve yönetme dağıtımlarınızda [Azure portalında](https://ms.portal.azure.com/). Azure portalından adını kullanarak Machine Learning Model Yönetimi hesap sayfanızda bulun. Ardından Model Yönetimi hesabı sayfasına gidin > Model Yönetimi > hizmetler.

```python
# ##### OPTIONAL - Interactive CLI setup helper ###### 
# # Interactive CLI setup helper, including model management account and deployment environment.
# # If you haven't setup you CLI before or if you want to change you CLI settings, you can use this block to help you interactively.
# # UNCOMMENT THE FOLLOWING LINES IF YOU HAVE NOT CREATED OR SET THE MODEL MANAGEMENT ACCOUNT AND DEPLOYMENT ENVIRONMENT

# from azuremltkbase.deployment import CliSetup
# CliSetup().run()
```


```python
from cvtk.operationalization import AMLDeployment

# set deployment name
deployment_name = "wsdeployment"

# Create deployment object
# It will use the current deployment environment (you can check it with CLI command "az ml env show").
deploy_obj = AMLDeployment(deployment_name=deployment_name, aml_env="cluster", associated_DNNModel=my_detector, replicas=1)

# Alternatively, you can provide azure machine learning deployment cluster name (environment name) and resource group name
# to deploy your model. It will use the provided cluster to deploy. To do that, please uncomment the following lines to create 
# the deployment object.

# azureml_rscgroup = "<resource group>"
# cluster_name = "<cluster name>"
# deploy_obj = AMLDeployment(deployment_name=deployment_name, associated_DNNModel=my_detector,
#                            aml_env="cluster", cluster_name=cluster_name, resource_group=azureml_rscgroup, replicas=1)

# Check if the deployment name exists, if yes remove it first.
if deploy_obj.is_existing_service():
    AMLDeployment.delete_if_service_exist(deployment_name)
    
# create the webservice
print("Deploying to Azure cluster...")
deploy_obj.deploy()
print("Deployment DONE")
```

### <a name="consume-the-web-service"></a>Web hizmetini kullanma

Web hizmeti oluşturulduktan sonra dağıtılan Web hizmeti ile görüntüleri puan. Birkaç seçeneğiniz vardır:

   - Dağıtım nesnesiyle birlikte bir Web hizmetini doğrudan puan: deploy_obj.score_image(image_path_or_url) 
   - Ya da hizmet anahtarını (hiçbiri yerel dağıtımı için) ve hizmet uç noktası URL'si ile kullanabilirsiniz: AMLDeployment.score_existing_service_with_image (image_path_or_url, service_endpoint_url, service_key = None)
   - Web Hizmeti uç noktası (Gelişmiş kullanıcılar için) doğrudan puanlamak için http isteği oluşturur.

### <a name="score-with-existing-deployment-object"></a>Var olan dağıtım nesnesiyle Puanlama
```
deploy_obj.score_image(image_path_or_url)
```


```python
# Score with existing deployment object

# Score local image with file path
print("Score local image with file path")
image_path_or_url = data_train.images[0].storage_path
print("Image source:",image_path_or_url)
serialized_result_in_json = deploy_obj.score_image(image_path_or_url, image_resize_dims=[224,224])
print("serialized_result_in_json:", serialized_result_in_json[:50])

# Score image url and remove image resizing
print("Score image url")
image_path_or_url = "https://cvtkdata.blob.core.windows.net/publicimages/microsoft_logo.jpg"
print("Image source:",image_path_or_url)
serialized_result_in_json = deploy_obj.score_image(image_path_or_url)
print("serialized_result_in_json:", serialized_result_in_json[:50])

```


```python
# Time image scoring
import timeit

num_images = 3
for img_index, img_obj in enumerate(data_train.images[:num_images]):
    print("Calling API for image {} of {}: {}...".format(img_index, num_images, img_obj.name))
    tic = timeit.default_timer()
    return_json = deploy_obj.score_image(img_obj.storage_path, image_resize_dims=[224,224])
    print("   Time for API call: {:.2f} seconds".format(timeit.default_timer() - tic))
```

### <a name="score-with-service-endpoint-url-and-service-key"></a>Hizmet uç noktasının URL'sini ve hizmet anahtarı ile Puanlama
```
    AMLDeployment.score_existing_service_with_image(image_path_or_url, service_endpoint_url, service_key=None)
```


```python
# Import related classes and functions
from cvtk.operationalization import AMLDeployment

service_endpoint_url = "http://xxx" # please replace with your own service url
service_key = "xxx" # please replace with your own service key

# score image url
image_path_or_url = "https://cvtkdata.blob.core.windows.net/publicimages/microsoft_logo.jpg"
print("Image source:",image_path_or_url)
serialized_result_in_json = AMLDeployment.score_existing_service_with_image(image_path_or_url,service_endpoint_url, service_key = service_key, image_resize_dims=[224,224])
print("serialized_result_in_json:", serialized_result_in_json[:50])
```

### <a name="score-endpoint-with-http-request-directly"></a>Http isteği ile uç noktası doğrudan Puanlama
Http isteği doğrudan Python'da oluşturmak için bazı örnek kodlar aşağıda verilmiştir. Diğer programlama dillerinde yapabilirsiniz.


```python
def score_image_with_http(image, service_endpoint_url, service_key=None, parameters={}):
    """Score local image with http request

    Args:
        image (str): Image file path
        service_endpoint_url(str): web service endpoint url
        service_key(str): Service key. None for local deployment.
        parameters (dict): Additional request parameters in dictionary. Default is {}.


    Returns:
        str: serialized result 
    """
    import requests
    from io import BytesIO
    import base64
    import json

    if service_key is None:
        headers = {'Content-Type': 'application/json'}
    else:
        headers = {'Content-Type': 'application/json',
                   "Authorization": ('Bearer ' + service_key)}
    payload = []
    encoded = None
    
    # Read image
    with open(image,'rb') as f:
        image_buffer = BytesIO(f.read()) ## Getting an image file represented as a BytesIO object
        
    # Convert your image to base64 string
    # image_in_base64 : "b'{base64}'"
    encoded = base64.b64encode(image_buffer.getvalue())
    image_request = {"image_in_base64": "{0}".format(encoded), "parameters": parameters}
    payload.append(image_request)
    body = json.dumps(payload)
    r = requests.post(service_endpoint_url, data=body, headers=headers)
    try:
        result = json.loads(r.text)
        json.loads(result[0])
    except:
        raise ValueError("Incorrect output format. Result cant not be parsed: " + r.text)
    return result[0]

```

### <a name="parse-serialized-result-from-webservice"></a>Web hizmeti seri hale getirilmiş sonuçtan ayrıştırılamıyor
Json dizesindeki ayrıştırılabilir web hizmetinden oluşur.


```python
image_path_or_url = image_path
print("Image source:",image_path_or_url)
serialized_result_in_json = deploy_obj.score_image(image_path_or_url)
print("serialized_result_in_json:", serialized_result_in_json[:50])
```


```python
# Parse result from json string
import numpy as np
parsed_result = TFFasterRCNN.parse_serialized_result(serialized_result_in_json)
print("Parsed result:", parsed_result)
```


```python
ax = detection_utils.visualize(image_path, parsed_result)
path_save = "./scored_images/scored_image_web.jpg"
path_save_dir = os.path.dirname(os.path.abspath(path_save))
os.makedirs(path_save_dir, exist_ok=True)
ax.get_figure().savefig(path_save)
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning paketi hakkında daha fazla görüntü işleme için bu makalelerde öğrenin:

+ Okuma [genel bakış paketini](https://aka.ms/aml-packages/vision).

+ Keşfedin [başvuru belgeleri](https://docs.microsoft.com/python/api/overview/azure-machine-learning/computer-vision) bu paket için.

+ Hakkında bilgi edinin [diğer Azure Machine Learning Python paketleri](reference-python-package-overview.md).
