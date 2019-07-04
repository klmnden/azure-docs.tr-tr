---
title: Hızlı Başlangıç - Azure SQL veritabanı'nda tek veritabanları | Microsoft Docs
description: Azure SQL veritabanı'nda tek veritabanları ile hızlıca çalışmaya başlama hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlr
manager: craigg
ms.date: 02/04/2019
ms.openlocfilehash: 7b52453bab661531461a2bec2f15f7659ec15a1c
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67441714"
---
# <a name="getting-started-with-single-databases-in-azure-sql-database"></a>Azure SQL veritabanı'nda tek veritabanları ile çalışmaya başlama

[Tek bir veritabanı](sql-database-single-index.yml) tam olarak yönetilen PaaS veritabanı buluttan kaynaklanan modern uygulamalar için ideal depolama altyapısı olan bir hizmeti (DbaaS) olarak. Bu bölümde, hızlı bir şekilde yapılandırmak ve SQL veritabanı'nda tek bir veritabanı oluşturmak öğreneceksiniz.

## <a name="quickstart-overview"></a>Hızlı genel bakış

Bu bölümde, tek veritabanları ile hızlıca çalışmaya başlamanıza yardımcı olabilecek makalelerde genel bakış görürsünüz. Hızlı bir şekilde tek veritabanı oluşturma, veritabanı sunucusu güvenlik duvarı kuralı yapılandırma ve sonra da veritabanını yeni kullanarak tek veritabanı içeri aktarma şu hızlı başlangıçlarda etkinleştirdiğiniz bir `.bacpac` dosyası:

- [Azure portalını kullanarak tek veritabanı oluşturma](sql-database-single-database-get-started.md).
- Veritabanını oluşturduktan sonra gerekir [güvenlik duvarı kurallarını yapılandırarak veritabanınızın güvenliğini sağlama](sql-database-server-level-firewall-rule.md).
- Varolan bir veritabanını Azure'a geçirmek istediğiniz SQL Server varsa yüklemelisiniz [Data Migration Yardımcısı (DMA)](https://www.microsoft.com/download/details.aspx?id=53595) , SQL Server veritabanlarınızı analiz edin ve tek geçişi engelleyebilecek herhangi bir sorun bulunamadı Veritabanı dağıtım seçeneği. Herhangi bir sorun bulamazsanız, veritabanınız olarak dışa aktarabilirsiniz `.bacpac` dosya ve [Azure portalı veya SqlPackage kullanarak içeri aktarma](sql-database-import.md).

## <a name="automating-management-operations"></a>Yönetim işlemlerini otomatikleştirme

Oluşturma, yapılandırma ve veritabanınızı ölçeklendirmek için PowerShell veya Azure CLI'yı kullanabilirsiniz.

- [Oluşturma ve PowerShell kullanarak tek bir veritabanını yapılandırma](scripts/sql-database-create-and-configure-database-powershell.md)
- [Oluşturma ve Azure CLI kullanarak tek veritabanı yapılandırma](scripts/sql-database-create-and-configure-database-cli.md)
- [PowerShell kullanarak tek veritabanı ve ölçek kaynaklarınızı güncelleştirme](scripts/sql-database-monitor-and-scale-database-powershell.md)
- [Azure CLI kullanarak tek veritabanı ve ölçek kaynaklarınızı güncelleştirme](scripts/sql-database-monitor-and-scale-database-cli.md)

## <a name="migrating-to-a-single-database-with-minimal-downtime"></a>Tek bir veritabanı en düşük kapalı kalma süresiyle geçiş

Bu hızlı başlangıçlara hızlıca oluşturmanızı veya veritabanınızı azure'a içeri aktarmaya etkinleştirme bir `.bacpac` dosya. Ancak, `.bacpac` ve `.dacpac` dosyaları hızlı bir şekilde SQL Server ve Azure SQL veritabanı içinde dağıtım seçenekleri farklı sürümleri arasında veritabanlarını taşıma veya sürekli tümleştirme, DevOps işlem hattınızda uygulamak için tasarlanmıştır. Yeni veri eklemeyi durdurmak için kaynak veritabanına verilmesini bekleyin, gerekir çünkü ancak, bu yöntem, üretim veritabanlarınızda en düşük kapalı kalma süresiyle geçiş için tasarlanmamıştır bir `.bacpac` tamamlamak için dosya ve sonra içe bekleyin Tamamlamak için Azure SQL veritabanı. Bu bekleyen tüm sonuçları kapalı kalma süresine, uygulamanızın, özellikle büyük veritabanları. Üretim veritabanınız taşımak için geçiş en düşük kapalı kalma süresini garanti eden geçirmek için daha iyi bir yol gerekir. Bunun için kullanmak [veri geçiş hizmeti (DMS)](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-azure-sql?toc=/azure/sql-database/toc.json) en az kesinti veritabanınızın geçirmek için... DMS bunu, geri yüklenmekte olan tek veritabanı için kaynak veritabanında yapılan değişiklikler artımlı olarak ileterek gerçekleştirir. Bu şekilde uygulamanızı kaynaktan hedef veritabanına en az kapalı kalma süresiyle hızlı bir şekilde geçiş yapabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Bulma bir [üst düzey Azure SQL veritabanı'nda desteklenen özellikler listesini](sql-database-features.md).
- Nasıl yapacağınızı öğrenin, [veritabanı daha güvenli](sql-database-security-tutorial.md).
- Nasıl daha gelişmiş bulma-için kullanıcının [Azure SQL veritabanı'nda tek bir veritabanını kullanmayı](sql-database-howto-single-database.md).
- Daha fazla örnek komut dosyası dilinde yazılmış Bul [PowerShell](sql-database-powershell-samples.md) ve [Azure CLI](sql-database-cli-samples.md).
- Daha fazla bilgi edinin [yönetim API'si](sql-database-single-databases-manage.md) veritabanlarınızı yapılandırmak için kullanabilirsiniz.
- [Doğru Azure SQL veritabanı/yönetilen örnek SKU'SUNDA şirket içi veritabanınızın tanımlamak](/sql/dma/dma-sku-recommend-sql-db/).