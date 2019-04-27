---
title: SQL veritabanı olağanüstü durum kurtarma Tatbikatlarını | Microsoft Docs
description: Yönergeler ve olağanüstü durum kurtarma tatbikatı gerçekleştirme amacıyla Azure SQL veritabanı'nı kullanmak için en iyi uygulamaları öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 12/18/2018
ms.openlocfilehash: 5d754ae558d485036a9a55f573a3f40162ed9f84
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60725440"
---
# <a name="performing-disaster-recovery-drill"></a>Olağanüstü durum kurtarma Tatbikatı gerçekleştirme

Kurtarma iş akışı için uygulama hazır olma durumu doğrulaması düzenli aralıklarla gerçekleştirilen önerilir. Uygulamaları ve uygulama davranışı doğrulama, yük devretme veri kaybı ve/veya kesinti içerir iyi bir mühendislik uygulamasıdır. Ayrıca, çoğu endüstri standartlarına göre iş sürekliliği sertifika kapsamında olmasa da gereklidir.

Olağanüstü durum kurtarma tatbikatı gerçekleştirme şunlardan oluşur:

* Veri katmanı kesinti benzetimi
* Kurtarılıyor
* Uygulama bütünlüğünü post kurtarma doğrula

Nasıl bağlı olarak, [uygulamanız iş sürekliliği için tasarlanmış](sql-database-business-continuity.md), detaya yürütülecek iş akışını farklılık gösterebilir. Bu makalede, Azure SQL veritabanı bağlamında olağanüstü durum kurtarma tatbikatı gerçekleştirme için en iyi uygulamaları açıklar.

## <a name="geo-restore"></a>Coğrafi Geri Yükleme

Olağanüstü durum kurtarma tatbikatı gerçekleştirme, olası veri kaybını önlemek için üretim ortamının bir kopya oluşturma ve uygulamanın yük devretme iş akışını doğrulamak için kullanarak bir test ortamı'nı kullanarak detaya gerçekleştirin.

### <a name="outage-simulation"></a>Kesinti simülasyonu

Kesinti benzetimini yapmak için kaynak veritabanının yeniden adlandırabilirsiniz. Bu ad değişikliği, uygulama bağlantı hatalarına neden oluyor.

### <a name="recovery"></a>Kurtarma

* Açıklandığı gibi farklı bir sunucuya veritabanı coğrafi geri yükleme gerçekleştirme [burada](sql-database-disaster-recovery.md).
* Kurtarılan veritabanına bağlanmak ve takip uygulama yapılandırmasını değiştirmek [bir veritabanının kurtarma işleminden sonra yapılandırma](sql-database-disaster-recovery.md) ve kurtarmayı tamamlamak için Kılavuzu.

### <a name="validation"></a>Doğrulama

(Bağlantı dizeleri, oturum açma bilgileri, temel işlevselliğini test etme veya diğer standart uygulama signoffs yordamları doğrulamaları kısmı dahil olmak üzere) uygulama bütünlüğünü post kurtarma doğrulayarak ayrıntıya tamamlayın.

## <a name="failover-groups"></a>Yük devretme grupları

Yük devretme grupları kullanarak korunan bir veritabanı için planlı yük devretme ikincil sunucuya ayrıntıya alıştırma içerir. Planlanmış yük devretme rolleri geçti, birincil ve ikincil veritabanları yük devretme grubuna eşitlenmiş durumda kalmasını sağlar. Planlanmamış yük devretme ayrıntıya üretim ortamında gerçekleştirilebilir şekilde bu işlem veri kaybına sonuçlanmaz.

### <a name="outage-simulation"></a>Kesinti simülasyonu

Kesinti simülasyonu yapma web uygulaması veya veritabanına bağlı sanal makine devre dışı bırakabilirsiniz. Bu kesinti simülasyonu web istemcileri için bağlantı hataları oluşur.

### <a name="recovery"></a>Kurtarma

* Yeni birincil tümüyle erişilebilir hale DR bölgeye işaret ikincil, eski uygulama yapılandırması emin olun.
* Başlatma [planlı yük devretme](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) ikincil sunucuya Yük devretme grubundan biri.
* İzleyin [bir veritabanının kurtarma işleminden sonra yapılandırma](sql-database-disaster-recovery.md) ve kurtarmayı tamamlamak için Kılavuzu.

### <a name="validation"></a>Doğrulama

(Bağlantı, temel işlevselliğini test etme veya ayrıntıya signoffs için gereken başka bir doğrulama dahil olmak üzere) uygulama bütünlüğünü post kurtarma doğrulayarak ayrıntıya tamamlayın.

## <a name="next-steps"></a>Sonraki adımlar

* İş sürekliliği senaryoları hakkında bilgi edinmek için [sürekliliği senaryoları](sql-database-business-continuity.md).
* Veritabanı otomatik yedeklemeler Azure SQL hakkında bilgi edinmek için bkz: [SQL veritabanı otomatik yedekleme](sql-database-automated-backups.md)
* Otomatik yedekleme, kurtarma için kullanma hakkında bilgi edinmek için [hizmet tarafından başlatılan yedeklemelerden veritabanını geri yükleme](sql-database-recovery-using-backups.md).
* Daha hızlı kurtarma seçenekleri hakkında bilgi edinmek için [etkin coğrafi çoğaltma](sql-database-active-geo-replication.md) ve [otomatik yük devretme grupları](sql-database-auto-failover-group.md).
