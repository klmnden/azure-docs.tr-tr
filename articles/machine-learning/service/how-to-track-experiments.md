---
title: Denemeler ve eğitim ölçümleri - Azure Machine Learning izleyin | Microsoft Docs
description: Azure Machine Learning hizmeti ile denemelerinizi izleyebilir ve modeli oluşturma işlemi geliştirmek için ölçümleri izleyin. Eğitim betiğinizi günlük ekleme, denemeyi göndermek nasıl, çalışan işin ilerleme durumunu denetlemek nasıl ve çalıştırmanın sonuçlarını görüntüleme öğrenin.
services: machine-learning
author: heatherbshapiro
ms.author: hshapiro
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/24/2018
ms.openlocfilehash: da92f59c4e25ec012cd9ad389c9afac410ba28e1
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2018
ms.locfileid: "51219316"
---
# <a name="track-experiments-and-training-metrics-in-azure-machine-learning"></a>Denemeler ve Azure Machine learning'de eğitim metriklerini izleme

Azure Machine Learning hizmetinde denemelerinizi izleyebilir ve modeli oluşturma işlemi geliştirmek için ölçümleri izleyin. Bu makalede, günlük eğitim betiğinizi eklemek için farklı yollar hakkında bilgi edineceksiniz ile deneme gönderme **start_logging** ve **ScriptRunConfig**, ilerleme durumunu denetlemek nasıl bir çalışan iş ve çalıştırmanın sonuçlarını görüntüleme. 

>[!NOTE]
> Bu makalede kod Azure Machine Learning SDK sürüm 0.1.74 ile test edilmiştir 

## <a name="list-of-training-metrics"></a>Eğitim ölçümlerin listesi 

