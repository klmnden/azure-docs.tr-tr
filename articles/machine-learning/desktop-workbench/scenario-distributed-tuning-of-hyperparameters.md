---
title: Azure Machine Learning Workbench'i kullanarak Hiperparametreleri, ayarlama dağıtılmış | Microsoft Docs
description: Bu senaryo Azure Machine Learning Workbench'i kullanarak hiperparametreleri dağıtılmış ayarlama yapmak nasıl gösterir
services: machine-learning
author: pechyony
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.author: dmpechyo
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.date: 09/20/2017
ROBOTS: NOINDEX
ms.openlocfilehash: f74889cdf727bc132723d16df295849769001ce9
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46951976"
---
# <a name="distributed-tuning-of-hyperparameters-using-azure-machine-learning-workbench"></a>Dağıtılmış bir şekilde ayarlama hiperparametreleri Azure Machine Learning Workbench'i kullanma

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 



Bu senaryoyu ayarlama hiperparametreleri scikit uygulayan machine learning algoritmalarının ölçeklendirmek için Azure Machine Learning Workbench'i kullanma işlemini gösterir-API öğrenin. Yapılandırma ve ayarlama hiperparametreleri için bir yürütme arka uç olarak uzak bir Docker kapsayıcısı ve Spark kümesi kullanma göstereceğiz.

## <a name="link-of-the-gallery-github-repository"></a>Galeri GitHub deposunun bağlantısı
Ortak GitHub deposu bağlantısı aşağıda verilmiştir: 

