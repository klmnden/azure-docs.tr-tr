---
title: Azure SQL veri ambarı sorunlarını giderme | Microsoft Docs
description: Azure SQL veri ambarı sorunlarını giderme.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 27445eb754a5e985485db101c9d0fe1ba8aa2451
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="troubleshooting-azure-sql-data-warehouse"></a>Azure SQL veri ambarı sorunlarını giderme
Bu konu genel sorun giderme soru listeler.

## <a name="connecting"></a>Bağlanıyor
| Sorun | Çözüm |
|:--- |:--- |
| Oturum açma 'NT Yetkili\Anonim Oturum açma' kullanıcısı için başarısız oldu. (Microsoft SQL Server, hata: 18456) |Bu hata bir AAD kullanıcı ana veritabanına bağlanmayı dener, ancak bir kullanıcının yöneticisinde yok oluşur.  Bu sorunu gidermek için da, bağlantı zaman bağlanmak veya ana veritabanına kullanıcı eklemek istediğiniz SQL Data Warehouse belirtin.  Bkz: [güvenliğine genel bakış] [ Security overview] daha fazla ayrıntı için makale. |
| Sunucu asıl "KullanıcıAdım" geçerli güvenlik bağlamı altında "ana" veritabanına erişim mümkün değil. Kullanıcının varsayılan veritabanı açılamıyor. Oturum açma başarısız. Oturum açma 'KullanıcıAdım' kullanıcısı için başarısız oldu. (Microsoft SQL Server, hata: 916) |Bu hata bir AAD kullanıcı ana veritabanına bağlanmayı dener, ancak bir kullanıcının yöneticisinde yok oluşur.  Bu sorunu gidermek için da, bağlantı zaman bağlanmak veya ana veritabanına kullanıcı eklemek istediğiniz SQL Data Warehouse belirtin.  Bkz: [güvenliğine genel bakış] [ Security overview] daha fazla ayrıntı için makale. |
| CTAIP hata |SQL server ana veritabanı üzerinde ancak SQL veri ambarı veritabanında bir oturum oluşturulduğunda bu hata oluşabilir.  Bu hatayla karşılaşırsanız, göz atın [güvenliğine genel bakış] [ Security overview] makalesi.  Bu makalede, bir oturum açma ve kullanıcı yöneticisinde nasıl oluşturulacağını ve SQL veri ambarı veritabanında bir kullanıcı oluşturmak nasıl açıklanmaktadır. |
| Güvenlik Duvarı tarafından engellendi |Azure SQL veritabanları, yalnızca bir veritabanı erişimi IP adreslerini bilinen emin olmak için sunucu ve veritabanı düzeyi güvenlik duvarları tarafından korunur. Bağlanmadan önce güvenlik duvarları, açıkça etkinleştirmelisiniz anlamına gelir varsayılan ve IP adresi veya adres aralığı ile güvenlidir.  Erişim için güvenlik duvarını yapılandırmak için adımları [sunucusu güvenlik duvarı erişimi için istemci IP yapılandırma] [ Configure server firewall access for your client IP] içinde [yönergeleri sağlama][Provisioning instructions]. |
| Aracı veya sürücüsü ile bağlantı kurulamıyor |SQL veri ambarı önerir kullanarak [SSMS][SSMS], [Visual Studio için SSDT][SSDT for Visual Studio], veya [sqlcmd] [ sqlcmd] verilerinizi sorgulanamıyor. Sürücüleri ve SQL Data Warehouse'a bağlanma hakkında daha fazla bilgi için bkz: [Azure SQL Data Warehouse için sürücüleri] [ Drivers for Azure SQL Data Warehouse] ve [Azure SQL Data Warehouse Bağlan] [ Connect to Azure SQL Data Warehouse] makaleleri. |

## <a name="tools"></a>Araçlar
| Sorun | Çözüm |
|:--- |:--- |
| Visual Studio Nesne Gezgini AAD kullanıcılarını eksik |Bu bilinen bir sorundur.  Geçici bir çözüm olarak, kullanıcılar görüntülemek [sys.database_principals][sys.database_principals].  Bkz: [Azure SQL veri ambarı için kimlik doğrulaması] [ Authentication to Azure SQL Data Warehouse] SQL Data Warehouse ile Azure Active Directory kullanma hakkında daha fazla bilgi edinmek için. |
|Komut dosyası, komut dosyası oluşturma Sihirbazı'nı kullanarak veya SSMS ile bağlanma kılavuzdur, yavaş, askıdaki veya üretmeye hataları| Kullanıcı ana veritabanında oluşturduğunuz emin olun. Komut dosyası seçenekleri de altyapı sürümü "Microsoft Azure SQL veri ambarı Edition" olarak ayarlanır ve "Microsoft Azure SQL veritabanı" altyapısı türü olduğundan emin olun.|

