---
title: "Azure SQL Veritabanında desteklenmeyen T-SQL deyimleri | Microsoft Belgeleri"
description: "Azure SQL Veritabanında tam olarak desteklenmeyen Transact-SQL deyimleri"
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: c05abd9e-28a7-4c97-9bdf-bc60d08fc92e
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 11/28/2016
ms.author: rickbyh
translationtype: Human Translation
ms.sourcegitcommit: 7db5939c88045ab59d7d9fb4b58f07374d7bca01
ms.openlocfilehash: ebec6ca87cfd5e9d2c4ea726d5d31d20998ef33f


---
# <a name="azure-sql-database-transact-sql-differences"></a>Azure SQL Database Transact-SQL farklılıkları   
Uygulamaların bağımlı olduğu Transact-SQL özelliklerinin çoğu hem Microsoft SQL Server hem de Azure SQL Veritabanında desteklenmektedir. Örneğin veri türleri, işleçler, dize, aritmetik, mantıksal ve imleç işlevleri gibi temel SQL bileşenleri SQL Server'da olduğu şekilde çalışır.

## <a name="why-some-transact-sql-is-not-supported"></a>Bazı Transact-SQL deyimlerinin desteklenmemesinin nedeni
Azure SQL Veritabanı, ana veritabanında ve işletim sisteminde özellikleri bağımlılıklardan ayıracak şekilde tasarlanmıştır. Sonuç olarak birçok sunucu düzeyi etkinlik, SQL Veritabanı için uygun değildir. Genelde sunucu düzeyi seçenekleri ve işletim sistemi bileşenlerini yapılandıran veya dosya sistemi yapılandırmasını belirten Transact-SQL deyimleri kullanım dışıdır. Kullanıcı veritabanı haricinde özellikler gerekli olduğunda, SQL Veritabanı veya başka bir Azure özelliği ya da hizmeti tarafından uygun bir alternatif sunulmaktadır. 

Örneğin Her Zaman Açık yerine Etkin Coğrafi Çoğaltma kullanılmaktadır. Bu nedenle kullanılabilirlik gruplarıyla ilgili Transact-SQL deyimleri SQL Veritabanı tarafından desteklenmez ve Her Zaman Açık özelliğiyle ilgili dinamik yönetimi görünümleri için destek sunulmaz.  

SQL Veritabanı tarafından desteklenen ve desteklenmeyen özelliklerin listesi için bkz. [Azure SQL Veritabanı hakkında dikkat edilmesi gereken noktalar, kılavuzlar ve özellikler](sql-database-features.md).

SQL Server'da kullanım dışı bırakılmış söz dizimleri genelde SQL Veritabanında desteklenmez.

## <a name="transact-sql-syntax-partially-supported-in-sql-database"></a>SQL Veritabanında kısmen desteklenen Transact-SQL söz dizimleri
SQL Veritabanı, karşılık gelen SQL Server 2016 Transact-SQL deyimlerinde mevcut olan bağımsız değişkenlerin hepsini olmasa da bir kısmını destekler. Örneğin `CREATE PROCEDURE` deyimi kullanılabilir ancak tüm `CREATE PROCEDURE` seçenekleri kullanılamaz. Burada tüm söz dizimlerini anlatmak kafa karıştırıcı ve gereksiz olacaktır. Her deyimin desteklenen bölümleriyle ilgili ayrıntılı bilgi için bağlantı verilen söz dizimi konularına başvurun.

