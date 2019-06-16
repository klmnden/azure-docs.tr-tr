---
title: Ölçeği genişletilen bulut veritabanları arasında veri taşıma | Microsoft Docs
description: Parçalar işlemek ve elastik veritabanı API'lerini kullanarak şirket içinde barındırılan bir hizmete veri taşıma açıklanmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: 2127c05d7e52b0103d91ecfac4fb5977a4815f31
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66123373"
---
# <a name="moving-data-between-scaled-out-cloud-databases"></a>Ölçeği genişletilen bulut veritabanları arasında veri taşıma

Bir hizmet geliştirici olarak bir yazılım olan ve uygulamanızı Bilim insanları için inanılmaz isteğe aniden uğradığında, büyümeye uyum sağlamak gerekir. Bu nedenle daha fazla veritabanları (parçalar) ekleyin. Yeni veritabanları için veriler nasıl dağıtabilir veri bütünlüğü kesintiye uğratmadan? Kullanım **bölme-birleştirme aracını** verileri kısıtlanmış veritabanlarından yeni veritabanlarını taşımak için.  

Ayırma-Birleştirme aracı, bir Azure web hizmeti olarak çalışır. Bir yöneticinizle veya geliştiricinizle parçacıklara (parça verileri) farklı veritabanları (parçalar) arasında taşımak için Aracı'nı kullanır. Aracı parça eşleme Yönetimi Hizmeti meta verileri veritabanı koruyup tutarlı eşlemeleri sağlamak için kullanır.

![Genel Bakış][1]

## <a name="download"></a>İndirme

[Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/)

## <a name="documentation"></a>Belgeler

