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
ms.workload: Inactive
ms.date: 07/31/2016
ms.author: sashan
ms.openlocfilehash: 8e395153fc9907107156c3412e5e0de554c83750
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="performing-disaster-recovery-drill"></a>Olağanüstü durum kurtarma ayrıntıya gerçekleştirme
Kurtarma iş akışı için uygulama hazırlık doğrulanması düzenli aralıklarla gerçekleştirilen önerilir. Uygulama davranışına ve etkileri doğrulama veri kaybına ve/veya kesintisi Bu yük devretme içerir iyi bir mühendislik uygulamadır. Ayrıca, çoğu endüstri standartlarına göre gerekli iş sürekliliği sertifika kapsamında değildir.

Bir olağanüstü durum kurtarma ayrıntıya gerçekleştirme oluşur:

* Benzetirme veri katmanı kesinti
* Kurtarma
* Uygulama bütünlüğü post kurtarma doğrula

Nasıl bağlı olarak, [uygulamanız iş sürekliliği için tasarlanmış](sql-database-business-continuity.md), ayrıntıya yürütmek için iş akışı değişebilir. Aşağıda Azure SQL veritabanı bağlamında bir olağanüstü durum kurtarma ayrıntıya gerçekleştirme en iyi uygulamaları açıklar.

## <a name="geo-restore"></a>Coğrafi Geri Yükleme
Bir olağanüstü durum kurtarma ayrıntıya yürütülürken olası veri kaybını önlemek için üretim ortamında bir kopyasını oluşturarak ve uygulamanın yük devretme iş akışı doğrulamak için kullanılarak bir test ortamı kullanarak ayrıntıya gerçekleştirme öneririz.

#### <a name="outage-simulation"></a>Kesinti benzetimi
Kesinti benzetimini yapmak için silebilir veya kaynak veritabanını yeniden adlandırın. Bu uygulama bağlantısı hataları neden olur.

#### <a name="recovery"></a>Kurtarma
* Açıklandığı gibi farklı bir sunucu veritabanının coğrafi geri yükleme gerçekleştirmek [burada](sql-database-disaster-recovery.md).
* Kurtarılan veritabanına bağlanmak ve izlenmesi uygulama yapılandırmasını değiştirme [bir veritabanını kurtarma işleminden sonra yapılandırma](sql-database-disaster-recovery.md) kurtarma işlemini tamamlamak için kılavuz.

#### <a name="validation"></a>Doğrulama
* Ayrıntıya (bağlantı dizeleri, oturum açma bilgileri, temel işlevselliğini test etme veya diğer standart uygulama signoffs yordamları doğrulamaları parçası dahil) uygulama bütünlüğü post kurtarma doğrulayarak tamamlayın.

## <a name="geo-replication"></a>Coğrafi çoğaltma
Coğrafi çoğaltma kullanılarak korunan bir veritabanı için planlanan yük devretme ikincil veritabanına ayrıntıya alıştırma içerir. Planlanmış yük devretme rolleri anahtarlı zaman birincil ve ikincil veritabanlarıyla eşitlenmiş kalmasını sağlar. Üretim ortamında ayrıntıya gerçekleştirilebilir şekilde planlanmamış yük devretme farklı olarak bu işlem veri kaybına oluşmaz.

#### <a name="outage-simulation"></a>Kesinti benzetimi
Kesinti benzetimini yapmak için web uygulaması ya da sanal makinenin veritabanına bağlı devre dışı bırakabilirsiniz. Bu web istemcileri için bağlantı hataları sonuçlanır.

#### <a name="recovery"></a>Kurtarma
* Uygulama yapılandırması DR bölgede hangi tamamen erişilebilir yeni birincil hale eski ikincil işaret ettiğinden emin olun.
* Gerçekleştirmek [planlanan yük devretme](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) ikincil veritabanını yeni bir birincil yapmak için
* İzleyin [bir veritabanını kurtarma işleminden sonra yapılandırma](sql-database-disaster-recovery.md) kurtarma işlemini tamamlamak için kılavuz.

#### <a name="validation"></a>Doğrulama
* Ayrıntıya (bağlantı dizeleri, oturum açma bilgileri, temel işlevselliğini test etme veya diğer standart uygulama signoffs yordamları doğrulamaları parçası dahil) uygulama bütünlüğü post kurtarma doğrulayarak tamamlayın.

## <a name="next-steps"></a>Sonraki adımlar
* İş sürekliliği senaryoları hakkında bilgi edinmek için [sürekliliği senaryoları](sql-database-business-continuity.md)
* Veritabanı Yedeklemeleri otomatik Azure SQL hakkında bilgi edinmek için bkz: [SQL veritabanı otomatik yedeklemeler](sql-database-automated-backups.md)
* Kurtarma için otomatik yedeklemeler kullanma hakkında bilgi edinmek için bkz: [bir veritabanı hizmeti tarafından başlatılan yedeklerden geri yükleme](sql-database-recovery-using-backups.md)
* Daha hızlı kurtarma seçenekleri hakkında bilgi edinmek için [aktif coğrafi çoğaltma](sql-database-geo-replication-overview.md)  
