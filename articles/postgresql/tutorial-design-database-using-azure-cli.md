---
title: "İlk Azure veritabanınız için Azure CLI kullanarak PostgreSQL tasarım | Microsoft Docs"
description: "Bu öğretici, Azure CLI kullanarak ilk Azure veritabanınız için PostgreSQL tasarlamak nasıl gösterir."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: azure-cli
ms.topic: tutorial
ms.date: 11/27/2017
ms.openlocfilehash: 97299ae904115d08c5d03be38be263203552b84b
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="design-your-first-azure-database-for-postgresql-using-azure-cli"></a>İlk Azure veritabanınız için Azure CLI kullanarak PostgreSQL tasarlama 
Bu öğreticide, Azure CLI (komut satırı arabirimi) ve diğer yardımcı programları öğrenmek için kullandığınız nasıl yapılır:
> [!div class="checklist"]
> * PostgreSQL için Azure Veritabanı sunucusu oluşturma
> * Sunucu Güvenlik Duvarı'nı yapılandırma
> * Kullanım [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) bir veritabanı oluşturmak için yardımcı programı
> * Örnek verileri yükleme
> * Verileri sorgulama
> * Verileri güncelleştirme
> * Verileri geri yükleme

Tarayıcıda, Azure bulut kabuğunu kullanabilirsiniz veya [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli) Bu öğreticide komutları çalıştırmak için kendi bilgisayarınızda.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

