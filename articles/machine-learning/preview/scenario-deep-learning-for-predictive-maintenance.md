---
title: "Tahmine dayalı bakım gerçek dünya senaryoları - Azure için ayrıntılı öğrenme | Microsoft Docs"
description: "Derin öğrenme öğretici Azure Machine Learning çalışma ekranı ile Tahmine dayalı bakım için çoğaltma öğrenin."
services: machine-learning
author: FrancescaLazzeri
ms.author: Lazzeri
manager: ireiter
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 11/22/2017
ms.openlocfilehash: a55209256c29fa62cc2da72f9653fbc7fc0e7c54
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="deep-learning-for-predictive-maintenance-real-world-scenarios"></a>Tahmine dayalı bakım gerçek dünya senaryoları için derin öğrenme

Derin öğrenme machine learning en popüler eğilimler biridir. Derin öğrenme birçok alanları ve driverless araba, konuşma ve görüntü tanıma, robotics ve Finans gibi uygulamalarda kullanılır. Derin öğrenme beyin (Biyolojik sinir ağları) ve makine öğrenme şeklini neden olacak algoritmaları kümesidir. Bilişsel bilimcilerine genellikle yapay sinir ağları (ANNs) olarak derin öğrenme bakın.

Tahmine dayalı bakım de yaygın olarak kullanılır. Tahmine dayalı bakım birçok farklı teknikleri donanım durumunu belirlemek ve bakımı yapılması gereken zaman tahmin etmek için tasarlanmıştır. Tahmine dayalı Bakım'ın bazı yaygın kullanımları hatası tahmin, hatası tanılama (kök neden analizi), hata algılama, hata türü sınıflandırma ve azaltma veya bakım Eylemler öneri hatasından sonra ' dir.

