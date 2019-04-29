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
ms.openlocfilehash: 21dbec52dded5a195cc764741ab9e8d79737c549
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60411866"
---
1. [az login](/cli/azure/) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin. Oturum açma hakkında daha fazla bilgi için bkz. [Azure CLI kullanmaya başlama](/cli/azure/get-started-with-azure-cli).

   ```azurecli
   az login
   ```
2. Birden çok Azure aboneliğiniz varsa, hesabın aboneliklerini listeleyin.

   ```azurecli
   az account list --all
   ```
3. Kullanmak istediğiniz aboneliği belirtin.

   ```azurecli
   az account set --subscription <replace_with_your_subscription_id>
   ```