1. [Elastik veritabanı bölme-Birleştirme aracı Öğreticisi](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
2. [Ayırma-birleştirme güvenliği yapılandırma](sql-database-elastic-scale-split-merge-security-configuration.md)
3. [Ayırma-birleştirme güvenliği konuları](sql-database-elastic-scale-split-merge-security-configuration.md)
4. [Parça eşleme yönetimi](sql-database-elastic-scale-shard-map-management.md)
5. [Ölçeği genişletilen mevcut veritabanlarını geçirme](sql-database-elastic-convert-to-use-elastic-tools.md)
6. [Esnek veritabanı araçları](sql-database-elastic-scale-introduction.md)
7. [Esnek veritabanı araçları sözlüğü](sql-database-elastic-scale-glossary.md)

## <a name="why-use-the-split-merge-tool"></a>Neden bölme-birleştirme aracını kullanma

- **Esneklik**

  Uygulamalar esnek bir şekilde tek bir Azure SQL DB veritabanı sınırları aşacak esnetme gerekir. Veri bütünlüğü koruyarak yeni veritabanları için gerektiği şekilde taşımak için Aracı'nı kullanın.

- **Büyüme için Böl**

  Büyümeleri işlemek için genel kapasitesini artırmak için veri parçalama tarafından ek kapasite oluşturmak ve kapasite gerekir kadar kademeli olarak daha fazla veritabanları arasında dağıtarak yerine. Bu bir birinci örneğidir **bölme** özelliği.

- **Daraltmak için birleştirme**

  Kapasite, bir iş dönemsel niteliği nedeniyle Daralt. Aracı için daha az ölçek birimleri iş yavaşlatır, ölçeği sağlar. Bu gereksinim 'merge' özelliğinde esnek ölçek bölme-birleştirme hizmetini kapsar.

- **Parçacıklara taşıyarak etkin noktaları yönetme**

  Veritabanı başına birden çok kiracıyla parçacıklara parçalara ayırma kapasitesini darboğazları bazı parçalar üzerinde neden olabilir. Bu parçacıklarda yeniden ayırma veya meşgul parçacıklara yeni veya daha az kullanılan parçalara taşıma gerektirir.

## <a name="concepts--key-features"></a>Kavramlar ve önemli özellikler

- **Müşteri tarafından barındırılan hizmetleri**

  Ayırma-birleştirme, müşteri tarafından barındırılan bir hizmet olarak teslim edilir. Dağıtma ve Microsoft Azure aboneliğiniz hizmeti barındırma gerekir. Nuget'ten indirme paketi, belirli bir dağıtım için bilgi ile tamamlamak için bir yapılandırma şablonu içerir. Bkz: [bölme-birleştirme öğretici](sql-database-elastic-scale-configure-deploy-split-and-merge.md) Ayrıntılar için. Azure aboneliğinizdeki hizmetin çalıştığından kontrol edebilir ve hizmet çoğu güvenlik özelliklerini yapılandırın. Varsayılan şablonu SSL istemci sertifikası tabanlı kimlik doğrulaması, şifreleme için depolanan kimlik bilgileri, kullanılan koruyarak DoS ve IP kısıtlamalarını yapılandırmak için seçenekleri içerir. Güvenlik konuları hakkında daha fazla bilgi belgesinde bulabilirsiniz [ayırma-birleştirme güvenliği yapılandırma](sql-database-elastic-scale-split-merge-security-configuration.md).

  Varsayılan bir çalışan ve bir web rolü hizmeti çalıştığında dağıtıldı. Her Azure bulut Hizmetleri'nde A1 VM boyutunu kullanır. Paket dağıtımı sırasında bu ayarları değiştiremezsiniz, ancak çalışan bulut hizmetini (Azure portalından) başarılı bir dağıtım sonrasında değiştirebilir. Çalışan rolü için birden çok tek bir örnek teknik nedenlerle yapılandırılmamalıdır olduğunu unutmayın.

- **Parça Haritası tümleştirmesi**

  Ayırma-birleştirme hizmetini uygulamanın parça eşlemesi ile etkileşime girer. Ayırma-birleştirme hizmeti aralıklarını ayırmak veya birleştirmek veya parçacıklara parçalar arasında taşımak için kullanılırken, hizmeti otomatik olarak parça eşlemesi güncel tutar. Bunu yapmak için hizmet uygulamasının parça eşleme Yöneticisi veritabanına bağlanır ve aralıkları ve eşlemeleri Birleştir/Böl/taşıma istekler devam ettikçe tutar. Bu bölme-birleştirme işlemleri bittiğini tıklattığınızda parça eşlemesini her zaman güncel bir görünüm sunan sağlar. Bölme, birleştirme ve parçacık taşıma işlemleri parçacıklara toplu kaynak parçadan veri hedef parçaya taşıyarak uygulanır. Parçacık taşıma işlemi sırasında geçerli toplu tabi parçacıklara parça eşlemesinde çevrimdışı olarak işaretlenir ve kullanarak verilere bağımlı yönlendirme bağlantılarında kullanılamaz **OpenConnectionForKey** API.

- **Tutarlı parçacık bağlantıları**

  Veri taşıma parçacıklara yeni toplu için başladığında, herhangi bir parça eşlemesini verilere bağımlı yönlendirme bağlantıları parçacık sonlandırılan parça depolamak için ve olsa da veri taşımayı parçacıklara API'lerine engellenen parça eşlemesinden sonraki bağlantılar sağlanmaktadır. tutarsızlıkları önlemek için devam ediyor. Başka parçacıklara işaret aynı parça üzerindeki bağlantı da sonlandırılan, ancak yeniden başarılı olur hemen üzerinde yeniden deneyin. Batch taşındıktan sonra parçacıklara yeniden hedef parça için çevrimiçi işaretlenmiş ve kaynak veri kaynağı parçadan veri kaldırılır. Tüm parçacıklarda taşınana kadar adımları her batch hizmeti geçer. Bu, tam Birleştir/Böl/taşıma işleminin Kurs sırasında birkaç bağlantı sonlandırma işlemleri önünü açacak.  

- **Parçacık kullanılabilirliğini yönetme**

  Yukarıda da tartışıldığı parçacıklara geçerli dizi için sonlandırmayı bağlantı sınırlaması kullanılamazlık kapsamını teker teker parçacıklara bir toplu iş kısıtlar. Bu bir yaklaşım üzerinde tam parça bir bölme ve birleştirme işleminin Kurs sırasında burada kendi parçacıklara için çevrimdışı kalır tercih edilir. Bir anda taşımak için ayrı parçacıklara sayısı olarak tanımlanan bir toplu işin boyutunu yapılandırma parametredir. Uygulamanın, kullanılabilirlik ve performans gereksinimlerine bağlı olarak her bölme ve birleştirme işlemi için tanımlanabilir. Parça eşlemesinde kilitli aralığın belirtilen toplu iş boyutu büyük olabileceğine dikkat edin. Veri parçalama anahtarı değerleri gerçek sayısını yaklaşık olarak toplu iş boyutu şekilde hizmet aralık boyutu seçer olmasıdır. Bu, özellikle seyrek doldurulmuş parçalama tuşları unutmamak önemlidir.

- **Meta veri depolama**

  Ayırma-birleştirme hizmetini durumunu korumak için ve isteği işleme sırasında günlüklerini tutmak için bir veritabanı kullanır. Kullanıcı, abonelikte bu veritabanı oluşturur ve bağlantı dizesi için hizmet dağıtımı için yapılandırma dosyasında sağlar. Kullanıcının kuruluş yöneticileri ayrıca istek ilerlemeyi gözden geçirin ve olası hataları ile ilgili ayrıntılı bilgileri incelemek için bu veritabanına bağlanabilirsiniz.

- **Parçalama tanıma**

  Ayırma-birleştirme hizmetini (1) parçalı tablolar, (2) başvuru tabloları ve (3) normal tablolar arasında ayırır. Birleştir/Böl/taşıma işlemi semantiği kullanılan tablo türüne bağlıdır ve şu şekilde tanımlanır:

  - **Parçalı tabloları**

    Bölme ve birleştirme taşıma işlemlerini parçacıklara kaynaktan hedef parçaya taşıyın. Genel istek başarıyla tamamlandıktan sonra bu parçacıklarda artık kaynak yok. Hedef tablo üzerinde hedef parça olması gerekir ve veri işleme işleminin önce hedef aralıktaki içermemelidir unutmayın.

  - **Başvuru tabloları**

    Birleştirme ve taşıma işlemleri için başvuru tabloları, bölünmüş hedef parçaya veri kaynağından kopyalayın. Ancak, herhangi bir satır zaten hedef bu tablodaki mevcut herhangi bir değişiklik belirli bir tabloya ait hedef parça üzerinde olduğunda olduğunu unutmayın. Tablo işlenemedi hiçbir başvurusu Tablo kopyalama işlemi için boş olması gerekir.

  - **Diğer tablolar**

    Kaynak veya hedef bir bölme ve birleştirme işleminin diğer tablolara bulunabilir. Ayırma-birleştirme hizmetini herhangi bir veri taşıma veya kopyalama işlemleri için bu tablolar yok sayar. Ancak, bunlar kısıtlamaları durumunda bu işlemleri ile engelleyebilir unutmayın.

    Parçalı tablolar ve başvuru bilgileri tarafından sağlanan `SchemaInfo` parça eşlemesi API'leri. Aşağıdaki örnekte verilen parça eşleme Yöneticisi nesnesi üzerinde bu API'lerin kullanımını göstermektedir:

    ```c#
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
    ```

    Tablolar 'region' ve 'ulus' başvuru tabloları tanımlanır ve ile Birleştir/Böl/taşıma işlemlerini kopyalanır. 'Müşteri' ve 'Siparişler' sırayla parçalı tablo olarak tanımlanır. `C_CUSTKEY` ve `O_CUSTKEY` parçalama anahtarı olarak hizmet eder.

- **Bilgi tutarlılığı**

  Ayırma-birleştirme hizmetini tabloları arasındaki bağımlılıkları analiz eder ve hazırlamak için başvuru tabloları ve Parçacıkların taşıma işlemleri için birincil anahtarı yabancı anahtar ilişkilerini kullanır. Genel olarak, parçacıklara bağımlılıklarını her yığın içindeki sırasına göre kopyalanır sonra başvuru tabloları ilk bağımlılık sırayla kopyalanır. Böylece yeni veriler ulaştıkça hedef parça FK PK kısıtlamalar dikkate alınır, bu gereklidir.

- **Parça eşleme tutarlılık ve nihai tamamlama**

  Hatalar varsa ayırma-birleştirme hizmetini operations sonra herhangi bir kesinti sürdürür ve ilerleme isteklerinde tamamlayın amaçlar. Ancak, kurtarılamaz durumlar olabilir, örneğin, hedef parça kayıp ya da onarılamayacak şekilde tehlikeye. Bu koşullar altında taşınacak beklenen bazı parçacıklara kaynak parça üzerinde bulunan devam edebilir. Hizmet gerekli verileri hedefine başarıyla kopyalandıktan sonra parçacık eşlemeleri yalnızca güncelleştirilmesini sağlar. Hedef, tüm verilerin kopyalandığından ve karşılık gelen eşlemeleri başarıyla güncelleştirildi, parçacıklara kaynağında yalnızca silinir. Aralık zaten hedef parça üzerinde çevrimiçi durumdayken silme işlemi arka planda gerçekleşir. Ayırma-birleştirme hizmetini her zaman parça eşlemesinde depolanan eşlemeleri doğruluğunu sağlar.

## <a name="the-split-merge-user-interface"></a>Ayırma-birleştirme kullanıcı arabirimi

Ayırma-birleştirme hizmeti paketi, bir çalışan rolü ve bir web rolü içerir. Web rolü, etkileşimli bir şekilde ayırma-birleştirme isteklerini göndermek için kullanılır. Kullanıcı arabirimi ana bileşenleri aşağıdaki gibidir:

- **İşlem türü**

  Bu istek için hizmeti tarafından gerçekleştirilen işlem türünü denetleyen bir radyo düğmesi işlemi türüdür. Bölme arasındaki seçin, birleştirme ve taşıma senaryoları. Ayrıca, daha önce gönderilen işlemi iptal edebilirsiniz. Bölünmüş kullanın, birleştirme ve taşıma istekler için aralık parça eşlemesi. Liste parça yalnızca destek taşıma işlemlerini eşler.

- **Parça eşlemesi**

  İstek parametreleri, bir sonraki bölümüne parça eşlemesi ve veritabanı, parça eşlemesi barındırma yönelik bilgiler ele alınmaktadır. Özellikle, parça eşleme veritabanını ve parça eşlemesi adı için son bağlanmak için kimlik bilgilerini shardmap barındırma veritabanı ve Azure SQL veritabanı sunucusu adını vermeniz gerekir. Şu anda, işlemi yalnızca tek bir kimlik bilgileri kümesi kabul eder. Kullanıcı verileri için de parça eşlemesine değişiklikleri parçalar üzerinde gerçekleştirmek için yeterli izinlere sahip bu kimlik bilgileri gerekir.

- **Kaynak (bölme ve birleştirme) aralığı**

  Bir bölme ve birleştirme işlemi, düşük ve yüksek anahtarı kullanarak bir aralık işler. Bir işlem ile sınırsız yüksek anahtar değeri belirtmek için "yüksek anahtar en fazla bir" onay kutusunu işaretleyin ve yüksek anahtar alanı boş bırakın. Belirlediğiniz aralığın anahtar değerleri bir eşleme ve alt sınırları, parça eşlemesindeki tam olarak eşleşmesi gerekmez. Herhangi bir aralığı sınırlarını hiç belirtmezseniz, hizmetin en yakın aralığı sizin için otomatik olarak çıkarımlar. Verilen parça eşlemesi geçerli eşlemelerin alınacak GetMappings.ps1 PowerShell betiğini kullanabilirsiniz.

- **Bölünmüş kaynak davranışı (bölme)**

  Bölme işlemleri için kaynak aralığı bölmek için noktasını tanımlayın. Bölme gerçekleşmesini istediğiniz parçalama anahtarı sağlayarak bunu yapabilirsiniz. Radyo düğmesini kullanın olup (bölme anahtar hariç) aralığının alt bölümünde taşımak istediğiniz veya üst kısmındaki (bölme anahtar dahil) taşımak isteyip istemediğinizi belirtin.

- **Kaynak parçacık (taşıma)**

  Kaynağı tanımlamak için bir aralık gerektirmeyen gibi taşıma işlemlerini bölme ve birleştirme işlemlerinden farklıdır. Bir kaynak taşıma için yalnızca taşımayı planlıyorsanız parçalama anahtarı değeri tarafından tanımlanır.

- **Hedef parça (bölme)**

  Bölme işlemi kaynak bilgileri girdikten sonra verilerin Azure SQL veritabanı sunucusu ve hedef veritabanı adı sağlayarak kopyalanacağı istediğiniz tanımlamanız gerekir.

- **Hedef aralığı (birleştirme)**

  Mevcut bir parça Operations taşıma parçacıklara birleştirin. Var olan parça, birleştirmek istediğiniz varolan aralığın aralığı sınırlarını sağlayarak belirleyin.

- **Toplu iş boyutu**

  Toplu iş boyutu, veri taşıma sırasında bir zaman çevrimdışı duruma parçacıklarda sayısını denetler. Bu uzun parçacıklara kalma süreleri önemli olduğunda, küçük değerler kullanabileceğiniz bir tamsayı değeridir. Daha büyük değerler, belirli bir parçacık zamanı artırır ancak çevrimdışı performansı artırabilir.

- **İşlem kimliği (İptal)**

  Artık gerekli olmadığında, devam eden bir işlem varsa, bu alanı, işlem kimliği sağlayarak işlemi iptal edebilirsiniz. İşlem kimliği istek durumu tablosundan alabilirsiniz (8.1 bölümüne bakın) veya isteği gönderdiğiniz yeri web tarayıcısında çıktısından.

## <a name="requirements-and-limitations"></a>Gereksinimler ve sınırlamalar

Geçerli ayırma-birleştirme hizmetini aşağıdaki gereksinimleri ve sınırlamaları tabi uygulamasıdır:

- Parçalar var ve bu parçaları bir bölme-birleştirme işlemi gerçekleştirilmeden önce parça eşlemesinde kayıtlı olması gerekir.
- Hizmet, tablolar veya diğer veritabanı nesnelerini işlemlerini bir parçası olarak otomatik olarak oluşturulmaz. Bu, parçalı tüm tabloları ve başvuru tabloları için şema üzerinde herhangi bir bölünmüş/Birleştir/taşıma işlemi öncesinde hedef parça mevcut olması gerektiğini anlamına gelir. Parçalı tablo özellikle boş bir bölme/Birleştir/taşıma işlemiyle eklenecek yeni parçacıklara nerede aralığında olması gerekir. Aksi takdirde işlemi hedef parça ilk tutarlılık denetimi başarısız olur. Ayrıca, başvuru tablosu boştur ve diğer eşzamanlı onaylamaz tutarlılık garantisi yoktur yazma işlemi başvuru tabloları veri yalnızca kaynaklanıyorsa bu başvuruyu unutmayın. Bunu önermemizin: ayırma/birleştirme işlemleri çalışırken, başka bir yazma işlemleri için başvuru tabloları değişiklikleri yapın.
- Hizmet bir benzersiz dizin veya performansını ve güvenilirliğini büyük parçacıklara parçalama anahtarı içeren bir anahtar tarafından kurulan satır kimliği kullanır. Bu, yalnızca parçalama anahtarı değerinden daha bir ayrıntı düzeyi, verileri taşımak hizmet verir. Bu, en fazla günlük alanı ve işlem sırasında gerekli kilitler azaltmaya yardımcı olur. Benzersiz bir dizin ya da bu tabloyu Birleştir/Böl/taşıma istekler ile kullanmak istiyorsanız, belirli bir tabloda parçalama anahtarı dahil olmak üzere bir birincil anahtar oluşturmayı düşünün. Performansla ilgili nedenlerden dolayı önde gelen bir anahtar veya dizin sütununda parçalama anahtarı olmalıdır.
- İsteğin işlenmesinin Kurs sırasında bazı parçacık verileri kaynak ve hedef parça mevcut olabilir. Bu, parçacık taşıma zamanı hatalarına karşı korumak gereklidir. Ayırma-birleştirme parça eşlemesi ile tümleştirmesini sağlar bağlantıları aracılığıyla verilere bağımlı yönlendirme API'lerini kullanarak **OpenConnectionForKey** parça eşlemesi metodunda tutarsız bir ara durum göremezsiniz. Ancak, kaynak veya hedef parçalar kullanmadan bağlanırken **OpenConnectionForKey** yöntemi, tutarsız Ara durumları olabilir görünür Birleştir/Böl/taşıma istekler bittiğini tıklattığınızda. Bu bağlantılar kısmi veya yinelenen bir zamanlama veya arka plandaki bağlantı parça bağlı olarak sonuçlara neden olabilir. Bu sınırlama, şu anda esnek ölçeklendirme çok-Shard-sorgular tarafından yapılan bağlantıları içerir.
- Ayırma-birleştirme hizmeti için meta verileri veritabanı farklı rolleri arasında Paylaşılmaması gerekir. Örneğin, bir üretim rolü farklı meta verileri veritabanı işaret edecek şekilde hazırlamada çalışan bölme-birleştirme hizmeti rolü gerekiyor.

## <a name="billing"></a>Faturalandırma

Ayırma-birleştirme hizmetini, Microsoft Azure Aboneliğinize bir bulut hizmeti olarak çalışır. Hizmet Örneğiniz için bulut Hizmetleri için ücret alınır. Birleştir/Böl/taşıma işlemlerini sık gerçekleştirmediğiniz sürece, bölme-birleştirme bulut hizmetinizi silmeniz önerilir. Çalıştırma maliyetinden tasarruf veya Bulut hizmeti örnekleri dağıtılmış. Yeniden Dağıt ve bölme ve birleştirme işlemleri gerçekleştirme ihtiyacınız olduğunda kolayca çalıştırılabilir yapılandırmanızı başlatın.

## <a name="monitoring"></a>İzleme

### <a name="status-tables"></a>Durum tabloları

Ayırma-birleştirme hizmeti sağlar **RequestStatus** tamamlanan ve Süren istekleri izlemek için meta veri deposu veritabanından tablo. Ayırma-birleştirme hizmetinin bu örneği için gönderilen her bölme-birleştirme isteği için bir satır tabloda listelenmiştir. Bunu her istek için aşağıdaki bilgileri sağlar:

- **Zaman damgası**

  İstek başlatıldığı tarih ve saat.

- **Operationıd**

  İstek benzersiz olarak tanımlayan GUID. Bu istek işlemi hala devam ederken iptal etmek için de kullanılabilir.

- **Durumu**

  İsteğin geçerli durumu. Devam eden istekleri için istek olduğu geçerli aşama ayrıca listelenir.

- **CancelRequest**

  İstek iptal olup olmadığını gösteren bir bayrak.

- **İlerleme durumu**

  İşlemi tamamlama yüzdesini tahmin. Bir değeri 50 işlemi yaklaşık %50 tamamlandı olduğunu gösterir.

- **Ayrıntılar**

  Daha ayrıntılı bir İlerleme raporu sağlar bir XML değeri. İlerleme raporu satır kümesi kaynaktan hedefe kopyalanır düzenli olarak güncelleştirilir. Hataları veya özel durumlar olması durumunda, bu sütunda ayrıca hata hakkında daha ayrıntılı bilgi içerir.

### <a name="azure-diagnostics"></a>Azure Tanılama

Ayırma-birleştirme hizmetini izleme ve tanılama için Azure SDK 2.5 üzerinde temel Azure Tanılama'yı kullanır. Burada açıklandığı gibi Tanılama yapılandırmasını denetler: [Azure bulut Hizmetleri ve sanal Makineler'de tanılamayı etkinleştirme](../cloud-services/cloud-services-dotnet-diagnostics.md). İndirme paketini iki tanılama yapılandırması - bir web rolü ve çalışan rolü için bir tane içerir. Bu, performans sayaçları, IIS günlükleri, Windows olay günlükleri ve ayırma-birleştirme uygulama olay günlüklerini açmak için tanımları içerir.

