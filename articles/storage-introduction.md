<properties urlDisplayName="Introduction to Azure Storage" pageTitle="Depolamaya Giriş | Microsoft Azure" metaKeywords="Get started  Azure storage introduction  Azure storage overview  Azure blob   Azure unstructured data   Azure unstructured storage   Azure blob   Azure blob storage  Azure queue   Azure asynchronous processing   Azure queue   Azure queue storage Azure table   Azure nosql   Azure large structured data store   Azure table   Azure table storage  Azure file storage  Azure file  Azure file share  Azure " description="An overview of Microsoft Azure Storage." metaCanonical="" disqusComments="1" umbracoNaviHide="1" services="storage" documentationCenter="" title="Introduction to Microsoft Azure Storage" authors="tamram" manager="adinah" editor="cgronlun" />

<tags ms.service="storage" ms.workload="storage" ms.tgt_pltfrm="na" ms.devlang="na" ms.topic="article" ms.date="01/01/1900" ms.author="tamram" />

# Microsoft Azure Depolama'ya Giriş

Bu makalede geliştiriciler, BT Uzmanları ve iş kararı yetkilileri için Microsoft Azure Depolama'ya giriş verilmiştir. Bunu okuduğunuzda şunları öğrenirsiniz:

- Azure Depolama nedir ve bulut, mobil, sunucu ve masaüstü uygulamalarınızda ondan nasıl yararlanabilirsiniz
- Azure Depolama hizmetlerine ne tür veriler depolayabilirsiniz: Blob, Tablo, Sıra ve Dosya depolama
- Azure Depolama'da verilerinize erişim nasıl yönetilir
- Azure Depolama verileriniz artıklık ve çoğaltma aracılığıyla nasıl korunur 
- İlk Azure Depolama uygulamanızı oluşturmak için atılacak sonraki adım

## Azure Depolama nedir? ##

Bulut bilgi işlem, verileri için ölçeklenebilir, dayanıklı ve yüksek oranda kullanılabilir depolama gerektiren uygulamalar için yeni senaryolara olanak verir; Microsoft'un Azure Depolama'yı geliştirme nedeni de tam olarak budur. Azure Depolama, geliştiricilerin yeni senaryoları desteklemek amacıyla büyük ölçekli uygulamalar geliştirmelerini mümkün kılmanın yanı sıra, sağlamlığının başka bir kanıtı olarak Azure Sanal Makineler için depolama temeli de sağlar. 

Azure Depolama son derece ölçeklenebilir olduğundan bilimsel, finansal analiz ve medya uygulamaları için gereken büyük veri senaryolarını desteklemek üzere yüzlerce terabaytlık verileri depolayabilir ve işleyebilirsiniz. Ya da bir küçük işletme Web sitesi için gereken küçük miktarlardaki verileri depolayabilirsiniz. İhtiyaçlarınız hangi düzeyde olursa olsun yalnızca depoladığınız verilerin ödemesini yaparsınız. Azure Depolama, şu an için onlarca trilyon benzersiz müşteri nesnesini depolamakta ve ortalama olarak saniyede milyonlarca isteğin işlemini yapmaktadır. 

Azure Depolama elastik olduğundan, global düzeyde büyük bir hedef kitle için uygulamalar tasarlayabilir ve o uygulamaları hem depolanan veri miktarı hem de bunlara yapılan istek sayısı anlamında ihtiyaca göre ölçeklendirebilirsiniz. Yalnızca kullandığınız kadarı için ve yalnızca kullandığınız zaman ödeme yaparsınız.

Azure Depolama, veri yükünüzü trafiğe göre otomatik olarak dengeleyen bir otomatik bölümleme sistemini kullanır. Buna göre uygulamanızın talepleri büyüdükçe Azure Depolama bunları karşılamak için gereken kaynakları otomatik olarak ayırır. 

Azure Depolama'ya dünya üzerinde her yerden, ister bulutta, ister masaüstünde isterse şirket sunucusunda veya mobil ya da tablet cihazda çalışan her tür uygulamadan erişilebilir. Azure Depolama'yı mobil senaryolarda kullanabilirsiniz; burada uygulama, verilerin bir alt kümesini cihazda depolar ve onları bulutta depolanmış tam veri kümesiyle eşitler.

