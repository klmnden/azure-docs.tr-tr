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
ms.openlocfilehash: dda5df58a83ddd3ce42fa887c3c32a3e23954920
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46946655"
---
# <a name="ssl-connectivity-in-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı'nda SSL bağlantısı
MariaDB için Azure veritabanı, Güvenli Yuva Katmanı (SSL) kullanan istemci uygulamalar için veritabanı sunucunuza bağlanmayı destekler. Veritabanı sunucunuzla istemci uygulamalarınız arasında SSL bağlantılarının zorunlu tutulması, sunucuya uygulamanız arasındaki veri akışını şifreleyerek "bağlantıyı izinsiz izleme" saldırılarına karşı korumaya yardımcı olur.

## <a name="default-settings"></a>Varsayılan ayarlar
MariaDB için bağlanırken SSL bağlantılarını zorunlu tutmak için varsayılan olarak, veritabanı hizmetinin yapılandırılması gerekir.  Mümkün olduğunda SSL seçeneğini devre dışı bırakılmasından kaçınmak için öneririz.

Azure portalı üzerinden MariaDB sunucu için yeni bir Azure veritabanı ve CLI sağlanırken zorlama SSL bağlantıları, varsayılan olarak etkindir.

Çeşitli programlama dilleri için bağlantı dizelerini, Azure portalında gösterilir. Bu bağlantı dizeleri, veritabanınıza bağlanmak için gereken SSL parametrelerini içerir. Azure portalında sunucunuzu seçin. Altında **ayarları** başlığı seçin **bağlantı dizeleri**. SSL parametresi örneğin bağlayıcı göre değişir "ssl = true" veya "sslmode = gerektirir" veya "sslmode = gerekli" yükleyeceklerdir.

<!-- To learn how to enable or disable SSL connection when developing application, refer to [How to configure SSL](howto-configure-ssl.md).-->

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [sunucu güvenlik duvarı kuralları](concepts-firewall-rules.md)
- Bilgi edinmek için nasıl [SSL'yi yapılandırma](howto-configure-ssl.md).
