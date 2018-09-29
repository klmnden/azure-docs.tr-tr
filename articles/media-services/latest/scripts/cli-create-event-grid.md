---
title: Azure CLI Betiği Örneği - Azure Event Grid aboneliği oluşturma | Microsoft Docs
description: Bu konudaki Azure CLI betiği, İş Durumu Değişiklikleri için hesap düzeyinde bir Event Grid aboneliği oluşturmayı gösterir.
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
ms.openlocfilehash: 5293ac55fdb13bba85996f5ed81034d4ebeeff12
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47093029"
---
# <a name="cli-example-create-an-azure-event-grid-subscription"></a>CLI örneği: Azure Event Grid aboneliği oluşturma 

Bu makaledeki Azure CLI betiği, İş Durumu Değişiklikleri için hesap düzeyinde bir Event Grid aboneliği oluşturmayı gösterir.

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0.20 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../../cli_scripts/media-services/create-event-grid/Create-EventGrid.sh "Create an EventGrid subscription")]

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla örnek için bkz. [Azure CLI örnekleri](../cli-samples.md).
