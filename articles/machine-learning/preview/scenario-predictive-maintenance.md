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
ms.openlocfilehash: 2687eb022bce0b71c217f0be611c8fabdfb66040
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="predictive-maintenance-real-world-scenario"></a>Tahmine dayalı bakım gerçek dünya senaryosu.

Zamanlanmamış ekipman kapalı kalma süresi etkisi herhangi bir işletme için detrimental olabilir. Bu nedenle, kullanımını ve performansını en üst düzeye çıkarmak için çalışan Canlı alan donanımına ve pahalı, zamanlanmamış kapalı kalma süresini en aza tarafından önemlidir. Sorunları erken tanımlaması uygun maliyetli bir şekilde sınırlı bakım kaynakları tahsis ve kalitesini geliştirmek ve tedarik zinciri süreçlerinin yardımcı olabilir. 

Bu senaryo için kullandığımız bir görece [büyük ölçekli veri](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data) veri alımı kullanıcıyı ana adımlarda size yol için mühendislik, model oluşturmanın ve ardından son olarak modeli operationalization ve dağıtım özelliği. Tüm işlem kodunu PySpark içinde yazılır ve Azure ML çalışma ekranının içinden Jupyter not defterleri kullanılarak uygulanan. En iyi modeli son hata tahminlerin gerçek zamanlı yapmak için bir üretim ortamında kullanmak için Azure Machine Learning modeli Management kullanarak kullanıma hazır hale getirilmiş.   

## <a name="link-to-the-gallery-github-repository"></a>Galeri GitHub deponuza bağlayın

Genel GitHub depo bağlantısını aşağıdadır: [https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance)


## <a name="use-case-overview"></a>Kullanım örneği'ne genel bakış

