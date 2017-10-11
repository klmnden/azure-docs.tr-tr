---
title: "CLI oluşturmak örnek bir Azure SQL veritabanı | Microsoft Docs"
description: "Bir SQL veritabanı oluşturmak için azure CLI örnek betik"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 908898ca691d2b53b9f54afa60c41e091163bd50
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="use-cli-to-create-a-single-azure-sql-database-and-configure-a-firewall-rule"></a>Tek bir Azure SQL veritabanı oluşturma ve bir güvenlik duvarı kuralı yapılandırmak için CLI kullanın

Bu Azure CLI betik örnek bir Azure SQL veritabanı oluşturur ve bir sunucu düzeyinde güvenlik duvarı kuralı yapılandırın. Betik başarılı şekilde gerçekleştirildikten sonra SQL veritabanı tüm Azure hizmetlerini ve yapılandırılan IP adresi erişilebilir. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli-interactive[Ana](../../../cli_scripts/sql-database/create-and-configure-database/create-and-configure-database.sh?highlight=9-10 "SQL veritabanı oluşturma")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Komut dosyası örneği çalıştırdıktan sonra kaynak grubu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanılabilir.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyasını aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [az grubu oluşturma](/cli/azure/group#create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az sql server oluşturun](/cli/azure/sql/server#create) | SQL veritabanı barındıran bir mantıksal sunucu oluşturur. |
| [az sql server güvenlik duvarı oluşturma](/cli/azure/sql/server/firewall-rule#create) | Girilen IP adresi aralığından sunucusundaki tüm SQL veritabanlarına erişim sağlamak için bir güvenlik duvarı kuralı oluşturur. |
| [az sql db oluştur](/cli/azure/sql/db#create) | SQL Database mantıksal sunucu oluşturur. |
| [az grubu Sil](/cli/azure/resource#delete) | Tüm iç içe kaynaklar dahil olmak üzere bir kaynak grubu siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek SQL veritabanı CLI kod örnekleri bulunabilir [Azure SQL veritabanı belgeleri](../sql-database-cli-samples.md).

