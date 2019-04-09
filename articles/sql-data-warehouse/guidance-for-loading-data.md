---
title: En iyi veri yükleme uygulamaları - Azure SQL Veri Ambarı | Microsoft Docs
description: Azure SQL Veri Ambarı’na veri yüklemeye yönelik öneriler ve performans iyileştirmeleri.
services: sql-data-warehouse
author: ckarst
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 04/17/2018
ms.author: cakarst
ms.reviewer: jrasnick
ms.custom: seoapril2019
ms.openlocfilehash: a8cb3714d11994b36991e56df7fc0f97d08c89ff
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59256915"
---
# <a name="best-practices-for-loading-data-into-azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı’na veri yüklemek için en iyi uygulamalar

Azure SQL Veri Ambarı’na veri yüklemeye yönelik öneriler ve performans iyileştirmeleri.

## <a name="preparing-data-in-azure-storage"></a>Azure Depolama’da verileri hazırlama

Gecikme süresini en aza indirmek için depolama katmanınız ve veri ambarınızı birlikte bulundurun.

Verileri ORC Dosya Biçimi’ne aktarırken, verilerde büyük metin sütunları varsa Java belleği yetersiz hatası gibi hatalar alabilirsiniz. Bu sınırlama için bir geçici çözüm olarak, sütunların yalnızca bir alt kümesini dışarı aktarın.

PolyBase 1.000.000 bayttan daha fazla veri içeren satırları yükleyemez. Azure Blob depolama veya Azure Data Lake Store’da verileri metin dosyalarına yerleştirdiğinizde, dosyaların 1.000.000 bayttan daha az veri içermesi gerekir. Bu bayt sınırlaması, tablo şemasından bağımsız olarak geçerlidir.

Tüm dosya biçimleri farklı performans özelliklerine sahiptir. En hızlı yükleme için, sıkıştırılan sınırlandırılmış metin dosyaları kullanın. UTF-8 ve UTF-16 arasındaki performans farkı azdır.

Büyük sıkıştırılmış dosyaları daha küçük sıkıştırılmış dosyalara bölün.

## <a name="running-loads-with-enough-compute"></a>Yükleri yeterli işlemle çalıştırma

En yüksek yükleme hızı için aynı anda yalnızca bir yük işi çalıştırın. Bunu yapmak uygun değilse, en az sayıda yükü eşzamanlı olarak çalıştırın. Büyük bir yükleme işi bekliyorsanız, yükten önce veri ambarınızın ölçeğini genişletmeyi düşünün.

Yükleri uygun işlem kaynaklarıyla çalıştırmak için, yükleri çalıştırmaya ayrılmış yükleme kullanıcıları oluşturun. Her bir yükleme kullanıcısını belirli bir kaynak sınıfına atayın. Bir yük çalıştırmak için yükleme kullanıcılarını biri olarak oturum açın ve açıp yükü çalıştırın. Yük, kullanıcının kaynak sınıfıyla çalıştırılır.  Bu yöntem bir kullanıcının kaynak sınıfını geçerli kaynak sınıfının ihtiyacına uygun olarak değiştirmeye çalışmaktan daha basittir.

### <a name="example-of-creating-a-loading-user"></a>Yükleme kullanıcısı oluşturmayla ilgili örnek

Bu örnekte, staticrc20 kaynak sınıfı için bir yükleme kullanıcısı oluşturulmaktadır. İlk adım, **ana öğeye bağlanmak** ve oturum açma bilgisi oluşturmaktır.

```sql
   -- Connect to master
   CREATE LOGIN LoaderRC20 WITH PASSWORD = 'a123STRONGpassword!';
```

Veri ambarına bağlanın ve bir kullanıcı oluşturun. Aşağıdaki kodda, mySampleDataWarehouse adlı bir veritabanına bağlı olduğunuz varsayılmıştır. Kodda, LoaderRC20 adlı bir kullanıcı oluşturma ve veritabanı üzerinde kullanıcı denetimi izni sağlama işlemlerinin nasıl yapılacağı gösterilmiştir. Daha sonra, kullanıcı staticrc20 veritabanı rolünün bir parçası olarak eklenmiştir.  

```sql
   -- Connect to the database
   CREATE USER LoaderRC20 FOR LOGIN LoaderRC20;
   GRANT CONTROL ON DATABASE::[mySampleDataWarehouse] to LoaderRC20;
   EXEC sp_addrolemember 'staticrc20', 'LoaderRC20';
```

