---
title: Machine Learning Studio sık sorulan sorular (SSS)
titleSuffix: Azure Machine Learning Studio
description: 'Azure Machine Learning Studio: Faturalama, özellikler ve bir bulut hizmeti için kolaylaştırılmış Tahmine dayalı modelleme sınırlamaları kapsayan SSS.'
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 06/02/2017
ms.openlocfilehash: 8c381ca02dcfb194b2807735415ef894d72f74fa
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55462288"
---
# <a name="azure-machine-learning-studio-faq-capabilities-and-limitations"></a>Azure Machine Learning Studio SSS: özellikler ve sınırlamalar
Aşağıda, tahmine dayalı modeller geliştirmeye ve web hizmetleri aracılığıyla çözümleri faaliyete geçirmeye yönelik bir bulut hizmeti olan Azure Machine Learning hakkında sık sorulan bazı sorular (SSS) ve yanıtları verilmiştir. 

## <a name="general-questions"></a>Genel sorular
**Machine Learning Studio nedir?**

Machine Learning Studio'da bir web tarayıcısı kullanarak erişebileceğiniz bir Sürükle ve bırak tuvali ortamıdır. Machine Learning Studio, deneme şeklinde uçtan uca veri bilimi akışı oluşturmanıza yardımcı olan bir görsel birleştirme arabiriminde bir modül paleti barındırır.

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

* İstek-yanıt hizmeti (RRS): Düşük gecikme süreli, oluşturulan ve Machine Learning Studio kullanılarak dağıtılan durum bilgisiz modeller için arabirim sağlar yüksek oranda ölçeklenebilir bir hizmettir.
* Toplu yürütme hizmeti (BES): Zaman uyumsuz bir hizmettir, veri kayıtları için toplu iş.

REST API’sini kullanmanın ve web hizmetine erişmenin birkaç yolu vardır. Örneğin, web hizmetini dağıttığınızda sizin için oluşturulan örnek kodu kullanarak C#, R veya Python’da bir uygulama yazabilirsiniz.

Örnek koda şuradan ulaşabilirsiniz:
- Web hizmetinin Azure Machine Learning Web Hizmetleri portalındaki Kullanım sayfası
- Web hizmetinin Machine Learning Studio'daki panosunda bulunan API Yardım Sayfası

Ayrıca, sizin için oluşturulan ve Machine Learning Studio’daki web hizmeti panosunda bulunan örnek Microsoft Excel çalışma kitabını da kullanabilirsiniz.

**Azure Machine Learning için ana güncelleştirmeler nelerdir?**

En son güncelleştirmeler için bkz. [Azure Machine Learning’deki yenilikler](../../active-directory/fundamentals/whats-new.md).

## <a name="import-and-export-data-for-machine-learning"></a>Machine Learning için verileri içeri ve dışarı aktarma
**Machine Learning hangi veri kaynaklarını destekliyor?**

Bir Machine Learning Studio denemesine üç yöntemle veri indirebilirsiniz:

- Yerel bir dosyayı veri kümesi olarak karşıya yükleme
- Bir modülü kullanarak bulu veri hizmetlerinden içeri veri aktarma
- Başka bir denemeden kaydedilen veri kümesini içeri aktarma

