---
title: "Azure veritabanı için MySQL server güvenlik duvarı kurallarını | Microsoft Docs"
description: "MySQL sunucusu için Azure veritabanınız için güvenlik duvarı kuralları açıklar."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 01/20/2018
ms.openlocfilehash: 15bf032280c9a1d874daa77a6351e092392fee05
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2018
---
# <a name="azure-database-for-mysql-server-firewall-rules"></a>Azure veritabanı için MySQL server güvenlik duvarı kuralları
Hangi bilgisayarların izniniz belirtene kadar güvenlik duvarları, veritabanı sunucunuza tüm erişimi engelliyor. Güvenlik Duvarı'nı her isteğin kaynak IP adresine göre sunucuya erişimi verir.

Bir Güvenlik Duvarı'nı yapılandırmak için kabul edilebilir IP adreslerinin aralıklarını belirtmek güvenlik duvarı kuralları oluşturun. Sunucu düzeyinde güvenlik duvarı kuralları oluşturabilirsiniz.

**Güvenlik duvarı kuralları:** MySQL sunucusu için tüm Azure veritabanınıza erişmek diğer bir deyişle, tüm veritabanları aynı mantıksal sunucu içindeki istemcileri bu kuralları etkinleştirin. Sunucu düzeyinde güvenlik duvarı kuralları, Azure portalında veya Azure CLI komutları kullanarak yapılandırılabilir. Sunucu düzeyinde güvenlik duvarı kuralları oluşturmak için abonelik sahibi veya abonelik Katılımcısı olması gerekir.

## <a name="firewall-overview"></a>Güvenlik duvarına genel bakış
Azure veritabanınızı tüm veritabanı erişimi MySQL sunucusu için güvenlik duvarı tarafından engellenmediğinden varsayılan olarak açıktır. Başka bir bilgisayardan sunucunuzu kullanmaya başlamak için sunucunuza erişimi etkinleştirmek için bir veya daha fazla sunucu düzeyinde güvenlik duvarı kuralları belirtmeniz gerekir. Adres aralıklarını izin vermek için Internet'ten hangi IP belirtmek için güvenlik duvarı kurallarını kullanın. Azure Portalı Web sitesine erişim kendisi güvenlik duvarı kuralları tarafından etkilenmez.

MySQL veritabanı için Azure veritabanınızı ulaşmadan bağlantı denemeleri Internet ve Azure ilk güvenlik duvarı aracılığıyla aşağıdaki çizimde gösterildiği gibi geçmesi gerekir:

![Örnek akış Güvenlik Duvarı nasıl çalışır](./media/concepts-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-the-internet"></a>İnternet'ten bağlanma
MySQL sunucusu için Azure veritabanı tüm veritabanları için sunucu düzeyinde güvenlik duvarı kuralları uygulanır.

İsteğin IP adresi sunucu düzeyinde güvenlik duvarı kurallarında belirtilen aralıklardan biri içinde ise, bağlantı verilir.

İsteğin IP adresi herhangi bir veritabanı düzeyi veya sunucu düzeyinde güvenlik duvarı kuralları içinde belirtilen aralıklarda dışında ise, bağlantı isteği başarısız olur.

## <a name="connecting-from-azure"></a>Azure'dan bağlanma
MySQL sunucusu için Azure veritabanınıza bağlanmak üzere Azure uygulamalardan izin vermek için Azure bağlantıları etkinleştirilmesi gerekir. Örneğin, bir Azure Web Apps uygulama veya bir Azure VM içinde çalışan bir uygulamayı barındırmak için veya bir Azure Data Factory veri yönetimi ağ geçidi'nden bağlanma. Kaynaklar aynı sanal ağ (VNET) veya kaynak grubu güvenlik duvarı kuralı için bu bağlantıları etkinleştirmek için olması gerekmez. Azure’dan bir uygulama, veritabanı sunucunuza bağlanmayı denediğinizde güvenlik duvarı Azure bağlantılarına izin verildiğini doğrular. Birkaç bu tür bağlantıları etkinleştirmek için yöntem vardır. Başlangıç ve bitiş adresi 0.0.0.0’a eşit olan bir güvenlik duvarı ayarı, bu bağlantılara izin verildiğini gösterir. Alternatif olarak, ayarlayabileceğiniz **Azure hizmetlerine erişime izin ver** için seçenek **ON** Portalı'nda **bağlantı güvenliği** bölmesinde ve isabet **Kaydet**. Bağlantı denemesi izin verilmiyorsa, Azure veritabanı MySQL sunucusu için istek ulaşmaz.

> [!IMPORTANT]
> Bu seçenek, diğer müşterilerin aboneliklerinden gelen bağlantılar dahil Azure’dan tüm bağlantılara izin verecek şekilde güvenlik duvarınızı yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.
> 

![Azure hizmetlerine erişime izin ver portalında yapılandırın](./media/concepts-firewall-rules/allow-azure-services.png)

## <a name="programmatically-managing-firewall-rules"></a>Güvenlik duvarı kurallarını programlı bir şekilde yönetme
Azure portalına ek Azure CLI kullanarak güvenlik duvarı kuralları programlı olarak yönetilebilir. Ayrıca bkz. [oluşturma ve Azure veritabanı için MySQL güvenlik duvarı kuralları Azure CLI kullanarak yönetme](./howto-manage-firewall-using-cli.md)

## <a name="troubleshooting-the-database-firewall"></a>Veritabanı güvenlik duvarı sorunlarını giderme
Microsoft Azure veritabanına erişimi MySQL server hizmeti için beklendiği gibi davranıyor değil yaparken aşağıdaki noktaları dikkate alın:

* **İzin verilenler listesine olmayan değişikliklerin etkili henüz:** için beş dakikalık bir gecikmeyle Azure etkili olması için veritabanı için MySQL Server güvenlik duvarı yapılandırma değişikliklerini kadar olabilir.

* **Oturum açma yetkili değil veya yanlış parola kullanıldı:** bir oturum açma, MySQL sunucusu için Azure veritabanı izinleri yok veya kullanılan parola yanlış olmadığını MySQL sunucusu için Azure veritabanına bağlantı reddedildi. Bir güvenlik duvarı ayarının oluşturulması yalnızca istemcilere sunucunuzla bağlantı kurmayı deneme fırsatı sunar; her istemci gerekli güvenlik kimlik bilgilerini belirtmek zorundadır.

* **Dinamik IP adresi:** dinamik IP adresleme ile Internet bağlantınız varsa ve güvenlik duvarı üzerinden alma sorun yaşıyorsanız, aşağıdaki çözümlerden birini deneyebilirsiniz:

* MySQL sunucusu için Azure veritabanına erişen istemci bilgisayarlara atanan IP adresi aralığı için Internet servis sağlayıcısı (ISS) isteyin ve sonra IP adresi aralığı bir güvenlik duvarı kuralı ekleyin.

* İstemci bilgisayarlarınız için bunun yerine statik IP adresi alın ve IP adreslerini güvenlik duvarı kuralları olarak ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

[Oluşturma ve Azure veritabanı için MySQL güvenlik duvarı kuralları Azure Portalı'nı kullanarak yönetme](./howto-manage-firewall-using-portal.md)
[oluşturma ve Azure veritabanı için MySQL güvenlik duvarı kuralları Azure CLI kullanarak yönetme](./howto-manage-firewall-using-cli.md)
