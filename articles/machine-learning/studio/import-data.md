---
title: Çeşitli veri kaynaklarından alınan verileri içeri aktar
titleSuffix: Azure Machine Learning Studio
description: Verilerinizi çeşitli veri kaynaklarından Azure Machine Learning Studio'ya içeri aktarma. Hangi veri türlerini ve veri biçimleri desteklendiğini öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 02/01/2019
ms.openlocfilehash: 41cc1d6638871f26ae942e724a402e17f52150fc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60811035"
---
# <a name="import-your-training-data-into-azure-machine-learning-studio-from-various-data-sources"></a>Eğitim verilerinizi çeşitli veri kaynaklarından Azure Machine Learning Studio’ya alma

Geliştirmek ve Tahmine dayalı analiz çözümü eğitmek için Machine Learning Studio'da kendi verilerinizi kullanmak için veri kullanabilirsiniz: 

* **Yerel dosya** -çalışma alanınızda bir veri kümesi modülü oluşturmak için sabit sürücünüzden önceden yerel veri yükleme
* **Çevrimiçi veri kaynakları** -kullanım [verileri içeri aktarma] [ import-data] denemenizi çalışırken çeşitli çevrimiçi kaynaklardan birinden modülü verilere erişme
* **Machine Learning Studio denemesine** -Machine Learning Studio'da bir veri kümesi olarak kaydedilmiş olan veri kullanın
* [**Şirket içi SQL Server veritabanı** ](use-data-from-an-on-premises-sql-server.md) -verileri el ile kopyalamak zorunda kalmadan bir şirket içi SQL Server veritabanından veri kullanan

> [!NOTE]
> Kullanılabilir Machine Learning Studio'da eğitim verilerini için kullanabileceğiniz birçok örnek veri kümesi yok. Bunlar hakkında daha fazla bilgi için bkz: [Azure Machine Learning Studio'da örnek veri kümelerini kullanan](use-sample-datasets.md).

## <a name="prepare-data"></a>Verileri hazırlama

Machine Learning Studio, ayrılmış veya bazı durumlarda dikdörtgen olmayan veri kullanılabilmesine rağmen bir veritabanından veri yapılandırılmış olan metin veriler gibi dikdörtgen ya da tablolu verileri ile çalışacak şekilde tasarlanmıştır.

Studio'ya almadan önce verilerinizi görece temiz olması durumunda en iyisidir. Örneğin, tırnak işareti olmayan dizeler gibi sorunların dikkatli olmanız gerekir.

Ancak, modüllerin verilerinizi içeri aktardıktan sonra bazı düzenleme deneyiminizi içinde veri sağlayan Studio'da kullanılabilen vardır. Yapılandırmanıza bağlı olarak makine öğrenimi algoritmaları kullanacaksınız, eksik değerleri ve seyrek veri gibi veri yapısal sorunlar ele alacağız nasıl karar gerekebilir ve yardımcı olan ile modüller vardır. Konum **veri dönüştürme** bu işlevleri gerçekleştirmek modüller için modül paletinin bölümü.

Denemenizi herhangi bir noktada görüntüleyebilir veya çıkış bağlantı noktasına tıklayarak modülü tarafından üretilen veri indirin. Modül bağlı olarak farklı indirme seçenekleri kullanılabilir olabilir veya web tarayıcınızda Studio içinde verileri görselleştirmek mümkün olabilir.

## <a name="supported-data-formats-and-data-types"></a>Desteklenen veri biçimlerini ve veri türleri

Denemenize birkaç veri türleri içeri aktarabilirsiniz, ne mekanizması bağlı olarak, veri ve burada işleminin yapıldığı içeri aktarın:

