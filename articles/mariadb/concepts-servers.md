---
title: MariaDB için Azure veritabanı sunucusu kavramları
description: Bu konu, MariaDB sunucuları için Azure veritabanı ile çalışmak için önemli noktalar ve yönergeler sağlar.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: f61f8740c9514f6276afb2ee84bcdccdc54c0710
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61040932"
---
# <a name="server-concepts-in-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı sunucusu kavramları
Bu makalede, MariaDB sunucuları için Azure veritabanı ile çalışmaya yönelik kurallar ve dikkat edilecek noktalar sunulmaktadır.

## <a name="what-is-an-azure-database-for-mariadb-server"></a>MariaDB server için Azure veritabanı nedir?

MariaDB server için Azure veritabanı, birden fazla veritabanı için merkezi bir yönetim noktası ' dir. Bu şirket içi dünyada alışkın olabileceğiniz MariaDB sunucu yapısı olur. Özellikle, MariaDB hizmeti için Azure veritabanı, yönetilen, performans garantileri sağlar ve erişim ve Özellikler sunucu düzeyinde kullanıma sunar.

MariaDB için Azure veritabanı:

- Bir Azure aboneliği içinde oluşturulur.
- Veritabanları için üst kaynaktır.
- Veritabanları için bir ad alanı sağlar.
- Bir kapsayıcı güçlü kullanım ömrü semantiğine sahip - bir sunucu silme ve kapsanan veritabanları siler.
- Bir bölgedeki kaynakları birlikte bulundurur.
- Sunucu ve veritabanı erişimi için bir bağlantı uç noktası sağlar.
- Veritabanlarını için uygulama yönetimi ilkeleri için kapsam sağlar: oturum açma, güvenlik duvarı, kullanıcılar, roller, yapılandırmaları, vb.
- MariaDB altyapı sürümü 10.2 kullanılabilir. Daha fazla bilgi için [MariaDB veritabanı sürümleri için desteklenen Azure veritabanı'na](./concepts-supported-versions.md).

MariaDB için Azure Veritabanı sunucusunda bir veya birden fazla veritabanı oluşturabilirsiniz. Tüm kaynakları veya kaynakları paylaşmak için birden çok veritabanını oluşturmak için sunucu başına tek bir veritabanı oluşturmak için tercih edebilirsiniz. Fiyatlandırma yapılandırılmış başına-fiyatlandırma katmanı, sanal çekirdek ve depolama alanı (GB) yapılandırmasına bağlı olarak, sunucusudur. Daha fazla bilgi için [fiyatlandırma katmanları](./concepts-pricing-tiers.md).

## <a name="how-do-i-secure-an-azure-database-for-mariadb-server"></a>MariaDB server için Azure veritabanı güvenliğini nasıl sağlayabilirim?

Aşağıdaki öğeleri veritabanınıza güvenli erişim sağlayın.

|||
| :--| :--|
| **Kimlik doğrulama ve yetkilendirme** | MariaDB için Azure veritabanı, yerel MySQL kimlik doğrulamasını destekler. Bağlanın ve Sunucu Yöneticisi oturum açma sunucusuyla kimlik doğrulaması. |
| **Protokolü** | Hizmet, MySQL tarafından kullanılan bir ileti tabanlı iletişim kuralı destekler. |
| **TCP/IP** | Protokol, TCP/IP üzerinde ve UNIX etki alanı Yuva üzerinden desteklenir. |
| **Güvenlik duvarı** | Hangi bilgisayarların izinli olduğunu belirtmenize kadar verilerinizi korumak için bir güvenlik duvarı kuralı tüm erişim veritabanı sunucunuza engeller. Bkz: [MariaDB sunucu güvenlik duvarı kuralları için Azure veritabanı](./concepts-firewall-rules.md). |
| **SSL** | Hizmet, uygulamalarınız ve veritabanı sunucunuz arasında zorunlu SSL bağlantılarını destekler. Bkz: [güvenli bir şekilde MariaDB için Azure veritabanı'na bağlanmak üzere uygulamanızda SSL yapılandırma bağlantısı](./howto-configure-ssl.md). |

## <a name="how-do-i-manage-a-server"></a>Bir sunucuya nasıl yönetebilirim?
MariaDB sunucuları için Azure veritabanı, Azure portalında veya Azure CLI kullanarak yönetebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- Hizmetine genel bakış için bkz. [MariaDB genel bakış için Azure veritabanı](./overview.md)
- Kotalar ve sınırlamalar temel alarak belirli bir kaynak hakkında bilgi için **hizmet katmanı**, bkz: [hizmet katmanları](./concepts-pricing-tiers.md)

<!-- - For information about connecting to the service, see [Connection libraries for Azure Database for MariaDB](./concepts-connection-libraries.md). -->
