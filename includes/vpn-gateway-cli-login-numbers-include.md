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
ms.openlocfilehash: 82de8eab089e5f666e1a2ce4eab09bfd2895185b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47020090"
---
1. [az login](/cli/azure/#login) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin. Oturum açma hakkında daha fazla bilgi için bkz. [Azure CLI kullanmaya başlama](/cli/azure/get-started-with-azure-cli).

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
