---
title: "SQL veritabanı olağanüstü durum kurtarma ayrıntılarını | Microsoft Docs"
description: "Kılavuzu ve olağanüstü durum kurtarma ayrıntılarını gerçekleştirmek için kullanarak Azure SQL veritabanı için en iyi yöntemleri öğrenin."
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: b44a269c-fe2a-404f-b013-290030860bd1
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.date: 10/20/2016
ms.workload: Inactive
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: 73c2cbe978c980cbe1269b34cdb9f5ff86113e61
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2017
---
# <a name="performing-disaster-recovery-drill"></a>Olağanüstü durum kurtarma ayrıntıya gerçekleştirme
Kurtarma iş akışı için uygulama hazırlık doğrulanması düzenli aralıklarla gerçekleştirilen önerilir. Uygulama davranışına ve etkileri doğrulama veri kaybına ve/veya kesintisi Bu yük devretme içerir iyi bir mühendislik uygulamadır. Ayrıca, çoğu endüstri standartlarına göre gerekli iş sürekliliği sertifika kapsamında değildir.

Bir olağanüstü durum kurtarma ayrıntıya gerçekleştirme oluşur:

* Benzetirme veri katmanı kesinti
* Kurtarma
* Uygulama bütünlüğü post kurtarma doğrula

Nasıl bağlı olarak, [uygulamanız iş sürekliliği için tasarlanmış](sql-database-business-continuity.md), ayrıntıya yürütmek için iş akışı değişebilir. Bu makalede bir olağanüstü durum kurtarma ayrıntıya Azure SQL veritabanı bağlamında yürütülmesi için en iyi uygulamaları açıklar.

## <a name="geo-restore"></a>Coğrafi Geri Yükleme
Bir olağanüstü durum kurtarma ayrıntıya yürütülürken olası veri kaybını önlemek için üretim ortamında bir kopyasını oluşturarak ve uygulamanın yük devretme iş akışı doğrulamak için kullanılarak bir test ortamı kullanarak ayrıntıya gerçekleştirin.

#### <a name="outage-simulation"></a>Kesinti benzetimi
Kesinti benzetimini yapmak için kaynak veritabanı yeniden adlandırabilirsiniz. Bu uygulama bağlantısı hataları neden olur.

#### <a name="recovery"></a>Kurtarma
* Açıklandığı gibi farklı bir sunucu veritabanının coğrafi geri yükleme gerçekleştirmek [burada](sql-database-disaster-recovery.md).
* Kurtarılan veritabanına bağlanmak ve izlenmesi uygulama yapılandırmasını değiştirme [bir veritabanını kurtarma işleminden sonra yapılandırma](sql-database-disaster-recovery.md) kurtarma işlemini tamamlamak için kılavuz.

#### <a name="validation"></a>Doğrulama
* Ayrıntıya (bağlantı dizeleri, oturum açma bilgileri, temel işlevselliğini test etme veya diğer standart uygulama signoffs yordamları doğrulamaları parçası dahil) uygulama bütünlüğü post kurtarma doğrulayarak tamamlayın.

## <a name="failover-groups"></a>Yük devretme grupları
Yük devretme grupları kullanılarak korunan bir veritabanı için planlanan yük devretme ikincil sunucuya ayrıntıya alıştırma içerir. Planlanmış yük devretme rolleri anahtarlı zaman birincil ve ikincil veritabanları yük devretme grubunda eşitlenmiş kalmasını sağlar. Üretim ortamında ayrıntıya gerçekleştirilebilir şekilde planlanmamış yük devretme farklı olarak bu işlem veri kaybına oluşmaz.

#### <a name="outage-simulation"></a>Kesinti benzetimi
Kesinti benzetimini yapmak için web uygulaması ya da sanal makinenin veritabanına bağlı devre dışı bırakabilirsiniz. Bu web istemcileri için bağlantı hataları sonuçlanır.

#### <a name="recovery"></a>Kurtarma
* Tamamen erişilebilir yeni birincil hale uygulama yapılandırması ikincil, eski DR bölgeye işaret emin olun.
* Başlatma [planlanan yük devretme](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) ikincil sunucudan yük devretme grubunun.
* İzleyin [bir veritabanını kurtarma işleminden sonra yapılandırma](sql-database-disaster-recovery.md) kurtarma işlemini tamamlamak için kılavuz.

#### <a name="validation"></a>Doğrulama
Ayrıntıya (bağlantı, temel işlevselliğini test etme veya ayrıntıya signoffs için gereken başka doğrulama dahil) uygulama bütünlüğü post kurtarma doğrulayarak tamamlayın.

## <a name="next-steps"></a>Sonraki adımlar
* İş sürekliliği senaryoları hakkında bilgi edinmek için [sürekliliği senaryoları](sql-database-business-continuity.md).
* Veritabanı Yedeklemeleri otomatik Azure SQL hakkında bilgi edinmek için bkz: [SQL veritabanı otomatik yedeklemeler](sql-database-automated-backups.md)
* Kurtarma için otomatik yedeklemeler kullanma hakkında bilgi edinmek için bkz: [bir veritabanını hizmeti tarafından başlatılan yedeklerden geri](sql-database-recovery-using-backups.md).
* Daha hızlı kurtarma seçenekleri hakkında bilgi edinmek için [etkin coğrafi çoğaltma ve yük devretme gruplar](sql-database-geo-replication-overview.md).  
