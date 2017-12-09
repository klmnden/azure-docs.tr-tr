---
title: "Azure Machine Learning çalışma ekranı kullanarak Hyperparameters, ayarlama dağıtılmış | Microsoft Docs"
description: "Bu senaryo Azure Machine Learning çalışma ekranı kullanarak hyperparameters dağıtılmış ayarlama yapmak nasıl gösterir"
services: machine-learning
author: pechyony
ms.service: machine-learning
ms.topic: article
ms.author: dmpechyo
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.date: 09/20/2017
ms.openlocfilehash: 9372e45e8666dc572b805dfd4a505c9446145079
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="distributed-tuning-of-hyperparameters-using-azure-machine-learning-workbench"></a>Azure Machine Learning çalışma ekranı kullanarak hyperparameters ayarlama dağıtılmış

Bu senaryo Azure Machine Learning çalışma ekranı scikit uygulayan machine learning algoritmaları hyperparameters ayarlama ölçeklendirmek için nasıl kullanılacağını gösterir-API öğrenin. Yapılandırmak ve bir uzak Docker kapsayıcısı ve Spark kümesi hyperparameters ayarlamak için bir yürütme arka ucu olarak kullanmak üzere nasıl gösteriyoruz.

## <a name="link-of-the-gallery-github-repository"></a>Galeri GitHub deposunun bağlantı
Genel GitHub depo bağlantısını aşağıdadır: 

