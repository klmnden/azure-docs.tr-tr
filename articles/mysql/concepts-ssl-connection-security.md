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
ms.date: 05/10/2017
ms.openlocfilehash: 4b03b3a2dbfad92cc0cfa84777b38ddff90452cf
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="ssl-connectivity-in-azure-database-for-mysql"></a>MySQL için Azure veritabanındaki SSL bağlantısı
MySQL için Azure veritabanı, veritabanı sunucunuzun Güvenli Yuva Katmanı (SSL) kullanarak istemci uygulamalara bağlanma destekler. Veritabanı sunucunuz ve istemci uygulamalarınız arasında SSL bağlantılarını zorlamayı "ortadaki adam" saldırılarına karşı uygulamanız ile sunucu arasındaki veri akışını şifreleyerek korunmasına yardımcı.

## <a name="default-settings"></a>Varsayılan ayarları
Varsayılan olarak, veritabanı hizmeti SSL bağlantıları için MySQL bağlanırken gerektirecek şekilde yapılandırılması gerekir.  Kaçının mümkün olduğunca SSL seçeneği devre dışı bırakılması önerilir. 

Azure portalı üzerinden MySQL sunucusu için yeni bir Azure veritabanı ve CLI sağlanırken, SSL bağlantılarını zorlama varsayılan olarak etkindir. 

Benzer şekilde, SSL kullanarak, veritabanı sunucunuza bağlanmak yaygın diller için gerekli parametreler sunucunuz Azure portalında altındaki "Bağlantı dizelerini" Ayarları'nda önceden tanımlanmış bağlantı dizeleri içerir. SSL parametre örneğin bağlayıcı göre değişir "ssl = true" veya "sslmode = gerektirir" veya "sslmode = gerekli" ve diğer Çeşitleme.

Etkinleştirme veya uygulama geliştirirken, SSL bağlantısı devre dışı bırakma hakkında bilgi edinmek için lütfen [SSL nasıl yapılandırılacağı](howto-configure-ssl.md).

## <a name="next-steps"></a>Sonraki adımlar
[MySQL için Azure veritabanı için bağlantı kitaplıkları](concepts-connection-libraries.md)
