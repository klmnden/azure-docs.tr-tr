---
title: "Tahmine dayalı bakım gerçek dünya senaryoları - Azure Öğrenme derin | Microsoft Docs"
description: "Bu belge, Azure Machine Learning çalışma ekranı ile Tahmine dayalı bakım öğretici için derin öğrenme çoğaltmak açıklar"
services: machine-learning
author: FrancescaLazzeri
ms.author: Lazzeri
manager: ireiter
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 11/22/2017
ms.openlocfilehash: 8997e38a81ed848c9e052ab08566ffc99560ed86
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2017
---
# <a name="deep-learning-for-predictive-maintenance-real-world-scenario"></a>Tahmine dayalı bakım gerçek dünya senaryosu için öğrenme derin

Derin öğrenme alanı günümüzde öğrenme makine en popüler eğilimler biridir ve birçok alanlar ve vardır uygulamaları Burada, çıkışı, driverless araba, konuşma ve görüntü tanıma, robotics ve Finans gibi anlamına gelir. Derin öğrenme bizim beyin (Biyolojik sinir ağları) şekil tarafından neden olacak bir algoritmaları kümesidir ve makine öğrenme ve bilişsel bilimcilerine genellikle yapay sinir ağları (ANN olarak) başvurduğu.

Tahmine dayalı bakım da burada birçok farklı teknikleri zaman bakım gerçekleştirileceğini tahmin etmek için donanım durumunu belirlemenize yardımcı olması için tasarlanmış bir çok popüler alanıdır. Tahmine dayalı bakım konuları dahil ancak bunlarla sınırlı olmamak üzere, çeşitli kapsar: hata tahmin, hatası tanılama (kök neden analizi), hata algılama, hata türü sınıflandırma ve öneri azaltma veya bakım eylemlerinin sonra hata oluştu.

