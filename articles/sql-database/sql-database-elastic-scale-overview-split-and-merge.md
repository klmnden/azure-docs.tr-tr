---
title: "Ölçeklendirilen bulut veritabanları arasında veri taşıma | Microsoft Docs"
description: "Parça işlemek ve esnek veritabanı API'leri kullanarak bir kendi kendini barındıran hizmet veri taşımak açıklanmaktadır."
services: sql-database
manager: craigg
author: stevestein
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 10/24/2016
ms.author: sstein
ms.openlocfilehash: 9e2b231ad2e9fc5ab07532daef44da9870cef4ae
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="moving-data-between-scaled-out-cloud-databases"></a>Ölçeği genişletilen bulut veritabanları arasında veri taşıma
Bir hizmet geliştiricisi olarak yazılımlardır ve aniden uygulamanızı inanılmaz isteğe bağlı olduğunda, büyüme uyum sağlaması gerekmektedir. Bu nedenle daha fazla veritabanı (bölümler) ekleyin. Nasıl veri bütünlüğü kesintiye uğratmadan yeni veritabanları için veri dağıtabilir? Kullanım **bölünmüş Birleştirme aracı** verileri kısıtlanmış veritabanlarından yeni veritabanlarını taşımak için.  

Bölünmüş Birleştirme aracı bir Azure web hizmeti olarak çalışır. Bir yönetici veya Geliştirici shardlets (bir parça verilerden) farklı veritabanlarını (bölümler) arasında taşıma Aracı'nı kullanır. Aracı parça eşleme yönetim hizmeti meta veri veritabanının bakımını yapmak ve tutarlı eşlemeleri sağlamak için kullanır.

![Genel Bakış][1]

## <a name="download"></a>İndirme
[Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/)

