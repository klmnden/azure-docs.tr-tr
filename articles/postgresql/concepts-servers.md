---
title: PostgreSQL - tek bir sunucu için Azure veritabanı sunucusu kavramları
description: Bu makalede, konuları ve - tek bir sunucu PostgreSQL için Azure veritabanı'nı yönetme ve yapılandırma için yönergeler sağlar.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: bc135e58d0fbabc809f3718915e9f4e35b8ed875
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65067152"
---
# <a name="azure-database-for-postgresql---single-server"></a>PostgreSQL - tek bir sunucu için Azure veritabanı
Bu makalede,-tek bir sunucu PostgreSQL için Azure veritabanı ile çalışmaya yönelik kurallar ve dikkat edilecek noktalar sunulmaktadır.

## <a name="what-is-an-azure-database-for-postgresql-server"></a>PostgreSQL sunucusu için Azure veritabanı nedir?
Bir PostgreSQL - tek sunuculu dağıtım seçeneği için Azure veritabanı'nda birden fazla veritabanı için merkezi bir yönetim noktası sunucusudur. Bu şirket içi dünyada alışkın olabileceğiniz PostgreSQL sunucusu yapısı olur. Özellikle, PostgreSQL hizmeti yönetilir, performans garantileri sağlar, erişim ve Özellikler sunucu düzeyinde kullanıma sunar.

PostgreSQL sunucusu için Azure veritabanı:

- Bir Azure aboneliği içinde oluşturulur.
- Veritabanları için üst kaynaktır.
- Veritabanları için bir ad alanı sağlar.
- Bir kapsayıcı güçlü kullanım ömrü semantiğine sahip - bir sunucu silme ve kapsanan veritabanları siler.
- Bir bölgedeki kaynakları birlikte bulundurur.
- Bir bağlantı uç noktası sunucusu ve veritabanı erişimi sağlar 
- Veritabanlarını için uygulama yönetimi ilkeleri için kapsam sağlar: oturum açma, güvenlik duvarı, kullanıcılar, roller, yapılandırmaları, vb.
- Birden çok sürümlerinde kullanılabilir. Daha fazla bilgi için [desteklenen PostgreSQL veritabanı sürümleri](concepts-supported-versions.md).
- Kullanıcı tarafından Genişletilebilir olur. Daha fazla bilgi için [PostgreSQL uzantıları](concepts-extensions.md).

PostgreSQL için Azure veritabanı içinde bir veya birden çok veritabanı oluşturabilirsiniz. Tüm kaynakları kullanmak için sunucu başına tek bir veritabanı oluşturmayı veya kaynakları paylaşmak için birden çok veritabanı oluşturmayı seçebilirsiniz. Fiyatlandırma yapılandırılmış başına-fiyatlandırma katmanı, sanal çekirdek ve depolama alanı (GB) yapılandırmasına bağlı olarak, sunucusudur. Daha fazla bilgi için [fiyatlandırma katmanları](./concepts-pricing-tiers.md).

## <a name="how-do-i-connect-and-authenticate-to-an-azure-database-for-postgresql-server"></a>Nasıl bağlanmak ve bir PostgreSQL sunucusu için Azure veritabanı kimlik doğrulaması?
Aşağıdaki öğeleri veritabanınıza güvenli erişimi yardımcı olur:

|||
|:--|:--|
| **Kimlik doğrulama ve yetkilendirme** | PostgreSQL sunucusu için Azure veritabanı, yerel PostgreSQL kimlik doğrulamasını destekler. Bağlanın ve Sunucu Yöneticisi oturum açma sunucusuyla kimlik doğrulaması. |
| **Protokolü** | Hizmet PostgreSQL kullanılan ileti tabanlı bir protokol destekler. |
| **TCP/IP** | Protokol, TCP/IP'yi üzerinden ve UNIX etki alanı Yuva üzerinden desteklenir. |
| **Güvenlik duvarı** | Hangi bilgisayarların izinli olduğunu belirtmenize kadar verilerinizi korumak için bir güvenlik duvarı kuralı tüm erişim sunucunuza ve veritabanlarına, engeller. Bkz: [PostgreSQL sunucusu güvenlik duvarı kuralları için Azure veritabanı](concepts-firewall-rules.md). |

## <a name="managing-your-server"></a>Sunucunuzu Yönetme
PostgreSQL sunucuları için Azure veritabanı kullanarak yönetebilirsiniz [Azure portalında](https://portal.azure.com) veya [Azure CLI](/cli/azure/postgres).

Bir sunucu oluşturulurken, yönetici kullanıcı için kimlik bilgilerini ayarlayın. Yönetici kullanıcı, sunucuda sahip yüksek ayrıcalık kullanıcıdır. İçin rol azure_pg_admin ait. Bu rol, tam bir süper kullanıcı izinlerine sahip değil. 

PostgreSQL süper kullanıcı özniteliği yönetilen hizmete ait azure_superuser atanır. Bu rol erişiminiz yok.

PostgreSQL sunucusu için Azure veritabanı, varsayılan veritabanı vardır: 
- **postgres** -bir kez sunucunuza bağlanabilir, varsayılan bir veritabanı oluşturulur.
- **azure_maintenance** -bu veritabanı, kullanıcı eylemlerine yönetilen hizmet sağlayan işlemlerin ayırmak için kullanılır. Bu veritabanına erişimi yok.
- **azure_sys** -bir veritabanı için Query Store. Query Store kapalı olduğunda, bu veritabanı veri saymaz; Varsayılan ayar budur. Daha fazla bilgi için [Query Store genel bakış](concepts-query-store.md).


## <a name="server-parameters"></a>Sunucu parametreleri
PostgreSQL sunucusu parametrelerini sunucusunun yapılandırmasını belirler. PostgreSQL için Azure veritabanı, parametre listesi görüntülenebilir ve Azure portalı veya Azure CLI kullanarak düzenlenebilir. 

Postgres için Yönetilen hizmet olarak PostgreSQL için Azure veritabanı'nda yapılandırılabilir parametreler yerel Postgres örneği parametrelerinde bir alt kümesi olan (Postgres parametreleri hakkında daha fazla bilgi için bkz. [PostgreSQL belgeleri](https://www.postgresql.org/docs/9.6/static/runtime-config.html)). PostgreSQL için Azure veritabanı sunucunuza oluşturma sırasında her parametre için varsayılan değerleri ile etkinleştirilir. Bir sunucu gerektiren bazı parametreler yeniden başlatabilir veya süper kullanıcı erişimi değişikliklerin etkili olabilmesi için kullanıcı tarafından yapılandırılamaz.


## <a name="next-steps"></a>Sonraki adımlar
- Hizmetine genel bakış için bkz. [PostgreSQL genel bakış için Azure veritabanı](overview.md).
- Kotalar ve sınırlamalar temel alarak belirli bir kaynak hakkında bilgi için **hizmet katmanı**, bkz: [hizmet katmanları](concepts-pricing-tiers.md).
- Hizmete bağlanma hakkında daha fazla bilgi için bkz. [PostgreSQL için Azure veritabanı için bağlantı kitaplıkları](concepts-connection-libraries.md).
- Görüntüleme ve düzenleme sunucu parametreleri aracılığıyla [Azure portalında](howto-configure-server-parameters-using-portal.md) veya [Azure CLI](howto-configure-server-parameters-using-cli.md).
