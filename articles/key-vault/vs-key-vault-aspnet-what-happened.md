---
title: Proje için bir ASP.NET yapılan değişiklikler, bağlanmak için Azure Key Vault | Microsoft Docs
description: Visual Studio bağlı hizmetlerini kullanarak toKey kasa bağlandığınızda ASP.NET projenizi ne açıklar.
services: key-vault
author: ghogen
manager: douge
tags: azure-resource-manager
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.topic: conceptual
ms.date: 04/15/2018
ms.author: ghogen
ms.openlocfilehash: a74e4e10681f6b91e028067d8985408b0745dcd2
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42056261"
---
# <a name="what-happened-to-my-aspnet-project-visual-studio-key-vault-connected-service"></a>ASP.NET projeme ne oldu (Visual Studio Key Vault bağlı hizmet)?

> [!div class="op_single_selector"]
> - [Başlarken](vs-key-vault-aspnet-get-started.md)
> - [Ne oldu](vs-key-vault-aspnet-what-happened.md)

Bu makalede ASP.NET projesini eklerken yapılan değişiklikleri tam tanımlayan [Key Vault bağlı hizmetini Visual Studio kullanarak](vs-key-vault-add-connected-service.md).

Bağlı hizmet ile çalışma hakkında daha fazla bilgi için bkz: [Başlarken](vs-key-vault-aspnet-get-started.md).

## <a name="added-references"></a>Ek başvurular

Proje dosyası *.NET başvuruları etkiler ve `packages.config` (NuGet başvurularını).

| Tür | Başvuru |
| --- | --- |
| .NET; NuGet | Microsoft.Azure.KeyVault |
| .NET; NuGet | Microsoft.Azure.KeyVault.WebKey |
| .NET; NuGet | Microsoft.Rest.ClientRuntime |
| .NET; NuGet | Microsoft.Rest.ClientRuntime.Azure |

## <a name="added-files"></a>Eklenen dosyalar

- Bağlı hizmet sağlayıcısı, sürümü ve belgeleri bağlantısı hakkında bazı bilgiler kaydeder ConnectedService.json eklenir.

## <a name="project-file-changes"></a>Proje dosya değişiklikleri

- Bağlı Hizmetleri ItemGroup ve ConnectedServices.json dosya eklendi.
- Açıklanan .NET derlemesine ilişkin başvurular [eklenen başvuruları](#added-references) bölümü.

## <a name="webconfig-or-appconfig-changes"></a>Web.config veya app.config değişiklikleri

- Aşağıdaki yapılandırma girdileri eklendi:

    ```xml
    <appSettings>
       <add key="vaultName" value="<your Key Vault name>" />
       <add key="vaultUri" value="<the URI to your Key Vault in Azure>" />
    </appSettings>
    ```

## <a name="changes-on-azure"></a>Azure üzerindeki değişiklikler

- Bir kaynak grubu oluşturulur (veya var olan bir kullanılır).
- Belirtilen kaynak grubunda bir Key Vault oluşturdunuz.

