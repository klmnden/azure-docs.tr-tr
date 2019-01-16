---
title: İşlem çoğaltma ile Azure SQL mantıksal sunucusu ve Azure SQL yönetilen örneği | "Microsoft Docs
description: Azure SQL veritabanı mantıksal sunucuları ve SQL yönetilen örneği ile SQL Server işlem çoğaltma kullanma hakkında bilgi edinin.
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
ms.date: 01/08/2019
ms.openlocfilehash: 1839ca0d2495a07f6fc734501540cddcdcb28e18
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54332904"
---
# <a name="transactional-replication-with-azure-sql-logical-server-and-azure-sql-managed-instance"></a>İşlem çoğaltma ile Azure SQL mantıksal sunucusu ve Azure SQL yönetilen örneği

İşlem çoğaltma bir Azure SQL veritabanı yönetilen örneği ve Azure SQL veritabanındaki bir tablodan veri çoğaltma olanak tanıyan SQL Server veya SQL Server uzak veritabanlarında yerleştirilen tablolara özelliğidir. Bu özellik, birden fazla tablo farklı veritabanlarındaki eşitlemenize olanak tanır.

## <a name="overview"></a>Genel Bakış 
İşlem çoğaltma anahtar bileşenler aşağıdaki resimde gösterilmektedir:  

![SQL veritabanı ile çoğaltma](media/replication-to-sql-database/replication-to-sql-database.png)


**Yayımcı** dağıtımcı güncelleştirmeleri göndererek örneği veya bazı tablolar (makaleler) yapılan değişiklikler yayımlayan sunucusunu olduğu. Bir şirket içi SQL Server'dan Azure SQL veritabanı yayımlama, aşağıdaki SQL Server sürümlerinde desteklenir:
    - SQL Server 2019 (Önizleme)
    - SQL Server 2016 SQL 2017
    - SQL Server 2014 SP1 CU3 veya büyük (12.00.4427)
    - SQL Server 2014 RTM CU10 (12.00.2556)
    - SQL Server 2012 SP3 veya büyük (11.0.6020)
    - SQL Server 2012 SP2 CU8 (11.0.5634.0)
    - Azure'da yayımlama nesnelere desteklemeyen başka SQL Server sürümleri için kullanmak mümkün [verileri yeniden yayımlayarak](https://docs.microsoft.com/sql/relational-databases/replication/republish-data) daha yeni SQL Server sürümleri için verileri taşımak için yöntemi. 

**Dağıtıcı** bir örneği ya da bir yayımcıdan değişiklikleri makalelerinde toplar ve bunları abonelere dağıtan bir sunucu. Dağıtımcısı, Azure SQL veritabanı yönetilen örneği veya SQL Server'ın (yayımcı sürümden daha yüksek veya ona eşit, uzun gibi herhangi bir sürüm) olabilir. 

**Abone** bir örneği veya yayımcı üzerinde yapılan değişiklikleri alan bir sunucu. Azure SQL veritabanı mantıksal sunucusu/yönetilen örnek veya SQL Server aboneleri olabilir. Bir mantıksal sunucu üzerinde bir abonelik anında iletme abonesi olarak yapılandırılması gerekir. 

| Rol | Mantıksal sunucu | Yönetilen Örnek |
| :----| :------------- | :--------------- |
| **Yayımcı** | Hayır | Evet | 
| **Dağıtıcı** | Hayır | Evet|
| **Abone çekme** | Hayır | Evet|
| **Anında iletme abone**| Evet | Evet|
| &nbsp; | &nbsp; | &nbsp; |

