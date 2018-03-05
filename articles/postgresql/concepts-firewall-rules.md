---
title: "Azure veritabanı PostgreSQL sunucunun güvenlik duvarı kuralları için"
description: "Bu makalede, Azure veritabanınızın PostgreSQL sunucu için güvenlik duvarı kuralları açıklanır."
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 8a3f5d9fa8f1c36d8468c38f7dda803d3ca1d832
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="azure-database-for-postgresql-server-firewall-rules"></a>Azure veritabanı PostgreSQL sunucunun güvenlik duvarı kuralları için
Hangi bilgisayarların izniniz belirtmediğiniz sürece PostgreSQL sunucu Güvenlik Duvarı'nda azure veritabanı, veritabanı sunucunuza tüm erişimi engeller. Güvenlik Duvarı'nı her isteğin kaynak IP adresine göre sunucuya erişimi verir.
Güvenlik duvarınızı yapılandırmak için kabul edilebilir IP adreslerinin aralıklarını belirten güvenlik duvarı kuralları oluşturun. Sunucu düzeyinde güvenlik duvarı kuralları oluşturabilirsiniz.

**Güvenlik duvarı kuralları:** PostgreSQL sunucusu için tüm Azure veritabanınıza erişmek diğer bir deyişle, tüm veritabanları aynı mantıksal sunucu içindeki istemcileri bu kuralları etkinleştirin. Sunucu düzeyinde güvenlik duvarı kuralları, Azure portalını kullanarak veya Azure CLI komutları kullanarak yapılandırılabilir. Sunucu düzeyinde güvenlik duvarı kuralları oluşturmak için abonelik sahibi veya abonelik Katılımcısı olması gerekir.

## <a name="firewall-overview"></a>Güvenlik duvarına genel bakış
Azure veritabanınızı tüm veritabanı erişimi PostgreSQL sunucusu için varsayılan olarak güvenlik duvarı tarafından engellenir. Başka bir bilgisayardan sunucunuzu kullanmaya başlamak için sunucunuza erişimi etkinleştirmek için bir veya daha fazla sunucu düzeyinde güvenlik duvarı kuralları belirtmeniz gerekir. Adres aralıklarını izin vermek için Internet'ten hangi IP belirtmek için güvenlik duvarı kurallarını kullanın. Azure Portalı Web sitesine erişim kendisi güvenlik duvarı kuralları tarafından etkilenmez.
PostgreSQL veritabanınızı ulaşmadan bağlantı denemeleri Internet ve Azure ilk güvenlik duvarı aracılığıyla aşağıdaki çizimde gösterildiği gibi geçmesi gerekir:

![Örnek akış Güvenlik Duvarı nasıl çalışır](media/concepts-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-the-internet"></a>İnternet'ten bağlanma
Sunucu düzeyinde güvenlik duvarı kuralları PostgreSQL sunucusu için aynı Azure veritabanı tüm veritabanları için geçerlidir. İstek IP adresi sunucu düzeyinde güvenlik duvarı kurallarında belirtilen aralıklardan biri içindeyse, bağlantı izni verilir.
IP adresi isteğin herhangi bir sunucu düzeyinde güvenlik duvarı kuralları içinde belirtilen aralık içinde değil bağlantı isteği başarısız olur.
Örneğin, uygulamanız için PostgreSQL JDBC sürücüsü ile bağlanıyorsa, güvenlik duvarı bağlantıyı engelliyor olduğunda bağlanmaya çalışırken bu hatayla karşılaşabilirsiniz.
> java.util.concurrent.ExecutionException: java.lang.RuntimeException: org.postgresql.util.PSQLException: FATAL: no pg\_hba.conf entry for host "123.45.67.890", user "adminuser", database "postgresql", SSL

## <a name="connecting-from-azure"></a>Azure'dan bağlanma
Azure uygulamalardan PostgreSQL server için Azure veritabanına bağlanmak izin vermek için Azure bağlantıları etkinleştirilmesi gerekir. Örneğin, bir Azure Web Apps uygulama veya bir Azure VM içinde çalışan bir uygulamayı barındırmak için veya bir Azure Data Factory veri yönetimi ağ geçidi'nden bağlanma. Kaynakların bu bağlantıları etkinleştirmek için aynı sanal ağ (VNet) veya kaynak grubu için güvenlik duvarı kuralı olması gerekmez. Azure’dan bir uygulama, veritabanı sunucunuza bağlanmayı denediğinizde güvenlik duvarı Azure bağlantılarına izin verildiğini doğrular. Birkaç bu tür bağlantıları etkinleştirmek için yöntem vardır. Başlangıç ve bitiş adresi 0.0.0.0’a eşit olan bir güvenlik duvarı ayarı, bu bağlantılara izin verildiğini gösterir. Alternatif olarak, ayarlayabileceğiniz **Azure hizmetlerine erişime izin ver** için seçenek **ON** Portalı'nda **bağlantı güvenliği** bölmesinde ve isabet **Kaydet**. Bağlantı denemesi izin verilmiyorsa, istek PostgreSQL sunucu için Azure veritabanı ulaşmaz.

> [!IMPORTANT]
> Bu seçenek, diğer müşterilerin aboneliklerinden gelen bağlantılar dahil Azure’dan tüm bağlantılara izin verecek şekilde güvenlik duvarınızı yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.
> 

![Azure hizmetlerine erişime izin ver portalında yapılandırın](media/concepts-firewall-rules/allow-azure-services.png)

## <a name="programmatically-managing-firewall-rules"></a>Güvenlik duvarı kurallarını programlı bir şekilde yönetme
Azure portalına ek olarak, güvenlik duvarı kuralları Azure CLI kullanarak programlı olarak yönetilebilir.
Ayrıca bkz. [oluşturma ve Azure veritabanı PostgreSQL güvenlik duvarı kuralları için Azure CLI kullanarak yönetme](howto-manage-firewall-using-cli.md)

## <a name="troubleshooting-the-database-server-firewall"></a>Veritabanı sunucusu güvenlik duvarı sorunlarını giderme
Beklediğiniz gibi Microsoft Azure veritabanına erişimi PostgreSQL Server hizmeti için davranıyor değil yaparken aşağıdaki noktaları dikkate alın:

* **İzin verilenler listesine olmayan değişikliklerin etkili henüz:** için beş dakikalık bir gecikmeyle Azure etkili olması için veritabanında PostgreSQL sunucu güvenlik duvarı yapılandırması için değişikliklerini kadar olabilir.

* **Oturum açma yetkili değil veya yanlış parola kullanıldı:** bir oturum açma izinleri PostgreSQL sunucu için Azure veritabanı yok veya kullanılan parola yanlış olmadığını PostgreSQL server için Azure veritabanına bağlantı reddedildi. Bir güvenlik duvarı ayarı oluşturma, yalnızca sunucunuza bağlanma girişiminde fırsat istemcilere sağlar; her istemci hala gerekli güvenlik kimlik bilgilerini sağlamanız gerekir.

Örneğin, bir JDBC İstemcisi'ni kullanarak aşağıdaki hata görünebilir.
> java.util.concurrent.ExecutionException: java.lang.RuntimeException: org.postgresql.util.PSQLException: FATAL: password authentication failed for user "yourusername"

* **Dinamik IP adresi:** Dinamik IP adresiyle kurulmuş bir İnternet bağlantınız varsa ve güvenlik duvarını aşmakta sorun yaşıyorsanız aşağıdaki çözümlerden birini deneyebilirsiniz:

* İstemci bilgisayarlarınız için PostgreSQL sunucusu Azure veritabanına erişmek için atanan IP adresi aralığı için Internet servis sağlayıcısı (ISS) isteyin ve sonra IP adresi aralığı bir güvenlik duvarı kuralı ekleyin.

* Statik IP bunun yerine istemci bilgisayarlarınız için adresleme alın ve ardından statik IP adresi bir güvenlik duvarı kuralı ekleyin.

## <a name="next-steps"></a>Sonraki adımlar
Sunucu ve veritabanı düzeyi güvenlik duvarı kuralları oluşturma hakkında daha fazla makaleler için bkz:
* [Oluşturma ve Azure veritabanı PostgreSQL güvenlik duvarı kuralları için Azure Portalı'nı kullanarak yönetme](howto-manage-firewall-using-portal.md)
* [Oluşturma ve Azure veritabanı PostgreSQL güvenlik duvarı kuralları için Azure CLI kullanarak yönetme](howto-manage-firewall-using-cli.md)
