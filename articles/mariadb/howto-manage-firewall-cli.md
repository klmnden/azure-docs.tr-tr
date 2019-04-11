---
title: Oluşturma ve Azure veritabanı Azure CLI kullanarak MariaDB için güvenlik duvarı kurallarını yönetme
description: Bu makalede, oluşturma ve Azure veritabanı Azure CLI, komut satırı kullanarak MariaDB için güvenlik duvarı kurallarını yönetme işlemleri açıklanır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 04/09/2019
ms.openlocfilehash: 562987b953f0a8a20a917e208f43557bd768c0a0
ms.sourcegitcommit: 6e32f493eb32f93f71d425497752e84763070fad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2019
ms.locfileid: "59471163"
---
# <a name="create-and-manage-azure-database-for-mariadb-firewall-rules-by-using-the-azure-cli"></a>Oluşturma ve Azure veritabanı Azure CLI kullanarak MariaDB için güvenlik duvarı kurallarını yönetme
Sunucu düzeyinde güvenlik duvarı kuralları, erişim belirli bir IP adresi veya bir IP adresi aralığı MariaDB sunucusu için Azure veritabanı'na yönetmek için kullanılabilir. Uygun Azure CLI'si komutlarını kullanarak, oluşturabilir, güncelleştirin, silin, listeleyin ve sunucunuzu yönetmek için güvenlik duvarı kurallarını gösterir. Bir Azure veritabanı'nın için MariaDB güvenlik duvarları için bkz: genel bakış [MariaDB sunucu güvenlik duvarı kuralları için Azure veritabanı](./concepts-firewall-rules.md).

Sanal ağ (VNet) kuralları, sunucunuza erişim güvenliğini sağlamak için de kullanılabilir. Daha fazla bilgi edinin [oluşturma ve yönetme sanal ağ hizmet uç noktaları ve Azure CLI kullanarak kurallar](howto-manage-vnet-cli.md).

