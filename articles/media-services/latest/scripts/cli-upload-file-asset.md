---
title: Azure CLI Betik Örneği - Bir kapsayıcıya dosya yükleme | Microsoft Docs
description: Azure CLI betiğini kullanarak yerel bir dosyayı bir depolama kapsayıcısına yükleyin.
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
ms.date: 11/11/2018
ms.author: juliako
ms.openlocfilehash: 5120d938d137efef77eeb0b69a5bf571bd4c509b
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51614506"
---
# <a name="cli-example-upload-a-local-file-to-a-container"></a>CLI örneği: Yerel bir dosyayı kapsayıcıya yükleme 

Bu makaledeki Azure CLI betiğinde, yerel bir dosyanın bir depolama kapsayıcısına nasıl yükleneceği gösterilir.

## <a name="prerequisites"></a>Önkoşullar 

- Yükleyin ve bu makalede Azure CLI 2.0 veya sonraki bir sürüm gerektirir, CLI'yı yerel olarak kullanın. Kullandığınız sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). 

    Şu anda tüm [Media Services v3 CLI](https://aka.ms/ams-v3-cli-ref) komutlar Azure Cloud Shell içinde çalışır. CLI'yi yerel olarak kullanmak için önerilir.

- [Bir Media Services hesabı oluşturma](../create-account-cli-how-to.md).

## <a name="example-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../../cli_scripts/media-services/upload-file-asset/UploadFile-Asset.sh "Upload a file")]

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla örnek için bkz. [Azure CLI örnekleri](../cli-samples.md).
