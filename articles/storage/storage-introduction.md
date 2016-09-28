<properties
    pageTitle="Depolamaya Giriş | Microsoft Azure"
    description="Microsoft’un buluttaki çevrimiçi veri depolama alanı Azure Storage’a göz atalım. Uygulamalarınızda en uygun bulut depolama çözümünü nasıl kullanabileceğinizi öğrenin."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="vamshik;tamram"/>


# Microsoft Azure Storage’a Giriş

## Genel Bakış

Azure Storage, müşterilerin ihtiyaçlarını karşılamak üzere sağlamlık, kullanılabilirlik ve ölçeklenebilirliğe dayanan modern uygulamalara yönelik bulut depolama çözümüdür. Bu makaleyi okuyan geliştiriciler, BT Uzmanları ve kurumsal karar alıcılar şu konularda bilgi edinebilir:

- Azure Storage’ın içeriği ve bulut, mobil, sunucu ve masaüstü uygulamalarınızda Azure Storage’dan nasıl faydalanılabileceği
- Azure Storage hizmetleri ile depolayabileceğiniz veriler: blob (nesne) verileri, NoSQL tablo verileri, kuyruk mesajları ve dosya paylaşımları
- Azure Storage’da verilerinize erişimin nasıl yönetildiği
- Azure Storage verilerinizin yedekleme ve çoğaltma ile nasıl dayanıklı hale getirildiği
- İlk Azure Storage uygulamanızı oluşturmak için yapılması gerekenler

Azure Storage’ı sorunsuz şekilde kullanmaya başlamak için bkz. [Beş dakikada Azure Storage’a başlayın](storage-getting-started-guide.md).

