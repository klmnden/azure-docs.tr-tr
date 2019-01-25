---
title: PostgreSQL için Azure Veritabanı’nda okuma amaçlı çoğaltmalar
description: Bu makalede, PostgreSQL için Azure veritabanı'nda salt okunur çoğaltmalar açıklanır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/23/2019
ms.openlocfilehash: 9270c3290bd7be0bbb79d30aff8becc04dcfc603
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2019
ms.locfileid: "54904021"
---
# <a name="read-replicas-in-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı çoğaltmalarını okuyun

> [!IMPORTANT]
> Salt okunur çoğaltma özelliği genel Önizleme aşamasındadır.

Salt okunur çoğaltma özelliği, bir PostgreSQL sunucusu (ana) için Azure veritabanı'ndan veri çoğaltmak (çoğaltmaları okuma) en fazla beş salt okunur sunuculara aynı Azure bölgesindeki sağlar. Okuma çoğaltmaları PostgreSQL altyapısının yerel çoğaltma teknolojisini kullanarak zaman uyumsuz olarak güncelleştirilir.

Çoğaltmaları benzer şekillerde PostgreSQL sunucuları için Azure veritabanı normal bağımsız olarak yönetilebilir yeni sunucularıdır. Okuma her çoğaltma için sanal Çekirdeklerde sağlanan işlem ve GB cinsinden aylık sağlanan depolama için faturalandırılırsınız.

## <a name="when-to-use-read-replicas"></a>Salt okunur çoğaltmalar kullanıldığı durumlar
Okuma açısından yoğun iş yükleri, ölçek ve performans geliştirilmesine yardımcı olarak okuma çoğaltması özelliğini yöneliktir. Yazma iş yüklerinin asıl yönlendirilebilir örneği için okuma iş yükleri için çoğaltmaları, yalıtılmış olabilir.

Sık karşılaşılan bir senaryodur BI sahip olmaktır ve analiz iş yükleri okuma çoğaltması raporlama için veri kaynağı olarak kullanın.

Çoğaltmaların salt okunur olduğundan, bunlar doğrudan yazma kapasite yüklerini asıl hafifletmek değil ve bu nedenle bu özellik yazma yoğunluklu iş yüklerini hedeflenmiş değil.

Okuma çoğaltması özelliğini PostgreSQL'ın zaman uyumsuz çoğaltma kullanır ve bu nedenle, zaman uyumlu çoğaltma senaryoları için tasarlanmamıştır. Ana ve çoğaltma arasında ölçülebilir bir gecikme olur. Veri çoğaltma üzerindeki ana verilerle birlikte sonunda tutarlı olur. Bu gecikme uyum iş yükleri için bu özelliği kullanın.

## <a name="creating-a-replica"></a>Bir çoğaltma oluşturma
Ana sunucu olmalıdır **azure.replication_support** ÇOĞALTMAYA ayarlayın. Bu parametre değiştirme etkili olması için sunucunun yeniden başlatılmasını gerektirir. (**Azure.replication_support** parametresi yalnızca genel amaçlı ve bellek için iyileştirilmiş katmanlar için geçerlidir).

PostgreSQL sunucusu için boş bir Azure veritabanı oluşturma çoğaltma iş akışı başlatıldığında oluşturulur. Yeni sunucunun ana sunucuya olan verilerle doldurulur. Yeni çoğaltma oluşturmak için gereken süreyi asıl ve haftalık tam yedekleme saatinden veri miktarına bağlıdır. Bu işlem birkaç dakika veya birkaç saat arasında farklılık gösterebilir.

PostgreSQL'ın fiziksel çoğaltma (mantıksal değil çoğaltma), okuma çoğaltması özelliğini kullanır. Akış yuvaları çoğaltma kullanarak çoğaltmayı varsayılan işlemi modudur. Gerektiğinde, günlük aktarma yakalama için kullanılır.

