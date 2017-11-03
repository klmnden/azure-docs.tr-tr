---
title: "İlk Azure veritabanınızı MySQL veritabanı - Azure CLI için Tasarım | Microsoft Docs"
description: "Bu öğretici, oluşturma ve MySQL server ve Azure CLI 2.0 komut satırından kullanarak veritabanı için Azure veritabanı yönetme açıklanmaktadır."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
ms.service: mysql
ms.devlang: azure-cli
ms.topic: tutorial
ms.date: 06/13/2017
ms.custom: mvc
ms.openlocfilehash: 0e6a92eeb9711b086359ab2cd1aea87a57f1fc36
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="design-your-first-azure-database-for-mysql-database"></a>İlk Azure veritabanınızı MySQL veritabanı için tasarlama

Azure veritabanı MySQL için bir MySQL Community Edition veritabanı altyapısını temel Microsoft bulut ilişkisel veritabanı hizmetidir. Bu öğreticide, Azure CLI (komut satırı arabirimi) ve diğer yardımcı programları öğrenmek için kullandığınız nasıl yapılır:

> [!div class="checklist"]
> * MySQL için Azure bir veritabanı oluşturun
> * Sunucu Güvenlik Duvarı'nı yapılandırma
> * Kullanım [mysql komut satırı aracı](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) bir veritabanı oluşturmak için
> * Örnek verileri yükleme
> * Verileri sorgulama
> * Verileri güncelleştirme
> * Verileri geri yükleme

Tarayıcıda, Azure bulut kabuğunu kullanabilirsiniz veya [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli) bu öğreticideki kod blokları çalıştırmak için kendi bilgisayarınızda.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

