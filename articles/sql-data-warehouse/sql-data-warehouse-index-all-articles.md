---
title: "SQL veri ambarı hizmeti için tüm konuları | Microsoft Docs"
description: "Azure hizmet için tüm konuları tablosunun http://azure.microsoft.com/documentation/articles/, başlığı ve açıklamayı mevcut bir SQL Data Warehouse adlı."
services: sql-data-warehouse
documentationcenter: 
author: barbkess
manager: jhubbard
editor: 
ms.assetid: a26a6dec-9c08-4415-8f58-4ee1dd41f718
ms.service: sql-data-warehouse
ms.workload: sql-data-warehouse
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: reference
ms.date: 03/30/2017
ms.author: barbkess
ms.openlocfilehash: 9fe41f12960dc099700e01573b4f03ebf63f8749
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="all-topics-for-azure-sql-data-warehouse-service"></a>Azure SQL Data Warehouse hizmeti için tüm konuları
Bu konuda doğrudan uygulayan her konuda listelenmiştir **SQL Data Warehouse** Azure hizmet. Anahtar sözcükler için bu Web sayfasını kullanarak arayabilirsiniz **Ctrl + F**, geçerli ilgi konuları bulmak için.

## <a name="new"></a>Yeni
| &nbsp; | Başlık | Açıklama |
| ---:|:--- |:--- |
| 1 |[SQL veri ambarı yedekleri](sql-data-warehouse-backups.md) |Bir geri yükleme noktası veya farklı bir coğrafi bölge için bir Azure SQL Data Warehouse geri yüklemek etkinleştirmeniz SQL Data Warehouse yerleşik veritabanı yedeklemeleri hakkında bilgi edinin. |

## <a name="updated-articles-sql-data-warehouse"></a>Güncelleştirilmiş makaleler, SQL veri ambarı
Bu bölümde, kısa bir süre önce güncelleştirildi makaleleri listeler büyük ya da önemli güncelleştirme olduğu. Güncelleştirilmiş her makale için eklenen markdown metin kaba parçacığıyla görüntülenir. Makaleleri tarih aralığı içinde güncelleştirildi **2016-08-22** için **2016-10-05**.