Vardır farklı [çoğaltma türleri](https://docs.microsoft.com/sql/relational-databases/replication/types-of-replication?view=sql-server-2017):


| Çoğaltma | Mantıksal sunucu | Yönetilen Örnek |
| :----| :------------- | :--------------- |
| [**işlem**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/transactional-replication) | Evet (yalnızca abonesi olarak) | Evet | 
| [**Anlık görüntü**](https://docs.microsoft.com/sql/relational-databases/replication/snapshot-replication) | Evet (yalnızca abonesi olarak) | Evet|
| [**Çoğaltma birleştirme**](https://docs.microsoft.com/sql/relational-databases/replication/merge/merge-replication) | Hayır | Hayır|
| [**Eşler arası**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/peer-to-peer-transactional-replication) | Hayır | Hayır|
| **Tek yönlü** | Evet | Evet|
| [**Çift yönlü**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/bidirectional-transactional-replication) | Hayır | Evet|
| [**Güncelleştirilebilir abonelikler**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication) | Hayır | Hayır|
| &nbsp; | &nbsp; | &nbsp; |

  >[!NOTE]
  > - Eski bir sürümü kullanılarak çoğaltma yapılandırmaya çalışmadan sonuçlanabilir hata numarası MSSQL_REPL20084 (işlem abone. bağlanamadı) ve MSSQ_REPL40532 (sunucu açamıyor <name> oturum açma tarafından istenen. Oturum açma başarısız.)
  > - Azure SQL veritabanı'nın tüm özelliklerini kullanabilmek için en son sürümlerini kullanarak [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017) ve [SQL Server veri Araçları (SSDT)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2017).

## <a name="requirements"></a>Gereksinimler
- Bağlantı çoğaltma katılımcılar SQL kimlik doğrulaması kullanır. 
- Çoğaltma tarafından kullanılan çalışma dizini için bir Azure depolama hesabı paylaşımı. 
- Bağlantı noktası 445 (TCP Giden) Azure dosya paylaşımına erişmek için yönetilen örnek alt güvenlik kurallarında açık olması gerekir. 
- Bağlantı noktası 1433 (TCP Giden) yayımcı/dağıtıcı bağlantılarının yönetilen örneği'nde ve şirket içi abone ise açılması gerekir. 

## <a name="common-configurations"></a>Ortak yapılandırmaları
Genel olarak, yayımcı ve dağıtıcı ya da bulutta veya şirket içinde olması gerekir. Aşağıdaki yapılandırmalar desteklenir: 

### <a name="publisher-with-local-distributor-on-a-managed-instance"></a>Yayımcı ile yönetilen örneğinde yerel dağıtıcı

![Yayımcı ve dağıtıcı olarak tek örnek ](media/replication-with-sql-database-managed-instance/01-single-instance-asdbmi-pubdist.png)

Yayımcı ve dağıtıcı tek bir yönetilen örnek içinde yapılandırılır. 

### <a name="publisher-with-remote-distributor-on-a-managed-instance"></a>İle yönetilen bir örneği hale getirirken uzak dağıtımcı yayımcı

![Yayımcı ve dağıtıcı için ayrı örnekleri](media/replication-with-sql-database-managed-instance/02-separate-instances-asdbmi-pubdist.png)

İki yönetilen örneği, yayımcı ve dağıtıcı yapılandırılır. Bu yapılandırmada

- Hem yönetilen aynı vNet üzerinde örnekleridir.
- Hem yönetilen örnekler aynı konumda olan. 

### <a name="publisher-and-distributor-on-premises-with-a-subscriber-on-a-managed-instance-or-logical-server"></a>Yayımcı ve dağıtıcı ile şirket içi bir yönetilen örneği veya mantıksal sunucuda abone 

![Abone olarak Azure SQL DB](media/replication-with-sql-database-managed-instance/03-azure-sql-db-subscriber.png)
 
Bu yapılandırmada, Azure SQL veritabanı (yönetilen örneği veya mantıksal sunucu) abone durumda. Bu yapılandırma şirket içinden azure'a geçişi destekler. Mantıksal sunucu üzerinde bir abonelik varsa gönderim modunda olması gerekir.  

## <a name="next-steps"></a>Sonraki adımlar
1. [İşlem çoğaltma için bir yönetilen örnek yapılandırma](https://docs.microsoft.com/azure/sql-database/replication-with-sql-database-managed-instance#configure-publishing-and-distribution-example). 
1. [Yayın oluşturma](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication).
1. [Gönderme temelli bir abonelik oluşturmak](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription) abonesi olarak Azure SQL veritabanı mantıksal sunucusu adını kullanarak (örneğin `N'azuresqldbdns.database.windows.net` ve hedef veritabanı olarak Azure SQL veritabanı adı (örneğin **Adventureworks**. )


## <a name="see-also"></a>Ayrıca Bkz.  

- [SQL Veritabanına Çoğaltma](replication-to-sql-database.md)
- [Yönetilen örnek için çoğaltma](replication-with-sql-database-managed-instance.md)
- [Bir yayın oluşturun](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication)
- [Gönderme temelli bir abonelik oluşturun](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription/)
- [Çoğaltma türleri](https://docs.microsoft.com/sql/relational-databases/replication/types-of-replication)
- [İzleme (Çoğaltma)](https://docs.microsoft.com/sql/relational-databases/replication/monitor/monitoring-replication)
- [Bir abonelik başlatma](https://docs.microsoft.com/sql/relational-databases/replication/initialize-a-subscription)  
