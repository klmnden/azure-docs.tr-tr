---
title: "Azure CLI aracını kullanarak PostgreSQL için Azure Veritabanı oluşturma | Microsoft Docs"
description: "Azure CLI (komut satırı arabirimi) aracını kullanarak PostgreSQL için Azure Veritabanı sunucusu oluşturma ve yönetmeye yönelik hızlı başlangıç kılavuzu."
services: postgresql
author: sanagama
ms.author: sanagama
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.devlang: azure-cli
ms.topic: hero-article
ms.date: 06/13/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 4f68f90c3aea337d7b61b43e637bcfda3c98f3ea
ms.openlocfilehash: b2be9e265075c58ed53a0a49c01a08e05db35a06
ms.contentlocale: tr-tr
ms.lasthandoff: 06/20/2017

---
<a id="create-an-azure-database-for-postgresql-using-the-azure-cli" class="xliff"></a>

# Azure CLI aracını kullanarak PostgreSQL için Azure Veritabanı oluşturma
PostgreSQL için Azure Veritabanı, bulutta yüksek düzeyde kullanılabilir olan PostgreSQL veritabanları çalıştırmanızı, yönetmenizi ve ölçeklendirmenizi sağlayan, yönetilen bir hizmettir. Azure CLI, komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu hızlı başlangıçta, Azure CLI aracını kullanarak bir [Azure kaynak grubunda](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) nasıl PostgreSQL için Azure Veritabanı sunucusu oluşturabileceğiniz gösterilir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

