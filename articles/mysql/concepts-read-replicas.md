---
title: MySQL için Azure veritabanı'nda çoğaltmaları okuyun.
description: Bu makalede, salt okunur çoğaltmalar için MySQL için Azure veritabanı açıklanır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 02/26/2019
ms.openlocfilehash: 6e33c7571dc735ce9984a0ce1b37275a6c4c7eca
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61093225"
---
# <a name="read-replicas-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı'nda okunur çoğaltmalar

Okuma çoğaltması özelliğini sunucularına en fazla beş salt okunur (Yineleme) aynı Azure bölgesindeki bir MySQL sunucusu (ana) için Azure veritabanı'ndan veri çoğaltmanıza olanak sağlar. Salt okunur çoğaltmalar MySQL altyapının (binlog) yerel ikili günlük dosyası konumu tabanlı çoğaltma teknolojisini kullanarak zaman uyumsuz olarak güncelleştirilir. Binlog çoğaltma hakkında daha fazla bilgi için bkz: [MySQL binlog Çoğaltmaya genel bakış](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html).

MySQL hizmeti için Azure veritabanı'nda oluşturulan yinelemeler normal/tek başına MySQL sunucuları aynı şekilde yönetilebilir yeni sunucularıdır. Okuma amaçlı her çoğaltma için sanal çekirdek cinsinden sağlanan işlem ve GB/ay cinsinden sağlanan depolama karşılığı faturalandırılırsınız.

MySQL çoğaltma özellikler ve sorunlar hakkında daha fazla bilgi için bkz. [MySQL çoğaltma belgeleri](https://dev.mysql.com/doc/refman/5.7/en/replication-features.html).

## <a name="when-to-use-read-replicas"></a>Salt okunur çoğaltmalar kullanıldığı durumlar

Uygulamaları ve yoğun okuma iş yükleri tarafından salt okunur çoğaltmaların sunulabilir. Salt okunur çoğaltmalar yalnızca tek bir sunucu okuma ve yazma için kullanılacak olsaydı karşılaştırıldığında okuma kapasite miktarını artırmak yardımcı olur. Yazma iş yüklerinin asıl yönlendirilebilir okuma iş yükleri için çoğaltmalar, yalıtılmış olabilir.

Sık karşılaşılan bir senaryodur BI sahip olmaktır ve analiz iş yükleri okuma çoğaltması raporlama için veri kaynağı olarak kullanın.

## <a name="considerations-and-limitations"></a>Önemli noktalar ve sınırlamalar

### <a name="pricing-tiers"></a>Fiyatlandırma katmanları

Okuma çoğaltma şu anda yalnızca genel amaçlı ve bellek için iyileştirilmiş fiyatlandırma katmanlarında kullanılabilir.

### <a name="master-server-restart"></a>Ana sunucunun yeniden başlatılması

Bir çoğaltma yok mevcut çoğaltmaları olan bir şablonu oluşturduğunuzda, ana ilk çoğaltma için hazırlanması için yeniden başlatılır. Lütfen bu dikkate alın ve yoğun olmayan bir dönem boyunca bu işlemleri gerçekleştirin.

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
- Güvenlik duvarı kuralları

Bir çoğaltma oluşturulduktan sonra fiyatlandırma katmanını değiştirebilirsiniz (Basic gelen ve giden hariç), işlem oluşturma, sanal çekirdek, depolama ve bağımsız olarak ana sunucudan yedekleme bekletme.

### <a name="master-server-configuration"></a>Ana sunucu yapılandırması

Asıl varsa ait sunucu yapılandırması (ör. Sanal çekirdek ve depolama) güncelleştirilir, çoğaltmaları yapılandırma da eşit veya daha büyük değerler için güncelleştirilmesi gerekir. Bu, olmadan çoğaltma sunucusu ana dala yapılan değişiklikleri takip edin mümkün olmayabilir ve sonuç olarak kilitlenebilir.

Bir çoğaltma sunucusu oluşturulduktan sonra ana sunucu için eklenen yeni güvenlik duvarı kuralları çoğaltmaya çoğaltılmaz. Çoğaltma bu yeni bir güvenlik duvarı kuralı da güncelleştirilmelidir.

### <a name="deleting-the-master-server"></a>Ana sunucu siliniyor

Ana sunucu silindiğinde, tüm salt okunur çoğaltmalar için çoğaltma durdurulur. Bu çoğaltmaların tek başına sunucuları olur. Ana sunucu silinir.

### <a name="user-accounts"></a>Kullanıcı hesapları

Ana sunucu üzerinde kullanıcılar, salt okunur kopyaya çoğaltılır. Yalnızca ana sunucu üzerinde kullanılabilir olan kullanıcı hesaplarını kullanarak bir okuma çoğaltması bağlanabilirsiniz.

### <a name="other"></a>Diğer

- Genel işlem tanımlayıcıları (GTID) desteklenmez.
- Bir çoğaltma bir kopyasını oluşturma desteklenmiyor.
- Bellek içi tablolar çoğaltmalar eşitlenmemiş hale neden olabilir. Bu MySQL çoğaltma teknolojisinin sınırlamasıdır. Daha fazla bilgi [MySQL başvuru belgeleri](https://dev.mysql.com/doc/refman/5.7/en/replication-features-memory.html) daha fazla bilgi için.
- Ayarlama [ `innodb_file_per_table` ](https://dev.mysql.com/doc/refman/5.7/en/innodb-multiple-tablespaces.html) parametresi bir ana sunucuya bir çoğaltma sunucusu oluşturma, çoğaltma eşitlenmemiş hale neden olabilir. Çoğaltma sunucusu farklı açabilmek uyumlu değildir.
- Ana sunucu tabloların birincil anahtarlara sahip olun. Birincil anahtarlar eksikliği ana ile çoğaltmalar arasındaki çoğaltma gecikmesine neden olabilir.
- MySQL çoğaltma sınırlamaları tam listesini gözden geçirin [MySQL belgeleri](https://dev.mysql.com/doc/refman/5.7/en/replication-features.html)


## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [oluşturma ve salt okunur çoğaltmalar Azure portalını kullanarak yönetme](howto-read-replicas-portal.md)
- Bilgi edinmek için nasıl [oluşturma ve salt okunur çoğaltmalar Azure CLI kullanarak yönetme](howto-read-replicas-cli.md)
