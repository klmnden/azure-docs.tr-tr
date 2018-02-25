---
title: "Tahmine dayalı bakım gerçek dünya senaryoları - Azure için ayrıntılı öğrenme | Microsoft Docs"
description: "Derin öğrenme öğretici Azure Machine Learning çalışma ekranı ile Tahmine dayalı bakım için çoğaltma öğrenin."
services: machine-learning
author: ehrlinger
ms.author: jehrling
manager: ireiter
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.devlang: 
ms.topic: article
ms.date: 11/22/2017
ms.openlocfilehash: 022e629469745fceb4fb9355cb6259a144dda222
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="deep-learning-for-predictive-maintenance-real-world-scenarios"></a>Tahmine dayalı bakım gerçek dünya senaryoları için derin öğrenme

Derin öğrenme machine learning en popüler eğilimler biridir ve uygulamalar dahil olmak üzere birçok alanlarına sahiptir:
- Driverless araba ve robotics.
- Konuşma ve görüntü tanıma.
- Finansal tahmin.

Derin sinir ağları (DNN) olarak da bilinen bu yöntemleri (Biyolojik sinir ağları) beyin içinde olan tek tek neurons tarafından neden olacak.

Zamanlanmamış ekipman kapalı kalma süresi etkisi herhangi bir işletme için detrimental olabilir. Alan donanım kullanımını ve performansını en üst düzeye çıkarmak ve pahalı, zamanlanmamış kapalı kalma süresini en aza indirmek için çalışan tutmak önemlidir. Sorunları erken tanımlaması uygun maliyetli bir şekilde sınırlı bakım kaynakları tahsis ve kalitesini geliştirmek ve tedarik zinciri süreçlerinin yardımcı olabilir. 