Birden fazla aboneliğiniz varsa kaynağın faturalanacağı uygun aboneliği seçin. [az account set](/cli/azure/account#set) komutunu kullanarak hesabınız altındaki belirli bir abonelik kimliğini seçin.
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

<a id="create-a-resource-group" class="xliff"></a>

## Kaynak grubu oluşturma

[az group create](/cli/azure/group#create) komutunu kullanarak bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) oluşturun. Kaynak grubu, Azure kaynaklarının grup olarak dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek `westus` konumunda `myresourcegroup` adlı bir kaynak grubu oluşturur.
```azurecli-interactive
az group create --name myresourcegroup --location westus
```

<a id="create-an-azure-database-for-postgresql-server" class="xliff"></a>

## PostgreSQL için Azure Veritabanı sunucusu oluşturma

[az postgres server create](/cli/azure/postgres/server#create) komutunu kullanarak [PostgreSQL sunucusu için Azure SQL Veritabanı ](overview.md) oluşturun. Sunucu, grup olarak yönetilen bir veritabanı grubu içerir. 

Aşağıdaki örnekte, `mylogin` sunucu yöneticisi oturum açma adıyla `myresourcegroup` kaynak grubunuzda `mypgserver-20170401` adlı bir sunucu oluşturulur. Bir sunucunun adı DNS adıyla eşleşir ve bu nedenle sunucunun Azure’da genel olarak benzersiz olması gerekir. `<server_admin_password>` değerini kendi değerinizle değiştirin.
```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mypgserver-20170401  --location westus --admin-user mylogin --admin-password <server_admin_password> --performance-tier Basic --compute-units 50 --version 9.6
```

> [!IMPORTANT]
> Burada belirttiğiniz sunucu yöneticisi kullanıcı adı ve parolası, bu hızlı başlangıcın sonraki bölümlerinde sunucuda ve veritabanlarında oturum açmak için gereklidir. Bu bilgileri daha sonra kullanmak üzere aklınızda tutun veya kaydedin.

Varsayılan olarak, **postgres** veritabanı sunucunuz altında oluşturulur. [Postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) veritabanı; kullanıcılar, yardımcı programlar ve üçüncü taraf uygulamalar tarafından kullanılmak üzere geliştirilmiş varsayılan bir veritabanıdır. 


<a id="configure-a-server-level-firewall-rule" class="xliff"></a>

## Sunucu düzeyinde güvenlik duvarı kuralı yapılandırma

[az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) komutunu kullanarak Azure PostgreSQL sunucusu düzeyinde bir güvenlik duvarı kuralı oluşturun. Sunucu düzeyindeki bir güvenlik duvarı kuralı, [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) veya [PgAdmin](https://www.pgadmin.org/) gibi bir dış uygulamanın Azure PostgreSQL hizmetinin güvenlik duvarı üzerinden sunucunuza bağlanmasına imkan tanır. 

Ağınızdan bağlanabilmek için bir IP aralığını kapsayan bir güvenlik duvarı kuralı ayarlayabilirsiniz. Aşağıdaki örnekte, [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) komutu kullanılarak bir IP adresi aralığı için `AllowAllIps` güvenlik duvarı kuralı oluşturulur. Tüm IP adreslerini açmak için başlangıç IP adresi olarak 0.0.0.0’ı, bitiş adresi olaraksa 255.255.255.255’i kullanın.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server mypgserver-20170401 --name AllowAllIps --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> Azure PostgreSQL sunucusu, 5432 bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden bağlanıyorsanız ağınızın güvenlik duvarı tarafından 5432 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. BT departmanınızdan Azure SQL Veritabanı sunucunuzla bağlantı için 5432 numaralı bağlantı noktasını açmasını isteyin.

<a id="get-the-connection-information" class="xliff"></a>

## Bağlantı bilgilerini alma

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

<a id="connect-to-postgresql-database-using-psql" class="xliff"></a>

## psql’yi kullanarak PostgreSQL veritabanına bağlanma

İstemci bilgisayarınızda PostgreSQL yüklüyse, [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html)’nin yerel bir örneğini kullanarak Azure PostgreSQL sunucusuna bağlanabilirsiniz. Şimdi psql komut satırı yardımcı programını kullanarak Azure PostgreSQL sunucusuna bağlanalım.

1. PostgreSQL için Azure Veritabanı sunucusuna bağlanmak üzere aşağıdaki psql komutunu çalıştırın
```azurecli-interactive
psql --host=<servername> --port=<port> --username=<user@servername> --dbname=<dbname>
```

  Örneğin aşağıdaki komut, erişim kimlik bilgilerini kullanarak **mypgserver-20170401.postgres.database.azure.com** PostgreSQL sunucunuzda **postgres** adlı varsayılan veritabanına bağlanır. Parola istendiğinde seçtiğiniz `<server_admin_password>` değerini girin.
  
  ```azurecli-interactive
psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
```

2.  Sunucuya bağlandıktan sonra, istemde boş bir veritabanı oluşturun.
```sql
CREATE DATABASE mypgsqldb;
```

3.  İstemde, bağlantıyı yeni oluşturulan **mypgsqldb** veritabanına geçirmek için aşağıdaki komutu yürütün:
```sql
\c mypgsqldb
```

<a id="connect-to-postgresql-database-using-pgadmin" class="xliff"></a>

## pgAdmin’i kullanarak PostgreSQL veritabanına bağlanma

_pgAdmin_ GUI aracını kullanarak Azure PostgreSQL sunucusuna bağlanmak için
1.  İstemci bilgisayarınızda _pgAdmin_ uygulamasını başlatın. _pgAdmin_’i http://www.pgadmin.org/ adresinden yükleyebilirsiniz.
2.  **Hızlı Bağlantılar** menüsünden **Yeni Sunucu Ekle**’yi seçin.
3.  **Oluştur - Sunucu** iletişim kutusunun **Genel** sekmesinde, sunucu için benzersiz bir kolay ad girin. Örneğin, **Azure PostgreSQL Sunucusu**.
 ![pgAdmin tool - create - server](./media/quickstart-create-server-database-azure-cli/1-pgadmin-create-server.png)
4.  **Oluştur - Sunucu** iletişim kutusunda, **Bağlantı** sekmesi:
    - **Ana Bilgisayar Adı/ Adresi** kutusuna tam sunucu adını (örneğin, **mypgserver-20170401.postgres.database.azure.com**) girin. 
    - **Bağlantı Noktası** kutusuna 5432 numaralı bağlantı noktasını girin. 
    - **Kullanıcı Adı** ve **Parola** kutularına, sırasıyla bu hızlı başlangıçta daha önce edinilen **Sunucu yöneticisi oturum açma adını (user@mypgserver)** ve sunucuyu oluştururken girdiğiniz parolayı girin.
    - **SSL Modu**’nu **Gerekli** olarak seçin. Tüm Azure PostgreSQL sunucuları, varsayılan olarak SSL’yi zorunlu tutma seçeneği AÇIK olacak şekilde oluşturulur. SSL’yi zorunlu tutma seçeneğini KAPALI konumuna getirmek için [SSL’yi zorunlu tutma](./concepts-ssl-connection-security.md) bölümündeki ayrıntıları inceleyin.

    ![pgAdmin - Oluştur - Sunucu](./media/quickstart-create-server-database-azure-cli/2-pgadmin-create-server.png)
5.  **Kaydet** düğmesine tıklayın.
6.  Tarayıcı sol bölmesinde **Sunucu Grupları**’nı genişletin. **Azure PostgreSQL Sunucusu** adlı sunucunuzu seçin.
7.  Bağlandığınız **Sunucu**’yu ve ardından altındaki **Veritabanları**’nı seçin. 
8.  Veritabanı oluşturmak için **Veritabanları**’na sağ tıklayın.
9.  Veritabanı adı için **mypgsqldb**’yi ve sahibi için sunucu yöneticisi oturum açma bilgileri olarak **mylogin**’i seçin.
10. Boş bir veritabanı oluşturmak için **Kaydet**’e tıklayın.
11. **Tarayıcı**’da **Sunucular** grubunu genişletin. Oluşturduğunuz sunucuyu genişletin ve altındaki **mypgsqldb** veritabanını bulun.
 ![pgAdmin - Oluştur - Veritabanı](./media/quickstart-create-server-database-azure-cli/3-pgadmin-database.png)


<a id="clean-up-resources" class="xliff"></a>

## Kaynakları temizleme

[Azure kaynak grubunu](../azure-resource-manager/resource-group-overview.md) silerek hızlı başlangıçta oluşturduğunuz tüm kaynakları temizleyin.

> [!TIP]
> Bu koleksiyondaki diğer hızlı başlangıçlar, bu hızlı başlangıcı temel alır. Sonraki hızlı başlangıçlarla çalışmaya devam etmeyi planlıyorsanız bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, Azure CLI aracında bu hızlı başlangıç ile oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

```azurecli-interactive
az group delete --name myresourcegroup
```

Yeni oluşturulan tek bir sunucuyu silmek istiyorsanız [az postgres server delete](/cli/azure/postgres/server#delete) komutunu kullanabilirsiniz.
```azurecli-interactive
az postgres server delete --resource-group myresourcegroup --name mypgserver-20170401
```

<a id="next-steps" class="xliff"></a>

## Sonraki adımlar
> [!div class="nextstepaction"]
> [Dışarı Aktarma ve İçeri Aktarma seçeneğini kullanarak veritabanınızı geçirme](./howto-migrate-using-export-and-import.md)