## <a name="documentation"></a>Belgeler
1. [Esnek veritabanı bölünmüş Birleştirme aracı Öğreticisi](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
2. [Bölünmüş birleştirme güvenlik yapılandırması](sql-database-elastic-scale-split-merge-security-configuration.md)
3. [Bölünmüş birleştirme güvenlik konuları](sql-database-elastic-scale-split-merge-security-configuration.md)
4. [Parça eşleme yönetimi](sql-database-elastic-scale-shard-map-management.md)
5. [Ölçeği genişletilen mevcut veritabanlarını geçirme](sql-database-elastic-convert-to-use-elastic-tools.md)
6. [Esnek veritabanı araçları](sql-database-elastic-scale-introduction.md)
7. [Esnek veritabanı araçlarını sözlüğü](sql-database-elastic-scale-glossary.md)

## <a name="why-use-the-split-merge-tool"></a>Bölünmüş Birleştirme aracı neden kullanılır?
**Esneklik**

Tek bir Azure SQL DB veritabanı sınırlarının dışına esnek uzatmak uygulamaları gerekir. Veri bütünlüğü koruyarak yeni veritabanları için gerektiği şekilde taşımak için Aracı'nı kullanın.

**Büyümeye Böl** 

Patlayıcı büyüme işlemek için genel kapasitesini artırmak gerekir. Bunu yapmak için ek kapasite parçalama veri ve kapasite gereksinimleri karşılandıktan kadar artımlı olarak daha fazla veritabanı arasında dağıtma tarafından oluşturun. Bu, 'bölme' özelliğinin prime bir örnektir. 

**Daraltmak için birleştirme**

Kapasite bir iş Mevsimlik yapısı nedeniyle Küçült. Aracı için daha az ölçek birimleri ölçek iş yavaşlatır zaman sağlar. Esnek ölçek bölme birleştirme hizmetindeki 'merge' özelliği bu gereksinim kapsar. 

**Etkin noktalarına shardlets taşıyarak yönetme**

Veritabanı başına birden çok Kiracı ile kapasite darboğazları üzerinde bazı parça parça shardlets ayırma yol açabilir. Bu, shardlets yeniden ayırma veya yeni veya daha az kullanılan parça meşgul shardlets taşıma gerektirir. 

## <a name="concepts--key-features"></a>Kavramları ve anahtar özellikler
**Müşteri barındırılan hizmetleri**

Bölünmüş birleştirme müşteri tarafından barındırılan bir hizmet olarak teslim edilir. Dağıtmak ve Microsoft Azure aboneliğiniz hizmetini barındırır. Nuget'ten yükleme paketi belirli dağıtımınız için bilgilerle tamamlamak için bir yapılandırma şablonu içerir. Bkz: [bölünmüş birleştirme öğretici](sql-database-elastic-scale-configure-deploy-split-and-merge.md) Ayrıntılar için. Azure aboneliğinizde hizmeti çalışır olduğundan, denetlemek ve hizmet çoğu güvenlik özelliklerini yapılandırın. Varsayılan şablonu SSL istemci sertifikası tabanlı kimlik doğrulaması, şifreleme depolanan kimlik bilgileri, kullanılan koruyarak DoS ve IP kısıtlamalarını yapılandırmak için seçenekleri içerir. Aşağıdaki belgede güvenlik konuları hakkında daha fazla bilgi bulabilirsiniz [bölünmüş birleştirme Güvenlik Yapılandırması](sql-database-elastic-scale-split-merge-security-configuration.md).

Varsayılan hizmeti çalıştığında bir çalışan ve bir web rolü ile birlikte dağıtılır. Her Azure Cloud Services A1 VM boyutu kullanır. Paket dağıtımı sırasında bu ayarları değiştiremezsiniz, ancak (Azure Portal'dan) çalışan bulut hizmeti başarılı bir dağıtım sonrasında değiştirebilirsiniz. Çalışan rolü birden çok tek bir örnek için teknik nedenlerle yapılandırılmamalıdır olduğunu unutmayın. 

**Parça eşleme tümleştirme**

Bölünmüş birleştirme hizmetine uygulama parça eşleme ile etkileşime girer. Bölünmüş birleştirme hizmeti ayırmak veya aralıklar birleştirme veya shardlets parça arasında taşımak için kullanırken, hizmetin otomatik olarak parça eşleme güncel tutar. Bunu yapmak için hizmet uygulama parça eşleme manager veritabanına bağlar ve aralıkları ve eşlemeleri birleştirme/bölünmüş/taşıma isteği ilerleme durumunu korur. Bu bölme birleştirme işlemleri yükleyecekseniz parça eşleme her zaman güncel bir görünüm sunar sağlar. Bölme, birleştirme ve shardlet taşıma işlemleri shardlets toplu kaynak parça hedef parça taşıyarak uygulanır. Shardlet taşıma işlemi sırasında geçerli toplu işlem tabi shardlets parça eşlemesinde çevrimdışı olarak işaretlenir ve kullanarak veri bağımlı yönlendirme bağlantılarında kullanılamaz **OpenConnectionForKey** API. 

**Tutarlı shardlet bağlantıları**

Veri taşıma shardlets yeni toplu için başladığında shardlet depolama parça parça eşleme sağlanan veri bağımlı yönlendirme bağlantılarına kli ve sonraki API'leri parça eşlemesinden bu bağlantılardır shardlets veri taşımayı Tutarsızlıklardan kaçınmak için işlemi devam ederken engellenir. Diğer shardlets aynı parça üzerinde de sonlandırıldı, ancak yeniden başarılı olur hemen üzerinde yeniden deneyin. Toplu taşındıktan sonra shardlets yeniden hedef parça için çevrimiçi olarak işaretlenir ve veri kaynağını kaynak parça kaldırılır. Tüm shardlets taşınana kadar hizmet her toplu işlemi için bu adımları geçer. Bu, tam birleştirme/bölünmüş/taşıma işlemi sürecinde birkaç bağlantı sonlandırma işlemleri neden olacaktır.  

**Shardlet kullanılabilirlik yönetme**

Yukarıda açıklanan şekilde shardlets geçerli yığını sonlandırılması bağlantı sınırlaması kullanılamazlık kapsamını aynı anda shardlets bir toplu iş kısıtlar. Bu bir yaklaşım tam parça bölme veya birleştirme işlemi sürecinde burada kendi shardlets için çevrimdışı kalarak tercih edilir. Bir kerede taşımak için ayrı shardlets sayısı olarak tanımlanan bir toplu bir yapılandırma parametresi boyutudur. Her bölme ve birleştirme işlemi uygulamanın kullanılabilirliğini ve performansını gereksinimlerine bağlı olarak tanımlanabilir. Parça eşlemesinde kilitli aralığın belirtilen toplu boyuttan büyük olabileceğine dikkat edin. Gerçek veri anahtar değerlerini parçalama yaklaşık toplu iş boyutu sayısıyla eşleştiğini şekilde hizmet aralık boyutu Çekmeleri olmasıdır. Bu, özellikle seyrek doldurulmuş parçalama tuşları unutmamak önemlidir. 

**Meta veri depolama**

Bölünmüş birleştirme hizmet durumunu korumak için ve günlükleri isteği işleme sırasında tutmak için bir veritabanı kullanır. Kullanıcı kendi abonelikte bu veritabanı oluşturur ve bağlantı dizesi için hizmet dağıtımı için yapılandırma dosyası sağlar. Kullanıcının kuruluş yöneticileri aynı zamanda isteği ilerleme durumunu incelemek ve olası hataları ile ilgili ayrıntılı bilgileri incelemek için bu veritabanına bağlanabilir.

**Parçalama tanıma**

Bölünmüş birleştirme hizmetine (1) parçalı tabloları, (2) başvuru tabloları ve (3) normal tablolar arasında ayırır. Bir birleştirme/bölünmüş/taşıma işlemi semantiği kullanılan tablo türüne bağlıdır ve şu şekilde tanımlanır: 

* **Parçalı tabloları**: bölünmüş, birleştirme ve taşıma işlemleri taşıyın shardlets kaynaktan hedef parça. Genel istek başarıyla tamamlandıktan sonra bu shardlets artık kaynağında yok. Hedef tablolar üzerinde hedef parça mevcut gerekir ve veri işleme işleminin önce hedef aralıktaki içermemelidir unutmayın. 
* **Başvuru tabloları**: başvuru tabloları, bölünmüş birleştirme ve kopyalama işlemlerini verileri kaynak sunucudan hedef parça taşıyın. Ancak, herhangi bir satırın hedef bu tablodaki zaten herhangi bir değişiklik belirli bir tablodaki hedef parça üzerinde ortaya olduğunu unutmayın. Tablo işlenen tüm başvuru tablosu kopyalama işlemi için boş olması gerekir.
* **Diğer tablolar**: kaynak veya hedef bölme ve birleştirme işleminin diğer tablolarla mevcut olabilir. Bölünmüş birleştirme hizmetine herhangi bir veri taşıma veya kopyalama işlemleri için bu tablolar yok sayar. Ancak, bunlar bu işlemleri kısıtlamaları durumunda olan etkileyebilir unutmayın.

Parçalı tablolar ve başvuru bilgileri tarafından sağlanan **SchemaInfo** API'leri parça harita üzerinde. Aşağıdaki örnekte verilen parça eşleme Yöneticisi nesnesi smm bu API'leri kullanımını göstermektedir: 

    // Create the schema annotations 
    SchemaInfo schemaInfo = new SchemaInfo(); 

    // Reference tables 
    schemaInfo.Add(new ReferenceTableInfo("dbo", "region")); 
    schemaInfo.Add(new ReferenceTableInfo("dbo", "nation")); 

    // Sharded tables 
    schemaInfo.Add(new ShardedTableInfo("dbo", "customer", "C_CUSTKEY")); 
    schemaInfo.Add(new ShardedTableInfo("dbo", "orders", "O_CUSTKEY")); 

    // Publish 
    smm.GetSchemaInfoCollection().Add(Configuration.ShardMapName, schemaInfo); 

Tablo 'bölge' ve 'ulus' başvuru tabloları tanımlanır ve birleştirme/bölünmüş/taşıma işlemleriyle kopyalanacak. sırayla 'Müşteri' ve 'Siparişler' parçalı tablo olarak tanımlanır. C_CUSTKEY ve O_CUSTKEY parçalama anahtarı olarak görev yapar. 

**Bilgi tutarlılığı**

Bölünmüş birleştirme hizmetine tabloları arasındaki bağımlılıkları analiz eder ve başvuru tabloları ve shardlets taşımak için işlem hazırlamak için yabancı anahtar birincil anahtar ilişkileri kullanır. Genel olarak, shardlets bağımlılıklarını her yığın içindeki sırasına kopyalanır sonra başvuru tabloları ilk bağımlılık sırayla kopyalanır. Böylece yeni veri ulaştığında hedef parça FK PK kısıtlamalar dikkate alınır, bu gereklidir. 

**Parça eşleme tutarlılık ve nihai tamamlama**

Hatalar, söz konusu olduğunda bölünmüş birleştirme hizmet işlemleri herhangi kesinti sonra devam eder ve ilerleme isteklerinde tamamlayın amaçlar. Ancak, kurtarılamaz durumlar olabilir, örneğin, hedef parça kaybolduğunda veya onarım tehlikeye. Bu koşullar altında kaynak parça bulunmasını taşınması gereken bazı shardlets devam edebilir. Hizmetin gerekli verileri hedef başarıyla kopyalandı sonra shardlet eşlemeleri yalnızca güncelleştirilmesini sağlar. Shardlets yalnızca kaynağında tüm verilerini, hedef kopyalanan ve karşılık gelen eşlemeleri başarıyla güncelleştirdiniz silinir. Aralık zaten hedef parça çevrimiçi durumdayken silme işlemi arka planda gerçekleşir. Bölünmüş birleştirme hizmeti her zaman parça eşlemesinde depolanan eşlemeleri doğruluk sağlar.

## <a name="the-split-merge-user-interface"></a>Bölünmüş birleştirme kullanıcı arabirimi
Bölünmüş birleştirme hizmet paketi çalışan rolü ve bir web rolü içerir. Web rolü, etkileşimli bir şekilde bölünmüş birleştirme istekleri göndermek için kullanılır. Kullanıcı arabirimi ana bileşenlerini aşağıdaki gibidir:

* İşlem türü: Bu istek için hizmeti tarafından gerçekleştirilen işlem türü denetleyen radyo düğmesi işlemi türüdür. Bölme arasındaki seçin, birleştirme ve taşıma senaryoları. Ayrıca, daha önce gönderilen işlemi iptal edebilirsiniz. Bölünmüş kullanın, birleştirme ve aralığı parça eşlemeleri için istekleri taşıyın. Liste parça yalnızca destek taşıma işlemleri eşler.
* Parça eşleyin: İstek parametreleri sonraki bölümü parça eşleme ve parça haritanızı barındırma veritabanı hakkındaki bilgileri kapsar. Özellikle, Azure SQL veritabanı sunucusunun adını ve veritabanı shardmap parça eşleme veritabanı ve parça eşleme adını son olarak bağlanmak için kimlik bilgilerini barındırma sağlamanız gerekir. Şu anda işlemi yalnızca tek bir kimlik bilgileri kümesi kabul eder. Bu kimlik bilgileri parça parça eşleme için kullanıcı verilerini de değişiklikleri gerçekleştirmek için yeterli izinlere sahip olmanız gerekir.
* Kaynak (bölme ve birleştirme) aralığı: bölme ve birleştirme işlemi, düşük ve yüksek anahtarı kullanarak bir aralık işler. Bir işlem ile sınırsız yüksek anahtar değeri belirtmek için "yüksek anahtar max" onay kutusunu işaretleyin ve yüksek anahtar alanı boş bırakın. Belirttiğiniz aralık anahtar değerleri tam olarak bir eşleme ve kendi sınırları içinde parça haritanızı eşleşmesi gerekmez. Tüm aralığı sınırları hiç belirtmezseniz, hizmet yakın aralığı sizin için otomatik olarak Infer. Verilen parça eşleme geçerli eşlemelerin almak için GetMappings.ps1 PowerShell betiğini kullanabilirsiniz.
* Bölünmüş kaynak davranış (bölme): bölünmüş işlemleri için kaynak aralığı bölüneceği noktasını tanımlayın. Bunun için gerçekleşmesi için bölünmüş istediğiniz parçalama anahtarı sağlayarak. Kullan radyo düğmesi olup (bölme anahtar hariç) aralığı alt kısmı taşımak istediğiniz ya da (bölme anahtar dahil) taşımak için üst kısmındaki isteyip istemediğinizi belirtin.
* Kaynak Shardlet (taşıma): kaynağını tanımlamak için bir aralığı gerektirmeyen gibi işlemleri bölme veya birleştirme işlemlerini farklı taşıyın. Taşıma için bir kaynak yalnızca taşımayı planlıyorsanız parçalama anahtar değeri tarafından tanımlanır.
* Hedef parça (bölme): bölme işlemi kaynağında bilgileri girdikten sonra hedef Azure SQL Db sunucusu ve veritabanı adını sağlayarak kopyalanmasını verilerin istediğiniz tanımlamanız gerekir.
* Hedef aralık (merge): birleştirme işlemleri için varolan bir parça shardlets taşıyın. Varolan parça ile birleştirmek istediğiniz varolan aralığının aralığı sınırları sağlayarak tanımlayın.
* Toplu iş boyutu: Toplu iş boyutu veri taşıma sırasında zaman çevrimdışı duruma shardlets sayısını denetler. Burada shardlets için kapalı kalma süresi dönemlerini uzun hassas olduğunda küçük değerler kullanabilirsiniz bir tamsayı değeri budur. Daha büyük değerler verilen shardlet süreyi artıracaktır çevrimdışı ancak performansı iyileştirebilir.
* İşlem kimliği (İptal): artık gerekli olmadığında devam eden bir işlem varsa, işlemi, işlem kimliği bu alandaki sağlayarak iptal edebilirsiniz. İstek durumu tablosundan işlem kimliği alabilirsiniz (Bölüm 8.1 bakın) veya istek gönderildiği web tarayıcısında çıktısından.

## <a name="requirements-and-limitations"></a>Gereksinimler ve sınırlamalar
Geçerli bölünmüş birleştirme hizmetine aşağıdaki gereksinimleri ve sınırlamaları tabi uygulamasıdır: 

* Parça var ve bu parça üzerinde bir bölme birleştirme işlemi gerçekleştirilmeden önce parça eşlemesinde kayıtlı olması gerekir. 
* Hizmet, tablolar veya başka bir veritabanı nesneleri işlemlerinin bir parçası olarak otomatik olarak oluşturmaz. Bu, hedef parça herhangi bir birleştirme/bölünmüş/taşıma işlemi önce mevcut tüm parçalı tablolar ve başvuru tabloları için şema gerektiği anlamına gelir. Parçalı tablolar özellikle bir birleştirme/bölünmüş/taşıma işlemi tarafından eklenecek yeni shardlets nerede aralıktaki boş olması gerekir. Aksi takdirde işlem hedef parça üzerinde ilk tutarlılık denetimi başarısız olur. Ayrıca, bu başvuruyu başvuru tablosu boştur ve tutarlılığı garanti diğer eşzamanlı açısından olduğunu yazma işlemi başvuru tabloları veri yalnızca kaynaklanıyorsa unutmayın. Bu öneririz: bölünmüş/birleştirme işlemleri çalışırken, başka bir yazma işlemleri için başvuru tabloları değişiklikleri yapın.
* Hizmet bir benzersiz dizin veya performansı ve güvenilirliği büyük shardlets artırmak için parçalama anahtar içerir anahtar tarafından oluşturulan satır kimliği kullanır. Bu, yalnızca parçalama anahtar değerinden daha hassas bir ayrıntı düzeyi, verileri taşımak hizmet sağlar. Bu günlük alanının ve işlem sırasında gerekli kilitler maksimum azaltılmasına yardımcı olur. Benzersiz bir dizin ya da bu tabloyu birleştirme/bölünmüş/taşıma isteği ile kullanmak istiyorsanız, belirli bir tablodaki parçalama anahtarı da dahil olmak üzere bir birincil anahtar oluşturmayı düşünün. Performans nedenleriyle parçalama anahtar anahtar veya dizin başında sütunu olmalıdır.
* İstek işleme sürecinde bazı shardlet veriler hem kaynak ve hedef parça mevcut olabilir. Bu shardlet taşıma sırasında arızalarına karşı korunmak gereklidir. Bölünmüş birleştirme parça eşleme ile tümleştirme sağlar bağımlı yönlendirme API'lerini kullanarak verileri aracılığıyla bağlantıları **OpenConnectionForKey** yöntemi parça harita üzerinde tutarsız bir ara durum göremezsiniz. Ancak, kaynak veya hedef parça kullanmadan bağlanırken **OpenConnectionForKey** yöntemi, tutarsız Ara durumlar olabilir görünür birleştirme/bölünmüş/taşıma isteği yükleyecekseniz. Bu bağlantıları, kısmi veya yinelenen sonuçları zamanlama veya arka plandaki bağlantı parça bağlı olarak gösterebilir. Bu sınırlama, şu anda esnek ölçek çok-Shard-sorgular tarafından yapılan bağlantılar içerir.
* Bölünmüş birleştirme hizmet için meta veri veritabanı farklı rolleri arasında paylaşılmayan gerekir. Örneğin, hazırlamada çalışan bölünmüş birleştirme hizmeti rolü üretim rol daha farklı meta veriler veritabanına işaret etmesi gerekir.

## <a name="billing"></a>Faturalandırma
Bölünmüş birleştirme hizmeti Microsoft Azure aboneliğiniz bulut hizmetinde çalışır. Bu nedenle ücretleri bulut Hizmetleri için hizmet Örneğiniz için geçerlidir. Birleştirme/bölünmüş/taşıma işlemleri sık gerçekleştirmediğiniz sürece, bölünmüş birleştirme bulut hizmetini silin öneririz. Çalışan maliyetlerini kaydeder veya Bulut hizmeti örnekleri dağıtılır. Yeniden dağıtın ve bölme veya birleştirme işlemlerini gerçekleştirmek ihtiyaç duyduğunuzda taşımalarına runnable yapılandırmanızı başlatın. 

## <a name="monitoring"></a>İzleme
### <a name="status-tables"></a>Durum tabloları
Bölünmüş birleştirme hizmetidir **RequestStatus** tamamlanmış ve devam eden isteklerinin izleme meta veri deposu veritabanı tablosunda. Tablo, bu bölme birleştirme hizmeti örneğine gönderilen her bölme birleştirme isteği için bir satır listeler. Her istek için aşağıdaki bilgileri sağlar:

* **Zaman damgası**: İstek zaman başlatıldı tarih ve saat.
* **Operationıd**: İstek benzersiz olarak tanımlayan bir GUID. Bu istek ederken hala devam eden işlemi iptal etmek için de kullanılabilir.
* **Durum**: isteğin geçerli durumu. Devam eden istekler için istek olduğu geçerli aşama da listeler.
* **CancelRequest**: istek iptal olup olmadığını belirten bir bayrak.
* **İlerleme**: işlem için tamamlanma yüzdesi tahmin. Bir değeri 50 işlemi yaklaşık %50 tamamlanmış olduğunu gösterir.
* **Ayrıntılar**: ayrıntılı ilerleme raporu sağlar XML değeri. Satır kümeleri kaynaktan hedefe kopyalanır gibi İlerleme raporu düzenli olarak güncelleştirilir. Hataları veya özel durumları durumunda, bu sütun ayrıca hata hakkında daha ayrıntılı bilgi içerir.

### <a name="azure-diagnostics"></a>Azure Tanılama
Bölünmüş birleştirme hizmeti Azure Tanılama izleme ve tanılama için Azure SDK 2.5 üzerinde temel kullanır. Burada açıklandığı gibi Tanılama yapılandırmasını kontrol: [Azure Cloud Services ve sanal makineleri etkinleştirme tanılamada](../cloud-services/cloud-services-dotnet-diagnostics.md). İndirme paketini iki tanılama yapılandırması - bir web rolü ve çalışan rolü için bir tane içerir. Bu tanılama yapılandırmalar hizmeti için kılavuzluğu izleyin [Microsoft Azure bulut hizmeti temel bilgileri](https://code.msdn.microsoft.com/windowsazure/Cloud-Service-Fundamentals-4ca72649). Performans sayaçları, IIS günlükleri, Windows olay günlüklerini ve bölünmüş birleştirme uygulama olay günlüklerini oturum için tanımları içerir. 

## <a name="deploy-diagnostics"></a>Tanılama dağıtma
İzleme ve tanılama NuGet paketi tarafından sağlanan web ve çalışan rolleri için tanılama Yapılandırması'nı kullanarak etkinleştirmek için Azure PowerShell kullanarak aşağıdaki komutları çalıştırın: 

    $storage_name = "<YourAzureStorageAccount>" 

    $key = "<YourAzureStorageAccountKey" 

    $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key  


    $config_path = "<YourFilePath>\SplitMergeWebContent.diagnostics.xml" 

    $service_name = "<YourCloudServiceName>" 

    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWeb" 


    $config_path = "<YourFilePath>\SplitMergeWorkerContent.diagnostics.xml" 

    $service_name = "<YourCloudServiceName>" 

    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWorker" 

Burada tanılama ayarlarını yapılandırmak ve dağıtmak hakkında daha fazla bilgi bulabilirsiniz: [Azure Cloud Services ve sanal makineleri etkinleştirme tanılamada](../cloud-services/cloud-services-dotnet-diagnostics.md). 

## <a name="retrieve-diagnostics"></a>Tanılama alma
Sunucu Gezgini ağacının Azure bölümü Visual Studio sunucu Gezgini'nde, tanılama kolayca erişebilirsiniz. Visual Studio örneğini açın ve menü çubuğunda Görünüm ve Server Explorer'ı tıklatın. Azure aboneliğinize bağlanmak için Azure simgesine tıklayın. Azure'a gidin depolama -> -> <your storage account> -> tabloları WADLogsTable ->. Daha fazla bilgi için bkz: [Sunucu Gezgini ile depolama kaynaklarını gözatma](http://msdn.microsoft.com/library/azure/ff683677.aspx). 

![WADLogsTable][2]

Yukarıdaki çizimde vurgulanan WADLogsTable ayrıntılı bölünmüş birleştirme hizmetin uygulama günlüğündeki olaylar içerir. İndirilen paketi varsayılan yapılandırması Üretim dağıtımı sağlamıştır olduğunu unutmayın. Bu nedenle, günlükleri ve sayaçları hizmet örneklerden alınır büyük (5 dakika) aralığıdır. Test ve geliştirme için tanılama ayarları web veya çalışan rolü gereksinimlerinize ayarlayarak aralığını azaltın. Visual Studio Sunucu Gezgini (yukarıya bakın) rolüne sağ tıklayın ve ardından tanılama yapılandırma ayarları iletişim kutusunda aktarım süresini ayarlamak: 

![Yapılandırma][3]

## <a name="performance"></a>Performans
Genel olarak, daha iyi performans arttıkça, daha fazla kullanıcı hizmet katmanlarındaki Azure SQL veritabanında olması beklenir. Daha yüksek hizmet katmanları için daha yüksek g/ç, CPU ve bellek ayırmaları toplu kopyalama yararlanabilir ve bölünmüş birleştirme hizmetinin kullandığı işlemleri silin. Bu nedenle, yalnızca bu veritabanları için hizmet katmanına bir tanımlanan, sınırlı süre artırın.

Hizmet ayrıca doğrulama sorguları normal işlemlerinin bir parçası olarak gerçekleştirir. Bu doğrulama sorgular hedef aralığındaki verileri beklenmeyen varlığını denetlemek ve herhangi bir birleştirme/bölünmüş/taşıma işlemi ile tutarlı bir durumdan başladığından emin olmak. Tüm bu sorguları parçalama anahtar aralıklarına işlemi kapsam tarafından tanımlanan ve istek tanımının bir parçası olarak sağlanan toplu iş boyutu üzerinde çalışır. Dizin başında sütun parçalama anahtara sahip mevcut olduğunda bu sorguları en iyi şekilde çalışır. 

Ayrıca, bir Benzersizlik özelliği parçalama anahtarla başında sütun olarak günlük alanının ve bellek açısından kaynak tüketimini sınırlandırır en iyi duruma getirilmiş bir yaklaşım kullanmak hizmet izin verir. Bu benzersizlik özelliği (genellikle yukarıdaki 1 GB) büyük veri boyutları taşımak için gereklidir. 

## <a name="how-to-upgrade"></a>Yükseltme
1. Adımları [bölünmüş birleştirme hizmet dağıtma](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
2. Bulut hizmeti yapılandırma dosyanızı yeni yapılandırma parametreleri yansıtacak şekilde bölünmüş birleştirme dağıtımınız için değiştirin. Yeni gerekli parametre şifreleme için kullanılan sertifika hakkındaki bilgilerdir. Bunu yapmak için kolay bir indirme varolan yapılandırmanızı karşı yeni yapılandırma şablon dosyasından Karşılaştırılacak yoludur. Hem web hem de çalışan rolü için "DataEncryptionPrimaryCertificateThumbprint" ve "DataEncryptionPrimary" ayarlarını eklediğinizden emin olun.
3. Güncelleştirme Azure'a dağıtmadan önce tüm çalışmakta bölünmüş birleştirme işlemlerinin tamamlandığından emin olun. Bu, devam eden istekleri için bölünmüş birleştirme meta veri veritabanındaki RequestStatus ve PendingWorkflows tabloları sorgulayarak kolayca yapabilirsiniz.
4. Mevcut bulut hizmeti dağıtımınızı Azure aboneliğinizde bölünmüş birleştirme için yeni paket ve güncelleştirilmiş hizmet yapılandırma dosyanızı güncelleştirin.

Yükseltme bölünmüş-birleştirme için yeni bir meta veri veritabanı sağlamak gerekmez. Yeni sürümü, yeni sürüme varolan meta verileri veritabanınızı otomatik olarak yükseltir. 

## <a name="best-practices--troubleshooting"></a>En iyi yöntemler ve sorun giderme
* Birden fazla parça en önemli birleştirme/bölünmüş/taşıma işlemlerinizin test kiracısı ile çalışma ve test Kiracı tanımlayın. Tüm meta veriler parça eşlemesinde doğru şekilde tanımlandığından ve işlemleri kısıtlamaları veya yabancı anahtarlar ihlal etmemek emin olun.
* Test Kiracı tutmak, değil karşılaşmadan veri boyutu emin olmak için en büyük Kiracı maksimum veri boyutu yukarıda veri boyutu ile ilgili sorunlar. Bu, tek bir kiracı taşımak için geçen süreyi üzerinde bir üst sınır değerlendirmenize yardımcı olur. 
* Şemanızı silme izin verdiğinden emin olun. Bölünmüş birleştirme hizmetine verileri hedef başarıyla kopyalandı sonra veri kaynağı parça kaldırma özelliği gerektirir. Örneğin, **Tetikleyicileri silme** hizmetin veri kaynağı üzerinde silmesini önlemek ve işlemleri başarısız olmasına neden olabilir.
* Parçalama anahtar başında sütun birincil anahtar veya benzersiz dizin tanımı içinde olmalıdır. Bölünmüş veya birleştirme doğrulama sorgular için ve her zaman parçalama anahtar aralıklarına üzerinde çalışan gerçek veri taşıma ve silme işlemleri için en iyi performans sağlar.
* Bölünmüş birleştirme hizmetinizi veritabanlarınızı bulunduğu bölgeye ve veri merkezi içinde bir arada. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-overview-split-and-merge/split-merge-overview.png
[2]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics.png
[3]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics-config.png

