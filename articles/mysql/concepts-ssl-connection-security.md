---
title: "Azure veritabanı için MySQL için SSL bağlantısı | Microsoft Docs"
description: "Azure veritabanı MySQL ve düzgün şekilde SSL bağlantılarını kullanmak için ilişkili uygulamalar için yapılandırma için bilgiler"
services: mysql
author: JasonMAnderson
ms.author: janders
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 11/02/2017
ms.openlocfilehash: 37e2bbbe1ed4b6a9cca0e0b001e5e632b9b73be2
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="ssl-connectivity-in-azure-database-for-mysql"></a>MySQL için Azure veritabanındaki SSL bağlantısı
MySQL için Azure veritabanı, veritabanı sunucunuzun Güvenli Yuva Katmanı (SSL) kullanarak istemci uygulamalara bağlanma destekler. Veritabanı sunucunuz ve istemci uygulamalarınız arasında SSL bağlantılarını zorlamayı "ortadaki adam" saldırılarına karşı uygulamanız ile sunucu arasındaki veri akışını şifreleyerek korunmasına yardımcı.

## <a name="default-settings"></a>Varsayılan ayarları
Varsayılan olarak, veritabanı hizmeti SSL bağlantıları için MySQL bağlanırken gerektirecek şekilde yapılandırılması gerekir.  Mümkün olduğunda SSL seçeneği devre dışı öneririz. 

Azure portalı üzerinden MySQL sunucusu için yeni bir Azure veritabanı ve CLI sağlanırken, SSL bağlantılarını zorlama varsayılan olarak etkindir. 

Çeşitli programlama dillerini için bağlantı dizelerini Azure portalında gösterilir. Bu bağlantı dizeleri, veritabanına bağlanmak için gereken SSL parametrelerini içerir. Azure portalında sunucunuzu seçin. Altında **ayarları** başlığını seçin **bağlantı dizeleri**. SSL parametre örneğin bağlayıcı göre değişir "ssl = true" veya "sslmode = gerektirir" veya "sslmode = gerekli" ve diğer Çeşitleme.

Etkinleştirme veya uygulama geliştirirken, SSL bağlantısı devre dışı bırakma hakkında bilgi edinmek için başvurmak [SSL nasıl yapılandırılacağı](howto-configure-ssl.md). 

## <a name="next-steps"></a>Sonraki adımlar
[MySQL için Azure veritabanı için bağlantı kitaplıkları](concepts-connection-libraries.md)
