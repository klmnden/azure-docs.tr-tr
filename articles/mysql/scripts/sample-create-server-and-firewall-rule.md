---
title: "Azure CLI komut dosyası - Azure veritabanı için MySQL oluşturun. | Microsoft Docs"
description: "Bu örnek CLI betik MySQL sunucusu için bir Azure veritabanı oluşturur ve bir sunucu düzeyinde güvenlik duvarı kuralı yapılandırır."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.custom: mvc
ms.topic: sample
ms.date: 11/02/2017
ms.openlocfilehash: 50893a2265fcb9a610564748ba316a70c5205b4a
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="create-a-mysql-server-and-configure-a-firewall-rule-using-the-azure-cli"></a>MySQL sunucusu oluşturun ve Azure CLI kullanarak bir güvenlik duvarı kuralı yapılandırma
Bu örnek CLI betik MySQL sunucusu için bir Azure veritabanı oluşturur ve bir sunucu düzeyinde güvenlik duvarı kuralı yapılandırır. Komut dosyası başarıyla çalıştıktan sonra MySQL server tüm Azure hizmetlerini ve yapılandırılan IP adresi tarafından erişilebilir.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, bu makalede, Azure CLI Sürüm 2.0 veya sonraki sürümünü çalıştırdığınızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek komut dosyası
Bu örnek betik yönetici kullanıcı adı ve parola özelleştirmek için vurgulanan satırlar düzenleyin.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/create-mysql-server-and-firewall-rule/create-mysql-server-and-firewall-rule.sh?highlight=15-16 "Create an Azure Database for MySQL, and server-level firewall rule.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Komut dosyası örneği çalıştırdıktan sonra kaynak grubu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanılabilir.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/create-mysql-server-and-firewall-rule/delete-mysql.sh "Delete the resource group.")]

## <a name="script-explanation"></a>Komut dosyası açıklaması
Bu komut dosyasını aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| **Komutu** | **Notlar** |
|---|---|
| [az grubu oluşturma](/cli/azure/group#create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az mysql sunucusu oluşturun](/cli/azure/mysql/server#create) | Veritabanlarını barındıran MySQL server oluşturur. |
| [az mysql server güvenlik duvarı oluşturma](/cli/azure/mysql/server/firewall-rule#create) | Girilen IP adresi aralığından altındaki veritabanlarını ve sunucu erişmesine izin vermek için bir güvenlik duvarı kuralı oluşturur. |
| [az grubu Sil](/cli/azure/group#delete) | Tüm iç içe kaynaklar dahil olmak üzere bir kaynak grubu siler. |

## <a name="next-steps"></a>Sonraki adımlar
- Azure CLI hakkında daha fazla bilgi okuyun: [Azure CLI belgelerine](/cli/azure/overview).
- Ek komut dosyaları deneyin: [Azure veritabanı için MySQL için Azure CLI örnekleri](../sample-scripts-azure-cli.md)
