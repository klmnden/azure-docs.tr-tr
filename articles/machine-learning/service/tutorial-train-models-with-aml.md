---
title: 'Öğretici: Azure Machine Learning hizmeti ile görüntü sınıflandırma modelini eğitme'
description: Bu öğretici, Azure Machine Learning hizmetini kullanarak Python Jupyter not defterinde scikit-learn ile bir görüntü sınıflandırma modelinin nasıl eğitildiğini gösterir. Bu öğretici, iki bölümden oluşan bir serinin birinci bölümüdür.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: tutorial
author: hning86
ms.author: haining
ms.reviewer: sgilley
ms.date: 12/04/2018
ms.openlocfilehash: 8d3dd87adaad168d193b53507dbbb40efab57810
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52879494"
---
# <a name="tutorial-1-train-an-image-classification-model-with-azure-machine-learning-service"></a>Öğretici 1: Azure Machine Learning hizmeti ile görüntü sınıflandırma modelini eğitme

Bu öğreticide, makine öğrenmesi modelini hem yerel olarak hem de uzak işlem kaynaklarında eğitiyorsunuz. Azure Machine Learning hizmetinde bir Python Jupyter not defteri için eğitim ve dağıtım iş akışı'nı kullanacaksınız.  Ardından not defterini şablon olarak kullanıp kendi verilerinizle kendi makine öğrenmesi modelinizi eğitebilirsiniz. Bu öğretici, **iki bölümden oluşan bir öğretici serisinin birinci bölümüdür**.  

