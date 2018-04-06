---
title: Bellek içi OLTP artırır SQL işlemlerinin perf | Microsoft Docs
description: Varolan bir SQL veritabanında işlem performansı artırmak için adopt bellek içi OLTP.
services: sql-database
author: jodebrui
manager: craigg
ms.reviewer: MightyPen
ms.service: sql-database
ms.custom: develop databases
ms.topic: article
ms.date: 11/22/2016
ms.author: jodebrui
ms.openlocfilehash: 00823ca44ec7135a9937bb37dd4ed58ec996c89d
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="use-in-memory-oltp-to-improve-your-application-performance-in-sql-database"></a>SQL veritabanı, uygulama performansını artırmak için kullanım bellek içi OLTP
[Bellek içi OLTP](sql-database-in-memory.md) içinde hareket işleme, veri alımı ve geçici veri senaryoları performansını artırmak için kullanılan [Premium ve iş kritik katmanı](sql-database-service-tiers.md) fiyatlandırma katmanı artırma olmadan veritabanları. 

> [!NOTE] 
> Bilgi nasıl [çekirdek % 70'sql Database tarafından DTU düşürürken anahtar veritabanının iş yükü Katlar](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)


Bellek içi OLTP, varolan bir veritabanında benimsemek için aşağıdaki adımları izleyin.

## <a name="step-1-ensure-you-are-using-a-premium-and-business-critical-tier-database"></a>1. adım: bir Premium ve iş kritik katmanı veritabanı kullandığınızdan emin olun
Bellek içi OLTP yalnızca Premium ve iş kritik katmanı veritabanlarında desteklenir. Bellek içi döndürülen sonuç 1 ise desteklenir (0):

```
SELECT DatabasePropertyEx(Db_Name(), 'IsXTPSupported');
```

*XTP* anlamına gelir *aşırı işlem işleme*



## <a name="step-2-identify-objects-to-migrate-to-in-memory-oltp"></a>2. adım: Bellek içi OLTP geçirilecek nesneleri tanımlar
SSMS içeren bir **işlem Performans Analizi genel bakış** etkin bir iş yükü olan bir veritabanına karşı çalıştırabilirsiniz rapor. Rapor, tablolar ve bellek içi OLTP geçiş adaylar saklı yordamlar tanımlar.

SSMS içinde bir rapor oluşturmak için:

* İçinde **Nesne Gezgini**, veritabanı düğümünü sağ tıklatın.
* Tıklatın **raporları** > **standart raporları** > **işlem Performans Analizi genel bakış**.

Daha fazla bilgi için bkz: [belirleme bir tablo veya saklı yordam gereken olması bağlantı noktası kurulmuş bellek içi OLTP için](http://msdn.microsoft.com/library/dn205133.aspx).

## <a name="step-3-create-a-comparable-test-database"></a>3. adım: karşılaştırılabilir test veritabanı oluşturma
Bellek için iyileştirilmiş bir tabloya dönüştürülür için yararlı bir tablo veritabanınızı sahip rapor gösterir varsayalım. İlk test ederek göstergesi onaylamak için test etmenizi öneririz.

Üretim veritabanınız test kopyasını gerekir. Test veritabanı üretim veritabanınız olarak aynı hizmeti katman düzeyinde olması gerekir.

Sınama kolaylaştırmak için aşağıdaki gibi test veritabanınızı ince ayar:

1. SSMS kullanarak test veritabanına bağlanın.
2. Sorguları WITH (SNAPSHOT) seçeneğinde gerek önlemek için aşağıdaki T-SQL deyiminde gösterildiği gibi veritabanı seçeneği ayarlayın:
   
   ```
   ALTER DATABASE CURRENT
    SET
        MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = ON;
   ```

## <a name="step-4-migrate-tables"></a>4. adım: tabloları geçirme
Oluşturma ve test etmek istediğiniz tablonun bellek için iyileştirilmiş bir kopyasını doldurmak gerekir. Kullanarak oluşturabilirsiniz:

* Kullanışlı belleği en iyi duruma getirme Sihirbazı'nda SSMS.
* El ile T-SQL.

#### <a name="memory-optimization-wizard-in-ssms"></a>Belleği en iyi duruma getirme Sihirbazı'nda SSMS
Bu geçiş seçeneği kullanmak için:

1. SSMS ile test veritabanına bağlanın.
2. İçinde **Object Explorer**, tabloyu sağ tıklatın ve ardından **belleği en iyi duruma getirme Danışmanı**.
   
   * **Tablo bellek iyileştirici Danışmanı** Sihirbazı görüntülenir.
3. Sihirbazı'nda tıklatın **geçiş doğrulama** (veya **sonraki** düğmesi) tablosu bellek için iyileştirilmiş tablolarda desteklenmeyen herhangi desteklenmeyen özellikler olup olmadığını görmek için. Daha fazla bilgi için bkz.
   
   * *Belleği en iyi duruma getirme denetim listesi* içinde [belleği en iyi duruma getirme Danışmanı](http://msdn.microsoft.com/library/dn284308.aspx).
   * [Bellek içi OLTP tarafından desteklenmeyen transact-SQL yapıları](http://msdn.microsoft.com/library/dn246937.aspx).
   * [Bellek içi OLTP geçirme](http://msdn.microsoft.com/library/dn247639.aspx).
4. Tablonun desteklenmeyen hiçbir özellik varsa, advisor gerçek şemayı ve veri geçişi sizin için gerçekleştirebilirsiniz.

#### <a name="manual-t-sql"></a>El ile T-SQL
Bu geçiş seçeneği kullanmak için:

1. Test veritabanınızı SSMS (veya benzer bir yardımcı programını) kullanarak bağlanın.
2. Tablonuz ve dizinlerini için tam T-SQL komut dosyasını alın.
   
   * SSMS, tablo düğümünü sağ tıklatın.
   * Tıklatın **komut tablo olarak** > **için oluşturun** > **yeni sorgu penceresinde**.
3. Komut penceresinde WITH ekleyin (memory_optımızed = ON) CREATE TABLE deyimi için.
4. Bir kümelenmiş dizin ise NONCLUSTERED için değiştirin.
5. Varolan tablonun SP_RENAME kullanarak yeniden adlandırın.
6. Yeni bellek için iyileştirilmiş tablonun kopyasını düzenlenen CREATE TABLE komut dosyasını çalıştırarak oluşturun.
7. Veri ekleme kullanarak bellek için iyileştirilmiş tabloya Kopyala... SEÇİN * İÇİNE:

```
INSERT INTO <new_memory_optimized_table>
        SELECT * FROM <old_disk_based_table>;
```


## <a name="step-5-optional-migrate-stored-procedures"></a>(İsteğe bağlı) 5. adım: saklı yordamları geçirme
Bellek içi özelliği, Gelişmiş performans için bir saklı yordam de değiştirebilirsiniz.

### <a name="considerations-with-natively-compiled-stored-procedures"></a>Yerel koda derlenmiş saklı yordamlar ile ilgili önemli noktalar
Yerel koda derlenmiş bir saklı yordam aşağıdaki seçenekler, T-SQL WITH yan tümcesi'olmalıdır:

* NATIVE_COMPILATION ÖĞESİNİ
* Şemaya Bağlama: anlamı tablolar saklı yordamı saklı yordamı bırakma sürece saklı yordam etkileyecek herhangi bir şekilde değiştirilmiş sütun tanımlarını sahip olamaz.

Yerel bir modül biri büyük kullanmalısınız [ATOMİK bloklar](http://msdn.microsoft.com/library/dn452281.aspx) işlem yönetimi. Herhangi bir rol veya geri alma işlemi için açık BEGIN TRANSACTION yok. Kodunuzu bir iş kuralı ihlal algılarsa, atomik blok ile sonlandırabilir bir [THROW](http://msdn.microsoft.com/library/ee677615.aspx) deyimi.

### <a name="typical-create-procedure-for-natively-compiled"></a>İçin tipik CREATE PROCEDURE yerel koda derlenmiş
Genellikle yerel koda derlenmiş bir saklı yordam oluşturmak için T-SQL aşağıdaki şablonu benzer:

```
CREATE PROCEDURE schemaname.procedurename
    @param1 type1, …
    WITH NATIVE_COMPILATION, SCHEMABINDING
    AS
        BEGIN ATOMIC WITH
            (TRANSACTION ISOLATION LEVEL = SNAPSHOT,
            LANGUAGE = N'your_language__see_sys.languages'
            )
        …
        END;
```

* TRANSACTION_ISOLATION_LEVEL için anlık görüntü yerel koda derlenmiş saklı yordam için en yaygın değerdir. Ancak, bir alt kümesini diğer değerleri de desteklenir:
  
  * YİNELENEBİLİR OKUMA
  * SERİ HALE GETİRİLEBİLİR
* Dil değerini sys.languages görünümünde mevcut olması gerekir.

### <a name="how-to-migrate-a-stored-procedure"></a>Saklı yordam geçirme
Geçiş adımları şunlardır:

1. Normal yorumlanan saklı yordama CREATE PROCEDURE betik alın.
2. Üstbilgisinde önceki şablon eşleşecek şekilde yeniden yazın.
3. Saklı yordam T-SQL kodunu yerel koda derlenmiş saklı yordamlar için desteklenmeyen herhangi bir özellik kullanıp kullanmadığını onaylaması. Geçici Çözümler gerekirse uygulayın.
   
   * Ayrıntılar için bkz: [yerel olarak depolanan yordamlar derlenmiş geçiş sorunları](http://msdn.microsoft.com/library/dn296678.aspx).
4. Eski saklı yordam SP_RENAME kullanarak yeniden adlandırın. Veya yalnızca BIRAKILAMIYOR.
5. Düzenlenen oluşturma yordamı T-SQL betiğini çalıştırın.

## <a name="step-6-run-your-workload-in-test"></a>6. adım: İş yükünüzün testi çalıştırın.
Bir iş yükü, üretim veritabanınız çalışan iş yüküne benzer test veritabanınızda çalıştırın. Bu, bellek içi özelliği kullanımınız için tabloları ve saklı yordamları tarafından gerçekleştirilen performans kazancı ortaya çıkarır.

İş yükü önemli öznitelikleri şunlardır:

* Eşzamanlı bağlantı sayısı.
* Okuma/yazma oranı.

Uyarlamanıza ve test iş yükünü çalıştırmak için gösterilen kullanışlı ostress.exe aracı kullanmayı düşünün [burada](sql-database-in-memory.md).

Ağ gecikmesini en aza indirmek için veritabanının bulunduğu aynı Azure coğrafi bölgede testinizi çalıştırın.

## <a name="step-7-post-implementation-monitoring"></a>7. adım: Uygulama sonrası izleme
Bellek içi uygulamalarınızda üretim performans etkisini izleme göz önünde bulundurun:

* [Bellek içi depolama izleme](sql-database-in-memory-oltp-monitoring.md).
* [Dinamik yönetim görünümlerini kullanarak Azure SQL Database’i izleme](sql-database-monitoring-with-dmvs.md)

## <a name="related-links"></a>İlgili bağlantılar
* [Bellek içi OLTP (bellek içi iyileştirme)](http://msdn.microsoft.com/library/dn133186.aspx)
* [Yerel koda derlenmiş saklı yordamlar giriş](http://msdn.microsoft.com/library/dn133184.aspx)
* [Belleği en iyi duruma getirme Danışmanı](http://msdn.microsoft.com/library/dn284308.aspx)

