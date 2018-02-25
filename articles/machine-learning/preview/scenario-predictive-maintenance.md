---
title: "Gerçek dünya senaryoları için Tahmine dayalı bakım | Microsoft Docs"
description: "PySpark kullanarak gerçek dünya senaryoları için Tahmine dayalı bakım"
services: machine-learning
author: ehrlinger
ms.author: jehrling
manager: jhubbard
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.custom: mvc
ms.date: 10/05/2017
ms.openlocfilehash: 81e227194ff64d7b7af842a208349ccc63528ab8
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="predictive-maintenance-for-real-world-scenarios"></a>Gerçek dünya senaryoları için Tahmine dayalı bakım

Zamanlanmamış ekipman kapalı kalma süresi etkisi herhangi bir işletme için detrimental olabilir. Alan donanım kullanımını ve performansını en üst düzeye çıkarmak ve pahalı, zamanlanmamış kapalı kalma süresini en aza indirmek için çalıştıran tutmak önemlidir. Sorunları erken tanımlaması uygun maliyetli bir şekilde sınırlı bakım kaynakları tahsis ve kalitesini geliştirmek ve tedarik zinciri süreçlerinin yardımcı olabilir. 

Bu senaryoyu ele bir görece [büyük ölçekli benzetimli veri kümesi](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data) üzerinden Tahmine dayalı bakım veri bilimi proje veri alım yol için mühendislik, model yapı ve model operationalization özellik ve dağıtımı. Tüm işlem kodunu Jupyter not defteri Azure Machine Learning çalışma ekranı PySpark kullanarak yazılır. Son model, gerçek zamanlı donanım hatası tahminlerde için Azure Machine Learning modeli Yönetimi kullanılarak dağıtılır.   

### <a name="cortana-intelligence-gallery-github-repository"></a>Cortana Intelligence Galerisi GitHub deposunu

Cortana Intelligence Galerisi'nde PM öğretici için ortak GitHub deposudur ([https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance)) burada rapor sorunları ve olun katkı.


## <a name="use-case-overview"></a>Kullanım örneği'ne genel bakış

Varlık ağır sektörlerde işletmelerde tarafından karşılaştığı bir ana mekanik sorunlar nedeniyle gecikmeler ile ilişkili önemli maliyetleri sorunudur. Çoğu işletmeler, bu sorunları ortaya çıkmadan önce bunları önceden önlemek için doğabilecek olduğunda tahmin etmeye ilginizi çekiyor mu. Kapalı kalma süresini azaltarak maliyetleri azaltmak ve büyük olasılıkla güvenliği artırmak için belirtilir. 

Bu senaryo gelen fikirleri alır [Tahmine dayalı bakım playbook](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/cortana-analytics-playbook-predictive-maintenance) sanal bir veri kümesi için Tahmine dayalı bir model oluşturmak nasıl yapılacağını göstermek üzere. Örnek veri birçok Tahmine dayalı bakım kullanım durumlarında uyulması gereken ortak öğeler türetilir.

İş bu benzetimli veri tarafından bileşen hatalarına neden olduğu sorunları tahmin etmek için sorunudur. İş Soru "*bir bileşen hatası nedeniyle bir makine arıza olasılık nedir?*" Bu sorunu birden çok sınıf sınıflandırma sorunu (makine başına birden çok bileşeni) olarak biçimlendirilir. Makine öğrenme algoritmasını Tahmine dayalı bir model oluşturmak için kullanılır. Model makinelerden toplanan geçmiş verileri eğitildi. Bu senaryoda, kullanıcı Machine Learning çalışma ekranı ortamında model uygulamak için çeşitli adımları geçer.

## <a name="prerequisites"></a>Önkoşullar