* Düz metin (.txt)
* Virgülle ayrılmış değerler (CSV üst bilgisi (.csv) ile veya olmadan) (. nh.csv)
* Sekmeyle ayrılmış değerler (TSV üst bilgisi (.tsv) ile veya olmadan) (. nh.tsv)
* Excel dosyası
* Azure tablosu
* Hive tablosu
* SQL veritabanı tablosu
* OData değerleri
* SVMLight veri (.svmlight) (bkz [SVMLight tanımı](http://svmlight.joachims.org/) biçim bilgilerini için)
* İlişki dosyası biçimi'ne (ARFF) veri (.arff) özniteliği (bkz [ARFF'ye tanımı](https://weka.wikispaces.com/ARFF) biçim bilgilerini için)
* Zip dosyası (.zip)
* R nesne veya çalışma alanı dosyası (. RData)

Studio bu meta veriler gibi meta veriler içeren ARFF'ye biçiminde içeri aktarırsanız başlık ve her bir sütunun veri türünü tanımlamak için kullanır.

Bu meta veriler içermeyen TSV veya CSV biçiminde gibi verileri içe aktarırsanız, verileri yeniden örnekleyerek Studio her bir sütunun veri türünü çıkarır. Studio, verileri de sütun başlıklarını yoksa, varsayılan adları sağlar.

Açıkça belirtebilir veya sütunların kullanarak başlıklar ve veri türlerini değiştirme [meta verileri Düzenle] [ edit-metadata] modülü.

Aşağıdaki veri türlerini Studio tarafından tanınmaktadır:

* String
* Tamsayı
* Double
* Boolean
* DateTime
* TimeSpan

Studio adlı bir iç veri türü kullanan ***veri tablosu*** modülleri arasında veri iletmek için. Veri biçimi kullanarak tablosuna verilerinizi açıkça dönüştürebilir [veri kümesine Dönüştür] [ convert-to-dataset] modülü.

Veri tablosu dışında biçimlerini kabul eden herhangi bir modülü verilerini tablo sonraki modülüne iletmeden önce sessizce dönüştürür.

Gerekirse, verileri tablo biçiminde geri CSV, TSV, ARFF'ye veya diğer dönüştürme modüllerini kullanarak SVMLight biçimine dönüştürebilirsiniz.
Konum **veri biçim dönüştürmelerini** bu işlevleri gerçekleştirmek modüller için modül paletinin bölümü.

## <a name="data-capacities"></a>Veri kapasitesi

Machine Learning Studio'daki modüller, ortak kullanım durumları için en fazla 10 GB boyutunda yoğun sayısal verili veri kümelerini destekler. Bir modülün birden fazla giriş aldığı durumlarda 10 GB değeri tüm giriş boyutlarının toplamıdır. Hive veya Azure SQL veritabanı sorguları kullanarak daha büyük veri kümelerinden örnek veya veri bazında sayılar, içeri aktarmadan önce ön işleme öğrenme kullanabilirsiniz.  

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

Birkaç GB'den büyük olan veri kümeleri için Azure depolama veya Azure SQL veritabanına veri yükleme veya doğrudan yerel dosyadan yüklemek yerine Azure HDInsight'ı kullanın.

Görüntü verileri hakkında bilgi bulabilirsiniz [görüntüleri içeri aktarma](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/import-images#bkmk_Notes) modül başvurusu.

## <a name="import-from-a-local-file"></a>Yerel bir dosyadan içeri aktar

Studio'da eğitim verilerini olarak kullanılacak sabit sürücünüzden bir veri dosyasını karşıya yükleyebilirsiniz. Bir veri dosyası içeri aktardığınızda, bir veri kümesi modülü kullanıma hazır denemeleri çalışma alanınızda oluşturun.

Bir yerel sabit sürücünüzden verileri içeri aktarmak için aşağıdakileri yapın:

1. Tıklayın **+ yeni** Studio penceresinin alt kısmındaki.
2. Seçin **veri KÜMESİ** ve **yerel DOSYADAN**.
3. İçinde **yeni bir veri kümesi karşıya** iletişim kutusunda, karşıya yüklemek istediğiniz dosyaya göz atın.
4. Bir ad girin, veri türünü tanımlar ve isteğe bağlı olarak bir açıklama girin. Bir açıklama önerilen - veri gelecekte kullanırken unutmayın istediğiniz veriler hakkında herhangi bir özelliği kaydetmenize olanak sağlar.
5. Onay kutusunu **bu var olan bir dataset yeni sürümüdür** , var olan bir dataset yeni veriler ile güncelleştirmenizi sağlar. Bunu yapmak için bu onay kutusuna tıklayın ve ardından var olan bir veri kümesinin adını girin.

![Yeni bir veri kümesi karşıya yükleme](./media/import-data/upload-dataset-from-local-file.png)

Karşıya yükleme, veri boyutu ve hizmete bağlantınızın hızına bağlı zaman. Dosya uzun sürmesi biliyorsanız beklerken Studio içinde başka şeyler yapabilirsiniz. Ancak, verileri karşıya yükleme tamamlanmadan önce tarayıcının kapanması karşıya yükleme başarısız olmasına neden olur.

Verilerinizi karşıya yüklendikten sonra bir veri kümesi modülde depolanır ve çalışma alanınızdaki tüm deneme sunulur.

Bir deney düzenlerken, karşıya yüklediğiniz veri kümeleri bulabilirsiniz **My veri kümeleri** altında listesinde **kaydedilmiş veri kümeleri** modül paletindeki listesi. Daha fazla analiz ve makine öğrenimi için veri kümesini kullanmak istediğinizde veri kümesini deneme tuvaline sürükleyip bırakabilirsiniz.

## <a name="import-from-online-data-sources"></a>Çevrimiçi veri kaynaklarından içeri aktarma

Kullanarak [Veri Al] [ import-data] modülü, denemenizi içeri aktarabilir veri çalıştıran deneme sırasında çeşitli çevrimiçi veri kaynaklarından.

> [!NOTE]
> Bu makalede, hakkında genel bilgiler sağlanmaktadır. [verileri içeri aktarma] [ import-data] modülü. Erişebileceğiniz veri türleri hakkında daha ayrıntılı bilgi için biçimleri, parametreleri ve sık sorulan soruların yanıtlarını modülü başvurusu için konusuna [verileri içeri aktarma] [ import-data] modülü.

Kullanarak [verileri içeri aktarma] [ import-data] modülü erişebilirsiniz veri çeşitli çevrimiçi veri kaynaklarından biri denemenizi çalışırken:

* HTTP kullanarak bir Web URL'si
* Hadoop HiveQL kullanma
* Azure blob depolama
* Azure tablosu
* Azure SQL veritabanı ya da Azure vm'lerde SQL Server
* Şirket içi SQL Server veritabanı
* Bir veri sağlayıcısı, şu anda OData akışı
* Azure Cosmos DB

Bu eğitim verilerini denemenizi çalışırken erişildiği için yalnızca bu deneme kullanılabilir. Buna karşılık olarak çalışma alanınızdaki tüm denemenize bir veri kümesi modülde depolanan veriler kullanılabilir.

Studio denemenizi, çevrimiçi veri kaynaklarına erişmek için ekleme [verileri içeri aktarma] [ import-data] denemenizi modülü. Ardından **veri içeri aktarma sihirbazını başlatma** altında **özellikleri** seçmek ve veri kaynağını yapılandırmak kullanıcının yönlendirildiği adım adım yönergeler için. Alternatif olarak, el ile seçebilir **veri kaynağı** altında **özellikleri** ve verilere erişmek için gereken parametreleri sağlayın.

Desteklenen çevrimiçi veri kaynakları, aşağıdaki tabloda listelenen. Bu tablo, desteklenen dosya biçimleri ve verilere erişmek için kullanılan parametreleri de özetler.

> [!IMPORTANT]
> Şu anda [verileri içeri aktarma] [ import-data] ve [verileri dışarı aktarma] [ export-data] modülleri okuma ve yalnızca klasik kullanılarak oluşturulan Azure Depolama'dan veri yazma dağıtım modeli. Diğer bir deyişle, sık erişimli depolama erişim katmanı veya seyrek erişimli depolama erişim katmanı sağlayan yeni Azure Blob Depolama hesap türü henüz desteklenmiyor.
>
> Tüm Azure depolama hesaplarını genel olarak, bu hizmet seçeneği kullanılabilir olmadan önce oluşturmuş olabileceğiniz olduğunu etkilenmez.
> Yeni bir hesap oluşturmanız gerekiyorsa, seçin **Klasik** dağıtım modeli veya Kaynak Yöneticisi'ni kullanın ve seçin **genel amaçlı** yerine **Blob Depolama** için **Hesap türü**.
>
> Daha fazla bilgi için [Azure Blob Depolama: Sık erişimli ve seyrek erişimli depolama katmanları](../../storage/blobs/storage-blob-storage-tiers.md).

### <a name="supported-online-data-sources"></a>Desteklenen çevrimiçi veri kaynakları
Azure Machine Learning Studio **verileri içeri aktarma** Modülü aşağıdaki veri kaynaklarını destekler:

| Veri Kaynağı | Açıklama | Parametreler |
| --- | --- | --- |
| HTTP üzerinden Web URL'si |Verileri virgülle ayrılmış değerler (CSV), sekmeyle ayrılmış değerler (TSV), öznitelik-ilişki dosyası biçimi'ne (ARFF) ve Destek vektör makineler (ışık SVM) biçimleri, HTTP kullanan herhangi bir web URL'den okur |<b>URL</b>: Site URL'si ve tüm uzantılı dosya adı dahil olmak üzere dosyanın tam adını belirtir. <br/><br/><b>Veri biçimi</b>: Desteklenen veri biçimlerinden birini belirtir: CSV, TSV, ARFF'ye veya SVM açık. Verileri bir üst bilgi satırı varsa, sütun adları atamak için kullanılır. |
| Hadoop/HDFS |Hadoop dağıtılmış depolama alanından verileri okur. HiveQL, bir SQL benzeri sorgu dili kullanarak istediğiniz verileri belirtin. HiveQL veri toplama ve veri Studio'ya verileri eklemeden önce filtreleme yapmak için de kullanılabilir. |<b>Hive veritabanı sorgusu</b>: Verileri oluşturmak için kullanılan Hive sorgusu belirtir.<br/><br/><b>HCatalog sunucusu URI </b> : Belirtilen biçimi kullanarak kümenizin adını  *&lt;küme adınızı&gt;. azurehdinsight.net.*<br/><br/><b>Hadoop kullanıcı hesabı adı</b>: Kümesi sağlamak için kullanılan Hadoop kullanıcı hesabı adını belirtir.<br/><br/><b>Hadoop kullanıcı hesabı parolası</b> : Küme sağlama kullanılan kimlik bilgilerini belirtir. Daha fazla bilgi için [Hadoop kümeleri oluşturma HDInsight](/azure/hdinsight/hdinsight-hadoop-provision-linux-clusters).<br/><br/><b>Çıktı verilerini konumunu</b>: Verileri bir Hadoop dağıtılmış dosya sistemi (HDFS) içinde veya azure'da depolanan belirtir. <br/><ul>HDFS çıktı verilerini depolamak, HDFS sunucusuna URI belirtin. (HTTPS:// ön eki olmadan HDInsight küme adı kullandığınızdan emin olun). <br/><br/>Çıktı verilerinizi Azure'da depolamak, Azure depolama hesabı adı, depolama erişim anahtarı ve depolama kapsayıcısı adı belirtmeniz gerekir.</ul> |
| SQL veritabanı |Bir Azure SQL veritabanı'nda veya bir Azure sanal makinesinde çalışan SQL Server veritabanında depolanan verileri okur. |<b>Veritabanı sunucusu adı</b>: Veritabanını çalıştıran sunucunun adını belirtir.<br/><ul>Azure SQL veritabanı durumunda, oluşturulan sunucu adını girin. Genellikle bu biçimde  *&lt;generated_identifier&gt;. database.windows.net.* <br/><br/>Bir Azure sanal makinede barındırılan bir SQL server durumunda girin *tcp:&lt;sanal makine DNS adı&gt;, 1433*</ul><br/><b>Veritabanı adı </b>: Sunucuda veritabanı adını belirtir. <br/><br/><b>Server kullanıcı hesabı adı</b>: Veritabanı için erişim izinleri olan bir hesabın kullanıcı adını belirtir. <br/><br/><b>Server kullanıcı hesabı parolası</b>: Kullanıcı hesabının parolasını belirtir.<br/><br/><b>Veritabanı sorgusu</b>: okumak istediğiniz verileri tanımlayan bir SQL deyimi girin. |
| Şirket içinde SQL veritabanı |Bir şirket içi SQL veritabanı'nda depolanan verileri okur. |<b>Veri ağ geçidi</b>: Veri Yönetimi ağ geçidi, SQL Server veritabanınıza erişebildiği bir bilgisayarda yüklü adını belirtir. Ağ geçidini ayarlama hakkında daha fazla bilgi için bkz. [kullanarak verileri şirket içi SQL Server'dan Azure Machine Learning Studio ile Gelişmiş analiz gerçekleştirme](use-data-from-an-on-premises-sql-server.md).<br/><br/><b>Veritabanı sunucusu adı</b>: Veritabanını çalıştıran sunucunun adını belirtir.<br/><br/><b>Veritabanı adı </b>: Sunucuda veritabanı adını belirtir. <br/><br/><b>Server kullanıcı hesabı adı</b>: Veritabanı için erişim izinleri olan bir hesabın kullanıcı adını belirtir. <br/><br/><b>Kullanıcı adı ve parola</b>: Tıklayın <b>değerleri girin</b> veritabanı kimlik bilgilerinizi girmeniz gerekmez. Windows tümleşik kimlik doğrulaması veya SQL Server şirket içi SQL Server'ınızı nasıl yapılandırıldığına bağlı olarak kimlik doğrulaması kullanabilirsiniz.<br/><br/><b>Veritabanı sorgusu</b>: okumak istediğiniz verileri tanımlayan bir SQL deyimi girin. |
| Azure Tablosu |Azure depolama tablo hizmeti veri okur.<br/><br/>Azure tablo hizmeti, büyük miktarlarda verinin seyrek okuma kullanın. Esnek, sağlayan ilişkisel olmayan (NoSQL), yüksek düzeyde ölçeklenebilir, Hesaplı ve yüksek oranda kullanılabilir bir depolama çözümü. |Seçenekler **verileri içeri aktarma** , genel bilgi ya da oturum açma kimlik bilgileri gerektiren özel bir depolama hesabı erişim bağlı olarak değiştirin. Bu belirlenir <b>kimlik doğrulama türü</b> değeri "PublicOrSAS" veya "Account", kendi parametrelerinin her biri sahip olabilir. <br/><br/><b>Genel veya paylaşılan erişim imzası (SAS) URI</b>: Parametreler şunlardır:<br/><br/><ul><b>Tablo URI</b>: Tablo için genel veya SAS URL'sini belirtir.<br/><br/><b>Özellik adlarını taramak için satırı belirten</b>: Değerler <i>üst n</i> belirtilen sayıda satırı, tarama veya <i>ScanAll</i> tablodaki tüm satırları alınamıyor. <br/><br/>Veriler homojen ve tahmin edilebilir değilse, seçtiğiniz önerilir *üst n* ve N. için bir sayı girin Büyük tablolar için bu daha hızlı okuma kez sonuçlanabilir.<br/><br/>Veri kümeleri özelliklerinin derinliği göre değişir ve tablonun konumu ile yapılandırılırsa seçin *ScanAll* tüm satırları taramak için seçeneği. Bu meta veri dönüştürme ve sonuçta elde edilen özelliği bütünlüğü sağlar.<br/><br/></ul><b>Özel depolama hesabı</b>: Parametreler şunlardır: <br/><br/><ul><b>Hesap adı</b>: Okunacak tabloyu içeren hesabının adını belirtir.<br/><br/><b>Hesap anahtarı</b>: Hesapla ilişkili depolama anahtarını belirtir.<br/><br/><b>Tablo adı</b> : Okunacak veriler içeren bir tablo adını belirtir.<br/><br/><b>Özellik adlarını taranacak satır</b>: Değerler <i>üst n</i> belirtilen sayıda satırı, tarama veya <i>ScanAll</i> tablodaki tüm satırları alınamıyor.<br/><br/>Veriler homojen ve tahmin edilebilir değilse, seçtiğiniz öneririz *üst n* ve N. için bir sayı girin Büyük tablolar için bu daha hızlı okuma kez sonuçlanabilir.<br/><br/>Veri kümeleri özelliklerinin derinliği göre değişir ve tablonun konumu ile yapılandırılırsa seçin *ScanAll* tüm satırları taramak için seçeneği. Bu meta veri dönüştürme ve sonuçta elde edilen özelliği bütünlüğü sağlar.<br/><br/> |
| Azure Blob Depolama |Resimler, yapılandırılmamış metin veya ikili veriler de dahil olmak üzere Azure depolama, Blob hizmetinde depolanan verileri okur.<br/><br/>Blob hizmeti, verileri genel olarak kullanıma sunmak veya uygulama verilerini özel olarak depolamak için kullanabilirsiniz. Her yerden verilerinize erişebilirsiniz HTTP veya HTTPS bağlantıları kullanarak. |Seçenekler **verileri içeri aktarma** genel bilgi ya da oturum açma kimlik bilgileri gerektiren özel bir depolama hesabı erişmeye çalıştığınız bağlı olarak değişiklik modülü. Bu belirlenir <b>kimlik doğrulama türü</b> "PublicOrSAS" veya "Hesap" bir değer olabilir.<br/><br/><b>Genel veya paylaşılan erişim imzası (SAS) URI</b>: Parametreler şunlardır:<br/><br/><ul><b>URI</b>: Depolama blobu, genel veya SAS URL'sini belirtir.<br/><br/><b>Dosya biçimine</b>: Blob hizmetinde verilerin biçimini belirtir. Desteklenen biçimler şunlardır: CSV, TSV ve ARFF'ye.<br/><br/></ul><b>Özel depolama hesabı</b>: Parametreler şunlardır: <br/><br/><ul><b>Hesap adı</b>: Okumak istediğiniz blob içeren hesabının adını belirtir.<br/><br/><b>Hesap anahtarı</b>: Hesapla ilişkili depolama anahtarını belirtir.<br/><br/><b>Kapsayıcı, dizin veya blob yolu </b> : Okunacak verileri içeren blob adını belirtir.<br/><br/><b>BLOB dosya biçimi</b>: Blob hizmetinde verilerin biçimini belirtir. Desteklenen veri biçimlerini belirtilen kodlama ve Excel, CSV, TSV, ARFF'ye, CSV olan. <br/><br/><ul>Biçim, CSV veya TSV ise, dosyanın bir üst bilgi satırı içerip içermediğini belirtmek emin olun.<br/><br/>Excel çalışma kitaplarından veri okumak için Excel seçeneğini kullanabilirsiniz. İçinde <i>Excel veri biçimi</i> seçeneğinde, gösteren veriler bir Excel çalışma sayfası aralıktaki ya da bir Excel tablosunda olup olmadığı. İçinde <i>Excel sayfası veya katıştırılmış tablo </i>seçeneğinde, sayfadaki veya okumak istediğiniz tablo adını belirtin.</ul><br/> |
| Veri akışı sağlayıcısı |Desteklenen bir akış Sağlayıcısı'ndan veri okur. Şu anda yalnızca açık veri Protokolü (OData) biçiminde desteklenir. |<b>Veri içerik türü</b>: OData biçimini belirtir.<br/><br/><b>Kaynak URL</b>: Veri akışı tam URL'sini belirtir. <br/>Örneğin, aşağıdaki URL, Northwind örnek veritabanından okur: https://services.odata.org/northwind/northwind.svc/ |

## <a name="import-from-another-experiment"></a>Başka bir denemeden içeri aktarma

Ne zaman bir denemeden bir ara sonuç alıp başka bir denemeden bir parçası olarak kullanmak isteyeceksiniz kez olacaktır. Bunu yapmak için modül bir veri kümesi olarak Kaydet:

1. Bir veri kümesi olarak kaydetmek istediğiniz bir modülün çıkışına tıklayın.
2. Tıklayın **veri kümesi olarak Kaydet**.
3. İstendiğinde, bir ad ve izin, veri kümesi bir kolayca belirlemek bir açıklama girin.
4. Tıklayın **Tamam** onay işareti.

Kayıt tamamlandığında, veri kümesini çalışma alanınızdaki tüm deneme içinde kullanmak için kullanılabilir. İçinde bulabilirsiniz **kaydedilmiş veri kümeleri** modül paletindeki listesi.

## <a name="next-steps"></a>Sonraki adımlar

[Veri içeri aktarma ve veri gönderme modüllerini kullanan Azure Machine Learning studio web hizmetleri dağıtma](web-services-that-use-import-export-modules.md)


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C/


<!-- Module References -->
[convert-to-dataset]: https://msdn.microsoft.com/library/azure/72bf58e0-fc87-4bb1-9704-f1805003b975/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
