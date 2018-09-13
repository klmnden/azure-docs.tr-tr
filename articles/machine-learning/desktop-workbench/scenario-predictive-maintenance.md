---
title: Gerçek dünya senaryoları için Tahmine dayalı bakım | Microsoft Docs
description: PySpark kullanarak gerçek dünya senaryoları için Tahmine dayalı bakım
services: machine-learning
author: ehrlinger
ms.author: jehrling
manager: jhubbard
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.custom: mvc
ms.date: 10/05/2017
ms.openlocfilehash: a5531ae256a263f1c34496819ac435ce67156b49
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35651041"
---
# <a name="predictive-maintenance-for-real-world-scenarios"></a>Gerçek dünya senaryoları için Tahmine dayalı bakım

Çoğu işletme için zamanlanmamış donanım kapalı kalma süresinin olumsuz etkileri olabilir. Kullanımı ve performansı en üst düzeye çıkarmak ve yüksek maliyetli ve zamanlanmamış kapalı kalma süresini en aza indirmek için çalışan alan ekipman tutmak önemlidir. Sorunları erken tanımlaması sınırlı bakım kaynakları uygun maliyetli bir şekilde ayırmanıza ve kalitesini artırmak ve tedarik zinciri işlemleri yardımcı olabilir. 

Bu senaryoyu ele bir görece [sanal büyük ölçekli veri kümesi](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data) veri alma bir Tahmine dayalı bakım veri bilimi proje geçmek, mühendislik, model oluşturma ve modeli kullanıma hazır hale getirme özelliğini ve Dağıtım. İşlem için kod, Azure Machine Learning Workbench'te PySpark kullanarak Jupyter not defteri yazılır. Son modelin gerçek zamanlı bir donanım hatası tahminlerde bulunmak üzere Azure Machine Learning Model Yönetimi kullanılarak dağıtılır.   

### <a name="cortana-intelligence-gallery-github-repository"></a>Cortana Intelligence Galerisi GitHub deposu

Cortana Intelligence Galerisi'nde PM öğreticisi için bir ortak GitHub deposu olduğu ([https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance)) burada rapor sorunları ve katkı.


## <a name="use-case-overview"></a>Kullanım örneği genel bakış

Mekanik sorunları nedeniyle gecikmeleri ile ilişkili maliyetleri varlık ağır sektörde şirketler tarafından karşılaşılan önemli bir sorun var. Çoğu işletmenin, bu sorunları ortaya çıkmadan önce proaktif bir şekilde önlemek için ortaya çıkabilir, tahmin etme ilgilenmemektedir. Kapalı kalma süresini azaltarak maliyetleri azaltmak ve büyük olasılıkla güvenliği artırmak için hedeftir. 

Bu senaryo fikirlerinden alan [Tahmine dayalı bakım playbook](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/cortana-analytics-playbook-predictive-maintenance) benzetimli bir veri kümesi için Tahmine dayalı bir model derleme işlemini göstermek için. Örnek veri birçok Tahmine dayalı bakım kullanım durumlarında gözlemlenen ortak öğeler türetilir.

İş sorununu bu sanal verileri için bileşeni hatalarının neden olduğu sorunları tahmin etmektir. İş Soru "*bir makine, bir bileşenin hatası nedeniyle arıza olasılığı nedir?*" Bu sorun, bir çok sınıflı sınıflandırma problemi (makine başına birden çok bileşen) olarak biçimlendirilir. Bir makine öğrenimi algoritmasının, Tahmine dayalı bir model oluşturmak için kullanılır. Model, makinelerden toplanan geçmiş veriler üzerinde çalıştırılır. Bu senaryoda, kullanıcı modeli Machine Learning Workbench ortamında uygulamak için çeşitli adımları geçer.

## <a name="prerequisites"></a>Önkoşullar

