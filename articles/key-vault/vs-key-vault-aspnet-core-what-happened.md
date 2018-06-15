---
title: Proje için ASP.NET Core yapılan değişiklikler, bağlantı Azure anahtar kasası için | Microsoft Docs
description: Visual Studio bağlı hizmetleri kullanarak toKey kasası bağlandığında ASP.NET Core projenize olanlar açıklar.
services: key-vault
author: ghogen
manager: douge
tags: azure-resource-manager
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 04/15/2018
ms.author: ghogen
ms.openlocfilehash: 8b6c590344db2997c1a987da14cabba76cdca83b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33787377"
---
# <a name="what-happened-to-my-aspnet-core-project-visual-studio-key-vault-connected-service"></a>My ASP.NET Core projeye ne (Visual Studio anahtar kasası bağlı hizmet)?

> [!div class="op_single_selector"]
> - [Başlarken](vs-key-vault-aspnet-core-get-started.md)
> - [Ne oldu](vs-key-vault-aspnet-core-what-happened.md)

Bu makalede bir ASP.NET projesi eklerken tam değişikliklerinin tanımlayan [anahtar kasası bağlı Visual Studio kullanarak hizmet](vs-key-vault-add-connected-service.md).

Bağlantılı hizmeti ile çalışma hakkında daha fazla bilgi için bkz: [Başlarken](vs-key-vault-aspnet-core-get-started.md).

## <a name="added-references"></a>Ek başvurular

Proje dosyası *.NET başvuruları ve NuGet paket referanslarını etkiler.

| Tür | Başvuru |
| --- | --- |
| NuGet | Microsoft.AspNetCore.AzureKeyVault.HostingStartup |

## <a name="added-files"></a>Eklenen dosyalar

- Bağlı hizmet sağlayıcısı, sürüm ve bir bağlantı belgeleri hakkında bazı bilgiler kaydeder ConnectedService.json eklenir.

## <a name="project-file-changes"></a>Proje dosya değişiklikleri

- Bağlı hizmetler ItemGroup ve ConnectedServices.json dosya eklendi.

## <a name="launchsettingsjson-changes"></a>launchsettings.JSON değişiklikleri

- IIS Express profilinde hem web projenizin adına eşleşen profili için aşağıdaki ortam değişkeni girdileri eklendi:

    ```json
      "environmentVariables": {
        "ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONENABLED": "true",
        "ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONVAULT": "<your keyvault URL>"
      }
    ```

## <a name="changes-on-azure"></a>Azure üzerindeki değişiklikler

- Bir kaynak grubu oluşturduk (veya mevcut bir kullanılan).
- Bir anahtar kasası belirtilen kaynak grubunda oluşturuldu.