## <a name="performance"></a>Performans
| Sorun | Çözüm |
|:--- |:--- |
| Sorgu performans sorunlarını giderme |Belirli bir sorgu giderilir çalışıyorsanız başlayın [sorgularınızı izlemek öğrenme][Learning how to monitor your queries]. |
| Zayıf sorgu performansı ve planları eksik istatistikleri sonucunu görülür |En yaygın performansın tablolarınızı istatistiklerle eksikliği nedenidir.  Bkz: [tablo istatistikleri koruma] [ Statistics] istatistiklerinin nasıl oluşturulacağı ve neden, performans kritik hakkında ayrıntılar için. |
| Düşük eşzamanlılık / sorguları sıraya alındı |Anlama [iş yükü Yönetim] [ Workload management] bellek ayırma ile eşzamanlılık dengelemeye yönelik olduğunu anlamak için önemlidir. |
| En iyi yöntemler uygulama |Sorgu performansını artırmak için yollarını öğrenmek başlatmak için en iyi yerdir [SQL veri ambarı en iyi yöntemler] [ SQL Data Warehouse best practices] makalesi. |
| Ölçeklendirme performansı nasıl |Bazen performansı iyileştirme için çözüm yalnızca daha fazla işlem sorgular tarafından üssüne eklemektir [SQL veri ambarı ölçeklendirme][Scaling your SQL Data Warehouse]. |
| Zayıf dizin kalitesi zayıf sorgu performansı |Bazı kez sorgular yavaşlar nedeniyle [zayıf columnstore dizini kalite][Poor columnstore index quality].  Bu makalede daha fazla bilgi için bkz: ve nasıl [segment kalitesini artırmak için dizinleri yeniden][Rebuild indexes to improve segment quality]. |

## <a name="system-management"></a>Sistem Yönetimi
| Sorun | Çözüm |
|:--- |:--- |
| Msg 40847: sunucu 45000 izin verilen veritabanı işlem birimi kotasını aşacağından işlem gerçekleştirilemedi. |Ya da [DWU] [ DWU] oluşturmaya çalıştığınız veritabanının veya [kota artışı isteği][request a quota increase]. |
| Alan kullanımı araştırma |Bkz: [tablo boyutları] [ Table sizes] sisteminizi alanı kullanımını anlamak için. |
| Yönetme konusunda Yardım tabloları |Bkz: [tablo genel bakışı] [ Overview] tablolarınızı yönetme konusunda yardım için makale.  Bu makalede içine gibi daha ayrıntılı konulara bağlantılar da içerir. [tablo veri türleri][Data types], [tablo dağıtma][Distribute], [tablo dizin][Index], [bir tablo bölümleme][Partition], [tablo istatistikleri koruma] [ Statistics] ve [geçici tablolar][Temporary]. |
|Saydam veri şifreleme (TDE) ilerleme çubuğu Azure Portalı'nda güncelleştirilmiyor|TDE durumunu görüntüleyebilirsiniz [powershell](/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryption).|

## <a name="polybase"></a>Polybase
| Sorun | Çözüm |
|:--- |:--- |
| Büyük satırları nedeniyle yükleme başarısız |Şu anda büyük satır desteği için Polybase kullanılabilir değil.  Başka bir deyişle, VARCHAR(MAX), NVARCHAR(MAX) veya VARBINARY(MAX) tablonuz içeriyorsa, verilerinizi yüklemek için dış tabloları kullanılamaz.  Büyük satırları yüklenirken şu anda yalnızca Azure Data Factory (BCP), Azure akış analizi, SSIS, BCP veya .NET SQLBulkCopy sınıfı desteklenir. PolyBase destek büyük satırlar için gelecekteki bir sürümde eklenecek. |
| en büyük veri türüne sahip tablosunun BCP yükleme başarısız oluyor |VARCHAR(MAX), NVARCHAR(MAX) veya VARBINARY(MAX) Bazı senaryolarda tablonun sonuna yerleştirilmesi gerektiren bilinen bir sorun yoktur.  En fazla sütun tablonun sonuna taşımayı deneyin. |

