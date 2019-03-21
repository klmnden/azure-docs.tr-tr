---
title: Azure Media Services hesabı - Azure CLI oluşturma | Microsoft Docs
description: Azure CLI betiği, Azure Media Services hesabı oluşturmak için kullanın.
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
ms.openlocfilehash: 7e668852346de1b48bd34658dea001435ba8a625
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "57842235"
---
# <a name="cli-example-create-an-azure-media-services-account"></a>CLI örneği: Azure Media Services hesabı oluşturma

Bu konudaki Azure CLI betiği, Azure Media Services hesabı oluşturmayı gösterir. 

Media Services hesabı ve tüm ilişkili depolama hesapları aynı Azure aboneliğinde olması gerekir. Media Services hesabıyla aynı konumda depolama hesaplarını kullanmak için önerilir.

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
