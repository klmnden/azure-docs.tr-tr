---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/27/2019
ms.author: glenga
ms.openlocfilehash: 8110d0a9d574c6691322df2162ca877b031cbc59
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67442265"
---
Bağlama uzantıları yüklemek için en kolay yolu etkinleştirmektir [uzantı paketleri](../articles/azure-functions/functions-bindings-register.md#extension-bundles). Etkin, paketleri ile önceden tanımlanmış bir uzantı paketleri otomatik olarak yüklenir.

Uzantı paketleri etkinleştirmek için *host.json* dosya ve içeriğini aşağıdaki kod eşleşecek şekilde güncelleştirin:

```json
{
    "version": "2.0",
    "extensionBundle": {
        "id": "Microsoft.Azure.Functions.ExtensionBundle",
        "version": "[1.*, 2.0.0)"
    }
}
```