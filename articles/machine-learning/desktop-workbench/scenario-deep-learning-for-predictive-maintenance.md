---
title: Derin öğrenme için Tahmine dayalı bakım gerçek dünya senaryoları - Azure | Microsoft Docs
description: Azure Machine Learning Workbench ile Tahmine dayalı bakım için derin öğrenme öğreticiyi çoğaltmak öğrenin.
services: machine-learning
author: ehrlinger
ms.author: jehrling
manager: ireiter
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc
ms.devlang: ''
ms.topic: article
ms.date: 11/22/2017
ms.openlocfilehash: 83e1f14db317f59ab2063a9d020adbdb6fe78e5f
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35651414"
---
# <a name="deep-learning-for-predictive-maintenance-real-world-scenarios"></a>Tahmine dayalı bakım gerçek dünya senaryoları için ayrıntılı öğrenme

Derin öğrenme, machine learning'de en popüler eğilimler biridir ve uygulamaları dahil olmak üzere birçok alanlara sahiptir:
- Sürücüsüz araba ve robotlara ilişkin.
- Konuşma ve görüntü tanıma.
- Finansal tahmin.

Derin sinir ağı (DNN) da bilinen, beyin (Biyolojik sinir ağları) içinde tek tek neurons bu yöntemleri ilham.

Çoğu işletme için zamanlanmamış donanım kapalı kalma süresinin olumsuz etkileri olabilir. Kullanımı ve performansı en üst düzeye çıkarmak ve yüksek maliyetli ve zamanlanmamış kapalı kalma süresini en aza indirmek için çalışan alan ekipman tutmak önemlidir. Sorunları erken tanımlaması sınırlı bakım kaynakları uygun maliyetli bir şekilde ayırmanıza ve kalitesini artırmak ve tedarik zinciri işlemleri yardımcı olabilir. 

Tahmine dayalı Bakım (PM) stratejisi bakımının sıd'lerde olumsuz makine performansını önlemek için donanım durumunu belirlemek için machine learning yöntemleri kullanır. Veriler PM, makinenin durumunu izlemek için zaman içerisinde toplanır ve hataları tahmin etmek için kalıpları bulmak için analiz edilir. [Uzun kısa vadeli bellek (LSTM)](http://colah.github.io/posts/2015-08-Understanding-LSTMs/) ağlardır veri serilerinden öğrenmek için tasarlanmışlardır olduğundan bu ayarı için çekici.

### <a name="cortana-intelligence-gallery-github-repository"></a>Cortana Intelligence Galerisi GitHub deposu

Cortana Intelligence Galerisi'nde PM öğreticisi için bir ortak GitHub deposu olduğu ([https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance)) burada rapor sorunları ve katkı.


## <a name="use-case-overview"></a>Kullanım örneği genel bakış

Bu öğreticide sanal uçak motoru çalıştırma hatasına olayları örneği işlem modelleme Tahmine dayalı bakım göstermek için kullanılır. Senaryo anlatıldığı [Tahmine dayalı Bakım](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3).

Bu ayarda ana varsayımına aşamalı olarak yaşam süresi boyunca düşürülmüş altyapısıdır. Performans düşüşü altyapısı algılayıcı ölçüleri algılanabilir. Bu algılayıcı değerlerini değişiklikleri ve geçmiş hataların arasındaki ilişkiyi model PM çalışır. Model, ardından Algılayıcı ölçümleri'nin geçerli durumunu temel alan gelecekte ne zaman ve altyapı başarısız olabilir tahmin edebilirsiniz.