> [!NOTE]
> Depolama uyarı kümesi sunucularınız üzerinde Bu çoğaltma etkiler olduğundan, bir sunucu bulunduğunda bildirmek için depolama sınırına yaklaşıyor bunu yapmanız önerilir değil varsa.

[Azure portalında bir salt okunur çoğaltma oluşturmayı öğrenin](howto-read-replicas-portal.md).

## <a name="connecting-to-a-replica"></a>Bir kopyaya bağlanma
Bir çoğaltma oluşturduğunuzda, sanal ağ hizmet uç noktası ana sunucu ve güvenlik duvarı kuralları devralmaz. Bu kurallar, çoğaltma için bağımsız olarak ayarlanmalıdır.

Çoğaltma, yönetici hesabı, ana sunucudan devralır. Tüm kullanıcı hesapları ana sunucu üzerinde salt okunur kopyaya çoğaltılır. Yalnızca ana sunucu üzerinde kullanılabilir olan kullanıcı hesaplarını kullanarak bir okuma çoğaltması bağlanabilirsiniz.

PostgreSQL sunucusu için normal bir Azure veritabanında olduğu gibi kendi ana bilgisayar adı ve geçerli kullanıcı hesabı kullanarak çoğaltma için bağlanabilirsiniz. Örneği için myreplica sunucunun adıdır ve yönetici kullanıcı adı myadmin ise, ona psql gibi bağlanabilirsiniz:

```
psql -h myreplica.postgres.database.azure.com -U myadmin@myreplica -d postgres
```
ve isteminde kullanıcı hesabının parolasını girin.

## <a name="monitoring-replication"></a>Yineleme izleme
Var. bir **yinelemeler boyunca en fazla gecikme** ölçümü Azure İzleyici'de kullanılabilir. Bu ölçüm yalnızca ana sunucu üzerinde kullanılabilir. Ölçüm ana çoğu İzolasyonu çoğaltma arasındaki gecikme süresini gösterir. 

Ayrıca, bildirimde bir **çoğaltma gecikmesi** ölçüm Azure İzleyici'de. Bu ölçüm yalnızca çoğaltmalar için kullanılabilir. 

Ölçüm pg_stat_wal_receiver görünümden hesaplanır:

```SQL
EXTRACT (EPOCH FROM now() - pg_last_xact_replay_timestamp())
```
Son işlem yeniden olduğundan çoğaltma gecikmesi ölçüm zamanı gösteren unutmayın. Ana şablonunuzu üzerinde gerçekleşen işlem varsa, bu zaman gecikmesini ölçüm yansıtır.

Çoğaltma gecikmesi, iş yükü için kabul edilebilir olmayan bir değer ulaştığında bildiren bir uyarı ayarlamanızı öneririz. 

Hakkındaki ek bilgiler için ana sunucunun doğrudan bayt tüm çoğaltmaları üzerindeki çoğaltma gecikmesi'nin süresi almak için sorgulayın.
PG 10:
```SQL
select pg_wal_lsn_diff(pg_current_wal_lsn(), stat.replay_lsn) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

PG 9.6 ve aşağıda:
```SQL
select pg_xlog_location_diff(pg_current_xlog_location(), stat.replay_location) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

> [!NOTE]
> Bir ana veya çoğaltma sunucusu başlatılırsa, yeniden başlatın ve ardından yakalamak için gereken süreyi çoğaltma gecikmesi ölçümü yansıtılır.

## <a name="stopping-replication-to-a-replica"></a>Bir çoğaltma için çoğaltma durdurma
Bir ana ve çoğaltma arasında çoğaltmayı durdurmak seçebilirsiniz. Bu, çoğaltma, çoğaltma ayarlarını kaldırmak için yeniden başlatmak neden olur. Bir ana çoğaltma sunucusu arasında çoğaltmayı durduruldu sonra tek başına sunucu çoğaltma sunucusu olur. Tek başına sunucu verileri çoğaltma durdurma komutunun başlatıldığı anda çoğaltma üzerinde kullanılabilir olan verilerdir. Bu tek başına sunucu ana sunucu ile yakalamaz.