Azure Depolama, geliştirmeyi kolaylaştırmak için çok çeşitli işletim sistemlerini (Windows ve Linux dahil) kullanan kullanıcıları ve çeşitli programlama dillerini (.NET, Java ve C++ dahil) destekler. Azure Depolama ayrıca HTTP/HTTPS üzerinden veri gönderip alabilen her istemcinin kullanabildiği basit REST API'leri aracılığıyla veri kaynaklarını kullanıma açar. 

## Azure Depolama Hizmetlerine Giriş ##

Azure Depolama hizmetleri Blob depolama, Tablo depolama, Sıra depolama ve Dosya depolamadır:

- **Blob depolama** dosya verilerini depolar. Bir blob belge, medya dosyası veya uygulama yükleyici gibi herhangi bir türde metin veya ikili veri dosyası olabilir. 
- **Tablo depolama** yapısal veri kümelerini depolar. Tablo depolama, hızlı geliştirmeye ve büyük miktarlardaki verilere hızlı erişime izin veren bir NoSQL anahtar-öznitelik veri deposudur.
- **Sıra depolama** iş akışı işlemleri ve bulut hizmetlerinin bileşenleri arasındaki iletişim için güvenilir mesajlaşma desteği sağlar.
- **Dosya depolama** , standart SMB 2.1 protokolünü kullanan eski uygulamalar için paylaşılan depolama sunar. Azure sanal makineleri ve bulut hizmetleri bağlı paylaşımlar aracılığıyla dosya verilerini uygulama bileşenleri arasında paylaşabilir ve şirket içi uygulamalar bir paylaşımdaki dosya verilerine Dosya hizmeti REST API'si aracılığıyla erişebilir.

Blob, Tablo ve Sıra depolama, her depolama hesabında bulunurken Dosya depolama [Azure Preview sayfasından](/tr-tr/services/preview/) istek üzerine kullanılabilir.

