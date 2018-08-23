---
title: Azure SQL veritabanı hizmet katmanları - DTU | Microsoft Docs
description: Tek hizmet katmanları ve performans düzeyleri ve depolama alanı boyutları sağlamak için havuz veritabanları hakkında bilgi edinin.
services: sql-database
author: sachinpMSFT
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 08/01/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: d0250d508ca6d21ee09c9402e10d2fdb025529ac
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2018
ms.locfileid: "42055859"
---
# <a name="choosing-a-dtu-based-service-tier-performance-level-and-storage-resources"></a>Depolama kaynaklarını DTU tabanlı hizmet katmanı ve performans düzeyi seçme 

Hizmet katmanları, performans düzeyleri dahil edilen depolama alanı, yedekleme ve sabit fiyat için bekletme süresi sabit tutarda aralığına göre ayrılır. Tüm hizmet katmanları ve performans düzeylerini kapalı kalma süresi olmadan değiştirmenin esneklik sağlar. Tek veritabanları ve elastik havuzlar, hizmet katmanı ve performans düzeyine göre saatlik faturalandırılır.

> [!IMPORTANT]
> SQL veritabanı yönetilen örneği, DTU tabanlı satın alma modeli genel önizlemede şu anda desteklemiyor. Daha fazla bilgi için [Azure SQL veritabanı yönetilen örneği](sql-database-managed-instance.md). 

## <a name="choosing-a-dtu-based-service-tier"></a>DTU tabanlı hizmet katmanı seçme

Hizmet katmanı seçme, öncelikli olarak iş sürekliliği, depolama ve performans gereksinimlerine bağlıdır.
||Temel|Standart|Premium|
| :-- | --: |--:| --:| --:| 
|Hedef iş yükü|Geliştirme ve üretim|Geliştirme ve üretim|Geliştirme ve üretim||
|Çalışma Süresi SLA'sı|%99,99|%99,99|%99,99|Önizleme sırasında yok|
|Yedekleri bekletme|7 gün|35 gün|35 gün|
|CPU|Düşük|Düşük, Orta, yüksek|Orta, yüksek|
|GÇ verimliliği (yaklaşık) |2.5 DTU başına IOPS| 2.5 DTU başına IOPS | DTU başına 48 IOPS|
|GÇ gecikmesi (yaklaşık)|5 ms (okuma), 10 ms (yazma)|5 ms (okuma), 10 ms (yazma)|2 ms (okuma/yazma)|
|Columnstore dizini oluşturma |Yok|S3 ve üstü|Desteklenen|
|Bellek içi OLTP|Yok|Yok|Desteklenen|
|||||

## <a name="single-database-dtu-and-storage-limits"></a>Tek veritabanı DTU ve depolama limitleri

