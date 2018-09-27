---
title: Azure Media Services API'sine - Azure CLI erişim | Microsoft Docs
description: Bu yöntem, Azure Media Services API'sine erişmek için adımları izleyin.
services: media-services
documentationcenter: ''
author: Juliako
manager: cflower
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: mvc
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: e20cac5f1063589bdbfee0f384ac6af5a39811ed
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47096800"
---
# <a name="access-azure-media-services-api-with-the-azure-cli"></a>Azure CLI ile API erişim Azure medya Hizmetleri
 
Azure Media Services API'sine bağlanmak için Azure AD hizmet sorumlusu kimlik doğrulaması kullanmanız gerekir. Aşağıdaki parametrelere sahip bir Azure AD belirteç istemek uygulamanız gerekir:

* Azure AD Kiracı uç noktası
* Media Services kaynak URI'si
* Kaynak URI REST Media Services için
* Azure AD uygulama değerlerini: istemci Kimliğini ve istemci gizli anahtarı

Bu makalede bir Azure AD uygulaması ve hizmet sorumlusu oluşturma ve Azure Media Services kaynaklarına erişmek için gerekli değerleri almak için Azure CLI'yı kullanmayı gösterir.

## <a name="prerequisites"></a>Önkoşullar 

[Bu hızlı başlangıçta](create-account-cli-quickstart.md) açıklandığı gibi yeni bir Azure Media Services hesabı oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](http://portal.azure.com) oturum açın ve **CloudShell** başlatarak sonraki adımlarda gösterildiği gibi CLI komutlarını yürütün.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Kullandığınız sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). 

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir dosyayı akışa alma](stream-files-dotnet-quickstart.md)

## <a name="see-also"></a>Ayrıca bkz.

[Azure CLI](https://docs.microsoft.com/en-us/cli/azure/ams?view=azure-cli-latest)
