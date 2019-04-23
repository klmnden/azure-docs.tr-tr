---
title: Azure SQL veri ambarı sorunlarını giderme | Microsoft Docs
description: Azure SQL veri ambarı sorunlarını giderme.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 12/04/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: dc78fbc93d625b39379e07f240eef7fbad10d194
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60003862"
---
# <a name="troubleshooting-azure-sql-data-warehouse"></a>Azure SQL veri ambarı sorunlarını giderme
Bu makalede, genel sorun giderme soru listelenmektedir.

## <a name="connecting"></a>Bağlanıyor
| Sorun                                                        | Çözüm                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Oturum açma kullanıcısı 'NT Yetkili\Anonim Oturum açma' için başarısız oldu. (Microsoft SQL Server, hata: 18456) | Bir AAD kullanıcısı ana veritabanına bağlanmayı dener, ancak bir kullanıcının ana veritabanında yok. Bu hata oluşur.  Bu sorunu düzeltmek için ya da SQL veri ambarı bağlantı zamanında bağlanmak veya ana veritabanına kullanıcı eklemek istediğiniz belirtin.  Bkz: [güvenliğine genel bakış] [ Security overview] makale daha fazla ayrıntı için. |
| Sunucu asıl "KullanıcıAdım", geçerli güvenlik bağlamı altında "ana" veritabanına erişebilir değil. Kullanıcının varsayılan veritabanı açılamıyor. Oturum açma başarısız. Oturum açma 'KullanıcıAdım' kullanıcı için başarısız oldu. (Microsoft SQL Server, hata: 916) | Bir AAD kullanıcısı ana veritabanına bağlanmayı dener, ancak bir kullanıcının ana veritabanında yok. Bu hata oluşur.  Bu sorunu düzeltmek için ya da SQL veri ambarı bağlantı zamanında bağlanmak veya ana veritabanına kullanıcı eklemek istediğiniz belirtin.  Bkz: [güvenliğine genel bakış] [ Security overview] makale daha fazla ayrıntı için. |
| CTAIP hata                                                  | SQL server ana veritabanı üzerinde ancak SQL veri ambarı veritabanındaki bir oturum açma oluşturulduğunda bu hata oluşabilir.  Bu hatayla karşılaşırsanız, göz atın [güvenliğine genel bakış] [ Security overview] makalesi.  Bu makalede, asıl oturum açma ve kullanıcı oluşturma ve SQL veri ambarı veritabanında bir kullanıcı oluşturma işlemini açıklar. |
| Güvenlik Duvarı tarafından engellendi                                          | Azure SQL veritabanları, bir veritabanına erişimi olan IP adresleri yalnızca bilinen emin olmak için sunucu ve veritabanı düzeyinde güvenlik duvarları tarafından korunur. Bağlanabilmesi için önce güvenlik duvarları, açıkça etkinleştirmelisiniz yani varsayılan ve IP adresi veya adres aralığını güvenlidir.  Erişim için güvenlik duvarını yapılandırmanız için adımları izleyin. [sunucu Güvenlik Duvarı erişimi yapılandırmak için istemci IP] [ Configure server firewall access for your client IP] içinde [yönergeleri sağlama] [Provisioning instructions]. |
| Aracı veya sürücü ile bağlantı kurulamıyor                           | SQL veri ambarı kullanılmasını önerir [SSMS][SSMS], [Visual Studio için SSDT][SSDT for Visual Studio], veya [sqlcmd] [ sqlcmd] , verileri sorgulamak için. Sürücüleri ve SQL veri ambarı'na bağlanma hakkında daha fazla bilgi için bkz. [Azure SQL veri ambarı için sürücüleri] [ Drivers for Azure SQL Data Warehouse] ve [Azure SQL Data Warehouse'a bağlanma] [ Connect to Azure SQL Data Warehouse] makaleler. |

