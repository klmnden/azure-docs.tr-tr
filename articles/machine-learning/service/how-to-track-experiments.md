---
title: Eğitim çalıştırmaları sırasında ölçümleri günlüğe kaydedin
titleSuffix: Azure Machine Learning service
description: Denemelerinizi izleyebilir ve modeli oluşturma işlemi geliştirmek için ölçümleri izleyin. Eğitim betiğinizi günlük ekleme, denemeyi göndermek nasıl, çalışan işin ilerleme durumunu denetlemek nasıl ve çalıştırmanın sonuçlarını görüntüleme öğrenin.
services: machine-learning
author: heatherbshapiro
ms.author: hshapiro
ms.service: machine-learning
ms.subservice: core
ms.workload: data-services
ms.topic: conceptual
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 50cd5969ce02ee5eea0637c950069d684d67b5d3
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58361464"
---
# <a name="log-metrics-during-training-runs-in-azure-machine-learning"></a>Azure Machine Learning'de eğitim sırasında günlük ölçümleri çalıştırır

Azure Machine Learning hizmetinde denemelerinizi izleyebilir ve modeli oluşturma işlemi geliştirmek için ölçümleri izleyin. Bu makalede, günlük eğitim komut dosyanıza ekleyin, bir deneme çalıştırma gönderin, çalıştırmasını izlemek ve çalıştırmanın sonuçlarını görüntülemek öğrenin.

## <a name="list-of-training-metrics"></a>Eğitim ölçümlerin listesi 

Aşağıdaki ölçümler, bir denemeyi eğitim sırasında çalıştırılacak eklenebilir. Bir çalıştırmada izlenebilir daha ayrıntılı bir listesi görmek için bkz: [sınıfı başvuru belgeleri çalıştırma](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py).

|Tür| Funkce Pythonu | Notlar|
|----|:----|:----|
|Skaler değerler |İşlev:<br>`run.log(name, value, description='')`<br><br>Örnek:<br>Run.log ("doğruluğu", 0.95) |Günlük bir sayısal veya dize değeri belirtilen ada sahip farklı çalıştır. Bir ölçüm için bir çalıştırma günlüğe kaydetme, denemeyi çalıştırma kaydı depolanması, ölçüm neden olur.  Bu ölçüm oluşan bir vektörü olarak kabul sonucu bir çalıştırma içinde birden çok kez aynı Ölçüm oturum açabilirsiniz.|
|Listeler|İşlev:<br>`run.log_list(name, value, description='')`<br><br>Örnek:<br>Run.log_list ("doğruluk" [0,6, 0,7, 0.87]) | Belirtilen ada sahip farklı çalıştır değerlerin bir listesini oturum.|
|Satır|İşlev:<br>`run.log_row(name, description=None, **kwargs)`<br>Örnek:<br>Run.log_row ("Y" X üzerinden x = 1, y 0.4 =) | Kullanarak *log_row* kwargs içinde anlatıldığı gibi birden çok sütun içeren bir ölçü oluşturur. Her adlandırılmış parametre ile belirtilen değer bir sütun oluşturur.  *log_row* rasgele bir tanımlama grubu ya da birden çok kez bir döngüde tam bir tablo oluşturmak için oturum volat pouze jednou.|
|Tablo|İşlev:<br>`run.log_table(name, value, description='')`<br><br>Örnek:<br>Run.log_table ("Y" X üzerinden {"x": [1, 2, 3], "y": [0,6, 0,7, 0.89]}) | Sözlük nesnesi, verilen ada sahip farklı çalıştır oturum açın. |
|Görüntüler|İşlev:<br>`run.log_image(name, path=None, plot=None)`<br><br>Örnek:<br>`run.log_image("ROC", plt)` | Görüntü çalıştırma kaydı için oturum açın. Bir görüntü dosyası veya bir matplotlib oturum log_image kullanın çizim farklı çalıştır.  Bu görüntüler, görünür ve çalışma kaydındaki benzer olacaktır.|
|Bir etiketi|İşlev:<br>`run.tag(key, value=None)`<br><br>Örnek:<br>run.tag (","Evet"seçeneği") | Bir dize anahtarı ve isteğe bağlı dize değeri olan çalıştırma etiketleyin.|
|Dosya veya dizin karşıya yükleme|İşlev:<br>`run.upload_file(name, path_or_stream)`<br> <br> Örnek:<br>Run.upload_file ("best_model.pkl", ". / model.pkl") | Çalıştırma kaydı için bir dosya yükleyin. Çalıştırmaları otomatik olarak varsayılan olarak belirtilen çıkış dizini dosyasında yakala ". / çıkışlar" türleri çoğu çalıştırın.  Yalnızca ek dosyalar yüklenmek üzere gerektiğinde kullanım upload_file veya bir çıktı dizini belirtilmedi. Eklemeyi önerin `outputs` BT'nin çıkış dizinine karşıya, böylece adı. Tüm ilişkili olan dosyaları listeleyebilirsiniz bu çalıştırma kaydı tarafından çağırılır `run.get_file_names()`|

