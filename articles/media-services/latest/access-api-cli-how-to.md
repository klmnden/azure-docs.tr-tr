---
title: Azure Media Services API - CLI 2.0 erişim | Microsoft Docs
description: Azure Media Services API erişmek için bu nasıl yapılır adımları izleyin.
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
ms.openlocfilehash: a4a7c59e93b860245d67695de90fbae2becac3e9
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="access-azure-media-services-api-with-cli-20"></a>CLI 2.0 ile erişim Azure Media Services API
 
Azure Media Services API'sine bağlanmak için Azure AD hizmet sorumlusu kimlik doğrulaması kullanmanız gerekir. Aşağıdaki parametreleri olan bir Azure AD belirteç istemek uygulamanız gerekir:

* Azure AD Kiracı uç noktası
* Media Services kaynak URI'si
* Kaynak URI'si REST Media Services için
* Azure AD uygulama değerleri: istemci Kimliğini ve istemci parolası

Bu makale, CLI 2.0 bir Azure AD uygulaması ve hizmet sorumlusu oluşturma ve Azure Media Services kaynaklara erişmek için gerekli değerleri almak için nasıl kullanılacağını gösterir.

## <a name="prerequisites"></a>Önkoşullar 

Bölümünde açıklandığı gibi yeni bir Azure Media Services hesabı oluşturma [Bu Hızlı Başlangıç](create-account-cli-quickstart.md).

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Oturum [Azure portal](http://portal.azure.com) ve başlatma **CloudShell** CLI komutları, sonraki adımlarda gösterildiği gibi yürütmek için.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Kullandığınız sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir dosya akışı](stream-files-dotnet-quickstart.md)
