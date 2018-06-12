---
title: Azure veritabanı için MySQL için SSL bağlantısı
description: Azure veritabanı MySQL ve düzgün şekilde SSL bağlantılarını kullanmak için ilişkili uygulamalar için yapılandırma için bilgiler
services: mysql
author: JasonMAnderson
ms.author: janders
editor: jasonwhowell
manager: kfile
ms.service: mysql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: ee7e0ec8524d66ee89cf7b2c4d44b70efa784f8f
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35265070"
---
# <a name="ssl-connectivity-in-azure-database-for-mysql"></a>MySQL için Azure veritabanındaki SSL bağlantısı
MySQL için Azure veritabanı, veritabanı sunucunuzun Güvenli Yuva Katmanı (SSL) kullanarak istemci uygulamalara bağlanma destekler. Veritabanı sunucunuzla istemci uygulamalarınız arasında SSL bağlantılarının zorunlu tutulması, sunucuya uygulamanız arasındaki veri akışını şifreleyerek "bağlantıyı izinsiz izleme" saldırılarına karşı korumaya yardımcı olur.

## <a name="default-settings"></a>Varsayılan ayarlar
Varsayılan olarak, veritabanı hizmeti SSL bağlantıları için MySQL bağlanırken gerektirecek şekilde yapılandırılması gerekir.  Mümkün olduğunda SSL seçeneği devre dışı öneririz. 

Azure portalı üzerinden MySQL sunucusu için yeni bir Azure veritabanı ve CLI sağlanırken, SSL bağlantılarını zorlama varsayılan olarak etkindir. 

Çeşitli programlama dillerini için bağlantı dizelerini Azure portalında gösterilir. Bu bağlantı dizeleri, veritabanına bağlanmak için gereken SSL parametrelerini içerir. Azure portalında sunucunuzu seçin. Altında **ayarları** başlığını seçin **bağlantı dizeleri**. SSL parametre örneğin bağlayıcı göre değişir "ssl = true" veya "sslmode = gerektirir" veya "sslmode = gerekli" ve diğer Çeşitleme.

Etkinleştirme veya uygulama geliştirirken, SSL bağlantısı devre dışı bırakma hakkında bilgi edinmek için başvurmak [SSL nasıl yapılandırılacağı](howto-configure-ssl.md). 

## <a name="next-steps"></a>Sonraki adımlar
[MySQL için Azure veritabanı için bağlantı kitaplıkları](concepts-connection-libraries.md)
