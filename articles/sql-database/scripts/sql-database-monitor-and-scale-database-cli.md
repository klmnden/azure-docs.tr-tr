---
title: CLI örneği-tek bir Azure SQL veritabanı izleme-ölçeklendirme | Microsoft Docs
description: Tek bir Azure SQL veritabanını izleme ve ölçeklendirmeye yönelik Azure CLI örnek betiği
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 10/02/2018
ms.openlocfilehash: 9021f04dad41f81b935b9412900336687011e87a
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48249273"
---
# <a name="use-cli-to-monitor-and-scale-a-single-sql-database"></a>CLI kullanarak tek bir SQL veritabanını izleme ve ölçeklendirme

Bu Azure CLI betik örneği, veritabanının boyut bilgilerini sorguladıktan sonra tek bir Azure SQL veritabanı örneğini farklı bir işlem boyutuna ölçeklendirir. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.sh "Monitor and scale single SQL Database")]

> [!TIP]
> Veritabanı üzerinde gerçekleştirilen işlemlerin listesini almak için [az sql db op list](/cli/azure/sql/db/op?#az-sql-db-op-list), veritabanı üzerindeki bir güncelleştirme işlemini iptal etmek için [az sql db op cancel](/cli/azure/sql/db/op#az-sql-db-op-cancel) komutunu kullanın.

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Betik örneği çalıştırıldıktan sonra, kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az sql server create](https://docs.microsoft.com/cli/azure/sql/server#az-sql-server-create) | Veritabanını barındıran bir mantıksal sunucu oluşturur. |
| [az sql db show-usage](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-show-usage) | Bir veritabanının boyut kullanım bilgilerini gösterir. |
| [az sql db update](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-update) | Veritabanı özelliklerini (hizmet katmanı veya işlem boyutu) güncelleştirir veya bir veritabanını elastik havuzun içine veya dışına ya da elastik havuzlar arasında taşır. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az-vm-extension-set) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek SQL Veritabanı CLI betiği örnekleri, [Azure SQL Veritabanı belgeleri](../sql-database-cli-samples.md) içinde bulunabilir.