Bu sunucu bir yinelemeye yeniden yapılamıyor.

Çoğaltma için çoğaltma durdurma önce ihtiyacınız olan tüm verileri olduğundan emin olun.

Yapabilecekleriniz [nasıl yapılır belgelerini yinelemede Durdur öğrenin](howto-read-replicas-portal.md).


## <a name="considerations"></a>Dikkat edilmesi gerekenler

### <a name="preparing-for-replica"></a>Çoğaltma için hazırlama
**Azure.replication_support** ana sunucuya çoğaltma için bir çoğaltma oluşturabilmeniz için önce ayarlanmalıdır. Bu parametre değiştirme etkili olması için sunucunun yeniden başlatılmasını gerektirir. Bu parametre yalnızca genel amaçlı ve bellek için iyileştirilmiş katmanlar için geçerlidir.

### <a name="stopped-replicas"></a>Durdurulan çoğaltmalar
Bir ana ve çoğaltma arasında çoğaltmayı durdurmak seçtiğinizde, çoğaltmanın bu değişiklikleri uygulamak için yeniden başlatılır. Daha sonra bunu bir çoğaltma yeniden yapılamıyor.

### <a name="replicas-are-new-servers"></a>Çoğaltmaları olan yeni sunucular
Çoğaltmalar, PostgreSQL sunucuları için yeni bir Azure veritabanı olarak oluşturulur. Mevcut sunucuları, yinelemeler yapılamaz.

### <a name="replica-server-configuration"></a>Çoğaltma sunucusunu yapılandırma
Çoğaltma sunucusu, aşağıdaki yapılandırmaları içerir asıl aynı sunucu yapılandırmaları kullanılarak oluşturulur:
- Fiyatlandırma katmanı
- İşlem oluşturma
- Sanal çekirdekler
- Depolama
- Yedekleme bekletme süresi
- Fazladan yedek seçeneği
- PostgreSQL altyapısı sürümü

Bir çoğaltma oluşturduktan sonra fiyatlandırma katmanını (temel gelen ve giden hariç), işlem oluşturma, sanal çekirdek, depolama ve yedekleme bekletme süresi değiştirilebilir bağımsız olarak ana sunucu ile.

> [!IMPORTANT]
> Sunucu Yapılandırma Yöneticisi'nin yeni değerlere güncelleştirilmeden önce çoğaltmaları yapılandırma eşit veya daha büyük değerler için güncelleştirilmesi gerekir. Bu, çoğaltmaları ana dala yapılan değişiklikleri takip edin mümkün olmasını sağlar.

Özellikle, aksi takdirde çoğaltma başlatılmaz Yöneticisi'nin değerine eşit veya daha büyük olacak şekilde parametresi max_connections çoğaltma sunucusu değeri Postgres gerektirir. PostgreSQL için Azure veritabanı'nda max_connections değeri SKU'ya bağlı olarak ayarlanır. Daha fazla bilgi için okuma [sınırları doc](concepts-limits.md). 

Bu ihlal eden bir güncelleştirme Bunu yapma girişimi bir hataya yol açacaktır.


### <a name="deleting-the-master"></a>Ana siliniyor
Ana sunucu silindiğinde, tüm salt okunur çoğaltmalar tek başına sunucu haline gelir. Çoğaltmalar, bu değişikliği yansıtacak şekilde yeniden başlatılır.

### <a name="other"></a>Diğer
- Okunur çoğaltmalar yalnızca ana ile aynı Azure bölgesinde oluşturulabilir.
- Bir çoğaltma bir kopyasını oluşturma desteklenmiyor.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure portalında çoğaltmalar oluşturmak ve yönetmek nasıl okuyun](howto-read-replicas-portal.md).
