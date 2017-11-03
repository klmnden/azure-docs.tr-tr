---
title: "Azure depolama performans ve ölçeklenebilirlik Yapılacaklar listesi | Microsoft Docs"
description: "Azure Storage ile kullanıcı uygulamaları geliştirme kullanılmak kanıtlanmış yöntemleri listesi."
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 959d831b-a4fd-4634-a646-0d2c0c462ef8
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2016
ms.author: tamram
ms.openlocfilehash: 6f5a136d1be7a4bb4093baad820271770305b718
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="microsoft-azure-storage-performance-and-scalability-checklist"></a>Microsoft Azure Depolama Performansı ve Ölçeklenebilirlik Onay Listesi
## <a name="overview"></a>Genel Bakış
Microsoft Azure Storage Hizmetleri sürümü,'den itibaren Microsoft bu hizmetleri bir kullanıcı şekilde kullanmak için kanıtlanmış uygulamalar sayısı geliştirmiştir ve bunların en önemli bir denetim listesi stili listesine birleştirmek için bu makalede hizmet. Bu makalede amacı uygulama geliştiricileri kanıtlanmış yöntemleri Azure Storage ile kullandığınızdan emin olun yardımcı olmak ve benimsemeyi düşünmelisiniz kanıtlanmış diğer yöntemler tanımlamaya yardımcı olmak için ' dir. Bu makalede her olası performans ve ölçeklenebilirlik iyileştirme kapsayacak şekilde denemez — etkilerini küçük veya geniş çapta geçerli olanlar hariç tutar. Uygulamanın davranışını tasarım sırasında öngörülebiliyorsa toplasa bile, bunları önceden performans sorunlarla çalışacak tasarımları önlemek için göz önünde tutmak kullanışlıdır.  

Azure Storage kullanarak her uygulama geliştiricisinin bu makaleyi okuyun ve kendi uygulama her aşağıda listelenen kanıtlanmış uygulamalarının izlediğini denetleyin zaman.  

## <a name="checklist"></a>Denetim listesi
Bu makalede kanıtlanmış yöntemler aşağıdaki gruplar halinde düzenler. Kanıtlanmış yöntemleri için geçerlidir:  

