---
title: Azure CLI Betik Örneği - Hesabınızın kimlik bilgilerini sıfırlama | Microsoft Docs
description: Azure CLI betiğini kullanarak hesabınızın kimlik bilgilerini sıfırlayın ve app.config ayarlarını geri alın.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/11/2018
ms.author: juliako
ms.openlocfilehash: 5ec63bd8be84481780337d0b5bc2497770f22b0d
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="cli-example-reset-the-account-credentials"></a>CLI örneği: Hesap kimlik bilgilerini sıfırlama

Bu makaledeki Azure CLI betiği, hesabınızın kimlik bilgilerini sıfırlamayı ve app.config ayarlarını geri almayı gösterir.

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0.20 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../../cli_scripts/media-services/reset-account-credentials/Reset-Account-Credentials.sh "Reset credentials")]

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla örnek için bkz. [Azure CLI örnekleri](../cli-samples.md).