[https://github.com/Azure/MachineLearningSamples-DistributedHyperParameterTuning](https://github.com/Azure/MachineLearningSamples-DistributedHyperParameterTuning)

## <a name="use-case-overview"></a>Kullanım örneği'ne genel bakış

Birçok machine learning algoritmaları hyperparameters adlı bir veya daha fazla düğmelerini sahip. Kullanıcı tarafından belirtilen ölçümleri göre ölçülen gelecekteki veriler üzerinde kendi performansını iyileştirmek için algoritmalarının ayarlama bu düğmelerini izin ver (örneğin, doğruluk, AUC, RMSE). Veri Bilimcisi bir model eğitim verileri üzerinden ve gelecekteki test verileri görmesini önce oluştururken hyperparameters değerlerini sağlaması gerekir. Model iyi bir performans bilinmeyen sınama verilerinde sahip olması bilinen eğitim veri can nasıl göre hyperparameters değerleri ayarlarız? 

Hyperparameters ayarlama yaygın olarak kullanılan bir tekniktir bir *kılavuz arama* birlikte *çapraz doğrulama*. Çapraz doğrulama ne kadar iyi test küme üzerinde bir eğitim kümesinde eğitilmiş bir modeli tahmin değerlendirir bir tekniktir. Bu teknik kullanılarak, biz ilk veri kümesi K Katlama ayırın ve bir hepsini şekilde algoritması K kez eğitmek. Tüm bunu ancak Katlama birini "tutulan çıkış Katlama" denir. Biz K modelleri ölçümleri, ortalama değerini K tutulan çıkış Katlama işlem. Adlı bu ortalama değer *arası doğrulanmış performans tahmin*, K modelleri oluşturulurken kullanılan hyperparameters değerlerine bağlıdır. Hyperparameters ayarlama, biz çapraz doğrulama performansı en iyi duruma olanları tahmin bulmak için aday hyperparameter değerleri alanı arayın. Kılavuz arama ortak bir arama tekniktir. Kılavuz aramada çapraz ürün tek tek hyperparameters adayı değerlerinin kümelerinin birden çok hyperparameters adayı değerlerini alanıdır. 

Çapraz doğrulama kullanarak kılavuz arama zaman alıcı olabilir. Bir algoritma beş hyperparameters her beş adayı değerlerle varsa, K = 5 Katlama kullanırız. Biz ardından göre kılavuz arama tamamlayın 5 eğitim<sup>6</sup>15625 modelleri =. Neyse ki, kılavuz arama çapraz doğrulama kullanmaktır utandırıcı derecede paralel bir yordam ve bu modeller paralel olarak Eğitilecek.

## <a name="prerequisites"></a>Ön koşullar

* Bir [Azure hesabı](https://azure.microsoft.com/free/) (ücretsiz deneme kullanılabilir).
* Yüklü bir kopyasını [Azure Machine Learning çalışma ekranı](./overview-what-is-azure-ml.md) aşağıdaki [yükleme ve hızlı başlangıç oluşturma](./quickstart-installation.md) çalışma ekranı yükleyip hesapları oluşturun.
* Bu senaryo, Azure ML çalışma ekranı Windows 10 veya MacOS yerel olarak yüklenmiş Docker altyapısıyla çalıştığını varsayar. 
* Senaryo ile uzak bir Docker kapsayıcısı çalıştırmak için Ubuntu veri bilimi sanal makine (DSVM) izleyerek sağlamak [yönergeleri](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-data-science-provision-vm). En az 8 çekirdek ve bellek 28 Gb ile bir sanal makine kullanmanızı öneririz. Sanal makineler D4 örneklerini böyle kapasiteye sahip. 
* Bu senaryo bir Spark kümesiyle çalıştırmak için sağlama Azure Hdınsight kümesi bu izleyerek [yönergeleri](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-provision-linux-clusters). İle bir kümede en az olması öneririz 
- altı çalışan düğümleri
- sekiz Çekirdeği
- Üstbilgi ve çalışan düğümleri bellekte 28 Gb. Sanal makineler D4 örneklerini böyle kapasiteye sahip. Kümenin performansını en üst düzeye çıkarmak için aşağıdaki parametreleri değiştirme öneririz.
- Spark.Executor.instances
- Spark.Executor.cores
- Spark.Executor.Memory 

Bu takip edebilirsiniz [yönergeleri](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-resource-manager) ve "özel spark varsayılan olarak" bölümündeki tanımları düzenleyin.

     **Troubleshooting**: Your Azure subscription might have a quota on the number of cores that can be used. The Azure portal does not allow the creation of cluster with the total number of cores exceeding the quota. To find you quota, go in the Azure portal to the Subscriptions section, click on the subscription used to deploy a cluster and then click on **Usage+quotas**. Usually quotas are defined per Azure region and you can choose to deploy the Spark cluster in a region where you have enough free cores. 

* Veri kümesi depolamak için kullanılan bir Azure depolama hesabı oluşturun. İzleyin [yönergeleri](https://docs.microsoft.com/en-us/azure/storage/common/storage-create-storage-account) bir depolama hesabı oluşturmak için.

## <a name="data-description"></a>Veri açıklaması

Kullanırız [TalkingData dataset](https://www.kaggle.com/c/talkingdata-mobile-user-demographics/data). Bu veri kümesi, cep telefonları uygulamalardan olaylarını sahiptir. Cep telefonu kullanıcı türünü telefon ve kullanıcı en son oluşturulan olaylar verilen cinsiyeti ve yaş kategorisini tahmin etmek için belirtilir.  

## <a name="scenario-structure"></a>Senaryo yapısı
Bu senaryoda, GitHub deposunda birden fazla klasör sahiptir. Kod ve yapılandırma dosyaları olan **kod** tüm belgeleri klasörüdür içinde **belgeleri** klasör ve tüm görüntüleri **görüntüleri** klasör. Kök klasör bu senaryo kısa bir özetini içeren Benioku dosyası içerir.

### <a name="getting-started"></a>Başlarken
Azure Machine Learning çalışma ekranı çalıştırın ve "Dağıtılmış ayarlama, Hyperparameters" şablonu bir proje oluşturmak için Azure Machine Learning çalışma ekranı simgesine tıklayın. Yeni bir proje oluşturma hakkında ayrıntılı yönergeler bulabilirsiniz [yükleyin ve hızlı başlangıç oluşturma](quickstart-installation.md).   

### <a name="configuration-of-execution-environments"></a>Yürütme ortamları yapılandırma
Uzak bir Docker kapsayıcısı ve bir Spark kümesi bizim kodu çalıştırmak nasıl gösteriyoruz. Her iki ortamlar için ortak olan ayarlarının açıklaması ile başlatın. 

Kullanırız [scikit-öğrenin](https://anaconda.org/conda-forge/scikit-learn), [xgboost](https://anaconda.org/conda-forge/xgboost), ve [azure depolama](https://pypi.python.org/pypi/azure-storage) Azure Machine Learning çalışma ekranı varsayılan Docker kapsayıcısında sağlanmamış olan paketler. Azure depolama paketi yüklemesini gerektirir [şifreleme](https://pypi.python.org/pypi/cryptography) ve [azure](https://pypi.python.org/pypi/azure) paketler. Docker kapsayıcısı ve Spark küme düğümlerinin bu paketleri yüklemek için biz conda_dependencies.yml dosyasında değiştirin:

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

Değiştirilen conda\_dependencies.yml dosya öğretici aml_config dizininde depolanır. 

Sonraki adımlarda size Azure hesabına yürütme ortamı bağlayın. Dosya menüsü AML çalışma ekranı sol üst köşesinden'ı tıklatın. Ve "komut istemini açın"'i seçin. Ardından CLI çalıştırın

    az login

Bir ileti alma

    To sign in, use a web browser to open the page https://aka.ms/devicelogin and enter the code <code> to authenticate.

Bu web sayfası için bir kod girin ve Azure hesabınızda oturum açın gidin. Bu adımdan sonra CLI çalıştırın

    az account list -o table

ve AML çalışma ekranı çalışma alanı hesabınızın sahip Azure abonelik kimliği bulunamadı. Son olarak, CLI içinde çalıştırın

    az account set -s <subscription ID>

Azure aboneliğinize bağlantıyı tamamlamak için.

Sonraki iki bölümde uzak docker ve Spark küme yapılandırmasını tamamlamak nasıl gösterir.

#### <a name="configuration-of-remote-docker-container"></a>Uzak Docker kapsayıcısı yapılandırması

 Uzak bir Docker kapsayıcısı ayarlama ayarlamak için CLI çalıştırın

    az ml computetarget attach remotedocker --name dsvm --address <IP address> --username <username> --password <password> 

IP adresi, kullanıcı adınızı ve parolanızı DSVM ile. IP adresi DSVM DSVM sayfanızı genel bakış bölümünde Azure portalında bulunabilir:

![VM IP](media/scenario-distributed-tuning-of-hyperparameters/vm_ip.png)

#### <a name="configuration-of-spark-cluster"></a>Spark kümesi yapılandırması

Spark ortamını ayarlamak için CLI çalıştırın

    az ml computetarget attach cluster--name spark --address <cluster name>-ssh.azurehdinsight.net  --username <username> --password <password> 

Küme, kümenin SSH kullanıcı adı ve parola adı ile. SSH kullanıcı adı varsayılan değeri `sshuser`sürece küme hazırlama sırasında değiştirildi. Küme adı, küme sayfanızı özellikleri bölümünde Azure portalında bulunabilir:

![Küme adı](media/scenario-distributed-tuning-of-hyperparameters/cluster_name.png)

Spark sklearn paket dağıtılmış hyperparameters ayarlama yürütme ortamı olarak Spark sağlamak için kullanırız. Biz, Spark yürütme ortamı kullanıldığında, bu paketi yüklemek için spark_dependencies.yml dosyasını değiştirilme tarihi:

    configuration: {}
    repositories:
      - "https://mmlspark.azureedge.net/maven"
      - "https://spark-packages.org/packages"
    packages:
      - group: "com.microsoft.ml.spark"
        artifact: "mmlspark_2.11"
        version: "0.7"
      - group: "databricks"
        artifact: "spark-sklearn"
        version: "0.2.0"

Değiştirilen spark\_dependencies.yml dosya öğretici aml_config dizininde depolanır. 

### <a name="data-ingestion"></a>Veri alımı
Bu senaryoda kod, veriler Azure blob depolama alanına depolanır varsayar. Başlangıçta verileri Kaggle sitesinden bilgisayarınıza indirin ve blob depolama alanına yüklemek nasıl gösteriyoruz. Ardından blob depolama alanından verileri okumak nasıl gösterir. 

Kaggle veri indirmek için Git [dataset sayfa](https://www.kaggle.com/c/talkingdata-mobile-user-demographics/data) ve Yükle düğmesini tıklayın. Kaggle için oturum açma istenir. Oturum açtıktan sonra veri kümesi sayfasına yönlendirilir. Daha sonra bunları seçerek ve Yükle düğmesini tıklatarak sol sütunda ilk yedi dosyalarını indirin. İndirilen dosyaların toplam boyutu 289 Mb'tır. Blob depolama için bu dosyaları karşıya yüklemek için depolama hesabınızdaki 'veri kümesi' blob depolama kapsayıcısını oluşturun. Depolama hesabı, BLOB'lar ve ardından tıklatarak + kapsayıcı Azure sayfasına giderek yapabilirsiniz. 'Veri kümesi' bir ad girin ve Tamam'ı tıklatın. Aşağıdaki ekran görüntüleri adımları gösterilmiştir:

![Açık blob](media/scenario-distributed-tuning-of-hyperparameters/open_blob.png)
![açık kapsayıcısı](media/scenario-distributed-tuning-of-hyperparameters/open_container.png)

Bundan sonra dataset kapsayıcı listeden seçin ve karşıya yükleme düğmesini tıklatın. Azure portalı aynı anda birden çok dosya karşıya yüklemenize olanak sağlar. "Blob karşıya yükle" bölümünde klasör düğmesini tıklatın, veri kümesinden alınan tüm dosyalar'ı seçin, Aç'ı tıklatın ve ardından Yükle'yi tıklatın. Aşağıdaki ekran görüntüsünde adımları gösterilmektedir:

![BLOB karşıya yükleme](media/scenario-distributed-tuning-of-hyperparameters/upload_blob.png) 

Dosyaları karşıya yükleme Internet bağlantınızın bağlı olarak birkaç dakika sürer. 

Bizim kodda kullanırız [Azure depolama SDK'sı](https://azure-storage.readthedocs.io/en/latest/) dataset geçerli yürütme ortamı için blob depolama alanından indirmek için. Karşıdan yükleme gerçekleştirilen\_load_data.py dosyasından data() işlevi. Bu kodu kullanmak için < ACCOUNT_NAME > değiştirmeniz gerekiyor ve < ACCOUNT_KEY > adı ile dataset barındıran depolama hesabınızın birincil anahtar. Depolama hesabınızın Azure sayfasının sol üst köşesinde hesap adında görebilirsiniz. Hesap almak için depolama Azure sayfasındaki anahtar, select erişim tuşları (veri alımı bölümündeki ilk ekran görüntüsüne bakın) hesap ve ardından uzun bir dize anahtar sütunun ilk satırında kopyalayın:
 
![Erişim anahtarı](media/scenario-distributed-tuning-of-hyperparameters/access_key.png)

Aşağıdaki kod load_data() işlevden tek bir dosya yükler:

    from azure.storage.blob import BlockBlobService

    # Define storage parameters 
    ACCOUNT_NAME = "<ACCOUNT_NAME>"
    ACCOUNT_KEY = "<ACCOUNT_KEY>"
    CONTAINER_NAME = "dataset"

    # Define blob service     
    my_service = BlockBlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)

    # Load blob
    my_service.get_blob_to_path(CONTAINER_NAME, 'app_events.csv.zip', 'app_events.csv.zip')

Load_data.py dosyasını el ile çalıştırın gerekmez dikkat edin. Daha sonra diğer dosyalarından adı verilir.

### <a name="feature-engineering"></a>Özellik mühendisliği
Tüm özellikleri bilgi işlem kodunu özelliktir\_engineering.py dosya. Feature_engineering.py dosyasını el ile çalıştırmak gerekmez. Daha sonra diğer dosyalarından çağrılır.

Birden çok özellik kümeleri oluşturuyoruz:
* Marka ve cep telefonu modelinin bir hot kodlama (bir\_etkin\_brand_model işlevi)
* Her iş günü içinde kullanıcı tarafından oluşturulan olayları kesir (hafta içi günü\_hour_features işlevi)
* Her bir saat içinde kullanıcı tarafından oluşturulan olayları kesir (hafta içi günü\_hour_features işlevi)
* Her hafta içi günü ve saat birleşimi kullanıcı tarafından oluşturulan olayları kesir (hafta içi günü\_hour_features işlevi)
* Her uygulama kullanıcı tarafından oluşturulan olayları kesir (bir\_etkin\_app_labels işlevi)
* Her uygulama etiket kullanıcı tarafından oluşturulan olayları kesir (bir\_etkin\_app_labels işlevi)
* Her uygulama kategorideki kullanıcı tarafından oluşturulan olayların kesir (metin\_category_features işlevi)
* Gösterge özellikleri kategorileri için kullanılan uygulama için oluşturulan olaylar (bir\_hot_category işlevi)

Bu özellikler tarafından Kaggle çekirdek neden olacak [uygulamaları ve etiketleri doğrusal bir model](https://www.kaggle.com/dvasyukova/a-linear-model-on-apps-and-labels).

Bu özelliklerin hesaplama önemli miktarda bellek gerektirir. İlk 16 GB RAM ile yerel ortamda özellikleri işlem denedik. Biz ilk dört özellik kümesi işlem açabildi ancak beşinci özellik kümesi hesaplanırken 'yetersiz bellek' hata alındı. İlk dört özellik kümeleri hesaplama singleVMsmall.py dosyasıdır ve bunu yerel ortamda çalıştırarak yürütülebilir. 

     az ml experiment submit -c local .\singleVMsmall.py   

CLI penceresinde.

Yerel ortamdaki tüm ayarlar özellik bilgi işlem için çok küçük olduğundan, biz büyük belleğe sahip uzak DSVM geçin. DSVM içinde yürütme AML çalışma ekranı tarafından yönetilen Docker kapsayıcısı içinde yapılır. Bu DSVM kullanarak tüm özellikleri işlem ve modelleri eğitme ve hyperparameters (sonraki bölüme bakın) ayarlamak için duyuyoruz. singleVM.py dosya Tamamlama özelliği hesaplama ve kod modelleme vardır. Sonraki bölümde uzak DSVM singleVM.py çalıştırma göstereceğiz. 

### <a name="tuning-hyperparameters-using-remote-dsvm"></a>Uzak DSVM kullanarak hyperparameters ayarlama
Kullanırız [xgboost](https://anaconda.org/conda-forge/xgboost) gradyan ağaç artırmanın, uygulama [1]. Kullanırız [scikit-öğrenin](http://scikit-learn.org/) xgboost hyperparameters ayarlamak için paket. Xgboost scikit bir parçası olmasa da-paket, bilgi scikit uygulayan-API öğrenmek ve bu nedenle scikit işlevlerini ayarlama hyperparameter ile birlikte kullanılabilir-öğrenin. 

Xgboost sekiz hyperparameters sahiptir:
* n_estimators
* max_depth
* reg_alpha
* reg_lambda
* colsample\_by_tree
* learning_rate
* colsample\_by_level
* subsample
* Bu hyperparameters açıklamasını bulunabilir hedefi
- http://xgboost.readthedocs.io/en/Latest/Python/python_api.HTML#Module-xgboost.sklearn-https://github.com/dmlc/xgboost/blob/master/doc/parameter.md). 
- 
Başlangıçta, uzak DSVM kullanır ve küçük bir kılavuz adayı değerlerin gelen hyperparameters ince ayar:

    tuned_parameters = [{'n_estimators': [300,400], 'max_depth': [3,4], 'objective': ['multi:softprob'], 'reg_alpha': [1], 'reg_lambda': [1], 'colsample_bytree': [1],'learning_rate': [0.1], 'colsample_bylevel': [0.1,], 'subsample': [0.5]}]  

Bu kılavuz hyperparameters değerlerini dört birleşimlerini sahiptir. 5-fold çapraz doğrulama kullanıyoruz, sonuçta elde edilen 4 x 5 = 20 xgboost çalıştırılır. Modelleri performansını ölçmek için negatif günlük kaybı ölçüm kullanırız. Aşağıdaki kod arası doğrulanmış negatif günlük kaybını en üst düzeye hyperparameters kılavuz gelen değerlerini bulur. Kod bu değerleri tam eğitim küme üzerinde son modeli eğitmek için de kullanır:

    clf = XGBClassifier(seed=0)
    metric = 'neg_log_loss'
    
    clf_cv = GridSearchCV(clf, tuned_parameters, scoring=metric, cv=5, n_jobs=8)
    model = clf_cv.fit(X_train,y_train)

Model oluşturduktan sonra biz hyperparameter ayarlama sonuçlarını kaydedin. API, AML çalışma ekranı günlüğü hyperparameters en iyi değerlerini ve karşılık gelen arası doğrulanmış tahmin negatif günlük kaybı kaydetmek için kullanırız:

    from azureml.logging import get_azureml_logger

    # initialize logger
    run_logger = get_azureml_logger()

    ...

    run_logger.log(metric, float(clf_cv.best_score_))

    for key in clf_cv.best_params_.keys():
        run_logger.log(key, clf_cv.best_params_[key]) 

Biz de sweeping_results.txt dosyası arası doğrulandı, negatif günlük zararları tüm bileşimlerinden birini kılavuzdaki hyperparameter değerleri ile oluşturun.

    if not path.exists('./outputs'):
        makedirs('./outputs')
    outfile = open('./outputs/sweeping_results.txt','w')

    print("metric = ", metric, file=outfile)
    for i in range(len(model.grid_scores_)):
        print(model.grid_scores_[i], file=outfile)
    outfile.close()

Bu dosya bir özel saklanır. / directory çıkarır. Daha sonra onun nasıl indirileceğini gösterir.  

 SingleVM.py içinde uzak DSVM ilk defa çalıştırmadan önce Docker kapsayıcısı var. çalıştırarak oluşturuyoruz 

    az ml experiment prepare -c dsvm

CLI Windows'ta. Oluşturma, Docker kapsayıcısı birkaç dakika sürer. Bundan sonra biz singleVM.py içinde DSVM çalıştırın:

    az ml experiment submit -c dsvm .\singleVM.py

Bu komut, 1 saat 38 DSVM 8 çekirdek ve bellek 28 Gb olduğunda dakika içinde tamamlanır. Günlüğe kaydedilen değerler AML ekranının çalıştırmak Geçmiş penceresinde görüntülenebilir:

![çalıştırma geçmişi](media/scenario-distributed-tuning-of-hyperparameters/run_history.png)

Varsayılan olarak, günlüğe kaydedilen değerler 1-2 çalıştırma Geçmişi penceresini değerleri ve ilk grafiklerini gösterir. Hyperparameters seçilen değerlerini tam listesini görmek için önceki ekran görüntüsünde kırmızı daire ile işaretlenen ayarlar simgesine tıklayın. Ardından tabloda gösterilen hyperparameters seçin. Ayrıca, çalıştırma Geçmişi penceresini üst kısmında gösterilen grafikleri seçmek için mavi bir daire olarak işaretlenmiş ayarı simgesini tıklatın ve listeden grafikleri seçin. 

Hyperparameters seçilen değerlerini de Çalıştır özellikleri penceresinde incelenebilir: 

![özelliklerini çalıştır](media/scenario-distributed-tuning-of-hyperparameters/run_properties.png)

Çalıştır özellikleri penceresinde sağ üst köşesinde, bir bölümü yoktur çıktı dosyaları listesi ile oluşturulmuş tüm dosyaların '. \output' klasörü. yerleştirmez\_sonuç.txt seçerek ve Yükle düğmesini tıklayarak buradan yüklenebilir. sweeping_results.txt aşağıdaki çıkış olmalıdır:

    metric =  neg_log_loss
    mean: -2.29096, std: 0.03748, params: {'colsample_bytree': 1, 'learning_rate': 0.1, 'subsample': 0.5, 'n_estimators': 300, 'reg_alpha': 1, 'objective': 'multi:softprob', 'colsample_bylevel': 0.1, 'reg_lambda': 1, 'max_depth': 3}
    mean: -2.28712, std: 0.03822, params: {'colsample_bytree': 1, 'learning_rate': 0.1, 'subsample': 0.5, 'n_estimators': 400, 'reg_alpha': 1, 'objective': 'multi:softprob', 'colsample_bylevel': 0.1, 'reg_lambda': 1, 'max_depth': 3}
    mean: -2.28706, std: 0.03863, params: {'colsample_bytree': 1, 'learning_rate': 0.1, 'subsample': 0.5, 'n_estimators': 300, 'reg_alpha': 1, 'objective': 'multi:softprob', 'colsample_bylevel': 0.1, 'reg_lambda': 1, 'max_depth': 4}
    mean: -2.28530, std: 0.03927, params: {'colsample_bytree': 1, 'learning_rate': 0.1, 'subsample': 0.5, 'n_estimators': 400, 'reg_alpha': 1, 'objective': 'multi:softprob', 'colsample_bylevel': 0.1, 'reg_lambda': 1, 'max_depth': 4}

### <a name="tuning-hyperparameters-using-spark-cluster"></a>Spark kümesi kullanarak hyperparameters ayarlama
Spark kümesi hyperparameters ayarlama ölçeklendirme ve daha büyük kılavuz kullanmak için kullanırız. Bizim yeni kılavuz

    tuned_parameters = [{'n_estimators': [300,400], 'max_depth': [3,4], 'objective': ['multi:softprob'], 'reg_alpha': [1], 'reg_lambda': [1], 'colsample_bytree': [1], 'learning_rate': [0.1], 'colsample_bylevel': [0.01, 0.1], 'subsample': [0.5, 0.7]}]

Bu kılavuz hyperparameters değerlerini 16 birleşimlerini sahiptir. Biz 5-fold çapraz doğrulama kullandığından, biz xgboost 16 x 5 = 80 çalıştırmak kez.

scikit-paket Spark kümesi kullanarak hyperparameters ayarlama, yerel bir desteği yok öğrenin. Neyse ki, [spark sklearn](https://spark-packages.org/package/databricks/spark-sklearn) Databricks paketinden bu boşluğu doldurur. Bu paket scikit neredeyse aynı API GridSearchCV işlevi olarak olan GridSearchCV işlev sağlar-öğrenin. Spark sklearn kullanın ve Spark kullanarak hyperparameters ayarlamak için Spark bağlam oluşturmak için bağlanmak ihtiyacımız

    from pyspark import SparkContext
    sc = SparkContext.getOrCreate()

Biz değiştirin 

    from sklearn.model_selection import GridSearchCV

with 

    from spark_sklearn import GridSearchCV

Ayrıca biz scikit GridSearchCV çağrısından değiştirin-spark sklearn adresinden öğrenin:

    clf_cv = GridSearchCV(sc = sc, param_grid = tuned_parameters, estimator = clf, scoring=metric, cv=5)

Spark kullanarak hyperparameters ayarlama son kod içinde dağıtılmış\_sweep.py dosya. SingleVM.py distributed_sweep.py arasındaki fark kılavuz tanımını ve dört ek kod satırı de sağlanır. Ayrıca AML çalışma ekranı Hizmetleri nedeniyle günlük kodu yürütme ortamı için Spark kümesi uzak DSVM değiştirirken değiştirmemesini dikkat edin.

Distributed_sweep.PY Spark kümesinde ilk defa çalıştırmadan önce biz Python paketlerini yüklemeniz gerekir. Bu çalıştırarak sağlanabilir 

    az ml experiment prepare -c spark

CLI Windows'ta. Bu yükleme birkaç dakika sürer. Bundan sonra biz distributed_sweep.py Spark kümesinde çalıştırın:

    az ml experiment submit -c spark .\distributed_sweep.py

Bu komut, 1 saat 6 Spark kümesi 28 Gb bellek ile 6 çalışan düğümleri olduğunda dakika içinde tamamlanır. Sonuçları hyperparameter ayarlama, Azure Machine Learning çalışma uzaktan DSVM yürütme aynı şekilde erişilebilir. (yani günlükleri, en iyi değerlerini hyperparameters ve sweeping_results.txt dosyası)

### <a name="architecture-diagram"></a>Mimari diyagramı

Genel iş akışını Aşağıdaki diyagramda gösterilmiştir: ![mimarisi](media/scenario-distributed-tuning-of-hyperparameters/architecture.png) 

## <a name="conclusion"></a>Sonuç 

Bu senaryoda, Azure Machine Learning çalışma ekranı hyperparameters uzak sanal makineler ve Spark kümeleri, ayarı yapmak için nasıl kullanılacağı gösterilmiştir. Azure Machine Learning çalışma ekranı yürütme ortamlarının kolay bir yapılandırma için araçlar sağlar gördük. Ayrıca, kolayca aralarında geçiş sağlar. 

## <a name="references"></a>Başvurular

[1] T. Chen ve c Guestrin. [XGBoost: Sistem artırmanın ölçeklenebilir ağaç](https://arxiv.org/abs/1603.02754). KDD 2016.