Performans düzeyleri tek veritabanları için Veritabanı İşlem Birimleri (DTU’lar), elastik havuzlar için de elastik Veritabanı İşlem Birimleri (eDTU’lar) ile ifade edilir. Dtu'lar ve Edtu'lar hakkında daha fazla bilgi için bkz. [Dtu'lar ve Edtu'lar nelerdir](sql-database-service-tiers.md#what-are-database-transaction-units-dtus)?

||Temel|Standart|Premium|
| :-- | --: | --: | --: | --: |
| Maksimum depolama boyutu | 2 GB | 1 TB | 4 TB  | 
| En fazla dtu sayısı | 5 | 3000 | 4000 | |
||||||

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri kazanmak için bir veritabanı daraltma gerekebilir. Daha fazla bilgi için [Azure SQL veritabanı'nda dosya alanı yönetmek](sql-database-file-space-management.md).

## <a name="elastic-pool-edtu-storage-and-pooled-database-limits"></a>Elastik havuz eDTU ve depolama havuza veritabanı sınırları

| | **Temel** | **Standart** | **Premium** | 
| :-- | --: | --: | --: | --: |
| Veritabanı başına maksimum depolama boyutu  | 2 GB | 1 TB | 1 TB | 
| Havuz başına maksimum depolama boyutu | 156 GB | 4 TB | 4 TB | 
| Veritabanı başına maksimum Edtu | 5 | 3000 | 4000 | 
| Havuz başına maksimum Edtu | 1600 | 3000 | 4000 | 
| Havuz başına veritabanı sayısı | 500  | 500 | 100 | 
||||||

> [!IMPORTANT]
> 1 TB'den fazla depolama Premium katmanında şu anda aşağıdakiler dışındaki tüm bölgelerde: Batı Orta ABD, Doğu Çin, USDoDCentral, Almanya Orta, USDoDEast, ABD Devleti Güneybatı, USGov Iowa, Almanya Kuzeydoğu, Kuzey Çin. Diğer bölgelerde Premium katmanda depolama için 1 TB üst sınırı uygulanır. Bkz. [P11 P15 Geçerli Sınırlamalar](sql-database-dtu-resource-limits-single-databases.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri kazanmak için bir veritabanı daraltma gerekebilir. Daha fazla bilgi için [Azure SQL veritabanı'nda dosya alanı yönetmek](sql-database-file-space-management.md).

## <a name="dtu-benchmark"></a>DTU Kıyaslama

Gerçek veritabanı iş yükünün benzetimini gerçekleştiren bir Kıyaslama kullanarak kalibre her DTU ölçü için ilişkili fiziksel özelliklerini (CPU, bellek, g/ç).

### <a name="correlating-benchmark-results-to-real-world-database-performance"></a>Gerçek dünya veritabanı performans için kıyaslama sonuçları ilişkilendirme
Tüm değerlendirmesi yalnızca temsili ve hatırlanması olduğunu anlamak önemlidir. Kıyaslama uygulama ile elde edilen işlem hızları, diğer uygulamalarla elde aynı olması değil. Kıyaslama farklı işlem türleri birçok farklı tablolar ve veri türlerini içeren bir şema karşı çalıştırmak koleksiyonunu içerir. Tüm OLTP iş yükleri için ortak olan temel işlemlerin aynısını Kıyaslama uygular, ancak veritabanı veya uygulamanın belirli herhangi bir sınıf temsil etmiyor. Kıyaslama amacı, göreli ölçeği artırılabilen veya azaltılabilen olduğunda performans düzeyleri arasında beklenebilir bir veritabanının performansı için makul bir kılavuz sağlamaktır. Veritabanları farklı boyutta ve karmaşıklık gerçekte, farklı iş yüklerini bileşimleri karşılaştığınız ve farklı şekilde yanıt verir. Örneğin, yoğun g/ç uygulama GÇ eşiklerini daha çabuk karşılaşabilirsiniz veya CPU yoğunluklu uygulama CPU sınırları daha çabuk karşılaşabilirsiniz. Herhangi bir veritabanı Kıyaslama artan yük altında aynı şekilde, ölçeği bir garanti yoktur.

Kıyaslama ve metodolojisi aşağıda daha ayrıntılı olarak açıklanmıştır.

### <a name="benchmark-summary"></a>Kıyaslama özeti
Çevrimiçi işlem işleme (OLTP) iş yükleri en sık gerçekleşen temel veritabanı işlem bir karışımını performansını ASDB ölçer. Kıyaslama bulut bilgi işlem, veritabanı şemasını, veri doldurma göz önünde tasarlanmıştır ve işlem OLTP iş yüklerinde en sık kullanılan temel öğe eyaleti'nde temsili olacak şekilde tasarlanmıştır ancak.

### <a name="schema"></a>Şema
Şema yeterli çeşitlilik ve karmaşıklığı operations geniş bir destek sağlamak için tasarlanmıştır. Kıyaslama altı tablolarından oluşan bir veritabanına karşı çalışır. Tabloları üç kategoriye ayrılır: sabit-ölçeklendirme ve büyüyen boyutu. İki sabit boyutlu tablo vardır. üç ölçeklendirme tablo; ve giderek büyüyen bir tablo. Sabit boyutlu tablolar sabit sayıda satır vardır. Ölçeklendirme tabloların veritabanı performans için doğru orantılıdır ancak sırasında Kıyaslama değişmez bir kardinalite vardır. Büyüyen tablonun ilk yük ancak satırlar eklenen ve Silinen Kıyaslama çalıştırma sırasında kardinalite değişiklikleri ölçeklendirme bir tablo gibi boyutlandırılır.

Şema, tamsayı dahil olmak üzere, veri türlerinin bir karışımını içeren sayısal, karakter ve tarih/saat. Birincil ve ikincil anahtarları, ancak hiçbir yabancı anahtarlar şeması içerir - diğer bir deyişle, hiçbir başvurusal bütünlük kısıtlamaları tablolar bulunur arasında.

Bir veri nesil program başlangıç veritabanı verileri oluşturur. Tamsayı ve sayısal veriler, çeşitli stratejileri ile oluşturulur. Bazı durumlarda değerler rastgele bir aralığı içinde dağıtılır. Diğer durumlarda, bir dizi rastgele bir belirli dağıtım korunduğundan emin olmak için dizilmiş. Metin alanları ağırlıklı gerçekçi görünen verileri üretmek üzere sözcükler listesinden oluşturulur.

Bir "ölçek faktörü üzerinde." temel veritabanı boyutu Ölçeklendirme ve tabloları büyüyen kardinalite (SF kısaltılır) ölçek faktörü belirler. Aşağıdaki bölümde kullanıcılar ve ilerleme, veritabanı boyutu, kullanıcılar ve tüm diğer derlemekten ölçeklendirme en yüksek performansı sayısı açıklandığı gibi.

### <a name="transactions"></a>İşlemler
Aşağıdaki tabloda gösterildiği gibi iş yükü dokuz işlem türleri, oluşur. Her işlem, belirli bir veritabanı altyapısı ve sistem donanımı diğer işlemlerden yüksek karşıtlık ile sistem özelliklerine kümesini vurgulamak için tasarlanmıştır. Bu yaklaşım, farklı bileşenleri için genel performans etkisini değerlendirmek kolaylaştırır. Örneğin, "Okuma yoğun" işlem önemli bir diskten okunan işlem sayısını üretir.

| İşlem Türü | Açıklama |
| --- | --- |
| Lite okuyun |SEÇİN. bellek içi; salt okunur |
| Okuma Orta |SEÇİN. genellikle belleğe; salt okunur |
| Okuma ağır |SEÇİN. çoğunlukla değil bellek içi; salt okunur |
| Lite güncelleştirme |GÜNCELLEŞTİRME; bellek içi; okuma-yazma |
| Ağır güncelleştir |GÜNCELLEŞTİRME; çoğunlukla değil bellek içi; okuma-yazma |
| Lite Ekle |EKLEYİN. bellek içi; okuma-yazma |
| Ağır Ekle |EKLEYİN. çoğunlukla değil bellek içi; okuma-yazma |
| Sil |DELETE; bellek içi ve bellek içinde olmayan karışımı; okuma-yazma |
| CPU yoğun |SEÇİN. bellek içi; oldukça yoğun CPU yükü; salt okunur |

### <a name="workload-mix"></a>İş yükü karışımı
İşlem, aşağıdaki genel karışımı ile ağırlıklı bir dağıtım rastgele seçilir. Okuma/yazma yaklaşık 2:1 oranında genel karışımı vardır.

| İşlem Türü | Karışımı yüzdesi |
| --- | --- |
| Lite okuyun |35 |
| Okuma Orta |20 |
| Okuma ağır |5 |
| Lite güncelleştirme |20 |
| Ağır güncelleştir |3 |
| Lite Ekle |3 |
| Ağır Ekle |2 |
| Sil |2 |
| CPU yoğun |10 |

### <a name="users-and-pacing"></a>Kullanıcılar ve hızı
Kıyaslama iş yükünün, işlemler arasında eş zamanlı kullanıcı sayısıyla davranışını benzetmek için bağlantıları birtakım gönderen aracından yönlendirilir. Tüm bağlantılar ve işlemleri oluşturulan makine olsa da, kolaylık olması için bu bağlantıları "kullanıcılar" diyoruz Her kullanıcının diğer tüm kullanıcıları bağımsız olarak çalışır, ancak tüm kullanıcılar aynı döngüsünü aşağıda gösterilen adımları uygulayın:

1. Veritabanı bağlantı kurun.
2. Çıkmak için sinyal kadar yineleyin:
   * Bir işlemden rastgele (Ağırlıklı dağılım için) seçin.
   * Seçili işlem gerçekleştirin ve yanıt süresini ölçme.
   * İlerleme Gecikmesini tamamlanmasını bekleyin.
3. Veritabanı bağlantısını kapatın.
4. Çıkış.

Gönderilmemiş gecikmeyi (Adım 2c) rastgele seçilir, ancak 1.0 ikinci bir ortalaması olan bir dağıtım ile. Bu nedenle her kullanıcı ortalama olarak, saniyede en fazla bir işlem oluşturur.

### <a name="scaling-rules"></a>Ölçeklendirme kuralları
Kullanıcı sayısı, veritabanı boyutu (Ölçek faktörü birimleri) tarafından belirlenir. Her beş ölçek faktörü birim için bir kullanıcı yoktur. Gönderilmemiş gecikme nedeniyle, bir kullanıcı en fazla bir işlem, saniyede ortalama oluşturabilirsiniz.

Örneğin, bir ölçek faktörü 500 (SF = 500) veritabanı 100 kullanıcı sahip olur ve en çok 100 TPS saniyede elde edebilirsiniz. Daha yüksek bir TPS sürücüsüne oranı daha fazla kullanıcı ve daha büyük bir veritabanı gerektirir.

### <a name="measurement-duration"></a>Ölçüm süresi
Geçerli bir Kıyaslama en az bir saat kararlı bir duruma ölçüm bulunulmasını gerektirir.

### <a name="metrics"></a>Ölçümler
Ana ölçümler Kıyaslama aktarım hızı ve yanıt süresi ' dir.

* Temel performans ölçüsü Kıyaslama aktarım hızıdır. Aktarım hızı birimi,-tüm işlem türleri sayım zamanı başına işlemlerde bildirilir.
* Yanıt süresi, performans öngörülebilirliğini ölçüsüdür. Yanıt süresi kısıtlaması hizmetiyle aşağıda gösterildiği gibi daha katı bir yanıt zaman gereksinimini olması daha yüksek sınıflarına sınıfının göre değişir.

| Hizmet sınıfı | Aktarım hızı ölçümü | Yanıt süresi gereksinimi |
| --- | --- | --- |
| Premium |Saniye başına işlem |95. yüzdebirlik 0,5 saniye |
| Standart |Dakika başına işlem |1.0 saniye 90. yüzdebirlik |
| Temel |Saat başına işlem |2.0 saniye 80. yüzdebirlik |


## <a name="next-steps"></a>Sonraki adımlar

- Belirli performans düzeylerini ve tek veritabanları için kullanılabilen depolama boyutu seçenekleri hakkında daha fazla bilgi için bkz: [tek veritabanları için SQL veritabanı DTU tabanlı kaynak sınırları](sql-database-dtu-resource-limits-single-databases.md#single-database-storage-sizes-and-performance-levels).
- Belirli bir performans düzeyleri ve depolama boyutu elastik havuzlar için kullanılabilir seçenekler hakkında daha fazla bilgi için bkz: [SQL veritabanı DTU tabanlı kaynak sınırları](sql-database-dtu-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-performance-levels).
