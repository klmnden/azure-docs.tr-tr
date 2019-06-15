---
title: MariaDB için Azure veritabanı'nda okunur çoğaltmalar
description: Bu makalede, salt okunur çoğaltmalar için MariaDB için Azure veritabanı açıklanır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 06/10/2019
ms.openlocfilehash: 8abe257090b5159053a37350c9e24cc27073679b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079372"
---
# <a name="read-replicas-in-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı'nda okunur çoğaltmalar

Salt okunur çoğaltma özelliği, verileri MariaDB server için Azure veritabanı salt okunur bir sunucuya çoğaltma olanak sağlar. Ana aynı bölgede en fazla beş çoğaltmalarına, ana sunucu ile çoğaltma yapabilirsiniz. Çoğaltmalar zaman uyumsuz olarak MariaDB altyapının (binlog) ikili günlük dosyası konumu tabanlı çoğaltma teknoloji genel işlem kimliği (GTID) kullanılarak güncelleştirilir. Binlog çoğaltma hakkında daha fazla bilgi için bkz: [binlog Çoğaltmaya genel bakış](https://mariadb.com/kb/en/library/replication-overview/).

> [!IMPORTANT]
> Aynı bölge çoğaltma şu anda genel önizlemede okuyun.

Çoğaltmalar, MariaDB sunucuları için benzer normal için Azure veritabanı'nı yönetme yeni sunucularıdır. Her yineleme okumak için sanal Çekirdeklerde sağlanan işlem ve depolama GB için faturalandırılırsınız / ay.

GTID çoğaltma hakkında daha fazla bilgi için bkz. [MariaDB çoğaltma belgeleri](https://mariadb.com/kb/en/library/gtid/).

## <a name="when-to-use-a-read-replica"></a>Salt okunur bir çoğaltması kullanmak üzere ne zaman

Salt okunur çoğaltma özelliği okuma açısından yoğun iş yükleri, ölçek ve performans artırmaya yardımcı olur. Yazma iş yüklerinin asıl yönlendirilebilir okuma iş yükleri için çoğaltmalar, yalıtılmış olabilir.

Sık karşılaşılan bir senaryodur BI sahip olmaktır ve analiz iş yükleri okuma çoğaltması raporlama için veri kaynağı olarak kullanın.

Çoğaltmaların salt okunur olduğundan, doğrudan asıl kapasite yazma yüklerini azaltmak yok. Bu özellik, yazma yoğunluklu iş yükleri hedeflenen değil.

Zaman uyumsuz çoğaltma okuma çoğaltması özelliğini kullanır. Bu özellik, zaman uyumlu çoğaltma senaryoları için tasarlanmamıştır. Ana ve çoğaltma arasında ölçülebilir bir gecikme olur. Veri çoğaltma sonunda asıl verilerle tutarlı hale gelir. Bu gecikme uyum iş yükleri için bu özelliği kullanın.

Okuma çoğaltma, olağanüstü durum kurtarma planınızı geliştirebilirsiniz. Bölgesel bir olağanüstü durum yoktur ve ana sunucunuz yoksa, başka bir bölgede bir yineleme için iş yükünüzü yönlendirebilir. Bunu yapmak için ilk çoğaltma durdurma çoğaltma işlevini kullanarak yazma kabul olanak tanır. Ardından, bağlantı dizesini güncelleştirerek uygulamanızı yönlendirebilirsiniz. Daha fazla bilgi [çoğaltma durdurma](#stop-replication) bölümü.

## <a name="create-a-replica"></a>Çoğaltma oluşturma

Ana sunucu mevcut hiçbir çoğaltma sunucuları varsa, ana ilk çoğaltma için hazırlanması için yeniden başlatılır.

MariaDB için boş bir Azure veritabanı oluşturma çoğaltma iş akışı başlattığınızda oluşturulur. Yeni sunucunun ana sunucuya olan verilerle doldurulur. Oluşturma zamanı asıl ve haftalık tam yedekleme saatinden veri miktarına bağlıdır. Süre birkaç dakikadan birkaç saate kadar değişebilir.

> [!NOTE]
> Depolama uyarı kümesi sunucularınızda yoksa, bunu yapmanız önerilir. Uyarı ne zaman bir sunucu çoğaltma etkiler, depolama sınırına yaklaşıyor size bildirir.

Bilgi edinmek için nasıl [salt okunur bir çoğaltması Azure Portalı'nda oluşturma](howto-read-replicas-portal.md).

## <a name="connect-to-a-replica"></a>Bir kopyaya bağlanın

Bir çoğaltma oluşturduğunuzda, sanal ağ hizmet uç noktası ana sunucu ve güvenlik duvarı kuralları devralmaz. Bu kurallar, çoğaltma için bağımsız olarak ayarlanmalıdır.

Çoğaltma yönetici hesabı, ana sunucudan devralır. Tüm kullanıcı hesapları ana sunucu üzerinde salt okunur kopyaya çoğaltılır. Salt okunur bir çoğaltması için yalnızca ana sunucu üzerinde kullanılabilir olan kullanıcı hesaplarını kullanarak da bağlanabilirsiniz.

MariaDB için normal bir Azure veritabanı üzerinde yaptığınız gibi kendi ana bilgisayar adı ve geçerli kullanıcı hesabı kullanarak çoğaltmaya bağlanabilirsiniz. Adlı bir sunucu için **myreplica** yönetici kullanıcı adı ile **myadmin**, çoğaltma için mysql CLI kullanarak bağlanabilirsiniz:

```bash
mysql -h myreplica.mariadb.database.azure.com -u myadmin@myreplica -p
```

İstemde, kullanıcı hesabı için parolayı girin.

## <a name="monitor-replication"></a>İzleyici çoğaltma

MariaDB için Azure veritabanı tarafından sağlanan **çoğaltma bekleme süresini saniye cinsinden** ölçüm Azure İzleyici'de. Bu ölçüm yalnızca çoğaltmalar için kullanılabilir.

Bu ölçüm kullanılarak hesaplanır `seconds_behind_master` Mariadb'nin içinde kullanılabilir ölçüm `SHOW SLAVE STATUS` komutu.

Çoğaltma gecikmesi, iş yükü için kabul edilebilir olmayan bir değer ulaştığında bildirmek için uyarı ayarlama.

## <a name="stop-replication"></a>Çoğaltmayı Durdur

Bir ana ve çoğaltma arasında çoğaltmayı durdurabilirsiniz. Ana sunucu ile bir salt okunur çoğaltma arasında çoğaltmayı durdurulduktan sonra çoğaltma, tek başına sunucu haline gelir. Tek başına sunucu verileri çoğaltma durdurma komutunun başlatılmasından çoğaltma üzerinde kullanılabilir olan verilerdir. Tek başına sunucu ana sunucu ile Kaçırdığınız değil.

Bir çoğaltma için çoğaltma durdurma seçtiğinizde, önceki ana ve diğer yinelemeler için tüm bağlantılar kaybeder. Hiçbir otomatik yük devretme çoğaltması arasındaki asıl yoktur.

> [!IMPORTANT]
> Tek başına sunucu ile bir çoğaltma yeniden yapılamıyor.
> Salt okunur bir çoğaltma üzerinde çoğaltma durdurmadan önce çoğaltma gerektiren tüm verilere sahip olun.

Bilgi edinmek için nasıl [bir çoğaltma için çoğaltma durdurma](howto-read-replicas-portal.md).

## <a name="considerations-and-limitations"></a>Önemli noktalar ve sınırlamalar

### <a name="pricing-tiers"></a>Fiyatlandırma katmanları

Okuma çoğaltma şu anda yalnızca genel amaçlı ve bellek için iyileştirilmiş fiyatlandırma katmanlarında kullanılabilir.

### <a name="master-server-restart"></a>Ana sunucunun yeniden başlatılması

Bir çoğaltma yok mevcut çoğaltmaları olan bir şablonu oluşturduğunuzda, ana ilk çoğaltma için hazırlanması için yeniden başlatılır. Lütfen bu dikkate alın ve yoğun olmayan bir dönem boyunca bu işlemleri gerçekleştirin.

### <a name="new-replicas"></a>Yeni yineleme

MariaDB sunucu için yeni bir Azure veritabanı salt okunur bir çoğaltması oluşturulur. Mevcut bir sunucu ile bir çoğaltma yapılamaz. Bir kopyasını başka bir okuma çoğaltması oluşturulamıyor.

### <a name="replica-configuration"></a>Çoğaltma yapılandırması

Çoğaltma Yöneticisi olarak aynı sunucu yapılandırmasını kullanarak oluşturulur. Bir çoğaltma oluşturulduktan sonra birkaç ayar bağımsız olarak ana sunucu ile değiştirilebilir: işlem oluşturma, sanal çekirdek, depolama, yedekleme bekletme süresi ve MariaDB altyapı sürümü. Fiyatlandırma katmanı da ayrı ayrı değiştirilebilir ya da temel katmandan hariç.

> [!IMPORTANT]
> Bir ana sunucu yapılandırması için yeni değerleri güncelleştirilmeden önce çoğaltma yapılandırması eşit veya daha fazla değerlerle güncelleştirin. Bu eylem, çoğaltma ana dala yapılan değişiklikler ile koruyabilirsiniz sağlar.

### <a name="stopped-replicas"></a>Durdurulan çoğaltmalar

Bir ana sunucu ve bir salt okunur çoğaltma arasında çoğaltmayı durdurursanız, durdurulmuş çoğaltma hem okuma hem de yazma işlemleri kabul eden bir tek başına sunucu haline gelir. Tek başına sunucu ile bir çoğaltma yeniden yapılamıyor.

### <a name="deleted-master-and-standalone-servers"></a>Silinen Yöneticisi ve tek başına sunucular

Ana sunucu silindiğinde, tüm salt okunur çoğaltmalar için çoğaltma durdurulur. Bu çoğaltmaların tek başına sunucuları olur. Ana sunucu silinir.

### <a name="user-accounts"></a>Kullanıcı hesapları

Ana sunucu üzerinde kullanıcılar, salt okunur kopyaya çoğaltılır. Yalnızca ana sunucu üzerinde kullanılabilir olan kullanıcı hesaplarını kullanarak bir okuma çoğaltması bağlanabilirsiniz.

### <a name="server-parameters"></a>Sunucu parametreleri

Veri eşitlenmemiş hale gelmesini önlemek ve olası veri kaybı veya bozulması önlemek için bazı sunucu parametreleri kullanarak çoğaltmaları okuduğunuzda güncelleştirilmiş kilitlenir.

Aşağıdaki sunucu parametreleri hem ana hem de çoğaltma sunucularında kilitli:
- [`innodb_file_per_table`](https://mariadb.com/kb/en/library/innodb-system-variables/#innodb_file_per_table) 
- [`log_bin_trust_function_creators`](https://mariadb.com/kb/en/library/replication-and-binary-log-system-variables/#log_bin_trust_function_creators)

[ `event_scheduler` ](https://mariadb.com/kb/en/library/server-system-variables/#event_scheduler) Parametresi ile çoğaltma sunucularında kilitlendi.

### <a name="other"></a>Diğer

- Bir çoğaltma bir kopyasını oluşturma desteklenmiyor.
- Bellek içi tablolar çoğaltmalar eşitlenmemiş hale neden olabilir. MariaDB çoğaltma teknolojisiyle kısıtlamasıdır.
- Ana sunucu tabloların birincil anahtarlara sahip olun. Birincil anahtarlar eksikliği ana ile çoğaltmalar arasındaki çoğaltma gecikmesine neden olabilir.

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [oluşturma ve salt okunur çoğaltmalar Azure portalını kullanarak yönetme](howto-read-replicas-portal.md)