Tahmine dayalı bakım senaryolarda verileri donanım durumunu izlemek için zaman içerisinde toplanır. Tahmin etmek ve sonuçta hatalarını engellemeye yardımcı olabilir desenleri bulmak için belirtilir. Kullanarak [uzun kısa vadeli bellek (LSTM)](http://colah.github.io/posts/2015-08-Understanding-LSTMs/) Tahmine dayalı bakım özellikle çekici yöntemi öğrenme kapsamlı bir ağdır. LSTM ağlar öğrenme dizilerinden en iyi. Zaman serisi veri, hata desenlerini algılayabilir süreyi daha uzun aralıklarla geri aramak için kullanılabilir.

Bu öğreticide, biz LSTM ağ konumunda açıklanan senaryo ve veri kümesi için yapı [Tahmine dayalı Bakım](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3). Uçak motorları kalan kullanım ömrü tahmin etmek için ağ kullanırız. Şablonun sanal uçak algılayıcı değerlerini uçak motorunun gelecekte ne zaman başarısız olur tahmin etmek için kullanır. Bu tahmin kullanarak, önceden hatası önlemek için planlı Bakım.

Bu öğretici kullanır [Keras](https://keras.io/) derin kitaplığı ve Microsoft Bilişsel Araç Seti öğrenme [CNTK](https://docs.microsoft.com/cognitive-toolkit/Using-CNTK-with-Keras) bir arka uç olarak.

Bu öğretici için örnekleri olan bir genel GitHub depo altındadır [https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance).

## <a name="use-case-overview"></a>Kullanım örneği'ne genel bakış

Bu öğretici benzetimli uçak motoru çalıştırma hatası olaylarının örneği işlem modelleme Tahmine dayalı bakım göstermek için kullanır. 

Örtük burada açıklanan modelleme veri varlığı devam etme düşüşü düzeni olduğunu varsayılır. Desen varlığın algılayıcı ölçüleri yansıtılır. Zaman içinde varlığın algılayıcı değerlerini inceleyerek makine öğrenme algoritmasını algılayıcı değerlerini, algılayıcı değerlerini değişiklikleri ve geçmiş hataları arasındaki ilişkiyi öğrenebilirsiniz. Bu ilişki hataları gelecekte tahmin etmek için kullanılır. 

Veri biçimi inceleyin ve kendi tarihle örnek verileri değiştirmek önce şablonun üç adımı tamamlamanız öneririz.

## <a name="prerequisites"></a>Önkoşullar

- Bir [Azure hesabı](https://azure.microsoft.com/free/) (ücretsiz deneme kullanılabilir).
- Azure Machine Learning çalışma ekranı, oluşturulan bir çalışma alanıyla.
- Model operationalization için: Azure Machine Learning Operationalization kurma, bir yerel dağıtım ortamı ve bir [Azure Machine Learning modeli yönetim hesabı](https://docs.microsoft.com/azure/machine-learning/preview/model-management-overview).

## <a name="create-a-new-workbench-project"></a>Yeni bir çalışma ekranı projesi oluşturma

Bu örnek bir şablon kullanarak yeni bir proje oluşturun:

1. Machine Learning çalışma ekranı açın.
2. Üzerinde **projeleri** sayfasında,  **+** ve ardından **yeni proje**.
3. İçinde **yeni proje oluştur** bölmesinde, yeni projeniz için bilgileri girin.
4. İçinde **arama proje şablonları** arama kutusu, girin **Tahmine dayalı Bakım**ve ardından şablonu seçin.
5. **Oluştur**’u seçin.

## <a name="prepare-the-notebook-server-computation-target"></a>Not Defteri sunucusu hesaplama hedef hazırlama

Machine Learning çalışma ekranı üzerinde yerel bilgisayarınızda **dosya** menüsünde seçin **komut istemini açın** veya **açık PowerShell**. Seçtiğiniz seçeneği komut istemi penceresinde aşağıdaki komutları çalıştırın:

`az ml experiment prepare --target docker --run-configuration docker`

Not Defteri sunucu DS4_V2 standart üzerinde çalışan önerdiğimiz [Linux (Ubuntu) için veri bilimi sanal makine (DSVM)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu). Sonra DSVM yapılandırılır ve Docker resimler hazırlamak için aşağıdaki komutları çalıştırın:

`az ml computetarget attach remotedocker --name [connection_name] --address [VM_IP_address] --username [VM_username] --password [VM_password]`

`az ml experiment prepare --target [connection_name] --run-configuration [connection_name]`

Hazırlanan Docker görüntülerle Jupyter not defteri sunucunun açın. Jupyter not defteri sunucu açmak için ya da Machine Learning çalışma ekranına gidin **not defterlerini** sekmesinde veya tarayıcı tabanlı bir sunucuya Başlat:`az ml notebook start`

Not defterlerini Jupyter ortamı kodu dizininde depolanır. Code\1_data_ingestion_and_preparation.ipynb ile başlayarak bu dizüstü bilgisayarlar sıralı olarak numaralandırılmış, çalıştırın.

[Connection_name], [project_name] _şablon eşleşen ve daha sonra seçmek için çekirdek seçin **ayarlamak çekirdek**.

## <a name="data-description"></a>Veri açıklaması

Şablon üç veri kümeleri PM_train.txt, PM_test.txt ve PM_truth.txt dosyalarında giriş olarak kullanır.

-  **Eğitim verileri**: Uçak motoru çalıştırma hatası verileri. Tren veriler (PM_train.txt) ile birden çok multivariate zaman dizi oluşur *döngüsü* zaman birimi olarak. Bu, her döngü için 21 sensör okumaları içerir. 

    Her zaman serisi aynı türde başka bir altyapısı oluşturulacak varsayılabilir. Her motor ilk yıpranması ve değişim üretim derecelerde ile başlatmak için kabul edilir. Bu bilgiler kullanıcıya bilinmiyor. 

    Bu sanal verilerde altyapısı her zaman serisi başlangıcında normal olarak çalışıyor olabilir varsayılır. Belirli bir noktada bir dizi işletim döngüsü sırasında düşmeye başlatır. Bozulması ilerler ve boyutları büyür. 

    Önceden tanımlanmış bir Eşiğe ulaşıldığında altyapısı daha fazla işlem için güvenli olarak kabul edilir. Her zaman serisi son döngüsünde karşılık gelen altyapısı başarısızlık noktası kabul edilebilir.

-   **Test verileri**: kaydedilen hatası olaylarının işletim veri uçak motoru. Test verileri (PM_test.txt) aynı veri şeması eğitim verileri olarak sahiptir. Yalnızca veri hatası oluştuğunda göstermez farktır (son süre mu *değil* başarısızlık noktasını temsil eder). Bu başarısız olmadan önce kaç tane daha fazla döngüleri bu altyapısı devam edebileceği bilinmiyor.

- **Gerçekte veri**: her motor sınama verilerinde bilgilerin doğru kalan döngüleri. Plan gerçekte veri sınama verilerinde altyapıları için kalan çalışma döngüsü sayısını sağlar.

## <a name="scenario-structure"></a>Senaryo yapısı

Senaryo için içerik [GitHub deposunu] mevcuttur (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance). 

Depoda [Benioku] (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance/blob/master/README.md) dosyası oluşturma ve model faaliyete geçirmeye yönelik verileri hazırlama alanından işlemleri özetlenmektedir. Üç Jupyter not defterleri depo [kod] (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance/tree/master/Code) klasöründe kullanılabilir. 

Ardından, adım adım senaryosu iş akışını açıklar.

### <a name="task-1-data-ingestion-and-preparation"></a>Görev 1: Veri alımı ve hazırlık

Veri alımı Jupyter not defteri Code/1_data_ingestion_and_preparation.ipnyb üç giriş veri kümeleri Pandas dataframe biçimine yükler. Ardından, bu verileri modelleme için hazırlar ve bazı ön veri görselleştirme yapar. Veriler daha sonra PySpark biçimine dönüştürülen ve bir sonraki modelleme görevi kullanmak için Azure aboneliğinizin Azure Blob Depolama kapsayıcısında depolanır.

### <a name="task-2-model-building-and-evaluation"></a>Görev 2: Model oluşturma ve değerlendirme

Model oluşturma Jupyter not defteri Code/2_model_building_and_evaluation.ipnyb okuma PySpark eğitmek ve Blob depolamadan veri kümelerini test edin. Ardından, bir LSTM ağ eğitim veri kümeleriyle yerleşik olarak bulunur. Model performansı test kümesinde ölçülür. Sonuç olarak oluşan model serileştirilmiş ve operationalization görev kullanmak için yerel işlem bağlamında depolanır.

### <a name="task-3-operationalization"></a>Görev 3: Operationalization

Operationalization Code/3_operationalization.ipnyb Jupyter Not Defteri, işlevleri ve model bir Azure barındırılan web hizmetini çağırmak için gerekli olan şema oluşturmak için saklı modelini kullanır. Not Defteri işlevleri sınar ve (sıkıştırır) operationalization varlıklar içine bir .zip dosyası zips.

Sıkıştırılmış dosya bu dosya içerir:

- **modellstm.JSON**: dağıtım için şema tanımı dosyası. 
- **lstmscore.PY**: **init()** ve **run()** Azure web hizmeti için gereken işlevleri.
- **lstm.model**: model tanım dizin.

Not Defteri, dağıtım için operationalization varlıkların paketleri önce model tanım kullanarak işlevleri sınar. Dağıtımı için yönergeler not defteri sonunda dahil edilir.


## <a name="conclusion"></a>Sonuç

Bu öğretici, yalnızca bir veri kaynağı (algılayıcı değerlerini) tahminleri yapmak için kullanılan basit bir senaryoyu kullanır. Daha fazla bilgi için Tahmine dayalı bakım senaryolarını gibi gelişmiş [Tahmine dayalı bakım modelleme Kılavuzu R not defteri](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-R-Notebook-1), çok veri kaynağı için kullanılabilir. Diğer veri kaynakları, geçmiş bakım kayıtları, hata günlüklerini ve makine ve işleci özelliklerini içerebilir. Ek veri kaynaklarını derin öğrenme ağlarda kullanılacak treatments farklı türlerde gerektirebilir. Sağ parametre modellerini gibi pencere boyutunu ayarlamak önemlidir. 

Bu senaryonun ilgili bölümleri düzenleyin ve olanları açıklandığı gibi farklı bir sorun senaryoları deneyin [Tahmine dayalı bakım modelleme Kılavuzu](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1), birden fazla veri kaynağını içerir.


## <a name="references"></a>Başvurular

- [Tahmine dayalı bakım çözüm şablonu](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/cortana-analytics-playbook-predictive-maintenance)
- [Tahmine dayalı bakım modelleme Kılavuzu](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
- [Tahmine dayalı Bakım Kılavuzu Python not defteri modelleme](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
- [PySpark kullanarak Tahmine dayalı bakım](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)