## <a name="prerequisites"></a>Önkoşullar
* [Azure CLI’yı yükleyin](https://docs.microsoft.com/cli/azure/install-azure-cli).
* Bir [MariaDB sunucu ve veritabanı için Azure veritabanı](quickstart-create-mariadb-server-database-using-azure-cli.md).

## <a name="firewall-rule-commands"></a>Güvenlik duvarı kuralı komutlar:
**Az mariadb sunucu güvenlik duvarı kuralı** oluştur, Sil, listesinde, görüntülemek ve güvenlik duvarı kurallarını güncelleştirmek için Azure CLI üzerinden komutu kullanılır.

Komutlar:
- **Oluşturma**: Bir Azure MariaDB sunucu güvenlik duvarı kuralı oluşturun.
- **Silme**: Bir Azure MariaDB sunucu güvenlik duvarı kuralını silin.
- **Liste**: Azure MariaDB sunucu güvenlik duvarı kuralları listesi.
- **Göster**: Güvenlik duvarı kuralı Azure MariaDB sunucusunun ayrıntılarını göster.
- **Güncelleştirme**: Bir Azure MariaDB sunucu güvenlik duvarı kuralı güncelleştirin.

## <a name="sign-in-to-azure-and-list-your-azure-database-for-mariadb-servers"></a>Azure'da oturum açın ve MariaDB sunucuları için Azure veritabanınızı listeleme
Azure CLI kullanarak güvenli bir şekilde Azure hesabınızla bağlanmanız **az login** komutu.

1. Komut satırından aşağıdaki komutu çalıştırın:
   ```azurecli
   az login
   ```
   Bu komut, sonraki adımda kullanmak üzere bir kod çıkarır.

2. Bir web tarayıcısı kullanarak [ https://aka.ms/devicelogin ](https://aka.ms/devicelogin)ve ardından kodu girin.

3. İstemde, Azure kimlik bilgilerinizi kullanarak oturum açın.

4. Oturum açma bilgilerinizi yetkilendirildikten sonra konsolda Aboneliklerin listesini yazdırılır. Geçerli bir abonelik kullanmak için ayarlanacak istediğiniz abonelik Kimliğini kopyalayın. Kullanım [az hesabı kümesi](/cli/azure/account#az-account-set) komutu.
   ```azurecli-interactive
   az account set --subscription <your subscription id>
   ```

5. Abonelik ve kaynak grubunuzun MariaDB sunucuları için Azure veritabanlarının adlarını değilseniz listeleyin. Kullanım [az mariadb sunucu listesi](/cli/azure/mariadb/server#az-mariadb-server-list) komutu.

   ```azurecli-interactive
   az mariadb server list --resource-group myresourcegroup
   ```

   MariaDB çalıştığı sunucuya belirtmeniz gereken listenin adı özniteliği unutmayın. Gerekirse, bu sunucu için ayrıntıları onaylayın ve doğru olduğundan emin olmak için ad özniteliğini kullanarak. Kullanım [az mariadb server show](/cli/azure/mariadb/server#az-mariadb-server-show) komutu.

   ```azurecli-interactive
   az mariadb server show --resource-group myresourcegroup --name mydemoserver
   ```

## <a name="list-firewall-rules-on-azure-database-for-mariadb-server"></a>MariaDB için Azure veritabanı güvenlik duvarı kurallarını Listele 
Sunucu adı ve kaynak grubu adını kullanarak, sunucudaki var olan sunucu güvenlik duvarı kuralları listesi. Kullanım [az mariadb sunucu güvenlik duvarı listesi](/cli/azure/mariadb/server/firewall-rule#az-mariadb-server-firewall-rule-list) komutu.  Belirtilen sunucu adı özniteliği bildirimi **--sunucu** geçiş değildir **--adı** geçin. 
```azurecli-interactive
az mariadb server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver
```
Çıkış JSON içindeki (varsayılan) biçimlendirme kuralları listeler. Kullanabileceğiniz **--çıktı tablosu** daha okunabilir bir tablo biçiminde sonuçlarını çıkarmak için anahtar.
```azurecli-interactive
az mariadb server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver --output table
```
## <a name="create-a-firewall-rule-on-azure-database-for-mariadb-server"></a>Bir güvenlik duvarı kuralı MariaDB sunucusu için Azure veritabanı oluşturma
Azure MariaDB sunucu adını ve kaynak grubu adını kullanarak sunucuda yeni bir güvenlik duvarı kuralı oluşturun. Kullanım [az mariadb sunucu güvenlik duvarı oluşturma](/cli/azure/mariadb/server/firewall-rule#az-mariadb-server-firewall-rule-create) komutu. Başlangıç IP yanı sıra, kural için bir ad sağlayın ve kural için (bir IP adresi aralığı erişim sağlamak için) IP bitmelidir.
```azurecli-interactive
az mariadb server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.15
```

Tek bir IP adresi için erişime izin vermek için bu örnekte olduğu gibi aynı IP adresi Başlangıç hem bitiş IP'si olarak sağlayın.
```azurecli-interactive
az mariadb server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 1.1.1.1 --end-ip-address 1.1.1.1
```

Uygulamaların Azure IP adreslerinden MariaDB için Azure veritabanı sunucunuza bağlanması için izin vermek için bu örnekte olduğu gibi IP adresi olarak Başlangıç hem bitiş IP'si 0.0.0.0 sağlar.
```azurecli-interactive
az mariadb server firewall-rule create --resource-group myresourcegroup --server mariadb --name "AllowAllWindowsAzureIps" --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!IMPORTANT]
> Bu seçenek, diğer müşterilerin aboneliklerinden gelen bağlantılar dahil Azure’dan tüm bağlantılara izin verecek şekilde güvenlik duvarınızı yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.
> 

Başarılı olduktan sonra her oluşturma komutu çıkış JSON biçiminde (varsayılan) oluşturduğunuz güvenlik duvarı kuralı ayrıntılarını listeler. Bir hata varsa, çıktı yerine hata iletisi metni gösterir.

## <a name="update-a-firewall-rule-on-azure-database-for-mariadb-server"></a>MariaDB sunucusu için Azure veritabanı üzerinde bir güvenlik duvarı kuralını güncelleştir 
Azure MariaDB sunucu adını ve kaynak grubu adını kullanarak sunucuda mevcut bir güvenlik duvarı kuralı güncelleştirin. Kullanım [az mariadb sunucu Güvenlik Duvarı'nı güncelleştirme](/cli/azure/mariadb/server/firewall-rule#az-mariadb-server-firewall-rule-update) komutu. Mevcut güvenlik duvarı kuralı adı güncelleştirmek için IP ve bitiş IP öznitelikler giriş, aynı zamanda başlangıç sağlar.
```azurecli-interactive
az mariadb server firewall-rule update --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.1
```
Başarılı olduktan sonra komut çıktısı (varsayılan) JSON biçiminde güncelleştirdik güvenlik duvarı kuralı ayrıntılarını listeler. Bir hata varsa, çıktı yerine hata iletisi metni gösterir.

> [!NOTE]
> Güvenlik duvarı kuralı mevcut değilse, kuralı güncelleştirme komutu tarafından oluşturulur.

## <a name="show-firewall-rule-details-on-azure-database-for-mariadb-server"></a>Güvenlik duvarı kural ayrıntılarını MariaDB sunucusu için Azure veritabanı Göster
Azure MariaDB sunucu adını ve kaynak grubu adını kullanarak, mevcut güvenlik duvarı kuralı ayrıntılarını sunucudan gösterir. Kullanım [az mariadb sunucu güvenlik duvarı show](/cli/azure/mariadb/server/firewall-rule#az-mariadb-server-firewall-rule-show) komutu. Mevcut güvenlik duvarı kuralı adı giriş olarak sağlayın.
```azurecli-interactive
az mariadb server firewall-rule show --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1
```
Başarılı olduktan sonra komut çıktısı, JSON biçiminde (varsayılan), belirtmiş olduğunuz güvenlik duvarı kuralı ayrıntılarını listeler. Bir hata varsa, çıktı yerine hata iletisi metni gösterir.

## <a name="delete-a-firewall-rule-on-azure-database-for-mariadb-server"></a>MariaDB sunucusu için Azure veritabanı üzerinde bir güvenlik duvarı kuralını Sil
Azure MariaDB sunucu adını ve kaynak grubu adını kullanarak mevcut bir güvenlik duvarı kuralı, söz konusu sunucudan kaldırın. Kullanım [az mariadb sunucu güvenlik duvarı Sil](/cli/azure/mariadb/server/firewall-rule#az-mariadb-server-firewall-rule-delete) komutu. Mevcut güvenlik duvarı kuralı adını belirtin.
```azurecli-interactive
az mariadb server firewall-rule delete --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1
```
Başarılı olduktan sonra hiçbir çıktı yok. Başarısızlık durumunda, hata iletisi metni görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar
- Hakkında daha fazla bilgi edinin [MariaDB sunucu güvenlik duvarı kuralları için Azure veritabanı](./concepts-firewall-rules.md).
- [Oluşturma ve Azure veritabanı Azure portalını kullanarak MariaDB için güvenlik duvarı kurallarını yönetme](./howto-manage-firewall-portal.md).
- Daha fazla güvenli sunucunuz tarafından erişim [oluşturma ve yönetme sanal ağ hizmet uç noktaları ve Azure CLI kullanarak kurallar](howto-manage-vnet-cli.md).
