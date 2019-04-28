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
ms.openlocfilehash: 74d7bd087df4b00c0bafb5ec33fbbdfa5c57b379
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60390684"
---
[az login](/cli/azure/) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin. Oturum açma hakkında daha fazla bilgi için bkz. [Azure CLI kullanmaya başlama](/cli/azure/get-started-with-azure-cli).

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