## <a name="differences-from-sql-database"></a>SQL veritabanı arasındaki farklar
| Sorun | Çözüm |
|:--- |:--- |
| Desteklenmeyen SQL veritabanı özellikleri |Bkz: [desteklenmeyen tablo özellikleri][Unsupported table features]. |
| Desteklenmeyen SQL veritabanı veri türleri |Bkz: [desteklenmeyen veri türleri][Unsupported data types]. |
| SİLME ve güncelleştirme sınırlamaları |Bkz: [güncelleştirme geçici çözümler][UPDATE workarounds], [DELETE geçici çözümler] [ DELETE workarounds] ve [kullanarak geçici olarak çözmek için CTAS desteklenmeyen UPDATE ve DELETE sözdizimi][Using CTAS to work around unsupported UPDATE and DELETE syntax]. |
| MERGE deyimi desteklenmiyor |Bkz: [birleştirme geçici çözümler][MERGE workarounds]. |
| Saklı yordam sınırlamaları |Bkz: [depolanan yordamı sınırlamaları] [ Stored procedure limitations] saklı yordamlar sınırlamaları bazıları anlamak için. |
| UDF'ler SELECT deyimleri desteklemiyor |Bu bizim UDF'ler geçerli bir kısıtlamasıdır.  Bkz: [CREATE FUNCTION] [ CREATE FUNCTION] destekliyoruz söz dizimi. |

## <a name="next-steps"></a>Sonraki adımlar
Sorununuzu çözüme bulma daha fazla yardım için deneyebilirsiniz diğer bazı kaynaklar aşağıda verilmiştir.

* [Bloglar]
* [Özellik istekleri]
* [Videolar]
* [CAT ekibi blogları]
* [Destek bileti oluşturma]
* [MSDN forumu]
* [Stack Overflow forumu]
* [Twitter]

<!--Image references-->

<!--Article references-->
[Security overview]: sql-data-warehouse-overview-manage-security.md
[SSMS]: /sql/ssms/download-sql-server-management-studio-ssms
[SSDT for Visual Studio]: sql-data-warehouse-install-visual-studio.md
[Drivers for Azure SQL Data Warehouse]: sql-data-warehouse-connection-strings.md
[Connect to Azure SQL Data Warehouse]: sql-data-warehouse-connect-overview.md
[Destek bileti oluşturma]: sql-data-warehouse-get-started-create-support-ticket.md
[Scaling your SQL Data Warehouse]: sql-data-warehouse-manage-compute-overview.md
[DWU]: sql-data-warehouse-overview-what-is.md
[request a quota increase]: sql-data-warehouse-get-started-create-support-ticket.md
[Learning how to monitor your queries]: sql-data-warehouse-manage-monitor.md
[Provisioning instructions]: sql-data-warehouse-get-started-provision.md
[Configure server firewall access for your client IP]: sql-data-warehouse-get-started-provision.md
[SQL Data Warehouse best practices]: sql-data-warehouse-best-practices.md
[Table sizes]: sql-data-warehouse-tables-overview.md#table-size-queries
[Unsupported table features]: sql-data-warehouse-tables-overview.md#unsupported-table-features
[Unsupported data types]: sql-data-warehouse-tables-data-types.md#unsupported-data-types
[Overview]: sql-data-warehouse-tables-overview.md
[Data types]: sql-data-warehouse-tables-data-types.md
[Distribute]:/sql-data-warehouse-tables-distribute.md
[Index]: sql-data-warehouse-tables-index.md
[Partition]: sql-data-warehouse-tables-partition.md
[Statistics]: sql-data-warehouse-tables-statistics.md
[Temporary]: sql-data-warehouse-tables-temporary.md
[Poor columnstore index quality]: sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality
[Rebuild indexes to improve segment quality]: sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Workload management]: resource-classes-for-workload-management.md
[Using CTAS to work around unsupported UPDATE and DELETE syntax]: sql-data-warehouse-develop-ctas.md#using-ctas-to-work-around-unsupported-features
[UPDATE workarounds]: sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[DELETE workarounds]: sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[MERGE workarounds]: sql-data-warehouse-develop-ctas.md#replace-merge-statements
[Stored procedure limitations]: sql-data-warehouse-develop-stored-procedures.md#limitations
[Authentication to Azure SQL Data Warehouse]: sql-data-warehouse-authentication.md


<!--MSDN references-->
[sys.database_principals]: /sql/relational-databases/system-catalog-views/sys-database-principals-transact-sql
[CREATE FUNCTION]: /sql/t-sql/statements/create-function-sql-data-warehouse
[sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--Other Web references-->
[Bloglar]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[CAT ekibi blogları]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Özellik istekleri]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN forumu]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse
[Stack Overflow forumu]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videolar]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
