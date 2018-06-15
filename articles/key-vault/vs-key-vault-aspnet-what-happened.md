---
title: Proje için ASP.NET yapılan değişiklikler, bağlantı Azure anahtar kasası için | Microsoft Docs
description: Visual Studio bağlı hizmetleri kullanarak toKey kasası bağlandığında ASP.NET projenize olanlar açıklar.
services: key-vault
author: ghogen
manager: douge
tags: azure-resource-manager
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.topic: conceptual
ms.date: 04/15/2018
ms.author: ghogen
ms.openlocfilehash: a43f893c7ee87ffb02179c06ea5786715547e93a
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33787363"
---
# <a name="what-happened-to-my-aspnet-project-visual-studio-key-vault-connected-service"></a>My ASP.NET projesi ne (Visual Studio anahtar kasası bağlı hizmet)?

> [!div class="op_single_selector"]
> - [Başlarken](vs-key-vault-aspnet-get-started.md)
> - [Ne oldu](vs-key-vault-aspnet-what-happened.md)

Bu makalede bir ASP.NET projesi eklerken tam değişikliklerinin tanımlayan [anahtar kasası bağlı Visual Studio kullanarak hizmet](vs-key-vault-add-connected-service.md).

Bağlantılı hizmeti ile çalışma hakkında daha fazla bilgi için bkz: [Başlarken](vs-key-vault-aspnet-get-started.md).

## <a name="added-references"></a>Ek başvurular

Proje dosyası *.NET başvuruları etkiler ve `packages.config` (NuGet başvurularını).

| Tür | Başvuru |
| --- | --- |
| .NET; NuGet | Microsoft.Azure.KeyVault |
| .NET; NuGet | Microsoft.Azure.KeyVault.WebKey |
| .NET; NuGet | Microsoft.Rest.ClientRuntime |
| .NET; NuGet | Microsoft.Rest.ClientRuntime.Azure |

## <a name="added-files"></a>Eklenen dosyalar

- Bağlı hizmet sağlayıcısı, sürüm ve bir bağlantı belgelerine hakkında bazı bilgiler kaydeder ConnectedService.json eklenir.

## <a name="project-file-changes"></a>Proje dosya değişiklikleri

- Bağlı hizmetler ItemGroup ve ConnectedServices.json dosya eklendi.
- Açıklanan .NET derlemelerini başvurular [eklenen başvuruları](#added-references) bölümü.

## <a name="webconfig-or-appconfig-changes"></a>Web.config veya app.config değişiklikleri

- Aşağıdaki yapılandırma girdileri eklendi:

    ```xml
    <appSettings>
       <add key="vaultName" value="<your Key Vault name>" />
       <add key="vaultUri" value="<the URI to your Key Vault in Azure>" />
    </appSettings>
    ```

## <a name="changes-on-azure"></a>Azure üzerindeki değişiklikler

- Bir kaynak grubu oluşturduk (veya mevcut bir kullanılan).
- Bir anahtar kasası belirtilen kaynak grubunda oluşturuldu.

