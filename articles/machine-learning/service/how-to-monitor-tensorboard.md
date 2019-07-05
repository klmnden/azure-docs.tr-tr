---
title: Denemeleri TensorBoard ve Azure Machine Learning hizmeti ile görselleştirin
description: Denemeyi çalıştırma geçmişlerini görselleştirmek için TensorBoard başlatın ve ayarlama ve yeniden eğitme hiper parametre için olası alanları tanımlayın.
services: machine-learning
author: maxluk
ms.author: maxluk
ms.reviewer: nibaccam
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 06/28/2019
ms.openlocfilehash: babd4cdf8b7ed9e04b4bd975d840688b27439c4f
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67560846"
---
# <a name="visualize-experiment-runs-and-metrics-with-tensorboard-and-azure-machine-learning"></a>Deneme çalıştırmalarınızın ve ölçümleri TensorBoard ve Azure Machine Learning ile görselleştirin

Bu makalede, TensorBoard kullanarak deneme çalıştırmalarınızın ve ölçümleri görüntüleme hakkında bilgi edinin [ `tensorboard` paket](https://docs.microsoft.com/python/api/azureml-tensorboard/?view=azure-ml-py) ana Azure Machine Learning'de hizmeti SDK'sı. Bu sayede deneme çalıştırmalarınızın inceledi bir kez daha iyi ayarlayın ve makine öğrenimi modelleri yeniden eğitme.

[TensorBoard](https://www.tensorflow.org/tensorboard/r1/overview) inceleyerek ve denemeyi yapısı ve performans anlamak için web uygulamaları paketidir.

Azure Machine Learning denemeleri TensorBoard nasıl başlatma, deneme türüne bağlıdır:
+ Denemenizi gibi PyTorch, Chainer ve TensorFlow denemeleri TensorBoard tarafından tüketilebilir günlük dosyaları yerel olarak çıkarır sonra yapabilecekleriniz [TensorBoard doğrudan başlatma](#direct) denemeden ait çalıştırma geçmişi. 

+ Yerel olarak TensorBoard tüketilebilir, gibi Scikit-öğrenme veya Azure Machine Learning denemeleri gibi çıkış dosyalarını yoksa denemeleri kullanmak [ `export_to_tensorboard()` yöntemi](#export) çalıştırma geçmişleri TensorBoard günlükleri olarak dışarı aktarmak ve başlatmak için Buradan TensorBoard. 

## <a name="prerequisites"></a>Önkoşullar

* Denemelerinizi TensorBoard ve görünüm çalıştırma geçmişlerini denemenizi başlatmak için daha önce Ölçümler ve performans izleme için günlüğe kaydetmeyi etkinleştirdiyseniz gerekir.  

* Bu nasıl yapılır kod aşağıdaki ortamları birini çalıştırabilirsiniz: 

    * Azure Machine Learning not defteri VM - herhangi bir indirme veya yükleme gerekli

        * Tamamlamak [bulut tabanlı bir not defteri hızlı](quickstart-run-cloud-notebook.md#create-notebook) adanmış notebook sunucusu önceden yüklenmiş SDK'sı ve örnek depoyu oluşturmak için.

        * Tamamlandı ve genişletilmiş not defterleri bu dizine giderek iki not defteri sunucusu üzerinde samples klasöründe Bul: **Yardım-How-to-kullanın-azureml > Eğitim ile derin öğrenme**.
        * export-run-history-to-run-history.ipynb
        * tensorboard.ipynb

    * Kendi Juptyer not defteri sunucusu
      * Kullanım [çalışma makale oluşturma](setup-create-workspace.md) için
          * [Azure Machine Learning SDK'sını yükleyin](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) ile `tensorboard` ek
          * Bir çalışma alanı ve yapılandırma dosyası (config.json) oluşturma
  
<a name="direct"></a>
## <a name="option-1-directly-view-run-history-in-tensorboard"></a>1\. seçenek: Doğrudan TensorBoard içinde çalıştırma geçmişi görünümü

Bu seçenek, çıkışlar PyTorch, bağlayıcı gibi TensorBoard tarafından tüketilebilir dosyaları yerel olarak oturum ve TensorFlow denemeleri denemeleri için çalışır. Denemeniz durumunda değil kullanırsanız [ `export_to_tensorboard()` yöntemi](#export) yerine.

Aşağıdaki örnek kod [MNIST tanıtım deneme](https://raw.githubusercontent.com/tensorflow/tensorflow/r1.8/tensorflow/examples/tutorials/mnist/mnist_with_summaries.py) TensorFlow'ın depodan bir uzak işlem hedef Azure Machine Learning işlem. Ardından, biz modelimizi SDK'ın özel ile eğitme [TensorFlow estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py), ve ardından bu TensorFlow keşfedin, diğer bir deyişle, TensorBoard olay dosyaları yerel olarak veren deneme karşı TensorBoard başlatın.

### <a name="set-experiment-name-and-create-project-folder"></a>Deney adı ayarlayın ve proje klasörü oluşturun

Biz burada deneme adını ve alt klasör oluşturun. 
 
```python
from os import path, makedirs
experiment_name = 'tensorboard-demo'

# experiment folder
exp_dir = './sample_projects/' + experiment_name

if not path.exists(exp_dir):
    makedirs(exp_dir)

```

### <a name="download-tensorflow-demo-experiment-code"></a>TensorFlow tanıtım deneme kodunu indirin

TensorFlow'ın havuz bir MNIST tanıtım kapsamlı TensorBoard araçlarla vardır. Biz bulunmayan ve herhangi bir Azure Machine Learning hizmeti ile çalışabilmesi için bu tanıtım 's kodunun alter gerekir. Aşağıdaki kodda, MNIST kodu indirmek ve müşterilerimize yeni oluşturulan deneme klasörüne kaydedin.

```python
import requests
import os

tf_code = requests.get("https://raw.githubusercontent.com/tensorflow/tensorflow/r1.8/tensorflow/examples/tutorials/mnist/mnist_with_summaries.py")
with open(os.path.join(exp_dir, "mnist_with_summaries.py"), "w") as file:
    file.write(tf_code.text)
```
MNIST kod dosyası boyunca mnist_with_summaries.py, olduğuna dikkat edin, bu çağrı satırları `tf.summary.scalar()`, `tf.summary.histogram()`, `tf.summary.FileWriter()` vs. Bu yöntem grubu, log ve çalıştırma geçmişini içine denemelerinizi başlıca ölçümlerini etiketi. `tf.summary.FileWriter()` TensorBoard bunları dışına görselleştirmeler oluşturmak izin veren oturum deneme ölçümlerinizi verileri serileştirir gibi özellikle önemlidir.

 ### <a name="configure-experiment"></a>Deneme yapılandırma

Aşağıdaki, biz bizim deneme yapılandırmak ve günlükleri ve verileri için dizinleri ayarlama. Bu günlükler Yapıt TensorBoard daha sonra erişir hizmetine yüklenir.

>[!Note]
> TensorFlow Bu örnekte, yerel makinenizde TensorFlow yüklemeniz gerekir. Ayrıca, yerel makine TensorBoard çalıştırılanlar olduğundan TensorBoard Modülü (diğer bir deyişle, bir TensorFlow ile dahil) bu not defterinin çekirdeğe erişilebilir olması gerekir.

```Python
import azureml.core
from azureml.core import Workspace
from azureml.core import Experiment

ws = Workspace.from_config()

# create directories for experiment logs and dataset
logs_dir = os.path.join(os.curdir, "logs")
data_dir = os.path.abspath(os.path.join(os.curdir, "mnist_data"))

if not path.exists(data_dir):
    makedirs(data_dir)

os.environ["TEST_TMPDIR"] = data_dir

# Writing logs to ./logs results in their being uploaded to Artifact Service,
# and thus, made accessible to our TensorBoard instance.
arguments_list = ["--log_dir", logs_dir]

# Create an experiment
exp = Experiment(ws, experiment_name)
```

### <a name="create-a-cluster-for-your-experiment"></a>Denemeniz için küme oluşturma
Denemelerinizi herhangi bir ortamda oluşturulabilir ve TensorBoard karşı çalıştırma geçmişi denemeyi başlatmak yine de kullanabilirsiniz ancak bu deneme bir AmlCompute küme oluştururuz. 

```Python
from azureml.core.compute import ComputeTarget, AmlCompute

cluster_name = "cpucluster"

cts = ws.compute_targets
found = False
if cluster_name in cts and cts[cluster_name].type == 'AmlCompute':
   found = True
   print('Found existing compute target.')
   compute_target = cts[cluster_name]
if not found:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2', 
                                                           max_nodes=4)

    # create the cluster
    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

compute_target.wait_for_completion(show_output=True, min_node_count=None)

# use get_status() to get a detailed status for the current cluster. 
# print(compute_target.get_status().serialize())
```

### <a name="submit-run-with-tensorflow-estimator"></a>TensorFlow tahmin aracı ile Çalıştır gönderin

TensorFlow estimator işlem hedefi üzerinde bir TensorFlow eğitimi işini başlatmak için basit bir yol sağlar. Genel uygulanır [ `estimator` ](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) herhangi bir çerçeveyi desteklemek için kullanılan sınıf. Genel tahmin Aracı'nı kullanarak modellerin eğitimi hakkında daha fazla bilgi için bkz. [tahmin Aracı'nı kullanarak Azure Machine Learning modellerini eğitin](how-to-train-ml-models.md)

```Python
from azureml.train.dnn import TensorFlow
script_params = {"--log_dir": "./logs"}

tf_estimator = TensorFlow(source_directory=exp_dir,
                          compute_target=compute_target,
                          entry_script='mnist_with_summaries.py',
                          script_params=script_params)

run = exp.submit(tf_estimator)
```

### <a name="launch-tensorboard"></a>TensorBoard başlatın

Çalıştırma sırasında veya tamamlandıktan sonra TensorBoard başlatabilirsiniz. Aşağıda, TensorBoard nesne örneği oluştururuz `tb`alır, denemeyi çalıştırma, yüklenmiş geçmişi `run`ve ardından TensorBoard ile başlatır `start()` yöntemi. 
  
[TensorBoard Oluşturucusu](https://docs.microsoft.com/python/api/azureml-tensorboard/azureml.tensorboard.tensorboard?view=azure-ml-py) bir dizi alan çalıştırmalar, bu nedenle emin olun ve bunu tek öğeli bir dizi geçirin.

```python
from azureml.tensorboard import Tensorboard

tb = Tensorboard([run])

# If successful, start() returns a string with the URI of the instance.
tb.start()

# After your job completes, be sure to stop() the streaming otherwise it will continue to run. 
tb.stop()
```

<a name="export"></a>

## <a name="option-2-export-history-as-log-to-view-in-tensorboard"></a>2\. seçenek: Geçmiş TensorBoard içinde görüntülemek için günlük Dışarı Aktar

Aşağıdaki kod, örnek deneme ayarlama ayarlar, Azure Machine Learning çalıştırma geçmişi API'leri kullanarak günlüğe kaydetme işlemi başlar ve tüketilebilir günlüklerine tarafından TensorBoard görselleştirme için çalıştırma geçmişi denemeyi dışarı aktarır. 

### <a name="set-up-experiment"></a>Deneme ayarlama

Aşağıdaki kod ayarlar yeni bir deneme ayarlama ve çalıştırma dizinine adları `root_run`. 

```python
from azureml.core import Workspace, Experiment
import azuremml.core

# set experiment name and run name
ws = Workspace.from_config()
experiment_name = 'export-to-tensorboard'
exp = Experiment(ws, experiment_name)
root_run = exp.start_logging()
```

Burada ailelere veri kümesi--scikit ile birlikte gelen yerleşik küçük bir veri kümesi yüklediğimiz-test ve eğitim kümeleri halinde bölme ve öğrenin.

```Python
from sklearn.datasets import load_diabetes
from sklearn.linear_model import Ridge
from sklearn.metrics import mean_squared_error
from sklearn.model_selection import train_test_split
X, y = load_diabetes(return_X_y=True)
columns = ['age', 'gender', 'bmi', 'bp', 's1', 's2', 's3', 's4', 's5', 's6']
x_train, x_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=0)
data = {
    "train":{"x":x_train, "y":y_train},        
    "test":{"x":x_test, "y":y_test}
}
```

### <a name="run-experiment-and-log-metrics"></a>Denemeyi çalıştırın ve oturum ölçümleri

Bu kod, biz bir doğrusal regresyon modeli eğitmek ve ana ölçümleri, alfa katsayısı, oturum `alpha` ve ortalama karesi alınmış hata `mse`, geçmiş çalıştırın.

```Python
from tqdm import tqdm
alphas = [.1, .2, .3, .4, .5, .6 , .7]
# try a bunch of alpha values in a Linear Regression (aka Ridge regression) mode
for alpha in tqdm(alphas):
  # create child runs and fit lines for the resulting models
  with root_run.child_run("alpha" + str(alpha)) as run
 
   reg = Ridge(alpha=alpha)
   reg.fit(data["train"]["x"], data["train"]["y"])    
 
   preds = reg.predict(data["test"]["x"])
   mse = mean_squared_error(preds, data["test"]["y"])
   # End train and eval

# log alpha, mean_squared_error and feature names in run history
   root_run.log("alpha", alpha)
   root_run.log("mse", mse)
```

### <a name="export-runs-to-tensorboard"></a>Dışarı aktarma için TensorBoard çalıştırır.

SDK'ları ile [export_to_tensorboard()](https://docs.microsoft.com/python/api/azureml-tensorboard/azureml.tensorboard.export?view=azure-ml-py) yöntemi, biz dışarı aktarabilir, TensorBoard günlükleri, Azure machine learning denemesine çalıştırma geçmişini biz TensorBoard görüntüleyebilmeniz.  

Aşağıdaki kodda, oluşturduğumuz klasörü `logdir` bizim geçerli çalışma dizininde. Bu klasör burada biz bizim deneme çalıştırma geçmişini dışarı aktarır ve oturumu `root_run` ve çalıştırılan tamamlandı olarak işaretleyin. 

```Python
from azureml.tensorboard.export import export_to_tensorboard
import os

logdir = 'exportedTBlogs'
log_path = os.path.join(os.getcwd(), logdir)
try:
    os.stat(log_path)
except os.error:
    os.mkdir(log_path)
print(logdir)

# export run history for the project
export_to_tensorboard(root_run, logdir)

root_run.complete()
```

>[!Note]
 Çalıştırma adını belirterek TensorBoard için belirli bir çalıştırma verebilirsiniz  `export_to_tensorboard(run_name, logdir)`

Başlatma ve durdurma TensorBoard bu deneme için sunduğumuz çalıştırma geçmişini dışarı kez, biz başlatabilir TensorBoard ile [start()](https://docs.microsoft.com/python/api/azureml-tensorboard/azureml.tensorboard.tensorboard?view=azure-ml-py#start-start-browser-false-) yöntemi. 

```Python
from azureml.tensorboard import Tensorboard

# The TensorBoard constructor takes an array of runs, so be sure and pass it in as a single-element array here
tb = Tensorboard([], local_root=logdir, port=6006)

# If successful, start() returns a string with the URI of the instance.
tb.start()
```

İşiniz bittiğinde çağırdığınızdan emin olun [stop()](https://docs.microsoft.com/python/api/azureml-tensorboard/azureml.tensorboard.tensorboard?view=azure-ml-py#stop--) TensorBoard nesnesinin yöntemi. Aksi takdirde, TensorBoard not defteri çekirdek kapatana kadar çalışmaya devam eder. 

```python
tb.stop()
```

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılır, iki denemeleri oluşturduğunuz ve olası ayarlama ve yeniden eğitme alanları belirlemek için çalıştırma geçmişlerini karşı TensorBoard başlatma işleminin nasıl yapılacağını öğrendiniz. 

* Modelinizi memnun kaldığınızda, attıktan bizim [model dağıtma](how-to-deploy-and-where.md) makalesi. 
* Daha fazla bilgi edinin [hiper parametre ayarı](how-to-tune-hyperparameters.md). 
