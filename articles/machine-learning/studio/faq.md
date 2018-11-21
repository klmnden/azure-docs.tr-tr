---
title: Azure Machine Learning hakkında sık sorulan sorular (SSS) | Microsoft Belgeleri
description: "Azure Machine Learning'e giriş: Kolaylaştırılmış tahmine dayalı modelleme için bir bulut hizmetinin faturalamasını, özelliklerini ve sınırlamalarını kapsayan SSS."
keywords: machine learning giriş,tahmini modelleme,machine learning nedir
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=hshapiro, author=heatherbshapiro)
ms.author: amlstudiodocs
ms.assetid: a4a32a06-dbed-4727-a857-c10da774ce66
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/02/2017
ms.openlocfilehash: 9545c9a0cf77274b4ea68dea7a6640453a849093
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52263443"
---
# <a name="azure-machine-learning-frequently-asked-questions-billing-capabilities-limitations-and-support"></a>Azure Machine Learning ile ilgili sık sorulan sorular: Faturalama, özellikler, sınırlamalar ve destek
Aşağıda, tahmine dayalı modeller geliştirmeye ve web hizmetleri aracılığıyla çözümleri faaliyete geçirmeye yönelik bir bulut hizmeti olan Azure Machine Learning hakkında sık sorulan bazı sorular (SSS) ve yanıtları verilmiştir. Bu SSS bölümlerinde faturalama modeli, özellikler, sınırlamalar ve destek dahil olmak üzere hizmetin nasıl kullanılacağına ilişkin sorular sağlanır.

**Burada olmayan bir sorunuz mu var?**

Azure Machine Learning'in MSDN üzerinde veri bilimi topluluğu üyelerinin Azure Machine Learning hakkında sorular sorabileceği bir forumu da bulunur. Forum, Azure Machine Learning ekibi tarafından izlenir. Yanıt aramak veya yeni bir soru sormak için [Azure Machine Learning Forumuna](https://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) gidin.

## <a name="general-questions"></a>Genel sorular
**Azure Machine Learning nedir?**

