---
title: Azure Media Services hesabı oluşturma - Azure CLI | Microsoft Docs
description: Bir Azure Media Services hesabı oluşturmak için Azure CLI betiğini kullanın.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/25/2019
ms.author: juliako
ms.openlocfilehash: 512eb765e626118e49d2732338d0d5bd7252f3f8
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2019
ms.locfileid: "55100589"
---
# <a name="cli-example-create-an-azure-media-services-account"></a>CLI örneği: Azure Media Services hesabı oluşturma

Bu konudaki Azure CLI betiği, Azure Media Services hesabı oluşturmayı gösterir. Media Services hesabı ve ilişkili depolama hesabı aynı veri merkezinde ve aynı kaynak grubunun parçası olması gerekir.

[!INCLUDE [media-services-cli-instructions.md](../../../../includes/media-services-cli-instructions.md)]

## <a name="example-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../../cli_scripts/media-services/media-services-create-account/Create-Account.sh "Create Account")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli
az group delete --name amsResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | Bir depolama hesabı oluşturur. |
| [az ams account create](https://docs.microsoft.com/cli/azure/ams/account?view=azure-cli-latest#az-ams-account-create) | Bir Media Services hesabı oluşturur. |
| [az ams account sp create](https://docs.microsoft.com/cli/azure/ams/account/sp?view=azure-cli-latest#az-ams-account-sp-create) | Parola ile bir hizmet sorumlusu oluşturur ve bu hizmet sorumlusunun Azure Media Services hesabına erişimini yapılandırır. 
| [az group delete](/cli/azure/group#az-group-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla örnek için bkz. [Azure CLI örnekleri](../cli-samples.md).
