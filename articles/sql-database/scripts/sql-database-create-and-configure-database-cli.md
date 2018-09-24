---
title: CLI örneği-Azure SQL veritabanı oluşturma | Microsoft Docs
description: Bir SQL veritabanı oluşturmak için bu Azure CLI örnek betiğini kullanın.
services: sql-database
documentationcenter: sql-database
author: CarlRabeler
manager: janeng
editor: carlrab
ms.service: sql-database
ms.custom: DBs & servers, mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.author: carlrab
ms.date: 09/07/2018
ms.openlocfilehash: a7537b59a73c00a5364bfa4a758a6584a9ef6003
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46964284"
---
# <a name="use-cli-to-create-a-single-azure-sql-database-and-configure-a-firewall-rule"></a>Tek bir Azure SQL veritabanı oluşturmak ve bir güvenlik duvarı kuralını yapılandırmak için CLI kullanma

Bu Azure CLI betiği örneği, bir Azure SQL veritabanı oluşturur ve sunucu düzeyinde bir güvenlik duvarı kuralını yapılandırır. Betik başarılı şekilde çalıştırıldıktan sonra, SQL Veritabanı tüm Azure hizmetlerinden ve yapılandırılmış IP adresinden erişilebilir. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu, Azure CLI 2.0 veya sonraki bir sürümünü kullanmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/create-and-configure-database/create-and-configure-database.sh?highlight=9-10 "Create SQL Database")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Betik örneği çalıştırıldıktan sonra, kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az sql server create](/cli/azure/sql/server#az_sql_server_create) | SQL Veritabanını barındıran bir mantıksal sunucu oluşturur. |
| [az sql server firewall create](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_create) | Sunucudaki tüm SQL Veritabanlarına, girilen IP adresi aralığından erişmeyi sağlayacak bir güvenlik duvarı kuralı oluşturur. |
| [az sql db create](/cli/azure/sql/db#az_sql_db_create) | Mantıksal sunucuda SQL Veritabanı oluşturur. |
| [az group delete](/cli/azure/resource#az_resource_delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek SQL Veritabanı CLI betiği örnekleri, [Azure SQL Veritabanı belgeleri](../sql-database-cli-samples.md) içinde bulunabilir.