Azure Machine Learning, tahmine dayalı analitik çözümleri bulutta oluşturmak, test etmek, işletmek ve yönetmek için kullanabileceğiniz tam olarak yönetilen bir hizmettir. Yalnızca bir tarayıcıyla oturum açabilir, verileri yükleyebilir ve anında makine öğrenimi denemelerine başlayabilirsiniz. Tahmine dayalı sürükle ve bırak modelleme, büyük bir modül paleti ve başlangıç şablonları kitaplığı, ortak makine öğrenimi görevlerini basit ve hızlı hale getirir. Daha fazla bilgi için bkz. [Azure Machine Learning hizmetine genel bakış](https://azure.microsoft.com/services/machine-learning/). Makine öğrenimiyle ilgili önemli terimleri ve kavramları açıklayan bir giriş için bkz. [Azure Machine Learning'e giriş](what-is-machine-learning.md).

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

**Machine Learning Studio nedir?**

Machine Learning Studio, bir web tarayıcısı kullanarak erişebileceğiniz bir çalışma ekranı ortamıdır. Machine Learning Studio, deneme şeklinde uçtan uca veri bilimi akışı oluşturmanıza yardımcı olan bir görsel birleştirme arabiriminde bir modül paleti barındırır.

Machine Learning Studio hakkında daha fazla bilgi için bkz. [Machine Learning Studio nedir?](what-is-ml-studio.md).

**Machine Learning API hizmeti nedir?**

Machine Learning API'si hizmeti, Machine Learning Studio'da yerleşik olanlar gibi tahmine dayalı modelleri ölçeklenebilir ve hataya dayanıklı web hizmetleri olarak dağıtmanızı sağlar. Machine Learning API hizmetinin oluşturduğu web hizmetleri, dış uygulamalar ile tahmine dayalı analiz modelleriniz arasında iletişim için bir arabirim sağlayan REST API'lerdir.

Daha fazla bilgi için bkz. [Azure Machine Learning web hizmetini kullanma](consume-web-services.md).

**Klasik web hizmetlerim nerede listelenir? Yeni (Azure Resource Manager tabanlı) web hizmetlerim nerede listelenir?**

Klasik dağıtım modeli kullanılarak oluşturulan web hizmetleri ve Yeni Azure Resource Manager dağıtım modeli kullanılarak oluşturulan web hizmetleri [Microsoft Azure Machine Learning Web Hizmetleri](https://services.azureml.net/) portalında listelenir.

Klasik web hizmetleri, [Machine Learning Studio](http://studio.azureml.net)'daki **Web hizmetleri** sekmesinde de listelenir.

## <a name="azure-machine-learning-questions"></a>Azure Machine Learning soruları
**Microsoft Azure Machine Learning web hizmetleri nelerdir?**

Machine Learning web hizmetleri, bir uygulama ile Machine Learning iş akışı puanlama modeli arasında bir arabirim sağlar. Bir dış uygulama, Azure Machine Learning’i kullanarak bir Machine Learning iş akışı puanlama modeli ile gerçek zamanlı olarak iletişim kurabilir. Machine Learning web hizmetine bir çağrı yapılırsa tahmin sonuçları bir dış uygulamaya döndürülür. Bir web hizmetine çağrı yapmak için web hizmeti dağıtılırken oluşturulan bir API anahtarını geçirirsiniz. Machine Learning web hizmetleri, web programlama projeleri için popüler bir mimari seçimi olan REST’i temel alır.

Azure Machine Learning iki tür web hizmeti içerir:

* İstek-Yanıt Hizmeti (RRS): Machine Learning Studio kullanılarak oluşturulup dağıtılan durum bilgisiz modeller için bir arabirim sağlayan düşük gecikme süreli, yüksek düzeyde ölçeklenebilir bir hizmettir.
* Toplu Yürütme Hizmeti (BES): Veri kayıtları için toplu iş yapan zaman uyumsuz bir hizmettir.

REST API’sini kullanmanın ve web hizmetine erişmenin birkaç yolu vardır. Örneğin, web hizmetini dağıttığınızda sizin için oluşturulan örnek kodu kullanarak C#, R veya Python’da bir uygulama yazabilirsiniz.

Örnek koda şuradan ulaşabilirsiniz:
- Web hizmetinin Azure Machine Learning Web Hizmetleri portalındaki Kullanım sayfası
- Web hizmetinin Machine Learning Studio'daki panosunda bulunan API Yardım Sayfası

Ayrıca, sizin için oluşturulan ve Machine Learning Studio’daki web hizmeti panosunda bulunan örnek Microsoft Excel çalışma kitabını da kullanabilirsiniz.

**Azure Machine Learning için ana güncelleştirmeler nelerdir?**

En son güncelleştirmeler için bkz. [Azure Machine Learning’deki yenilikler](../../active-directory/fundamentals/whats-new.md).

## <a name="machine-learning-studio-questions"></a>Machine Learning Studio soruları
### <a name="import-and-export-data-for-machine-learning"></a>Machine Learning için verileri içeri ve dışarı aktarma
**Machine Learning hangi veri kaynaklarını destekliyor?**

Bir Machine Learning Studio denemesine üç yöntemle veri indirebilirsiniz:

- Yerel bir dosyayı veri kümesi olarak karşıya yükleme
- Bir modülü kullanarak bulu veri hizmetlerinden içeri veri aktarma
- Başka bir denemeden kaydedilen veri kümesini içeri aktarma

Desteklenen dosya biçimleri hakkında daha fazla bilgi edinmek için bkz. [Machine Learning Studio'da eğitim verilerini içeri aktarma](import-data.md).

#### <a id="ModuleLimit"></a>Veri kümesi, modüllerim için ne kadar büyüyebilir?
Machine Learning Studio'daki modüller, ortak kullanım durumları için en fazla 10 GB boyutunda yoğun sayısal verili veri kümelerini destekler. Bir modülün birden fazla giriş aldığı durumlarda 10 GB değeri tüm giriş boyutlarının toplamıdır. Ayrıca, Hive veya Azure SQL Veritabanı’ndan yapılan sorguları kullanarak daha büyük veri kümelerinden örnek oluşturabilir ya da alımdan önce Sayımlarla Öğrenme ön işlemesini kullanabilirsiniz.  

Aşağıdaki veri türleri, özellik normalleştirme sırasında daha büyük veri kümelerine genişleyebilir ve boyutu 10 GB’den az olacak şekilde sınırlıdır:

* Seyrek
* Kategorik
* Dizeler
* İkili veriler

Aşağıdaki modüller, boyutu 10 GB'den az veri kümeleriyle sınırlıdır:

* Öneren modüller
* Synthetic Minority Oversampling Technique (SMOTE) modülü
* Betik modülleri: R, Python, SQL
* Katılma veya Özellik Karma gibi çıkış veri boyutunun giriş veri boyutundan büyük olabileceği modüller
* Yineleme sayısının çok büyük olduğu durumlarda Çapraz doğrulama, Model Ayarlama Hiperparametreleri, Sıralı Regresyon ve Tek veya Tüm Çoklu Sınıflar

#### <a id="UploadLimit"></a>Karşıya veri yükleme limitleri nelerdir?
Boyutu birkaç GB'den büyük olan veri kümeleri için, verileri doğrudan bir yerel dosyadan karşıya yüklemek yerine Azure Depolama veya Azure SQL Veritabanı’na yükleyin ya da Azure HDInsight’ı kullanın.

**Verileri Amazon S3'ten okuyabilir miyim?**

Az miktarda veriniz varsa ve bunu bir HTTP URL'si aracılığıyla kullanıma sunmak istiyorsanız [Verileri İçeri Aktarma][import-data] modülünü kullanabilirsiniz. Daha büyük miktarda olan verileri Azure Depolama'ya aktarın ve ardından [Verileri İçeri Aktarma][import-data] modülünü kullanarak denemenize aktarın.
<!--

<SEE CLOUD DS PROCESS>
-->

**Yerleşik bir görüntü girişi özelliği var mı?**

Görüntü girişi özelliği hakkında [Görüntüleri İçeri Aktarma][image-reader] başvurusundan bilgi edinebilirsiniz.

### <a name="modules"></a>Modüller
**Aradığım algoritma, veri kaynağı, veri biçimi veya veri dönüştürme işlemi Azure Machine Learning Studio'da yok. Seçeneklerim neler?**

Takip ettiğimiz özellik isteklerini görmek için [kullanıcı geri bildirim forumuna](https://go.microsoft.com/fwlink/?LinkId=404231) gidebilirsiniz. Aradığınız bir özellik zaten istenmişse isteğe oyunuzu ekleyin. Aradığınız özellik mevcut değilse yeni bir istek oluşturun. Bu forumda isteğinizin durumunu da görüntüleyebilirsiniz. Bu listeyi yakından takip edip özellik kullanılabilirliği durumunu sık sık güncelleştiriyoruz. Buna ek olarak, R ve Python için yerleşik desteği kullanarak gerektiğinde özel dönüştürmeler oluşturabilirsiniz.

**Var olan kodumu Machine Learning Studio'ya getirebilir miyim?**

Evet, var olan R veya Python kodunuzu Machine Learning Studio'ya getirebilir, Azure Machine Learning öğrencileriyle birlikte aynı denemede bunu çalıştırabilir ve Azure Machine Learning aracılığıyla çözümü bir web hizmeti olarak dağıtabilirsiniz. Daha fazla bilgi için bkz. [R ile denemenizi genişletme](extend-your-experiment-with-r.md) ve [Azure Machine Learning Studio'da Python makine öğrenimi betiklerini yürütme](execute-python-scripts.md).

**Bir modeli tanımlamak için [PMML](http://en.wikipedia.org/wiki/Predictive_Model_Markup_Language) benzeri bir dil kullanmak mümkün mü?**

Hayır, Predictive Model Markup Language (PMML) desteklenmez. Bir modülü tanımlamak için özel R ve Python kodu kullanabilirsiniz.

**Denememe paralel şekilde kaç modül yürütebilirim?**  

Bir denemede paralel şekilde en fazla dört modül yürütebilirsiniz.

### <a name="data-processing"></a>Veri işleme
**Denemede, verileri etkileşimli bir şekilde görselleştirme (R görselleştirmesinin ötesinde) özelliği var mı?**

Verileri görselleştirmek ve istatistikleri elde etmek için bir modülün çıkışına tıklayın.

**Bir tarayıcıda sonuçların veya verilerin önizlemesi yapıldığı sırada satır ve sütun sayısı sınırlı oluyor. Neden?**

Bir tarayıcıya büyük miktarda veri gönderilebileceğinden, Machine Learning Studio’nun yavaşlamaması için veri boyutu sınırlanır. Tüm verilerin/sonuçların görselleştirilmesi için verileri indirip Excel veya başka bir araç kullanmak daha iyidir.

### <a name="algorithms"></a>Algoritmalar
**Machine Learning Studio'da hangi mevcut algoritmalar destekleniyor?**

Machine Learning Studio, Microsoft Research'te geliştirilen Ölçeklenebilir Artırılmış Karar ağaçları, Bayes Önerisi sistemleri, Derin Sinir Ağları ve Karar Ormanları gibi teknoloji harikası algoritmaları sağlar. Vowpal Wabbit gibi ölçeklenebilir açık kaynaklı makine öğrenimi paketleri de dahildir. Machine Learning Studio, çok sınıflı ve ikili sınıflandırma, regresyon ve kümeleme için makine öğrenimi algoritmalarını destekler. [Machine Learning Modüllerinin][machine-learning-modules] tam listesine bakın.

**Verilerim için kullanılacak doğru Machine Learning algoritmasını otomatik olarak öneriyor musunuz?**

Hayır, ancak Machine Learning Studio'da sorununuz için doğru sonucu belirlemek amacıyla her bir algoritmanın sonucunu karşılaştırmaya yönelik çeşitli yollar mevcuttur.

**Sağlanan algoritmalar için bir algoritma yerine başka bir algoritmayı seçmeye yönelik herhangi bir yönerge var mı?**

Bkz. [Algoritma seçme](algorithm-choice.md).

**Sağlanan algoritmalar R veya Python ile mi yazılmış?**

Hayır, bu algoritmalar genellikle daha iyi performans sağlamak için derlenmiş dillerde yazılır.

**Algoritmalarla ilgili herhangi bir ayrıntı sağlanır mı?**

Belgelerde algoritmalar hakkında bazı bilgiler sağlanır ve algoritmayı kullanımınız için en iyi duruma getirme amacıyla ayar parametreleri açıklanır.  

**Çevrimiçi öğrenmeye yönelik herhangi bir destek var mı?**

Hayır, şu anda yalnızca programlama yoluyla yeniden eğitim desteklenir.

**Bir Sinir Ağı Modelinin katmanlarını yerleşik modülü kullanarak görselleştirebilir miyim?**

Hayır.

**Kendi modüllerimi C# veya başka bir dilde oluşturabilir miyim?**

Şu anda yeni özel modüller oluşturmak için yalnızca R kullanabilirsiniz.

### <a name="r-module"></a>R modülü
**Machine Learning Studio'da hangi R paketleri kullanılabilir?**

Machine Learning Studio, şu an 400’ün üzerinde CRAN R paketini desteklemektedir ve buna dahil olan tüm paketlerin [güncel listesi](http://az754797.vo.msecnd.net/docs/RPackages.xlsx) buradadır. Ayrıca, bu listeyi nasıl alacağınızı öğrenmek için bkz. [Denemenizi R ile genişletme](extend-your-experiment-with-r.md). İstediğiniz paket bu listede değilse [kullanıcı geri bildirim forumunda](https://go.microsoft.com/fwlink/?LinkId=404231) paketin adını sağlayın.

**Özel bir R modülü oluşturulabilir mi?**

Evet, daha fazla bilgi için bkz. [Azure Machine Learning'de özel R modülleri yazma](custom-r-modules.md).

**R için bir REPL ortamı var mı?**

Hayır, Studio'da R için Read-Eval-Print-Loop (REPL) ortamı yoktur.

### <a name="python-module"></a>Python modülü
**Özel bir Python modülü oluşturulabilir mi?**

Şu anda değil ancak aynı sonucu elde etmek için bir veya daha fazla [Python Betiği Yürütme][python] modülünü kullanabilirsiniz.

**Python için bir REPL ortamı var mı?**

Machine Learning Studio'daki Jupyter Not Defterlerini kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure Machine Learning Studio'da Jupyter Not Defterlerine Giriş](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).

## <a name="web-service"></a>Web hizmeti
### <a name="retrain"></a>Yeniden eğitme
**Azure Machine Learning modellerini program aracılığıyla nasıl yeniden eğitebilirim?**

Yeniden eğitme API'lerini kullanın. Daha fazla bilgi için bkz. [Machine Learning modellerini programlama aracılığıyla yeniden eğitme](retrain-models-programmatically.md). Örnek kod [Microsoft Azure Machine Learning Yeniden Eğitme Demosu](https://azuremlretrain.codeplex.com/)'nda da kullanılabilir.

### <a name="create"></a>Oluştur
**Modeli yerel olarak veya İnternet bağlantısı olmayan bir uygulamada dağıtabilir miyim?**

Hayır.

**Tüm web hizmetleri için beklenen bir temel gecikme süresi var mı?**

Bkz. [Azure abonelik limitleri](../../azure-subscription-service-limits.md).

### <a name="use"></a>Kullanım
**Tahmine dayalı modelimi, Toplu İş Yürütme hizmeti veya İstek Yanıtı hizmeti olarak çalıştırmayı ne zaman isterim?**

İstek Yanıtı hizmeti (RRS), deneme ortamından oluşturulan ve dağıtılan durum bilgisi olmayan modellere bir arabirim sağlamak için kullanılan düşük gecikme süreli ve yüksek ölçekli bir web hizmetidir. Toplu Yürütme Hizmeti (BES), toplu veri kayıtlarını zaman uyumsuz olarak puanlayan bir hizmettir. BES girişi, RRS’nin kullandığı veri girişi gibidir. Aralarındaki temel fark, BES'nin Azure Blob depolama, Azure Tablo depolama, Azure SQL Veritabanı, HDInsight (hive sorgusu) ve HTTP kaynakları gibi çeşitli kaynaklardan bir kayıt bloğunu okumasıdır. Daha fazla bilgi için bkz. [Azure Machine Learning web hizmetini kullanma](consume-web-services.md).

**Modeli, dağıtılan web hizmeti için nasıl güncelleştiririm?**

Zaten dağıtılmış olan bir hizmete yönelik tahmine dayalı bir modeli güncelleştirmek için eğitilen modeli yazmak ve kaydetmek için kullandığınız denemeyi değiştirin ve yeniden çalıştırın. Eğitilen modelin yeni bir sürümünü elde ettikten sonra, Machine Learning Studio size web hizmetini güncelleştirmek isteyip istemediğinizi sorar. Dağıtılmış bir web hizmetini güncelleştirme hakkında ayrıntılı bilgi edinmek için bkz. [Machine Learning web hizmeti dağıtma](publish-a-machine-learning-web-service.md).

Yeniden Eğitme API'lerini de kullanabilirsiniz.
Daha fazla bilgi için bkz. [Machine Learning modellerini programlama aracılığıyla yeniden eğitme](retrain-models-programmatically.md). Örnek kod [Microsoft Azure Machine Learning Yeniden Eğitme Demosu](https://azuremlretrain.codeplex.com/)'nda da kullanılabilir.

**Üretim ortamında dağıtılan web hizmetimi nasıl izlerim?**

Tahmine dayalı bir model dağıttıktan sonra bu modeli Azure Machine Learning Web Hizmetleri portalından izleyebilirsiniz. Dağıtılan her bir hizmet kendine panosuna sahiptir ve burada bu hizmetin izleme bilgilerini görebilirsiniz. Dağıtılmış web hizmetlerinizi yönetme konusunda daha fazla bilgi edinmek için bkz. [Azure Machine Learning Web Hizmetleri portalını kullanarak bir Web hizmetini yönetme](manage-new-webservice.md) ve [Azure Machine Learning çalışma alanını yönetme](manage-workspace.md).

**RRS/BES hizmetimin çıkışını görebileceğim bir yer var mı?**

RRS için sonucu genellikle web hizmeti yanıtında görürsünüz. Bunu Azure Blob depolama alanına da yazabilirsiniz. BES çıktısı varsayılan olarak bir bloba yazılır. Ayrıca, [Verileri Dışarı Aktarma][export-data] modülünü kullanarak çıkışı bir veritabanına veya tabloya da yazabilirsiniz.

**Yalnızca Machine Learning Studio'da oluşturulan modellerden mi web hizmetleri oluşturabilirim?**

Hayır, doğrudan Jupyter Not Defterlerinden ve RStudio'dan da web hizmetleri oluşturabilirsiniz.

**Hata kodlarıyla ilgili bilgileri nerede bulabilirim?**

Hata kodları ve açıklamaları listesi için bkz: [Machine Learning Hata Kodları](https://msdn.microsoft.com/library/azure/dn905910.aspx).

## <a name="scalability"></a>Ölçeklenebilirlik
**Web hizmetinin ölçeklenebilirliği nedir?**

Şu anda varsayılan uç nokta, uç nokta başına 20 eş zamanlı RRS isteği ile sağlanır. Bunu, uç nokta başına 200 eş zamanlı istek olacak şekilde ölçeklendirebilirsiniz ve [Web Hizmetini Ölçeklendirme](scaling-webservice.md)'de açıklandığı gibi her bir web hizmetini, web hizmeti başına 10.000 uç nokta olacak şekilde ölçeklendirebilirsiniz. BES için her bir uç nokta tek seferde 40 istek işleyebilir ve 40'ı aşan ek istekler kuyruğa alınır. Bu kuyruğa alınan istekler, sıra azaldıkça otomatik olarak çalıştırılır.

**R işleri düğümlere yayılır mı?**

Hayır.  

**Eğitim için ne kadar veri kullanabilirim?**

Machine Learning Studio'daki modüller, ortak kullanım durumları için en fazla 10 GB boyutunda yoğun sayısal verili veri kümelerini destekler. Bir modülün birden fazla giriş aldığı durumlarda tüm girişlerin toplam boyutu 10 GB'tır. Hive sorguları, Azure SQL Veritabanı sorguları ya da alımdan önce [Sayımlarla Öğrenme][counts] modülleriyle ön işleme aracılığıyla daha büyük veri kümelerinden de örnek oluşturabilirsiniz.  

Aşağıdaki veri türleri, özellik normalleştirme sırasında daha büyük veri kümelerine genişleyebilir ve boyutu 10 GB’den az olacak şekilde sınırlıdır:

* Seyrek
* Kategorik
* Dizeler
* İkili veriler

Aşağıdaki modüller, boyutu 10 GB'den az veri kümeleriyle sınırlıdır:

* Öneren modüller
* Synthetic Minority Oversampling Technique (SMOTE) modülü
* Betik modülleri: R, Python, SQL
* Katılma veya Özellik Karma gibi çıkış veri boyutunun giriş veri boyutundan büyük olabileceği modüller
* Yineleme sayısının çok büyük olduğu durumlarda Çapraz Doğrulama, Model Ayarlama Hiperparametreleri, Sıralı Regresyon ve Tek veya Tüm Çoklu Sınıflar

Boyutu birkaç GB'den büyük olan veri kümeleri için verileri doğrudan bir yerel dosyadan karşıya yüklemek yerine Azure Depolama veya Azure SQL Veritabanı’na yükleyin ya da Azure HDInsight’ı kullanın.

**Herhangi bir vektör boyutu sınırlaması var mı?**

Satırların ve sütunların her biri 2.147.483.647 Maks. Tamsayısı .NET sınırlamasına tabidir.

**Web hizmetini çalıştıran sanal makinenin boyutunu değiştirebilir miyim?**

Hayır.  

## <a name="security-and-availability"></a>Güvenlik ve kullanılabilirlik
**Web hizmetinin HTTP uç noktasına varsayılan olarak kimler erişebilir? Uç noktasına yönelik erişimi nasıl kısıtlarım?**

Bir web hizmeti dağıtıldıktan sonra, bu hizmet için varsayılan bir uç noktası oluşturulur. Varsayılan uç noktası, API anahtarı kullanılarak çağrılabilir. Web Hizmetleri portalından veya programlama aracılığıyla Web Hizmet Yönetimi API'lerini kullanarak, kendi anahtarlarına sahip ek uç noktalar ekleyebilirsiniz. Erişim tuşları, web hizmetine çağrı yapmak için gereklidir. Daha fazla bilgi için bkz. [Azure Machine Learning web hizmetini kullanma](consume-web-services.md).

**Azure depolama hesabım bulunamazsa ne olur?**

Machine Learning Studio, iş akışını yürütürken ara verileri kaydetmek için kullanıcı tarafından sağlanan bir Azure depolama hesabı kullanır. Bu depolama hesabı, bir çalışma alanı oluşturulduğunda Machine Learning Studio'ya sağlanır. Çalışma alanı oluşturulduktan sonra depolama hesabı silinir ve bulunamazsa çalışma alanı işlemeyi durdurur ve bu çalışma alanındaki tüm denemeler başarısız olur.

Depolama hesabını yanlışlıkla sildiyseniz, silinen depolama hesabıyla aynı bölgede aynı isimli bir depolama hesabını yeniden oluşturun. Bunun ardından erişim tuşunu yeniden eşitleyin.

**Depolama hesabımın erişim anahtarı eşitlenmemişse ne olur?**

Machine Learning Studio, iş akışını yürütürken ara verileri saklamak için kullanıcı tarafından sağlanan bir Azure depolama hesabı kullanır. Bu depolama hesabı, bir çalışma alanı oluşturulduğunda Machine Learning Studio'ya sağlanır ve erişim anahtarları bu çalışma alanıyla ilişkilendirilir. Çalışma alanı oluşturulduktan sonra erişim anahtarları değiştirilirse, çalışma alanı depolama hesabına bir daha erişemez. Çalışmayı durdurur ve bu çalışma alanındaki tüm denemeler başarısız olur.

Depolama hesabının erişim anahtarlarını değiştirdiyseniz, Azure portalını kullanarak çalışma alanındaki erişim anahtarlarını yeniden eşitleyin.  

## <a name="support-and-training"></a>Destek ve eğitim
**Azure Machine Learning için nereden eğitim alabilirim?**

[Azure Machine Learning Belge Merkezi](https://azure.microsoft.com/services/machine-learning/), video öğreticilerinin yanı sıra nasıl yapılır kılavuzlarını da barındırır. Bu adım adım kılavuzlar hizmetleri tanıtır ve Azure Machine Learning kullanarak verileri içeri aktarmanın, temizlemenin, tahmine dayalı modeller oluşturmanın ve bunları üretimde dağıtmanın veri bilimi yaşam döngüsünü açıklar.

Machine Learning Merkezi'ne düzenli olarak yeni materyaller ekleriz. Machine Learning Merkezi hakkında [kullanıcı geri bildirim forumunda](https://windowsazure.uservoice.com/forums/257792-machine-learning) ek öğrenme materyalleri için istek gönderebilirsiniz.

[Microsoft Virtual Academy](http://www.microsoftvirtualacademy.com/training-courses/getting-started-with-microsoft-azure-machine-learning)'de de eğitim bulabilirsiniz.

**Azure Machine Learning için nasıl destek alabilirim?**

Azure Machine Learning için teknik destek almak istiyorsanız [Azure Desteği](https://azure.microsoft.com/support/options/)'ne gidin ve **Machine Learning**'i seçin.

Azure Machine Learning'in hizmet hakkında sorular sorabileceğiniz bir MSDN topluluk forumu da bulunur. Forum, Azure Machine Learning ekibi tarafından izlenir. [Azure Forumu](https://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning)'na gidin.

## <a name="billing-questions"></a>Faturalama soruları
**Machine Learning için faturalama nasıl gerçekleşir?**

Azure Machine Learning iki bileşenden oluşur: Machine Learning Studio ve Machine Learning web hizmetleri.

Machine Learning Studio’yu değerlendirdiğiniz süre boyunca Ücretsiz faturalandırma katmanını kullanabilirsiniz. Ücretsiz katmanı, kapasitesi sınırlı olan bir klasik web hizmeti dağıtmanıza da imkan sağlar.

Azure Machine Learning’in ihtiyaçlarınıza uygun olduğuna karar verirseniz Standart katmanına kaydolabilirsiniz. Kaydolmak için bir Microsoft Azure aboneliğine sahip olmanız gerekir.

Standart katmanındaysanız Machine Learning Studio’da tanımladığınız her çalışma alanı için aylık olarak faturalandırılırsınız. Studio’da bir denemeyi çalıştırırken, deneme çalıştırma sırasındaki işlem kaynakları faturalandırılır. Klasik bir web hizmeti dağıttığınızda işlemler ve işlem saatleri Kullandıkça Öde temelinde faturalandırılır.

Yeni (Resource Manager tabanlı) web hizmetleri, maliyetlerde daha fazla öngörülebilirlik sağlayan faturalandırma planları sunar. Katmanlı fiyatlandırma modeli, büyük miktarda kapasiteye gereksinim duyan müşteriler için indirimli fiyatlar sunar.

Bir plan oluşturduğunuzda API işlem saatleri ve API işlemlerinin dahil edilen miktarı ile birlikte gelen sabit bir maliyete bağlı olursunuz. Daha fazla dahil edilen miktara ihtiyacınız olursa planınıza başka örnekler ekleyebilirsiniz. Çok daha fazla dahil edilen miktar gerekli olursa, çok daha fazla dahil edilen miktar ve daha iyi bir indirimli fiyat sağlayan daha yüksek katmanlı bir plan seçebilirsiniz.

Mevcut örneklerdeki dahil edilen miktarlar tüketildikten sonra gerçekleştirilen ek kullanım, faturalandırma planı katmanı ile ilişkili fazla kullanım fiyatı üzerinden ücretlendirilir.

> [!NOTE]
Dahil edilen miktarlar 30 günde bir yeniden ayrılır ve dahil edilen miktarların kullanılmayan kısmı sonraki döneme aktarılmaz.

Faturalama ve fiyatlandırma hakkında ek bilgi için bkz. [Machine Learning Fiyatlandırması](https://azure.microsoft.com/pricing/details/machine-learning/).

**Machine Learning'in ücretsiz deneme sürümü var mı?**

 Azure Machine Learning’in [Machine Learning Fiyatlandırması](https://azure.microsoft.com/pricing/details/machine-learning/) bölümünde açıklanan ücretsiz bir abonelik seçeneği vardır. [Machine Learning Studio](https://studio.azureml.net/?selectAccess=true&o=2)’da oturum açtığınızda size sekiz saatlik bir hızlı değerlendirme denemesi sunulur.

 Ayrıca, bir ücretsiz Azure deneme sürümüne kaydolduğunuzda herhangi bir Azure hizmetini bir ay süreyle deneyebilirsiniz. Azure ücretsiz deneme sürümü hakkında daha fazla bilgi için [Azure ücretsiz deneme sürümü ile ilgili SSS](https://azure.microsoft.com/pricing/free-trial-faq/) sayfasını ziyaret edin.

**İşlem nedir?**

İşlem, Azure Machine Learning’in yanıt verdiği bir API çağrısını temsil eder. İstek-Yanıt Hizmeti (RRS) ve Toplu Yürütme Hizmeti (BES) çağrılarındaki işlemler toplanır ve faturalandırma planınıza göre ücretlendirilir.

**Bir plana eklenen işlem miktarlarını, hem RRS hem de BES işlemleri için kullanabilir miyim?**

Evet, RRS ve BES işlemleriniz toplu hale getirilir ve faturalandırma planınıza göre ücretlendirilir.

**API işlem saati nedir?**

API işlem saati, API çağrılarının Machine Learning işlem kaynakları kullanılarak ne kadar süre çalıştırıldığına yönelik faturalandırma birimidir. Tüm çağrılarınız faturalandırma amacıyla toplanır.

**Tipik bir üretim API'si çağrısı ne kadar sürer?**

Üretim API’is çağrı süreleri önemli ölçüde değişiklik gösterebilir ve genellikle birkaç milisaniye ile birkaç saniye arasındadır. Bazı API çağrıları, veri işleme sürecinin karmaşıklığına ve makine öğrenimi modeline bağlı olarak dakika gerektirebilir. Üretim API’si çağrı sürelerini tahmin etmenin en iyi yolu Machine Learning hizmetindeki bir modelin kıyaslanmasıdır.

**Studio işlem saati nedir?**

Studio işlem saati, denemelerinizin Studio’daki işlem kaynaklarını kullandığı toplam süreye yönelik faturalandırma birimidir.

**Yeni (Azure Resource Manager tabanlı) web hizmetlerindeki Geliştirme/Test katmanı hangi amaca yöneliktir?**

Resource Manager tabanlı web hizmetleri, faturalandırma planınızı sağlamak için kullanabileceğiniz birden fazla katman sağlar. Geliştirme/Test fiyatlandırma katmanı, denemenizi herhangi bir maliyet olmaksızın yeni bir web hizmeti olarak test etmenize imkan tanıyan ve sınırlı oranda dahil edilen miktar sağlayan bir katmandır. Bunun nasıl çalıştığını görme fırsatınız vardır.

**Ayrı depolama ücretleri var mı?**

Machine Learning Ücretsiz katmanı ayrı depolama gerektirmez veya sağlamaz. Machine Learning Standart katmanı kullanıcıların bir Azure depolama hesabına sahip olmasını gerektirir. Azure Depolama [ayrı olarak faturalandırılır](https://azure.microsoft.com/pricing/details/storage/).

**Machine Learning yüksek kullanılabilirliği destekler mi?**

Evet. Ayrıntılı bilgi edinmek ve hizmet düzeyi sözleşmesinin (SLA) açıklamasını görmek için bkz. [Machine Learning Fiyatlandırması](https://azure.microsoft.com/pricing/details/machine-learning/).

**Üretim API'si çağrılarım, hangi belirli türdeki işlem kaynakları üzerinde çalıştırılır?**

Machine Learning hizmeti çok kiracılı bir hizmettir. Arka uçta kullanılan gerçek işlem kaynakları farklılık gösterir ve performans ve öngörülebilirlik sağlayacak şekilde iyileştirilir.

### <a name="management-of-new-resource-manager-based-web-services"></a>Yeni (Resource Manager tabanlı) web hizmetlerinin yönetimi
**Planımı silersem ne olur?**

Plan aboneliğinizden kaldırılır ve eşit olarak bölünmüş kullanım için faturalandırılırsınız.

> [!NOTE]
Bir web hizmeti tarafından kullanılmakta olan planları silemezsiniz. Planı silmek için web hizmetine yeni bir plan atamanız ya da web hizmetini silmeniz gerekir.

**Plan örneği nedir?**

Plan örneği, faturalandırma planınıza ekleyebileceğiniz dahil edilen miktarlar birimidir. Faturalandırma planınız için bir faturalandırma katmanı seçtiğinizde katman bir örnek ile birlikte gelir. Daha fazla dahil edilen miktar gerekli olursa planınıza seçili faturalandırma katmanının örneklerini ekleyebilirsiniz.

**Kaç plan örneği ekleyebilirim?**

Bir abonelikte Geliştirme/Test fiyatlandırma katmanının bir örneğine sahip olabilirsiniz.

Standart S1, Standart S2 ve Standart S3 katmanları için gerekli sayıda örnek ekleyebilirsiniz.

> [!NOTE]
Öngörülen kullanımınıza bağlı olarak, geçerli katmana örnek eklemektense daha fazla dahil edilen miktar içeren bir katmana yükseltmeniz daha hesaplı olabilir.

**Plan katmanımı değiştirirsem (yükseltme/eski sürümü yükleme) ne olur?**

Eski plan silinir ve geçerli kullanım eşit olarak bölünmüş temelde faturalandırılır. Sürenin geri kalanı için yükseltilen/indirgenen katmanın dahil edilen tüm miktarlarını içeren yeni bir plan oluşturulur.

> [!NOTE]
Dahil edilen miktarlar her dönem ayrılır ve dahil edilen miktarların kullanılmayan kısmı sonraki döneme aktarılmaz.

**Bir plandaki örnekleri artırdığımda ne olur?**

Miktarlar eşit olarak bölünmüş temelde dahil edilir ve etkili olması 24 saat sürebilir.

**Plandaki bir örneği sildiğimde ne olur?**

Örnek aboneliğinizden kaldırılır ve eşit olarak bölünmüş kullanım için faturalandırılırsınız.

### <a name="sign-up-for-new-resource-manager-based-web-services-plans"></a>Yeni (Resource Manager tabanlı) web hizmetleri için kaydolun
**Bir plana nasıl kaydolabilirim?**

Faturalandırma planları oluşturmanın iki yolu vardır.

Resource Manager tabanlı bir web hizmetini ilk kez dağıtırken mevcut bir planı seçebilir ya da yeni bir plan oluşturabilirsiniz.

Bu şekilde oluşturduğunuz planlar varsayılan bölgenizdedir ve web hizmetiniz bu bölgeye dağıtılır.

Hizmetinizi varsayılan bölgeden başka bölgelere dağıtmak istiyorsanız hizmetinizi dağıtmadan önce faturalandırma planlarınızı tanımlayabilirsiniz.

Bu durumda, Azure Machine Learning Web Hizmetleri portalında oturum açıp planlar sayfasına gidebilirsiniz. Buradan plan ekleyebilir, silebilir ve mevcut planları değiştirebilirsiniz.

**Başlangıçta hangi planı seçmem gerekir?**

Standart S1 katmanı ile başlangıç yapmanız ve hizmetinizin kullanımını izlemeniz önerilir. Dahil edilen miktarlarınızı hızlıca kullandığınızı fark ettiğinizde örnek ekleyebilir veya daha yüksek bir katmana geçip daha indirimli fiyatlardan yararlanabilirsiniz. Faturalandırma planlarınızı faturalandırma döngünüz boyunca gereken şekilde ayarlayabilirsiniz.

**Yeni planlar hangi bölgelerde kullanılabilir?**

Yeni faturalandırma planları yeni web hizmetlerini desteklediğimiz üç üretim bölgesinde kullanılabilir:

* Orta Güney ABD
* Batı Avrupa
* Güneydoğu Asya

**Birden çok bölgede web hizmetim var. Her bölge için bir planımın olması gerekiyor mu?**

Evet. Plan fiyatlandırması bölgeye göre değişir. Bir web hizmetini başka bir bölgeye dağıttığınızda, web hizmetine ilgili bölgeye özgü bir plan atamanız gerekir. Daha fazla bilgi için bkz. [Bölgelere göre kullanılabilir ürünler]( https://azure.microsoft.com/regions/services/).

### <a name="new-web-services-overages"></a>Yeni web hizmetleri: Fazla kullanım
**Web hizmeti kullanım sınırımı aşıp aşmadığımı nasıl kontrol edebilirim?**

Tüm planlarınızdaki kullanımı Azure Machine Learning Web Hizmetleri portalındaki Planlar sayfasından inceleyebilirsiniz. Portalda oturum açın ve **Planlar** menü seçeneğine tıklayın.

Tablonun **İşlemler** ve **İşlem** sütunlarında planın dahil edilen miktarlarını ve kullanılan yüzdeyi görebilirsiniz.

**Geliştirme/Test fiyatlandırma katmanında dahil edilen miktarların tamamını kullanırsam ne olur?**

Geliştirme/Test fiyatlandırma katmanı atanmış hizmetler sonraki döneme kadar veya ücretli katmanlardan birine taşınana kadar durdurulur.

**Klasik web hizmetleri için ve Yeni (Resource Manager tabanlı) web hizmetlerinin fazla kullanımlarında, İstek-Yanıt Hizmeti (RRS) ve Toplu İş Yürütme Hizmeti (BES) iş yükleri için fiyatlar nasıl hesaplanır?**

Bir RRS iş yükünde yaptığınız her API işlemi çağrısının yanı sıra bu isteklerle ilişkili işlem süreleri ücretlendirilir. RRS üretim API’si işlem maliyetleri, yaptığınız toplam API çağrısı sayısının 1.000 işlem başına fiyatla (tek işlem için eşit oranda bölünmüş şekilde) çarpılmasıyla hesaplanır. RRS API üretim API’si işlem saati maliyetleriniz her bir API çağrısının çalışması için gereken süre miktarının toplam API işlemi sayısıyla çarpılması ve çıkan sonucun üretim API’si için uygulanan işlem saati başına fiyatla çarpılmasıyla hesaplanır.

Örneğin, Standart S1 fazla kullanımı için her birinin çalışması 0,72 saniye süren 1.000.000 API işlemi, üretim API’si işlem maliyetlerinde (1.000.000 * 0,50 $/1000 API işlemi) 500 $ ve üretim API’si işlem saatlerinde (1.000.000 * 0,72 saniye * 2 $/saat) 400 $ olmak üzere toplam 900 $ ile sonuçlanır.

BES iş yükleri için de aynı şekilde ücretlendirilirsiniz. Bununla birlikte, API işlem maliyetleri gönderdiğiniz toplu iş sayısını, işlem maliyetleri ise bu toplu işlerle ilişkili işlem süresini temsil eder. BES üretim API’si işlem maliyetleri gönderdiğiniz toplam iş sayısının 1.000 işlem başına fiyatla (tek işlem için eşit oranda bölünmüş şekilde) çarpılmasıyla hesaplanır. BES API üretim API’si işlem saati maliyetleriniz, işinizdeki her satır için gereken süre miktarının, işinizdeki toplam satır sayısı çarpı toplam iş sayısı çarpı üretim API’si işlem saati başına fiyat ile çarpımı olarak hesaplanır. Machine Learning hesaplayıcısını kullanırken işlem ölçer, göndermeyi planladığınız iş sayısını; işlem alanı başına süreyse çalıştırılacak her bir işteki tüm satırlar için gereken birleşik süreyi temsil eder.

Örneğin, Standart S1 fazla kullanımı gerçekleştiğini ve günlük her biri 0,72 saniye süren 500’er satırdan oluşan 100 iş gönderdiğinizi varsayalım. Aylık fazla kullanım maliyetleriniz üretim API’si işlem maliyetlerinden (günde 100 iş = 3.100 iş/ay * 0,50 $/1000 API işlemi) 1,55 $ ve üretim API’si işlem saatlerinden (500 satır * 0,72 saniye * 3.100 İş * 2 $/sa) 620 $ olmak üzere toplam 621,55 $ olur.

### <a name="azure-machine-learning-classic-web-services"></a>Azure Machine Learning Klasik web hizmetleri
**Kullandıkça Öde seçeneği hâlâ mevcut mu?**

Evet, Azure Machine Learning’de Klasik web hizmetleri hala kullanılabilmektedir.  

### <a name="azure-machine-learning-free-and-standard-tier"></a>Azure Machine Learning Ücretsiz ve Standart katmanı
**Azure Machine Learning Ücretsiz katmanına neler dahildir?**

Azure Machine Learning Ücretsiz katmanı, Azure Machine Learning Studio’ya kapsamlı bir giriş sağlamaya yöneliktir. Kaydolmak için tek ihtiyacınız olan bir Microsoft hesabıdır. Ücretsiz katmanı her [Microsoft hesabı](https://account.microsoft.com/account) için bir Azure Machine Learning Studio çalışma alanına ücretsiz erişim içerir. Bu katmanda en fazla 10 GB depolama alanı kullanabilir ve modelleri hazırlama API'leri olarak operasyona dahil edebilirsiniz. Ücretsiz katmanı iş yükleri bir SLA kapsamında değildir ve yalnızca geliştirme ve kişisel kullanım amaçlıdır. 

Ücretsiz katmanı çalışma alanları aşağıdaki sınırlamalara sahiptir:

* İş yükleri, verilere erişmek için SQL Server çalıştıran yerinde bir sunucuya bağlanarak verilere erişemez.
* Yeni Resource Manager temel web hizmetleri dağıtılamaz.


**Azure Machine Learning Standart katmanına ve planlarına neler dahildir?**

Azure Machine Learning Standart katmanı, Azure Machine Learning Studio’nun ücretli bir üretim sürümüdür. Azure Machine Learning Studio’nun aylık ücreti çalışma alanı başına faturalandırılır ve kısmi aylar için eşit oranda bölünür. Azure Machine Learning Studio deneme saatleri, etkin deneme için işlem saati başına faturalandırılır. Fatura, kısmi saatler için eşit olarak bölünür.  

Azure Machine Learning API, hizmetin Klasik bir web hizmeti mi yoksa yeni (Resource Manager tabanlı) bir web hizmeti mi olduğuna göre faturalandırılır.

Aşağıdaki ücretler aboneliğiniz için çalışma alanı başına toplanır.

* Machine Learning Çalışma Alanı Aboneliği: Machine Learning çalışma alanı aboneliği, bir Machine Learning Studio çalışma alanına erişim sağlayan aylık bir ücrettir. Stüdyoda deneme çalıştırmak ve üretim API'lerini kullanmak için abonelik gerekir.
* Studio Deneme saatleri: Bu ölçüm, Machine Learning Studio’da deneme çalıştırmaktan ve hazırlık ortamında üretim API’si çağrılarını çalıştırmaktan doğan tüm işlem ücretlerinin toplamıdır.
* Modellerinizde eğitim ve puanlama amacıyla SQL Server çalıştıran bir şirket içi sunucuya bağlanarak verilere erişin.
* Klasik web hizmetleri için:
  * Üretim API’si İşlem Saatleri: Bu ölçüm, üretimde çalışan web hizmetlerinden doğan işlem ücretlerini içerir.
  * Üretim API’si İşlemleri (1000’lik bloklar halinde): Bu ölçüm, üretim web hizmetine yapılan her çağrıdan doğan ücretleri içerir.

Yukarıdaki ücretleri bir kenara bırakacak olursak, Resource Manager tabanlı bir web hizmeti söz konusu olduğunda ücretler seçili plana toplanır:

* Standart S1/S2/S3 API Planı (Birim): Bu ölçümler, Resource Manager tabanlı web hizmetleri için seçilen örneğin türünü temsil eder.
* Standart S2/S1/S3 Fazla Kullanım API İşlem Saatleri: Bu ölçüm, mevcut örneklerdeki dahil edilen miktarlar tüketildikten sonra üretimde çalışan Resource Manager tabanlı web hizmetlerinden doğan işlem ücretlerini içerir. Ek kullanım, S1/S2/S3 plan katmanıyla ilişkili fazla kullanım fiyatı üzerinden ücretlendirilir.
* Standart S1/S2/S3 Fazla Kullanım API İşlemleri (1.000’lik bloklar) - Bu ölçüm, mevcut örneklerdeki dahil edilen miktarlar tüketildikten sonra Resource Manager tabanlı üretim web hizmetinize yapılan çağrı başına doğan ücretleri içerir. Ek kullanım, S1/S2/S3 plan katmanıyla ilişkili fazla kullanım fiyatından ücretlendirilir.
* Dahil Edilen Miktar API İşlem Saatleri: Bu ölçüm, Resource Manager tabanlı web hizmetleri için dahil edilen API işlem saati miktarını temsil eder.
* Dahil Edilen Miktar API İşlemleri (1000'lik bloklar): Bu ölçüm, Resource Manager tabanlı web hizmetleri için dahil edilen API işlemi miktarını temsil eder.

**Azure Machine Learning Ücretsiz katmanına nasıl kaydolabilirim?**

Tek ihtiyacınız olan bir Microsoft hesabıdır. [Azure Machine Learning giriş sayfasına](https://azure.microsoft.com/services/machine-learning/) gidin ve **Şimdi Başlat**’a tıklayın. Microsoft hesabınızla oturum açtığınızda Ücretsiz katmanında sizin için bir çalışma alanı oluşturulur. Machine Learning denemelerini keşfetmeye ve oluşturmaya hemen başlayabilirsiniz.

**Azure Machine Learning Standart katmanına nasıl kaydolabilirim?**

Standart Machine Learning çalışma alanı oluşturmak için öncelikle bir Azure aboneliğine erişiminiz olmalıdır. 30 günlük Azure ücretsiz deneme aboneliğine kaydolabilir, sonrasında ücretli bir Azure aboneliğine yükseltebilir ya da hemen ücretli bir Azure aboneliği satın alabilirsiniz. Aboneliğe erişim elde ettikten sonra Microsoft Azure portalından bir Machine Learning çalışma alanı oluşturabilirsiniz. [Adım adım yönergelere](https://azure.microsoft.com/trial/get-started-machine-learning-b/) bakın.

Alternatif olarak, Standart Machine Learning çalışma alanı sahibi tarafından sahibin çalışma alanına erişmeye davet edilebilirsiniz.

**Ücretsiz katmanı ile kullanmak için kendi Azure Blob Depolama hesabımı belirtebilir miyim?**

Hayır, Standart katman Machine Learning hizmetinin katmanlar sunulmadan önce mevcut olan sürümüne eşdeğerdir.

**Makine öğrenimi modellerimi, Ücretsiz katmanında API olarak dağıtabilir miyim?**

Evet, makine öğrenimi modellerini Ücretsiz katmanının bir parçası olarak hazırlık API hizmetlerinde operasyona dahil edebilirsiniz. Hazırlık API hizmetini üretime sokmak ve operasyona dahil edilmiş hizmet için bir üretim uç noktası edinmek istiyorsanız Standart katmanını kullanmanız gerekir.

**Azure ücretsiz deneme sürümü ile Azure Machine Learning Ücretsiz katmanı arasındaki fark nedir?**

[Microsoft Azure ücretsiz deneme](https://azure.microsoft.com/free/) sürümü bir ay boyunca herhangi bir Azure hizmetine uygulayabileceğiniz krediler sunar. Azure Machine Learning Ücretsiz katmanı, üretim dışı iş yükleri için Azure Machine Learning’e özel olarak sürekli erişim imkanı sunar.

**Bir denemeyi Ücretsiz katmanından Standart katmanına nasıl taşıyabilirim?**

Denemelerinizi Ücretsiz katmanından Standart katmanına kopyalamak için:

1. Azure Machine Learning Studio’da oturum açın ve üst gezinti çubuğundaki çalışma alanı seçicisinde hem Ücretsiz çalışma alanını hem de Standart çalışma alanını görebildiğinizden emin olun.
2. Standart çalışma alanındaysanız Ücretsiz çalışma alanına geçin.
3. Deneme listesi görünümünde kopyalamak istediğiniz bir denemeyi seçin ve **Kopyala** komut düğmesine tıklayın.
4. Açılan iletişim kutusundan Standart çalışma alanını seçin ve **Kopyala** düğmesine tıklayın.
   Tüm ilişkili veri kümeleri, eğitilen model vb. denemeyle birlikte Standart çalışma alanına kopyalanır.
5. Denemeyi yeniden çalıştırmanız ve web hizmetinizi Standart çalışma alanınıza yeniden yayımlamanız gerekir.

### <a name="studio-workspace"></a>Studio çalışma alanı
**Farklı çalışma alanları için farklı faturalar görür müyüm?**

Çalışma alanı ücretleri tek bir fatura üzerinde, geçerli olan her bir ölçüm için ayrıca listelenir.

**Denemelerim hangi belirli türdeki işlem kaynakları üzerinde çalıştırılacak?**

Machine Learning hizmeti çok kiracılı bir hizmettir. Arka uçta kullanılan gerçek işlem kaynakları farklılık gösterir ve performans ve öngörülebilirlik sağlayacak şekilde iyileştirilir.

### <a name="guest-access"></a>Konuk Erişimi
**Azure Machine Learning Studio'ya Konuk Erişimi nedir?**

Konuk Erişimi kısıtlı bir deneme deneyimidir. Azure Machine Learning Studio’da herhangi bir maliyet olmadan ve kimlik doğrulaması gerçekleştirmeden denemeler oluşturup çalıştırabilirsiniz. Konuk oturumları kalıcı değildir (kaydedilemez) ve sekiz saatle sınırlıdır. Diğer sınırlamalar ise R ve Python desteğinin olmaması, hazırlık API’lerinin olmaması, kısıtlı veri kümesi boyutu ve kısıtlı depolama kapasitesidir. Buna karşılık olarak bir Microsoft hesabıyla oturum açan kullanıcılar Machine Learning Studio’nun daha önce açıklanan ve kalıcı bir çalışma alanı ile daha kapsamlı özellikler içeren Ücretsiz katmanına tam erişime sahip olur. Ücretsiz Machine Learning deneyiminizi seçmek için [https://studio.azureml.net](https://studio.azureml.net) sayfasındaki **Kullanmaya başlayın**’a tıklayıp **Konuk Erişimi**’ni seçin ya da bir Microsoft hesabı ile oturum açın.

<!-- Module References -->
[image-reader]: https://msdn.microsoft.com/library/azure/893f8c57-1d36-456d-a47b-d29ae67f5d84/
[join]: https://msdn.microsoft.com/library/azure/124865f7-e901-4656-adac-f4cb08248099/
[machine-learning-modules]: https://msdn.microsoft.com/library/azure/6d9e2516-1343-4859-a3dc-9673ccec9edc/
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[python]: https://msdn.microsoft.com/library/azure/CDB56F95-7F4C-404D-BDE7-5BB972E6F232
[counts]: https://msdn.microsoft.com/library/azure/dn913056.aspx
