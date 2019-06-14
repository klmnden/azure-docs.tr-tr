---
title: Azure SQL veritabanı için çoğaltma | "Microsoft Docs
description: Azure SQL veritabanı tek veritabanları ve elastik havuzlardaki veritabanları ile SQL Server çoğaltma kullanma hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: allenwux
ms.author: xiwu
ms.reviewer: mathoma
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: b9d6569504b5352c6187afe12d903c986019c517
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60646811"
---
# <a name="replication-to-sql-database-single-and-pooled-databases"></a>SQL veritabanı tek ve havuza alınmış veritabanlarını çoğaltma

SQL Server çoğaltma üzerinde tek ve havuza alınmış veritabanları için yapılandırılabilir bir [SQL veritabanı sunucusu](sql-database-servers.md) Azure SQL veritabanı'nda.  

## <a name="supported-configurations"></a>**Desteklenen yapılandırmalar:**
  
- SQL Server, SQL Server'ın şirket içi veya buluttaki bir Azure sanal makinesinde çalışan SQL Server örneğini çalıştıran bir örneği olabilir. Daha fazla bilgi için [SQL Server Azure sanal makinelerine genel bakış](https://azure.microsoft.com/documentation/articles/virtual-machines-sql-server-infrastructure-services/).  
- Azure SQL veritabanındaki bir SQL Server yayımcısı bir anında iletme abonesi olması gerekir.  
- Bir Azure SQL veritabanı'nda, dağıtım veritabanı ve çoğaltma aracıları yerleştirilemez.  
- Anlık görüntü ve tek yönlü işlem çoğaltma desteklenir. Eşler arası işlem çoğaltma ve birleştirme çoğaltması desteklenmez.
- Çoğaltma, Azure SQL veritabanı yönetilen örneği'te genel önizlemesi için kullanılabilir. Yönetilen örnek, yayımcı ve dağıtıcı abone veritabanlarını barındırabilir. Daha fazla bilgi için [SQL veritabanı yönetilen örneği ile çoğaltma](replication-with-sql-database-managed-instance.md).

## <a name="versions"></a>Sürümler  

- Yayımcı ve dağıtıcı en az aşağıdaki sürümlerinden biri olmalıdır:  
- SQL Server 2017 (14.x)
- SQL Server 2016 (13.x)
- SQL Server 2014'ü (12.x) SP1 CU3'ten
- SQL Server 2014'ü (12.x) RTM CU10
- SQL Server 2012 (11.x) SP2 CU8 veya SP3
- Eski bir sürümü kullanılarak çoğaltma yapılandırmaya çalışmadan sonuçlanabilir hata numarası (işlem abone. bağlanamadı) MSSQL_REPL20084 ve MSSQL_REPL40532 (sunucu açamıyor \<adı > oturum açma tarafından istenen. "Oturum açma başarısız.).  
- Azure SQL veritabanı'nın tüm özelliklerini kullanabilmek için en son sürümlerini kullanarak [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) ve [SQL Server veri Araçları](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt).  
  
## <a name="remarks"></a>Açıklamalar

- Çoğaltma kullanılarak yapılandırılabilir [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) veya yayımcı üzerinde Transact-SQL deyimleri çalıştırma. Azure portalını kullanarak çoğaltma yapılandırılamıyor.  
- Çoğaltma, yalnızca bir Azure SQL veritabanına bağlanmak için SQL Server kimlik doğrulaması oturum açma bilgileri kullanabilirsiniz.
- Çoğaltılmış tablolar, bir birincil anahtarı olmalıdır.  
- Mevcut bir Azure aboneliğine sahip olmalıdır.  
- Herhangi bir bölgede Azure SQL veritabanı abone olabilir.  
- Tek bir SQL Server yayınında, hem Azure SQL veritabanı ve SQL Server'ın (şirket içi ve Azure sanal makine'de SQL Server) aboneleri destekleyebilir.  
- Çoğaltma yönetim, izleme ve sorun giderme şirket içi SQL Server'dan gerçekleştirilmelidir.  
- Azure SQL veritabanında yalnızca iletme abonelikleri desteklenir.  
- Yalnızca `@subscriber_type = 0` desteklenir **sp_addsubscription** SQL veritabanı için.  
- Azure SQL veritabanı iki yönlü, anında, güncelleştirilebilir veya eşler arası çoğaltma desteklemez.

