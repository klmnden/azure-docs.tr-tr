---
title: Bellek içi OLTP artırır SQL işlemleri perf | Microsoft Docs
description: Mevcut bir SQL veritabanı'nda işlem performansı artırmak için bellek içi OLTP adopt.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: MightyPen
manager: craigg
ms.date: 11/07/2018
ms.openlocfilehash: ad66253d33b2e99f0be79bfaddc86b3274f5cab0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60337799"
---
# <a name="use-in-memory-oltp-to-improve-your-application-performance-in-sql-database"></a>SQL veritabanı'nda, uygulama performansını artırmak için kullanım bellek içi OLTP

[Bellek içi OLTP](sql-database-in-memory.md) içinde geçici veri senaryoları, işlem ve veri alımı performansını geliştirmek için kullanılan [Premium ve iş açısından kritik katmanında](sql-database-service-tiers-vcore.md) fiyatlandırma katmanını artırma olmadan veritabanları. 

> [!NOTE] 
> Bilgi nasıl [çekirdek SQL veritabanı ile 70 oranında DTU düşürürken anahtar veritabanının iş yükü Katlar](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)


Mevcut veritabanınızda bellek içi OLTP'yi benimseme için bu adımları izleyin.

## <a name="step-1-ensure-you-are-using-a-premium-and-business-critical-tier-database"></a>1. Adım: Premium ve iş açısından kritik katmanında veritabanı kullandığınızdan emin olun

Bellek içi OLTP yalnızca Premium ve iş açısından kritik katman veritabanlarında desteklenir. Bellek içi döndürülen sonuç 1 ise desteklenir (0):

```
SELECT DatabasePropertyEx(Db_Name(), 'IsXTPSupported');
```

*XTP* anlamına gelen *aşırı hareket işleme*



## <a name="step-2-identify-objects-to-migrate-to-in-memory-oltp"></a>2. Adım: Bellek içi OLTP geçirilecek nesneleri tanımlar
SSMS içeren bir **işlem Performans Analizi genel bakış** etkin bir iş yüküne sahip bir veritabanında çalıştırmak üzere bir rapor. Rapor, tablolar ve bellek içi OLTP geçiş için aday niteliği taşıyan saklı yordamlar tanımlar.

SSMS'de, raporu oluşturmak için şunu yazın:

* İçinde **Nesne Gezgini**, veritabanı düğümü.
* Tıklayın **raporları** > **standart raporlar** > **işlem Performans Analizi genel bakış**.

