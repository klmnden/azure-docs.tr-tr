<properties
    pageTitle="Azure Machine Learning ile ilgili SSS | Microsoft Azure"
    description="Azure Machine Learning'e giriş: Kolaylaştırılmış tahmine dayalı modelleme için bir bulut hizmetinin faturalamasını, özelliklerini ve sınırlamalarını kapsayan SSS."
    keywords="machine learning introduction,predictive modeling,what is machine learning"
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
    ms.date="04/18/2016"
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

Daha fazla bilgi için bkz. [Machine Learning web hizmetine bağlanma](machine-learning-connect-to-azure-machine-learning-web-service.md).


## Faturalama soruları

**Machine Learning faturalaması nasıl çalışır?**

Faturalama ve fiyatlandırma bilgileri için bkz. [Machine Learning Fiyatlandırması](https://azure.microsoft.com/pricing/details/machine-learning/).

**Machine Learning'in ücretsiz deneme sürümü var mı?**

 Bir ücretsiz Azure deneme sürümüne kaydolduğunuzda, herhangi bir Azure hizmetini bir ay süreyle deneyebilirsiniz. Azure ücretsiz deneme sürümü hakkında daha fazla bilgi için [Azure Ücretsiz Deneme Sürümü ile ilgili SSS](/pricing/free-trial-faq/).

## Machine Learning Studio soruları

### Deneme oluşturma

**Deneme grafikleri için sürüm denetimi veya Git tümleştirmesi var mı?**

Hayır ancak Machine Learning Studio, bir denemenin diğer kullanıcılar tarafından değiştirilemeyen her bir yinelemesini korur.
Daha fazla bilgi için bkz. [Machine Learning Studio'da deneme yinelemelerini yönetme](machine-learning-manage-experiment-iterations.md).

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

Az miktarda veriniz varsa ve bunu bir http URL'si aracılığıyla kullanıma sunmak istiyorsanız [Verileri İçeri Aktarma][import-data] modülünü kullanabilirsiniz. Daha büyük bir veri miktarını öncelikle Azure Storage'a aktarın ve ardından bunu denemenize getirmek için [Verileri İçeri Aktarma][import-data] modülünü kullanın.
<!--
<SEE CLOUD DS PROCESS>
-->

**Yerleşik bir görüntü girişi özelliği var mı?**

Görüntü girişi özelliği hakkında [Görüntüleri İçe Aktarma][image-reader] başvurusundan bilgi edinebilirsiniz.

### Modüller

**Aradığım algoritma, veri kaynağı, veri biçimi veya veri dönüştürme işlemi Azure Machine Learning Studio'da yok. Seçeneklerim neler?**

Takip ettiğimiz özellik isteklerini görmek için [kullanıcı geri bildirim forumunu](http://go.microsoft.com/fwlink/?LinkId=404231) ziyaret edebilirsiniz. Aradığınız bir özellik zaten istenmişse isteğe oyunuzu ekleyin. Aradığınız özellik mevcut değilse yeni bir istek oluşturun. Bu forumda isteğinizin durumunu da görüntüleyebilirsiniz. Bu listeyi yakından takip edip özellik kullanılabilirliği durumunu sık sık güncelleştiriyoruz. Buna ek olarak, R ve Python için yerleşik destekle gerektiğinde özel dönüştürmeler oluşturulabilir.


**Var olan kodumu Machine Learning Studio'ya getirebilir miyim?**

Evet, var olan R veya Python kodunuzu Machine Learning Studio'ya getirebilir, Azure Machine Learning öğrencileriyle birlikte aynı denemede bunu çalıştırabilir ve Azure Machine Learning aracılığıyla çözümü bir web hizmeti olarak dağıtabilirsiniz. Daha fazla bilgi için bkz. [R ile denemenizi genişletme](machine-learning-extend-your-experiment-with-r.md) ve [Azure Machine Learning Studio'da Python makine öğrenimi betiklerini yürütme](machine-learning-execute-python-scripts.md).

**Bir modeli tanımlamak için [PMML](http://en.wikipedia.org/wiki/Predictive_Model_Markup_Language) gibi bir şey kullanmak mümkün mü?**

Hayır, bu desteklenmiyor ancak özel R ve Python kodu bir modülün tanımlanması için kullanılabilir.

**Denememe paralel şekilde kaç tane modül yürütebilirim?**  

Bir denemede paralel şekilde en fazla 4 modül yürütebilirsiniz.


### Veri işleme

**Denemenin içinde verileri etkileşimli bir şekilde görselleştirme (R görselleştirmesinin ötesinde) yeteneği var mı?**

Bir modülün çıkışına tıklayarak verileri görselleştirebilir ve istatistikleri elde edebilirsiniz.

**Tarayıcıda sonuçları veya verileri önizlerken satır ve sütunların sayısı sınırlı; bunun nedeni nedir?**

Tarayıcıya veri aktarıldığından ve bu veri büyük olabileceğinden, Machine Learning Studio'nun yavaşlamasının önlenmesi için veri boyutu sınırlanır. Tüm verinin/sonucun görselleştirilmesi için, verileri indirip Excel veya başka bir araç kullanılması daha iyi olur.

### Algoritmalar

**Machine Learning Studio'da hangi var olan algoritmalar desteklenmektedir?**

Machine Learning Studio, Microsoft Research'te geliştirilen Ölçeklenebilir Artırılmış Karar ağaçları, Bayes Önerisi sistemleri, Derin Sinir Ağları ve Karar Ormanları gibi teknoloji harikası algoritmaları sağlar. Vowpal Wabbit gibi ölçeklenebilir açık kaynaklı makine öğrenimi paketleri de dahildir. Machine Learning Studio, çok sınıflı ve ikili sınıflandırma, regresyon ve kümeleme için makine öğrenimi algoritmalarını destekler. [Machine Learning Modüllerinin][machine-learning-modules] tam listesine bakın.

**Verilerim için kullanılacak doğru Machine Learning algoritmasını otomatik olarak öneriyor musunuz?**

Hayır ancak sorununuz için doğru olana karar vermek amacıyla her bir algoritmanın sonucunu karşılaştırmaya yönelik birkaç yol Machine Learning Studio'da mevcuttur.

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

Yeniden Eğitme API'lerini kullanın. Daha fazla bilgi için bkz. [Machine Learning modellerini programlama aracılığıyla yeniden eğitme](machine-learning-retrain-models-programmatically.md). Örnek kod [Microsoft Azure Machine Learning Yeniden Eğitme Demosu](https://azuremlretrain.codeplex.com/)'nda da kullanılabilir.

### Oluşturma

**Modeli yerel olarak veya bir internet bağlantısı olmayan bir uygulamada dağıtabilir miyim?**

Hayır.


**Tüm web hizmetleri için beklenen temel gecikme süresi var mı?**

Bkz. [Azure abonelik limitleri](../azure-subscription-service-limits.md)

### Kullanım

**Tahmine dayalı modelimi Batch hizmeti ile İstek Yanıtı hizmeti karşılaştırması olarak çalıştırmayı ne zaman isterim?**

İstek Yanıtı hizmeti (RRS), deneme ortamından oluşturulan ve dağıtılan durum bilgisi olmayan modellere bir arabirim sağlamak için kullanılan düşük gecikme süreli ve yüksek ölçekli bir web hizmetidir. Batch hizmeti (BES), toplu veri kayıtlarının uyumsuz şekilde puanlamasına yönelik bir hizmettir. BES girişi, RRS'te kullanılan veri girişine benzerdir. Ana farklılık, BES'in Azure'daki Blob hizmeti ve Tablo hizmeti, Azure SQL Database, HDInsight (hive sorgusu) ve HTTP kaynakları gibi çeşitli kaynaklardan bir kayıt bloğunu okumasıdır. Daha fazla bilgi için bkz. [Machine Learning web hizmetlerini kullanma](machine-learning-consume-web-services.md).

**Dağıtılan web hizmeti için modeli nasıl güncelleştiririm?**

Zaten dağıtılmış olan bir hizmet için tahmine dayalı bir modeli güncelleştirme, eğitilen modeli yazmak ve kaydetmek için kullandığınız denemeyi değiştirmek ve yeniden çalıştırmak kadar kolaydır. Yeni bir kullanılabilen eğitim modeli sürümüne sahip olduğunuzda, Machine Learning Studio size web hizmetini güncelleştirmek isteyip istemediğinizi sorar. Dağıtılmış bir web hizmetini güncelleştirmek hakkında ayrıntılar için bkz. [Bir Machine Learning web hizmetini dağıtma](machine-learning-publish-a-machine-learning-web-service.md) 

Yeniden Eğitme API'lerini de kullanabilirsiniz.
Daha fazla bilgi için bkz. [Machine Learning modellerini programlama aracılığıyla yeniden eğitme](machine-learning-retrain-models-programmatically.md). Örnek kod [Microsoft Azure Machine Learning Yeniden Eğitme Demosu](https://azuremlretrain.codeplex.com/)'nda da kullanılabilir.

**Üretimde dağıtılan web hizmetimi nasıl izleyebilirim?**

Tahmine dayalı bir model dağıtıldıktan sonra, modeli Klasik Azure portalından izleyebilirsiniz. Dağıtılan her bir hizmet kendine panosuna sahiptir ve burada bu hizmetin izleme bilgilerini görebilirsiniz.

**RRS/BES'imin çıktısını görebileceğim bir yer var mı?**

RRS için sonucu genellikle web hizmeti yanıtında görürsünüz. Bunu Azure blob depolama alanına da yazabilirsiniz. BES çıktısı varsayılan olarak bir bloba yazılır. Ayrıca, [Verileri Dışarı Aktarma][export-data] modülünü kullanarak çıktıyı bir veritabanına veya tabloya yazabilirsiniz.

**Web hizmetlerini yalnızca Machine Learning Studio'da oluşturulan modellerden mi oluşturabilirim?**

Hayır, web hizmetleri doğrudan Jupyter Not Defterlerinden ve RStudio'dan da oluşturabilirsiniz.

**Hata kodları hakkında bilgileri nerede bulabilirim?**

Hata kodları ve açıklamaları listesi için bkz: [Machine Learning Hata Kodları](https://msdn.microsoft.com/library/azure/dn905910.aspx).

## Ölçeklenebilirlik

**Web hizmetinin ölçeklenebilirliği nedir?**

Şu anda varsayılan uç nokta, uç nokta başına 20 eş zamanlı RRS isteği ile sağlanır. Bunu uç nokta başına 200 eş zamanlı isteğe ölçeklendirebilirsiniz ve [API uç noktalarını ölçeklendirme](machine-learning-scaling-endpoints.md)'de açıklandığı üzere her bir web hizmetini web hizmeti başına 10.000 uç noktaya ölçeklendirebilirsiniz. BES için her bir uç nokta, her seferinde 40 isteği işlemeye olanak sağlar ve 40'ı aşan ek istekler kuyruğa alınır. Bu kuyruğa alınan istekler, sıra azaldıkça otomatik olarak çalıştırılır.


**R işleri düğümlere yayılır mı?**

Hayır.  


**Eğitim içim ne kadar veri kullanabilirim?**

Machine Learning Studio'daki modüller, ortak kullanım durumları için en fazla 10 GB boyutunda yoğun sayısal verili veri kümelerini destekler. Bir modülün birden fazla girdi aldığı durumlarda tüm girdilerin toplam boyutu 10 GB'tır. Hive veya Azure SQL Database sorguları ya da alımdan önce [Sayımlarla Öğrenme][counts] modülleriyle ön işleme aracılığıyla daha büyük veri kümelerinden de örnek oluşturabilirsiniz.  

Aşağıdaki veri türleri, özellik normalleştirme sırasında daha büyük veri kümelerine genişleyebilir ve boyutu 10 GB'tan az olacak şekilde sınırlıdır:

- seyrek
- kategorik
- dizeler
- ikili veriler

Aşağıdaki modüller, boyutu 10 GB'tan az veri kümeleriyle sınırlıdır:

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

Depolama hesabını yanlışlıkla sildiyseniz bu durumdan kurtulmanın tek yolu silinen depolama hesabıyla aynı bölgede aynı isimli bir depolama hesabını yeniden oluşturmaktır. Bunun ardından Erişim Tuşu'nu yeniden eşitleyin.


**Depolama hesabımın erişim anahtarı eşitlenmemişse ne olur?**

Machine Learning Studio, iş akışı yürütülürken ara verileri kaydetmek için kullanıcı tarafından sağlanan bir Azure depolama hesabı kullanır. Bu depolama hesabı, bir çalışma alanı oluşturulduğunda Machine Learning Studio'ya sağlanır ve Erişim Anahtarları da bu çalışma alanıyla ilişkilendirilir. Çalışma alanı oluşturulduktan sonra Erişim Anahtarları değiştirilirse bu çalışma alanı artık depolama hesabına erişemez, işlemeyi durdurur ve bu çalışma alanındaki tüm denemeler başarısız olur.

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



<!----HONumber=Jun16_HO2-->


