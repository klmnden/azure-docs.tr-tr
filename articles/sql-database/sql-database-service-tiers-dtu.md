---
title: Azure SQL veritabanı hizmet katmanları - DTU tabanlı satın alma modeli | Microsoft Docs
description: Hizmet katmanlarında işlem ve depolama boyutları sağlamak tek ve havuza alınmış veritabanları için DTU tabanlı satın alma modeli hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
manager: craigg
ms.date: 06/25/2019
ms.openlocfilehash: 964a2931267ad2f1e2842693eadf43f8f629a084
ms.sourcegitcommit: a7ea412ca4411fc28431cbe7d2cc399900267585
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2019
ms.locfileid: "67357234"
---
# <a name="service-tiers-in-the-dtu-based-purchase-model"></a>DTU tabanlı satın alma modeli olarak hizmet katmanları

DTU tabanlı satın alma modeli hizmet katmanlarında işlem boyutları dahil edilen depolama alanı, yedekleme ve sabit fiyat için bekletme süresi sabit tutarda aralığına göre ayrılır. DTU tabanlı satın alma modeli tüm hizmet katmanlarında işlem boyutları en düşük ile değiştirme esnekliği [kapalı kalma süresi](https://azure.microsoft.com/support/legal/sla/sql-database/v1_2/); ancak olduğundan geçiş dönemi boyunca bağlantı olduğu için veritabanı kayıp kısa bir zaman miktarı için hangi yeniden deneme mantığı kullanılarak azaltılabilir. Tek veritabanları ve elastik havuzlar, hizmet katmanı ve işlem boyutu saatlere göre faturalandırılır.

> [!IMPORTANT]
> SQL veritabanı yönetilen örneği, DTU tabanlı satın alma modeli desteklemez. Daha fazla bilgi için [Azure SQL veritabanı yönetilen örneği](sql-database-managed-instance.md).
> [!NOTE]
> Sanal çekirdek tabanlı hizmet katmanları hakkında daha fazla bilgi için bkz: [sanal çekirdek tabanlı hizmet katmanları](sql-database-service-tiers-vcore.md). DTU tabanlı hizmet katmanları ve sanal çekirdek tabanlı hizmet katmanları ayrım yapma hakkında daha fazla bilgi için bkz: [Azure SQL veritabanı'nın satın alma modeli](sql-database-purchase-models.md).

## <a name="compare-the-dtu-based-service-tiers"></a>DTU tabanlı hizmet katmanlarını karşılaştırın

Hizmet katmanı seçme, öncelikli olarak iş sürekliliği, depolama ve performans gereksinimlerine bağlıdır.

||Temel|Standart|Premium|
| :-- | --: |--:| --:|
|Hedef iş yükü|Geliştirme ve üretim|Geliştirme ve üretim|Geliştirme ve üretim|
|Çalışma Süresi SLA'sı|%99,99|%99,99|%99,99|
|Yedekleri bekletme|7 gün|35 gün|35 gün|
|CPU|Düşük|Düşük, Orta, yüksek|Orta, yüksek|
|GÇ verimliliği (yaklaşık) |2.5 DTU başına IOPS| 2.5 DTU başına IOPS | DTU başına 48 IOPS|
|GÇ gecikmesi (yaklaşık)|5 ms (okuma), 10 ms (yazma)|5 ms (okuma), 10 ms (yazma)|2 ms (okuma/yazma)|
|Columnstore dizini oluşturma |Yok|S3 ve üstü|Desteklenen|
|Bellek içi OLTP|Yok|Yok|Desteklenen|
|||||

> [!NOTE]
> Azure keşfetmek için ücretsiz bir Azure hesabı ile birlikte temel bir hizmet katmanında, ücretsiz bir Azure SQL veritabanı alabilirsiniz. Bilgi için [Azure ücretsiz hesabınızla yönetilen bir bulut veritabanı oluşturun](https://azure.microsoft.com/free/services/sql-database/).

## <a name="single-database-dtu-and-storage-limits"></a>Tek veritabanı DTU ve depolama limitleri

İşlem boyutları, tek veritabanları için veritabanı işlem birimleri (Dtu'lar) ve elastik havuzlar için esnek veritabanı işlem birimleri (Edtu) cinsinden ifade edilir. Dtu'lar ve Edtu'lar hakkında daha fazla bilgi için bkz. [DTU tabanlı satın alma modeli](sql-database-purchase-models.md#dtu-based-purchasing-model)?

||Temel|Standart|Premium|
| :-- | --: | --: | --: |
| Maksimum depolama boyutu | 2 GB | 1 TB | 4 TB  |
| En fazla dtu sayısı | 5 | 3000 | 4000 | 
|||||

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri kazanmak için bir veritabanı daraltma gerekebilir. Daha fazla bilgi için [Azure SQL veritabanı'nda dosya alanı yönetmek](sql-database-file-space-management.md).

## <a name="elastic-pool-edtu-storage-and-pooled-database-limits"></a>Elastik havuz eDTU ve depolama havuza veritabanı sınırları

| | **Temel** | **Standart** | **Premium** |
| :-- | --: | --: | --: |
| Veritabanı başına maksimum depolama boyutu  | 2 GB | 1 TB | 1 TB |
| Havuz başına maksimum depolama boyutu | 156 GB | 4 TB | 4 TB |
| Veritabanı başına maksimum Edtu | 5 | 3000 | 4000 |
| Havuz başına maksimum Edtu | 1600 | 3000 | 4000 |
| Havuz başına veritabanı sayısı | 500  | 500 | 100 |
|||||

> [!IMPORTANT]
> 1 TB'den fazla depolama Premium katmanında şu anda tüm bölgelerde kullanılabilir: Çin Doğu, Kuzey Çin, Almanya Orta, Almanya Kuzeydoğu, Batı Orta ABD, US DoD bölgeler ve ABD kamu orta. Bu bölgelerde Premium katmanda depolama için 1 TB üst sınırı uygulanır.  Daha fazla bilgi için [P11 P15 geçerli sınırlamalar](sql-database-single-database-scale.md#p11-and-p15-constraints-when-max-size-greater-than-1-tb).  
> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri kazanmak için bir veritabanı daraltma gerekebilir. Daha fazla bilgi için [Azure SQL veritabanı'nda dosya alanı yönetmek](sql-database-file-space-management.md).

## <a name="dtu-benchmark"></a>DTU Kıyaslama

Gerçek veritabanı iş yükünün benzetimini gerçekleştiren bir Kıyaslama kullanarak kalibre her DTU ölçü için ilişkili fiziksel özelliklerini (CPU, bellek, g/ç).

### <a name="correlating-benchmark-results-to-real-world-database-performance"></a>Gerçek dünya veritabanı performans için kıyaslama sonuçları ilişkilendirme

Tüm değerlendirmesi yalnızca temsili ve hatırlanması olduğunu anlamak önemlidir. Kıyaslama uygulama ile elde edilen işlem hızları, diğer uygulamalarla elde aynı olması değil. Kıyaslama farklı işlem türleri birçok farklı tablolar ve veri türlerini içeren bir şema karşı çalıştırmak koleksiyonunu içerir. Tüm OLTP iş yükleri için ortak olan temel işlemlerin aynısını Kıyaslama uygular, ancak veritabanı veya uygulamanın belirli herhangi bir sınıf temsil etmiyor. Kıyaslama amacı, göreli zaman arasında bilgi işlem boyutlarına ölçeği artırılabilen veya azaltılabilen beklenebilir bir veritabanı performansını artırmak için makul bir kılavuz sağlamaktır. Veritabanları farklı boyutta ve karmaşıklık gerçekte, farklı iş yüklerini bileşimleri karşılaştığınız ve farklı şekilde yanıt verir. Örneğin, yoğun g/ç uygulama GÇ eşiklerini daha çabuk karşılaşabilirsiniz veya CPU yoğunluklu uygulama CPU sınırları daha çabuk karşılaşabilirsiniz. Herhangi bir veritabanı Kıyaslama artan yük altında aynı şekilde, ölçeği bir garanti yoktur.

Kıyaslama ve metodolojisi aşağıda daha ayrıntılı olarak açıklanmıştır.

### <a name="benchmark-summary"></a>Kıyaslama özeti

Çevrimiçi işlem işleme (OLTP) iş yükleri en sık gerçekleşen temel veritabanı işlem bir karışımını performansını Kıyaslama ölçer. Kıyaslama bulut bilgi işlem, veritabanı şemasını, veri doldurma göz önünde tasarlanmıştır ve işlem OLTP iş yüklerinde en sık kullanılan temel öğe eyaleti'nde temsili olacak şekilde tasarlanmıştır ancak.

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
   - Bir işlemden rastgele (Ağırlıklı dağılım için) seçin.
   - Seçili işlem gerçekleştirin ve yanıt süresini ölçme.
   - İlerleme Gecikmesini tamamlanmasını bekleyin.
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

- Temel performans ölçüsü Kıyaslama aktarım hızıdır. Aktarım hızı birimi,-tüm işlem türleri sayım zamanı başına işlemlerde bildirilir.
- Yanıt süresi, performans öngörülebilirliğini ölçüsüdür. Yanıt süresi kısıtlaması hizmetiyle aşağıda gösterildiği gibi daha katı bir yanıt zaman gereksinimini olması daha yüksek sınıflarına sınıfının göre değişir.

| Hizmet sınıfı | Aktarım hızı ölçümü | Yanıt süresi gereksinimi |
| --- | --- | --- |
| Premium |Saniye başına işlem |95. yüzdebirlik 0,5 saniye |
| Standart |Dakika başına işlem |1\.0 saniye 90. yüzdebirlik |
| Temel |Saat başına işlem |2\.0 saniye 80. yüzdebirlik |

## <a name="next-steps"></a>Sonraki adımlar

- Özel hakkında ayrıntılı bilgi işlem boyutlarına ve tek veritabanları için kullanılabilen depolama boyutu seçenekleri için bkz: [tek veritabanları için SQL veritabanı DTU tabanlı kaynak sınırları](sql-database-dtu-resource-limits-single-databases.md#single-database-storage-sizes-and-compute-sizes).
- Özel hakkında ayrıntılı bilgi işlem boyutları ve elastik havuzlar için kullanılabilir depolama boyutu seçenekleri için bkz: [SQL veritabanı DTU tabanlı kaynak sınırları](sql-database-dtu-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-compute-sizes).
