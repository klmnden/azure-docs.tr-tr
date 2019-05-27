---
title: Azure depolama performansı ve ölçeklenebilirlik denetim listesi | Microsoft Docs
description: Azure depolama ile yüksek performanslı uygulamalar geliştirmek kullanmak için kendini kanıtlamış yöntemleri listesi.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 12/08/2016
ms.author: rogarana
ms.subservice: common
ms.openlocfilehash: 904b9b8ba98be5e14b1d769a0e1d8c2d6084e24d
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65951165"
---
# <a name="microsoft-azure-storage-performance-and-scalability-checklist"></a>Microsoft Azure Depolama Performansı ve Ölçeklenebilirlik Onay Listesi
## <a name="overview"></a>Genel Bakış
Microsoft Azure depolama hizmetleri sürümü,'den itibaren Microsoft, yüksek performanslı bir şekilde bu hizmetleri kullanmaya yönelik kendini kanıtlamış bir dizi geliştirdi ve bunların en önemli bir denetim stili listesine birleştirmek için bu makalede hizmet. Bu makalede amacınıza uygulama geliştiricileri, kendini kanıtlamış Azure depolama ile kullandığınızdan emin olun ve benimsemeyi düşünmelisiniz diğer kendini kanıtlamış belirlemenize yardımcı olmak amacıyla ' dir. Bu makalede, her olası performans ve ölçeklenebilirlik iyileştirme kapsayacak şekilde denemez — etkilerine küçük veya kapsamlı olarak uygulanabilir olanlar hariç tutar. Uygulamanın davranışını tasarım sırasında tahmin edilebilmesi için toplasa bile, bu performans sorunlarla karşılaşırsanız çalıştıracak tasarımları erkenden önlemek için göz önünde tutmak kullanışlıdır.  

Azure depolama kullanan her uygulama geliştiricisine, bu makaleyi okuyun ve uygulamalarını her biri aşağıda listelenen kendini kanıtlamış izlediğini denetlemek için zamanınız olması gerekir.  

## <a name="checklist"></a>Denetim listesi
Bu makalede, kendini kanıtlamış yöntemleri aşağıdaki gruplar halinde düzenler. Kendini kanıtlamış yöntemleri uygulanabilir:  

