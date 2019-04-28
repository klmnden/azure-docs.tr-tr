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
ms.openlocfilehash: 2159e594f0cc6a43905f3562c0ad6f3e3c9984fe
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61084477"
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
| [az lab vm start](/cli/azure/lab/vm?view=azure-cli-latest#az-lab-vm-start) | Laboratuvardaki bir sanal makineyi (VM) başlatır. Bu işlemin tamamlanması biraz zaman alabilir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek Azure Lab Services CLI betik örnekleri, [Azure Lab Services CLI örnekleri](../samples-cli.md) içinde bulunabilir.