Bu öğretici, Azure Machine Learning hizmeti ile [MNIST](https://yann.lecun.com/exdb/mnist/) veri kümesini ve [scikit-learn](https://scikit-learn.org) kitaplığını kullanarak basit bir lojistik regresyonu eğitir.  MNIST, 70.000 gri tonlamalı resimden oluşan popüler bir veri kümesidir. Her resim 28x28 piksel bir el yazısı rakamdır ve 0 ile 9 arası bir sayıyı temsil eder. Amaç, belirli bir resim temsil ettiği rakamı tanımlamak için çok sınıflı bir sınıflandırıcı oluşturmaktır. 

Şunları nasıl yapacağınızı öğrenin:

> [!div class="checklist"]
> * Geliştirme ortamınızı kurma
> * Verilere erişme ve onları inceleme
> * Popüler scikit-learn makine öğrenimi kitaplığını kullanarak basit bir lojistik regresyon modelini eğitme 
> * Uzak kümede birden çok modeli eğitme
> * Eğitim sonuçlarını inceleme ve en iyi modeli kaydetme

Daha sonra [bu öğreticinin ikinci bölümünde](tutorial-deploy-models-with-aml.md) model seçmeyi ve dağıtmayı öğreneceksiniz. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://aka.ms/AMLfree) oluşturun.

>[!NOTE]
> Bu makalede kod Azure Machine Learning SDK sürümü 1.0.2 ile test edilmiştir

## <a name="get-the-notebook"></a>Not defterini alma

Kolaylık olması için, bu öğretici bir [Jupyter notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/img-classification-part1-training.ipynb) olarak sağlanır. `tutorials/img-classification-part1-training.ipynb` notebook'unu Azure Notebooks üzerinde veya kendi Jupyter notebook sunucunuzda çalıştırın.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-in-azure-notebook.md)]


## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı kurma

Geliştirme çalışmanızdaki tüm kurulum bir Python not defterinde gerçekleştirilebilir.  Kurulum şunları içerir:

* Python paketlerini içeri aktarma
* Yerel bilgisayarınızla uzak kaynaklar arasında iletişim kurulmasına olanak tanımak için çalışma alanına bağlanma
* Tüm çalıştırmalarınızı izlemek için bir deneme oluşturma
* Eğitim için kullanılacak uzak işlem hedefini oluşturma

### <a name="import-packages"></a>Paketleri içeri aktarma

Bu oturumda ihtiyacınız olan Python paketlerini içeri aktarın. Ayrıca Azure Machine Learning SDK'sının sürümünü görüntüleyin.

```python
%matplotlib inline
import numpy as np
import matplotlib
import matplotlib.pyplot as plt

import azureml
from azureml.core import Workspace, Run

# check core SDK version number
print("Azure ML SDK Version: ", azureml.core.VERSION)
```

### <a name="connect-to-workspace"></a>Çalışma alanına bağlanma

Mevcut çalışma alanından bir çalışma alanı nesnesi oluşturun. `Workspace.from_config()`, **config.json** dosyasını okur ve ayrıntıları `ws` adlı nesneye yükler.

```python
# load workspace configuration from the config.json file in the current folder.
ws = Workspace.from_config()
print(ws.name, ws.location, ws.resource_group, ws.location, sep = '\t')
```

### <a name="create-experiment"></a>Deneme oluşturma

Çalışma alanınızdaki çalıştırmaları izleyecek bir deneme oluşturun. Bir çalışma alanının birden çok denemesi olabilir. 

```python
experiment_name = 'sklearn-mnist'

from azureml.core import Experiment
exp = Experiment(workspace=ws, name=experiment_name)
```

### <a name="create-or-attach-existing-amlcompute"></a>Oluşturun veya var olan AMlCompute ekleme

Azure Machine Learning yönetilen Compute(AmlCompute) kümelerinde GPU desteğine sahip VM'ler gibi Azure sanal makineler, makine öğrenimi modellerini eğitmek veri bilimcilerine sağlayan bir yönetilen hizmettir.  Bu öğreticide, eğitim ortamınızı AmlCompute oluşturun. Çalışma alanınızda zaten yoksa, bu kod, sizin için işlem kümeleri olarak oluşturur.

 **İşlemin oluşturulması yaklaşık 5 dakika sürer.** İşlem çalışma alanında ise bu kod kullanır ve oluşturma işlemini atlar.


```python
from azureml.core.compute import AmlCompute
from azureml.core.compute import ComputeTarget
import os

# choose a name for your cluster
from azureml.core.compute import AmlCompute
from azureml.core.compute import ComputeTarget
import os

# choose a name for your cluster
compute_name = os.environ.get("AML_COMPUTE_CLUSTER_NAME", "cpucluster")
compute_min_nodes = os.environ.get("AML_COMPUTE_CLUSTER_MIN_NODES", 0)
compute_max_nodes = os.environ.get("AML_COMPUTE_CLUSTER_MAX_NODES", 4)

# This example uses CPU VM. For using GPU VM, set SKU to STANDARD_NC6
vm_size = os.environ.get("AML_COMPUTE_CLUSTER_SKU", "STANDARD_D2_V2")


if compute_name in ws.compute_targets:
    compute_target = ws.compute_targets[compute_name]
    if compute_target and type(compute_target) is AmlCompute:
        print('found compute target. just use it. ' + compute_name)
else:
    print('creating a new compute target...')
    provisioning_config = AmlCompute.provisioning_configuration(vm_size = vm_size,
                                                                min_nodes = compute_min_nodes, 
                                                                max_nodes = compute_max_nodes)

    # create the cluster
    compute_target = ComputeTarget.create(ws, compute_name, provisioning_config)
    
    # can poll for a minimum number of nodes and for a specific timeout. 
    # if no min node count is provided it will use the scale settings for the cluster
    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
    
     # For a more detailed view of current AmlCompute status, use the 'status' property    
    print(compute_target.status.serialize())
```

Artık bulutta bir modeli eğitmek için gerekli paketleriniz ve işlem kaynaklarınız vardır. 

## <a name="explore-data"></a>Verileri inceleme

Modeli eğitmeden önce, eğitimde kullandığınız verileri anlamanız gerekir.  Ayrıca bulut eğitim ortamınızın erişebilmesi için verileri buluta kopyalamalısınız.  Bu bölümde şunları nasıl yapabileceğinizi öğrenirsiniz:

* MNIST veri kümesini indirme
* Bazı örnek görüntüleri gösterme
* Verileri buluta yükleme

### <a name="download-the-mnist-dataset"></a>MNIST veri kümesini indirme

MNIST veri kümesini indirin ve dosyaları yerel olarak `data` dizinine kaydedin.  Hem eğitim hem test için görüntüler ve etiketler karşıya yüklenir.


```python
import os
import urllib.request

os.makedirs('./data', exist_ok = True)

urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz', filename='./data/train-images.gz')
urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz', filename='./data/train-labels.gz')
urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz', filename='./data/test-images.gz')
urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz', filename='./data/test-labels.gz')
```

### <a name="display-some-sample-images"></a>Bazı örnek görüntüleri gösterme

Sıkıştırılmış dosyaları `numpy` dizilerine yükleyin. Ardından `matplotlib` kullanarak, üst kısımlarında etiketleriyle veri kümesinden 30 rastgele görüntü çizin. Bu adım gerektirir Not bir `load_data` dahil işlevi bir `util.py` dosya. Bu dosya örnek klasöründe bulunur. Lütfen bu not defteriyle aynı klasöre yerleştirildiğinden emin olun. `load_data` İşlevi yalnızca sıkıştırılmış dosyalar numpy diziye ayrıştırır.



```python
# make sure utils.py is in the same directory as this code
from utils import load_data

# note we also shrink the intensity values (X) from 0-255 to 0-1. This helps the model converge faster.
X_train = load_data('./data/train-images.gz', False) / 255.0
y_train = load_data('./data/train-labels.gz', True).reshape(-1)

X_test = load_data('./data/test-images.gz', False) / 255.0
y_test = load_data('./data/test-labels.gz', True).reshape(-1)

# now let's show some randomly chosen images from the traininng set.
count = 0
sample_size = 30
plt.figure(figsize = (16, 6))
for i in np.random.permutation(X_train.shape[0])[:sample_size]:
    count = count + 1
    plt.subplot(1, sample_size, count)
    plt.axhline('')
    plt.axvline('')
    plt.text(x=10, y=-10, s=y_train[i], fontsize=18)
    plt.imshow(X_train[i].reshape(28, 28), cmap=plt.cm.Greys)
plt.show()
```

Rastgele görüntü örnekleri gösterilir:

![rastgele görüntü örnekleri](./media/tutorial-train-models-with-aml/digits.png)

Artık bu görüntülerin nasıl göründüğü ve beklenen tahmin sonucu hakkında bir fikriniz oldu.

### <a name="upload-data-to-the-cloud"></a>Verileri buluta yükleme

Şimdi verileri yerel makinenizden Azure'a yükleyerek uzaktan erişilebilir duruma getirin. Böylece uzaktan eğitim için bunlara erişilebilir. Veri deposu, verileri karşıya yüklemeniz/indirmeniz ve uzak işlem hedeflerinizden verilerle etkileşimli çalışmanız için çalışma alanınızla ilişkilendirilmiş kullanışlı bir yapıdır. Azure blob depolama hesabı ile desteklenir.

MNIST dosyaları, veri deposunun kökündeki `mnist` adlı bir dizine yüklenir.

```python
ds = ws.get_default_datastore()
print(ds.datastore_type, ds.account_name, ds.container_name)

ds.upload(src_dir='./data', target_path='mnist', overwrite=True, show_progress=True)
```
Artık modeli eğitmeye başlamak için gereken her şeye sahipsiniz. 

## <a name="train-a-local-model"></a>Yerel bir model eğitip

Scikit kullanarak bir basit Lojistik regresyon modelini eğitme-yerel olarak öğrenin.

Bilgisayarınızın yapılandırmasına bağlı olarak **yerel eğitmek bir veya iki dakika sürer**.

```python
%%time
from sklearn.linear_model import LogisticRegression

clf = LogisticRegression()
clf.fit(X_train, y_train)
```

Ardından, test kümesini kullanarak tahminler yapın ve doğruluğu ölçün. 

```python
y_hat = clf.predict(X_test)
print(np.average(y_hat == y_test))
```

Yerel modelin doğruluğu görüntülenir:

`0.9202`

Yalnızca birkaç kod satırıyla %92 doğruluk elde ettiniz.

## <a name="train-on-a-remote-cluster"></a>Uzak kümede eğitme

Şimdi, farklı bir düzenleme hızıyla bir model oluşturarak bu basit modeli genişletebilirsiniz. Bu kez modeli uzak kaynakta eğiteceksiniz.  

Bu görev için, işi daha önce ayarlamış olduğunuz uzak eğitim kümesine gönderin.  İş göndermek için şunları yaparsınız:
* Dizin oluşturma
* Eğitim betiği oluşturma
* Tahmin nesne oluşturma
* İşi gönderme 

### <a name="create-a-directory"></a>Dizin oluşturma

Gerekli kodu bilgisayarınızdan uzak kaynağa teslim etmek için bir dizin oluşturun.

```python
import os
script_folder = './sklearn-mnist'
os.makedirs(script_folder, exist_ok=True)
```

### <a name="create-a-training-script"></a>Eğitim betiği oluşturma

İşi kümeye göndermek için, önce bir eğitim betiği oluşturun. Aşağıdaki kodu çalıştırarak, az önce oluşturduğunuz dizinde `train.py` adlı eğitim betiğini oluşturun. Bu eğitim, eğitim algoritmasına bir düzenleme oranı ekler ve bu nedenle yerel sürümden biraz farklı bir model oluşturur.

```python
%%writefile $script_folder/train.py

import argparse
import os
import numpy as np

from sklearn.linear_model import LogisticRegression
from sklearn.externals import joblib

from azureml.core import Run
from utils import load_data

# let user feed in 2 parameters, the location of the data files (from datastore), and the regularization rate of the logistic regression model
parser = argparse.ArgumentParser()
parser.add_argument('--data-folder', type=str, dest='data_folder', help='data folder mounting point')
parser.add_argument('--regularization', type=float, dest='reg', default=0.01, help='regularization rate')
args = parser.parse_args()

data_folder = os.path.join(args.data_folder, 'mnist')
print('Data folder:', data_folder)

# load train and test set into numpy arrays
# note we scale the pixel intensity values to 0-1 (by dividing it with 255.0) so the model can converge faster.
X_train = load_data(os.path.join(data_folder, 'train-images.gz'), False) / 255.0
X_test = load_data(os.path.join(data_folder, 'test-images.gz'), False) / 255.0
y_train = load_data(os.path.join(data_folder, 'train-labels.gz'), True).reshape(-1)
y_test = load_data(os.path.join(data_folder, 'test-labels.gz'), True).reshape(-1)
print(X_train.shape, y_train.shape, X_test.shape, y_test.shape, sep = '\n')

# get hold of the current run
run = Run.get_context()

print('Train a logistic regression model with regularizaion rate of', args.reg)
clf = LogisticRegression(C=1.0/args.reg, random_state=42)
clf.fit(X_train, y_train)

print('Predict the test set')
y_hat = clf.predict(X_test)

# calculate accuracy on the prediction
acc = np.average(y_hat == y_test)
print('Accuracy is', acc)

run.log('regularization rate', np.float(args.reg))
run.log('accuracy', np.float(acc))

os.makedirs('outputs', exist_ok=True)
# note file saved in the outputs folder is automatically uploaded into experiment record
joblib.dump(value=clf, filename='outputs/sklearn_mnist_model.pkl')
```

Betiğin verileri nasıl aldığına ve modelleri nasıl kaydettiğine dikkat edin:

+ Eğitim betiği verileri içeren dizini bulmak için bir bağımsız değişkeni okur.  Daha sonra işi gönderdiğinizde, bu bağımsız değişken için veri deposuna işaret edersiniz: `parser.add_argument('--data-folder', type=str, dest='data_folder', help='data directory mounting point')`

+ Eğitim betiği modelinizi outputs adlı dizine kaydeder. <br/>
`joblib.dump(value=clf, filename='outputs/sklearn_mnist_model.pkl')`<br/>
Bu dizine yazılan her şey otomatik olarak çalışma alanınıza yüklenir. Öğreticide daha sonra bu dizinden modelinize erişeceksiniz.
Veri kümesini doğru yüklemek için eğitim betiğinden `utils.py` dosyasına başvurulur.  Bu betiği betik klasörünüze kopyalayın. Böylelikle, uzak kaynakta eğitim betiğiyle birlikte bu betiğe de erişilebilir.


```python
import shutil
shutil.copy('utils.py', script_folder)
```


### <a name="create-an-estimator"></a>Tahmin aracı oluşturma

Tahmin aracı nesnesi, çalıştırmayı göndermek için kullanılır.  Şunları tanımlamak için aşağıdaki kodu çalıştırın ve tahmin aracınızı oluşturun:

* Tahmin aracı nesnesinin adı, `est`
* Betiklerinizi içeren dizin. Bu dizindeki dosyaların tümü yürütülmek üzere küme düğümlerine yüklenir. 
* Bilgi işlem hedefi.  Bu durumda, oluşturduğunuz Azure Machine Learning işlem kümesi kullanır.
* Eğitim betik adı, train.py
* Eğitin betiğinden gerekli parametreler 
* Eğitim için gereken Python paketleri

Bu öğreticide, bu AmlCompute hedefidir. Betik klasördeki tüm dosyaları yürütme için küme düğümlerine yüklenir. Data_folder veri deposunu kullanacak şekilde ayarlanır (`ds.as_mount()`).

```python
from azureml.train.estimator import Estimator

script_params = {
    '--data-folder': ds.as_mount(),
    '--regularization': 0.8
}

est = Estimator(source_directory=script_folder,
                script_params=script_params,
                compute_target=compute_target,
                entry_script='train.py',
                conda_packages=['scikit-learn'])
```


### <a name="submit-the-job-to-the-cluster"></a>İşi kümeye gönderme

Tahmin aracı nesnesini göndererek denemeyi çalıştırın.

```python
run = exp.submit(config=est)
run
```

Çağrı zaman uyumsuz olduğundan, iş başlatıldığı anda **Hazırlanıyor** veya **Çalıştırılıyor** durumunu gönderir.

## <a name="monitor-a-remote-run"></a>Uzaktan çalıştırmayı izleme

Toplamda, ilk çalıştırma **yaklaşık 10 dakika** sürer. Ama betik bağımlılıkları değişmediği sürece bunu izleyen çalıştırmalarda aynı görüntü yeniden kullanılır ve dolayısıyla kapsayıcı başlatma süresi çok daha kısa olur.

Siz beklerken şunlar gerçekleştirilir:

- **Görüntü oluşturma**: Tahmin aracının belirttiği Python ortamıyla eşleşen bir Docker görüntüsü oluşturulur. Görüntü, çalışma alanına yüklenir. Görüntüyü oluşturma ve karşıya yükleme **yaklaşık 5 dakika** sürer. 

  Bu aşama her Python ortamı için tek bir kez gerçekleştirilir çünkü bunu izleyen çalıştırmalar için kapsayıcı önbelleğe alınır.  Görüntü oluşturma sırasında, günlükler çalıştırma geçmişine aktarılır. Bu günlükleri kullanarak görüntü oluşturma işleminin ilerleme durumunu izleyebilirsiniz.

- **Ölçeklendirme**: Çalıştırmayı yürütmek için uzak kümeye şu anda sağlanandan daha fazla düğüm gerekiyorsa, otomatik olarak daha fazla düğüm eklenir. Ölçekleme genellikle **yaklaşık 5 dakika** sürer.

- **Çalıştırma**: Bu aşamada, gerekli betikler ve dosyalar işlem hedefine gönderilir, ardından veri depoları bağlanır/kopyalanır ve entry_script çalıştırılır. İş çalıştırılırken, stdout ve ./logs dizini çalıştırma geçmişine aktarılır. Bu günlükleri kullanarak çalıştırma işleminin ilerleme durumunu izleyebilirsiniz.

- **Son İşlem**: Çalıştırmanın ./outputs dizini çalışma alanınızdaki çalıştırma geçmişine kopyalanarak bu sonuçlara erişmeniz sağlanır.


Çalışan işin ilerleme durumunu çeşitli yollarla denetleyebilirsiniz. Bu öğreticide hem Jupyter pencere öğesi hem de `wait_for_completion` yöntemi kullanılır. 

### <a name="jupyter-widget"></a>Jupyter pencere öğesi

Çalıştırmanın ilerleme durumunu Jupyter pencere öğesiyle izleyin.  Çalıştırma gönderimi gibi pencere öğesi de zaman uyumsuzdur ve iş tamamlanana kadar her 10-15 saniyede bir canlı güncelleştirmeler sağlar.


```python
from azureml.widgets import RunDetails
RunDetails(run).show()
```

Burada, pencere öğesinin eğitimin sonunda gösterilen durağan bir anlık görüntüsü verilmiştir:

![not defteri pencere öğesi](./media/tutorial-train-models-with-aml/widget.png)

### <a name="get-log-results-upon-completion"></a>Tamamlandıktan sonra günlük sonuçlarını alma

Model eğitimi ve izlemesi arka planda yapılır. Daha fazla kod çalıştırmadan önce model eğitiminin tamamlanmasını bekleyin. Model eğitiminin ne zaman tamamlandığını göstermek için `wait_for_completion` kullanın. 


```python
run.wait_for_completion(show_output=False) # specify True for a verbose log
```

### <a name="display-run-results"></a>Çalıştırma sonuçlarını görüntüleme

Artık uzak düğümde eğitilmiş bir modeliniz vardır.  Modelin doğruluğunu alın:

```python
print(run.get_metrics())
```
Çıkışta uzak modelin doğruluğunun yerel modelden biraz fazla olduğu gösterilir; bunun nedeni eğitim sırasında kurallaştırma oranının eklenmesidir.  

`{'regularization rate': 0.8, 'accuracy': 0.9204}`

Sonraki Öğreticide bu modeli daha ayrıntılı inceleyeceksiniz.

## <a name="register-model"></a>Modeli kaydetme

Son adımda eğitim betiği işin yürütüldüğü kümenin sanal makinesinde yer alan `outputs` adlı dizine `outputs/sklearn_mnist_model.pkl` dosyasını yazar. `outputs` özel bir dizindir; şöyle ki, dizindeki tüm içerik otomatik olarak çalışma alanınıza yüklenir.  Bu içerik çalışma alanınızın altında yer alan denemedeki çalıştırma kaydında gösterilir. Bu nedenle, model dosyası artık çalışma alanınızda da kullanılabilir durumdadır.

Bu çalıştırmayla ilişkilendirilmiş dosyaları görebilirsiniz.

```python
print(run.get_file_names())
```

Bu modelin siz (veya birlikte çalıştıklarınızdan biri) tarafından daha sonra sorgulanabilmesi, incelenebilmesi ve dağıtılabilmesi için, çalışma alanında modeli kaydedin.

```python
# register model 
model = run.register_model(model_name='sklearn_mnist', model_path='outputs/sklearn_mnist_model.pkl')
print(model.name, model.id, model.version, sep = '\t')
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

Doğrudan Azure Yönetilen İşlem kümesini de silebilirsiniz. Öte yandan, otomatik ölçeklendirme açıldığından ve küme alt sınırı 0 olduğundan dolayı, bu özel kaynak kullanımda olmadığında ek işlem ücreti alınmaz.


```python
# optionally, delete the Azure Managed Compute cluster
compute_target.delete()
```

## <a name="next-steps"></a>Sonraki adımlar

Bu Azure Machine Learning hizmeti öğreticisinde Python kullanarak aşağıdakileri yaptınız:

> [!div class="checklist"]
> * Geliştirme ortamınızı kurma
> * Verilere erişme ve onları inceleme
> * Popüler scikit-learn makine öğrenimi kitaplığını kullanarak basit bir lojistik regresyon modelini eğitme
> * Uzak kümede birden çok modeli eğitme
> * Eğitim ayrıntılarını inceleme ve en iyi modeli kaydetme

Öğretici serisinin sonraki bölümünde verilen yönergeleri kullanarak bu kayıtlı modeli dağıtmaya hazırsınız:

> [!div class="nextstepaction"]
> [Öğretici 2 - Modelleri dağıtma](tutorial-deploy-models-with-aml.md)