Birden fazla aboneliğiniz varsa kaynağın mevcut olduğu ve faturalandırıldığı uygun aboneliği seçin. [az account set](/cli/azure/account#set) komutunu kullanarak hesabınız altındaki belirli bir abonelik kimliğini seçin.
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Oluşturma bir [Azure kaynak grubu](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) ile [az grubu oluşturma](https://docs.microsoft.com/cli/azure/group#az_group_create) komutu. Kaynak grubu, Azure kaynaklarının grup olarak dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnek `westus` konumunda `mycliresource` adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name mycliresource --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a>MySQL için Azure Veritabanı sunucusu oluşturma
MySQL sunucusu az mysql server ile oluşturmak için komutu bir Azure veritabanı oluşturun. Bir sunucu birden çok veritabanını yönetebilir. Genellikle her proje veya kullanıcı için farklı bir veritabanı kullanılır.

Aşağıdaki örnekte, `westus` bölgesinde bulunan `mycliresource` kaynak grubundaki `mycliserver` adlı MySQL sunucusu için bir Azure Veritabanı oluşturulur. Sunucunun `myadmin` şeklinde bir oturum adı ve `Password01!` şeklinde bir parolası vardır. Sunucu **Temel** performans katmanıyla oluşturulmuştur ve sunucudaki tüm veritabanları **50** işlem birimini ortak olarak kullanır. Uygulama gereksinimlerine bağlı olarak işlem ve depolama ölçeğini büyütebilir veya küçültebilirsiniz.

```azurecli-interactive
az mysql server create --resource-group mycliresource --name mycliserver
--location westus --user myadmin --password Password01!
--performance-tier Basic --compute-units 50
```

## <a name="configure-firewall-rule"></a>Güvenlik duvarı kuralını yapılandırma
MySQL sunucu düzeyinde güvenlik duvarı kuralı az mysql sunucu güvenlik duvarı kuralı oluşturmak için komutu bir Azure veritabanı oluşturun. Sunucu düzeyinde güvenlik duvarı kuralı gibi bir dış uygulamaya izin verir **mysql** komut satırı aracını veya MySQL Azure MySQL hizmeti güvenlik duvarı aracılığıyla sunucunuza bağlanmak için çalışma ekranı. 

Aşağıdaki örnek, bir önceden tanımlanmış adres aralığı için bir güvenlik duvarı kuralı oluşturur. Bu örnek, tüm olası IP adresleri aralığı gösterir.

```azurecli-interactive
az mysql server firewall-rule create --resource-group mycliresource --server mycliserver --name AllowYourIP --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

## <a name="get-the-connection-information"></a>Bağlantı bilgilerini alma

Sunucunuza bağlanmak için ana bilgisayar bilgilerini ve erişim kimlik bilgilerini sağlamanız gerekir.
```azurecli-interactive
az mysql server show --resource-group mycliresource --name mycliserver
```

Sonuç JSON biçimindedir. **fullyQualifiedDomainName** ve **administratorLogin** bilgilerini not alın.
```json
{
  "administratorLogin": "myadmin",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "mycliserver.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mycliresource/providers/Microsoft.DBforMySQL/servers/mycliserver",
  "location": "westus",
  "name": "mycliserver",
  "resourceGroup": "mycliresource",
  "sku": {
    "capacity": 50,
    "family": null,
    "name": "MYSQLS2M50",
    "size": null,
    "tier": "Basic"
  },
  "storageMb": 2048,
  "tags": null,
  "type": "Microsoft.DBforMySQL/servers",
  "userVisibleState": "Ready",
  "version": null
}
```

## <a name="connect-to-the-server-using-mysql"></a>MySQL kullanarak sunucuya bağlanın
Kullanım [mysql komut satırı aracı](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) Azure veritabanınıza MySQL sunucusu için bağlantı kuramıyor. Bu örnekte, bir komut şöyledir:
```cmd
mysql -h mycliserver.database.windows.net -u myadmin@mycliserver -p
```

## <a name="create-a-blank-database"></a>Boş veritabanı oluşturma
Bir kez sunucusuna bağlandıktan sonra boş bir veritabanı oluşturun.
```sql
mysql> CREATE DATABASE mysampledb;
```

İsteminde bağlantı yeni veritabanı oluşturulan bu geçiş yapmak için aşağıdaki komutu çalıştırın:
```sql
mysql> USE mysampledb;
```

## <a name="create-tables-in-the-database"></a>Veritabanında tabloları oluşturma
MySQL veritabanı için Azure veritabanına bağlanmak nasıl bildiğinize göre biz temel bazı görevlerin nasıl üzerinden gidebilirsiniz.

İlk olarak, size bir tablo oluşturun ve bazı verilerle yükleyin. Envanter bilgilerini depolayan bir tablo oluşturalım.
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-the-tables"></a>Veri tablolarına yükleme
Bir tablo sahibiz, biz bazı veri içine ekleyebilirsiniz. Açık komut istemi penceresinde, bazı veri satırı eklemek için aşağıdaki sorguyu çalıştırın.
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

Şimdi örnek verilerin daha önce oluşturduğunuz tabloya iki satır var.

## <a name="query-and-update-the-data-in-the-tables"></a>Sorgulamak ve tablolarındaki verileri güncelleyin
Veritabanı tablosundan bilgi almak için aşağıdaki sorguyu çalıştırın.
```sql
SELECT * FROM inventory;
```

Ayrıca tablolardaki verileri güncelleştirebilirsiniz.
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

Veri aldığınızda satır buna göre güncelleştirilir.
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Bir veritabanını daha önceki bir noktaya geri yükleme
Bu tablo yanlışlıkla silinmiş düşünün. Bu, kolayca kurtaramazsınız şeydir. Azure veritabanı için MySQL 35 gün için son yukarı zamanda herhangi bir noktaya kadar geri dönün ve bu noktaya zaman yeni bir sunucuya geri izin verir. Bu yeni sunucu silinen verilerinizi kurtarmak için kullanabilirsiniz. Tablo eklenmeden önce aşağıdaki adımları örnek sunucunun bir noktaya geri yükleme.

Geri yüklemek için aşağıdaki bilgiler gereklidir:

- Geri yükleme noktası: bir nokta sunucu değiştirilmeden önce oluşan zamanında seçin. Kaynak veritabanının en eski yedekleme değerine eşit veya daha büyük olmalıdır.
- Hedef sunucu: geri yüklemek istediğiniz yeni bir sunucu adı sağlayın
- Kaynak sunucu: geri yüklemek istediğiniz sunucunun adını belirtin
- Konumu: Bölge seçemezsiniz, varsayılan olarak kaynak sunucuyla aynı.

```azurecli-interactive
az mysql server restore --resource-group mycliresource --name mycliserver-restored --restore-point-in-time "2017-05-4 03:10" --source-server-name mycliserver
```

Sunucuyu geri yüklemek için ve [geri bir nokta zaman için](./howto-restore-server-portal.md) tablo silinmeden önce. Sunucuyu zamanında farklı bir noktaya geri yükleme oluşturur yinelenen yeni bir sunucu noktası itibariyle özgün sunucusu olarak belirttiğiniz süre için saklama dönemi içinde olmasını sağlanan, [hizmet katmanı](./concepts-service-tiers.md).

## <a name="next-steps"></a>Sonraki Adımlar
Bu öğreticide için öğrenilen:
> [!div class="checklist"]
> * MySQL için Azure bir veritabanı oluşturun
> * Sunucu Güvenlik Duvarı'nı yapılandırma
> * Kullanım [mysql komut satırı aracı](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) bir veritabanı oluşturmak için
> * Örnek verileri yükleme
> * Verileri sorgulama
> * Verileri güncelleştirme
> * Verileri geri yükleme

> [!div class="nextstepaction"]
> [Azure veritabanı için MySQL - Azure CLI örnekleri](./sample-scripts-azure-cli.md)
