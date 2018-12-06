---
title: Azure SQL veri ambarı sürüm notları Ekim 2018 | Microsoft Docs
description: Azure SQL veri ambarı için sürüm notları.
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 12/04/2018
ms.author: mausher
ms.reviewer: twounder
ms.openlocfilehash: e67edf382a49839d890d2c1dec50c44bbb19705a
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52966832"
---
# <a name="whats-new-in-azure-sql-data-warehouse-october-2018"></a>Azure SQL veri ambarı'nda yenilikler nelerdir? Ekim 2018
Azure SQL veri ambarı, sürekli olarak iyileştirmeler alır. Bu makalede, Ekim 2018'de sunulan değişiklikler ve yeni özellikleri açıklar.

## <a name="devops-for-data-warehousing"></a>Veri ambarlama için DevOps
SQL veri ambarı'nı (SQL DW) için yüksek oranda istenen özellik Önizleme desteği için SQL Server veri Aracı (SSDT) Visual Studio ile artık kullanılabilir! Geliştirici ekipleri, artık bir tek, sürüm denetimli kod temeli üzerinde işbirliği yapın ve hızlı bir şekilde dünyanın herhangi bir örneğe değişiklikleri dağıtın. Birleştirme ilgileniyor musunuz? Bu özellik şu anda önizleme olarak kullanıma sunuldu! Ziyaret ederek kaydedebilirsiniz [SQL veri ambarı Visual Studio SQL Server veri Araçları (SSDT) - önizleme kayıt formu](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR4-brmKy3TZOjoktwuHd7S1UODkwQ1lVMEw1NDBGRjNLRDNWOFlQRUpIRi4u). Yüksek talep göz önünde bulundurulduğunda, müşterilerimiz için en iyi deneyimi sağlamak için Önizleme kabul edilmeyi yönetiyorsunuz. Kaydolduktan sonra Hedefimiz yedi iş günü içinde durumunuzu doğrulamaktır.

## <a name="row-level-security-generally-available"></a>Satır düzeyinde güvenlik genel kullanıma sunuldu
Azure SQL veri ambarı (SQL DW) artık, hassas verilerin güvenliğini sağlamak için güçlü bir özellik ekleme satır düzeyi güvenlik (RLS) destekler. RLS sunulmasıyla birlikte, hangi satırların erişebilen gibi tablolardaki satır erişimi denetlemek için güvenlik ilkeleri uygulayabilirsiniz. RLS, veri ambarınızın yeniden tasarlamanız gerek kalmadan bu ayrıntılı erişim denetimi sağlar. RLS basitleştirir genel güvenlik modeli olarak erişim kısıtlama mantığı bulunduğu veritabanı katmanındaki yerine başka bir uygulamadaki verileri işlerini hayatınızdan çıkarın. RLS, ayrıca erişim denetimi Yönetimi satırları filtrelemek için görünümler tanıtmak için ihtiyacını ortadan kaldırır. Bu tüm müşterilerimiz için kurumsal düzeyde güvenlik özelliği için ek ücret yoktur.

## <a name="advanced-advisors"></a>Gelişmiş danışmanları
Gelişmiş Azure SQL veri ambarı (SQL DW) yalnızca ayarlama ek veri ambarı öneriler ve ölçümler ile basit alındı. Azure Danışmanı aracılığıyla ek Gelişmiş performans önerisi, elinizin dahil olmak üzere:
1.  Uyarlamalı önbelleği – önerilir, önbellek kullanımını iyileştirmek için ölçek.
2.  Dağıtım tablosu: veri taşıma azaltmak ve iş yükü performansı artırmak için tablolar çoğaltmak ne zaman belirleme. 
3.  Tempdb – tempdb çekişmeyi azaltmak için ölçeği ve kaynak yapılandırmak için sınıfları olduğunda anlayın.

