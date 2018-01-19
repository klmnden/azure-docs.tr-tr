---
title: "Oluşturma ve Azure veritabanı PostgreSQL güvenlik duvarı kuralları için Azure CLI kullanarak yönetme | Microsoft Docs"
description: "Bu makalede, oluşturma ve Azure veritabanı PostgreSQL güvenlik duvarı kuralları için Azure CLI komut satırını kullanarak yönetme açıklar."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 01/18/2018
ms.openlocfilehash: 233479bff0a0d331161b52d33f31b45c5714362b
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-azure-cli"></a>Oluşturma ve Azure veritabanı PostgreSQL güvenlik duvarı kuralları için Azure CLI kullanarak yönetme
Bir Azure veritabanına PostgreSQL sunucusu için belirli bir IP adresi veya IP adresi aralığı erişimi yönetmek üzere yöneticiler sunucu düzeyinde güvenlik duvarı kuralları etkinleştirin. Uygun Azure CLI komutları kullanarak, oluşturabilir, güncelleştirme, silin, listeleyin ve sunucunuzu yönetmek için güvenlik duvarı kuralları gösterir. Genel Bakış Azure veritabanı için PostgreSQL güvenlik duvarı kuralları için bkz: [PostgreSQL sunucunun güvenlik duvarı kuralları için Azure veritabanı](concepts-firewall-rules.md)

## <a name="prerequisites"></a>Önkoşullar
Nasıl yapılır bu kılavuzu adım için gerekir:
- Bir [PostgreSQL sunucu ve veritabanı için Azure veritabanı](quickstart-create-server-database-azure-cli.md).
- Yükleme [Azure CLI 2.0](/cli/azure/install-azure-cli) komut satırı yardımcı programı veya tarayıcıda Azure bulut Kabuğu'nu kullanın.

## <a name="configure-firewall-rules-for-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı için güvenlik duvarı kurallarını yapılandırma
[Az postgres sunucu güvenlik duvarı kuralı](/cli/azure/postgres/server/firewall-rule) komutlar, güvenlik duvarı kurallarını yapılandırmak için kullanılır.

## <a name="list-firewall-rules"></a>Liste güvenlik duvarı kuralları 
Var olan sunucu güvenlik duvarı kuralları listelemek için Çalıştır [az postgres sunucu güvenlik duvarı kuralı listesi](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_list) komutu.
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server-name mypgserver-20170401
```
Varsa, varsayılan değer JSON olarak biçimlendirmek istiyorsanız çıkış güvenlik duvarı kuralları listeler. Anahtar kullanabilir `--output table` çıktı olarak daha okunabilir bir tablo biçiminde için.
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server-name mypgserver-20170401 --output table
```
## <a name="create-firewall-rule"></a>Güvenlik duvarı kuralı oluşturma
Sunucu üzerinde yeni bir güvenlik duvarı kuralı oluşturmak için çalıştırın [az postgres sunucu güvenlik duvarı kuralı oluşturmak](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_create) komutu. 

0.0.0.0 olarak belirterek `--start-ip-address` ve 255.255.255.255 olarak `--end-ip-address` aralığı, aşağıdaki örnekte sağlar sunucuya erişmek tüm IP adresleri **mypgserver 20170401.postgres.database.azure.com**
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server-name mypgserver-20170401 --name AllowIpRange --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```
Tekil bir IP adresi erişmesine izin vermek için aynı adres sağlamak `--start-ip-address` ve `--end-ip-address`, bu örnekteki gibi.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server-name mypgserver-20170401 --name AllowSingleIpAddress --start-ip-address 13.83.152.1 --end-ip-address 13.83.152.1
```
Başarılı, komut çıktısı JSON biçiminde varsayılan olarak, oluşturmuş olduğunuz güvenlik duvarı kuralı ayrıntılarını listeler. Çıkış hatası varsa, bir hata iletisi bunun yerine gösterir.

## <a name="update-firewall-rule"></a>Güncelleştirme güvenlik duvarı kuralı 
Kullanarak sunucu üzerinde var olan bir güvenlik duvarı kuralı güncelleştirme [az postgres sunucu güvenlik duvarı kuralı güncelleştirme](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_update) komutu. Var olan güvenlik duvarı kuralı adı girişi ve başlangıç güncelleştirmek için IP ve bitiş IP öznitelikleri sağlar.
```azurecli-interactive
az postgres server firewall-rule update --resource-group myresourcegroup --server-name mypgserver-20170401 --name AllowIpRange --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.255
```
Başarılı, komut çıktısı varsayılan olarak JSON biçiminde güncelleştirdiniz güvenlik duvarı kuralı ayrıntılarını listeler. Çıkış hatası varsa, bir hata iletisi bunun yerine gösterir.
> [!NOTE]
> Güvenlik duvarı kuralı mevcut değilse güncelleştirme komutu tarafından oluşturulan.

## <a name="show-firewall-rule-details"></a>Güvenlik duvarı kuralı ayrıntıları göster
Çalıştırarak da mevcut bir sunucu düzeyinde güvenlik duvarı kuralı ayrıntılarını gösterebilirsiniz [az postgres sunucu güvenlik duvarı kuralı Göster](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_show) komutu.
```azurecli-interactive
az postgres server firewall-rule show --resource-group myresourcegroup --server-name mypgserver-20170401 --name AllowIpRange
```
Başarılı, komut çıktısı JSON biçiminde varsayılan olarak belirttiğiniz güvenlik duvarı kuralı ayrıntılarını listeler. Çıkış hatası varsa, bir hata iletisi bunun yerine gösterir.

## <a name="delete-firewall-rule"></a>Güvenlik duvarı kuralını siler
Bir IP aralığı sunucuya erişimi iptal etmek için mevcut bir güvenlik duvarı kuralı yürüterek silme [az postgres sunucu güvenlik duvarı kuralı silme](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_delete) komutu. Var olan güvenlik duvarı kuralının adını sağlayın.
```azurecli-interactive
az postgres server firewall-rule delete --resource-group myresourcegroup --server-name mypgserver-20170401 --name AllowIpRange
```
Başarı hiçbir çıktısı yok. Başarısızlık durumunda, hata iletisi metni döndürülür.

## <a name="next-steps"></a>Sonraki adımlar
- Benzer şekilde, bir web tarayıcısı kullanabilirsiniz [oluşturma ve PostgreSQL güvenlik duvarı kuralları için Azure portalını kullanarak Azure veritabanını yönetme](howto-manage-firewall-using-portal.md).
- Hakkında daha iyi anlamak [Azure veritabanı PostgreSQL sunucunun güvenlik duvarı kuralları için](concepts-firewall-rules.md).
- PostgreSQL sunucu için bir Azure veritabanına bağlanma konusunda yardım için bkz: [PostgreSQL için Azure veritabanı için bağlantı kitaplıkları](concepts-connection-libraries.md).
