---
title: PostgreSQL için Azure veritabanı'nda sunucu günlüklerini
description: Bu makalede, PostgreSQL, sorgu ve Hata günlüklerini oluşturur ve saklama nasıl oturum için Azure veritabanı nasıl yapılandırıldığını açıklar.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: bcca8ce8d11482dd8517992297b7e8a5b94ac8b1
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37435499"
---
# <a name="server-logs-in-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı'nda sunucu günlüklerini 
PostgreSQL için Azure veritabanı oluşturur, sorgu ve hata günlükleri. Ancak, işlem günlükleri erişimi desteklenmiyor. Sorgu ve Hata günlüklerini belirlemek, sorun giderme ve yapılandırma hatalarını ve performansın onarmak için kullanılabilir. Daha fazla bilgi için [hata bildirimi ve günlüğe kaydetme](https://www.postgresql.org/docs/9.6/static/runtime-config-logging.html).

## <a name="access-server-logs"></a>Sunucu günlüklerine erişme
Liste ve Azure portalını kullanarak Azure PostgreSQL sunucusu hata günlüklerini indir [Azure CLI](howto-configure-server-logs-using-cli.md)ve Azure REST API'leri.

## <a name="log-retention"></a>Günlük tutma
Sistem günlüklerini kullanma saklama süresini ayarlayabilirsiniz **günlük\_bekletme\_süresi** sunucunuzla ilişkili parametre. Bu parametre için gün birimidir. Varsayılan değer 3 gündür. En yüksek değer 7 gündür. Sunucunuz, yeterli olmalıdır saklanan günlük dosyalarını içerecek şekilde depolama ayrılmış.
Günlük dosyaları, her bir saat veya 100 MB boyutunda döndürme, hangisinin önce geldiğine.

## <a name="configure-logging-for-azure-postgresql-server"></a>Azure PostgreSQL sunucusu için günlüğe kaydetmeyi yapılandırma
Sorgu günlüğü ve hata günlüğünü için sunucunuzun etkinleştirebilirsiniz. Hata günlüklerini otomatik elektrikli, bağlantı ve kontrol noktaları bilgiler içerebilir.

İki sunucu parametreleri ayarlayarak PostgreSQL veritabanı Örneğiniz için sorgu günlük kaydını etkinleştirebilirsiniz: `log_statement` ve `log_min_duration_statement`.

**Günlük\_deyimi** parametre denetler hangi SQL deyimleri kaydedilir. Bu parametre ayarını öneririz ***tüm*** tüm deyimler; oturum için varsayılan değer, Yok'tur.

**Günlük\_min\_süresi\_deyimi** parametre sınırı deyiminin günlüğe kaydedilecek milisaniye cinsinden ayarlar. Parametre ayarından daha uzun çalışması tüm SQL deyimleri günlüğe kaydedilir. Bu parametre devre dışı bırakıldı ve varsayılan olarak 1 (-1) ayarlayın. Bu parametre etkinleştirme uygulamalarınızı iyileştirilmemiş sorgularda aşağı izlemek için yararlı olabilir.

**Günlük\_min\_iletileri** sunucusu günlüğe yazılan düzeyleri ileti denetimi size verir. Varsayılan, uyarı örneğidir. 

Bu ayarlar hakkında daha fazla bilgi için bkz. [hata bildirimi ve günlüğe kaydetme](https://www.postgresql.org/docs/9.6/static/runtime-config-logging.html) belgeleri. Azure veritabanı için PostgreSQL sunucusu parametrelerini özellikle yapılandırmak için bkz: [Azure CLI kullanarak sunucu yapılandırma parametrelerini özelleştirme](howto-configure-server-parameters-using-cli.md).

## <a name="next-steps"></a>Sonraki adımlar
- Azure CLI komut satırı arabirimi kullanarak günlüklerine erişmek için bkz: [Azure CLI kullanarak sunucu günlükleri ve erişimi Yapılandır](howto-configure-server-logs-using-cli.md).
- Sunucu parametreleri hakkında daha fazla bilgi için bkz. [Azure CLI kullanarak sunucu yapılandırma parametrelerini özelleştirme](howto-configure-server-parameters-using-cli.md).
