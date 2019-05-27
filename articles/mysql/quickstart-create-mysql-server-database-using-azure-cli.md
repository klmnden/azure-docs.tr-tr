---
title: 'Hızlı Başlangıç: MySQL sunucusu - Azure CLI için Azure veritabanı oluşturma'
description: Bu hızlı başlangıçta, Azure CLI aracını kullanarak bir Azure kaynak grubunda nasıl MySQL için Azure Veritabanı sunucusu oluşturabileceğiniz açıklanır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.devlang: azurecli
ms.topic: quickstart
ms.date: 01/09/2019
ms.custom: mvc
ms.openlocfilehash: 10acb353e282508c838bee89b131d94dcd3fa7ee
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66160415"
---
# <a name="quickstart-create-an-azure-database-for-mysql-server-using-azure-cli"></a>Hızlı Başlangıç: Azure CLI aracını kullanarak MySQL için Azure Veritabanı sunucusu oluşturma

> [!TIP]
> Basit kullanmayı [az mysql'i ayarlama](/cli/azure/ext/db-up/mysql#ext-db-up-az-mysql-up) (şu anda önizlemede) Azure CLI komutu. Denemenin [hızlı](./quickstart-create-server-up-azure-cli.md).

Bu hızlı başlangıçta, Azure CLI aracını kullanarak bir Azure kaynak grubunda yaklaşık beş dakikada nasıl MySQL için Azure Veritabanı sunucusu oluşturabileceğiniz açıklanır. Azure CLI, komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme]( /cli/azure/install-azure-cli). 

