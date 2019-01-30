---
title: 'Görüntü sınıflandırma Öğreticisi: Modelleri eğitme'
titleSuffix: Azure Machine Learning service
description: Bu öğretici, Azure Machine Learning hizmetini kullanarak Python Jupyter not defterinde scikit-learn ile bir görüntü sınıflandırma modelinin nasıl eğitildiğini gösterir. Bu öğretici, iki bölümden oluşan bir serinin birinci bölümüdür.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: hning86
ms.author: haining
ms.reviewer: sgilley
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: ed5e506e5bb38e6c11c3d8ecd52c85d4f21cf1f2
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55242935"
---
# <a name="tutorial-train-an-image-classification-model-with-azure-machine-learning-service"></a>Öğretici: Bir Azure Machine Learning hizmeti ile görüntü sınıflandırma modeli eğitme

Bu öğreticide, makine öğrenmesi modelini hem yerel olarak hem de uzak işlem kaynaklarında eğitiyorsunuz. Azure Machine Learning hizmetinde bir Python Jupyter not defteri için eğitim ve dağıtım iş akışı'nı kullanın. Ardından not defterini şablon olarak kullanıp kendi verilerinizle kendi makine öğrenmesi modelinizi eğitebilirsiniz. Bu öğretici, **iki bölümden oluşan bir öğretici serisinin birinci bölümüdür**.  