## <a name="tools"></a>Araçlar
| Sorun                                                        | Çözüm                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| AAD kullanıcıları Visual Studio nesne Gezgini'nde eksik           | Bu bilinen bir sorundur.  Geçici çözüm olarak, kullanıcılar görüntülemek [sys.database_principals][sys.database_principals].  Bkz: [kimlik doğrulaması için Azure SQL veri ambarı] [ Authentication to Azure SQL Data Warehouse] SQL veri ambarı ile Azure Active Directory kullanma hakkında daha fazla bilgi edinmek için. |
| Yanıt vermiyor veya hatalar üretme komut dosyası, komut dosyası Sihirbazı'nı kullanarak veya SSMS ile bağlanma el ile de yavaş | Ana veritabanında kullanıcılar oluşturduğunuzdan emin olun. Betik oluşturma seçenekleri, ayrıca altyapı sürümü "Microsoft Azure SQL veri ambarı sürümü olarak" olarak ayarlanır ve "Microsoft Azure SQL veritabanı" altyapısı türüdür emin olun. |
| SSMS'de betikleri başarısız oluştur                             | ' % S'seçeneği "Bağımlı nesneler için betik oluştur" seçeneği "True" olarak ayarlanmışsa SQL veri ambarı için bir komut dosyası oluşturma başarısız Geçici çözüm olarak, kullanıcıların el ile Araçlar gerekir -> Seçenekler -> SQL Server Nesne Gezgini bağımlı seçenekleri için betik Oluştur -> ve false olarak ayarlayın |

## <a name="performance"></a>Performans
| Sorun                                                        | Çözüm                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Sorgu performansı sorunlarını giderme                            | Belirli bir sorgu sorunlarını giderme çalışıyorsanız başlayın [sorgularınızı izlemek öğrenme][Learning how to monitor your queries]. |
| Zayıf sorgu performansı ve planları genellikle bir sonucudur eksik istatistikleri | En yaygın kötü performans tablolarınızı istatistiklerle eksikliği nedenidir.  Bkz: [tablo istatistikleri koruma] [ Statistics] istatistikleri oluşturmak nasıl ve neden, performansı için kritik olan hakkında ayrıntılı bilgi için. |
| Düşük eşzamanlılık / sorgularının kuyrukta                             | Anlama [iş yükü yönetimi] [ Workload management] eşzamanlılık ile bellek ayırma dengelemeye yönelik anlamak için önemlidir. |
| En iyi uygulama                              | Sorgu performansını artırmanın yollarını öğrenmek başlatmak için en iyi yerdir [SQL veri ambarı en iyi] [ SQL Data Warehouse best practices] makalesi. |
| Ölçeklendirme performansı nasıl                      | Bazen performansını iyileştirme için daha fazla işlem gücü sorgularınıza tarafından eklemeniz yeterlidir çözümdür [SQL veri ambarınızın ölçeklendirme][Scaling your SQL Data Warehouse]. |
| Zayıf dizin kalitesini kötü sorgu performansı     | Bazı kez sorgular yavaşlamasına nedeniyle [zayıf bir columnstore dizini kalite][Poor columnstore index quality].  Daha fazla bilgi için bu makaleye bakın ve nasıl [segment kalitesini artırmak için dizinleri yeniden][Rebuild indexes to improve segment quality]. |

## <a name="system-management"></a>Sistem Yönetimi
| Sorun                                                        | Çözüm                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Msg 40847: Sunucu 45000 izin verilen veritabanı işlem birimi kotasını aşacağından işlem gerçekleştirilemedi. | Ya da [DWU] [ DWU] oluşturmaya çalıştığınız veritabanının veya [bir kota artırım talebinde][request a quota increase]. |
| Alan kullanımının araştırılması                              | Bkz: [tablo boyutları] [ Table sizes] sisteminizi alanı kullanımını anlamak için. |
| Tabloları yönetmek                                    | Bkz: [tabloya genel bakış] [ Overview] makale tablolarınızı yönetme konusunda Yardım.  Bu makalede gibi daha ayrıntılı konuların bağlantıları da içerir [tablo veri türleri][Data types], [tablo dağıtma][Distribute], [Tablo dizin][Index], [bir tablo bölümleme][Partition], [tablo istatistikleri koruma] [ Statistics] ve [geçici tablolar][Temporary]. |
| Saydam veri şifrelemesi (TDE) ilerleme çubuğu, Azure Portalı'nda güncelleştirilmiyor | TDE durumunu görüntüleyebileceğiniz [powershell](/powershell/module/az.sql/get-azsqldatabasetransparentdataencryption). |