Azure Storage ile çalışma konusunda gerekli araçlar, kitaplıklar ve diğer kaynaklar için aşağıda yer alan [Sonraki Adımlar](#next-steps)’a göz atın.

## Azure Storage nedir?

Bulut bilgi işlem; veriler için ölçeklenebilir, sağlam ve yüksek seviyede kullanılabilir depolama alanına ihtiyaç duyan uygulamalara yönelik yeni senaryolara olanak tanıyor. Microsoft’un Azure Storage’ı geliştirmesinin nedeni budur. Azure Storage, geliştiricilerin yeni senaryoları desteklemek üzere geniş ölçekli uygulamalar oluşturmalarını mümkün kılmanın yanında, sağlamlığının daha güçlü bir kanıtı olarak Azure Virtual Machines için depolamaya temel oluşturuyor.

Azure Storage çok büyük boyutta ölçeklenebilir. Bu sayede bilimsel, finansal analiz ve medya uygulamalarının ihtiyaç duyduğu büyük veri senaryolarını desteklemek üzere yüzlerce terabayt veriyi depolayıp işleyebilirsiniz. Veya küçük ölçekli bir işletmenin web sitesi için gerekli olan küçük miktarda veriyi depolayabilirsiniz. İhtiyaçlarınız ne olursa olsun yalnızca depoladığınız veri için ücret ödersiniz. Şu anda Azure Storage onlarca trilyon benzersiz müşteri nesnesi depolamaktadır ve saniyede ortalama olarak milyonlarca talebi ele alır.

Azure Storage esnektir, böylece geniş global kullanıcılara yönelik uygulamalar tasarlayabilir ve hem depolanan veri miktarı hem de bu verilerle ilgili gerçekleştirilen talep sayısı açısından söz konusu uygulamaları gerektiği şekilde ölçekleyebilirsiniz. Yalnızca kullandıklarınız için ve kullandığınız zaman ücret ödersiniz.

Azure Storage trafiğe bağlı olarak verilerinize otomatik olarak yük dengelemesi yapan bir otomatik bölümleme sistemi kullanır. Bu, uygulamanıza yönelik talepler arttıkça Azure Storage’ın bunları karşılamak üzere ilgili kaynakları otomatik olarak ayıracağı anlamına gelir.

Azure Storage hizmetine bulutta, masaüstünde veya şirket içi bir sunucuda, mobil cihazlarda veya tablette çalışan her tür uygulama aracılığıyla dünyanın her yerinden erişilebilir. Azure Storage’ı, uygulamanın bir veri alt kümesini cihazda depoladığı ve buluttaki eksiksiz veri kümesi ile eşitlediği mobil senaryolarda kullanabilirsiniz.

Azure Storage, geliştirme sürecini kolaylaştırmak için farklı işletim sistemleri (Windows ve Linux dahil) kullanan istemcileri ve farklı programlama dillerini (.NET, Java, Node.js, Python, Ruby, PHP, C++ ve mobil programlama dilleri) destekler. Azure Storage HTTP/HTTPS üzerinden veri gönderme ve alma özelliğine sahip herhangi bir istemcinin kullanabileceği basit REST API’leri aracılığıyla veri kaynaklarını da gösterir.

Azure Premium Storage, Azure Virtual Machines’de çalışan G/Ç açısından yoğun iş yükleri için yüksek performanslı ve düşük gecikmeli disk desteği sağlar. Azure Premium Storage ile birden çok kalıcı veri diskini sanal bir makineye bağlayabilir ve performans gereksinimlerinizi karşılayacak şekilde yapılandırabilirsiniz. Azure Premium Storage’da her veri diski maksimum G/Ç performansı için bir SSD disk ile desteklenir. Ayrıntılar için bkz. [Premium Storage: Azure Virtual Machines İş Yükleri İçin Yüksek Performanslı Depolama](storage-premium-storage.md).

## Azure Storage Hizmetlerine Giriş

Azure Storage şu dört hizmeti sunar: Blob Storage, Table Storage, Kuyruk depolama ve File Storage.

- Blob Storage yapılandırılmamış nesne verilerini depolar. Blob; bir belge, ortam dosyası veya uygulama yükleyici gibi herhangi bir türde metin veya ikili veri olabilir. Blob Storage ayrıca Nesne depolama olarak adlandırılır.
- Table Storage yapılandırılmış veri kümelerini depolar. Table Storage, yüksek miktarda verinin hızla dağıtılmasını ve verilere hızla erişilebilmesini sağlayan NoSQL anahtar özniteliği veri deposudur.
- Kuyruk Depolama bulut hizmetlerinin bileşenleri arasında iş akışı işlemeye ve iletişime yönelik güvenilir mesajlaşma sağlar.
- File Storage standart SMB protokolünü kullanan eski uygulamalar için paylaşılan depolama alanı sağlar. Azure Virtual Machines ve Cloud Services bağlı paylaşımlar üzerinden uygulama bileşenleri arasında dosya verileri paylaşabilir ve şirket içi uygulamalar, REST API Dosya hizmeti üzerinden dosya verilerine erişebilir.

Azure Storage hesabı, Azure Storage’da hizmetlerine erişmenizi sağlayan güvenli bir hesaptır. Depolama hesabınız depolama kaynaklarınız için benzersiz ad alanı sağlar. Aşağıdaki görüntü depolama hesabındaki Azure Storage kaynakları arasındaki ilişkiyi göstermektedir:

![Azure Storage Kaynakları](./media/storage-introduction/storage-concepts.png)

[AZURE.INCLUDE [storage-account-types-include](../../includes/storage-account-types-include.md)]

[AZURE.INCLUDE [storage-versions-include](../../includes/storage-versions-include.md)]

## Blob Storage

Blob Storage, bulutta depolanacak yüksek miktarda yapılandırılmamış nesne verisine sahip kullanıcılara uygun maliyetli ve ölçeklenebilir bir çözüm sunar. Blob Storage’ı kullanarak şu içerikleri depolayabilirsiniz:

- Belgeler
- Fotoğraf, video, müzik ve blog gibi sosyal veriler
- Dosyaların, bilgisayarların, veritabanlarının ve cihazların yedekleri
- Web uygulamaları için görüntüler ve metinler
- Bulut uygulamaları için yapılandırma verileri
- Günlükler ve diğer büyük veri kümeleri gibi büyük veriler

Her blob bir kapsayıcı halinde düzenlenmiştir. Kapsayıcılar ayrıca nesne gruplarına güvenlik ilkeleri atamaya ilişkin kullanışlı bir yöntem sunar. Bir depolama hesabının içerebileceği kapsayıcı sayısına ilişkin bir sınırlama yoktur. Bir kapsayıcı, depolama hesabının 500 TB kapasite sınırını dolduracak kadar blob içerebilir.  

Blob Storage blok blobları, ekleme blobları ve sayfa blobları (diskler) olmak üzere üç türde blob sunar.

- Blok blobları bulut nesnelerinin akış ve depolanması için en iyi duruma getirilmiştir ve belge, ortam dosyaları ve yedekler vb. öğelerin depolanması için uygun bir seçenektir.
- Ekleme blobları blok bloblarına benzer ancak ekleme işlemleri için en iyi duruma getirilmiştir. Bir ekleme blobu yalnızca sonuna yeni bir blok eklenerek güncelleştirilebilir. Ekleme blobları, yeni verilerin yalnızca blobun sonuna yazılması gereken günlüğe kaydetme gibi senaryolar için iyi bir seçenektir.
- Sayfa blobları IaaS disklerini temsil etmek ve rastgele yazmaları desteklemek üzere iyileştirilmiştir ve boyutları 1 TB’ye ulaşabilir. IaaS diskine bağlı bir Azure Virtual Machine ağı, sayfa blobu olarak kaydedilen bir VHD’dir.

Ağ kısıtlamalarının kablo üzerinden Blob Storage’a veri yükleme veya indirme yapmayı kullanışsız hale getirdiği çok büyük veri kümelerinde verileri doğrudan veri merkezinden içeri veya dışarı aktarmak için Microsoft’a bir sabit sürücü gönderebilirsiniz. Bkz: [Blob Storage’a Veri Aktarmak için Microsoft Azure İçeri/Dışarı Aktarma Hizmeti Kullanma](storage-import-export-service.md).

## Table Storage

Modern uygulamalar genellikle eski nesil yazılımların gerektirdiğinden daha fazla ölçeklenebilirlik ve esneklik özelliklerine sahip veri depoları gerektirir. Table Storage yüksek seviyede kullanılabilir ve ölçeklenebilir depolama sunar, böylece uygulamanız kullanıcı taleplerini karşılayacak şekilde otomatik olarak ölçeklendirilir. Table Storage Microsoft’un NoSQL anahtar/öznitelik deposudur ve geleneksel ilişkisel veritabanlarından farklı olarak şemasız bir tasarıma sahiptir. Şemasız veri deposu sayesinde, uygulamanızın ihtiyaçları geliştikçe verilerinizi uyarlamak da kolaylaşır. Table Storage’ın kullanımı son derece kolaydır, böylece geliştiriciler uygulamalarını hızla geliştirebilir. Her türlü uygulama için verilere erişim hızlı ve uygun maliyetlidir.  Table Storage, benzer hacimdeki veriler için geleneksel SQL’e oranla çok daha düşük maliyetlidir.

Table Storage bir anahtar öznitelik deposudur; bu, bir tablodaki her değerin türü belirtilmiş bir özellik adıyla depolandığı anlamına gelir. Özellik adı filtreleme ve seçim kriterlerinin belirlenmesi için kullanılabilir. Özellik ve değerlerinin toplamı bir varlığı oluşturur. Table Storage şemasız olduğu için aynı tablodaki iki varlık farklı özellik koleksiyonları içerebilir ve bu özellikler farklı türde olabilir.

Web uygulamaları için kullanıcı verileri, adres defterleri, cihaz bilgileri ve hizmetiniz için gerekli olan tüm diğer meta veri türleri gibi esnek veri kümelerini depolamak üzere Table Storage’ı kullanabilirsiniz.  Bir tabloda istediğiniz kadar varlık depolayabilirsiniz ve bir depolama hesabı kapasite limitini dolduracak kadar tablo içerebilir.

Geliştiriciler Bloblar ve Kuyruklarda olduğu gibi standart REST protokollerini kullanarak Table Storage’ı da yönetebilir ve depolamaya erişebilir. Ancak Table Storage gelişmiş sorgu özelliklerini basitleştiren ve hem JSON hem de AtomPub (XML tabanlı) biçimlerine olanak tanıyan OData protokolünün bir alt kümesini de destekler.

Bugünün İnternet tabanlı uygulamaları için Table Storage gibi NoSQL veri tabanları geleneksel veri tabanlarına göre popüler bir alternatif oluşturur.

## Kuyruk Depolama

Ölçeklendirmek üzere uygulama tasarlarken, uygulama bileşenleri birbirinden bağımsız şekilde ölçeklenebilmek için genellikle birbirinden ayrılır. Kuyruk depolama bulutta, masaüstünde, şirket içi sunucuda veya mobil bir cihazda çalışan uygulama bileşenleri arasındaki zaman uyumsuz iletişim için güvenilir bir mesajlaşma çözümü sunar. Kuyruk depolama zaman uyumsuz görevlerin yönetilmesini ve süreç iş akışlarının oluşturulmasını da destekler.

Bir depolama hesabı herhangi sayıda kuyruk içerebilir. Bir kuyruk, depolama hesabının kapasite sınırını dolduracak kadar ileti içerebilir. Tek bir ileti boyut olarak en fazla 64 KB olabilir.

## File Storage

Azure File Storage, dosya paylaşımlarına bağlı olan eski uygulamaları maliyetli yeniden yazdırmaya gerek kalmadan Azure’a taşıyabilmeniz için bulut tabanlı SMB dosya paylaşımları sunar. Azure File Storage sayesinde, Azure Virtual Machines veya Cloud Services’da çalışan uygulamalar, bir masaüstü uygulamasının tipik bir SMB paylaşımına bağlandığı şekilde buluta bir dosya paylaşımı bağlayabilir. Ardından herhangi sayıda uygulama bileşeni eş zamanlı olarak File Storage paylaşımını bağlayıp buna erişim sağlayabilir.

File Storage paylaşımı standart SMB dosya paylaşımı olduğu için Azure’da çalışan uygulamalar dosya sisteminin G/Ç API’leri üzerinden paylaşımdaki veriye erişebilir. Böylece geliştiriciler mevcut uygulamalarını taşımak üzere kullandıkları kodlar ve yeteneklerden yararlanabilir. BT Uzmanları Azure uygulamalarını yönetmenin bir parçası olarak File Storage paylaşımlarını oluşturmak, bunları bağlamak ve yönetmek için PowerShell.cmdlet’leri kullanabilir.

File Storage diğer Azure Storage hizmetlerinde olduğu gibi paylaşımdaki verilere erişmek için bir REST API gösterir. Şirket içi uygulamalar dosya paylaşımındaki verilere erişmek için File Storage REST API’sini arayabilir. Bu şekilde bir kuruluş bazı eski uygulamalarını Azure’a taşımayı, diğerleriniyse kendi kurumlarından çalıştırmayı seçebilir. Dosya paylaşımının yalnızca Azure’da çalıştırılan uygulamalar için geçerli olduğunu unutmayın; şirket içi uygulamalar yalnızca REST API üzerinden dosya paylaşımına erişebilir.

Dağıtılan uygulamalar ayrıca kullanışlı uygulama verilerini depolamak ve paylaşmak, araç geliştirmek ve test etmek için File Storage kullanabilir. Örneğin bir uygulama File Storage paylaşımındaki günlükler, ölçümler ve kilitlenme bilgi dökümleri gibi yapılandırma dosyaları ve tanılama verilerini depolayabilir. Böylece söz konusu veriler birden çok sanal makine veya rol tarafından erişilebilir. Geliştiriciler ve yöneticiler, her sanal makine veya rol örneğine yükleme yapmak yerine tüm bileşenler tarafından kullanılabilen ve bir uygulamanın oluşturulması veya yönetilmesi için gerekli yardımcı programları File Storage’a depolayabilir.

## Blob, Tablo, Kuyruk ve Dosya Kaynaklarına Erişim

Varsayılan olarak, yalnızca depolama hesabı sahibi depolama hesabındaki kaynaklara erişebilir. Verilerinizin güvenliği için hesabınızdaki kaynaklara yönelik her isteğin kimliğinin doğrulanması gerekir. Kimlik doğrulama bir Paylaşılan Anahtar modelini kullanır. Bloblar anonim kimlik doğrulamayı destekleyecek şekilde de yapılandırılabilir.

Depolama hesabınız oluşturulduğunda, kimlik doğrulama için kullanılmak üzere hesabınıza iki özel erişim tuşu atanır. İki anahtarın kullanılması, genel güvenlik anahtar yönetimi uygulaması olarak anahtarları düzenli şekilde yenilediğinizde uygulamanızın kullanılabilir durumda kalmasını sağlar.

Kullanıcıların depolama kaynaklarınıza denetimli bir şekilde erişmesini istiyorsanız paylaşılan erişim imzası oluşturabilirsiniz. Paylaşılan erişim imzası (SAS), bir depolama kaynağına yetkilendirilmiş erişim sağlayan URL’ye eklenebilen bir belirteçtir. Bu belirtece sahip olan herkes, belirtecin geçerli olduğu süre boyunca söz konusu izinlerin belirttiği kaynaklara erişebilir. Azure Storage, 2015-04-05 sürümüyle birlikte hizmet SAS ve hesap SAS olmak üzere iki tür paylaşılan erişim imzası destekler.

Hizmet SAS; Blob, Kuyruk, Tablo veya Dosya hizmeti olmak üzere yalnızca bir depolama hizmetindeki kaynağa erişim atar.

Hesap SAS ise bir veya daha fazla depolama hizmetindeki kaynaklara erişim atar. Hizmet SAS ile kullanılamayan hizmet düzeyinde işlemlerine erişim için yetkilendirme yapabilirsiniz. Bununla birlikte hizmet SAS ile izin verilmeyen blob kapsayıcılar, tablolar kuyruklar ve dosya paylaşımları üzerinde okuma, yazma ve silme işlemleri için yetkilendirme yapabilirsiniz.

Son olarak bir kapsayıcı ve bloblarını veya belirli bir blobu genel erişime açabilirsiniz. Bir kapsayıcı veya bir blobun genel erişime açıldığını belirttiğinizde herkes anonim olarak okuyabilir, herhangi bir kimlik doğrulama gerekli değildir.  Genel kapsayıcılar ve bloblar web sitelerinde barındırılan ortam ve belgeler gibi kaynakların sunulması için kullanışlı bir seçenektir.  Global çapta kullanıcılar için ağ gecikmesini azaltmak üzere Azure CDN ile web siteleri tarafından kullanılan blob verilerini önbelleğe taşıyabilirsiniz.

Paylaşılan erişim imzaları ile ilgili daha fazla bilgi edinmek için bkz. [Paylaşılan Erişim İmzaları (SAS) kullanma](storage-dotnet-shared-access-signature-part-1.md). Depolama hesabınıza güvenli erişim ile ilgili daha fazla bilgi için bkz. [Kapsayıcılar ve bloblara anonim okuma erişimini yönetme](storage-manage-access-to-resources.md) ve [Azure Storage Hizmetleri için Kimlik Doğrulama](https://msdn.microsoft.com/library/azure/dd179428.aspx) 

## Dayanıklılık ve Yüksek Seviyede Kullanılabilirlik için Çoğaltma

Microsoft Azure Storage hesabınızdaki veriler, geçici donanım arızalarında bile [Depolama için SLA](https://azure.microsoft.com/support/legal/sla/storage/)’nın sağlanması için dayanıklılık ve yüksek seviyede kullanılabilirlik sağlamak üzere her zaman çoğaltılır.

Bir bölgede hangi hizmetin sağlandığına dair daha fazla bilgi için bkz. [Azure Bölgeleri](https://azure.microsoft.com/regions/#services).

Bir depolama hesabı oluşturduğunuzda, aşağıdaki çoğaltma seçeneklerinden birini seçmeniz gerekir:  

- **Yerel olarak yedekli depolama (LRS).** Yerel olarak yedekli depolama verilerinizin üç kopyasını tutar. LRS, tek bir bölgedeki tek bir tesis içinde üç kez çoğaltılır. LRS normal donanım arızalarına karşı verilerinizi korur ancak tek bir tesisin arızalanmasına karşı koruyamaz.  

    LRS indirimli fiyatla sunulur. En üst düzeyde dayanıklılık için aşağıda açıklanan coğrafi olarak yedekli depolamayı kullanmanızı öneririz.


- **Bölgesel olarak yedekli depolama (ZRS).** Bölgesel olarak yedekli depolama verilerinizin üç kopyasını tutar. ZRS bir veya iki bölgede, iki veya üç tesis üzerinde üç kez çoğaltılır ve böylece LRS’den daha fazla dayanıklılık sunar.. ZRS, verilerinizin tek bir bölge içinde dayanıklı olmasını sağlar.  

    ZRS, LRS'ye daha yüksek düzeyde dayanıklılık sağlar; Ancak, en üst düzeyde dayanıklılık için aşağıda açıklanan coğrafi olarak yedekli depolamayı kullanmanızı öneririz.  

    > [AZURE.NOTE] ZRS şu an yalnızca blok bloblar için kullanılabilir ve yalnızca 2014 02 14 ve daha yeni sürümleri destekler.
    >
    > Depolama hesabınızı oluşturup ZRS’yi seçtiğinizde farklı bir tür çoğaltma seçeneği kullanmak üzere dönüştüremezsiniz; tersi durumda da aynısı söz konusudur.

- **Coğrafi olarak yedekli depolama (GRS)**. GRS verilerinizin altı kopyasını tutar. GRS ile verileriniz birincil bölge içinde üç kez çoğaltılır ve ayrıca birincil bölgeden yüzlerce kilometre ötedeki ikincil bir bölgede üç kez çoğaltılarak en üst seviyede dayanıklılık sağlanır. Birincil bölgede bir arıza olması durumunda Azure Storage ikincil bölgeye yük devredecektir. GRS, verilerinizin iki ayrı bölge içinde dayanıklı olmasını sağlar.

    Bölgeye göre birincil ve ikincil eşleştirmeler hakkında daha fazla bilgi için bkz. [Azure Bölgeleri](https://azure.microsoft.com/regions/).

- **Coğrafi olarak yedekli depolamaya okuma erişimi (RA-GRS)**. Coğrafi olarak yedekli depolamaya okuma erişimi, depolama hesabınızı oluşturduğunuzda hesabınız için varsayılan olarak etkinleştirilir. Coğrafi olarak yedekli depolamaya okuma erişimi verilerinizi ikincil bir coğrafi konumda çoğaltır, bununla birlikte ikincil konumdaki verilerinize okuma erişimi sağlar. Konumlardan birinin kullanılamaz durumda olması halinde Coğrafi olarak yedekli depolama okuma erişimi verilerinize birincil veya ikincil konumdan erişmenizi sağlar.

    > [AZURE.IMPORTANT] Hesabınızı oluştururken ZRS seçmediyseniz, depolama hesabınız oluşturulduktan sonra verilerinizin çoğaltılma yöntemini değiştirebilirsiniz. Buna karşın LRS’den GRS’ye veya RA-GRS’ye geçiş yaparsanız tek seferlik veri aktarımı ücreti ödemeniz gerekebileceğini unutmayın.

Depolama çoğaltma seçenekleri ile ilgili ayrıntılar için bkz. [Azure Storage çoğaltma](storage-redundancy.md).

Depolama hesabı çoğaltma ile ilgili fiyat bilgileri için bkz. [Azure Storage Fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).

Azure Storage ile dayanıklılık konusunda mimari ayrıntılar için bkz. [SOSP Belgesi - Azure Storage: Güçlü Tutarlılıkla Yüksek Seviyede Kullanılabilen Bulut Depolama Hizmeti](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).


## Azure Storage’da Veri Alışverişi

Depolama hesabınızda veya depolama hesaplarınız arasında blob, dosya ve tablo verilerinizi kopyalamak için AzCopy komut satırı yardımcı programını kullanabilirsiniz. Daha fazla bilgi için bkz. [AzCopy Komut Satırı Yardımcı Programı ile Veri Aktarma](storage-use-azcopy.md).

AzCopy şu anda önizlemede yer alan [Azure Veri Taşıma Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/) üzerine kurulmuştur.

Azure İçeri/Dışarı Aktarma hizmeti, Azure veri merkezine posta ile gönderilen sabit sürücü üzerinden depolama hesabınızla blob verisi alışverişi için bir yöntem sunar. İçeri/Dışarı Aktarma hizmeti hakkında daha fazla bilgi için bkz. [Blob Storage Veri Aktarma için Microsoft Azure İçeri/Dışarı Aktarma Hizmeti Kullanma](storage-import-export-service.md).

## Fiyatlandırma

[AZURE.INCLUDE [storage-account-billing-include](../../includes/storage-account-billing-include.md)]

## Depolama API'leri, Kitaplıklar ve Araçlar

Azure Storage kaynakları HTTP/HTTPS isteği yapabilen her dil ile erişilebilir. Ayrıca, Azure Storage birkaç popüler dilde programlama kitaplıkları sunar. Bu kitaplıklar zaman uyumlu ve zaman uyumsuz çağrılar, işlemleri gruplama, özel durum yönetimi, otomatik yeniden denemeler, işlemsel davranışlar vb. ayrıntıları ele alarak Azure Storage ile çalışmanın çoğu yönünü basitleştirir. Kitaplıklar şu anda aşağıdaki diller için mevcuttur ve yeni kitaplıklar geliştirme aşamasındadır:

### Azure Storage Veri Hizmetleri

- [Depolama Hizmetleri REST API'si](http://msdn.microsoft.com/library/azure/dd179355.aspx)
- [.NET, Windows Phone, ve Windows Runtime için Depolama İstemci Kitaplığı](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [C++ için Depolama İstemci Kitaplığı](https://github.com/Azure/azure-storage-cpp)
- [Java/Android için Depolama İstemci Kitaplığı](/develop/java/)
- [Node.js için Depolama İstemci Kitaplığı](http://dl.windowsazure.com/nodestoragedocs/index.html)
- [PHP için Depolama İstemci Kitaplığı](/develop/php/)
- [Ruby için Depolama İstemci Kitaplığı](/develop/ruby/)
- [Python için Depolama İstemci Kitaplığı](/develop/python/)
- [PowerShell 1.0 için Depolama Cmdlet'leri](https://msdn.microsoft.com/library/azure/mt269418.aspx)

### Azure Storage Yönetimi Hizmetleri

- [Depolama Kaynak Sağlayıcısı REST API Başvurusu](https://msdn.microsoft.com/library/azure/mt163683.aspx)
- [.NET için Depolama Kaynak Sağlayıcısı İstemci Kitaplığı](https://msdn.microsoft.com/library/azure/mt131037.aspx)
- [PowerShell 1.0 için Depolama Kaynak Sağlayıcısı Cmdlet'leri](https://msdn.microsoft.com/library/azure/mt607151.aspx)
- [Depolama Hizmet Yönetimi REST API'si (Klasik)](https://msdn.microsoft.com/library/azure/ee460790.aspx)

### Azure Storage Veri Taşıma Hizmetleri

- [Depolama İçeri/Dışarı Aktarma Hizmeti REST API’si](https://msdn.microsoft.com/library/azure/dn529096.aspx)
- [.NET için Depolama Veri Taşıma İstemci Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/)

### Araçlar ve Yardımcı Programlar

- [Azure Storage Gezgini](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)
- [Azure Storage İstemci Araçları](storage-explorers.md)
- [Azure SDK’leri ve Araçları](https://azure.microsoft.com/tools/)
- [Azure Storage Öykünücüsü](http://www.microsoft.com/download/details.aspx?id=43709)
- [Azure PowerShell](../powershell-install-configure.md)
- [AzCopy Komut Satırı Yardımcı Programı](http://aka.ms/downloadazcopy)

## Sonraki Adımlar

Azure Storage hakkında daha fazla bilgi için şu kaynakları araştırın:

### Belgeler

- [Azure Storage Belgeleri](https://azure.microsoft.com/documentation/services/storage/)

### Yöneticiler için

- [Azure Storage ile Azure PowerShell’i kullanma](storage-powershell-guide-full.md)
- [Azure Storage ile Azure CLI kullanma](storage-azure-cli.md)

### .NET Geliştiricileri için

- [.NET kullanarak Azure Blob Storage’ı kullanmaya başlayın](storage-dotnet-how-to-use-blobs.md)
- [.NET kullanarak Azure Table Storage’ı kullanmaya başlayın](storage-dotnet-how-to-use-tables.md)
- [.NET kullanarak Azure Kuyruk Depolamaya başlayın](storage-dotnet-how-to-use-queues.md)
- [.NET kullanarak Azure File Storage’ı kullanmaya başlayın](storage-dotnet-how-to-use-files.md)

### Java/Android Geliştiricileri için

- [Java’dan Blob Storage kullanma](storage-java-how-to-use-blob-storage.md)
- [Java’dan Table Storage’ı kullanma](storage-java-how-to-use-table-storage.md)
- [Java’dan Kuyruk depolama kullanma](storage-java-how-to-use-queue-storage.md)
- [Java’dan File Storage kullanma](storage-java-how-to-use-file-storage.md)

### Node.js Geliştiricileri için

- [Node.js’den Blob Storage kullanma](storage-nodejs-how-to-use-blob-storage.md)
- [Node.js’den Table Storage’ı kullanma](storage-nodejs-how-to-use-table-storage.md)
- [Node.js’den Kuyruk depolama kullanma](storage-nodejs-how-to-use-queues.md)

### PHP Geliştiricileri için

- [PHP’den Blob Storage kullanma](storage-php-how-to-use-blobs.md)
- [PHP’den Table Storage’ı kullanma](storage-php-how-to-use-table-storage.md)
- [PHP’den Kuyruk depolama kullanma](storage-php-how-to-use-queues.md)

### Ruby Geliştiricileri için

- [Ruby’den Blob Storage kullanma](storage-ruby-how-to-use-blob-storage.md)
- [Ruby’den Table Storage’ı kullanma](storage-ruby-how-to-use-table-storage.md)
- [Ruby’den Kuyruk depolama kullanma](storage-ruby-how-to-use-queue-storage.md)

### Python Geliştiricileri için

- [Python’dan Blob Storage kullanma](storage-python-how-to-use-blob-storage.md)
- [Python’dan Table Storage’ı kullanma](storage-python-how-to-use-table-storage.md)
- [Python’dan Kuyruk depolama kullanma](storage-python-how-to-use-queue-storage.md)
- [Python’dan File Storage kullanma](storage-python-how-to-use-file-storage.md)



<!--HONumber=Sep16_HO3-->


