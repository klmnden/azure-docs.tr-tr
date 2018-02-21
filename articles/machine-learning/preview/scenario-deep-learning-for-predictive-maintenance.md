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
ms.openlocfilehash: 7d4fe98b5c45767fb06391218e80789fc0c96a3b
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2018
---
# <a name="deep-learning-for-predictive-maintenance-real-world-scenario"></a>Tahmine dayalı bakım gerçek dünya senaryosu için öğrenme derin.

Derin öğrenme dahil olmak üzere birçok alanlara uygulamalarla machine learning en popüler eğilimler biridir:
- driverless araba ve robotics
- Konuşma ve görüntü tanıma
- Finansal tahmin.

Derin sinir ağları (DNN olarak) da bilinen bu yöntemleri (Biyolojik sinir ağları) beyin içinde tek tek neurons tarafından neden olacak.

Zamanlanmamış ekipman kapalı kalma süresi etkisi herhangi bir işletme için detrimental olabilir. Alan donanım kullanımını ve performansını en üst düzeye çıkarmak ve pahalı, zamanlanmamış kapalı kalma süresini en aza indirmek için çalışan tutmak önemlidir. Sorunları erken tanımlaması uygun maliyetli bir şekilde sınırlı bakım kaynakları tahsis ve kalitesini geliştirmek ve tedarik zinciri süreçlerinin yardımcı olabilir. 

