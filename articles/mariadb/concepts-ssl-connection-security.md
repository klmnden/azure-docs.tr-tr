---
title: MariaDB için Azure veritabanı için SSL bağlantısı
description: MariaDB ve SSL bağlantıları düzgün bir şekilde kullanmak için ilişkili uygulamalar için Azure veritabanı'nı yapılandırma hakkında bilgi
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 1d0b27a8fd7e3882a73624fa1b668ac602a85e6b
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48249511"
---
# <a name="ssl-connectivity-in-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı'nda SSL bağlantısı
MariaDB için Azure veritabanı, Güvenli Yuva Katmanı (SSL) kullanan istemci uygulamalar için veritabanı sunucunuza bağlanmayı destekler. Veritabanı sunucunuzla istemci uygulamalarınız arasında SSL bağlantılarının zorunlu tutulması, sunucuya uygulamanız arasındaki veri akışını şifreleyerek "bağlantıyı izinsiz izleme" saldırılarına karşı korumaya yardımcı olur.

## <a name="default-settings"></a>Varsayılan ayarlar
MariaDB için bağlanırken SSL bağlantılarını zorunlu tutmak için varsayılan olarak, veritabanı hizmetinin yapılandırılması gerekir.  Mümkün olduğunda SSL seçeneğini devre dışı bırakılmasından kaçınmak için öneririz.

Azure portalı üzerinden MariaDB sunucu için yeni bir Azure veritabanı ve CLI sağlanırken zorlama SSL bağlantıları, varsayılan olarak etkindir.

Çeşitli programlama dilleri için bağlantı dizelerini, Azure portalında gösterilir. Bu bağlantı dizeleri, veritabanınıza bağlanmak için gereken SSL parametrelerini içerir. Azure portalında sunucunuzu seçin. Altında **ayarları** başlığı seçin **bağlantı dizeleri**. SSL parametresi örneğin bağlayıcı göre değişir "ssl = true" veya "sslmode = gerektirir" veya "sslmode = gerekli" yükleyeceklerdir.

Etkinleştirmek veya uygulama geliştirirken SSL bağlantısı devre dışı bırakma hakkında bilgi edinmek için bkz [SSL yapılandırma](howto-configure-ssl.md).

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [sunucu güvenlik duvarı kuralları](concepts-firewall-rules.md)
- Bilgi edinmek için nasıl [SSL'yi yapılandırma](howto-configure-ssl.md).
