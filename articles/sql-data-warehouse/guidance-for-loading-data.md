---
title: "Veri yükleme kılavuzu - Azure SQL Veri Ambarı | Microsoft Docs"
description: "Azure SQL Veri Ambarı ile veri yükleme ve ELT gerçekleştirme önerileri."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.assetid: 7b698cad-b152-4d33-97f5-5155dfa60f79
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 12/13/2017
ms.author: barbkess
ms.openlocfilehash: 8903be1361d1574a5d81b69223f608ccb7a698ea
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="guidance-for-loading-data-into-azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı’na veri yükleme kılavuzu
Azure SQL Veri Ambarı’na veri yüklemeye yönelik öneriler ve performans iyileştirmeleri. 

- PolyBase ve Ayıklama, Yükleme ve Dönüştürme (ELT) işlemi hakkında daha fazla bilgi edinmek için, bkz. [SQL Veri Ambarı için ELT Tasarlama](design-elt-data-loading.md).
- Yükleme öğreticisi için, bkz. [Azure blob depolamadan verileri Azure SQL Veri Ambarı’na yüklemek için PolyBase kullanma](load-data-from-azure-blob-storage-using-polybase.md).


## <a name="extract-source-data"></a>Kaynak verileri ayıklama

SQL Server veya Azure SQL Veri Ambarı’nda verileri bir ORC Dosya Biçimi’ne dışarı aktarırken, Java yetersiz bellek hataları nedeniyle yoğun metin içerikli sütunların sayısı 50 ile sınırlandırılabilir. Bu sorun için bir geçici çözüm olarak, sütunların yalnızca bir alt kümesini dışarı aktarın.


## <a name="land-data-to-azure"></a>Verileri Azure’a taşıma
PolyBase bir milyon bayttan daha fazla veri içeren satırları yükleyemez. Azure Blob depolama veya Azure Data Lake Store’da verileri metin dosyalarına yerleştirdiğinizde, dosyaların bir milyon bayttan daha az veri içermesi gerekir. Bu, tanımlanan tablo şemasından bağımsız olarak geçerlidir.

Gecikme süresini en aza indirmek için depolama katmanınız ve veri ambarınızı birlikte bulundurun.

## <a name="data-preparation"></a>Veri hazırlama

Tüm dosya biçimleri farklı performans özelliklerine sahiptir. En hızlı yükleme için, sıkıştırılmış sınırlı metin dosyaları kullanın. UTF-8 ve UTF-16 arasındaki performans farkı azdır.

Büyük sıkıştırılmış dosyaları daha küçük sıkıştırılmış dosyalara bölün.

## <a name="create-designated-loading-users"></a>Ayrılmış yükleme kullanıcıları oluşturma
Yükleri uygun bilgisayar kaynaklarıyla çalıştırmak için, çalıştırma yükleri için ayrılmış yükleme kullanıcıları oluşturun. Her bir yükleme kullanıcısını belirli bir kaynak sınıfına atayın. Bir yük çalıştırmak için, yükleme kullanıcılarından biri olarak oturum açıp yükü çalıştırın. Yük, kullanıcının kaynak sınıfıyla çalıştırılır.  Bu yöntem bir kullanıcının kaynak sınıfını geçerli sınıfı ihtiyacına uygun olarak değiştirmeye çalışmaktan daha basittir.

Bu kod staticrc20 kaynak sınıfı için bir yükleme kullanıcısı oluşturur. Kullanıcılara bir veritabanında denetim izni verir ve kullanıcıyı staticrc20 veritabanı rolünün bir üyesi olarak ekler. StaticRC20 kaynak sınıflarıyla bir yükü çalıştırmak için, LoaderRC20 olarak oturum açıp yükü çalıştırın. 

    ```sql
    CREATE LOGIN LoaderRC20 WITH PASSWORD = 'a123STRONGpassword!';
    CREATE USER LoaderRC20 FOR LOGIN LoaderRC20;
    GRANT CONTROL ON DATABASE::[mySampleDataWarehouse] to LoaderRC20;
    EXEC sp_addrolemember 'staticrc20', 'LoaderRC20';
    ```

