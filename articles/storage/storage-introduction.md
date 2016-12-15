---
title: "Depolamaya Giriş | Microsoft Belgeleri"
description: "Microsoft’un buluttaki çevrimiçi veri depolama alanı Azure Storage’a göz atalım. Uygulamalarınızda en uygun bulut depolama çözümünü nasıl kullanabileceğinizi öğrenin."
services: storage
documentationcenter: 
author: tamram
manager: carmonm
editor: tysonn
ms.assetid: a4a1bc58-ea14-4bf5-b040-f85114edc1f1
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/17/2016
ms.author: tamram
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: ac0044da9cf804dabd9d71e3380782120728a55a


---
# <a name="introduction-to-microsoft-azure-storage"></a>Microsoft Azure Storage’a Giriş
## <a name="overview"></a>Genel Bakış
Azure Storage, müşterilerin ihtiyaçlarını karşılamak üzere sağlamlık, kullanılabilirlik ve ölçeklenebilirliğe dayanan modern uygulamalara yönelik bulut depolama çözümüdür. Bu makaleyi okuyan geliştiriciler, BT Uzmanları ve kurumsal karar alıcılar şu konularda bilgi edinebilir:

* Azure Storage’ın içeriği ve bulut, mobil, sunucu ve masaüstü uygulamalarınızda Azure Storage’dan nasıl faydalanılabileceği
* Azure Storage hizmetleri ile depolayabileceğiniz veriler: blob (nesne) verileri, NoSQL tablo verileri, kuyruk mesajları ve dosya paylaşımları
* Azure Storage’da verilerinize erişimin nasıl yönetildiği
* Azure Storage verilerinizin yedekleme ve çoğaltma ile nasıl dayanıklı hale getirildiği
* İlk Azure Storage uygulamanızı oluşturmak için yapılması gerekenler

Azure Storage’ı sorunsuz şekilde kullanmaya başlamak için bkz. [Beş dakikada Azure Storage’a başlayın](storage-getting-started-guide.md).

