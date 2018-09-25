---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: e3961878663a26d721478effc01d968e21e34c18
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47020804"
---
[az login](/cli/azure/#login) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin. Oturum açma hakkında daha fazla bilgi için bkz. [Azure CLI ile çalışmaya başlama](/cli/azure/get-started-with-azure-cli).

```azurecli
az login
```

Birden çok Azure aboneliğiniz varsa, hesabın aboneliklerini listeleyin.

```azurecli
az account list --all
```

Kullanmak istediğiniz aboneliği belirtin.

```azurecli
az account set --subscription <replace_with_your_subscription_id>
```
