---
title: Azure CLI Betik Örneği - Laboratuvarda sanal makine başlatma | Microsoft Docs
description: Bu Azure CLI betiği, Azure DevTest Labs’deki bir laboratuvarda bir sanal makine başlatır.
services: lab-services
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/12/2018
ms.author: spelluru
ms.custom: mvc
ms.openlocfilehash: a635766c1a7fb9ae10a651d09ecd7da9a5f01e51
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763145"
---
# <a name="use-azure-cli-to-start-a-virtual-machine-in-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs’deki bir laboratuvarda bir sanal makine başlatmak için Azure CLI’yi kullanma

Bu Azure CLI betiği, laboratuvardaki bir sanal makineyi (VM) başlatır. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/devtest-lab/start-connect-virtual-machine-in-lab/start-connect-virtual-machine-in-lab.sh "Start a VM")]


## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır:

| Komut | Notlar |
|---|---|
| [az lab vm start ](/cli/azure/lab/vm?view=azure-cli-latest#az-lab-vm-start) | Laboratuvardaki bir sanal makineyi (VM) başlatır. Bu işlemin tamamlanması biraz zaman alabilir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek Azure Lab Services CLI betik örnekleri, [Azure Lab Services CLI örnekleri](../samples-cli.md) içinde bulunabilir.