| &nbsp; | Makale | Güncelleştirilen metin, kod parçacığında | Ne zaman güncelleştirildi |
| ---:|:--- |:--- |:--- |
| 2 |[Azure blob depolama alanından SQL Data Warehouse'a (PolyBase) veri yükleme](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) |/-Bayt ve dosyaları SELECT r.command, s.request_id, r.status, sayım (ayrı input_name) nbr_files, olarak izlemek için (s.bytes_processed) / 1024/1024 sys.dm_pdw_exec_requests r INNER JOIN sys.dm_pdw_dms_external_work s r.request_id üzerinde gb_processed olarak sum s.request_id = WHERE r. Etiket = ' CTAS: yük cso. DimProduct ' veya r. Etiket = ' CTAS: yük cso. FactOnlineSales'GROUP BY r.command, s.request_id, r.status ORDER BY nbr_files desc, gb_processed desc; |2016-09-07 |
| 3 |[SQL veri ambarı geri yükleme](sql-data-warehouse-restore-database-overview.md) |** Duraklatılmış veri ambarını geri? ** duraklatılmış bir veri ambarı geri yüklemek için öncelikle tekrar çevrimiçi duruma getirmek gerekir. Veri ambarı yeniden çevrimiçi olduktan sonra geri yükleme noktaları aralarından seçim yapabileceğiniz yedi gün sahip. ** Geri coğrafi olarak yedekli depolama kullanıyorsanız coğrafi olarak yedekli bölge ** için veri ambarı eşleştirilmiş veri merkezinizde farklı coğrafi bölge için geri yükleyebilirsiniz. Veri ambarı son günlük yedekten geri yüklendi. ** Geri zaman çizelgesi ** herhangi bir geri yükleme noktası bir veritabanı son yedi gün içinde geri yükleyebilirsiniz. Anlık görüntüler için dört sekiz saatte başlatmak ve yedi gün için kullanılabilir. Bir anlık görüntü yedi günden daha eski olduğunda süresi dolmadan ve geri yükleme noktası artık kullanılamıyor. ** Geri maliyetleri ** Azure Premium Storage hızında geri yüklenen veri ambarı için depolama ücret faturalandırılır. Geri yüklenen veri ambarı duraklatmak, depolama Azure Premium Storage oranı için ücretlendirilirsiniz. Duraklatma avantajı, ücret olmayan olmasıdır. |2016-09-29 |

## <a name="get-started"></a>başlarken
| &nbsp; | Başlık | Açıklama |
| ---:|:--- |:--- |
| 4 |[Azure SQL Veri Ambarı’nda kimlik doğrulama](sql-data-warehouse-authentication.md) |Azure Active Directory (AAD) ve SQL Server kimlik doğrulaması Azure SQL veri ambarı. |
| 5 |[Azure SQL Veri Ambarı için en iyi yöntemler](sql-data-warehouse-best-practices.md) |Azure SQL Veri Ambarı için çözüm geliştirirken bilmeniz gerekenlerle ilgili öneriler ve en iyi yöntemler. Bu veriler, başarılı olmanıza yardımcı olacaktır. |
| 6 |[Azure SQL Data Warehouse için sürücüler](sql-data-warehouse-connection-strings.md) |Bağlantı dizeleri ve SQL Data Warehouse için sürücüleri |
| 7 |[Azure SQL Data Warehouse'a bağlanma](sql-data-warehouse-connect-overview.md) |Azure SQL Veri Ambarı için sunucu adı ve bağlantı dizesini bulma |
| 8 |[Azure Machine Learning ile veri çözümleme](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md) |Azure SQL Data Warehouse’a depolanmış verilere göre tahmine dayalı bir machine learning modeli oluşturmak için Azure Machine Learning’i kullanın. |
| 9 |[Sorgu Azure SQL Data Warehouse (sqlcmd)](sql-data-warehouse-get-started-connect-sqlcmd.md) |Azure SQL Data Warehouse’u sqlcmd Komut Satırı Yardımcı Programı ile sorgulama. |
| 10 |[Transact-SQL (TSQL) kullanarak SQL Data Warehouse veritabanı oluşturma](sql-data-warehouse-get-started-create-database-tsql.md) |TSQL ile Azure SQL Data Warehouse oluşturmayı öğrenin |
| 11 |[SQL Data Warehouse için destek bileti oluşturma](sql-data-warehouse-get-started-create-support-ticket.md) |Azure SQL Data Warehouse'da destek bileti oluşturma |
| 12 |[Azure Data Factory ile veri yükleme](sql-data-warehouse-get-started-load-with-azure-data-factory.md) |Azure Data Factory ile veri yüklemeyi öğrenin |
| 13 |[SQL Data warehouse'da PolyBase ile veri yükleme](sql-data-warehouse-get-started-load-with-polybase.md) |PolyBase'in ne olduğunu ve veri depolama senaryolarında nasıl kullanılacağını öğrenin. |
| 14 |[Bir Azure SQL Data Warehouse oluşturma](sql-data-warehouse-get-started-provision.md) |Azure portalında Azure SQL Veri Ambarı oluşturmayı öğrenin |
| 15 |[PowerShell kullanarak SQL Data Warehouse oluşturma](sql-data-warehouse-get-started-provision-powershell.md) |PowerShell kullanarak SQL Data Warehouse oluşturma |
| 16 |[Power BI ile verileri görselleştirme](sql-data-warehouse-get-started-visualize-with-power-bi.md) |Power BI ile SQL Data Warehouse verilerini görselleştirme |
| 17 |[Azure SQL Veri Ambarı’nı (Visual Studio) Sorgulama](sql-data-warehouse-query-visual-studio.md) |Visual Studio ile SQL Data Warehouse'u sorgulayın. |

## <a name="develop"></a>Geliştirme
| &nbsp; | Başlık | Açıklama |
| ---:|:--- |:--- |
| 18 |[İşlemleri SQL Data Warehouse için en iyi duruma getirme](sql-data-warehouse-develop-best-practices-transactions.md) |Azure SQL Data Warehouse'da verimli işlem güncelleştirmeleri yazma en iyi uygulama kılavuzunu |
| 19 |[SQL veri ambarı eşzamanlılık ve iş yükü yönetimi](sql-data-warehouse-develop-concurrency.md) |Çözümleri geliştirme için Azure SQL Data Warehouse eşzamanlılık ve iş yükü yönetimi anlayın. |
| 20 |[SQL veri ambarı'nda create Table As Select (CTAS)](sql-data-warehouse-develop-ctas.md) |Çözümleri geliştirme için Azure SQL Data Warehouse select (CTAS) deyiminde olarak oluşturma tabloyla kodlamak için ipuçları. |
| 21 |[SQL veri ambarı dinamik SQL](sql-data-warehouse-develop-dynamic-sql.md) |Çözümleri geliştirme için Azure SQL Data Warehouse'da dinamik SQL kullanma ipuçları. |
| 22 |[SQL veri ambarı seçeneklerinde göre gruplandırmak](sql-data-warehouse-develop-group-by-options.md) |Çözümleri geliştirme için Azure SQL Data Warehouse seçeneklerinde tarafından Grup uygulamak için ipuçları. |
| 23 |[SQL veri ambarı'nda gereç sorgular için etiketleri kullanma](sql-data-warehouse-develop-label.md) |Çözümleri geliştirme için Azure SQL Data Warehouse'da gereç sorgulara etiketleri kullanma ipuçları. |
| 24 |[SQL Data warehouse'da döngüler](sql-data-warehouse-develop-loops.md) |Transact-SQL döngüler ve çözümleri geliştirmek için Azure SQL Data Warehouse değiştirerek imleçler için ipuçları. |
| 25 |[Saklı yordamlar SQL veri ambarı](sql-data-warehouse-develop-stored-procedures.md) |Saklı yordamlar çözümleri geliştirmek için Azure SQL Data Warehouse'da uygulamak için ipuçları. |
| 26 |[SQL veri ambarı işlemleri](sql-data-warehouse-develop-transactions.md) |Çözümleri geliştirme için Azure SQL Data Warehouse'da işlemleri uygulamak için ipuçları. |
| 27 |[Kullanıcı tanımlı şemaları SQL veri ambarı](sql-data-warehouse-develop-user-defined-schemas.md) |Çözümleri geliştirme için Azure SQL Data Warehouse'da Transact-SQL şemalarını kullanma ipuçları. |
| 28 |[SQL veri ambarı değişkenlerde atayın](sql-data-warehouse-develop-variable-assignment.md) |Çözümleri geliştirme için Azure SQL Data Warehouse Transact-SQL değişkenleri atamak için ipuçları. |
| 29 |[SQL veri ambarı görünümlerde](sql-data-warehouse-develop-views.md) |Çözümleri geliştirme için Azure SQL Data Warehouse'da Transact-SQL görünümlerini kullanma ipuçları. |
| 30 |[SQL Data Warehouse için tasarım kararları ve kodlama teknikleri](sql-data-warehouse-overview-develop.md) |Geliştirme kavramları, tasarım kararları, öneriler ve SQL Data Warehouse için kodlama teknikleri. |

## <a name="manage"></a>Yönet
| &nbsp; | Başlık | Açıklama |
| ---:|:--- |:--- |
| 31 |[Azure SQL Data warehouse'da (genel bakış) işlem güç yönetimi](sql-data-warehouse-manage-compute-overview.md) |Azure SQL veri ambarı özellikleri genişletme performans. Dwu ayarlayarak ölçeğini veya duraklatıp işlem kaynakları maliyet tasarrufu sağlamak. |
| 32 |[Azure SQL Data warehouse'da (Azure portalı) işlem güç yönetimi](sql-data-warehouse-manage-compute-portal.md) |İşlem gücüne yönetmek için azure portal görevleri. Dwu ayarlayarak işlem kaynaklarını ölçeklendirme. Veya, duraklatma ve sürdürme işlem kaynaklarını maliyet tasarrufu sağlamak. |
| 33 |[Azure SQL Data warehouse'da (PowerShell) işlem güç yönetimi](sql-data-warehouse-manage-compute-powershell.md) |İşlem gücüne yönetmek için PowerShell görevleri. Dwu ayarlayarak işlem kaynaklarını ölçeklendirme. Veya, duraklatma ve sürdürme işlem kaynaklarını maliyet tasarrufu sağlamak. |
| 34 |[İşlem güç Azure SQL veri ambarı (REST) yönetme](sql-data-warehouse-manage-compute-rest-api.md) |İşlem gücüne yönetmek için PowerShell görevleri. Dwu ayarlayarak işlem kaynaklarını ölçeklendirme. Veya, duraklatma ve sürdürme işlem kaynaklarını maliyet tasarrufu sağlamak. |
| 35 |[Azure SQL Data warehouse'da (T-SQL) işlem güç yönetimi](sql-data-warehouse-manage-compute-tsql.md) |Dwu ayarlayarak genişleme performans görevlere Transact-SQL (T-SQL). Yoğun olmayan saatlerde ölçeklendirme tarafından maliyet tasarrufu. |
| 36 |[DMV’leri kullanarak iş yükünüzü izleme](sql-data-warehouse-manage-monitor.md) |Dmv'leri kullanarak, iş yükünü izlemek öğrenin. |
| 37 |[Azure SQL Data Warehouse veritabanlarında yönetme](sql-data-warehouse-overview-manage.md) |SQL veri ambarı veritabanlarını yönetmeye genel bakış. Yönetim Araçları, Dwu ve sorgu performansı, iyi bir güvenlik ilkeleri oluşturma ve veri bozulması veya bölgesel bir kesintinin bir veritabanını geri yükleme sorunlarını giderme genişleme performans içerir. |
| 38 |[Azure SQL veri ambarı İzleyicisi kullanıcı sorguları](sql-data-warehouse-overview-manage-user-queries.md) |Dikkat edilecek noktalar, en iyi yöntemler ve Azure SQL Data Warehouse kullanıcı sorgularda izleme görevleri genel bakış |
| 39 |[SQL veri ambarı geri yükleme](sql-data-warehouse-restore-database-overview.md) |Azure SQL Data Warehouse bir veritabanına kurtarmak için veritabanı geri yükleme seçeneklerine genel bakış. |
| 40 |[Bir Azure SQL veri ambarı (Portal) geri yükleme](sql-data-warehouse-restore-database-portal.md) |Azure SQL Data Warehouse geri yüklemek için azure portal görevler. |
| 41 |[Bir Azure SQL veri ambarı (PowerShell) geri yükleme](sql-data-warehouse-restore-database-powershell.md) |Azure SQL Data Warehouse geri yüklemek için PowerShell görevler. |
| 42 |[Bir Azure SQL veri ambarı (REST API'si) geri yükleme](sql-data-warehouse-restore-database-rest-api.md) |Azure SQL Data Warehouse geri yüklemek için REST API görevler. |

## <a name="tables-and-indexes"></a>Tablolar ve dizinler
| &nbsp; | Başlık | Açıklama |
| ---:|:--- |:--- |
| 43 |[SQL veri ambarı tablolarda için veri türleri](sql-data-warehouse-tables-data-types.md) |Azure SQL Data Warehouse tablolar için veri türleri ile çalışmaya başlama. |
| 44 |[SQL veri ambarı tablolarda dağıtma](sql-data-warehouse-tables-distribute.md) |Azure SQL Data Warehouse tablolarda dağıtma ile çalışmaya başlama. |
| 45 |[SQL veri ambarı tablolarda dizin oluşturma](sql-data-warehouse-tables-index.md) |Azure SQL Data Warehouse'da dizin tablo ile çalışmaya başlama. |
| 46 |[SQL veri ambarı tablolarda genel bakış](sql-data-warehouse-tables-overview.md) |Azure SQL veri ambarı tabloları ile çalışmaya başlama. |
| 47 |[SQL veri ambarı tablolarda bölümlendirme](sql-data-warehouse-tables-partition.md) |Azure SQL Data Warehouse'da tablo bölümleme ile çalışmaya başlama. |
| 48 |[SQL veri ambarı tablolarda istatistiklerle yönetme](sql-data-warehouse-tables-statistics.md) |Azure SQL Data Warehouse tablolarda istatistiklerle ile çalışmaya başlama. |
| 49 |[SQL veri ambarı geçici tabloları](sql-data-warehouse-tables-temporary.md) |Geçici tablolarda Azure SQL Data Warehouse ile çalışmaya başlama. |

## <a name="integrate"></a>Tümleştirme
| &nbsp; | Başlık | Açıklama |
| ---:|:--- |:--- |
| 50 |[SQL Data Warehouse ile Azure Data Factory kullanın](sql-data-warehouse-integrate-azure-data-factory.md) |Çözümleri geliştirme için Azure SQL Data Warehouse ile Azure Data Factory (ADF) kullanma ipuçları. |
| 51 |[SQL Data Warehouse ile Azure Machine Learning kullanın](sql-data-warehouse-integrate-azure-machine-learning.md) |Çözüm geliştirmek üzere Azure SQL Data Warehouse ile Azure Machine Learning kullanma öğreticisi. |
| 52 |[SQL Data Warehouse ile Azure akış analizi kullanın](sql-data-warehouse-integrate-azure-stream-analytics.md) |Çözümleri geliştirme için Azure SQL Data Warehouse ile Azure akış analizi kullanımı hakkında ipuçları. |
| 53 |[Power BI ile SQL veri ambarı kullanın](sql-data-warehouse-integrate-power-bi.md) |Power BI çözümleri geliştirmek için Azure SQL Data Warehouse ile kullanma ipuçları. |
| 54 |[SQL veri ambarı diğer hizmetlerle yararlanın](sql-data-warehouse-overview-integrate.md) |Araçlar ve iş ortakları ile SQL Data Warehouse ile tümleştirme çözümleri. |

## <a name="load"></a>Yükleme
| &nbsp; | Başlık | Açıklama |
| ---:|:--- |:--- |
| 55 |[Azure blob depolama alanından Azure SQL Data Warehouse'a (Azure Data Factory) veri yükleme](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md) |Azure Data Factory ile veri yüklemeyi öğrenin |
| 56 |[Azure blob depolama alanından SQL Data Warehouse'a (PolyBase) veri yükleme](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) |PolyBase verileri Azure blob depolama alanından SQL Data Warehouse'a veri yüklemek için nasıl kullanılacağını öğrenin. Birkaç tablolar Contoso perakende veri ambarı şemasına genel verileri yüklenemiyor. |
| 57 |[SQL Server'dan Azure SQL Data Warehouse'a (AZCopy) veri yükleme](sql-data-warehouse-load-from-sql-server-with-azcopy.md) |SQL Server'dan düz dosyalara veri aktarmak için bcp'yi, Azure blob depolama alanına veri aktarmak için AZCopy'yi ve verileri Azure SQL Data Warehouse'a almak için PolyBase'i kullanın. |
| 58 |[SQL Server'dan Azure SQL Data Warehouse'a (düz dosyalar) veri yükleme](sql-data-warehouse-load-from-sql-server-with-bcp.md) |Küçük boyutlu veriler söz konusu olduğunda SQL Server'dan düz dosyalara veri aktarmak ve verileri doğrudan Azure SQL Data Warehouse'a aktarmak için bcp yardımcı programını kullanın. |
| 59 |[SQL Server'dan Azure SQL veri ambarı (SSIS) içine veri yükleme](sql-data-warehouse-load-from-sql-server-with-integration-services.md) |SQL veri ambarı için çok çeşitli veri kaynakları verileri taşımak için bir SQL Server Integration Services (SSIS) paketi oluşturmak gösterilmiştir. |
| 60 |[SQL Data warehouse'da PolyBase ile veri yükleme](sql-data-warehouse-load-from-sql-server-with-polybase.md) |SQL Server'dan düz dosyalara veri aktarmak için bcp'yi, Azure blob depolama alanına veri aktarmak için AZCopy'yi ve verileri Azure SQL Data Warehouse'a almak için PolyBase'i kullanın. |
| 61 |[SQL Data Warehouse'da PolyBase kullanarak Kılavuzu](sql-data-warehouse-load-polybase-guide.md) |Kılavuzları ve PolyBase kullanarak SQL Data Warehouse senaryolarda ilgili öneriler. |
| 62 |[SQL Data Warehouse'a örnek veri yükleme](sql-data-warehouse-load-sample-databases.md) |SQL Data Warehouse'a örnek veri yükleme |
| 63 |[Bcp ile veri yükleme](sql-data-warehouse-load-with-bcp.md) |bcp'nin ne olduğunu ve veri depolama senaryolarında nasıl kullanılacağını öğrenin. |
| 64 |[Azure SQL Veri Ambarı’na veri yükleme](sql-data-warehouse-overview-load.md) |Veri SQL Data Warehouse'a veri yükleme için genel senaryolar hakkında bilgi edinin. Bunlar PolyBase, Azure blob depolama, düz dosyalar ve disk sevkiyat kullanarak içerir. Üçüncü taraf araçları da kullanabilirsiniz. |

## <a name="migrate"></a>Geçiş
| &nbsp; | Başlık | Açıklama |
| ---:|:--- |:--- |
| 65 |[SQL kodunuzu SQL veri ambarına geçirme](sql-data-warehouse-migrate-code.md) |Çözümleri geliştirme için Azure SQL Data Warehouse SQL kodunuzu geçirmek için ipuçları. |
| 66 |[Verilerinizi geçirme](sql-data-warehouse-migrate-data.md) |Çözümleri geliştirme için Azure SQL Data Warehouse verilerinizi geçirme ipuçları. |
| 67 |[Veri ambarı geçiş yardımcı programı (Önizleme)](sql-data-warehouse-migrate-migration-utility.md) |SQL veri ambarı'na geçirin. |
| 68 |[SQL veri ambarına şemanızı geçirme](sql-data-warehouse-migrate-schema.md) |Çözümleri geliştirme için Azure SQL veri ambarı'na şemanızı geçirmek için ipuçları. |
| 69 |[Çözümünüzü SQL Veri Ambarı’na taşıyın](sql-data-warehouse-overview-migrate.md) |Azure SQL Data Warehouse platformuna çözümünüzü getiren Geçiş Kılavuzu. |

## <a name="partners"></a>İş Ortakları
| &nbsp; | Başlık | Açıklama |
| ---:|:--- |:--- |
| 70 |[SQL veri ambarı iş zekası ortakları](sql-data-warehouse-partner-business-intelligence.md) |SQL veri ambarı destek çözümleri üçüncü taraf iş zekası ortaklarıyla listeler. |
| 71 |[SQL veri ambarı veri tümleştirme ortakları](sql-data-warehouse-partner-data-integration.md) |Azure SQL Data Warehouse destekleyen veri tümleştirme çözümleri üçüncü taraf iş ortaklarıyla listeler. |
| 72 |[SQL veri ambarı veri yönetimi ortakları](sql-data-warehouse-partner-data-management.md) |Üçüncü taraf veri yönetimi SQL veri ambarı destek çözümleri iş ortaklarıyla listeler. |

## <a name="reference"></a>Başvuru
| &nbsp; | Başlık | Açıklama |
| ---:|:--- |:--- |
| 73 |[SQL veri ambarı için başvuru konuları](sql-data-warehouse-overview-reference.md) |SQL veri ambarı için referans içerik bağlantıları. |
| 74 |[PowerShell cmdlet'leri ve SQL Data Warehouse için REST API'leri](sql-data-warehouse-reference-powershell-cmdlets.md) |Üst PowerShell cmdlet'leri için Azure SQL veri duraklatma ve sürdürme bir veritabanı da dahil olmak üzere ambarı bulun. |
| 75 |[Dil öğeleri](sql-data-warehouse-reference-tsql-language-elements.md) |İçin referans içerik bağlantıları için SQL Data Warehouse için kullanılan Transact-SQL dil öğeleri listesi. |
| 76 |[Transact-SQL konuları](sql-data-warehouse-reference-tsql-statements.md) |SQL veri ambarı tarafından kullanılan Transact-SQL konular için referans içerik bağlantıları. |
| 77 |[Sistem görünümleri](sql-data-warehouse-reference-tsql-system-views.md) |Sistem bağlantılar SQL Data Warehouse için içeriği görüntüler. |

## <a name="security"></a>Güvenlik
| &nbsp; | Başlık | Açıklama |
| ---:|:--- |:--- |
| 78 |[SQL Data Warehouse - denetime ve dinamik veri maskeleme için alt düzey istemci desteği](sql-data-warehouse-auditing-downlevel-clients.md) |Verileri denetlemek için SQL Data Warehouse alt düzey istemci desteği hakkında bilgi edinin |
| 79 |[Azure SQL veri ambarı'nda denetleme](sql-data-warehouse-auditing-overview.md) |Azure SQL Data Warehouse'da denetimi ile çalışmaya başlama |
| 80 |[İle saydam veri şifreleme (TDE) SQL veri ambarı kullanmaya başlama](sql-data-warehouse-encryption-tde.md) |SQL Data warehouse'da saydam veri şifreleme (TDE) |
| 81 |[Saydam veri şifreleme (TDE) ile çalışmaya başlama](sql-data-warehouse-encryption-tde-tsql.md) |Saydam veri şifreleme (TDE) SQL veri ambarı (T-SQL) |
| 82 |[SQL veri ambarı veritabanında güvenli](sql-data-warehouse-overview-manage-security.md) |Çözümleri geliştirme için Azure SQL veri ambarı veritabanında güvenliğini sağlamaya yönelik ipuçları. |

## <a name="miscellaneous"></a>Muhtelif Hükümler
| &nbsp; | Başlık | Açıklama |
| ---:|:--- |:--- |
| 83 |[SQL Data Warehouse için Visual Studio ve SSDT yükleme](sql-data-warehouse-install-visual-studio.md) |Azure SQL Data Warehouse için Visual Studio'yu ve SQL Server Veri Araçları'nı (SSDT) Yükleme |
| 84 |[Premium depolama ayrıntıları geçiş](sql-data-warehouse-migrate-to-premium-storage.md) |Mevcut bir SQL veri ambarı premium depolama alanına geçirmek için yönergeler |
| 85 |[Tehdit algılama ile çalışmaya başlama](sql-data-warehouse-security-threat-detection.md) |Tehdit algılama ile çalışmaya nasıl başlayacağınız |
| 86 |[SQL Data Warehouse kapasite sınırları](sql-data-warehouse-service-capacity-limits.md) |Bağlantılar, veritabanları, tablolar ve SQL Data Warehouse için sorguları için en yüksek değerleri. |
| 87 |[Azure SQL veri ambarı sorunlarını giderme](sql-data-warehouse-troubleshoot.md) |Azure SQL veri ambarı sorunlarını giderme. |