Desteklenen dosya biçimleri hakkında daha fazla bilgi edinmek için bkz. [Machine Learning Studio'da eğitim verilerini içeri aktarma](import-data.md).

### <a id="ModuleLimit"></a>Veri kümesi, modüllerim için ne kadar büyüyebilir?
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

### <a id="UploadLimit"></a>Karşıya veri yükleme limitleri nelerdir?
Boyutu birkaç GB'den büyük olan veri kümeleri için, verileri doğrudan bir yerel dosyadan karşıya yüklemek yerine Azure Depolama veya Azure SQL Veritabanı’na yükleyin ya da Azure HDInsight’ı kullanın.

**Verileri Amazon S3'ten okuyabilir miyim?**

Az miktarda veriniz varsa ve bunu bir HTTP URL'si aracılığıyla kullanıma sunmak istiyorsanız [Verileri İçeri Aktarma][import-data] modülünü kullanabilirsiniz. Daha büyük miktarda olan verileri Azure Depolama'ya aktarın ve ardından [Verileri İçeri Aktarma][import-data] modülünü kullanarak denemenize aktarın.
<!--

<SEE CLOUD DS PROCESS>
-->

**Yerleşik bir görüntü girişi özelliği var mı?**

Görüntü girişi özelliği hakkında [Görüntüleri İçeri Aktarma][image-reader] başvurusundan bilgi edinebilirsiniz.

## <a name="modules"></a>Modüller
**Aradığım algoritma, veri kaynağı, veri biçimi veya veri dönüştürme işlemi Azure Machine Learning Studio'da yok. Seçeneklerim neler?**

Takip ettiğimiz özellik isteklerini görmek için [kullanıcı geri bildirim forumuna](https://go.microsoft.com/fwlink/?LinkId=404231) gidebilirsiniz. Aradığınız bir özellik zaten istenmişse isteğe oyunuzu ekleyin. Aradığınız özellik mevcut değilse yeni bir istek oluşturun. Bu forumda isteğinizin durumunu da görüntüleyebilirsiniz. Bu listeyi yakından takip edip özellik kullanılabilirliği durumunu sık sık güncelleştiriyoruz. Buna ek olarak, R ve Python için yerleşik desteği kullanarak gerektiğinde özel dönüştürmeler oluşturabilirsiniz.

**Var olan kodumu Machine Learning Studio'ya getirebilir miyim?**

Evet, var olan R veya Python kodunuzu Machine Learning Studio'ya getirebilir, Azure Machine Learning öğrencileriyle birlikte aynı denemede bunu çalıştırabilir ve Azure Machine Learning aracılığıyla çözümü bir web hizmeti olarak dağıtabilirsiniz. Daha fazla bilgi için bkz. [R ile denemenizi genişletme](extend-your-experiment-with-r.md) ve [Azure Machine Learning Studio'da Python makine öğrenimi betiklerini yürütme](execute-python-scripts.md).

**Bir modeli tanımlamak için [PMML](http://en.wikipedia.org/wiki/Predictive_Model_Markup_Language) benzeri bir dil kullanmak mümkün mü?**

Hayır, Predictive Model Markup Language (PMML) desteklenmez. Bir modülü tanımlamak için özel R ve Python kodu kullanabilirsiniz.

**Denememe paralel şekilde kaç modül yürütebilirim?**  

Bir denemede paralel şekilde en fazla dört modül yürütebilirsiniz.

## <a name="data-processing"></a>Veri işleme
**Denemede, verileri etkileşimli bir şekilde görselleştirme (R görselleştirmesinin ötesinde) özelliği var mı?**

Verileri görselleştirmek ve istatistikleri elde etmek için bir modülün çıkışına tıklayın.

**Bir tarayıcıda sonuçların veya verilerin önizlemesi yapıldığı sırada satır ve sütun sayısı sınırlı oluyor. Neden?**

Bir tarayıcıya büyük miktarda veri gönderilebileceğinden, Machine Learning Studio’nun yavaşlamaması için veri boyutu sınırlanır. Tüm verilerin/sonuçların görselleştirilmesi için verileri indirip Excel veya başka bir araç kullanmak daha iyidir.

## <a name="algorithms"></a>Algoritmalar
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

## <a name="r-module"></a>R modülü
**Machine Learning Studio'da hangi R paketleri kullanılabilir?**

Machine Learning Studio, şu an 400’ün üzerinde CRAN R paketini desteklemektedir ve buna dahil olan tüm paketlerin [güncel listesi](http://az754797.vo.msecnd.net/docs/RPackages.xlsx) buradadır. Ayrıca, bu listeyi nasıl alacağınızı öğrenmek için bkz. [Denemenizi R ile genişletme](extend-your-experiment-with-r.md). İstediğiniz paket bu listede değilse [kullanıcı geri bildirim forumunda](https://go.microsoft.com/fwlink/?LinkId=404231) paketin adını sağlayın.

**Özel bir R modülü oluşturulabilir mi?**

Evet, daha fazla bilgi için bkz. [Azure Machine Learning'de özel R modülleri yazma](custom-r-modules.md).

**R için bir REPL ortamı var mı?**

Hayır, Studio'da R için Read-Eval-Print-Loop (REPL) ortamı yoktur.

## <a name="python-module"></a>Python modülü
**Özel bir Python modülü oluşturulabilir mi?**

Şu anda değil ancak aynı sonucu elde etmek için bir veya daha fazla [Python Betiği Yürütme][python] modülünü kullanabilirsiniz.

**Python için bir REPL ortamı var mı?**

Machine Learning Studio'daki Jupyter Not Defterlerini kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure Machine Learning Studio'da Jupyter Not Defterlerine Giriş](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).

## <a name="web-service"></a>Web hizmeti

**Azure Machine Learning modellerini program aracılığıyla nasıl yeniden eğitebilirim?**

Yeniden eğitme API'lerini kullanın. Daha fazla bilgi için bkz. [Machine Learning modellerini programlama aracılığıyla yeniden eğitme](retrain-models-programmatically.md). Örnek kod [Microsoft Azure Machine Learning Yeniden Eğitme Demosu](https://azuremlretrain.codeplex.com/)'nda da kullanılabilir.

**Modeli yerel olarak veya İnternet bağlantısı olmayan bir uygulamada dağıtabilir miyim?**

Hayır.

**Tüm web hizmetleri için beklenen bir temel gecikme süresi var mı?**

Bkz. [Azure abonelik limitleri](../../azure-subscription-service-limits.md).

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

Satırları ve sütunları her maks .NET SORUMLULUĞUN sınırlı şunlardır: 2,147,483,647.

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


## <a name="billing-questions"></a>Faturalama soruları

Faturalama ve fiyatlandırma bilgileri için bkz. [Machine Learning Fiyatlandırması](https://azure.microsoft.com/pricing/details/machine-learning/).


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
