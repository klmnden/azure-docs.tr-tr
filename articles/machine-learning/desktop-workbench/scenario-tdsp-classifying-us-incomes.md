---
title: Gelir sınıflandırma - takım veri bilimi işlem - Azure Machine Learning | Microsoft Docs
description: Azure Machine Learning ile ABD yüksek sınıflandırır bir proje oluşturmak için takım veri bilimi işlem şablonunu kullanma
services: machine-learning
documentationcenter: ''
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2017
ms.author: bradsev
ms.openlocfilehash: 48c88f541f650fac3bdec431f3164138fb0f3205
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="income-classification-with-team-data-science-process-tdsp-project"></a>Takım veri bilimi işlem (TDSP) proje ile gelir sınıflandırma

## <a name="introduction"></a>Giriş

Standartlaştırma yapısı ve veri bilimi belgelerine projeleri, yani kurulmuş bir bağlantılı [veri bilimi yaşam döngüsü](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md), veri bilimi ekipleri etkili işbirliği kolaylaştırmanın için anahtarıdır. Azure Machine Learning projelerle oluşturma [takım veri bilimi işlem (TDSP)](https://github.com/Azure/Microsoft-TDSP) şablon gibi Standartlaştırma için bir çerçeve sağlar.

Daha önce yayımladık bir [TDSP proje yapısını ve şablonları için GitHub depo](https://github.com/Azure/Azure-TDSP-ProjectTemplate). Ancak şimdi TDSP yapısı ve şablonlar içindeki bir veri bilimi aracı örneği oluşturmak olası kadar değildi. Biz şimdi ile örneği Azure Machine Learning projeleri oluşturulmasını etkinleştirdiyseniz [TDSP yapısı ve belgeleri şablonları Azure Machine Learning için](https://github.com/amlsamples/tdsp). Azure Machine Learning TDSP yapısı ve şablonları kullanma konusunda yönergeler sağlanır [burada](https://aka.ms/how-to-use-tdsp-in-aml). Burada nasıl gerçek makine öğrenme projesinde TDSP yapısını kullanarak oluşturulabilir, yapılar ve belgeler gibi projeye özgü kodu ile doldurulur ve Azure Machine Learning içinde yürütülen bir örnek sağlar.

## <a name="link-to-github-repository"></a>GitHub deponuza bağlayın
Özet belgelerine sağladığımız [burada](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) örnek hakkında. Daha kapsamlı belgeler GitHub sitesinde bulunabilir.

### <a name="purpose"></a>Amaç
Bu örnek birincil amacı, örneği ve machine learning kullanarak projesi yürütmek nasıl göstermektir [takım veri bilimi işlem (TDSP)](https://github.com/Azure/Microsoft-TDSP) yapısı ve Azure Machine Learning şablonlarında. Bu amaç için iyi bilinen kullanırız [1994 BİZE Census veri UCI Machine Learning depodan](https://archive.ics.uci.edu/ml/datasets/adult). Modelleme ABD yıllık geliri sınıflardan BİZE Census bilgileri (örneğin, geçerlilik süresi, yarış, eğitim düzeyi, ülke / bölge kaynağını, vb.) tahmin etmek için bir görevdir

### <a name="scope"></a>Kapsam
 * Veri keşfi, eğitim ve kullanım durumu genel bakış içinde açıklanan tahmin soruna yönelik bir makine öğrenimi modeline dağıtımı. 
 * Bu proje için Azure Machine Learning ekibi veri bilimi işlem (TDSP) şablonu kullanarak Azure Machine Learning projesinde yürütme. Proje yürütme ve raporlama için TDSP yaşam döngüsü kullanmayı yapacağız.
 * Azure Machine Learning Azure kapsayıcı Services'de çözümden doğrudan operationalization.

 Proje çeşitli özelliklerini Azure Machine Learning, bu tür TDSP yapısı örneklemesi ve kullanımı, Python dosyaların yanı sıra Jupyter not defterleri kodda ve Docker ve Kubernetes kullanarak Azure kapsayıcı Services içinde kolay operationalization yürütülmesini vurgular.


## <a name="team-data-science-process-tdsp-lifecycle"></a>Takım veri bilimi işlem (TDSP) yaşam döngüsü
Bkz: [ekip veri bilimi işlem (TDSP) yaşam döngüsü](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md)

![](./media/scenario-tdsp-classifying-us-incomes/tdsp-lifecycle.jpg)

## <a name="prerequisites"></a>Önkoşullar
### <a name="required-subscription-hardware-software"></a>Gerekli: Abonelik, donanım, yazılım
1. Bir Azure [abonelik](https://azure.microsoft.com). Alabileceğiniz bir [ücretsiz abonelik](https://azure.microsoft.com/free/?v=17.16&WT.srch=1&WT.mc_id=AID559320_SEM_cZGgGOIg) Bu örnek ayrıca yürütülecek.
2. Bir [Azure veri bilimi sanal makine (DSVM) Windows Server 2016](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.windows-data-science-vm), (VM boyutu: [DS3_V2](https://docs.microsoft.com/azure/virtual-machines/windows/sizes)4 sanal CPU ve 14 Gb RAM). Bir Azure DSVM test karşın, herhangi bir Windows 10 makinede çalışması olasıdır.
3. Azure Machine Learning ve kendi ilgili belgeleri gözden Hizmetleri (bağlantılar için aşağıya bakın).
4. Azure Machine Learning tarafından düzgün yüklenmiş olduğundan emin olun [hızlı başlangıç Yükleme Kılavuzu](../service/quickstart-installation.md).

Bu örnek için veri kümesi UCI ML depodur [[bağlantı]](https://archive.ics.uci.edu/ml/datasets/adult). 1994 BİZE Census veritabanından alınır ve yaklaşık 50.000 kişiler için census ve geliri bilgi içerir. Bu sayısal sahip yapılandırılmış veri kümesi, kategorik özellikleri ve iki gelir kategorisi oluşan bir kategorik hedefi olan ('> 50 K' veya ' < 50 K ='). 

### <a name="optional-version-control-repository"></a>İsteğe bağlı: Sürüm denetim deposu
Kaydetmek istiyorsanız ve sürüm projenizi ve içeriğini, burada bu yapılabilir bir sürüm denetim deposu olması gerekir. Azure Machine Learning TDSP şablonunu kullanarak yeni proje oluşturulurken Git deposu konumu girebilirsiniz. Bkz: [Git Azure Machine Learning kullanma](using-git-ml-project.md) daha ayrıntılı bilgi için.

### <a name="informational-about-azure-machine-learning"></a>Bilgi amaçlı: Azure Machine Learning hakkında
* [SSS - nasıl başlayacağınızı](frequently-asked-questions.md)
* [Genel Bakış](../service/overview-what-is-azure-ml.md)
* [Yükleme](../service/quickstart-installation.md)
* [Yürütme](experimentation-service-configuration.md)
* [TDSP kullanma](https://aka.ms/how-to-use-tdsp-in-aml)
* [Dosyaları okuma ve yazma](how-to-read-write-files.md)
* [Azure Machine Learning ile Git kullanma](using-git-ml-project.md)
* [ML modelinde bir web hizmeti olarak dağıtma](model-management-service-deploy.md)

### <a name="create-a-new-workbench-project"></a>Yeni bir çalışma ekranı projesi oluşturma

Bu örnek bir şablon kullanarak yeni bir proje oluşturun:
1.  Açık Azure Machine Learning çalışma ekranı
2.  Üzerinde **projeleri** sayfasında, **+** oturum ve seçin **yeni proje**
3.  İçinde **yeni proje oluştur** bölmesinde, yeni projeniz için bilgileri doldurun
4.  İçinde **arama proje şablonları** arama kutusu, "Sınıflandır ABD gelirler - TDSP proje" yazın ve şablonu seçin
5.  **Oluştur**'a tıklayın

Boş bir Git deposu konumu (uygun kutusunda) proje oluşturma sırasında sağlarsanız, ardından bu depoyu Proje yapısı ve içeriği ile Proje oluşturulduktan sonra doldurulur.

## <a name="use-case-overview"></a>Kullanım örneği'ne genel bakış
BİZE Census yakalanan nasıl socio ekonomik verileri anlamak için ABD bireylerin yıllık geliri tahmin etmeye yardımcı olması sorunudur. Bu tür Census özelliklerini temel alarak, tek bir geliri başına 50.000 olup olmadığını tahmin etmek için makine öğrenme görev, (ikili sınıflandırma görev).

## <a name="data-description"></a>Veri açıklaması
Veriler hakkında ayrıntılı bilgi için bkz: [açıklama](https://archive.ics.uci.edu/ml/machine-learning-databases/adult/adult.names) UCI deposunda. 

Bu verilerin bulunduğu Census kuruluşu veritabanından ayıklandı: https://www.census.gov/en.html. 


* (Önce hiçbir filtreleme) 48,842 örnekleri toplam, sürekli ve ayrık karışımı (eğitmek = 32, 561, test = 16, 281)
* Etiketin olasılık ' > 50 K': %23.93 / %24.78 (olmadan bilinmeyenli iki denklemi)
* Etiketin olasılık ' < 50 K =': %76.07 / %75.22 (olmadan bilinmeyenli iki denklemi)  

* **Hedef**: geliri sınıfı ' > 50 K', ' < 50 K ='. Bunlar 0 ve 1 ile sırasıyla veri hazırlık aşamasında değiştirilir.
* **ÖZELLİKLERİ**: yaşı, iş sınıfı, eğitim düzeyi, eğitim düzeyi, yarış, seks, vb. hafta başına iş saatleri.


## <a name="project-structure-execution-and-reporting"></a>Proje yapısı, yürütme ve Raporlama

### <a name="structure"></a>yapısı
Bu proje için kullanıyoruz (aşağıda), hangi aşağıdaki TDSP klasör yapısı ve belgeleri şablonları [TDSP yaşam döngüsü](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md). 

Proje sağlanan yönergeleri göre oluşturulur [burada](https://aka.ms/how-to-use-tdsp-in-aml). Projenin kod ve yapıları ile karşılandıktan sonra yapısı (aşağıdaki şekilde kırmızıyla Kutulu proje yapısını bakın) gibi görünüyor.


<img src="./media/scenario-tdsp-classifying-us-incomes/instantiation-4.png" width="900" height="700">

### <a name="execution"></a>Yürütme
Bu örnekte, biz kod yürütmek **yerel işlem ortamı**. Üzerinde daha ayrıntılı bilgi için Azure Machine Learning belgelere başvurun [yürütme seçeneklerini](experimentation-service-configuration.md).

Yerel Python çalışma zamanında bir Python komut dosyası yürütme kolaydır:

    az ml experiment submit -c local my_script.py

IPython not defteri dosyaları Azure Machine Learning UI soldaki proje yapısından tıklatıldığında ve Jypyter not defteri sunucunun çalıştırın.


Adım adım veri bilimi akışı gibi oluştu:

* [**Veri alımı ve anlama**](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/tree/master/code/01_data_acquisition_and_understanding)

İndirilen UCI ML deposunda URL'lerden .csv formunda veri [[bağlantı]](https://archive.ics.uci.edu/ml/datasets/adult). Özellikler, hedefi ve bunların dönüşümleri ProjectReport.md dosyasında ayrıntılı açıklanmıştır.

Veri alımı ve anlamak için kod bulunur: / kod/01_data_acquisition_and_understanding.

Veri keşfi, Python 3 kullanarak gerçekleştirilir [IDEAR (etkileşimli veri keşfi ve raporlama) yardımcı programı](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/DataReport-Utils/Python) bir parçası olarak yayımlanan [TDSP suite veri bilimi araçların](https://github.com/Azure/Azure-TDSP-Utilities). Bu yardımcı programı, sayısal ve kategorik özellikleri ve hedef içeren veriler için standartlaştırılmış veri araştırması raporları oluşturmak için yardımcı olur. Python 3 IDEAR yardımcı programı nasıl kullanıldığını Ayrıntılar aşağıda sağlanır. 

Son veri araştırması raporu konumudur [IDEAR.html](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/tree/master/docs/deliverable_docs). Bir görünüm IDEAR raporun aşağıda gösterilmiştir:

![](./media/scenario-tdsp-classifying-us-incomes/idear.png)

* [**Modelleme**](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/tree/master/code/02_modeling)

İki modeli ile 3-fold çapraz doğrulama oluşturduk: esnek Net ve rasgele orman. Kullandık [59 noktası örnekleme](http://www.jmlr.org/papers/volume13/bergstra12a/bergstra12a.pdf) bir strateji olarak rastgele kılavuz arama çapraz doğrulama ve model parametresi iyileştirme için. Modelleri doğruluğunu ölçülen test veri kümesinde AUC (Eğri alanında) kullanarak. 

Modelleme bulunan için kod: / kod/02_modeling.

Esnek Net ve rasgele orman modelleri AUC > 0.85 yoktu. Biz her iki modeli pickled.pkl dosyaları ve her iki modelde de ROC çizer çıkış kaydedin. Rastgele orman AUC modeli 0.92 ve esnek Net modelinkiyle 0,90 olmuştur. Ayrıca, model yorumu için özellik önem rastgele orman modeli için bir .csv dosyasına çıkarmak ve bir pdf (Tahmine dayalı özellikleri ilk 20 yalnızca) çizilen.

ROC eğrisi, **rastgele orman modeli** test verileri aşağıda verilmiştir. Dağıtılan modeli, bu:

![](./media/scenario-tdsp-classifying-us-incomes/rf-auc.png)

Rastgele orman modeli (ilk 20) önemini özelliği aşağıda gösterilmiştir. Gösterir özellikleri sermaye kazanç tutarı, eduction, medeni, en yüksek özelliği öneme sahip.

![](./media/scenario-tdsp-classifying-us-incomes/featImportance.png)

* [**Dağıtım**](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/tree/master/code/03_deployment)

Bir küme üzerinde bir web hizmeti olarak rastgele orman modeli dağıttığımız [Azure kapsayıcı Hizmeti'ni (ACS)](https://azure.microsoft.com/services/container-service/). Operationalization ortam Docker ve Kubernetes web hizmeti dağıtımını yönetmek için kümedeki sağlar. Daha fazla operationalization süreci hakkında bilgiler bulabilirsiniz [burada](model-management-service-deploy.md). 

Dağıtım bulunan için kod: / kod/03_deployment.


### <a name="final-project-reporthttpsgithubcomazuremachinelearningsamples-tdspuciadultincomeblobmasterdocsdeliverabledocsprojectreportmd"></a>[Son Proje raporu](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/blob/master/docs/deliverable_docs/ProjectReport.md)
Yukarıdaki bölümlerde ayrıntılarını derlenmiş son proje raporda sağlanan [ProjectReport](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/blob/master/docs/deliverable_docs/ProjectReport.md). Proje rapor ayrıca kullanım örneği, model performans ölçümleri, dağıtım ve üzerinde proje geliştirilen ve dağıtılan altyapı hakkında ayrıntılı bilgi içerir.

Tüm proje klasörünü ve sürüm denetimi deponuza istemciye teslim edilebilir içeriğini birlikte proje rapor.


## <a name="conclusion"></a>Sonuç

Bu örnekte, biz TDSP yapısı ve şablonları Azure Machine Learning ile kullanmak üzere şimdi gösterdi. Belge ve yapı şablonlarını şunları yapabilirsiniz:
1. Düzgün amacı ve bir projenin kapsamını tanımlayın
2. Proje ekibi dağıtılmış rolleri ve sorumlulukları oluşturun
3. Yapı ve projeleri TDSP yaşam döngüsünün aşamaları göre yürütme
4. TDSP veri bilimi hizmet programları (IDEAR veri keşfi ve görselleştirme raporu gibi) kullanarak standart raporları geliştirin.
5. Bir istemciye teslim edilebilir bir son veri bilimi proje rapor hazırlayın

Standartlaştırma ve işbirliği içinde veri bilimi ekipleri kolaylaştırmak için Azure Machine Learning bu özelliği kullanmak umuyoruz.

## <a name="next-steps"></a>Sonraki Adımlar

Başlamak için aşağıdaki başvurulara bakın:

[Azure Machine Learning ekibi veri bilimi işlem (TDSP) kullanma](https://aka.ms/how-to-use-tdsp-in-aml)

[Takım veri bilimi işlemi (TDSP)](https://github.com/Azure/Microsoft-TDSP)

[Azure Machine Learning için TDSP proje şablonu](https://aka.ms/tdspamlgithubrepo)

[Veri kümesi UCI ML depodan ABD gelir](https://archive.ics.uci.edu/ml/datasets/adult)