## <a name="replication-architecture"></a>Çoğaltma mimarisi  

![Çoğaltma için sql veritabanı](./media/replication-to-sql-database/replication-to-sql-database.png)  

## <a name="scenarios"></a>Senaryolar  

### <a name="typical-replication-scenario"></a>Normal çoğaltma senaryosu  

1. Bir şirket içi SQL Server veritabanı üzerinde işlemsel çoğaltma yayın oluşturun.  
2. Şirket içi SQL Server üzerinde kullanmak **Yeni Abonelik Sihirbazı'nı** veya Transact-SQL deyimlerini bir anında iletme aboneliğine Azure SQL veritabanı oluşturun.  
3. Azure SQL veritabanı'nda tek ve havuza alınmış veritabanları ile ilk veri kümesi anlık görüntü aracısı tarafından oluşturulan ve dağıtılan ve Dağıtım Aracısı tarafından uygulanan bir anlık görüntüdür. Yönetilen örnek veritabanı ile bir veritabanı yedeği abone veritabanının çekirdeğini oluşturma için de kullanabilirsiniz.

### <a name="data-migration-scenario"></a>Veri geçiş senaryosu  

1. Azure SQL veritabanı'na bir şirket içi SQL Server veritabanından veri çoğaltmak için işlem çoğaltma kullanma.  
2. Azure SQL veritabanı kopyasını güncelleştirmek için orta katman uygulamalar ve istemci yeniden yönlendirme.  
3. SQL Server sürümü güncelleştirme durdurun ve yayını kaldırın.  

## <a name="limitations"></a>Sınırlamalar

Aşağıdaki seçenekler, Azure SQL veritabanı abonelikler için desteklenmez:

- Dosya grupları ilişkilendirmesi kopyalayın  
- Bölümleme düzenleri tabloyu Kopyala  
- Dizin düzenleri bölümleme kopyalayın  
- Kullanıcı tanımlı istatistikleri kopyalayın  
- Kopya varsayılan bağlamaları  
- Kural bağlamaları kopyalayın  
- Tam metin dizinleri kopyalama  
- XML XSD kopyalayın  
- XML dizinleri kopyalama  
- İzinleri kopyalayın  
- Uzaysal dizinleri kopyalama  
- Filtrelenmiş dizinler kopyalayın  
- Veri sıkıştırma özniteliği kopyalayın  
- Seyrek sütun özniteliği kopyalayın  
- FILESTREAM en fazla veri türlerine dönüştürme  
- HierarchyId en fazla veri türlerine dönüştürme  
- En fazla veri türleri için uzamsal Dönüştür  
- Genişletilmiş özellikler kopyalayın  
- İzinleri kopyalayın  

### <a name="limitations-to-be-determined"></a>Belirlenecek sınırlamaları

- Harmanlama kopyalayın  
- SP seri hale getirilmiş bir işlemde yürütme  

## <a name="examples"></a>Örnekler

Bir yayın ve gönderme temelli bir abonelik oluşturun. Daha fazla bilgi için bkz.
  
- [Bir yayın oluşturun](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication)
- [Bir itme aboneliği oluşturmak](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription/) abonesi olarak Azure SQL veritabanı sunucu adını kullanarak (örneğin **N'azuresqldbdns.database.windows.net'** ) ve hedef veritabanı olarak (Azure SQL veritabanı adı örnek **AdventureWorks**).  

## <a name="see-also"></a>Ayrıca Bkz.  

- [İşlem çoğaltması](sql-database-managed-instance-transactional-replication.md)
- [Bir yayın oluşturun](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication)
- [Gönderme temelli bir abonelik oluşturun](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription/)
- [Çoğaltma türleri](https://docs.microsoft.com/sql/relational-databases/replication/types-of-replication)
- [İzleme (Çoğaltma)](https://docs.microsoft.com/sql/relational-databases/replication/monitor/monitoring-replication)
- [Bir abonelik başlatma](https://docs.microsoft.com/sql/relational-databases/replication/initialize-a-subscription)  
