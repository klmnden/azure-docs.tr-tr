---
title: "Azure CLI: Tek bir SQL veritabanı oluşturma ve sorgu yürütme | Microsoft Belgeleri"
description: "Azure CLI kullanarak SQL Veritabanı mantıksal sunucusu, sunucu düzeyi güvenlik duvarı kuralı ve veritabanı oluşturmayı öğrenin."
keywords: "sql veritabanı öğreticisi, sql veritabanı oluşturma"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: quick start
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: cli
ms.topic: hero-article
ms.date: 03/13/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: 8322e46f462b6c940f9808411d99aa1cee0beea5
ms.lasthandoff: 03/15/2017

---

# <a name="create-and-query-a-single-azure-sql-database-with-the-azure-cli"></a>Azure CLI ile tek bir Azure SQL veritabanı oluşturma ve sorgu yürütme

Azure CLI, komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu kılavuzda Azure CLI kullanarak bir Azure SQL veritabanı dağıtma işleminin ayrıntıları verilmektedir.

Başlamadan önce Azure CLI’nin yüklü olduğundan emin olun. Daha fazla bilgi için bkz. [Azure CLI yükleme kılavuzu](https://docs.microsoft.com/cli/azure/install-azure-cli). 

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[az login](/cli/azure/#login) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#create) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek `westeurope` konumunda `myResourceGroup` adlı bir kaynak grubu oluşturur.

```azurecli
az group create --name myResourceGroup --location westeurope
```
## <a name="create-a-logical-server"></a>Mantıksal sunucu oluşturma

[az sql server create](/cli/azure/sql/server#create) komutuyla mantıksal bir sunucu oluşturun. Aşağıdaki örnek, kaynak grubunuzda `ServerAdmin` yönetici oturum açma bilgileri ve `ChangeYourAdminPassword1` parolası ile rastgele olarak adlandırılmış bir sunucu oluşturur. Bu önceden tanımlı değerleri istediğiniz gibi değiştirin.

```azurecli
servername=server-$RANDOM
az sql server create --name $servername --resource-group myResourceGroup --location westeurope \
    --admin-user ServerAdmin --admin-password ChangeYourAdminPassword1
```

## <a name="configure-a-server-firewall-rule"></a>Sunucu güvenlik duvarı kurallarını yapılandırma

[az sql server firewall create](/cli/azure/sql/server/firewall-rule#create) komutuyla sunucu düzeyinde bir güvenlik duvarı kuralı oluşturun. Bir sunucu düzeyi güvenlik duvarı kuralı SQL Server Management Studio veya SQLCMD yardımcı programı gibi bir dış uygulamanın SQL Veritabanı hizmet güvenlik duvarı üzerinden bir SQL veritabanına bağlanmasına olanak sağlar. Aşağıdaki örnekte önceden tanımlanmış bir adres aralığı için bir güvenlik duvarı kuralı oluşturulmaktadır. Bu örnek için aralık, olası tüm IP adresleri aralığıdır. Bu önceden tanımlı değerleri kendi dış IP adresiniz veya IP adresi aralığınız için değerlerle değiştirin. 

```azurecli
az sql server firewall-rule create --resource-group myResourceGroup --server $servername \
    -n AllowYourIp --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

## <a name="create-a-database-in-the-server"></a>Sunucuda veritabanı oluşturma

[az sql db create](/cli/azure/sql/db#create) komutuyla sunucuda bir veritabanı oluşturun. Aşağıdaki örnek `mySampleDatabase` adlı boş bir veritabanı oluşturur. Bu önceden tanımlı değeri istediğiniz gibi değiştirin.

```azurecli
az sql db create --resource-group myResourceGroup --server $servername \
    --name mySampleDatabase --service-objective S0
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu Hızlı Başlangıç tarafından oluşturulan tüm kaynakları kaldırmak için, şu komutu çalıştırın:

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

- SQL Server Management Studio kullanarak bağlanmak ve sorgu yürütmek için, bkz. [SSMS ile bağlanma ve sorgu yürütme](sql-database-connect-query-ssms.md)
- Visual Studio kullanarak bağlanmak için bkz. [Visual Studio ile bağlanma ve sorgu yürütme](sql-database-connect-query.md).
- SQL Veritabanı’na teknik bir genel bakış için, bkz. [SQL Veritabanı hizmeti hakkında](sql-database-technical-overview.md).

