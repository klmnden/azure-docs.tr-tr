---
title: Hızlı Başlangıç - Azure SQL veritabanı | Microsoft Docs
description: Azure SQL veritabanı - tekil hızla öğrenin
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlr
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: dad9f004b49ee98e6efdd7d10b779ec3bd81fa47
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2019
ms.locfileid: "55105960"
---
# <a name="getting-started-with-azure-sql-database-singleton"></a>Azure SQL veritabanı tek ile çalışmaya başlama

[Azure SQL veritabanı - tekil](https://docs.microsoft.com/azure/sql-database/sql-database-single-index) veya tek veritabanı tam olarak yönetilen PaaS veritabanı buluttan kaynaklanan modern uygulamalar için ideal depolama altyapısı olan bir hizmeti (DbaaS) olarak. Bu bölümde, hızlı bir şekilde yapılandırın ve SQL veritabanı oluşturmayı öğreneceksiniz.

## <a name="quickstart-overview"></a>Hızlı genel bakış

Bu bölümde, tek veritabanı ile hızlıca çalışmaya başlamanıza yardımcı olabilecek makalelerde genel bakış görürsünüz. İlk SQL veritabanınızı oluşturmak için en kolay yolu kullanmaktır [Azure portalında](sql-database-get-started-portal.md) gerekli parametreleri yapılandırabileceğiniz.
Oluşturulduktan sonra gerekir [güvenlik duvarı kurallarını yapılandırarak veritabanınızın güvenliğini sağlama](sql-database-get-started-portal-firewall.md). 

Varolan bir veritabanını Azure'a geçirmek istediğiniz SQL Server varsa yüklemelisiniz [Data Migration Yardımcısı (DMA)](https://www.microsoft.com/download/details.aspx?id=53595) , SQL Server veritabanlarınızı analiz edin ve tek geçişi engelleyebilecek herhangi bir sorun bulunamadı Veritabanı. Herhangi bir sorun bulamazsanız, veritabanınız olarak dışa aktarabilirsiniz `.bacpac` dosya ve [Azure portalı veya SqlPackage kullanarak içeri aktarma](sql-database-import.md).

Bu hızlı başlangıçlar, hızlı bir şekilde yapılandırmak, oluşturmak ve veritabanlarınızı Azure bulutunda almak etkinleştirin.

## <a name="automating-management-operations"></a>Yönetim işlemlerini otomatikleştirme

Azure portalı, Azure tek veritabanı kolayca oluşturup sağlar. Alternatif olarak kullanabileceğiniz [PowerShell](scripts/sql-database-create-and-configure-database-powershell.md), veya [Azure CLI](scripts/sql-database-create-and-configure-database-cli.md) veritabanı oluşturma.
Kullanarak, tek veritabanı ve ölçek kaynakları da güncelleştirebilirsiniz [PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md) veya [Azure CLI](scripts/sql-database-monitor-and-scale-database-cli.md).

## <a name="migrating-to-azure-single-database-with-minimal-downtime"></a>En düşük kapalı kalma süresi ile Azure tek veritabanı'na geçirme

Bu hızlı başlangıç makalelerinde etkinleştirme hızlıca oluşturmanızı veya veritabanınızı azure'a içeri aktarmaya `.bacpac`. Ancak, `.bacpac` ve `.dacpack` dosyaları, veritabanını SQL Server ve Azure SQL veritabanı (tek bir veritabanı veya yönetilen örneği ') farklı sürümleri arasında hızlıca taşımak veya sürekli tümleştirme, DevOps işlem hattınızda uygulamak için tasarlanmıştır. Kaynak veritabanı olarak dışarı aktarmak için beklemeniz gerekir çünkü ancak, bu yöntem, üretim veritabanlarınızda en düşük kapalı kalma süresiyle geçiş için tasarlanmamıştır `.bacpac` dosya ve Azure SQL, bazı kapalı kalma durumlarına neden veritabanına aktarın, Veritabanı özellikle büyük olduğunda, uygulama. Geçişin en düşük kapalı kalma süresi garanti eder daha iyi şekilde geçirmek için üretim veritabanınız taşıyorsanız, büyük olasılıkla gerekir. [Data Migration hizmeti](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-azure-sql?toc=/azure/sql-database/toc.json) veritabanınızı en düşük kapalı kalma süresi ile geçirebileceğiniz bir hizmettir. Bu şekilde uygulamanızı kaynaktan hedef veritabanına en az kapalı kalma süresiyle hızlı bir şekilde geçiş yapabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Bulma bir [üst düzey Azure SQL veritabanı'nda desteklenen özellikler listesini](sql-database-features.md). 
* Nasıl yapacağınızı öğrenin, [veritabanı daha güvenli](sql-database-security-tutorial.md). 
* Öğreticilerde daha gelişmiş bulma [nasıl bölümüne](sql-database-howto-single-database.md). 
* Daha fazla örnek komut dosyası dilinde yazılmış Bul [PowerShell](sql-database-powershell-samples.md) ve [Azure CLI](sql-database-cli-samples.md). 
* Daha fazla bilgi edinin [yönetim API](sql-database-single-databases-manage.md) veritabanlarınızı yapılandırmak için kullanabilirsiniz. 