> [!NOTE]
> Skalerler, listeler, satır ve tablolar için ölçümleri türü olabilir: kayan noktalı sayı, tamsayı veya dize.

## <a name="start-logging-metrics"></a>Günlüğe kaydetme ölçümleri Başlat

İzlemek veya denemenizi izlemek istiyorsanız, çalıştırma gönderdiğinizde oturum başlatmak için kod eklemeniz gerekir. Çalıştırma gönderim tetiklemek için yollar şunlardır:
* __Run.start_logging__ - günlük işlevleri eğitim komut dosyanıza ekleyin ve bir etkileşimli günlüğe kaydetme oturumu içinde belirtilen denemeyi başlatın. **start_logging** dizüstü bilgisayarlar gibi senaryolarda kullanım için etkileşimli bir çalışma oluşturur. Oturumu sırasında günlüğe kaydedilen tüm ölçümler, denemeyi çalıştırma kaydı eklenir.
* __ScriptRunConfig__ - günlük işlevleri eğitim komut dosyanıza ekleyin ve Çalıştır tüm betik klasörünü yükleyemedi.  **ScriptRunConfig** betiği için yapılandırmaları ayarlamayı çalıştırmaları için bir sınıftır. Bu seçenekle tamamlanmasından almak veya izlemek için görsel bir pencere öğesi almak için izleme kodu ekleyebilirsiniz.

## <a name="set-up-the-workspace"></a>Çalışma alanını ayarlama
Günlüğe kaydetme ve deneme gönderme eklemeden önce çalışma alanını ayarlamanız gerekir.

