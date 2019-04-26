---
title: Oluşturma ve Azure veritabanı Azure CLI kullanarak PostgreSQL için güvenlik duvarı kurallarını yönetme
description: Bu makalede, oluşturma ve Azure veritabanı Azure CLI komut satırını kullanarak PostgreSQL için güvenlik duvarı kurallarını yönetme işlemleri açıklanır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 04/09/2019
ms.openlocfilehash: 9a96361d3fb155ea5b400990690e3c2b1f65f819
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60422381"
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-azure-cli"></a>Oluşturma ve Azure veritabanı Azure CLI kullanarak PostgreSQL için güvenlik duvarı kurallarını yönetme
Sunucu düzeyinde güvenlik duvarı kuralları, erişim belirli bir IP adresi veya IP adresi aralığı PostgreSQL sunucusu için Azure veritabanı'na yönetmek için kullanılabilir. Uygun Azure CLI'si komutlarını kullanarak, oluşturabilir, güncelleştirin, silin, listeleyin ve sunucunuzu yönetmek için güvenlik duvarı kurallarını gösterir. Bir Azure veritabanı'nın için PostgreSQL güvenlik duvarı kuralları için bkz: genel bakış [PostgreSQL sunucusu güvenlik duvarı kuralları için Azure veritabanı](concepts-firewall-rules.md).

Sanal ağ (VNet) kuralları, sunucunuza erişim güvenliğini sağlamak için de kullanılabilir. Daha fazla bilgi edinin [oluşturma ve yönetme sanal ağ hizmet uç noktaları ve Azure CLI kullanarak kurallar](howto-manage-vnet-using-cli.md).

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda adımlamak için ihtiyacınız vardır:
- Yükleme [Azure CLI](/cli/azure/install-azure-cli) komut satırı yardımcı programı veya tarayıcıda Azure Cloud Shell kullanın.
- Bir [PostgreSQL sunucusu ve veritabanı için Azure veritabanı](quickstart-create-server-database-azure-cli.md).

## <a name="configure-firewall-rules-for-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı için güvenlik duvarı kuralları yapılandırma
[Az postgres server güvenlik duvarı kuralı](/cli/azure/postgres/server/firewall-rule) komutları, güvenlik duvarı kurallarını yapılandırmak için kullanılır.

## <a name="list-firewall-rules"></a>Güvenlik duvarı kurallarını Listele 
Mevcut sunucu güvenlik duvarı kurallarını listeleme için çalıştırın [az postgres server güvenlik duvarı kuralı listesi](/cli/azure/postgres/server/firewall-rule) komutu.
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver
```
Varsa, varsayılan değer JSON olarak biçimlendirmek istiyorsanız çıkış güvenlik duvarı kuralları listeler. Anahtar kullanabilir `--output table` daha okunabilir bir tablo biçiminde çıktı olarak için.
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver --output table
```
## <a name="create-firewall-rule"></a>Güvenlik duvarı kuralı oluşturma
Sunucuda yeni bir güvenlik duvarı kuralı oluşturmak için çalıştırın [az postgres server-güvenlik duvarı oluşturma](/cli/azure/postgres/server/firewall-rule) komutu. 

```
To allow access to a singular IP address, provide the same address in the `--start-ip-address` and `--end-ip-address`, as in this example, replacing the IP shown here with your specific IP.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name AllowSingleIpAddress --start-ip-address 13.83.152.1 --end-ip-address 13.83.152.1
```
PostgreSQL için Azure veritabanı sunucunuza bağlanmak Azure IP adreslerinden gelen uygulamalara izin vermek üzere, bu örnekte olduğu gibi IP adresi olarak Başlangıç hem bitiş IP'si 0.0.0.0 sağlar.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name AllowAllAzureIps --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!IMPORTANT]
> Bu seçenek, diğer müşterilerin aboneliklerinden gelen bağlantılar dahil Azure’dan tüm bağlantılara izin verecek şekilde güvenlik duvarınızı yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.
> 

Başarılı olduktan sonra komut çıktısı varsayılan olarak JSON biçiminde, oluşturmuş olduğunuz güvenlik duvarı kuralı ayrıntılarını listeler. Bir hata varsa, çıktı bunun yerine bir hata iletisi gösterir.

## <a name="update-firewall-rule"></a>Güvenlik duvarı kuralını güncelleştir 
Sunucu kullanarak mevcut bir güvenlik duvarı kuralını Güncelleştir [az postgres server güvenlik duvarı kuralı güncelleştirme](/cli/azure/postgres/server/firewall-rule) komutu. Mevcut güvenlik duvarı kuralı adı girişi ve başlangıç güncelleştirmek için IP ve bitiş IP öznitelikler sağlar.
```azurecli-interactive
az postgres server firewall-rule update --resource-group myresourcegroup --server-name mydemoserver --name AllowIpRange --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.0
```
Başarılı olduktan sonra komut çıktısı varsayılan olarak JSON biçiminde güncelleştirdik güvenlik duvarı kuralı ayrıntılarını listeler. Bir hata varsa, çıktı bunun yerine bir hata iletisi gösterir.
> [!NOTE]
> Güvenlik duvarı kuralı mevcut değilse güncelleştirme komutu tarafından oluşturulan.

## <a name="show-firewall-rule-details"></a>Güvenlik duvarı kural ayrıntılarını göster
Mevcut bir sunucu düzeyinde güvenlik duvarı kuralı ayrıntılarını çalıştırarak da gösterebilirsiniz [az postgres server güvenlik duvarı kuralı show](/cli/azure/postgres/server/firewall-rule) komutu.
```azurecli-interactive
az postgres server firewall-rule show --resource-group myresourcegroup --server-name mydemoserver --name AllowIpRange
```
Başarılı olduktan sonra komut çıktısı varsayılan olarak JSON biçiminde belirttiğiniz güvenlik duvarı kuralı ayrıntılarını listeler. Bir hata varsa, çıktı bunun yerine bir hata iletisi gösterir.

## <a name="delete-firewall-rule"></a>Güvenlik duvarı kuralını Sil
Sunucu bir IP aralığına erişimi iptal etmek için mevcut bir güvenlik duvarı kuralını silin [az postgres server güvenlik duvarı kuralını Sil](/cli/azure/postgres/server/firewall-rule) komutu. Mevcut güvenlik duvarı kuralı adını belirtin.
```azurecli-interactive
az postgres server firewall-rule delete --resource-group myresourcegroup --server-name mydemoserver --name AllowIpRange
```
Başarılı olduktan sonra hiçbir çıktı yok. Başarısızlık durumunda, hata iletisi metni döndürülür.

## <a name="next-steps"></a>Sonraki adımlar
- Benzer şekilde, bir web tarayıcısı kullanabilirsiniz [oluşturun ve Azure portalını kullanarak PostgreSQL güvenlik duvarı kuralları için Azure veritabanı'nı yönetme](howto-manage-firewall-using-portal.md).
- Hakkında daha fazla bilgi edinin [PostgreSQL sunucusu güvenlik duvarı kuralları için Azure veritabanı](concepts-firewall-rules.md).
- Daha fazla güvenli sunucunuz tarafından erişim [oluşturma ve yönetme sanal ağ hizmet uç noktaları ve Azure CLI kullanarak kurallar](howto-manage-vnet-using-cli.md).
- PostgreSQL sunucusu için Azure veritabanı bağlanma konusunda yardım için bkz. [PostgreSQL için Azure veritabanı için bağlantı kitaplıkları](concepts-connection-libraries.md).
