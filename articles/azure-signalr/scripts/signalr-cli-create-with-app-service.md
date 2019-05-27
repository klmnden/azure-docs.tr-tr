---
title: Azure CLI betik örneği - bir App Service ile SignalR hizmeti oluşturma
description: Azure CLI Betik Örneği - App Service ile SignalR Hizmeti Oluşturma
author: sffamily
ms.service: signalr
ms.devlang: azurecli
ms.topic: sample
ms.date: 04/20/2018
ms.author: zhshang
ms.custom: mvc
ms.openlocfilehash: 5fd9e593a9a5158b35d3c0e17436d54bc7c1bd03
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66128401"
---
# <a name="create-a-signalr-service-with-an-app-service"></a>App Service ile SignalR Hizmeti Oluşturma

Bu örnek betik, istemcilere gerçek zamanlı içerik güncelleştirmeleri göndermek için kullanılan yeni bir Azure SignalR Hizmeti kaynağı oluşturur. Bu betik, SignalR Hizmetini kullanan ASP.NET Core Web Uygulamanızı barındırmak için yeni bir Web Uygulaması ve Uygulama Hizmeti planı da ekler. Web uygulaması, yeni SignalR hizmet kaynağına bağlanmak için *AzureSignalRConnectionString* adlı bir Uygulama Ayarı ile yapılandırılır.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek betik

Bu betik, Azure CLI için *signalr* uzantısını kullanır. Bu örnek betiği kullanmadan önce, aşağıdaki komutu yürüterek Azure CLI için *signalr* uzantısını yükleyin:

```azurecli-interactive
az extension add -n signalr
```

[!code-azurecli-interactive[main](../../../cli_scripts/azure-signalr/create-signalr-with-app-service/create-signalr-with-app-service.sh "Create a new Azure SignalR Service and Web App")]

Yeni kaynak grubu için oluşturulan gerçek adı not edin. Çıktıda gösterilecektir. Tüm grup kaynaklarını silmek istediğinizde bu kaynak grubu adını kullanacaksınız.

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Tablodaki her komut, komuta özgü belgelere yönlendirir. Bu betik şu komutları kullanır:

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az signalr create](/cli/azure/ext/signalr/signalr#ext-signalr-az-signalr-create) | Bir Azure SignalR Hizmeti kaynağı oluşturur. |
| [az signalr key list](/cli/azure/ext/signalr/signalr/key#ext-signalr-az-signalr-key-list) | SignalR ile gerçek zamanlı içerik güncelleştirmeleri gönderilirken uygulamanız tarafından kullanılacak anahtarları listeler. |
| [az appservice plan create](/cli/azure/appservice/plan#az-appservice-plan-create) | Web uygulamalarını barındırmak için bir Azure App Service Planı oluşturur. |
| [az webapp create](/cli/azure/webapp#az-webapp-create) | App Service barındırma planını kullanarak bir Azure Web uygulaması oluşturur. |
| [az webapp config appsettings set](/cli/azure/webapp/config/appsettings#az-webapp-config-appsettings-set) | Web uygulaması için yeni bir uygulama ayarı ekler. Bu uygulama ayarı, SignalR bağlantı dizesini depolamak için kullanılır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek Azure SignalR Hizmeti CLI betik örnekleri, [Azure SignalR Hizmeti belgelerinde](../signalr-reference-cli.md) bulunabilir.
