<properties
    pageTitle="Azure Machine Learning ile ilgili SSS | Microsoft Azure"
    description="Azure Machine Learning'e giriş: Kolaylaştırılmış tahmine dayalı modelleme için bir bulut hizmetinin faturalamasını, özelliklerini ve sınırlamalarını kapsayan SSS."
    keywords="machine learning giriş,tahmini modelleme,machine learning nedir"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/14/2016"
    ms.author="garye"/>


# Azure Machine Learning ile ilgili Sık Sorulan Sorular (SSS): Faturalama, özellikler, sınırlamalar ve destek

Bu SSS, tahmine dayalı modeller geliştirmeye ve web hizmetleri aracılığıyla çözümleri faaliyete geçirmeye yönelik bir bulut hizmeti olan Azure Machine Learning hakkındaki soruları yanıtlar. Bu SSS; faturalama modeli, özellikler, sınırlamalar ve destek dahil olmak üzere hizmetin kullanımı hakkındaki soruları kapsar.

## Genel sorular

**Azure Machine Learning nedir?**

Azure Machine Learning, tahmine dayalı analitik çözümleri bulutta oluşturmak, test etmek, işletmek ve yönetmek için kullanabileceğiniz tam olarak yönetilen bir hizmettir. Yalnızca bir tarayıcıyla oturum açabilir, verileri yükleyebilir ve anında makine öğrenimi denemelerine başlayabilirsiniz. Tahmine dayalı sürükle ve bırak modelleme, büyük bir modül paleti ve başlangıç şablonları kitaplığı, ortak makine öğrenimi görevlerini basit ve hızlı hale getirir.  Daha fazla bilgi için bkz. [Azure Machine Learning hizmetine genel bakış](https://azure.microsoft.com/services/machine-learning/). Machine learning'e önemli terminolojiyi ve kavramları kapsayan bir giriş için bkz. [Azure Machine Learning'e giriş](machine-learning-what-is-machine-learning.md).

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**Machine Learning Studio nedir?**

Machine Learning Studio, bir web tarayıcısı üzerinden erişilen bir çalışma ekranı ortamıdır. Machine Learning Studio, bir deneme şeklinde uçtan uca veri bilimi akışı oluşturmanızı sağlayan bir görsel birleştirme arabirimi ile bir modül paleti barındırır.

Machine Learning Studio hakkında daha fazla bilgi için bkz. [Machine Learning Studio nedir?](machine-learning-what-is-ml-studio.md).

**Machine Learning API'si hizmeti nedir?**

Machine Learning API'si hizmeti, Machine Learning Studio'da yerleşik olanlar gibi tahmine dayalı modelleri ölçeklenebilir ve hataya dayanıklı web hizmetleri olarak dağıtmanızı sağlar. Machine Learning API hizmeti tarafından oluşturulan web hizmetleri, harici uygulamalar ve tahmine dayalı analiz modelleriniz arasında iletişim için bir arabirim sağlayan REST API'leridir.

Daha fazla bilgi için bkz. [Bir Machine Learning web hizmetine bağlanma](machine-learning-connect-to-azure-machine-learning-web-service.md).

**Klasik web hizmetlerim nerede listeleniyor? Yeni Azure Resource Manager tabanlı web hizmetlerim nerede listelenir?**

Klasik web hizmetleri, web hizmetleri sekmesindeki [Machine Learning Studio](http://studio.azureml.net) içinde listelenir. Yeni Azure Resource Manager tabanlı web hizmetleri, [Microsoft Azure Machine Learning Web Hizmetleri](https://services.azureml.net/) portalında listelenir. Çapraz bir liste mevcut değildir.

## Microsoft Azure Machine Learning Web Hizmeti soruları

**Azure ML Web Hizmetleri nedir?**

Azure Machine Learning web hizmeti sayesinde bir dış uygulama, Machine Learning iş akışı puanlama modeli ile gerçek zamanlı olarak iletişim kurar. Machine Learning web hizmeti çağrısı bir dış uygulamaya tahmin sonuçlarını döndürür. Machine Learning web hizmeti çağrısı yapmak için, web hizmetini dağıttığınızda oluşturulan bir API anahtarı geçirirsiniz. Machine Learning web hizmeti, web programlama projeleri için popüler bir mimari seçimi olan REST’i temel alır.

Azure Machine Learning iki tür hizmet içerir:

* İstek-Yanıt Hizmeti (RRS) - Machine Learning Studio’dan oluşturulup dağıtılan durum bilgisiz modeller için arabirim sağlayan düşük gecikmeli, yüksek düzeyde ölçeklenebilir bir hizmettir.
* Toplu Yürütme Hizmeti (BES) - Veri kayıtları için toplu iş yapan zaman uyumsuz bir hizmettir.

REST API’sini kullanmanın ve web hizmetine erişmenin birkaç yolu vardır. Örneğin, web hizmetini dağıttığınızda sizin için oluşturulan örnek kodu kullanarak C#, R veya Python’da bir uygulama yazabilirsiniz.

Örnek kodu şurada bulunur: Azure Machine Learning Web Hizmetleri portalında Web hizmetinin Kullanım sayfası, Machine Learning Studio web hizmeti panosundaki API Yardım Sayfası.

Veya sizin için oluşturulan örnek Microsoft Excel çalışma kitabını kullanabilirsiniz (yine Studio web hizmeti panosunda mevcuttur).

**Yeni Azure ML Web Hizmetleri ile ilgili ana güncelleştirmeler nelerdir?**

Yeni Azure Machine Learning Web Hizmetleri hakkında daha fazla bilgi için [ilgili belgelere](machine-learning-whats-new.md) başvurun.

## Machine Learning Studio soruları

### Deneme oluşturma

**Deneme grafikleri için sürüm denetimi veya Git tümleştirmesi var mı?**

Hayır ancak Machine Learning Studio, bir denemenin diğer kullanıcılar tarafından değiştirilemeyen her bir yinelemesini korur.
Daha fazla bilgi için bkz. [Machine Learning Studio'da deneme yinelemelerini yönetme](machine-learning-manage-experiment-iterations.md).


### Deneme dağıtma

**Tahmini bir denemeyi daha önce klasik web hizmeti olarak dağıttıysam yeni (Azure Resource Manager tabanlı) bir web hizmeti olarak dağıtabilir miyim?**

Hayır, daha önce klasik web hizmeti olarak dağıtılmış bir denemeyi dağıtamazsınız. Bunun yerine yeni bir tahmini deneme oluşturup dağıtmanız gerekir.


### Machine Learning için verileri içeri ve dışarı aktarma

**Machine Learning hangi veri kaynaklarını destekliyor?**

Bir Machine Learning Studio denemesine üç şekilde veri yüklenebilir: yerel bir dosyayı bir veri kümesi olarak yükleyerek, bulut veri hizmetlerinden verileri içeri aktarmak için bir modül kullanarak veya başka bir denemeden kaydedilen bir veri kümesini içeri aktararak. Desteklenen dosya biçimleri hakkında daha fazla bilgi edinmek için bkz. [Machine Learning Studio'da eğitim verilerini içeri aktarma](machine-learning-data-science-import-data.md).


#### <a id="ModuleLimit"></a>Modüllerim için veri kümesi ne kadar büyük olabilir?

Machine Learning Studio'daki modüller, ortak kullanım durumları için en fazla 10 GB boyutunda yoğun sayısal verili veri kümelerini destekler. Bir modülün birden fazla giriş aldığı durumlarda 10 GB tüm giriş boyutlarının toplamıdır. Hive veya Azure SQL Database sorguları ya da alımdan önce Sayımlarla Öğrenme ön işlemesi aracılığıyla daha büyük veri kümelerinden de örnek oluşturabilirsiniz.  

Aşağıdaki veri türleri, özellik normalleştirme sırasında daha büyük veri kümelerine genişleyebilir ve boyutu 10 GB'tan az olacak şekilde sınırlıdır:

- Seyrek
- Kategorik
- Dizeler
- İkili veriler

Aşağıdaki modüller, boyutu 10 GB'tan az veri kümeleriyle sınırlıdır:

- Öneren modüller
- SMOTE modülü
- Betik modülleri: R, Python, SQL
- Katılma veya Özellik Karma gibi çıktı veri boyutunun girdi veri boyutundan büyük olabileceği modüller.
- Yinelemelerin sayısının çok büyük olduğu durumlarda Çapraz doğrulama, Model Ayarlama Hiperparametreleri, Sıralı Regresyon ve Tek veya Tüm Çoklu Sınıflar.

Birkaç GB boyutundan büyük veri kümeleri için, doğrudan yerel dosyadan yüklemek yerine, verileri Azure Storage veya Azure SQL Database'e yüklemeniz ya da HDInsight kullanmanız gerekir.


####<a id="UploadLimit"></a>Veri yükleme sınırları nelerdir?
Boyutu birkaç GB'ı aşan büyük veri kümeleri için, doğrudan yerel dosyadan yüklemek yerine, verileri Azure Storage veya Azure SQL Database'e yüklemeniz ya da HDInsight kullanmanız gerekir.

**Verileri Amazon S3'ten okuyabilir miyim?**

Az miktarda veriniz varsa ve bunu bir http URL'si aracılığıyla kullanıma sunmak istiyorsanız [Import Data (Verileri İçeri Aktarma)][import-data] modülünü kullanabilirsiniz. Daha büyük bir veri miktarını öncelikle Azure Storage'a aktarın ve ardından bunu denemenize getirmek için [Import Data (Verileri İçeri Aktarma)][import-data] modülünü kullanın.
<!--
<SEE CLOUD DS PROCESS>
-->

**Yerleşik bir görüntü girişi özelliği var mı?**

Görüntü girişi özelliği hakkında [Import Images (Görüntüleri İçeri Aktarma)][image-reader] başvurusundan bilgi edinebilirsiniz.

### Modüller

**Aradığım algoritma, veri kaynağı, veri biçimi veya veri dönüştürme işlemi Azure Machine Learning Studio'da yok. Seçeneklerim neler?**

Takip ettiğimiz özellik isteklerini görmek için [kullanıcı geri bildirim forumunu](http://go.microsoft.com/fwlink/?LinkId=404231) ziyaret edebilirsiniz. Aradığınız bir özellik zaten istenmişse isteğe oyunuzu ekleyin. Aradığınız özellik mevcut değilse yeni bir istek oluşturun. Bu forumda isteğinizin durumunu da görüntüleyebilirsiniz. Bu listeyi yakından takip edip özellik kullanılabilirliği durumunu sık sık güncelleştiriyoruz. Buna ek olarak, R ve Python için yerleşik destekle gerektiğinde özel dönüştürmeler oluşturulabilir.


**Var olan kodumu Machine Learning Studio'ya getirebilir miyim?**

Evet, var olan R veya Python kodunuzu Machine Learning Studio'ya getirebilir, Azure Machine Learning öğrencileriyle birlikte aynı denemede bunu çalıştırabilir ve Azure Machine Learning aracılığıyla çözümü bir web hizmeti olarak dağıtabilirsiniz. Daha fazla bilgi için bkz. [R ile denemenizi genişletme](machine-learning-extend-your-experiment-with-r.md) ve [Azure Machine Learning Studio'da Python makine öğrenimi betiklerini yürütme](machine-learning-execute-python-scripts.md).

**Bir modeli tanımlamak için [PMML](http://en.wikipedia.org/wiki/Predictive_Model_Markup_Language) gibi bir şey kullanmak mümkün mü?**

Hayır, bu desteklenmiyor ancak özel R ve Python kodu bir modülün tanımlanması için kullanılabilir.

**Denememe paralel şekilde kaç tane modül yürütebilirim?**  

Bir denemede paralel şekilde en fazla dört modül yürütebilirsiniz.


### Veri işleme

**Denemenin içinde verileri etkileşimli bir şekilde görselleştirme (R görselleştirmesinin ötesinde) yeteneği var mı?**

Bir modülün çıkışına tıklayarak verileri görselleştirebilir ve istatistikleri elde edebilirsiniz.

**Tarayıcıda sonuçları veya verileri önizlerken satır ve sütunların sayısı sınırlı; bunun nedeni nedir?**

Tarayıcıya veri aktarıldığından ve bu veri büyük olabileceğinden, Machine Learning Studio'nun yavaşlamasının önlenmesi için veri boyutu sınırlanır. Tüm verinin/sonucun görselleştirilmesi için, verileri indirip Excel veya başka bir araç kullanılması daha iyi olur.

### Algoritmalar

**Machine Learning Studio'da hangi var olan algoritmalar desteklenmektedir?**

Machine Learning Studio, Microsoft Research'te geliştirilen Ölçeklenebilir Artırılmış Karar ağaçları, Bayes Önerisi sistemleri, Derin Sinir Ağları ve Karar Ormanları gibi teknoloji harikası algoritmaları sağlar. Vowpal Wabbit gibi ölçeklenebilir açık kaynaklı makine öğrenimi paketleri de dahildir. Machine Learning Studio, çok sınıflı ve ikili sınıflandırma, regresyon ve kümeleme için makine öğrenimi algoritmalarını destekler. [Machine Learning Modüllerinin][machine-learning-modules] tam listesine bakın.

**Verilerim için kullanılacak doğru Machine Learning algoritmasını otomatik olarak öneriyor musunuz?**

Hayır ancak sorununuz için doğru olana karar vermek amacıyla her bir algoritmanın sonucunu karşılaştırmaya yönelik çeşitli yollar Machine Learning Studio'da mevcuttur.

**Sağlanan algoritmalar için bir algoritma yerine başkasını seçmeye yönelik herhangi bir yönerge var mı?**
Bkz. [Algoritma seçme](machine-learning-algorithm-choice.md).

**Sağlanan algoritmalar R veya Python ile mi yazılmış?**

Hayır, bu algoritmalar genellikle daha yüksek performans sağlamak için derlenmiş dillerde yazılır.

**Sağlanan algoritmaların herhangi bir ayrıntısı var mı?**

Belgeler algoritmalar hakkında bazı bilgiler sağlar ve ayarlamak için sağlanan parametreler, kullanımınız için algoritmayı en iyi duruma getirme amacıyla açıklanır.  

**Çevrimiçi eğitim için herhangi bir destek var mı?**

Hayır, şu anda yalnızca programlama yoluyla yeniden eğitim desteklenir.

**Yerleşik Modülü kullanarak bir Sinir Ağı Modelinin katmanlarını görselleştirebilir miyim?**

Hayır.

**C# veya başka bir dilde kendi modüllerimi oluşturabilir miyim?**

Şu anda yeni özel modüller yalnızca R'de oluşturulabilir.

### R modülü

**Machine Learning Studio'da hangi R paketleri kullanılabilir?**

Machine Learning Studio, şu an 400 + CRAN R paketlerini destekler ve dahil olan tüm paketlerin [güncel listesi](http://az754797.vo.msecnd.net/docs/RPackages.xlsx) buradadır. Ayrıca, bu listeyi nasıl alacağınızı öğrenmek için bkz. [Denemenizi R ile genişletme](machine-learning-extend-your-experiment-with-r.md). İstediğiniz paket bu listede değilse [kullanıcı geri bildirim forumunda](http://go.microsoft.com/fwlink/?LinkId=404231) paketin adını sağlayın.

**Özel bir R modülü oluşturmak mümkün müdür?**

Evet, daha fazla bilgi için bkz. [Azure Machine Learning'de özel R modülleri yazma](machine-learning-custom-r-modules.md).

**R için bir REPL ortamı var mıdır?**

Hayır, Studio'da R için REPL ortamı yoktur.

### Python modülü

**Özel bir Python modülü oluşturmak mümkün müdür?**

Şu anda değil ancak aynı sonucu elde etmek için bir veya daha fazla [Python Betiği Yürütme][python] modülünü kullanabilirsiniz.

**Python için bir REPL ortamı var mıdır?**

Machine Learning Studio'daki Jupyter Not Defterlerini kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure Machine Learning Studio'da Jupyter Not Defterlerine Giriş] (http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).

## Web hizmeti

###Programlama Aracılığıyla Modelleri Yeniden Eğitme

**Programlama aracılığıyla Azure Machine Learning modellerini nasıl yeniden eğitebilirim?**

Yeniden Eğitme API'lerini kullanın. daha fazla bilgi için bkz. [Machine Learning modellerini programlama aracılığıyla yeniden eğitme](machine-learning-retrain-models-programmatically.md). Örnek kod [Microsoft Azure Machine Learning Yeniden Eğitme Demosu](https://azuremlretrain.codeplex.com/)'nda da kullanılabilir.

### Oluştur

**Modeli yerel olarak veya bir internet bağlantısı olmayan bir uygulamada dağıtabilir miyim?**

Hayır.


**Tüm web hizmetleri için beklenen temel gecikme süresi var mı?**

Bkz. [Azure abonelik limitleri](../azure-subscription-service-limits.md)

### Kullanım

**Tahmine dayalı modelimi Batch hizmeti ile İstek Yanıtı hizmeti karşılaştırması olarak çalıştırmayı ne zaman isterim?**

İstek Yanıtı hizmeti (RRS), deneme ortamından oluşturulan ve dağıtılan durum bilgisi olmayan modellere bir arabirim sağlamak için kullanılan düşük gecikme süreli ve yüksek ölçekli bir web hizmetidir. Batch hizmeti (BES), toplu veri kayıtlarının uyumsuz şekilde puanlamasına yönelik bir hizmettir. BES girişi, RRS'te kullanılan veri girişine benzerdir. Ana farklılık, BES'in Azure'daki Blob hizmeti ve Tablo hizmeti, Azure SQL Database, HDInsight (hive sorgusu) ve HTTP kaynakları gibi çeşitli kaynaklardan bir kayıt bloğunu okumasıdır. Daha fazla bilgi için bkz. [Machine Learning web hizmetlerini kullanma](machine-learning-consume-web-services.md).

**Dağıtılan web hizmeti için modeli nasıl güncelleştiririm?**

Zaten dağıtılmış olan bir hizmet için tahmine dayalı bir modeli güncelleştirme, eğitilen modeli yazmak ve kaydetmek için kullandığınız denemeyi değiştirmek ve yeniden çalıştırmak kadar kolaydır. Yeni bir kullanılabilen eğitim modeli sürümüne sahip olduğunuzda, Machine Learning Studio size web hizmetini güncelleştirmek isteyip istemediğinizi sorar. Dağıtılmış bir web hizmetini güncelleştirmek hakkında ayrıntılar için bkz. [Bir Machine Learning web hizmetini dağıtma](machine-learning-publish-a-machine-learning-web-service.md).

Yeniden Eğitme API'lerini de kullanabilirsiniz.
Daha fazla bilgi için bkz. [Machine Learning modellerini programlama aracılığıyla yeniden eğitme](machine-learning-retrain-models-programmatically.md). Örnek kod [Microsoft Azure Machine Learning Yeniden Eğitme Demosu](https://azuremlretrain.codeplex.com/)'nda da kullanılabilir.

**Üretimde dağıtılan web hizmetimi nasıl izleyebilirim?**

Tahmine dayalı bir model dağıtıldıktan sonra, modeli Klasik Azure portalından izleyebilirsiniz. Dağıtılan her bir hizmet kendine panosuna sahiptir ve burada bu hizmetin izleme bilgilerini görebilirsiniz. Dağıtılmış web hizmetlerinizi yönetme hakkında daha fazla bilgi için bkz. [Azure Machine Learning çalışma alanını yönetme](machine-learning-manage-workspace.md).

**RRS/BES'imin çıktısını görebileceğim bir yer var mı?**

RRS için sonucu genellikle web hizmeti yanıtında görürsünüz. Bunu Azure blob depolama alanına da yazabilirsiniz. BES çıktısı varsayılan olarak bir bloba yazılır. Ayrıca, [Export Data (Verileri Dışarı Aktarma)][export-data] modülünü kullanarak çıktıyı bir veritabanına veya tabloya yazabilirsiniz.

**Web hizmetlerini yalnızca Machine Learning Studio'da oluşturulan modellerden mi oluşturabilirim?**

Hayır, web hizmetleri doğrudan Jupyter Not Defterlerinden ve RStudio'dan da oluşturabilirsiniz.

**Hata kodları hakkında bilgileri nerede bulabilirim?**

Hata kodları ve açıklamaları listesi için bkz: [Machine Learning Hata Kodları](https://msdn.microsoft.com/library/azure/dn905910.aspx).

## Ölçeklenebilirlik

**Web hizmetinin ölçeklenebilirliği nedir?**

Şu anda varsayılan uç nokta, uç nokta başına 20 eş zamanlı RRS isteği ile sağlanır. Bunu uç nokta başına 200 eş zamanlı isteğe ölçeklendirebilirsiniz ve [Web Hizmetini Ölçeklendirme](machine-learning-scaling-webservice.md)'de açıklandığı gibi her bir web hizmetini, web hizmeti başına 10.000 uç noktaya ölçeklendirebilirsiniz. BES için her bir uç nokta, her seferinde 40 isteği işlemeye olanak sağlar ve 40'ı aşan ek istekler kuyruğa alınır. Bu kuyruğa alınan istekler, sıra azaldıkça otomatik olarak çalıştırılır.


**R işleri düğümlere yayılır mı?**

Hayır.  


**Eğitim içim ne kadar veri kullanabilirim?**

Machine Learning Studio'daki modüller, ortak kullanım durumları için en fazla 10 GB boyutunda yoğun sayısal verili veri kümelerini destekler. Bir modülün birden fazla girdi aldığı durumlarda tüm girdilerin toplam boyutu 10 GB'tır. Hive veya Azure SQL Database sorguları ya da alımdan önce [Learning with Counts (Sayımlarla Öğrenme)][counts] modülleriyle ön işleme aracılığıyla daha büyük veri kümelerinden de örnek oluşturabilirsiniz.  

Aşağıdaki veri türleri, özellik normalleştirme sırasında daha büyük veri kümelerine genişleyebilir ve boyutu 10 GB'tan az olacak şekilde sınırlıdır:

- seyrek
- kategorik
- dizeler
- ikili veriler

Aşağıdaki modüller, boyutu 10 GB'den az veri kümeleriyle sınırlıdır:

- Öneren modüller
- SMOTE modülü
- Betik modülleri: R, Python, SQL
- Katılma veya Özellik Karma gibi çıktı veri boyutunun girdi veri boyutundan büyük olabileceği modüller.
- Yinelemelerin sayısı çok büyük olduğu durumlarda Çapraz Doğrulama, Model Ayarlama Hiperparametreleri, Sıralı Regresyon ve Tek veya Tüm Çoklu Sınıflar.

Boyutu birkaç GB'ı aşan veri kümeleri için, doğrudan yerel dosyadan yüklemek yerine, verileri Azure Storage veya Azure SQL Database'e yüklemeniz ya da HDInsight kullanmanız gerekir.


**Herhangi bir vektör boyutu sınırlaması var mı?**

Satırların ve sütunların her biri 2.147.483.647 Maks. Tamsayısı .NET sınırlamasına tabidir.

**Web hizmetini çalıştırmak için kullanılan sanal makinenin boyutu ayarlanabilir mi?**

Hayır.  

## Güvenlik ve kullanılabilirlik

**Varsayılan olarak web hizmeti için http uç noktasına kimin erişimi var? Uç noktasına erişimi nasıl kısıtlarım?**

Bir web hizmeti dağıtıldıktan sonra, bu hizmet için varsayılan bir uç noktası oluşturulur. Varsayılan uç noktası, API anahtarı kullanılarak çağrılabilir. Ek uç noktaları klasik Azure portalından veya programlama aracılığıyla Web Hizmeti Yönetimi API'leri kullanılarak kendi anahtarlarıyla eklenebilir. Erişim tuşları, web hizmetine çağrı yapmak için gereklidir. Daha fazla bilgi için bkz. [Bir Machine Learning web hizmetine bağlanma](machine-learning-connect-to-azure-machine-learning-web-service.md).


**Azure Storage hesabım bulunamazsa ne olur?**

Machine Learning Studio, iş akışı yürütülürken ara verileri kaydetmek için kullanıcı tarafından sağlanan bir Azure depolama hesabı kullanır. Bu depolama hesabı, bir çalışma alanı oluşturulduğunda Machine Learning Studio'ya sağlanır. Çalışma alanı oluşturulduktan sonra depolama hesabı silinir ve bulunamazsa çalışma alanı işlemeyi durdurur ve bu çalışma alanındaki tüm denemeler başarısız olur.

Depolama hesabını yanlışlıkla sildiyseniz, silinen depolama hesabıyla aynı bölgede aynı isimli bir depolama hesabını yeniden oluşturun. Bunun ardından Erişim Tuşu'nu yeniden eşitleyin.


**Depolama hesabımın erişim anahtarı eşitlenmemişse ne olur?**

Machine Learning Studio, iş akışı yürütülürken ara verileri depolamak için kullanıcı tarafından sağlanan bir Azure depolama hesabı kullanır. Bu depolama hesabı, bir çalışma alanı oluşturulduğunda Machine Learning Studio'ya sağlanır ve Erişim Anahtarları da bu çalışma alanıyla ilişkilendirilir. Erişim Tuşları değiştirilirse çalışma alanı oluşturulduktan sonra depolama hesabına bir daha erişemez. Çalışmayı durdurur ve bu çalışma alanındaki tüm denemeler başarısız olur.

Depolama hesabının Erişim Anahtarlarını değiştirdiyseniz klasik Azure portalını kullanarak çalışma alanındaki Erişim Anahtarlarını yeniden eşitleyin.  


## Azure Market

Bkz. [Machine Learning Marketi'ndeki uygulamaları yayımlama ve kullanma ile ilgili SSS](machine-learning-marketplace-faq.md).

## Destek ve eğitim

**Azure Machine Learning için nereden eğitim alabilirim?**

[Azure Machine Learning Belge Merkezi](https://azure.microsoft.com/services/machine-learning/), video öğreticilerinin yanı sıra nasıl yapılır kılavuzlarını da barındırır. Bu adım adım kılavuzlar, hizmetlere bir giriş sağlar ve Azure Machine Learning ile verileri içeri aktarmanın, verileri temizlemenin, tahmine dayalı modeller oluşturmanın ve bunları üretimde dağıtmanın veri bilimi yaşam döngüsünde rehberlik sunar.

Machine Learning Merkezi'ne düzenli olarak yeni materyaller ekliyor olacağız. Machine Learning Merkezi hakkında [kullanıcı geri bildirim forumunda](https://windowsazure.uservoice.com/forums/257792-machine-learning) ek öğrenme materyalleri için istek gönderebilirsiniz.

[Microsoft Virtual Academy](http://www.microsoftvirtualacademy.com/training-courses/getting-started-with-microsoft-azure-machine-learning)'de de eğitim bulabilirsiniz.

**Azure Machine Learning için nasıl destek alabilirim?**

Azure Machine Learning için teknik destek almak için [Azure Desteği](/support/options/)'ne gidin ve **Machine Learning**'i seçin.

Azure Machine Learning'in, Azure Machine Learning ile ilgili sorular sorabileceğiniz bir MSDN topluluk forumu da bulunur. Forum, Azure Machine Learning ekibi tarafından izlenir. [Azure Forumu](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning)'nu ziyaret edin.

## Faturalama soruları

**Machine Learning faturalaması nasıl çalışır?**

Azure Machine Learning hizmetinin iki bileşeni vardır. Machine Learning Studio ve Machine Learning Web Hizmetleri.

Machine Learning Studio’yu değerlendirirken ücretsiz faturalandırma katmanını kullanabilirsiniz.  Ücretsiz katmanı, bir klasik web hizmetini sınırlı kapasite ile dağıtmanızı sağlar.

Azure Machine Learning’in ihtiyaçlarınıza uygun olduğuna karar verdikten sonra standart katmanı için kaydolabilirsiniz. Kaydolmak için bir Microsoft Azure Aboneliğine sahip olmanız gerekir.

Standart katmanında Machine Learning Studio’da tanımladığınız her çalışma alanı için aylık fatura alırsınız. Studio’da bir denemeyi çalıştırırken, deneme çalıştırma sırasındaki işlem kaynakları faturalandırılır. Klasik Web Hizmeti dağıttığınızda işlemler ve işlem saatleri Kullandıkça Öde (PAYG) temelinde faturalandırılır.

Yeni Machine Learning Web Hizmetleri, maliyetlerde daha fazla öngörülebilirlik sağlayan Faturalandırma Planları sunar. Katmanlı fiyatlandırma büyük miktarda kapasiteye gereksinim duyan müşteriler için indirimli fiyatlar sunar.

Bir plan oluşturduğunuzda API işlem saatleri ve API işlemlerinin dahil edilen miktarı ile birlikte gelen sabit bir maliyete bağlı olursunuz. Daha fazla dahil edilen miktar gerekli olursa planınıza başka örnekler ekleyebilirsiniz. Çok daha fazla dahil edilen miktar gerekli olursa, çok daha fazla dahil edilen miktar ve daha iyi bir indirimli fiyat sağlayan daha yüksek katmanlı bir plan seçebilirsiniz.

Var olan örneklerdeki dahil edilen miktarlar tüketildikten sonra, faturalandırma planı katmanı ile ilişkili fazla kullanım fiyatından ek kullanım ücretlendirilir.

Not: Dahil edilen miktarlar 30 günde bir yeniden ayrılır ve kullanılmamış dahil edilen miktarlar sonraki döneme aktarılmaz.

Faturalama ve fiyatlandırma hakkında ek bilgi için bkz. [Machine Learning Fiyatlandırması](https://azure.microsoft.com/pricing/details/machine-learning/).

**Machine Learning'in ücretsiz deneme sürümü var mı?**

 Azure Machine Learning bir ücretsiz abonelik seçeneğine (ayrıntılar için bkz. [Machine Learning Fiyatlandırması](https://azure.microsoft.com/pricing/details/machine-learning/)), Machine Learning Studio ise 8 saatlik bir hızlı değerlendirme deneme sürümüne sahiptir (bu deneme sürümü için [Machine Learning Studio](https://studio.azureml.net/?selectAccess=true&o=2)’da oturum açın).

 Ayrıca, bir ücretsiz Azure deneme sürümüne kaydolduğunuzda herhangi bir Azure hizmetini bir ay süreyle deneyebilirsiniz. Azure ücretsiz deneme sürümü hakkında daha fazla bilgi için [Azure Ücretsiz Deneme Sürümü ile ilgili SSS](/pricing/free-trial-faq/).

**İşlem nedir?**

İşlem, Azure Machine Learning’in yanıt verdiği bir API çağrısını temsil eder. İstek-Yanıt Hizmeti (RRS) ve Toplu Yürütme Hizmeti (BES) çağrılarındaki işlemler toplanır ve faturalandırma planınıza göre ücretlendirilir.

**Bir plandaki dahil edilen işlem miktarlarını hem RRS hem de BES işlemleri için kullanabilir miyim?**

Evet, RRS ve BES işlemleriniz toplu hale getirilir ve faturalandırma planınıza göre ücretlendirilir.

**API işlem saati nedir?**

API işlem saati, ML işlem kaynakları kullanılarak API Çağrılarının çalışması için geçen süreye yönelik faturalandırma birimidir. Tüm çağrılarınız faturalandırma amacıyla toplanır.

**Tipik bir üretim API’si çağrısı ne kadar sürer?**

Üretim API’si çağrı süreleri genellikle yüzlerce milisaniye ile birkaç saniye arasında olmak üzere büyük ölçüde farklılık gösterebilir, ancak veri işleme karmaşıklığına ve makine öğrenimi modeline bağlı olarak birçoğu birkaç dakika gerektirebilir. Üretim API’si çağrı sürelerini tahmin etmenin en iyi yolu Machine Learning hizmetindeki bir modelin kıyaslanmasıdır.

**Studio İşlem saati nedir?**

Studio İşlem saati, denemelerinin Studio’da işlem kaynaklarını kullandığı toplam süreye yönelik faturalandırma birimidir.

**Yeni Web Hizmetleri’nde geliştirme/test katmanının amacı nedir?**

Azure ML yeni Web Hizmetleri, faturalandırma planınızı sağlamak için kullanabileceğiniz birden fazla katman sağlar. Geliştirme/test katmanı, denemenizi herhangi bir maliyet olmadan yeni bir web hizmeti olarak test etmenize imkan tanıyan ve sınırlı dahil edilen miktarlar sağlayan bir katmandır. Nasıl çalıştığını görmek için "Satın Almadan Önce İnceleme" fırsatınız vardır.

**Ayrı depolama ücretleri var mı?**

Machine Learning Ücretsiz katmanı ayrı depolama gerektirmez veya sağlamaz. Machine Learning Standart katmanı kullanıcıların bir Azure depolama hesabına sahip olmasını gerektirir. Azure depolama hizmeti [ayrı olarak faturalandırılır](https://azure.microsoft.com/pricing/details/storage/).

**Machine Learning yüksek kullanılabilirlik çalışmasını nasıl destekler?**

Üretim API’si çağrı süreleri genellikle yüzlerce milisaniye ile birkaç saniye arasında olmak üzere büyük ölçüde farklılık gösterebilir, ancak veri işleme karmaşıklığına ve makine öğrenimi modeline bağlı olarak birçoğu birkaç dakika gerektirebilir. Üretim API’si çağrı sürelerini tahmin etmenin en iyi yolu Machine Learning hizmetindeki bir modelin kıyaslanmasıdır.

**Üretim API’si çağrılarım hangi tür işlem kaynakları üzerinde çalıştırılacak?**

Machine Learning hizmeti çok kiracılı bir hizmettir ve arka uçta kullanılan gerçek işlem kaynakları farklılık gösterirken performans ve öngörülebilirlik için iyileştirilir.

### Yeni Web Hizmetleri yönetimi

**Planımı silersem ne olur?**

Plan aboneliğinizden kaldırılır ve eşit olarak bölünmüş kullanım için faturalandırılır.

Not: Bir web hizmeti tarafından kullanılmakta olan planı silemezsiniz. Planı silmek için web hizmetine yeni bir plan atamanız ya da web hizmetini silmeniz gerekir.

**Plan örneği nedir?**

Plan örneği, faturalandırma planınıza ekleyebileceğiniz dahil edilen miktarlar birimidir. Faturalandırma planınız için bir faturalandırma katmanı seçtiğinizde katman bir örnek ile birlikte gelir. Daha fazla dahil edilen miktar gerekli olursa planınıza seçili faturalandırma katmanının örneklerini ekleyebilirsiniz.

**Kaç plan örneği ekleyebilirim?**

Bir abonelikte geliştirme/test katmanının bir örneğine sahip olabilirsiniz.

S1, S2 ve S3 katmanları için gerektiği kadar örnek ekleyebilirsiniz.

Not: Öngörülen kullanımınıza bağlı olarak, daha yüksek bir dahil edilen miktarlar katmanına yükseltme yapılması, örneklerin geçerli katmana eklenmesinden daha uygun maliyetli olabilir.

**Plan katmanlarını değiştirirsem (yükseltme/indirgeme) ne olur?**

Eski plan silinir ve geçerli kullanım eşit olarak bölünmüş temelde faturalandırılır. Sürenin geri kalanı için yükseltilen/indirgenen katmanın dahil edilen tüm miktarlarını içeren yeni bir plan oluşturulur.

Not: Dahil edilen miktarlar her dönem ayrılır ve kullanılmamış dahil edilen miktarları sonraki döneme aktarılmaz.

**Bir plandaki örnekleri artırdığımda ne olur?**

Miktarlar eşit olarak bölünmüş temelde dahil edilir ve etkili olması 24 saat sürebilir.

**Bir planın örneğini sildiğimde ne olur?**

Örnek aboneliğinizden kaldırılır ve eşit olarak bölünmüş kullanım için faturalandırılır.


### Yeni Web Hizmetleri planlarına kaydolma

**Bir plana nasıl kaydolabilirim?**

Faturalandırma planları oluşturmanın iki yolu vardır.

Yeni bir web hizmetini ilk kez dağıttığınızda var olan bir planı seçebilir ya da yeni plan oluşturabilirsiniz.

Bu şekilde oluşturulan planlar varsayılan bölgenizdedir ve web hizmetiniz bu bölgeye dağıtılır.

Hizmetinizi varsayılan bölgeden başka bölgelere dağıtmak istiyorsanız hizmetinizi dağıtmadan önce faturalandırma planlarınızı tanımlayabilirsiniz,

Bu durumda Azure Machine Learning Web Hizmetleri portalında oturum açabilir ve planlar sayfasına gidebilirsiniz. Buradan, plan ekleyip kaldırabilir ve var olan planları değiştirebilirsiniz.

**Başlangıçta hangi planı seçmem gerekir?**

Standart S1 katmanı ile başlangıç yapmanız ve hizmetinizin kullanımını izlemeniz önerilir. Dahil edilen miktarlarınızı hızlıca kullandığınızı fark ettiğinizde örnek ekleyebilir veya daha yüksek bir katmana geçip daha indirimli fiyatlar alabilirsiniz. Faturalandırma planlarınızı faturalandırma döngünüz boyunca gereken şekilde ayarlayabilirsiniz.

**Yeni planlar hangi bölgelerde kullanılabilir?**

Yeni faturalandırma planları yeni web hizmetlerini desteklediğimiz üç üretim bölgesinde kullanılabilir:

* Orta Güney ABD
* Batı Avrupa
* Güneydoğu Asya

**Birden çok bölgede web hizmetim var. Her bölge için bir plan gerekiyor mu?**

Evet. Plan fiyatlandırması bölgeye göre değişir. Bir web hizmetini başka bir bölgeye dağıttığınızda web hizmetine ilgili bölgeye özgü planı atamanız gerekir.

### Yeni Web Hizmetleri - Fazla Kullanım

**Web hizmeti kullanımımın kapasiteyi aşıp aşmadığını nasıl denetlerim?**

Tüm planlarınızdaki kullanımı Azure Machine Learning Web Hizmetleri portalındaki Planlar sayfasından inceleyebilirsiniz. Portalda oturum açın ve Planlar menü seçeneğine tıklayın.

Tablonun İşlemler ve İşlem sütunlarında planın dahil edilen miktarlarını ve kullanılan yüzdeyi görebilirsiniz.

**Geliştirme/test katmanında dahil edilen miktarları kullanırsam ne olur?**

Geliştirme/test katmanı atanmış hizmetler sonraki döneme kadar veya ücretli katmanlardan birine taşımanıza kadar durdurulur.

**Klasik Web Hizmetleri ve Yeni Web Hizmetleri Fazla Kullanımlarda fiyatlar İstek Yanıt (RRS) ve Toplu (BES) iş yükleri için nasıl hesaplanır?**

Bir RRS iş yükünde yaptığınız her API işlemi çağrısının yanı sıra bu isteklerle ilişkili işlem süreleri ücretlendirilir. RRS Üretim API’si İşlem maliyetleri yaptığınız toplam API çağrısı sayısının 1.000 işlem başına fiyatla (tek işlem için eşit oranda bölünmüş şekilde) çarpılmasıyla hesaplanır. RRS API Üretim API’si İşlem Saati maliyetleriniz her bir API çağrısının çalışması için gereken süre miktarının, toplam API işlemi sayısına çarpılması ve çıkan sonucun Üretim API’si İşlem Saati başına fiyatıyla çarpılmasıyla hesaplanır.

Örneğin Standart S1 fazla kullanımı için her birinin çalışması 0,72 saniye süren 1.000.000 API İşlemi, Üretim API’si İşlem maliyetlerinde (1.000.000 * 0,50 $/1000 API işlemi) 500 $ ve Üretim API’si İşlem Saatlerinde (1.000.000 * 0,72 saniye * 2 $/saat) 400 $ olmak üzere toplam 900 $ ile sonuçlanır.

Bir BES iş yükü de aynı şekilde ücretlendirilir; ancak, API işlem maliyetleri gönderdiğiniz toplu iş sayısını, işlem maliyetleri ise bu toplu işlerle ilişkili işlem süresini temsil eder. BES Üretim API’si İşlem maliyetleri gönderdiğiniz toplam iş sayısının 1.000 işlem başına fiyatla (tek işlem için eşit oranda bölünmüş şekilde) çarpılmasıyla hesaplanır. BES API Üretim API’si İşlem Saati maliyetleriniz, işinizdeki her satır için gereken süre miktarının, işinizdeki toplam satır sayısı çarpı toplam iş sayısı çarpı Üretim API’si İşlem Saati başına fiyat ile çarpımı olarak hesaplanır. Machine Learning hesaplayıcısı kullanılırken işlem ölçer, göndermeyi planladığınız iş sayısını; işlem alanı başına süreyse çalıştırılacak her bir işteki tüm satırlar için gereken birleşik süreyi temsil eder.

Örneğin, Standart S1 fazla kullanımı ile bir günde her biri 0,72 saniye süren 500 satır içeren 100 iş gönderirseniz aylık fazla kullanım maliyetleriniz Üretim API’si İşlem maliyetlerinde (günde 100 iş = 3.100 iş/ay * 0,50 $/1000 API işlemi) 1,55 $ ve Üretim API’si İşlem Saatlerinde (500 satır * 0,72 saniye * 3.100 İş * 2 $/sa) 620 $ olmak üzere toplam 621,55 $ olur.

### Azure ML Klasik Web Hizmetleri

**Kullandıkça Öde seçeneği hala mevcut mu?**
Evet, Klasik Web Hizmetleri, Azure Machine Learning ile hala kullanılabilmektedir.  

### Azure Machine Learning Ücretsiz ve Standart Katmanı

**Azure Machine Learning Ücretsiz katmanına neler dahildir?**

Azure Machine Learning Ücretsiz katmanı, Azure Machine Learning Studio’ya kapsamlı bir giriş sağlamaya yöneliktir. Kaydolmak için tek ihtiyacınız olan bir Microsoft hesabıdır. Ücretsiz katmanı her [Microsoft hesabı](https://www.microsoft.com/account/default.aspx) için bir Azure Machine Learning Studio çalışma alanına ücretsiz erişim içerir. Buna 10 GB depolama alanı kullanımı ve modelleri, hazırlama API’leri olarak kullanıma hazır hale getirebilme imkanı dahildir. Ücretsiz katmanı iş yükleri bir SLA kapsamında değildir ve yalnızca geliştirme ve kişisel kullanım amaçlıdır. Ücretsiz katmanı iş yükleri bir şirket içi SQL sunucusuna bağlanarak verilere erişemez.

**Azure Machine Learning Standart katmanına ve planlarına neler dahildir?**

Azure Machine Learning Standart katmanı, Azure Machine Learning Studio’nun ücretli bir üretim sürümüdür. Azure ML Service Studio’nun aylık ücreti çalışma alanı başına faturalandırılır ve kısmi aylar için eşit oranda bölünür. Azure ML Studio deneme saatleri, etkin deneme için işlem saati başına faturalandırılır. Fatura, kısmi saatler için eşit olarak bölünür.  

Azure ML API hizmeti, klasik web hizmetleri veya yeni bir web hizmeti olmasına göre faturalandırılır.

Aşağıdaki ücretler aboneliğiniz için çalışma alanı başına toplanır.

* Machine Learning Çalışma Alanı Aboneliği - Machine Learning Çalışma Alanı Aboneliği bir ML Studio çalışma alanına erişim sağlayan ve hem Studio’da denemeleri çalıştırmak için gerekli olan hem de üretim API’lerini kullanan bir aylık ücrettir.
* Studio Deneme Saatleri - bu ölçüm ML Studio’da deneme çalıştırmaktan ve hazırlık ortamında üretim API’si çağrılarını çalıştırmaktan doğan tüm işlem ücretlerini toplar.
* Eğitim ve puanlama amacıyla modellerinizdeki bir şirket içi SQL sunucusuna bağlanarak verilere erişin.
* Klasik Web Hizmetleri için:
    * Üretim API’si İşlem Saatleri - Bu ölçüm üretimde çalışan Web hizmetlerinden doğan işlem ücretlerini içerir.
    * Üretim API’si İşlemleri (1000’Lik bloklar saniye) - Bu ölçüm üretim web hizmetine yapılan her çağrı için doğan ücretleri içerir.

Yukarıdaki ücretler dışında Yeni Web Hizmetleri söz konusu olduğunda ücretler seçili plana toplanır:

* Standart S1/S2/S3 API Planı (Birim) - Bu ölçümler yeni Web Hizmetleri için seçilen örneğin türünü temsil eder
* Standart S2/S1/S3 Fazla Kullanım API İşlem Saatleri - bu ölçüm var olan örneklerdeki dahil edilen miktarlar tüketildikten sonra üretimde çalışan yeni Web Hizmetleri’nden doğan işlem ücretlerini içerir. Ek kullanım, S1/S2/S3 plan katmanıyla ilişkili fazla kullanım fiyatından ücretlendirilir.
* Standart S1/S2/S3 Fazla Kullanım API İşlemleri (1.000 saniye) - Bu ölçüm var olan örneklerdeki dahil edilen miktarlar tüketildikten sonra üretim Yeni Web Hizmeti’nize yapılan çağrı başına doğan ücretleri içerir. Ek kullanım, S1/S2/S3 plan katmanıyla ilişkili fazla kullanım fiyatından ücretlendirilir.
* Dahil Edilen Miktar API İşlem Saatleri - Yeni Web Hizmetleri ile bu ölçüm API İşlem Saatlerinin dahil edilen miktarını temsil eder
* Dahil Edilen Miktar API İşlemleri (1000'lik bloklar)- Yeni Web Hizmetleri ile bu ölçüm API İşlemlerinin dahil edilen miktarını temsil eder


**Azure ML Ücretsiz katmanına nasıl kaydolabilirim?**

Tek ihtiyacınız olan bir Microsoft hesabıdır. [Azure Machine Learning giriş sayfasına](https://azure.microsoft.com/services/machine-learning/) gidin ve **Şimdi Başlat** düğmesine tıklayın. Microsoft hesabınızla oturum açtığınızda Ücretsiz katmanında sizin için bir çalışma alanı oluşturulur. Machine Learning denemelerini keşfetmeye ve oluşturmaya hemen başlayabilirsiniz.

**Azure ML Standart katmanına nasıl kaydolabilirim?**

Standart ML çalışma alanı oluşturmak için öncelikle bir Azure aboneliğine erişiminiz olmalıdır. 30 günlük ücretsiz Azure aboneliği deneme sürümüne kaydolabilir ve sonra ücretli bir Azure aboneliğine yükseltme yapabilir ya da hemen ücretli bir Azure aboneliği satın alabilirsiniz. Bundan sonra Microsoft klasik Azure portalından bir Machine Learning çalışma alanı oluşturabilirsiniz. [Adım adım yönergelere](https://azure.microsoft.com/trial/get-started-machine-learning-b/) bakın.

Alternatif olarak, Standart ML çalışma alanı sahibi tarafından sahibin çalışma alanına erişmeye davet edilebilirsiniz.

**Ücretsiz katmanı ile kullanmak üzere kendi Azure Blob Depolama hesabımı belirtebilir miyim?**

Hayır, Standart katman Machine Learning hizmetinin katmanlar sunulmadan önce mevcut olan sürümüne eşdeğerdir.

**Makine öğrenimi modellerimi Ücretsiz katmanında API olarak dağıtabilir miyim?**

Evet, makine öğrenimi modellerini ücretsiz katmanının bir parçası olarak hazırlık API hizmetlerinde kullanıma hazır hale getirebilirsiniz. Hazırlık API hizmetini üretime sokmak ve kullanıma hazır hale getirilmiş hizmet için bir üretim uç noktası edinmek üzere Standart katmanını kullanmanız gerekir.

**Azure Ücretsiz deneme sürümü ile ve Azure Machine Learning Ücretsiz katmanı arasındaki fark nedir?**

[Microsoft Azure ücretsiz deneme](https://azure.microsoft.com/free/) sürümü bir ay boyunca herhangi bir Azure hizmetine uygulanabilen krediler sunar. Azure Machine Learning Ücretsiz katmanı, üretim dışı iş yükleri için Azure Machine Learning hizmetine özel sürekli erişim sunar.

**Bir denemeyi Ücretsiz katmanından Standart katmanına nasıl taşıyabilirim?**

Denemelerinizi Ücretsiz katmanından Standart katmanına kopyalamak için:

1.  Azure Machine Learning Studio’da oturum açın ve üst gezinti çubuğundaki çalışma alanı seçicisinde hem Ücretsiz çalışma alanını hem de Standart çalışma alanını görebildiğinizden emin olun.
2.  Standart çalışma alanındaysanız Ücretsiz çalışma alanına geçin.
3.  Deneme listesi görünümünde kopyalamak istediğiniz bir denemeyi seçin ve Kopyala komut düğmesine tıklayın.
4.  Açılan iletişim kutusundan Standart çalışma alanını seçin ve Kopyala düğmesine tıklayın.
    Tüm ilişkili veri kümeleri, eğitilen model vb. denemeyle birlikte Standart çalışma alanına kopyalanır.
6.  Denemeyi yeniden çalıştırmanız ve web hizmetinizi Standart çalışma alanınıza yeniden yayımlamanız gerekir.

### Studio Çalışma Alanı

**Farklı çalışma alanları için farklı faturalar görecek miyim?**

Çalışma alanı ücretleri tek bir fatura üzerinde, geçerli olan her bir ölçüm için ayrıca listelenir.

**Denemelerim hangi tür işlem kaynakları üzerinde çalıştırılacak?**

Machine Learning hizmeti çok kiracılı bir hizmettir ve arka uçta kullanılan gerçek işlem kaynakları farklılık gösterirken performans ve öngörülebilirlik için iyileştirilir.

### Konuk erişimi

**Azure Machine Learning Studio’ya Konuk Erişimi nedir?**

Konuk Erişimi, Azure Machine Learning Studio’da herhangi bir maliyet ve kimlik doğrulaması olmadan denemeler oluşturup çalıştırmanıza imkan tanıyan kısıtlı bir deneme sürümüdür. Konuk oturumları kalıcı değildir (kaydedilemez) ve 8 saatle sınırlıdır. R ve Python desteğinin olmaması, hazırlık API’lerinin olmaması, kısıtlı veri kümesi boyutu ve kısıtlı depolama kapasitesi ise diğer sınırlamalardır. Bir Microsoft hesabıyla oturum açmayı seçen kullanıcılar Machine Learning Studio’nun yukarıda açıklanan ve kalıcı bir çalışma alanı ile daha kapsamlı özellikler içeren Ücretsiz katmanına tam erişime sahip olur. [https://studio.azureml.net](https://studio.azureml.net) sayfasındaki **Kullanmaya başlayın**’a tıklayarak ve Konuk Erişimi ya da Microsoft hesabı ile oturum aç seçeneğini belirleyerek ücretsiz Machine Learning deneyiminizi seçin.

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



<!--HONumber=Oct16_HO3-->


