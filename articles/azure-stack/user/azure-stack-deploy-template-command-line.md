---
title: "Azure yığınında komut satırı ile şablonlarını dağıtma | Microsoft Docs"
description: "Platformlar arası komut satırı arabirimi (CLI) şablonları Azure yığınına dağıtmak için nasıl kullanılacağını öğrenin."
services: azure-stack
documentationcenter: 
author: heathl17
manager: byronr
editor: 
ms.assetid: 9584177f-4af3-4834-864d-930b09ae0995
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: d58d80d89bb2544c4d4a34f608177a96760406ba
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-templates-in-azure-stack-using-the-command-line"></a>Azure komut satırı kullanılarak yığınında şablonlarını dağıtma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure Resource Manager şablonları Azure yığın Geliştirme Seti dağıtmak için komut satırını kullanın. Azure Resource Manager şablonları dağıtın ve tek ve eşgüdümlü bir işlemle uygulamanızda için tüm kaynakları sağlayın.

## <a name="before-you-begin"></a>Başlamadan önce
 - [Yüklemek ve bağlamak](azure-stack-connect-cli.md) Azure CLI ile Azure yığınına
 - Dosyaları karşıdan *azuredeploy.json* ve *azuredeploy.parameters.json* gelen [depolama hesabı örnek şablonu oluşturma](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-create-storage-account).
 
## <a name="deploy-template"></a>Şablon dağıtma
Burada bu dosyaları indirilir ve şablonu dağıtmak için aşağıdaki komutu çalıştırın klasöre gidin:

    azure group create "cliRG" "local" –f azuredeploy.json –d "testDeploy" –e azuredeploy.parameters.json

Bu komut kaynak grubuna şablon dağıtır **cliRG** Azure yığın POC'ın varsayılan konumu.

## <a name="validate-template-deployment"></a>Şablon dağıtımı doğrulama
Bu kaynak grubu ve depolama hesabı görmek için aşağıdaki komutları kullanın:

    azure group list

    azure storage account list