[https://github.com/Azure/MachineLearningSamples-DistributedHyperParameterTuning](https://github.com/Azure/MachineLearningSamples-DistributedHyperParameterTuning)

## <a name="use-case-overview"></a>Kullanım örneği genel bakış

Birçok makine öğrenimi algoritmaları hiperparametreleri adlı bir veya daha fazla düğmelerini vardır. Kullanıcı tarafından belirtilen ölçüm göre ölçülen gelecekteki veriler üzerinde performanslarını en iyi duruma getirme algoritmaları ayarlama bu düğmelerini izin ver (örneğin, doğruluk AUC, RMSE). Veri uzmanı, bir model eğitim verilerini ve gelecekteki test verileri görme hakkındaki önce oluştururken hiperparametreleri değerini vermesi gerekir. Model bilinmeyen test verilerinde iyi bir performans sahip olacak şekilde nasıl bağlı olarak bilinen bir eğitim veri can hiperparametreleri değerlerini ayarlarız? 
    
Popüler teknik ayarlama hiperparametreleri, bir *kılavuz arama* birlikte *çapraz doğrulama*. Çapraz doğrulama, bir eğitim kümesinde eğitilmiş bir modeli test küme üzerinde ne kadar iyi tahmin değerlendirir bir tekniktir. Bu tekniği kullanarak, biz öncelikle veri kümesi K hatları bölebilir ve ardından algoritması K zamanları hepsini bir biçimde eğitin. Tüm bunu ancak bir kat sayısı "tutulan genişletme Katlama" denir. Biz K modelleri ölçümlerini ortalama değerini K tutulan genişletme eşlemedeki işlem. Olarak adlandırılan bu ortalama değer *arası doğrulanmış performans tahmin*, K modelleri oluştururken kullandığınız hiperparametreleri değerlerine bağlıdır. Ayarlama hiperparametreleri, biz çapraz doğrulama performansını en iyi duruma olanları tahmin bulmak için aday hiper parametre değerlerinin alanı arayın. Kılavuz arama yaygın bir arama tekniktir. Kılavuz araması'nda, çapraz ürün aday değerlerinin tek tek hiperparametreleri kümelerinin birden çok hiperparametreleri aday değerlerini alandır. 

Çapraz doğrulama kullanarak kılavuz arama, zaman alıcı olabilir. Beş hiperparametreleri her beş aday değerlerle yönelik bir algoritmaya sahiptir, bin = 5 hatları kullanırız. Biz, ardından bir kılavuz aramada tamamlayın 5 eğitim<sup>6</sup>15625 modelleri =. Neyse ki, kılavuz arama çapraz doğrulama kullanmaktır utandırıcı derecede paralel bir yordam ve bu modeller paralel olarak düşünürler.

## <a name="prerequisites"></a>Önkoşullar

* Bir [Azure hesabı](https://azure.microsoft.com/free/) (ücretsiz denemeler kullanılabilir).
* Yüklü bir kopyasını [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md) aşağıdaki [yükleme ve oluşturma Hızlı Başlangıç](quickstart-installation.md) Workbench'i yükleme ve hesapları oluşturun.
* Bu senaryo, Azure ML Workbench Windows 10 veya Mac OS x üzerinde yerel olarak yüklü Docker altyapısıyla çalıştığını varsayar. 
* Senaryo ile uzak bir Docker kapsayıcısında çalıştırmak için Ubuntu veri bilimi sanal makinesi (DSVM) izleyerek sağlama [yönergeleri](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-provision-vm). En az 8 çekirdek ve bellek, 28 Gb ile bir sanal makine kullanmanızı öneririz. Sanal makine örneklerini D4 bu kapasiteye sahip. 
* Bu senaryo bir Spark kümesi ile çalıştırmak için sağlama Spark HDInsight küme izleyerek [yönergeleri](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-provision-linux-clusters). Hem üst hem de çalışan düğümleriniz aşağıdaki yapılandırma ile küme öneririz:
    - dört alt düğüm
    - sekiz Çekirdeği
    - 28 Gb bellek  
      
  Sanal makine örneklerini D4 bu kapasiteye sahip. 

     **Sorun giderme**: Azure aboneliğiniz kota, kullanılabilen çekirdek sayısına sahip olabilir. Azure portalında çekirdek kotasını toplam sayısı ile küme oluşturma izin vermez. Kota bulmak için Azure portalında abonelikleri bölümüne bir kümeyi dağıtmak için kullanılan aboneliğe tıklayın ve ardından Git **kullanım ve kotalar**. Genellikle kotalar Azure bölgesi tanımlanır ve yeterli sayıda serbest çekirdeğe sahip olduğu bir bölgede Spark kümesini dağıtmayı tercih edebilirsiniz. 

* Veri kümesi depolamak için kullanılan bir Azure depolama hesabı oluşturun. İzleyin [yönergeleri](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account) bir depolama hesabı oluşturmak için.

## <a name="data-description"></a>Veri açıklaması

Kullandığımız [TalkingData dataset](https://www.kaggle.com/c/talkingdata-mobile-user-demographics/data). Bu veri kümesi, cep telefonları uygulamalarından olayları vardır. Cep telefonu kullanıcı türünü telefon ve kullanıcı yakın zamanda oluşturulan olayları verilen cinsiyet ve yaş kategorisi tahmin olmaktır.  

## <a name="scenario-structure"></a>Senaryo yapısı
Bu senaryo, GitHub deposunda birden fazla klasör vardır. Kod ve yapılandırma dosyalar, **kod** tüm belgeler klasöründe konusu **Docs** klasör ve tüm görüntüleri **görüntüleri** klasör. Kök klasör bu senaryoya kısa bir özetini içeren Benioku dosyası içerir.

### <a name="getting-started"></a>Başlarken
Azure Machine Learning Workbench'i çalıştırma ve "Dağıtılmış ayarlama, Hiperparametreleri" şablondan bir proje oluşturmak için Azure Machine Learning Workbench simgesine tıklayın. Yeni bir proje oluşturma konusunda ayrıntılı yönergeleri bulabilirsiniz [yükleme ve oluşturma Hızlı Başlangıç](quickstart-installation.md).   

### <a name="configuration-of-execution-environments"></a>Yürütme ortamları yapılandırma
Uzak bir Docker kapsayıcısı ve bir Spark kümesi kodumuz çalıştırma göstereceğiz. Her iki ortamlar için ortak olan ayarları açıklaması ile başlayın. 

Kullandığımız [scikit-bilgi](https://anaconda.org/conda-forge/scikit-learn), [xgboost](https://anaconda.org/conda-forge/xgboost), ve [azure depolama](https://pypi.python.org/pypi/azure-storage) Azure Machine Learning Workbench varsayılan Docker kapsayıcısında sağlanmamış olan paketler. Azure depolama paketinden yüklenmesini gerektirir [şifreleme](https://pypi.python.org/pypi/cryptography) ve [azure](https://pypi.python.org/pypi/azure) paketleri. Docker kapsayıcısı ve Spark kümesi düğümleri bu paketleri yüklemek için biz conda_dependencies.yml dosyasını değiştirin:

    name: project_environment
    channels:
      - conda-forge
    dependencies:
      - python=3.5.2
      - scikit-learn
      - xgboost
      - pip:
        - cryptography
        - azure
        - azure-storage

Değiştirilen conda\_dependencies.yml dosya öğreticinin aml_config dizininde depolanır. 

Sonraki adımlarda size yürütme ortamı Azure hesabınıza bağlanın. AML Workbench sol üst köşesinden dosya menüsüne tıklayın. Ve "komut istemini Aç" ı seçin. Ardından, CLI'de çalıştırın

    az login

Bir ileti alırsınız

    To sign in, use a web browser to open the page https://aka.ms/devicelogin and enter the code <code> to authenticate.

Gidin. Bu web sayfası için kodu girin ve Azure hesabınızda oturum açın. Bu adımdan sonra CLI'da çalıştırın

    az account list -o table

ve AML Workbench çalışma alanı hesabınızın Azure abonelik kimliği bulunamadı. Son olarak, CLI'da çalıştırın

    az account set -s <subscription ID>

Azure aboneliğinize bağlantıyı tamamlamak için.

Uzak docker ve Spark küme yapılandırmasını tamamlamak nasıl göstereceğiz sonraki iki bölümden.

#### <a name="configuration-of-remote-docker-container"></a>Uzak Docker kapsayıcısının yapılandırma

 Uzak bir Docker kapsayıcısı ayarlamak için CLI'da çalıştırın.

    az ml computetarget attach remotedocker --name dsvm --address <IP address> --username <username> --password <password> 

IP adresi, kullanıcı adı ve parola DSVM ile. DSVM IP adresini Azure portalında DSVM sayfanızın genel bakış bölümünde bulunabilir:

![VM IP'si](media/scenario-distributed-tuning-of-hyperparameters/vm_ip.png)

#### <a name="configuration-of-spark-cluster"></a>Spark kümesi yapılandırması

Spark ortamı ayarlamak için CLI'da çalıştırın.

    az ml computetarget attach cluster --name spark --address <cluster name>-ssh.azurehdinsight.net  --username <username> --password <password> 

Küme, küme SSH kullanıcı adı ve parola adı ile. SSH kullanıcı adı, varsayılan değer `sshuser`sürece küme hazırlama sırasında değiştirildi. Küme adı, Azure portalında küme sayfanızın özellikleri bölümünde bulunabilir:

![Küme adı](media/scenario-distributed-tuning-of-hyperparameters/cluster_name.png)

Spark hiperparametreleri dağıtılmış ayarlama yürütme ortamı olarak sağlamak için spark sklearn paketini kullanıyoruz. Biz, Spark yürütme ortamı kullanıldığında bu paketi yüklemek için spark_dependencies.yml dosyası değiştirildi:

    configuration: 
      #"spark.driver.cores": "8"
      #"spark.driver.memory": "5200m"
      #"spark.executor.instances": "128"
      #"spark.executor.memory": "5200m"  
      #"spark.executor.cores": "2"
  
    repositories:
      - "https://mmlspark.azureedge.net/maven"
      - "https://spark-packages.org/packages"
    packages:
      - group: "com.microsoft.ml.spark"
        artifact: "mmlspark_2.11"
        version: "0.7.91"
      - group: "databricks"
        artifact: "spark-sklearn"
        version: "0.2.0"

Değiştirilen spark\_dependencies.yml dosya öğreticinin aml_config dizininde depolanır. 

### <a name="data-ingestion"></a>Veri alımı
Bu senaryoda kod, veriler Azure blob depolama alanında depolanır varsayar. Başlangıçta verileri Kaggle sitesinden bilgisayarınıza indirin ve blob depolama alanına yükleme göstereceğiz. Ardından verileri blob depolama alanından okuma göstereceğiz. 

Kaggle verileri indirmek için Git [veri kümesi sayfası](https://www.kaggle.com/c/talkingdata-mobile-user-demographics/data) ve indir düğmesine tıklayın. Kaggle'için oturum açma istenir. Açtıktan sonra veri kümesi sayfasına yönlendirilirsiniz. Daha sonra bunları seçerek ve indir düğmesine tıklayarak sol sütunda ilk yedi dosyaları indirin. İndirilen dosyaların toplam boyutu 289 MB'dir. Bu dosya, BLOB depolamaya yüklemek için depolama hesabınızdaki blob depolama kapsayıcısı 'dataset' oluşturun. Azure depolama hesabı, Blobları ve ardından tıklatarak + kapsayıcı sayfasına giderek bunu yapabilirsiniz. 'Dataset' bir ad girin ve Tamam'a tıklayın. Aşağıdaki ekran görüntüleri, şu adımları göstermektedir:

![Açık blob](media/scenario-distributed-tuning-of-hyperparameters/open_blob.png)
![açık kapsayıcı](media/scenario-distributed-tuning-of-hyperparameters/open_container.png)

Bundan sonra bu veri kümesi kapsayıcı listeden seçin ve karşıya yükleme düğmesini tıklatın. Azure portalı, birden çok dosyayı eşzamanlı olarak karşıya sağlar. Bölümünde "blob karşıya yükle" Klasör düğmesini tıklatın, veri kümesinden tüm dosyaları seçin, Aç'a tıklayın ve ardından Yükle'yi tıklatın. Aşağıdaki ekran görüntüsünde, bu adımları gösterir:

![Blobu karşıya yükle](media/scenario-distributed-tuning-of-hyperparameters/upload_blob.png) 

Dosyaları karşıya yükleme, Internet bağlantınıza bağlı olarak birkaç dakika sürer. 

Bizim kodda kullandığımız [Azure depolama SDK'sı](https://docs.microsoft.com/python/azure/) veri kümesi, geçerli yürütme ortamı için blob Depolama'yı indirmek için. Karşıdan yükleme gerçekleştirilir\_load_data.py dosyasından data() işlevi. Bu kodu kullanmak için < ACCOUNT_NAME > değiştirmeniz gerekir ve < ACCOUNT_KEY > adını ve veri kümesi barındıran depolama hesabınızın birincil anahtarını ile. Azure depolama hesabınızın sayfasındaki sol üst köşesindeki hesap adında görebilirsiniz. Hesabı almak için Azure sayfası depolama erişim anahtarlarını anahtar seçeneğini belirleyin (veri alımı bölümündeki ilk ekran görüntüsüne bakın) hesabı ve uzun bir dize anahtar sütunun ilk satırına kopyalayın:
 
![Erişim anahtarı](media/scenario-distributed-tuning-of-hyperparameters/access_key.png)

Aşağıdaki kodu load_data() işlevden tek bir dosya yükler:

    from azure.storage.blob import BlockBlobService

    # Define storage parameters 
    ACCOUNT_NAME = "<ACCOUNT_NAME>"
    ACCOUNT_KEY = "<ACCOUNT_KEY>"
    CONTAINER_NAME = "dataset"

    # Define blob service     
    my_service = BlockBlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)

    # Load blob
    my_service.get_blob_to_path(CONTAINER_NAME, 'app_events.csv.zip', 'app_events.csv.zip')

Load_data.py dosyasını el ile çalıştırın gerekmez dikkat edin. Diğer dosyalardan daha sonra çağrılır.

### <a name="feature-engineering"></a>Özellik mühendisliği
Tüm özellikleri bilgi işlem kodunu özelliktir\_engineering.py dosya. Feature_engineering.py dosyasını el ile çalıştırmak gerekmez. Daha sonra diğer dosyalardan çağrılır.

Birden çok özellik kümeleri oluşturacağız:
* Marka ve model cep telefonu, sık erişimli bir kodlama (bir\_sık erişimli\_brand_model işlevi)
* Her iş günü içinde kullanıcı tarafından oluşturulan olayları kesiri (hafta içi\_hour_features işlevi)
* Her bir saat içinde kullanıcı tarafından oluşturulan olayları kesiri (hafta içi\_hour_features işlevi)
* Her haftanın günü ve saat bileşimi kullanıcı tarafından oluşturulan olayları kesiri (hafta içi\_hour_features işlevi)
* Her bir uygulamada kullanıcı tarafından oluşturulan olayları kesiri (bir\_sık erişimli\_app_labels işlevi)
* Her uygulama etiketi kullanıcı tarafından oluşturulan olayları kesiri (bir\_sık erişimli\_app_labels işlevi)
* Her uygulama kategorisi kullanıcı tarafından oluşturulan olayları kesiri (metin\_category_features işlevi)
* Gösterge özellikleri için kullanılan uygulama kategorileri için oluşturulan olaylar (bir\_hot_category işlevi)

Bu özellikler tarafından Kaggle çekirdek ilham [uygulama ve etiketleri doğrusal bir model](https://www.kaggle.com/dvasyukova/a-linear-model-on-apps-and-labels).

Bu özelliklerin hesaplama önemli miktarda bellek gerektirir. Başlangıçta özellikleri 16 GB RAM ile yerel ortamda işlem çalıştık. Size ilk dört özellik kümeleri hesaplayabilir ancak beşinci özellik kümesini hesaplanırken 'yetersiz bellek' hata alındı. İlk dört özellik kümeleri hesaplama singleVMsmall.py dosyasındadır ve yerel ortamda çalıştırarak yürütülmek üzere 

     az ml experiment submit -c local .\singleVMsmall.py   

CLI penceresinde.

Yerel ortam tüm özellik kümeleri bilgi işlem için çok küçük olduğundan, biz daha büyük bellek uzak değerini DSVM Örneğinize geçin. DSVM içinde yürütme AML Workbench tarafından yönetilen bir Docker kapsayıcısı içinde gerçekleştirilir. Bu DSVM kullanarak işlem tüm özellikleri, modelleri eğitme ve ayarlama hiperparametreleri (sonraki bölüme bakın) yükümlü. Tamamlama özelliği hesaplama ve kod modelleme singleVM.py dosyası vardır. Sonraki bölümde uzak DSVM singleVM.py çalıştırma göstereceğiz. 

### <a name="tuning-hyperparameters-using-remote-dsvm"></a>Ayarlama hiperparametreleri uzak DSVM'sini kullanma
Kullandığımız [xgboost](https://anaconda.org/conda-forge/xgboost) gradyan ağaç artırma, uygulama [1]. Biz de [scikit-bilgi](http://scikit-learn.org/) xgboost hiperparametreleri ayarlamak için paket. Xgboost scikit parçası olmasa da-paketi bilgi scikit uygulayan-API öğrenin ve bu nedenle hiper parametre scikit işlevlerini ayarlama ile birlikte kullanılabilir-öğrenin. 

Xgboost sahip açıklanan sekiz hiperparametreleri [burada](https://github.com/dmlc/xgboost/blob/master/doc/parameter.md):
* n_estimators
* max_depth
* reg_alpha
* reg_lambda
* colsample\_by_tree
* learning_rate
* colsample\_by_level
* subsample
* Hedefi  
 
Başlangıçta biz uzak DSVM'sini kullanma ve aday değerlerinin küçük bir grid'den hiperparametreleri ayarlayın:

    tuned_parameters = [{'n_estimators': [300,400], 'max_depth': [3,4], 'objective': ['multi:softprob'], 'reg_alpha': [1], 'reg_lambda': [1], 'colsample_bytree': [1],'learning_rate': [0.1], 'colsample_bylevel': [0.1,], 'subsample': [0.5]}]  

Bu kılavuz, dört hiperparametreleri değerleri birleşimlerini sahiptir. 5-fold çapraz doğrulama, kullandığımız 4 x 5 = 20 kaynaklanan xgboost çalıştırır. Model performansını ölçmek için negatif günlük kaybı ölçüm kullanırız. Aşağıdaki kod, çapraz doğrulanmış negatif günlük kaybı en üst düzeye hiperparametreleri kılavuzdan değerlerini bulur. Kod Ayrıca, tam Eğitim kümesi son modeli eğitmek için bu değerleri kullanır:

    clf = XGBClassifier(seed=0)
    metric = 'neg_log_loss'
    
    clf_cv = GridSearchCV(clf, tuned_parameters, scoring=metric, cv=5, n_jobs=8)
    model = clf_cv.fit(X_train,y_train)

Model oluşturduktan sonra biz hiper parametre ayarı sonuçlarını kaydedin. AML API Workbench oturum hiperparametreleri en iyi değerlerini ve ilgili arası doğrulanmış tahmin negatif günlük kaybı kaydetmek için kullanırız:

    from azureml.logging import get_azureml_logger

    # initialize logger
    run_logger = get_azureml_logger()

    ...

    run_logger.log(metric, float(clf_cv.best_score_))

    for key in clf_cv.best_params_.keys():
        run_logger.log(key, clf_cv.best_params_[key]) 

Ayrıca sweeping_results.txt dosya ile tüm kılavuzdaki hiper parametre değerleri birleşimlerini arası doğrulandı ve negatif günlük kayıplarını oluştururuz.

    if not path.exists('./outputs'):
        makedirs('./outputs')
    outfile = open('./outputs/sweeping_results.txt','w')

    print("metric = ", metric, file=outfile)
    for i in range(len(model.grid_scores_)):
        print(model.grid_scores_[i], file=outfile)
    outfile.close()

Bu dosya özel depolanır. / directory çıkarır. Daha sonra nasıl indireceğiniz göstereceğiz.  

 SingleVM.py içinde uzak DSVM ilk defa çalıştırmadan önce bir Docker kapsayıcısı var. çalıştırarak oluşturuyoruz 

    az ml experiment prepare -c dsvm

CLI windows. Oluşturma, Docker kapsayıcı birkaç dakika sürer. Bundan sonra singleVM.py DSVM içinde çalıştırıyoruz:

    az ml experiment submit -c dsvm .\singleVM.py

Bu komut, 1 saat 38 DSVM 8 çekirdek ve bellek, 28 Gb olduğunda dakika içinde tamamlanır. Günlüğe kaydedilen değerleri AML Workbench'i çalıştırma geçmişi penceresinde görüntülenebilir:

![Çalıştırma geçmişi](media/scenario-distributed-tuning-of-hyperparameters/run_history.png)

Varsayılan olarak günlüğe kaydedilen değerler 1-2 değerleri ve grafikler ilk çalıştırma Geçmişi penceresini gösterir. Hiperparametreleri seçilen değerlerini tam listesini görmek için önceki ekran görüntüsünde kırmızı bir daire ile işaretlenmiştir ayarlar simgesine tıklayın. Ardından tabloda gösterilecek hiperparametreleri seçin. Ayrıca, çalıştırma geçmişi penceresinin üst kısmında gösterilen grafikleri seçmek için mavi daire işaretlenen ayar simgesine tıklayın ve grafik listeden seçin. 

Hiperparametreleri seçilen değerlerini de çalıştırma Özellikleri penceresinde incelenmesi: 

![çalıştırma özellikleri](media/scenario-distributed-tuning-of-hyperparameters/run_properties.png)

Çalıştırma özellikleri penceresinin sağ üst köşesinde bir bölümünü çıkış dosyalarının listesi ile oluşturulan tüm dosyalar yok '. \output' klasörü. Süpürme\_sonuç.txt seçip indirme düğmesine tıklatarak oradan indirilebilir. Aşağıdaki çıktı sweeping_results.txt sahip olmalıdır:

    metric =  neg_log_loss
    mean: -2.29096, std: 0.03748, params: {'colsample_bytree': 1, 'learning_rate': 0.1, 'subsample': 0.5, 'n_estimators': 300, 'reg_alpha': 1, 'objective': 'multi:softprob', 'colsample_bylevel': 0.1, 'reg_lambda': 1, 'max_depth': 3}
    mean: -2.28712, std: 0.03822, params: {'colsample_bytree': 1, 'learning_rate': 0.1, 'subsample': 0.5, 'n_estimators': 400, 'reg_alpha': 1, 'objective': 'multi:softprob', 'colsample_bylevel': 0.1, 'reg_lambda': 1, 'max_depth': 3}
    mean: -2.28706, std: 0.03863, params: {'colsample_bytree': 1, 'learning_rate': 0.1, 'subsample': 0.5, 'n_estimators': 300, 'reg_alpha': 1, 'objective': 'multi:softprob', 'colsample_bylevel': 0.1, 'reg_lambda': 1, 'max_depth': 4}
    mean: -2.28530, std: 0.03927, params: {'colsample_bytree': 1, 'learning_rate': 0.1, 'subsample': 0.5, 'n_estimators': 400, 'reg_alpha': 1, 'objective': 'multi:softprob', 'colsample_bylevel': 0.1, 'reg_lambda': 1, 'max_depth': 4}

### <a name="tuning-hyperparameters-using-spark-cluster"></a>Spark kümesi kullanarak hiperparametreleri ayarlama
Spark kümesi ayarlama hiperparametreleri ölçeklendirme ve daha büyük bir kılavuz kullanmak için kullanırız. Yeni sunduğumuz kılavuz

    tuned_parameters = [{'n_estimators': [300,400], 'max_depth': [3,4], 'objective': ['multi:softprob'], 'reg_alpha': [1], 'reg_lambda': [1], 'colsample_bytree': [1], 'learning_rate': [0.1], 'colsample_bylevel': [0.01, 0.1], 'subsample': [0.5, 0.7]}]

Bu kılavuz, 16 hiperparametreleri değerleri birleşimlerini sahiptir. Biz 5-fold çapraz doğrulama kullandığından, xgboost 16 x 5 = 80 çalıştırıyoruz. saatler.

scikit-paket yerel bir Spark kümesi kullanarak hiperparametreleri ayarlama desteği yok öğrenin. Neyse ki, [spark sklearn](https://spark-packages.org/package/databricks/spark-sklearn) Databricks paketinden bu boşluğu doldurur. Bu paket scikit neredeyse aynı API GridSearchCV işlevi olarak olan GridSearchCV işlev sağlar-öğrenin. Spark sklearn öğesini kullanın ve Spark bağlamını oluşturmak için ihtiyacımız olan Spark kullanarak hiperparametreleri ayarlamak için

    from pyspark import SparkContext
    sc = SparkContext.getOrCreate()

Biz değiştirin 

    from sklearn.model_selection import GridSearchCV

with 

    from spark_sklearn import GridSearchCV

Biz de scikit'dan GridSearchCV çağrısını değiştirin-spark sklearn adresinden öğrenin:

    clf_cv = GridSearchCV(sc = sc, param_grid = tuned_parameters, estimator = clf, scoring=metric, cv=5)

Spark kullanarak hiperparametreleri ayarlama son kod içinde dağıtılmış\_sweep.py dosya. SingleVM.py distributed_sweep.py arasındaki fark, kılavuz tanımını ve dört ek kod satırı ' dir. AML Workbench Hizmetleri nedeniyle de dikkat edin, günlük kod yürütme ortamı Spark kümesine uzak DSVM değiştirilirken değiştirmez.

İlk kez Spark kümesinde distributed_sweep.PY çalıştırmadan önce Python paketlerini yüklememiz gerekir. Bu çalıştırarak sağlanabilir. 

    az ml experiment prepare -c spark

CLI windows. Bu yükleme birkaç dakika sürer. Bundan sonra distributed_sweep.py Spark kümesinde çalıştırıyoruz:

    az ml experiment submit -c spark .\distributed_sweep.py

Bu komut, 1 saat 6 Spark kümesi, 28 Gb bellek 6 çalışan düğümleri olduğunda dakika içinde tamamlanır. Hiper parametre ayarı sonuçlarını Azure Machine Learning Workbench uygulamasında uzaktan DSVM yürütme aynı şekilde erişilebilir. (yani, günlükleri, en iyi değerlerini hiperparametreleri sweeping_results.txt dosya ve)

### <a name="architecture-diagram"></a>Mimari diyagramı

Genel iş akışı aşağıdaki diyagramda gösterilmiştir: ![mimarisi](media/scenario-distributed-tuning-of-hyperparameters/architecture.png) 

## <a name="conclusion"></a>Sonuç 

Bu senaryoda, biz hiperparametreleri uzak makinelere ve Spark kümeleri, ayarı yapmak için Azure Machine Learning Workbench'i kullanabilmeniz nasıl gösterilmiştir. Azure Machine Learning Workbench yürütme ortamlarını kolayca yapılandırması için araçlar sağlar gördük. Ayrıca, bunlar arasında kolayca geçiş sağlar. 

## <a name="references"></a>Başvurular

[1] t Chen ve C. Guestrin. [XGBoost: Sistem artırma ölçeklenebilir ağaç](https://arxiv.org/abs/1603.02754). KDD 2016.




