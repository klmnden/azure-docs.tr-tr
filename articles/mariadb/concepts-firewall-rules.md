---
title: Sunucu güvenlik duvarı kuralları MariaDB için Azure veritabanı
description: MariaDB için Azure veritabanı sunucunuza için güvenlik duvarı kurallarını açıklar.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 6fb9099ebfe884fc6eee58882ee23e46ba550e13
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60734447"
---
# <a name="azure-database-for-mariadb-server-firewall-rules"></a>Sunucu güvenlik duvarı kuralları MariaDB için Azure veritabanı
Güvenlik duvarları, hangi bilgisayarların izinli olduğunu belirtmenize kadar veritabanı sunucunuza tüm erişimi engeller. Güvenlik Duvarı her isteğin kaynak IP adresi tabanlı sunucu erişim verir.

Bir Güvenlik Duvarı'nı yapılandırmak için kabul edilebilir IP adreslerinin aralıklarını belirten güvenlik duvarı kuralları oluşturun. Sunucu düzeyinde güvenlik duvarı kuralları oluşturabilirsiniz.

**Güvenlik duvarı kuralları:** Bu kurallar, diğer bir deyişle, tüm aynı mantıksal sunucu içindeki veritabanlarına istemcilerin tüm Azure veritabanınızı MariaDB sunucusuna erişmesini sağlar. Azure portal veya Azure CLI komutlarını kullanarak sunucu düzeyinde güvenlik duvarı kuralları yapılandırılabilir. Sunucu düzeyinde güvenlik duvarı kuralları oluşturma için abonelik sahibi veya abonelik katkıda bulunanı olmanız gerekir.

## <a name="firewall-overview"></a>Güvenlik duvarına genel bakış
MariaDB sunucusu için Azure veritabanı sunucunuza tüm veritabanı erişimi, güvenlik duvarı tarafından engellenmediğinden varsayılan olarak açıktır. Başka bir bilgisayardan sunucunuzu kullanmaya başlamak için sunucunuza erişimi etkinleştirmek için bir veya daha fazla sunucu düzeyinde güvenlik duvarı kurallarını belirtmek gerekir. Adres aralıklarını izin vermek için Internet'ten güvenlik duvarı kuralları, hangi IP belirtmek için kullanın. Azure Portalı Web sitesine erişimi kendisi güvenlik duvarı kuralları tarafından etkilenmez.

MariaDB veritabanı için Azure veritabanınızı ulaşmadan önce İnternet'e ve Azure gelen bağlantı denemeleri Aşağıdaki diyagramda gösterildiği gibi ilk güvenlik duvarından geçmelidir:

![Güvenlik Duvarı'nı nasıl çalıştığına ilişkin örnek akış](./media/concepts-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-the-internet"></a>İnternet'ten bağlanma
Tüm veritabanlarında MariaDB için Azure veritabanı sunucu düzeyinde güvenlik duvarı kuralları uygulanır.

İsteğin IP adresi sunucu düzeyinde güvenlik duvarı kurallarında belirtilen aralıklardan biri içinde ise, bağlantı izni verilir.

İsteğin IP adresi veritabanı düzeyinde veya sunucu düzeyinde güvenlik duvarı kuralları hiçbirinde belirtilen aralıkların dışında ise, ardından bağlantı isteği başarısız olur.

## <a name="connecting-from-azure"></a>Azure'dan bağlanma
MariaDB için Azure veritabanı sunucunuza bağlanmak Azure uygulamalarından izin vermek için Azure bağlantılarının etkinleştirilmesi gerekir. Örneğin, bir Azure Web Apps uygulama veya bir Azure sanal Makinesinde çalışan bir uygulamayı barındırmak için veya bir Azure Data Factory veri yönetimi ağ geçidi bağlanmak için. Kaynakların bu bağlantıları etkinleştirmek için aynı sanal ağ (VNet) veya kaynak grubu için güvenlik duvarı kuralı olması gerekmez. Azure’dan bir uygulama, veritabanı sunucunuza bağlanmayı denediğinizde güvenlik duvarı Azure bağlantılarına izin verildiğini doğrular. Birkaç bu tür bağlantıları etkinleştirmek için yöntem vardır. Başlangıç ve bitiş adresi 0.0.0.0’a eşit olan bir güvenlik duvarı ayarı, bu bağlantılara izin verildiğini gösterir. Alternatif olarak, ayarlayabileceğiniz **Azure hizmetlerine erişime izin ver** seçeneğini **ON** Portalı'nda **bağlantı güvenliği** bölmesi ve isabet **Kaydet**. Bağlantı denemesi izin verilmiyorsa, istek MariaDB için Azure veritabanı ulaşmaz.

> [!IMPORTANT]
> Bu seçenek, diğer müşterilerin aboneliklerinden gelen bağlantılar dahil Azure’dan tüm bağlantılara izin verecek şekilde güvenlik duvarınızı yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.
> 

![Portalda Azure hizmetlerine erişime izin ver'ı yapılandırma](./media/concepts-firewall-rules/allow-azure-services.png)

## <a name="programmatically-managing-firewall-rules"></a>Güvenlik duvarı kurallarını programlı bir şekilde yönetme
Azure portalına ek olarak Azure CLI kullanarak güvenlik duvarı kurallarını programlı bir şekilde yönetilebilir. 

<!--See also [Create and manage Azure Database for MariaDB firewall rules using Azure CLI](./howto-manage-firewall-using-cli.md)-->

## <a name="troubleshooting-the-database-firewall"></a>Veritabanı güvenlik duvarı sorunlarını giderme
Microsoft Azure veritabanına erişimi MariaDB server hizmeti için beklendiği gibi davranmadığında aşağıdaki noktaları göz önünde bulundurun:

* **İzin verilenler listesine değişiklikler henüz etkili olmamıştır:** Bir beş dakikaya kadar değişikliklerin etkili olması için Azure veritabanı MariaDB sunucusu güvenlik duvarı yapılandırması olabilir.

* **Oturum açma yetkili değil veya yanlış parola kullanıldı:** Bir oturum açma MariaDB için Azure veritabanı üzerinde izni yok veya kullanılan parola yanlışsa, MariaDB için Azure veritabanı bağlantısı reddedildi. Bir güvenlik duvarı ayarının oluşturulması yalnızca istemcilere sunucunuzla bağlantı kurmayı deneme fırsatı sunar; her istemci gerekli güvenlik kimlik bilgilerini belirtmek zorundadır.

* **Dinamik IP adresi:** Dinamik IP adresiyle kurulmuş bir Internet bağlantısı varsa ve güvenlik duvarını aşmakta sorun yaşıyorsanız aşağıdaki çözümlerden birini deneyebilirsiniz:

* MariaDB için Azure veritabanı erişen istemci bilgisayarlarınıza atanmış IP adresi aralığını Internet servis sağlayıcınıza (ISS) sorun ve IP adres aralığı olarak bir güvenlik duvarı kuralı ekleyin.

* İstemci bilgisayarlarınız için bunun yerine statik IP adresi alın ve IP adreslerini güvenlik duvarı kuralları olarak ekleyin.

## <a name="next-steps"></a>Sonraki adımlar
- [Oluşturma ve Azure veritabanı Azure portalını kullanarak MariaDB için güvenlik duvarı kurallarını yönetme](./howto-manage-firewall-portal.md)

<!--
- [Create and manage Azure Database for MariaDB firewall rules using Azure CLI](./howto-manage-firewall-using-cli.md) -->
