---
title: -Tek bir sunucu PostgreSQL için Azure veritabanı çoğaltmalarını okuyun
description: Bu makalede, PostgreSQL - tek bir sunucu için Azure veritabanı'nda okuma çoğaltma özelliği açıklanır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 13580289144d798a57e636f15ab5bce629ff3572
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66242288"
---
# <a name="read-replicas-in-azure-database-for-postgresql---single-server"></a>-Tek bir sunucu PostgreSQL için Azure veritabanı çoğaltmalarını okuyun

Okuma çoğaltması özelliği salt okunur bir sunucuya bir PostgreSQL sunucusu için Azure veritabanı'ndan veri çoğaltmanıza olanak sağlar. En fazla beş çoğaltmalar için ana sunucu ile çoğaltma yapabilirsiniz. Çoğaltmalar, PostgreSQL altyapısı yerel çoğaltma teknolojisiyle zaman uyumsuz olarak güncelleştirilir.

> [!IMPORTANT]
> Salt okunur bir çoğaltması, ana sunucunuz ile aynı bölgede ya da diğer Azure bölgesinde, tercih ettiğiniz oluşturabilirsiniz. Bölgeler arası çoğaltma şu anda genel Önizleme aşamasındadır.

Çoğaltmalar, PostgreSQL sunucuları için benzer normal için Azure veritabanı'nı yönetme yeni sunucularıdır. Her yineleme okumak için sanal Çekirdeklerde sağlanan işlem ve depolama GB için faturalandırılırsınız / ay.

Bilgi edinmek için nasıl [oluşturma ve yinelemeleri yönetme](howto-read-replicas-portal.md).

## <a name="when-to-use-a-read-replica"></a>Salt okunur bir çoğaltması kullanmak üzere ne zaman
Salt okunur çoğaltma özelliği okuma açısından yoğun iş yükleri, ölçek ve performans artırmaya yardımcı olur. Yazma iş yüklerinin asıl yönlendirilebilir okuma iş yükleri için çoğaltmalar, yalıtılmış olabilir.

Sık karşılaşılan bir senaryodur BI sahip olmaktır ve analiz iş yükleri okuma çoğaltması raporlama için veri kaynağı olarak kullanın.

Çoğaltmaların salt okunur olduğundan, doğrudan asıl kapasite yazma yüklerini azaltmak yok. Bu özellik, yazma yoğunluklu iş yükleri hedeflenen değil.

PostgreSQL zaman uyumsuz çoğaltma okuma çoğaltması özelliğini kullanır. Bu özellik, zaman uyumlu çoğaltma senaryoları için tasarlanmamıştır. Ana ve çoğaltma arasında ölçülebilir bir gecikme olur. Veri çoğaltma sonunda asıl verilerle tutarlı hale gelir. Bu gecikme uyum iş yükleri için bu özelliği kullanın.