Yükleri dinamik yerine statik kaynak sınıfları altında çalıştırın. Statik kaynak sınıflarını kullanmak, [hizmet düzeyinizden](performance-tiers.md#service-levels) bağımsız olarak aynı kaynakları garantiler. Bir dinamik kaynak sınıfı kullanırsanız, kaynaklar hizmet düzeyinize göre değişir. Dinamik sınıflar için, daha düşük bir hizmet düzeyi, yükleme kullanıcınız için daha büyük bir kaynak sınıfı kullanmanız gerektiğini gösteriyor olabilir.

### <a name="example-for-isolating-loading-users"></a>Yükleme kullanıcılarını yalıtma örneği

Genellikle bir SQL DW’ye veri yükleyebilen birden çok kullanıcı olması gerekir. [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)] veritabanında CONTROL izinleri gerektirdiğinden, tüm şemalarda denetim izinlerine sahip birden fazla kullanıcı olur. Bunu sınırlandırmak için, DENY CONTROL deyimini kullanabilirsiniz.

Örneğin, A departmanı için schema_A ve B departmanı için schema_B olduğunu düşünelim. user_A ve user_B adlı veritabanı kullanıcıları sırayla A ve B departmanları için PolyBase yükleme kullanıcıları olsun. Her ikisine de CONTROL veritabanı izinleri verilmiştir. A ve B şemasının sahipleri DENY kullanarak şemalarını kilitler:

```sql
   DENY CONTROL ON SCHEMA :: schema_A TO user_B;
   DENY CONTROL ON SCHEMA :: schema_B TO user_A;
```   

User_A ve user_B artık diğer departmanın şemasına erişemez.


## <a name="load-to-a-staging-table"></a>Hazırlama şablonuna yükleme

En hızlı yükleme hızları için, bir round_robin yığın hazırlama tablosuna yükleyin. Bu, verileri Azure Depolama katmanından SQl Veri Ambarı’na taşımanın en etkili yoludur.

Büyük bir yükleme işi bekliyorsanız veri ambarınızın ölçeğini artırın.

En iyi yük performansı için tek seferde yalnızca bir iş çalıştırın

### <a name="optimize-columnstore-index-loads"></a>Columnstore dizin yüklemelerini en iyi duruma getirme

Columnstore dizinleri, verileri yüksek kaliteli satır grupları olarak sıkıştırmak için yüksek miktarda bellek gerektirir. En iyi sıkıştırma ve dizin verimliliği için, columnstore dizininin her satır grubunda 1.048.576 satırı sıkıştırması gerekir. Bu, satır grubu başına en fazla satır sayısıdır. Bellek baskısı olduğunda, columnstore dizini en yüksek sıkıştırma oranlarına ulaşamayabilir. Bu da sorgu performansını etkiler. Derinlemesine bir bakış için, bkz. [Columnstore bellek iyileştirmeleri](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md).

- Yükleme kullanıcısının en yüksek sıkıştırma oranlarına ulaşmak için yeterli belleğe sahip olduğundan emin olmak için, orta veya büyük bir kaynak sınıfının üyesi olan yükleme kullanıcılarını kullanın. 
- Yeni satır gruplarını tamamen doldurmak için yeterli satır yükleyin. Toplu yükleme sırasında, her 1.048.576 satır doğrudan columnstore’a gider. 102.400’den az satıra sahip yükler satırları sıkıştırma için yeterli satır olana kadar kümeli bir dizinde bekleten deltastore’a gönderir. Çok az sayıda satır yüklerseniz, hepsi deltastore’a gönderilerek hemen columnstore biçiminde sıkıştırılmayabilir.


### <a name="handling-loading-failures"></a>Yükleme hatalarını işleme

