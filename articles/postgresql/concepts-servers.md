---
title: "Azure veritabanında PostgreSQL için sunucu kavramları | Microsoft Docs"
description: "Bu konu hakkında önemli noktalar ve yapılandırma ve Azure veritabanı PostgreSQL sunucuları yönetmeye yönelik yönergeler sağlar."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 12/02/2017
ms.openlocfilehash: d7eec2735e48f57500eb2ea822f0949d2ec2e585
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="azure-database-for-postgresql-servers"></a>Azure veritabanı PostgreSQL sunucuları için
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

Bir Azure veritabanı içinde PostgreSQL sunucu için bir veya birden çok veritabanı oluşturabilirsiniz. Tüm kaynakları kullanmak için sunucu başına tek bir veritabanı oluşturmayı veya kaynakları paylaşmak için birden çok veritabanı oluşturmayı seçebilirsiniz. Fiyatlandırma yapılandırılmış başına-fiyatlandırma katmanı, işlem birimleri ve depolama (GB) yapılandırmasını temel alarak, sunucusudur. Daha fazla bilgi için bkz: [fiyatlandırma katmanlarına](./concepts-service-tiers.md).

## <a name="how-do-i-connect-and-authenticate-to-an-azure-database-for-postgresql-server"></a>Nasıl bağlanmak ve PostgreSQL sunucu için bir Azure veritabanı kimlik doğrulaması?
Aşağıdaki öğeler veritabanınıza güvenli erişim sağlamaya yardımcı olmak:

|||
|:--|:--|
| **Kimlik doğrulama ve yetkilendirme** | Azure veritabanı PostgreSQL sunucu için yerel PostgreSQL kimlik doğrulamasını destekler. Bağlanabilir ve sunucunun yönetici oturum açma ile sunucusuna kimlik doğrulaması. |
| **Protokol** | Hizmet PostgreSQL tarafından kullanılan bir ileti tabanlı iletişim kuralı destekler. |
| **TCP/IP'Yİ** | Protokol UNIX etki alanı Yuva üzerinden ve TCP/IP üzerinden desteklenir. |
| **Güvenlik duvarı** | İznine sahip olan bilgisayarları belirttiğiniz kadar verilerinizin korunmasına yardımcı olmak için bir güvenlik duvarı kuralı tüm erişim sunucunuz ve veritabanlarını engeller. Bkz: [Azure veritabanı PostgreSQL sunucunun güvenlik duvarı kuralları için](concepts-firewall-rules.md). |

## <a name="how-do-i-manage-a-server"></a>Bir sunucu nasıl yönetebilirim?
Azure veritabanı PostgreSQL sunucuları için kullanarak yönetebileceğiniz [Azure portal](https://portal.azure.com) veya [Azure CLI](/cli/azure/postgres).

## <a name="server-parameters"></a>Sunucu parametreleri
Sunucu yapılandırmasını PostgreSQL sunucu parametreleri belirleyin. Azure için veritabanında PostgreSQL, parametrelerin listesi görüntülenebilir ve Azure portalında veya Azure CLI kullanarak düzenlenebilir. 

Postgres için Yönetilen hizmet olarak yapılandırılabilir Azure veritabanında PostgreSQL için bir alt kümesini yerel Postgres örneğinin parametrelerinde parametreleridir (Postgres parametreleri hakkında daha fazla bilgi için bkz: [PostgreSQL belgelerine](https://www.postgresql.org/docs/9.6/static/runtime-config.html)). Oluşturma sırasında her parametre için varsayılan değerlerle Azure veritabanınız PostgreSQL sunucu için etkin. Bir sunucu gerektirir parametreleri yeniden başlatın veya süper kullanıcı erişimi değişikliklerin etkili olması kullanıcı tarafından yapılandırılamaz.


## <a name="next-steps"></a>Sonraki adımlar
- Hizmeti genel bakış için bkz: [Azure veritabanı PostgreSQL genel bakış için](overview.md).
- Kotalar ve sınırlamalara bağlı olarak belirli bir kaynak hakkında bilgi için **hizmet katmanı**, bkz: [hizmet katmanları](concepts-service-tiers.md).
- Hizmete bağlanma hakkında daha fazla bilgi için bkz: [PostgreSQL için Azure veritabanı için bağlantı kitaplıkları](concepts-connection-libraries.md).
- Görüntüleme ve düzenleme sunucu parametreleriyle [Azure portal](howto-configure-server-parameters-using-portal.md) veya [Azure CLI](howto-configure-server-parameters-using-cli.md).