1. Çalışma alanı yükleyin. Çalışma alanı yapılandırması ayarlama hakkında daha fazla bilgi bağlantısındaki [bir Azure Machine Learning hizmeti çalışma alanı oluşturma](setup-create-workspace.md#sdk).

   ```python
   from azureml.core import Experiment, Run, Workspace
   import azureml.core
  
   ws = Workspace(workspace_name = <<workspace_name>>,
               subscription_id = <<subscription_id>>,
               resource_group = <<resource_group>>)
   ```
  
## <a name="option-1-use-startlogging"></a>1. seçenek: Start_logging kullanın

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
   # Get an experiment object from Azure Machine Learning
   experiment = Experiment(workspace = ws, name = "train-within-notebook")
  
   # Create a run object in the experiment
   run = experiment.start_logging()# Log the algorithm parameter alpha to the run
   run.log('alpha', 0.03)

   # Create, fit, and test the scikit-learn Ridge regression model
   regression_model = Ridge(alpha=0.03)
   regression_model.fit(data['train']['X'], data['train']['y'])
   preds = regression_model.predict(data['test']['X'])

   # Output the Mean Squared Error to the notebook and to the run
   print('Mean Squared Error is', mean_squared_error(data['test']['y'], preds))
   run.log('mse', mean_squared_error(data['test']['y'], preds))

   # Save the model to the outputs directory for capture
   joblib.dump(value=regression_model, filename='outputs/model.pkl')

   # Take a snapshot of the directory containing this notebook
   run.take_snapshot('./')

   # Complete the run
   run.complete()
  
   ```

Komut dosyası ile biter ```run.complete()```, tamamlandı olarak çalıştırmayı işaretler.  Bu işlev, genellikle etkileşimli bir not defteri senaryolarda kullanılır.

## <a name="option-2-use-scriptrunconfig"></a>2. seçenek: ScriptRunConfig kullanın

[**ScriptRunConfig** ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py) betiği için yapılandırmaları ayarlamayı çalıştırmaları için bir sınıftır. Bu seçenekle tamamlanmasından almak veya izlemek için görsel bir pencere öğesi almak için izleme kodu ekleyebilirsiniz.

Bu örnek, yukarıda temel sklearn Ridge modeli genişletir. Alfa değerleri ölçümleri yakalamak için model ve deneme altında çalıştığı eğitilen modeller üzerinde basit bir parametre için tarama Süpürme yapar. Örnek, bir kullanıcı tarafından yönetilen ortamında yerel olarak çalıştırılır. 

1. Bir eğitim betiği oluşturmak `train.py`.

   ```python
   # train.py

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

2. `train.py` Komut dosyası başvuruları `mylib.py` ridge modelinde alfa değerleri listesi sağlar.

   ```python
   # mylib.py
  
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
  
   experiment = Experiment(workspace=ws, name="train-on-local")
   src = ScriptRunConfig(source_directory = './', script = 'train.py', run_config = run_config_user_managed)
   run = experiment.submit(src)
   ```

## <a name="cancel-a-run"></a>Bir çalıştırmayı iptal et

Deney adı bildiğiniz ve çalıştırma kimliği sürece nesne başvurusu kaybetmiş olsanız bile Alter çalıştırma gönderildikten iptal edebilirsiniz 


```python
from azureml.core import Experiment
exp = Experiment(ws, "my-experiment-name")

# if you don't know the run id, you can list all runs under an experiment
for r in exp.get_runs():  
    print(r.id, r.get_status())

# if you know the run id, you can "rehydrate" the run
from azureml.core import get_run
r = get_run(experiment=exp, run_id="my_run_id", rehydrate=True)
  
# check the returned run type and status
print(type(r), r.get_status())

# you can cancel a run if it hasn't completed or failed
if r.get_status() not in ['Complete', 'Failed']:
    r.cancel()
```
Şu anda yalnızca ScriptRun ve PipelineRun türleri iptal işlemini destekler.

Ayrıca, aşağıdaki komutu kullanarak CLI ile bir çalıştırma iptal edebilirsiniz:
```shell
az ml run cancel -r <run_id> -p <project_path>
```


## <a name="view-run-details"></a>Çalıştırma Ayrıntıları görünümü

### <a name="monitor-run-with-jupyter-notebook-widgets"></a>İzleyici pencere öğelerinde Jupyter not defteri çalıştırma
Kullanırken **ScriptRunConfig** göndermek için gereken yöntemini çalıştırır, Jupyter not defteri olan bir pencere öğesi ile çalışma ilerlemesini izleyebilirsiniz. Çalıştırma gönderimi gibi pencere öğesi de zaman uyumsuzdur ve iş tamamlanana kadar her 10-15 saniyede bir canlı güncelleştirmeler sağlar.

1. Jupyter pencere öğesi, çalışma tamamlanması beklenirken görüntüleyin.

   ```python
   from azureml.widgets import RunDetails
   RunDetails(run).show()
   ```

   ![Ekran görüntüsü, Jupyter not defteri pencere öğesi](./media/how-to-track-experiments/widgets.PNG)

2. **[Çalıştırmalarını otomatik makine öğrenme]**  Grafikleri önceki bir çalıştırma işleminden erişmek için. Değiştirin `<<experiment_name>>` uygun deney adı ile:

   ``` 
   from azureml.widgets import RunDetails
   from azureml.core.run import Run

   experiment = Experiment (workspace, <<experiment_name>>)
   run_id = 'autoML_my_runID' #replace with run_ID
   run = Run(experiment, run_id)
   RunDetails(run).show()
   ```

   ![Jupyter not defteri pencere otomatik Machine Learning için](./media/how-to-track-experiments/azure-machine-learning-auto-ml-widget.png)


Daha fazla işlem hattını bir işlem hattı tıklama ayrıntılarını görüntülemek için keşfetmek tabloda istediğiniz ve grafikleri, Azure portalından bir açılır pencere içinde işlenir.

### <a name="get-log-results-upon-completion"></a>Tamamlandıktan sonra günlük sonuçlarını alma

Beklerken diğer görevleri çalıştırabileceği şekilde onlara eğitim ve izleme modeli arka planda gerçekleştirilir. Ayrıca daha fazla kod çalıştırmadan önce eğitim modeli tamamlanana kadar bekleyebilirsiniz. Kullanırken **ScriptRunConfig**, kullanabileceğiniz ```run.wait_for_completion(show_output = True)``` modeli eğitimi ne zaman tamamlandığını göstermek için. ```show_output``` Bayrağı ayrıntılı çıkış sağlar. 
  
### <a name="query-run-metrics"></a>Sorgu ölçümleri Çalıştır

Eğitilen modeli kullanarak bir ölçümleri görüntüleyebilir ```run.get_metrics()```. Artık tüm en iyi modeli belirlemek için yukarıdaki örnekte, günlüğe kaydedilen ölçümleri de alabilirsiniz.

<a name="view-the-experiment-in-the-web-portal"></a>
## <a name="view-the-experiment-in-the-azure-portal"></a>Azure portalında deneme görüntüleyin

Bir deney çalışması tamamlandığında kayıtlı denemeyi çalıştırma kaydı için göz atabilirsiniz. Yapabileceğiniz iki yolla geçmişine erişebilir:

* Farklı Çalıştır URL'sini doğrudan ```print(run.get_portal_url())```
* Çalıştırma adı göndererek çalıştırma ayrıntılarını görüntüleyin. (Bu durumda, ```run```). Bu şekilde deney adı, kimliği, türü, durum, Ayrıntılar sayfası, Azure portalına bir bağlantı ve belgeler için bir bağlantı gösterir.

Farklı Çalıştır bağlantısını çalıştırma ayrıntıları sayfasına doğrudan Azure portalında getirir. Burada, özellikleri, izlenen ölçümler, görüntüleri ve deneme günlüğe kaydedilen grafik görebilirsiniz. Bu durumda, MSE ve alfa değerleri oturum.

  ![Azure portalında çalışma ayrıntıları](./media/how-to-track-experiments/run-details-page-web.PNG)

Ayrıca, herhangi bir çıkış veya çalıştırma için günlükleri görüntüleyin veya anlık görüntüsünü ve böylelikle deneme klasörü başkalarıyla paylaşabilirsiniz gönderdiğiniz denemeyi indirin.

### <a name="viewing-charts-in-run-details"></a>Grafikler çalıştırma ayrıntılarını görüntüleme

Bir çalıştırma sırasında günlük kaydı ölçümleri farklı kayıt türleri için API'leri kullanma ve bunları Azure portalındaki grafikleri olarak görüntülemek için çeşitli yollar vardır. 

|Günlüğe kaydedilen değeri|Örnek kod| Portalda görüntüle|
|----|----|----|
|Günlük sayısal değerler dizisi| `run.log_list(name='Fibonacci', value=[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89])`|Tek değişkenli çizgi grafik|
|Sürekli olarak kullanılan aynı Ölçüm adı tek bir sayısal değer oturum (içinden benzer bir for döngüsü)| `for i in tqdm(range(-10, 10)):    run.log(name='Sigmoid', value=1 / (1 + np.exp(-i))) angle = i / 2.0`| Tek değişkenli çizgi grafik|
|Bir satır ile 2 sayısal sütunlara tekrar tekrar oturum|`run.log_row(name='Cosine Wave', angle=angle, cos=np.cos(angle))   sines['angle'].append(angle)      sines['sine'].append(np.sin(angle))`|İki değişken çizgi grafik|
|2 sayısal sütunlara sahip günlüğü tablosu|`run.log_table(name='Sine Wave', value=sines)`|İki değişken çizgi grafik|

<a name="auto"></a>
## <a name="understanding-automated-ml-charts"></a>ML grafikleri anlama otomatik

Bir otomatik bir not defteri ML işi gönderdikten sonra bu çalıştırma geçmişini, machine learning hizmeti çalışma alanını içinde bulunabilir. 

Aşağıdakiler hakkında daha fazla bilgi edinin:
+ [Grafikler ve sınıflandırma modelleri için Eğriler](#classification)
+ [Grafikler ve graflar için regresyon modelleri](#regression)
+ [Modeli açıklayan özelliği](#model-explain-ability-and-feature-importance)


### <a name="view-the-run-charts"></a>Çalıştırma grafikleri görüntüleme

1. Çalışma alanınıza gidin. 

1. Seçin **denemeleri** çalışma alanınızın en soldaki panelde.

   ![Deneme menüsünün ekran görüntüsü](./media/how-to-track-experiments/azure-machine-learning-auto-ml-experiment_menu.PNG)

1. İlginizi çeken bir denemeyi seçin.

   ![Deneme listesi](./media/how-to-track-experiments/azure-machine-learning-auto-ml-experiment_list.PNG)

1. Tabloda, çalıştırma numarası seçin.

   ![Denemeyi çalıştırma](./media/how-to-track-experiments/azure-machine-learning-auto-ml-experiment_run.PNG)

1. Tabloda, yineleme numarasını daha fazlasını keşfetmek istediğiniz modeli seçin.

   ![Deneme modeli](./media/how-to-track-experiments/azure-machine-learning-auto-ml-experiment_model.PNG)



### <a name="classification"></a>Sınıflandırma

Otomatik makine öğrenimi özellikleri, Azure Machine Learning kullanarak oluşturduğunuz her sınıflandırma modeli için aşağıdaki grafikleri görebilirsiniz: 
+ [Karışıklık Matrisi](#confusion-matrix)
+ [Duyarlık geri çekme grafiği](#precision-recall-chart)
+ [Alıcı bir işlem özelliklerini (ya da ROC)](#ROC)
+ [Eğri Yükselt](#lift-curve)
+ [Kazançlar eğri](#gains-curve)
+ [Ayar çizimi](#calibration-plot)

#### <a name="confusion-matrix"></a>Karışıklık Matrisi

Karışıklık matrisi performansını bir sınıflandırma modeli tanımlamak için kullanılır. Her bir satır gerçek sınıf örneklerini görüntüler ve her sütun, tahmin edilen sınıf örneklerini temsil eder. Karışıklık matrisi doğru sınıflandırılmış etiketleri ve belirli bir model için yanlış sınıflandırılmış etiketleri gösterir.

Sınıflandırma sorunlar için Azure Machine Learning, bir karışıklık matrisi otomatik olarak oluşturulan her model için sağlar. Her bir karışıklık matrisi için otomatik ML doğru sınıflandırılmış etiketleri kırmızı, yeşil ve yanlış sınıflandırılmış etiket olarak gösterir. Dairenin boyutu bu depo içindeki örnek sayısını temsil eder. Ayrıca, tahmin edilen her etiket ve her true etiket sıklığı sayısı bitişik çubuk grafiklerdeki sağlanır. 

Örnek 1: Düşük doğruluk sınıflandırma modeliyle ![düşük doğruluk ile bir sınıflandırma modeli](./media/how-to-track-experiments/azure-machine-learning-auto-ml-confusion_matrix1.PNG)

Örnek 2: Yüksek doğruluk (idealdir) ile bir sınıflandırma modeli ![yüksek doğruluk ile bir sınıflandırma modeli](./media/how-to-track-experiments/azure-machine-learning-auto-ml-confusion_matrix2.PNG)


#### <a name="precision-recall-chart"></a>Duyarlık geri çekme grafiği

Bu grafiği, duyarlık geri çekme eğrileri kesinlik ve belirli iş sorununuz için geri çağırma arasında kabul edilebilir bir ilişki hangi modelle belirlemek her model için karşılaştırabilirsiniz. Bu grafik, makro ortalama duyarlık geri çekme, mikro ortalama duyarlık geri çekme ve duyarlık-tüm sınıflar için bir model ile ilişkili geri çağırma gösterir.

Terim duyarlık sınıflandırıcı tüm örnekleri doğru şekilde etiketlemek için bu özelliği temsil eder. Geri çağırma belirli bir etiketi tüm örneklerini bulmak bir sınıflandırıcı özelliği temsil eder. Duyarlık geri çekme eğriyi iki Bu kavramlar arasındaki ilişkiyi gösterir. İdeal olarak, model % 100 kesinlik ve % 100 doğruluğu sahip olması gerekir.

Örnek 1: Düşük duyarlılık ve düşük geri çağırma sahip bir sınıflandırma modeli ![düşük duyarlılık ve düşük geri çağırma sahip bir sınıflandırma modeli](./media/how-to-track-experiments/azure-machine-learning-auto-ml-precision_recall1.PNG)

Örnek 2: ~ %100 kesinlik ve ~ %100 çağırma (idealdir) ile bir sınıflandırma modeli ![sınıflandırma modeli Yüksek duyarlılık ve çağırma](./media/how-to-track-experiments/azure-machine-learning-auto-ml-precision_recall2.PNG)

#### <a name="roc"></a>ROC

Özelliği (veya ROC) çalışan ve belirli bir modelde yanlış sınıflandırılmış etiketlerini doğru sınıflandırılmış etiketlerin bir çizim alıcıdır. Eğitim modeller olarak yüksek sapması veri kümeleri üzerinde yanlış pozitif etiketleri gösterme ROC eğrisi daha bilgilendirici olabilir.

Örnek 1: Düşük true etiketleri ve yüksek false etiketleri ile bir sınıflandırma modeli ![düşük true etiketleri ve yüksek false etiketleri ile sınıflandırma modeli](./media/how-to-track-experiments/azure-machine-learning-auto-ml-roc1.PNG)

Örnek 2: Yüksek true etiketleri ve düşük false etiketleri ile bir sınıflandırma modeli ![yüksek true etiketleri ve düşük false etiketleri ile bir sınıflandırma modeli](./media/how-to-track-experiments/azure-machine-learning-auto-ml-roc2.PNG)

#### <a name="lift-curve"></a>Eğri Yükselt

Otomatik olarak bu belirli modelin değeri kazanç görüntülemek için taban çizgisine Azure Machine Learning ile geliştirilmiş modelinin lift karşılaştırabilirsiniz.

Lift grafikleri bir sınıflandırma modeli performansını değerlendirmek için kullanılır. Bu, ne kadar daha iyi, bir model karşılaştırıldığında bir model ile yapmak, bekleyebileceğiniz gösterir. 

Örnek 1: Model gerçekleştiren bir rastgele seçim modeli daha da kötüsü ![rastgele seçim daha da kötüsü olarak modelleyen bir sınıflandırma modeli](./media/how-to-track-experiments/azure-machine-learning-auto-ml-lift_curve1.PNG)

Örnek 2: Model gerçekleştiren bir rastgele seçim modeli daha iyi ![daha iyi bir sınıflandırma modeli](./media/how-to-track-experiments/azure-machine-learning-auto-ml-lift_curve2.PNG)

#### <a name="gains-curve"></a>Kazançlar eğri

Kazançlar grafiği her veri bölümü tarafından bir sınıflandırma modeli performansını değerlendirir. Bu, veri kümesinin ne kadar iyi gerçekleştirmesini bekleyebilirsiniz karşı bir rastgele seçim modeli karşılaştırması, her yüzdebirlik dilime gösterir.

İstenen bir erişim modelden karşılık gelen bir yüzdesini kullanarak sınıflandırma kesme seçmenize yardımcı olmak için toplam kazancı grafik kullanın. Bu bilgiler eşlik eden lift grafik sonuçları bakarak başka bir yol sağlar.

Örnek 1: En az kazanç sınıflandırma modeliyle ![kazanç en az bir sınıflandırma modeli](./media/how-to-track-experiments/azure-machine-learning-auto-ml-gains_curve1.PNG)

Örnek 2: Önemli kazanç sınıflandırma modeliyle ![önemli kazanç ile bir sınıflandırma modeli](./media/how-to-track-experiments/azure-machine-learning-auto-ml-gains_curve2.PNG)

#### <a name="calibration-plot"></a>Ayar çizimi

Tüm sınıflandırma sorunları ayarlama satır micro-ortalama, makro ortalama ve her sınıfta belirli bir Tahmine dayalı model için gözden geçirebilirsiniz. 

Bir ayar çizim, Tahmine dayalı bir modelin güvenle görüntülemek için kullanılır. Bunu "olasılık" temsil ettiği bazı etiket altında belirli bir örneğine ait olasılığını tahmin edilen olasılığını ve gerçek olasılığını arasındaki ilişkiyi gösteren göre yapar. Y ile de ayarlanmış bir model hizalar = x, Öngörüler makul başarılara olduğu satır. Fazla kişiye modeli ile y hizalar = 0 satırı, burada tahmin olasılık mevcuttur ancak gerçek hiç olasılık yoktur.

Örnek 1: Daha iyi ayarlanmış bir model ![ daha iyi ayarlanmış modeli](./media/how-to-track-experiments/azure-machine-learning-auto-ml-calib_curve1.PNG)

Örnek 2: Fazla kişiye modeli ![fazla kişiye modeli](./media/how-to-track-experiments/azure-machine-learning-auto-ml-calib_curve2.PNG)

### <a name="regression"></a>Regresyon
Her bir regresyon modeli için otomatik makine öğrenimi özellikleri, Azure Machine Learning kullanarak yapı, aşağıdaki grafikleri görebilirsiniz: 
+ [Tahmin edilen vs. TRUE](#pvt)
+ [Kalanlar Histogramı](#histo)

<a name="pvt"></a>

#### <a name="predicted-vs-true"></a>Tahmin edilen vs. True

Tahmin edilen vs. TRUE, tahmin edilen bir değer ile ilişkilendirilmesini true değeri için regresyon problemi arasındaki ilişkiyi gösterir. Bu grafiğin y yakın olarak model performansını ölçmek için kullanılan = x satır tahmin edilen değerler, Tahmine dayalı bir model doğruluğunu daha iyi.

Sonrasında her çalıştırma, tahmin edilen bir karşılaştırması için her bir regresyon modeli true grafı görebilirsiniz. Veri gizliliği korumak için değerleri birlikte binned ve her bir depo boyutu, grafik alanı alt kısmında bulunan bir çubuk grafik olarak gösterilir. Tahmine dayalı model hatası kenar boşlukları, model olması gerektiği ideal değerle gösteren açık gölge alanı ile karşılaştırabilirsiniz.

Örnek 1: Bir regresyon modeli ile Öngörüler düşük doğruluk ![Öngörüler düşük doğruluk ile bir regresyon modeli](./media/how-to-track-experiments/azure-machine-learning-auto-ml-regression1.PNG)

Örnek 2: Yüksek doğruluk, Öngörüler ile bir regresyon modeli ![kendi Öngörüler yüksek doğruluk ile bir regresyon modeli](./media/how-to-track-experiments/azure-machine-learning-auto-ml-regression2.PNG)

<a name="histo"></a>

#### <a name="histogram-of-residuals"></a>Kalanlar Histogramı

Bir fazlalığı gözlemlenen y – tahmin edilen y temsil eder. Bir hata ile düşük sapması göstermek için 0 ortalanmış bir bell eğri olarak Kalanlar histogram şeklinde. 

Örnek 1: Bir regresyon modeli ile kendi hataları sapması ![SA regresyon modeli ile kendi hataları sapması](./media/how-to-track-experiments/azure-machine-learning-auto-ml-regression3.PNG)

Örnek 2: Hataları daha eşit dağıtım ile bir regresyon modeli ![bir regresyon modeli ile hataları daha eşit dağıtım](./media/how-to-track-experiments/azure-machine-learning-auto-ml-regression4.PNG)

### <a name="model-explain-ability-and-feature-importance"></a>Model açıklayan özelliği ve özellik önem derecesi

Özellik önem nasıl değerli bir model oluşturma, her bir özellik olduğunu gösteren bir puan verir. Genel olarak Tahmine dayalı bir model sınıfı başına model özellik önem puanını gözden geçirebilirsiniz. Özellik nasıl önemini her sınıf karşı ve genel karşılaştırır görebilirsiniz.

![Özellik açıklama özelliği](./media/how-to-track-experiments/azure-machine-learning-auto-ml-feature_explain1.PNG)

## <a name="example-notebooks"></a>Örnek Not Defterleri
Aşağıdaki not defterleri, bu makaledeki kavramları göstermektedir:
* [How-to-use-azureml/Training/Train-within-Notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook)
* [How-to-use-azureml/Training/Train-on-Local](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-on-local)
* [How-to-use-azureml/Training/Logging-api/Logging-api.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/logging-api)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

Python için Azure Machine Learning SDK'sı kullanma hakkında bilgi edinmek için sonraki adımları deneyin:

* En iyi modeli kaydedin ve öğreticide dağıtmak nasıl bir örneğini görmek [bir Azure Machine Learning ile görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md).

* Bilgi edinmek için nasıl [eğitme PyTorch modeller Azure Machine Learning ile](how-to-train-pytorch.md).
