---
title: "Tahmine dayalı bakım gerçek dünya senaryoları | Microsoft Docs"
description: "Tahmine dayalı bakım gerçek dünya PySpark kullanarak senaryoları"
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
ms.openlocfilehash: 0299e73aecca3b3e5714b37c8b0b776ec8561e29
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="predictive-maintenance-real-world-scenario"></a>Tahmine dayalı bakım gerçek dünya senaryosu.

Zamanlanmamış ekipman kapalı kalma süresi etkisi herhangi bir işletme için detrimental olabilir. Bu nedenle, kullanımını ve performansını en üst düzeye çıkarmak için çalışan Canlı alan donanımına ve pahalı, zamanlanmamış kapalı kalma süresini en aza tarafından önemlidir. Sorunları erken tanımlaması uygun maliyetli bir şekilde sınırlı bakım kaynakları tahsis ve kalitesini geliştirmek ve tedarik zinciri süreçlerinin yardımcı olabilir. 

Bu senaryoyu ele bir görece [büyük ölçekli benzetimli veri kümesi](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data) üzerinden Tahmine dayalı bakım veri bilimi proje veri alım yol için mühendislik, model yapı ve model operationalization özellik ve dağıtımı. Tüm işlem kodunu Jupyter not defterleri Azure ML çalışma ekranının içinden PySpark kullanılarak yazılır. Son model, gerçek zamanlı donanım hatası tahminlerde için Azure Machine Learning modeli Yönetimi kullanılarak dağıtılır.   

## <a name="link-to-the-gallery-github-repository"></a>Galeri GitHub deponuza bağlayın

Genel GitHub depo bağlantısını aşağıdadır: [https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance)


## <a name="use-case-overview"></a>Kullanım örneği'ne genel bakış

Varlık ağır sektörlerde işletmelerde tarafından karşılaştığı önemli sorun mekanik sorunları gecikmeleri ile ilişkili önemli maliyet oluşturur. Çoğu işletme ortaya çıkmadan önce bunları önceden önlemek için bu sorunları ortaya olduğunda tahmin etmeye ilginizi çekiyor mu. Kapalı kalma süresini azaltarak maliyetleri azaltmak ve büyük olasılıkla güvenliği artırmak için belirtilir. 

Bu senaryo gelen fikirleri alır [Tahmine dayalı bakım playbook](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/cortana-analytics-playbook-predictive-maintenance) sanal bir veri kümesi için Tahmine dayalı bir model oluşturma göstermek için. Örnek veri birçok Tahmine dayalı bakım kullanım durumlarında gözlenen ortak öğeler türetilir.

İş bu benzetimli veri sorunları tarafından bileşen hatalarına neden tahmin etmek için sorunudur. İş soru bu nedenle olan "*bir bileşen hatası nedeniyle bir makine arıza olasılık nedir*?" Bu sorun çok sınıflı sınıflandırma sorunu (makine başına birden çok bileşeni) olarak biçimlendirilmiş ve makine öğrenme algoritmasını Tahmine dayalı bir model oluşturmak için kullanılır. Model makinelerden toplanan geçmiş verileri eğitildi. Bu senaryoda, kullanıcı bu tür bir model Azure Machine Learning çalışma ekranı ortamında gerçekleştirilmesinin çeşitli adımları geçer.

## <a name="prerequisites"></a>Önkoşullar

