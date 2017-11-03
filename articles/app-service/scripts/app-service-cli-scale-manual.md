---
title: "Azure CLI komut dosyası örneği - ölçek el ile Azure CLI 2.0 kullanan bir Web uygulaması | Microsoft Docs"
description: "Azure CLI komut dosyası örneği - ölçek el ile Azure CLI 2.0 kullanan bir Web uygulaması"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 251d9074-8fff-4121-ad16-9eca9556ac96
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: a8e9dc2d7767459fafe80a4986f5ac87e95e6e34
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="scale-a-web-app-manually"></a>Bir web uygulaması elle ölçeklendirme

Bu senaryoda, bir kaynak grubu, uygulama hizmeti planı ve web uygulaması oluşturmak öğreneceksiniz. Ardından, uygulama hizmeti planı tek bir örnekten birden çok örneklerine ölçeklenir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/scale-manual/scale-manual.sh "Manual Scale")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, bir kaynak grubu, web uygulaması ve tüm ilgili kaynaklar oluşturmak için aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [az grubu oluşturma](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az uygulama hizmeti planı oluşturma](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | App Service planı oluşturur. Bu, Azure web uygulamanız için bir sunucu grubu gibidir. |
| [az webapp oluşturma](https://docs.microsoft.com/cli/azure/webapp#az_webapp_create) | Azure web uygulaması oluşturur. |
| [az uygulama hizmeti planı güncelleştirme](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_update) | Uygulama hizmeti planının özelliklerini güncelleştirir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek uygulama hizmeti CLI kod örnekleri bulunabilir [Azure App Service belgeleri](../app-service-cli-samples.md).
