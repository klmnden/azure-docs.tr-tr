---
title: "Azure veritabanı için MySQL Server kavramları | Microsoft Docs"
description: "Bu konu, MySQL sunucuları için Azure veritabanı ile çalışmak için önemli noktalar ve yönergeler sağlar."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 10/03/2017
ms.openlocfilehash: 17f045f516792e50f7349805ec9f7a01e3f9d5b0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="server-concepts-in-azure-database-for-mysql"></a>Azure veritabanı için MySQL Server kavramları
Bu konu, MySQL sunucuları için Azure veritabanı ile çalışmak için önemli noktalar ve yönergeler sağlar.

## <a name="what-is-an-azure-database-for-mysql-server"></a>MySQL sunucusu için bir Azure veritabanı nedir?

Bir Azure MySQL sunucusu için birden fazla veritabanı için merkezi bir yönetim noktası veritabanıdır. Bu şirket içi dünyada tanıyor olabileceği MySQL server yapı olur. Özellikle, Azure veritabanı için MySQL hizmeti, yönetilen, performans garantileri sağlar ve erişimi ve sunucu düzeyinde özellikler sunar.

Azure veritabanı MySQL sunucusu için:

- Bir Azure aboneliği içinde oluşturulur.
- Veritabanları için üst kaynaktır.
- Veritabanları için bir ad alanı sağlar.
- Bir kapsayıcı güçlü ömrü semantiği ile - bir sunucu silme ve kapsanan veritabanları siler.
- Bir bölgedeki kaynakları collocates.
- Bağlantı uç noktasının sunucu ve veritabanı erişimi sağlar.
- Veritabanlarını için uygulama yönetimi ilkeleri için kapsam sağlar: oturum açma, güvenlik duvarı, kullanıcılar, roller, yapılandırmaları, vb.
- Birden çok sürümlerinde kullanılabilir. Daha fazla bilgi için bkz: [desteklenen Azure veritabanı için MySQL veritabanı sürümleri](./concepts-supported-versions.md).

MySQL sunucusu için Azure Veritabanı içinde bir veya birden fazla veritabanı oluşturabilirsiniz. Tüm kaynakları kullanmasını veya kaynakları paylaşmak için birden çok veritabanı oluşturmak için sunucu başına tek bir veritabanı oluşturmak için tercih edebilirsiniz. Fiyatlandırma yapılandırılmış başına-fiyatlandırma katmanı, işlem birimleri ve depolama (GB) yapılandırmasını temel alarak, sunucusudur. Daha fazla bilgi için bkz: [fiyatlandırma katmanlarına](./concepts-service-tiers.md).

## <a name="how-do-i-connect-and-authenticate-to-an-azure-database-for-mysql-server"></a>Nasıl bağlanmak ve MySQL sunucusu için bir Azure veritabanı kimlik doğrulaması?

Aşağıdaki öğeler için veritabanınızda güvenli erişim sağlamaya yardımcı olur.

|||
| :-- | :-- |
| **Kimlik doğrulama ve yetkilendirme** | MySQL sunucusu için Azure veritabanı yerel MySQL kimlik doğrulamasını destekler. Bağlanabilir ve sunucunun yönetici oturum açma sahip bir sunucuya kimlik doğrulaması. |
| **Protokol** | Hizmeti MySQL tarafından kullanılan bir ileti tabanlı iletişim kuralı destekler. |
| **TCP/IP'Yİ** | Protokol UNIX etki alanı Yuva üzerinden ve TCP/IP üzerinden desteklenir. |
| **Güvenlik duvarı** | İznine sahip olan bilgisayarları belirtmediğiniz sürece, verilerinizin korunmasına yardımcı olmak için bir güvenlik duvarı kuralı tüm erişim veritabanı sunucunuz veya veritabanlarını engeller. Bkz: [Azure veritabanı için MySQL Server güvenlik duvarı kuralları](./concepts-firewall-rules.md). |
| **SSL** | Hizmet, uygulamalar ve veritabanı sunucusu arasındaki zorunlu SSL bağlantılarını destekler.  Bkz. [MySQL için Azure Veritabanına güvenli bir şekilde bağlanmak üzere uygulamanızda SSL bağlantısı yapılandırma](./howto-configure-ssl.md). |
|||

## <a name="how-do-i-manage-a-server"></a>Bir sunucu nasıl yönetebilirim?
Azure portalında veya Azure CLI kullanarak Azure veritabanı için MySQL sunucuları yönetebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- Hizmeti genel bakış için bkz: [Azure veritabanı için MySQL genel bakış](./overview.md)
- Kotalar ve sınırlamalara bağlı olarak belirli bir kaynak hakkında bilgi için **hizmet katmanı**, bkz: [hizmet katmanları](./concepts-service-tiers.md)
- Hizmete bağlanma hakkında daha fazla bilgi için bkz: [Azure veritabanı için MySQL için bağlantı kitaplıkları](./concepts-connection-libraries.md).
