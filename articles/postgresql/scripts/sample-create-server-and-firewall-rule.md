---
title: "Azure CLI SCRIPT - PostgreSQL için bir Azure veritabanı oluşturma | Microsoft Docs"
description: "Azure CLI komut dosyası örneği - PostgreSQL sunucu için bir Azure veritabanı oluşturur ve bir sunucu düzeyinde güvenlik duvarı kuralı yapılandırır."
services: postgresql
author: salonisonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: azure-cli
ms.topic: sample
ms.date: 05/31/2017
ms.openlocfilehash: e545b568cd57fdcf28ab33a5ebfa34a495111c7f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-azure-database-for-postgresql-server-and-configure-a-firewall-rule-using-the-azure-cli"></a>PostgreSQL sunucu için bir Azure veritabanı oluşturma ve Azure CLI kullanarak bir güvenlik duvarı kuralı yapılandırma
Bu örnek CLI betik PostgreSQL sunucu için bir Azure veritabanı oluşturur ve bir sunucu düzeyinde güvenlik duvarı kuralı yapılandırır. Betik başarılı şekilde gerçekleştirildikten sonra tüm Azure hizmetlerini ve yapılandırılan IP adresi PostgreSQL sunucunun erişilebilir.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek komut dosyası
Bu örnek betik yönetici kullanıcı adı ve parola özelleştirmek için vurgulanan satırlar düzenleyin.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/create-postgresql-server-and-firewall-rule/create-postgresql-server-and-firewall-rule.sh?highlight=15-16 "Create an Azure Database for PostgreSQL, and server-level firewall rule.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Komut dosyası örneği çalıştırdıktan sonra kaynak grubu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanılabilir.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/create-postgresql-server-and-firewall-rule/delete-postgresql.sh "Delete the resource group.")]

## <a name="script-explanation"></a>Komut dosyası açıklaması
Bu komut dosyasını aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| **Komutu** | **Notlar** |
|---|---|
| [az grubu oluşturma](/cli/azure/group#create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az postgres sunucu oluşturma](/cli/azure/postgres/server#create) | Veritabanlarını barındıran bir PostgreSQL sunucusu oluşturur. |
| [az postgres sunucu güvenlik duvarı oluşturma](/cli/azure/postgres/server/firewall-rule#create) | Girilen IP adresi aralığından altındaki veritabanlarını ve sunucu erişmesine izin vermek için bir güvenlik duvarı kuralı oluşturur. |
| [az grubu Sil](/cli/azure/group#delete) | Tüm iç içe kaynaklar dahil olmak üzere bir kaynak grubu siler. |

## <a name="next-steps"></a>Sonraki adımlar
- Azure CLI hakkında daha fazla bilgi okuyun: [Azure CLI belgeleri](/cli/azure/overview)
- Ek komut dosyaları deneyin: [PostgreSQL Azure veritabanı için Azure CLI örnekleri](../sample-scripts-azure-cli.md)