Depolama hesabı, Azure Depolama'ya erişmenizi sağlayan benzersiz bir ad alanıdır. Her depolama hesabı 500 TB'a kadar birleşik blob, sıra, tablo ve dosya verisi içerebilir. Azure depolama hesabının kapasitesi hakkında ayrıntılar için bkz. [Azure Depolama'nın Ölçeklenebilirliği ve Performans Hedefleri](http://msdn.microsoft.com/library/windowsazure/dn249410.aspx).

Aşağıdaki resimde Azure depolama kaynakları arasındaki ilişki gösterilmiştir:

![Azure Storage Resources](./media/storage-introduction/storage-concepts.png)

Depolama hesabı oluşturabilmeniz için çeşitli Azure hizmetlerine erişebilmenizi sağlayan bir tarife olan Azure aboneliğiniz olmalıdır. Tek bir abonelikle benzersiz ada sahip 100 kadar depolama hesabı oluşturabilirsiniz. Toplu alım fiyatı hakkında bilgi için bkz. [Depolama Fiyat Ayrıntıları](http://www.windowsazure.com/tr-tr/pricing/details/storage/).

[Ücretsiz deneme sürümü](/tr-tr/pricing/free-trial/) ile Azure kullanmaya başlayabilirsiniz. Bir tarifeyi satın almaya karar verdiğinizde çeşitli [satın alma seçeneklerinden](/tr-tr/pricing/purchase-options/) birini seçebilirsiniz. [MSDN abonesiyseniz](/tr-tr/pricing/member-offers/msdn-benefits-details/) Azure Depolama dahil Azure hizmetlerinde kullanabileceğiniz aylık ücretsiz krediler alırsınız.

## Blob Depolama ##

Büyük miktarlarda yapısal olmayan verileri olan kullanıcıların buluta depolayabilmesi için Blob depolama uygun maliyetli ve ölçeklenebilir bir çözüm sunar. Aşağıdaki içerikleri depolamak için Blob depolamayı kullanabilirsiniz:

- Belgeler 
- Fotoğraf, video, müzik ve blog gibi sosyal veriler
- Dosya, bilgisayar, veritabanı ve cihazların yedekleri
- Web uygulamalarının görüntü ve metinleri
- Bulut uygulamalarının yapılandırma verileri
- Günlükler ve diğer hacimli veri kümeleri gibi büyük veriler

Her blob bir kapsayıcı halinde düzenlenmiştir. Kapsayıcılar ayrıca nesne gruplarına güvenlik ilkeleri atamak için kullanışlı bir yol sağlarlar. Bir depolama hesabı herhangi sayıda kapsayıcı içerebilir ve bir kapsayıcı, depolama hesabının 500 TB'lık kapasite sınırına kadar herhangi sayıda blob içerebilir.  

Blob depolama iki tür blob sunar: blok blob'ları ve sayfa blob'ları (diskler). Blok blob'ları bulut nesnelerini akışla aktarmak ve depolamak üzere iyileştirilmiş olup belgeleri, medya dosyalarını, yedeklemeleri vb. depolamak için iyi bir seçenektir. Bir blok blob'unun boyutu 200 GB kadar olabilir. Sayfa blob'ları IaaS disklerini gösterecek ve rastgele yazmaları destekleyecek şekilde iyileştirilmiş olup boyutu 1 TB kadar olabilir. IaaS diski takılı bir Azure sanal makine ağı sayfa blob'u olarak depolanan bir VHD'dir.

Ağ kısıtlamalarının Blob depolamaya ağ üzerinden veri yüklenmesini veya indirilmesini gerçekçi kılmadığı çok büyük veri kümelerinde, [Azure İçeri/Dışarı Aktarma Hizmeti](http://azure.microsoft.com/tr-tr/documentation/articles/storage-import-export-service/)'ni kullanarak verileri doğrudan veri merkezinden içeri veya dışarı aktarması için Microsoft'a sabit sürücü gönderebilirsiniz. Ayrıca blob verilerini depolama hesabınız içinde veya depolama hesaplarınız arasında kopyalayabilirsiniz. 

## Tablo Depolama ##

Modern uygulamalar çoğunlukla önceki nesil yazılımların istediğinden daha büyük ölçeklenebilirliğe ve esnekliğe sahip veri depoları talep eder. Tablo depolamanın sunduğu yüksek oranda kullanılabilen, büyük hacimlerde ölçeklenebilen depolama alanı sayesinde, uygulamanız ihtiyacınızı karşılayacak şekilde otomatik olarak ölçeklenebilir. Tablo depolama Microsoft'un NoSQL anahtar/öznitelik deposudur; şemasız tasarımı nedeniyle geleneksel ilişkisel veritabanlarından farklıdır. Şemasız bir veri deposunda, uygulamanızın gereksinimleri geliştikçe verilerinize uyum sağlamak kolaydır. Tablo depolamayı kullanmak kolay olduğundan geliştiriciler hızla uygulama oluşturabilirler. Her tür uygulamada verilere erişim hızlı ve uygun maliyetlidir.  Tablo depolamanın maliyeti genellikle benzer hacimlerdeki veri için geleneksel SQL'den anlamlı düzeyde daha düşüktür.

Tablo depolama bir anahtar-öznitelik deposudur, yani tablodaki her değer, türü belirtilmiş bir özellik adıyla depolanır. Bu özellik adı, seçim ölçütlerini filtrelemek ve belirtmek için kullanılabilir. Özelliklerin ve onlara ait değerlerin koleksiyonu bir varlık oluşturur. Tablo depolama şemasız olduğundan, aynı tablodaki iki varlık farklı özellik koleksiyonları içerebilir ve bu özellikler farklı türlerde olabilir. 

Web uygulamalarına ait kullanıcı verileri, adres defterleri, cihaz bilgileri ve hizmetinizin gerektirdiği diğer herhangi bir meta veri türü gibi esnek veri kümelerini depolamak için Tablo depolamayı kullanabilirsiniz.  Tabloda istediğiniz sayıda varlık depolayabilirsiniz ve depolama hesabı kendi kapasite sınırına kadar herhangi sayıda tabloyu içerebilir.

Blob'larda ve Sıralarda olduğu gibi geliştiriciler standart REST protokollerini kullanarak Tablo Depolamayı yönetebilir ve ona erişebilirler, ancak Tablo Depolama OData protokolünün bir alt kümesini de destekleyerek gelişmiş sorgulama özelliklerini basitleştirir ve JSON ve AtomPub (XML tabanlı) biçimlerinin her ikisine de olanak tanır.

Günümüz İnternet tabanlı uygulamalarında, Tablo depolama gibi NoSQL veritabanları geleneksel ilişkisel veritabanlarına popüler bir alternatif oluştururlar. 

## Sıra Depolama ##

Ölçeklemeye yönelik uygulamalar tasarlarken uygulama bileşenleri bağımsız olarak ölçeklenebilmeleri için çoğunlukla birbirinden ayrılırlar. Sıra depolama, uygulama bileşenlerinin bulutta, masaüstünde, şirket içi bir sunucuda veya bir mobil cihazda çalıştırılmasından bağımsız olarak bileşenler arasında zaman uyumsuz iletişim için güvenilir bir mesajlaşma çözümü sağlar. Sıra depolama özelliği zaman uyumsuz görevleri yönetmeyi ve işlem iş akışları oluşturmayı da destekler. 

Depolama hesabı herhangi bir sayıda sıra içerebilir. Bir sıra ise depolama hesabının kapasite sınırına kadar herhangi sayıda mesaj içerebilir. Tek mesaj boyutu 64 KB kadar olabilir.

## Dosya Depolama ##

Birçok eski uygulama, buluta taşınmalarını karmaşıklaştıran bir bağımlılık olan dosya paylaşımlarını kullanır. Dosya depolama ise eski uygulamalarınıza hızla ve pahalı yeniden yazma işlemleri olmadan Azure'a geçirebilmeniz için bulut tabanlı dosya paylaşımları sunar. 

Azure sanal makinelerinde veya bulut hizmetlerinde çalışan uygulamalar dosya verilerine erişmek için tıpkı bir masaüstü uygulamasının normal bir SMB paylaşımını bağlaması gibi Dosya depolama paylaşımını bağlayabilir. Herhangi sayıda uygulama bileşeni Dosya depolama paylaşımına aynı anda bağlanabilir ve erişebilir.

Dosya depolama paylaşımı standart bir SMB 2.1 dosya paylaşımı olduğundan Azure'de çalışan uygulamalar dosya sistemi G/Ç API'leri aracılığıyla bu paylaşımdaki verilere erişebilirler. Geliştiriciler bu sayede mevcut kodlarından ve becerilerinden yararlanarak var olan uygulamalarını geçirebilirler. BT Uzmanları PowerShell cmdlet'lerini kullanarak Azure uygulamalarının yönetiminin bir parçası olarak Dosya depolama paylaşımları oluşturabilir, bağlayabilir ve yönetebilirler.

Diğer Azure depolama hizmetlerinde olduğu gibi paylaşımdaki verilere erişmek için Dosya depolama bir REST API'sini açığa çıkarır. Şirket içi uygulamalar dosya paylaşımındaki verilere erişmek için Dosya depolama REST API'sini çağırabilirler. Kuruluş bu yolla bazı eski uygulamalarını Azure'e geçirirken diğerlerini kendi kuruluşu içinden çalıştırmaya devam etmeyi seçebilir. Dosya paylaşımı bağlamanın yalnızca Azure'de çalışan uygulamalar için mümkün olduğuna dikkat edin; şirket içi bir uygulama dosya paylaşımına yalnızca REST API aracılığıyla erişebilir.

Dağıtılmış uygulamalar, yararlı uygulama verileri ile geliştirme ve test araçlarını depolamak ve paylaşmak için Dosya depolamayı da kullanabilir. Örneğin, bir uygulama yapılandırma dosyalarını ve günlükler, ölçümler ve kilitlenme bilgisi dökümleri gibi tanılama verilerini bir Dosya depolama paylaşımına depolayabilir böylece bunlar birden çok sanal makine veya rol tarafından kullanılabilir. Geliştiriciler ve yöneticiler bir uygulama geliştirmek veya yönetmek için ihtiyaç duydukları yardımcı programları her sanal makineye veya rol örneğine yüklemek yerine tüm bileşenlerin kullanabildiği bir Dosya depolama paylaşımına depolayabilir.


## Blob, Tablo, Sıra ve Dosya Kaynaklarına Erişim ##

Varsayılan seçenek olarak yalnızca depolama hesabı sahibi depolama hesabındaki kaynaklara erişebilir. Verilerinizin güvenliği için hesabınızdaki kaynaklara yapılan her isteğin kimliği doğrulanmalıdır. Kimlik doğrulama işlemi Paylaşılan Anahtar modelini kullanır. Blob'lar anonim kimlik doğrulamayı destekleyecek şekilde de yapılandırılabilir. 

Depolama hesabınız oluşturulduğu sırada ona kimlik doğrulama için kullanılan iki özel erişim anahtarı atanır. Genel bir güvenlik anahtarı yönetimi uygulaması olarak anahtarları düzenli olarak yeniden oluşturuyorsanız iki anahtar olması uygulamanızın kullanılabilir durumda olmasını sağlar.

Kullanıcıların depolama kaynaklarınıza erişmesine denetimli olarak izin vermeniz gerekiyorsa [paylaşılan erişim imzası](../storage-dotnet-shared-access-signature-part-1/) oluşturabilirsiniz. Paylaşılan erişim imzası, URL'nin sonuna eklenebilen ve bir kapsayıcıya, blob'a, tabloya veya sıraya erişme izni vermeyi sağlayan bir belirteçtir. Bu belirtece sahip olan herkes belirtecin işaret ettiği kaynağa belirttiği izinlerle ve geçerli olduğu süre boyunca erişebilir. Azure Dosya depolamanın paylaşılan erişim imzalarını şimdilik desteklemediğini unutmayın.

Son olarak, bir kapsayıcının ve onun blob'larının veya belirli bir blob'un genel erişime açık olmasını belirtebilirsiniz. Kapsayıcının veya blob'un genel olduğunu belirttiğinizde, herkes anonim olarak onu okuyabilir; bir kimlik doğrulama gerekmez.  Genel kapsayıcılar ve blob'lar web sitelerinde barındırılan medya ve belgeler gibi kaynakları sunmak için kullanışlıdır.  Genel hedef kitleye yönelik ağ gecikmesini azaltmak için Azure CDN'si olan web siteleri tarafından kullanılan blob verilerini önbelleğe kaydedebilirsiniz.

## Dayanıklılık ve Yüksek Kullanılabilirlik İçin Çoğaltma ##

[WACOM.INCLUDE [depolama-çoğaltma-seçenekler](../includes/storage-replication-options.md)]

## Fiyatlar ##

Müşteriler Azure Depolama için dört etkene bakılarak ücretlendirilir: kullanılan depolama kapasitesi, seçilen çoğaltma seçeneği, hizmete yapılan istek sayısı ve veri çıkışı. 

Depolama kapasitesi, verileri depolamak için tahsis edilen depolama hesabınızın ne kadarını kullandığınız anlamına gelir. Sadece verilerinizi depolama maliyeti, ne kadar veri depoladığınız ve bunların nasıl çoğaltıldığıyla belirlenir. Azure Depolamaya yapılan her okuma ve yazma işlemi de hizmete karşı bir istek oluşturur. Veri çıkışı, Windows Azure bölgesinin dışına aktarılan veriler anlamına gelir. Depolama hesabınızdaki verilere aynı bölgede çalışmayan bir uygulama tarafından erişildiğinde bu uygulama ister bir bulut hizmeti ister başka tür bir uygulama olsun, veri çıkışı için ücretlendirilirsiniz. (Windows Azure hizmetlerinde işlem ve veri çıkış ücretlerini azaltmak ya da ortadan kaldırmak için veri ve hizmetlerinizi aynı veri merkezlerinde gruplandırmak için adımlar atabilirsiniz.) 

[Depolama Fiyat Ayrıntıları](/tr-tr/pricing/details/storage/) sayfası depolama kapasitesi, çoğaltma ve işlemlerle ilgili ayrıntılı fiyat bilgileri sağlar. [Veri Aktarımları Fiyat Ayrıntıları](/tr-tr/pricing/details/data-transfers/), veri çıkışı için ayrıntılı fiyat bilgileri sağlar. Maliyetlerinizi tahmin etmeye yardımcı olması için [Azure Depolama Ücret Hesaplayıcı](/tr-tr/pricing/calculator/?scenario=data-management)'yı kullanabilirsiniz.

## Depolama Özelliğini Geliştirme ##

Azure Depolama, HTTP/HTTPS istekleri yapabilen her dilden çağrılabilen bir [REST API](http://msdn.microsoft.com/library/windowsazure/dd179355.aspx) aracılığıyla depolama kaynaklarını kullanıma açar. Ayrıca, Azure Depolama birkaç popüler dilde programlama kitaplıkları sunar. Bu kitaplıklar, zaman uyumlu ve zaman uyumsuz çağrı, işlemlerin toplu yapılması, özel durum yönetimi, otomatik yeniden denemeler, işletimsel davranış ve benzeri ayrıntıları işleyerek Azure Depolama ile çalışmayı birçok yönden basitleştirir. Kitaplıklar şu an için aşağıdaki dil ve platformlarda kullanılabilmektedir ve diğerleri de sıradadır:

- [.NET](http://go.microsoft.com/fwlink/?LinkID=390731)
- [Doğal kod](http://msdn.microsoft.com/library/dn495438.aspx)
- [Java/Android](/tr-tr/develop/java/)
- [Node.js](/tr-tr/develop/nodejs/)
- [PHP](/tr-tr/develop/php/)
- [Ruby](/tr-tr/develop/ruby/)
- [Python](/tr-tr/develop/python/)
- [PowerShell](http://msdn.microsoft.com/library/dn495240.aspx)

## Sonraki Adımlar ##

Azure Depolama kullanmaya başlamak için şu kaynakları araştırın:

- [Azure Depolama Belgeleri](/tr-tr/documentation/services/storage/)
- [Azure Depolama Ölçeklenebilirlik ve Performans Hedefleri](http://msdn.microsoft.com/library/windowsazure/dn249410.aspx)

<h3>.NET Geliştiricileri için</h3>

- [.NET'ten Blob Depolamayı kullanma](../storage-dotnet-how-to-use-blobs/)
- [.NET'ten Tablo Depolamayı kullanma](../storage-dotnet-how-to-use-tables/)
- [.NET'ten Sıra Depolamayı kullanma](../storage-dotnet-how-to-use-queues/)

<h3>Java/Android Geliştiricileri için</h3>

- [Java/Android'den Blob Depolamayı kullanma](../storage-java-how-to-use-blob-storage/)
- [Java/Android'den Tablo Depolamayı kullanma](../storage-java-how-to-use-table-storage/)
- [Java/Android'den Sıra Depolamayı kullanma](../storage-java-how-to-use-queue-storage/)

<h3>Node.js Geliştiricileri için</h3>

- [Node.js'den Blob Depolamayı kullanma](../storage-nodejs-how-to-use-blob-storage/)
- [Node.js'den Tablo Depolamayı kullanma](../storage-nodejs-how-to-use-table-storage/)
- [Node.js'den Sıra Depolamayı kullanma](../storage-nodejs-how-to-use-queues/)

<h3>PHP Geliştiricileri için</h3>

- [PHP'den Blob Depolamayı kullanma](../storage-php-how-to-use-blobs/)
- [PHP'den Tablo Depolamayı kullanma](../storage-php-how-to-use-table-storage/)
- [PHP'den Sıra Depolamayı kullanma](../storage-php-how-to-use-queues/)

<h3>Ruby Geliştiricileri için</h3>

- [Ruby'den Blob Depolamayı kullanma](../storage-ruby-how-to-use-blob-storage/)
- [Ruby'den Tablo Depolamayı kullanma](../storage-ruby-how-to-use-table-storage/)
- [Ruby'den Sıra Depolamayı kullanma](../storage-ruby-how-to-use-queue-storage/)

<h3>Python Geliştiricileri için</h3>

- [Python'dan Blob Depolamayı kullanma](../storage-python-how-to-use-blob-storage/)
- [Python'dan Tablo Depolamayı kullanma](../storage-python-how-to-use-table-storage/)
- [Python'dan Sıra Depolamayı kullanma](../storage-python-how-to-use-queue-storage/)
