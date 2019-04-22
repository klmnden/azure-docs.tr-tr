---
title: Genel amaçlı hizmet katmanı - Azure SQL veritabanı | Microsoft Docs
description: Azure SQL veritabanı genel amaçlı katmanı hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop-msft
ms.reviewer: sstein
manager: craigg
ms.date: 02/07/2019
ms.openlocfilehash: dc379f1ee67174cd806840e4244054701d18f0d4
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59784031"
---
# <a name="general-purpose-service-tier---azure-sql-database"></a>Genel amaçlı hizmet katmanı - Azure SQL veritabanı

> [!NOTE]
> Genel amaçlı sanal çekirdek tabanlı satın alma modeli hizmet katmanında standart Hizmet katmanını DTU tabanlı satın alma modeli çağrılır. Sanal çekirdek tabanlı satın alma modeli DTU tabanlı satın alma modeli ile bir karşılaştırması için bkz: [Azure SQL veritabanı'nın modelleri ve kaynakları satın alma](sql-database-purchase-models.md).

Azure SQL veritabanı, SQL Server veritabanı altyapısı mimarisi altyapı hataları durumda bile % 99,99 kullanılabilirlik sağlamak üzere bulut ortamı için uyarlanmış dayanır. Azure SQL veritabanı, her biri farklı bir mimari modelleri kullanılan üç hizmet katmanı vardır. Bu hizmet katmanlarıdır:

- Genel amaçlı
- İş açısından kritik
- Hiper ölçeklendirme

Genel amaçlı hizmet katmanı için Mimari modelini ayrımı işlem ve depolama temel alır. Bu mimari bir model üzerinde yüksek kullanılabilirlik kullanır ve şeffaf bir şekilde veritabanı dosyaları çoğaltır ve temel alınan altyapı hatası durumunda veri kaybı olmadan garanti eder, Azure Blob Depolama güvenilirliğini gerçekleşir.

Aşağıdaki şekilde, standart mimari modelinde ayrılmış işlem ve depolama katmanları ile dört düğüm gösterilmektedir.

![İşlem ve depolama ayrımı](media/sql-database-managed-instance/general-purpose-service-tier.png)

Genel amaçlı hizmet katmanı için Mimari modelinde iki katman vardır:

- Çalışmakta olan bir durum bilgisi olmayan bilgi işlem katmanı `sqlserver.exe` işlemek ve yalnızca geçici ve önbelleğe alınan verileri (örneğin-planı önbellek, arabellek havuzu, sütun depolama havuzu) içerir. Azure Service Fabric tarafından işlem başlatır, düğümün sistem durumu denetimleri ve gerekirse, başka bir yere yük devretme gerçekleştirir, bu durum bilgisi olmayan SQL Server düğümünde çalıştırılır.
- Bir durum bilgisi olan veri katmanı ile Azure Blob depolamada depolanan veritabanı dosyaları (.mdf/.ldf). Azure Blob Depolama, veri kaybı olmadan bir veritabanı dosyasına yerleştirilir herhangi bir kayıt olacaktır garanti eder. Azure depolama, SQL Server işlem çökse bile her kayıt günlük dosyasında veya sayfa veri dosyasında korunur sağlar yerleşik veri kullanılabilirlik/yedeklilik sahiptir.

Veritabanı altyapısı veya işletim sistemi yükseltme olduğunda, bazı altyapının parçası başarısız olursa veya SQL Server işleminde bazı önemli sorun algılanırsa, başka bir durum bilgisi olmayan bir işlem düğümüne yüklenecek Azure Service Fabric durum bilgisi olmayan SQL Server işlemi taşınır. Yeni işlem hizmeti birincil düğüm başarısız olması durumunda yük devretme süresini en aza indirmek için durumda çalıştırılmayı bekliyor yedek düğümleri kümesini yoktur. Verileri Azure depolama katmanında etkilenmez ve yeni oluşturulmuş SQL Server işlemi için veri/günlük dosyalar eklenir. Bu işlem, % 99,99 oranında kullanılabilirlik garanti eder, ancak bu geçiş süresi nedeniyle çalıştıran ağır iş yükü üzerindeki bazı performans etkileri olabilir ve yeni SQL Server düğümü olgu soğuk önbellek ile başlar.

## <a name="when-to-choose-this-service-tier"></a>Bu hizmet katmanını seçmek ne zaman

Genel amaçlı hizmet katmanı, genel iş yüklerinin çoğu için tasarlanmış bir Azure SQL veritabanı'nda varsayılan hizmet katmandır. % 99,99 SLA genellikle, Azure SQL Iaas uyan 5 ve 10 ms arasındaki depolama gecikme süresi ile tam olarak yönetilen veritabanı altyapısıyla gerekiyorsa, genel amaçlı katmanı, seçenektir.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [iş açısından kritik](sql-database-service-tier-business-critical.md) ve [hiper ölçekli](sql-database-service-tier-hyperscale.md) katmanları.
- Hakkında bilgi edinin [Service Fabric](../service-fabric/service-fabric-overview.md).
- Yüksek kullanılabilirlik ve olağanüstü durum kurtarma için daha fazla seçenek için bkz: [iş sürekliliği](sql-database-business-continuity.md).
