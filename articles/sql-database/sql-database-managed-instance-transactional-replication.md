---
title: Azure SQL veritabanı işlem çoğaltması | "Microsoft Docs
description: Havuza alınmış, tek ve Azure SQL veritabanı'nda örnek veritabanları ile SQL Server işlem çoğaltma kullanma hakkında öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.reviewer: carlrab
manager: craigg
ms.date: 02/08/2019
ms.openlocfilehash: 1c62fb466774a3599972d6a9cc340cca300eee59
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67696199"
---
# <a name="transactional-replication-with-single-pooled-and-instance-databases-in-azure-sql-database"></a>İşlem çoğaltma, tek bir havuzda ve Azure SQL veritabanı'nda veritabanları örnek

İşlem çoğaltması, Azure SQL veritabanı ve SQL Server, Azure SQL veritabanındaki bir tablodan veri çoğaltmanıza olanak sağlar veya uzak veritabanlarında yerleştirilmiş tablolar için SQL Server özelliğidir. Bu özellik, birden fazla tablo farklı veritabanlarındaki eşitlemenize olanak tanır.

## <a name="when-to-use-transactional-replication"></a>Ne zaman işlem çoğaltma kullanma

İşlem çoğaltma, aşağıdaki senaryolarda kullanışlıdır:
- Bir veya daha fazla veritabanı tablolarında yapılan değişiklikleri Yayımla ve abone değişikliklerini bir veya daha çok SQL Server veya Azure SQL veritabanlarına dağıtın.
- Birden fazla dağıtılmış veritabanı eşitlenmiş durumda tutun.
- Veritabanlarını bir SQL Server veya yönetilen örneği için başka bir veritabanına değişiklikleri sürekli olarak yayımlayarak geçirme.

## <a name="overview"></a>Genel Bakış

İşlem çoğaltma anahtar bileşenler aşağıdaki resimde gösterilmektedir:  

![SQL veritabanı ile çoğaltma](media/replication-to-sql-database/replication-to-sql-database.png)

**Yayımcı** dağıtımcı güncelleştirmeleri göndererek örneği veya bazı tablolar (makaleler) yapılan değişiklikler yayımlayan sunucusunu olduğu. Yayımlama için herhangi bir Azure SQL veritabanı bir şirket içi SQL Server'dan SQL Server'ın aşağıdaki sürümleriyle desteklenir:

- SQL Server 2019 (Önizleme)
- SQL Server 2016 SQL 2017
- SQL Server 2014 SP1 CU3 veya büyük (12.00.4427)
- SQL Server 2014 RTM CU10 (12.00.2556)
- SQL Server 2012 SP3 veya büyük (11.0.6020)
- SQL Server 2012 SP2 CU8 (11.0.5634.0)
- Azure'da yayımlama nesnelere desteklemeyen başka SQL Server sürümleri için kullanmak mümkün [verileri yeniden yayımlayarak](https://docs.microsoft.com/sql/relational-databases/replication/republish-data) daha yeni SQL Server sürümleri için verileri taşımak için yöntemi. 

**Dağıtıcı** bir örneği ya da bir yayımcıdan değişiklikleri makalelerinde toplar ve bunları abonelere dağıtan bir sunucu. Dağıtımcısı, Azure SQL veritabanı yönetilen örneği veya SQL Server'ın (yayımcı sürümden daha yüksek veya ona eşit, uzun gibi herhangi bir sürüm) olabilir. 

**Abone** bir örneği veya yayımcı üzerinde yapılan değişiklikleri alan bir sunucu. Aboneleri veya havuza alınmış, tek ve Azure SQL veritabanı veya SQL Server veritabanları örneği olabilir. Abone tek veya havuza alınmış bir veritabanı üzerinde anında iletme abonesi olarak yapılandırılması gerekir. 

| Role | Tek ve havuza alınmış veritabanları | Örnek veritabanları |
| :----| :------------- | :--------------- |
| **Yayımcı** | Hayır | Evet | 
| **Dağıtıcı** | Hayır | Evet|
| **Abone çekme** | Hayır | Evet|
| **Anında iletme abone**| Evet | Evet|
| &nbsp; | &nbsp; | &nbsp; |

  >[!NOTE]
  > Dağıtıcı bir örnek veritabanı ve abone değil bir istek temelli aboneliğe desteklenmez. 

