---
title: Azure CLI ile Azure Media Services hesabı oluşturma | Microsoft Docs
description: Azure Media Services hesabı oluşturmak için bu hızlı başlangıç adımlarını izleyin.
services: media-services
documentationcenter: ''
author: Juliako
manager: cflower
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 03/27/2018
ms.author: juliako
ms.openlocfilehash: ca01f32709ce7c9fc49629415cd8697a9d9ba43a
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35979855"
---
# <a name="create-an-azure-media-services-account"></a>Azure Media Services hesabı oluşturma

Şifreleme, kodlama, çözümleme, yönetme ve azure'da medya içeriği akışı başlatmak için bir Media Services hesabı oluşturmanız gerekir. Bir Media Services hesabı oluşturduğunuzda Media Services hesabı ile aynı coğrafi bölgede ilişkili bir depolama hesabı da oluşturursunuz (ya da var olanı kullanırsınız).

Bu konuda, Azure CLI kullanarak yeni bir Azure Media Services hesabı oluşturma adımları açıklanmaktadır.  

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](http://portal.azure.com) oturum açın ve **CloudShell** başlatarak sonraki adımlarda gösterildiği gibi CLI komutlarını yürütün.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Kullandığınız sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). 

## <a name="set-the-azure-subscription"></a>Azure aboneliğini ayarlama

Aşağıdaki komutta, Media Services hesabı için kullanmak istediğiniz Azure abonelik kimliğini sağlayın. [Abonelikler](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)’e giderek erişimine sahip olduğunuz aboneliklerin listesini görebilirsiniz.

```azurecli-interactive
az account set --subscription mySubscriptionId
```
 
[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]
 
## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir dosyayı akışa alma](stream-files-dotnet-quickstart.md)