* Tüm Azure depolama hizmetleri (BLOB'lar, tablolar, kuyruklar ve dosyalar)
* Bloblar
* Tablolar
* Sıralar  

| Bitti | Alan | Category | Soru |
| --- | --- | --- | --- |
| &nbsp; | Tüm Hizmetler |Ölçeklenebilirlik hedefleri |[Uygulamanızı ölçeklenebilirlik hedefleri yaklaşan önlemek için tasarlanmıştır?](#subheading1) |
| &nbsp; | Tüm Hizmetler |Ölçeklenebilirlik hedefleri |[Adlandırma kuralınızın, daha iyi Yük Dengeleme sağlamak için tasarlanmıştır?](#subheading47) |
| &nbsp; | Tüm Hizmetler |Ağ |[İstemci tarafı cihazları yeterince yüksek bant genişliği ve gerekli performans elde etmek için düşük gecikme süresi var mı?](#subheading2) |
| &nbsp; | Tüm Hizmetler |Ağ |[İstemci tarafı cihazları yeterince yüksek kaliteli bağlantı var mı?](#subheading3) |
| &nbsp; | Tüm Hizmetler |Ağ |[İstemci uygulaması "neredeyse" depolama hesabı bulunur?](#subheading4) |
| &nbsp; | Tüm Hizmetler |İçerik Dağıtımı |[İçerik dağıtımı için bir CDN kullanıyorsunuz?](#subheading5) |
| &nbsp; | Tüm Hizmetler |Doğrudan istemci erişimi |[Depolama proxy yerine doğrudan erişmesine izin vermek için SAS ve CORS kullanıyorsunuz?](#subheading6) |
| &nbsp; | Tüm Hizmetler |Önbelleğe Alma |[Sürekli olarak kullanılan uygulama verileri önbelleğe alma ve değişiklikleri nadiren mi?](#subheading7) |
| &nbsp; | Tüm Hizmetler |Önbelleğe Alma |[Uygulamanız, (bunların istemci tarafı önbelleğe alma ve sonra daha büyük kümeleri karşıya) güncelleştirmeleri'toplu işlem?](#subheading8) |
| &nbsp; | Tüm Hizmetler |.NET yapılandırma |[Yeterli sayıda eş zamanlı bağlantı kullanmak için istemcinizi yapılandırdıktan?](#subheading9) |
| &nbsp; | Tüm Hizmetler |.NET yapılandırma |[.NET iş parçacığı yeterli sayıda kullanmak için yapılandırdığınız?](#subheading10) |
| &nbsp; | Tüm Hizmetler |.NET yapılandırma |[.NET 4.5 kullandığınız ya da daha sonra çöp toplama geliştirmiştir?](#subheading11) |
| &nbsp; | Tüm Hizmetler |Paralellik |[Böylece, istemci yeteneklerini veya ölçeklenebilirlik hedefleri aşırı yükleme yapmaz paralellik uygun şekilde sınırlanmış sağlamış olursunuz?](#subheading12) |
| &nbsp; | Tüm Hizmetler |Araçlar |[Microsoft en son sürümünü kullanarak istemci kitaplıkları ve araçları sağlanır?](#subheading13) |
| &nbsp; | Tüm Hizmetler |Yeniden Deneme Sayısı |[Kullanarak bir üstel geri alma yeniden deneme ilkesi hatalarını ve zaman aşımları azaltma için misiniz?](#subheading14) |
| &nbsp; | Tüm Hizmetler |Yeniden Deneme Sayısı |[Uygulama kaçınarak yeniden deneme denenemeyen hata mı?](#subheading15) |
| &nbsp; | Bloblar |Ölçeklenebilirlik hedefleri |[Çok sayıda eşzamanlı olarak tek bir nesne erişen istemciler var mı?](#subheading46) |
| &nbsp; | Bloblar |Ölçeklenebilirlik hedefleri |[Uygulamanızın içinde tek bir blob için bant genişliği veya işlem ölçeklenebilirlik hedefine kalıyor?](#subheading16) |
| &nbsp; | Bloblar |Blobları kopyalama |[Verimli bir şekilde, kopyalama blobları misiniz?](#subheading17) |
| &nbsp; | Bloblar |Blobları kopyalama |[BLOB toplu kopyaları için AzCopy kullanıyorsunuz?](#subheading18) |
| &nbsp; | Bloblar |Blobları kopyalama |[Azure içeri/dışarı aktarma, büyük hacimli verileri aktarmak için kullanıyorsunuz?](#subheading19) |
| &nbsp; | Bloblar |Meta verileri kullanın |[BLOB'ları hakkında sık kullanılan meta verileri, meta verileri depoladığını?](#subheading20) |
| &nbsp; | Bloblar |Hızlı yükleme |[Bir blob hızla yüklenmeye çalışılırken paralel bloklarında yüklüyorsunuz?](#subheading21) |
| &nbsp; | Bloblar |Hızlı yükleme |[Çok sayıda BLOB'ları hızlıca yüklenmeye çalışılırken blobları paralel yüklüyorsunuz?](#subheading22) |
| &nbsp; | Bloblar |Doğru Blob türü |[Sayfa blobları ya da blok blobları uygun olduğunda kullanıyorsunuz?](#subheading23) |
| &nbsp; | Tablolar |Ölçeklenebilirlik hedefleri |[Saniye başına varlıklar için ölçeklenebilirlik hedefleri yaklaştığı?](#subheading24) |
| &nbsp; | Tablolar |Yapılandırma |[Tablo isteklerinizi JSON kullanıyor musunuz?](#subheading25) |
| &nbsp; | Tablolar |Yapılandırma |[Nagle küçük isteklerinin performansını artırmak için devre dışı bırakmış?](#subheading26) |
| &nbsp; | Tablolar |Tablolar ve bölümler |[Düzgün bir şekilde verilerinizi bölümlere?](#subheading27) |
| &nbsp; | Tablolar |Etkin bölümler |[Salt eklenir ve yalnızca önüne ekleyin desenleri önleme?](#subheading28) |
| &nbsp; | Tablolar |Etkin bölümler |[Eklemeler/güncelleştirmeler birçok bölümler arasında yayılır mı?](#subheading29) |
| &nbsp; | Tablolar |Sorgu kapsamı |[Çoğu durumda kullanılacak noktası sorgular ve gerektiğinde kullanılmak üzere tablo sorguları için izin vermek için şema tasarladığınız?](#subheading30) |
| &nbsp; | Tablolar |Sorgu yoğunluğu |[Sorgular genellikle yalnızca, tarama yapmak ve uygulamanızın kullanacağı satırları döndürür?](#subheading31) |
| &nbsp; | Tablolar |Döndürülen verileri sınırlama |[Gerekli olmayan varlıklar döndürüyor önlemek için filtreleme kullanıyorsunuz?](#subheading32) |
| &nbsp; | Tablolar |Döndürülen verileri sınırlama |[Gerekli olmayan özellikler döndüren önlemek için yansıtma kullanıyorsunuz?](#subheading33) |
| &nbsp; | Tablolar |Normalleştirilmişlikten çıkarma |[Veri alınmaya çalışılırken verimsiz sorguları ya da birden çok okuma isteklerinin kaçının, verilerinizi normal dışı?](#subheading34) |
| &nbsp; | Tablolar |Ekleme/güncelleştirme/silme |[Toplu işleme, işlem için gereken istekleri veya gidiş dönüş azaltmak için aynı anda yapılabilir mi?](#subheading35) |
| &nbsp; | Tablolar |Ekleme/güncelleştirme/silme |[Yalnızca ekleme veya güncelleştirme aramak belirlemek için bir varlık alma önleme?](#subheading36) |
| &nbsp; | Tablolar |Ekleme/güncelleştirme/silme |[Sık alınır veri serisi, tek bir varlık yerine birden çok varlık özellikleri olarak birlikte depolama bulundurduğunuzdan?](#subheading37) |
| &nbsp; | Tablolar |Ekleme/güncelleştirme/silme |[Her zaman birlikte alınır ve toplu (örneğin, zaman serisi verileri) olarak yazılan varlıklar için BLOB'ları, tabloları yerine kullanarak bulundurduğunuzdan?](#subheading38) |
| &nbsp; | Sıralar |Ölçeklenebilirlik hedefleri |[Saniye başına ileti için ölçeklenebilirlik hedefleri yaklaştığı?](#subheading39) |
| &nbsp; | Sıralar |Yapılandırma |[Nagle küçük isteklerinin performansını artırmak için devre dışı bırakmış?](#subheading40) |
| &nbsp; | Sıralar |İleti Boyutu |[Kuyruğun performansını artırmak için iletileri compact misiniz?](#subheading41) |
| &nbsp; | Sıralar |Toplu Al |["Get" tek bir işlemde birden çok ileti alıyor?](#subheading42) |
| &nbsp; | Sıralar |Yoklama sıklığı |[Uygulamanızı algılanan gecikme süresini azaltmak için yoklama sıklıkta?](#subheading43) |
| &nbsp; | Sıralar |İletiyi güncelleştirme |[Bir hata oluşursa, iletinin tamamı yeniden işlemek zorunda ilerleme iletilerini işleme depolamak için UpdateMessage kullanıyorsunuz?](#subheading44) |
| &nbsp; | Sıralar |Mimari |[Uzun süre çalışan iş yükleri kritik yol dışında tutarak tüm uygulamanızın daha ölçeklenebilir yapmak ve birbirinden bağımsız olarak ölçeklendirme için kuyrukları kullanıyorsunuz?](#subheading45) |

## <a name="allservices"></a>Tüm hizmetler
Bu bölüm, herhangi bir Azure depolama hizmeti (BLOB, tablo, kuyruk veya dosyaları) için geçerlidir, kendini kanıtlamış uygulamaları listeler.  

### <a name="subheading1"></a>Ölçeklenebilirlik hedefleri
Azure depolama kendisini her Abonelikteki bölge başına 250 depolama hesapları sınırı vardır. Bu sınıra ulaştıysanız, bu abonelik/bölge birlikte daha fazla tüm depolama hesaplarını oluşturmak mümkün olmayacaktır.

Azure Depolama hizmetlerinin her biri, Kapasite (GB), işlem hızı ve bant genişliği için ölçeklenebilirlik hedefleri sahiptir. Uygulamanızı yaklaştığında veya herhangi bir ölçeklenebilirlik hedefleri aşıyor, yüksek işlem gecikme süresi veya azaltma karşılaşabilirsiniz. Uygulamanızı bir depolama hizmetine kısıtlar, "503 Sunucu meşgul" veya "500 işlem zaman aşımı" hata kodları bazı depolama işlemleri için döndürülecek hizmetin başlar. Bu bölümde, özellikle ölçeklenebilirlik hedefleri ve bant genişliği ölçeklenebilirlik hedefleri ile ilgili iki genel yaklaşım ele alınmaktadır. Ayrı ayrı depolama hizmetleri ile ilgili sonraki bölümlerde, belirli bir hizmet bağlamında ölçeklenebilirlik hedefleri açıklanmaktadır:  

* [BLOB bant genişliği ve saniyede istek](#subheading16)
* [Saniye başına tablo varlıkları](#subheading24)
* [Saniye başına kuyruk iletileri](#subheading39)  

#### <a name="sub1bandwidth"></a>Tüm hizmetler için bant genişliği ölçeklenebilirlik hedefine
Makalenin yazıldığı sırada, bant genişliği hedefleri coğrafi olarak yedekli depolama (GRS) hesabı için ABD'de 10 Gigabit / saniye (Gbps) (depolama hesabına gönderilen veriler) giriş ve çıkış (depolama hesabından gönderilen veriler) için 20 GB/sn içindir. Bir yerel olarak yedekli depolama (LRS) hesabı için sınırları daha yüksek – 20 GB/sn giriş ve çıkış için 30 GB/sn.  Uluslararası bant genişliği sınırlarını daha düşük olabilir ve bulunabilir bizim [ölçeklenebilirlik hedefleri sayfa](https://msdn.microsoft.com/library/azure/dn249410.aspx).  Depolama yedekliliği seçenekleri hakkında daha fazla bilgi için faydalı kaynaklar aşağıdaki bağlantılara bakın.  

#### <a name="what-to-do-when-approaching-a-scalability-target"></a>Ölçeklenebilirlik hedefine bölümüyle iletişime geçerken yapmanız gerekenler
Depolama hesabı içinde bir belirli aboneliğe/bölge birleşimi olabilir sınırına yaklaşılıyor, uygulama ve depolama hesapları kullanımını değerlendirin ve Bu koşullardan herhangi biri geçerli olup olmadığına belirleyin.

* Yönetilmeyen diskler depolama hesaplarının kullanılması ve sanal makinelerinize bu diskleri ekleme. Bu senaryoda kullanılması önerilir [yönetilen diskler](../../virtual-machines/windows/managed-disks-overview.md)depolama disk ölçeklenebilirliği, ayrı depolama hesaplarını oluşturur ve yönetirken gerek kalmadan işledikleri gibi.
* Veri yalıtımı amacıyla, müşteri başına temelinde bir depolama hesabı kullanma. Bu senaryoda depolama kapsayıcıları tüm depolama hesabınız yerine her müşteri için kullanmanızı öneririz. Azure depolama artık rol tabanlı erişim denetimi üzerinde belirtmenize olanak verir bir başına [kapsayıcı temel](storage-auth-aad-rbac-portal.md).
* Birden çok depolama hesabında parçalara ayırmak için giriş/çıkış/IOPS/kapasitesi daha büyük ölçeklendirme kullanma. Bu senaryoda, mümkünse, avantajından öneririz [artırılmış sınırlar](https://azure.microsoft.com/blog/announcing-larger-higher-scale-storage-accounts/) yükünüz için gerekli depolama hesapları sayısını azaltmak için standart depolama hesabı.

Uygulamanız için bir tek bir depolama hesabı ölçeklenebilirlik hedefleri yaklaşıyorsa, aşağıdaki yaklaşımlardan birini benimsemeyi göz önünde bulundurun:  

* Uygulamanızın yaklaşımını veya ölçeklenebilirlik hedef en fazla iş yükü yeniden belirleyin. Daha az bant genişliği veya kapasite ya da daha az işlem farklı kullanacak şekilde tasarlayabilirsiniz?
* Bir uygulama ölçeklenebilirlik hedefleri birini aşması gereken, uygulama verilerinizi birden çok depolama hesapları ve bölüm bu birden çok depolama hesabında yer oluşturmanız gerekir. Bu düzeni kullanın, ardından Yük Dengeleme için depolama hesabı daha sonra ekleyebilirsiniz. böylece uygulamanızı tasarlayın emin olun. Makalenin yazıldığı sırada, her bir Azure aboneliği en fazla 100 depolama hesabı olabilir.  Depolama hesapları, ücretsiz dışındaki kullanımınız açısından depolanan veriler, yapılan işlemleri ve aktarılan veriler de var.
* Uygulamanızı bant hedefleri olursa, veri depolama hizmetine göndermek için gereken bant genişliğini azaltmak üzere istemci verileri sıkıştırma göz önünde bulundurun.  Bu bant genişliğinden tasarruf ve ağ performansı, ancak ayrıca bazı olumsuz etkileri olabilir.  Bu performans etkisi nedeniyle, sıkıştırma ve veri İstemcisi'nde açma ek işleme gereksinimleri belirlemelidir. Ayrıca, sıkıştırılmış veri depolama, standart araçlarını kullanarak depolanan verileri görüntülemek daha zor olabilir beri sorunlarını gidermek daha zor zorlaştırabilir.
* Uygulamanız için ölçeklenebilirlik hedefleri değerse, ardından yeniden deneme sayısı için bir üstel geri alma kullandığınızdan emin olun (bkz [deneme](#subheading14)).  Hiçbir zaman (yukarıdaki yöntemlerden birini kullanarak) ölçeklenebilirlik hedefleri yaklaşımı, ancak bu uygulama yalnızca hızlı bir şekilde, daha da kötüsü azaltma yapmadan yeniden denemeye devam olmaz sağlayacak emin olmak daha iyidir.  

#### <a name="useful-resources"></a>Yararlı Kaynaklar
Aşağıdaki bağlantılar ölçeklenebilirlik hedefleri hakkında ek ayrıntılar sağlar:

* Bkz: [Azure Storage ölçeklenebilirlik ve performans hedefleri](storage-scalability-targets.md) ölçeklenebilirlik hedefleri hakkında bilgi için.
* Bkz: [Azure depolama çoğaltma](storage-redundancy.md) ve blog gönderisinde [Azure depolama Yedekliliği seçenekleri ve okuma erişimli coğrafi olarak yedekli depolama](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx) depolama yedekliliği seçenekleri hakkında bilgi için.
* Azure Hizmetleri için fiyatlandırma hakkında güncel bilgiler için bkz [Azure fiyatlandırması](https://azure.microsoft.com/pricing/overview/).  

### <a name="subheading47"></a>Bölüm adlandırma kuralları
Azure depolama, bir aralık tabanlı bölümleme düzeni için ölçek ve Yük Dengeleme sistemi kullanır. Bölüm anahtarı, aralıkları ve bu aralıklar veri yük sistem dengeli bölüm için kullanılır. Bu adlandırma kuralları gibi sözcük sıralama (örneğin, msftpayroll, msftperformance, msftemployees, vb.) veya zaman damgalarını (log20160101, log20160102, log20160102, vb.) kullanarak kiralamak kendisi üzerinde olası birlikte bölüm anlamına gelir. bir yük dengeleme işlemi bunları dışarı daha küçük aralıkları böler kadar aynı bölüm sunucusu. Örneğin, bir kapsayıcıdaki tüm BLOB'lar bu bloblarda yük daha fazla bölüm aralığı yeniden dengelenmesi gerekir kadar tek bir sunucu tarafından sunulabilir. Benzer şekilde, hafifçe yüklü hesaplarıyla sözcük sırayla düzenlenmiş adları bir grup kadar bir yükleme tek bir sunucu tarafından sunulacak veya tüm bu hesaplarının bunları birden çok bölüm sunucular arasında bölünmesi gereken gerektirir. Her yük dengeleme işlemi, işlem sırasında depolama çağrıları gecikme süresini etkileyebilir. Yük Dengeleme işlemi kicks açma ve bölüm anahtar aralığı yeniden dengeler kadar tek bölüm sunucu ölçeklenebilirliğini tarafından sistemin bir bölüme trafik ani bir aşırı işleme yeteneği sınırlıdır.  

Bu işlemler sıklığını azaltmak için bazı en iyi uygulamaları takip edebilirsiniz.  

* Hesapları, kapsayıcıları, blobları, tablolar ve Kuyruklar, yakından kullandığınız adlandırma kuralını inceleyin. Hesap adlarını ihtiyaçlarınıza uygun bir karma işlevi kullanarak 3 haneli karma ile önek göz önünde bulundurun.  
* Zaman damgaları veya sayısal tanımlayıcıları kullanarak verilerinizi düzenlemek, bir yalnızca ekleme (veya yalnızca önüne ekleyin) trafiği desenlerini kullanmıyorsanız emin olmak zorunda. Bu desenleri için bir aralık uygun değil-Sistem bölümleme tabanlı ve müşteri adayı sistemden etkili bir şekilde sınırlama ve tek bir bölüme giden tüm trafik için Yük Dengeleme. Yyyymmdd gibi bir zaman damgasına sahip bir blob nesnesi kullanma günlük işlemler varsa, örneğin, ardından günlük bu işlem için tüm trafik tek bölüm sunucusu tarafından sunulan tek bir nesneye yönlendirilir. Konum olup başına blob limitler ve bölüm başına limitler gereksinimlerinizi karşılayacak ve bu işlemi gerekiyorsa birden çok BLOB olarak bozucu göz önünde bulundurun. Tabloları zaman serisi verilerini depolamak, benzer şekilde, tüm trafik olabilir son anahtar ad parçası gösterilmiştir. Zaman damgaları veya sayısal kimlikleri kullanmanız gerekirse, önek kimliği 3 haneli karma veya zaman damgası söz konusu olduğunda ssyyyymmdd gibi zamanın saniye parçası önek. Listeleme ve sorgulama işlemleri düzenli olarak gerçekleştirilir, sorgular, sayısını sınırlar bir karma işlevi seçin. Diğer durumlarda, rastgele bir önek yeterli olabilir.  
* Azure Depolama'da kullanılan bölümleme düzeni hakkında daha fazla bilgi okumak için SOSP kağıt [burada](https://sigops.org/sosp/sosp11/current/2011-Cascais/printable/11-calder.pdf).

### <a name="networking"></a>Ağ
Genellikle uygulamanın fiziksel ağ kısıtlamaları yalnızca birkaç API çağrısı sırasında performansı büyük bir etkiye sahip. Aşağıdaki sınırlamalar kullanıcının karşılaşabileceği bazılarını açıklar.  

#### <a name="client-network-capability"></a>İstemci ağ özelliği
##### <a name="subheading2"></a>Aktarım hızı
İçin bant genişliği, sorun genellikle istemci yeteneklerini olmasıdır. Örneğin, 10 GB/sn veya daha fazla giriş tek bir depolama hesabında işleyebilir sırada (bkz [bant genişliği ölçeklenebilirlik hedefleri](#sub1bandwidth)), "Küçük" Azure çalışan rolü örneği cinsinden ağ hızı yalnızca yaklaşık 100 MB/sn bant yeteneğine sahiptir. Tek bir makineden daha yüksek ağ limitlere ihtiyacınız varsa daha büyük bir örnek veya daha fazla sanal makine kullanmayı düşünmeniz gerekir böylece büyük Azure örneklerinde NIC büyük kapasitede bulunur. Açık bir şirket içi uygulamaya ait bir depolama hizmetine eriştiğiniz sonra aynı kural uygulanır: istemci cihazı ve ağ bağlantısı Azure depolama konumu için ağ yeteneklerini anlamalarına ve ya da bunları gerektiği gibi artırın veya tasarım, yeteneklerini içinde çalışmak için uygulama.  

##### <a name="subheading3"></a>Bağlantı kalitesi
Tüm ağ kullanımla olduğu gibi hataları ve paket kaybı ağ koşulları etkin aktarım hızı yavaşlatır unutmayın.  WireShark veya NetMon kullanarak bu sorunu tanılamaya yardımcı olabilir.  

##### <a name="useful-resources"></a>Yararlı Kaynaklar
Sanal makine boyutları ve ayrılmış bant genişliği hakkında daha fazla bilgi için bkz: [Windows VM boyutları](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) veya [Linux VM boyutları](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  

#### <a name="subheading4"></a>Konum
Dağıtılmış bir ortamda istemci yakın sunucu yerleştirme en iyi performans sunar. En düşük gecikme ile Azure depolama erişimi için en iyi istemci için aynı Azure bölgesinde konumdur. Azure depolama kullanan bir Azure Web siteniz varsa, örneğin, her ikisinin de tek bir bölgede (örneğin, Batı ABD veya Güneydoğu Asya) bulmanıza. Bu gecikme süresini ve maliyetini azaltır — makalenin yazıldığı sırada, tek bir bölgede bant genişliği kullanımı ücretsizdir.  

Uygulamaları Azure içinde (mobil cihaz uygulamaları gibi veya şirket içi Kurumsal Hizmetler), daha sonra tekrar barındırılmamaktadır istemciniz, erişen cihazların yakın bir bölgede depolama hesabına yerleştirilmesi genellikle gecikme süresi azaltacak durumunda. İstemcilerinize (için örnek, bazı Kuzey Amerika ve Avrupa'daki bazı) geniş çapta dağıtılan sonra birden fazla depolama hesabı kullanmayı düşünmelisiniz: Kuzey Amerika bölgesi, diğeri Avrupa bölgesinde bulunan. Bu, her iki bölgede de kullanıcılar için gecikme süresini azaltmak için yardımcı olur. Bu yaklaşım, uygulama verilerini depolayan, uygulamanız bireysel kullanıcılar için özeldir ve depolama hesapları arasında verilerin çoğaltılması gerektirmez çok daha kolay olur.  Geniş içerik dağıtımı için bir CDN önerilir: daha fazla ayrıntı için sonraki bölüme bakın.  

### <a name="subheading5"></a>İçerik dağıtımı
Bazen, çok sayıda kullanıcı (bir Web sitesinin giriş sayfasında kullanılır. Örneğin, bir ürün tanıtım videosu), aynı veya birden çok bölgede bulunan aynı içerik sunmak bir uygulama gerekir. Bu senaryoda, Content Delivery Network (CDN) gibi Azure CDN kullanmalıdır ve CDN, Azure depolama veri kaynağı kullanırsınız. Tek bir bölgede mevcut olan ve diğer bölgeler için düşük gecikme süresine sahip, içerik teslim edilemiyor bir Azure depolama hesabı farklı olarak, Azure CDN, dünyanın dört bir yanındaki birden fazla veri merkezinde sunucuları kullanır. Ayrıca, bir CDN genellikle tek bir depolama hesabında çok daha yüksek çıkış sınırlardan destekleyebilir.  

Azure CDN hakkında daha fazla bilgi için bkz. [Azure CDN](https://azure.microsoft.com/services/cdn/).  

### <a name="subheading6"></a>SAS ve CORS kullanma
JavaScript gibi bir kod bir kullanıcının web tarayıcısında veya mobil telefon uygulaması Azure Depolama'daki verilere erişme yetkisi vermek, ihtiyacınız olduğunda, bir uygulamanın web rolü, proxy olarak kullanmak için bir yaklaşım ise: kullanıcının cihazına web rolüyle kimliğini doğrular. , hangi sırayla yetkilendirir depolama kaynaklarına erişim. Bu şekilde, güvenli olmayan cihazlarda, depolama hesabı anahtarlarını gösterme önleyebilirsiniz. Kullanıcının cihaz ve depolama hizmeti arasında aktarılan tüm veriler web rolü geçmesi gerekir çünkü Bununla birlikte, bu önemli bir ek yük web rolünde yerleştirir. Bir web rolü bir proxy olarak depolama hizmeti için paylaşılan erişim imzaları (SAS) bazen üstbilgileri çıkış noktaları arası kaynak paylaşımı (CORS) ile birlikte kullanarak önleyebilirsiniz. SAS kullanarak, kullanıcı cihazının bir sınırlı erişim belirteci kullanarak bir depolama hizmetine doğrudan isteğinde bulunmak izin verebilirsiniz. Örneğin, bir kullanıcı bir fotoğraf uygulamanızı karşıya yüklemek isterse, web rolü oluşturma ve sonraki 30 (sonra SAS belirteci süre sonu) dakika boyunca bir bloba veya kapsayıcıya yazma izni veren bir SAS belirteci kullanıcının cihazına gönderin.

Normalde, bir tarayıcıda JavaScript bir "başka bir etki alanı için PUT" gibi belirli işlemleri gerçekleştirmek için bir etki alanındaki bir Web sitesi tarafından barındırılan bir sayfada izin vermez. Örneğin, "aynı çıkış noktası İlkesi" tarayıcı "contosomarketing.cloudapp.net" bir web rolü barındırmak ve blob depolama hesabınıza "contosoproducts.blob.core.windows.net" yüklemek için istemci tarafı JavaScript kullanmak istiyorsanız, bu işlem yasaklar. CORS tarayıcıya (Bu durumda web rolü) kaynak etki alanındaki kaynaklanan istekler güvenleri iletişim kurmak için hedef etki alanında (Bu durumda depolama hesabı) sağlayan bir tarayıcı özelliğidir.  

Bu iki teknoloji gereksiz yük (ve performans sorunlarını) web uygulamanızda önlemenize yardımcı olabilir.  

#### <a name="useful-resources"></a>Yararlı Kaynaklar
SAS hakkında daha fazla bilgi için bkz: [paylaşılan erişim imzaları, 1. Bölüm: SAS modelini anlama](../storage-dotnet-shared-access-signature-part-1.md).  

CORS hakkında daha fazla bilgi için bkz: [Azure Storage Hizmetleri için çıkış noktaları arası kaynak paylaşımı (CORS) desteği](https://msdn.microsoft.com/library/azure/dn535601.aspx).  

### <a name="caching"></a>Önbelleğe Alma
#### <a name="subheading7"></a>Veri alma
Genel olarak, bir hizmetten veri alma, bir kez daha iki kez alma daha iyidir. Zaten 50 MB'lık bir blobun bir kullanıcıya içerik olarak görev yapacak Depolama hizmetinden alınan bir web rolünde çalışan bir MVC web uygulaması örneği göz önünde bulundurun. Uygulama, daha sonra bir kullanıcı istekte ya da yerel olarak disk ve sonraki kullanıcı istekleri için önbelleğe alınmış sürümünü yeniden önbelleğe alınamadı, aynı blob alabilir. Ayrıca, her bir kullanıcı verileri, uygulama sorunu, değiştirilmemiş varsa tüm blob girmeyi önlemek değiştirme saati için koşullu bir üstbilgiyle ALABİLİR ister. Bu aynı deseni tablo varlıklarla çalışmaya uygulayabilirsiniz.  

Bazı durumlarda, uygulamanızın blob alınması sonra kısa bir süre için geçerli kalmasını ve bu dönemde uygulamanın blob değiştirildiği denetlemek gerekmeyen varsayabilirsiniz karar verebilirsiniz.

Yapılandırma, arama ve her zaman uygulama tarafından kullanılan diğer veri önbelleğe almak için iyi adaylar olan.  

.NET kullanarak son değiştirilme tarihi bulmak için bir blob'un özellikleri almak nasıl bir örnek için bkz [ayarlama ve alma özellikleri ve meta verileri](../blobs/storage-properties-metadata.md). Koşullu indirmeler hakkında daha fazla bilgi için bkz: [koşullu olarak bir Blob yerel bir kopyasını yenilemek](https://msdn.microsoft.com/library/azure/dd179371.aspx).  

#### <a name="subheading8"></a>Toplu veri yükleme
Bazı uygulama senaryolarında yerel olarak veri toplama ve sonra düzenli aralıklarla hemen her veri parçası karşıya yerine toplu karşıya yükleyin. Örneğin, bir web uygulaması etkinliklerinin bir günlük dosyası tutabilir: (birçok depolama işlemleri gerektiren) Tablo varlığı olur veya Etkinlik ayrıntıları yerel bir günlük dosyasına kaydedebilir uygulama ya da her etkinlik ayrıntılarını yükleyebilir ve ardından düzenli aralıklarla tüm etkinlik ayrıntılarını bir bloba sınırlandırılmış bir dosyanın olarak karşıya yükleyin. Her günlük girişinin, 1 KB boyutundaki ise, binlerce (64 MB'lik boyutu tek bir işlemde bir blob karşıya yükleyebilirsiniz) tek bir "yerleştirme Blob" işlemde karşıya yükleyebilirsiniz. Yerel makinede karşıya yükleme öncesinde çökerse, potansiyel olarak bazı günlük verileri kaybedersiniz: uygulama geliştiricisi tasarlamak için istemci cihaz olasılığını veya karşıya yükleme hataları.  Etkinlik verileri timespans (yalnızca tek etkinliği) için indirilmesi gerekiyorsa, BLOB'ları tablolar üzerindeki tavsiye edilir.

### <a name="net-configuration"></a>.NET yapılandırma
.NET Framework kullanıyorsa, bu bölümde, önemli performans geliştirmeleri yapmak için kullanabileceğiniz birkaç Hızlı yapılandırma ayarları listelenir.  Diğer dillerde kullanıyorsanız benzer kavramları seçtiğiniz dilde uygulanacaksa denetleyin.  

#### <a name="subheading9"></a>Varsayılan bağlantı sınırını artırmak
. NET'te, aşağıdaki kod (genellikle olan bir istemci ortamda 2 ya da bir sunucu ortamında 10) varsayılan bağlantı sınırı 100 artırır. Genellikle, yaklaşık uygulamanız tarafından kullanılan iş parçacıklarının sayısı değeri ayarlamanız gerekir.  

```csharp
ServicePointManager.DefaultConnectionLimit = 100; //(Or More)  
```

Tüm bağlantıları açmadan önce bağlantı üst sınırına ayarlamanız gerekir.  

Diğer programlama dili için nasıl bağlantı üst sınırına ayarlanacağını belirlemek için dil belgelerine bakın.  

Daha fazla bilgi için bkz. blog gönderisine [Web Hizmetleri: Eş zamanlı bağlantı](https://blogs.msdn.com/b/darrenj/archive/2005/03/07/386655.aspx).  

#### <a name="subheading10"></a>İş parçacığı havuzu en az iş parçacığı zaman uyumlu bir kod ile zaman uyumsuz görevleri kullanıyorsanız artırın
Bu kod, iş parçacığı havuzu min iş parçacıkları artırır:  

```csharp
ThreadPool.SetMinThreads(100,100); //(Determine the right number for your application)  
```

Daha fazla bilgi için [ThreadPool.SetMinThreads yöntemi](https://msdn.microsoft.com/library/system.threading.threadpool.setminthreads%28v=vs.110%29.aspx).  

#### <a name="subheading11"></a>.NET 4.5 çöp toplama yararlanın
.NET 4.5 veya sonraki istemci uygulaması için sunucu çöp toplamada performans iyileştirmelerinden yararlanmak için kullanın.

Daha fazla bilgi için bkz [bir genel bakış, performans geliştirmeleri .NET 4.5](https://msdn.microsoft.com/magazine/hh882452.aspx).  

### <a name="subheading12"></a>Sınırsız paralellik
Paralellik performans için harika olabilir, ancak sınırsız paralellik (iş parçacıkları ve/veya paralel istekler sayısı sınır) karşıya yükleyin veya birden çok bölüme erişmek için birden fazla çalışana kullanarak verileri yüklemek için kullanırken dikkatli (kapsayıcılar, kuyruklar, veya Tablo bölümleri) aynı depolama hesabındaki veya birden çok öğe aynı bölüme erişmek için. Paralellik sınırsız ise, uygulamanızın istemci cihazın özelliklerini aşabilir miyim veya depolama hesabının ölçeklenebilirlik elde edilen uzun gecikme süreleri ve azaltma hedefler.  

### <a name="subheading13"></a>Depolama istemci kitaplıkları ve araçları
Her zaman en son Microsoft sağlanan istemci kitaplıkları ve araçları kullanın. Makalenin yazıldığı sırada, diğer diller için Önizleme kitaplıkları yanı sıra .NET, Windows Phone, Windows çalışma zamanı, Java ve C++ için kullanılabilir olan istemci kitaplıkları vardır. Ayrıca, Microsoft, PowerShell cmdlet'leri ve Azure depolama ile çalışmak için Azure CLI komutlarını kullanıma sundu. Microsoft etkin bir şekilde performans aklınızda bu araçlarla geliştiren, en son hizmet sürümleri ile güncel kalmasını sağlar ve kanıtlanmış performansından uygulamalarının dahili olarak işledikleri sağlar.  

### <a name="retries"></a>Yeniden Deneme Sayısı
#### <a name="subheading14"></a>Azaltma ServerBusy
Bazı durumlarda, depolama hizmeti, uygulamanızın azaltma veya yalnızca bazı geçici koşul nedeniyle isteği hizmet ve "503 Sunucu meşgul" iletisi veya "500 zaman aşımı" dönmek olabilir.  Uygulamanızı herhangi bir ölçeklenebilirlik hedefleri yaklaştığını veya bölümlenmiş verilerinizi daha yüksek aktarım hızı için izin vermek için sistemin yeniden Dengeleme yoksa bu durum oluşabilir.  İstemci uygulaması genellikle, bu tür bir hataya neden olan işlemi yeniden denemesinin: daha sonra aynı istek denemesi başarılı olabilir. Bununla birlikte, ölçeklenebilirlik hedefleri aşılıyor çünkü depolama hizmeti, uygulamanızın azaltma veya hizmet isteği başka bir nedenle hizmet kuramadı olsa bile, agresif yeniden deneme genellikle sorun daha da kötüsü oluşturur. Bu nedenle, bir üstel geri alma (Bu davranışı için istemci kitaplıkları varsayılan) kullanmanız gerekir. Örneğin, uygulamanız 2 saniye sonra 4 saniye sonra 10 saniye sonra 30 saniye sonra yeniden deneyin ve tamamen vazgeçerlerdi. Bu davranış, önemli ölçüde hizmet yüküne azaltma yerine problemleri exacerbating uygulamanızda sonuçlanır.  

Azaltma sonucu değildir ve geçici olması nedeniyle bağlantı hataları hemen yeniden denenebilir.  

#### <a name="subheading15"></a>Denenemeyen hata
İstemci kitaplıkları, hangi hataları yeniden deneme yapabilir ve desteklenmeyen farkındayız. Depolama REST API'sini karşı kendi kod yazıyorsanız, Bununla birlikte, yeniden bazı hatalar unutmayın: Örneğin, bir 400 (hatalı yanıt, istemci uygulama, olmadığından işlenemedi bir istek gönderdiğini gösterir istek) Beklenen bir biçimde. Hiçbir noktası, yeniden deneniyor, bu nedenle bu isteği yeniden her zaman aynı yanıtı neden olur. Depolama REST API'sini karşı kendi kod yazıyorsanız, hata kodlarını ne anlama geliyor ve (veya etkinleştirmezsiniz) yeniden denemek için en uygun yolu unutmayın her biri için.  

#### <a name="useful-resources"></a>Yararlı Kaynaklar
Depolama hata kodları hakkında daha fazla bilgi için bkz. [durumu ve hata kodları](https://msdn.microsoft.com/library/azure/dd179382.aspx) Microsoft Azure web sitesinde.  

## <a name="blobs"></a>Bloblar
Kendini kanıtlamış yöntemleri için ek olarak [tüm hizmetleri](#allservices) daha önce açıklandığı gibi başarısı kanıtlanmış yöntemler aşağıdaki özel olarak blob hizmeti için geçerlidir.  

### <a name="blob-specific-scalability-targets"></a>BLOB özgü ölçeklenebilirlik hedefleri
#### <a name="subheading46"></a>Birden çok istemci aynı anda tek bir nesneye erişme
Çok sayıda eşzamanlı olarak tek bir nesne erişen istemciler varsa, nesne ve depolama hesabı ölçeklenebilirlik hedefleri göz önünde bulundurmanız gerekir. Tek bir nesne erişen istemcilerin sayısına nesnesi eşzamanlı olarak isteyen istemcilerin nesnenin boyutu sayısı gibi faktörlere bağlı olarak değişir, vb. koşulları ağ.

Nesne aracılığıyla dağıtılabilmesi durumunda bir Web sitesinden bir CDN görüntü veya video gibi hizmet ve CDN kullanmanız gerekir. Bkz: [burada](#subheading5).

Bilimsel benzetimleri veriyi gizli olduğu gibi diğer senaryolarda, iki seçeneğiniz vardır. İş yükünüzün erişim sağlayacak şekilde nesnesi aynı anda erişilen süresine karşılık verinceye erişilen basamaklandırmak için davranıştır. Alternatif olarak, bu nedenle toplam IOPS nesne başına ve depolama hesaplarınız arasında artan birden fazla depolama hesabı için geçici olarak nesne kopyalayabilirsiniz. Sınırlı testinde, 100 GB'lık blob (her sanal makine 32 iş parçacığı kullanarak indirme paralelleştirmek) paralel yaklaşık 25 sanal makineleri aynı anda yüklenemedi bulduk. Nesneye erişmek için önce ikinci bir depolama hesabına kopyalayın ve sonra gerek kalmadan 100 istemci olsaydı ilk 50 Vm'leri ilk blob ve ikinci 50 Vm'leri erişim ikinci blob erişin. Bu tasarım sırasında test etmeniz gerekir, böylece sonuçlar, uygulama davranışına bağlı olarak değişir. 

#### <a name="subheading16"></a>Bant genişliği ve Blob başına işlem
Okuma veya yazma sırasında tek bir bloba en fazla 60 MB/saniye (yaklaşık 480 Mb/sn aşıyor (fiziksel NIC'ye istemci cihazdaki dahil) çok sayıda istemci tarafı ağ yeteneklerini budur. Ayrıca, tek bir blob, saniyede en fazla 500 istek destekler. Aynı blob okumak için gereken birden çok istemci varsa ve bu limitlerin, blob dağıtmak için bir CDN kullanmayı düşünmeniz gerekir.  

Blobları hedef performans düzeyleri hakkında daha fazla bilgi için bkz. [Azure Storage ölçeklenebilirlik ve performans hedefleri](storage-scalability-targets.md).  

### <a name="copying-and-moving-blobs"></a>Kopyalama ve BLOB'ları taşıma
#### <a name="subheading17"></a>Kopya blob'u
Depolama REST API sürümü 2012-02-12 yararlı blobları hesapları arasında kopyalama olanağı sundu: bir istemci uygulaması (büyük olasılıkla farklı depolama hesabındaki) başka bir kaynaktan bir bloba kopyalamak için depolama hizmeti isteyin ve ardından hizmet gerçekleştirme sağlar kopya zaman uyumsuz olarak. Bu, indirmek ve verileri karşıya yüklemek gerekmez çünkü, diğer depolama hesaplarından veri geçişi sırasında uygulama için gereken bant genişliğini önemli ölçüde azaltabilir.  

Bir göz önünde bulundurarak, ancak depolama hesapları arasında kopyalama yapılırken, kopyalama tamamlandığında üzerinde süresi garantisi yoktur. Uygulamanızı hızlı bir şekilde denetiminiz altında bir blob kopyası tamamlaması gerektiğinde, bir VM'ye indirerek ve hedef olarak karşıya yükleme blob kopyalamak daha iyi olabilir.  Bu durumda tam öngörülebilirlik için aynı Azure bölgesinde çalışan bir VM tarafından gerçekleştirilen kopyalama, aksi takdirde, ağ koşulları kopyalama performansınızı etkileyen olabilir ve muhtemelen emin olun.  Ayrıca, program aracılığıyla bir zaman uyumsuz kopya ilerlemesini izleyebilirsiniz.  

Aynı depolama hesabında kendisini kopyaları genellikle hızlı bir şekilde tamamlanır.  

Daha fazla bilgi için [kopya blob'u](https://msdn.microsoft.com/library/azure/dd894037.aspx).  

#### <a name="subheading18"></a>Azcopy'yi kullanma
Azure depolama ekibi bir komut satırı aracı "AzCopy" yayınlandı birçok bloblar için gelen ve depolama hesaplarınız arasında aktarma toplu yardımcı olmak amacıyla tasarlanmıştır.  Bu araç, bu senaryo için en iyi duruma getirilmiş ve yüksek aktarım hızı elde edebilirsiniz.  Kullanımı toplu karşıya yükleme, indirme ve kopyalama senaryoları için önerilir. Hakkında daha fazla bilgi edinin ve indirmek için bkz: [AzCopy komut satırı yardımcı programı ile veri aktarma](storage-use-azcopy.md).  

#### <a name="subheading19"></a>Azure içeri/dışarı aktarma hizmeti
Büyük hacimli verileri (1 TB'den fazla) için Azure Depolama'ya yükleme ve blob depolama alanından sevkiyat sabit sürücüler tarafından indirme veren içeri/dışarı aktarma hizmeti sunar.  Bir sabit sürücüde verilerinizden ve karşıya yükleme için Microsoft'a göndermek veya boş bir sabit sürücü verileri indirmek için Microsoft'a gönderin.  Daha fazla bilgi için bkz. [Blob Depolama'ya Veri Aktarmak için Microsoft Azure İçeri/Dışarı Aktarma Hizmetini Kullanma](../storage-import-export-service.md).  Bu, karşıya yükleme/bu veri hacmini ağ üzerinden indirilmesine göre çok daha verimli olabilir.  

### <a name="subheading20"></a>Meta verileri kullanın
Blob hizmeti, blob meta verilerini içeren head isteklerini destekler. Örneğin, bir fotoğraf dışında EXIF verilerine uygulamanız gerekirse, fotoğraf alabilir ve ayıklayın. Uygulamanın karşıya fotoğraf, bant genişliği kazandırabilir ve performansı artırmak için uygulamanızı EXIF verilerine blobun meta verilerde depolayabilir: önemli bant genişliğinden daha sonra yalnızca bir HEAD isteği kullanarak meta verilerini EXIF verilerinde alabilir ve işleme süresi, her blobun okunur EXIF verileri ayıklamak gerekli. Bu, yalnızca meta verilerin yanı sıra, bir blob değil tam içeriği gereken senaryolarda yararlı olabilir.  Böylece veriler, boyutu uygun değilse, size bu yaklaşımı kullanmak mümkün olmayabilir (hizmet deposu birden çok, bir istek kabul etmeyecek), blob başına yalnızca 8 KB'lık meta verileri depolanabilir.  

.NET kullanarak bir blobun meta verilerini almak nasıl bir örnek için bkz [ayarlama ve alma özellikleri ve meta verileri](../blobs/storage-properties-metadata.md).  

### <a name="uploading-fast"></a>Hızlı yükleme
Hızlı blobları karşıya yüklemek için ilk soru olduğu: olan, bir blob ya da birden çok yükleme?  Kullanan aşağıdaki senaryonuza bağlı olarak kullanmak için doğru yöntemini belirlemek üzere Kılavuzu.  

#### <a name="subheading21"></a>Bir büyük blob hızlı bir şekilde karşıya yükleme
Tek bir büyük blob hızlı bir şekilde karşıya yüklemek için İstemci uygulamanızın, blokları veya sayfaları paralel (oluşturduğunu tek tek bloblar ve bir bütün olarak depolama hesabı için ölçeklenebilirlik hedefleri olan) yüklemeniz gerekir.  Resmi Microsoft tarafından sağlanan RTM depolama istemcisi kitaplıkları (.NET, Java) bunu yapmak için sahipsiniz.  Her kitaplık eşzamanlılık düzeyini ayarlamak için belirtilen nesne/özelliği altında:  

* .NET: ParallelOperationThreadCount kullanılacak Locationmode nesnesi üzerinde ayarlayın.
* Java/Android: Use BlobRequestOptions.setConcurrentRequestCount()
* Node.js: İstek seçenekleri veya blob hizmetinde parallelOperationThreadCount kullanın.
* C++: Blob_request_options::set_parallelism_factor yöntemi kullanın.

#### <a name="subheading22"></a>Hızlı bir şekilde birçok blobları karşıya yükleme
Hızlı bir şekilde birçok blobları karşıya yüklemek için BLOB'ları paralel karşıya yükleyin. Bu, depolama hizmetinin birden çok bölümde karşıya yükleme yayılan çünkü tek bloblar aynı anda paralel blok karşıya karşıya daha hızlıdır. Tek bir blob aktarım hızı 60 MB/saniye (yaklaşık 480 Mb/sn) yalnızca destekler. Makalenin yazıldığı sırada bir ABD bankasına bağlı LRS hesabına daha fazladır tek bir blob tarafından desteklenen üretilen iş 20 GB/sn giriş kadar destekler.  [AzCopy](#subheading18) yüklemeleri varsayılan olarak paralel olarak gerçekleştirir ve bu senaryo için önerilir.  

### <a name="subheading23"></a>Blob doğru türünü seçme
Azure depolama blob iki tür destekler: *sayfa* blobları ve *blok* blobları. Belirli kullanım senaryosu için seçtiğiniz blob türü, performans ve ölçeklenebilirlik çözümünüzün etkiler. Blok blobları, büyük miktarlarda verileri verimli bir şekilde karşıya yüklemek istediğiniz zaman uygundur: Örneğin, blob depolama alanına fotoğraf veya videoyu karşıya yüklemek bir istemci uygulaması gerekebilir. Sayfa blobları, rastgele yazmaları veriler üzerinde gerçekleştirmek uygulamanın gerekiyorsa uygun: Örneğin, Azure VHD'leri sayfa blobları depolanır.  

Daha fazla bilgi için [anlama blok Blobları, ekleme Blobları ve sayfa Blobları](https://msdn.microsoft.com/library/azure/ee691964.aspx).  

## <a name="tables"></a>Tablolar
Kendini kanıtlamış yöntemleri için ek olarak [tüm hizmetleri](#allservices) daha önce açıklandığı gibi başarısı kanıtlanmış yöntemler aşağıdaki tabloda hizmete özellikle uygulayın.  

### <a name="subheading24"></a>Tablo özgü ölçeklenebilirlik hedefleri
Ek olarak bir depolama hesabının tamamını bant genişliği sınırlamaları tabloların aşağıdaki belirli ölçeklenebilirlik sınırı vardır.  Sistem, trafiği arttıkça Yük Dengelemesi, ancak ani artışları trafiğiniz varsa, bu aktarım hızı birimi hemen almak mümkün olmayabilir.  Desen ani artışlar içeriyorsa, azaltma ve/veya zaman aşımları aşırı depolama sırasında otomatik olarak yük dengeleyen tablonuzun kullanıma hizmet görmeyi beklemelisiniz.  Yukarı yavaş genellikle ramping uygun şekilde yük dengelemesi için sistem saatini verir gibi daha iyi sonuçlar sahiptir.  

#### <a name="entities-per-second-account"></a>Varlıkları (hesap) saniye başına
Tabloları erişmek için ölçeklenebilirlik sınırı kadar 20.000 varlıklar (1 KB her) başına ikinci bir hesabı içindir.  Genel olarak, eklenen her varlık güncelleştirildi, silinen veya bu hedef doğru sayar taranır.  Bu nedenle 100 varlık içeren bir toplu yerleştirme, 100 varlık sayılacaktır.  1000 varlıkları tarar ve 5 döndüren bir sorgu 1000 varlıklar olarak sayılacaktır.  

#### <a name="entities-per-second-partition"></a>Varlıkları / saniye (bölüm)
Tek bir bölüm içinde tablolar erişmek için ölçeklenebilirlik 2.000 varlıklar (1 KB her) önceki bölümde açıklandığı gibi aynı sayım kullanarak saniyede hedefidir.  

### <a name="configuration"></a>Yapılandırma
Bu bölümde, tablo hizmetinde önemli performans geliştirmeleri yapmak için kullanabileceğiniz birkaç Hızlı yapılandırma ayarları listelenmiştir:  

#### <a name="subheading25"></a>JSON kullanın
Depolama hizmeti sürüm 2013-08-15 ile başlayarak, tablo hizmeti JSON XML tabanlı AtomPub biçimi yerine tablo verilerini aktarmak için kullanarak destekler. Bu, yük boyutları olarak 75 oranında azaltabilir ve uygulamanızın performansını önemli ölçüde artırabilir.

Daha fazla bilgi için gönderiye bakın [Microsoft Azure tabloları: JSON ile tanışın](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/05/windows-azure-tables-introducing-json.aspx) ve [tablo hizmeti işlemleri için yükü biçimi](https://msdn.microsoft.com/library/azure/dn535600.aspx).

#### <a name="subheading26"></a>Nagle kapalı
Nagle'nın algoritması, yaygın olarak TCP/IP ağları arasında ağ performansını artırmak için bir yol uygulanır. Ancak, tüm durumlarda (örneğin, yüksek oranda etkileşimli ortamları) uygun değil. Azure depolama için Nagle'nın algoritması, tablo ve kuyruk hizmetlere yönelik istekler, performans üzerinde olumsuz bir etkisi yoktur. ve mümkünse devre dışı.

### <a name="schema"></a>Şema
Nasıl temsil eder ve verilerinizi sorgulayın, tablo Hizmeti performansını etkileyen en büyük tek faktördür. Her uygulama farklı olsa da, bu bölümde ile ilgili bazı genel kendini kanıtlamış yöntemleri açıklar:  

* Tablo Tasarımı
* Verimli sorgular
* Verimli veri güncelleştirmeleri  

#### <a name="subheading27"></a>Tablolar ve bölümler
Tablolar bölümlere ayrılır. Bir bölüm içinde depolanan her varlık aynı bölüm anahtarına paylaşır ve bu bölüm içinde tanımlamak için benzersiz bir satır anahtarına sahiptir. Bölümler avantajları sağlar, ancak sıra ölçeklenebilirlik sınırları uygular.  

* Avantajlar: En fazla 100 ayrı depolama işlemleri (4 MB Toplam boyut sınırı) içeren bir tek atomic, toplu işlem aynı bölümde varlıklarda güncelleştirebilirsiniz. Alınacak varlıkları aynı sayıda varsayıldığında, ayrıca tek bir bölüm içindeki verileri kapsayan (ancak tablo verilerini sorgulama hakkında daha fazla öneri için okumaya devam edin) bölümleri verileri daha verimli sorgulayabilirsiniz.
* Ölçeklenebilirlik sınırı: Tek bir bölüm içinde depolanan varlıklara erişimi-atomik toplu işlem bölümleri tarafından desteklemediklerinden dengeli yük olamaz. Bu nedenle, tek tek tablo bölümü için ölçeklenebilirlik hedefine bir bütün olarak için tablo hizmeti daha düşüktür.  

Tablolar ve bölümler bu özelliklerini nedeniyle, aşağıdaki tasarım ilkeleri benimseyin:  

* İstemci uygulamanızı sık güncelleştirilen veya iş aynı mantıksal biriminde sorgulanan veriler aynı bölümde yer almalıdır.  Bu, uygulamanızı yazma araya getirdiği anlamına gelir veya atomik toplu işlemleri yararlanmak istediğinden olabilir.  Ayrıca, tek bir bölüm verileri daha verimli bir şekilde tek bir sorguda veri bölümler arasında sorgulanabilir.
* İstemci uygulamanızı değil ekleme/güncelleştirme verileri veya sorgu iş (tek bir sorgu veya toplu güncelleştirme) aynı mantıksal biriminde ayrı bölümlerde yer almalıdır.  Önemli bir Not, bir sorun değildir ve performansı etkilenmez bölüm anahtarları milyonlarca sahip sınırsız sayıda bölüm anahtarı tek bir tabloda olduğunu ' dir.  Uygulamanızın kullanıcı oturum açma ile popüler bir Web sitesi ise, örneğin, bölüm anahtarı olarak kullanıcı Kimliğini kullanarak iyi bir seçim olabilir.  

#### <a name="hot-partitions"></a>Etkin bölümler
Sık erişimli bir bölüm trafiğin bir hesaba orantısız bir yüzde alan biridir ve tek bir bölüm olduğundan, yük dengeli olamaz.  Genel olarak, etkin bölümler iki yoldan biriyle oluşturulur:  

##### <a name="subheading28"></a>Yalnızca ekleme ve yalnızca desenleri önüne ekleyin
"Yalnızca ekleme" burada verilen PK trafiği tüm (veya neredeyse tüm) artırır ve azaltır göre geçerli zaman bir desendir.  Uygulamanız için günlük verileri bölüm anahtarı olarak geçerli tarihi kullandıysanız bir örnektir.  Bu tüm giderek ekler son bölüm tablonuzdaki olur ve tüm yazma tablonun sonuna kadar giderek çünkü sistem Bakiye yüklenemiyor.  Bu bölüm için trafik bölüm düzeyinde ölçeklenebilirlik hedefine aşarsa, azaltma sonuçlanır.  Trafiği tablonuzun arasında istekleri birden fazla bölüme yük dengelemeyi etkinleştirmek için gönderilir emin olmak daha iyidir.  

##### <a name="subheading29"></a>Yüksek trafik verileri
Bölümleme düzeninizi yalnızca diğer bölümler çok fazla kullanılan verileri içeren tek bir bölüm sonuçlanırsa, bölüm tek bir bölüm için ölçeklenebilirlik hedefine yaklaşıyor olarak azaltma de görebilirsiniz.  Bölümleme düzeninizi hiçbir tek bölüm ölçeklenebilirlik hedefleri yaklaşan sonuçları emin olmak daha iyidir.  

#### <a name="querying"></a>Sorgulama
Bu bölümde, tablo hizmeti sorgulamak için kendini kanıtlamış yöntemleri açıklar.  

##### <a name="subheading30"></a>Sorgu kapsamı
Sorgu varlıklara aralığını belirtmek için çeşitli yollar vardır.  Her biri hakkında ayrıntılı bilgi verilmiştir.  

Genel olarak, taramalar (sorgular tek bir varlık büyük) kaçının, ancak tarama gerekir, böylece, taramalar tarama veya önemli miktarda ihtiyacınız olmayan varlıklar döndürüyor ihtiyacınız olan verileri alma, verilerinizi düzenlemek deneyin.  

###### <a name="point-queries"></a>Nokta sorgu
Bir nokta sorgu tam olarak bir varlığı alır. Bölüm anahtarını ve satır anahtarı almak için varlık belirterek bunu yapar. Bu sorguların verimli olması ve mümkünse kullanmanız gerekir.  

###### <a name="partition-queries"></a>Bölüm sorguları
Bölüm sorgusu ortak bir bölüm anahtarı paylaşan veri kümesi alır bir sorgudur. Genellikle, sorgu, bir dizi satır anahtarı değerlerinin veya ek bir bölüm anahtarı olarak bazı varlık özelliği için değer aralığını belirtir. Bu nokta sorgu daha az verimlidir ve kullanılmamalıdır.  

###### <a name="table-queries"></a>Tablo sorguları
Tablo sorgusu, ortak bir bölüm anahtarı paylaşmaz varlık kümesini alır bir sorgudur. Bu sorguları verimli değildir ve mümkünse kaçınmanız gerekir.  

##### <a name="subheading31"></a>Sorgu yoğunluğu
Sorgu verimliliği başka bir anahtarı faktörünü döndürülen kümesi bulmak için taranan varlıkların sayısı karşılaştırıldığında döndürülen varlıkları sayısıdır. Uygulamanızı bir tablo sorgusu yalnızca %1 veri paylaşımları, 100 varlık döndürdüğü her bir varlık için sorgu tarama bir özellik değeri için bir filtre ile uyguluyorsa. Ele alınan tablo ölçeklenebilirlik hedefleri daha önce tüm taranan varlıkların sayısı ve döndürülen varlık sayısını ilişkilendirebilirsiniz: düşük sorgusunu yoğunluk kolayca tablo hizmeti, çok sayıda varlıklara tarama gerekir çünkü, uygulamanın kısıtlanmasına neden olabilir Aradığınız varlığı alır.  Aşağıdaki bölümde bkz üzerinde [normalleştirilmişlikten çıkarma](#subheading34) bu durumu önlemek nasıl daha fazla bilgi için.  

##### <a name="limiting-the-amount-of-data-returned"></a>Döndürülen veri miktarını sınırlama
###### <a name="subheading32"></a>Filtreleme
Sorgu istemci uygulamasına ihtiyacınız olmayan varlıkları döndürme bildiğiniz durumlarda, döndürülen kümesinin boyutunu azaltmak için bir filtre kullanan göz önünde bulundurun. İstemciye döndürülen değil varlık hala doğru ölçeklenebilirlik sınırları sayısı, ancak daha düşük ağ yükü boyutu ve daha az istemci uygulamanızı işlemelisiniz varlıkların sayısı nedeniyle uygulama performansınızı artırır.  Not bakın [sorgusunu yoğunluk](#subheading31), ancak – ölçeklenebilirlik hedefleri taranan varlıkların sayısı için ilişkili şekilde birkaç varlıkları döndürülen bile çok varlık filtreleyen sorgu hala kısıtlama, neden olabilir.  

###### <a name="subheading33"></a>Projeksiyon
Tablonuzdaki varlıkları özelliklerinden yalnızca sınırlı sayıda istemci uygulamanız gerekiyorsa, döndürülen veri kümesinin boyutunu sınırlamak için yansıtma kullanabilirsiniz. Sahip olarak filtreleme, bu ağ yükü ve işleme istemci azaltmaya yardımcı olur.  

##### <a name="subheading34"></a>Normalleştirilmişlikten çıkarma
İlişkisel veritabanları ile çalışma aksine, verimli bir şekilde tablo verilerini sorgulama için kendini kanıtlamış verilerinizi normal durumdan çıkarmayı sağlama. Diğer bir deyişle, aynı verileri birden çok varlık (verileri bulmak üzere kullanabilir her anahtar için bir tane) çoğaltma istemci verileri bulmak üzere bir sorgu taraması varlık sayısını en aza indirmek için gerekli varlıkları uygulamanıza verileri bulmak için çok sayıda taramak zorunda yerine Uygulama gerekir.  Örneğin, bir e-ticaret Web sitesinde bir sipariş hem müşteri Kimliğine göre bulmak istediğiniz (bu müşterinin sipariş ver) ve tarihe göre (bir tarihte sipariş ver).  Tablo depolamada varlık (veya buna bir başvuru) depolamak en iyi iki kez – bir kez tablo adıyla PK ve ağ Yazıcılarına müşteri tarafından bulma kolaylaştırmak için kimlik, bir kez tarihe göre bulma kolaylaştırmak için.  

#### <a name="insertupdatedelete"></a>Ekleme/güncelleştirme/silme
Bu bölümde, tablo hizmetinde saklanan varlıkları değiştirmek için kendini kanıtlamış yöntemleri açıklar.  

##### <a name="subheading35"></a>Toplu işleme
Toplu işlem, Azure Depolama'daki varlık grubu işlemleri (ETG) olarak bilinir; bir ETG içindeki tüm işlemler tek bir tablodaki tek bir bölüm olmalıdır. Mümkünse, ETGs toplu olarak ekleme, güncelleştirme ve silme gerçekleştirmek için kullanın. Bu istemci uygulamanızın sunucuya gidiş dönüş sayısını azaltan, (bir ETG faturalama amacıyla tek bir işlem olarak sayılır ve 100'e kadar depolama işlemleri içerebilir) Faturalanabilir işlem sayısını azaltır ve atomik güncelleştirmeleri (tümü sağlar işlemleri başarılı veya içinde bir ETG tümü başarısız). Mobil cihazlar gibi yüksek gecikme süreleriyle ortamları ETGs kullanımından büyük ölçüde avantaj sağlayacaktır.  

##### <a name="subheading36"></a>Upsert
Kullanımı tablo **Upsert** işlemlerini mümkün olan her yerde. İki tür vardır **Upsert**, ikisi için de geleneksel bir daha verimli olabilir **Ekle** ve **güncelleştirme** işlemleri:  

* **InsertOrMerge**: Bir varlığın özellik alt kümesi karşıya yüklemek istediğiniz, ancak emin değilseniz varlık zaten var olup, bunu kullanın. Varlık varsa, bu çağrı dahil özelliklerini güncelleştirir. **Upsert** işlemi ve tüm mevcut özellikler çıktığında varlık mevcut değilse, olduğu gibi yeni bir varlık ekler. Değiştirmekte olduğunuz özelliklerini yüklemek yalnızca gerektiren bu sorguda projeksiyon kullanmaya benzer.
* **Insertorreplace**: Tamamen yeni bir varlık karşıya yüklemek istediğiniz, ancak zaten var olup olmadığını emin değilseniz bu kullanın. Yeni yüklenen varlık tamamen eski varlık yazdığından tamamen doğru olduğunu bildiğiniz durumlarda yalnızca bu kullanmanız gerekir. Örneğin, uygulama daha önce depolanan kullanıcı konum verileri olup olmadığına bakılmaksızın, bir kullanıcının geçerli konumuna depolar varlık güncelleştirmek istediğiniz; Yeni bir konum varlık tamamlandıktan ve önceki herhangi bir varlık bilgileri gerekmez.

##### <a name="subheading37"></a>Veri serisi içinde tek bir varlık depolama
Bazı durumlarda, bir uygulamayı sık tümünü tek seferde almak için gereken verileri bir dizi depolar: Örneğin, bir uygulamanın CPU kullanım zaman içinde son 24 saat verileri sıralı bir grafiği çizmek için izleyebilir. Belirli bir saati temsil eden ve CPU kullanımı bu saat için depolama her bir varlık ile bir tablo varlık, saatlik bir yaklaşımdır. Bu verileri çizmek için uygulamanın en son 24 saatten verileri tutan varlıkları alma gerekir.  

Uygulamanızı ayrı özelliği tek bir varlık olarak her saat için alternatif olarak, CPU kullanımı depolayabilir: her saat güncelleştirmek için uygulamanız tek bir kullanabilir **InsertOrMerge Upsert** değeri için en son güncelleştirmeye çağrısı saat. Veri çizimi için uygulamanın yalnızca 24, verimli bir sorgu için yapmak yerine tek bir varlık alma gerekir (tartışmaya bakın [query kapsamın](#subheading30)).

##### <a name="subheading38"></a>Bloblar yapılandırılmış verileri depolama
Bazen yapılandırılmış verileri tablolarda, gitmesi gereken ancak aralıkları varlıklar her zaman birlikte alınır ve batch eklenebilir gibi hissettirir.  Bunun iyi bir örnek günlük dosyasıdır.  Bu durumda, birkaç dakikadan günlükler batch, bunları eklemek ve ardından, her zaman aynı anda de günlüklerinin birkaç dakika alıyor.  Bu durumda, performans için yazılan /, genellikle yapılan gereken isteklerinin sayısı yanı sıra döndürülen nesne sayısını önemli ölçüde azaltabilirsiniz beri tabloları yerine, BLOB'ları kullanmak en iyisidir.  

## <a name="queues"></a>Sıralar
Kendini kanıtlamış yöntemleri için ek olarak [tüm hizmetleri](#allservices) daha önce açıklandığı gibi aşağıdaki başarısı kanıtlanmış yöntemler özellikle sıra hizmete uygulayın.  

### <a name="subheading39"></a>Ölçeklenebilirlik sınırları
Tek bir sıra (burada bir ileti olarak her AddMessage GetMessage ve DeleteMessage sayısı) saniyede yaklaşık 2.000 ileti (1 KB her) işleyebilir. Bu, uygulamanız için yetersiz kalırsa, birden fazla kuyruk kullanan ve iletileri bunların arasında yaymaktır gerekir.  

En geçerli ölçeklenebilirlik hedeflerini görüntülemek [Azure Storage ölçeklenebilirlik ve performans hedefleri](storage-scalability-targets.md).  

### <a name="subheading40"></a>Nagle kapalı
Nagle algoritması ele tablo yapılandırma bölümüne bakın: Nagle algoritmasıdır sıra isteklerini performansı için genellikle hatalı ve devre dışı bırakmalısınız.  

### <a name="subheading41"></a>İleti boyutu
İleti boyutu arttıkça, kuyruk performans ve ölçeklenebilirlik azalır. Bir ileti yalnızca bir alıcı gerekli bilgileri yerleştirmeniz gerekir.  

### <a name="subheading42"></a>Batch alma
Tek bir işlemde bir kuyruktan 32 adede kadar iletileri alabilirsiniz. Bu gidiş-dönüş sayısı, mobil cihazlar gibi ortamlarda kullanışlıdır istemci uygulamadan alınan yüksek gecikme süresiyle azaltabilir.  

### <a name="subheading43"></a>Kuyruk yoklama aralığı
Çoğu uygulama için uygulama işlemlerinin büyük kaynaklardan biri olabilir bir kuyruktan ileti için yoklama. Akıllıca, yoklama aralığı seçin: sıra için ölçeklenebilirlik hedefleri yaklaşım uygulamanız çok sık yoklama neden olabilir. Ancak, (yazma sırasında) $0,01 için 200.000 işlemleri bir ay boyunca her saniye kadar maliyet küçüktür 15 Sent maliyeti sonra yoklama tek bir işlemcinin genellikle yoklama aralığı tercih ettiğiniz etkileyen bir faktör değildir.  

Maliyet güncel bilgi için bkz: [Azure depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).  

### <a name="subheading44"></a>UpdateMessage
Kullanabileceğiniz **UpdateMessage** görünmezlik zaman aşımı süresini artırın veya iletinin durum bilgilerini güncelleştirmek için. Bu güçlü olmakla birlikte, her unutmayın **UpdateMessage** ölçeklenebilirlik hedef işlem sayılır. Ancak, bu işin her adımı tamamlandı olarak bir iş, bir kuyruktan yanında, geçirir. bir iş akışı sahip daha çok daha etkili bir yaklaşım olabilir. Kullanarak **UpdateMessage** işlemi iletiyi iş durumu Kaydet ve bir adım tamamlanan her zaman yapılacak işin bir sonraki adım için requeuing yerine çalışma, devam etmek, uygulamanızın sağlar.  

Daha fazla bilgi için bkz [nasıl yapılır: Kuyruğa Alınan iletinin içeriğini değiştirme](../queues/storage-dotnet-how-to-use-queues.md#change-the-contents-of-a-queued-message).  

### <a name="subheading45"></a>Uygulama mimarisi
Uygulama Mimarinizi ölçeklenebilir hale getirmek için kuyrukları kullanmanız gerekir. Kuyruklar, uygulamanızın daha ölçeklenebilir hale getirmek için kullanabileceğiniz bazı yollarını listeler:  

* İşleme iş biriktirme listeleri oluşturun ve uygulamanızda iş yüklerinin ölçeğini düzgün kuyruklarını kullanabilirsiniz. Örneğin, karşıya yüklenen görüntüleri yeniden boyutlandırmayı gibi işlemci yoğunluklu işlerini yapmak için kullanıcılardan gelen istekleri oluşturan kuyruğa alınamadı.
* Kuyruklar, uygulamanızın parçalarını birbirinden bağımsız şekilde ölçeklenebilmek böylece ayırmak için kullanabilirsiniz. Örneğin, bir web ön ucu, kullanıcıların anket sonuçlarını sonraki çözümleme ve depolama için bir kuyruğun içine koyabilir. Kuyruk verileri gereken şekilde işlemek için daha fazla çalışan rolü örnekleri ekleyebilirsiniz.  

## <a name="conclusion"></a>Sonuç
Bu makalede başarısı kanıtlanmış uygulamalar, Azure depolama kullanırken performansı iyileştirmek için en yaygın bazıları açıklanmıştır. Biz, her uygulama geliştiricisine, yukarıda belirtilen yöntemlerin her biri ile uygulamalarını değerlendirmelerini ve Azure Depolama kullanan uygulamalarından mükemmel performans alma önerilerini dikkate almalarını öneririz.