Daha fazla bilgi için [belirleme bir tablo veya saklı yordam gereken olması Unity'nin bellek içi OLTP için](https://msdn.microsoft.com/library/dn205133.aspx).

## <a name="step-3-create-a-comparable-test-database"></a>3. Adım: Karşılaştırılabilir test veritabanı oluşturma
Veritabanı bellek için iyileştirilmiş bir tabloya dönüştürülür avantaj elde edecektir tablolu raporu gösterir varsayalım. İlk test ederek göstergesi onaylamak için test etmenizi öneririz.

Size, üretim veritabanınızı test bir kopyasına ihtiyacınız vardır. Test veritabanı, üretim veritabanınız olarak aynı hizmet katmanında düzeyinde olması gerekir.

Test kolaylaştırmak için test veritabanınızı gibi ince ayar:

1. SSMS kullanarak test veritabanına bağlanın.
2. Sorgular için WITH (SNAPSHOT) seçeneğinde gerek önlemek için veritabanı seçeneği aşağıdaki T-SQL deyiminde gösterilen şekilde ayarlayın:
   
   ```
   ALTER DATABASE CURRENT
    SET
        MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = ON;
   ```

## <a name="step-4-migrate-tables"></a>4. Adım: Tabloları geçirme
Oluşturma ve test etmek istediğiniz tablosu bellek için iyileştirilmiş bir kopyasını doldurulması gerekir. Kullanarak oluşturabilirsiniz:

* Ssms'de kullanışlı bellek iyileştirme Sihirbazı.
* Manual T-SQL.

#### <a name="memory-optimization-wizard-in-ssms"></a>Ssms'de bellek iyileştirme Sihirbazı
Bu geçiş seçeneği kullanmak için:

1. SSMS ile test veritabanına bağlanın.
2. İçinde **Nesne Gezgini**tablosunda sağ tıklayın ve ardından **bellek iyileştirme Advisor**.
   
   * **Tablo bellek iyileştirici Advisor** Sihirbazı görüntülenir.
3. Sihirbazı'nda tıklatın **geçiş doğrulama** (veya **sonraki** düğmesi) tablosu bellek için iyileştirilmiş tablolarda desteklenmeyen desteklenmeyen özellikler olup olmadığını belirlemek için. Daha fazla bilgi için bkz.
   
   * *Bellek iyileştirme denetim* içinde [bellek iyileştirme Advisor](https://msdn.microsoft.com/library/dn284308.aspx).
   * [Bellek içi OLTP tarafından desteklenmeyen transact-SQL yapıları](https://msdn.microsoft.com/library/dn246937.aspx).
   * [Bellek içi OLTP geçirme](https://msdn.microsoft.com/library/dn247639.aspx).
4. Tabloda hiç desteklenmeyen özellikler varsa, advisor gerçek şema ve veri geçişi sizin için gerçekleştirebilirsiniz.

#### <a name="manual-t-sql"></a>El ile T-SQL
Bu geçiş seçeneği kullanmak için:

1. SSMS (veya benzer bir yardımcı programını) kullanarak test veritabanınıza bağlanın.
2. T-SQL, tablo ve dizinlerini için betiğin edinin.
   
   * SSMS'de, tablo düğümüne sağ tıklayın.
   * Tıklayın **komut dosyası tablo olarak** > **için oluşturma** > **yeni bir sorgu penceresi**.
3. Komut penceresinde WITH ekleyin (memory_optımızed = ON) CREATE TABLE deyimi için.
4. Bir kümelenmiş dizin yoksa, NONCLUSTERED için değiştirin.
5. Varolan tablonun sp_rename seçeneğini kullanarak yeniden adlandırın.
6. Düzenlenen CREATE TABLE betiğinizi çalıştırarak tablosu bellek için iyileştirilmiş yeni kopyasını oluşturun.
7. Veri ekleme kullanarak bellek için iyileştirilmiş tabloya Kopyala... SEÇİN * İÇİNE:

```
INSERT INTO <new_memory_optimized_table>
        SELECT * FROM <old_disk_based_table>;
```


## <a name="step-5-optional-migrate-stored-procedures"></a>(İsteğe bağlı) 5. adım: Saklı yordamları geçirme
Bellek içi özelliği, performansı artırmak için bir saklı yordam de değiştirebilirsiniz.

### <a name="considerations-with-natively-compiled-stored-procedures"></a>Yerel koda derlenmiş saklı yordamlar hakkında konuları
Yerel koda derlenmiş bir saklı yordam, aşağıdaki seçenekler, T-SQL WITH yan tümcesi olmalıdır:

* NATIVE_COMPILATION
* SCHEMABINDING: anlamı tablolar saklı yordamı saklı yordamı bırak sürece saklı yordamı etkileyecek herhangi bir yolla değiştirilen sütun tanımlarını sahip olamaz.

Yerel bir modül bir büyük kullanmanız gerekir [ATOMİK bloklar](https://msdn.microsoft.com/library/dn452281.aspx) işlem yönetimi için. Herhangi bir rol veya geri alma işlemi için açık bir BEGIN TRANSACTION yok. Kodunuzu bir iş kuralı ihlal algılarsa, atomik blok ile sonlandırabilirsiniz bir [THROW](https://msdn.microsoft.com/library/ee677615.aspx) deyimi.

### <a name="typical-create-procedure-for-natively-compiled"></a>Tipik oluşturma yordamı için doğal olarak derlenen
Genellikle aşağıdaki şablonu için yerel koda derlenmiş bir saklı yordam oluşturmak için T-SQL benzer:

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

* TRANSACTION_ISOLATION_LEVEL için anlık görüntü yerel koda derlenmiş saklı yordam için en yaygın değerdir. Ancak, diğer değerleri kümesini de desteklenir:
  
  * TEKRARLANABİLİR OKUMA
  * SERİ HALE GETİRİLEBİLİR
* Dil değerini sys.languages Görünümü'nde mevcut olmalıdır.

### <a name="how-to-migrate-a-stored-procedure"></a>Bir saklı yordam geçirme
Geçiş adımları şunlardır:

1. Yorumlanan normal saklı yordam oluşturma yordamı betik edinin.
2. Kendi üst bilgisi önceki şablon eşleşecek şekilde yeniden yazın.
3. T-SQL kodunu saklı yordamı yerel koda derlenmiş saklı yordamlar için desteklenmeyen özellikleri kullanıp kullanmadığını belirlemek. Geçici Çözümler gerekirse uygulayın.
   
   * Ayrıntılar için bkz. [geçiş sorunları için yerel olarak depolanan yordamları derlenmiş](https://msdn.microsoft.com/library/dn296678.aspx).
4. Eski bir saklı yordam sp_rename seçeneğini kullanarak yeniden adlandırın. Veya yalnızca sürükleyip bırakın.
5. Düzenlenen oluşturma yordamı T-SQL betiğinizi çalıştırmak.

## <a name="step-6-run-your-workload-in-test"></a>6. Adım: İş yükünüz testi çalıştırın
Bir iş yükü, üretim veritabanınız çalışan iş yüküne benzer test veritabanınızda çalıştırın. Bu, bellek içi özellik kullanımınız için tablolarına ve depolanmış yordamlarına tarafından gerçekleştirilen performans kazancı ortaya.

İş yükü önemli öznitelikleri şunlardır:

* Eş zamanlı bağlantı sayısı.
* Okuma/yazma oranı.

Uyumlu hale getirmenizi ve test iş yükü çalıştırmak için gösterilen kullanışlı ostress.exe Aracı'nı kullanmayı düşünün [burada](sql-database-in-memory.md).

Ağ gecikmesini en aza indirmek için veritabanının mevcut olduğu aynı Azure coğrafi bölgesinde testinizi çalıştırın.

## <a name="step-7-post-implementation-monitoring"></a>7. Adım: Uygulama sonrası izleme
İzleme, bir bellek içi üretim uygulamalarında performans etkilerini göz önünde bulundurun:

* [Bellek içi depolama alanı izleme](sql-database-in-memory-oltp-monitoring.md).
* [Dinamik yönetim görünümlerini kullanarak Azure SQL Veritabanı’nı izleme](sql-database-monitoring-with-dmvs.md)

## <a name="related-links"></a>İlgili bağlantılar
* [Bellek içi OLTP (bellek içi iyileştirme)](https://msdn.microsoft.com/library/dn133186.aspx)
* [Yerel koda derlenmiş saklı yordamlar giriş](https://msdn.microsoft.com/library/dn133184.aspx)
* [Belleği en iyi duruma getirme Danışmanı](https://msdn.microsoft.com/library/dn284308.aspx)

