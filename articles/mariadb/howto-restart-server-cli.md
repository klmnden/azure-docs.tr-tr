---
title: Azure veritabanı Azure CLI kullanarak MariaDB sunucuyu yeniden başlatın
description: Bu makalede, Azure CLI kullanarak MariaDB için Azure veritabanı nasıl yeniden açıklanır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/28/2019
ms.openlocfilehash: a6e0509d941d9bfdfe6db7a8b93ee49c5bece1a6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66171431"
---
# <a name="restart-azure-database-for-mariadb-server-using-the-azure-cli"></a>Azure veritabanı Azure CLI kullanarak MariaDB sunucuyu yeniden başlatın
Bu konuda, MariaDB server için Azure veritabanı nasıl yeniden açıklanmaktadır. Sunucu işlemi gerçekleştirirken, kısa bir kesintiye neden sunucunuzun bakım nedeniyle yeniden başlatmanız gerekebilir.

Sunucunun yeniden başlatılması, Hizmet meşgul olduğunda engellenir. Örneğin, hizmet sanal çekirdekler ölçeklendirme gibi daha önce istenen bir işlemin işliyor olabilir.

Yeniden başlatma tamamlamak için gereken süreyi MariaDB kurtarma işlemi bağlıdır. Yeniden başlatma süresini azaltmak için yeniden başlatma öncesinde sunucusunda gerçekleşen etkinliği miktarını en aza öneririz.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda tamamlanması gerekir:
- Bir [MariaDB için Azure veritabanı](quickstart-create-mariadb-server-database-using-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> Bu nasıl yapılır kılavuzunda, Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Azure CLI komut isteminde sürümünü onaylamak için girin `az --version`. Yüklemek veya yükseltmek için bkz: [Azure CLI yükleme]( /cli/azure/install-azure-cli).


## <a name="restart-the-server"></a>Sunucuyu yeniden başlatın

Aşağıdaki komutla sunucuyu yeniden başlatın:

```azurecli-interactive
az mariadb server restart --name mydemoserver --resource-group myresourcegroup
```

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [parametreleri MariaDB için Azure veritabanı'nda ayarlama](howto-configure-server-parameters-cli.md)