* Tüm Azure Storage Hizmetleri (BLOB'lar, tablolar, kuyruklar ve dosyaları)
* Bloblar
* Tablolar
* Kuyruklar  

| Bitti | Alan | Kategori | Soru |
| --- | --- | --- | --- |
| &nbsp; | Tüm Hizmetler |Ölçeklenebilirlik hedefleri |[Uygulamanızı ölçeklenebilirlik hedefleri yaklaşan önlemek üzere tasarlanmıştır?](#subheading1) |
| &nbsp; | Tüm Hizmetler |Ölçeklenebilirlik hedefleri |[Adlandırma kuralınızın daha iyi Yük Dengeleme sağlamak için tasarlanmıştır?](#subheading47) |
| &nbsp; | Tüm Hizmetler |Ağ |[İstemci tarafı aygıtları yeterince yüksek bant genişliği ve gerekli performans elde etmek için düşük gecikme süresi var mı?](#subheading2) |
| &nbsp; | Tüm Hizmetler |Ağ |[İstemci tarafı aygıtları yeterince yüksek kaliteli bağlantı var mı?](#subheading3) |
| &nbsp; | Tüm Hizmetler |Ağ |[İstemci uygulaması "yakın" depolama hesabı bulunuyor?](#subheading4) |
| &nbsp; | Tüm Hizmetler |İçerik Dağıtımı |[İçerik dağıtımı için bir CDN kullanıyor musunuz?](#subheading5) |
| &nbsp; | Tüm Hizmetler |Doğrudan istemci erişimi |[Depolama proxy yerine doğrudan erişmesine izin vermek için SAS ve CORS kullanıyor musunuz?](#subheading6) |
| &nbsp; | Tüm Hizmetler |Önbelleğe alma |[Sürekli olarak kullanılan uygulama verileri önbelleğe alma ve değişiklikleri nadiren mi?](#subheading7) |
| &nbsp; | Tüm Hizmetler |Önbelleğe alma |[Uygulamanız (istemci tarafında önbelleğe alma ve daha büyük kümeleri karşıya) güncelleştirmeleri yığınlama?](#subheading8) |
| &nbsp; | Tüm Hizmetler |.NET yapılandırması |[Yeterli sayıda eşzamanlı bağlantı kullanmak için istemci yapılandırdınız mı?](#subheading9) |
| &nbsp; | Tüm Hizmetler |.NET yapılandırması |[İş parçacığı yeterli sayıda kullanmak için .NET yapılandırdınız mı?](#subheading10) |
| &nbsp; | Tüm Hizmetler |.NET yapılandırması |[.NET 4.5 kullanıyor veya daha sonra hangi çöp toplama geliştirilmiştir?](#subheading11) |
| &nbsp; | Tüm Hizmetler |Paralellik |[Böylece istemci yeteneklerinizi veya ölçeklenebilirlik hedefleri aşırı yükleme yok paralellik uygun şekilde sınırlıdır sağlamış olursunuz?](#subheading12) |
| &nbsp; | Tüm Hizmetler |Araçlar |[Microsoft en son sürümünü kullanarak istemci kitaplıkları ve araçlar sağlanır?](#subheading13) |
| &nbsp; | Tüm Hizmetler |Yeniden deneme sayısı |[Üstel geri alma kullanarak yeniden deneme ilkesi hataları ve zaman aşımlarına azaltma için misiniz?](#subheading14) |
| &nbsp; | Tüm Hizmetler |Yeniden deneme sayısı |[Uygulama kaçınarak yeniden deneme denenemeyen hataları mı?](#subheading15) |
| &nbsp; | Bloblar |Ölçeklenebilirlik hedefleri |[Çok sayıda istemci aynı anda tek bir nesne erişimi var mı?](#subheading46) |
| &nbsp; | Bloblar |Ölçeklenebilirlik hedefleri |[Uygulamanızın tek bir blob için bant genişliği veya işlemleri ölçeklenebilirlik hedef içinde kaldığını?](#subheading16) |
| &nbsp; | Bloblar |BLOB kopyalama |[Kopyalama BLOB'lar verimli bir şekilde misiniz?](#subheading17) |
| &nbsp; | Bloblar |BLOB kopyalama |[BLOB'lar için toplu kopyalarını AzCopy kullanıyor musunuz?](#subheading18) |
| &nbsp; | Bloblar |BLOB kopyalama |[Çok büyük miktarda veri aktarmak için Azure içeri/dışarı aktarma kullanıyor musunuz?](#subheading19) |
| &nbsp; | Bloblar |Meta veri kullanın |[BLOB'ları hakkında sık kullanılan meta verileri, meta verilerde depoluyorsanız?](#subheading20) |
| &nbsp; | Bloblar |Hızlı yükleme |[Hızla bir blob yüklenmeye çalışılırken blokları paralel karşıya yükleniyor?](#subheading21) |
| &nbsp; | Bloblar |Hızlı yükleme |[Çok sayıda BLOB'ları hızlı bir şekilde yüklenmeye çalışılırken BLOB'ları paralel karşıya yükleniyor?](#subheading22) |
| &nbsp; | Bloblar |Doğru Blob türü |[Sayfa bloblarını veya blok blobları uygun olduğunda kullanıyor?](#subheading23) |
| &nbsp; | Tablolar |Ölçeklenebilirlik hedefleri |[Saniye başına varlıklar için ölçeklenebilirlik hedefleri yaklaştığı?](#subheading24) |
| &nbsp; | Tablolar |Yapılandırma |[JSON tablosu isteklerinizi kullanıyor musunuz?](#subheading25) |
| &nbsp; | Tablolar |Yapılandırma |[Nagle küçük istekleri performansını artırmak için devre dışı bırakmış?](#subheading26) |
| &nbsp; | Tablolar |Tabloları ve bölümleri |[Verilerinizi düzgün bir şekilde bölümlenmiş mi?](#subheading27) |
| &nbsp; | Tablolar |Sık kullanılan bölümleri |[Yalnızca ekleme ve yalnızca başına desenleri önleme?](#subheading28) |
| &nbsp; | Tablolar |Sık kullanılan bölümleri |[Ekleme/güncelleştirme birçok bölüm arasında yayılır mı?](#subheading29) |
| &nbsp; | Tablolar |Sorgu kapsamı |[Çoğu durumda kullanılması için noktası sorgular ve tutumlu kullanılacak tablo sorgular için izin vermek için şema tasarladığınız?](#subheading30) |
| &nbsp; | Tablolar |Sorgu yoğunluğu |[Uygulamanız kullanacak satırları döndürür ve sorguları genellikle yalnızca taramanız musunuz?](#subheading31) |
| &nbsp; | Tablolar |Sınırlama döndürülen veri |[Gerekli olmayan varlık döndüren önlemek için filtreleme kullanıyor?](#subheading32) |
| &nbsp; | Tablolar |Sınırlama döndürülen veri |[Gerekli olmayan özellikler döndüren önlemek için yansıtma kullanıyor musunuz?](#subheading33) |
| &nbsp; | Tablolar |Denormalization |[Veri almaya çalışırken Verimsiz sorgulara veya birden çok okuma isteklerinin kaçının, verilerinizi normal dışı mi?](#subheading34) |
| &nbsp; | Tablolar |Ekleme/güncelleştirme/silme |[Toplu işlem için gereken istekleri veya gidiş dönüş azaltmak için aynı anda yapılabilir misiniz?](#subheading35) |
| &nbsp; | Tablolar |Ekleme/güncelleştirme/silme |[Yalnızca ekleme veya güncelleştirme aramak karar vermek için bir varlık alma önleme?](#subheading36) |
| &nbsp; | Tablolar |Ekleme/güncelleştirme/silme |[Sık alınır veri serisi birden çok varlık yerine özellikleri olarak tek bir varlık birlikte depolama göz önünde bulundurduğunuzdan?](#subheading37) |
| &nbsp; | Tablolar |Ekleme/güncelleştirme/silme |[Her zaman birlikte alınır ve toplu (örn. zaman serisi veri) yazılabilir varlıklar için BLOB tabloları yerine kullanarak göz önünde bulundurduğunuzdan?](#subheading38) |
| &nbsp; | Kuyruklar |Ölçeklenebilirlik hedefleri |[Saniye başına ileti ölçeklenebilirlik hedefleri yaklaştığı?](#subheading39) |
| &nbsp; | Kuyruklar |Yapılandırma |[Nagle küçük istekleri performansını artırmak için devre dışı bırakmış?](#subheading40) |
| &nbsp; | Kuyruklar |İleti Boyutu |[Sıranın performansını artırmak için iletileri compact misiniz?](#subheading41) |
| &nbsp; | Kuyruklar |Toplu alma |[Tek bir işlemde "Get" birden çok ileti alma?](#subheading42) |
| &nbsp; | Kuyruklar |Yoklama sıklığı |[Uygulamanızın algılanan gecikme süresini azaltmak için yeterince sık yoklama?](#subheading43) |
| &nbsp; | Kuyruklar |Güncelleştirme iletisi |[İlerleme durumu iletilerini işleme bir hata oluşursa, tüm ileti yeniden işlemek zorunda önleme depolamak için UpdateMessage kullanıyor musunuz?](#subheading44) |
| &nbsp; | Kuyruklar |Mimari |[Uzun süre çalışan iş yükleri kritik yolunu dışında tutarak, tüm uygulama daha ölçeklenebilir yapmak ve bağımsız olarak ölçeklendirmek için kuyrukları kullanıyor musunuz?](#subheading45) |

## <a name="allservices"></a>Tüm hizmetler
Bu bölümde, herhangi bir Azure depolama hizmetleri (BLOB, tablo, kuyruk veya dosyaları) kullanmak için geçerli olan kanıtlanmış yöntemler listelenmiştir.  

### <a name="subheading1"></a>Ölçeklenebilirlik hedefleri
Azure Storage hizmetlerinin her biri, Kapasite (GB), işlem hızını ve bant genişliği için ölçeklenebilirlik hedefleri vardır. Uygulamanızı yaklaşıyor veya ölçeklenebilirlik hedefleri hiçbirini aşıyor, artan hareket gecikmeleri veya azaltma karşılaşabilirsiniz. Uygulamanızı bir depolama birimi hizmeti kısıtlar, hizmet "503 Sunucu meşgul" veya "500 işlem zaman aşımı" hata kodları bazı depolama işlemleri için geri dönmek başlar. Bu bölümde ölçeklenebilirlik hedefleri ve bant genişliği ölçeklenebilirlik hedefleri özellikle ilgili her iki genel yaklaşım açıklanmaktadır. Tek depolama hizmetleri ile ilgili sonraki bölümlerde bu belirli hizmet bağlamında ölçeklenebilirlik hedefleri açıklanmaktadır:  

* [BLOB bant genişliği ve saniye başına istek sayısı](#subheading16)
* [Saniye başına tablo varlıkları](#subheading24)
* [İletileri / saniye](#subheading39)  

#### <a name="sub1bandwidth"></a>Tüm hizmetler için bant genişliği ölçeklenebilirlik hedefi
Yazma zaman ABD coğrafi olarak yedekli depolama (GRS) hesabı için bant genişliği hedefleri 10 Gigabit / saniye (Gbps) (depolama hesabınıza gönderilen veriler) giriş ve çıkış (depolama hesabından gönderilen veri) için 20 GB/sn içindir. Sınırları için bir yerel olarak yedekli depolama (LRS) hesabı, daha yüksek – 20 GB/sn giriş ve çıkışı için 30 GB/sn.  Uluslararası bant genişliği sınırlarını daha düşük olabilir ve bulunabilir bizim [ölçeklenebilirlik hedefleri sayfa](http://msdn.microsoft.com/library/azure/dn249410.aspx).  Bağlantıları Depolama artıklık seçenekleri hakkında daha fazla bilgi için bkz: [kullanışlı kaynaklar](#sub1useful) aşağıda.  

#### <a name="what-to-do-when-approaching-a-scalability-target"></a>Ölçeklenebilirlik hedefe yaklaştığını ne yapacakları
Uygulamanızın tek bir depolama hesabı ölçeklenebilirlik hedefleri yaklaşıyorsa aşağıdaki yaklaşımlardan birini benimsenmesi göz önünde bulundurun:  

* Uygulamanızın yaklaşan veya ölçeklenebilirlik hedef aşan neden olan iş yükü alan. Daha az bant genişliği veya kapasite ya da daha az işlemleri farklı şekilde kullanacak şekilde tasarlayabilirsiniz?
* Bir uygulama ölçeklenebilirlik hedefleri birini aşmalıdır, uygulama verilerinizi birden çok depolama hesapları ve bölüm arasında birden çok bu depolama hesabı oluşturmanız gerekir. Bu deseni kullanır, böylece Yük Dengeleme için daha fazla depolama hesapları gelecekte ekleyebilirsiniz uygulamanızı tasarlayın emin olun. Yazıldığı sırada her Azure aboneliğinin en fazla 100 depolama hesapları olabilir.  Depolama hesapları kullanımınızı depolanan veriler, yapılan işlemleri ya da aktarılan verilerin bakımından dışındaki herhangi bir ücret de.
* Uygulamanızı bant hedefleri değerse, depolama hizmetine veri göndermek için gereken bant genişliğini azaltmak üzere istemci verileri sıkıştırmayı göz önünde bulundurun.  Bu bant genişliğinden tasarruf ve ağ performansını iyileştirmeye olsa da, bu da bazı olumsuz etkiler olabileceğini unutmayın.  Bu performans etkisi sıkıştırma ve veri istemcisinde boyutunda ek işleme gereksinimleri nedeniyle değerlendirmelisiniz. Ayrıca, sıkıştırılmış veri depolama daha standart araçlarını kullanarak depolanan verileri görüntülemek daha zor olabilir beri sorunları gidermek zorlaştırabilir.
* Uygulamanızı ölçeklenebilirlik hedefleri değerse, ardından yeniden deneme için üstel geri alma kullandığınızdan emin olun (bkz [yeniden deneme](#subheading14)).  Hiçbir zaman (yukarıdaki yöntemlerden birini kullanarak) ölçeklenebilirlik hedefleri yaklaşımını, ancak bu, uygulamanızın yalnızca hızlı bir şekilde, da kötüsü azaltma yapmadan denemeye devam olmaz sağlayacak emin olmak daha iyidir.  

#### <a name="useful-resources"></a>Yararlı Kaynaklar
Aşağıdaki bağlantılar ölçeklenebilirlik hedefleri hakkında ek ayrıntılı bilgi sağlar:

* Bkz: [Azure Storage ölçeklenebilirlik ve performans hedefleri](storage-scalability-targets.md) ölçeklenebilirlik hedefleri hakkında bilgi için.
* Bkz: [Azure Storage çoğaltma](storage-redundancy.md) ve blog postası [Azure Depolama artıklık seçenekleri ve okuma erişimli coğrafi olarak yedekli depolama](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx) Depolama artıklık seçenekleri hakkında bilgi.
* Azure Hizmetleri için fiyatlandırma hakkında güncel bilgiler için bkz: [Azure fiyatlandırma](https://azure.microsoft.com/pricing/overview/).  

### <a name="subheading47"></a>Bölüm adlandırma kuralı
Azure depolama bir aralık tabanlı bölümleme düzeni ölçek ve Yük Dengeleme sistemi için kullanır. Bölüm anahtarı, aralıkları ve bu aralıklar verisine sistem üzerindeki dengeli bölüm için kullanılır. Bu sözcük sıralama (örneğin msftpayroll, msftperformance, msftemployees, vb.) gibi adlandırma kuralları anlamına gelir veya zaman damgalarını (log20160101, log20160102, log20160102, vb.) kullanarak kendi potansiyel olarak aynı bölüm sunucuda birlikte bulunuyor bölümleri ödünç vermek, kadar bir yük dengeleme işlemi bunları küçük aralıkları böler. Örneğin, daha fazla bölüm aralıklarını dengelenmesi bu BLOB'lar üzerindeki yükü gerektirir kadar bir kapsayıcıdaki tüm blob'lara tek bir sunucu tarafından sunulabilen. Benzer şekilde, bir sözcük sırayla düzenlenmiş adları ile hafifçe yüklü hesap grubunun tek bir sunucu tarafından bir yükü kadar hizmet edilebilir veya bu hesapların tümü birden çok bölüm sunucular arasında bölünmesi gerekir. Her yük dengeleme işlemi depolama çağrıları gecikme işlemi sırasında etkileyebilir. İşlemi Yük Dengeleme kicks bileşenini ve bölüm anahtarı aralığının yeniden dengeler kadar tek bölüm sunucu ölçeklenebilirliğini tarafından sistemin bir bölüm için trafiği ani bir veri bloğu işleme yeteneği sınırlıdır.  

Bu tür işlemler sıklığını azaltmak için bazı en iyi uygulamalar izleyebilirsiniz.  

* Hesapları, kapsayıcıları, BLOB'lar, tablolar ve Kuyruklar, yakından kullanmak adlandırma kuralını inceleyin. Gereksinimlerinize en uygun karma bir işlevi kullanarak 3 basamaklı karma ile hesap adlarını önek göz önünde bulundurun.  
* Zaman damgaları veya sayısal tanımlayıcıları kullanarak verilerinizi düzenlemek, yalnızca ekleme (veya yalnızca başına) trafiği desenlerini kullanmıyorsanız olmanız gerekir. Bu düzenleri için bir aralığı uygun olmayan-sistem bölümleme temel ve sağlama için tek bir bölüm giden veya sistemden etkili bir şekilde sınırlama tüm trafiği için Yük Dengeleme. Yyyyaagg gibi bir zaman damgasına sahip bir blob nesnesi kullanan günlük işlemler varsa, örneğin, sonra günlük bu işlem için tüm trafik için tek bölüm sunucusu tarafından sunulan tek bir nesne yönlendirilir. Ara mi blob sınırları başına ve bölüm başına sınırları ihtiyaçlarınızı karşılamak ve gerekirse bu işlemin birden çok BLOB bölmeyi düşünün. Zaman serisi veri tabloları depolarsanız, benzer şekilde, tüm trafik olabilir son anahtar ad alanının parçası yönlendirilmiş. Zaman damgaları veya sayısal kimlikleri kullanmanız gerekiyorsa, 3 basamaklı karma ile ya da zaman damgaları durumunda kimliği öneki ssyyyymmdd gibi zamanı saniye parçası önek. Listeleme ve işlemleri sorgulama düzenli olarak yapılan sorguları sayısı sınırlar bir karma işlevi belirleyin. Diğer durumlarda, rastgele bir önek yeterli olabilir.  
* Azure Storage'da kullanılan bölümleme şeması hakkında daha fazla bilgi okumak için SOSP belgesi [burada](http://sigops.org/sosp/sosp11/current/2011-Cascais/printable/11-calder.pdf).

### <a name="networking"></a>Ağ
Konuya API çağrılarının olsa da, genellikle uygulamanın fiziksel ağ kısıtlamalarını bir önemli performansı etkiler. Aşağıdaki sınırlamalar kullanıcılar karşılaşabilirsiniz bazıları açıklanmaktadır.  

#### <a name="client-network-capability"></a>İstemci ağ özelliği
##### <a name="subheading2"></a>Üretilen iş
Bant genişliği için genellikle istemci özelliklerini sorunudur. Örneğin, tek bir depolama hesabına 10 GB/sn veya daha fazla giriş işleyebileceği sırasında (bkz [bant genişliği ölçeklenebilirlik hedefleri](#sub1bandwidth)), bir "Küçük" Azure çalışan rolü örneği cinsinden ağ hızı yalnızca yaklaşık 100 MB/sn yeteneğine sahiptir. Daha büyük Azure örnekleri NIC'lerin daha büyük kapasitede vardır, yani tek bir makineden daha yüksek ağ sınırları ihtiyacınız varsa daha büyük bir örneği veya daha fazla VM'nin kullanmayı düşünmeniz gerekir. Bir depolama birimi hizmeti üzerinde bir şirket içi uygulamasından erişme sonra aynı kural geçerlidir: istemci cihazı ve ağ bağlantısını Azure depolama konumuna ağ yeteneklerini anlama ve ya da bunları gibi gerekli veya yeteneklerini içinde çalışmak için uygulamanızı tasarlayın geliştirin.  

##### <a name="subheading3"></a>Bağlantı kalitesi
Tüm ağ kullanımı olduğu gibi hataları ve paket kaybı ağ koşulları etkili verimlilik yavaşlatır unutmayın.  WireShark veya NetMon kullanarak bu sorunu tanılamalarına yardımcı olabilir.  

##### <a name="useful-resources"></a>Yararlı Kaynaklar
Sanal makine boyutları ve ayrılan bant hakkında daha fazla bilgi için bkz: [Windows VM boyutları](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) veya [Linux VM boyutları](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  

#### <a name="subheading4"></a>Konum
Dağıtılmış bir ortamda istemcinin sunucunun yakınında olmak yerleştirme en iyi performansı sunar. En düşük gecikme ile Azure depolama erişmek için en iyi istemciniz aynı Azure bölgesinde konumudur. Azure depolama kullanan bir Azure Web sitesi varsa, örneğin, her ikisinin de tek bir bölge içinde (örneğin, ABD Batı'ya da Güneydoğu Asya) bulmanıza. Bu gecikme süresi ve maliyeti azaltır — yazma aynı anda tek bir bölge içinde bant genişliği kullanımı ücretsizdir.  

Uygulamaları Azure içinde (mobil cihaz uygulamalarını gibi veya şirket içi Kurumsal Hizmetler), daha sonra tekrar barındırılan değil, istemci, erişecek aygıtları yakınında olmak bir bölgede depolama hesabı yerleştirme genellikle gecikme süresini azaltır İstemcilerinizi (örneğin, bazı Kuzey Amerika ve Avrupa'da bazı için) kapsamlı dağıtılır sonra birden çok depolama hesabı kullanmayı düşünmelisiniz: Kuzey Amerika bölge, diğeri Avrupa bir bölgede bulunan. Bu, her iki bölgelerdeki kullanıcılar için gecikme süresini azaltmak için yardımcı olur. Bu genellikle çok daha kolay uygulama verilerini depolayan durumunda uygulama bireysel kullanıcılara özeldir ve depolama hesapları arasında veri çoğaltmak gerektirmez yaklaşımdır.  Geniş içerik dağıtımı için bir CDN önerilen – daha fazla ayrıntı için sonraki bölüme bakın.  

### <a name="subheading5"></a>İçerik dağıtımı
Bazı durumlarda, bir uygulama için çok sayıda kullanıcı (bir Web sitesinin giriş sayfasında kullanılan örneğin bir ürün tanıtım videosu), aynı ya da birden çok bölgeye içinde bulunan aynı içerik sunmanızı gerekiyor. Bu senaryoda, Azure CDN gibi bir içerik teslim ağı (CDN) kullanmanız gerekir ve CDN Azure storage veri kaynağı kullanır. Tek bir bölgede bulunduğunu ve diğer bölgelere düşük gecikme ile içerik sunan olamaz bir Azure Storage hesabı farklı olarak, birden çok veri merkezlerinde dünyanın sunucuları Azure CDN kullanır. Ayrıca, bir CDN genellikle tek bir depolama hesabına çok daha yüksek çıkış sınırlardan destekleyebilir.  

Azure CDN hakkında daha fazla bilgi için bkz: [Azure CDN](https://azure.microsoft.com/services/cdn/).  

### <a name="subheading6"></a>SAS ve CORS kullanma
Kodda JavaScript gibi bir kullanıcının web tarayıcısı veya cep telefonu uygulamanızı Azure storage'da verilere erişme yetkisi gerektiğinde, bir yaklaşım uygulama proxy'si olarak web rolü kullanmaktır: sırayla depolama hizmeti ile kimlik doğrulaması web rolüyle cihazın kimliğini doğrular. Bu şekilde, depolama hesabı anahtarlarınızı güvenli olmayan cihazlarda gösterme önleyebilirsiniz. Kullanıcının cihaz ve depolama hizmeti arasında aktarılan tüm verilerin web rolü geçmesi gerekir çünkü ancak, bu büyük yükü web rolü yerleştirir. Web rolü bir proxy olarak depolama hizmeti için paylaşılan erişim imzaları (SAS) bazen üstbilgileri çıkış noktaları arası kaynak paylaşımı (CORS) ile birlikte kullanarak önleyebilirsiniz. SAS kullanarak, kullanıcı cihazının sınırlı erişim belirteci yoluyla doğrudan bir depolama birimi hizmeti isteğinde izin verebilirsiniz. Örneğin, bir kullanıcı bir fotoğraf uygulamanızı karşıya yükleme isterse, web rolü oluşturmak ve kullanıcının cihazına bir belirli blob veya kapsayıcı sonraki 30 (daha sonra SAS belirteci süresi dolar) dakika boyunca yazma izni veren bir SAS belirteci gönderin.

Normalde, bir tarayıcı JavaScript bir "başka bir etki alanına PUT" gibi belirli işlemler gerçekleştirmek için bir etki alanındaki bir Web sitesi tarafından barındırılan bir sayfasında izin vermez. Örneğin, "contosomarketing.cloudapp.net" bir web rolü barındırır ve istemci tarafı JavaScript "contosoproducts.blob.core.windows.net," tarayıcı 's adresindeki depolama hesabınızda blob karşıya yüklemek için kullanmak istediğiniz "aynı kaynak İlkesi" Bu işlemi yasaklamaz. CORS hedef etki alanında (Bu durumda depolama hesabı) (Bu durumda web rolü) kaynak etki alanındaki kaynaklanan istekler güvenleri tarayıcıya iletişim kurmasını sağlayan bir tarayıcı özelliğidir.  

Bu iki teknolojinin, gereksiz yük (ve performans sorunlarını), web uygulamanızda önlemenize yardımcı olabilir.  

#### <a name="useful-resources"></a>Yararlı Kaynaklar
SAS hakkında daha fazla bilgi için bkz: [paylaşılan erişim imzaları, 1. parça: SAS modelini anlama](../storage-dotnet-shared-access-signature-part-1.md).  

CORS hakkında daha fazla bilgi için bkz: [Azure Storage Hizmetleri için çıkış noktaları arası kaynak paylaşımı (CORS) Destek](http://msdn.microsoft.com/library/azure/dn535601.aspx).  

### <a name="caching"></a>Önbelleğe alma
#### <a name="subheading7"></a>Veri alma
Genel olarak, verileri bir hizmetinden kez alınıyor, iki kez alma daha iyidir. Bir kullanıcıya içerik olarak hizmet verecek Depolama hizmetinden 50 MB blob zaten alınan bir web rolü çalıştıran bir MVC web uygulaması örneği göz önünde bulundurun. Uygulama, daha sonra bir kullanıcı istekte ya da yerel olarak disk ve sonraki kullanıcı istekleri için önbelleğe alınan sürüm yeniden önbelleğe her zaman o aynı blob alınamadı. Ayrıca, her bir kullanıcı verileri, uygulama sorunu değiştirilip değiştirilmediğini varsa tüm blob alma kaçının değiştirme saati için koşullu bir üstbilgiyle ALABİLİR ister. Bu aynı düzeni tablo varlıklarla çalışmaya uygulayabilirsiniz.  

Bazı durumlarda, uygulamanızın blob almadan sonra kısa bir süre için geçerli kalmasını ve bu dönemde uygulamanın blob değiştirildiği denetlemek gerekmediğini varsayabilirsiniz karar verebilirsiniz.

Yapılandırma, arama ve her zaman uygulama tarafından kullanılan diğer verileri önbelleğe almak için harika bir aday değildir.  

.NET kullanarak son değiştirilme tarihi bulmak için bir blob'un özelliklerini alma örneği için bkz: [ayarlama ve alma özelliklerini ve meta veri](../blobs/storage-properties-metadata.md). Koşullu yüklemeler hakkında daha fazla bilgi için bkz: [koşullu bir Blob yerel bir kopyasını yenileme](http://msdn.microsoft.com/library/azure/dd179371.aspx).  

#### <a name="subheading8"></a>Toplu veri yüklemek
Bazı uygulama senaryolarında verilerini yerel olarak toplamak ve düzenli aralıklarla, her veri parçası hemen karşıya yerine bir toplu işte karşıya yükleyin. Örneğin, bir web uygulaması bir günlük dosyası etkinliklerin tutabilirsiniz: uygulama ya da her etkinlik ayrıntılarını (çok sayıda depolama işlemleri gerektirir) bir tablo varlığı olur veya etkinliği ayrıntıları yerel günlük dosyasına kaydedin ve ardından düzenli aralıklarla tüm Etkinlik ayrıntıları blob sınırlandırılmış dosyası olarak karşıya yükleme gibi karşıya yüklenemedi. Her günlük girişinin 1 KB boyutunda ise, binlerce (en fazla 64 MB boyutunda tek bir işlemde bir blob'u karşıya yükleyebilirsiniz) tek bir "Put Blob" işlemde karşıya yükleyebilirsiniz. Elbette, yerel makine önce karşıya yükleme çökerse bazı günlük verilerini potansiyel olarak kaybedersiniz: uygulama geliştiricisi tasarlamak için istemci cihazı olasılığını veya hataları karşıya yükleyin.  Etkinlik verileri timespans (yalnızca tek etkinliği) indirilmesi gerekiyorsa, BLOB'ları tablolar üzerindeki önerilir.

### <a name="net-configuration"></a>.NET yapılandırması
.NET Framework kullanılıyorsa, bu bölümde, önemli performans geliştirmeleri sağlamak için kullanabileceğiniz çeşitli hızlı yapılandırma ayarları listeler.  Diğer diller kullanıyorsanız, benzer kavram, seçtiğiniz dilde uygulanacaksa denetleyin.  

#### <a name="subheading9"></a>Varsayılan bağlantı sınırını artırmak
.NET içinde (genellikle olan bir istemci ortamında 2 ya da bir sunucu ortamında 10) varsayılan bağlantı sınırı 100 aşağıdaki kodu artırır. Genellikle, yaklaşık uygulamanız tarafından kullanılan iş parçacığı sayısı değeri ayarlamanız gerekir.  

```csharp
ServicePointManager.DefaultConnectionLimit = 100; //(Or More)  
```

Tüm bağlantılar açmadan önce bağlantı sınırı ayarlamanız gerekir.  

Diğer programlama dilleri için nasıl bağlantı sınırı belirlemek için dil belgelerine bakın.  

Ek bilgi için blog gönderisine bakın [Web Hizmetleri: eşzamanlı bağlantı](http://blogs.msdn.com/b/darrenj/archive/2005/03/07/386655.aspx).  

#### <a name="subheading10"></a>İş parçacığı havuzu en az iş parçacığı zaman uyumlu bir kod ile zaman uyumsuz görevleri kullanıyorsanız artırın
Bu kod iş parçacığı havuzu min iş parçacıkları artmasına neden olur:  

```csharp
ThreadPool.SetMinThreads(100,100); //(Determine the right number for your application)  
```

Daha fazla bilgi için bkz: [ThreadPool.SetMinThreads yöntemi](http://msdn.microsoft.com/library/system.threading.threadpool.setminthreads%28v=vs.110%29.aspx).  

#### <a name="subheading11"></a>.NET 4.5 çöp toplama yararlanın
Sunucu çöp toplama performans geliştirmeleri avantajlarından yararlanmak için .NET 4.5 veya üzeri istemci uygulaması için kullanın.

Daha fazla bilgi için bkz: [bir genel bakış, performans iyileştirmeleri .NET 4.5 içinde](http://msdn.microsoft.com/magazine/hh882452.aspx).  

### <a name="subheading12"></a>Sınırsız paralellik
Paralellik performans için harika olabilirler, ancak karşıya yükleme veya indirme aynı depolama hesabında birden çok bölüm (kapsayıcıları, kuyruk veya tablo bölümlerini) erişmek için veya birden çok öğe aynı bölüme erişmek için birden çok Worker kullanarak verileri için sınırsız paralellik (iş parçacıkları ve/veya paralel isteklerinin sayısı sınır) kullanma hakkında dikkatli olun. Paralellik sınırsız ise, uygulamanızın istemci cihaz yetenekleri aşabilir veya depolama hesabı ölçeklenebilirlik uzun gecikme süreleri kaynaklanan ve azaltma hedefler.  

### <a name="subheading13"></a>Depolama istemcisi kitaplıklarını ve araçları
Her zaman istemci kitaplıkları ve Araçlar sağlanan en son Microsoft kullanın. Yazma zaman istemci kitaplıkları .NET, Windows Phone, Windows çalışma zamanı, Java ve C++ için kullanılabilir gibi diğer diller için Önizleme kitaplıkları vardır. Ayrıca, Microsoft, PowerShell cmdlet'leri ve Azure Storage ile çalışmak için Azure CLI komutları yayımladı. Microsoft etkin şekilde performans aklınızda bu araçların geliştirir, en son hizmet sürümleri ile güncel tutar ve bunlar kanıtlanmış performans uygulamalarının çoğu kendi içinde işleyemediği sağlar.  

### <a name="retries"></a>Yeniden deneme sayısı
#### <a name="subheading14"></a>Azaltma ServerBusy
Bazı durumlarda, depolama birimi hizmeti uygulamanız kısıtlama veya yalnızca bazı geçici koşul nedeniyle isteği hizmet ve "503 Sunucu meşgul" iletisi ya da "500 zaman aşımı" dönmek olabilir.  Bu, uygulamanızın ölçeklenebilirlik hedefleri hiçbirini yaklaşıyorsa veya sistem bölümlenmiş verileriniz için daha yüksek işleme izin verecek şekilde yeniden dengelenmesi yoksa oluşabilir.  İstemci uygulaması genellikle bu tür bir hataya neden olan işlemi yeniden denemeniz gerekir: aynı isteği daha sonra çalışırken başarılı olabilir. Ancak, ölçeklenebilirlik hedefleri aşan çünkü depolama hizmeti uygulamanız azaltma ya da hizmet isteği başka bir nedenle hizmet sunamaz olsa bile, agresif yeniden deneme genellikle sorunu daha da kötüsü oluşturur. Bu nedenle, bir üstel geri alma (istemci kitaplıkları varsayılan Bu davranış) kullanmanız gerekir. Örneğin, uygulamanız 2 saniye sonra 4 saniye sonra 10 saniye sonra 30 saniye sonra yeniden deneyin ve tamamen işlemden vazgeçerlerdi. Bu davranış önemli ölçüde kendi yük hizmette azaltma yerine herhangi bir sorun exacerbating uygulamanızda sonuçlanır.  

Çünkü bunlar azaltma sonucunu değildir ve geçici olması beklenen bağlantı hataları hemen yeniden denenebilir olduğunu unutmayın.  

#### <a name="subheading15"></a>Denenemeyen hata
İstemci kitaplıkları hangi hataları yeniden deneme yapabilir ve hangi olmadığı kullanan. REST API depolama karşı kendi kodunuzu yazıyorsanız, Bununla birlikte, yeniden bazı hatalar var. unutmayın: 400 Örneğin, (Hatalı istek yanıtı gösteriyor istemci uygulaması, beklenen bir formda olmadığından, işlenemedi bir istek gönderdi). Bu yüzden, yeniden deneniyor içinde hiçbir noktası bu isteği göndermeden, her zaman aynı yanıt neden olur. REST API depolama karşı kendi kodunuzu yazıyorsanız, ne hata kodlarını anlama ve (veya değil) yeniden denemek için uygun şekilde unutmayın her biri için.  

#### <a name="useful-resources"></a>Yararlı Kaynaklar
Depolama hata kodları hakkında daha fazla bilgi için bkz: [durum ve hata kodları](http://msdn.microsoft.com/library/azure/dd179382.aspx) Microsoft Azure web sitesinde.  

## <a name="blobs"></a>Bloblar
Kanıtlanmış uygulamalar için ek olarak [tüm hizmetleri](#allservices) daha önce açıklandığı gibi aşağıdaki yöntemler kanıtlanmış özellikle blob hizmete uygulayın.  

### <a name="blob-specific-scalability-targets"></a>BLOB özgü ölçeklenebilirlik hedefleri
#### <a name="subheading46"></a>Birden çok istemci aynı anda tek bir nesne erişme
Çok sayıda eş zamanlı olarak tek bir nesne erişen istemcilerin varsa nesnesi ve depolama hesabımın ölçeklenebilirlik hedefleri göz önünde bulundurmanız gerekir. Tek bir nesneye erişmek için istemcilerin tam sayı nesne aynı anda isteyen istemcilerin nesnenin boyutu sayısı gibi etkenlere bağlı olarak farklılık gösterir, koşullar vb. ağ.

Nesne üzerinden dağıtılabilir CDN görüntü veya video gibi bir Web sitesinden sunulan sonra bir CDN kullanmanız gerekir. Bkz: [burada](#subheading5).

Bilimsel benzetimleri veri gizli olduğu gibi diğer senaryolarda, iki seçeneğiniz vardır. İlk İş yükünüzün erişim sağlayacak şekilde nesne aynı anda erişilen zaman vs döneminde erişilen kademelendirebilirsiniz sağlamaktır. Alternatif olarak, bu nedenle depolama hesapları arasında ve nesne başına toplam IOPS artırma birden çok depolama hesabı için geçici olarak nesne kopyalayabilirsiniz. Sınırlı testinde yaklaşık 25 VM'ler 100 GB blob paralel (her VM 32 iş parçacığı kullanma indirme parallelizing) aynı anda yüklenemedi bulduk. Nesneye erişim, önce ikinci bir depolama hesabı kopyalayın ve sonra gerek 100 istemciye olsaydı ilk 50 VM'ler ilk blob ve ikinci 50 VM'ler erişim ikinci blob erişin. Bu tasarım sırasında test etmeniz gerekir böylece sonuçları uygulamaları davranışına bağlı olarak değişir. 

#### <a name="subheading16"></a>Bant genişliği ve Blob başına işlem
Okuma veya en fazla 60 MB/saniye (Bu, (istemci cihazda fiziksel NIC de dahil olmak üzere) çok sayıda istemci tarafı ağ yeteneklerini aşan yaklaşık 480 MB / sn'dir. tek bir blob yazma Ayrıca, tek bir blob saniye başına en fazla 500 isteklerini destekler. Aynı blob okumak için gereken birden çok istemciler varsa ve bu sınırları aşabilir blob dağıtmak amacıyla bir CDN kullanmayı düşünmeniz gerekir.  

BLOB'lar için hedef işleme hakkında daha fazla bilgi için bkz: [Azure Storage ölçeklenebilirlik ve performans hedefleri](storage-scalability-targets.md).  

### <a name="copying-and-moving-blobs"></a>Kopyalama ve BLOB'lar taşıma
#### <a name="subheading17"></a>BLOB kopyalama
REST API sürümü 2012-02-12 depolama hesapları arasında BLOB kopyalama becerisini yararlı sunulan: bir istemci uygulaması (büyük olasılıkla, farklı bir depolama hesabı) başka bir kaynaktan bir blob kopyalamak için depolama birimi hizmeti istemek ve kopyayı gerçekleştirip hizmeti olanak tanır. Bu uygulama için diğer depolama hesaplarından veri geçirirken indirip verileri karşıya yüklemek gerekmediği için gereken bant genişliğini önemli ölçüde azaltabilir.  

Bir göz önünde bulundurarak, ancak, depolama hesapları arasında kopyalarken, kopyalama tamamlandığında üzerinde hiçbir zaman garantisi olmasıdır. Uygulamanızı bir blob kopyalama denetiminiz altında hızlı bir şekilde tamamlamak gerekirse, bir VM'ye indirip hedefe karşıya blob kopyalamak daha iyi olabilir.  Bu durumda tam öngörülebilirlik için kopyalama aynı Azure bölgesinde çalıştıran bir VM tarafından gerçekleştirilen, aksi takdirde ağ koşulları, kopyalama performansını etkileyen olabilir ve muhtemelen emin olun.  Ayrıca, program aracılığıyla zaman uyumsuz bir kopya ilerlemesini izleyebilirsiniz.  

Aynı depolama hesabındaki kendisini kopyaları genellikle hızlı bir şekilde tamamlanır unutmayın.  

Daha fazla bilgi için bkz: [kopyalama Blob](http://msdn.microsoft.com/library/azure/dd894037.aspx).  

#### <a name="subheading18"></a>AzCopy kullanma
Azure depolama ekibi komut satırı aracı "AzCopy" yayımladı birçok BLOB'lar için gelen ve depolama hesapları arasında aktarma toplu yardımcı olmayı yöneliktir.  Bu araç, bu senaryo için en iyi duruma getirilmiş ve yüksek aktarım hızlarıyla elde edebilirsiniz.  Kullanımı toplu karşıya yükleme, indirme ve kopyalama senaryoları için teşvik edilir. Hakkında daha fazla bilgi edinmek ve yüklemek için bkz: [AzCopy komut satırı yardımcı programı ile veri aktarma](storage-use-azcopy.md).  

#### <a name="subheading19"></a>Azure içeri/dışarı aktarma hizmeti
(Birden fazla 1 TB) veri çok büyük birimlerde karşıya yükleme ve blob depolama alanından sevkiyat sabit sürücüler tarafından indirme sağlar içeri/dışarı aktarma hizmeti Azure Storage sunar.  Verilerinizi bir sabit sürücüye yerleştirin ve karşıya yükleme için Microsoft'a göndermek veya boş bir sabit sürücü veri karşıdan yüklemek üzere Microsoft'a gönder.  Daha fazla bilgi için bkz: [Blob depolama alanına veri aktarmak için Microsoft Azure içeri/dışarı aktarma hizmeti kullanma](../storage-import-export-service.md).  Karşıya yükleme/bu birimi verilerin ağ üzerinden indirme daha çok daha etkili olabilir.  

### <a name="subheading20"></a>Meta veri kullanın
Blob hizmeti blob hakkındaki meta verileri içerebilir head isteklerini destekler. Örneğin, uygulamanızın bir fotoğraf dışında EXIF veri gerekirse, bu fotoğrafı almak ve ayıklayın Uygulama fotoğrafı karşıya zaman bant genişliğinden tasarruf ve performansı artırmak için uygulamanızın EXIF veri blob'un meta verilerde saklayabilirsiniz: istek yalnızca HEAD kullanarak meta verilerde EXIF verilerini alma, EXIF ayıklamak için veriler her zaman blob kaydetme önemli bant genişliği ve işleme süresi okuma sonra kullanabilirsiniz. Bu, burada yalnızca meta veri ve blob değil tam içeriğini gerekir senaryolarda yararlı olacaktır.  Yalnızca 8 KB meta verileri bu boyut SIĞMADIĞINDA, bu yaklaşımı kullanmak mümkün olmaması (hizmet deposu daha yüksek bir istek kabul etmeyecek) blob depolanabilir unutmayın.  

.NET kullanarak bir blob'un meta verilerini almak nasıl bir örnek için bkz: [ayarlama ve alma özelliklerini ve meta veri](../blobs/storage-properties-metadata.md).  

### <a name="uploading-fast"></a>Hızlı yükleme
BLOB'ları hızlı karşıya yüklemek için yanıtlamak için ilk soru şudur: misiniz bir blob ya da birden çok karşıya?  Kullanın senaryonuza bağlı olarak kullanmak üzere doğru yöntemini belirlemek için yönergeler aşağıda.  

#### <a name="subheading21"></a>Bir büyük blob hızlı bir şekilde karşıya yükleme
Tek bir büyük blob hızlı bir şekilde yüklemek için istemci uygulamanız blok veya sayfaları paralel (tek tek bloblar ve bir bütün olarak depolama hesabı için ölçeklenebilirlik hedefleri dikkatli olmak) yüklemeniz gerekir.  Resmi Microsoft tarafından sağlanan RTM depolama istemcisi kitaplıklarını (.NET, Java) bunu olanağı gerektiğini unutmayın.  Her kitaplıkların kullanmak eşzamanlılık düzeyini ayarlamak için belirtilen nesne/özelliği altında:  

* .NET: kullanılacak kümesi ParallelOperationThreadCount BlobRequestOptions nesne üzerinde.
* Java/Android: BlobRequestOptions.setConcurrentRequestCount() kullanın
* Node.js: parallelOperationThreadCount isteği seçenekleri veya blob hizmeti kullanın.
* C++: blob_request_options::set_parallelism_factor yöntemini kullanın.

#### <a name="subheading22"></a>Çok sayıda BLOB'ları hızlı bir şekilde karşıya yükleme
Çok sayıda BLOB'ları hızlı bir şekilde yüklemek için BLOB'ları paralel karşıya yükleyin. Bu, depolama birimi hizmeti birden çok bölüm arasında karşıya yükleme yayılan çünkü tek BLOB'lar aynı anda paralel blok karşıya karşıya daha hızlıdır. Tek bir blob yalnızca 60 MB/saniye (yaklaşık 480 Mbps) üretimini destekler. Yazma aynı anda tek bir blob tarafından desteklenen işleme daha çok daha fazla olan en fazla 20 GB/sn giriş ABD tabanlı LRS hesabı destekler.  [AzCopy](#subheading18) yüklemeleri varsayılan olarak paralel olarak gerçekleştirir ve bu senaryo için önerilir.  

### <a name="subheading23"></a>Blob doğru türünü seçme
Azure Storage blobu iki türlerini destekler: *sayfa* blobları ve *blok* BLOB'lar. Belirli kullanım senaryosu için tercih ettiğiniz blob türü performans ve ölçeklenebilirlik çözümünüzün etkiler. Blok blobları, büyük miktarlarda verinin verimli bir şekilde karşıya yüklemek istediğiniz zaman uygundur: Örneğin, bir istemci uygulaması fotoğraf veya video blob depolama alanına yükleme yapmanız gerekebilir. Sayfa bloblarını uygulama verilerini rastgele yazma işlemlerini gerçekleştirmek gerekirse uygun: Örneğin, Azure VHD'ler sayfa BLOB olarak depolanır.  

Daha fazla bilgi için bkz: [anlama blok Blobları, ekleme Blobları ve sayfa Bloblarını](http://msdn.microsoft.com/library/azure/ee691964.aspx).  

## <a name="tables"></a>Tablolar
Kanıtlanmış uygulamalar için ek olarak [tüm hizmetleri](#allservices) daha önce açıklandığı gibi aşağıdaki yöntemler kanıtlanmış özellikle tablo hizmete uygulayın.  

### <a name="subheading24"></a>Tablo özgü ölçeklenebilirlik hedefleri
Ek olarak tüm depolama hesabınız bant genişliği sınırlamaları tabloları aşağıdaki belirli ölçeklenebilirlik sınırına sahip.  Sistem, trafiği arttıkça Yük Dengelemesi, ancak trafiğinizi ani WINS'e varsa, bu birimi sn'ye hemen almak mümkün olmayabilir unutmayın.  Desen WINS'e varsa, azaltma ve/veya zaman aşımları aşırı depolama sırasında otomatik olarak yük bakiyelerini tablonuz çıkışı hizmet görmeyi beklemelisiniz.  Yukarı yavaş genellikle ramping uygun şekilde dengelemek için sistem saatini verir gibi daha iyi sonuçlar vardır.  

#### <a name="entities-per-second-account"></a>Varlıkları / saniye (hesap)
Tabloları erişmek için ölçeklenebilirlik sınırına kadar 20.000 varlıkları (1 KB her) başına ikinci bir hesabıdır.  Genel olarak, eklenir, her bir varlık güncelleştirildi, silinmiş veya bu hedef doğru sayar taranan.  Bu nedenle 100 varlıklarını içeren bir toplu yerleştirme 100 varlık olarak sayar.  1000 varlıklar tarar ve 5 döndüren bir sorgu 1000 varlıklar sayılır.  

#### <a name="entities-per-second-partition"></a>Varlıkları / saniye (bölüm)
Tek bir bölüm tablolar erişmek için ölçeklenebilirlik 2.000 varlıkları (1 KB her) önceki bölümde açıklandığı gibi aynı sayım kullanarak saniyede hedefidir.  

### <a name="configuration"></a>Yapılandırma
Bu bölümde tablo hizmetinde önemli performans geliştirmeleri sağlamak için kullanabileceğiniz çeşitli hızlı yapılandırma ayarları listelenmiştir:  

#### <a name="subheading25"></a>JSON kullanın
Depolama hizmeti sürüm 2013-08-15'den başlayarak, tablo hizmeti tablo verileri aktarmak için yerine XML tabanlı AtomPub biçimi kullanarak destekler. Bu yük boyutları % 75 azaltabilir ve uygulamanızın performansını önemli ölçüde artırabilir.

Post daha fazla bilgi için bkz: [Microsoft Azure tabloları: Giriş JSON](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/05/windows-azure-tables-introducing-json.aspx) ve [tablo hizmeti işlemleri için yük biçimi](http://msdn.microsoft.com/library/azure/dn535600.aspx).

#### <a name="subheading26"></a>Nagle devre dışı
Nagle'nın algoritması, yaygın olarak TCP/IP'yi ağlarda ağ performansını artırmak için bir yol uygulanır. Ancak, tüm durumlarda (örneğin, yüksek oranda etkileşimli ortamlarda) uygun değil. Azure Storage, tablo ve kuyruk Hizmetleri isteklerine performansı olumsuz etkileyebilir Nagle'nın algoritmaya sahiptir ve mümkünse bu seçeneği devre dışı.  

Bizim blog gönderisi daha fazla bilgi için bkz: [Nagle'nın algoritmasıdır küçük isteklerini doğru değil kolay](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/06/25/nagle-s-algorithm-is-not-friendly-towards-small-requests.aspx), neden Nagle'nın algoritması kötü tablo ve kuyruk istekleri ile etkileşim kurar ve istemci uygulamanızı devre dışı bırakma gösterir açıklar.  

### <a name="schema"></a>Şema
Nasıl temsil eder ve verilerinizi sorgu tablo Hizmeti performansını etkileyen en büyük tek faktördür. Her uygulama farklı olsa da, bu bölümde ile ilgili bazı genel kanıtlanmış uygulamalar özetlenmektedir:  

* Tablo Tasarımı
* Verimli sorguları
* Verimli veri güncelleştirmeleri  

#### <a name="subheading27"></a>Tabloları ve bölümleri
Tabloları bölümlere ayrılır. Bir bölümdür her varlık aynı bölüm anahtarına paylaşır ve bu bölüm içinde tanımlamak için benzersiz bir satır anahtarı yok. Bölümler fayda sağlar, ancak aynı zamanda ölçeklenebilirlik sınırları tanıtır.  

* Yararları: En fazla 100 ayrı depolama işlemleri (4 MB Toplam boyut sınırı) içeren bir tek atomik, toplu işlem içinde aynı bölüme varlıklarda güncelleştirebilirsiniz. Alınacak varlıkları aynı sayıda varsayıldığında, de tek bir bölüm içindeki verileri (ancak daha fazla tablo verileri Sorgulama öneriler için okumaya devam edin) bölümleri yayılan verileri daha verimli sorgulayabilirsiniz.
* Ölçeklenebilirlik sınırına: tek bir bölüm içinde depolanan varlıklarına erişmek için olamaz yük dengeli bölümleri atomik toplu işlemleri desteklemek için. Bu nedenle, tek tek tablo bölümü için ölçeklenebilirlik hedef bir bütün olarak tablo hizmeti için daha düşüktür.  

Bu özellikleri tablolar ve bölümler nedeniyle, aşağıdaki tasarım ilkeleri benimsemeye:  

* İstemci uygulamanızı sık güncelleştirilen veya iş aynı mantıksal biriminde sorgulanan verilerin aynı bölümde bulunmalıdır.  Bu, uygulamanızın yazma toplama veya atomik toplu işlemleri yararlanmak istediğiniz nedeniyle, olabilir.  Ayrıca, tek bir bölüm verileri daha verimli bir şekilde tek bir sorguda veri bölümler sorgulanabilir.
* İstemci uygulamanızı değil ekleme/güncelleştirme veriler veya sorgu iş (tek bir sorgu veya toplu güncelleştirme) aynı mantıksal birimine ayrı bölümlerinde bulunmalıdır.  Önemli bir not tek bir tabloda bölüm anahtar sayısı için bir sınır yoktur böylece bir sorun değildir ve performansını etkilemez bölüm anahtarlarını milyonlarca sahip olmasıdır.  Örneğin, uygulamanızın kullanıcı oturum açma ile popüler bir Web sitesi ise, bölüm anahtarı olarak kullanıcı kimliğini kullanarak iyi bir seçimdir olması olabilir.  

#### <a name="hot-partitions"></a>Sık kullanılan bölümleri
Bir sık kullanılan bir hesap trafiği orantısız yüzdesi alan bir bölümdür ve tek bir bölüm olduğundan, yük dengeli olamaz.  Genel olarak, dinamik bölümleri iki yoldan biriyle oluşturulur:  

##### <a name="subheading28"></a>Yalnızca ekleme ve yalnızca desenleri önüne ekleyin
"Yalnızca Ekle" deseni burada verilen PK trafiği tümü (veya neredeyse tüm) artırır ve göre geçerli saati azaltır biridir.  Uygulamanız için günlük verileri bölüm anahtarı olarak geçerli tarih kullandıysanız bir örnektir.  Bu tüm giderek eklemeleri son tablonuz bölümünde sonuçlanır ve tüm yazma tablonuz sonuna kadar giden sistem Bakiye yükleyemiyor.  Bu bölüm trafik hacmi bölüm düzeyi ölçeklenebilirlik hedef aşarsa, azaltma içinde sonuçlanır.  Trafiği tablonuz arasında istekleri Yük Dengeleme sağlamak için birden çok bölüm için gönderilir sağlamak en iyisidir.  

##### <a name="subheading29"></a>Yüksek trafik verileri
Yalnızca diğer bölümler çok fazla kullanılan verileri içeren tek bir bölüm, bölümleme düzeni sonuçlanırsa, bölüm tek bir bölüm için ölçeklenebilirlik hedef yaklaşıyor olarak azaltma de görebilirsiniz.  Bölüm düzeni ölçeklenebilirlik hedefleri yaklaşan tek bölüm içinde sonuçları emin olmak daha iyidir.  

#### <a name="querying"></a>Sorgulama
Bu bölümde tablo Hizmeti'ni sorgulamak için kanıtlanmış yöntemleri açıklar.  

##### <a name="subheading30"></a>Sorgu kapsamı
Sorgu varlıklara aralığını belirtmek için çeşitli yollar vardır.  Tartışma için kullandığı her verilmiştir.  

Genel olarak, tarama (sorguları tek bir varlık büyük) kaçının, ancak tarama gerekir, böylece taramalarınızda tarama veya önemli miktarda gerekmeyen varlıklar döndürüyor gereken verileri almak, verilerinizi düzenlemek deneyin.  

###### <a name="point-queries"></a>Noktası sorguları
Noktası sorgu tam olarak bir varlığı alır. Bu bölüm anahtarı ve alınacak varlığın satır anahtarı belirterek yapar. Bu sorgular çok verimli ve onları mümkün olduğunda kullanmalısınız.  

###### <a name="partition-queries"></a>Bölüm sorguları
Ortak bir bölüm anahtarı paylaşan veri kümesini alır. bir sorgu bir bölüm sorgudur. Genellikle, sorgu satır anahtar değerleri aralığı ya da bölüm anahtarı ek olarak bazı varlık özelliği için değer aralığını belirtir. Bunlar noktası sorguları az verimlidir ve kullanılmamalıdır.  

###### <a name="table-queries"></a>Tablo sorguları
Tablo sorgusu ortak bir bölüm anahtarı paylaşmaz varlık kümesini alır bir sorgudur. Bu sorguları etkili değildir ve mümkünse kaçınmalısınız.  

##### <a name="subheading31"></a>Sorgu yoğunluğu
Başka bir anahtar etken sorgu verimliliği de döndürülen döndürülen kümesi bulmak için taranan varlıkların sayısı karşılaştırıldığında varlıkların sayısıdır. Uygulamanızı bir tablo sorgusu yalnızca %1 veri paylaşımlarının 100 varlıklar döndürdüğü her bir varlık için sorgu tarama bir özellik değeri için bir filtre ile uyguluyorsa. Ele alınan tablo ölçeklenebilirlik hedefleri daha önce tüm taranan varlıkların sayısı ve döndürülen varlıkları sayısını değil ilişkilendirebilirsiniz: düşük sorgu yoğunluğu kolayca aradığınız varlığını almak için çok fazla sayıda varlıklar tarama gerekir çünkü uygulamanız kısıtlama tablo hizmeti neden olabilir.  Bölümüne bakın üzerinde [denormalization](#subheading34) bu durumu önlemek hakkında daha fazla bilgi için.  

##### <a name="limiting-the-amount-of-data-returned"></a>Döndürülen veri miktarını sınırlama
###### <a name="subheading32"></a>Filtreleme
Bir sorgu istemci uygulamasında gerekmeyen varlıkları döndürme bildiğiniz durumlarda, döndürülen kümesi boyutunu azaltmak için bir filtre kullanmayı deneyin. İstemciye döndürülen olmayan varlıklar hala doğru ölçeklenebilirlik sınırları saymak olsa da, azaltılmış ağ yükü boyutu ve daha az istemci uygulamanız işlemelidir varlıkların sayısı nedeniyle, uygulama performansı iyileştirir.  Not bakın [sorgu yoğunluğu](#subheading31), birkaç varlıklar döndürülen olsa bile birçok varlık filtreler bir sorgu hala azaltma içinde sonuçlanabilir şekilde ölçeklenebilirlik hedefleri taranan varlıkların sayısı ancak – ilişkilidir.  

###### <a name="subheading33"></a>Yansıtma
İstemci uygulamanızı yalnızca sınırlı sayıda tablonuz varlıklarda özelliklerinden gerekirse, projeksiyon döndürülen veri kümesi boyutunu sınırlamak için kullanabilirsiniz. İle gibi filtreleme, bu ağ yükü ve işleme istemci azaltılmasına yardımcı olur.  

##### <a name="subheading34"></a>Denormalization
Tablo verisi verimli bir şekilde sorgulamak için kanıtlanmış yöntemleri, ilişkisel veritabanları ile çalışma aksine, verilerinizi denormalizing için yol açar. Diğer bir deyişle, birden çok varlık (verileri bulmak üzere kullanabilir her anahtar için bir tane) aynı verileri çoğaltma istemci verileri bulmak üzere bir sorgu taramalısınız varlıkların sayısını en aza indirmek için gereken yerine verileri bulmak üzere varlıklar çok sayıda tarama gerek kalmadan uygulamanız gerekir.  Örneğin, bir e-ticaret Web sitesi göre bir sıralamayı hem Müşteri Kimliği bulmak istediğiniz (bu müşterinin sipariş ver) ve tarihe göre (bir tarihte sipariş ver).  Table Storage ' varlık (veya bir başvuru) depolamak en iyisidir iki kez – bir kez tablo adıyla PK ve işareti müşteri tarafından bulma kolaylaştırmak için kimliği, bir kez tarihe göre bulma kolaylaştırmak için.  

#### <a name="insertupdatedelete"></a>Ekleme/güncelleştirme/silme
Bu bölümde tablo hizmetinde saklanan varlıkları değiştirme kanıtlanmış yöntemleri açıklar.  

##### <a name="subheading35"></a>Toplu işleme
Toplu işlem, Azure storage'da varlık Grup işlemleri (ETG) olarak adlandırılır; bir ETG tüm işlemlerini tek bir tablodaki tek bir bölüm olması gerekir. Mümkünse, ETGs toplu olarak ekleme, güncelleştirme ve silme gerçekleştirmek için kullanın. Bu istemci uygulamanızdan sunucuya gidiş dönüş sayısını azaltan (bir ETG faturalandırma için tek bir işlem olarak sayar ve 100'e kadar depolama işlemleri içerebilir) Faturalanabilir işlemleri sayısını azaltır ve atomik güncelleştirmeleri (tüm işlemlerin başarılı ya da bir ETG içinde başarısız) etkinleştirir. Mobil cihazlar gibi yüksek gecikme süreleriyle ortamları ETGs kullanarak büyük ölçüde yararlanabilir.  

##### <a name="subheading36"></a>Upsert
Kullanımı tablo **Upsert** işlemlerini mümkün olduğunda. İki tür vardır **Upsert**, her ikisi de Geleneksel daha etkili olabilir **Ekle** ve **güncelleştirme** işlemleri:  

* **InsertOrMerge**: varlığın özelliklerinin bir alt karşıya yüklemek istediğiniz zaman bu kullanın, ancak varlık zaten var olup emin değilseniz. Varlık zaten varsa, bu çağrıyı dahil özellikleri güncelleştirmeleri **Upsert** işlemi ve tüm mevcut özellikleri bırakır varlık mevcut değilse, olduğu gibi yeni varlık ekler. Yalnızca değiştiriyorsunuz özellikleri yüklemek gereken bu sorguda projeksiyon kullanmaya benzer.
* **Yerleştir veya Değiştir**: tamamen yeni bir varlık karşıya yüklemek istediğiniz, ancak olup zaten var. emin değilseniz bunu kullanın. Yeni yüklenen varlık tamamen eski varlık yazdığından tamamen doğru olduğunu bildiğiniz durumlarda yalnızca bu kullanmanız gerekir. Örneğin, kullanıcının geçerli konumuna uygulamayı daha önce depolanan kullanıcı için konum verilerini olup olmadığına bakılmaksızın depolar varlık güncelleştirmek istediğiniz; Yeni bir konum varlık tamamlandıktan ve tüm önceki varlıktan herhangi bir bilgi gerekmez.

##### <a name="subheading37"></a>Tek bir varlık veri serisi depolama
Bazı durumlarda, bir uygulama, sık sık aynı anda almak için gereken verileri bir dizi depolar: Örneğin, bir uygulama CPU kullanımı zaman içinde son 24 saat verileri çalışırken grafiği çizmek için izlemek. Belirli bir saati temsil eden ve CPU kullanımı, saat için depolama her varlığıyla, saatte bir tablo varlık sahip bir yaklaşımdır. Bu verileri çizmek için en son 24 saat verilerini tutan varlıkları almak uygulamanın gerekir.  

Uygulamanızın ayrı özelliği tek bir varlık olarak her bir saat için alternatif olarak, CPU kullanımı saklayabilirsiniz: her saat güncelleştirmek için uygulamanızın tek bir kullanabilirsiniz **InsertOrMerge Upsert** çağrısı en son saat için değeri güncelleştirin. Verileri çizmek için uygulamanın yalnızca 24, çok verimli bir sorgu için yapmak yerine tek bir varlık alma gerekir (tartışma bakın [sorgu kapsam](#subheading30)).

##### <a name="subheading38"></a>BLOB'ları yapılandırılmış veri depolama
Bazen yapılandırılmış verileri tablolarda, gitmesi gereken ancak aralıkları varlıkların her zaman birlikte alınır ve toplu eklenebilir gibi hissettirir.  Bu iyi bir örneği günlük dosyasıdır.  Bu durumda, birkaç dakikadan günlükler toplu, bunları eklemek ve sonra her zaman aynı anda de günlüklerinin birkaç dakika alıyor.  Bu durumda, performans için yazılan /, genellikle gereksinim yapılan isteklerin sayısı yanı sıra döndürülen nesne sayısını önemli ölçüde azaltabilir beri tablolar yerine, BLOB'ları kullanmak en iyisidir.  

## <a name="queues"></a>Kuyruklar
Kanıtlanmış uygulamalar için ek olarak [tüm hizmetleri](#allservices) daha önce açıklandığı gibi aşağıdaki yöntemler kanıtlanmış özellikle sıra hizmete uygulayın.  

### <a name="subheading39"></a>Ölçeklenebilirlik sınırları
Tek bir sırada (Buraya bir ileti olarak her AddMessage, GetMessage ve DeleteMessage sayısı) saniyede yaklaşık 2.000 iletileri (1 KB her) işleyebilir. Bu, uygulamanız için yetersiz ise, birden çok sıraya kullanın ve iletileri bunların arasında yayılır.  

Konumundaki geçerli ölçeklenebilirlik hedefleri görüntülemek [Azure Storage ölçeklenebilirlik ve performans hedefleri](storage-scalability-targets.md).  

### <a name="subheading40"></a>Nagle devre dışı
Nagle algoritması ele tablo yapılandırma bölümüne bakın — Nagle algoritmasıdır sıra isteklerini performansı için genellikle bozuk ve devre dışı bırakmalısınız.  

### <a name="subheading41"></a>İleti boyutu
Sıra performans ve ölçeklenebilirlik azalıyor ileti boyutu artar. Bir ileti yalnızca alıcı gereksinim duyduğu bilgileri yerleştirmeniz gerekir.  

### <a name="subheading42"></a>Toplu alma
Tek bir işlemde bir kuyruktan en fazla 32 iletileri alabilirsiniz. Bu gidiş-dönüş sayısını mobil cihazlar gibi ortamlar için özellikle yararlıdır istemci uygulamasından yüksek gecikme süresiyle azaltabilir.  

### <a name="subheading43"></a>Sıra yoklama aralığı
Bu uygulama için işlemlerinin büyük kaynaklardan biri olabilen bir sıradan iletileri için uygulamaların çoğu yoklar. Yoklama aralığı akıllıca seçin: çok sık yoklama sıra için ölçeklenebilirlik hedefleri yaklaşmak için uygulamanızı neden olabilir. Ancak, (yazma zamanında) $0,01 için 200.000 işlemleri sırasında bir ay boyunca her saniye kadar Maliyet değerinden 15 Sent maliyeti sonra yoklama tek bir işlemcinin genellikle seçiminizi yoklama aralığı etkileyen bir faktör değil.  

Güncel maliyet bilgi için bkz: [Azure Storage fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).  

### <a name="subheading44"></a>UpdateMessage
Kullanabileceğiniz **UpdateMessage** görünmezlik zaman aşımı süresini artırın veya bir ileti durumu bilgilerini güncelleştirmek için. Bu güçlü olmakla birlikte, her unutmayın **UpdateMessage** işlemi ölçeklenebilirlik hedef sayar. Ancak, bu işin her adım tamamlandı olarak bir iş yanında, bir kuyruktan geçirmeden bir iş akışı sahip daha çok daha verimli bir yaklaşım olabilir. Kullanarak **UpdateMessage** işlemi iletiye iş durumu kaydedin ve ardından bir adım tamamlandıktan her zaman yeniden işin bir sonraki adım için message queuing yerine çalışmaya devam edebilmek için uygulamanızı sağlar.  

Daha fazla bilgi için bkz: [nasıl yapılır: kuyruğa alınan iletinin içeriğini değiştirme](../queues/storage-dotnet-how-to-use-queues.md#change-the-contents-of-a-queued-message).  

### <a name="subheading45"></a>Uygulama mimarisi
Uygulama Mimarinizi ölçeklenebilir yapmak için kuyrukları kullanmanız gerekir. Uygulamanızı daha ölçeklenebilir yapmak için kuyrukları kullanabilirsiniz bazı yollarını listeler:  

* İşleme iş biriktirme listeleri oluşturma ve uygulamanızda iş yükleri kesintisiz için kuyrukları kullanabilirsiniz. Örneğin, karşıya yüklenen görüntüleri yeniden boyutlandırma gibi yoğun çalışma işlemci gerçekleştirmek için kullanıcıların isteklerini yukarı sıraya.
* Böylece, bağımsız olarak ölçeklendirebilirsiniz uygulamanızın parçalarını ayırırsınız için kuyrukları kullanabilirsiniz. Örneğin, bir web ön uç anket sonuçlarını kullanıcılardan sonraki çözümleme ve depolama için bir sıra yerleştirin. Sıra verileri gereken şekilde işlemek için daha fazla çalışan rolü örnekleri ekleyebilirsiniz.  

## <a name="conclusion"></a>Sonuç
Bu makalede en yaygın bazı Azure depolama kullanırken performansını iyileştirmek için yöntemler kanıtlanmış açıklanmıştır. Biz, her uygulama geliştiricisine, yukarıda belirtilen yöntemlerin her biri ile uygulamalarını değerlendirmelerini ve Azure Depolama kullanan uygulamalarından mükemmel performans alma önerilerini dikkate almalarını öneririz.