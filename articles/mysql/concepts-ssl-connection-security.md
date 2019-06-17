---
title: MySQL için Azure veritabanı için SSL bağlantısı
description: MySQL ve SSL bağlantıları düzgün bir şekilde kullanmak için ilişkili uygulamalar için Azure veritabanı'nı yapılandırma hakkında bilgi
author: JasonMAnderson
ms.author: janders
ms.service: mysql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: 129f90d495627edb25dfafdeb1b274aa2c4c71cb
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60525827"
---
# <a name="ssl-connectivity-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı'nda SSL bağlantısı
MySQL için Azure veritabanı, Güvenli Yuva Katmanı (SSL) kullanan istemci uygulamalar için veritabanı sunucunuza bağlanmayı destekler. Veritabanı sunucunuzla istemci uygulamalarınız arasında SSL bağlantılarının zorunlu tutulması, sunucuya uygulamanız arasındaki veri akışını şifreleyerek "bağlantıyı izinsiz izleme" saldırılarına karşı korumaya yardımcı olur.

## <a name="default-settings"></a>Varsayılan ayarları
Mysql'e bağlanırken SSL bağlantılarını zorunlu tutmak için varsayılan olarak, veritabanı hizmetinin yapılandırılması gerekir.  Mümkün olduğunda SSL seçeneğini devre dışı bırakılmasından kaçınmak için öneririz. 

Azure portalından MySQL sunucusu için yeni bir Azure veritabanı ve CLI sağlanırken zorlama SSL bağlantıları, varsayılan olarak etkindir. 

Çeşitli programlama dilleri için bağlantı dizelerini, Azure portalında gösterilir. Bu bağlantı dizeleri, veritabanınıza bağlanmak için gereken SSL parametrelerini içerir. Azure portalında sunucunuzu seçin. Altında **ayarları** başlığı seçin **bağlantı dizeleri**. SSL parametresi örneğin bağlayıcı göre değişir "ssl = true" veya "sslmode = gerektirir" veya "sslmode = gerekli" yükleyeceklerdir.

Etkinleştirmek veya uygulama geliştirirken SSL bağlantısı devre dışı bırakma hakkında bilgi edinmek için bkz [SSL yapılandırma](howto-configure-ssl.md). 

## <a name="next-steps"></a>Sonraki adımlar
[MySQL için Azure veritabanı için bağlantı kitaplıkları](concepts-connection-libraries.md)