Vardır farklı [çoğaltma türleri](https://docs.microsoft.com/sql/relational-databases/replication/types-of-replication):


| Çoğaltma | Tek ve havuza alınmış veritabanları | Örnek veritabanları|
| :----| :------------- | :--------------- |
| [**Standart işlem**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/transactional-replication) | Evet (yalnızca abonesi olarak) | Evet | 
| [**Anlık görüntü**](https://docs.microsoft.com/sql/relational-databases/replication/snapshot-replication) | Evet (yalnızca abonesi olarak) | Evet|
| [**Çoğaltma birleştirme**](https://docs.microsoft.com/sql/relational-databases/replication/merge/merge-replication) | Hayır | Hayır|
| [**Eşler arası**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/peer-to-peer-transactional-replication) | Hayır | Hayır|
| [**Çift yönlü**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/bidirectional-transactional-replication) | Hayır | Evet|
| [**Güncelleştirilebilir abonelikler**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication) | Hayır | Hayır|
| &nbsp; | &nbsp; | &nbsp; |

  >[!NOTE]
  > - Eski bir sürümü kullanılarak çoğaltma yapılandırmaya çalışmadan sonuçlanabilir hata numarası (işlem abone. bağlanamadı) MSSQL_REPL20084 ve MSSQ_REPL40532 (sunucu açamıyor \<adı > oturum açma tarafından istenen. Oturum açma başarısız.)
  > - Azure SQL veritabanı'nın tüm özelliklerini kullanabilmek için en son sürümlerini kullanarak [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) ve [SQL Server veri Araçları (SSDT)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt).
  
  ### <a name="supportability-matrix-for-instance-databases-and-on-premises-systems"></a>Örnek veritabanları ve şirket içi sistemler için desteklenebilirlik Matrisi
  Çoğaltma desteklenebilirliği matris örneği için veritabanları aynı olup şirket içi SQL Server için. 
  
  | **Yayımcı**   | **Dağıtıcı** | **Abone** |
| :------------   | :-------------- | :------------- |
| SQL Server 2017 | SQL Server 2017 | SQL Server 2017 <br/> SQL Server 2016 <br/> SQL Server 2014 |
| SQL Server 2016 | SQL Server 2017 <br/> SQL Server 2016 | SQL Server 2017 <br/>SQL Server 2016 <br/> SQL Server 2014 <br/> SQL Server 2012 |
| SQL Server 2014 | SQL Server 2017 <br/> SQL Server 2016 <br/> SQL Server 2014 <br/>| SQL Server 2017 <br/> SQL Server 2016 <br/> SQL Server 2014 <br/> SQL Server 2012 <br/> SQL Server 2008 R2 <br/> SQL Server 2008 |
| SQL Server 2012 | SQL Server 2017 <br/> SQL Server 2016 <br/> SQL Server 2014 <br/>SQL Server 2012 <br/> | SQL Server 2016 <br/> SQL Server 2014 <br/> SQL Server 2012 <br/> SQL Server 2008 R2 <br/> SQL Server 2008 | 
| SQL Server 2008 R2 <br/> SQL Server 2008 | SQL Server 2017 <br/> SQL Server 2016 <br/> SQL Server 2014 <br/>SQL Server 2012 <br/> SQL Server 2008 R2 <br/> SQL Server 2008 | SQL Server 2014 <br/> SQL Server 2012 <br/> SQL Server 2008 R2 <br/> SQL Server 2008 <br/>  |
| &nbsp; | &nbsp; | &nbsp; |

## <a name="requirements"></a>Gereksinimler

- Bağlantı, çoğaltma katılımcıları arasında SQL Kimlik Doğrulaması kullanır. 
- Çoğaltma tarafından kullanılan çalışma dizini için bir Azure depolama hesabı paylaşımı. 
- Bağlantı noktası 445 (TCP Giden) Azure dosya paylaşımına erişmek için yönetilen örnek alt güvenlik kurallarında açık olması gerekir. 
- Bağlantı noktası 1433 (TCP Giden) yayımcı/dağıtıcı bağlantılarının yönetilen örneği'nde ve şirket içi abone ise açılması gerekir.

  >[!NOTE]
  > Bir Azure depolama dosyasına giden ağ güvenlik grubu (NSG) bağlantı noktası 445 dağıtıcı bir örnek veritabanı ve şirket içi abone olduğunda engellenirse bağlanırken hata 53 karşılaşabilirsiniz. [VNet NSG update](/azure/storage/files/storage-troubleshoot-windows-file-connection-problems) bu sorunu çözmek için. 

### <a name="compare-data-sync-with-transactional-replication"></a>İşlem çoğaltma ile veri eşitleme karşılaştırın

| | Data Sync | İşlem Çoğaltması |
|---|---|---|
| Yararları | -Etkin-etkin desteği<br/>İki yönlü şirket içi ve Azure SQL veritabanı arasında | -Daha düşük gecikme süresi<br/>-İşlem tutarlılığı<br/>-Mevcut topolojisi geçişten sonra yeniden kullanma |
| Dezavantajları | -5 dakika veya daha fazla gecikme süresi<br/>-İşlem tutarlılığı<br/>-Daha yüksek performans etkisi | -Azure SQL veritabanı tek veritabanı veya havuza veritabanı yayımlanamıyor<br/>-Yüksek bakım maliyeti |
| | | |

## <a name="common-configurations"></a>Ortak yapılandırmaları

Genel olarak, yayımcı ve dağıtıcı ya da bulutta veya şirket içinde olması gerekir. Aşağıdaki yapılandırmalar desteklenir: 

### <a name="publisher-with-local-distributor-on-a-managed-instance"></a>Yayımcı ile yönetilen örneğinde yerel dağıtıcı

![Yayımcı ve dağıtıcı olarak tek örnek](media/replication-with-sql-database-managed-instance/01-single-instance-asdbmi-pubdist.png)

Yayımcı ve dağıtıcı tek bir yönetilen örnek ve diğer yönetilen örneği, tek veritabanı ve havuza alınmış veritabanını dağıtılmasını değişiklikleri içinde yapılandırılmış veya şirket içi SQL Server. Bu yapılandırmada, yayımcı/dağıtıcı yönetilen örnek yapılandırılamaz ile [coğrafi çoğaltma ve otomatik yük devretme grupları](sql-database-auto-failover-group.md).

### <a name="publisher-with-remote-distributor-on-a-managed-instance"></a>İle yönetilen bir örneği hale getirirken uzak dağıtımcı yayımcı

Bu yapılandırmada, bir yönetilen örnek başka bir yönetilen hizmet birçok kaynak yönetilen örneği ve bir veya daha çok hedef yönetilen örneği, tek veritabanı, havuza veritabanı değişikliklerini dağıtmak örneği yerleştirildiği dağıtıcı değişiklikleri yayımlar veya SQL Server.

![Yayımcı ve dağıtıcı için ayrı örnekleri](media/replication-with-sql-database-managed-instance/02-separate-instances-asdbmi-pubdist.png)

İki yönetilen örneği, yayımcı ve dağıtıcı yapılandırılır. Bu yapılandırmada

- Hem yönetilen aynı vNet üzerinde örnekleridir.
- Hem yönetilen örnekler aynı konumda olan.
- Yayımlanan barındıran yönetilen örnekleri ve dağıtımcısı veritabanları [coğrafi olarak çoğaltılmış otomatik yük devretme grupları kullanarak](sql-database-auto-failover-group.md).

### <a name="publisher-and-distributor-on-premises-with-a-subscriber-on-a-single-pooled-and-instance-database"></a>Yayımcı ve dağıtıcı ile şirket içi bir abone tek bir havuzda ve veritabanı örneği 

![Abone olarak Azure SQL DB](media/replication-with-sql-database-managed-instance/03-azure-sql-db-subscriber.png)
 
Bu yapılandırmada, Azure SQL veritabanı (tek bir havuzda ve veritabanı örneği) abone durumda. Bu yapılandırma şirket içinden azure'a geçişi destekler. Bir tek veya havuza alınmış veritabanının abone ise gönderim modunda olması gerekir.  


## <a name="next-steps"></a>Sonraki adımlar

1. [İki yönetilen örnekleri arasında çoğaltmayı yapılandırma](replication-with-sql-database-managed-instance.md). 
1. [Yayın oluşturma](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication).
1. [Gönderme temelli bir abonelik oluşturmak](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription) abonesi olarak Azure SQL veritabanı sunucu adını kullanarak (örneğin `N'azuresqldbdns.database.windows.net` ve hedef veritabanı olarak Azure SQL veritabanı adı (örneğin **Adventureworks**. )



## <a name="see-also"></a>Ayrıca Bkz.  

- [SQL Veritabanına Çoğaltma](replication-to-sql-database.md)
- [Yönetilen örnek için çoğaltma](replication-with-sql-database-managed-instance.md)
- [Bir yayın oluşturun](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication)
- [Gönderme temelli bir abonelik oluşturun](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription/)
- [Çoğaltma türleri](https://docs.microsoft.com/sql/relational-databases/replication/types-of-replication)
- [İzleme (Çoğaltma)](https://docs.microsoft.com/sql/relational-databases/replication/monitor/monitoring-replication)
- [Bir abonelik başlatma](https://docs.microsoft.com/sql/relational-databases/replication/initialize-a-subscription)  
