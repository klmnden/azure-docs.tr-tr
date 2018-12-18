---
title: Azure CLI betiği - Bir MySQL için Azure Veritabanı sunucusunu zaman içinde önceki bir noktaya geri yükleme
description: Bu örnek CLI betiği, bir MySQL için Azure Veritabanı sunucusunu zaman içinde önceki bir noktaya geri yükler.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc
ms.date: 02/28/2018
ms.openlocfilehash: 8506e6eaac51730cb3d7a20f1d21f1c8bce03bee
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2018
ms.locfileid: "53540036"
---
# <a name="restore-an-azure-database-for-mysql-server-using-azure-cli"></a>Azure CLI kullanarak MySQL için Azure Veritabanı sunucusunu geri yükleme
Bu örnek CLI betiği, tek bir MySQL için Azure Veritabanı sunucusunu zaman içinde önceki bir noktaya geri yükler.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu örnek, Azure CLI 2.0 veya sonraki bir sürümünü kullanmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek betik
Bu örnek betikte, vurgulanan satırları değiştirerek yönetici kullanıcı adını ve parolasını özelleştirin. Az izleyici komutlarında kullanılan abonelik kimliğini kendi abonelik kimliğinizle değiştirin.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/backup-restore-pitr/backup-restore.sh?highlight=15-16 "Restore Azure Database for MySQL.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Betik örneği çalıştırıldıktan sonra, kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/backup-restore-pitr/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Betik açıklaması
Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| **Komut** | **Notlar** |
|---|---|
| [az group create](/cli/azure/group#create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az mysql server create](/cli/azure/mysql/server#create) | Veritabanlarını barındıran bir MySQL sunucusu oluşturur. |
| [az mysql server restore](/cli/azure/mysql/server#restore) | Bir sunucuyu yedekten geri yükler. |
| [az group delete](/cli/azure/group#delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar
- Azure CLI hakkında daha fazla bilgi edinin: [Azure CLI belgeleri](/cli/azure).
- Ek betikleri deneyin: [MySQL için Azure veritabanı Azure CLI örnekleri](../sample-scripts-azure-cli.md)