* Bir [Azure hesabı](https://azure.microsoft.com/en-us/free/) (ücretsiz deneme kullanılabilir).
* Yüklü bir kopyasını [Azure Machine Learning çalışma ekranı](./overview-what-is-azure-ml.md) aşağıdaki [hızlı başlangıç Yükleme Kılavuzu'na](./quickstart-installation.md) programı yüklemek ve bir çalışma alanı oluşturmak için.
* Azure Machine Learning Operationalization yerel dağıtım ortamı gerektirir ve bir [model yönetim hesabı](https://docs.microsoft.com/azure/machine-learning/preview/model-management-overview)

Bu örnekte her AML çalışma ekranı işlem bağlam üzerinde çalıştırılabilir. Ancak, ile en az çalıştırmak için önerilir 16 GB bellek. Bu senaryo yerleşik ve bir uzak DS4_V2 standart çalıştıran bir Windows 10 makineye test [Linux (Ubuntu) için veri bilimi sanal makine](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu).

Model operationalization yapıldı, Azure ML CLI Sürüm 0.1.0a22 kullanarak.

## <a name="create-a-new-workbench-project"></a>Yeni bir çalışma ekranı projesi oluşturma

Bu örnek bir şablon kullanarak yeni bir proje oluşturun:
1.  Açık Azure Machine Learning çalışma ekranı
2.  Üzerinde **projeleri** sayfasında,  **+**  oturum ve seçin **yeni proje**
3.  İçinde **yeni proje oluştur** bölmesinde, yeni projeniz için bilgileri doldurun
4.  İçinde **arama proje şablonları** arama kutusu, "Tahmine dayalı bakım" yazın ve şablonu seçin
5.  **Oluştur**'a tıklayın

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

[Benzetimli veri](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data) beş virgülle ayrılmış değerler (.csv) dosyaları içerir. Veri kümeleri için daha ayrıntılı bir açıklama daha fazla yararlanmak için bağlantıları izleyin.

* [Makineler](https://pdmmodelingguide.blob.core.windows.net/pdmdata/machines.csv): her makine ayrım özellikleri. Örneğin, geçerlilik süresi ve model.
* [Hata](https://pdmmodelingguide.blob.core.windows.net/pdmdata/errors.csv): hata günlüğünü makine hala durumdayken durum bölünemez hatalar içeriyor. Gelecekteki hata olayı Tahmine dayalı olabilir bu hataları hataları olarak kabul edilmez. Telemetri verileri bir saatlik hızında toplanır beri hata tarih-saat en yakın saate yuvarlanır.
* [Bakım](https://pdmmodelingguide.blob.core.windows.net/pdmdata/maint.csv): Bakım günlüğü, her iki zamanlanmış ve zamanlanmamış bakım kayıtları içerir. Zamanlanmış bakım bileşenleri normal incelenmesi karşılık gelen, zamanlanmamış bakım mekanik hatası veya başka bir performans düşüşü ortaya çıkabilir. Telemetri verileri bir saatlik hızında toplanır beri bakım tarih-saat en yakın saate yuvarlanır.
* [Telemetri](https://pdmmodelingguide.blob.core.windows.net/pdmdata/telemetry.csv): veri oluşur zaman serisi telemetri ölçer her makine içinde birden çok algılayıcılar gelen. Veriler, her bir saat aralığı içinde algılayıcı değerlerini ortalayarak günlüğe kaydedilir.
* [Hataları](https://pdmmodelingguide.blob.core.windows.net/pdmdata/failures.csv): bileşen değişikliklerini bakım günlüğü içindeki hataları karşılık gelir. Her kayıt makine kimliği, bileşen türü ve değiştirme tarihi ve saati içerir. Bu kayıtlar makine öğrenimi modeli tahmin etmek için çalışıyor etiketleri oluşturmak için kullanılır.

Bkz: [veri alımı](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb) Jupyter not defteri senaryo ham veri kümeleri GitHub deposundan indirip Bu çözümleme PySpark veri kümeleri oluşturmak için kod bölümünde.

## <a name="scenario-structure"></a>Senaryo yapısı
Senaryo için içeriği şu adresten edinilebilir [GitHub deposunu](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance). 

[Benioku](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/README.md) dosya verileri hazırlama, bir model oluşturma ve üretim için bir çözüm dağıtma iş akışı özetlenmektedir. Her adım iş akışının bir Jupyter not defteri kapsüllenir [kod](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/tree/master/Code) klasör depo içinde.   

[`Code\1_data_ingestion.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb): Bu dizüstü bilgisayar beş giriş .csv dosyaları indirir, bazı ön veri temizleme ve görselleştirme yapar. Not Defteri, her bir veri kümesi PySpark biçimine dönüştürür ve özellik Mühendisliği not defteri kullanmak için bir Azure blob kapsayıcısına depolar.

[`Code\2_feature_engineering.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/2_feature_engineering.ipynb): Azure blob, telemetri, hataları ve Bakım verileri için Standart Saati serisi yaklaşım kullanarak özellikleri oluşturulan model ham kümesinden kullanma. Bileşen hatası ilgili değiştirmeler hangi bileşenin başarısız açıklayan model etiketleri oluşturmak için kullanılır. Etiketli özellik verilerinin not defteri oluşturma modeli için bir Azure blob kaydedilir.

[`Code\3_model_building.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/3_model_building.ipynb): Etiketli özelliği veri kümesini kullanarak modelleme dizüstü bilgisayar eğitimi ve geliştirme veri kümeleri tarih-saat damgası boyunca verileri ayıran. Not Defteri ile Kurulum kümesi deneme olduğu `pyspark.ml.classification` modeller. Eğitim verileri vectorized ve kullanıcı ile ya da deneyebilirsiniz bir `DecisionTreeClassifier` veya `RandomForestClassifier`, model gerçekleştirmek için en iyi bulmak için hyperparameters düzenleme. Performansı geliştirme dataset ölçü istatistik değerlendirerek belirlenir. Bu istatistikler AML çalıştırmak çalışma ekranı zaman ekranına izleme için doğru geri kaydedilir. Her çalışma, Not Defteri sonuç olarak oluşan model Jupyter not defteri çekirdek çalıştıran yerel diske kaydeder. 

[`Code\4_operationalization.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/4_operationalization.ipynb): Yerel (Jupyter not defteri çekirdek) diske kaydedilmiş son modelini kullanarak, model bir Azure web hizmetine dağıtma bileşenleri bu not oluşturur. Tam işletimsel varlıklar halinde düzenlenmiş `o16n.zip` dosya başka bir Azure blob kapsayıcısında depolanır. Sıkıştırılmış dosya içerir:

* `service_schema.json`Dağıtım için şema tanımı dosyası. 
* `pdmscore.py`Azure web hizmeti için gereken init() ve run() işlevleri
* `pdmrfull.model`Model tanım dizin.
    
 Not Defteri dağıtım operationalization varlıklar paketleme önce model tanımını işlevleriyle sınar. Dağıtımı için yönergeler not defteri sonunda dahil edilir.

## <a name="conclusion"></a>Sonuç

Bu senaryo okuyucu Azure ML çalışma ekranındaki Jupyter not defteri ortamında PySpark kullanarak bir uçtan uca Tahmine dayalı bakım çözümü oluşturmak nasıl bir bakış sağlar. Bu örnek senaryo da Azure Machine Learning modeli yönetim ortamı donanım hatası tahminlerin gerçek zamanlı etkinleştirmek için model dağıtımıyla ayrıntılarını verir.

## <a name="references"></a>Başvurular

Bu kullanım örneği daha önce birden çok platformu üzerinde geliştirilmiştir:

* [Tahmine dayalı bakım çözüm şablonu](https://docs.microsoft.com/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance)
* [Tahmine dayalı bakım modelleme Kılavuzu](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
* [Tahmine dayalı bakım modelleme SQL R Services kullanarak Kılavuzu](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-Modeling-Guide-using-SQL-R-Services-1)
* [Tahmine dayalı Bakım Kılavuzu Python not defteri modelleme](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
* [PySpark kullanarak Tahmine dayalı bakım](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)

## <a name="next-steps"></a>Sonraki adımlar

Diğer birçok örnek senaryolar ürünün ek özelliklerini gösteren Azure Machine Learning çalışma ekranı içinde kullanılabilir. 