Bu senaryo, geçmiş algılayıcı değerlerini kullanarak, uçak motorları, kalan faydalı ömrü (RUL) tahmin etmek için bir LSTM ağ oluşturur. Senaryo kullanır [Keras](https://keras.io/) kitaplığıyla [Tensorflow](https://www.tensorflow.org/) derin öğrenme framework olarak bir işlem altyapısı. Senaryo altyapıları kümesiyle LSTM eğitir ve ağ üzerinde görünmeyen altyapısı kümesi sınar.

## <a name="prerequisites"></a>Önkoşullar
- Bir [Azure hesabı](https://azure.microsoft.com/free/) (ücretsiz denemeler kullanılabilir).
- Azure Machine Learning Workbench'te, bir çalışma alanı oluşturulur.
- Modeli kullanıma hazır hale getirme: Azure Machine Learning operasyonel hale getirme ayarlamak, bir yerel dağıtım ortamı ile bir [Azure Machine Learning Model Yönetimi hesabı](model-management-overview.md).

## <a name="create-a-new-workbench-project"></a>Workbench yeni bir proje oluşturun

Bu örnek bir şablon kullanarak yeni bir proje oluşturun:

1. Machine Learning Workbench’i açın.
2. Üzerinde **projeleri** sayfasında **+** ve ardından **yeni proje**.
3. İçinde **yeni proje oluştur** bölmesinde, yeni projeniz için bilgi girin.
4. İçinde **proje şablonlarında Ara** arama kutusuna "Tahmine dayalı bakım" yazın ve seçin **derin öğrenme için Tahmine dayalı bakım senaryosunda** şablonu.
5. **Oluştur**’u seçin.

## <a name="prepare-the-notebook-server-computation-target"></a>Not Defteri sunucusu hesaplama hedef hazırlama

Machine Learning Workbench'ten yerel makinenizde çalıştırılacak **dosya** menüsünde seçin **komut istemini Aç** veya **açık PowerShell CLI**. CLI kullanarak Azure hizmetlerinizi erişmenize olanak sağlayan `az` komutları. İlk olarak, bu komutla Azure hesabınızda oturum açın:

```
az login
``` 

Bu komut bir kimlik doğrulama anahtarı ile https sağlar:\\aka.ms\devicelogin URL'si. CLI'yı cihaz oturum açma işlemi döndürür ve bazı bağlantı bilgilerini sağlar bekler. Yerel bir varsa sonraki [Docker](https://www.docker.com/get-docker) yükleme, komutu yerel işlem ortamını hazırlayın:

```
az ml experiment prepare --target docker --run-configuration docker
```

Çalıştırmak için tercih edilir bir [Linux (Ubuntu) için veri bilimi sanal makinesi (DSVM)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu) bellek ve disk gereksinimleri. DSVM yapılandırdıktan sonra aşağıdaki iki komutu ile uzak Docker ortamını hazırlayın:

```
az ml computetarget attach remotedocker --name [Connection_Name] --address [VM_IP_Address] --username [VM_Username] --password [VM_UserPassword]
```

Uzak Docker kapsayıcısına bağlandıktan sonra komutu DSVM Docker işlem ortamını hazırlayın: 

```
az ml experiment prepare --target [Connection_Name] --run-configuration [Connection_Name]
```

Hazırlanmış Docker işlem ortamı ile Jupyter not defteri sunucusu Machine Learning Workbench uygulamasını açın. **not defterlerini** sekmesinde veya tarayıcı tabanlı bir sunucuya komutla başlatın: 

```
az ml notebook start
```

Örnek Not Defterleri kodu dizininde depolanır. Not defterlerini ardışık olarak çalıştırmak için ilk (Code\1_data_ingestion.ipynb) not başlangıç ayarlanır. Her bir not defteri açtığınızda, hesaplama çekirdeğini seçin istenir. Önceden yapılandırılmış DSVM üzerinde yürütmek için [Project_Name] [Connection_Name] _şablon çekirdek seçin.

## <a name="data-description"></a>Veri açıklaması

Şablon dosyalarında PM_train.txt PM_test.txt ve PM_truth.txt girdi olarak üç veri kümelerini kullanır. 

- **Eğitim verileri**: Uçak motoru çalıştırma hatası verileri. Birden çok, çok değişkenli zaman serisi ile eğitme veri (PM_train.txt) oluşan *döngüsü* olarak zaman birimi. Bu, her döngü için 21 sensör okumaları içerir. 

    - Her zaman serisi, aynı türde farklı altyapısından oluşturulur. Her motor ilk giyim ve bazı benzersiz üretim değişim derecesi ile başlar. Bu bilgiler, kullanıcı tarafından bilinmiyor. 

    - Bu sanal verileri her zaman serisi başlangıcında normalde çalışmanız altyapısı varsayılır. Belirli bir noktada bir dizi işletim döngüsü sırasında düşmeye başlar. Performans düşüşü ilerler ve boyutları büyür. 

    - Önceden tanımlanmış bir Eşiğe ulaşıldığında, engine işlemi için daha fazla güvenli olarak kabul edilir. Her zaman serisinin son döngüsünden bu altyapısı bir hata noktasıdır.

- **Test verileri**: Uçak işletim verileri kaydedilen hatası olaylarının altyapısı. Test verilerini (PM_test.txt), eğitim verilerini aynı veri şemaya sahip. Tek fark, hata oluştuğunda veriler göstermez. (son süre mu *değil* hata noktasını temsil eder). Kaç tane daha fazla döngüleri başarısız önce bu altyapı sürebilir bilinmiyor.

- **Gerçekte veri**: sınama verilerinde her motor true kalan bilgilerin geçiş yapar. Toprak truth veri sınama veri altyapıları için kalan çalışma döngüsü sayısını sağlar.

## <a name="scenario-structure"></a>Senaryo yapısı

Senaryo iş akışı içinde üç adımı ayrılmıştır ve her adımın bir Jupyter not defteri yürütülür. Her not defterlerinde kullanmak için yerel olarak kalıcı veri yapıtı üretir.

### <a name="task-1-data-ingestion-and-preparation"></a>1. Görev: Veri alımı ve hazırlığı

Üç giriş veri kümeleri, veri alımı Jupyter not defteri Code/1_data_ingestion_and_preparation.ipnyb Pandas DataFrame biçimine yükler. Not defteri için modelleme verileri hazırlar ve bazı ön veri görselleştirme yapar. Veri kümeleri, Model oluşturma Jupyter not defteri kullanmak için işlem bağlamı için yerel olarak depolanır.

### <a name="task-2-model-building-and-evaluation"></a>Görev 2: Model oluşturma ve değerlendirme

Modeli oluşturma Jupyter not defteri Code/2_model_building_and_evaluation.ipnyb diskten eğitme ve test veri kümeleri okur ve bir eğitim veri kümesi için LSTM ağ derlemeler. Model performansını test veri kümesinde ölçülür. Sonuç olarak oluşan model serileştirilmiş ve kullanıma hazır hale getirme görev kullanmak için yerel işlem bağlamını depolanır.

### <a name="task-3-operationalization"></a>Görev 3: kullanıma hazır hale getirme

Code/3_operationalization.ipnyb kullanıma hazır hale getirme Jupyter Not Defteri, İşlevler ve modelin bir Azure'da barındırılan web hizmeti çağırmak için şema oluşturmak için saklı modeli kullanır. Not Defteri işlevleri test eder ve sonra varlıklar LSTM_o16n.zip dosyasına sıkıştırır. Dosya, dağıtım için Azure depolama kapsayıcısını açın yüklenir.

LSTM_o16n.zip dağıtım dosyası şu yapıtları içerir:

- **webservices_conda.yaml**: LSTM model dağıtım hedefi üzerinde çalıştırmak için gereken Python paketlerini tanımlar.  
- **service_schema.JSON**: LSTM modeli tarafından beklenen veri şemasını tanımlar.   
- **lstmscore.PY**: dağıtım hedefi yeni verileri puan çalıştıran işlevleri tanımlar.    
- **modellstm.JSON**: LSTM mimarisini tanımlar. Mimari ve model başlatmak için ağırlıkları lstmscore.py işlevleri okuyun.
- **modellstm.h5**: model ağırlıkları tanımlar.
- **test_service.PY**: dağıtım uç noktası ile test veri kayıtlarının çağıran bir test betiği. 
- **PM_test_files.pkl**: test_service.py betik PM_test_files.pkl dosyadan geçmiş altyapısı verileri okur ve yeterli bir altyapı hatası olasılığını döndürülecek LSTM döngülerini web hizmetine gönderir.

Not defterini işlevleri dağıtımı için kullanıma hazır hale getirme varlıkları paketleri önce model tanımı kullanarak test eder. Ayarlama ve web hizmeti test etmek için yönergeler Code/3_operationalization.ipnyb not defteri sonunda dahil edilir.

## <a name="conclusion"></a>Sonuç

Bu öğretici, tahminlerde bulunmak için algılayıcı değerlerini kullanan basit bir senaryo sağlar. Daha gelişmiş gibi Tahmine dayalı bakım senaryolarını [Tahmine dayalı bakım modelleme Kılavuzu R not defteri](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-R-Notebook-1) geçmiş bakım kayıtları, hata günlüklerini ve makine özellikleri gibi birçok veri kaynağına kullanabilirsiniz. Ek veri kaynakları, derin öğrenme ile kullanmak için farklı treatments gerektirebilir.


## <a name="references"></a>Başvurular

Aşağıdaki başvuruları, diğer Tahmine dayalı bakım örnekleri çeşitli platformlar için kullanım örnekleri sağlar:

* [Tahmine dayalı bakım çözüm şablonu](https://docs.microsoft.com/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance)
* [Tahmine dayalı bakım modelleme Kılavuzu](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
* [Tahmine dayalı bakım modelleme SQL R Services kullanarak Kılavuzu](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-Modeling-Guide-using-SQL-R-Services-1)
* [Tahmine dayalı Bakım Kılavuzu Python not defteri modelleme](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
* [PySpark kullanarak Tahmine dayalı bakım](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)
* [Tahmine dayalı bakım gerçek dünya senaryoları](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/scenario-predictive-maintenance)

## <a name="next-steps"></a>Sonraki adımlar

Machine Learning Workbench'te ürünün ek özelliklerini gösteren diğer örnek senaryolar kullanılabilir. 
