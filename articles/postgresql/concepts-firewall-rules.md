---
title: PostgreSQL - tek bir sunucu için Azure veritabanı'nda güvenlik duvarı kuralları
description: Bu makalede, PostgreSQL - tek bir sunucu için Azure veritabanı güvenlik duvarı kuralları açıklanır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 40a675fbefe9743f5de1f9766cf33ae7dba9e5a7
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65073580"
---
# <a name="firewall-rules-in-azure-database-for-postgresql---single-server"></a>PostgreSQL - tek bir sunucu için Azure veritabanı'nda güvenlik duvarı kuralları
Hangi bilgisayarların izinli olduğunu belirtmenize kadar PostgreSQL sunucusu güvenlik duvarı için azure veritabanı, veritabanı sunucunuza tüm erişimi engeller. Güvenlik Duvarı her isteğin kaynak IP adresi tabanlı sunucu erişim verir.
Güvenlik duvarınızı yapılandırmak için kabul edilebilir IP adreslerinin aralıklarını belirten güvenlik duvarı kuralları oluşturun. Sunucu düzeyinde güvenlik duvarı kuralları oluşturabilirsiniz.

**Güvenlik duvarı kuralları:** Bu kurallar, PostgreSQL sunucusu için Azure veritabanınızı tamamen erişmek başka bir deyişle, tüm aynı mantıksal sunucu içindeki veritabanlarına istemcilerin sağlar. Azure portalını kullanarak veya Azure CLI komutlarını kullanarak sunucu düzeyinde güvenlik duvarı kuralları yapılandırılabilir. Sunucu düzeyinde güvenlik duvarı kuralları oluşturma için abonelik sahibi veya abonelik katkıda bulunanı olmanız gerekir.

## <a name="firewall-overview"></a>Güvenlik duvarına genel bakış
PostgreSQL için Azure veritabanı sunucunuza tüm veritabanı erişimi varsayılan olarak güvenlik duvarı tarafından engellenir. Başka bir bilgisayardan sunucunuzu kullanmaya başlamak için sunucunuza erişimi etkinleştirmek için bir veya daha fazla sunucu düzeyinde güvenlik duvarı kurallarını belirtmek gerekir. Adres aralıklarını izin vermek için Internet'ten güvenlik duvarı kuralları, hangi IP belirtmek için kullanın. Azure Portalı Web sitesine erişimi kendisi güvenlik duvarı kuralları tarafından etkilenmez.
PostgreSQL veritabanınızın ulaşmadan önce İnternet'e ve Azure gelen bağlantı denemeleri Aşağıdaki diyagramda gösterildiği gibi ilk güvenlik duvarından geçmelidir:

![Güvenlik Duvarı'nı nasıl çalıştığına ilişkin örnek akış](media/concepts-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-the-internet"></a>İnternet'ten bağlanma
Tüm veritabanları PostgreSQL sunucusu için aynı Azure veritabanı sunucu düzeyinde güvenlik duvarı kuralları uygulanır. İstek IP adresi sunucu düzeyinde güvenlik duvarı kurallarında belirtilen aralıklardan biri içindeyse, bağlantı izni verilir.
İsteğin IP adresi sunucu düzeyinde güvenlik duvarı kuralları hiçbirinde belirtilen aralıklar dahilinde değilse, bağlantı isteği başarısız olur.
Örneğin, uygulamanız için PostgreSQL JDBC sürücüsü ile bağlanıyorsa, güvenlik duvarı bağlantıyı engelliyor olsa bile bağlan çalışırken bu hatayla karşılaşabilirsiniz.
> java.util.concurrent.ExecutionException: java.lang.RuntimeException: org.postgresql.util.PSQLException: Önemli: hiçbir pg\_"123.45.67.890" ana bilgisayar, kullanıcı "adminuser" veritabanı "postgresql", SSL hba.conf giriş

## <a name="connecting-from-azure"></a>Azure'dan bağlanma
PostgreSQL için Azure veritabanı sunucunuza bağlanmak Azure uygulamalarından izin vermek için Azure bağlantılarının etkinleştirilmesi gerekir. Örneğin, bir Azure Web Apps uygulama veya bir Azure sanal Makinesinde çalışan bir uygulamayı barındırmak için veya bir Azure Data Factory veri yönetimi ağ geçidi bağlanmak için. Kaynakların bu bağlantıları etkinleştirmek için aynı sanal ağ (VNet) veya kaynak grubu için güvenlik duvarı kuralı olması gerekmez. Azure’dan bir uygulama, veritabanı sunucunuza bağlanmayı denediğinizde güvenlik duvarı Azure bağlantılarına izin verildiğini doğrular. Birkaç bu tür bağlantıları etkinleştirmek için yöntem vardır. Başlangıç ve bitiş adresi 0.0.0.0’a eşit olan bir güvenlik duvarı ayarı, bu bağlantılara izin verildiğini gösterir. Alternatif olarak, ayarlayabileceğiniz **Azure hizmetlerine erişime izin ver** seçeneğini **ON** Portalı'nda **bağlantı güvenliği** bölmesi ve isabet **Kaydet**. Bağlantı denemesi izin verilmiyorsa, istek PostgreSQL sunucusu için Azure veritabanı ulaşmaz.

> [!IMPORTANT]
> Bu seçenek, diğer müşterilerin aboneliklerinden gelen bağlantılar dahil Azure’dan tüm bağlantılara izin verecek şekilde güvenlik duvarınızı yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.
> 

![Portalda Azure hizmetlerine erişime izin ver'ı yapılandırma](media/concepts-firewall-rules/allow-azure-services.png)

## <a name="programmatically-managing-firewall-rules"></a>Güvenlik duvarı kurallarını programlı bir şekilde yönetme
Azure portalına ek olarak, güvenlik duvarı kurallarını programlı bir şekilde Azure CLI kullanarak yönetilebilir.
Ayrıca bkz: [oluşturun ve Azure veritabanı Azure CLI kullanarak PostgreSQL için güvenlik duvarı kurallarını yönetme](howto-manage-firewall-using-cli.md)

## <a name="troubleshooting-the-database-server-firewall"></a>Veritabanı sunucusu güvenlik duvarı sorunlarını giderme
PostgreSQL Server hizmeti için Microsoft Azure veritabanına erişim beklediğiniz gibi davranmadığında aşağıdaki noktaları göz önünde bulundurun:

* **İzin verilenler listesine değişiklikler henüz etkili olmamıştır:** Bir beş dakikaya kadar değişikliklerin etkili olması için Azure veritabanı PostgreSQL sunucusu güvenlik duvarı yapılandırması olabilir.

* **Oturum açma yetkili değil veya yanlış parola kullanıldı:** Bir oturum açma, PostgreSQL sunucusu için Azure veritabanı izni yok veya kullanılan parola yanlışsa, PostgreSQL sunucusu için Azure veritabanı bağlantısı reddedildi. Bir güvenlik duvarı ayarının oluşturulması yalnızca istemcilere sunucunuzla bağlantı kurmayı deneme fırsatı sağlar. yine de her istemci gerekli güvenlik kimlik bilgilerini sağlamanız gerekir.

Örneğin, JDBC İstemcisi'ni kullanarak, aşağıdaki hata görünebilir.
> java.util.concurrent.ExecutionException: java.lang.RuntimeException: org.postgresql.util.PSQLException: Önemli: "kullanıcıadınız" kullanıcı için parola kimlik doğrulaması başarısız

* **Dinamik IP adresi:** Dinamik IP adresiyle kurulmuş bir Internet bağlantısı varsa ve güvenlik duvarını aşmakta sorun yaşıyorsanız aşağıdaki çözümlerden birini deneyebilirsiniz:

* PostgreSQL sunucusu için Azure veritabanına erişen istemci bilgisayarlarınıza atanmış IP adresi aralığını Internet servis sağlayıcınıza (ISS) sorun ve IP adres aralığı olarak bir güvenlik duvarı kuralı ekleyin.

* Statik IP yerine istemci bilgisayarlarınız için adresi alın ve ardından statik IP adresi olarak bir güvenlik duvarı kuralı ekleyin.

## <a name="next-steps"></a>Sonraki adımlar
Sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları oluşturma hakkında makaleler için bkz:
* [Oluşturma ve Azure veritabanı PostgreSQL güvenlik duvarı kuralları için Azure Portalı'nı kullanarak yönetme](howto-manage-firewall-using-portal.md)
* [Oluşturma ve Azure veritabanı Azure CLI kullanarak PostgreSQL için güvenlik duvarı kurallarını yönetme](howto-manage-firewall-using-cli.md)