Birden fazla aboneliğiniz varsa kaynağın mevcut olduğu ve faturalandırıldığı uygun aboneliği seçin. [az account set](/cli/azure/account#az_account_set) komutunu kullanarak hesabınız altındaki belirli bir abonelik kimliğini seçin.
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
[az group create](/cli/azure/group#az_group_create) komutunu kullanarak bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) oluşturun. Kaynak grubu, Azure kaynaklarının grup olarak dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek `westus` konumunda `myresourcegroup` adlı bir kaynak grubu oluşturur.
```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-postgresql-server"></a>PostgreSQL için Azure Veritabanı sunucusu oluşturma
[az postgres server create](/cli/azure/postgres/server#az_postgres_server_create) komutunu kullanarak [PostgreSQL sunucusu için Azure SQL Veritabanı ](overview.md) oluşturun. Sunucu, grup olarak yönetilen bir veritabanı grubu içerir. 

Aşağıdaki örnek adı verilen bir sunucu oluşturur `mypgserver-20170401` kaynak grubunuzdaki `myresourcegroup` Sunucu Yöneticisi oturum açma ile `mylogin`. Bir sunucunun adı DNS adıyla eşleşir ve bu nedenle sunucunun Azure’da genel olarak benzersiz olması gerekir. `<server_admin_password>` değerini kendi değerinizle değiştirin.
```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mypgserver-20170401 --location westus --admin-user mylogin --admin-password <server_admin_password> --performance-tier Basic --compute-units 50 --version 9.6
```

> [!IMPORTANT]
> Burada belirttiğiniz sunucu yöneticisi kullanıcı adı ve parolası, bu hızlı başlangıcın sonraki bölümlerinde sunucuda ve veritabanlarında oturum açmak için gereklidir. Bu bilgileri daha sonra kullanmak üzere aklınızda tutun veya kaydedin.

Varsayılan olarak, **postgres** veritabanı sunucunuz altında oluşturulur. [Postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) veritabanı; kullanıcılar, yardımcı programlar ve üçüncü taraf uygulamalar tarafından kullanılmak üzere geliştirilmiş varsayılan bir veritabanıdır. 


## <a name="configure-a-server-level-firewall-rule"></a>Sunucu düzeyinde güvenlik duvarı kuralı yapılandırma

[az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_create) komutunu kullanarak Azure PostgreSQL sunucusu düzeyinde bir güvenlik duvarı kuralı oluşturun. Sunucu düzeyindeki bir güvenlik duvarı kuralı, [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) veya [PgAdmin](https://www.pgadmin.org/) gibi bir dış uygulamanın Azure PostgreSQL hizmetinin güvenlik duvarı üzerinden sunucunuza bağlanmasına imkan tanır. 

Ağınızdan bağlanabilmek için bir IP aralığını kapsayan güvenlik duvarı kuralı ayarlayabilirsiniz. Aşağıdaki örnek kullanır [az postgres sunucu güvenlik duvarı kuralı oluşturmak](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_create) bir güvenlik duvarı kuralı oluşturmak için `AllowAllIps` herhangi bir IP adresi arasında bağlantı sağlar. Tüm IP adreslerini açmak için başlangıç IP adresi olarak 0.0.0.0’ı, bitiş adresi olaraksa 255.255.255.255’i kullanın.

Ağınıza yalnızca Azure PostgreSQL sunucunuza erişimi kısıtlamak için güvenlik duvarı kuralı yalnızca, şirket ağındaki IP adresi aralığı kapsayacak şekilde ayarlayabilirsiniz.

```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server mypgserver-20170401 --name AllowAllIps --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> Azure PostgreSQL sunucusu, 5432 bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden bağlanıyorsanız ağınızın güvenlik duvarı tarafından 5432 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. BT departmanınızdan Azure SQL Veritabanı sunucunuzla bağlantı için 5432 numaralı bağlantı noktasını açmasını isteyin.
>

## <a name="get-the-connection-information"></a>Bağlantı bilgilerini alma

Sunucunuza bağlanmak için ana bilgisayar bilgilerini ve erişim kimlik bilgilerini sağlamanız gerekir.
```azurecli-interactive
az postgres server show --resource-group myresourcegroup --name mypgserver-20170401
```

Sonuç JSON biçimindedir. **administratorLogin** ve **fullyQualifiedDomainName** bilgilerini not alın.
```json
{
  "administratorLogin": "mylogin",
  "fullyQualifiedDomainName": "mypgserver-20170401.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforPostgreSQL/servers/mypgserver-20170401",
  "location": "westus",
  "name": "mypgserver-20170401",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "capacity": 50,
    "family": null,
    "name": "PGSQLS2M50",
    "size": null,
    "tier": "Basic"
  },
  "sslEnforcement": null,
  "storageMb": 51200,
  "tags": null,
  "type": "Microsoft.DBforPostgreSQL/servers",
  "userVisibleState": "Ready",
  "version": "9.6"
}
```

## <a name="connect-to-azure-database-for-postgresql-database-using-psql"></a>Psql kullanarak PostgreSQL veritabanı için Azure veritabanına bağlan
İstemci bilgisayarınızda yüklü PostgreSQL varsa, yerel bir örneğini kullanabilirsiniz [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html), veya bir Azure PostgreSQL sunucusuna bağlanmak için Azure bulut Konsolu. Şimdi PostgreSQL için Azure Veritabanı sunucusuna bağlanmak üzere psql komut satır yardımcı programını kullanalım.

1. Bir PostgreSQL veritabanı için Azure veritabanına bağlanmak için aşağıdaki psql komutu çalıştırın:
```azurecli-interactive
psql --host=<servername> --port=<port> --username=<user@servername> --dbname=<dbname>
```

  Örneğin aşağıdaki komut, erişim kimlik bilgilerini kullanarak **mypgserver-20170401.postgres.database.azure.com** PostgreSQL sunucunuzda **postgres** adlı varsayılan veritabanına bağlanır. Parola istendiğinde seçtiğiniz `<server_admin_password>` değerini girin.
  
  ```azurecli-interactive
psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 ---dbname=postgres
```

2.  Sunucusuna bağlandıktan sonra boş bir veritabanı isteminde oluşturun:
```sql
CREATE DATABASE mypgsqldb;
```

3.  İstemde, bağlantıyı yeni oluşturulan **mypgsqldb** veritabanına geçirmek için aşağıdaki komutu yürütün:
```sql
\c mypgsqldb
```

## <a name="create-tables-in-the-database"></a>Veritabanında tabloları oluşturma
PostgreSQL için Azure veritabanına bağlanmak nasıl bildiğinize göre biz temel bazı görevlerin nasıl üzerinden gidebilirsiniz.

İlk olarak, size bir tablo oluşturun ve bazı verilerle yükleyin. Envanter bilgilerini izleyen bir tablo oluşturalım:
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

Yazarak Tablo listesinde yeni oluşturulan tabloda şimdi görebilirsiniz:
```sql
\dt
```

## <a name="load-data-into-the-table"></a>Tabloya veri yükleme
Bir tablo sahibiz, biz bazı veri içine ekleyebilirsiniz. Açık komut istemi penceresinde, bazı veri satırı eklemek için aşağıdaki sorguyu çalıştırın:
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

Şimdi, daha önce oluşturduğunuz tabloya iki örnek veri satırı eklediniz.

## <a name="query-and-update-the-data-in-the-tables"></a>Sorgulamak ve tablolarındaki verileri güncelleyin
Stok tablosundan bilgi almak için aşağıdaki sorguyu çalıştırın: 
```sql
SELECT * FROM inventory;
```

Ayrıca Envanter tablodaki verileri güncelleştirebilirsiniz:
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

Veri aldığınızda güncelleştirilmiş değerleri görebilirsiniz:
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Bir veritabanını daha önceki bir noktaya geri yükleme
Yanlışlıkla bir tabloyu sildiniz düşünün. Bu, kolayca kurtaramazsınız şeydir. Azure veritabanı PostgreSQL için tüm noktası zamanında (Basic en çok 7 gün) ve standart 35 gün için geri dönün ve bu noktası zaman içinde yeni bir sunucuya geri yüklemek sağlar. Bu yeni sunucu silinen verilerinizi kurtarmak için kullanabilirsiniz. 

Tablo eklenmeden önce aşağıdaki komutu örnek sunucunun bir noktaya geri yükler:
```azurecli-interactive
az postgres server restore --resource-group myResourceGroup --name mypgserver-restored --restore-point-in-time 2017-04-13T13:59:00Z --source-server mypgserver-20170401
```

`az postgres server restore` Komutu aşağıdaki parametreleri gerekir:
| Ayar | Önerilen değer | Açıklama  |
| --- | --- | --- |
| --kaynak-grubu |  myResourceGroup |  Kaynak sunucunun bulunduğu kaynak grubu.  |
| --adı | mypgserver geri | Geri yükleme komutu tarafından oluşturulan yeni sunucunun adıdır. |
| zaman içinde geri yükleme noktası | 2017-04-13T13:59:00Z | Bir noktayı geri yüklemek için zaman içinde seçin. Bu tarih ve saat kaynak sunucunun yedekleme saklama dönemi içinde olmalıdır. ISO8601 tarih ve saat biçimini kullanın. Örneğin, kendi yerel saat dilimi gibi kullanabilir `2017-04-13T05:59:00-08:00`, veya UTC Zulu dili biçimi kullanın `2017-04-13T13:59:00Z`. |
| --Kaynak sunucusu | mypgserver 20170401 | Adı veya geri yüklemek için kaynak sunucunun kimliği. |

Sunucuyu bir nokta zaman için geri yükleme noktası itibariyle özgün sunucusu olarak belirttiğiniz zamanda kopyalanan yeni bir sunucu oluşturur. Geri yüklenen sunucusunun fiyatlandırma katmanı değerleri ve konumunu kaynak sunucu ile aynı olur.

Komut zaman uyumlu ve sunucunun geri yüklendikten sonra döndürür. Geri yükleme tamamlandıktan sonra oluşturulan yeni sunucu bulun. Verileri beklenen şekilde geri doğrulayın.


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure CLI (komut satırı arabirimi) ve diğer yardımcı programlarını nasıl kullanılacağı hakkında bilgi edindiniz:
> [!div class="checklist"]
> * PostgreSQL için Azure Veritabanı sunucusu oluşturma
> * Sunucu Güvenlik Duvarı'nı yapılandırma
> * Kullanım [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) bir veritabanı oluşturmak için yardımcı programı
> * Örnek verileri yükleme
> * Verileri sorgulama
> * Verileri güncelleştirme
> * Verileri geri yükleme

Ardından, Azure portalında benzer görevleri yapmak için Bu öğretici gözden geçirmek için nasıl kullanacağınızı öğrenin: [PostgreSQL için Azure Portalı'nı kullanarak ilk Azure veritabanınızı tasarlama](tutorial-design-database-using-azure-portal.md)