Aşağıdaki ölçümler, bir denemeyi eğitim sırasında çalıştırılacak eklenebilir. Bir çalıştırmada izlenebilir daha ayrıntılı bir listesi görmek için bkz: [SDK başvuru belgeleri](https://aka.ms/aml-sdk).

|Tür| Funkce Pythonu | Örnek | Notlar|
|----|:----|:----|:----|
|Skaler değerler | `run.log(name, value, description='')`| `run.log("accuracy", 0.95) ` |Günlük bir sayısal veya dize değeri belirtilen ada sahip farklı çalıştır. Bir ölçüm için bir çalıştırma günlüğe kaydetme, denemeyi çalıştırma kaydı depolanması, ölçüm neden olur.  Bu ölçüm oluşan bir vektörü olarak kabul sonucu bir çalıştırma içinde birden çok kez aynı Ölçüm oturum açabilirsiniz.|
|Listeler| `run.log_list(name, value, description='')`| `run.log_list("accuracies", [0.6, 0.7, 0.87])` | Belirtilen ada sahip farklı çalıştır değerlerin bir listesini oturum.|
|Satır| `run.log_row(name, description=None, **kwargs)`| `run.log_row("Y over X", x=1, y=0.4)` | Kullanarak *log_row* kwargs içinde anlatıldığı gibi birden çok sütun içeren bir ölçü oluşturur. Her adlandırılmış parametre ile belirtilen değer bir sütun oluşturur.  *log_row* rasgele bir tanımlama grubu ya da birden çok kez bir döngüde tam bir tablo oluşturmak için oturum volat pouze jednou.|
|Tablo| `run.log_table(name, value, description='')`| `run.log_table("Y over X", {"x":[1, 2, 3], "y":[0.6, 0.7, 0.89]})` | Sözlük nesnesi, verilen ada sahip farklı çalıştır oturum açın. |
|Görüntüler| `run.log_image(name, path=None, plot=None)`| `run.log_image("ROC", plt)` | Görüntü çalıştırma kaydı için oturum açın. Bir görüntü dosyası veya bir matplotlib oturum log_image kullanın çizim farklı çalıştır.  Bu görüntüler, görünür ve çalışma kaydındaki benzer olacaktır.|
|Bir etiketi| `run.tag(key, value=None)`| `run.tag("selected", "yes")` | Bir dize anahtarı ve isteğe bağlı dize değeri olan çalıştırma etiketleyin.|
|Dosya veya dizin karşıya yükleme|`run.upload_file(name, path_or_stream)`| Run.upload_file ("best_model.pkl", ". / model.pkl") | Çalıştırma kaydı için bir dosya yükleyin. Çalıştırmaları otomatik olarak varsayılan olarak belirtilen çıkış dizini dosyasında yakala ". / çıkışlar" türleri çoğu çalıştırın.  Yalnızca ek dosyalar yüklenmek üzere gerektiğinde kullanım upload_file veya bir çıktı dizini belirtilmedi. Eklemeyi önerin `outputs` BT'nin çıkış dizinine karşıya, böylece adı. Tüm ilişkili olan dosyaları listeleyebilirsiniz bu çalıştırma kaydı tarafından çağırılır `run.get_file_names()`|

> [!NOTE]
> Skalerler, listeler, satır ve tablolar için ölçümleri türü olabilir: kayan noktalı sayı, tamsayı veya dize.

## <a name="log-metrics-for-experiments"></a>Denemeleri için günlük ölçümleri

İzlemek veya denemenizi izlemek istiyorsanız, çalıştırma gönderdiğinizde oturum başlatmak için kod eklemeniz gerekir. Çalıştırma gönderim tetiklemek için yollar şunlardır:
* __Run.start_logging__ - günlük işlevleri eğitim komut dosyanıza ekleyin ve bir etkileşimli günlüğe kaydetme oturumu içinde belirtilen denemeyi başlatın. **start_logging** dizüstü bilgisayarlar gibi senaryolarda kullanım için etkileşimli bir çalışma oluşturur. Oturumu sırasında günlüğe kaydedilen tüm ölçümler, denemeyi çalıştırma kaydı eklenir.
* __ScriptRunConfig__ - günlük işlevleri eğitim komut dosyanıza ekleyin ve Çalıştır tüm betik klasörünü yükleyemedi.  **ScriptRunConfig** betiği için yapılandırmaları ayarlamayı çalıştırmaları için bir sınıftır. Bu seçenekle tamamlanmasından almak veya izlemek için görsel bir pencere öğesi almak için izleme kodu ekleyebilirsiniz.

## <a name="set-up-the-workspace-and-experiment"></a>Çalışma alanı ve deneme ayarlama
Günlüğe kaydetme ve deneme gönderme eklemeden önce çalışma ve deneme ayarlamanız gerekir.

1. Çalışma alanı yükleyin. Çalışma alanı yapılandırması ayarlama hakkında daha fazla bilgi edinmek için izleyin [hızlı](https://docs.microsoft.com/azure/machine-learning/service/quickstart-get-started).

  ```python
  from azureml.core import Workspace, Run
  import azureml.core
  
  ws = Workspace(workspace_name = <<workspace_name>>,
               subscription_id = <<subscription_id>>,
               resource_group = <<resource_group>>)
   ```

2. Deneme oluşturma.

  ```python
  from azureml.core import Experiment

  # make up an arbitrary name
  experiment_name = 'train-in-notebook'
  ```
  
## <a name="option-1-use-startlogging"></a>1. seçenek: start_logging kullanma

**start_logging** dizüstü bilgisayarlar gibi senaryolarda kullanım için etkileşimli bir çalışma oluşturur. Oturumu sırasında günlüğe kaydedilen tüm ölçümler, denemeyi çalıştırma kaydı eklenir.

Aşağıdaki örnek, yerel Jupyter not defterini yerel olarak basit bir sklearn Ridge modeli eğitir. Farklı ortamlara denemeleri gönderme hakkında daha fazla bilgi için bkz. [işlem hedeflerine yönelik Azure Machine Learning hizmeti ile model eğitiminin ayarlama](https://docs.microsoft.com/azure/machine-learning/service/how-to-set-up-training-targets).

1. Bir eğitim betiği yerel bir Jupyter not defteri oluşturun. 

  ``` python
  # load diabetes dataset, a well-known small dataset that comes with scikit-learn
  from sklearn.datasets import load_diabetes
  from sklearn.linear_model import Ridge
  from sklearn.metrics import mean_squared_error
  from sklearn.model_selection import train_test_split
  from sklearn.externals import joblib

  X, y = load_diabetes(return_X_y = True)
  columns = ['age', 'gender', 'bmi', 'bp', 's1', 's2', 's3', 's4', 's5', 's6']
  X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
  data = {
      "train":{"X": X_train, "y": y_train},        
      "test":{"X": X_test, "y": y_test}
  }
  reg = Ridge(alpha = 0.03)
  reg.fit(data['train']['X'], data['train']['y'])
  preds = reg.predict(data['test']['X'])
  print('Mean Squared Error is', mean_squared_error(preds, data['test']['y']))
  joblib.dump(value = reg, filename = 'model.pkl');
  ```

2. Azure Machine Learning hizmeti SDK'sını kullanarak deneme izleme ekleyebilir ve kalıcı bir modeli çalıştırma kaydı denemesine yükleme. Aşağıdaki kod, etiketler, günlükleri, ekler ve denemeyi çalıştırmak için bir model dosyası yükler.

  ```python
  experiment = Experiment(workspace = ws, name = experiment_name)
  run = experiment.start_logging()
  run.tag("Description","My first run!")
  run.log('alpha', 0.03)
  reg = Ridge(alpha = 0.03)
  reg.fit(data['train']['X'], data['train']['y'])
  preds = reg.predict(data['test']['X'])
  run.log('mse', mean_squared_error(preds, data['test']['y']))
  joblib.dump(value = reg, filename = 'model.pkl')
  # Upload file directly to the outputs folder
  run.upload_file(name = 'outputs/model.pkl', path_or_stream = './model.pkl')

  run.complete()
  ```

Komut dosyası ile biter ```run.complete()```, tamamlandı olarak çalıştırmayı işaretler.  Bu, genellikle etkileşimli bir not defteri senaryolarda kullanılır.

## <a name="option-2-use-scriptrunconfig"></a>2. seçenek: ScriptRunConfig kullanma

**ScriptRunConfig** betiği için yapılandırmaları ayarlamayı çalıştırmaları için bir sınıftır. Bu seçenekle tamamlanmasından almak veya izlemek için görsel bir pencere öğesi almak için izleme kodu ekleyebilirsiniz.

Bu örnek, yukarıda temel sklearn Ridge modeli genişletir. Alfa değerleri ölçümleri yakalamak için model ve deneme altında çalıştığı eğitilen modeller üzerinde basit bir parametre için tarama Süpürme yapar. Örnek, bir kullanıcı tarafından yönetilen ortamında yerel olarak çalıştırılır. 

1. Bir eğitim betiği oluşturun. Bu kullanır ```%%writefile%%``` betik klasöre eğitim kod yazma ```train.py```.

  ```python
  %%writefile $project_folder/train.py

  import os
  from sklearn.datasets import load_diabetes
  from sklearn.linear_model import Ridge
  from sklearn.metrics import mean_squared_error
  from sklearn.model_selection import train_test_split
  from azureml.core.run import Run
  from sklearn.externals import joblib

  import numpy as np

  #os.makedirs('./outputs', exist_ok = True)

  X, y = load_diabetes(return_X_y = True)

  run = Run.get_context()

  X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
  data = {"train": {"X": X_train, "y": y_train},
          "test": {"X": X_test, "y": y_test}}

  # list of numbers from 0.0 to 1.0 with a 0.05 interval
  alphas = mylib.get_alphas()

  for alpha in alphas:
      # Use Ridge algorithm to create a regression model
      reg = Ridge(alpha = alpha)
      reg.fit(data["train"]["X"], data["train"]["y"])

      preds = reg.predict(data["test"]["X"])
      mse = mean_squared_error(preds, data["test"]["y"])
      # log the alpha and mse values
      run.log('alpha', alpha)
      run.log('mse', mse)

      model_file_name = 'ridge_{0:.2f}.pkl'.format(alpha)
      # save model in the outputs folder so it automatically get uploaded
      with open(model_file_name, "wb") as file:
          joblib.dump(value = reg, filename = model_file_name)

      # upload the model file explicitly into artifacts 
      run.upload_file(name = model_file_name, path_or_stream = model_file_name)

      # register the model
      #run.register_model(file_name = model_file_name)

      print('alpha is {0:.2f}, and mse is {1:0.2f}'.format(alpha, mse))
  
  ```

2. ```train.py``` Komut dosyası başvuruları ```mylib.py```. Bu dosya ridge modelinde alfa değerleri listesini almak sağlar.

  ```python
  %%writefile $script_folder/mylib.py
  import numpy as np

  def get_alphas():
      # list of numbers from 0.0 to 1.0 with a 0.05 interval
      return np.arange(0.0, 1.0, 0.05)
  ```

3. Bir kullanıcı tarafından yönetilen yerel ortam yapılandırın.

  ```python
  from azureml.core.runconfig import RunConfiguration

  # Editing a run configuration property on-fly.
  run_config_user_managed = RunConfiguration()

  run_config_user_managed.environment.python.user_managed_dependencies = True

  # You can choose a specific Python environment by pointing to a Python path 
  #run_config.environment.python.interpreter_path = '/home/user/miniconda3/envs/sdk2/bin/python'
  ```

4. Gönderme ```train.py``` kullanıcı tarafından yönetilen bir ortamda çalıştırılacak komut. Bu tüm kod klasörü eğitim için gönderilen dahil olmak üzere ```mylib.py``` dosya.

  ```python
  from azureml.core import ScriptRunConfig

  src = ScriptRunConfig(source_directory = './', script = 'train.py', run_config = run_config_user_managed)
  run = experiment.submit(src)
  ```
  
## <a name="view-run-details"></a>Çalıştırma Ayrıntıları görünümü

### <a name="monitor-run-with-jupyter-notebook-widgets"></a>İzleyici pencere öğelerinde Jupyter not defteri çalıştırma
Kullanırken **ScriptRunConfig** göndermek için gereken yöntemini çalıştırır, Jupyter not defteri olan bir pencere öğesi ile çalışma ilerlemesini izleyebilirsiniz. Çalıştırma gönderimi gibi pencere öğesi de zaman uyumsuzdur ve iş tamamlanana kadar her 10-15 saniyede bir canlı güncelleştirmeler sağlar.

1. Jupyter pencere öğesi, çalışma tamamlanması beklenirken görüntüleyin.

  ```python
  from azureml.train.widgets import RunDetails
  RunDetails(run).show()
  ```

  ![Ekran görüntüsü, Jupyter not defteri pencere öğesi](./media/how-to-track-experiments/widgets.PNG)

### <a name="get-log-results-upon-completion"></a>Tamamlandıktan sonra günlük sonuçlarını alma

Beklerken diğer görevleri çalıştırabileceği şekilde onlara eğitim ve izleme modeli arka planda gerçekleştirilir. Ayrıca daha fazla kod çalıştırmadan önce eğitim modeli tamamlanana kadar bekleyebilirsiniz. Kullanırken **ScriptRunConfig**, kullanabileceğiniz ```run.wait_for_completion(show_output = True)``` modeli eğitimi ne zaman tamamlandığını göstermek için. ```show_output``` Bayrağı ayrıntılı çıkış sağlar. 
  
### <a name="query-run-metrics"></a>Sorgu ölçümleri Çalıştır

Eğitilen modeli kullanarak bir ölçümleri görüntüleyebilir ```run.get_metrics()```. Artık tüm en iyi modeli belirlemek için yukarıdaki örnekte, günlüğe kaydedilen ölçümleri de alabilirsiniz.

<a name='view-the-experiment-in-the-web-portal'/>
## <a name="view-the-experiment-in-the-azure-portal"></a>Azure portalında deneme görüntüleyin

Bir deney çalışması tamamlandığında kayıtlı denemeyi çalıştırma kaydı için göz atabilirsiniz. Bunu iki şekilde yapabilirsiniz:

* Farklı Çalıştır URL'sini doğrudan ```print(run.get_portal_url())```
* Çalıştırma adı göndererek çalıştırma ayrıntılarını görüntüleyin. (Bu durumda, ```run```). Bu, deney adı, kimliği, türü, durum, Ayrıntılar sayfası, Azure portalına bir bağlantı ve belgeler için bir bağlantı gösterir.

Farklı Çalıştır bağlantısını çalıştırma ayrıntıları sayfasına doğrudan Azure portalında getirir. Burada, özellikleri, izlenen ölçümler, görüntüleri ve deneme günlüğe kaydedilen grafik görebilirsiniz. Bu durumda, MSE ve alfa değerleri oturum.

  ![Azure portalında çalıştırma ayrıntılarını, ekran görüntüsü](./media/how-to-track-experiments/run-details-page-web.PNG)

Ayrıca, herhangi bir çıkış veya çalıştırma için günlükleri görüntüleyin veya anlık görüntüsünü ve böylelikle deneme klasörü başkalarıyla paylaşabilirsiniz gönderdiğiniz denemeyi indirin.
### <a name="viewing-charts-in-run-details"></a>Grafikler çalıştırma ayrıntılarını görüntüleme

Bir çalıştırma sırasında günlük kaydı ölçümleri farklı kayıt türleri için API'leri kullanma ve bunları Azure portalındaki grafikleri olarak görüntülemek için çeşitli yollar vardır. 

|Günlüğe kaydedilen değeri|Örnek kod| Portalda görüntüle|
|----|----|----|
|Günlük sayısal değerler dizisi| `run.log_list(name='Fibonacci', value=[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89])`|Tek değişkenli çizgi grafik|
|Sürekli olarak kullanılan aynı Ölçüm adı tek bir sayısal değer oturum (içinden benzer bir for döngüsü)| `for i in tqdm(range(-10, 10)):    run.log(name='Sigmoid', value=1 / (1 + np.exp(-i))) angle = i / 2.0`| Tek değişkenli çizgi grafik|
|Bir satır ile 2 sayısal sütunlara tekrar tekrar oturum|`run.log_row(name='Cosine Wave', angle=angle, cos=np.cos(angle))   sines['angle'].append(angle)      sines['sine'].append(np.sin(angle))`|İki değişken çizgi grafik|
|2 sayısal sütunlara sahip günlüğü tablosu|`run.log_table(name='Sine Wave', value=sines)`|İki değişken çizgi grafik|

## <a name="example-notebooks"></a>Örnek Not Defterleri
Aşağıdaki not defterleri, bu makaledeki kavramları göstermektedir:
* [01.Getting-Started/01.Train-within-Notebook/01.Train-within-Notebook.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/01.train-within-notebook)
* [01.Getting-Started/02.Train-on-Local/02.Train-on-Local.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local)
* [01.Getting-Started/06.Logging-api/06.Logging-api.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/06.logging-api/06.logging-api.ipynb)

Bu not defterlerini alın: [!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

Python için Azure Machine Learning SDK'sı kullanma hakkında bilgi edinmek için sonraki adımları deneyin:

* En iyi modeli kaydedin ve öğreticide dağıtmak nasıl bir örneğini görmek [bir Azure Machine Learning ile görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md).

* Bilgi edinmek için nasıl [eğitme PyTorch modeller Azure Machine Learning ile](how-to-train-pytorch.md).