Veri ambarı ölçümleri ile daha ayrıntılı bir tümleştirme yoktur [Azure İzleyici](https://azure.microsoft.com/blog/enhanced-capabilities-to-monitor-manage-and-integrate-sql-data-warehouse-in-the-azure-portal/) genel bakış dikey penceresinde bir Gelişmiş özelleştirilebilir izleme grafiğine için neredeyse gerçek zamanlı ölçümler de dahil olmak üzere. Artık kullanımı izlerken veya veri ambarı önerilerini doğrulayıp uygularken Azure İzleyici ölçümlerine erişmek için veri ambarı genel bakış dikey penceresinden ayrılmanız gerekmez. Ayrıca, yeni ölçüm vardır tempdb ve performans önerilerinizi tamamlamak üzere Uyarlamalı önbellek kullanımı gibi kullanılabilir.

## <a name="advanced-tuning-with-integrated-advisors"></a>Gelişmiş tümleşik Danışmanlarıyla ayarlama
Gelişmiş Azure SQL veri ambarı (SQL DW) yalnızca ayarlama ek veri ambarı öneriler, ölçümleri ve Azure Danışmanı'nı ve Azure İzleyici ile tümleşik bir deneyim sağlayan portalına genel bakış dikey penceresinde yeniden tasarlanmış basit alındı.

## <a name="accelerated-database-recovery-adr"></a>Hızlandırılmış veritabanı kurtarma (ADR)
Azure SQL veri ambarı hızlandırılmış veritabanı kurtarma (ADR), artık genel Önizleme aşamasındadır. ADR bir yeni SQL Server tamamen geçerli kurtarma işlemini baştan yedekleme WINS'i tarafından veritabanı kullanılabilirlik, özellikle uzun süre çalışan işlemler varlığını önemli ölçüde artıran altyapısıdır. ADR başlıca yararları şunlardır: hızlı ve tutarlı bir veritabanını kurtarma ve anlık bir işlem geri alma.

## <a name="azure-monitor-diagnostics-logs"></a>Azure İzleyici tanılama günlükleri
SQL veri ambarı'nı (SQL DW) artık doğrudan Azure İzleyici tanılama günlükleri ile tümleştirerek analitik iş yükleri hakkında gelişmiş Öngörüler sağlar. Bu yeni özellik, geliştiricilerin uzun bir süre boyunca iş yükü davranışını analiz edin ve bilgiye dayalı kararlar sorgu iyileştirme veya kapasite yönetimi sağlar. Artık bir dış günlüğe kaydetme işlemi aracılığıyla ekledik [Azure İzleyici tanılama günlükleri](https://docs.microsoft.com/azure/monitoring/monitoring-data-collection?toc=/azure/azure-monitor/toc.json#logs) , veri ambarı iş yükü ek Öngörüler sağlar. Tek bir düğmeye tıklatma ile artık özelliklerini kullanarak sorun giderme geçmiş sorgu performansı için tanılama günlüklerini yapılandırmak için [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-queries). Azure İzleyici tanılama günlükleri denetim amacıyla, bir depolama hesabına neredeyse gerçek zamanlı telemetri ınsights ve Log Analytics kullanarak günlüklerini çözümleme özelliği, event hubs'a akış günlükleri yeteneği günlükleri kaydederek özelleştirilebilir bekletme süreleri destekler. ile [oturum sorguları](). Tanılama günlükleri, veri ambarınızın SQL Veri Ambarı için en sık kullanılan performans sorunlarını giderme DMV’lerine eşdeğer telemetri görünümlerinden oluşur. Bu ilk sürümde aşağıdaki sistem dinamik yönetim görünümleri için görünümleri etkinleştirdik:

- [sys.dm_pdw_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql)
- [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql)
- [sys.dm_pdw_dms_workers](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-dms-workers-transact-sql)
- [sys.dm_pdw_waits](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-waits-transact-sql)
- [sys.dm_pdw_sql_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-sql-requests-transact-sql)

## <a name="columnstore-memory-management"></a>Columnstore bellek yönetimi
Sıkıştırılmış sütun deposu satır grubu sayısı arttıkça, bu satır grupları için iç sütun segmenti meta verileri yönetmek için gerekli bellek artırır.  Sonuç olarak, sorgu performansı ve bazı Columnstore dinamik yönetim görünümlerini (Dmv'ler) karşı yürütülen sorgular düşürebilir.  Geliştirmeleri, geliştirilmiş deneyim ve performans gibi sorguları için önde gelen bu gibi durumlarda, iç meta veri boyutu en iyi duruma getirmek için bu sürümde yapıldı. 

## <a name="azure-data-lake-storage-gen2-integration-ga"></a>Azure Data Lake depolama Gen2 tümleştirme (GA)
Azure SQL veri ambarı (SQL DW) artık yerel tümleştirme sayesinde Azure Data Lake depolama Gen2'ye sahiptir. Müşteriler, artık SQL DW'ye ABFS dış tabloları kullanarak veri yükleyebilir. Data Lake depolama Gen2'de, veri gölleri tümleştirmek müşterilerin bu işlevi etkinleştirir. 

## <a name="bug-fixes"></a>Hata düzeltmeleri

| Unvan | Açıklama |
|:---|:---|
| **Veri ambarları DW2000 ve daha küçük kaynak sınıflarında Parquet hataları için CETAS** | Bu düzeltme, Parquet kod yolu null başvuru oluşturma dış tablo olarak doğru şekilde tanımlar. |
|**Kimlik sütunu değeri bazı CTAS işlemi kaybedebilir** | Bir kimlik sütununun değeri zaman korunmayabilir CTASed için başka bir tablo. Bloga bildirdi: [ https://blog.westmonroepartners.com/azure-sql-dw-identity-column-bugs/ ](https://blog.westmonroepartners.com/azure-sql-dw-identity-column-bugs/). |
| **Bir sorgu çalışmaya devam ederken oturum sonlandırıldığında, bazı durumlarda iç hatası** | Bu düzeltme sorgu hala çalışırken bir oturumun sona ererse InvalidOperationException tetiklenir. |
| **(Kasım 2018'de dağıtılır) Müşteriler, Polybase kullanarak birden çok küçük dosyaları ADLS (Gen1) yüklenmeye çalışılırken bir performansın yaşıyordu.** | Sistem performansı, AAD güvenlik belirtecini doğrulamadan sırasında performansı düşürdüğünü gösterir. Performans sorunları, güvenlik belirteçlerinin önbelleğe almayı etkinleştirerek azaltılabilir. |


## <a name="next-steps"></a>Sonraki adımlar
SQL veri ambarı hakkında biraz bilmek, bilgi nasıl hızlı bir şekilde [SQL veri ambarı oluşturma][create a SQL Data Warehouse]. Azure'da yeniyseniz yeni terimlerle karşılaşabileceğinizi için [Azure sözlüğünü][Azure glossary] yararlı bulabilirsiniz. Alternatif olarak, aşağıdaki diğer SQL Veri Ambarı Kaynakları’na göz atın.  

* [Müşteri başarı hikayeleri]
* [Bloglar]
* [Özellik istekleri]
* [Videolar]
* [Müşteri Danışma Ekibi blogları]
* [Stack Overflow forumu]
* [Twitter]


[Bloglar]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Müşteri Danışma Ekibi blogları]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Müşteri başarı hikayeleri]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Özellik istekleri]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Stack Overflow forumu]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videolar]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[create a SQL Data Warehouse]: ./create-data-warehouse-portal.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md
