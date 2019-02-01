---
title: Genel amaçlı katmanında - Azure SQL veritabanı hizmeti | Microsoft Docs
description: Azure SQL veritabanı genel amaçlı katmanı hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop-msft
ms.reviewer: carlrab
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: 943982b056a83488426c48763deac14fd5347b8e
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55509013"
---
# <a name="general-purpose-tier---azure-sql-database"></a>Genel amaçlı katmanında - Azure SQL veritabanı

> [!NOTE]
> Genel amaçlı katmanı standart DTU satın alma modeli denir. Sanal çekirdek tabanlı satın alma modeli DTU tabanlı satın alma modeli ile bir karşılaştırması için bkz: [Azure SQL veritabanı'nın modelleri ve kaynakları satın alma](sql-database-service-tiers.md).

Azure SQL veritabanı altyapı hataları durumda bile % 99,99 kullanılabilirlik sağlamak üzere bulut ortamı için uyarlanmış SQL Server veritabanı altyapısı mimarisini temel alır. Azure SQL veritabanı'nda kullanılan üç Mimari modeli vardır:
- Genel Amaçlı 
- İş Açısından Kritik
- Hiper ölçeklendirme

Genel amaçlı modeli ayrımı işlem ve depolama temel alır. Bu mimari bir model kullanır üzerinde yüksek kullanılabilirlik ve güvenilirlik şeffaf bir şekilde veritabanı dosyaları çoğaltır ve temel alınan altyapı hatası durumunda veri kaybı olmadan garanti Azure Premium Depolama'nın gerçekleşir.

Aşağıdaki şekilde, standart mimari modelinde ayrılmış işlem ve depolama katmanları ile dört düğüm gösterilmektedir.

![İşlem ve depolama ayrımı](media/sql-database-managed-instance/general-purpose-service-tier.png)

Genel amaçlı modelinde iki katman vardır:

- Çalışmakta olan bir durum bilgisi olmayan bilgi işlem katmanı `sqlserver.exe` işlemek ve yalnızca geçici ve önbelleğe alınan verileri (örneğin-planı önbellek, arabellek havuzu, sütun depolama havuzu) içerir. Azure Service Fabric tarafından işlem başlatır, düğümün sistem durumu denetimleri ve gerekirse, başka bir yere yük devretme gerçekleştirir, bu durum bilgisi olmayan SQL Server düğümünde çalıştırılır.
- Azure Premium Depolama'da depolanan veritabanı dosyaları (.mdf/.ldf) ile bir durum bilgisi olan veri katmanı. Azure depolama, veri kaybı olmadan bir veritabanı dosyasına yerleştirilir herhangi bir kayıt olacaktır garanti eder. Azure depolama, SQL Server işlem çökse bile her kayıt günlük dosyasında veya sayfa veri dosyasında korunur sağlar yerleşik veri kullanılabilirlik/yedeklilik sahiptir.

Veritabanı altyapısı veya işletim sistemi yükseltme olduğunda, bazı altyapının parçası başarısız olursa veya SQL Server işleminde bazı önemli sorun algılanırsa, başka bir durum bilgisi olmayan bir işlem düğümüne yüklenecek Azure Service Fabric durum bilgisi olmayan SQL Server işlemi taşınır. Yeni işlem hizmeti birincil düğüm başarısız olması durumunda yük devretme süresini en aza indirmek için durumda çalıştırılmayı bekliyor yedek düğümleri kümesini yoktur. Verileri Azure depolama katmanında etkilenmez ve yeni oluşturulmuş SQL Server işlemi için veri/günlük dosyalar eklenir. Bu işlem, % 99,99 oranında kullanılabilirlik garanti eder, ancak bu geçiş süresi nedeniyle çalıştıran ağır iş yükü üzerindeki bazı performans etkileri olabilir ve yeni SQL Server düğümü olgu soğuk önbellek ile başlar.

## <a name="when-to-choose-this-service-tier"></a>Bu hizmet katmanını seçmek ne zaman?

Genel amaçlı hizmet katmanı, genel iş yüklerinin çoğu için tasarlanmış bir Azure SQL veritabanı'nda varsayılan hizmet katmandır. % 99,99 SLA genellikle, Azure SQL Iaas uyan 5 ve 10 ms arasındaki depolama gecikme süresi ile tam olarak yönetilen veritabanı altyapısıyla gerekiyorsa, genel amaçlı katmanı, seçenektir.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [iş açısından kritik](sql-database-service-tier-business-critical.md) ve [hiper ölçekli](sql-database-service-tier-hyperscale.md) katmanları.
- Hakkında bilgi edinin [Service Fabric](../service-fabric/service-fabric-overview.md).
- Yüksek kullanılabilirlik ve olağanüstü durum kurtarma için daha fazla seçenek için bkz: [iş sürekliliği](sql-database-business-continuity.md).
