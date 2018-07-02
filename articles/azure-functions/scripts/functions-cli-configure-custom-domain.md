---
title: Azure CLI Betik Örneği - Özel bir etki alanını bir işlev uygulaması ile eşleme | Microsoft Docs
description: Azure CLI Betik Örneği - Azure’da özel bir etki alanını bir işlev uygulaması ile eşleme.
services: functions
documentationcenter: ''
author: ggailey777
manager: cfowler
editor: ''
tags: azure-service-management
ms.assetid: d127e347-7581-47d7-b289-e0f51f2fbfbc
ms.service: functions
ms.workload: na
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/26/2018
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 7d3fc71bc53e85fa7555dbee5ee79b3f06f27fe8
ms.sourcegitcommit: 0408c7d1b6dd7ffd376a2241936167cc95cfe10f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960347"
---
# <a name="map-a-custom-domain-to-a-function-app"></a>Özel bir etki alanını işlev uygulaması ile eşleme

Bu örnek betik, ilgili kaynaklarıyla birlikte bir işlev uygulaması oluşturur ve sonra onu `www.<yourdomain>` ile eşler. İşlev uygulamanız bir [App Service planında](../functions-scale.md#app-service-plan) olduğunda CNAME veya A kaydı kullanılarak özel etki alanı eşleyebilirsiniz. [Tüketim planındaki](../functions-scale.md#consumption-plan) işlev uygulamalarında yalnızca CNAME seçeneği desteklenir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI’yi yerel olarak yükleyip kullanmayı tercih ederseniz Azure CLI 2.0 veya sonraki bir sürümü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 


## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-custom-domain/configure-custom-domain.sh?highlight=3 "Map a custom domain to a function app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_create) | İşlev uygulaması için gereken bir depolama hesabı oluşturur. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Özel etki alanını eşlemek için gereken bir App Service planı oluşturur. |
| [az functionapp create]() | Bir işlev uygulaması oluşturur. |
| [az appservice web config hostname add](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#az_appservice_web_config_hostname_add) | Özel bir etki alanını işlev uygulaması ile eşler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek İşlevler CLI betiği örnekleri, [Azure İşlevleri belgelerinde]() bulunabilir.