## <a name="deploy-diagnostics"></a>Tanılama dağıtma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Azure Resource Manager PowerShell modülü, Azure SQL veritabanı tarafından hala desteklenmektedir, ancak tüm gelecekteki geliştirme için Az.Sql modüldür. Bu cmdlet'ler için bkz. [Azurerm.SQL'e](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az modül ve AzureRm modülleri komutları için bağımsız değişkenler büyük ölçüde aynıdır.

İzleme ve tanılama yapılandırması için NuGet paketi tarafından sağlanan web ve çalışan rollerini kullanarak tanılamayı etkinleştirmek için Azure PowerShell kullanarak aşağıdaki komutları çalıştırın:

```powershell
    $storage_name = "<YourAzureStorageAccount>"
    $key = "<YourAzureStorageAccountKey"
    $storageContext = New-AzStorageContext -StorageAccountName $storage_name -StorageAccountKey $key  
    $config_path = "<YourFilePath>\SplitMergeWebContent.diagnostics.xml"
    $service_name = "<YourCloudServiceName>"
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWeb"
    $config_path = "<YourFilePath>\SplitMergeWorkerContent.diagnostics.xml"
    $service_name = "<YourCloudServiceName>"
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWorker"
```

Tanılama ayarlarını yapılandırmak ve dağıtmak hakkında daha fazla bilgi bulabilirsiniz: [Azure bulut Hizmetleri ve sanal Makineler'de tanılamayı etkinleştirme](../cloud-services/cloud-services-dotnet-diagnostics.md).

## <a name="retrieve-diagnostics"></a>Tanılama alma

Visual Studio sunucu Gezgini'nde Azure bölümünde Sunucu Gezgini ağacının tanılama kolayca erişebilirsiniz. Visual Studio örneği açın ve menü çubuğunda Görünüm'ü ve Sunucu Gezgini tıklayın. Azure aboneliğinize bağlanmak için Azure simgesine tıklayın. Ardından Azure'a gidin -> depolama -> `<your storage account>` -> tabloları WADLogsTable ->. Daha fazla bilgi için [Sunucu Gezgini](https://msdn.microsoft.com/library/x603htbk.aspx).

![WADLogsTable][2]

Yukarıdaki çizimde vurgulanan WADLogsTable bölme-birleştirme hizmetinin uygulama günlüğüne ayrıntılı olayları içerir. İndirilen paketi varsayılan yapılandırmasını doğru bir üretim dağıtımı yöneliktir unutmayın. Bu nedenle, günlükleri ve sayaçları hizmet örneklerden alınır büyük (5 dakika) aralığıdır. Test ve geliştirme için aralık, web veya çalışan rolü ihtiyaçlarınıza tanılama ayarlarını ayarlayarak düşürün. Visual Studio sunucu Gezgini'ndeki (yukarıya bakın) rolüne sağ tıklayın ve sonra iletişim kutusunda tanılama yapılandırma ayarları için aktarım süresi ayarlayın:

![Yapılandırma][3]

## <a name="performance"></a>Performans

Genel olarak, daha iyi performans arttıkça, daha yüksek performanslı hizmet katmanlarında Azure SQL veritabanı'ndan olması beklenir. Daha yüksek hizmet katmanlarına yönelik daha yüksek g/ç, CPU ve bellek ayırmaları toplu kopyalama avantajından yararlanabilir ve ayırma-birleştirme hizmetinin kullandığı silme. Bu nedenle, yalnızca söz konusu veritabanları için hizmet katmanına bir tanımlanmış, sınırlı süre için artırın.

Hizmet doğrulama sorguları, normal işlemlerin parçası olarak da gerçekleştirir. Bu doğrulama sorguları beklenmeyen hedef aralıktaki veri varlığını denetlemek ve herhangi bir bölünmüş/Birleştir/taşıma işlemi tutarlı bir durumdan başladığından emin olmak. Tüm bu sorguları işleminin kapsamı tarafından tanımlanan parçalama anahtarı aralık ve istek tanımının bir parçası sağlanan toplu iş boyutu üzerinde çalışır. Önde gelen bir sütun olarak parçalama anahtarı içeren geçerli bir dizin olduğunda bu sorguları en iyi şekilde çalışır.

Ayrıca, önde gelen bir sütun olarak parçalama anahtarı bir Benzersizlik özelliğiyle sınırlar kaynak tüketimi günlük alanı ve bellek açısından iyileştirilmiş bir yaklaşım kullanmak için hizmet sağlayacaktır. Bu benzersiz özellik, büyük veri boyutları (genellikle yukarıda 1 GB) taşımak için gereklidir.

## <a name="how-to-upgrade"></a>Yükseltme

1. Bağlantısındaki [ayırma-birleştirme hizmetini dağıtma](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
2. Ayırma-birleştirme dağıtımınızın yeni yapılandırma parametrelerini yansıtmak bulut hizmeti yapılandırma dosyanızı değiştirin. Yeni bir gerekli parametre şifreleme için kullanılan sertifika hakkındaki bilgilerdir. Bunu yapmanın kolay bir yolu, yeni bir yapılandırma şablon dosyası karşıdan yükleme yapılandırmanızda karşı karşılaştırma sağlamaktır. Hem web hem de çalışan rolü için "DataEncryptionPrimaryCertificateThumbprint" ve "DataEncryptionPrimary" ayarlarını eklediğinizden emin olun.
3. Güncelleştirme Azure'a dağıtmadan önce tüm çalışmakta bölme-birleştirme işlemleri tamamlandığından emin olun. Bu, devam eden istekler için bölme-birleştirme meta verileri veritabanı RequestStatus ve PendingWorkflows tabloları sorgulayarak kolayca yapabilirsiniz.
4. Mevcut bulut hizmeti dağıtımınız için bölme-birleştirme, Azure aboneliğinizdeki yeni paket ve güncelleştirilmiş hizmet yapılandırma dosyanızı güncelleştirin.

Yükseltmek bölme-birleştirme için yeni bir meta veri veritabanı sağlanması gerekmez. Yeni sürümü, mevcut meta verileri veritabanını otomatik olarak yeni sürüme yükseltir.

## <a name="best-practices--troubleshooting"></a>En iyi yöntemler ve sorun giderme

- Bir de test kiracılığınız tanımlayın ve birden fazla parçaya Birleştir/Böl/taşıma işlemlerinizi en önemli test kiracısı ile çalışma. Tüm meta verileri, parça eşlemesinde doğru şekilde tanımlanır ve işlemleri kısıtlamaları veya yabancı anahtarlar ihlal etmemek emin olun.
- Test Kiracı tutun büyük kiracınızın veri boyutu değil karşılaştığınızı emin olmak için en yüksek veri boyutu Yukarıdaki veri boyutu ile ilgili sorunlar. Bu, tek bir kiracının gezinebilirsiniz süresini üzerinde bir üst sınır değerlendirmenize yardımcı olur.
- Şemanızı silme izin verdiğinden emin olun. Ayırma-birleştirme hizmetini veri hedefine başarıyla kopyalandı sonra veri kaynağı parçadan veri kaldırma özelliği gerektirir. Örneğin, **tetikleyicilerini Sil** hizmet kaynak verileri silmesi engelleyebilir ve işlemlerinin başarısız olmasına neden olabilir.
- Parçalama anahtarı önde gelen sütun birincil anahtar veya benzersiz dizin tanımı içinde olması gerekir. Her zaman parçalama anahtarı aralıklarda çalışan gerçek veri taşıma ve silme işlemlerinin yanı sıra, bölme ve birleştirme doğrulama sorgular için en iyi performansı sağlar.
- Ayırma-birleştirme hizmetinizi veritabanlarınızı bulunduğu bölgeye ve veri merkezinde ISS'de.

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-overview-split-and-merge/split-merge-overview.png
[2]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics.png
[3]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics-config.png
