---
title: "Azure CLI komut dosyası örneği - eşlemesi bir işlev uygulaması için özel bir etki alanı | Microsoft Docs"
description: "Azure CLI komut dosyası örneği - eşlemesi bir işlev uygulaması Azure için özel bir etki alanı."
services: functions
documentationcenter: 
author: ggailey777
manager: cfowler
editor: 
tags: azure-service-management
ms.assetid: d127e347-7581-47d7-b289-e0f51f2fbfbc
ms.service: functions
ms.workload: na
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/01/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 87b79d6222f40e3dc1306ecace51bae50b06e484
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="map-a-custom-domain-to-a-function-app"></a>Eşlemesi bir işlev uygulaması için özel bir etki alanı

Bu örnek komut dosyası ile ilgili kaynaklar bir işlev uygulaması oluşturur ve ardından eşler `www.<yourdomain>` ona. Özel bir etki alanına eşlemek için işlevi uygulamanıza bir uygulama hizmeti planı yer alan ve tüketim planı oluşturulması gerekir. Azure işlevleri yalnızca destekleyen bir A kaydı kullanarak özel bir etki alanı eşleme.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Yükleyip CLI yerel olarak kullanmak isterseniz, Azure CLI Sürüm 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 


## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-custom-domain/configure-custom-domain.sh?highlight=3 "Map a custom domain to a function app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyasını aşağıdaki komutları kullanır: komutu belirli belgeleri tablo bağlanan her komutu.

| Komut | Notlar |
|---|---|
| [az grubu oluşturma](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az depolama hesabı oluşturma](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_create) | İşlev uygulaması tarafından gerekli bir depolama hesabı oluşturur. |
| [az uygulama hizmeti planı oluşturma](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Özel etki alanını eşlemek için gerekli bir uygulama hizmeti planı oluşturur. |
| [az functionapp oluşturma]() | Bir işlev uygulaması oluşturur. |
| [az appservice web yapılandırma ana bilgisayar adı ekleyin](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#az_appservice_web_config_hostname_add) | Özel bir etki alanı için bir işlev uygulaması eşler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek işlevler CLI kod örnekleri bulunabilir [Azure işlevleri belgelerine]().