Tahmine dayalı bakım senaryolarda hataları tahmin etmek için desenleri bulma son hedefi donanım durumunu izlemek için zaman içinde verileri toplanır. Yöntemleri, öğrenme derin arasında [uzun kısa vadeli bellek (LSTM)](http://colah.github.io/posts/2015-08-Understanding-LSTMs/) ağlarının olduğundan Tahmine dayalı bakım etki alanına özellikle çekici öğrenme dizilerinden en çok iyi due için nedeni. Bu olgu kendisini kullanarak hata desenleri algılamak için zaman serisi veri geri uzun süreyle aramak olası hale getirerek kullandıkları uygulamalar için uygundur.

Bu öğreticide, biz LSTM ağ veri kümesi için yapı ve senaryo açıklanan adresindeki [Tahmine dayalı Bakım](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3) uçak motorları kullanım ömrünü kalan tahmin etmek için. Özet olarak, böylece bakım önceden planlanması uçak motorunun gelecekte başarısız olduğunda tahmin etmek için benzetimli uçak algılayıcı değerlerini şablonu kullanır.

Bu öğretici kullanır [keras](https://keras.io/) Microsoft Bilişsel araç seti ile derin learning Kitaplığı [CNTK](https://docs.microsoft.com/en-us/cognitive-toolkit/Using-CNTK-with-Keras) arka uç olarak.

## <a name="link-to-the-gallery-github-repository"></a>Galeri GitHub deponuza bağlayın 

Genel GitHub depo bağlantısını aşağıdadır: [https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance)

## <a name="use-case-overview"></a>Kullanım örneği'ne genel bakış

Bu öğretici benzetimli uçak motoru çalıştırma hatası olaylarının örneği işlem modelleme Tahmine dayalı bakım göstermek için kullanır. Aşağıdaki gibi veri modelleme örtük ilgi varlık varlığın algılayıcı ölçüleri yansıtılan bir devam etme düşüşü düzenine sahip olduğunu varsayılır. Zaman içinde varlığın algılayıcı değerlerini inceleyerek makine öğrenme algoritmasını algılayıcı değerlerini ve hataları gelecekte öngörmek için geçmiş hatalarına algılayıcı değerlerini değişiklikleri arasındaki ilişkiyi öğrenebilirsiniz. Veri biçimi incelenerek ve verileri kendi değerlerinizle değiştirerek önce üç adımı şablonun giderek öneririz.

## <a name="prerequisites"></a>Ön koşullar

- Bir [Azure hesabı](https://azure.microsoft.com/free/) (ücretsiz deneme kullanılabilir).
- Azure Machine Learning çalışma ekranı yüklü bir kopyası ile oluşturulan bir çalışma alanı.
- Model operationalization için: yerel dağıtım ortamı kurulumu ile Azure Machine Learning Operationalization ve [model yönetim hesabı](https://docs.microsoft.com/en-us/azure/machine-learning/preview/model-management-overview)

## <a name="create-a-new-workbench-project"></a>Yeni bir çalışma ekranı projesi oluşturma

Bu örnek bir şablon kullanarak yeni bir proje oluşturun:

1. Açık Azure Machine Learning çalışma ekranı
2. Projeler sayfasında, tıklatın + oturum açın ve yeni bir proje seçin
3. Yeni Proje oluştur bölmesinde yeni projeniz için bilgileri girin
4. Arama proje şablonları arama kutusuna "Tahmine dayalı bakım" yazın ve şablonu seçin
5. Oluştur’a tıklayın

## <a name="prepare-the-notebook-server-computation-target"></a>Not Defteri sunucusu hesaplama hedef hazırlama

Yerel makinenizde AML çalışma ekranından çalıştırmak için `File` menüsünde seçin `Open Command Prompt` veya `Open PowerShell` CLI. CLI Windows'a aşağıdaki komutları çalıştırın:

`az ml experiment prepare --target docker --run-configuration docker`

 DS4_V2 standart üzerinde çalışan önerdiğimiz [Linux (Ubuntu) için veri bilimi sanal makine](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu). DSVM yapılandırıldıktan sonra aşağıdaki iki komutları çalıştırmanız gerekir:

`az ml computetarget attach remotedocker --name [Desired_Connection_Name] --address [VM_IP_Address] --username [VM_Username] --password [VM_UserPassword]`

`az ml experiment prepare --target [Desired_Connection_Name] --run-configuration [Desired_Connection_Name]`

Docker görüntülerle _hazırlanmış_, jupyter not defteri sunucunun içinde ya da açmak *AML çalışma ekranı* not defterlerini sekmesinde veya tarayıcı tabanlı bir sunucuya başlatmak için çalıştırın:`az ml notebook start`

- Not defterlerini depolanır `Code` directory Jupyter ortamında bulunamadı. Başlangıç sıralı olarak numaralandırılmış, bu dizüstü çalıştıracağımız (`Code\1_data_ingestionand_and_preparation.ipynb`).

- Çekirdek ayarlama [Desired_Connection_Name], [Project_Name] _şablon eşleşen tıklatıp çekirdek seçin

## <a name="data-description"></a>Veri açıklaması

Şablon girdi olarak üç veri kümeleri alır: "PM_train.txt", "PM_test.txt" ve "PM_truth.txt"
1. Eğitim verileri: Uçak motoru çalıştırma hatası verileri değil. Tren veri ("PM_train.txt") "döngü" zaman birimi olarak 21 sensör okumaları birlikte her döngüyü ile birden çok multivariate zaman serisi oluşur. Her zaman serisi aynı türde başka bir altyapısı oluşturulan olarak kabul edilebilir. Her motor ilk yıpranması ve değişim üretim derecelerde ile başlatmak için kabul edilir ve bu bilgileri kullanıcıya bilinmiyor. Bu sanal verilerde altyapısı her zaman serisi başlangıcında normal olarak çalışıyor olabilir varsayılır. Belirli bir noktada bir dizi işletim döngüsü sırasında düşmeye başlatır. Bozulması ilerler ve boyutları büyür. Önceden tanımlanmış bir Eşiğe ulaşıldığında altyapısı daha fazla işlem için güvenli olarak değerlendirilir. Diğer bir deyişle, her zaman serisi son döngüsünde karşılık gelen altyapısı hata noktası olarak kabul edilebilir.

2. Test verileri: veri kaydedilen hatası olaylarının işletim uçak motorunu olduğu. Test verileri ("PM_test.txt"), aynı veri şeması eğitim verileri olarak sahiptir. Yalnızca veri hatası oluştuğunda göstermez farktır (diğer bir deyişle, son süre hata noktası göstermiyor). Bu başarısız olmadan önce kaç tane daha fazla döngüleri bu altyapısı devam edebileceği bilinmiyor.

3. Gerçekte veri: true kalan döngüleri sınama verilerinde her motor için bilgiler içerir. Plan gerçekte veri sınama verilerinde altyapıları için kalan çalışma döngüsü sayısını sağlar.

## <a name="scenario-structure"></a>Senaryo yapısı

Senaryo için içerik [GitHub deposuna] mevcuttur (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance) 

Deposunda oluşturma ve faaliyete geçirmeye yönelik kadar verileri hazırlama alanından işlemleri özetlenmektedir [Benioku] (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance/blob/master/README.md) dosyası yok Model. Üç Jupyter not defterleri [kod] (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance/tree/master/Code) klasör depo içinde kullanılabilir. 

Sonraki adım adım senaryosu iş akışını açıklar:

### <a name="task-1-data-ingestion--preparation"></a>Görev 1: Veri alımı & hazırlama

Veri alımı Jupyter not defteri `Code/1_data_ingestion_and_preparation.ipnyb` yükleri üç giriş veri kümeleri halinde `Pandas` dataframe biçimi modelleme bölümü için veri hazırlar ve bazı ön veri görselleştirme yapar. Veri sonra dönüştürülür `PySpark` biçimlendirmek ve aboneliğinizi sonraki modelleme görevi kullanmak için bir Azure Blob Depolama kapsayıcısında depolanır.

### <a name="task-2-model-building--evaluation"></a>Görev 2: Model oluşturma ve değerlendirme

Model oluşturma Jupyter not defteri `Code/2_model_building_and_evaluation.ipnyb` okuyan `PySpark` eğitme ve test blob depolamadan veri kümeleri. Ardından LSTM ağ eğitim veri kümeleriyle yerleşik olarak bulunur. Model performansı test kümesinde ölçülür. Sonuç olarak oluşan model serileştirilmiş ve operationalization görev kullanmak için yerel işlem bağlamında depolanır.

### <a name="task-3-operationalization"></a>Görev 3: Operationalization

Jupyter not defteri operationalization içinde `Code/3_operationalization.ipnyb` web hizmeti saklı modeli ve derlemeleri gerekli işlevleri ve şeması model bir Azure üzerinde çağırmak için barındırılan alır. Not Defteri işlevleri sınar ve ayrıca Azure Blob Depolama kapsayıcınızı depolanan bir zip dosyasına operationalization varlıklar zips.
Sıkıştırılmış dosya içerir:

- `service_schema.json`Dağıtım için şema tanımı dosyası. 
- `lstmscore.py`Azure web hizmeti için gereken init() ve run() işlevleri
- `lstmfull.model`Model tanım dizin.

Not Defteri dağıtım operationalization varlıklar paketleme önce model tanımını işlevleriyle sınar. Dağıtımı için yönergeler not defteri sonunda dahil edilir.


## <a name="conclusion"></a>Sonuç

Bu öğretici Jupyter not defteri ortamında içinde Tahmine dayalı bakım etki alanındaki derin öğrenme uygulamak için arayan yeni başlayanlar için bir kılavuz olarak hizmet veren *Azure Machine Learning çalışma ekranı*. Bu öğretici, yalnızca bir veri kaynağı (algılayıcı değerlerini) tahminlerde için kullanıldığı basit bir senaryoyu kullanır. Daha gelişmiş senaryolarda Tahmine dayalı bakım gibi [Tahmine dayalı modelleme Kılavuzu Bakım](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-modeling-Guide-R-Notebook-1), birçok diğer veri kaynakları (yani geçmiş bakım kayıtları, hata günlüklerini, makine ve işleci özellikleri vb.) vardır; farklı türde ağları öğrenme ayrıntılı kullanılacak treatments gerektirebilir. Tahmine dayalı bakım derin öğrenme için tipik bir etki alanı olmadığından, kendi uygulama araştırma açık bir alandır.

## <a name="references"></a>Başvurular

- [Tahmine dayalı bakım çözüm şablonu](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/cortana-analytics-playbook-predictive-maintenance)
- [Tahmine dayalı bakım modelleme Kılavuzu](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-modeling-Guide-1)
- [Tahmine dayalı Bakım Kılavuzu Python not defteri modelleme](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-modeling-Guide-Python-Notebook-1)
- [PySpark kullanarak Tahmine dayalı bakım](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)

## <a name="future-directions-and-improvements"></a>Gelecekteki yönergeleri ve geliştirmeleri

Bu öğretici derin learning Tahmine dayalı bakım kullanma temelleri kapsar; birçok Tahmine dayalı bakım sorunları, genellikle bu etki alanında derin öğrenme uygularken dikkate alınması gereken veri kaynakları çeşitli içerir. Ayrıca, pencere boyutu gibi doğru parametreleri modellerini ayarlamak önemlidir. Okuyucu bu senaryonun ilgili bölümleri düzenleyin ve olanları açıklandığı gibi farklı bir sorun senaryo deneyin [Tahmine dayalı bakım modelleme Kılavuzu](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-modeling-Guide-1) birden çok diğer veri kaynakları söz konusu olduğu.