Bu öğreticide basit bir Lojistik regresyon kullanarak eğitir [MNIST](http://yann.lecun.com/exdb/mnist/) veri kümesi ve [scikit-bilgi](https://scikit-learn.org) Azure Machine Learning hizmeti ile. MNIST, 70.000 gri tonlamalı resimden oluşan popüler bir veri kümesidir. Her bir el yazısı basamak 28 x 28 piksel, dokuz sıfırdan bir sayıyı temsil eden görüntüsüdür. Belirli bir görüntüyü basamak tanımlamak için bir çok sınıflı sınıflandırıcı oluşturulacağını temsil eden hedeftir. 

Aşağıdaki eylemleri gerçekleştirmeniz öğrenin:

> [!div class="checklist"]
> * Geliştirme ortamınızı ayarlayın.
> * Erişim ve verileri inceleyin.
> * Popüler scikit kullanarak yerel olarak basit bir Lojistik regresyon eğitme-makine öğrenimi kitaplığı öğrenin. 
> * Bir uzak kümesi üzerinde birden çok modeli eğitin.
> * Eğitim sonuçlarını gözden geçirin ve en iyi modeli kaydedin.

Bir model seçin ve bunu dağıtma hakkında bilgi edinin [Bu öğreticinin İkinci bölüm](tutorial-deploy-models-with-aml.md). 

Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](http://aka.ms/AMLFree) bugün.

>[!NOTE]
> Bu makalede kod, Azure Machine Learning SDK sürümü 1.0.2 ile test edilmiştir.

## <a name="get-the-notebook"></a>Not defterini alma

Kolaylık olması için, bu öğretici bir [Jupyter notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/img-classification-part1-training.ipynb) olarak sağlanır. Çalıştırma `tutorials/img-classification-part1-training.ipynb` Not Defteri veya [Azure not defterleri](https://notebooks.azure.com/) veya kendi Jupyter notebook sunucusu.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-in-azure-notebook.md)]


## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı kurma

Geliştirme çalışmanızdaki tüm kurulum bir Python not defterinde gerçekleştirilebilir. Kurulumu, aşağıdaki eylemleri içerir:

* Python paketlerini içeri aktarın.
* Yerel bilgisayarınızda uzak kaynaklarla iletişim kurabilmesi için bir çalışma alanına bağlayın.
* Tüm çalıştırmalar izlemek için bir deneme oluşturun.
* Eğitim için kullanılacak uzak işlem hedefi oluşturmak.

### <a name="import-packages"></a>Paketleri içeri aktarma

Bu oturumda ihtiyacınız olan Python paketlerini içeri aktarın. Ayrıca Azure Machine Learning SDK sürümü görüntüler:

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

### <a name="connect-to-a-workspace"></a>Bir çalışma alanına bağlayın

Mevcut çalışma alanından bir çalışma alanı nesnesi oluşturun. `Workspace.from_config()` Dosya Okuma **config.json** ve ayrıntıları adlı bir nesnesine yükler `ws`:

```python
# load workspace configuration from the config.json file in the current folder.
ws = Workspace.from_config()
print(ws.name, ws.location, ws.resource_group, ws.location, sep = '\t')
```

### <a name="create-an-experiment"></a>Deneme oluşturma

Çalışma alanınızdaki çalıştırmaları izleyecek bir deneme oluşturun. Bir çalışma alanı, birden çok deneme olabilir: 

```python
experiment_name = 'sklearn-mnist'

from azureml.core import Experiment
exp = Experiment(workspace=ws, name=experiment_name)
```

### <a name="create-or-attach-an-existing-amlcompute"></a>Oluşturun veya var olan bir AMlCompute ekleme

Azure Machine Learning işlem (AmlCompute) yönetilen bir hizmet kullanarak veri bilimcileri makine öğrenimi modellerini Azure sanal makinelerini kümeleri hakkında eğitebilirsiniz. Örnekler, GPU desteğine sahip sanal makinelerini içerir. Bu öğreticide, eğitim ortamınızı AmlCompute oluşturun. Çalışma alanınızda zaten mevcut olmaması durumunda bu kod, sizin için işlem kümeleri olarak oluşturur.

 **İşlem oluşturulması yaklaşık beş dakika sürer.** İşlem çalışma alanında ise, bu kod kullanır ve oluşturma işlemi atlanıyor:


```python
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

Bir model eğitip önce onu eğitmek için kullandığınız verileri anlamak gerekir. Ayrıca, verileri buluta kopyalama gerekir. Ardından, bulut eğitim ortamı tarafından erişilebilir. Bu bölümde, aşağıdaki eylemleri hakkında bilgi edinin:

* MNIST veri kümesini indirin.
* Bazı örnek görüntüler.
* Verilerini buluta yükleyin.

### <a name="download-the-mnist-dataset"></a>MNIST veri kümesini indirme

MNIST veri kümesini indirin ve dosyaları yerel olarak `data` dizinine kaydedin. Görüntüleri ve etiketleri eğitim ve test için yüklenir:


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

Sıkıştırılmış dosyaları `numpy` dizilerine yükleyin. Ardından `matplotlib` kullanarak, üst kısımlarında etiketleriyle veri kümesinden 30 rastgele görüntü çizin. Bu adım gerektirir bir `load_data` dahil işlevi bir `util.py` dosya. Bu dosya örnek klasöründe bulunur. Bu not defteri ile aynı klasörde yerleştirildiğinden emin olun. `load_data` İşlevi yalnızca sıkıştırılmış dosyalar numpy diziye ayrıştırır:



```python
# make sure utils.py is in the same directory as this code
from utils import load_data

# note we also shrink the intensity values (X) from 0-255 to 0-1. This helps the model converge faster.
X_train = load_data('./data/train-images.gz', False) / 255.0
y_train = load_data('./data/train-labels.gz', True).reshape(-1)

X_test = load_data('./data/test-images.gz', False) / 255.0
y_test = load_data('./data/test-labels.gz', True).reshape(-1)

# now let's show some randomly chosen images from the training set.
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

![rastgele örnek görüntüleri](./media/tutorial-train-models-with-aml/digits.png)

Artık bu görüntülerin nasıl göründüğü ve beklenen tahmin sonucu hakkında bir fikriniz oldu.

### <a name="upload-data-to-the-cloud"></a>Verileri buluta yükleme

Artık verilerin erişilebilir uzaktan Azure'a bu verileri yerel makinenizden karşıya yükleyerek hale getirir. Ardından uzak eğitim için erişilebilir. Veri deposu, karşıya yükleme veya verileri indirmek için çalışma alanıyla ilişkili kullanışlı bir yapıdır. Ayrıca, uzak işlem hedeflerden ile de etkileşim kurabilirsiniz. Bu, bir Azure Blob Depolama hesabı tarafından desteklenir.

Adlı bir dizine MNIST dosyalar yüklendiğinde `mnist` veri deposu, kökünde:

```python
ds = ws.get_default_datastore()
print(ds.datastore_type, ds.account_name, ds.container_name)

ds.upload(src_dir='./data', target_path='mnist', overwrite=True, show_progress=True)
```
Artık modeli eğitmeye başlamak için gereken her şeye sahipsiniz. 

## <a name="train-a-local-model"></a>Yerel bir model eğitip

Scikit kullanarak basit bir Lojistik regresyon modelini eğitme-yerel olarak öğrenin.

**Yerel olarak eğitim alabilir bir veya iki dakika** bilgisayar yapılandırmanıza bağlı olarak:

```python
%%time
from sklearn.linear_model import LogisticRegression

clf = LogisticRegression()
clf.fit(X_train, y_train)
```

Sonraki test kümesi kullanarak tahmin yapmayı ve doğrulukta hesaplama: 

```python
y_hat = clf.predict(X_test)
print(np.average(y_hat == y_test))
```

Yerel modelin doğruluğu görüntülenir:

`0.9202`

Yalnızca birkaç kod satırıyla, 92 yüzde kesinliğe sahip.

## <a name="train-on-a-remote-cluster"></a>Uzak kümede eğitme

Şimdi, farklı bir düzenleme hızıyla bir model oluşturarak bu basit modeli genişletebilirsiniz. Bu kez, uzak kaynak modeli eğitin.  

Bu görev için, işi daha önce ayarlamış olduğunuz uzak eğitim kümesine gönderin. Bir işi göndermek için aşağıdaki adımları uygulayın:
* Bir dizin oluşturun.
* Bir eğitim betiği oluşturun.
* Tahmin nesnesi oluşturun.
* İşi Gönder.

### <a name="create-a-directory"></a>Dizin oluşturma

Gerekli kodu bilgisayarınızdan uzak kaynağa sunmak için bir dizin oluşturun:

```python
import os
script_folder = './sklearn-mnist'
os.makedirs(script_folder, exist_ok=True)
```

### <a name="create-a-training-script"></a>Eğitim betiği oluşturma

İşi kümeye göndermek için, önce bir eğitim betiği oluşturun. Adlı bir eğitim betiği oluşturmak için aşağıdaki kodu çalıştırın `train.py` oluşturduğunuz dizine. Bu eğitim, eğitim algoritması için normalleştirme oranını ekler. Bu nedenle yerel belirtilenden biraz daha farklı bir model oluşturur:

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

print('Train a logistic regression model with regularization rate of', args.reg)
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

+ Eğitim betiği, veri içeren dizine bulmak için bir bağımsız değişken okur. Daha sonra işi gönderdiğinizde, bu bağımsız değişken için veri deposu üzerine: `parser.add_argument('--data-folder', type=str, dest='data_folder', help='data directory mounting point')`.

+ Eğitim betiğini modelinizi adlı bir dizine kaydeder. **çıkarır**: <br/>
`joblib.dump(value=clf, filename='outputs/sklearn_mnist_model.pkl')`.<br/>
Bu dizine yazılan her şey otomatik olarak çalışma alanınıza yüklenir. Öğreticinin ilerleyen bölümlerinde bu dizinden modelinizi erişin.
Veri kümesini doğru yüklemek için eğitim betiğinden `utils.py` dosyasına başvurulur. Uzak Kaynak eğitim betiği birlikte erişilebilir olacak şekilde bu betiği komut dosyası klasörüne kopyalayın.


```python
import shutil
shutil.copy('utils.py', script_folder)
```


### <a name="create-an-estimator"></a>Tahmin aracı oluşturma

Tahmin aracı nesnesi, çalıştırmayı göndermek için kullanılır. Bu öğeleri tanımlamak için aşağıdaki kodu çalıştırarak, tahmin oluşturun:

* Tahmin nesnesinin adını `est`.
* Betiklerinizi içeren dizin. Bu dizindeki dosyaların tümü yürütülmek üzere küme düğümlerine yüklenir. 
* Bilgi işlem hedefi. Bu durumda, oluşturduğunuz Azure Machine Learning işlem kümesi kullanın.
* Eğitim betik adı **train.py**.
* Eğitim betikten gerekli parametreleri. 
* Eğitim için gereken Python paketleri.

Bu öğreticide, bu AmlCompute hedefidir. Betik klasördeki tüm dosyaları çalıştırmak için küme düğümlerinin yüklenir. **Data_folder** veri deposu kullanmak üzere ayarlanmış `ds.as_mount()`:

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

Denemeyi estimator nesne göndererek çalıştırın:

```python
run = exp.submit(config=est)
run
```

Zaman uyumsuz bir çağrı olduğundan, onu döndürür bir **hazırlama** veya **çalıştıran** iş başlatıldıktan hemen sonra durum.

## <a name="monitor-a-remote-run"></a>Uzaktan çalıştırmayı izleme

Toplam olarak, ilk çalıştırma alır **yaklaşık 10 dakika**. Ancak, sonraki çalıştırmalar için betik bağımlılıkları değiştirme sürece, aynı görüntüsü yeniden kullanılır. Bu nedenle kapsayıcı başlatma süresi çok daha hızlıdır.

Beklerken ne olur:

- **Görüntü oluşturma**: Bir Docker görüntüsü estimator tarafından belirtilen Python ortamı eşleşen oluşturulur. Görüntü, çalışma alanına yüklenir. Görüntü oluşturma ve karşıya yükleme alır **yaklaşık beş dakikada**. 

  Sonraki çalıştırmaları için kapsayıcı önbelleğe alındığından bu aşamanın her Python ortamı için bir kez gerçekleşir. Görüntü oluşturma sırasında, günlükler çalıştırma geçmişine aktarılır. Bu günlükleri kullanarak görüntü oluşturma ilerleme durumunu izleyebilirsiniz.

- **Ölçeklendirme**: Uzak kümeye çalıştırma daha şu anda kullanılabilir yapmak için daha fazla düğüm gerekiyorsa, ek düğümler otomatik olarak eklenir. Genellikle ölçekleme alan **yaklaşık beş dakika.**

- **Çalışan**: Bu aşamada, gerekli betikler ve dosyaları işlem hedefine gönderilir. Ardından veri depoları takılı kopyaladığınız veya. Ardından **entry_script** çalıştırılır. İş çalışırken **stdout** ve **. / günlükleri** dizin için çalıştırma geçmişi akış. Bu günlükleri kullanarak çalışmanın ilerlemesini izleyebilirsiniz.

- **İşleme sonrası**: **. / Çıkışlar** çalıştırma dizinine kopyalanır üzerinden çalıştırma geçmişini çalışma alanınızda bu sonuçları erişebilmesi için.


Çeşitli şekillerde çalışan işin ilerleme durumunu kontrol edebilirsiniz. Bu öğreticide bir Jupyter pencere öğesi ve bir `wait_for_completion` yöntemi. 

### <a name="jupyter-widget"></a>Jupyter pencere öğesi

Çalıştırmanın ilerleme durumunu Jupyter pencere öğesiyle izleyin. Çalıştırma gönderim gibi pencere öğesi zaman uyumsuz ve canlı güncelleştirmeler iş tamamlanana kadar her 10 ila 15 saniyede sağlar:


```python
from azureml.widgets import RunDetails
RunDetails(run).show()
```

Bu yine de anlık görüntü eğitim sonunda gösterilen pencere öğesi şöyledir:

![Not Defteri pencere öğesi](./media/tutorial-train-models-with-aml/widget.png)

### <a name="get-log-results-upon-completion"></a>Tamamlandıktan sonra günlük sonuçlarını alma

Model eğitimi ve izlemesi arka planda yapılır. Daha fazla kod çalıştırmadan önce model eğitim tamamlanana kadar bekleyin. Kullanım `wait_for_completion` modeli eğitimi tamamlandığında göstermek için: 


```python
run.wait_for_completion(show_output=False) # specify True for a verbose log
```

### <a name="display-run-results"></a>Çalıştırma sonuçlarını görüntüleme

Artık uzak düğümde eğitilmiş bir modeliniz vardır. Modelin doğruluğunu alın:

```python
print(run.get_metrics())
```
Çıktı normalleştirme oranını eklenmesini eğitim sırasında nedeniyle yerel modelinden biraz daha yüksek doğruluk uzaktan modeline sahip gösterir:  

`{'regularization rate': 0.8, 'accuracy': 0.9204}`

Sonraki öğreticide, bu modeli daha ayrıntılı keşfedin.

## <a name="register-model"></a>Modeli kaydetme

Son adımda eğitim betik dosyası yazıldı `outputs/sklearn_mnist_model.pkl` adlı bir dizinde `outputs` işin çalıştırıldığı kümesinin VM. `outputs` Bu dizindeki tüm içeriği için çalışma alanınızı otomatik olarak yüklenir, özel bir dizindir. Bu içerik çalışma alanınızın altında yer alan denemedeki çalıştırma kaydında gösterilir. Bu nedenle model dosyası ayrıca çalışma alanınızda kullanıma sunuldu.

Çalıştırılan ile ilişkili dosyaları görebilirsiniz:

```python
print(run.get_file_names())
```

Böylece, veya diğer ortak çalışanlar daha sonra sorgu inceleyin ve bu model dağıtma modeli çalışma alanında, kaydedin:

```python
# register model 
model = run.register_model(model_name='sklearn_mnist', model_path='outputs/sklearn_mnist_model.pkl')
print(model.name, model.id, model.version, sep = '\t')
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

Yalnızca Azure Machine Learning işlem kümesi de silebilirsiniz. Ancak, otomatik ölçeklendirme açık olduğundan ve kümenin en az sıfırdır. Bu nedenle bu kaynak, kullanımda olmadığında ek işlem ücreti alınmayacaktır:


```python
# optionally, delete the Azure Machine Learning Compute cluster
compute_target.delete()
```

## <a name="next-steps"></a>Sonraki adımlar

Bu Azure Machine Learning hizmeti öğreticide aşağıdaki görevler için Python kullanılır:

> [!div class="checklist"]
> * Geliştirme ortamınızı ayarlayın.
> * Erişim ve verileri inceleyin.
> * Popüler scikit kullanarak yerel olarak basit bir Lojistik regresyon eğitme-makine öğrenimi kitaplığı öğrenin.
> * Bir uzak kümesi üzerinde birden çok modeli eğitin.
> * Eğitim ayrıntılarını gözden geçirin ve en iyi modeli kaydedin.

Kayıtlı Bu model, öğretici serisinin bir sonraki bölümü içindeki yönergeleri kullanarak dağıtmaya hazırsınız:

> [!div class="nextstepaction"]
> [Öğretici 2 - Modelleri dağıtma](tutorial-deploy-models-with-aml.md)
