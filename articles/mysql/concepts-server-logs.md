---
title: "Azure veritabanı için MySQL için sunucu günlüklerine | Microsoft Docs"
description: "MySQL ve farklı günlük düzeylerini etkinleştirmek için kullanılabilir parametreler için Azure veritabanı'nda kullanılabilir günlük açıklar."
services: mysql
author: rachel-msft
ms.author: raagyema
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 10/18/2017
ms.openlocfilehash: 13b30df82c1a6c4c45a621a1f7a40148a55a7648
ms.sourcegitcommit: b723436807176e17e54f226fe00e7e977aba36d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2017
---
# <a name="server-logs-in-azure-database-for-mysql"></a>Sunucu, Azure veritabanı için MySQL kaydeder.
MySQL için Azure veritabanı'nda yavaş sorgu günlüğü kullanıcılar tarafından kullanılabilir. İşlem günlüğü erişimi desteklenmiyor. Yavaş sorgu günlüğü sorun giderme için performans sorunlarını tanımlamak için kullanılabilir. 

MySQL başvuru el ile 's MySQL yavaş sorgu günlüğü hakkında daha fazla bilgi için bkz: [yavaş sorgu günlük bölümü](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html).

## <a name="access-server-logs"></a>Sunucu günlüklerine erişme
Liste ve Azure veritabanı için MySQL server günlüklerini Azure portalı ve Azure CLI kullanarak yükleyin.

Azure portalında Azure veritabanınızı MySQL sunucusu için seçin. Altında **izleme** başlığını seçin **sunucu günlükleri** sayfası.

Azure CLI hakkında daha fazla bilgi için bkz: [ve erişimi Yapılandır sunucu günlüklerini Azure CLI kullanarak](howto-configure-server-logs-in-cli.md).

## <a name="log-retention"></a>Günlük tutma
Günlükleri, yedi gündür kendi oluşturma. 7.5 GB kullanılabilir günlüklerini toplam boyutunu aşarsa, alan kullanılabilir oluncaya kadar eski dosyalar silinir. 

Günlükleri Döndürülmüş her 24 saat veya 7.5 GB, hangisi gelir ilk.


## <a name="configure-logging"></a>Günlük tutmayı yapılandırma 
Yavaş sorgu günlüğü varsayılan olarak devre dışıdır. Etkinleştirmek için slow_query_log açık olarak ayarlanmış.

Ayarlayabileceğiniz diğer parametreler şunları içerir:

- **long_query_time**: (saniye cinsinden) bir sorgu long_query_time uzun sürerse, bu sorguyu günlüğe kaydedilir. Varsayılan değer 10 saniyedir.
- **log_slow_admin_statements**: ON slow_query_log yazılmış deyimlerinde ALTER_TABLE ve ANALYZE_TABLE gibi yönetim deyimleri içeriyorsa.
- **log_queries_not_using_indexes**: dizinleri kullanmayın sorguları slow_query_log günlüğe kaydedilip kaydedilmeyeceğini belirler
- **log_throttle_queries_not_using_indexes**: Bu parametre yavaş sorgu günlüğüne yazılan dizini olmayan sorguları sayısını sınırlar. Bu parametre, log_queries_not_using_indexes ON olarak ayarlandığında etkinleşir.

MySQL bkz [yavaş sorgu günlüğü belgelerine](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html) yavaş sorgu günlüğü parametrelerini tam açıklamaları için.

## <a name="next-steps"></a>Sonraki Adımlar
- [Nasıl yapılandırılacağı ve sunucu günlüklerini Azure CLI üzerinden erişim](howto-configure-server-logs-in-cli.md).