StaticRC20 kaynak sınıflarıyla için kaynaklara sahip bir yük çalıştırmak için LoaderRC20 olarak oturum açın ve yükü çalıştırın.

Yükleri dinamik yerine statik kaynak sınıfları altında çalıştırın. Statik kaynak sınıflarını kullanmak, bağımsız olarak aynı kaynakları garantiler, [veri ambarı birimleri](what-is-a-data-warehouse-unit-dwu-cdwu.md). Bir dinamik kaynak sınıfı kullanırsanız, kaynaklar hizmet düzeyinize göre değişir. Dinamik sınıflar için, daha düşük bir hizmet düzeyi, yükleme kullanıcınız için daha büyük bir kaynak sınıfı kullanmanız gerektiğini gösteriyor olabilir.

## <a name="allowing-multiple-users-to-load"></a>Birden çok kullanıcının yüklemesine izin verme

Genellikle bir veri ambarına veri yükleyebilen birden çok kullanıcı olması gerekir. İle yükleme [CREATE TABLE AS SELECT (Transact-SQL)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) veritabanının CONTROL izinleri gerektirir.  CONTROL izinleri tüm şemalara denetim erişimi verir. Tüm yükleme kullanıcılarının tüm şemalarda denetim erişimine sahip olmasını istemeyebilirsiniz. İzinleri sınırlandırmak için, DENY CONTROL deyimini kullanabilirsiniz.

Örneğin, A departmanı için schema_A ve B departmanı için schema_B adında veritabanı şemaları olduğunu düşünelim. user_A ve user_B adlı veritabanı kullanıcıları sırayla A ve B departmanları için PolyBase yükleme kullanıcıları olsun. Her ikisine de CONTROL veritabanı izinleri verilmiştir. A ve B şemalarını oluşturanlar DENY kullanarak bu şemaları kilitler:

```sql
   DENY CONTROL ON SCHEMA :: schema_A TO user_B;
   DENY CONTROL ON SCHEMA :: schema_B TO user_A;
```

User_A ve user_B artık diğer departmanın şemasına erişemez.

## <a name="loading-to-a-staging-table"></a>Hazırlama tablosuna yükleme

Bir veri ambarı tablosuna verileri taşımak için en yüksek yükleme hızını elde etmek üzere verileri bir hazırlama tablosuna yükleyin.  Hazırlama tablosunu bir yığın olarak tanımlayın ve dağıtım seçeneği için hepsine bir kez yöntemini kullanın. 

Yüklemenin genellikle ilk olarak bir hazırlama tablosuna yüklediğiniz, daha sonra verileri bir üretim veri ambarı tablosuna eklediğiniz iki adımlı bir işlem olduğunu göz önünde bulundurun. Üretim tablosu bir karma dağıtım kullanıyorsa, hazırlama tablosunu karma dağıtımla tanımlamanız durumunda toplam yükleme ve ekleme süresi daha hızlı olabilir. Hazırlama tablosuna yükleme işlemi daha uzun sürer, ancak satırları üretim tablosuna eklemeyi içeren ikinci adım, dağıtımlar arasında veri hareketi oluşturmaz.

## <a name="loading-to-a-columnstore-index"></a>Bir columnstore dizinine yükleme

Columnstore dizinleri, verileri yüksek kaliteli satır grupları olarak sıkıştırmak için yüksek miktarlarda bellek gerektirir. En iyi sıkıştırma ve dizin verimliliği için, columnstore dizininin her satır grubunda en fazla 1.048.576 satırı sıkıştırması gerekir. Bellek baskısı olduğunda, columnstore dizini en yüksek sıkıştırma oranlarına ulaşamayabilir. Bu da sorgu performansını etkiler. Derinlemesine bir bakış için, bkz. [Columnstore bellek iyileştirmeleri](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md).

- Yükleme kullanıcısının en yüksek sıkıştırma oranlarına ulaşmak için yeterli belleğe sahip olduğundan emin olmak için, orta veya büyük bir kaynak sınıfının üyesi olan yükleme kullanıcılarını kullanın. 
- Yeni satır gruplarını tamamen doldurmak için yeterli satır yükleyin. Bir toplu yükleme sırasında her 1.048.576 satır, tam bir satır grubu olarak doğrudan columnstore’da sıkıştırılır. 102.400’den daha az satır içeren yükler, satırları bir b ağacı dizininde tutulduğu deltastore’a gönderir. Çok az sayıda satır yüklerseniz, hepsi deltastore’a gönderilerek hemen columnstore biçiminde sıkıştırılmayabilir.

