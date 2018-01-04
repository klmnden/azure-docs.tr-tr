---
title: "CLI İzleyici ölçek tek örnek Azure SQL veritabanı | Microsoft Docs"
description: "Tek bir Azure SQL veritabanı ölçekleme ve izlemek için azure CLI örnek betik"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune, mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 12/14/2017
ms.author: janeng
<<<<<<< HEAD
ms.openlocfilehash: 5913c8ec1b62fc38161e553dc2364c793951e047
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
=======
ms.openlocfilehash: 741c066d62364e34b788883bfc96fba1ea3507c3
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2017
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
---
# <a name="use-cli-to-monitor-and-scale-a-single-sql-database"></a>İzlemek ve tek bir SQL veritabanı ölçeklendirmek için CLI kullanın

Bu Azure CLI betik örnek veritabanının boyutu bilgileri sorgulama sonra farklı performans düzeyi tek bir Azure SQL veritabanına ölçeklendirir. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.sh "Monitor and scale single SQL Database")]

> [!TIP]
> Kullanmak [az sql db op listesi](/cli/azure/sql/db/op?#az_sql_db_op_list) kullanın ve veritabanı üzerinde gerçekleştirilen işlemler listesini almak için [az sql db op iptal](/cli/azure/sql/db/op#az_sql_db_op_cancel) veritabanında bir güncelleştirme işlemi iptal etmek için.

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Komut dosyası örneği çalıştırdıktan sonra kaynak grubu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanılabilir.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyasını aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [az grubu oluşturma](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az sql server oluşturun](https://docs.microsoft.com/cli/azure/sql/server#az_sql_server_create) | Bir veritabanını barındıran bir mantıksal sunucu oluşturur. |
| [az sql db Göster-kullanım](https://docs.microsoft.com/cli/azure/sql/db#az_sql_db_show_usage) | Bir veritabanı boyutu kullanım bilgilerini gösterir. |
| [az sql db güncelleştirme](https://docs.microsoft.com/cli/azure/sql/db#az_sql_db_update) | Veritabanı Özellikleri (örneğin, hizmet katmanı veya performans düzeyini) güncelleştirir veya bir veritabanı içine, dışı veya esnek havuzlar arasında taşır. |
| [az grubu Sil](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Tüm iç içe kaynaklar dahil olmak üzere bir kaynak grubu siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek SQL veritabanı CLI kod örnekleri bulunabilir [Azure SQL veritabanı belgeleri](../sql-database-cli-samples.md).
