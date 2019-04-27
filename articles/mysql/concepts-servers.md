---
title: MySQL için Azure veritabanı sunucusu kavramları
description: Bu konuda, MySQL Server için Azure veritabanı ile çalışmak için kurallar ve dikkat edilecek noktalar sunulmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: 565e1bf7a4972e230b3cf56232ebd24519fcab5c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60525860"
---
# <a name="server-concepts-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı sunucusu kavramları

Bu makalede, MySQL Server için Azure veritabanı ile çalışmaya yönelik kurallar ve dikkat edilecek noktalar sunulmaktadır.

## <a name="what-is-an-azure-database-for-mysql-server"></a>MySQL sunucusu için Azure veritabanı nedir?

MySQL sunucusu için Azure veritabanı, birden fazla veritabanı için merkezi bir yönetim noktası ' dir. Bu şirket içi dünyada alışkın olabileceğiniz MySQL server yapısı olur. Özellikle, MySQL hizmeti için Azure veritabanı, yönetilen, performans garantileri sağlar ve erişim ve Özellikler sunucu düzeyinde kullanıma sunar.

MySQL sunucusu için Azure veritabanı:

- Bir Azure aboneliği içinde oluşturulur.
- Veritabanları için üst kaynaktır.
- Veritabanları için bir ad alanı sağlar.
- Bir kapsayıcı güçlü kullanım ömrü semantiğine sahip - bir sunucu silme ve kapsanan veritabanları siler.
- Bir bölgedeki kaynakları birlikte bulundurur.
- Sunucu ve veritabanı erişimi için bir bağlantı uç noktası sağlar.
- Veritabanlarını için uygulama yönetimi ilkeleri için kapsam sağlar: oturum açma, güvenlik duvarı, kullanıcılar, roller, yapılandırmaları, vb.
- Birden çok sürümlerinde kullanılabilir. Daha fazla bilgi için [MySQL veritabanı sürümleri için desteklenen Azure veritabanı'na](./concepts-supported-versions.md).

MySQL sunucusu için Azure Veritabanı içinde bir veya birden fazla veritabanı oluşturabilirsiniz. Tüm kaynakları veya kaynakları paylaşmak için birden çok veritabanını oluşturmak için sunucu başına tek bir veritabanı oluşturmak için tercih edebilirsiniz. Fiyatlandırma yapılandırılmış başına-fiyatlandırma katmanı, sanal çekirdek ve depolama alanı (GB) yapılandırmasına bağlı olarak, sunucusudur. Daha fazla bilgi için [fiyatlandırma katmanları](./concepts-service-tiers.md).

## <a name="how-do-i-connect-and-authenticate-to-an-azure-database-for-mysql-server"></a>Nasıl bağlanmak ve MySQL için Azure veritabanı kimlik doğrulaması?

Aşağıdaki öğeleri veritabanınıza güvenli erişim sağlayın.

|     |     |
| :-- | :-- |
| **Kimlik doğrulama ve yetkilendirme** | MySQL sunucusu için Azure veritabanı, yerel MySQL kimlik doğrulamasını destekler. Bağlanın ve Sunucu Yöneticisi oturum açma sunucusuyla kimlik doğrulaması. |
| **Protokol** | Hizmet, MySQL tarafından kullanılan bir ileti tabanlı iletişim kuralı destekler. |
| **TCP/IP** | Protokol, TCP/IP üzerinde ve UNIX etki alanı Yuva üzerinden desteklenir. |
| **Güvenlik duvarı** | Hangi bilgisayarların izinli olduğunu belirtmenize kadar verilerinizi korumak için bir güvenlik duvarı kuralı tüm erişim veritabanı sunucunuza engeller. Bkz: [MySQL sunucusu güvenlik duvarı kuralları için Azure veritabanı](./concepts-firewall-rules.md). |
| **SSL** | Hizmet, uygulamalarınız ve veritabanı sunucunuz arasında zorunlu SSL bağlantılarını destekler.  Bkz. [MySQL için Azure Veritabanına güvenli bir şekilde bağlanmak üzere uygulamanızda SSL bağlantısı yapılandırma](./howto-configure-ssl.md). |

## <a name="how-do-i-manage-a-server"></a>Bir sunucuya nasıl yönetebilirim?

Azure portalında veya Azure CLI kullanarak MySQL Server için Azure veritabanı yönetebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Hizmetine genel bakış için bkz. [MySQL genel bakış için Azure veritabanı](./overview.md)
- Kotalar ve sınırlamalar temel alarak belirli bir kaynak hakkında bilgi için **hizmet katmanı**, bkz: [hizmet katmanları](./concepts-service-tiers.md)
- Hizmete bağlanma hakkında daha fazla bilgi için bkz: [MySQL için Azure veritabanı için bağlantı kitaplıkları](./concepts-connection-libraries.md).