## <a name="handling-loading-failures"></a>Yükleme hatalarını işleme

Bir dış tablo kullanan bir yük *"Sorgu iptal edildi-- dış bir kaynaktan okunurken en yüksek reddedilme sayısına ulaşıldı"* hatasıyla başarısız olabilir. Bu ileti, dış verilerinizin kirli kayıtlar içerdiğini gösterir. Veri türleri ve sütun sayısı dış tablonun sütun tanımlarıyla eşleşmiyorsa veya veriler belirtilen dış dosya biçimine uymuyorsa veri kaydı kirli olarak değerlendirilir. 

Kirli kayıtları düzeltmek için dış tablo ve dış dosya biçimlerinizin doğru olduğundan ve dış verilerinizin bu tanımlara uyduğundan emin olun. Dış verilerin alt kümesinin kirli olması durumunda, CREATE EXTERNAL TABLE içinde reddetme seçeneklerini kullanarak sorgularınız için bu kayıtları reddedebilirsiniz.

## <a name="inserting-data-into-a-production-table"></a>Üretim tablosuna veri ekleme

Küçük bir tabloya bir [INSERT](/sql/t-sql/statements/insert-transact-sql) deyimiyle tek seferlik yükleme yapmak veya `INSERT INTO MyLookup VALUES (1, 'Type 1')` gibi bir deyimle bir aramanın düzenli aralıklarla yeniden yüklenmesi yeterlidir.  Ancak, tekli ton eklemeleri toplu yükleme gerçekleştirmek kadar verimli değildir. 

Gün boyunca binlerce ekleme yapmanız gerekiyorsa, eklemeleri toplu olarak yüklemek için toplu iş haline getirin.  Bir dosyaya tekli eklemeleri eklemek için işlemlerinizi geliştirin ve ardından dosyayı düzenli olarak yükleyen başka bir işlem oluşturun.

## <a name="creating-statistics-after-the-load"></a>Yüklemeden sonra istatistik oluşturma

Sorgu performansını geliştirmek için ilk yüklemeden veya verilerdeki önemli değişikliklerden sonra istatistiklerin tüm sütunlarda oluşturulması önemlidir.  İstatistiklerin ayrıntılı bir açıklaması için bkz. [İstatistikler](sql-data-warehouse-tables-statistics.md). Aşağıdaki örnek Customer_Speed tablosunun beş sütununda istatistikler oluşturur.

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="rotate-storage-keys"></a>Depolama anahtarlarını döndürme

Blob depolamanızın erişim anahtarlarını düzenli olarak değiştirmek iyi bir güvenlik uygulamasıdır. Blob depolama hesabınız için anahtarları geçirmenizi sağlayan iki depolama anahtarınız bulunur.

Azure Depolama hesabı anahtarlarını döndürmek için:

Anahtarı değişen her depolama hesabı için, [VERİTABANI KAPSAMLI KİMLİK BİLGİSİNİ DEĞİŞTİR](/sql/t-sql/statements/alter-database-scoped-credential-transact-sql) yazın.

Örnek:

Özgün anahtar oluşturuldu

```sql
CREATE DATABASE SCOPED CREDENTIAL my_credential WITH IDENTITY = 'my_identity', SECRET = 'key1'
``` 

1. anahtardan 2. anahtara geçin

```sql
ALTER DATABASE SCOPED CREDENTIAL my_credential WITH IDENTITY = 'my_identity', SECRET = 'key2' 
```

Temel dış veri kaynaklarında başka bir değişiklik yapılması gerekmez.

## <a name="next-steps"></a>Sonraki adımlar

- PolyBase ve Ayıklama, Yükleme ve Dönüştürme (ELT) işlemi hakkında daha fazla bilgi edinmek için, bkz. [SQL Veri Ambarı için ELT Tasarlama](design-elt-data-loading.md).
- Yükleme öğreticisi için, bkz. [Azure blob depolamadan verileri Azure SQL Veri Ambarı’na yüklemek için PolyBase kullanma](load-data-from-azure-blob-storage-using-polybase.md).
- Veri yüklerini izlemek için bkz. [DMV’leri kullanarak iş yükünüzü izleme](sql-data-warehouse-manage-monitor.md).