* Bir [Azure hesabı](https://azure.microsoft.com/free/) (ücretsiz denemeler kullanılabilir).
* Yüklü bir kopyasını [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md). İzleyin [Hızlı Yükleme Kılavuzu](../service/quickstart-installation.md) programı yüklemek ve bir çalışma alanı oluşturun.
* Azure Machine Learning operasyonel hale getirme, yerel dağıtım ortamı gerektirir ve bir [Azure Machine Learning Model Yönetimi hesabı](model-management-overview.md).

Bu örnek tüm Machine Learning Workbench işlem bağlam üzerinde çalıştırılır. Ancak, en az 16 GB bellekle örneği çalıştırmak için önerilir. Bu senaryoda oluşturulan ve bir uzak DS4_V2 standart çalıştıran bir Windows 10 makineye test [Linux (Ubuntu) için veri bilimi sanal makinesi (DSVM)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu).

Azure Machine Learning CLI'ın sürümü 0.1.0a22 kullanarak modeli kullanıma hazır hale getirme işlemi gerçekleştirildi.

## <a name="create-a-new-workbench-project"></a>Workbench yeni bir proje oluşturun

Bu örnek bir şablon kullanarak yeni bir proje oluşturun:
1.  Machine Learning Workbench’i açın.
2.  Üzerinde **projeleri** sayfasında **+** ve ardından **yeni proje**.
3.  İçinde **yeni proje oluştur** bölmesinde, yeni projeniz için bilgileri doldurun.
4.  İçinde **proje şablonlarında Ara** arama kutusuna "Tahmine dayalı bakım" yazın ve seçin **Tahmine dayalı Bakım** şablonu.
5.  **Oluştur**’u seçin.

## <a name="prepare-the-notebook-server-computation-target"></a>Not Defteri sunucusu hesaplama hedef hazırlama

Machine Learning Workbench'ten yerel makinenizde çalıştırılacak **dosya** menüsünde seçin **komut istemini Aç** veya **açık PowerShell CLI**. CLI kullanarak Azure hizmetlerinizi erişmenize olanak sağlayan `az` komutları. İlk olarak, bu komutla Azure hesabınızda oturum açın:

```
az login
``` 

Bu komut bir kimlik doğrulama anahtarı ile https kullanacak şekilde sağlar:\\aka.ms\devicelogin URL'si. CLI'yı cihaz oturum açma işlemi döndürür ve bazı bağlantı bilgilerini sağlar bekler. Yerel bir varsa sonraki [Docker](https://www.docker.com/get-docker) yükleme, komutu yerel işlem ortamını hazırlayın:

```
az ml experiment prepare --target docker --run-configuration docker
```

Çalıştırmak için tercih edilir bir [Linux (Ubuntu) için DSVM](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu) bellek ve disk gereksinimleri. DSVM yapılandırdıktan sonra aşağıdaki iki komutu ile uzak Docker ortamını hazırlayın:

```
az ml computetarget attach remotedocker --name [Connection_Name] --address [VM_IP_Address] --username [VM_Username] --password [VM_UserPassword]
```

Uzak Docker kapsayıcısına bağlandıktan sonra komutu DSVM Docker işlem ortamını hazırlayın: 

```
az ml experiment prepare --target [Connection_Name] --run-configuration [Connection_Name]
```

Hazırlanmış Docker işlem ortamı ile Azure Machine Learning Workbench'ten Jupyter notebook sunucusu açın **not defterlerini** sekmesinde veya tarayıcı tabanlı bir sunucuya komutla başlatın: 

```
az ml notebook start
```

Örnek Not Defterleri kodu dizininde depolanır. Not defterlerini ardışık olarak çalıştırmak için ilk (Code\1_data_ingestion.ipynb) not başlangıç ayarlanır. Her bir not defteri açtığınızda, hesaplama çekirdeğini seçin istenir. Önceden yapılandırılmış DSVM üzerinde yürütmek için [Project_Name] [Connection_Name] _şablon çekirdek seçin.

## <a name="data-description"></a>Veri açıklaması

[Simülasyonu veri](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data) beş virgülle ayrılmış değerler (.csv) dosyaları içerir. Veri kümeleri hakkında ayrıntılı açıklamaları almak için aşağıdaki bağlantıları kullanın.

* [Makineleri](https://pdmmodelingguide.blob.core.windows.net/pdmdata/machines.csv): yaş ve model gibi her bir makine ayırt özellikleri.
* [Hata](https://pdmmodelingguide.blob.core.windows.net/pdmdata/errors.csv): hata günlüğünü makine hala çalışır durumdayken oluşan hataya neden olmayan hatalar içeriyor. Gelecekteki bir hata olayı Tahmine dayalı olabilir ancak bu hataları hataları dikkate alınmaz. Saatlik ücretle toplanan telemetri verilerini bu yana hataları tarih ve saat değerlerini en yakın saate yuvarlanır.
* [Bakım](https://pdmmodelingguide.blob.core.windows.net/pdmdata/maint.csv): Bakım günlüğü hem zamanlanmış hem de zamanlanmamış bakım kayıtları içerir. Zamanlanan bakım bileşenlerinin normal İnceleme ile karşılık gelir. Zamanlanmamış bakım mekanik hata veya diğer performans düşüşü ortaya çıkabilir. Tarih ve saat değerleri için bakım, saatlik ücretle toplanan telemetri verilerini bu yana en yakın saate yuvarlanır.
* [Telemetri](https://pdmmodelingguide.blob.core.windows.net/pdmdata/telemetry.csv): zaman serisi ölçümleri her makine içinde birden çok sensörlerden alınan telemetri verilerini oluşur. Veriler, her bir saatlik zaman aralığında algılayıcı değerlerinin ortalamasını alarak günlüğe kaydedilir.
* [Hataları](https://pdmmodelingguide.blob.core.windows.net/pdmdata/failures.csv): Bakım günlüğü içinde bileşeni değişiklik hataları karşılık gelir. Her kayıt, makine kimliği, bileşen türü ve değiştirme tarihi ve saati içerir. Bu kayıtlar, makine öğrenimi modeli tahmin etmek için çalışıyor etiketleri oluşturmak için kullanılır.

Ham veri kümeleri GitHub deposundan indirin ve bu analiz için PySpark veri kümeleri oluşturmak için bkz: [veri alımı](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb) Jupyter not defteri senaryoda kod klasörü.

## <a name="scenario-structure"></a>Senaryo yapısı
Senaryo için içerik kullanılabilir [GitHub deposu](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance). 

[Benioku](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/README.md) dosya verileri hazırlama, bir model oluşturmak ve ardından üretim için bir çözüm dağıtma iş akışı açıklanır. İş akışı her adımında bir Jupyter Not Defteri içinde kapsüllenir [kod](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/tree/master/Code) klasör deponun içinde.   

[Code\1_data_ingestion.ipynb](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb): Bu Not beş giriş .csv dosyalarını indirir ve bazı ön veri temizleme ve görselleştirme yapar. Not Defteri, her bir veri kümesi PySpark biçimine dönüştürür ve özellik Mühendisliği not defterini kullanmak için bir Azure blob kapsayıcısındaki depolar.

[Code\2_feature_engineering.ipynb](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/2_feature_engineering.ipynb): model özellikleri ham veri kümesinde Azure Blob depolama alanından telemetri, hataları ve Bakım verileri Standart Saati serisi yaklaşımı kullanarak oluşturulur. Bileşen hatası ile ilgili değişiklik hangi bileşeni başarısız oldu açıklayan model etiketleri oluşturmak için kullanılır. Etiketli özelliği veri modeli oluşturma not defteri bir Azure blob içinde kaydedilir.

[Code\3_model_building.ipynb](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/3_model_building.ipynb): modeli yapı not defteri etiketli özelliği veri kümesi kullanır ve verileri tarih ve saat damgası boyunca eğitme ve geliştirme veri kümelerine ayırır. Not defterini pyspark.ml.classification modelleri ile deneme olarak ayarlanır. Eğitim verileri vektörleştirildi. Kullanıcı ile deneme yapabileceğiniz bir **DecisionTreeClassifier** veya **RandomForestClassifier** en iyi performansa sahip model bulmak için hiperparametreleri işlemek için. Performansı geliştirme veri kümesindeki Ölçüm istatistikleri değerlendirerek belirlenir. Bu istatistikler izleme için Machine Learning Workbench çalışma zamanı ekranına doğru geri kaydedilir. Her çalışma zamanında Not Defteri, sonuç olarak oluşan model Jupyter not defteri çekirdek çalıştıran yerel diske kaydeder. 

[Code\4_operationalization.ipynb](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/4_operationalization.ipynb): Bu not defterini bir Azure web hizmetinde modeli dağıtma için bileşenler oluşturmak için yerel (Jupyter not defteri çekirdek) diske kaydedilmiş son model kullanır. Tam işletimsel varlıkları, başka bir Azure blob kapsayıcısında depolanan o16n.zip dosyaya sıkıştırılamadı. Sıkıştırılmış dosya içeriyor:

* **service_schema.JSON**: dağıtım için şema tanımı dosyası. 
* **pdmscore.PY**: **init()** ve **uygulamasındaki run()** Azure tarafından gerekli işlevleri web hizmeti.
* **pdmrfull.model**: model tanımı dizini.
    
Not Defteri dağıtımı için kullanıma hazır hale getirme varlıkları paketleme önce model tanımı işlevleriyle sınar. Not defterini sonunda dağıtımı için yönergeler bulunur.

## <a name="conclusion"></a>Sonuç

Bu senaryo, Machine Learning workbench'te Jupyter not defteri ortamında PySpark kullanarak bir uçtan uca Tahmine dayalı bakım çözümü oluşturmaya genel bir bakış sağlar. Bu örnek senaryo, gerçek zamanlı bir donanım hatası tahminlerde bulunmak için Machine Learning Model Yönetimi ortamı üzerinden model dağıtımı da ayrıntıları.

## <a name="references"></a>Başvurular

Aşağıdaki başvuruları, diğer Tahmine dayalı bakım örnekleri çeşitli platformlar için kullanım örnekleri sağlar:

* [Tahmine dayalı bakım çözüm şablonu](https://docs.microsoft.com/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance)
* [Tahmine dayalı bakım modelleme Kılavuzu](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
* [Tahmine dayalı bakım modelleme SQL R Services kullanarak Kılavuzu](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-Modeling-Guide-using-SQL-R-Services-1)
* [Tahmine dayalı Bakım Kılavuzu Python not defteri modelleme](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
* [PySpark kullanarak Tahmine dayalı bakım](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)
* [Tahmine dayalı bakım için derin öğrenme](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/scenario-deep-learning-for-predictive-maintenance)

## <a name="next-steps"></a>Sonraki adımlar

Machine Learning Workbench'te ürünün ek özelliklerini gösteren diğer örnek senaryolar kullanılabilir. 