Bir dış tablo kullanan bir yük *"Sorgu iptal edildi-- dış bir kaynaktan okunurken en yüksek reddedilme sayısına ulaşıldı"* hatasıyla başarısız olabilir. Bu, dış verilerinizin *kirli* kayıtlar içerdiğini gösterir. Gerçek veri türü/sütun sayısı dış tablonun sütun tanımlarıyla eşleşmiyorsa veya veriler belirtilen dış dosya biçimine uymuyorsa veri kaydı 'kirli' olarak değerlendirilir. 

Bunu düzeltmek için dış tablo ve dış dosya biçimlerinizin doğru olduğundan ve dış verilerinizin bu tanımlara uyduğundan emin olun. Dış verilerin alt kümesinin kirli olması durumunda, CREATE EXTERNAL TABLE DDL içinde reddetme seçeneklerini kullanarak sorgularınız için bu kayıtları reddedebilirsiniz.



## <a name="insert-data-into-production-table"></a>Üretim tablosuna veri ekleme
Bunlar üretim tablolarına satır eklemeye yönelik önerilerdir.


### <a name="batch-insert-statements"></a>Toplu INSERT deyimleri
Küçük bir tabloya bir [INSERT](/sql/t-sql/statements/insert-transact-sql.md) deyimiyle tek seferlik yükleme yapmak veya `INSERT INTO MyLookup VALUES (1, 'Type 1')` gibi bir deyimle bir aramanın düzenli aralıklarla yeniden yüklenmesi işinizi fazlasıyla görebilir.  Tekli ton eklemeleri toplu yükleme gerçekleştirmek kadar verimli değildir. 

Gün boyunca binlerce ekleme yapmanız gerekiyorsa, eklemeleri toplu olarak yüklemek için toplu iş haline getirin.  Bir dosyaya tekli eklemeleri eklemek için işlemlerinizi geliştirin ve ardından dosyayı düzenli olarak yükleyen başka bir işlem oluşturun.

### <a name="create-statistics-after-the-load"></a>Yüklemeden sonra istatistik oluşturma

Sorgu performansını geliştirmek için ilk yüklemeden veya verilerdeki önemli değişikliklerden sonra istatistiklerin tüm sütunlarda oluşturulması önemlidir.  İstatistiklerin ayrıntılı bir açıklaması için bkz. [İstatistikler][İstatistikler]. Aşağıdaki örnek Customer_Speed tablosunun beş sütununda istatistikler oluşturur.

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="rotate-storage-keys"></a>Depolama anahtarlarını döndürme
Blob depolamanızın erişim anahtarlarını düzenli olarak değiştirmek iyi bir güvenlik uygulamasıdır. Blob depolama hesabınız için iki depolama anahtarınız bulunur. Bu anahtarlar arasında geçiş yapabilmeniz içindir.

Azure Depolama hesabı anahtarlarını döndürmek için:

1. İkincil depolama erişim anahtarını temel alan ikinci veritabanı kapsamlı kimlik bilgilerini oluşturun.
2. Bu yeni kimlik bilgilerini temel alan ikinci bir dış veri kaynağı oluşturun.
3. Dış tabloları yeni dış veri kaynaklarına işaret etmek üzere bırakın ve oluşturun. 

Dış tablolarınızı yeni veri kaynağına geçirdikten sonra, bu temizleme görevlerini gerçekleştirin:

1. İlk dış veri kaynağını bırakın.
2. Birincil depolama erişim anahtarını temel alan ilk veritabanı kapsamlı kimlik bilgilerini bırakın.
3. Azure’da oturum açıp bir sonraki döndürme için hazır olması için birincil erişim anahtarını yeniden oluşturun.


## <a name="next-steps"></a>Sonraki adımlar
Yükleme işlemini izlemek için, bkz. [DMV’leri kullanarak iş yükünüzü izleme](sql-data-warehouse-manage-monitor.md).



