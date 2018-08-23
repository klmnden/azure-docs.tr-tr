---
title: Gelir sınıflandırma - Team Data Science Process - Azure Machine Learning | Microsoft Docs
description: Azure Machine Learning'de ABD içi gelirleri sınıflandıran bir proje oluşturmak için Team Data Science Process şablon kullanma
services: machine-learning
documentationcenter: ''
author: deguhath
ms.author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2017
ms.openlocfilehash: edc3fc5e2a625a14bcb48b03f32cd99069a0ad53
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2018
ms.locfileid: "42056370"
---
# <a name="income-classification-with-team-data-science-process-tdsp-project"></a>Team Data Science işlem (TDSP) proje ile gelir sınıflandırma

## <a name="introduction"></a>Giriş

Standartlaştırma yapısı ve belgeleri veri bilimi projeleri, diğer bir deyişle kurulan bir bağlantılı [veri bilimi yaşam döngüsünü](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md), veri bilimi ekipleri etkin işbirliği etkinleştirme için anahtardır. Azure Machine Learning projeleriyle oluşturma [Team Data Science işlem (TDSP)](https://github.com/Azure/Microsoft-TDSP) şablonu tür Standartlaştırma için bir çerçeve sağlar.

Daha önce yayımladık bir [TDSP Proje yapısı ve şablonlar için GitHub deposu](https://github.com/Azure/Azure-TDSP-ProjectTemplate). Ancak artık TDSP yapısı ve bir veri bilimi aracını şablonlarında örneklemek mümkün kadar değildi. Azure Machine Learning projelerin ile örneği oluşturma artık etkinleştirdik [TDSP yapısı ve belgeler şablonları Azure Machine Learning için](https://github.com/amlsamples/tdsp). TDSP yapısı ve şablonları Azure Machine Learning'de kullanmak hakkında yönergeler sağlanır [burada](https://aka.ms/how-to-use-tdsp-in-aml). Burada nasıl bir gerçek machine learning projesi TDSP yapısını kullanarak oluşturulabilir, yapıtlar ve belgeler gibi projeye özgü kod ile doldurulur ve Azure Machine Learning içinde yürütülen bir örnek sağlar.

## <a name="link-to-github-repository"></a>GitHub deponuza bağlayın
Özet belgeler sunuyoruz [burada](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) hakkında bir örnek. Daha kapsamlı belgeler GitHub sitesinde bulunabilir.

### <a name="purpose"></a>Amaç
Birincil amacı, bu örnek, örneği ve bir machine learning projesi kullanarak yürütmek nasıl göstermektir [Team Data Science işlem (TDSP)](https://github.com/Azure/Microsoft-TDSP) yapısı ve Azure Machine Learning şablonlar. Bu amaç için iyi bilinen kullandığımız [1994 BİZE Görselleştirmenizdeki verilerin UCI Machine Learning deposundan](https://archive.ics.uci.edu/ml/datasets/adult). Modelleme ABD yıllık gelir sınıfları BİZE Görselleştirmenizdeki bilgileri (örneğin, yaş, yarış, eğitim düzeyini, vb. kaynak ülke) tahmin etmek için bir görevdir

### <a name="scope"></a>Kapsam
 * Veri araştırma, eğitim ve kullanım çalışması genel bakış içinde açıklanan tahmin soruna yönelik bir makine öğrenme modelinin dağıtımı. 
 * Bu proje için Azure Machine Learning Team Data Science işlem (TDSP) şablonu kullanarak Azure Machine Learning projede yürütme. Proje yürütme ve raporlama için TDSP yaşam döngüsü kullanmayı ekleyeceğiz.
 * Azure Machine Learning, Azure kapsayıcı Hizmetleri'ndeki doğrudan çözümün kullanıma hazır hale getirme

 Proje çeşitli özellikler, Azure Machine Learning gibi TDSP yapısı örnekleme ve kullanımı, Docker ve Kubernetes kullanarak Azure Container Services kolay kullanıma hazır hale getirme ve Jupyter not defterleri ve bunun yanı sıra Python dosyaları kodu yürütme vurgular.


## <a name="team-data-science-process-tdsp-lifecycle"></a>Team Data Science Process (TDSP) yaşam döngüsü
Bkz: [takım veri bilimi Process (TDSP) yaşam döngüsü](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md)

![](./media/scenario-tdsp-classifying-us-incomes/tdsp-lifecycle.jpg)

## <a name="prerequisites"></a>Önkoşullar
### <a name="required-subscription-hardware-software"></a>Gerekli: Abonelik, donanım, yazılım
1. Bir Azure [abonelik](https://azure.microsoft.com). Alabileceğiniz bir [ücretsiz abonelik](https://azure.microsoft.com/free/?v=17.16&WT.srch=1&WT.mc_id=AID559320_SEM_cZGgGOIg) Bu örnek ayrıca yürütülecek.
2. Bir [Azure veri bilimi sanal makinesi (DSVM) Windows Server 2016](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.windows-data-science-vm), (sanal makine boyutu: [DS3_V2](https://docs.microsoft.com/azure/virtual-machines/windows/sizes)4 sanal CPU'lar ve 14 Gb RAM). Bir Azure DSVM üzerinde test olsa da, herhangi bir Windows 10 makinede çalışması olasıdır.
3. Azure Machine Learning ve ilgili kendi belgelerini gözden geçirin (bağlantılar için aşağıya bakın) Hizmetleri.
4. Azure Machine Learning tarafından düzgün bir şekilde yüklediğinizden emin olun [hızlı başlangıç Yükleme Kılavuzu](../service/quickstart-installation.md).

Bu örnek için bir veri kümesini UCI ML depodur [[LINK]](https://archive.ics.uci.edu/ml/datasets/adult). 1994 BİZE Görselleştirmenizdeki veritabanından alınır ve yaklaşık 50.000 kişiler için görselleştirmenizdeki ve gelir bilgilerini içerir. Sayısal sahip yapılandırılmış bir veri kümesi, kategorik özellikleri ve iki gelir kategorisi oluşan bir kategorik hedef budur ('> 50 K' veya ' < 50 bin ='). 

### <a name="optional-version-control-repository"></a>İsteğe bağlı: Sürüm denetimi deposu
Kaydetmek istiyorsanız ve sürüm projenizi ve içeriği, burada bu yapılabilir bir sürüm denetimi deposu olması gerekir. Azure Machine Learning'de TDSP şablonuyla yeni proje oluşturma sırasında Git depo konumunu girebilirsiniz. Bkz: [Azure Machine Learning'de Git kullanmayı](using-git-ml-project.md) daha ayrıntılı bilgi için.

### <a name="informational-about-azure-machine-learning"></a>Bilgi amaçlı: Azure Machine Learning hakkında
* [SSS - kullanmaya nasıl başlayacağınızı](frequently-asked-questions.md)
* [Genel Bakış](../service/overview-what-is-azure-ml.md)
* [Yükleme](../service/quickstart-installation.md)
* [Yürütme](experimentation-service-configuration.md)
* [TDSP kullanma](https://aka.ms/how-to-use-tdsp-in-aml)
* [Dosyaları okuma ve yazma](how-to-read-write-files.md)
* [Azure Machine Learning ile Git kullanma](using-git-ml-project.md)
* [ML model bir web hizmeti olarak dağıtma](model-management-service-deploy.md)

### <a name="create-a-new-workbench-project"></a>Workbench yeni bir proje oluşturun

Bu örnekte, şablon olarak kullanarak yeni bir proje oluşturun:
1.  Açık bir Azure Machine Learning Workbench'i
2.  Üzerinde **projeleri** sayfasında **+** açıp seçmek **yeni proje**
3.  İçinde **yeni proje oluştur** bölmesinde, yeni projeniz için bilgileri girin
4.  İçinde **proje şablonlarında Ara** arama kutusuna "Sınıflandır ABD içi gelirleri - TDSP projesi" yazın ve şablonu seçin
5.  **Oluştur**'a tıklayın

(Uygun kutusunda) proje oluşturma sırasında boş bir Git deposu konumu belirtin, ardından bu depoyu Proje yapısı ve içeriği ile Proje oluşturulduktan sonra doldurulur.

## <a name="use-case-overview"></a>Kullanım örneği genel bakış
Sorunu BİZE sayım yakalanan nasıl socio ekonomik verileri anlamak için ABD'de kişilerin yıllık gelir tahmin yardımcı olabileceği konudur. Böyle Görselleştirmenizdeki özelliklere göre bir bireyin gelir 50.000 TL olup olmadığını tahmin etmek için machine learning görev olduğunu (ikili sınıflandırma görevi).

## <a name="data-description"></a>Veri açıklaması
Veriler hakkında ayrıntılı bilgi için bkz. [açıklama](https://archive.ics.uci.edu/ml/machine-learning-databases/adult/adult.names) UCI deposunda. 

Bu veri bulundu Görselleştirmenizdeki büro veritabanından ayıklanan: https://www.census.gov/en.html. 


* (Hiçbir filtreleme önce) 48,842 örneklerinin toplam, sürekli ve ayrık karışımı (eğitme = 32, 561, test = 16, 281)
* Etiket için olasılık ' > 50 K': / %24.78 23.93 yüzdesi (olmadan bilinmeyenli iki denklemi)
* Etiket için olasılık ' < 50 bin =': / %75.22 76.07 yüzdesi (olmadan bilinmeyenli iki denklemi)  

* **Hedef**: gelir sınıfı ' > 50 K', ' < 50 bin ='. Bunlar 0 ve 1 ile sırasıyla veri hazırlama aşamasında değiştirilir.
* **Özellikler**: yaş, iş sınıfı, eğitim düzeyini, eğitim düzeyini, yarış, cinsiyet, haftalık, vb. başına iş saati sayısı.


## <a name="project-structure-execution-and-reporting"></a>Proje yapısı, yürütme ve Raporlama

### <a name="structure"></a>yapısı
Bu proje için kullandığımız TDSP klasör yapısını ve belgelerini şablonları (aşağıda), izleyen [TDSP yaşam döngüsü](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md). 

Proje sağlanan yönergeler göre oluşturulan [burada](https://aka.ms/how-to-use-tdsp-in-aml). Proje kodu ve yapıları ile doldurulmuş sonra (bkz. Proje yapısı aşağıdaki şekilde kırmızıyla Kutulu) yapısı şu şekilde görünür.


<img src="./media/scenario-tdsp-classifying-us-incomes/instantiation-4.png" width="900" height="700">

### <a name="execution"></a>Yürütme
Biz bu örnekte, kod yürütme **yerel işlem ortamı**. Üzerinde daha ayrıntılı bilgi için Azure Machine Learning belgeleri için başvurmak [yürütme seçeneklerini](experimentation-service-configuration.md).

Yerel bir Python çalışma zamanında bir Python betiği yürütme kolaydır:

    az ml experiment submit -c local my_script.py

Ipython not defteri dosyaları, Azure Machine Learning UI soldaki proje yapısından açılmaz ve Jypyter not defteri sunucuda çalıştırın.


Adım adım veri bilimi iş akışı gibi şöyleydi:

* [**Veri edinme ve anlama**](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/tree/master/code/01_data_acquisition_and_understanding)

Veri .csv formunda UCI ML deposunda URL'lerden indirilen [[LINK]](https://archive.ics.uci.edu/ml/datasets/adult). Özellikler, hedef ve bunların dönüşümleri ProjectReport.md dosyasındaki ayrıntılı açıklanmıştır.

Veri edinme ve anlama için kodu bulunur: / kod/01_data_acquisition_and_understanding.

Veri keşfi, Python 3'ü kullanarak gerçekleştirilir [IDEAR (etkileşimli veri keşfi ve raporlama) yardımcı programı](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/DataReport-Utils/Python) bir parçası olarak yayımlanan [veri bilimi araçları TDSP suite](https://github.com/Azure/Azure-TDSP-Utilities). Bu yardımcı programı, sayısal ve kategorik özellikleri ve hedef içeren veriler için standartlaştırılmış veri araştırma raporları oluşturmak için yardımcı olur. Python 3 IDEAR yardımcı programı nasıl kullanıldığını ayrıntıları aşağıda sağlanmıştır. 

Son veri araştırma rapor konumudur [IDEAR.html](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/tree/master/docs/deliverable_docs). IDEAR rapor görünümünü aşağıda gösterilmiştir:

![](./media/scenario-tdsp-classifying-us-incomes/idear.png)

* [**Modelleme**](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/tree/master/code/02_modeling)

3-fold çapraz doğrulama ile iki modeli oluşturduk: esnek Net ve rastgele orman. Kullandık [noktadan 59 örnekleme](http://www.jmlr.org/papers/volume13/bergstra12a/bergstra12a.pdf) bir strateji olarak rastgele bir kılavuz arama çapraz doğrulama ve model parametresi iyileştirme için. Model doğruluğunu ölçülen test veri kümesinde AUC (Eğri alanında) kullanarak. 

Modelleme bulunan kodu: / kod/02_modeling.

Esnek Net ve rastgele orman modelleri AUC > 0.85 yoktu. Pickled.pkl dosyaları ve çıkış ROC her iki modelde de çizer her iki modeli kaydedin. Rastgele orman AUC modeli 0.92 ve esnek ağ modeli, 0.90 şeklindeydi. Ayrıca, model yorumu için özellik önem rastgele orman modeli için bir .csv dosyasında çıktı ve çizilen bir pdf (Tahmine dayalı özellikleri ilk 20 yalnızca).

ROC eğrisi, **rastgele orman modeli** test verileri aşağıda gösterilmiştir. Bu, dağıtılan model şöyleydi:

![](./media/scenario-tdsp-classifying-us-incomes/rf-auc.png)

Rasgele orman modeli (ilk 20) önemini özelliği aşağıda gösterilmiştir. Gösterir özellikler büyük kazanç tutarı, eduction, Medeni durum, özellik en yüksek öneme sahip.

![](./media/scenario-tdsp-classifying-us-incomes/featImportance.png)

* [**Dağıtım**](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/tree/master/code/03_deployment)

Biz bir kümede bulunan bir web hizmeti olarak rasgele orman modeli dağıtılan [Azure Container Service (ACS)](https://azure.microsoft.com/services/container-service/). Kullanıma hazır hale getirme ortamı, Docker ve Kubernetes kümesini web hizmeti dağıtımı Yönet sağlar. Daha fazla kullanıma hazır hale getirme işlemi hakkında bilgi bulabilirsiniz [burada](model-management-service-deploy.md). 

Dağıtım bulunan kodu: / kod/03_deployment.


### <a name="final-project-reporthttpsgithubcomazuremachinelearningsamples-tdspuciadultincomeblobmasterdocsdeliverabledocsprojectreportmd"></a>[Son Proje raporu](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/blob/master/docs/deliverable_docs/ProjectReport.md)
Yukarıdaki bölümlerde ayrıntılarını derlenmiş son proje raporda sağlanan [ProjectReport](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/blob/master/docs/deliverable_docs/ProjectReport.md). Proje raporunu, ayrıca kullanım örneği, model performans ölçümleri, dağıtım ve üzerinde proje geliştirilen ve dağıtılan altyapı hakkında daha ayrıntılı bilgi içerir.

Sürüm denetimi deposu istemciye teslim edilebilir ve tüm proje klasörünün içeriğini birlikte proje rapor.


## <a name="conclusion"></a>Sonuç

Bu örnekte biz TDSP yapısı ve şablonları Azure Machine Learning'de kullanmak üzere şimdi gösterdi. Belge ve yapı şablonları şunları yapabilirsiniz:
1. Düzgün amacı ve bir projenin kapsamını tanımlayın
2. Dağıtılmış rolleri ve sorumlulukları ile bir proje ekibi oluşturun.
3. Yapı ve projeleri TDSP yaşam döngüsünün aşamaları göre çalıştırın
4. Standart raporlar (örneğin, IDEAR veri keşfi ve görselleştirme rapor) TDSP veri bilimi Araçları'nı kullanarak geliştirin.
5. Bir istemciye teslim edilebilir bir son veri bilimi proje rapor hazırlayın

Standartlaştırma ile işbirliği içinde veri bilimi ekipleri kolaylaştırmak için Azure Machine Learning, bu özelliği kullanmadan umuyoruz.

## <a name="next-steps"></a>Sonraki Adımlar

Kullanmaya başlamak için aşağıdaki başvuruları bakın:

[Azure Machine Learning'de Team Data Science işlem (TDSP) kullanma](https://aka.ms/how-to-use-tdsp-in-aml)

[Team Data Science Process (TDSP)](https://github.com/Azure/Microsoft-TDSP)

[Azure Machine Learning için TDSP projesi şablonu](https://aka.ms/tdspamlgithubrepo)

[Veri kümesi UCI ML depodan ABD gelir](https://archive.ics.uci.edu/ml/datasets/adult)