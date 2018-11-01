---
title: MySQL için Azure veritabanı'nda çoğaltmaları okuyun.
description: Bu makalede, salt okunur çoğaltmalar için MySQL için Azure veritabanı açıklanır.
services: mysql
author: ajlam
ms.author: andrela
editor: jasonwhowell
ms.service: mysql
ms.topic: conceptual
ms.date: 10/30/2018
ms.openlocfilehash: b4e79723072a19f2637bea16d0534cb85588e9e3
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50412457"
---
# <a name="read-replicas-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı'nda okunur çoğaltmalar

Salt okunur çoğaltma özelliği (genel Önizleme), en fazla beş salt okunur sunucularına (Yineleme) aynı Azure bölgesindeki bir MySQL sunucusu (ana) için Azure veritabanı'ndan veri çoğaltmak sağlar. Salt okunur çoğaltmalar MySQL altyapının (binlog) yerel ikili günlük dosyası konumu tabanlı çoğaltma teknolojisini kullanarak zaman uyumsuz olarak güncelleştirilir. Binlog çoğaltma hakkında daha fazla bilgi için bkz: [MySQL binlog Çoğaltmaya genel bakış](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html).

MySQL hizmeti için Azure veritabanı'nda oluşturulan yinelemeler normal/tek başına MySQL sunucuları aynı şekilde yönetilebilir yeni sunucularıdır. Bu sunucular bir tek başına sunucu aynı fiyattan ücretlendirilir.

MySQL çoğaltma özellikler ve sorunlar hakkında daha fazla bilgi için bkz. [MySQL çoğaltma belgeleri](https://dev.mysql.com/doc/refman/5.7/en/replication-features.html).

## <a name="when-to-use-read-replicas"></a>Salt okunur çoğaltmalar kullanıldığı durumlar

Uygulamaları ve yoğun okuma iş yükleri tarafından salt okunur çoğaltmaların sunulabilir. Salt okunur çoğaltmalar yalnızca tek bir sunucu okuma ve yazma için kullanılacak olsaydı karşılaştırıldığında okuma kapasite miktarını artırmak yardımcı olur. Yazma iş yüklerinin asıl yönlendirilebilir okuma iş yükleri için çoğaltmalar, yalıtılmış olabilir.

Sık karşılaşılan bir senaryodur BI sahip olmaktır ve analiz iş yükleri okuma çoğaltması raporlama için veri kaynağı olarak kullanın.

## <a name="considerations-and-limitations"></a>Önemli noktalar ve sınırlamalar

### <a name="pricing-tiers"></a>Fiyatlandırma katmanları

Okuma çoğaltma şu anda yalnızca genel amaçlı ve bellek için iyileştirilmiş fiyatlandırma katmanlarında kullanılabilir.

### <a name="master-server-restart"></a>Ana sunucunun yeniden başlatılması

Mevcut hiçbir çoğaltması olan bir şablonu için bir çoğaltma oluşturduğunuzda bu Önizleme boyunca, ana ilk çoğaltma için hazırlanması için yeniden başlatılır. Lütfen bu dikkate alın ve yoğun olmayan bir dönem boyunca bu işlemleri gerçekleştirin.

### <a name="stopping-replication"></a>Çoğaltmayı durdurma

Bir ana ve çoğaltma sunucusu arasında çoğaltmayı durdurmak seçebilirsiniz. Çoğaltma durdurma Yöneticisi ile çoğaltma sunucusu arasındaki çoğaltma ilişkisini kaldırır.

Çoğaltma durdurulduğunda bir tek başına sunucu çoğaltma sunucusu olur. Tek başına sunucu verileri "çoğaltma stop" komutunu başlatıldığı anda çoğaltma üzerinde kullanılabilir olan verilerdir. Tek başına sunucu ana sunucu ile yakalamaz. Bu sunucu bir yinelemeye yeniden yapılamıyor.

### <a name="replicas-are-new-servers"></a>Çoğaltmaları olan yeni sunucular

Çoğaltmalar, MySQL Server için yeni bir Azure veritabanı olarak oluşturulur. Mevcut sunucuları, yinelemeler yapılamaz.

### <a name="replica-server-configuration"></a>Çoğaltma sunucusunu yapılandırma

Çoğaltma sunucusu, aşağıdaki yapılandırmaları içerir asıl aynı sunucu yapılandırmaları kullanılarak oluşturulur:

- Fiyatlandırma katmanı
- İşlem oluşturma
- Sanal çekirdekler
- Depolama
- Yedekleme bekletme süresi
- Fazladan yedek seçeneği
- MySQL altyapısı sürümü

Bir çoğaltma oluşturulduktan sonra fiyatlandırma katmanını değiştirebilirsiniz (Basic gelen ve giden hariç), işlem oluşturma, sanal çekirdek, depolama ve bağımsız olarak ana sunucudan yedekleme bekletme.

### <a name="master-server-configuration"></a>Ana sunucu yapılandırması

Asıl varsa ait sunucu yapılandırması (ör. Sanal çekirdek ve depolama) güncelleştirilir, çoğaltmaları yapılandırma da eşit veya daha büyük değerler için güncelleştirilmesi gerekir. Bu, olmadan çoğaltma sunucusu ana dala yapılan değişiklikleri takip edin mümkün olmayabilir ve sonuç olarak kilitlenebilir. 

### <a name="deleting-the-master-server"></a>Ana sunucu siliniyor

Ana sunucu silindiğinde, tüm salt okunur çoğaltmalar için çoğaltma durdurulur. Bu çoğaltmaların tek başına sunucuları olur. Ana sunucu silinir.

### <a name="user-accounts"></a>Kullanıcı hesapları

Ana sunucu üzerinde kullanıcılar, salt okunur kopyaya çoğaltılır. Yalnızca ana sunucu üzerinde kullanılabilir olan kullanıcı hesaplarını kullanarak bir okuma çoğaltması bağlanabilirsiniz.

### <a name="other"></a>Diğer

- Genel işlem tanımlayıcıları (GTID) desteklenmez.
- Bir çoğaltma bir kopyasını oluşturma desteklenmiyor.
- Bellek içi tablolar çoğaltmalar eşitlenmemiş hale neden olabilir. Bu MySQL çoğaltma teknolojisinin sınırlamasıdır. Daha fazla bilgi [MySQL başvuru belgeleri](https://dev.mysql.com/doc/refman/5.7/en/replication-features-memory.html) daha fazla bilgi için.
- Ayarlama [ `innodb_file_per_table` ](https://dev.mysql.com/doc/refman/5.7/en/innodb-multiple-tablespaces.html) parametresi bir ana sunucuya bir çoğaltma sunucusu oluşturma, çoğaltma eşitlenmemiş hale neden olabilir. Çoğaltma sunucusu farklı açabilmek uyumlu değildir.
- MySQL çoğaltma sınırlamaları tam listesini gözden geçirin [MySQL belgeleri](https://dev.mysql.com/doc/refman/5.7/en/replication-features.html)


## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [oluşturma ve salt okunur çoğaltmalar Azure portalını kullanarak yönetme](howto-read-replicas-portal.md)

<!--
- Learn how to [create and manage read replicas using the Azure CLI](howto-read-replicas-using-cli.md)
-->