Okuma çoğaltma, olağanüstü durum kurtarma planınızı geliştirebilirsiniz. İlk yöneticisinden farklı bir Azure bölgesinde bir çoğaltma olması gerekir. Bir bölge olağanüstü durum varsa, bu çoğaltma için çoğaltma durdurma ve iş yükünüz yönlendirin. Çoğaltma durdurma yazma kabul başlamak çoğaltmanın sağlar, hem de okur. Daha fazla bilgi [çoğaltma durdurma](#stop-replication) bölümü. 

## <a name="create-a-replica"></a>Çoğaltma oluşturma
Ana sunucu olmalıdır `azure.replication_support` parametresini **çoğaltma**. Bu parametre değiştiğinde, değişikliğin etkili olması için sunucunun yeniden başlatılması gereklidir. ( `azure.replication_support` Parametresi yalnızca için genel amaçlı ve bellek için iyileştirilmiş katmanlar için geçerlidir).

PostgreSQL sunucusu için boş bir Azure veritabanı oluşturma çoğaltma iş akışı başlattığınızda oluşturulur. Yeni sunucunun ana sunucuya olan verilerle doldurulur. Oluşturma zamanı asıl ve haftalık tam yedekleme saatinden veri miktarına bağlıdır. Süre birkaç dakikadan birkaç saate kadar değişebilir.

Her çoğaltma için depolama etkin [otomatik büyütme](concepts-pricing-tiers.md#storage-auto-grow). Auto-grow özelliği için yinelenen verileri takip edin ve bir sonu dışında depolama hataları nedeniyle çoğaltma önlemek için çoğaltmayı sağlar.

PostgreSQL fiziksel çoğaltma, çoğaltma olmayan mantıksal okuma çoğaltması özelliğini kullanır. Çoğaltma, çoğaltma yuvaları kullanarak akış, varsayılan işlem modu kullanılır. Gerektiğinde, günlük aktarma bilgi edinmek için kullanılır.

Bilgi edinmek için nasıl [salt okunur bir çoğaltması Azure Portalı'nda oluşturma](howto-read-replicas-portal.md).

## <a name="connect-to-a-replica"></a>Bir kopyaya bağlanın
Bir çoğaltma oluşturduğunuzda, sanal ağ hizmet uç noktası ana sunucu ve güvenlik duvarı kuralları devralmaz. Bu kurallar, çoğaltma için bağımsız olarak ayarlanmalıdır.

Çoğaltma yönetici hesabı, ana sunucudan devralır. Tüm kullanıcı hesapları ana sunucu üzerinde salt okunur kopyaya çoğaltılır. Salt okunur bir çoğaltması için yalnızca ana sunucu üzerinde kullanılabilir olan kullanıcı hesaplarını kullanarak da bağlanabilirsiniz.

PostgreSQL sunucusu için normal bir Azure veritabanında olduğu gibi kendi ana bilgisayar adı ve geçerli kullanıcı hesabı kullanarak çoğaltmaya bağlanabilirsiniz. Adlı bir sunucu için **kopyamı** yönetici kullanıcı adı ile **myadmin**, çoğaltmaya psql kullanarak bağlanabilirsiniz:

```
psql -h myreplica.postgres.database.azure.com -U myadmin@myreplica -d postgres
```

İstemde, kullanıcı hesabı için parolayı girin.

## <a name="monitor-replication"></a>İzleyici çoğaltma
PostgreSQL için Azure veritabanı tarafından sağlanan **arasında en fazla gecikme çoğaltmaları** ölçüm Azure İzleyici'de. Bu ölçüm yalnızca ana sunucu üzerinde kullanılabilir. Ölçüm lag ana çoğu İzolasyonu çoğaltma arasındaki bayt cinsinden gösterir. 

PostgreSQL için Azure veritabanı'nı da sağlar **çoğaltma gecikmesi** ölçüm Azure İzleyici'de. Bu ölçüm yalnızca çoğaltmalar için kullanılabilir. 

Ölçüm hesaplandığı `pg_stat_wal_receiver` görüntüle:

```SQL
EXTRACT (EPOCH FROM now() - pg_last_xact_replay_timestamp());
```

Çoğaltma gecikmesi ölçüm yeniden yürütülmüş işlemin son daraltılmasından gösterir. Ölçüm, ana sunucunuz üzerinde gerçekleşen işlem varsa, bu zaman gecikmesini yansıtır.

Çoğaltma gecikmesi, iş yükü için kabul edilebilir olmayan bir değer ulaştığında bildirmek için uyarı ayarlama. 

Hakkındaki ek bilgiler için ana sunucunun doğrudan bayt tüm çoğaltmaları üzerindeki çoğaltma gecikmesi'nin süresi almak için sorgulayın.

PostgreSQL sürümü 10'da:

```SQL
select pg_wal_lsn_diff(pg_current_wal_lsn(), stat.replay_lsn) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

PostgreSQL sürümü 9.6 ve önceki sürümleri:

```SQL
select pg_xlog_location_diff(pg_current_xlog_location(), stat.replay_location) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

> [!NOTE]
> Bir ana sunucu veya okuma çoğaltması yeniden başlatılırsa, yeniden başlatın ve güncel duruma gelmesi için gereken süreyi çoğaltma gecikmesi ölçümü yansıtılır.

## <a name="stop-replication"></a>Çoğaltmayı Durdur
Bir ana ve çoğaltma arasında çoğaltmayı durdurabilirsiniz. Durdurma eylemi, çoğaltmayı yeniden başlatın ve çoğaltma ayarlarını kaldırmak için neden olur. Ana sunucu ile bir salt okunur çoğaltma arasında çoğaltmayı durdurulduktan sonra çoğaltma, tek başına sunucu haline gelir. Tek başına sunucu verileri çoğaltma durdurma komutunun başlatılmasından çoğaltma üzerinde kullanılabilir olan verilerdir. Tek başına sunucu ana sunucu ile Kaçırdığınız değil.

> [!IMPORTANT]
> Tek başına sunucu ile bir çoğaltma yeniden yapılamıyor.
> Salt okunur bir çoğaltma üzerinde çoğaltma durdurmadan önce çoğaltma gerektiren tüm verilere sahip olun.

Çoğaltma durdurduğunuzda, çoğaltma önceki ana ve diğer yinelemeler için tüm bağlantılar kaybeder. Hiçbir otomatik Yük Devretme Yöneticisi ve çoğaltma arasında yoktur. 

Bilgi edinmek için nasıl [bir çoğaltma için çoğaltma durdurma](howto-read-replicas-portal.md).


## <a name="considerations"></a>Dikkat edilmesi gerekenler

Bu bölümde, salt okunur çoğaltma özelliği hakkında dikkat edilecek noktalar özetlenmektedir.

### <a name="prerequisites"></a>Önkoşullar
Salt okunur bir çoğaltması oluşturmadan önce `azure.replication_support` parametresi ayarlanmalıdır **çoğaltma** ana sunucu üzerinde. Bu parametre değiştiğinde, değişikliğin etkili olması için sunucunun yeniden başlatılması gereklidir. `azure.replication_support` Parametresi yalnızca için genel amaçlı ve bellek için iyileştirilmiş katmanlar için geçerlidir.

### <a name="new-replicas"></a>Yeni yineleme
PostgreSQL sunucusu için yeni bir Azure veritabanı salt okunur bir çoğaltması oluşturulur. Mevcut bir sunucu ile bir çoğaltma yapılamaz. Bir kopyasını başka bir okuma çoğaltması oluşturulamıyor.

### <a name="replica-configuration"></a>Çoğaltma yapılandırması
Çoğaltma Yöneticisi olarak aynı sunucu yapılandırmasını kullanarak oluşturulur. Bir çoğaltma oluşturulduktan sonra birkaç ayar bağımsız olarak ana sunucu ile değiştirilebilir: işlem oluşturma, sanal çekirdek, depolama ve yedekleme bekletme süresi. Fiyatlandırma katmanı da ayrı ayrı değiştirilebilir ya da temel katmandan hariç.

> [!IMPORTANT]
> Bir ana sunucu yapılandırması için yeni değerleri güncelleştirilmeden önce çoğaltma yapılandırması eşit veya daha fazla değerlerle güncelleştirin. Bu eylem, çoğaltma ana dala yapılan değişiklikler ile koruyabilirsiniz sağlar.

PostgreSQL gerektirir değerini `max_connections` olmaz parametresi değerinden büyük veya ana değerine eşit; tersi durumda okuma çoğaltması çoğaltmayı Başlat. PostgreSQL için Azure veritabanı'nda `max_connections` parametre değeri, SKU üzerinde dayanır. Daha fazla bilgi için [sınırları PostgreSQL için Azure veritabanı'nda](concepts-limits.md). 

Sunucu değerleri güncelleştirmek üzere deneyin ancak sınırlara yoksa, bir hata alırsınız.

### <a name="stopped-replicas"></a>Durdurulan çoğaltmalar
Bir ana sunucu ve bir salt okunur çoğaltma arasında çoğaltmayı durdurursanız, çoğaltma değişikliği uygulamak için yeniden başlatır. Durdurulan çoğaltma hem okuma hem de yazma işlemleri kabul eden bir tek başına sunucu haline gelir. Tek başına sunucu ile bir çoğaltma yeniden yapılamıyor.

### <a name="deleted-master-and-standalone-servers"></a>Silinen Yöneticisi ve tek başına sunucular
Ana sunucu silindiğinde, tüm okuma çoğaltmalarını tek başına sunucu haline gelir. Çoğaltmalar, bu değişikliği yansıtacak şekilde yeniden başlatılır.

## <a name="next-steps"></a>Sonraki adımlar
Bilgi edinmek için nasıl [oluşturmak ve salt okunur çoğaltmalar Azure portalında yönetmek](howto-read-replicas-portal.md).
