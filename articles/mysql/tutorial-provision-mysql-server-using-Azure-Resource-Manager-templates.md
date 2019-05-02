---
title: 'Öğretici: Azure Resource Manager şablonu kullanarak MySQL için Azure veritabanı sağlama'
description: Bu öğreticide, sağlamak ve Azure Resource Manager şablonu kullanarak MySQL sunucusu dağıtımları için Azure veritabanı otomatikleştirmek açıklanmaktadır.
author: savjani
ms.author: pariks
ms.service: mysql
ms.devlang: json
ms.topic: tutorial
ms.date: 12/21/2018
ms.custom: mvc
ms.openlocfilehash: 6e4bb7622fe51c0cab4fc45e945e5bb07b1d32f1
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64925835"
---
# <a name="tutorial-provision-an-azure-database-for-mysql-server-using-azure-resource-manager-template"></a>Öğretici: Azure Resource Manager şablonu kullanarak MySQL için Azure veritabanı sağlama

[MySQL REST API'si için Azure veritabanı](https://docs.microsoft.com/rest/api/mysql/) çağırıp sağlama, yapılandırma ve işlemleri yönetilen MySQL sunucularının ve veritabanlarının azure'da tümleştirme, DevOps mühendislerine sağlar.  API, MySQL hizmeti için Azure veritabanı oluşturma, numaralandırma, yönetim ve MySQL sunucuları ve veritabanlarına silinmesini sağlar.

Azure Resource Manager, temel alınan bildirmek için REST API ve Azure kaynaklarını kod kavram olarak altyapısıyla hizalama ölçekli dağıtımlar için gerekli program yararlanın. Azure kaynak adı, SKU, ağ, güvenlik duvarı yapılandırması, şablon parametreleştiren ve bir oluşturulmasına izin ayarları, saat ve birden çok kez kullanıldı.  Azure Resource Manager şablonları kolayca oluşturulabilir kullanarak [Azure portalında](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal) veya [Visual Studio Code](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-visual-studio-code?tabs=CLI). Uygulama paketleme, Standardizasyon ve DevOps CI/CD işlem hattında tümleştirilebilir dağıtımı Otomasyonu, tanırlar.  Örneği için Azure veritabanı ile bir Web uygulaması için MySQL arka ucuna kolayca dağıtmak istiyorsanız, bunu kullanarak uçtan uca dağıtım gerçekleştirebilirsiniz [Hızlı Başlangıç şablonu](https://azure.microsoft.com/resources/templates/101-webapp-managed-mysql/) GitHub Galerisi.

Bu öğreticide, Azure Resource Manager şablonu ve diğer yardımcı programları öğrenmek için kullandığınız nasıl yapılır:

> [!div class="checklist"]
> * Azure Resource Manager şablonu kullanarak sanal ağ hizmet uç ile MySQL sunucusu için Azure veritabanı oluşturma
> * Veritabanı oluşturmak için [mysql komut satırı aracı](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) kullanma
> * Örnek verileri yükleme
> * Verileri sorgulama
> * Verileri güncelleştirme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

## <a name="create-an-azure-database-for-mysql-server-with-vnet-service-endpoint-using-azure-resource-manager-template"></a>Azure Resource Manager şablonu kullanarak sanal ağ hizmet uç ile MySQL sunucusu için Azure veritabanı oluşturma

MySQL sunucusu için Azure veritabanı JSON şablon başvurusu almak için Git [Microsoft.DBforMySQL sunucuları](/azure/templates/microsoft.dbformysql/servers) şablon başvurusu. Sanal ağ hizmet uç noktası ile bir MySQL için Azure veritabanı'nı çalıştıran yeni bir sunucu oluşturmak için kullanılan örnek JSON şablonu aşağıdadır.
```json
{
  "apiVersion": "2017-12-01",
  "type": "Microsoft.DBforMySQL/servers",
  "name": "string",
  "location": "string",
  "tags": "string",
  "properties": {
    "version": "string",
    "sslEnforcement": "string",
    "administratorLogin": "string",
    "administratorLoginPassword": "string",
    "storageProfile": {
      "storageMB": "string",
      "backupRetentionDays": "string",
      "geoRedundantBackup": "string"
    }
  },
  "sku": {
    "name": "string",
    "tier": "string",
    "capacity": "string",
    "family": "string"
  },
  "resources": [
    {
      "name": "AllowSubnet",
      "type": "virtualNetworkRules",
      "apiVersion": "2017-12-01",
      "properties": {
        "virtualNetworkSubnetId": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworkName'), parameters('subnetName'))]",
        "ignoreMissingVnetServiceEndpoint": true
      },
      "dependsOn": [
        "[concat('Microsoft.DBforMySQL/servers/', parameters('serverName'))]"
      ]
    }
  ]
}
```
Bu istekte özelleştirilmek üzere gereken değerleri şunlardır:
+   `name` -MySQL sunucunuzun (olmadan, etki alanı adı) adını belirtin.
+   `location` -MySQL sunucunuz için geçerli bir Azure veri merkezi bölgesi belirtin. Örneğin, westus2.
+   `properties/version` -Dağıtmak için MySQL server sürümü belirtin. Örneğin, 5.6 veya 5.7.
+   `properties/administratorLogin` -MySQL yönetici oturum açma sunucusu belirtin. Azure_superuser, yönetici, yönetici, kök, konuk veya ortak yönetici oturum açma adı olamaz.
+   `properties/administratorLoginPassword` -Yukarıda belirtilen MySQL yönetici kullanıcı için parola belirtin.
+   `properties/sslEnforcement` -SslEnforcement etkinleştir/devre dışı bırakmak için etkin/devre dışı belirtin.
+   `storageProfile/storageMB` -Megabayt sunucusu için gereken en fazla sağlanan depolama boyutunu belirtin. Örneğin, 5120.
+   `storageProfile/backupRetentionDays` -Gün içinde İstenen yedekleme bekletme süresi belirtin. Örneğin, 7. 
+   `storageProfile/geoRedundantBackup` -Etkin/devre dışı Geo-DR gereksinimlerine bağlı olarak belirtin.
+   `sku/tier` -Dağıtım için temel, depolaması veya MemoryOptimized katmanını belirtin.
+   `sku/capacity` -Sanal çekirdek kapasitesini belirtin. Olası değerler, 2, 4, 8, 16, 32 veya 64 içerir.
+   `sku/family` -5. nesil sunucu dağıtımı için donanım oluşturma seçmek için belirtin.
+   `sku/name` -TierPrefix_family_capacity belirtin. Örneğin B_Gen5_1, GP_Gen5_16, MO_Gen5_32. Bkz: [fiyatlandırma katmanları](./concepts-pricing-tiers.md) bölge ve katmanı geçerli değerleri anlamak için belgeler.
+   `resources/properties/virtualNetworkSubnetId` -Azure MySQL sunucusu nereye yerleştirileceğini Vnet'te alt ağın Azure tanımlayıcısını belirtin. 
+   `tags(optional)` -Belirtin isteğe bağlı etiketlerdir kaynaklar faturalandırma için kategorilere ayırmak için kullanacağınız bir anahtar-değer çiftlerinin vs.

Kuruluşunuz için MySQL dağıtımları için Azure veritabanı otomatik hale getirmek için bir Azure Resource Manager şablonu oluşturmak için kullanmak istiyorsanız, öneri örneği başlatmak için olacaktır [Azure Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-managed-mysql-with-vnet) azure'da Hızlı Başlangıç GitHub Galerisi ilk üzerinde derleyin. 

Yeni Azure Resource Manager şablonları ve denemek istiyorsanız aşağıdaki adımları izleyerek başlayabilirsiniz:
+   Kopyala veya indir örnek [Azure Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-managed-mysql-with-vnet) Azure hızlı başlangıç Galerisi.  
+   Tercihinize bağlı parametre değerleri güncelleştirmek üzere azuredeploy.parameters.json değiştirin ve dosyayı kaydedin. 
+   Aşağıdaki komutları kullanarak Azure MySQL sunucusu oluşturmak için Azure CLI'yi kullanma

Bu öğreticide kod bloklarını çalıştırmak için kendi bilgisayarınızda tarayıcıda Azure Cloud Shell veya Azure CLI Yükleme'ı kullanabilirsiniz.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

```azurecli-interactive
az login
az group create -n ExampleResourceGroup  -l “West US2”
az group deployment create -g $ ExampleResourceGroup   --template-file $ {templateloc} --parameters $ {parametersloc}
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
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "mydemoserver.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforMySQL/servers/mydemoserver",
  "location": "westus2",
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

## <a name="connect-to-the-server-using-mysql"></a>mysql kullanarak sunucuya bağlanma
MySQL sunucusu için Azure Veritabanınız ile bağlantı kurmak üzere [mysql komut satırı aracını](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) kullanın. Bu örnekte, komut şöyledir:
```cmd
mysql -h mydemoserver.database.windows.net -u myadmin@mydemoserver -p
```

## <a name="create-a-blank-database"></a>Boş veritabanı oluşturma
Sunucuya bağlandıktan sonra, boş bir veritabanı oluşturun.
```sql
mysql> CREATE DATABASE mysampledb;
```

İstemde, bağlantıyı bu yeni oluşturulan veritabanına değiştirmek için şu komutu çalıştırın:
```sql
mysql> USE mysampledb;
```

## <a name="create-tables-in-the-database"></a>Veritabanında tablo oluşturma
Artık MySQL veritabanı için Azure Veritabanına nasıl bağlanacağınızı bildiğinize göre bazı temel görevleri tamamlayın.

İlk olarak, bir tablo oluşturun ve bu tabloya bazı veriler yükleyin. Envanter bilgilerini depolayan bir tablo oluşturalım.
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-the-tables"></a>Tablolara veri yükleme
Bir tablonuz olduğuna göre içine bazı veriler ekleyin. Açık komut istemi penceresinde, birkaç veri satırı eklemek için şu sorguyu çalıştırın.
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

Daha önce oluşturduğunuz tabloda artık iki satırlık örnek verileriniz var.

## <a name="query-and-update-the-data-in-the-tables"></a>Tablolardaki verileri sorgulama ve güncelleştirme
Veritabanı tablosundan bilgi almak için şu sorguyu yürütün.
```sql
SELECT * FROM inventory;
```

Ayrıca tablolardaki verileri de güncelleştirebilirsiniz.
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

Siz veri aldıkça satır güncelleştirilir.
```sql
SELECT * FROM inventory;
```

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:
> [!div class="checklist"]
> * Azure Resource Manager şablonu kullanarak sanal ağ hizmet uç ile MySQL sunucusu için Azure veritabanı oluşturma
> * Veritabanı oluşturmak için [mysql komut satırı aracı](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) kullanma
> * Örnek verileri yükleme
> * Verileri sorgulama
> * Verileri güncelleştirme
> 
> [MySQL için Azure Veritabanına nasıl uygulama bağlanır](./howto-connection-string.md)
