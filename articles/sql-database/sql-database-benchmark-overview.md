---
title: Azure SQL veritabanı DTU Kıyaslama genel bakış
description: Bu konuda, Azure SQL veritabanını Azure SQL veritabanı performansını ölçmek kullanılan Kıyaslama açıklanmaktadır.
services: sql-database
author: jan-eng
manager: jhubbard
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: article
ms.date: 04/01/2018
ms.author: janeng
ms.openlocfilehash: 0dae04ab3de16e2ef51134ec6979c6c289a6da3f
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="azure-sql-database-dtu-benchmark-overview"></a>Azure SQL veritabanı DTU Kıyaslama genel bakış
## <a name="overview"></a>Genel Bakış
DTU tabanlı satın alma modeli, Microsoft Azure SQL veritabanı üç sunar [hizmet katmanları](sql-database-service-tiers.md) birden çok performans düzeyine sahip. Her performans düzeyi kaynakları ya da 'power' giderek daha yüksek verimlilik sağlamak üzere tasarlanmış, artan bir dizi sağlar.

> [!IMPORTANT]
> Azure SQL veritabanı artık bir vCore tabanlı satın alma modeli (Önizleme) destekler. Bilgi için bkz: [vCore tabanlı satın alma modeli (Önizleme)](sql-database-service-tiers.md#vcore-based-purchasing-model-preview).

Her performans düzeyi artan gücünü artan veritabanı performansını nasıl çevirir ölçme olması önemlidir. Bu Microsoft yapmak için Azure SQL veritabanı Kıyaslama (ASDB) geliştirmiştir. Kıyaslama temel işlemleri tüm OLTP iş yüklerini bulunan bir karışımını uygular. Biz her performans düzeyi çalışan veritabanları için elde edilen işleme ölçün.

Kaynaklar ve her hizmet katmanını ve performans düzeyi gücünü cinsinden ifade edilir [veritabanı işlem birimleri (Dtu'lar)](sql-database-what-is-a-dtu.md). Dtu'lar CPU, bellek, karışık bir ölçüyü temel performans düzeyinin göreli kapasitesini açıklar için bir yol sağlar ve okuma ve oranları her performans düzeyi tarafından sunulan yazma. Bir veritabanı DTU derecesi Katlama veritabanı güç Katlama için karşılık gelir. Kıyaslama her performans düzeyine göre gerçek veritabanı işlemleri, veritabanı boyutu, kullanıcı sayısı ve işlem hızları veritabanına sağlanan kaynakları orantılı olarak ölçekleme sırasında kullanan tarafından sunulan artan güç veritabanı performans üzerindeki etkisini değerlendirmenize olanak tanır.

Temel Hizmet katmanını verimini ifade ederek işlemleri saat başına, işlemleri dakika başına kullanarak standart hizmet katmanı ve Premium Hizmet katmanını işlemleri performansı hızla ilişkilendirmek olası her hizmet katmanının bir uygulama gereksinimlerini kolaylaştırır saniyede, kullanma kullanıyor.

## <a name="correlating-benchmark-results-to-real-world-database-performance"></a>Gerçek dünya veritabanı performans kıyaslama sonuçları ilişkilendirme
ASDB, tüm değerlendirmeleri gibi yalnızca temsili ve hatırlanması olduğunu anlamak önemlidir. Kıyaslama uygulama ile elde edilen işlem hızları aynı olan diğer uygulamalarla elde olmaz. Kıyaslama farklı bir işlem türleri bir dizi tablolar ve veri türlerini içeren bir şemaya karşı çalıştırmak koleksiyonunu içerir. Tüm OLTP iş yükleri için ortak olan aynı temel işlemleri Kıyaslama uygular, ancak herhangi belirli bir sınıf uygulama veya veritabanı göstermiyor. Kıyaslama amacı yukarı veya aşağı Ölçeklendirmesi, performans düzeyleri arasında beklenen bir veritabanının göreli performansı için makul bir kılavuz sağlamaktır. Gerçekte, veritabanları farklı boyutlarda ve karmaşıklık olan, iş yüklerini farklı bileşimleri karşılaştığınız ve farklı şekilde yanıt verir. Örneğin, bir g/ç yoğunluklu uygulama GÇ eşikleri er isabet veya bir CPU-yoğun uygulama CPU sınırları er isabet. Herhangi bir veritabanı artan yük altında Kıyaslama aynı şekilde ölçeklendirir garantisi yoktur.

Kıyaslama ve metodolojisi aşağıdaki daha ayrıntılı olarak açıklanmıştır.

## <a name="benchmark-summary"></a>Kıyaslama özeti
ASDB çevrimiçi işlem işleme (OLTP) iş yükleri en sık karşılaşılan temel veritabanı işlemleri bir karışımını performansını ölçer. Kıyaslama unutmayın, veritabanı şemasını, veri doldurma bulut tasarlanmıştır ve işlemleri OLTP iş yüklerini en sık kullanılan temel öğelerinin kapsamlı temsilcisi olacak şekilde tasarlanmış olmasını rağmen.

## <a name="schema"></a>Şema
Şema yeterli çeşitli ve çok çeşitli işlemleri desteklemek için karmaşıklık sağlamak üzere tasarlanmıştır. Kıyaslama altı tabloları oluşan bir veritabanına karşı çalışır. Tabloları üç kategoriye ayrılır: sabit ölçekleme ve büyüyen boyutlu,. İki sabit boyutlu tablo vardır; üç ölçeklendirme tablolar; ve artan bir tablo. Sabit boyutlu tabloları sabit sayıda satıra sahip. Ölçeklendirme tablolar için veritabanı performansını orantılıdır ancak Kıyaslama sırasında değişmez bir önem düzeyi vardır. Büyüyen tablonun ilk yük ancak satırları eklenen ve Silinen Kıyaslama çalıştırma esnasında kardinalite değişiklikleri ölçeklendirme bir tablo gibi boyutlandırılır.

Şema, tamsayı dahil olmak üzere veri türlerinin bir karışımını içeren sayısal karakter ve tarih/saat. Birincil ve ikincil anahtarlar, ancak hiçbir yabancı anahtarlar şeması içerir - diğer bir deyişle, vardır hiçbir başvuru bütünlüğü kısıtlamalarını tablolar arasında.

Bir veri nesil program ilk veritabanı için veriler üretir. Tamsayı ve sayısal veriler çeşitli stratejiler kullanılarak oluşturulur. Bazı durumlarda değerler rastgele bir aralığı içinde dağıtılır. Diğer durumlarda, bir değerleri kümesi rastgele belirli bir dağıtım korunduğundan emin olmak için dizilmiş. Metin alanları gerçekçi görünümlü veri üretmek için sözcükler ağırlıklı listesinden üretilir.

Bir "ölçek faktörü üzerinde." temel veritabanı boyutu (BT kısaltılır) ölçek çarpanı ölçekleme ve tabloları büyütme önemi belirler. Aşağıdaki bölümde kullanıcılar ve ilerleme, veritabanı boyutu, kullanıcılar ve tüm diğer orantılı olarak ölçekleme en yüksek performans sayısı açıklandığı gibi.

## <a name="transactions"></a>İşlemler
Aşağıdaki tabloda gösterildiği gibi iş yükü dokuz işlem türlerini oluşur. Her işlem, sistem özelliklerine veritabanı motoru ve sistem donanımı diğer işlemlerdeki yüksek karşıtlık ile belirli bir dizi vurgulamak için tasarlanmıştır. Bu yaklaşım, farklı bileşenler için genel performans etkisi değerlendirmek kolaylaştırır. Örneğin, "Okuma ağır" işlem çok sayıda diskten okuma işlemleri üretir.

| İşlem türü | Açıklama |
| --- | --- |
| Lite okuma |SEÇİN; bellek içi; salt okunur |
| Okuma Orta |SEÇİN; çoğunlukla bellek içi; salt okunur |
| Okuma ağır |SEÇİN; çoğunlukla olmayan bellek içi; salt okunur |
| Lite güncelleştirme |GÜNCELLEŞTİRME; bellek içi; okuma-yazma |
| Ağır güncelleştir |GÜNCELLEŞTİRME; çoğunlukla olmayan bellek içi; okuma-yazma |
| Lite Ekle |INSERT; bellek içi; okuma-yazma |
| Ağır Ekle |INSERT; çoğunlukla olmayan bellek içi; okuma-yazma |
| Sil |DELETE; bellek içi ve değil bellek karışımı; okuma-yazma |
| CPU ağır |SEÇİN; bellek içi; görece yoğun CPU yükünü; salt okunur |

## <a name="workload-mix"></a>İş yükü karışımı
İşlemleri, aşağıdaki genel karışımı ile ağırlıklı dağıtım rastgele seçilir. Genel karışımı yaklaşık 2:1 okuma/yazma oranını sahiptir.

| İşlem türü | Karışımı yüzdesi |
| --- | --- |
| Lite okuma |35 |
| Okuma Orta |20 |
| Okuma ağır |5 |
| Lite güncelleştirme |20 |
| Ağır güncelleştir |3 |
| Lite Ekle |3 |
| Ağır Ekle |2 |
| Sil |2 |
| CPU ağır |10 |

## <a name="users-and-pacing"></a>Kullanıcılar ve Adımlama
Kıyaslama iş yükü, bir dizi eş zamanlı kullanıcı sayısı davranışını benzetmek için bağlantıları işlemleri gönderir bir aracından yönetilir. Tüm bağlantılar ve işlemleri oluşturulan makine olsa da, kolaylık sağlamak için Biz bu bağlantılar "kullanıcılar" olarak bakın Diğer tüm kullanıcıları bağımsız olarak her kullanıcı çalışır rağmen tüm kullanıcılar adımları aşağıda gösterilen aynı döngüsünü gerçekleştirin:

1. Bir veritabanı bağlantısı kurun.
2. Çıkmak için sinyal kadar yineleyin:
   * Bir işlem rastgele (dağıtımdan bir ağırlıklı) seçin.
   * Seçili işlem gerçekleştirmek ve yanıt süresini ölçün.
   * İlerleme Gecikmesini bekleyin.
3. Veritabanı bağlantısı kapatın.
4. Çıkış.

(2c adımında) ilerleme gecikmesini rastgele seçilir, ancak bir dağıtım 1.0 ikinci ortalama sahip. Bu nedenle her kullanıcı ortalama, saniye başına en fazla bir işlem oluşturabilir.

## <a name="scaling-rules"></a>Ölçeklendirme kuralları
Kullanıcı sayısı, veritabanı boyutu (Ölçek çarpanı birimleri) tarafından belirlenir. Her beş ölçek çarpanı birimler için bir kullanıcı yoktur. İlerleme Gecikmesini nedeniyle, bir kullanıcı en fazla bir işlem saniye başına ortalama oluşturabilir.

Örneğin, bir ölçek faktörü 500 (BT = 500) veritabanı 100 kullanıcıların gerekir ve en çok saniyede 100 TP'leri elde edebilirsiniz. Daha yüksek bir TP'leri sürücü oranı daha fazla kullanıcı ve daha büyük bir veritabanı gerektirir.

Aşağıdaki tabloda her hizmet katmanını ve performans düzeyini gerçekten Sürdürülen kullanıcı sayısını gösterir.

| Hizmet Katmanı (performans düzeyi) | Kullanıcılar | Veritabanı Boyutu |
| --- | --- | --- |
| Temel |5 |720 MB |
| Standart (S0) |10 |1 GB |
| Standart (S1) |20 |2.1 GB |
| Standart (S2) |50 |7.1 GB |
| Premium (P1) |100 |14 GB |
| Premium (P2) |200 |28 GB |
| Premium (P6) |800 |114 GB |

## <a name="measurement-duration"></a>Ölçüm süresi
Geçerli bir Kıyaslama en az bir saat kararlı durum ölçüm süresi gerektirir.

## <a name="metrics"></a>Ölçümler
Anahtar Kıyaslama üretilen iş ve yanıt süresi ölçümleridir.

* Üretilen iş Kıyaslama temel performans ölçüsüdür. İşleme birim--tüm işlem türleri sayım zaman, başına işlemlerde bildirilir.
* Yanıt süresi, performans öngörülebilirlik ölçüsüdür. Yanıt süresi kısıtlaması hizmetiyle aşağıda gösterildiği gibi daha sıkı bir yanıt süresi gereksinimi olan hizmet yüksek sınıfları sınıfının göre değişir.

| Hizmet sınıfı | Üretilen iş ölçü | Yanıt süresi gereksinimi |
| --- | --- | --- |
| Premium |Saniye başına işlem |0,5 saniye adresindeki 95. yüzdebirlik |
| Standart |Dakika başına işlem |1.0 saniye adresindeki 90 yüzdebirlik |
| Temel |Saat başına işlem |2.0 saniye adresindeki 80. yüzdebirlik |

## <a name="conclusion"></a>Sonuç
Azure SQL veritabanı Kıyaslama kullanılabilir hizmet katmanları ve performans düzeyleri aralığı çalışan Azure SQL veritabanı performanslarını ölçer. Kıyaslama çevrimiçi işlem işleme (OLTP) iş yükleri en sık karşılaşılan temel veritabanı işlemleri bir karışımını uygular. Gerçek performans ölçerek Kıyaslama etkisi daha anlamlı bir değerlendirme performans düzeyinin CPU hızı, bellek boyutu ve IOPS gibi her düzey tarafından sağlanan kaynaklar listeleyerek mümkün olandan değiştirmenin verimlilik sağlar. Gelecekte, biz kapsamını genişletmek ve sağlanan verileri genişletmek için Kıyaslama gelişmeye devam eder.

## <a name="resources"></a>Kaynaklar
[SQL veritabanı giriş](sql-database-technical-overview.md)

[Hizmet katmanları ve performans düzeyleri](sql-database-service-tiers.md)

[Tek veritabanları için performans rehberi](sql-database-performance-guidance.md)