* Bir [Azure hesabı](https://azure.microsoft.com/free/) (ücretsiz deneme kullanılabilir).
* Yüklü bir kopyasını [Azure Machine Learning çalışma ekranı](./overview-what-is-azure-ml.md). İzleyin [hızlı başlangıç Yükleme Kılavuzu](./quickstart-installation.md) programı yüklemek ve bir çalışma alanı oluşturmak için.
* Azure Machine Learning Operationalization yerel dağıtım ortamı gerektirir ve bir [Azure Machine Learning modeli yönetim hesabı](model-management-overview.md).

Bu örnekte her Machine Learning çalışma ekranı işlem bağlam üzerinde çalışır. Ancak, bu en az 16 GB bellek ile örneği çalıştırmak için önerilir. Bu senaryo yerleşik ve bir uzak DS4_V2 standart çalıştıran bir Windows 10 makineye test [Linux (Ubuntu) için veri bilimi sanal makine (DSVM)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu).

Model operationalization sürüm 0.1.0a22, Azure Machine Learning CLI kullanarak gerçekleştirilir.

## <a name="create-a-new-workbench-project"></a>Yeni bir çalışma ekranı projesi oluşturma

Bu örnek bir şablon kullanarak yeni bir proje oluşturun:
1.  Machine Learning çalışma ekranı açın.
2.  Üzerinde **projeleri** sayfasında,  **+** ve ardından **yeni proje**.
3.  İçinde **yeni proje oluştur** bölmesinde, yeni projeniz için bilgileri doldurun.
4.  İçinde **arama proje şablonları** arama kutusu, "Tahmine dayalı bakım" yazın ve seçin **Tahmine dayalı Bakım** şablonu.
5.  **Oluştur**’u seçin.

## <a name="prepare-the-notebook-server-computation-target"></a>Not Defteri sunucusu hesaplama hedef hazırlama

Machine Learning çalışma ekranından yerel makinenizde çalıştırmak için **dosya** menüsünde seçin **komut istemini açın** veya **açık PowerShell CLI**. CLI kullanarak Azure hizmetlerinizi erişmenize olanak sağlayan `az` komutları. İlk olarak, Azure komutu hesabınızla oturum açın:

```
az login
``` 

Bu komut ile https kullanmak için bir kimlik doğrulama anahtarı sağlar:\\aka.ms\devicelogin URL. CLI cihaz oturumu açma işlemi döndürür ve bazı bağlantı bilgilerini sağlar bekler. Yerel varsa sonraki [Docker](https://www.docker.com/get-docker) yükleme komutu ile yerel işlem ortamını hazırlayın:

```
az ml experiment prepare --target docker --run-configuration docker
```

Çalıştırmak için tercih edilir bir [DSVM Linux (Ubuntu) için](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu) bellek ve disk gereksinimleri için. DSVM yapılandırıldıktan sonra aşağıdaki iki komutu ile uzaktan Docker ortamını hazırlayın:

```
az ml computetarget attach remotedocker --name [Connection_Name] --address [VM_IP_Address] --username [VM_Username] --password [VM_UserPassword]
```

Uzak Docker kapsayıcısı bağlandıktan sonra komutu ile DSVM Docker bilgi işlem ortamı hazırlamalısınız: 

```
az ml experiment prepare --target [Connection_Name] --run-configuration [Connection_Name]
```

Hazırlanan Docker bilgi işlem ortamı ile Jupyter not defteri sunucunun Azure Machine Learning çalışma ekranından açmak **not defterlerini** sekmesinde veya tarayıcı tabanlı bir sunucuya komutu ile başlatın: 

```
az ml notebook start
```

Örnek not defterlerini kodu dizininde depolanır. Not defterlerini ardışık olarak çalıştırmak için ilk (Code\1_data_ingestion.ipynb) not defteri başlangıç ayarlanır. Her not defteri açtığınızda, işlem çekirdek seçin istenir. Önceden yapılandırılmış DSVM üzerinde yürütmek için [Project_Name] _şablon [Connection_Name] çekirdek seçin.

## <a name="data-description"></a>Veri açıklaması

[Benzetimli veri](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data) beş virgülle ayrılmış değerler (.csv) dosyaları içerir. Veri kümeleri hakkında ayrıntılı açıklamalar almak için aşağıdaki bağlantıları kullanın.

* [Makineler](https://pdmmodelingguide.blob.core.windows.net/pdmdata/machines.csv): yaş ve model gibi her bir makine ayırt özellikleri.
* [Hata](https://pdmmodelingguide.blob.core.windows.net/pdmdata/errors.csv): Makine hala durumdayken oluşturulan bölünemez hataları için hata günlüğünü içerir. Bunlar gelecekteki hata olayı Tahmine dayalı olabilir ancak bu hataları hataları, dikkate alınmaz. Telemetri verileri bir saatlik hızında toplanır beri hataları tarih-saat değerlerini en yakın saate yuvarlanır.
* [Bakım](https://pdmmodelingguide.blob.core.windows.net/pdmdata/maint.csv): Bakım günlüğü, her iki zamanlanmış ve zamanlanmamış bakım kayıtları içerir. Zamanlanmış bakım bileşenleri normal denetleme ile karşılık gelir. Zamanlanmamış bakım mekanik hatası veya başka bir performans düşüşü ortaya çıkabilir. Telemetri verileri bir saatlik hızında toplanır beri bakım tarih-saat değerlerini en yakın saate yuvarlanır.
* [Telemetri](https://pdmmodelingguide.blob.core.windows.net/pdmdata/telemetry.csv): telemetri verilerini her makine içinde birden çok algılayıcılar zaman serisi ölçümler oluşur. Veriler, her bir saat aralığı içinde algılayıcı değerlerini ortalayarak günlüğe kaydedilir.
* [Hataları](https://pdmmodelingguide.blob.core.windows.net/pdmdata/failures.csv): bileşen değişikliklerini bakım günlüğü içindeki hataları karşılık gelir. Her kayıt makine kimliği, bileşen türü ve değiştirme tarihi ve saati içerir. Bu kayıtlar makine öğrenimi modeli tahmin etmek için çalışıyor etiketleri oluşturmak için kullanılır.

Ham veri kümeleri Github'da depodan indirin ve bu çözümleme PySpark veri kümeleri oluşturmak için bkz: [veri alımı](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb) Jupyter not defteri senaryo kodu klasöründeki.

## <a name="scenario-structure"></a>Senaryo yapısı
Senaryo için içeriği şu adresten edinilebilir [GitHub deposunu](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance). 

[Benioku](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/README.md) dosya verileri hazırlama, bir model oluşturma ve üretim için bir çözüm dağıtma iş akışı özetlenmektedir. İş akışı her adımında bir Jupyter not defteri kapsüllenir [kod](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/tree/master/Code) klasör depo içinde.   

[Code\1_data_ingestion.ipynb](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb): Bu Not beş giriş .csv dosyalarını yükler ve bazı ön veri temizleme ve görselleştirme yapar. Not Defteri her bir veri kümesi PySpark biçimine dönüştürür ve özellik Mühendisliği not defteri kullanmak için bir Azure blob kapsayıcısındaki depolar.

[Code\2_feature_engineering.ipynb](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/2_feature_engineering.ipynb): modeli özelliklerini ham veri kümesinde Azure Blob depolama alanından telemetri, hataları ve Bakım verileri için bir Standart Saati serisi yaklaşım kullanarak oluşturulur. Bileşen hatası ilgili değiştirmeler hangi bileşenin başarısız açıklayan model etiketleri oluşturmak için kullanılır. Etiketli özelliği veri modeli oluşturma not defteri bir Azure blob kaydedilir.

[Code\3_model_building.ipynb](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/3_model_building.ipynb): Model oluşturma dizüstü etiketli özelliği veri kümesi kullanır ve tarih-saat damgası boyunca eğitimi ve geliştirme veri kümeleri veri ayıran. Not Defteri pyspark.ml.classification modellerle bir deney olarak ayarlanır. Eğitim verileri vectorized. Kullanıcı ile ya da deneyebilirsiniz bir **DecisionTreeClassifier** veya **RandomForestClassifier** modeli gerçekleştirmek için en iyi bulmak için hyperparameters işlemek için. Performans Ölçüm istatistikleri geliştirme veri kümesi üzerinde değerlendirerek belirlenir. Bu istatistikler izleme için Machine Learning çalışma ekranı çalışma ekranına doğru geri kaydedilir. Her çalışma, Not Defteri sonuç olarak oluşan model Jupyter not defteri çekirdek çalıştıran yerel diske kaydeder. 

[Code\4_operationalization.ipynb](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/4_operationalization.ipynb): Bu Not model bir Azure web hizmetine dağıtma bileşenleri oluşturmak için yerel (Jupyter not defteri çekirdek) diske kaydedilmiş son modelini kullanır. Tam işletimsel varlıklar, başka bir Azure blob kapsayıcısında depolanır o16n.zip dosyasına sıkıştırılmış. Sıkıştırılmış dosya içerir:

* **service_schema.JSON**: dağıtım için şema tanımı dosyası. 
* **pdmscore.PY**: **init()** ve **run()** web hizmeti Azure tarafından gerekli çalışır.
* **pdmrfull.model**: model tanım dizin.
    
Not Defteri dağıtım operationalization varlıklar paketleme önce model tanımını işlevleriyle sınar. Dağıtımı için yönergeler not defteri sonunda dahil edilir.

## <a name="conclusion"></a>Sonuç

Bu senaryo, Machine Learning çalışma ekranındaki Jupyter not defteri ortamında PySpark kullanarak bir uçtan uca Tahmine dayalı bakım çözümü oluşturma genel bir bakış sağlar. Bu örnek senaryo da Machine Learning modeli yönetim ortamı gerçek zamanlı donanım hatası tahminlerde modeli dağıtımıyla ayrıntılarını verir.

## <a name="references"></a>Başvurular

Aşağıdaki başvuru diğer Tahmine dayalı bakım örnekleri çeşitli platformlar için kullanım sağlar:

* [Tahmine dayalı bakım çözüm şablonu](https://docs.microsoft.com/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance)
* [Tahmine dayalı bakım modelleme Kılavuzu](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
* [Tahmine dayalı bakım modelleme SQL R Services kullanarak Kılavuzu](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-Modeling-Guide-using-SQL-R-Services-1)
* [Tahmine dayalı Bakım Kılavuzu Python not defteri modelleme](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
* [PySpark kullanarak Tahmine dayalı bakım](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)
* [Tahmine dayalı bakım için öğrenme derin](https://docs.microsoft.com/en-us/azure/machine-learning/preview/scenario-deep-learning-for-predictive-maintenance)

## <a name="next-steps"></a>Sonraki adımlar

Machine Learning çalışma içinde ekranı ürünün ek özellikleri gösteren kullanılabilir diğer örnek senaryolar verilmiştir. 
