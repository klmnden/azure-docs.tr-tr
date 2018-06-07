---
title: Azure veritabanı PostgreSQL için sunucu kavramlar
description: Bu makalede, ilgili önemli noktalar ve yapılandırma ve Azure veritabanı PostgreSQL sunucuları yönetmeye yönelik yönergeler sağlar.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 05/22/2018
ms.openlocfilehash: f877f6df51cd7aed29260331d27d5c96f0584afc
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34640022"
---
# <a name="azure-database-for-postgresql-servers"></a>PostgreSQL için Azure Veritabanı sunucuları
Bu makale, PostgreSQL sunucuları için Azure veritabanı ile çalışmak için önemli noktalar ve yönergeler sağlar.

## <a name="what-is-an-azure-database-for-postgresql-server"></a>Azure veritabanı PostgreSQL sunucu için nedir?
Bir Azure PostgreSQL sunucusu için birden fazla veritabanı için merkezi bir yönetim noktası veritabanıdır. Bu, şirket içi dünyada tanıyor olabileceği aynı PostgreSQL sunucu yapıdır. Özel olarak, PostgreSQL hizmeti yönetilen, performans garantileri sağlar, erişimi ve sunucu düzeyinde özellikler sunar.

Azure veritabanı PostgreSQL sunucu için:

- Bir Azure aboneliği içinde oluşturulur.
- Veritabanları için üst kaynaktır.
- Veritabanları için bir ad alanı sağlar.
- Bir kapsayıcı güçlü ömrü semantiği ile - bir sunucu silme ve kapsanan veritabanları siler.
- Bir bölgedeki kaynakları collocates.
- Sunucu ve veritabanına erişim için bir bağlantı uç noktasının sağlar (. postgresql.database.azure.com).
- Veritabanlarını için uygulama yönetimi ilkeleri için kapsam sağlar: oturum açma, güvenlik duvarı, kullanıcılar, roller, yapılandırmaları, vb.
- Birden çok sürümlerinde kullanılabilir. Daha fazla bilgi için bkz: [desteklenen PostgreSQL veritabanına sürümleri](concepts-supported-versions.md).
- Kullanıcılar tarafından genişletilebilirdir. Daha fazla bilgi için bkz: [PostgreSQL uzantıları](concepts-extensions.md).

Bir Azure veritabanı içinde PostgreSQL sunucu için bir veya birden çok veritabanı oluşturabilirsiniz. Tüm kaynakları kullanmak için sunucu başına tek bir veritabanı oluşturmayı veya kaynakları paylaşmak için birden çok veritabanı oluşturmayı seçebilirsiniz. Fiyatlandırma yapılandırılmış başına-fiyatlandırma katmanı, vCores ve depolama (GB) yapılandırmasını temel alarak, sunucusudur. Daha fazla bilgi için bkz: [fiyatlandırma katmanlarına](./concepts-pricing-tiers.md).

## <a name="how-do-i-connect-and-authenticate-to-an-azure-database-for-postgresql-server"></a>Nasıl bağlanmak ve PostgreSQL sunucu için bir Azure veritabanı kimlik doğrulaması?
Aşağıdaki öğeler veritabanınıza güvenli erişim sağlamaya yardımcı olmak:

|||
|:--|:--|
| **Kimlik doğrulama ve yetkilendirme** | Azure veritabanı PostgreSQL sunucu için yerel PostgreSQL kimlik doğrulamasını destekler. Bağlanabilir ve sunucunun yönetici oturum açma ile sunucusuna kimlik doğrulaması. |
| **Protokol** | Hizmet PostgreSQL tarafından kullanılan bir ileti tabanlı iletişim kuralı destekler. |
| **TCP/IP** | Protokol UNIX etki alanı Yuva üzerinden ve TCP/IP üzerinden desteklenir. |
| **Güvenlik duvarı** | İznine sahip olan bilgisayarları belirttiğiniz kadar verilerinizin korunmasına yardımcı olmak için bir güvenlik duvarı kuralı tüm erişim sunucunuz ve veritabanlarını engeller. Bkz: [Azure veritabanı PostgreSQL sunucunun güvenlik duvarı kuralları için](concepts-firewall-rules.md). |

## <a name="managing-your-server"></a>Sunucunuzu Yönetme
Azure veritabanı PostgreSQL sunucuları için kullanarak yönetebileceğiniz [Azure portal](https://portal.azure.com) veya [Azure CLI](/cli/azure/postgres).

Bir sunucu oluşturulurken, yönetici kullanıcı için kimlik bilgilerini ayarlayın. Yönetici kullanıcı, sunucu üzerinde sahip yüksek ayrıcalık kullanıcıdır. Rol azure_pg_admin aittir. Bu rolün tam süper kullanıcı izinleri yok. 

PostgreSQL süper kullanıcı özniteliği yönetilen hizmete ait azure_superuser atanır. Bu rol için erişim hakkınız yok.

Azure veritabanı PostgreSQL sunucu için iki varsayılan veritabanı vardır: 
- **postgres** -kez sunucunuza bağlanabilir, varsayılan bir veritabanı oluşturulur.
- **azure_maintenance** -bu veritabanına kullanıcı eylemlerini yönetilen hizmetinden sağlamak işlemleri ayırmak için kullanılır. Bu veritabanına erişime sahip değil.


## <a name="server-parameters"></a>Sunucu parametreleri
Sunucu yapılandırmasını PostgreSQL sunucu parametreleri belirleyin. Azure için veritabanında PostgreSQL, parametrelerin listesi görüntülenebilir ve Azure portalında veya Azure CLI kullanarak düzenlenebilir. 

Postgres için Yönetilen hizmet olarak yapılandırılabilir Azure veritabanında PostgreSQL için bir alt kümesini yerel Postgres örneğinin parametrelerinde parametreleridir (Postgres parametreleri hakkında daha fazla bilgi için bkz: [PostgreSQL belgelerine](https://www.postgresql.org/docs/9.6/static/runtime-config.html)). Oluşturma sırasında her parametre için varsayılan değerlerle Azure veritabanınız PostgreSQL sunucu için etkin. Bir sunucu gerektiren bazı parametreler yeniden başlatın veya süper kullanıcı erişimi değişikliklerin etkili olması kullanıcı tarafından yapılandırılamaz.


## <a name="next-steps"></a>Sonraki adımlar
- Hizmeti genel bakış için bkz: [Azure veritabanı PostgreSQL genel bakış için](overview.md).
- Kotalar ve sınırlamalara bağlı olarak belirli bir kaynak hakkında bilgi için **hizmet katmanı**, bkz: [hizmet katmanları](concepts-pricing-tiers.md).
- Hizmete bağlanma hakkında daha fazla bilgi için bkz: [PostgreSQL için Azure veritabanı için bağlantı kitaplıkları](concepts-connection-libraries.md).
- Görüntüleme ve düzenleme sunucu parametreleriyle [Azure portal](howto-configure-server-parameters-using-portal.md) veya [Azure CLI](howto-configure-server-parameters-using-cli.md).
