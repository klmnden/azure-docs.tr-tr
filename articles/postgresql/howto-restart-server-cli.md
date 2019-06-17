---
title: Azure veritabanını Postgresql'ye - Azure CLI kullanarak tek bir sunucu yeniden başlatın.
description: Bu makalede, PostgreSQL - Azure CLI kullanarak tek bir sunucu için Azure veritabanı nasıl yeniden başlatabilirsiniz açıklanır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 0a7cd815724fcebd6311860576e620eb9273523b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65068970"
---
# <a name="restart-azure-database-for-postgresql---single-server-using-the-azure-cli"></a>Azure veritabanını Postgresql'ye - Azure CLI kullanarak tek bir sunucu yeniden başlatın.
Bu konuda, PostgreSQL sunucusu için Azure veritabanı nasıl yeniden açıklanmaktadır. Sunucu işlemi gerçekleştirirken, kısa bir kesintiye neden sunucunuzun bakım nedeniyle yeniden başlatmanız gerekebilir.

Sunucunun yeniden başlatılması, Hizmet meşgul olduğunda engellenir. Örneğin, hizmet sanal çekirdekler ölçeklendirme gibi daha önce istenen bir işlemin işliyor olabilir.
 
Yeniden başlatma tamamlamak için gereken süreyi PostgreSQL kurtarma işlemi bağlıdır. Yeniden başlatma süresini azaltmak için yeniden başlatma öncesinde sunucusunda gerçekleşen etkinliği miktarını en aza öneririz.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda tamamlanması gerekir:
- Bir [PostgreSQL sunucusu için Azure veritabanı](quickstart-create-server-up-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> Bu nasıl yapılır kılavuzunda, Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Azure CLI komut isteminde sürümünü onaylamak için girin `az --version`. Yüklemek veya yükseltmek için bkz: [Azure CLI yükleme]( /cli/azure/install-azure-cli).


## <a name="restart-the-server"></a>Sunucuyu yeniden başlatın

Aşağıdaki komutla sunucuyu yeniden başlatın:

```azurecli-interactive
az postgres server restart --name mydemoserver --resource-group myresourcegroup
```

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [nasıl PostgreSQL için Azure veritabanı'nda parametrelerini ayarla](howto-configure-server-parameters-using-cli.md)