## <a name="polybase"></a>Polybase
| Sorun                                           | Çözüm                                                   |
| :---------------------------------------------- | :----------------------------------------------------------- |
| Büyük satır nedeniyle yükleme başarısız                | Şu anda çok satır desteği için Polybase mevcut değildir.  Başka bir deyişle, tablo, VARCHAR(MAX), NVARCHAR(MAX) veya VARBINARY(MAX) içeriyorsa, verilerinizi yüklemek için dış tablolar kullanılamaz.  Büyük satırlar yüklenirken şu anda yalnızca (BCP ile) Azure Data Factory, Azure Stream Analytics, SSIS, BCP veya .NET SQLBulkCopy sınıfı desteklenir. Büyük satır için PolyBase destek gelecek sürümlerin birinde eklenecektir. |
| BCP yük tablonun en fazla veri türü ile başarısız oluyor | VARCHAR(MAX), NVARCHAR(MAX) veya VARBINARY(MAX) Bazı senaryolarda tablonun sonuna yerleştirilmesini gerektirir, bilinen bir sorun yoktur.  En fazla sütun tablonun sonuna taşımayı deneyin. |

## <a name="differences-from-sql-database"></a>SQL veritabanı arasındaki farklar
| Sorun                                 | Çözüm                                                   |
| :------------------------------------ | :----------------------------------------------------------- |
| Desteklenmeyen SQL veritabanı özellikleri     | Bkz: [desteklenmeyen Tablo Özellikler][Unsupported table features]. |
| Desteklenmeyen SQL veritabanı veri türleri   | Bkz: [desteklenmeyen veri türleri][Unsupported data types].        |
| SİLME ve güncelleştirme sınırlamaları         | Bkz: [güncelleştirme geçici çözümler][UPDATE workarounds], [silme geçici çözümler] [ DELETE workarounds] ve [geçici olarak çözmek için CTAS kullanarak güncelleştirme desteklenmeyen ve DELETE söz dizimi][Using CTAS to work around unsupported UPDATE and DELETE syntax]. |
| MERGE deyimi desteklenmiyor      | Bkz: [birleştirme geçici çözümler][MERGE workarounds].                  |
| Saklı yordam sınırlamaları          | Bkz: [depolanan yordam sınırlamaları] [ Stored procedure limitations] bazı sınırlamaları saklı yordamların anlamak için. |
| UDF SELECT deyimleri desteklemiyor | Bu, bizim UDF'ler, geçerli bir sınırlamadır.  Bkz: [CREATE FUNCTION] [ CREATE FUNCTION] söz dizimi destekliyoruz. |

## <a name="next-steps"></a>Sonraki adımlar
Sorununuz için çözüm bulma daha fazla yardım için deneyebileceğiniz bazı kaynaklar aşağıda verilmiştir.

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
[Distribute]: sql-data-warehouse-tables-distribute.md
[Index]: sql-data-warehouse-tables-index.md
[Partition]: sql-data-warehouse-tables-partition.md
[Statistics]: sql-data-warehouse-tables-statistics.md
[Temporary]: sql-data-warehouse-tables-temporary.md
[Poor columnstore index quality]: sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality
[Rebuild indexes to improve segment quality]: sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Workload management]: resource-classes-for-workload-management.md
[Using CTAS to work around unsupported UPDATE and DELETE syntax]: sql-data-warehouse-develop-ctas.md
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
[Stack Overflow forumu]: https://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videolar]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
