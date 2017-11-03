---
title: "Azure veritabanı PostgreSQL sunucunun güvenlik duvarı kuralları için | Microsoft Docs"
description: "Azure veritabanı PostgreSQL sunucu için güvenlik duvarı kuralları açıklar."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: a8e1980900b430e56b0c01a4446dc525d25698da
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
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
> java.util.concurrent.ExecutionException: java.lang.RuntimeException: org.postgresql.util.PSQLException: Önemli: hiçbir pg\_ana bilgisayar "123.45.67.890", kullanıcı "adminuser" veritabanı "postgresql", SSL için hba.conf giriş

## <a name="programmatically-managing-firewall-rules"></a>Güvenlik duvarı kurallarını programlı bir şekilde yönetme
Azure portalına ek olarak, güvenlik duvarı kuralları Azure CLI kullanarak programlı olarak yönetilebilir.
Ayrıca bkz. [oluşturma ve Azure veritabanı PostgreSQL güvenlik duvarı kuralları için Azure CLI kullanarak yönetme](howto-manage-firewall-using-cli.md)

## <a name="troubleshooting-the-database-server-firewall"></a>Veritabanı sunucusu güvenlik duvarı sorunlarını giderme
Beklediğiniz gibi Microsoft Azure veritabanına erişimi PostgreSQL Server hizmeti için davranıyor değil yaparken aşağıdaki noktaları dikkate alın:

* **İzin verilenler listesine olmayan değişikliklerin etkili henüz:** için beş dakikalık bir gecikmeyle Azure etkili olması için veritabanında PostgreSQL sunucu güvenlik duvarı yapılandırması için değişikliklerini kadar olabilir.

* **Oturum açma yetkili değil veya yanlış parola kullanıldı:** bir oturum açma izinleri PostgreSQL sunucu için Azure veritabanı yok veya kullanılan parola yanlış olmadığını PostgreSQL server için Azure veritabanına bağlantı reddedildi. Bir güvenlik duvarı ayarı oluşturma, yalnızca sunucunuza bağlanma girişiminde fırsat istemcilere sağlar; her istemci hala gerekli güvenlik kimlik bilgilerini sağlamanız gerekir.

Örneğin, bir JDBC İstemcisi'ni kullanarak aşağıdaki hata görünebilir.
> java.util.concurrent.ExecutionException: java.lang.RuntimeException: org.postgresql.util.PSQLException: Önemli: parola kimlik doğrulaması "kullanıcıadınız" kullanıcı için başarısız oldu

* **Dinamik IP adresi:** Dinamik IP adresiyle kurulmuş bir İnternet bağlantınız varsa ve güvenlik duvarını aşmakta sorun yaşıyorsanız aşağıdaki çözümlerden birini deneyebilirsiniz:

* İstemci bilgisayarlarınız için PostgreSQL sunucusu Azure veritabanına erişmek için atanan IP adresi aralığı için Internet servis sağlayıcısı (ISS) isteyin ve sonra IP adresi aralığı bir güvenlik duvarı kuralı ekleyin.

* Statik IP bunun yerine istemci bilgisayarlarınız için adresleme alın ve ardından statik IP adresi bir güvenlik duvarı kuralı ekleyin.

## <a name="next-steps"></a>Sonraki adımlar
Sunucu düzeyinde güvenlik duvarı kuralları oluşturma hakkında daha fazla makaleler için bkz:
* [Oluşturma ve Azure veritabanı PostgreSQL güvenlik duvarı kuralları için Azure Portalı'nı kullanarak yönetme](howto-manage-firewall-using-portal.md).
* [Oluşturma ve Azure veritabanı PostgreSQL güvenlik duvarı kuralları için Azure CLI kullanarak yönetme](howto-manage-firewall-using-cli.md).