Tahmine dayalı Bakım (PM) stratejisi makine öğrenme yöntemlerini erken önlem olumsuz makine performansı önlemek için bakım gerçekleştirmek için donanım durumunu belirlemek için kullanır. PM içinde veri makinenin durumunu izlemek için zaman içerisinde toplanır ve hataları tahmin etmek için desenleri bulmak için analiz edilir. [Uzun kısa vadeli bellek (LSTM)](http://colah.github.io/posts/2015-08-Understanding-LSTMs/) ağlardır veri serilerinden öğrenmek için tasarlanmışlardır beri bu ayarı için çekici.

### <a name="cortana-intelligence-gallery-github-repository"></a>Cortana Intelligence Galerisi GitHub deposunu

Cortana Intelligence Galerisi'nde PM öğretici için ortak GitHub deposudur ([https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance)) burada rapor sorunları ve olun katkı.


## <a name="use-case-overview"></a>Kullanım örneği'ne genel bakış

Bu öğretici benzetimli uçak motoru çalıştırma hatası olaylarının örneği işlem modelleme Tahmine dayalı bakım göstermek için kullanır. Senaryo konusunda açıklandığı [Tahmine dayalı Bakım](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3).

Bu ayarda ana varsayımına aşamalı olarak yaşam süresi boyunca düşürülmüş altyapısıdır. Düşüşü altyapısı algılayıcı ölçüleri algılanabilir. Bu algılayıcı değerlerini değişiklikleri ve geçmiş hataları arasındaki ilişkiyi modellemek PM çalışır. Model, ardından ne zaman ve altyapı algılayıcı ölçümleri geçerli durumuna göre gelecekte başlatılamayabilir tahmin edebilirsiniz.

Bu senaryo, geçmiş algılayıcı değerlerini kullanarak, uçak motorları, kalan kullanım ömrü (RUL) tahmin etmek için bir LSTM ağ oluşturur. Senaryo kullanır [Keras](https://keras.io/) kitaplıkla [Tensorflow](https://www.tensorflow.org/) bilgi işlem altyapısı olarak derin öğrenme çerçevesi. Senaryo motorları bir dizi LSTM eğitir ve ağ üzerinde görünmeyen altyapısı kümesi sınar.

## <a name="prerequisites"></a>Önkoşullar
- Bir [Azure hesabı](https://azure.microsoft.com/free/) (ücretsiz deneme kullanılabilir).
- Azure Machine Learning çalışma ekranı, oluşturulan bir çalışma alanıyla.
- Model operationalization için: Azure Machine Learning Operationalization kurma, bir yerel dağıtım ortamı ve bir [Azure Machine Learning modeli yönetim hesabı](model-management-overview.md).

## <a name="create-a-new-workbench-project"></a>Yeni bir çalışma ekranı projesi oluşturma

Bu örnek bir şablon kullanarak yeni bir proje oluşturun:

1. Machine Learning çalışma ekranı açın.
2. Üzerinde **projeleri** sayfasında,  **+** ve ardından **yeni proje**.
3. İçinde **yeni proje oluştur** bölmesinde, yeni projeniz için bilgileri girin.
4. İçinde **arama proje şablonları** arama kutusu, "Tahmine dayalı bakım" yazın ve seçin **derin Learning Tahmine dayalı bakım senaryosunda için** şablonu.
5. **Oluştur**’u seçin.

## <a name="prepare-the-notebook-server-computation-target"></a>Not Defteri sunucusu hesaplama hedef hazırlama

Machine Learning çalışma ekranından yerel makinenizde çalıştırmak için **dosya** menüsünde seçin **komut istemini açın** veya **açık PowerShell CLI**. CLI kullanarak Azure hizmetlerinizi erişmenize olanak sağlayan `az` komutları. İlk olarak, Azure komutu hesabınızla oturum açın:

```
az login
``` 

Https ile kullanılacak bir kimlik doğrulama anahtarı bu komut sağlar:\\aka.ms\devicelogin URL. CLI cihaz oturumu açma işlemi döndürür ve bazı bağlantı bilgilerini sağlar bekler. Yerel varsa sonraki [Docker](https://www.docker.com/get-docker) yükleme komutu ile yerel işlem ortamını hazırlayın:

```
az ml experiment prepare --target docker --run-configuration docker
```

Çalıştırmak için tercih edilir bir [Linux (Ubuntu) için veri bilimi sanal makine (DSVM)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu) bellek ve disk gereksinimleri için. DSVM yapılandırıldıktan sonra aşağıdaki iki komutu ile uzaktan Docker ortamını hazırlayın:

```
az ml computetarget attach remotedocker --name [Connection_Name] --address [VM_IP_Address] --username [VM_Username] --password [VM_UserPassword]
```

Uzak Docker kapsayıcısı bağlandıktan sonra komutu ile DSVM Docker bilgi işlem ortamı hazırlamalısınız: 

```
az ml experiment prepare --target [Connection_Name] --run-configuration [Connection_Name]
```

Hazırlanan Docker bilgi işlem ortamı ile Jupyter not defteri sunucunun Machine Learning çalışma ekranından açmak **not defterlerini** sekmesinde veya tarayıcı tabanlı bir sunucuya komutu ile başlatın: 

```
az ml notebook start
```

Örnek not defterlerini kodu dizininde depolanır. Not defterlerini ardışık olarak çalıştırmak için ilk (Code\1_data_ingestion.ipynb) not defteri başlangıç ayarlanır. Her not defteri açtığınızda, işlem çekirdek seçin istenir. Önceden yapılandırılmış DSVM üzerinde yürütmek için [Project_Name] _şablon [Connection_Name] çekirdek seçin.

## <a name="data-description"></a>Veri açıklaması

Şablon üç veri kümeleri PM_train.txt, PM_test.txt ve PM_truth.txt dosyalarında giriş olarak kullanır. 

- **Eğitim verileri**: Uçak motoru çalıştırma hatası verileri. Tren veri (PM_train.txt) ile birden çok multivariate zaman serisi oluşur *döngüsü* zaman birimi olarak. Bu, her döngü için 21 sensör okumaları içerir. 

    - Her zaman serisi aynı türde başka bir altyapısı oluşturulur. Her motor ilk yıpranması ve bazı benzersiz üretim değişim derecelerde ile başlar. Bu bilgiler kullanıcıya bilinmiyor. 

    - Bu sanal verilerde altyapısı her zaman serisi başlangıcında normal olarak çalışıyor olabilir varsayılır. Belirli bir noktada bir dizi işletim döngüsü sırasında düşmeye başlatır. Bozulması ilerler ve boyutları büyür. 

    - Önceden tanımlanmış bir Eşiğe ulaşıldığında altyapısı daha fazla işlem için güvenli olarak kabul edilir. Her zaman serisinin son döngüsünden bu altyapıyı hata noktasıdır.

- **Test verileri**: kaydedilen hatası olaylarının işletim veri uçak motoru. Test verileri (PM_test.txt) aynı veri şeması eğitim verileri olarak sahiptir. Yalnızca veri hatası oluştuğunda göstermez farktır (son süre mu *değil* başarısızlık noktasını temsil eder). Bu başarısız olmadan önce kaç tane daha fazla döngüleri bu altyapısı devam edebileceği bilinmiyor.

- **Gerçekte veri**: her motor sınama verilerinde bilgilerin doğru kalan döngüleri. Plan gerçekte veri sınama verilerinde altyapıları için kalan çalışma döngüsü sayısını sağlar.

## <a name="scenario-structure"></a>Senaryo yapısı

Senaryo iş akışı üç adımı ayrılmıştır ve her adımı bir Jupyter not defteri yürütülür. Her not not defterlerini kullanmak için yerel olarak kalıcı veri yapıları üretir.

### <a name="task-1-data-ingestion-and-preparation"></a>Görev 1: Veri alımı ve hazırlık

Veri alımı Jupyter not defteri Code/1_data_ingestion_and_preparation.ipnyb üç giriş veri kümeleri Pandas DataFrame biçimine yükler. Not defteri verileri modelleme için hazırlar ve bazı ön veri görselleştirme yapar. Veri kümeleri, Model oluşturma Jupyter not defteri kullanmak için işlem bağlamı için yerel olarak depolanır.

### <a name="task-2-model-building-and-evaluation"></a>Görev 2: Model oluşturma ve değerlendirme

Model oluşturma Jupyter not defteri Code/2_model_building_and_evaluation.ipnyb eğitin ve test veri kümelerini diskten okur ve eğitim veri kümesi için bir LSTM ağ oluşturur. Model performansı test veri kümesinde ölçülür. Sonuç olarak oluşan model serileştirilmiş ve operationalization görev kullanmak için yerel işlem bağlamında depolanır.

### <a name="task-3-operationalization"></a>Görev 3: Operationalization

Code/3_operationalization.ipnyb Operationalization Jupyter Not Defteri, işlevleri ve şeması model bir Azure barındırılan web hizmetini çağırmak için yapı için saklı modelini kullanır. Not Defteri işlevleri sınar ve sonra varlıkları LSTM_o16n.zip dosyaya sıkıştırır. Dosyayı açın, Azure depolama kapsayıcısının dağıtımı için yüklenir.

LSTM_o16n.zip dağıtım dosyası aşağıdaki yapılar içerir:

- **webservices_conda.yaml**: dağıtım hedefteki LSTM modeli çalıştırmak için gereken Python paketlerini tanımlar.  
- **service_schema.JSON**: LSTM modeli tarafından beklenen veri şeması tanımlar.   
- **lstmscore.PY**: dağıtım hedef puan yeni verileri çalıştıran işlevleri tanımlar.    
- **modellstm.JSON**: LSTM mimari tanımlar. Lstmscore.py işlevleri mimarisi ve model başlatmak için ağırlıkları okuyun.
- **modellstm.h5**: model ağırlıkları tanımlar.
- **test_service.PY**: dağıtım uç noktası ile test veri kayıtlarını çağıran bir test komut dosyası. 
- **PM_test_files.pkl**: test_service.py betik PM_test_files.pkl dosyasından geçmiş altyapısı verileri okur ve web hizmeti altyapısı hatası olasılığını döndürülecek LSTM için yeterli döngüleri gönderir.

Not Defteri, dağıtım için operationalization varlıkların paketleri önce model tanım kullanarak işlevleri sınar. Ayarlama ve web hizmeti test etme için yönergeler Code/3_operationalization.ipnyb not defteri sonunda dahil edilir.

## <a name="conclusion"></a>Sonuç

Bu öğretici tahminlerde için algılayıcı değerlerini kullanan basit bir senaryoyu sağlar. Tahmine dayalı bakım senaryolarını gibi daha gelişmiş [Tahmine dayalı bakım modelleme Kılavuzu R not defteri](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-R-Notebook-1) geçmiş bakım kayıtları, hata günlüklerini ve makine özellikler gibi birçok veri kaynaklarını kullanabilir. Ek veri kaynaklarını derin öğrenme ile kullanmak için farklı treatments gerektirebilir.


## <a name="references"></a>Başvurular

Aşağıdaki başvuru diğer Tahmine dayalı bakım örnekleri çeşitli platformlar için kullanım sağlar:

* [Tahmine dayalı bakım çözüm şablonu](https://docs.microsoft.com/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance)
* [Tahmine dayalı bakım modelleme Kılavuzu](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
* [Tahmine dayalı bakım modelleme SQL R Services kullanarak Kılavuzu](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-Modeling-Guide-using-SQL-R-Services-1)
* [Tahmine dayalı Bakım Kılavuzu Python not defteri modelleme](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
* [PySpark kullanarak Tahmine dayalı bakım](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)
* [Tahmine dayalı bakım gerçek dünya senaryosu](https://docs.microsoft.com/en-us/azure/machine-learning/preview/scenario-predictive-maintenance)

## <a name="next-steps"></a>Sonraki adımlar

Machine Learning çalışma içinde ekranı ürünün ek özellikleri gösteren kullanılabilir diğer örnek senaryolar verilmiştir. 
