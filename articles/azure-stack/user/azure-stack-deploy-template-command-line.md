---
title: Şablonları Azure Stack'te komut satırı ile dağıtma | Microsoft Docs
description: Şablonları Azure Stack'e dağıtma için platformlar arası komut satırı arabirimi (CLI) kullanmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 9584177f-4af3-4834-864d-930b09ae0995
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2019
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: fec5378b9165ca7e240f52c629c47cdda799ef51
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2019
ms.locfileid: "54155193"
---
# <a name="deploy-templates-in-azure-stack-using-the-command-line"></a>Şablonları komut satırını kullanarak Azure Stack'te dağıtma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Resource Manager şablonları Azure Stack geliştirme Seti'ni ortamında dağıtmak için komut satırını kullanın. Azure Resource Manager şablonları, dağıtın ve tek ve eşgüdümlü bir işlemle uygulamanıza yönelik tüm kaynakları sağlayın.

## <a name="before-you-begin"></a>Başlamadan önce

- [Yükleme ve bağlanma](azure-stack-version-profiles-azurecli2.md) Azure CLI ile Azure Stack için.
- Dosyaları indirmek *azuredeploy.json* ve *azuredeploy.parameters.json* gelen [depolama hesabı örnek şablonu oluşturma](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-create-storage-account).

## <a name="deploy-template"></a>Şablon dağıtma

Bu dosyaları karşıdan yüklendiği klasöre gidin ve şablonu dağıtmak için aşağıdaki komutu çalıştırın:

```azurecli
az group create "cliRG" "local" –f azuredeploy.json –d "testDeploy" –e azuredeploy.parameters.json
```

Bu komut kaynak grubuna şablon dağıtır **cliRG** Azure Stack POC varsayılan konumda.

## <a name="validate-template-deployment"></a>Şablon dağıtımı doğrulama

Bu kaynak grubu ve depolama hesabı görmek için aşağıdaki CLI komutları kullanın:

```azurecli
az group list

az storage account list
```

## <a name="next-steps"></a>Sonraki adımlar

- Şablon dağıtımı hakkında daha fazla bilgi için bkz:

[Şablonları PowerShell ile dağıtma](azure-stack-deploy-template-powershell.md)