- Veritabanları: [CREATE](https://msdn.microsoft.com/library/dn268335.aspx)/[ALTER DATABASE](https://msdn.microsoft.com/library/ms174269.aspx)   
- İşlevler: [CREATE](https://msdn.microsoft.com/library/ms186755.aspx)/[ALTER FUNCTION](https://msdn.microsoft.com/library/ms186967.aspx)   
- Oturum açma bilgileri: [CREATE](https://msdn.microsoft.com/library/ms189751.aspx)/[ALTER LOGIN](https://msdn.microsoft.com/library/ms189828.aspx)   
- Saklı yordamlar: [CREATE](https://msdn.microsoft.com/library/ms187926.aspx)/[ALTER PROCEDURE](https://msdn.microsoft.com/library/ms189762.aspx)   
- Tablolar: [CREATE](https://msdn.microsoft.com/library/dn305849.aspx)/[ALTER TABLE](https://msdn.microsoft.com/library/ms190273.aspx)   
- Türler (özel): [CREATE TYPE](https://msdn.microsoft.com/library/ms175007.aspx)   
- Kullanıcılar: [CREATE](https://msdn.microsoft.com/library/ms173463.aspx)/[ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx)   
- Görünümler: [CREATE](https://msdn.microsoft.com/library/ms187956.aspx)/[ALTER VIEW](https://msdn.microsoft.com/library/ms173846.aspx)   

## <a name="transact-sql-syntax-not-supported-in-sql-database"></a>SQL Veritabanında desteklenmeyen Transact-SQL söz dizimleri   
[Azure SQL Veritabanı hakkında dikkat edilmesi gereken noktalar, kılavuzlar ve özellikler](sql-database-features.md) sayfasında açıklanan desteklenmeyen özelliklerle ilgili Transact-SQL deyimlerine ek olarak aşağıdaki deyim ve deyim grupları da desteklenmemektedir.
- Harmanlanmış sistem nesneleri
- Bağlantıyla ilişkili: Uç nokta deyimleri, `ORIGINAL_DB_NAME`. SQL Veritabanı Windows kimlik doğrulamasını desteklemez ancak ona benzer olan Azure Active Directory kimlik doğrulamasını destekler. Bazı kimlik doğrulaması türleri için SSMS'nin en son sürümü gerekir. Daha fazla bilgi için bkz. [Azure Active Directory Kimlik Doğrulamasını Kullanarak SQL Veritabanına veya SQL Veri Ambarına Bağlanma](sql-database-aad-authentication.md).
- Üç veya dört bölüm adı kullanan veritabanları arası sorgular. (Salt okunur veritabanları arası sorgular [elastik veritabanı sorgusu](sql-database-elastic-query-overview.md) kullanılarak desteklenir.)
- Veritabanları arası sahiplik zinciri, `TRUSTWORTHY` ayarı
- `DATABASEPROPERTY` Bunun yerine `DATABASEPROPERTYEX` kullanın.
- `EXECUTE AS LOGIN` Bunun yerine "EXECUTE AS USER" kullanın.
- Genişletilebilir anahtar yönetimi haricinde şifreleme desteklenir
- Olay: olaylar, olay bildirimleri, sorgu bildirimleri
- Veritabanı dosya yerleşimi, boyut ve Microsoft Azure tarafından otomatik olarak yönetilen veritabanı dosyalarıyla ilgili söz dizimi.
- Microsoft Azure hesabınız aracılığıyla yönetilen yüksek kullanılabilirlikle ilgili söz dizimi. Buna yedekleme, geri yükleme, Her Zaman Açık, veritabanı yansıtması, günlük aktarma ve kurtarma modları için söz dizimleri dahildir.
- SQL Veritabanında bulunmayan günlük okuyucusuna bağlı söz dizimleri: Gönderme Temelli Çoğaltma, Veri Yakalamayı Değiştir. SQL Veritabanı gönderme temelli çoğaltma gönderisinin abonesi olabilir.
- SQL Server Agent veya MSDB veritabanına bağımlı söz dizimleri: uyarılar, işleçler, merkezi yönetim sunucuları. Bunun yerine Azure PowerShell gibi betik uygulamaları kullanın.
- İşlevler: `fn_get_sql`, `fn_virtualfilestats`, `fn_virtualservernodes`
- Genel geçici tablolar
- Sunucu donanımı ayarlarıyla ilgili söz dizimleri: bellek, çalışan iş parçacığı, CPU benzeşimi, izleme bayrakları vs. Bunun yerine hizmet düzeylerini kullanın.
- `HAS_DBACCESS`
- `KILL STATS JOB`
- `OPENQUERY`, `OPENROWSET`, `OPENDATASOURCE`, `BULK INSERT` ve dört kısımlı adlar
- .NET Framework [SQL Server ile CLR tümleştirmesi](http://msdn.microsoft.com/library/ms254963.aspx)
- Anlamsal arama
- Sunucu kimlik bilgileri. Bunun yerine veritabanı kapsamlı kimlik bilgilerini kullanın.
- Sunucu düzeyi öğeler: Sunucu rolleri, `IS_SRVROLEMEMBER`, `sys.login_token`. `GRANT`, `REVOKE` ve `DENY` sunucu düzeyi izinler kullanılamaz ancak bazıları veritabanı düzeyi izinlerle değiştirilmiştir. Sunucu düzeyi kullanışlı DMV'lerden bazıları, eşdeğer veritabanı düzeyi DMV'lerine sahiptir.
- `SET REMOTE_PROC_TRANSACTIONS`
- `SHUTDOWN`
- `sp_addmessage`
- `sp_configure` seçenekleri ve `RECONFIGURE`. Bazı seçenekler [ALTER DATABASE SCOPED CONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx) ile kullanılabilir.
- `sp_helpuser`
- `sp_migrate_user_to_contained`
- SQL Server denetimi. Bunun yerine SQL Veritabanı denetimini kullanın.
- SQL Server izleme
- İzleme bayrakları. Bazı izleme bayrağı öğeleri uyumluluk modlarına taşınmıştır.
- Transact-SQL hata ayıklama
- Tetikleyiciler: Sunucu kapsamlı veya oturum açma tetikleyicileri
- `USE` deyimi: Veritabanı bağlamını farklı bir veritabanıyla değiştirmek için yeni veritabanıyla yeni bir bağlantı kurmanız gerekir.

## <a name="full-transact-sql-reference"></a>Tam Transact-SQL başvurusu
Transact-SQL dil bilgisi, kullanımı ve örnekleri için SQL Server Çevrimiçi Kitapları sayfasında [Transact-SQL Başvurusu (Veritabanı Altyapısı)](https://msdn.microsoft.com/library/bb510741.aspx) bölümüne bakın. 

### <a name="about-the-applies-to-tags"></a>"Uygulandığı öğe" etiketleri hakkında
Transact-SQL başvurusu, SQL Server 2008'den güncel sürümlere kadar olan konu başlıklarını içerir. Konu başlığının altındaki simge çubuğunda dört SQL Server platformu listelenir ve uygulanabilirliği belirtilir. Örneğin kullanılabilirlik grupları SQL Server 2012'de tanıtılmıştır. [CREATE AVAILABILTY GROUP](https://msdn.microsoft.com/library/ff878399.aspx) konu başlığı, deyimin **SQL Server (2012'den itibaren)** için geçerli olduğunu belirtir. Deyim SQL Server 2008, SQL Server 2008 R2, Azure SQL Veritabanı, Azure SQL Veri Ambarı veya Paralel Veri Ambarı için geçerli değildir.

Bazı durumlarda bir konu başlığının üst başlığı bir üründe kullanılıyor olabilir ancak ürünler arasında küçük farklar vardır. Farklar, konu başlığının içinde uygun yerlerde belirtilmiştir.



<!--HONumber=Dec16_HO3-->


