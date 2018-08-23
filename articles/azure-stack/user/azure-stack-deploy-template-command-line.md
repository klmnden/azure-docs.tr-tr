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
ms.date: 08/15/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 7814552256f17c5265bbeb4ce8c069dd8dca1bb2
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42060948"
---
# <a name="deploy-templates-in-azure-stack-using-the-command-line"></a>Şablonları komut satırını kullanarak Azure Stack'te dağıtma

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack Geliştirme Seti için Azure Resource Manager şablonlarını dağıtmak için komut satırını kullanın. Azure Resource Manager şablonları, dağıtın ve tek ve eşgüdümlü bir işlemle uygulamanıza yönelik tüm kaynakları sağlayın.

## <a name="before-you-begin"></a>Başlamadan önce
 - [Yükleme ve bağlanma](azure-stack-version-profiles-azurecli2.md) Azure CLI ile Azure Stack
 - Dosyaları indirmek *azuredeploy.json* ve *azuredeploy.parameters.json* gelen [depolama hesabı örnek şablonu oluşturma](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-create-storage-account).
 
## <a name="deploy-template"></a>Şablon dağıtma
Burada bu dosyaları indirilir ve şablonu dağıtmak için aşağıdaki komutu çalıştırın klasörüne gidin:

    azure group create "cliRG" "local" –f azuredeploy.json –d "testDeploy" –e azuredeploy.parameters.json

Bu komut kaynak grubuna şablon dağıtır **cliRG** Azure Stack POC'ın varsayılan konumu.

## <a name="validate-template-deployment"></a>Şablon dağıtımı doğrulama
Bu kaynak grubu ve depolama hesabı görmek için aşağıdaki komutları kullanın:

    azure group list

    azure storage account list




