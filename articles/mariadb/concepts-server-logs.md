---
title: MariaDB için Azure veritabanı sunucusu günlükleri
description: MariaDB ve farklı günlüğe kaydetme düzeylerini etkinleştirmek için kullanılabilir parametreleri için Azure veritabanı'nda kullanılabilir günlükleri açıklar.
author: rachel-msft
ms.author: raagyema
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: a26f61eb199d8f370e1a9dd010932dc868b74ae4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61041267"
---
# <a name="server-logs-in-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı'nda sunucu günlüklerini
MariaDB için Azure veritabanı'nda yavaş sorgu günlüğü kullanıcılar tarafından kullanılabilir. İşlem günlüğü erişimi desteklenmiyor. Yavaş sorgu günlüğü, sorun giderme için performans sorunlarını tanımlamak için kullanılabilir.

Yavaş sorgu günlüğü hakkında daha fazla bilgi için bkz: MariaDB belgelerine [yavaş sorgu günlüğü](https://mariadb.com/kb/en/library/slow-query-log-overview/).

## <a name="access-server-logs"></a>Sunucu günlüklerine erişme
Liste ve Azure veritabanı, Azure portalı ve Azure CLI kullanarak MariaDB için sunucu günlüklerine indirin.

Azure portalında MariaDB için Azure veritabanı sunucunuza seçin. Altında **izleme** başlığı seçin **sunucu günlükleri** sayfası.

<!-- For more information on Azure CLI, see [Configure and access server logs using Azure CLI](howto-configure-server-logs-in-cli.md).-->

## <a name="log-retention"></a>Günlük tutma
Günlükleri, bunların oluşturma yedi güne kadar kullanılabilir. Kullanılabilir günlükleri toplam boyutu 7 GB aşarsa alanı olana kadar eski dosyalar silinir.

Günlükleri Döndürülmüş her 24 saat veya 7 GB, hangisi gelir önce.

## <a name="configure-logging"></a>Günlük tutmayı yapılandırma
Yavaş sorgu günlüğü varsayılan olarak devre dışıdır. Bunu etkinleştirmek için slow_query_log açık olarak ayarlayın.

Ayarlayabileceğiniz diğer parametreler şunlardır:

- **long_query_time**: sorgu (saniye cinsinden) long_query_time uzun sürerse, sorgu günlüğe kaydedilir. Varsayılan değer 10 saniyedir.
- **log_slow_admin_statements**: ON slow_query_log için yazılan deyimlerinde ALTER_TABLE ve ANALYZE_TABLE gibi yönetim deyimleri içeriyorsa.
- **log_queries_not_using_indexes**: dizinleri kullanmayan sorgular için slow_query_log kaydedilip kaydedilmeyeceğini belirler
- **log_throttle_queries_not_using_indexes**: Bu parametre için yavaş sorgu günlüğü yazılabilir olmayan dizin sorguların sayısını sınırlar. Log_queries_not_using_indexes ON olarak ayarlandığında bu parametre etkinleşir.

MariaDB bkz [yavaş sorgu günlüğü belgeleri](https://mariadb.com/kb/en/library/slow-query-log-overview/) yavaş sorgu günlüğü parametrelerini tam açıklamaları için.

## <a name="next-steps"></a>Sonraki Adımlar
- [Sunucu günlükleri, Azure portalından erişmek ve yapılandırma](howto-configure-server-logs-portal.md).
