---
title: Sürümde yenilikler nelerdir?
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmeti ve makine öğrenimi için en son güncelleştirmeleri öğrenin ve Python SDK'ları veri hazırlama.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
ms.author: larryfr
author: Blackmist
ms.date: 05/14/2019
ms.custom: seodec18
ms.openlocfilehash: 7aedb0804626d1204121568904763bec5e83e858
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67786262"
---
# <a name="azure-machine-learning-service-release-notes"></a>Azure Machine Learning hizmeti sürüm notları

Bu makalede, Azure Machine Learning hizmet sürümleri hakkında bilgi edinin.  Her bir SDK tam bir açıklaması için başvuru belgelerini ziyaret edin:
+ Azure Machine Learning'ın [ **Python için ana SDK'sı**](https://aka.ms/aml-sdk)
+ Azure Machine Learning [ **veri hazırlama SDK'sı**](https://aka.ms/data-prep-sdk)

Bkz: [bilinen sorunların listesi](resource-known-issues.md) bilinen hataların ve geçici çözümleri hakkında bilgi edinmek için.

## <a name="2019-07-09"></a>2019-07-09

### <a name="visual-interface"></a>Görsel arabirim
+ **Önizleme özellikleri**
  + Görsel arabirim eklendi "Yürütme R betiği" modülü.

### <a name="azure-machine-learning-sdk-for-python-v1048"></a>Azure Machine SDK için Python v1.0.48 Learning

+ **Yeni Özellikler**
  + **azureml opendatasets**
    + **azureml contrib opendatasets** olarak artık kullanıma sunuldu **azureml opendatasets**. Eski paketi çalışmaya devam, ancak kullanarak öneririz **azureml opendatasets** daha zengin özellikleri ve geliştirmeleri için geçmeden.
    + Bu yeni paketi, açık veri kümeleri AML çalışma alanındaki veri kümesi olarak Kaydet ve veri kümesi sunar. hangi işlevleri yararlanarak olanak tanır.
    + Konum, hava durumu gibi bazı veri kümesi için birleştirir ve ayrıca Pandas/SPARK veri çerçevelerini olarak açık veri kümelerini kullanma gibi var olan yetenekleri içerir.

+ **Önizleme özellikleri**
    + HyperDriveConfig kullanarak bir işlem hattı, Hiper parametre ayarı desteklemek için parametre olarak işlem hattı nesne artık kabul edebilir.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + **azureml-train-automl**
    + Dönüştürme işleminin ardından sütun türleri kaybetmeden ilgili hata düzeltildi.
    + Hiçbiri (s) başında içeren bir nesne türü olmasını y_query izin vermek için hata düzeltildi. 
    + Puanları sabit kaldığını olmasa bile, sonuçta elde edilen topluluğu gereksiz yere büyümekte topluluğu seçimi yordamda sorunu düzeltildi.
    + AutoMLStep whitelist_models ve blacklist_models ayarlarla sorun düzeltildi.
    + Azure ML işlem hatları bağlamında AutoML kullanılmış olduğunda ön işleme kullanımı önleyen bir sorun düzeltildi.
  + **azureml opendatasets**
    + Taşınan azureml-contrib-opendatasets azureml opendatasets için.
    + İzin verilen açık veri kümesi sınıflarını AML çalışma alanına kayıtlı olması ve AML veri kümesi özellikleri sorunsuz bir biçimde yararlanın.
    + Geliştirilmiş NoaaIsdWeather SPARK olmayan sürümü performansı önemli ölçüde zenginleştirin.
  + **azureml-explain-model**
    + İnterpretability nesneler için çevrimiçi belgeleri güncelleştirildi.
    + Eklenen açıklama taklit edecek şekilde batch_size olduğunda include_local genel açıklamaları DecisionTreeExplainableModel yürütme süresini artırmak için toplu akış için = False.
    + Sorun düzeltildi burada `explanation.expected_values` kayan noktalı bir listesi yerine bir float içinde bazen döndürecektir.
    + Modeli kitaplığı içinde mimic açıklama için çıkış automl eklenen beklenen değerleri açıklanmaktadır.
    + Raw özelliği önem almak için dönüşümleri bağımsız değişken sağlandığında permütasyon özellik önem düzeltildi.
    + Eklenen açıklama taklit edecek şekilde batch_size olduğunda include_local genel açıklamaları DecisionTreeExplainableModel modeli explainability kitaplığı için yürütme süresini artırmak için toplu akış için = False.
  + **azureml-core**
    + AzureML CLI DBFS veri depoları ekleme özelliği eklendi.
    + Boş bir klasör varsa oluşturulduğu veri deposu karşıya yükleme ile sorunu düzeltildi `target_path` kullanmaya `/`.
    + İki veri kümesi etkin karşılaştırması.
    + Model ve görüntü silme artık bir Yukarı Akış bağımlılık nedeniyle silme başarısız olursa bunlara bağımlı yukarı akış nesneleri alma hakkında daha fazla bilgi sağlar.
    + Kullanılmayan auto_prepare_environment RunConfiguration ayarı kullanım dışı.
  + **azureml-mlflow**
    + Geliştirilmiş kaynak kullanımı azureml.mlflow kullanan uzaktan çalıştırmalar.
    + Azureml mlflow paket belgeleri geliştirdik.
    + Yapıtları "my_dir/yapıt-yolları" yerine "yolları yapıt" altında mlflow.log_artifacts("my_dir") kaydetmek neden olan sorun düzeltildi.
  + **azureml-dataprep**
    + Veri akışı nesneleri artık bir dizi kayıtları oluşturan üzerinden yinelenir.
    + Sorun düzeltildi burada `Dataflow.read_pandas_dataframe` ne zaman başarısız olacağı `in_memory` bağımsız değişkenini True olarak ayarlayın.
    + Pandas DataFrames dize olmayan sütun dizinleri ile Gelişmiş şekilde işlenmesi.
    + Kullanıma sunulan `set_diagnostics_collection()` programlı etkinleştirme/devre dışı bırakmak için telemetri toplama izin vermek için.
    + Eklenen topValues ve bottomValues özetler.
  + **azureml işlem hattı çekirdek**
    + Tüm işlem hattı adımlar için parametre hash_paths kullanım dışıdır ve gelecekte kaldırılacaktır. Kaynak_dizin varsayılan içeriklerini (dışında .amlignore listede veya .gitignore dosyaları) karma
    + Devam eden iyileştirme modülü ve ModuleStep desteklemek için işlem hatları, bunların kullanımını kilidini açmak için RunConfiguration tümleştirme ve ilerideki değişiklikler için hazırlık türü belirli modüller işlem.
  + **azureml işlem hattı adımları**
    + AzureBatchStep: Girdiler/çıktılar bakımından iyileştirilmiş belgeleri.
    + AzureBatchStep: Delete_batch_job_after_finish varsayılan değerini true olarak değiştirdi.
  + **azureml train çekirdek**
    + Dizeleri artık işlem hedefi için hiper parametre ayarı otomatik olarak kabul edilir.
    + Kullanılmayan auto_prepare_environment RunConfiguration ayarı kullanım dışı.
    + Kullanım dışı parametreler `conda_dependencies_file_path` ve `pip_requirements_file_path` sunulmasıyla `conda_dependencies_file` ve `pip_requirements_file` sırasıyla.
  + **azureml opendatasets**
    + NoaaIsdWeather geliştirmek SPARK olmayan sürümü performansı önemli ölçüde zenginleştirin.
    
## <a name="2019-07-01"></a>2019-07-01

### <a name="azure-machine-learning-data-prep-sdk-v117"></a>Azure Machine Learning veri hazırlama SDK v1.1.7

Bu Azure Databricks kullanarak bazı müşteriler için sorunlara neden gibi biz Gelişmiş performans, bir değişikliği geri döndürüldü. Azure Databricks üzerinde bir sorun olduysa, aşağıdaki yöntemlerden birini kullanarak 1.1.7 sürümüne yükseltebilirsiniz:
1. Yükseltmek için bu betiği çalıştırın: `%sh /home/ubuntu/databricks/python/bin/pip install azureml-dataprep==1.1.7`
2. En son veri hazırlığı SDK sürümü yükleyecek kümeyi yeniden oluşturun.

## <a name="2019-06-25"></a>2019-06-25

### <a name="azure-machine-learning-sdk-for-python-v1045"></a>Azure Machine SDK için Python v1.0.45 Learning

+ **Yeni Özellikler**
  + Azureml açıklayan model paketindeki açıklama taklit etmek için karar ağacı vekil modeli ekleme
  + Çıkarım görüntülerinde yüklenmesi için CUDA sürümünü belirtme seçeneği. CUDA 10.0 9.0 ve 9.1 için destek.
  + Azure ML eğitim bilgi temel görüntüleri adresinde artık [Azure ML kapsayıcıları GitHub deposunu](https://github.com/Azure/AzureML-Containers) ve [DockerHub](https://hub.docker.com/_/microsoft-azureml)
  + Eklenen CLI için işlem hattı zamanlamayı destekler. Daha fazla bilgi için "az ml işlem hattı -h" çalıştırın
  + AKS webservice dağıtım yapılandırması ve CLI özel Kubernetes ad alanı parametresi eklendi.
  + İşlem hattı adımların tümünü kullanım dışı hash_paths parametresi
  + Model.Register destekleyen birden çok tek tek dosya kullanımını tek bir model olarak kaydetme `child_paths` parametresi.
  
+ **Önizleme özellikleri**
    + Puanlama explainers artık isteğe bağlı olarak conda kaydedebilir ve daha güvenilir serileştirme ve seri durumundan çıkarma için bilgi pip.
    + Otomatik özellik Seçici için hata düzeltmesi.
    + Düzeltme hataları yeni bir uygulama tarafından kullanıma sunulan yeni API'ye mlflow.azureml.build_image güncelleştirildi.

+ **Bozucu değişiklikler**

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Azureml çekirdekli'den kaldırılan paramiko bağımlılığı. Eski işlem hedefi için eklenen kullanımdan kaldırma uyarıları yöntemleri ekleyin.
  + Run.create_children'ın performansını artırma
  + Öğretmen olasılık şekil değerleri ölçeklendirme için kullanılan ikili dosya sınıflandırıcı ile mimic açıklama içinde olasılıklar sırası düzeltildi
  + Geliştirilmiş hata işleme ve machine Learning otomatik ileti. 
  + Otomatik machine learning için yineleme zaman aşımı sorun düzeltildi.
  + Otomatik machine learning için zaman serisi dönüştürme performansı geliştirildi.

## <a name="2019-06-24"></a>2019-06-24

### <a name="azure-machine-learning-data-prep-sdk-v116"></a>Azure Machine Learning veri hazırlama SDK v1.1.6

+ **Yeni Özellikler**
  + Eklenen en yüksek değerleri için Özet işlevleri (`SummaryFunction.TOPVALUES`) ve alt değerleri (`SummaryFunction.BOTTOMVALUES`).

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Performansını önemli ölçüde geliştirildi `read_pandas_dataframe`.
  + Neden olan hata düzeltildi `get_profile()` başarısız olmasına ikili dosyalara işaret eden bir veri akışı üzerinde.
  + Kullanıma sunulan `set_diagnostics_collection()` programlı etkinleştirme/devre dışı bırakmak için telemetri toplama izin vermek için.
  + Davranışı değişti `get_profile()`. NaN değerleri artık Min, ortalama, standart ve Pandas davranışını hizalar toplam, göz ardı edilir.


## <a name="2019-06-10"></a>2019-06-10

### <a name="azure-machine-learning-sdk-for-python-v1043"></a>Azure Machine SDK için Python v1.0.43 Learning

+ **Yeni Özellikler**
  + Azure Machine Learning artık popüler makine öğrenimi ve veri analizi için framework Scikit-öğrenme birinci sınıf destek sağlar. Kullanarak [ `SKLearn` estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py), kullanıcılar kolayca eğitin ve Scikit-öğrenme modellerini dağıtabilirsiniz.
    + Bilgi edinmek için nasıl [Scikit-öğrenme ile hiper parametre ayarı çalıştırın HyperDrive kullanarak](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-hyperparameter-tune-deploy-with-sklearn/train-hyperparameter-tune-deploy-with-sklearn.ipynb).
  + İşlem birimi ModuleStep yeniden kullanılabilir yönetmek için modülü ve ModuleVersion sınıfları birlikte işlem hatları oluşturma desteği eklendi.
  + ACI webservices'a kalıcı scoring_uri güncelleştirmeler artık desteklenmektedir. Scoring_uri FQDN için bir IP değişecektir. FQDN için Dns ad etiketi üzerinde deploy_configuration dns_name_label ayarlanarak yapılandırılabilir. 
  + Otomatik makine öğrenme yeni özellikleri:
    + Tahmin için STL özelliği Oluşturucu
    + KMeans kümeleme özelliği Süpürme için etkindir
  + AmlCompute kota onaylar yalnızca daha hızlı hale geldi! Biz, artık bir eşik içinde kota istekleri onaylama işlemi otomatikleştirdiniz. Kotalar nasıl çalıştığı hakkında daha fazla bilgi için bilgi [kotalarını yönetmek nasıl](https://docs.microsoft.com/azure/machine-learning/service/how-to-manage-quotas).

+ **Önizleme özellikleri**
    + İle tümleştirme [MLflow](https://mlflow.org) azureml mlflow paket üzerinden izleme 1.0.0 ([örnek not defterleri](https://aka.ms/azureml-mlflow-examples)).
    + Jupyter not defteri çalıştırma olarak gönderin. [API başvuru belgeleri](https://docs.microsoft.com/python/api/azureml-contrib-notebook/azureml.contrib.notebook?view=azure-ml-py)
    + Genel önizlemeye sunulduğunu [veri değişikliklerini algılayıcısı](https://docs.microsoft.com/python/api/azureml-contrib-datadrift/azureml.contrib.datadrift?view=azure-ml-py) azureml contrib datadrift paketi aracılığıyla ([örnek not defterleri](https://aka.ms/azureml-datadrift-example)). Veri değişikliklerini başlıca nedenlerdendir burada doğruluğu zamanla düşüyor biridir. Veri sunulduğunda üretimde model için model üzerinde eğitim almış verilerden farklı olur. AML veri değişikliklerini algılayıcısı, müşterinin veri değişikliklerini izlemek için yardımcı olur ve kaymaları algılandığında uyarı gönderir. 

+ **Bozucu değişiklikler**

+ **Hata düzeltmeleri ve geliştirmeleri**
  + RunConfiguration yükleyin ve önceki davranışı için tam arka/compat ile tam dosya yolunu belirterek destekler kaydedin.
  + Varsayılan olarak kapalı ServicePrincipalAuthentication, önbelleğe almayı eklendi.
  + Ölçüm aynı adla birden fazla çizim günlüğünü etkinleştirin.
  + Model sınıfı artık azureml.core düzgün bir şekilde alınabilir (`from azureml.core import Model`).
  + İşlem hattı adımlarda `hash_path` parametresi artık kullanım dışı. Listede .amlignore veya .gitignore dosyaları hariç tüm kaynak_dizin karma yeni davranıştır.
  + İşlem hattı paketler, çeşitli `get_all` ve `get_all_*` yöntemleri, şunun için kaldırılmıştır `list` ve `list_*`sırasıyla.
  + azureml.Core.get_run artık özgün çalıştırması türü döndürmeden önce içeri aktarılacak sınıfları gerektirir.
  + Bazı çağrıları WebService güncelleştirme bir güncelleştirme burada tetiklendi değil bir sorun düzeltildi.
  + Zaman aşımı AKS webservices'a üzerinde Puanlama 300000ms 5 MSN arasında olmalıdır. En fazla 5 dakikaya kadar indirgenmesine scoring_timeout_ms Puanlama istekleri için 1 dakika izin verilir.
  + LocalWebservice nesneler artık sahip `scoring_uri` ve `swagger_uri` özellikleri.
  + Çıkış dizini oluşturma ve çıkış dizini karşıya kullanıcı işlemi dışında taşındı. Çalıştırma geçmişi her kullanıcı işlem içinde çalıştırmak için SDK'sı etkinleştirilmiş. Bu, çalışan tarafından dağıtılmış eğitimi deneyimli bazı eşitleme sorunlarını gidermeniz gerekir.
  + Kullanıcı işlemi adından yazılan azureml günlük adını artık işlem adı (için Dağıtılmış yalnızca eğitme) ve PID içerir.

### <a name="azure-machine-learning-data-prep-sdk-v115"></a>Azure Machine Learning veri hazırlama SDK v1.1.5

+ **Hata düzeltmeleri ve geliştirmeleri**
  + 2-basamaklı yıl biçimini sahip yorumlanan datetime değerleri için aralığı geçerli yıl Windows olabilir yayın eşleşecek şekilde güncelleştirildi. Aralığın 1930-2029 1950 2049 değiştirildi.
  + Okuma sırasında dosya ve ayarı `handleQuotedLineBreaks=True`, `\r` yeni bir satır kabul edilir.
  + Neden bir hata düzeltildi `read_pandas_dataframe` bazı durumlarda başarısız.
  + Geliştirilmiş performansını `get_profile`.
  + Geliştirilmiş hata iletileri.

## <a name="2019-05-28"></a>2019-05-28

### <a name="azure-machine-learning-data-prep-sdk-v114"></a>Azure Machine Learning veri hazırlama SDK v1.1.4

+ **Yeni Özellikler**
  + Aşağıdaki ifade dil işlevleri artık çıkarma ve yeni bir sütuna tarih/saat değerleri için de kullanabilirsiniz.
    + `RegEx.extract_record()` Yeni bir sütun tarih/saat öğeleri ayıklar.
    + `create_datetime()` DateTime nesnesi ayrı datetime öğeleri oluşturur.
  + Çağrılırken `get_profile()`, artık quantile sütun değerlerinin anların olduğunu açıkça belirtmek için (tahmini) etiketlenmiştir görebilirsiniz.
  + Artık ** Azure Blob depolama alanından okurken Glob.
    + Örneğin `dprep.read_csv(path='https://yourblob.blob.core.windows.net/yourcontainer/**/data/*.csv')`

+ **Hata düzeltmeleri**
  + Bir uzak kaynaktan (Azure Blob) bir Parquet dosya okuma için ilgili bir hata düzeltildi.

## <a name="2019-05-14"></a>2019-05-14

### <a name="azure-machine-learning-sdk-for-python-v1039"></a>Azure Machine SDK için Python v1.0.39 Learning
+ **Değişiklikleri**
  + Çalıştırma yapılandırma auto_prepare_environment seçeneği kullanım dışı, varsayılan haline otomatik hazırlama.

## <a name="2019-05-08"></a>2019-05-08

### <a name="azure-machine-learning-data-prep-sdk-v113"></a>Azure Machine Learning veri hazırlama SDK v1.1.3

+ **Yeni Özellikler**
  + Read_postgresql çağırma veya bir veri deposu kullanılarak bir PostgresSQL veritabanından okumak için destek eklendi.
    + Nasıl yapılır kılavuzlarından örneklere bakın:
      + [Veri alımı not defteri](https://aka.ms/aml-data-prep-ingestion-nb)
      + [Veri deposu not defteri](https://aka.ms/aml-data-prep-datastore-nb)

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Tür dönüştürme sütunu giderilen sorunlar:
  + Artık doğru şekilde Boole veya sayısal bir sütun için bir Boole sütunu dönüştürür.
  + Artık bir tarih sütunu tarih türü olarak ayarlanacak çalışırken başarısız olmaz.
  + Geliştirilmiş birleştirmetürü türleri ve eşlik eden başvuru belgeleri. İki veri akışı eklerken, artık bu birleşim türlerinin birini belirtebilirsiniz:
    + NONE, EŞLEŞEN, İÇ, UNMATCHLEFT, LEFTANTI, LEFTOUTER, UNMATCHRIGHT, RIGHTANTI, RIGHTOUTER, FULLANTI, TAM.
  + Geliştirilmiş veri diğer tarih biçimleri tanımak için çıkarım yazın.

## <a name="2019-05-06"></a>2019-05-06

### <a name="azure-portal"></a>Azure portal

Azure Portalı'nda artık şunları yapabilirsiniz:
+ Oluşturma ve otomatik ML denemeleri çalıştırma 
+ Örnek Jupyter not defterlerini denemek için bir not defteri VM oluşturma veya kendi.
+ Yeni yazma (Önizleme) bölümünde otomatik Machine Learning, görsel arabirim ve barındırılan not defteri Vm'leri içeren Machine Learning hizmeti çalışma alanı
    + Otomatik olarak otomatik makine öğrenimini kullanarak model oluşturma 
    + Bir Sürükle ve bırak denemeleri çalıştırmak için görsel arabirim
    + Verileri araştırmak için modeller oluşturun ve Hizmetleri dağıtmak için bir not defteri VM oluşturun.
+ Canlı grafik ve çalışma raporları güncelleştirme ölçüm ve ayrıntıları sayfaları çalıştırın
+ Günlükler, çıktı ve çalıştırma ayrıntıları sayfaları anlık görüntü için güncelleştirilmiş dosya görüntüleyici.
+ Yeni ve geliştirilmiş rapor oluşturma deneyimi denemeleri sekmesindedir. 
+ Azure Machine Learning hizmeti çalışma alanında genel bakış sayfasından config.json dosyasını indirmek için özelliği eklendi.
+ Azure Databricks çalışma alanından Machine Learning hizmeti çalışma alanı oluşturma desteği 

## <a name="2019-04-26"></a>2019-04-26

### <a name="azure-machine-learning-sdk-for-python-v1033"></a>Azure Machine SDK için Python v1.0.33 Learning
+ **Yeni Özellikler**
  + _Workspace.create_ yöntemi artık varsayılan CPU ve GPU kümeleri için küme yapılandırmaları kabul eder.
  + Çalışma alanı oluşturma başarısız olursa, kullanıcıda kaynakları temizlenir.
  + Varsayılan Azure Container Registry SKU'SUNUN temel olarak değiştirildi.
  + Azure Container Registry, gevşek, çalıştırma veya görüntü oluşturmak için gerektiğinde oluşturulur.
  + Eğitim çalıştırmaları için ortamları için destek.

### <a name="notebook-virtual-machine"></a>Not Defteri sanal makine 

Bir not defteri VM, Jupyter not defterleri, makine öğrenimi denemeleri programı, modelleri web uç noktalar olarak dağıtabilir ve Azure Machine Learning Python kullanarak SDK'sı tarafından desteklenen diğer tüm işlemleri gerçekleştirmek için güvenli, Kurumsal kullanıma hazır bir barındırma ortamı kullanın. Bu, çeşitli özellikleri sağlar:
+ [Önceden yapılandırılmış bir not VM hızla çalıştırın](quickstart-run-cloud-notebook.md) , Azure Machine Learning SDK ve ilişkili paketlerin en son sürüme sahip.
+ HTTPS, Azure Active Directory kimlik doğrulama ve yetkilendirme gibi kanıtlanmış teknolojiler aracılığıyla erişimi güvenli hale getirilir.
+ Güvenilir bulut depolama Not defterlerinin ve Azure Machine Learning çalışma alanı blob depolama hesabınızda kod. Dizüstü bilgisayarınızı VM iş kaybetmeden güvenli bir şekilde silebilirsiniz.
+ Keşfetmek ve Azure Machine Learning hizmeti özellikleri denemek için örnek not defterleri makineleridir.
+ Azure Vm'leri, herhangi bir VM türü, tüm paketler, tüm sürücüleri tam özelleştirme özellikleri. 

## <a name="2019-04-26"></a>2019-04-26

### <a name="azure-machine-learning-sdk-for-python-v1033-released"></a>Python v1.0.33 yayımlanan için Azure Machine Learning SDK.

+ Azure ML donanım hızlandırılmış modeller üzerinde [FPGA](concept-accelerate-with-fpgas.md) genel kullanıma sunulmuştur.
  + Artık [azureml hızlandırma modelleri paketi kullanan](how-to-deploy-fpga-web-service.md) için:
    + Desteklenen bir derin sinir ağı (ResNet-50, ResNet 152, DenseNet 121, VGG-16 ve SSD VGG) ağırlıkları eğitin
    + Aktarımlı öğrenme ile desteklenen DNN kullanın
    + Model Yönetimi Hizmeti ile modeli kaydedin ve model kapsayıcılı hale getirme
    + Azure Kubernetes Service (AKS) kümesi içinde bir FPGA ile bir Azure VM için model dağıtma
  + Kapsayıcınıza dağıtmanın bir [Azure veri kutusu Edge](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview) sunucu cihaz
  + Verilerinizi bu gRPC uç noktasıyla puan [örnek](https://github.com/Azure-Samples/aml-hardware-accelerated-models)

### <a name="automated-machine-learning"></a>Otomatik makine öğrenimi

+ Performans iyileştirmesi için dinamik olarak ekleme featurizers etkinleştirmek için üst düzey özellik. Yeni featurizers: iş Gömmeleri, kanıt, hedef Kodlamalar, hedef metin kodlama, küme uzaklık ağırlığı
+ İçinde otomatik ML Train/geçerli işlemek için akıllı CV ayırır
+ Bazı bellek iyileştirme değişiklikleri ve çalışma zamanı performansı geliştirme
+ Performans geliştirmesinden model açıklaması
+ Yerel çalıştırma için ONNX model dönüştürme
+ Subsampling desteği eklendi
+ Akıllı tanımlı hiçbir çıkış ölçütü durduruluyor
+ Yığılmış Kümelemeler

+ Zaman serilerini tahmin etme
  + Tahmin işlevi yeni tahmin   
  + Zaman serisi verileri üzerinde çalışırken kaynak çapraz doğrulama artık kullanabilirsiniz
  + Zaman serisi aksamalar yapılandırmak için eklenen yeni işlevler 
  + Sıralı penceresi toplama özelliklerini desteklemek için eklenen yeni işlevler
  + Yeni tatil algılama ve ülke kodu olarak tanımlandığında özelliği Oluşturucu ayarları denemeler yapın.

+ Azure Databricks
  + Zaman serisi tahmini etkin ve model explainabilty/interpretability özelliği
  + Artık iptal edebilir ve devam et (otomatik ML devam) denemeleri
  + Çok çekirdekli işleme desteği eklendi

### <a name="mlops"></a>MLOps
+ **Yerel dağıtım ve kapsayıcıları Puanlama için hata ayıklama**<br/> Artık, ML model yerel olarak dağıtma ve puanlama dosyası ve beklendiği gibi davranmaya sağlamak için bağımlılıkları üzerinde hızla yineleme.

+ **Sunulan InferenceConfig & Model.deploy()**<br/> Model dağıtımı artık, bir kaynak klasör betiğiyle bir girdisi, bir RunConfig aynı belirterek destekler.  Ayrıca, model dağıtımı, tek bir komut için basitleştirilmiştir.

+ **Git başvurusu izleme**<br/> Bazı sürede yardımcı bir uçtan uca denetim izi'ni korumak için müşteriler temel Git tümleştirme özelliklerini isteme. Git ile ilgili meta verileri (depo, işleme, temiz bir durum) için Azure ML büyük varlıklar arasında biz izleme uyguladınız. Bu bilgiler SDK ve CLI tarafından otomatik olarak toplanır.

+ **Model profil oluşturma ve doğrulama hizmeti**<br/> Müşteriler, kendi çıkarımı hizmetiyle ilişkili işlemin düzgün bir şekilde boyutlandırmak için zorluk sık şikayet. İle hizmeti profil oluşturmasını modelimizi örnek girişler müşteri sağlayabilir ve biz arasında farklı 16 CPU profil / dağıtımı için en iyi belirlemek için bellek yapılandırmaları boyutlandırma.

+ **Kendi temel görüntü için çıkarım Getir**<br/> Başka bir ortak şikayet deneme çıkarımı yeniden paylaşım bağımlılıkları taşıma zorluk oluştu. Yeni temel görüntü özelliği paylaşımı bizim artık, deneme temel görüntüleri, bağımlılıklarını ve çıkarım için tüm yeniden kullanabilirsiniz. Bu dağıtımları hızlandırmak ve boşluk iç için dış döngü azaltmak gerekir.

+ **Swagger şema oluşturma deneyimi İyileştirildi**<br/> Bizim önceki swagger oluşturma yöntemi, hatalara açık ve otomatik hale getirmek mümkün olmayan bir hata oluştu. Dekoratörler aracılığıyla herhangi bir Python işlevi swagger şemaları oluşturmak, yeni bir satır içi yolunu sunuyoruz. Biz açık bu kodu kaynaklı ve bizim şema oluşturma protokolü Azure ML platformuna eşleşmiş değil.

+ **Genel kullanıma (GA) Azure ML CLI aracı**<br/> Modelleri, artık tek bir CLI komutu ile dağıtılabilir. Genel Müşteri geri bildirimi, yapılandırdığımıza hiç bir Jupyter not defteri ML modelinden dağıtır. [ **CLI referans belgelerini** ](https://aka.ms/azmlcli) güncelleştirildi.


## <a name="2019-04-22"></a>2019-04-22

Python v1.0.30 yayımlanan için Azure Machine Learning SDK.

[ `PipelineEndpoint` ](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline_endpoint.pipelineendpoint?view=azure-ml-py) Olan tanıtmak aynı uç noktayı korurken yeni bir sürümü yayımlanmış bir işlem hattı eklemek için.

## <a name="2019-04-17"></a>2019-04-17

### <a name="azure-machine-learning-data-prep-sdk-v112"></a>Azure Machine Learning veri hazırlama SDK v1.1.2

Not: Veri hazırlığı Python SDK'sını yükleme artık `numpy` ve `pandas` paketleri. Bkz: [yükleme yönergeleri güncelleştirildi](https://aka.ms/aml-data-prep-installation).

+ **Yeni Özellikler**
  + Pivot dönüştürme artık kullanabilirsiniz.
    + Nasıl yapılır kılavuzunda: [Pivot not defteri](https://aka.ms/aml-data-prep-pivot-nb)
  + Yerel işlevlerde artık normal ifadeler kullanabilirsiniz.
    + Örnekler:
      + `dflow.filter(dprep.RegEx('pattern').is_match(dflow['column_name']))`
      + `dflow.assert_value('column_name', dprep.RegEx('pattern').is_match(dprep.value))`
  + Artık `to_upper`  ve `to_lower`  ifade dilindeki işlevleri.
  + Artık, bir veri profilinde her bir sütunun benzersiz değerlerin sayısını da görebilirsiniz.
  + Bazı yaygın olarak kullanılan okuyucu adımları için şimdi de geçirebilirsiniz `infer_column_types` bağımsız değişken. Bu ayarlanırsa `True`, veri hazırlığı, algılamak ve sütun türleri otomatik olarak dönüştürmek deneyecek.
    + `inference_arguments` artık kullanılmıyor.
  + Artık çağırabilirsiniz `Dataflow.shape`.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + `keep_columns` artık ek isteğe bağlı bağımsız değişken kabul eden `validate_column_exists`, varsa denetler sonucunu `keep_columns` hiçbir sütun içerir.
  + (Bu bir dosyadan okunan) okuyucu adımların tümünü artık ek isteğe bağlı bağımsız değişken kabul `verify_exists`.
  + Pandas dataframe okunuyor ve veri profilleri alma performansı İyileştirildi.
  + Başarısız olan tek bir dizin bir tek bir veri akışı adımından dilimleme olduğu bir hata düzeltildi.

## <a name="2019-04-15"></a>2019-04-15

### <a name="azure-portal"></a>Azure portal
  + Şimdi, var olan bir uzak işlem kümesi üzerinde varolan bir komut dosyasını yeniden gönderebilirsiniz. 
  + Artık yeni parametrelerle yayımlanan bir işlem hattı işlem hatları sekmesinde çalıştırabilirsiniz. 
  + Çalıştırma ayrıntılarını yeni bir anlık görüntü dosyası Görüntüleyici şimdi destekler. Belirli bir çalıştırma gönderildiğinde bir anlık görüntü dizininin görüntüleyebilirsiniz. Not Defteri çalıştırma başlatmak için gönderildi de indirebilirsiniz.
  + Şimdi, Azure portalından üst çalıştırmaları iptal edebilirsiniz.

## <a name="2019-04-08"></a>2019-04-08

### <a name="azure-machine-learning-sdk-for-python-v1023"></a>Azure Machine SDK için Python v1.0.23 Learning

+ **Yeni Özellikler**
  + Azure Machine Learning SDK'sı artık Python 3.7 destekler.
  + Azure Machine Learning DNN Estimators artık yerleşik çoklu sürüm desteği sağlar. Örneğin, `TensorFlow`  estimator artık kabul eden bir `framework_version` parametresi ve kullanıcıları '1.10' veya '1.12' sürümünü belirtebilirsiniz. Geçerli SDK sürümünüzü tarafından desteklenen sürümlerinin listesi için çağrı `get_supported_versions()` istenen framework sınıfında (örneğin, `TensorFlow.get_supported_versions()`).
  En son SDK'sı sürümü tarafından desteklenen sürümlerinin bir listesi için bkz. [DNN Estimator belgeleri](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn?view=azure-ml-py).

### <a name="azure-machine-learning-data-prep-sdk-v111"></a>Azure Machine Learning veri hazırlama SDK v1.1.1

+ **Yeni Özellikler**
  + Birden çok veri deposu/DataPath/DataReference kaynakları read_ * dönüşümler kullanarak okuyabilir.
  + Sütunları yeni bir sütun oluşturmak için aşağıdaki işlemleri gerçekleştirebilirsiniz: bölme, modül, güç, floor, uzunluğu.
  + Veri hazırlığı artık Azure ML tanılama Suite'in bir parçası ve tanılama bilgileri varsayılan olarak oturum açar.
    + Bu devre dışı bırakmak için bu ortam değişkenini true olarak ayarlayın: DISABLE_DPREP_LOGGER

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Yaygın olarak kullanılan sınıfları ve işlevleri için geliştirilmiş kod belgeleri.
  + Excel dosyaları okunamadı auto_read_file içinde bir hata düzeltildi.
  + Read_pandas_dataframe klasöründe üzerine seçeneği eklendi.
  + Geliştirilmiş performans dotnetcore2 bağımlılık yükleme ve Fedora 27/28 ve Ubuntu 1804 için destek eklendi.
  + Azure BLOB'ları okuma performansını geliştirdik.
  + Sütun türü algılama artık uzun türünde sütunlar destekler.
  + Python datetime nesneler yerine zaman damgası olarak bazı tarih değerlerini görüntüleyen hata düzeltildi.
  + Double yerine tamsayı olarak bazı türü sayıları görüntülendiği hata düzeltildi.

  
## <a name="2019-03-25"></a>2019-03-25

### <a name="azure-machine-learning-sdk-for-python-v1021"></a>Azure Machine SDK için Python v1.0.21 Learning

+ **Yeni Özellikler**
  + *Azureml.core.Run.create_children* yöntemi sağlayan birden çok alt düşük gecikme süreli oluşturulmasını tek bir çağrı ile çalışır.

### <a name="azure-machine-learning-data-prep-sdk-v110"></a>Azure Machine Learning veri hazırlama SDK v1.1.0

+ **Bozucu değişiklikler**
  + Veri hazırlık paketi kavramı, kullanım dışı bırakıldı ve artık desteklenmiyor. Tek bir pakette birden çok veri akışı kalıcı yerine, veri akışlarını ayrı ayrı kalıcı hale getirebilirsiniz.
    + Nasıl yapılır kılavuzunda: [Açma ve kaydetme veri akışlarını not defteri](https://aka.ms/aml-data-prep-open-save-dataflows-nb)

+ **Yeni Özellikler**
  + Veri hazırlığı, belirli bir anlam türüyle eşleşmesi ve buna göre bölme sütunlar artık tanıyabilirsiniz. Şu anda desteklenen STypes içerir: e-posta adresi, coğrafi koordinatlar vardır (enlem ve boylam), IPv4 ve IPv6 adresleri, ABD telefon numarası ve ABD posta kodu.
    + Nasıl yapılır kılavuzunda: [Anlam türleri not defteri](https://aka.ms/aml-data-prep-semantic-types-nb)
  + Veri hazırlığı, iki sayısal sütunları sonuç bir sütun oluşturmak için aşağıdaki işlemleri artık destekliyor: çıkarma, çarpma, bölme, modül ve.
  + Çağırabilirsiniz `verify_has_data()` çalıştırıldığında veri akışı kayıtları neden olup olmadığını denetlemek için bir veri akışı üzerinde.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Artık bir histogramda için sayısal bir sütun profilleri kullanmak için depo sayısını belirtebilirsiniz.
  + `read_pandas_dataframe` Dönüşüm artık DataFrame dize - olmasını gerektiriyor veya bayt - yazılan sütun adları.
  + Bir hata düzeltildi `fill_nulls` dönüştürme, burada değerleri doğru doldurulmamış sahipse sütunu eksik.

## <a name="2019-03-11"></a>2019-03-11

### <a name="azure-machine-learning-sdk-for-python-v1018"></a>Azure Machine SDK için Python v1.0.18 Learning

 + **Değişiklikleri**
   + Azureml tensorboard paket azureml contrib tensorboard değiştirir.
   + Bu sürümle birlikte, oluşturma sırasında yönetilen hesaplama kümenizde (amlcompute), bir kullanıcı hesabı ayarlayabilirsiniz. Bu, sağlama yapılandırmada bu özelliklerin geçirerek yapılabilir. Daha fazla bilgi bulabilirsiniz [SDK başvuru belgeleri](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py#provisioning-configuration-vm-size-----vm-priority--dedicated---min-nodes-0--max-nodes-none--idle-seconds-before-scaledown-none--admin-username-none--admin-user-password-none--admin-user-ssh-key-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--tags-none--description-none-).

### <a name="azure-machine-learning-data-prep-sdk-v1017"></a>Azure Machine Learning veri hazırlama SDK v1.0.17

+ **Yeni Özellikler**
  + Artık ifade dilini kullanarak bir sonuç sütun oluşturmak için iki sayısal sütunlar ekleyerek destekler.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Belgeler ve parametre random_split için İyileştirildi.
  
## <a name="2019-02-27"></a>2019-02-27

### <a name="azure-machine-learning-data-prep-sdk-v1016"></a>Azure Machine Learning veri hazırlama SDK v1.0.16

+ **Hata düzeltmesi**
  + Bir hizmet sorumlusu kaynaklandı kimlik doğrulama sorunu düzeltildi API değişikliği.

## <a name="2019-02-25"></a>2019-02-25

### <a name="azure-machine-learning-sdk-for-python-v1017"></a>Azure Machine SDK için Python v1.0.17 Learning

+ **Yeni Özellikler**

  + Azure Machine Learning, popüler DNN çerçevesini bağlayıcı artık birinci sınıf destek sağlar. Kullanarak [ `Chainer` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) sınıfı kullanıcılar kolayca eğitin ve bağlayıcı Modellerinizi dağıtın.
    + Bilgi nasıl [dağıtılmış eğitimi ChainerMN ile çalıştırın.](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/distributed-chainer/distributed-chainer.ipynb)
    + Bilgi edinmek için nasıl [HyperDrive kullanarak bağlayıcı ile hiper parametre ayarı çalıştırın](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-chainer/train-hyperparameter-tune-deploy-with-chainer.ipynb)
  + Azure Machine Learning işlem hatlarını özelliği tetikleyici işlem hattı çalıştırmasını veri deposu değişikliklerine göre eklendi. İşlem hattı [zamanlama not defteri](https://aka.ms/pl-schedule) bu özelliği göstermek için güncelleştirilir.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Azure Machine Learning işlem hatlarını destek source_directory_data_store özelliği (örneğin, bir blob depolama) istenen bir veri deposu olarak ayarlanması için üzerinde ekledik [RunConfigurations](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.runconfiguration?view=azure-ml-py) için sağlanan [ PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep?view=azure-ml-py). Varsayılan olarak, adımları çok sayıda eşzamanlı olarak çalıştırıldığında azaltma sorunları içine çalışabilir yedekleme veri deposu olarak Azure dosya depolama adımları kullanın.

### <a name="azure-portal"></a>Azure portal

+ **Yeni Özellikler**
  + Yeni bir Sürükle ve bırak Düzenleyicisi deneyimi raporlar için tablo. Kullanıcılar bir sütun tablonun önizlemesi nerede görüntülenecek tablo alanı sekme grubundan sürükleyebilirsiniz. Sütunları yeniden.
  + Yeni günlük dosyası Görüntüleyici
  + Çalıştırmaları, işlem, modelleri, görüntüleri ve dağıtımları etkinlikleri sekmesinden denemek için bağlantılar

### <a name="azure-machine-learning-data-prep-sdk-v1015"></a>Azure Machine Learning veri hazırlama SDK v1.0.15

+ **Yeni Özellikler**
  + Dosya yazma destekleyen bir veri akışı akışlardan artık veri hazırlama. Ayrıca yeni dosya adları oluşturmak için dosya akışı adları değiştirme olanağı sağlar.
    + Nasıl yapılır kılavuzunda: [Dosya akışları ile çalışmayı not defteri](https://aka.ms/aml-data-prep-file-stream-nb)

+ **Hata düzeltmeleri ve geliştirmeleri**
  + T-özetinin büyük veri kümeleri üzerinde performansı İyileştirildi.
  + Veri hazırlığı, artık bir DataPath okuma verileri destekler.
  + Sık erişimli bir kodlama artık Boole türü and sayısal sütunlarda çalışır.
  + Diğer çeşitli hata düzeltmeleri.

## <a name="2019-02-11"></a>2019-02-11

### <a name="azure-machine-learning-sdk-for-python-v1015"></a>Azure Machine SDK için Python v1.0.15 Learning

+ **Yeni Özellikler**
  + Azure Machine Learning işlem hatlarını AzureBatchStep eklendi ([not defteri](https://aka.ms/pl-azbatch)), HyperDriveStep (Not) ve zaman tabanlı işlevselliği zamanlama ([not defteri](https://aka.ms/pl-schedule)).
  +  PostgreSQL için Azure veritabanı ve Azure SQL Server ile çalışmak için DataTranferStep güncelleştirilir ([not defteri](https://aka.ms/pl-data-trans)).

+ **Değişiklikleri**
  + Kullanım dışı `PublishedPipeline.get_published_pipeline` sunulmasıyla `PublishedPipeline.get`.
  + Kullanım dışı `Schedule.get_schedule` sunulmasıyla `Schedule.get`.

### <a name="azure-machine-learning-data-prep-sdk-v1012"></a>Azure Machine Learning veri hazırlama SDK v1.0.12

+ **Yeni Özellikler**
  + Veri deposu kullanarak bir Azure SQL veritabanından okumayla veri hazırlığı destekler.
 
+ **Değişiklikleri**
  + Büyük veriler üzerinde belirli işlemlerin bellek performansı geliştirildi.
  + `read_pandas_dataframe()` artık ıntune hizmetini kullanabilmeleri `temp_folder` belirtilmelidir.
  + `name` Özelliği `ColumnProfile` olmuştur kullanım dışı - kullanın `column_name` yerine.

## <a name="2019-01-28"></a>2019-01-28

### <a name="azure-machine-learning-sdk-for-python-v1010"></a>Azure Machine SDK için Python v1.0.10 Learning

+ **Değişiklikleri**: 
  + Azure ML SDK'sı, bağımlılık olarak artık azure-cli paketleri vardır. Özellikle, azure CLI core ve azure CLI profili bağımlılıkları azureml çekirdekten kaldırıldı. Kullanıcının etkileyen değişiklikler şunlardır:
    + "Az login" gerçekleştirme ve ardından azureml-SDK'sını kullanarak, SDK'sı tarayıcı veya cihaz kod günlük içinde bir kez daha yapın. Bunu, "az login" oluşturulan herhangi bir kimlik bilgileri durumu kullanmaz.
    + "Az login" komutunu kullanarak gibi Azure CLI kimlik doğrulaması kullanan _azureml.core.authentication.AzureCliAuthentication_ sınıfı. Azure CLI kimlik doğrulaması için yapmak _pip, azure CLI yükleme_ azureml sdk yüklediğiniz Python ortamında.
    + "Az login Otomasyon için bir hizmet sorumlusunu kullanarak" yapıyorsanız kullanmanızı öneririz _azureml.core.authentication.ServicePrincipalAuthentication_ azureml sdk, azure CLI tarafından oluşturulan kimlik bilgilerini durumu kullanmaz gibi sınıf. 

+ **Hata düzeltmeleri**: Bu sürüm, çoğunlukla küçük hata düzeltmeleri içerir

### <a name="azure-machine-learning-data-prep-sdk-v108"></a>Azure Machine Learning veri hazırlama SDK v1.0.8

+ **Hata düzeltmeleri**
  + Veri profilleri alma performansı geliştirildi.
  + Hata Raporlama ile ilgili küçük hata düzeltildi.
  
### <a name="azure-portal-new-features"></a>Azure portalı: yeni özellikler
+ Raporlar için yeni bir Sürükle ve bırak grafik deneyimi. Kullanıcılar bir sütun veya öznitelik sistem bir veri türüne göre kullanıcı için uygun grafik türü otomatik olarak nerede seçip grafik alanı için sekme grubundan sürükleyebilirsiniz. Kullanıcılar, diğer uygulanabilir türleri için grafik türünü değiştirmek veya ek öznitelikler ekleyin.

    Grafik türleri desteklenir:
    - Çizgi grafik
    - Çubuk grafik
    - Yığılmış çubuk grafik
    - Kutu Çizimi
    - Dağılım
    - Kabarcık çizimi
+ Portal şimdi raporlar denemeleri için dinamik olarak oluşturur. Bir kullanıcı için bir deneme çalıştırma gönderdiğinde, günlüğe kaydedilen Ölçümler ve grafikler karşılaştırma farklı çalıştırmaları arasında izin verecek şekilde otomatik olarak bir rapor oluşturulur. 

## <a name="2019-01-14"></a>2019-01-14

### <a name="azure-machine-learning-sdk-for-python-v108"></a>Azure Machine SDK için Python v1.0.8 Learning

+ **Hata düzeltmeleri**: Bu sürüm, çoğunlukla küçük hata düzeltmeleri içerir

### <a name="azure-machine-learning-data-prep-sdk-v107"></a>Azure Machine Learning veri hazırlama SDK v1.0.7

+ **Yeni Özellikler**
  + Veri deposu geliştirmeleri (belirtilmiştir [Yardım How-to veri deposu-Kılavuzu](https://aka.ms/aml-data-prep-datastore-nb))
    + Azure dosya paylaşımı ve ADLS veri depoları içinde ölçek büyütme yazma ve okuma özelliği eklendi.
    + Veri depoları kullanırken, veri hazırlığı artık bir hizmet sorumlusu kimlik doğrulaması yerine etkileşimli kimlik doğrulaması kullanarak destekler.
    + Wasb ve wasbs URL'ler için destek eklendi.

## <a name="2019-01-09"></a>2019-01-09

### <a name="azure-machine-learning-data-prep-sdk-v106"></a>Azure Machine Learning veri hazırlama SDK v1.0.6

+ **Hata düzeltmeleri**
  + Spark üzerinde okunabilir ortak Azure Blob kapsayıcıları okumaya ile ilgili hata düzeltildi

## <a name="2018-12-20"></a>2018-12-20 

### <a name="azure-machine-learning-sdk-for-python-v106"></a>Azure Machine SDK için Python v1.0.6 Learning
+ **Hata düzeltmeleri**: Bu sürüm, çoğunlukla küçük hata düzeltmeleri içerir

### <a name="azure-machine-learning-data-prep-sdk-v104"></a>Azure Machine Learning veri hazırlama SDK v1.0.4

+ **Yeni Özellikler**
  + `to_bool` işlevi, hata değerlerini dönüştürülecek eşleşmeyen değerlere artık izin verir. Yeni varsayılan uyuşmazlığı davranışı budur `to_bool` ve `set_column_types`, önceki davranışı False olarak eşleşmeyen değerlerini dönüştürmek için expectedhash.
  + Çağrılırken `to_pandas_dataframe`, sayısal sütunlardaki değerleri null/eksik NaN yorumlamak için yeni bir seçenek yoktur.
  + Bazı ifadelerin türü tutarlılığı sağlamak ve erken başarısızlık için dönüş türü denetlenecek özelliği eklendi.
  + Artık çağırabilirsiniz `parse_json` bir sütundaki değerleri JSON nesnesi olarak ayrıştırılamadı ve bunları birden çok sütuna genişletin.

+ **Hata düzeltmeleri**
  + Kilitlenen bir hata düzeltildi `set_column_types` Python 3.5.2 içinde.
  + Veri deposuna AML görüntüyü kullanarak bağlanırken kilitlenen bir hata düzeltildi.

+ **Güncelleştirmeler**
  * [Örnek Not Defterleri](https://aka.ms/aml-data-prep-notebooks) başlatılan öğreticiler, örnek olay incelemeleri ve nasıl yapılır kılavuzlarından alma.

## <a name="2018-12-04-general-availability"></a>2018-12-04: Genel Erişilebilirlik

Azure Machine Learning hizmeti genel kullanıma sunulmuştur.

### <a name="azure-machine-learning-compute"></a>Azure Machine Learning işlem
Bu sürümle birlikte, yeni bir yönetilen bilgi işlem deneyimi aracılığıyla duyuruyoruz [Azure Machine Learning işlem](how-to-set-up-training-targets.md#amlcompute). Bu işlem hedef Azure Machine Learning için Azure Batch AI işlem değiştirir. 

Bu işlem hedef:
+ Eğitim ve batch çıkarımı/Puanlama modeli için kullanılır
+ Tek - için çok - node işlem
+ Küme yönetimi yapar ve proje kullanıcı için planlama
+ Varsayılan olarak Daralttığında
+ Hem CPU hem de GPU kaynakları için destek 
+ Düşük öncelikli VM'ler daha az maliyet için kullanılmasına izin verir

Azure Machine Learning işlem, Azure portal veya CLI kullanarak Python'da oluşturulabilir. Çalışma alanınızın bölgesinde oluşturulmalıdır ve herhangi bir çalışma alanına bağlı olamaz. Bu işlem hedef çalıştırmak için bir Docker kapsayıcısı kullanır ve aynı ortamdaki tüm düğümleri arasında çoğaltılmasını bağımlılıklarınızı paketler.

> [!Warning]
> Azure Machine Learning işlem kullanmak için yeni bir çalışma alanı oluşturmanızı öneririz. Kullanıcılar Azure Machine Learning işlem mevcut bir çalışma alanından oluşturulmaya çalışılırken bir hata görebilirsiniz uzak bir fırsat yoktur. Mevcut işlem çalışma alanınızdaki etkilenmeyen çalışmaya devam etmesi gerekir.

### <a name="azure-machine-learning-sdk-for-python-v102"></a>Azure Machine SDK için Python v1.0.2 Learning
+ **Bozucu değişiklikler**
  + Bu sürümle birlikte, Azure Machine Learning ile VM oluşturmaya yönelik destek kaldırıyoruz. Veya uzak bir şirket içi sunucusu hala var olan bir buluta VM ekleyebilirsiniz. 
  + Ayrıca, tüm Azure Machine Learning işlem artık desteklenmelidir BatchAI, desteği kaldırıyoruz.

+ **Yeni**
  + Machine learning işlem hatları için:
    + [EstimatorStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.estimator_step.estimatorstep?view=azure-ml-py)
    + [HyperDriveStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.hyper_drive_step.hyperdrivestep?view=azure-ml-py)
    + [MpiStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.mpi_step.mpistep?view=azure-ml-py)


+ **Güncelleştirildi**
  + Machine learning işlem hatları için:
    + [DatabricksStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.databricks_step.databricksstep?view=azure-ml-py) artık runconfig kabul eder
    + [DataTransferStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py) için ve SQL veri kaynağı artık kopyalar
    + Zamanlama oluşturma ve güncelleştirme zamanlamaları yayımlanan işlem hatları çalıştırmak için SDK'sındaki işlevleri

<!--+ **Bugs fixed**-->

### <a name="azure-machine-learning-data-prep-sdk-v052"></a>Azure Machine Learning veri hazırlama SDK v0.5.2
+ **Bozucu değişiklikler** 
  * `SummaryFunction.N` yeniden adlandırıldı `SummaryFunction.Count`.
  
+ **Hata düzeltmeleri**
  * En son AML çalıştırma okuma ve veri depoları için uzaktan çalıştırmalar yazarken belirteci kullanın. Python'da AML çalıştırma belirteci güncelleştirilir, daha önce veri hazırlığı çalışma zamanı güncelleştirilmiş AML çalıştırma belirteci ile güncelleştirilmez.
  * NET ek hata iletileri
  * to_spark_dataframe() artık kilitlenme Spark kullandığında `Kryo` seri hale getirme
  * Inspector değer sayısı 1000'den fazla benzersiz değerler artık gösterebilirsiniz
  * Özgün veri akışı bir adı yoksa rastgele bir bölünmüş artık başarısız  

+ **Daha fazla bilgi**
  * [Azure Machine Learning Veri Hazırlama SDK'sı](https://aka.ms/data-prep-sdk)

### <a name="docs-and-notebooks"></a>Belgeleri ve Not Defterleri
+ ML işlem hatları
  + İşlem hatları, batch kapsamlar ve stil ile çalışmaya başlama için yeni ve güncelleştirilmiş not defterlerini örnekler aktarın: https://aka.ms/aml-pipeline-notebooks
  + Bilgi edinmek için nasıl [ilk işlem hattınızı oluşturun](how-to-create-your-first-pipeline.md)
  + Bilgi edinmek için nasıl [komut zincirlerini kullanarak batch Öngörüler çalıştırın](how-to-run-batch-predictions.md)
+ Azure Machine Learning işlem hedefi
  + [Örnek Not Defterleri](https://aka.ms/aml-notebooks) artık yeni yönetilen bir işlem kullanacak şekilde güncelleştirildi.
  + [Bu işlem hakkında bilgi edinin](how-to-set-up-training-targets.md#amlcompute)

### <a name="azure-portal-new-features"></a>Azure portalı: yeni özellikler
+ Oluşturma ve yönetme [Azure Machine Learning işlem](how-to-set-up-training-targets.md#amlcompute) portalında türleri.
+ Kota kullanımını izlemek ve [isteği kota](how-to-manage-quotas.md) Azure Machine Learning işlemi için.
+ Azure Machine Learning işlem küme durumunu gerçek zamanlı olarak görüntüleyin.
+ Azure Machine Learning işlem ve Azure Kubernetes hizmeti oluşturmak için sanal ağ desteği eklendi.
+ Yayımlanan hatlarınızı mevcut parametrelerle yeniden çalıştırın.
+ Yeni [machine learning grafikleri otomatik](how-to-track-experiments.md#auto) sınıflandırma modellerini (lift, Kazanç, ayarlama, özellik önem modeli explainability grafikle) ve regresyon modellerini (Kalanlar ve modeli ile özellik önem grafiği explainability). 
+ İşlem hatları Azure portalında görüntülenebilir




## <a name="2018-11-20"></a>2018-11-20

### <a name="azure-machine-learning-sdk-for-python-v0180"></a>Azure Machine SDK için Python v0.1.80 Learning

+ **Bozucu değişiklikler** 
  * *azureml.Train.widgets* ad alanı taşındığı *azureml.widgets*.
  * *azureml.core.compute.AmlCompute* aşağıdaki sınıflar - kullanımdan kaldırıldı *azureml.core.compute.BatchAICompute* ve *azureml.core.compute.DSVMCompute*. İkinci sınıfı sonraki sürümlerde kaldırılacak. AmlCompute sınıfı artık daha kolay bir tanıma sahip yalnızca bir vm_size ve max_nodes gerekir ve bir iş gönderildiğinde kümenize 0 max_nodes otomatik olarak ölçeklendirir. Bizim [örnek not defterleri](https://github.com/Azure/MachineLearningNotebooks/tree/master/training) bu bilgilerle güncelleştirildi ve kullanım örnekleri vermeniz gerekir. Bu basitleştirme ve çok daha heyecan verici özelliklerin bir sonraki sürümde gelir gibi umuyoruz!

### <a name="azure-machine-learning-data-prep-sdk-v051"></a>Azure Machine Learning veri hazırlama SDK v0.5.1 

Veri hazırlığı SDK'sı hakkında daha fazla bilgi edinmek [başvuru belgeleri](https://aka.ms/data-prep-sdk).
+ **Yeni Özellikler**
   * DataPrep paketleri çalıştırmak ve bir veri kümesi veya veri akışı veri profilini görüntülemek için yeni bir DataPrep CLI oluşturdunuz
   * Kullanılabilirliği iyileştirmek için tasarlanan SetColumnType API
   * Yeniden adlandırılan smart_read_file auto_read_file için
   * Şimdi veri profilinde sapması ve basıklık içerir
   * İle stratified örnekleme örnek oluşturabilirsiniz
   * CSV dosyaları içeren zip dosyaları okuyabilir
   * Rastgele bölünmüş row-wise kümeleriyle (örneğin,. test train kümeleri halinde) bölebilirsiniz.
   * Tüm sütun veri türlerini bir veri akışı veri profili çağırarak alabilir veya `.dtypes`
   * Satır sayısı bir veri akışı veri profili çağırarak alabilir veya `.row_count`

+ **Hata düzeltmeleri**
   * Uzun çift dönüştürme sabit 
   * Sabit sütun ekleyin, sonra onaylama 
   * Grupları nereden bazı durumlarda algılaması değil, FuzzyGrouping ile bir sorun düzeltildi
   * Birden çok sütunu sıralamayı cevaben sabit bir sıralama işlevi
   * Sabit ve/veya nasıl benzer ifadeler `pandas` bunları işler
   * Dbfs yolundan sabit okuma
   * Hata iletileri daha anlaşılır hale 
   * AML belirteci kullanarak uzak işlem hedefte okurken şimdi artık başarısız
   * Artık üzerinde Linux DSVM'sini artık başarısız
   * Dize olmayan değerler dize koşullarda şimdi artık çöküyor
   * Artık doğru şekilde veri akışı başarısız olursa, onaylama hatalarını ele alır
   * Artık Azure Databricks'te dbutils bağlı depolama konumları destekler

## <a name="2018-11-05"></a>2018-11-05

### <a name="azure-portal"></a>Azure portal 
Azure Machine Learning hizmeti için Azure portalı aşağıdaki güncelleştirmeler bulunur:
  * Yeni bir **işlem hatları** yayımlanan işlem hatları için sekmesinde.
  * İşlem hedefi olarak var olan bir HDInsight kümesine ekleme desteği eklendi.

### <a name="azure-machine-learning-sdk-for-python-v0174"></a>Azure Machine SDK için Python v0.1.74 Learning

+ **Bozucu değişiklikler** 
  * \* Workspace.compute_targets, veri depoları, denemeleri, resimler, modeller ve *webservices'a* yöntemleri yerine özellikler. Örneğin, *Workspace.compute_targets()* ile *Workspace.compute_targets*.
  * *Run.get_context* kullanımdan kaldırıldı *Run.get_submitted_run*. İkinci yöntem, sonraki sürümlerde kaldırılacak.
  * *PipelineData* sınıfı bir parametre yerine olarak datastore_name bir veri deposu nesnesi artık bekliyor. Benzer şekilde, *işlem hattı* default_datastore_name yerine default_datastore kabul eder.

+ **Yeni Özellikler**
  * Azure Machine Learning işlem hatlarını [örnek not defteri](https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline/pipeline-mpi-batch-prediction.ipynb) artık MPI adımları kullanır.
  * Jupyter not defterleri için RunDetails pencere, işlem hattının görselleştirilmesi göstermek için güncelleştirilir.

### <a name="azure-machine-learning-data-prep-sdk-v040"></a>Azure Machine Learning veri hazırlama SDK v0.4.0 
 
+ **Yeni Özellikler**
  * Veri profili türü sayısı eklendi 
  * Değer sayısı ve Histogram kullanıma sunuldu
  * Daha fazla yüzdebirliklerini veri profilinde
  * ORTANCA özetleme kullanılabilir
  * Python 3.7 artık desteklenmektedir
  * Veri depoları DataPrep paketi içeren bir veri akışı kaydettiğinizde, veri deposu bilgileri DataPrep paketinin bir parçası sürdürülecek
  * Veri deposu yazma artık desteklenmektedir 
        
+ **Hata düzeltildi**
  * 64-bit işaretsiz tamsayı taşma artık düzgün bir şekilde Linux üzerinde işlenir
  * Düz metin dosyalarında smart_read hatalı sabit metin etiketi
  * Dize sütun türü artık ölçümleri görünümünde gösterilir
  * Türü sayısı artık tek tek olanları yerine tek FieldType eşlenen ValueKinds gösterecek şekilde düzeltildi
  * Bir dize olarak yol sağlandığında Write_to_csv artık başarısız
  * Replace kullanırken, "boş bulma" bırakarak artık başarısız olur 

## <a name="2018-10-12"></a>2018-10-12

### <a name="azure-machine-learning-sdk-for-python-v0168"></a>Azure Machine SDK için Python v0.1.68 Learning

+ **Yeni Özellikler**
  * Yeni çalışma alanı oluştururken, çok kiracılı desteği.

+ **Düzeltilen hatalar**
  * Artık web hizmetini dağıtırken pynacl kitaplığı sürüm sabitlemek gerekir.

### <a name="azure-machine-learning-data-prep-sdk-v030"></a>Azure Machine Learning veri hazırlama SDK v0.3.0

+ **Yeni Özellikler**
  * Kullanıcıların yürütmek için bir Python dosyası yolunda geçirilecek yöntemi transform_partition_with_file(script_path) eklendi

## <a name="2018-10-01"></a>2018-10-01

### <a name="azure-machine-learning-sdk-for-python-v0165"></a>Azure Machine SDK için Python v0.1.65 Learning
[Sürüm 0.1.65](https://pypi.org/project/azureml-sdk/0.1.65) yeni özellikler, daha fazla belge, hata düzeltmeleri ve daha fazlasını içeren [örnek not defterleri](https://aka.ms/aml-notebooks).

Bkz: [bilinen sorunların listesi](resource-known-issues.md) bilinen hataların ve geçici çözümleri hakkında bilgi edinmek için.

+ **Bozucu değişiklikler**
  * Workspace.experiments, Workspace.models, Workspace.compute_targets, Workspace.images, Workspace.web_services dönüş sözlük, daha önce listesi döndürdü. Bkz: [azureml.core.Workspace](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py) API belgeleri.

  * Machine Learning otomatik normalleştirilmiş Ortalama kare hata birincil ölçümleri kaldırıldı.

+ **HyperDrive**
  * Bayes çeşitli HyperDrive hata düzeltmeleri, ölçümleri çağrıları için performans geliştirmeleri alın. 
  * Tensorflow 1.10 1.9 sürümüne yükseltme 
  * Docker görüntü iyileştirmesi hazırlıksız başlatma için. 
  * Bunlar dışında 0 hata kodu ile çıkmak bile işleri artık doğru durum rapor. 
  * Öznitelik doğrulamada SDK RunConfig. 
  * Çalıştırma HyperDrive nesnesi iptal normal çalışmaya benzer destekler: herhangi bir parametre geçirin gerek yoktur. 
  * Dağıtılmış çalıştırır ve HyperDrive çalıştırmalar için aşağı açılan değerleri durumunu korumak için pencere öğesi geliştirmeleri. 
  * Parametre sunucusu için sabit, TensorBoard ve diğer günlük dosyalarını destekler. 
  * Hizmet tarafı Intel(r) MPI desteği. 
  * Hata düzeltmesi, için parametre ayarlama için düzeltme çalışma BatchAI doğrulama sırasında dağıtılmış. 
  * Bağlam Yöneticisi artık birincil tanıtır. 

+ **Azure portal deneyimi**
  * Çalıştırma ayrıntıları log_table() ve log_row() desteklenir. 
  * Otomatik olarak 1, 2 veya 3 sayısal sütunlara ve isteğe bağlı bir kategorik sütun tabloları ve satırları için grafikler oluşturun.

+ **Otomatik makine öğrenimi**
  * Geliştirilmiş hata işleme ve belgeler 
  * Çalıştırma özelliği alımı performans sorunlarını düzelttik. 
  * Sabit çalışma sorunu devam edin. 
  * Ensembling yineleme sorunlar düzeltildi.
  * MAC OS sabit eğitim asılı hata.
  * Aşağı örnekleme makrosu ortalama çekme isteği/ROC eğrisi özel doğrulama senaryosunda.
  * Ek dizin mantıksal kaldırıldı.
  * Filtre get_output API ' kaldırıldı.

+ **İşlem hatları**
  * Bir yöntem bir işlem hattı yürütme ilk çalıştırma gerek kalmadan doğrudan yayımlamak için Pipeline.publish() eklendi.   
  * Yayımlanan bir ardışık düzen tarafından oluşturulan işlem hattı çalıştırmaları getirilecek PipelineRun.get_pipeline_runs() bir yöntem eklendi.

+ **Project Brainwave**
  * Yeni AI modelleri FPGA üzerinde kullanılabilir güncelleştirilmiş destek.

### <a name="azure-machine-learning-data-prep-sdk-v020"></a>Azure Machine Learning veri hazırlama SDK v0.2.0
[Sürüm 0.2.0](https://pypi.org/project/azureml-dataprep/0.2.0/) özellikler ve hata düzeltmeleri içerir:

+ **Yeni Özellikler**
  * Bir sık erişimli kodlaması için destek
  * Quantile dönüştürme için destek
   
+ **Hata düzeltildi:**
  * Herhangi bir hortum sürümü ile çalışır hortum sürümünüzü düşürme gerekmez.
  * Değer için tüm değerleri, yalnızca ilk üç sayar.

## <a name="2018-09-public-preview-refresh"></a>2018-09 (genel Önizleme yenileme)

Yeni bir Azure Machine Learning sürümü yenilendi: Bu sürüm hakkında daha fazlasını okuyun: https://azure.microsoft.com/blog/what-s-new-in-azure-machine-learning-service/


## <a name="next-steps"></a>Sonraki adımlar

Genel bakışı okuyun [Azure Machine Learning hizmeti](../service/overview-what-is-azure-ml.md).
