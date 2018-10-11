---
title: 'Hızlı Başlangıç: MariaDB için Azure Veritabanı sunucusu oluşturma - Azure CLI'
description: Bu hızlı başlangıçta, Azure CLI aracını kullanarak bir Azure kaynak grubunda nasıl MariaDB için Azure Veritabanı sunucusu oluşturabileceğiniz açıklanır.
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.devlang: azure-cli
ms.topic: quickstart
ms.date: 09/24/2018
ms.custom: mvc
ms.openlocfilehash: 9e87dacb80aa7fc5f5b073e631fc06ae74b3ad00
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46996613"
---
# <a name="create-an-azure-database-for-mariadb-server-using-azure-cli"></a>Azure CLI kullanarak MariaDB için Azure Veritabanı sunucusu oluşturma
Bu hızlı başlangıçta, Azure CLI aracını kullanarak bir Azure kaynak grubunda yaklaşık beş dakikada nasıl MariaDB için Azure Veritabanı sunucusu oluşturabileceğiniz açıklanır. Azure CLI, komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

Birden fazla aboneliğiniz varsa kaynağın mevcut olduğu ve faturalandırıldığı uygun aboneliği seçin. [az account set](/cli/azure/account#az-account-set) komutunu kullanarak hesabınız altındaki belirli bir abonelik kimliğini seçin.
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
[az group create](/cli/azure/group#az-group-create) komutunu kullanarak bir [Azure kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) oluşturun. Kaynak grubu, Azure kaynaklarının grup olarak dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnek `westus` konumunda `myresourcegroup` adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-mariadb-server"></a>MariaDB için Azure Veritabanı sunucusu oluşturma
**[az mariadb server create](/cli/azure/mariadb/server#az-mariadb-server-create)** komutunu kullanarak MariaDB için Azure Veritabanı sunucusu oluşturun. Bir sunucu birden çok veritabanını yönetebilir. Genellikle her proje veya kullanıcı için farklı bir veritabanı kullanılır.

**Ayar** | **Örnek değer** | **Açıklama**
---|---|---
ad | mydemoserver | MariaDB için Azure Veritabanı sunucunuzu tanımlayan benzersiz bir ad seçin. Sunucu adı yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir. 3 ila 63 karakter arası içermelidir.
resource-group | myresourcegroup | Azure kaynak grubunun adını sağlayın.
sku-name | GP_Gen5_2 | Sku'nun adı. Kısaca {fiyatlandırma katmanı}_{işlem nesli}_{sanal çekirdek sayısı} kuralına uyar. sku-name parametresi hakkında daha fazla bilgi için aşağıdaki tabloya bakın.
backup-retention | 7 | Yedeklemenin ne kadar süreyle tutulacağı. Birim olarak gün kullanılır. 7-35 aralığındadır. 
geo-redundant-backup | Devre dışı | Coğrafi olarak yedekli yedeklemelerin bu sunucu için etkinleştirilip etkinleştirilmeyeceği. İzin verilen değerler: Etkin, Devre Dışı.
location | westus | Sunucu için Azure konumu.
ssl-enforcement | Etkin | Bu sunucu için ssl'nin etkinleştirilip etkinleştirilmeyeceği. İzin verilen değerler: Etkin, Devre Dışı.
storage-size | 51200 | Sunucunun depolama kapasitesi (birim olan megabayt kullanılır). Geçerli storage-size en az 5120 MB'tır ve 1024 MB'lık artışlarla büyür. Depolama boyutu sınırları hakkında daha fazla bilgi için [fiyatlandırma katmanları](./concepts-pricing-tiers.md) belgesine bakın. 
version | 10.2 | MariaDB büyük altyapı sürümü.
admin-user | myadmin | Yöneticinin oturum açma kullanıcı adı. Şu değerler kullanılamaz: **azure_superuser**, **admin**, **administrator**, **root**, **guest** veya **public**.
admin-password | Password123 | Yönetici kullanıcının parolası. 8 ile 128 arasında karakter içermelidir. Parolanız şu üç kategoride yer alan karakterlerden oluşmalıdır: İngilizce büyük ve küçük harfler, sayılar ve alfasayısal olmayan karakterler.

sku-name parametresi değeri aşağıdaki örneklerde gösterildiği gibi {fiyatlandırma katmanı}\_{işlem oluşturma}\_{sanal çekirdek} kuralını kullanır:
+ `--sku-name B_Gen5_4` Temel, 5. Nesil ve 4 sanal çekirdekle eşleşir.
+ `--sku-name GP_Gen5_32` Genel Amaçlı, Gen 5 ve 32 sanal çekirdekle eşleşir.
+ `--sku-name MO_Gen5_2` Bellek için iyileştirilmiş, Gen 5 ve 2 sanal çekirdekle eşleşir.

Bölgeler ve katmanlar için geçerli olan değerleri anlamak için lütfen [fiyatlandırma katmanları](./concepts-pricing-tiers.md) belgesini inceleyin.

Aşağıdaki örnekte, Batı ABD bölgesinde `myadmin` sunucu yöneticisi oturum açma adıyla `myresourcegroup` kaynak grubunuzda `mydemoserver` adlı bir sunucu oluşturulur. Bu, 2 **sanal çekirdek** içeren, **5. Nesil** bir **Genel Amaçlı** sunucudur. Bir sunucunun adı DNS adıyla eşleşir ve bu nedenle sunucunun Azure’da genel olarak benzersiz olması gerekir. `<server_admin_password>` değerini kendi değerinizle değiştirin.
```azurecli-interactive
az mariadb server create --resource-group myresourcegroup --name mydemoserver  --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen5_2 --version 10.2
```

## <a name="configure-firewall-rule"></a>Güvenlik duvarı kuralını yapılandırma
**[az maridb server firewall-rule create](/cli/azure/mariadb/server/firewall-rule#az-mariadb-server-firewall-rule-create)** komutunu kullanarak MariaDB için Azure Veritabanı sunucusu düzeyinde bir güvenlik duvarı kuralı oluşturun. Sunucu düzeyindeki bir güvenlik duvarı kuralı, **mysql** komut satırı aracı veya MySQL Workbench gibi bir dış uygulamanın Azure MariaDB hizmetinin güvenlik duvarı üzerinden sunucunuza bağlanmasına imkan tanır. 

Aşağıdaki örnek `AllowMyIP` adında ve 192.168.0.1 IP adresinden gelen bağlantılara izin veren bir güvenlik duvarı kuralı oluşturur. IP adresini veya IP adresi aralıklarını bağlandığınız adreslere göre değiştirin. 

```azurecli-interactive
az mariadb server firewall-rule create --resource-group myresourcegroup --server mydemoserver --name AllowMyIP --start-ip-address 192.168.0.1 --end-ip-address 192.168.0.1
```

> [!NOTE]
> MariaDB için Azure Veritabanı bağlantıları, 3306 numaralı bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden bağlanmaya çalışıyorsanız, 3306 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda, BT departmanınız 3306 numaralı bağlantı noktasını açmadığı sürece sunucunuza bağlanamazsınız.
> 

## <a name="configure-ssl-settings"></a>SSL ayarlarını yapılandırma
Varsayılan olarak sunucunuz ile istemci uygulamaları arasında SSL bağlantıları zorunlu tutulur. Bu varsayılan ayar, İnternet üzerinden veri akışını şifreleyerek “hareket halindeki” verilerinizin güvenli olmasını sağlar. Bu hızlı başlangıcı daha da kolaylaştırmak üzere sunucunuz için SSL bağlantılarını devre dışı bırakın. Üretim sunucuları için SSL’in devre dışı bırakılması önerilmez. Daha ayrıntılı bilgi için bkz. [MariaDB için Azure Veritabanı'na güvenli bir şekilde bağlanmak üzere uygulamanızda SSL bağlantısını yapılandırma](./howto-configure-ssl.md).

Aşağıdaki örnekte, MariaDB sunucunuzda SSL’yi zorunlu tutma ayarı devre dışı bırakılır.
 
 ```azurecli-interactive
 az mariadb server update --resource-group myresourcegroup --name mydemoserver --ssl-enforcement Disabled
 ```

## <a name="get-the-connection-information"></a>Bağlantı bilgilerini alma

Sunucunuza bağlanmak için ana bilgisayar bilgilerini ve erişim kimlik bilgilerini sağlamanız gerekir.

```azurecli-interactive
az mariadb server show --resource-group myresourcegroup --name mydemoserver
```

Sonuç JSON biçimindedir. **fullyQualifiedDomainName** ve **administratorLogin** bilgilerini not alın.
```json
{
  "administratorLogin": "myadmin",
  "earliestRestoreDate": null,
  "fullyQualifiedDomainName": "mydemoserver.mariadb.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforMariaDB/servers/mydemoserver",
  "location": "westus",
  "name": "mydemoserver",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "capacity": 2,
    "family": "Gen5",
    "name": "GP_Gen5_2",
    "size": null,
    "tier": "GeneralPurpose"
  },
  "sslEnforcement": "Enabled",
  "storageProfile": {
    "backupRetentionDays": 7,
    "geoRedundantBackup": "Disabled",
    "storageMb": 5120
  },
  "tags": null,
  "type": "Microsoft.DBforMariaDB/servers",
  "userVisibleState": "Ready",
  "version": "10.2"
}
```

## <a name="connect-to-server-using-mysql-command-line"></a>mysql komut satırını kullanarak sunucuya bağlanma
**mysql** komut satırı aracını kullanarak sunucunuza bağlanın. Komut satırı aracını [buradan](https://dev.mysql.com/downloads/) indirebilir ve bilgisayarınıza yükleyebilirsiniz. Bunun yerine kod örneklerindeki **Deneyin** düğmesine veya Azure portalında sağ üstte bulunan `>_` düğmesine tıklayabilir ve **Azure Cloud Shell**'i başlatabilirsiniz.

Aşağıdaki komutları yazın: 

1. **mysql** komut satırı aracını kullanarak sunucuya bağlanın:
```azurecli-interactive
 mysql -h mydemoserver.mariadb.database.azure.com -u myadmin@mydemoserver -p
```

2. mysql> isteminde sunucu durumun görüntüleyin:
```sql
status
```
Her şey yolunda giderse komut satırı aracı aşağıdaki metni oluşturmalıdır:

```bash
C:\Users\>mysql -h mydemoserver.mariadb.database.azure.com -u myadmin@mydemoserver -p
Enter password: ***********
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 65512
Server version: 5.6.39.0 MariaDB Server

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> status
--------------
mysql  Ver 14.14 Distrib 5.7.23, for Linux (x86_64)

Connection id:          64681
Current database:
Current user:           myadmin@40.118.201.21
SSL:                    Cipher in use is AES256-SHA
Current pager:          stdout
Using outfile:          ''
Using delimiter:        ;
Server version:         5.6.39.0 MariaDB Server
Protocol version:       10
Connection:             mydemoserver.mariadb.database.azure.com via TCP/IP
Server characterset:    latin1
Db     characterset:    latin1
Client characterset:    utf8
Conn.  characterset:    utf8
TCP port:               3306
Uptime:                 1 day 3 hours 28 min 50 sec

Threads: 10  Questions: 29002  Slow queries: 0  Opens: 33  Flush tables: 3  Open tables: 1  Queries per second avg: 0.293
--------------

mysql>
```

> [!TIP]
> Ek komutlar için bkz. [MySQL 5.7 Başvuru Kılavuzu - Bölüm 4.5.1](https://dev.mysql.com/doc/refman/5.7/en/mysql.html).

## <a name="connect-to-server-using-mysql-workbench"></a>MySQL Workbench kullanarak sunucuya bağlanma
1.  İstemci bilgisayarınızda MySQL Workbench uygulamasını başlatın. MySQL Workbench uygulamasını [buradan](https://dev.mysql.com/downloads/workbench/) indirip yükleyebilirsiniz.

2.  **Setup New Connection** (Yeni Bağlantı Oluştur) iletişim kutusundaki **Parameters** (Parametreler) sekmesine aşağıdaki bilgileri girin:

   ![yeni bağlantı oluştur](./media/quickstart-create-mariadb-server-database-using-azure-cli/setup-new-connection.png)

| **Ayar** | **Önerilen Değer** | **Açıklama** |
|---|---|---|
|   Bağlantı Adı | Bağlantım | Bu bağlantı için bir etiket belirtin (herhangi bir şey olabilir) |
| Bağlantı Yöntemi | Standart (TCP/IP) seçeneğini belirleyin | MariaDB için Azure Veritabanı'na bağlanmak için TCP/IP protokolünü kullanın |
| Ana Bilgisayar Adı | mydemoserver.mariadb.database.azure.com | Daha önce not aldığınız sunucu adı. |
| Bağlantı noktası | 3306 | MariaDB için varsayılan bağlantı noktası kullanılır. |
| Kullanıcı adı | myadmin@mydemoserver | Daha önce not aldığınız sunucu yöneticisi oturum açma bilgileri. |
| Parola | **** | Önceden yapılandırdığınız yönetici parolasını kullanın. |

Tüm parametrelerin doğru yapılandırılıp yapılandırılmadığını test etmek için **Bağlantıyı Sına**’ya tıklayın.
Şimdi bağlantıya tıklayarak sunucuya başarıyla bağlanabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu kaynaklara başka bir hızlı başlangıç/öğretici için gereksinim duymuyorsanız aşağıdaki komutu çalıştırarak kaynakları silebilirsiniz: 

```azurecli-interactive
az group delete --name myresourcegroup
```

Yalnızca yeni oluşturulan sunucuyu silmek istiyorsanız **[az mariadb server delete](/cli/azure/mariadb/server#az-mariadb-server-delete)** komutunu kullanabilirsiniz.
```azurecli-interactive
az mariadb server delete --resource-group myresourcegroup --name mydemoserver
```

<!--
## Next steps

> [!div class="nextstepaction"]
> [Design a MariaDB Database with Azure CLI](./tutorial-design-database-using-cli.md)
-->