Tahmine dayalı Bakım (PM) stratejisi makine öğrenme yöntemlerini erken önlem olumsuz makine performansı önlemek için bakım gerçekleştirmek için donanım durumunu belirlemek için kullanır. PM içinde makinenin durumunu izlemek için zaman içerisinde toplanır hataları tahmin etmek için kullanılan desenleri bulmak için çözümlemeler veridir. [Uzun kısa vadeli bellek (LSTM)](http://colah.github.io/posts/2015-08-Understanding-LSTMs/) ağlardır veri serilerinden öğrenmek için tasarlanmış beri bu ayarı için çekici.

## <a name="link-to-the-gallery-github-repository"></a>Galeri GitHub deponuza bağlayın

Sorun raporları ve katkı için ortak GitHub depo bağlantısını aşağıdadır: [https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance) 

## <a name="use-case-overview"></a>Kullanım örneği'ne genel bakış

Bu öğretici benzetimli uçak motoru çalıştırma hatası olaylarının örneği işlem modelleme Tahmine dayalı bakım göstermek için kullanır. Senaryo konusunda açıklandığı [Tahmine dayalı bakım](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3)

Bu ayarda ana varsayımına aşamalı olarak yaşam süresi boyunca düşürülmüş altyapısıdır. Düşüşü altyapısı algılayıcı ölçüleri algılanabilir. Bu algılayıcı değerlerini değişiklikleri ve geçmiş hataları arasındaki ilişkiyi modellemek PM çalışır. Model, ardından ne zaman ve altyapı algılayıcı ölçümleri geçerli durumuna göre gelecekte başlatılamayabilir tahmin edebilirsiniz.

Bu senaryo, uçak motorları geçmiş algılayıcı değerlerini kullanarak, kalan kullanım ömrü (RUL) tahmin etmek için bir LSTM ağ oluşturur. Kullanarak [Keras](https://keras.io/) ile [Tensorflow](https://www.tensorflow.org/) derin bir bilgi işlem altyapısı olarak framework öğrenme, senaryo motorları ve test ağ görünmeyen altyapısı kümesi üzerinde bir dizi LSTM eğitir.

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
5. Tıklatın **oluşturma** düğmesi

## <a name="prepare-the-notebook-server-computation-target"></a>Not Defteri sunucusu hesaplama hedef hazırlama

Yerel makinenizde AML çalışma ekranından çalıştırmak için `File` menüsünde seçin `Open Command Prompt` veya `Open PowerShell CLI`. CLI kullanarak Azure hizmetlerinizi erişmenize olanak sağlayan `az` komutları. İlk olarak, oturum açma komut ile Azure hesabınızı:

```
az login
``` 

Bu komut ile kullanılmak üzere bir kimlik doğrulama anahtarı sağlar `https:\\aka.ms\devicelogin` URL. CLI cihaz oturumu açma işlemi döndürür ve bazı bağlantı bilgilerini sağlar bekler. Yerel varsa sonraki [docker](https://www.docker.com/get-docker) yüklemek, aşağıdaki komutlarla yerel işlem ortamını hazırlayın:

```
az ml experiment prepare --target docker --run-configuration docker
```

Çalıştırmak için tercih edilir bir [Linux (Ubuntu) için veri bilimi sanal makine](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu) bellek ve disk gereksinimleri için. DSVM yapılandırıldıktan sonra aşağıdaki iki komutu ile uzaktan docker ortamını hazırlayın:

```
az ml computetarget attach remotedocker --name [Connection_Name] --address [VM_IP_Address] --username [VM_Username] --password [VM_UserPassword]
```

İçin Uzak docker kapsayıcısı bağlandıktan sonra DSVM hazırlama docker bilgi işlem ortamı kullanarak: 

```
az ml experiment prepare --target [Connection_Name] --run-configuration [Connection_Name]
```

Docker ile hazırlanmış ortamı işlem, Jupyter not defteri sunucunun AML çalışma ekranı not defterlerini sekme içindeki ya da açmak veya tarayıcı tabanlı bir sunucuyla Başlat: 
```
az ml notebook start
```

Örnek not defterlerini depolanmış `Code` dizin. Not defterlerini ardışık olarak çalıştırmak için ilk günü başlayan ayarlanır (`Code\1_data_ingestion.ipynb`) dizüstü bilgisayar. Her not defteri açtığınızda, işlem çekirdek seçmeniz istenir. Seçin `[Project_Name]_Template [Connection_Name]` üzerinde önceden yapılandırılmış DSVM yürütmek için çekirdek.

## <a name="data-description"></a>Veri açıklaması

Şablon dosyalarında girdi olarak üç veri kümelerini kullanır **PM_train.txt**, **PM_test.txt**, ve **PM_truth.txt**. 

-  **Eğitim verileri**: Uçak motoru çalıştırma hatası verileri. Tren veriler (PM_train.txt) ile birden çok multivariate zaman dizi oluşur *döngüsü* zaman birimi olarak. Bu, her döngü için 21 sensör okumaları içerir. 

    - Her zaman serisi aynı türde başka bir altyapısı oluşturulur. Her motor ilk yıpranması ve bazı benzersiz üretim değişim derecelerde ile başlar. Bu bilgiler kullanıcıya bilinmiyor. 

    - Bu sanal verilerde altyapısı her zaman serisi başlangıcında normal olarak çalışıyor olabilir varsayılır. Belirli bir noktada bir dizi işletim döngüsü sırasında düşmeye başlatır. Bozulması ilerler ve boyutları büyür. 

    - Önceden tanımlanmış bir Eşiğe ulaşıldığında altyapısı daha fazla işlem için güvenli olarak kabul edilir. Her zaman serisinin son döngüsünden bu altyapıyı hata noktasıdır.

-   **Test verileri**: kaydedilen hatası olaylarının işletim veri uçak motoru. Test verileri (PM_test.txt) aynı veri şeması eğitim verileri olarak sahiptir. Yalnızca veri hatası oluştuğunda göstermez farktır (son süre mu *değil* başarısızlık noktasını temsil eder). Bu başarısız olmadan önce kaç tane daha fazla döngüleri bu altyapısı devam edebileceği bilinmiyor.

- **Gerçekte veri**: her motor sınama verilerinde bilgilerin doğru kalan döngüleri. Plan gerçekte veri sınama verilerinde altyapıları için kalan çalışma döngüsü sayısını sağlar.

## <a name="scenario-structure"></a>Senaryo yapısı

Senaryosu iş akışı üç adımı bölünür, her bir Jupyter not defterlerinde yürütülür. Her dizüstü bilgisayar aşağıdaki not defterlerini kullanmak için yerel olarak kalıcı veri yapıları üretir: 

### <a name="task-1-data-ingestion-and-preparation"></a>Görev 1: Veri alımı ve hazırlık

Veri alımı Jupyter not defteri `Code/1_data_ingestion_and_preparation.ipnyb` Pandas veri çerçeve biçimine üç giriş veri kümeleri yükler. Daha sonra verileri modelleme için hazırlar ve bazı ön veri görselleştirme yapar. Veri kümelerini yerel model yapı dizüstü kullanmak için işlem bağlamı için depolanır.

### <a name="task-2-model-building-and-evaluation"></a>Görev 2: Model oluşturma ve değerlendirme

Model oluşturma Jupyter not defteri `Code/2_model_building_and_evaluation.ipnyb` eğitin ve test veri kümelerini diskten okur ve LSTM ağ eğitim veri kümesi oluşturur. Model performansı test kümesinde ölçülür. Sonuç olarak oluşan model serileştirilmiş ve operationalization görev kullanmak için yerel işlem bağlamında depolanır.

### <a name="task-3-operationalization"></a>Görev 3: Operationalization

Jupyter not defteri operationalization içinde `Code/3_operationalization.ipnyb` işlevleri ve şeması model bir Azure barındırılan web hizmetini çağırmak için yapı için saklı modelini kullanır. Not Defteri işlevleri sınar ve varlıkları içine sıkıştırır `LSTM_o16n.zip` dağıtımı için Azure depolama kapsayıcısı üzerine yüklenen dosya.

`LSTM_o16n.zip` Dağıtım dosyasını aşağıdaki yapılar içerir:

- `webservices_conda.yaml` Dağıtım hedefteki LSTM modeli çalıştırmak için gereken python paketlerini tanımlar.  
- `service_schema.json` LSTM modeli tarafından beklenen veri şeması tanımlar.     
- `lstmscore.py` Dağıtım hedefi puan yeni verileri çalıştıran işlevleri tanımlar.    
- `modellstm.json` LSTM mimari tanımlar. Lstmscore.py işlevleri mimarisi ve model başlatmak için ağırlıkları okuyun.
- `modellstm.h5` model ağırlıkları tanımlar.
- `test_service.py` Dağıtım uç noktası ile test veri kayıtlarını çağıran bir test komut dosyası. 
- `PM_test_files.pkl` `test_service.py` Komut dosyası geçmiş altyapısı verileri okur `PM_test_files.pkl` dosya ve motor arızasından olasılığını döndürülecek LSTM için yeterli döngüleri webservice gönderir.

Not Defteri operationalization varlıkları dağıtım için paketleri önce model tanımı kullanılarak işlevleri sınar. Ayarlama ve Web hizmeti test etme için yönergeler sonunda dahil `Code/3_operationalization.ipnyb` dizüstü bilgisayar.

## <a name="conclusion"></a>Sonuç

Bu öğretici tahminlerde için algılayıcı değerlerini kullanarak basit bir senaryoyu kullanır. Tahmine dayalı bakım senaryolarını gibi daha gelişmiş [Tahmine dayalı bakım modelleme Kılavuzu R not defteri](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-R-Notebook-1), geçmiş bakım kayıtları, hata günlükleri gibi birçok veri kaynaklarını kullanmak ve makine özellikleri. Ek veri kaynaklarını derin öğrenme ile kullanılmak üzere farklı treatments gerektirebilir.


## <a name="references"></a>Başvurular

Çeşitli platformlarda kullanılabilir diğer Tahmine dayalı bakım kullanım örneği örnekler vardır:

* [Tahmine dayalı bakım çözüm şablonu](https://docs.microsoft.com/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance)
* [Tahmine dayalı bakım modelleme Kılavuzu](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
* [Tahmine dayalı bakım modelleme SQL R Services kullanarak Kılavuzu](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-Modeling-Guide-using-SQL-R-Services-1)
* [Tahmine dayalı Bakım Kılavuzu Python not defteri modelleme](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
* [PySpark kullanarak Tahmine dayalı bakım](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)

* [Tahmine dayalı bakım gerçek dünya senaryosu](https://docs.microsoft.com/en-us/azure/machine-learning/preview/scenario-predictive-maintenance)

## <a name="next-steps"></a>Sonraki adımlar

Diğer birçok örnek senaryolar ürünün ek özelliklerini gösteren Azure Machine Learning çalışma ekranı içinde kullanılabilir. 