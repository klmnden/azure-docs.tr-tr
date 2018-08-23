---
title: Proje için bir ASP.NET Core yapılan değişiklikler, bağlanmak için Azure Key Vault | Microsoft Docs
description: Visual Studio bağlı hizmetlerini kullanarak toKey kasa bağlandığınızda ASP.NET Core projenizi ne açıklar.
services: key-vault
author: ghogen
manager: douge
tags: azure-resource-manager
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 04/15/2018
ms.author: ghogen
ms.openlocfilehash: b7cbe55fa3a524965e0ebc16c5ff350a60d6e440
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42059760"
---
# <a name="what-happened-to-my-aspnet-core-project-visual-studio-key-vault-connected-service"></a>ASP.NET Core projeme ne oldu (Visual Studio Key Vault bağlı hizmet)?

> [!div class="op_single_selector"]
> - [Başlarken](vs-key-vault-aspnet-core-get-started.md)
> - [Ne oldu](vs-key-vault-aspnet-core-what-happened.md)

Bu makalede ASP.NET projesini eklerken yapılan değişiklikleri tam tanımlayan [Key Vault bağlı hizmetini Visual Studio kullanarak](vs-key-vault-add-connected-service.md).

Bağlı hizmet ile çalışma hakkında daha fazla bilgi için bkz: [Başlarken](vs-key-vault-aspnet-core-get-started.md).

## <a name="added-references"></a>Ek başvurular

NuGet paket başvuruları ve proje dosya *.NET başvuruları etkiler.

| Tür | Başvuru |
| --- | --- |
| NuGet | Microsoft.AspNetCore.AzureKeyVault.HostingStartup |

## <a name="added-files"></a>Eklenen dosyalar

- Bağlı hizmet sağlayıcısı, sürümü ve belgeleri bağlantısı hakkında bazı bilgiler kaydeder ConnectedService.json eklenir.

## <a name="project-file-changes"></a>Proje dosya değişiklikleri

- Bağlı Hizmetleri ItemGroup ve ConnectedServices.json dosya eklendi.

## <a name="launchsettingsjson-changes"></a>launchsettings.JSON değişiklikleri

- IIS Express profili ve web projenizin adına eşleşen profili için aşağıdaki ortam değişkeni girdileri eklendi:

    ```json
      "environmentVariables": {
        "ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONENABLED": "true",
        "ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONVAULT": "<your keyvault URL>"
      }
    ```

## <a name="changes-on-azure"></a>Azure üzerindeki değişiklikler

- Bir kaynak grubu oluşturulur (veya var olan bir kullanılır).
- Belirtilen kaynak grubunda bir Key Vault oluşturdunuz.