Azure Storage ile çalışma konusunda gerekli araçlar, kitaplıklar ve diğer kaynaklar için aşağıda yer alan [Sonraki Adımlar](#next-steps)’a göz atın.

## <a name="what-is-azure-storage"></a>Azure Storage nedir?
Bulut bilgi işlem; veriler için ölçeklenebilir, sağlam ve yüksek seviyede kullanılabilir depolama alanına ihtiyaç duyan uygulamalara yönelik yeni senaryolara olanak tanıyor. Microsoft’un Azure Storage’ı geliştirmesinin nedeni budur. Azure Storage, geliştiricilerin yeni senaryoları desteklemek üzere geniş ölçekli uygulamalar oluşturmalarını mümkün kılmanın yanında, sağlamlığının daha güçlü bir kanıtı olarak Azure Virtual Machines için depolamaya temel oluşturuyor.

Azure Storage çok büyük boyutta ölçeklenebilir. Bu sayede bilimsel, finansal analiz ve medya uygulamalarının ihtiyaç duyduğu büyük veri senaryolarını desteklemek üzere yüzlerce terabayt veriyi depolayıp işleyebilirsiniz. Veya küçük ölçekli bir işletmenin web sitesi için gerekli olan küçük miktarda veriyi depolayabilirsiniz. İhtiyaçlarınız ne olursa olsun yalnızca depoladığınız veri için ücret ödersiniz. Şu anda Azure Storage onlarca trilyon benzersiz müşteri nesnesi depolamaktadır ve saniyede ortalama olarak milyonlarca talebi ele alır.

Azure Storage esnektir, böylece geniş global kullanıcılara yönelik uygulamalar tasarlayabilir ve hem depolanan veri miktarı hem de bu verilerle ilgili gerçekleştirilen talep sayısı açısından söz konusu uygulamaları gerektiği şekilde ölçekleyebilirsiniz. Yalnızca kullandıklarınız için ve kullandığınız zaman ücret ödersiniz.

Azure Storage trafiğe bağlı olarak verilerinize otomatik olarak yük dengelemesi yapan bir otomatik bölümleme sistemi kullanır. Bu, uygulamanıza yönelik talepler arttıkça Azure Storage’ın bunları karşılamak üzere ilgili kaynakları otomatik olarak ayıracağı anlamına gelir.

Azure Storage hizmetine bulutta, masaüstünde veya şirket içi bir sunucuda, mobil cihazlarda veya tablette çalışan her tür uygulama aracılığıyla dünyanın her yerinden erişilebilir. Azure Storage’ı, uygulamanın bir veri alt kümesini cihazda depoladığı ve buluttaki eksiksiz veri kümesi ile eşitlediği mobil senaryolarda kullanabilirsiniz.

Azure Storage, geliştirme sürecini kolaylaştırmak için farklı işletim sistemleri (Windows ve Linux dahil) kullanan istemcileri ve farklı programlama dillerini (.NET, Java, Node.js, Python, Ruby, PHP, C++ ve mobil programlama dilleri) destekler. Azure Storage HTTP/HTTPS üzerinden veri gönderme ve alma özelliğine sahip herhangi bir istemcinin kullanabileceği basit REST API’leri aracılığıyla veri kaynaklarını da gösterir.

Azure Premium Storage, Azure Virtual Machines’de çalışan G/Ç açısından yoğun iş yükleri için yüksek performanslı ve düşük gecikmeli disk desteği sağlar. Azure Premium Storage ile birden çok kalıcı veri diskini sanal bir makineye bağlayabilir ve performans gereksinimlerinizi karşılayacak şekilde yapılandırabilirsiniz. Azure Premium Storage’da her veri diski maksimum G/Ç performansı için bir SSD disk ile desteklenir. Ayrıntılar için bkz. [Premium Storage: Azure Virtual Machines İş Yükleri İçin Yüksek Performanslı Depolama](storage-premium-storage.md).

## <a name="introducing-the-azure-storage-services"></a>Azure Storage Hizmetlerine Giriş
Azure Storage şu dört hizmeti sunar: Blob Storage, Table Storage, Kuyruk depolama ve File Storage.

* Blob Storage yapılandırılmamış nesne verilerini depolar. Blob; bir belge, ortam dosyası veya uygulama yükleyici gibi herhangi bir türde metin veya ikili veri olabilir. Blob Storage ayrıca Nesne depolama olarak adlandırılır.
* Table Storage yapılandırılmış veri kümelerini depolar. Table Storage, yüksek miktarda verinin hızla dağıtılmasını ve verilere hızla erişilebilmesini sağlayan NoSQL anahtar özniteliği veri deposudur.
* Kuyruk Depolama bulut hizmetlerinin bileşenleri arasında iş akışı işlemeye ve iletişime yönelik güvenilir mesajlaşma sağlar.
* File Storage standart SMB protokolünü kullanan eski uygulamalar için paylaşılan depolama alanı sağlar. Azure Virtual Machines ve Cloud Services bağlı paylaşımlar üzerinden uygulama bileşenleri arasında dosya verileri paylaşabilir ve şirket içi uygulamalar, REST API Dosya hizmeti üzerinden dosya verilerine erişebilir.

Azure Storage hesabı, Azure Storage’da hizmetlerine erişmenizi sağlayan güvenli bir hesaptır. Depolama hesabınız depolama kaynaklarınız için benzersiz ad alanı sağlar. Aşağıdaki görüntü depolama hesabındaki Azure Storage kaynakları arasındaki ilişkiyi göstermektedir:

![Azure Storage Kaynakları](./media/storage-introduction/storage-concepts.png)

[!INCLUDE [storage-account-types-include](../../includes/storage-account-types-include.md)]

[!INCLUDE [storage-versions-include](../../includes/storage-versions-include.md)]

## <a name="blob-storage"></a>Blob Storage
Blob Storage, bulutta depolanacak yüksek miktarda yapılandırılmamış nesne verisine sahip kullanıcılara uygun maliyetli ve ölçeklenebilir bir çözüm sunar. Blob Storage’ı kullanarak şu içerikleri depolayabilirsiniz:

* Belgeler
* Fotoğraf, video, müzik ve blog gibi sosyal veriler
* Dosyaların, bilgisayarların, veritabanlarının ve cihazların yedekleri
* Web uygulamaları için görüntüler ve metinler
* Bulut uygulamaları için yapılandırma verileri
* Günlükler ve diğer büyük veri kümeleri gibi büyük veriler

Her blob bir kapsayıcı halinde düzenlenmiştir. Kapsayıcılar ayrıca nesne gruplarına güvenlik ilkeleri atamaya ilişkin kullanışlı bir yöntem sunar. Bir depolama hesabının içerebileceği kapsayıcı sayısına ilişkin bir sınırlama yoktur. Bir kapsayıcı, depolama hesabının 500 TB kapasite sınırını dolduracak kadar blob içerebilir.  

Blob Storage blok blobları, ekleme blobları ve sayfa blobları (diskler) olmak üzere üç türde blob sunar.

* Blok blobları bulut nesnelerinin akış ve depolanması için en iyi duruma getirilmiştir ve belge, ortam dosyaları ve yedekler vb. öğelerin depolanması için uygun bir seçenektir.
* Ekleme blobları blok bloblarına benzer ancak ekleme işlemleri için en iyi duruma getirilmiştir. Bir ekleme blobu yalnızca sonuna yeni bir blok eklenerek güncelleştirilebilir. Ekleme blobları, yeni verilerin yalnızca blobun sonuna yazılması gereken günlüğe kaydetme gibi senaryolar için iyi bir seçenektir.
* Sayfa blobları IaaS disklerini temsil etmek ve rastgele yazmaları desteklemek üzere iyileştirilmiştir ve boyutları 1 TB’ye ulaşabilir. IaaS diskine bağlı bir Azure Virtual Machine ağı, sayfa blobu olarak kaydedilen bir VHD’dir.

Ağ kısıtlamalarının kablo üzerinden Blob Storage’a veri yükleme veya indirme yapmayı kullanışsız hale getirdiği çok büyük veri kümelerinde verileri doğrudan veri merkezinden içeri veya dışarı aktarmak için Microsoft’a bir sabit sürücü gönderebilirsiniz. Bkz: [Blob Storage’a Veri Aktarmak için Microsoft Azure İçeri/Dışarı Aktarma Hizmeti Kullanma](storage-import-export-service.md).

## <a name="table-storage"></a>Table Storage
Modern uygulamalar genellikle eski nesil yazılımların gerektirdiğinden daha fazla ölçeklenebilirlik ve esneklik özelliklerine sahip veri depoları gerektirir. Table Storage yüksek seviyede kullanılabilir ve ölçeklenebilir depolama sunar, böylece uygulamanız kullanıcı taleplerini karşılayacak şekilde otomatik olarak ölçeklendirilir. Table Storage Microsoft’un NoSQL anahtar/öznitelik deposudur ve geleneksel ilişkisel veritabanlarından farklı olarak şemasız bir tasarıma sahiptir. Şemasız veri deposu sayesinde, uygulamanızın ihtiyaçları geliştikçe verilerinizi uyarlamak da kolaylaşır. Table Storage’ın kullanımı son derece kolaydır, böylece geliştiriciler uygulamalarını hızla geliştirebilir. Her türlü uygulama için verilere erişim hızlı ve uygun maliyetlidir.  Table Storage, benzer hacimdeki veriler için geleneksel SQL’e oranla çok daha düşük maliyetlidir.

Table Storage bir anahtar öznitelik deposudur; bu, bir tablodaki her değerin türü belirtilmiş bir özellik adıyla depolandığı anlamına gelir. Özellik adı filtreleme ve seçim kriterlerinin belirlenmesi için kullanılabilir. Özellik ve değerlerinin toplamı bir varlığı oluşturur. Table Storage şemasız olduğu için aynı tablodaki iki varlık farklı özellik koleksiyonları içerebilir ve bu özellikler farklı türde olabilir.

Web uygulamaları için kullanıcı verileri, adres defterleri, cihaz bilgileri ve hizmetiniz için gerekli olan tüm diğer meta veri türleri gibi esnek veri kümelerini depolamak üzere Table Storage’ı kullanabilirsiniz.  Bir tabloda istediğiniz kadar varlık depolayabilirsiniz ve bir depolama hesabı kapasite limitini dolduracak kadar tablo içerebilir.

Geliştiriciler Bloblar ve Kuyruklarda olduğu gibi standart REST protokollerini kullanarak Table Storage’ı da yönetebilir ve depolamaya erişebilir. Ancak Table Storage gelişmiş sorgu özelliklerini basitleştiren ve hem JSON hem de AtomPub (XML tabanlı) biçimlerine olanak tanıyan OData protokolünün bir alt kümesini de destekler.

Bugünün İnternet tabanlı uygulamaları için Table Storage gibi NoSQL veri tabanları geleneksel veri tabanlarına göre popüler bir alternatif oluşturur.

## <a name="queue-storage"></a>Kuyruk Depolama
Ölçeklendirmek üzere uygulama tasarlarken, uygulama bileşenleri birbirinden bağımsız şekilde ölçeklenebilmek için genellikle birbirinden ayrılır. Kuyruk depolama bulutta, masaüstünde, şirket içi sunucuda veya mobil bir cihazda çalışan uygulama bileşenleri arasındaki zaman uyumsuz iletişim için güvenilir bir mesajlaşma çözümü sunar. Kuyruk depolama zaman uyumsuz görevlerin yönetilmesini ve süreç iş akışlarının oluşturulmasını da destekler.

Bir depolama hesabı herhangi sayıda kuyruk içerebilir. Bir kuyruk, depolama hesabının kapasite sınırını dolduracak kadar ileti içerebilir. Tek bir ileti boyut olarak en fazla 64 KB olabilir.

## <a name="file-storage"></a>File Storage
Azure File Storage, dosya paylaşımlarına bağlı olan eski uygulamaları maliyetli yeniden yazdırmaya gerek kalmadan Azure’a taşıyabilmeniz için bulut tabanlı SMB dosya paylaşımları sunar. Azure File Storage sayesinde, Azure Virtual Machines veya Cloud Services’da çalışan uygulamalar, bir masaüstü uygulamasının tipik bir SMB paylaşımına bağlandığı şekilde buluta bir dosya paylaşımı bağlayabilir. Ardından herhangi sayıda uygulama bileşeni eş zamanlı olarak File Storage paylaşımını bağlayıp buna erişim sağlayabilir.

File Storage paylaşımı standart SMB dosya paylaşımı olduğu için Azure’da çalışan uygulamalar dosya sisteminin G/Ç API’leri üzerinden paylaşımdaki veriye erişebilir. Böylece geliştiriciler mevcut uygulamalarını taşımak üzere kullandıkları kodlar ve yeteneklerden yararlanabilir. BT Uzmanları Azure uygulamalarını yönetmenin bir parçası olarak File Storage paylaşımlarını oluşturmak, bunları bağlamak ve yönetmek için PowerShell.cmdlet’leri kullanabilir.

File Storage diğer Azure Storage hizmetlerinde olduğu gibi paylaşımdaki verilere erişmek için bir REST API gösterir. Şirket içi uygulamalar dosya paylaşımındaki verilere erişmek için File Storage REST API’sini arayabilir. Bu şekilde bir kuruluş bazı eski uygulamalarını Azure’a taşımayı, diğerleriniyse kendi kurumlarından çalıştırmayı seçebilir. Dosya paylaşımının yalnızca Azure’da çalıştırılan uygulamalar için geçerli olduğunu unutmayın; şirket içi uygulamalar yalnızca REST API üzerinden dosya paylaşımına erişebilir.

Dağıtılan uygulamalar ayrıca kullanışlı uygulama verilerini depolamak ve paylaşmak, araç geliştirmek ve test etmek için File Storage kullanabilir. Örneğin bir uygulama File Storage paylaşımındaki günlükler, ölçümler ve kilitlenme bilgi dökümleri gibi yapılandırma dosyaları ve tanılama verilerini depolayabilir. Böylece söz konusu veriler birden çok sanal makine veya rol tarafından erişilebilir. Geliştiriciler ve yöneticiler, her sanal makine veya rol örneğine yükleme yapmak yerine tüm bileşenler tarafından kullanılabilen ve bir uygulamanın oluşturulması veya yönetilmesi için gerekli yardımcı programları File Storage’a depolayabilir.

## <a name="access-to-blob-table-queue-and-file-resources"></a>Blob, Tablo, Kuyruk ve Dosya Kaynaklarına Erişim
Varsayılan olarak, yalnızca depolama hesabı sahibi depolama hesabındaki kaynaklara erişebilir. Verilerinizin güvenliği için hesabınızdaki kaynaklara yönelik her isteğin kimliğinin doğrulanması gerekir. Kimlik doğrulama bir Paylaşılan Anahtar modelini kullanır. Bloblar anonim kimlik doğrulamayı destekleyecek şekilde de yapılandırılabilir.

Depolama hesabınız oluşturulduğunda, kimlik doğrulama için kullanılmak üzere hesabınıza iki özel erişim tuşu atanır. İki anahtarın kullanılması, genel güvenlik anahtar yönetimi uygulaması olarak anahtarları düzenli şekilde yenilediğinizde uygulamanızın kullanılabilir durumda kalmasını sağlar.

Kullanıcıların depolama kaynaklarınıza denetimli bir şekilde erişmesini istiyorsanız paylaşılan erişim imzası oluşturabilirsiniz. Paylaşılan erişim imzası (SAS), bir depolama kaynağına yetkilendirilmiş erişim sağlayan URL’ye eklenebilen bir belirteçtir. Bu belirtece sahip olan herkes, belirtecin geçerli olduğu süre boyunca söz konusu izinlerin belirttiği kaynaklara erişebilir. Azure Storage, 2015-04-05 sürümüyle birlikte hizmet SAS ve hesap SAS olmak üzere iki tür paylaşılan erişim imzası destekler.

Hizmet SAS; Blob, Kuyruk, Tablo veya Dosya hizmeti olmak üzere yalnızca bir depolama hizmetindeki kaynağa erişim atar.

Hesap SAS ise bir veya daha fazla depolama hizmetindeki kaynaklara erişim atar. Hizmet SAS ile kullanılamayan hizmet düzeyinde işlemlerine erişim için yetkilendirme yapabilirsiniz. Bununla birlikte hizmet SAS ile izin verilmeyen blob kapsayıcılar, tablolar kuyruklar ve dosya paylaşımları üzerinde okuma, yazma ve silme işlemleri için yetkilendirme yapabilirsiniz.

Son olarak bir kapsayıcı ve bloblarını veya belirli bir blobu genel erişime açabilirsiniz. Bir kapsayıcı veya bir blobun genel erişime açıldığını belirttiğinizde herkes anonim olarak okuyabilir, herhangi bir kimlik doğrulama gerekli değildir.  Genel kapsayıcılar ve bloblar web sitelerinde barındırılan ortam ve belgeler gibi kaynakların sunulması için kullanışlı bir seçenektir.  Global çapta kullanıcılar için ağ gecikmesini azaltmak üzere Azure CDN ile web siteleri tarafından kullanılan blob verilerini önbelleğe taşıyabilirsiniz.

Paylaşılan erişim imzaları ile ilgili daha fazla bilgi edinmek için bkz. [Paylaşılan Erişim İmzaları (SAS) kullanma](storage-dotnet-shared-access-signature-part-1.md). Depolama hesabınıza güvenli erişim ile ilgili daha fazla bilgi için bkz. [Kapsayıcılar ve bloblara anonim okuma erişimini yönetme](storage-manage-access-to-resources.md) ve [Azure Storage Hizmetleri için Kimlik Doğrulama](https://msdn.microsoft.com/library/azure/dd179428.aspx) 

## <a name="replication-for-durability-and-high-availability"></a>Dayanıklılık ve Yüksek Seviyede Kullanılabilirlik için Çoğaltma
Microsoft Azure Depolama hesabınızdaki veriler, dayanıklılık ve yüksek kullanılabilirlik sağlamak için her zaman çoğaltılır. Çoğaltma işlemi, belirlediğiniz çoğaltma seçeneğine göre verilerinizi aynı veri merkezine veya ikinci bir veri merkezine kopyalar. Çoğaltma işlemi, geçici donanım hataları söz konusu olduğunda uygulamanızın çalışma süresini ve verilerinizi korur. Verilerinizin ikinci bir veri merkezine çoğaltılması da birincil konumda gerçekleşen yıkıcı bir hataya karşı verilerinizi korur.

Çoğaltma işlemi, hata durumunda bile depolama hesabınızın [Depolama için Hizmet Düzeyi Sözleşmesi'ne (SLA)](https://azure.microsoft.com/support/legal/sla/storage/) uymasını sağlar. Azure Depolama'nın dayanıklılık ve kullanılabilirlikle ilgili sağladığı garantiler hakkında bilgi edinmek için SLA'ya göz atın. 

Bir depolama hesabı oluşturduğunuzda şu çoğaltma seçeneklerinden birini seçebilirsiniz:  

* **Yerel olarak yedekli depolama (LRS).** Yerel olarak yedekli depolama verilerinizin üç kopyasını tutar. LRS, tek bir bölgedeki tek bir veri merkezinde üç kez çoğaltılır. LRS, normal donanım arızalarına karşı verilerinizi korur ancak tek bir veri merkezinin arızalanmasına karşı korumaz.  
  
    LRS indirimli fiyatla sunulur. En üst düzeyde dayanıklılık için aşağıda açıklanan coğrafi olarak yedekli depolamayı kullanmanızı öneririz.
* **Bölgesel olarak yedekli depolama (ZRS).** Bölgesel olarak yedekli depolama verilerinizin üç kopyasını tutar. ZRS bir veya iki bölgede, iki veya üç tesis üzerinde üç kez çoğaltılır ve böylece LRS’den daha fazla dayanıklılık sunar.. ZRS, verilerinizin tek bir bölge içinde dayanıklı olmasını sağlar.  
  
    ZRS, LRS'ye daha yüksek düzeyde dayanıklılık sağlar; Ancak, en üst düzeyde dayanıklılık için aşağıda açıklanan coğrafi olarak yedekli depolamayı kullanmanızı öneririz.  
  
  > [!NOTE]
  > ZRS şu an yalnızca blok bloblar için kullanılabilir ve yalnızca 2014 02 14 ve daha yeni sürümleri destekler.
  > 
  > Depolama hesabınızı oluşturup ZRS’yi seçtiğinizde farklı bir tür çoğaltma seçeneği kullanmak üzere dönüştüremezsiniz; tersi durumda da aynısı söz konusudur.
  > 
  > 
* **Coğrafi olarak yedekli depolama (GRS)**. GRS verilerinizin altı kopyasını tutar. GRS ile verileriniz birincil bölge içinde üç kez çoğaltılır ve ayrıca birincil bölgeden yüzlerce kilometre ötedeki ikincil bir bölgede üç kez çoğaltılarak en üst seviyede dayanıklılık sağlanır. Birincil bölgede bir arıza olması durumunda Azure Storage ikincil bölgeye yük devredecektir. GRS, verilerinizin iki ayrı bölge içinde dayanıklı olmasını sağlar.
  
    Bölgeye göre birincil ve ikincil eşleştirmeler hakkında daha fazla bilgi için bkz. [Azure Bölgeleri](https://azure.microsoft.com/regions/).
* **Okuma erişimli coğrafi olarak yedekli depolama (RA-GRS)**. Okuma erişimli coğrafi olarak yedekli depolama, verilerinizi ikinci bir coğrafi konumda çoğaltır ve ikincil konumdaki verilerinize yönelik okuma erişimi sağlar. Konumlardan birinin kullanılamaz durumda olması halinde Coğrafi olarak yedekli depolama okuma erişimi verilerinize birincil veya ikincil konumdan erişmenizi sağlar. Depolama hesabınızı oluşturduğunuzda hesabınız için varsayılan seçenek, okuma erişimli coğrafi olarak yedekli depolamadır. 
  
  > [!IMPORTANT]
  > Hesabınızı oluştururken ZRS seçmediyseniz, depolama hesabınız oluşturulduktan sonra verilerinizin çoğaltılma yöntemini değiştirebilirsiniz. Buna karşın LRS’den GRS’ye veya RA-GRS’ye geçiş yaparsanız tek seferlik veri aktarımı ücreti ödemeniz gerekebileceğini unutmayın.
  > 
  > 

Depolama çoğaltma seçenekleri ile ilgili ayrıntılar için bkz. [Azure Storage çoğaltma](storage-redundancy.md).

Depolama hesabı çoğaltma ile ilgili fiyat bilgileri için bkz. [Azure Storage Fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/). Bir bölgede hangi hizmetin sağlandığına dair daha fazla bilgi için bkz. [Azure Bölgeleri](https://azure.microsoft.com/regions/#services).

Azure Storage ile dayanıklılık konusunda mimari ayrıntılar için bkz. [SOSP Belgesi - Azure Storage: Güçlü Tutarlılıkla Yüksek Seviyede Kullanılabilen Bulut Depolama Hizmeti](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).

## <a name="transferring-data-to-and-from-azure-storage"></a>Azure Storage’da Veri Alışverişi
Depolama hesabınızda veya depolama hesaplarınız arasında blob, dosya ve tablo verilerinizi kopyalamak için AzCopy komut satırı yardımcı programını kullanabilirsiniz. Daha fazla bilgi için bkz. [AzCopy Komut Satırı Yardımcı Programı ile Veri Aktarma](storage-use-azcopy.md).

AzCopy şu anda önizlemede yer alan [Azure Veri Taşıma Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/) üzerine kurulmuştur.

Azure İçeri/Dışarı Aktarma hizmeti, Azure veri merkezine posta ile gönderilen sabit sürücü üzerinden depolama hesabınızla blob verisi alışverişi için bir yöntem sunar. İçeri/Dışarı Aktarma hizmeti hakkında daha fazla bilgi için bkz. [Blob Storage Veri Aktarma için Microsoft Azure İçeri/Dışarı Aktarma Hizmeti Kullanma](storage-import-export-service.md).

## <a name="pricing"></a>Fiyatlandırma
[!INCLUDE [storage-account-billing-include](../../includes/storage-account-billing-include.md)]

## <a name="storage-apis-libraries-and-tools"></a>Depolama API'leri, Kitaplıklar ve Araçlar
Azure Storage kaynakları HTTP/HTTPS isteği yapabilen her dil ile erişilebilir. Ayrıca, Azure Storage birkaç popüler dilde programlama kitaplıkları sunar. Bu kitaplıklar zaman uyumlu ve zaman uyumsuz çağrılar, işlemleri gruplama, özel durum yönetimi, otomatik yeniden denemeler, işlemsel davranışlar vb. ayrıntıları ele alarak Azure Storage ile çalışmanın çoğu yönünü basitleştirir. Kitaplıklar şu anda aşağıdaki diller için mevcuttur ve yeni kitaplıklar geliştirme aşamasındadır:

### <a name="azure-storage-data-services"></a>Azure Storage Veri Hizmetleri
* [Depolama Hizmetleri REST API'si](http://msdn.microsoft.com/library/azure/dd179355.aspx)
* [.NET, Windows Phone, ve Windows Çalışma Zamanı için Depolama İstemcisi Kitaplığı](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [C++ için Depolama İstemcisi Kitaplığı](https://github.com/Azure/azure-storage-cpp)
* [Java/Android için Depolama İstemcisi Kitaplığı](/develop/java/)
* [Node.js için Depolama İstemcisi Kitaplığı](http://dl.windowsazure.com/nodestoragedocs/index.html)
* [PHP için Depolama İstemcisi Kitaplığı](/develop/php/)
* [Ruby için Depolama İstemcisi Kitaplığı](/develop/ruby/)
* [Python için Depolama İstemcisi Kitaplığı](/develop/python/)
* [PowerShell 1.0 için Depolama Cmdlet'leri](https://msdn.microsoft.com/library/azure/mt269418.aspx)

### <a name="azure-storage-management-services"></a>Azure Storage Yönetimi Hizmetleri
* [Depolama Kaynak Sağlayıcısı REST API Başvurusu](https://msdn.microsoft.com/library/azure/mt163683.aspx)
* [.NET için Depolama Kaynak Sağlayıcısı İstemci Kitaplığı](https://msdn.microsoft.com/library/azure/mt131037.aspx)
* [PowerShell 1.0 için Depolama Kaynak Sağlayıcısı Cmdlet'leri](https://msdn.microsoft.com/library/azure/mt607151.aspx)
* [Depolama Hizmet Yönetimi REST API'si (Klasik)](https://msdn.microsoft.com/library/azure/ee460790.aspx)

### <a name="azure-storage-data-movement-services"></a>Azure Storage Veri Taşıma Hizmetleri
* [Depolama İçeri/Dışarı Aktarma Hizmeti REST API'si](https://msdn.microsoft.com/library/azure/dn529096.aspx)
* [.NET için Depolama Veri Taşıma İstemcisi Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/)

### <a name="tools-and-utilities"></a>Araçlar ve Yardımcı Programlar
* [Azure Depolama Gezgini](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)
* [Azure Depolama İstemcisi Araçları](storage-explorers.md)
* [Azure SDK'ları ve Araçları](https://azure.microsoft.com/tools/)
* [Azure Depolama Öykünücüsü](http://www.microsoft.com/download/details.aspx?id=43709)
* [Azure PowerShell](/powershell/azureps-cmdlets-docs)
* [AzCopy Komut Satırı Yardımcı Programı](http://aka.ms/downloadazcopy)

## <a name="next-steps"></a>Sonraki Adımlar
Azure Storage hakkında daha fazla bilgi için şu kaynakları araştırın:

### <a name="documentation"></a>Belgeler
* [Azure Depolama Belgeleri](https://azure.microsoft.com/documentation/services/storage/)

### <a name="for-administrators"></a>Yöneticiler için
* [Azure Depolama ile Azure PowerShell’i kullanma](storage-powershell-guide-full.md)
* [Azure Depolama ile Azure CLI'sini kullanma](storage-azure-cli.md)

### <a name="for-net-developers"></a>.NET Geliştiricileri için
* [.NET kullanarak Azure Blob Depolama ile çalışmaya başlama](storage-dotnet-how-to-use-blobs.md)
* [.NET kullanarak Azure Tablo Depolama ile çalışmaya başlama](storage-dotnet-how-to-use-tables.md)
* [.NET kullanarak Azure Kuyruk Depolama ile çalışmaya başlama](storage-dotnet-how-to-use-queues.md)
* [Windows'da Azure Dosya Depolama ile çalışmaya başlama](storage-dotnet-how-to-use-files.md)

### <a name="for-javaandroid-developers"></a>Java/Android Geliştiricileri için
* [Java'da Blob Depolama'yı kullanma](storage-java-how-to-use-blob-storage.md)
* [Java'da Tablo Depolama'yı kullanma](storage-java-how-to-use-table-storage.md)
* [Java'da Kuyruk Depolama'yı kullanma](storage-java-how-to-use-queue-storage.md)
* [Java'da Dosya Depolama'yı kullanma](storage-java-how-to-use-file-storage.md)

### <a name="for-nodejs-developers"></a>Node.js Geliştiricileri için
* [Node.js'de Blob Depolama'yı kullanma](storage-nodejs-how-to-use-blob-storage.md)
* [Node.js'de Tablo Depolama'yı kullanma](storage-nodejs-how-to-use-table-storage.md)
* [Node.js'de Kuyruk Depolama'yı kullanma](storage-nodejs-how-to-use-queues.md)

### <a name="for-php-developers"></a>PHP Geliştiricileri için
* [PHP'de Blob Depolama'yı kullanma](storage-php-how-to-use-blobs.md)
* [PHP'de Tablo Depolama'yı kullanma](storage-php-how-to-use-table-storage.md)
* [PHP'de Kuyruk Depolama'yı kullanma](storage-php-how-to-use-queues.md)

### <a name="for-ruby-developers"></a>Ruby Geliştiricileri için
* [Ruby'de Blob Depolama'yı kullanma](storage-ruby-how-to-use-blob-storage.md)
* [Ruby'de Tablo Depolama'yı kullanma](storage-ruby-how-to-use-table-storage.md)
* [Ruby'de Kuyruk Depolama'yı kullanma](storage-ruby-how-to-use-queue-storage.md)

### <a name="for-python-developers"></a>Python Geliştiricileri için
* [Python'da Blob Depolama'yı kullanma](storage-python-how-to-use-blob-storage.md)
* [Python'da Tablo Depolama'yı kullanma](storage-python-how-to-use-table-storage.md)
* [Python'da Kuyruk Depolama'yı kullanma](storage-python-how-to-use-queue-storage.md)
* [Python'da Dosya Depolama'yı kullanma](storage-python-how-to-use-file-storage.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Depolama hesabı oluşturma](storage-create-storage-account.md)
* [Beş dakikada Azure Storage’ı kullanmaya başlayın](storage-getting-started-guide.md)


<!--HONumber=Dec16_HO1-->