Birden fazla aboneliğiniz varsa kaynağın mevcut olduğu ve faturalandırıldığı uygun aboneliği seçin. [az account set](/cli/azure/account#az-account-set) komutunu kullanarak hesabınız altındaki belirli bir abonelik kimliğini seçin.
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturun
[az group create](/cli/azure/group#az-group-create) komutunu kullanarak bir [Azure kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) oluşturun. Kaynak grubu, Azure kaynaklarının grup olarak dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnek `westus` konumunda `myresourcegroup` adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a>MySQL için Azure Veritabanı sunucusu oluşturma
**[az mysql server create](/cli/azure/mysql/server#az-mysql-server-create)** komutunu kullanarak MySQL için Azure Veritabanı sunucusu oluşturun. Bir sunucu birden çok veritabanını yönetebilir. Genellikle her proje veya kullanıcı için farklı bir veritabanı kullanılır.

**Ayar** | **Örnek değer** | **Açıklama**
---|---|---
name | mydemoserver | Azure veritabanınızı MySQL sunucusuna tanıtan benzersiz bir ad seçin. Sunucu adı yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir. 3 ila 63 karakter arası içermelidir.
resource-group | myresourcegroup | Azure kaynak grubunun adını girin.
sku-name | GP_Gen5_2 | Sku'nun adı. Kısaca {fiyatlandırma katmanı}\_{işlem nesli}\_{sanal çekirdek sayısı} kuralına uyar. sku-name parametresi hakkında daha fazla bilgi için aşağıdaki tabloya bakın.
backup-retention | 7 | Yedeklemenin ne kadar süreyle tutulacağı. Birim olarak gün kullanılır. 7-35 aralığındadır. 
geo-redundant-backup | Devre Dışı Bırakıldı | Coğrafi olarak yedekli yedeklemelerin bu sunucu için etkinleştirilip etkinleştirilmeyeceği. İzin verilen değerler: Etkin, devre dışı bırakıldı.
konum | westus | Sunucu için Azure konumu.
ssl-enforcement | Enabled | Bu sunucu için ssl'in etkinleştirilip etkinleştirilmeyeceği. İzin verilen değerler: Etkin, devre dışı bırakıldı.
storage-size | 51200 | Sunucunun depolama kapasitesi (birim olan megabayt kullanılır). Geçerli storage-size en az 5120 MB'tır ve 1024 MB'lık artışlarla büyür. Depolama boyutu sınırları hakkında daha fazla bilgi için [fiyatlandırma katmanları](./concepts-pricing-tiers.md) belgesine bakın. 
version | 5.7 | MySQL ana sürümü.
admin-user | myadmin | Yöneticinin oturum açma kullanıcı adı. Şu değerler kullanılamaz: **azure_superuser**, **admin**, **administrator**, **root**, **guest** veya **public**.
admin-password | *güvenli parola* | Yönetici kullanıcının parolası. 8 ile 128 arasında karakter içermelidir. Parolanız şu kategorilerin üçünden karakterler içermelidir: İngilizce harfler, İngilizce küçük harfler, sayılar ve alfasayısal olmayan karakterler büyük.


sku-name parametresi değeri aşağıdaki örneklerde gösterildiği gibi {fiyatlandırma katmanı}\_{işlem oluşturma}\_{sanal çekirdek} kuralını kullanır:
+ `--sku-name B_Gen5_1` Temel ve 5. nesil 1 sanal çekirdek eşlenir. Bu seçenek en küçük SKU kullanılabilir olur.
+ `--sku-name GP_Gen5_32` Genel Amaçlı, Gen 5 ve 32 sanal çekirdekle eşleşir.
+ `--sku-name MO_Gen5_2` Bellek için iyileştirilmiş, Gen 5 ve 2 sanal çekirdekle eşleşir.

Bölgeler ve katmanlar için geçerli olan değerleri anlamak için lütfen [fiyatlandırma katmanları](./concepts-pricing-tiers.md) belgesini inceleyin.

Aşağıdaki örnekte, Batı ABD bölgesinde `myadmin` sunucu yöneticisi oturum açma adıyla `myresourcegroup` kaynak grubunuzda `mydemoserver` adlı bir MySQL 5.7 sunucusu oluşturulur. Bu, 2 **sanal çekirdek** içeren, **4. Nesil** bir **Genel Amaçlı** sunucudur. `<server_admin_password>` değerini kendi değerinizle değiştirin.

```azurecli-interactive
az mysql server create --resource-group myresourcegroup --name mydemoserver  --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen5_2 --version 5.7
```

> [!NOTE]
> Temel fiyatlandırma katmanını kullanarak göz önünde bulundurun hafif işlem ve g/ç iş yükünüz için yeterli olup olmadığını. Temel fiyatlandırma katmanında oluşturulan sunucular daha sonra genel amaçlı veya bellek için iyileştirilmiş ölçeklendirilemiyor olduğunu unutmayın. Bkz: [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/mysql/) daha fazla bilgi için.
> 

## <a name="configure-firewall-rule"></a>Güvenlik duvarı kuralını yapılandırma
**[az mysql server firewall-rule create](/cli/azure/mysql/server/firewall-rule#az-mysql-server-firewall-rule-create)** komutunu kullanarak MySQL için Azure Veritabanı sunucusu düzeyinde bir güvenlik duvarı kuralı oluşturun. Sunucu düzeyindeki bir güvenlik duvarı kuralı, **mysql.exe** komut satırı aracı veya MySQL Workbench gibi bir dış uygulamanın Azure MySQL hizmetinin güvenlik duvarı üzerinden sunucunuza bağlanmasına imkan tanır. 

Aşağıdaki örnek `AllowMyIP` adında ve 192.168.0.1 IP adresinden gelen bağlantılara izin veren bir güvenlik duvarı kuralı oluşturur. IP adresini veya IP adresi aralıklarını bağlandığınız adreslere göre değiştirin. 

```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server mydemoserver --name AllowMyIP --start-ip-address 192.168.0.1 --end-ip-address 192.168.0.1
```

> [!NOTE]
> MySQL için Azure Veritabanı bağlantıları 3306 bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden bağlanmaya çalışıyorsanız, 3306 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda, BT departmanınız 3306 numaralı bağlantı noktasını açmadığı sürece sunucunuza bağlanamazsınız.
> 


## <a name="configure-ssl-settings"></a>SSL ayarlarını yapılandırma
Varsayılan olarak sunucunuz ile istemci uygulamaları arasında SSL bağlantıları zorunlu tutulur. Bu varsayılan ayar, İnternet üzerinden veri akışını şifreleyerek “hareket halindeki” verilerinizin güvenli olmasını sağlar. Bu hızlı başlangıcı daha da kolaylaştırmak üzere sunucunuz için SSL bağlantılarını devre dışı bırakın. Üretim sunucuları için SSL’in devre dışı bırakılması önerilmez. Daha ayrıntılı bilgi için bkz. [MySQL için Azure Veritabanı'na güvenli bir şekilde bağlanmak üzere uygulamanızda SSL bağlantısını yapılandırma](./howto-configure-ssl.md).

Aşağıdaki örnekte, MySQL sunucunuzda SSL’yi zorunlu tutma ayarı devre dışı bırakılır.
 
 ```azurecli-interactive
 az mysql server update --resource-group myresourcegroup --name mydemoserver --ssl-enforcement Disabled
 ```

## <a name="get-the-connection-information"></a>Bağlantı bilgilerini alma

Sunucunuza bağlanmak için ana bilgisayar bilgilerini ve erişim kimlik bilgilerini sağlamanız gerekir.

```azurecli-interactive
az mysql server show --resource-group myresourcegroup --name mydemoserver
```

Sonuç JSON biçimindedir. **fullyQualifiedDomainName** ve **administratorLogin** bilgilerini not alın.
```json
{
  "administratorLogin": "myadmin",
  "earliestRestoreDate": null,
  "fullyQualifiedDomainName": "mydemoserver.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforMySQL/servers/mydemoserver",
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
  "type": "Microsoft.DBforMySQL/servers",
  "userVisibleState": "Ready",
  "version": "5.7"
}
```

## <a name="connect-to-the-server-using-the-mysqlexe-command-line-tool"></a>mysql.exe komut satırı aracını kullanarak sunucuya bağlanma
**mysql.exe** komut satırı aracını kullanarak sunucunuza bağlanın. MySQL'i [buradan](https://dev.mysql.com/downloads/) indirerek bilgisayarınıza yükleyebilirsiniz. 

Aşağıdaki komutları yazın: 

1. **mysql** komut satırı aracını kullanarak sunucuya bağlanın:
   ```bash
   mysql -h mydemoserver.mysql.database.azure.com -u myadmin@mydemoserver -p
   ```

2. Sunucu durumunu görüntüleyin:
   ```sql
   mysql> status
   ```
   Her şey yolunda giderse komut satırı aracı aşağıdaki metni oluşturmalıdır:

```dos
C:\Users\>mysql -h mydemoserver.mysql.database.azure.com -u myadmin@mydemoserver -p
Enter password: ***********
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 65512
Server version: 5.6.26.0 MySQL Community Server (GPL)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> status
--------------
mysql  Ver 14.14 Distrib 5.6.35, for Win64 (x86_64)

Connection id:          65512
Current database:
Current user:           myadmin@116.230.243.143
SSL:                    Not in use
Using delimiter:        ;
Server version:         5.6.26.0 MySQL Community Server (GPL)
Protocol version:       10
Connection:             mydemoserver.mysql.database.azure.com via TCP/IP
Server characterset:    latin1
Db     characterset:    latin1
Client characterset:    gbk
Conn.  characterset:    gbk
TCP port:               3306
Uptime:                 2 days 9 hours 47 min 20 sec

Threads: 4  Questions: 34833  Slow queries: 2  Opens: 84  Flush tables: 4  Open tables: 1  Queries per second avg: 0.167
--------------

mysql>
```

> [!TIP]
> Ek komutlar için bkz. [MySQL 5.7 Başvuru Kılavuzu - Bölüm 4.5.1](https://dev.mysql.com/doc/refman/5.7/en/mysql.html).

## <a name="connect-to-the-server-using-the-mysql-workbench-gui-tool"></a>MySQL Workbench GUI aracını kullanarak sunucuya bağlanma
1. İstemci bilgisayarınızda MySQL Workbench uygulamasını başlatın. MySQL Workbench uygulamasını [buradan](https://dev.mysql.com/downloads/workbench/) indirip yükleyebilirsiniz.

2. **Setup New Connection** (Yeni Bağlantı Oluştur) iletişim kutusundaki **Parameters** (Parametreler) sekmesine aşağıdaki bilgileri girin:

   ![yeni bağlantı oluştur](./media/quickstart-create-mysql-server-database-using-azure-cli/setup-new-connection.png)

| **Ayar** | **Önerilen Değer** | **Açıklama** |
|---|---|---|
|   Bağlantı Adı | Bağlantım | Bu bağlantı için bir etiket belirtin (herhangi bir şey olabilir) |
| Bağlantı Yöntemi | Standart (TCP/IP) seçeneğini belirleyin | MySQL için Azure Veritabanı'na bağlanmak için TCP/IP protokolünü kullanın |
| Konak adı | mydemoserver.mysql.database.azure.com | Daha önce not aldığınız sunucu adı. |
| Port | 3306 | MySQL için varsayılan bağlantı noktası kullanılır. |
| Kullanıcı adı | myadmin@mydemoserver | Daha önce not aldığınız sunucu yöneticisi oturum açma bilgileri. |
| Parola | **** | Önceden yapılandırdığınız yönetici parolasını kullanın. |

Tüm parametrelerin doğru yapılandırılıp yapılandırılmadığını test etmek için **Bağlantıyı Sına**’ya tıklayın.
Şimdi bağlantıya tıklayarak sunucuya başarıyla bağlanabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu kaynaklara başka bir hızlı başlangıç/öğretici için gereksinim duymuyorsanız aşağıdaki komutu çalıştırarak kaynakları silebilirsiniz: 

```azurecli-interactive
az group delete --name myresourcegroup
```

Yeni oluşturulan sunucuyu silmek istiyorsanız **[az mysql server delete](/cli/azure/mysql/server#az-mysql-server-delete)** komutunu kullanabilirsiniz.
```azurecli-interactive
az mysql server delete --resource-group myresourcegroup --name mydemoserver
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure CLI ile bir MySQL Veritabanı tasarlama](./tutorial-design-database-using-cli.md)