Varlık ağır sektörlerde işletmelerde tarafından karşılaştığı önemli sorun mekanik sorunları gecikmeleri ile ilişkili önemli maliyet oluşturur. Çoğu işletme ortaya çıkmadan önce bunları önceden önlemek için bu sorunları ortaya olduğunda tahmin etmeye ilginizi çekiyor mu. Kapalı kalma süresini azaltarak maliyetleri azaltmak ve büyük olasılıkla güvenliği artırmak için belirtilir. Başvurmak [playbook Tahmine dayalı Bakım](https://docs.microsoft.com/en-us/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance) ortak ayrıntılı bir açıklaması için Tahmine dayalı bakım için kullanılan modelleme yaklaşım yanı sıra durumlarda kullanın.

Bu senaryo, birden çok gerçek iş sorunlarını bir Birleştirici üzerinde dayalı bir senaryo için Tahmine dayalı bir modeli uygulamak adımları sağlamak amacıyla playbook gelen fikirleri yararlanır. Bu örnek, ortak veri öğeleri arasında birçok Tahmine dayalı bakım gözlenen kullanım araya getirir.

İş bu benzetimli veri sorunları tarafından bileşen hatalarına neden tahmin etmek için sorunudur. İş soru bu nedenle olan "*bir bileşen hatası nedeniyle bir makine arıza olasılık nedir*?" Bu sorun çok sınıflı sınıflandırma sorunu (makine başına birden çok bileşeni) olarak biçimlendirilmiş ve makine öğrenme algoritmasını Tahmine dayalı bir model oluşturmak için kullanılır. Model makinelerden toplanan geçmiş verileri eğitildi. Bu senaryoda, kullanıcı bu tür bir model Azure Machine Learning çalışma ekranı ortamında gerçekleştirilmesinin çeşitli adımları geçer.

## <a name="prerequisites"></a>Ön koşullar

* Bir [Azure hesabı](https://azure.microsoft.com/en-us/free/) (ücretsiz deneme kullanılabilir).
* Yüklü bir kopyasını [Azure Machine Learning çalışma ekranı](./overview-what-is-azure-ml.md) aşağıdaki [hızlı başlangıç Yükleme Kılavuzu](./quickstart-installation.md) programı yüklemek ve bir çalışma alanı oluşturmak için.
* Azure Machine Learning Operationalization yerel dağıtım ortamı gerektirir ve bir [model yönetim hesabı](https://docs.microsoft.com/en-us/azure/machine-learning/preview/model-management-overview)

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

Yerel makinenizde AML çalışma ekranından çalıştırmak için `File` menüsünde seçin `Open Command Prompt` veya `Open PowerShell CLI`. CLI Windows'a aşağıdaki komutları çalıştırın:

`az ml experiment prepare --target docker --run-configuration docker`

Linux (Ubuntu) için bir veri bilimi sanal makinede çalışan öneririz. DSVM yapılandırıldıktan sonra aşağıdaki iki komutu çalıştırın:

`az ml computetarget attach remotedocker --name [Desired_Connection_Name] --address [VM_IP_Address] --username [VM_Username] --password [VM_UserPassword]`

`az ml experiment prepare --target [Desired_Connection_Name] --run-configuration [Desired_Connection_Name]`

Hazırlanan docker görüntülerle Jupyter not defteri sunucu AML çalışma ekranı not defterlerini sekme içindeki ya da açın veya tarayıcı tabanlı bir sunucuya başlatmak için çalıştırın: `az ml notebook start`.

Not defterlerini depolanır `Code` Jupyter ortamında dizin. Biz not defterlerini sırayla, ilk başlatma çalışır (`Code\1_data_ingestion.ipynb`) dizüstü bilgisayar. Her not defteri açtığınızda, için işlem çekirdek seçmeniz istenir. [Project_Name] _şablon [Desired_Connection_Name] seçip çekirdek Ayarla seçeneğini tıklatın.

## <a name="data-description"></a>Veri açıklaması

[Benzetimli veri](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data) beş virgülle ayrılmış değerler (.csv) dosyaları içerir. Veri kümeleri için daha ayrıntılı bir açıklama daha fazla yararlanmak için bağlantıları izleyin.

* [Makineler](https://pdmmodelingguide.blob.core.windows.net/pdmdata/machines.csv): her makine ayrım özellikleri. Örneğin, geçerlilik süresi ve model.
* [Hata](https://pdmmodelingguide.blob.core.windows.net/pdmdata/errors.csv): hata günlüğünü makine hala durumdayken durum bölünemez hatalar içeriyor. Gelecekteki hata olayı Tahmine dayalı olabilir bu hataları hataları olarak kabul edilmez. Telemetri verileri bir saatlik hızında toplanır beri hata tarih-saat en yakın saate yuvarlanır.
* [Bakım](https://pdmmodelingguide.blob.core.windows.net/pdmdata/maint.csv): Bakım günlüğü, her iki zamanlanmış ve zamanlanmamış bakım kayıtları içerir. Zamanlanmış bakım bileşenleri normal incelenmesi karşılık gelen, zamanlanmamış bakım mekanik hatası veya başka bir performans düşüşü ortaya çıkabilir. Telemetri verileri bir saatlik hızında toplanır beri bakım tarih-saat en yakın saate yuvarlanır.
* [Telemetri](https://pdmmodelingguide.blob.core.windows.net/pdmdata/telemetry.csv): Voltaj, döndürme, baskısı ve titreşim algılayıcı ölçümleri gerçek zamanlı her makineden toplanan telemetri time series verilerini oluşur. Veriler bir saatten fazla ortalaması ve telemetri günlüklerde depolanır
* [Hataları](https://pdmmodelingguide.blob.core.windows.net/pdmdata/failures.csv): bileşen değişikliklerini bakım günlüğü içindeki hataları karşılık gelir. Her kayıt makine kimliği, bileşen türü ve değiştirme tarihi ve saati içerir. Bu kayıtlar makine öğrenimi modeli tahmin etmek için çalışıyor etiketleri oluşturmak için kullanılır.

Bkz: [veri alımı](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb) Jupyter not defteri senaryo ham veri kümeleri GitHub deposundan indirip Bu çözümleme PySpark veri kümeleri oluşturmak için kod bölümünde.

## <a name="scenario-structure"></a>Senaryo yapısı
Senaryo için içeriği şu adresten edinilebilir [GitHub deposunu](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance). 

Depoda yok bir [Benioku](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/README.md) birkaç model oluşturmaya ve ardından son olarak en iyi modelden birini faaliyete geçirmeye yönelik kadar verileri hazırlama alanından işlemleri özetlenmektedir dosya. Dört Jupyter not defterleri kullanılabilir [kod](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/tree/master/Code) klasör depo içinde.   

Sonraki adım adım senaryosu iş akışını açıklar. Uçtan uca senaryoyu PySpark içinde yazılmış ve dört not defterlerine ayrılır:

[`Code\1_data_ingestion.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb): Bu dizüstü bilgisayar beş giriş .csv dosyaları indirir, bazı ön veri temizleme ve görselleştirme yapar. Not defteri verileri PySpark biçime dönüştürür ve sonuçları bir Azure blob kapsayıcısı özellik Mühendisliği kullanmak için depolar.

[`Code\2_feature_engineering.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/2_feature_engineering.ipynb): Önceki adımı, gecikme ve toplanan özellikleri Temizlenen veri kümesini kullanarak telemetri algılayıcılar ve hata sayısını, bileşen donanımlarının oluşturulur, makine bilgilerini telemetri verilerini birleştirilir. Bileşen hatası ilgili değiştirmeler hangi bileşenin başarısız açıklayan etiketleri oluşturmak için kullanılır. Etiketli özellik verilerinin görev oluşturma modeli için bir Azure blob kaydedilir.

[`Code\3_model_building.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/3_model_building.ipynb): Etiketli özelliği veri kümesini kullanarak modelleme dizüstü bilgisayar eğitimi ve geliştirme veri kümeleri tarih-saat damgası boyunca verileri ayıran. Not Defteri ile Kurulum kümesi deneme olduğu `pyspark.ml.classification` modeller. Eğitim verileri vectorized ve kullanıcı ile ya da deneyebilirsiniz bir `DecisionTreeClassifier` veya `RandomForestClassifier`, model gerçekleştirmek için en iyi bulmak için hyperparameters düzenleme. Performansı geliştirme dataset ölçü istatistik değerlendirerek belirlenir. Bu istatistikler AML çalıştırmak çalışma ekranı zaman ekranına izleme için doğru geri kaydedilir. Her çalışma, Not Defteri sonuç olarak oluşan model Jupyter not defteri çekirdek çalıştıran yerel diske kaydeder. 

[`Code\4_operationalization.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/4_operationalization.ipynb): Son modelini kullanarak yerel (Jupyter not defteri çekirdek) diske kaydedilmiş, model bir Azure web hizmetinde faaliyete geçirmeye yönelik bileşenleri bu not oluşturur. Tam işletimsel varlıklar halinde düzenlenmiş `o16n.zip` dosya başka bir Azure blob kapsayıcısında depolanır. Sıkıştırılmış dosya içerir:

* `service_schema.json`Dağıtım için şema tanımı dosyası. 
* `pdmscore.py`Azure web hizmeti için gereken init() ve run() işlevleri
* `pdmrfull.model`Model tanım dizin.
    
 Not Defteri dağıtım operationalization varlıklar paketleme önce model tanımını işlevleriyle sınar. Dağıtımı için yönergeler not defteri sonunda dahil edilir.

## <a name="conclusion"></a>Sonuç

Bu senaryo okuyucu Azure ML çalışma ekranındaki Jupyter not defteri ortamında PySpark kullanarak bir uçtan uca Tahmine dayalı bakım çözümü oluşturmak nasıl bir bakış sağlar. Senaryo da okuyucu nasıl en iyi modeli kolayca operationalized ve gerçek zamanlı hatası tahminleri yapmak için bir üretim ortamında kullanmak için Azure Machine Learning modeli yönetim ortamı kullanılarak dağıtılan size rehberlik eder. Ardından okuyucu iş gereksinimlerine sivriltmek senaryonun ilgili bölümleri düzenleyebilirsiniz.  

## <a name="references"></a>Başvurular

Bu kullanım örneği daha önce birden çok platformu üzerinde geliştirilmiştir:

* [Tahmine dayalı bakım çözüm şablonu](https://docs.microsoft.com/en-us/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance)
* [Tahmine dayalı bakım modelleme Kılavuzu](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
* [Tahmine dayalı bakım modelleme SQL R Services kullanarak Kılavuzu](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-Modeling-Guide-using-SQL-R-Services-1)
* [Tahmine dayalı Bakım Kılavuzu Python not defteri modelleme](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
* [PySpark kullanarak Tahmine dayalı bakım](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)

# <a name="next-steps"></a>Sonraki adımlar

Diğer birçok örnek senaryolar ürünün ek özelliklerini gösteren Azure Machine Learning çalışma ekranı içinde kullanılabilir. 